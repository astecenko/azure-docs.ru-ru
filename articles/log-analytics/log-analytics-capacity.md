---
title: "Решение \"Емкость и производительность\" в Azure Log Analytics | Документация Майкрософт"
description: "Используйте решение \"Емкость и производительность\" в службе Log Analytics, чтобы оценивать ресурсы серверов Hyper-V Server."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: 031a538c7e3a7dd381fa9bd996d8a027f761a50a
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2017
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-preview"></a>Планирование ресурсов виртуальной машины Hyper-V с помощью решения "Емкость и производительность" (предварительная версия)

![Символ "Емкость и производительность"](./media/log-analytics-capacity/capacity-solution.png)

Решение "Емкость и производительность" в службе Log Analytics позволяет оценивать ресурсы серверов Hyper-V Server. Это решение предоставляет подробные сведения о среде Hyper-V, отображая данные о совокупном использовании (ресурсов ЦП, памяти и дисков) узлов и виртуальных машин, запущенных на этих узлах Hyper-V. Кроме того, оно собирает метрики ЦП, памяти и дисков на всех узлах и их виртуальных машинах.

Это решение:

-   показывает узлы с максимальным и минимальным использованием памяти и ресурсов ЦП;
-   показывает виртуальные машины с максимальным и минимальным использованием памяти и ресурсов ЦП;
-   показывает виртуальные машины с максимальным и минимальным использованием пропускной способности и числом операций ввода-вывода в секунду;
-   показывает, какие виртуальные машины запущены на конкретных узлах;
-   показывает диски с самой высокой пропускной способностью, числом операций ввода-вывода в секунду и задержкой в общих томах кластера;
- позволяет выполнять настройку и фильтрацию на основе групп.

> [!NOTE]
> Предыдущая версия решения "Емкость и производительность" называлась "Управление ресурсами". Для работы этого решения требовались System Center Operations Manager и System Center Virtual Machine Manager. В обновленном решении эти зависимости не применяются.


## <a name="connected-sources"></a>Подключенные источники

В следующей таблице описаны подключенные источники, которые поддерживаются этим решением.

| Подключенный источник | Поддержка | ОПИСАНИЕ |
|---|---|---|
| [Агенты Windows](log-analytics-windows-agent.md) | Yes | Решение собирает сведения о емкости и производительности из агентов Windows. |
| [Агенты Linux](log-analytics-linux-agents.md) | Нет     | Решение не собирает сведения о емкости и производительности из прямых агентов Linux.|
| [Группы управления SCOM](log-analytics-om-agents.md) | Yes |Решение собирает сведения о емкости и производительности из агентов в подключенной группе управления SCOM. Прямое подключение агента SCOM к OMS не требуется. Данные пересылаются из группы управления в репозиторий OMS.|
| [Учетная запись хранения Azure](log-analytics-azure-storage.md) | Нет  | Служба хранилища Azure не содержит сведения о емкости и производительности.|

## <a name="prerequisites"></a>Технические условия

- Агенты Windows или Operations Manager должны быть установлены на узлах Hyper-V под управлением Windows Server 2012 (или более поздней версии), а не на виртуальных машинах.


## <a name="configuration"></a>Параметр Configuration

Чтобы добавить решение "Емкость и производительность" в рабочую область, сделайте следующее:

- Добавьте решение "Емкость и производительность" в рабочую область OMS, используя указания в статье [Добавление решений для управления Azure Log Analytics в рабочую область](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Пакеты управления

Если группа управления SCOM подключена к рабочей области OMS, при добавлении этого решения в SCOM будут установлены следующие пакеты. Никакая настройка или обслуживание для этих пакетов управления не требуются.

- Microsoft.IntelligencePacks.CapacityPerformance

Событие 1201 выглядит так:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

При обновлении решения "Емкость и производительность" номер версии изменится.

Дополнительные сведения об обновлении пакетов управления для решений см. в статье [Подключение Operations Manager к Log Analytics](log-analytics-om-agents.md).

## <a name="using-the-solution"></a>Использование решения

После добавления решения "Емкость и производительность" в рабочую область оно отображается на панели мониторинга "Обзор". На этой плитке отображается количество активных узлов Hyper-V и виртуальных машин, которые отслеживались в течение указанного периода времени.

![Плитка "Емкость и производительность"](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Просмотр сведений об использовании

Щелкните плитку "Емкость и производительность", чтобы открыть соответствующую панель мониторинга. Панель мониторинга содержит столбцы, перечисленные в приведенной ниже таблице. В каждом столбце содержится максимум десять элементов, соответствующих таким указанным критериям, как область действия и диапазон времени. Можно выполнить поиск по журналам, в результате которого возвращаются все записи, если щелкнуть заголовок этого столбца или **Показать все** внизу столбца.

- **Узлы**
    - **Загрузка ЦП узла**. Показывает в виде графика сведения об использовании ресурсов ЦП главных компьютеров в течение указанного периода времени, а также содержит список узлов. Чтобы просмотреть сведения на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть показатели счетчика ЦП для размещенных виртуальных машин, щелкните любое имя узла.
    - **Использование памяти узла**. Показывает в виде графика сведения об использовании памяти главных компьютеров в течение указанного периода времени, а также содержит список узлов. Чтобы просмотреть сведения на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть показатели счетчика памяти для размещенных виртуальных машин, щелкните любое имя узла.
- **Виртуальные машины**
    - **Загрузка ЦП ВМ**. Показывает в виде графика сведения об использовании ресурсов ЦП виртуальных машин в течение указанного периода времени, а также содержит список виртуальных машин. Чтобы просмотреть сведения для 3 первых виртуальных машин на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть объединенные показатели счетчика ЦП для виртуальной машины, щелкните любое имя виртуальной машины.
    - **Использование памяти виртуальной машиной.** Показывает в виде графика сведения об использовании ресурсов памяти виртуальных машин в течение указанного периода времени, а также содержит список виртуальных машин. Чтобы просмотреть сведения для 3 первых виртуальных машин на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть объединенные показатели счетчика памяти для виртуальной машины, щелкните любое имя виртуальной машины.
    - **Общее число дисковых операций ввода-вывода в секунду на ВМ**. Показывает в виде графика сведения об общем числе дисковых операций ввода-вывода в секунду виртуальных машин в течение указанного периода времени, а также содержит список виртуальных машин с операциями ввода-вывода в секунду каждой из них. Чтобы просмотреть сведения для 3 первых виртуальных машин на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть объединенные показатели счетчика дисковых операций ввода-вывода в секунду для виртуальной машины, щелкните любое имя виртуальной машины.
    - **Общая пропускная способность дисков виртуальной машины**. Показывает в виде графика сведения об общей пропускной способности дисков виртуальных машин в течение указанного периода времени, а также содержит список виртуальных машин с общей пропускной способностью дисков каждой из них. Чтобы просмотреть сведения для 3 первых виртуальных машин на определенный момент времени, наведите указатель мыши на график. Щелкните график, чтобы просмотреть дополнительные сведения в поиске по журналам. Чтобы открыть поиск по журналам и просмотреть объединенные показатели счетчика общей пропускной способности дисков для виртуальной машины, щелкните любое имя виртуальной машины.
- **Общие тома кластера**
    - **Общая пропускная способность**. Показывает сумму операций чтения и записи на общих томах кластера.
    - **Общее число операций ввода-вывода**. Показывает сумму операций ввода-вывода в секунду на общих томах кластера.
    - **Общая задержка**. Показывает общую задержку на общих томах кластера.
- **Плотность узла**. Первая плитка содержит сведения об общем числе узлов и виртуальных машин, доступных в решении. Щелкните эту плитку, чтобы просмотреть дополнительные сведения в поиске по журналам. Кроме того, здесь содержится список всех узлов, а также сведения о числе виртуальных машин, размещенных на этих узлах. Чтобы просмотреть подробные результаты для виртуальной машины в поиске по журналам, щелкните имя узла.


![Колонка "Узлы" на панели мониторинга](./media/log-analytics-capacity/dashboard-hosts.png)

![Колонка "Виртуальные машины" на панели мониторинга](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Оценка производительности

Рабочие вычислительные среды значительно отличаются в разных организациях. Кроме того, емкость и производительность может зависеть от того, как запущены виртуальные машины, и от того, что вы считаете нормальным. Определенные процессы, которые помогают оценить производительность, вероятно, не применяются к вашей среде. Поэтому в этом случае лучше использовать более обобщенные конкретные рекомендации. Корпорация Майкрософт публикует ряд статей с конкретными рекомендациями по измерению производительности.

Подводя итог, можно отметить, что это решение собирает сведения о емкости и производительности из различных источников, в том числе счетчиков производительности. Используйте эти сведения, представленные в различных областях решения, и сравните свои результаты с результатами, приведенными в статье [Measuring Performance on Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) (Измерение производительности на виртуальных машинах Hyper-V). Хотя эта статья опубликована ранее, приведенные в ней метрики, рекомендации и указания по-прежнему актуальны. В ней также содержатся ссылки на другие полезные ресурсы.


## <a name="sample-log-searches"></a>Пример поисков журналов

В приведенной ниже таблице содержатся примеры поисков по журналу для получения сведений о емкости и производительности, собранных и рассчитанных этим решением.

| Запрос | ОПИСАНИЕ |
|---|---|
| Конфигурация памяти всех узлов | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Конфигурация памяти всех виртуальных машин | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Показатель общего числа дисковых операций ввода-вывода в секунду на всех виртуальных машинах | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Показатель общей пропускной способности дисков на всех виртуальных машинах | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Показатель общего числа операций ввода-вывода в секунду на всех общих томах кластера | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Показатель общей пропускной способности на всех общих томах кластера | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Показатель общей задержки на всех общих томах кластера | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |

>[!NOTE]
> Если ваша рабочая область переведена на [язык запросов Log Analytics](log-analytics-log-search-upgrade.md), указанные выше запросы будут изменены следующим образом.

> | Запрос | ОПИСАНИЕ |
|:--- |:--- |
| Конфигурация памяти всех узлов | Perf &#124; where ObjectName == "Capacity and Performance" and CounterName == "Host Assigned Memory MB" &#124; summarize MB = avg(CounterValue) by InstanceName |
| Конфигурация памяти всех виртуальных машин | Perf &#124; where ObjectName == "Capacity and Performance" and CounterName == "VM Assigned Memory MB" &#124; summarize MB = avg(CounterValue) by InstanceName |
| Показатель общего числа дисковых операций ввода-вывода в секунду на всех виртуальных машинах | Perf &#124; where ObjectName == "Capacity and Performance" and (CounterName == "VHD Reads/s" or CounterName == "VHD Writes/s") &#124; summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Показатель общей пропускной способности дисков на всех виртуальных машинах | Perf &#124; where ObjectName == "Capacity and Performance" and (CounterName == "VHD Read MB/s" or CounterName == "VHD Write MB/s") &#124; summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Показатель общего числа операций ввода-вывода в секунду на всех общих томах кластера | Perf &#124; where ObjectName == "Capacity and Performance" and (CounterName == "CSV Reads/s" or CounterName == "CSV Writes/s") &#124; summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Показатель общей пропускной способности на всех общих томах кластера | Perf &#124; where ObjectName == "Capacity and Performance" and (CounterName == "CSV Reads/s" or CounterName == "CSV Writes/s") &#124; summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |
| Показатель общей задержки на всех общих томах кластера | Perf &#124; where ObjectName == "Capacity and Performance" and (CounterName == "CSV Read Latency" or CounterName == "CSV Write Latency") &#124; summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), CounterName, InstanceName |


## <a name="next-steps"></a>Дальнейшие действия
* Используйте [поиск по журналам в Log Analytics](log-analytics-log-searches.md), чтобы просмотреть подробные сведения о емкости и производительности.
