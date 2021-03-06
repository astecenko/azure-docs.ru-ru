---
title: "Анализ шаблонов навигации пользователя с помощью инструмента \"Маршруты пользователя\"в Azure Application Insights | Документация Майкрософт"
description: "Анализируйте то, как пользователи выполняют переходы между страницами и компонентами веб-приложения."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Анализ шаблонов навигации пользователя с помощью инструмента "Маршруты пользователя"в Azure Application Insights

![Инструмент "Маршруты пользователя" в Application Insights](./media/app-insights-usage-flows/flows.png)

Инструмент "Маршруты пользователя" позволяет анализировать то, как пользователи выполняют переходы между страницами и компонентами сайта. Он позволяет получить ответ на такие вопросы, как, например, следующие.
* Как пользователи покидают страницу на сайте?
* Какой элемент на странице нажимают пользователи сайта?
* Что на сайте отталкивает его посетителей больше всего?
* Существуют ли места на сайте, где пользователи снова и снова повторяют одни и те же действия?

Инструмент "Маршруты пользователя" начинает с просмотра начальной страницы или с указанного вами события. На основе этого просмотра страницы или пользовательского события инструмент показывает просмотры страницы и пользовательские события, отправленные пользователям сразу же после этого во время сеанса, за два действия после него, и т. д. Линии различной толщины показывают, сколько раз пользователи выбрали тот или иной путь. Особые узлы "Сеанс завершен" служат для того, чтобы показать, сколько пользователей не отправили просмотры страниц или пользовательские события после предшествующего узла, что позволяет выделить те моменты, когда пользователи, вероятно, покинули веб-сайт.



> [!NOTE]
> Ресурс Application Insights должен содержать просмотры страниц или пользовательские события для использования инструмента "Маршруты пользователя". [Узнайте, как настроить приложение для сбора сведений о просмотре страниц автоматически с помощью пакета SDK для JavaScript Application Insights](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Для начала выберите просмотр начальной страницы или пользовательское событие

![Выбор начального события для инструмента "Маршруты пользователя"](./media/app-insights-usage-flows/flows-initial-event.png)

Чтобы приступить к получению ответов на вопросы с помощью инструмента "Маршруты пользователя", выберите просмотр начальной страницы или пользовательское событие в качестве отправной точки для визуализации.
1. Щелкните ссылку на плитке "What do users do after...?" (Что делают пользователи после...?) или нажмите кнопку Edit (Изменить). 
2. Выберите просмотр страницы или пользовательское событие в раскрывающемся списке Initial event (Начальное событие).
3. Нажмите кнопку Create graph (Создать диаграмму).

В столбце Step 1 (Шаг 1) визуализации показано, что пользователи чаще всего делают после начального события (действия упорядочены сверху вниз по популярности). В столбце Step 2 (Шаг 2) и последующих столбцах показано, что пользователи делают в дальнейшем, что позволяет получить картину того, как пользователи перемещаются по веб-сайту.

По умолчанию инструмент "Маршруты пользователя" выполняет случайную выборку просмотров страниц и пользовательских событий с веб-сайта только за последние 24 часа. Можно увеличить период времени и изменить баланс производительности и точности случайной выборки в меню "Правка".

Чтобы скрыть ненужные просмотры страниц и пользовательские события, щелкните значок "X" для соответствующих узлов. После выбора узлов, которые нужно скрыть, нажмите кнопку Create graph (Создать диаграмму) под визуализацией. Чтобы отобразить все скрытые узлы, нажмите кнопку Edit (Изменить), а затем обратитесь к разделу Excluded events (Исключенные события).

Если просмотры страниц или пользовательские события, которые вы хотите увидеть в визуализации, отсутствуют.
* Перейдите к разделу Excluded events (Исключенные события) в меню "Правка".
* С помощью ползунка Detail level (Уровень детализации) в меню "Правка" в визуализацию можно включить достаточно редкие события.
* Если ожидаемые просмотр страницы или пользовательское событие выполняются редко, попробуйте увеличить диапазон времени визуализации в меню "Правка".
* Убедитесь, что ожидаемый просмотр страницы или пользовательское событие настроены на сбор пакетом SDK Application Insights в исходном коде веб-сайта. [Узнайте подробнее о создании пользовательских событий](app-insights-api-custom-events-metrics.md).

Чтобы отобразить в визуализации больше шагов, воспользуйтесь ползунком Number of steps (Число шагов) в меню "Правка".

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Куда переходят пользователи и что они нажимают после посещения страницы или компонента?

![Используйте инструмент "Маршруты пользователя", чтобы понять, где пользователи щелкают элементы сайта.](./media/app-insights-usage-flows/flows-one-step.png)

Если начальным событием является просмотр страницы, то первый столбец (Step 1 [Шаг 1]) визуализации позволяет быстро понять, что пользователи сделали сразу же после посещения страницы. Попробуйте открыть сайт в окне рядом с визуализацией в инструменте "Маршруты пользователя". Сравните ожидаемое взаимодействие пользователей с этой страницей со списком событий в столбце Step 1 (Шаг 1). Часто элемент пользовательского интерфейса, который кажется незначительным для команды, может входить в список наиболее часто используемых элементов на странице. Это может стать отличной отправной точкой для улучшения дизайна сайта.

Если начальным событием является пользовательское событие, то в первом столбце показано, что пользователи сделали сразу после выполнения этого действия. Как и в случае с просмотрами страниц, проверьте, совпадает ли поведение пользователей с целями и ожиданиями команды. Если, к примеру, выбрано начальное событие "Товар добавлен в корзину", обратите внимание в визуализации на то, появилось ли вскоре после этого событие "Переход к оформлению заказа" или "Покупка совершена". Если поведение пользователя сильно отличается от ожиданий, используйте визуализацию, чтобы понять, как пользователей "заинтересовывает" текущий дизайн веб-сайта.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Что на сайте отталкивает его посетителей больше всего?

Обратите внимание на большое количество узлов Session Ended (Сеанс завершен) в столбце визуализации, особенно на начальном этапе. Это означает, что многие пользователи, возможно, покинули веб-сайт после перехода по предыдущему пути просмотра страниц и взаимодействия с элементами пользовательского интерфейса. Иногда отток посетителей является ожидаемым явлением — например, после совершения покупки в интернет-магазине, — но обычно отток посетителей указывает на проблемы с дизайном, на плохую производительность или на иные проблемы с сайтом, которые следует устранить.

Помните, что узлы Session Ended (Сеанс завершен) основаны только на данных телеметрии, собираемых этим ресурсом Application Insights. Если Application Insights не получает данные телеметрии для определенных действий пользователя, пользователи по-прежнему могли бы взаимодействовать с сайтом этими способами после того, как инструмент "Маршруты пользователя" сообщил о завершении сеанса.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Существуют ли места на сайте, где пользователи снова и снова повторяют одни и те же действия?

Найдите просмотр страницы или пользовательское событие, которые повторяются несколькими пользователями на последующих шагах в визуализации. Обычно это означает, что пользователи выполняют повторяющиеся действия на вашем сайте. Если вы нашли такое повторение, рекомендуется изменить дизайн веб-сайта или добавить новые функции, чтобы уменьшить повторения. Например, можно добавить функцию массового редактирования, если вы обнаружили, что пользователи выполняют повторяющиеся действия в каждой строке элемента таблицы.

## <a name="common-questions"></a>Часто задаваемые вопросы

### <a name="why-do-steps-appear-disconnected"></a>Почему шаги отображаются несвязанными?

![Окно инструмента "Маршруты пользователей" с несвязанными шагами](./media/app-insights-usage-flows/flows-disconnected.png)

Если шаги (столбцы) в визуализации отображаются несвязанными, это означает, что отсутствуют пути, которыми посетители сайта пользовались достаточно часто, чтобы они отобразились в визуализации. Для отображения в визуализации более редких событий, чтобы шаги отображались связанными, отрегулируйте соответствующим образом ползунок Detail level (Уровень детализации) в меню "Правка".

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Начальные события представляют самые первые события во время сеанса или относятся к любому времени в сеансе?

Начальное событие в визуализации представляется только самый первый раз, когда пользователь отправил данные о просмотре страницы или пользовательском событии во время сеанса. Если пользователи могут отправлять начальные события несколько раз за сеанс, то в столбце Step 1 (Шаг 1) отображается только поведение пользователей после *первого* экземпляра начального события, а не все экземпляры.

## <a name="next-steps"></a>Дальнейшие действия

* [Общие сведения об использовании](app-insights-usage-overview.md)
* [Анализ пользователей, сеансов и событий в Application Insights](app-insights-usage-segmentation.md)
* [Сохранение](app-insights-usage-retention.md)
* [API Application Insights для пользовательских событий и метрик](app-insights-api-custom-events-metrics.md)