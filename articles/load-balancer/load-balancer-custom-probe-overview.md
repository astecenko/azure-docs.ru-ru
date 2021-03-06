---
title: "Использовать пользовательские зондов подсистемы балансировки нагрузки для мониторинга состояния работоспособности | Документы Microsoft"
description: "Узнайте, как выполнять мониторинг экземпляров за балансировщиком нагрузки с помощью пользовательских проверок для балансировщика нагрузки Azure"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 266132d8cbb6f9922ce7b49759981132c2c17f47
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="understand-load-balancer-probes"></a>Понимать зондов подсистемы балансировки нагрузки

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Балансировщик нагрузки Azure позволяет отслеживать работоспособность различных экземпляров сервера с помощью проверок. Если проверка не отвечает, балансировщик нагрузки Azure прекращает отправлять новое подключение неработоспособным экземплярам. Существующие подключения не затрагиваются, а новые подключения отправляются в работоспособные экземпляры.

Роли облачной службы (рабочие роли и веб-роли) используют гостевой агент для мониторинга проб. При использовании виртуальных машин за подсистемой балансировки нагрузки, необходимо настроить пользовательские зонды TCP или HTTP.

## <a name="understand-probe-count-and-timeout"></a>Общие сведения о количестве проверок и времени ожидания

Поведение проверки зависит от следующих параметров:

* Количество успешных проверок, достаточное для того, чтобы пометить экземпляр как работоспособный.
* Количество неудачных проверок, достаточное для того, чтобы пометить экземпляр как неработоспособный.

Значения времени ожидания и частоты, заданные в SuccessFailCount определить, подтвержден ли экземпляр запущен или не запущена. На портале Azure в качестве значения времени ожидания задается удвоенное значение частоты.

Все экземпляры с балансировкой нагрузки для конечной точки (набор с балансировкой нагрузки) должны иметь одинаковую конфигурацию проверки. Не может иметь пробы, отличный конфигурацию для каждого экземпляра роли или ВМ в одной и той же размещенной службе для сочетания определенной конечной точкой. Например, каждый экземпляр должен иметь два одинаковых локальных порта и времени ожидания.

> [!IMPORTANT]
> Зонд подсистемы балансировки нагрузки использует IP-адреса 168.63.129.16. Этот общедоступный IP-адрес упрощает взаимодействие с ресурсами внутренней платформы при использовании виртуальной сети Azure с собственным IP-адресом. Виртуальный открытый IP-адрес 168.63.129.16 используется во всех регионах, и не будет изменена. Рекомендуется разрешить этот IP-адрес во всех политиках локального брандмауэра. Этот адрес не должен считаться угрозой для безопасности, так как источником сообщений с него может быть только внутренняя платформа Azure. Если не разрешить этот IP-адрес в вашей политики брандмауэра, непредвиденное поведение в различных сценариях. Поведение включает в себя настройку же диапазон IP-адреса 168.63.129.16 и дублирования IP адресов.

## <a name="learn-about-the-types-of-probes"></a>Сведения о типах проверки

### <a name="guest-agent-probe"></a>Проверка гостевого агента

Гостевой агент зонда доступен только для облачных служб Azure. Подсистема балансировки нагрузки применяет guest agent внутри виртуальной Машины. Затем прослушивает и отвечает ответ HTTP 200 OK только в том случае, если экземпляр находится в состоянии готовности. (Другие состояния являются Busy, повторный запуск или остановка).

Дополнительные сведения см. в разделе [Настройка файла определения службы (csdef) для проверки работоспособности](https://msdn.microsoft.com/library/azure/ee758710.aspx) или [приступить к работе, создав внешняя Подсистема балансировки нагрузки для облачных служб](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>В каком случае проверка гостевого агента отметит экземпляр как неработоспособный?

Если гостевой агент не дает ответ HTTP 200 OK, подсистема балансировки нагрузки помечает экземпляр как отвечать на запросы. Затем, он перестает отправлять трафик к этому экземпляру. Балансировщик нагрузки продолжит проверять связь с этим экземпляром. Если гостевой агент отправляет ответ HTTP с кодом 200 (ОК), подсистема балансировки нагрузки снова начинает отправлять трафик к этому экземпляру.

При использовании веб-роли код веб-сайта обычно выполняется в w3wp.exe, который не отслеживаемый с помощью Azure fabric или гостевого агента. Ошибки в w3wp.exe (например, ответы HTTP 500) не передал гостевого агента. Следовательно Подсистема балансировки нагрузки, не принимает этот экземпляр из ротации.

### <a name="http-custom-probe"></a>Пользовательская проверка HTTP

Пользовательский зонд HTTP переопределяет Проба по умолчанию гостевого агента. Можно создать собственную пользовательскую логику для определения состояния экземпляра роли. Подсистема балансировки нагрузки по умолчанию проверяет конечную точку каждые 15 секунд. Экземпляр считается в подсистему балансировки нагрузки, если он отвечает HTTP 200 в течение периода ожидания. Время ожидания — 31 секунды по умолчанию.

Пользовательский зонд HTTP может быть полезным, если необходимо реализовать собственную логику, чтобы удалить экземпляры из ротации подсистемы балансировки нагрузки. Например можно удалить экземпляр, если она превышает 90% загрузки ЦП и возвращается в состояние отличный от 200. Если у вас есть веб-ролей, использующих w3wp.exe, можно также получить автоматический мониторинг веб-сайта. Ошибки в коде веб-сайта вернитесь состояние отличный от 200 зонд подсистемы балансировки нагрузки.

> [!NOTE]
> Пользовательская проверка HTTP поддерживает относительные пути и только HTTP-протокол. HTTPS не поддерживается.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>В каком случае пользовательская проверка HTTP отметит экземпляр как неработоспособный?

* Приложение HTTP возвращает код ответа HTTP, отличный от 200 (например 403, 404 или 500). Это подтверждающее оповещает занимать экземпляра приложения не работает.
* Серверу HTTP не ответил после истечения времени ожидания. В зависимости от того, задается значение времени ожидания нескольких запросов проверки может оставаться без ответа, чтобы пробы возвращает помечен как не запущена (то есть до SuccessFailCount зонды отправляются).
* Сервер закрывает подключение путем сброса TCP-соединения.

### <a name="tcp-custom-probe"></a>Пользовательская проверка TCP

Пользовательские зонды TCP инициировать подключение, выполнив трехэтапного с определенным портом.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>В каком случае пользовательская проверка TCP отметит экземпляр как неработоспособный?

* Серверу TCP не ответил после истечения времени ожидания. Когда проверка отмечена как неработоспособная в зависимости от количества неудачных запросов на проверку, достаточного для того, чтобы проверка была помечена как неработоспособная.
* Проверка получает сброс TCP-соединения от экземпляра роли.

Дополнительные сведения о том, как настроить проверку состояния HTTP или TCP-зонды см. в разделе [приступить к созданию внешняя Подсистема балансировки нагрузки в диспетчере ресурсов с помощью PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Добавить исправных экземпляров в подсистеме балансировки нагрузки

Проверки TCP и HTTP считаются допустимыми и отмечают экземпляр роли как работоспособный в следующих случаях:

* Подсистема балансировки нагрузки получает положительную пробу при первой загрузке виртуальной машины.
* Номер SuccessFailCount (описанную ранее) определяет значение успешной проверки, необходимые для пометки экземпляра роли работоспособен. Если экземпляр роли был удален, то для пометки экземпляра роли как неработоспособного количество последующих успешных проверок должно быть не меньше значения SuccessFailCount.

> [!NOTE]
> Если изменяется состояние экземпляра роли, подсистема балансировки нагрузки ожидания больше помещаются обратно в работоспособное состояние экземпляра роли. Время ожидания лишние защищает пользователя и инфраструктуры и преднамеренного политика.

## <a name="use-log-analytics-for-a-load-balancer"></a>Служба аналитики журналов использования для балансировки нагрузки.

Можно использовать [аналитика журналов](load-balancer-monitor-log.md) для проверки состояния работоспособности пробы подсистемы балансировки нагрузки и проверки count. Ведение журнала можно использовать с Power BI или оперативной аналитики Azure для предоставления статистические данные о состоянии работоспособности подсистемы балансировки нагрузки.
