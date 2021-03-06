---
title: "Мониторинг конечных точек диспетчера трафика Azure | Документация Майкрософт"
description: "Из этой статьи вы узнаете, каким образом благодаря мониторингу и автоматической отработке отказа конечных точек в диспетчере трафика пользователи Azure могут развертывать высокодоступные приложения."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: 3b30aa04854b779c25582abafc0f9ebba65b71ba
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>Мониторинг конечных точек в диспетчере трафика

Диспетчер трафика Azure включает в себя встроенные возможности мониторинга и автоматическую отработку отказа конечных точек. Благодаря этому можно получать высокодоступные приложения, которые устойчивы к сбоям конечной точки, в том числе к сбоям региона Azure.

## <a name="configure-endpoint-monitoring"></a>Настройка мониторинга конечных точек

Чтобы настроить мониторинг конечных точек, необходимо указать следующие параметры в профиле диспетчера трафика.

* **Протокол**. Выберите протокол HTTP, HTTPS или TCP, который диспетчер трафика будет использовать при проверке работоспособности конечной точки. Мониторинг HTTPS проверяет не действительность SSL-сертификата, а только его наличие.
* **Порт**. Выберите порт, используемый для запроса.
* **Путь**. Этот параметр конфигурации является допустимым только для протоколов HTTP и HTTPS, для которых требуется указание параметра пути. Если указать этот параметр для протокола мониторинга TCP, вы получите ошибку. Для протокола TCP введите относительный путь и имя веб-страницы или файла, к которому обращается средство мониторинга. В качестве относительного пути можно указать косую черту (/). Это значение указывает, что файл находится в корневом каталоге (по умолчанию).
* **Интервал проверки**. Это значение указывает, как часто проверяется работоспособность конечной точки с помощью агента проверки диспетчера трафика. Вы можете указать здесь два значения: 30 секунд (обычная проверка) и 10 секунд (быстрая проверка). Если не указать значения, профиль задаст значение по умолчанию 30 секунд. Посетите страницу [цен диспетчера трафика](https://azure.microsoft.com/pricing/details/traffic-manager), чтобы узнать больше о ценах на быструю проверку.
* **Допустимое число сбоев**. Это значение указывает количество сбоев, которое допускает агент проверки диспетчера трафика. По достижении этого количества конечная точка будет помечена как неработоспособная. Значение должно быть в диапазоне от 0 до 9. Значение 0 означает, что при первом сбое конечная точка будет помечена как неработоспособная. Если значение не задано, используется значение по умолчанию 3.
* **Время ожидания мониторинга**. Это свойство указывает количество времени ожидания (когда проба проверки работоспособности отправлена в конечную точку), по достижении которого диспетчер трафика определяет, что проверка завершилась сбоем. Если для интервала проверки задано значение 30 секунд, тогда вы можете задать значение времени ожидания 5–10 секунд. Если значение не задано, используется значение по умолчанию 10 секунд. Если для интервала проверки задано значение 10 секунд, тогда вы можете задать значение времени ожидания 5–9 секунд. Если значение времени ожидания не задано, используется значение по умолчанию 9 секунд.

![Мониторинг конечных точек в диспетчере трафика](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**Рис. 1. Мониторинг конечных точек в диспетчере трафика**

## <a name="how-endpoint-monitoring-works"></a>Как выполняется мониторинг конечных точек

Если в качестве протокола мониторинга используется протокол HTTP или HTTPS, агент проверки диспетчера трафика выполняет запрос GET в конечную точку, используя указанные протокол, порт и относительный путь. Если он возвращает ответ 200-OK, тогда эта конечная точка считается работоспособной. Если ответ имеет другое значение или отсутствует в течение указанного периода ожидания, тогда агент проверки диспетчера трафика повторяет попытки согласно параметру "Допустимое число сбоев" (если для этого параметра задано значение 0, повторная попытка не выполняется). Если число последовательных сбоев превышает значение, указанное в параметре допустимого числа сбоев, тогда эта конечная точка помечается как неработоспособная. 

Если протоколом мониторинга является протокол TCP, агент проверки диспетчера трафика запускает запрос на подключение TCP, используя указанный порт. Если конечная точка отвечает на этот запрос ответом на установку подключения, проверка работоспособности помечается как успешно завершенная и агент проверки диспетчера трафика сбрасывает подключение TCP. Если ответ имеет другое значение или отсутствует в течение указанного периода ожидания, тогда агент проверки диспетчера трафика повторяет попытки согласно параметру "Допустимое число сбоев" (если для этого параметра задано значение 0, повторная попытка не выполняется). Если число последовательных сбоев превышает значение, указанное в параметре допустимого числа сбоев, тогда эта конечная точка помечается как неработоспособная.

Во всех случаях диспетчер трафика выполняет проверку из нескольких расположений, а последовательное определение сбоев предусмотрено для каждого региона. Это также означает, что проверка работоспособности конечных точек диспетчером трафика выполняется с большей частотой, чем задано параметром, используемым для интервала проверки.

>[!NOTE]
>На стороне конечной точки для протоколов мониторинга HTTP и HTTPS распространенной практикой является реализация пользовательской страницы в приложении, например /health.aspx. Используя этот путь для мониторинга, можно выполнить проверки для конкретного приложения, такие как проверка счетчиков производительности или проверка доступности базы данных. На основании этих пользовательских проверок страница возвращает соответствующий код состояния HTTP.

Все конечные точки в профиле диспетчера трафика имеют общие параметры мониторинга. Если необходимо использовать разные параметры мониторинга для разных конечных точек, можно создать [вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Состояние конечной точки и профиля

Можно включать и отключать профили и конечные точки диспетчера трафика. Однако изменение состояния конечной точки также может произойти в результате автоматической настройки и процессов диспетчера трафика.

### <a name="endpoint-status"></a>Состояние конечной точки

Можно включить или отключить определенную конечную точку. Это не повлияет на базовую службу, которая может остаться работоспособной. Изменение состояния конечной точки определяет ее доступность в профиле диспетчера трафика. Если конечная точка отключена, то диспетчер трафика не проверяет ее работоспособность и эта конечная точка не включается в ответ DNS.

### <a name="profile-status"></a>Состояние профиля

С помощью параметра "состояние профиля" можно включить или отключить определенный профиль. Если состояние конечной точки влияет на одну конечную точку, то состояние профиля влияет на весь профиль, включая все конечные точки. При отключении профиля конечные точки не проверяются на работоспособность и не включаются в ответ DNS. Для запроса DNS возвращается код ответа [NXDOMAIN](https://tools.ietf.org/html/rfc2308).

### <a name="endpoint-monitor-status"></a>Отслеживаемое состояние конечной точки

Отслеживаемое состояние конечной точки — это параметр, создаваемый диспетчером трафика, который отражает состояние конечной точки. Этот параметр нельзя изменить вручную. Отслеживаемое состояние конечной точки представляет собой сочетание отслеживания и настроенного состояния конечной точки. В следующей таблице показаны возможные значения отслеживаемого состояния конечной точки:

| Состояние профиля | Состояние конечной точки | Отслеживаемое состояние конечной точки | Примечания |
| --- | --- | --- | --- |
| Отключено |Включено |Неактивно |Профиль отключен. Хотя конечная точка остается в состоянии "Включено", состояние профиля ("Отключено") имеет более высокий приоритет. Конечные точки в отключенных профилях не отслеживаются. Для запроса DNS возвращается код ответа NXDOMAIN. |
| &lt;любой&gt; |Отключено |Отключено |Конечная точка отключена. Отключенные конечные точки не отслеживаются. Эта конечная точка не включается в ответы DNS и поэтому не получает трафик. |
| Включено |Включено |В сети |Конечная точка отслеживается и работоспособна. Она включается в ответы DNS и может получать трафик. |
| Включено |Включено |Деградация |Проверки работоспособности мониторинга конечной точки завершаются ошибкой. Эта конечная точка не включается в ответы DNS и поэтому не получает трафик. <br>Исключение возможно в том случае, если все конечные точки имеют "пониженную функциональность", тогда все они возвращаются в ответе на запрос.</br>|
| Включено |Включено |Проверка конечной точки |Конечная точка отслеживается, но результаты первой проверки еще не получены. CheckingEndpoint — это временное состояние, которое обычно возникает сразу же после добавления конечной точки или ее включения в профиле. Конечная точка в таком состоянии включается в ответы DNS и может получать трафик. |
| Включено |Включено |Остановлено |Облачная служба или веб-приложение, на которое указывает конечная точка, не работает. Проверьте параметры облачной службы или веб-приложения. Это также возможно, если конечная точка является конечной точкой вложенного типа, а дочерний профиль отключен или неактивен. <br>Конечная точка в остановленном состоянии не отслеживается. Она не включается в ответы DNS и поэтому не получает трафик. Исключение возможно в том случае, если все конечные точки имеют "пониженную функциональность", тогда все они вернутся в ответе на запрос.</br>|

Дополнительные сведения о вычислении отслеживаемого состояния конечной точки для вложенных конечных точек см. в статье [Вложенные профили диспетчера трафика](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Состояние монитора профиля

Отслеживаемое состояние профиля — это сочетание настроенного состояния профиля и отслеживаемых состояний для всех конечных точек. В следующей таблице приведены возможные значения.

| Состояние профиля (согласно настройкам) | Отслеживаемое состояние конечной точки | Состояние монитора профиля | Примечания |
| --- | --- | --- | --- |
| Отключено |&lt;любой&gt; или профиль без определенных конечных точек. |Отключено |Профиль отключен. |
| Включено |По крайней мере одна конечная точка находится в состоянии пониженной функциональности. |Деградация |Просмотрите значения состояния отдельных конечных точек, чтобы определить, какие конечные точки необходимо рассмотреть. |
| Включено |По крайней мере одна конечная точка находится в сети. Отсутствуют конечные точки в состоянии пониженной функциональности. |В сети |Служба принимает трафик. Никаких действий от пользователя не требуется. |
| Включено |По крайней мере одна конечная точка находится в состоянии проверки конечной точки. Нет конечных точек, находящихся в сети или в состоянии пониженной функциональности. |Проверка конечных точек |Это переходное состояние возникает при создании или включении профиля. Работоспособность конечной точки проверяется в первый раз. |
| Включено |Все конечные точки профиля отключены или остановлены, либо в профиле не определены конечные точки. |Неактивно |Конечные точки не активны, но профиль включен. |

## <a name="endpoint-failover-and-recovery"></a>Отработка отказа и восстановление конечной точки

Диспетчер трафика периодически проверяет работоспособность всех конечных точек, включая неработоспособные конечные точки. Он обнаруживает, когда конечная точка восстановила работоспособность, и возвращает ее в работу.

Конечная точка считается неработоспособной при возникновении любого из следующих событий:
- Если используется протокол мониторинга HTTP или HTTPS:
    - получен ответ, отличный от 200 (включая различные коды 2xx или перенаправление 301/302).
- Если используется протокол мониторинга TCP: 
    - в ответ на запрос SYNC, отправленный диспетчером трафика, чтобы установить подключение, получен ответ, отличный от ACK или SYN-ACK.
- Превышено время ожидания. 
- Любые другие проблемы с подключением, из-за которых невозможно получить доступ к конечной точке.

Дополнительную информацию об устранении неполадок, вызвавших сбой при проверке, см. в разделе [Устранение неполадок с состоянием пониженной функциональности в диспетчере трафика Azure](traffic-manager-troubleshooting-degraded.md). 

Следующая временная шкала на рисунке 2 представляет подробное описание процесса мониторинга конечной точки в диспетчере трафика с применением следующих параметров: протокол мониторинга — HTTP, интервал проверки — 30 секунд, число допустимых сбоев — 3, значение времени ожидания — 10 секунд и срок жизни DNS — 30 секунд.

![Последовательность действий при отработке отказа и восстановлении после сбоя конечной точки диспетчера трафика](./media/traffic-manager-monitoring/timeline.png)

**Рис. 2. Последовательность восстановления и отработки отказа конечной точки диспетчера трафика**

1. **GET**. Для каждой конечной точки система мониторинга диспетчера трафика выполняет запрос GET для пути, указанного в параметрах мониторинга.
2. **200 ОК**. Система мониторинга ожидает возврата сообщения HTTP "200 ОК" в течение 10 секунд. Получив этот ответ, система распознает облачную службу как доступную.
3. **30 секунд между проверками**. Проверка работоспособности конечной точки выполняется каждые 30 секунд.
4. **Служба недоступна**. Служба становится недоступной. Диспетчеру трафика станет известно об этом только при следующей проверке работоспособности.
5. **Попытки получить доступ к пути мониторинга**. Система мониторинга выполняет запрос GET, но не получает ответ в течение периода ожидания длительностью 10 секунд (также может быть получен ответ, отличный от 200). Затем выполняется еще три попытки с интервалом в 30 секунд. Если одна из попыток оказывается успешной, число попыток сбрасывается.
6. **Устанавливается состояние пониженной функциональности**. После четырех последовательных неудачных попыток система мониторинга помечает недоступную конечную точку как находящуюся в состоянии пониженной функциональности.
7. **Трафик направляется на другие конечные точки**. DNS-серверы диспетчера трафика обновляются, и диспетчер трафика больше не возвращает эту конечную точку в ответ на запросы DNS. Новые подключения направляются к другим, доступным конечным точкам. Однако рекурсивные DNS-серверы и DNS-клиенты могут по-прежнему содержать в кэше предыдущие ответы DNS с этой конечной точкой. Клиенты продолжат использовать конечную точку, пока не истечет срок действия кэша DNS. По истечении срока действия этого кэша клиенты создадут новые запросы DNS и будут направлены к другим конечным точкам. Длительность кэширования задается с помощью параметра срока жизни в профиле диспетчера трафика. Например, это может быть 30 секунд.
8. **Проверки работоспособности продолжаются**. Диспетчер трафика продолжает проверять работоспособность конечной точки, пока она находится в состоянии пониженной функциональности. Диспетчер трафика обнаруживает, когда конечная точка восстановила работоспособность.
9. **Служба возвращается в сеть**. Служба становится доступной. Конечная точка будет оставаться в состоянии пониженной функциональности в диспетчере трафика, пока система мониторинга не выполнит следующую проверку работоспособности.
10. **Возобновляется направление трафика к службе**. Диспетчер трафика отправляет запрос GET и получает ответ о состоянии "200 OK". Служба вернулась в работоспособное состояние. Серверы имен диспетчера трафика снова обновляются и начинают передавать DNS-имя службы в ответах DNS. Трафик вернется к конечной точке, так как срок действия кэшированных ответов DNS, возвращающих другие конечные точки, истек, и существующие подключения к другим конечным точкам завершены.

    > [!NOTE]
    > Так как диспетчер трафика работает на уровне DNS, он не может влиять на существующие подключения к какой-либо конечной точке. При маршрутизации трафика между конечными точками (путем изменения параметров профиля либо во время отработки отказа или восстановления размещения) диспетчер трафика направляет новые подключения к доступным конечным точкам. Однако другие конечные точки могут по-прежнему получать трафик через имеющиеся подключения, пока эти сеансы не будут завершены. Чтобы трафик не проходил через имеющиеся подключения, для приложений следует ограничить продолжительность сеанса в каждой конечной точке.

## <a name="traffic-routing-methods"></a>Методы маршрутизации трафика

Если конечная точка переходит в состояние пониженной функциональности, она больше не возвращается в ответ на запросы DNS. Вместо этого выбирается и возвращается альтернативная конечная точка. Метод маршрутизации трафика, заданный в профиле, определяет, как выбирается альтернативная конечная точка.

* **Приоритет**. Конечные точки формируют список по приоритетам. Всегда возвращается первая доступная конечная точка в списке. Если она переходит в состояние пониженной функциональности, то возвращается следующая доступная конечная точка.
* **Взвешивание**. Случайным образом выбирается любая доступная конечная точка в зависимости от назначенного ей весового коэффициента и весовых коэффициентов других доступных конечных точек.
* **Производительность**. Возвращается конечная точка, ближайшая к конечному пользователю. В случае недоступности этой конечной точки диспетчер трафика перемещает трафик на конечные точки в следующем ближайшем регионе Azure. С помощью [вложенных профилей диспетчера трафика](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region) можно настроить альтернативные планы отработки отказа для повышения производительности маршрутизации.
* **Географический**. Возвращается конечная точка, сопоставленная для обработки географического расположения на основе IP-адреса запроса. Если эта конечная точка недоступна, другая конечная точка для отработки отказа выбрана не будет, так как географическое расположение можно сопоставить только с одной конечной точкой в профиле (дополнительные сведения см. в разделе [часто задаваемых вопросов](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). В качестве оптимального варианта при использовании географической маршрутизации мы рекомендуем клиентам использовать вложенные профили диспетчера трафика с несколькими конечными точками профиля.

Дополнительные сведения см. в разделе [Методы маршрутизации трафика диспетчером трафика](traffic-manager-routing-methods.md).

> [!NOTE]
> Единственное исключение для нормального поведения маршрутизации трафика возникает, когда все конечные точки находятся в состоянии пониженной функциональности. В этом случае диспетчер трафика по возможности *воспринимает конечные точки в состоянии пониженной функциональности как работающие и находящиеся в сети*. Такое поведение предпочтительнее альтернативного варианта — вообще не возвращать конечную точку в ответе DNS. Отключенные и остановленные конечные точки не отслеживаются, таким образом, они не считаются допустимыми для трафика.
>
> Это условие обычно вызвано неправильной конфигурацией службы, например:
>
> * проверки работоспособности диспетчера трафика блокируются списком управления доступом [ACL];
> * неправильная конфигурация порта или протокола мониторинга в профиле диспетчера трафика.
>
> В результате такого поведения, даже если проверки работоспособности диспетчера трафика настроены неправильно, с точки зрения маршрутизации трафика может показаться, что диспетчер трафика *работает правильно*. Кроме того, в этом случае при сбое конечной точки не происходит отработка отказа, что негативно влияет на общую доступность приложения. Поэтому важно убедиться, что профиль находится в сети, а не в состоянии пониженной функциональности. Состояние "В сети" указывает на то, что проверки работоспособности диспетчера трафика выполняются надлежащим образом.

Дополнительную информацию об устранении неполадок, вызвавших сбой при проверке работоспособности, см. в разделе [Устранение неполадок с состоянием пониженной функциональности в диспетчере трафика Azure](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Дальнейшие действия

Узнайте о том, [как работает диспетчер трафика](traffic-manager-how-traffic-manager-works.md)

Узнайте больше о [методах маршрутизации трафика](traffic-manager-routing-methods.md) , поддерживаемых в диспетчере трафика.

Узнайте, как [создать профиль диспетчера трафика](traffic-manager-manage-profiles.md)

[устранении неполадок с состоянием пониженной функциональности](traffic-manager-troubleshooting-degraded.md) конечной точки диспетчера трафика.
