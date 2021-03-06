---
title: "Отслеживание работоспособности и использования приложения с помощью Application Insights"
description: "Приступая к работе с Application Insights. Анализ использования, доступности и производительности локальных приложений или веб-приложений Microsoft Azure."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: mbullwin
ms.openlocfilehash: 32000f5a85c84913aa820df00f1bb7f877bf037f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-performance-in-web-applications"></a>Отслеживание производительности в веб-приложениях


Она позволяет удостовериться, что приложение работает с хорошей производительностью, и быстро выяснить, были ли сбои. [Application Insights][start] сообщает о любых проблемах с производительностью и исключениях, помогая выявлять и диагностировать их причины.

Application Insights можно отслеживать веб-приложения и службы Java и ASP.NET, а также службы WCF. Они могут размещаться локально, на виртуальных машинах, а также как веб-сайты Microsoft Azure. 

На стороне клиента Application Insights может извлекать телеметрию с веб-страниц и разнообразных устройств, включая приложения для iOS, Android и Магазина Windows.

>[!Note]
> Мы добавили новые возможности для поиска медленно выполняющихся страниц в веб-приложении. Если у вас нет доступа к ним, включите их, настроив параметры предварительной версии с помощью [колонки предварительной версии](app-insights-previews.md). Об этих новых возможностях можно прочитать в разделе [Поиск и устранение узких мест производительности с помощью интерактивного исследования производительности](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Настройка мониторинга производительности
Если вы еще не добавили Application Insights в проект (т. е. если в нем нет файла ApplicationInsights.config), вы можете приступить к работе с этой службой одним из следующих способов:

* [Веб-приложения ASP.NET](app-insights-asp-net.md)
  * [Добавление мониторинга исключений](app-insights-asp-net-exceptions.md)
  * [Добавление мониторинга зависимостей](app-insights-monitor-performance-live-website-now.md)
* [Веб-приложения J2EE](app-insights-java-get-started.md)
  * [Добавление мониторинга зависимостей](app-insights-java-agent.md)

## <a name="view"></a>Просмотр метрик производительности
На [портале Azure](https://portal.azure.com)перейдите к ресурсу Application Insights, настроенному для приложения. В колонке "Обзор" отображаются основные данные производительности:

Щелкните любую диаграмму, чтобы увидеть результаты более подробно и за больший период. Например, щелкните плитку "Запросы" и выберите временной диапазон.

![Щелкните плитки, чтобы увидеть подробные данные, и выберите временной диапазон](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Щелкните диаграмму, чтобы выбрать, какие метрики отображать, а также добавить новую диаграмму и выбрать метрики для нее.

![Щелкните график, чтобы выбрать метрики](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Отмените выбор всех метрик** , чтобы увидеть полный набор доступных метрик. Метрики разделены на группы: если выбран один из элементов группы, отображаются только метрики этой группы.

## <a name="metrics"></a>Что все это означает? Плитки и отчеты о производительности
Имеются различные метрики производительности, которые можно получить. Начнем с тех, которые отображаются в колонке приложения по умолчанию.

### <a name="requests"></a>Requests (Запросы)
Это число HTTP-запросов, полученных за указанный период. Сравните это значение с результатами в других отчетах, чтобы узнать, как ведет себя приложение при изменении нагрузки.

HTTP-запросы включают в себя все запросы GET и POST для страниц, данных и изображений.

Чтобы просмотреть счетчики для конкретных URL-адресов, щелкните плитку.

### <a name="average-response-time"></a>Average response time (Среднее время ответа)
Это время от поступления веб-запроса в приложение до возвращения ответа.

Точки показывают скользящее среднее. Если запросов много, для некоторых из них значения могут отклоняться от среднего, но при этом на графике не будет явных пиков или провалов.

Обращайте внимание на необычные пики. Как правило, время ответа растет с увеличением числа запросов. Если этот рост непропорционален, возможно, приложение исчерпало какой-либо ресурс, например ЦП или емкость используемой службы.

Чтобы просмотреть значения для конкретных URL-адресов, щелкните плитку.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Slowest requests (Самые медленные запросы)
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Показывает, какие запросы могут потребовать оптимизации производительности.

### <a name="failed-requests"></a>Failed requests (Неудачные запросы)
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Это счетчик запросов, которые вызвали не перехваченные исключения.

Чтобы просмотреть подробные сведения о конкретных сбоях, щелкните плитку. Чтобы просмотреть подробные сведения об отдельном запросе, выберите его. 

Для детального изучения доступна только репрезентативная выборка сбоев.

### <a name="other-metrics"></a>Другие метрики
Чтобы увидеть, какие еще метрики доступны, щелкните график и снимите все флажки метрик. Чтобы просмотреть определение конкретной метрики, щелкните значок (i).

![Снимите все флажки, чтобы увидеть полный набор метрик](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Выбор любой метрики отключит остальные, и они не будут отображаться на этой диаграмме.

## <a name="set-alerts"></a>Задайте предупреждения
Чтобы получать уведомления по электронной почте о нетипичных значениях любой метрики, добавьте предупреждение. Можно выбрать отправку либо по электронной почте администраторам учетной записи, либо на указанный электронный адрес.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Выбирайте ресурс прежде других свойств. Не выбирайте ресурсы webtest, если нужно задать предупреждения метрик производительности или использования.

Аккуратно указывайте единицы измерения для пороговых значений.

*Отсутствует кнопка «Добавить оповещение».* Вы используете учетную запись группы с доступом только для чтения? Обратитесь к администратору учетной записи.

## <a name="diagnosis"></a>Диагностические проблемы
Вот некоторые советы по выявлению и диагностике проблем с производительностью:

* настройте [веб-тесты][availability], чтобы получать оповещения, когда веб-сайт выходит из строя либо отвечает неправильно или медленно; 
* сравните число запросов с другими метриками, чтобы узнать, не связаны ли сбои или медленный ответ с нагрузкой;
* [вставьте в код инструкции трассировки и выполняйте поиск по ним][diagnostic], чтобы упростить выявление проблем.
* Отслеживайте работу веб-приложения с помощью [Live Metrics Stream][livestream].
* Запишите состояние приложения .NET с помощью [отладчика моментальных снимков][snapshot].

>[!Note]
> Изучение производительности Application Insights расширяется до интерактивного полноэкранного исследования. Разделы ниже содержат сведения о таком переходе, а также о прежнем исследовании производительности (если оно остается актуальным), которое по-прежнему доступно.

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-full-screen-performance-investigation"></a>Поиск и устранение узких мест производительности с помощью интерактивного полноэкранного исследования производительности

Вы можете использовать новую функцию интерактивного исследования производительности Application Insights для обнаружения операций с низкой производительностью в веб-приложении. Вы можете быстро выбрать определенную операцию с низкой производительностью и использовать [Профилировщик](app-insights-profiler.md), чтобы определить основную причину низкой производительности операций в коде. Используя новое распространение длительности, отображаемое для выбранной операции, вы можете практически за секунды определить ее качество производительности для клиентов. Кроме того, для каждой операции с низкой производительностью вы можете увидеть количество затронутых взаимодействий с пользователем. В примере ниже мы решили более подробно ознакомиться с операцией получения сведений о клиенте. В распространении длительности видно три пика. Пик слева составляет 400 мс и представляет достаточно высокую скорость. Средний пик составляет 1,2 с и представляет среднестатистическую скорость. И наконец, еще один небольшой пик 3,6 с, который представляет 99 процентилей, что совсем разочарует клиентов. Эта скорость в 10 раз медленнее, чем первая скорость выше для той же операции. 

![Три пика длительности операции получения сведений о клиенте](./media/app-insights-web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Чтобы получить более точное представление о взаимодействиях с пользователем для этой операции, можно выбрать более широкий диапазон времени. Затем можно также ограничить время по определенному временному окну, где скорость выполнения операции была самой низкой. В примере ниже мы переключили диапазон времени с 24 часов по умолчанию на 7 дней и выбрали временное окно с 9:47 до 12:47 между вторником (12) и средой (13). Обратите внимание, что распространение длительности и количество трассировок образца и профилировщика обновлены в правой области.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Чтобы рассмотреть медленные операции, выберем длительности между процентилями 95–99. Они представляют 4 % от взаимодействий с пользователем, скорость которых была значительно ограничена.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Теперь мы можем ознакомиться с типичными примерами, нажав кнопку "Образцы", или с типичными трассировками профилировщика, нажав кнопку трассировок профилировщика. В рамках этого примера имеется 4 трассировки, собранные для операции получения сведений о клиенте с определенным временным окном и диапазоном времени.

Иногда проблема заключается не в коде, а в зависимости, которую он вызывает. Вы можете переключиться на вкладку "Зависимости" в представлении рассмотрения производительности, чтобы более подробно ознакомиться с зависимостями с низкой производительностью. Обратите внимание, что по умолчанию представление производительности стремится к средним значениям, но большую важность имеет 95 процентиль (или 99, если вы отслеживаете сформировавшуюся службу). В примере ниже рассматривается медленная зависимость большого двоичного объекта Azure, где мы вызываем PUT fabrikamaccount. Хорошая скорость кластера составляет около 40 мс, в то время как скорость медленных вызовов одной зависимости в три раза меньше — 120 мс. Потребуется немного этих вызовов, чтобы значительно снизить скорость соответствующих операций. Вы можете более подробно ознакомиться с типичными примерами и трассировками профилировщика, а также вкладкой "Операции".

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/app-insights-web-monitor-performance/SlowDependencies95thTrend.png)

Другой мощной функцией, новой для интерактивного полноэкранного исследования производительности, является интеграция с аналитикой. Application Insights может обнаружить и отображать аналитические данные о снижении скорости реагирования, а также помочь определить общие свойства в необходимом примере набора. Чтобы просмотреть все доступные аналитики, рекомендуется переключиться на диапазон времени 30 дней, а затем выбрать параметр "Всего" для просмотра аналитики всех операций за прошлый месяц.

![Три пика длительности операции с диапазоном времени 7 дней и временным окном](./media/app-insights-web-monitor-performance/Performance30DayOveralllnsights.png)

Application Insights в новом представлении рассмотрения производительности может помочь найти любую причину, которая приводит к снижению производительности веб-приложений.

## <a name="deprecated-find-and-fix-performance-bottlenecks-with-a-narrow-bladed-legacy-performance-investigation"></a>Не рекомендуется: поиск и устранение узких мест производительности с помощью прошлых исследований производительности

Вы можете использовать устаревшую функцию исследования производительности Application Insights для обнаружения областей веб-приложения, которые снижают общую производительность. Вы можете найти определенные страницы, которые замедляют работу, и использовать [Профилировщик](app-insights-profiler.md) для трассировки основной причины низкой производительности операций в коде. 

### <a name="create-a-list-of-slow-performing-pages"></a>Создание списка медленно выполняющихся страниц 

Чтобы найти проблемы с производительностью, прежде всего необходимо получить список медленно выполняющихся страниц. Приведенный ниже снимок экрана демонстрирует использование колонки "Производительность" для получения списка потенциальных страниц для дальнейшего анализа. На этой странице вы можете легко увидеть, что задержка времени отклика приложения произошла приблизительно в 18:00, а затем еще раз — приблизительно в 22:00. Также вы увидите, что во время операции получения сведений о клиенте выполнялись долго выполняющиеся операции со средним временем отклика 507,05 мс. 

![Интерактивная производительность Application Insights](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Просмотр определенных страниц

Получив моментальный снимок производительности приложения, можно получить дополнительные сведения о медленно выполняющихся операциях. Щелкните любую операцию в списке, чтобы увидеть подробные сведения, как показано ниже. На диаграмме можно увидеть, была ли производительность основана на зависимости. Также можно увидеть, у скольких пользователей было различное время отклика. 

![Колонка "Операции" в Application Insights](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Просмотр данных для определенного периода времени

Определив точки во времени для исследования, углубитесь еще больше, чтобы рассмотреть определенные операции, которые могли вызвать снижение производительности. Щелкнув определенную точку во времени, вы получите подробности о странице, как это показано ниже. В примере ниже можно увидеть операции для данного периода времени, а также коды ответа сервера и длительность операции. Также имеется URL-адрес для открытия рабочего элемента TFS, если необходимо отправить эти сведения команде разработчиков.

![Временной срез Application Insights](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Просмотр подробных сведений об определенной операции

Определив точки во времени для исследования, углубитесь еще больше, чтобы рассмотреть определенные операции, которые могли вызвать снижение производительности. Щелкните операцию в списке, чтобы просмотреть подробности о ней, как показано ниже. В этом примере видно, что произошел сбой операции и Application Insights предоставила сведения об исключении, которое выдало приложение. Опять же, вы можете легко создать рабочий элемент TFS из этой колонки.

![Колонка "Операции" в Application Insights](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Дальнейшие действия
[Веб-тесты][availability] — настройте отправку веб-запросов к приложению через равные интервалы из разных точек мира.

[Сбор данных трассировки диагностики и поиск по ним][diagnostic] — добавьте вызовы трассировки и анализируйте результаты для выявления проблем.

[Отслеживание использования][usage] — получайте информацию о том, как люди используют ваше приложение.

[Устранение неполадок][qna] — вопросы и ответы



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



