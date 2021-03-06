---
title: "Запуск и остановка виртуальных машин во время решения нерабочие часы (Предварительная версия) | Документы Microsoft"
description: "Это решение для управления виртуальной Машины запускает и останавливает виртуальных машин диспетчера ресурсов Azure по расписанию и заранее отслеживание из анализа журналов."
services: automation
documentationCenter: 
authors: eslesar
manager: carmonm
editor: 
ms.assetid: 06c27f72-ac4c-4923-90a6-21f46db21883
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: magoedte
ms.openlocfilehash: 4424cbb83bdb31c60e15d62f9387b4050611a98d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="startstop-vms-during-off-hours-solution-preview-in-azure-automation"></a>Запуск и остановка виртуальных машин во время решения нерабочие часы (Предварительная версия) в службе автоматизации Azure

Запуск и остановка виртуальных машин в нерабочие часы решение запускает и останавливает виртуальных машин Azure на пользовательские расписания, предоставляет закономерностей путем анализа журналов Azure и отправляет дополнительные сообщения электронной почты с помощью [SendGrid](https://azuremarketplace.microsoft.com/marketplace/apps/SendGrid.SendGrid?tab=Overview). В большинстве сценариев оно поддерживает виртуальные машины Azure Resource Manager и классические виртуальные машины. 

Это решение позволяет automation, децентрализованной для пользователей, чтобы уменьшить издержки, связанные с использованием ресурсов без сервера, недорогое. Это решение позволяет:

* Планирование виртуальных машин для запуска и остановки.
* Планирование виртуальных машин для запуска и остановки в порядке возрастания с использованием тегов Azure (не поддерживается для классических виртуальных машин).
* Auto остановка виртуальных машин на низкий уровень использования ЦП.

## <a name="prerequisites"></a>Технические условия

- Для проверки подлинности модулей Runbook используется [учетная запись запуска от имени Azure](automation-offering-get-started.md#authentication-methods).  Учетная запись запуска от имени является предпочтительным методом аутентификации, так как оно использует сертификат проверки подлинности вместо пароля, может истечь срок действия или часто изменяются.  

- Это решение управляет только виртуальные машины, которые находятся в той же подписке, ваша учетная запись службы автоматизации Azure.  

- Это решение развертывается только в следующих регионах Azure: Юго-Восточная Австралия, Канады центра, центральной Индии, восток США, восток Японии, Юго-Восточная Азия, Южная часть соединенного Королевства и Западной Европе.  
    
    > [!NOTE]
    > Модули Runbook, используемые для управления расписанием виртуальных машин, могут быть нацелены на виртуальные машины в любом регионе.  

- Для отправки уведомлений по электронной почте после запуска и остановки модулей Runbook виртуальной Машины, при адаптации из Azure Marketplace, выберите **Да** развертывание SendGrid. 

    > [!IMPORTANT]
    > SendGrid — это служба сторонних разработчиков. Для получения поддержки обратитесь к [SendGrid](https://sendgrid.com/contact/).  
    >
   
    Ниже приведены ограничения, связанные с SendGrid.

    * Более одной учетной записи SendGrid на пользователя в подписку.
    * Более двух учетных записей SendGrid для каждой подписки.

Если вы развернули предыдущей версии этого решения, вам необходимо сначала удалить его из вашей учетной записи до развертывания этой версии.  

## <a name="solution-components"></a>Компоненты решения

Это решение включает в себя предварительно настроенные модули Runbook, расписаний и интеграции с помощью аналитики журналов, можно адаптировать запуске и завершении работы виртуальных машин в соответствии с потребностями бизнеса. 

### <a name="runbooks"></a>Модули Runbook

В следующей таблице перечислены модули Runbook, развернут в учетную запись автоматизации.  Не следует вносить изменения в код runbook. Вместо этого напишите собственный модуль runbook для новых функциональных возможностей.

> [!IMPORTANT]
> Не запускайте все runbook непосредственно с «дочернего» добавляется к его имени.
>

Включить все модули Runbook родительского *WhatIf* параметра. Если задано значение **True**, *WhatIf* runbook принимает поддерживает с подробным описанием точное поведение при запуске без *WhatIf* параметр и проверяет правильность которой виртуальные машины выбрано целевым объектом.  Модуль runbook выполняет только определенные действия при *WhatIf* параметра равным **False**. 

|**Runbook** | **Параметры** | **Описание**|
| --- | --- | ---| 
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Вызывается только из родительского модуля runbook. Создает оповещения на основе каждого ресурса для AutoStop сценария.| 
|AutoStop_CreateAlert_Parent | WhatIf: true или false. <br> VMList | Создает или обновляет правила генерации оповещений Azure для виртуальных машин в целевой подписке или группах ресурсов. <br> VMList: Разделенный запятыми список виртуальных машин.  Например *vm1 vm2, vm3*.| 
|AutoStop_Disable | Нет | Отключает предупреждения AutoStop и расписание по умолчанию.| 
|AutoStop_StopVM_Child | WebHookData | Вызывается только из родительского модуля runbook. Правила генерации оповещений вызывать этот модуль runbook, чтобы остановить виртуальную Машину.|  
|Bootstrap_Main | Нет | Для настройки начальной загрузки конфигураций, таких как webhookURI, которые обычно не доступны из диспетчера ресурсов Azure, используется один раз. Этот runbook автоматически удаляется после успешного развертывания.|  
|ScheduledStartStop_Child | VMName <br> Action: Stop или Start. <br> ResourceGroupName | Вызывается только из родительского модуля runbook. Выполняется остановка или запуск запланированного остановки.|  
|ScheduledStartStop_Parent | Action: Stop или Start. <br> WhatIf: true или false. | Это влияет на все виртуальные машины в подписке. Изменить **External_Start_ResourceGroupNames** и **External_Stop_ResourceGroupNames** для выполнения только на этих целевых групп ресурсов. Можно также исключить определенные виртуальные машины, обновив переменную **External_ExcludeVMNames**. *WhatIf* работает так же, как и другие модули Runbook.|  
|SequencedStartStop_Parent | Action: Stop или Start. <br> WhatIf: true или false. | Создание тегов с именем **щелчковНачало** и **SequenceStop** на каждую виртуальную Машину, для которого требуется создать последовательность запуска и остановки действия. Значение тега должно быть положительным целым числом (1, 2, 3), соответствует порядку, в котором требуется запустить или остановить. *WhatIf* работает так же, как и другие модули Runbook. <br> **Примечание**: виртуальные компьютеры должны быть в пределах группы ресурсов, определенные как External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames и External_ExcludeVMNames в переменных службы автоматизации Azure. Они должны иметь соответствующие теги для действия вступили в силу.|

### <a name="variables"></a>Переменные

В следующей таблице перечислены переменных, созданных в учетной записи автоматизации.  Следует изменять только переменные с префиксом **внешних**. Изменение переменных с префиксом **внутренний** вызывает нежелательные эффекты.  

|**Variable** | **Описание**|
---------|------------|
|External_AutoStop_Condition | Условный оператор, необходимые для настройки условия до активации оповещения. Допустимые значения: **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, и **LessThanOrEqual**.|  
|External_AutoStop_Description | Предупреждение, чтобы остановить виртуальную Машину, если процент использования ЦП превышает пороговое значение.|  
|External_AutoStop_MetricName; | Имя метрики производительности, для которого требуется настроить правило генерации оповещений Azure.| 
|External_AutoStop_Threshold; | Пороговое значение для правила оповещения Azure, указанном в переменной *External_AutoStop_MetricName*. Процентные значения могут находиться в диапазоне от 1 до 100.|  
|External_AutoStop_TimeAggregationOperator; | Время статистический оператор, применяемый к размер выбранное окно для оценки условия. Допустимые значения: **среднее**, **минимум**, **максимальное**, **общее**, и **последний**.|  
|External_AutoStop_TimeWindow. | Размер окна, во время которого Azure анализирует выбранные показатели для инициации оповещения. Этот параметр принимает входные данные в формате временного диапазона. Допустимые значения — от 5 минут до 6 часов.|  
|External_EmailFromAddress | Указывает отправителя сообщения электронной почты.|  
|External_EmailSubject | Указывает текст темы сообщения электронной почты.|  
|External_EmailToAddress | Задает получателей сообщения электронной почты. Разделяйте имена с помощью запятой.|  
|External_ExcludeVMNames | Введите имена виртуальных Машин должны быть исключены, разделяя их запятыми без пробелов.|  
|External_IsSendEmail | Задает параметр для отправки уведомлений по электронной почте по завершении.  Укажите **Yes**, чтобы отправлять уведомление, и **No**, чтобы не отправлять его.  Этот параметр должен быть **нет** Если уведомления по электронной почте не была включена во время первоначального развертывания.|  
|External_Start_ResourceGroupNames | Указывает один или несколько групп ресурсов, разделяя значения запятыми, предназначенные для запуска действия.|  
|External_Stop_ResourceGroupNames | Указывает один или несколько групп ресурсов, разделяя значения запятыми, предназначенные для остановки действия.|  
|Internal_AutomationAccountName | Задает имя учетной записи службы автоматизации Azure.|  
|Internal_AutoSnooze_WebhookUri | Указывает универсальный код ресурса (URI) веб-перехватчика, вызываемого в сценарии AutoStop.|  
|Internal_AzureSubscriptionId | Указывает идентификатор подписки Azure.|  
|Internal_ResourceGroupName | Указывает имя группы ресурсов учетной записи автоматизации.|  
|Internal_SendGridAccountName | Указывает имя учетной записи SendGrid.|  
|Internal_SendGridPassword | Указывает пароль учетной записи SendGrid.|  

<br>

Для всех сценариев **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, и **External_ExcludeVMNames** переменные необходимы для нацеливания на виртуальных машинах, за исключением предоставляя запятыми список виртуальных машин для **AutoStop_CreateAlert_Parent** runbook. То есть виртуальные машины необходимо находятся в целевой группы ресурсов для начала и остановить выполнение действий. Логика немного работает как система Azure политики, в том, что можно взять подписка или группа ресурсов и действий наследуются только что созданный виртуальных машин. Такой подход позволяет избежать необходимости поддерживать отдельные расписания для каждой виртуальной Машины и управлять ими начинается и останавливается на шкале.

### <a name="schedules"></a>Расписания

В таблице ниже перечислены расписания по умолчанию, создаваемые в вашей учетной записи службы автоматизации.  Можно изменить их или создать собственные пользовательские расписания.  По умолчанию, каждый из них отключены, за исключением **Scheduled_StartVM** и **Scheduled_StopVM**.

Не следует включать все расписания, так как это может создавать перекрывающиеся расписания действия. Рекомендуется определить, какие оптимизации, необходимо выполнить и соответствующим образом изменить.  Дополнительные пояснения можно получить, ознакомившись с примерами сценариев в разделе "Обзор".   

|**Имя расписания** | **Frequency** | **Описание**|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Каждые 8 часов | Запускает модуль runbook AutoStop_CreateAlert_Parent каждые 8 часов, который в свою очередь останавливает значения на основе виртуальной Машины в External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames и External_ExcludeVMNames в переменных службы автоматизации Azure.  Кроме того можно указать список разделенных запятыми виртуальных машин с помощью параметра VMList.|  
|Scheduled_StopVM | Определяемые пользователем, ежедневно | Запускается с параметром Scheduled_Parent runbook *остановить* каждый день в указанное время.  Автоматически останавливает все виртуальные машины, которые соответствуют правилам, определенным актива переменных. Следует включать связанные расписания **StartVM запланированное**.|  
|Scheduled_StartVM | Определяемые пользователем, ежедневно | Запускается с параметром Scheduled_Parent runbook *запустить* каждый день в указанное время.  Автоматически запускает все виртуальные машины, которые соответствуют правилам, определенным соответствующих переменных.  Следует включать связанные расписания **StopVM запланированное**.|
|Sequenced-StopVM | 1:00 AM (UTC), каждую пятницу | Запускается с параметром Sequenced_Parent runbook *остановить* каждую пятницу в указанное время.  Последовательно (по возрастанию) останавливает все виртуальные машины с тегом **SequenceStop** определяется соответствующих переменных.  См. раздел модулей Runbook, Дополнительные сведения о значения тега и переменные активов.  Следует включать связанные расписания **StartVM последовательного**.|
|Sequenced-StartVM | 1:00 PM (UTC), каждый понедельник | Запускает runbook Scheduled_Parent с параметром *Start* каждый понедельник в указанное время. Последовательно (по убыванию) запускает все виртуальные машины с тегом **щелчковНачало** определяется соответствующих переменных.  См. раздел модулей Runbook, Дополнительные сведения о значения тега и переменные активов.  Следует включать связанные расписания **StopVM последовательного**.|

<br>

## <a name="configuration"></a>Параметр Configuration

Выполните следующие действия для добавления запуск и остановка виртуальных машин во время решения нерабочие часы к вашей учетной записи автоматизации, а затем настройте переменные для настройки решений.

1. На портале Azure нажмите кнопку **Создать**.<br> ![портал Azure](media/automation-solution-vm-management/azure-portal-01.png)<br>  
2. В области Marketplace введите ключевое слово, такие как **запустить** или **запуск и остановка**. Как только вы начнете вводить символы, список отфильтруется соответствующим образом. Кроме того можно введите один или несколько ключевых слов из полное имя решения и нажмите клавишу ВВОД.  Выберите **Запуск и остановка виртуальных машин в нерабочее время [предварительная версия]** в результатах поиска.  
3. В области **Запуск и остановка виртуальных машин в нерабочее время [предварительная версия]** просмотрите сводные данные для выбранного решения и щелкните **Создать**.  
4. **Добавить решение** появится область. Вам будет предложено настроить решение, прежде чем его можно импортировать в подписке автоматизации.<br><br> ![Колонка "Добавление решения" в решении по управлению виртуальными машинами](media/automation-solution-vm-management/azure-portal-add-solution-01.png)<br><br>
5.  На **добавить решение** выберите **рабочей**. Выберите рабочую область OMS, связанного с учетной записи автоматизации находится в одной подписке. Если у вас нет рабочей области, выберите **создать новую рабочую область**. На **рабочей области OMS** панель, выполните следующие действия: 
   - Укажите имя новой **рабочей области OMS**.
   - Выберите **подписки** для связывания, выбрав из раскрывающегося списка, если выбор по умолчанию не подходит.
   - Для **группы ресурсов**, можно создать новую группу ресурсов или выберите существующий.  
   - Выберите **расположение**.  В настоящее время являются только местоположений, доступных **Юго-Восточная Австралия**, **Канада центра**, **центральной Индии**, **Восток США**, **Восточной Японии,**, **Юго-Восточная Азия**, **Южная часть соединенного Королевства**, и **Западной Европе**.
   - Выберите **ценовую категорию**.  Решение обеспечивает два уровня: освободите и платные OMS.  Уровень free имеет ограничение на объем собираемых данных, ежедневно, срок хранения и минут времени выполнения задания runbook.  На платном уровне (OMS) ограничение по объему ежедневно собираемых данных отсутствует.  

        > [!NOTE]
        > Несмотря на то, что на уровне каждого ГБ (автономный) платная отображается в качестве альтернативы, он неприменим.  Если вы выберите его и продолжить создание этого решения в вашей подписке, происходит сбой.  Это будет исправлено в официальном выпуске решения.<br>Это решение только использует автоматизации задания и минут приема журнала.  Не добавляет дополнительные узлы OMS в вашей среде.  

6. После предоставления необходимых сведений на **рабочей области OMS** области, нажмите кнопку **создать**.  Вы можете отслеживать ход его выполнения в группе **уведомления** меню, чтобы вернуться в **добавить решение** области после завершения.  
7. На **добавить решение** выберите **учетной записи автоматизации**.  Если вы создаете новую рабочую область OMS, необходимо также создать новую учетную запись автоматизации необходимо сопоставить с ним.  Выберите **создать учетную запись автоматизации**и на **учетной записи автоматизации, добавьте** области обеспечивают следующие: 
  - В поле **Имя** введите имя учетной записи службы автоматизации.

    Все остальные параметры заполняются автоматически в зависимости от выбранной рабочей области OMS. Эти параметры нельзя изменить.  По умолчанию проверка подлинности модулей Runbook, добавленных в это решение, осуществляется с помощью учетной записи запуска от имени Azure.  После нажатия кнопки **ОК**, проверяются параметры конфигурации и создания учетной записи автоматизации.  Ход создания можно просмотреть в разделе **Уведомления** в меню. 

    В противном случае можно выбрать имеющуюся учетную запись запуска от имени службы автоматизации.  Обратите внимание, что при выборе учетной записи не может быть связан уже другую рабочую область OMS. Если он уже связан, сообщение об ошибке, а затем нужно выбрать другую учетную запись запуска от автоматизации или создайте новую.<br><br> ![Учетная запись службы автоматизации, связанная с рабочей областью OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>

8. Наконец, на **добавить решение** выберите **конфигурации**. **Параметры** появится область.<br><br> ![Область параметров решения](media/automation-solution-vm-management/azure-portal-add-solution-02.png)<br><br>  Здесь отображается запрос на:  
   - Укажите **целевая группа ресурсов имена**. Это имена групп ресурсов, которые содержат виртуальных машин, управляемых с помощью этого решения.  Можно ввести несколько имен и каждым с помощью запятой (значения не учитывается регистр).  Если необходимо использовать виртуальные машины во всех группах ресурсов в рамках подписки, используйте подстановочный знак.
   - Укажите **списка исключений виртуальной Машины (string)**. Это имя одного или нескольких виртуальных машин из целевой группы ресурсов.  Можно ввести несколько имен и каждым с помощью запятой (значения не учитывается регистр).  Поддерживается использование подстановочных знаков.
   - Выберите **расписание**. Это повторяющееся даты и времени запуска и остановки виртуальных машин в целевой группы ресурсов.  По умолчанию расписание настраивается часовой пояс UTC. Выбрать другой регион недоступен.  Настройка расписания для конкретных часовой пояс после настройки решения, в разделе [изменения расписания запуска и завершения работы](#modifying-the-startup-and-shutdown-schedule).
   - Чтобы получать **уведомления по электронной почте** из SendGrid, оставьте значение по умолчанию **Да** и укажите адрес электронной почты. При выборе **нет** , но впоследствии решить, что вы хотите получать уведомления по электронной почте, вы можете обновить **External_EmailToAddress** переменную с допустимых адресов электронной почты, разделенных запятыми, а затем изменить переменную **External_IsSendEmail** со значением **Да**.  

9. После настройки начальной настройки, необходимые для решения выберите **создать**.  После проверки всех параметров развертывания решения для вашей подписки.  Этот процесс может занять несколько секунд для завершения, и вы можете отслеживать ход его выполнения в группе **уведомления** в меню. 

## <a name="collection-frequency"></a>Частота сбора

Автоматизация задания журнала и задания потока данных полученный в репозиторий анализа журналов каждые 5 минут.  

## <a name="using-the-solution"></a>Использование решения

При добавлении решения для управления виртуальной Машины, **StartStopVMView** Плитка добавляется на панель мониторинга в рабочей области аналитики журналов на портале Azure.  Эта Плитка отображается количество и графическое представление заданий runbook для решения, который запущен и завершена успешно.<br><br> ![Плитка StartStopVM View (Представление запуска и остановки виртуальных машин) в решении по управлению виртуальными машинами](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

В учетной записи автоматизации, вы можете обращаться и решения для управления, выбрав **рабочей** параметр. На странице "Аналитика журналов" выберите **решения** в левой области.  На **решения** выберите решение **Start-Stop-VM [рабочей области]** из списка.<br><br> ![Список решений в Log Analytics](media/automation-solution-vm-management/azure-portal-select-solution-01.png)  

При выборе решения отображаются **Start-Stop-VM [рабочей области]** панель решения. Здесь можно просмотреть важные сведения, такие как **StartStopVM** плитки. Как в рабочей области аналитики журналов этой плитки отображается количество и графическое представление заданий runbook для решения, которые были запущены и завершена успешно.<br><br> ![Страница обновления решения по управлению в службе автоматизации](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)  

Здесь можно выполнять дополнительные анализ записей задания, щелкнув плитку кольцо. Панели мониторинга решение показывает журнал заданий и предварительно определенные запросы поиска журналов. Переключиться на портале Advanced Analytics журнала для поиска на основании поисковому запросу.  

Включить все модули Runbook родительского *WhatIf* параметра. Если задано значение **True**, это поддерживает, с подробными сведениями о точное поведение, выполняемые модулем runbook при запуске без *WhatIf* , который проверяет целевой правильный виртуальных машин. Модули Runbook только выполнения определенных действий при *WhatIf* параметра равным **False**.  


### <a name="scenario-1-daily-startstop-vms-across-a-subscription-or-target-resource-groups"></a>Сценарий 1: Ежедневно запуск и остановка виртуальных машин на подписку или целевых групп ресурсов 

Это конфигурация по умолчанию, используемая после первого развертывания решения.  Например можно настроить к остановке всех виртуальных машин для подписки при выходе работы по расписанию и их запуска утра, когда вы будете в офисе. При настройке расписания **запланированное StartVM** и **StopVM запланированное** во время развертывания, их запуска и остановки целевые виртуальные машины.
>[!NOTE]
>Часовой пояс — часового пояса, при настройке параметра расписания time. Тем не менее сохраняется в формате UTC в службе автоматизации Azure.  Нет необходимости преобразовывать часовой пояс, так как это делается во время развертывания.

Можно выбирать, какие виртуальные машины находятся в области видимости, настроив следующие переменные: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, и **External_ ExcludeVMNames**.  

>[!NOTE]
> Значение для **имена целевых ResourceGroup** хранится в качестве значения для обоих **External_Start_ResourceGroupNames** и **External_Stop_ResourceGroupNames**. Чтобы увеличить степень детализации, можно изменить эти переменные для различных групп ресурсов.  Действие при запуске, используйте **External_Start_ResourceGroupNames**и действие остановки использовать **External_Stop_ResourceGroupNames**. Виртуальные машины, автоматически добавляются в начале и остановки расписаний.

Для тестирования и проверки конфигурации, вручную запустите **ScheduledStartStop_Parent** runbook и задать для параметра действие **запустить** или **остановить** и WHATIF параметр **True**.<br><br> ![Настройка параметров для runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)


 Предварительный просмотр Запланированное действие и внесите необходимые изменения перед реализацией для производственного виртуальных машин.  Можно вручную запустить runbook с параметром в **False**, или разрешить расписание автоматизации **расписания StartVM** и **расписания StopVM** запуска автоматически после предписанных расписания.

### <a name="scenario-2-sequence-the-startstop-vms-across-a-subscription-by-using-tags"></a>Сценарий 2: Запуск/остановка виртуальных машин через подписку последовательностей с помощью тегов
В среде, которая включает два или более компонентов, на несколько виртуальных машин, поддерживающий распределенную рабочую нагрузку поддерживающие последовательности запущенных и остановленных компонентов в порядке важно.  Это можно сделать, выполнив следующие действия:

1. Добавить **щелчковНачало** и **SequenceStop** тег с положительным целым числом для виртуальных машин, которые являются целевыми в **External_Start_ResourceGroupNames** и  **External_Stop_ResourceGroupNames** переменных.  Начало и окончание действия выполняются в порядке возрастания.  Дополнительную информацию о теге виртуальной Машины см. в разделе [тегом виртуальной машины Windows Azure](../virtual-machines/windows/tag.md) и [тега виртуальной машины Linux в Azure](../virtual-machines/linux/tag.md).
2. Изменение расписания **последовательного StartVM** и **StopVM последовательного** Дата и время, соответствующие требованиям и включить расписание.  

Для тестирования и проверки конфигурации, вручную запустите **SequencedStartStop_Parent** runbook. Значение параметра ACTION **запустить** или **остановить** и параметр WHATIF для **True**.<br><br> ![Настройка параметров для runbook](media/automation-solution-vm-management/solution-startrunbook-parameters-01.png)<br> Предварительный просмотр действия и внесите необходимые изменения перед реализацией для производственного виртуальных машин.  Когда готовый, вручную выполните runbook с параметром в **False**, или оставить расписание автоматизации **последовательного StartVM** и **StopVM последовательного** запуска автоматически после предписанных расписания.  

### <a name="scenario-3-automate-startstop-vms-across-a-subscription-based-on-cpu-utilization"></a>Сценарий 3: Автоматизировать запуск и остановка виртуальных машин в зависимости от загрузки ЦП подписки
Это решение может помочь сократить затраты на работающих виртуальных машин в вашей подписке, путем вычисления виртуальных машинах Azure, которые не используются в периоды наименьшей, например, в нерабочее и автоматически завершает их работу в случае использования процессора, меньше значения x %.  

По умолчанию решением является предварительно настроенным для оценки метрики ЦП процент, является ли средняя загрузка 5 процентов или менее.  Это управляется следующие переменные и могут быть изменены, если значения по умолчанию не соответствуют вашим требованиям:

* External_AutoStop_MetricName;
* External_AutoStop_Threshold;
* External_AutoStop_TimeAggregationOperator;
* External_AutoStop_TimeWindow.

Можно включить, предназначенных для действия в отношении подписки и группы ресурсов, либо предназначенных для конкретных список виртуальных машин, но не оба.  

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Нацеливание действия Stop на подписку и группу ресурсов

1. Настройте переменные **External_Stop_ResourceGroupNames** и **External_ExcludeVMNames** для указания целевых виртуальных машин.  
2. Включите и обновите расписание **Schedule_AutoStop_CreateAlert_Parent**.
3. Запустите **AutoStop_CreateAlert_Parent** runbook с параметром в действие **запустить** и параметр WHATIF, равным **True** для предварительного просмотра изменений.

#### <a name="target-the-stop-action-by-vm-list"></a>Целевую действие остановки, список виртуальных Машин

1. Запустите **AutoStop_CreateAlert_Parent** runbook с параметром в действие **запустить**, добавить виртуальные машины в список разделенных запятыми *VMList* параметра, а затем установите Параметр WHATIF для **True**. Предварительный просмотр изменений.  
2. Настройка **External_ExcludeVMNames** параметр с разделителями запятыми список виртуальных машин (VM1 VM2, VM3).
3. Этот сценарий не учитывает **External_Start_ResourceGroupNames** и **External_Stop_ResourceGroupnames** переменных.  Для этого сценария необходимо создать свое собственное расписание автоматизации. Дополнительные сведения см. в разделе [планирование runbook в автоматизации Azure](../automation/automation-schedules.md).

Теперь, когда расписание для остановки виртуальных машин по ЦП, необходимо включить один из следующих расписания для их запуска.

* Действие при запуске целевой подписки и группы ресурсов.  См. в [сценария 1](#scenario-1:-daily-stop/start-vms-across-a-subscription-or-target-resource-groups) для тестирования и включение **StartVM запланированное** расписания.
* Действие подписки, группы ресурсов и тег при запуске целевой.  См. в [сценарии 2](#scenario-2:-sequence-the-stop/start-vms-across-a-subscription-using-tags) для тестирования и включение **StartVM последовательного** расписания.


### <a name="configuring-email-notifications"></a>Настройка уведомлений по электронной почте

Чтобы настроить уведомления по электронной почте после развертывания решения, измените следующих трех переменных:

* External_EmailFromAddress: Укажите адрес электронной почты отправителя.
* External_EmailToAddress: Укажите список адресов электронной почты, разделенных запятыми (user@hotmail.com, user@outlook.com) для получения уведомлений по электронной почте.
* External_IsSendEmail: Значение **Да** для получения сообщений электронной почты. Чтобы отключить уведомления по электронной почте, задайте значение **нет**.   


### <a name="modifying-the-startup-and-shutdown-schedules"></a>Изменение расписаний запуска и завершения работы

Действия по управлению расписанием запуска и завершения работы в этом решении аналогичны описанным в статье [Создание расписания для Runbook в службе автоматизации Azure](automation-schedules.md).     

## <a name="log-analytics-records"></a>Записи Log Analytics

Автоматизация создает два типа записей в репозитории OMS: журналы задания и задания потоков.

### <a name="job-logs"></a>Журналы заданий

Свойство | ОПИСАНИЕ|
----------|----------|
Caller |  Сторона, инициировавшая операцию.  Допустимые значения: электронный адрес или system для запланированных заданий.|
Категория | Классификация типа данных.  Для службы автоматизации значением является JobLogs.|
CorrelationId | GUID, представляющий собой идентификатор корреляции задания Runbook.|
JobId | GUID, представляющий собой идентификатор задания Runbook.|
operationName | Указывает тип операции, выполняемой в Azure.  Для службы автоматизации значением является Job.|
ResourceId | Указывает тип ресурса в Azure.  Для службы автоматизации значением является учетная запись службы автоматизации, связанная с Runbook.|
ResourceGroup | Указывает имя группы ресурсов задания Runbook.|
ResourceProvider | Указывает службу Azure, которая предоставляет ресурсы для развертывания и управления.  Для службы автоматизации значением будет Azure Automation.|
ResourceType | Указывает тип ресурса в Azure.  Для службы автоматизации значением является учетная запись службы автоматизации, связанная с Runbook.|
resultType | Состояние задания Runbook.  Возможные значения:<br>Started<br>- Остановлена<br>Приостановлено<br>Сбой<br>- Succeeded.|
resultDescription | Описывает состояние результата задания Runbook.  Возможные значения:<br>— Job is started;<br>— Job Failed;<br>- Job Completed.|
RunbookName | Указывает имя модуля Runbook.|
SourceSystem | Указывает исходную систему для отправленных данных.  Для автоматизации значение равно OpsManager|
StreamType | Задает тип события. Возможные значения:<br>- Verbose.<br>— Output;<br>error<br>Предупреждение|
SubscriptionId | Указывает идентификатор подписки задания.
Время | Дата и время выполнения задания Runbook.|


### <a name="job-streams"></a>Потоки заданий

Свойство | ОПИСАНИЕ|
----------|----------|
Caller |  Сторона, инициировавшая операцию.  Допустимые значения: электронный адрес или system для запланированных заданий.|
Категория | Классификация типа данных.  Для службы автоматизации значением является JobStreams.|
JobId | GUID, представляющий собой идентификатор задания Runbook.|
operationName | Указывает тип операции, выполняемой в Azure.  Для службы автоматизации значением является Job.|
ResourceGroup | Указывает имя группы ресурсов задания Runbook.|
ResourceId | Указывает идентификатор ресурса в Azure.  Для службы автоматизации значением является учетная запись службы автоматизации, связанная с Runbook.|
ResourceProvider | Указывает службу Azure, которая предоставляет ресурсы для развертывания и управления.  Для службы автоматизации значением будет Azure Automation.|
ResourceType | Указывает тип ресурса в Azure.  Для службы автоматизации значением является учетная запись службы автоматизации, связанная с Runbook.|
resultType | Результат задания Runbook во время создания события. Возможным значением является:<br>- InProgress.|
resultDescription | Включает в себя выходной поток из Runbook.|
RunbookName | Имя Runbook.|
SourceSystem | Указывает исходную систему для отправленных данных.  Для автоматизации значение равно OpsManager.|
StreamType | Тип потока задания. Возможные значения:<br>-Выполняется<br>Выходные данные<br>Предупреждение<br>error<br>debug<br>- Verbose.|
Время | Дата и время выполнения задания Runbook.|

При выполнении любой поиска журналов, возвращающее записи категории **JobLogs** или **JobStreams**, можно выбрать **JobLogs** или **JobStreams**представление, в котором отображается набор плиток формирования сводных данных обновлений, полученных в результате поиска.

## <a name="sample-log-searches"></a>Пример поисков журналов

Следующая таблица содержит примеры поисков по журналу для получения записей заданий, собранных этим решением. 

Запрос | ОПИСАНИЕ|
----------|----------|
Найти задания для runbook ScheduledStartStop_Parent, завершено успешно | search Category == "JobLogs" &#124; where ( RunbookName_s == "ScheduledStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc|
Найти задания для runbook SequencedStartStop_Parent, завершено успешно | search Category == "JobLogs" &#124; where ( RunbookName_s == "SequencedStartStop_Parent" ) &#124; where ( ResultType == "Completed" )  &#124; summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) &#124; sort by TimeGenerated desc

## <a name="removing-the-solution"></a>Удаление решения

Если вы решите, что больше не требуется использовать решения, его можно удалить из учетной записи автоматизации.  Удаление решения удаляет только модули Runbook. Не удаляет расписания или переменные, которые были созданы при добавлении решения.  Эти ресурсы, которые необходимо удалить вручную, если вы не используете их с другими модулями Runbook.  

Чтобы удалить решение, сделайте следующее:

1.  Используя свою учетную запись службы автоматизации, выберите **Рабочая область** в левой области.  
2.  На странице **Решения** выберите решение **Start-Stop-VM[Workspace]**.  На **VMManagementSolution [рабочей области]** страницы, в меню выберите **удалить**.<br><br> ![Удаление решения VMManagement](media/automation-solution-vm-management/vm-management-solution-delete.png)
3.  В **удалить решение** окна, убедитесь, что вы хотите удалить решение.
4.  Пока проверяются данные, ход удаления решения можно проверить в разделе **Уведомления** в меню.  Вы вернетесь к **решения** страницу после запуска процесса удаления решения.  

В ходе этого процесса рабочая область Log Analytics и учетная запись службы автоматизации не удаляются.  Если вы не хотите сохранять с рабочей областью аналитики журналов, необходимо вручную удалить его.  Это можно сделать на портале Azure:

1.    Azure портала начальный экран, выберите **анализа журналов**.
2. На **анализа журналов** области, выберите рабочую область.
3. Выберите **удалить** меню на панели параметров.  
      
## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о способах создания разных поисковые запросы и просмотрите журналы заданий службы автоматизации с помощью аналитики журналов см. в разделе [входа поиска аналитики журналов](../log-analytics/log-analytics-log-searches.md).
- Дополнительные сведения о выполнении модулей Runbook, отслеживании заданий модуля Runbook и других технических деталях см. в статье [Выполнение модуля Runbook в службе автоматизации Azure](automation-runbook-execution.md).
- Дополнительные сведения о службе анализа журналов и коллекцию источников данных см. в разделе [Azure сбор данных из хранилища в обзоре анализа журналов](../log-analytics/log-analytics-azure-storage.md).






   

