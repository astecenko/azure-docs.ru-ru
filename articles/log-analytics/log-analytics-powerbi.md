---
title: "Импорт данных Log Analytics Azure в Power BI | Документация Майкрософт"
description: "Power BI — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  В этой статье описываются способы настройки импорта данных Log Analytics в Power BI и настройка этой службы для автоматического обновления."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/27/2017
ms.author: bwren
ms.openlocfilehash: 163ac33af43a8cb7a23742f6336efca5fe7c4b4e
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Импорт данных Log Analytics Azure в Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) — это облачная служба бизнес-аналитики от Майкрософт, предоставляющая расширенную визуализацию данных и отчеты по анализу различных наборов данных.  Результаты поиска по журналам Log Analytics можно импортировать в набор данных Power BI, чтобы можно было воспользоваться такими функциями, как объединение данных из различных источников и совместное использование отчетов в веб-устройствах и мобильных устройствах.

Эта статья содержит сведения об импорте данных Log Analytics в Power BI и о планировании автоматического обновления.  Для [обновленной](#upgraded-workspace) и рабочей области [прежней версии](#legacy-workspace) включены различные процессы.

## <a name="upgraded-workspace"></a>Обновленная рабочая область.


Для импорта данных из [обновленной рабочей области Log Analytics](log-analytics-log-search-upgrade.md) в Power BI создается набор данных в Power BI на основе запроса поиска по журналам в службе Log Analytics.  Запрос выполняется каждый раз при обновлении набора данных.  Затем можно создавать отчеты Power BI, которые используют данные из набора данных.  Чтобы создать набор данных в Power BI, нужно экспортировать запрос из Log Analytics в [язык Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx).  Потом этот импортированный запрос используется для создания запроса в Power BI Desktop, а затем публикуется в Power BI в качестве набора данных.  Ниже описаны подробности этого процесса.

![Из Log Analytics в Power BI](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Экспорт запроса
Нужно начать с создания [поиска по журналам](log-analytics-log-search-new.md), который возвращает данные из Log Analytics, которые в свою очередь необходимы для заполнения набора данных Power BI.  Затем экспортируйте этот запрос в [язык Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx), который может использоваться в Power BI Desktop.

1. Создайте поиск по журналам в Log Analytics для извлечения данных для своего набора данных.
2. Если вы используете портал поиска по журналам, щелкните **Power BI**.  Если вы используете портал Analytics, выберите **Экспорт** > **Power BI Query (M)**.  Оба этих параметра экспортируют запрос в текстовый файл с именем **PowerBIQuery.txt**. 

    ![Экспорт поиска по журналам](media/log-analytics-powerbi/export-logsearch.png) ![Экспорт поиска по журналам](media/log-analytics-powerbi/export-analytics.png)

3. Откройте текстовый файл и скопируйте его содержимое.

### <a name="import-query-into-power-bi-desktop"></a>Импорт запроса в Power BI Desktop
Power BI Desktop — это классическое приложение, позволяющее создавать наборы данных и отчеты, которые можно опубликовать в Power BI.  Его также можно использовать для создания запросов на языке Power Query, экспортированном из Log Analytics. 

1. Установите приложение [Power BI Desktop](https://powerbi.microsoft.com/desktop/), если у вас его еще нет, а затем откройте его.
2. Чтобы открыть новый запрос, выберите **Получить данные** > **Пустой запрос**.  Затем выберите **Расширенный редактор** и вставьте содержимое экспортированного файла в запрос. Нажмите кнопку **Done**(Готово).

    ![Запрос Power BI Desktop](media/log-analytics-powerbi/desktop-new-query.png)

5. Запрос выполняется и его результаты отображаются.  Возможно потребуется ввести учетные данные для подключения к Azure.  
6. Введите описательное имя для запроса.  По умолчанию используется значение **Query1**. Чтобы добавить набор данных в отчет, нажмите кнопку **Close and Apply** (Закрыть и применить).

    ![Имя Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Публикация в Power BI
При публикации в Power BI создается набор данных и отчет.  При создании отчета в Power BI Desktop он будет опубликован с вашими данными.  Если нет, то будет создан пустой отчет.  В Power BI можно изменить отчет или создать новый на основе набора данных.

8. Создайте отчет на основе данных.  Прочитайте [документацию по Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view), если вы не знакомы с ней.  Когда вы будете готовы отправить отчет в Power BI, выберите **Опубликовать**.  При появлении запроса выберите место назначения в своей учетной записи Power BI.  При отсутствии определенного места назначения можно использовать область **Моя рабочая область**.

    ![Публикация в Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Чтобы открыть Power BI с новым набором данных, после завершения публикации щелкните **Открыть в Power BI**.


### <a name="configure-scheduled-refresh"></a>Настройка запланированного обновления
В наборе данных, созданном в Power BI, будут содержаться те же данные, что и ранее в Power BI Desktop.  Чтобы снова запустить запрос и заполнить его последними данными из Log Analytics, нужно периодически обновлять набор данных.  

1. Щелкните в рабочей области, где находится переданный отчет, и выберите меню **Наборы данных**. Откройте контекстное меню нового набора данных и выберите **Параметры**. В разделе **Учетные данные источника данных** должно быть указано, что учетные данные недействительны.  Это связано с тем, что учетные данные, используемые при обновлении данных в наборе данных, еще не указаны.  Щелкните **Изменить учетные данные** и укажите учетные данные, позволяющие получить доступ к Log Analytics.

    ![Расписание Power BI](media/log-analytics-powerbi/powerbi-schedule.png)

5. В разделе **Запланированное обновление** включите переключатель в области **Поддерживать актуальность данных**.  При необходимости можно изменить **периодичность обновления**, а также задать одну или несколько точек времени для обновления.

    ![Обновление Power BI](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Рабочая область прежней версии.
Настраивая Power BI с использованием [рабочей области Log Analytics прежней версии](log-analytics-powerbi.md), вы создаете запросы журналов, экспортирующие свои результаты в соответствующие наборы данных в Power BI.  Запросы и экспорт продолжают автоматически выполняться по расписанию, установленному вами для обновления набора данных в соответствии с последними данными, собранными службой Log Analytics.

![Из Log Analytics в Power BI](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Расписания Power BI
*Расписание Power BI* включает поиск по журналам, который экспортирует набор данных из репозитория OMS в соответствующий набор данных в Power BI, а также расписание, определяющее периодичность выполнения этого поиска для актуализации набора данных.

Поля в наборе данных должны соответствовать свойствам записей, возвращаемых в результате поиска по журналам.  Если поиск выдает записи разных типов, набор данных включает все свойства каждого из включенных типов записей.  

### <a name="connecting-oms-workspace-to-power-bi"></a>Подключение рабочей области OMS к Power BI
Прежде чем экспортировать данные из службы Log Analytics в Power BI, необходимо подключить рабочую область OMS к учетной записи Power BI, выполнив указанные ниже действия.  

1. В консоли OMS щелкните элемент **Параметры** .
2. Выберите **Учетные записи**.
3. В разделе **Сведения о рабочей области** щелкните **Подключиться к учетной записи Power BI**.
4. Введите данные учетной записи Power BI.

### <a name="create-a-power-bi-schedule"></a>Создание расписания Power BI
Создайте расписание Power BI для каждого набора данных, выполнив указанные ниже действия.

1. В консоли OMS щелкните элемент **Поиск по журналам** .
2. Введите новый запрос или выберите сохраненный поиск, возвращающий данные, которые нужно экспортировать в **Power BI**.  
3. Нажмите кнопку **Power BI** в верхней части страницы, чтобы открыть диалоговое окно **Power BI**.
4. Введите данные в следующую таблицу и нажмите кнопку **Сохранить**.

| Свойство | Описание |
|:--- |:--- |
| Действие |Имя для идентификации расписания при просмотре списка расписаний Power BI. |
| Сохраненные поисковые запросы |Поисковый запрос, который нужно выполнить.  Вы можете выбрать текущий запрос и сохраненный поисковый запрос из раскрывающегося списка. |
| Расписание |Периодичность выполнения сохраненного поискового запроса и экспорта в набор данных Power BI.  Значение должно составлять от 15 минут до 24 часов. |
| Имя набора данных |Имя набора данных в Power BI.  Если оно не существует, то создается, а если существует, то обновляется. |

### <a name="viewing-and-removing-power-bi-schedules"></a>Просмотр и удаление расписаний Power BI
Просмотрите список существующих расписаний Power BI, выполнив указанные ниже действия.

1. В консоли OMS щелкните элемент **Параметры** .
2. Выберите **Power BI**.

Наряду с информацией о расписании отображаются данные о том, сколько раз расписание было выполнено за прошлую неделю, а также состояние последней синхронизации.  Если при синхронизации возникают ошибки, щелкните ссылку для поиска записей, содержащих сведения об ошибке, по журналам.

Расписание можно удалить, щелкнув **X** в столбце **Удалить столбец**.  Расписание можно отключить, выбрав параметр **Откл**.  Чтобы изменить расписание, необходимо удалить его и создать новое с новыми параметрами.

![Расписания Power BI](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Пример пошагового руководства
В следующем разделе демонстрируется создание расписания Power BI и использование его набора данных для создания простого отчета.  В этом примере все данные о производительности по набору компьютеров экспортируются в Power BI, после чего создается линейный график, который показывает загрузку процессора.

#### <a name="create-log-search"></a>Создание запроса поиска по журналу
Для начала создадим запрос для поиска данных, которые нужно отправить в набор данных.  В этом примере будет использоваться запрос, возвращающий все данные о производительности компьютеров, имена которых начинаются с *srv*.  

![Расписания Power BI](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Создания запроса для поиска по Power BI
Мы нажимаем кнопку **Power BI** , чтобы открыть диалоговое окно Power BI и ввести необходимые данные.  Мы хотим, чтобы этот поиск выполнялся один раз в час и создавал набор данных с именем *Contoso Perf*.  Так как у нас уже есть поисковый запрос, выдающий необходимые данные, для параметра *Сохраненный поисковый запрос* мы оставляем значение по умолчанию **Использовать текущий поисковый запрос**.

![Поиск Power BI](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Проверка поиска по Power BI
Чтобы проверить, правильно ли создано расписание, мы просматриваем список поисковых запросов Power BI в элементе **Параметры** на панели мониторинга OMS.  Мы ждем несколько минут и обновляем представление, пока оно не сообщит, что синхронизация выполнена.  Обычно планируется автоматическое обновление набора данных.

![Поиск Power BI](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Проверка набора данных в Power BI
Мы входим в свою учетную запись на сайте [powerbi.microsoft.com](http://powerbi.microsoft.com/) и прокручиваем экран до пункта **Datasets** (Наборы данных) в нижней части левой панели.  Как видите, набор данных *Contoso Perf* присутствует в списке с указанием на то, что экспорт был выполнен успешно.

![Набор данных Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Создание отчета на основе набора данных
Мы выбираем набор данных **Contoso Perf** и щелкаем **Results** (Результат) на панели **Fields** (Поля) справа, чтобы просмотреть, какие поля входят в этот набор данных.  Чтобы создать линейный график, отражающий использование процессора на каждом компьютере, мы выполняем следующие действия:

1. Выбираем визуализацию линейного графика.
2. Перетаскиваем параметр **ObjectName** (Имя объекта) в **Report level filter** (Фильтр уровней отчетности) и устанавливаем флажок **Processor** (Процесс).
3. Перетаскиваем параметр **CounterName** (Имя счетчика) в **Report level filter** (Фильтр уровней отчетности) и устанавливаем флажок **% Processor Time** (Процент процессорного времени).
4. Перетаскиваем параметр **CounterValue** (Значение счетчика) в поле **Values** (Значения).
5. Перетаскиваем параметр **Computer** (Компьютер) в поле **Legend** (Условные обозначения).
6. Перетаскиваем параметр **TimeGenerated** (Время создания) в поле **Axis** (Ось).

В результате отображается линейный график с данными из набора данных.

![График Power BI](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Сохранение отчета
Мы сохраняем отчет, нажав кнопку "Сохранить" в верхней части экрана, и проверяем, отображается ли он в разделе Report (Отчет) на левой панели.

![Отчеты Power BI](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше о [запросах поиска по журналам](log-analytics-log-searches.md) , чтобы создавать запросы, которые можно экспортировать в Power BI.
* Узнайте больше о [Power BI](http://powerbi.microsoft.com), чтобы создавать визуализации на основе экспорта Log Analytics.
