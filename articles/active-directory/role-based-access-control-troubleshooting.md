---
title: "Устранение неполадок управления доступом на основе ролей Azure | Документация Майкрософт"
description: "Справка по проблемам при управлении доступом к ресурсам на основе ролей и ответы на распространенные вопросы."
services: azure-portal
documentationcenter: na
author: andredm7
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b7155ed7613d46329229d8e572c75400041022ce
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Устранение неполадок при управлении доступом на основе ролей

В этой статье содержатся ответы на часто задаваемые вопросы об определенных правах доступа, которые предоставляются с помощью ролей. Вы узнаете, какие ситуации возможны при использовании ролей на портале Azure, а также сможете устранить неполадки, связанные с доступом. Следующие три роли охватывают все типы ресурсов:

* Владелец.  
* участник;  
* Читатель  

Владельцы и участники обладают полным доступом к действиям по управлению, но участник не может предоставлять доступ другим пользователям или группам. С ролью читателя не все так просто, поэтому мы уделим ей больше внимания. Дополнительные сведения о предоставлении доступа см. в статье [Использование назначений ролей для управления доступом к ресурсам в подписке Azure](role-based-access-control-configure.md).

## <a name="app-service-workloads"></a>Рабочие нагрузки службы приложений
### <a name="write-access-capabilities"></a>Возможности доступа на запись
Если предоставить пользователю доступ только для чтения к одному веб-приложению, некоторые компоненты могут неожиданно отключиться. Перечисленные ниже возможности управления требуют доступа к веб-приложению с правом **записи** (роль участника или владельца), поэтому они недоступны при наличии прав только на чтение.

* Команды (такие как запуск, остановка и т. д.).
* Изменение параметров, таких как общие параметры конфигурации, параметры масштабирования, резервного копирования и мониторинга.
* Доступ к учетным данным для публикации и другим секретным сведениям, например к параметрам приложений и строкам подключения.
* Журналы потоковой передачи.
* Конфигурация журналов диагностики.
* Консоль (командная строка).
* Активные и недавние развертывания (для локального непрерывного развертывания Git).
* Приблизительные затраты.
* Веб-тесты
* Виртуальная сеть (видна читателю только в том случае, если ранее была настроена пользователем с доступом на запись).

Если у вас нет доступа ни к одной из этих плиток, попросите администратора предоставить вам доступ к веб-приложению с правами участника.

### <a name="dealing-with-related-resources"></a>Работа со связанными ресурсами
Работа с веб-приложениями осложняется наличием нескольких взаимосвязанных ресурсов. Вот типичная группа ресурсов, связанная с несколькими веб-сайтами:

![Группа ресурсов веб-приложений](./media/role-based-access-control-troubleshooting/website-resource-model.png)

В результате, если вы предоставите кому-либо доступ только к веб-приложению, многие функции в колонке веб-сайта на портале Azure будут отключены.

Для этих элементов требуется доступ на **запись** к **плану службы приложений**, который соответствует вашему веб-сайту:  

* просмотр ценовой категории веб-приложения (например "Бесплатный" или "Стандартный");  
* параметры масштабирования (число экземпляров, размер виртуальной машины, настройки автоматического масштабирования);  
* квоты (квоты хранилища, пропускной способности, ресурсов ЦП).  

Элементы, требующие доступа на **запись** ко всей **группе ресурсов**, которая содержит веб-сайт:  

* SSL-сертификаты и привязки (SSL-сертификаты могут совместно использоваться сайтами, относящимися к одной группе ресурсов и находящимися в одном географическом расположении);  
* правила оповещений.  
* параметры автоматического масштабирования;  
* компоненты Application Insights;  
* Веб-тесты  

## <a name="virtual-machine-workloads"></a>Рабочие нагрузки виртуальной машины
Как и в случае с веб-приложениями, для некоторых функций в колонке виртуальной машины нужен доступ к виртуальной машине или другим ресурсам в группе ресурсов с правами на запись.

Виртуальные машины связаны с доменными именами, виртуальными сетями, учетными записями хранения и правилами оповещений.

Элементы, требующие доступа к **виртуальной** машине с правом **записи**:

* Конечные точки  
* IP-адреса;  
* диски;  
* расширения.  

Элементы, требующие доступа на **запись** как к **виртуальной машине**, так и к **группе ресурсов**, в которой она находится (а также к доменному имени):  

* Группа доступности  
* набор балансировки нагрузки;  
* правила оповещений.  

Если у вас нет доступа ни к одному из этих элементов, попросите администратора предоставить вам права участника для группы ресурсов.

## <a name="see-more"></a>Дополнительные сведения
* [Управление доступом на основе ролей.](role-based-access-control-configure.md) Начало работы с RBAC на портале Azure.
* [Встроенные роли.](role-based-access-built-in-roles.md) Сведения о стандартных ролях в RBAC.
* [Пользовательские роли в Azure RBAC](role-based-access-control-custom-roles.md). Сведения о создании пользовательских ролей в соответствии с потребностями доступа.
* [Создание отчета по журналу изменений доступа](role-based-access-control-access-change-history-report.md). Отслеживание изменения назначений ролей в RBAC.

