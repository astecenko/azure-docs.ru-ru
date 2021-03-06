---
title: "Получение журналов действий Azure в службе Log Analytics в разных подписках | Документация Майкрософт"
description: "Использование концентраторов событий и Logic Apps для сбора данных из журналов действий Azure и их отправка в рабочее пространство Azure Log Analytics в другом клиенте."
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Получение журналов действий Azure в службе Log Analytics в разных подписках

В этой статье описывается метод сбора журналов действий Azure в рабочем пространстве Log Analytics с использованием соединителя сборщика данных Azure Log Analytics для Logic Apps. Следуйте приведенной здесь процедуре, когда вам нужно отправить журналы в рабочее пространство другого клиента Azure Active Directory. Например, если вы являетесь поставщиком управляемых служб, вы можете собирать журналы действий из подписки клиента и хранить их в рабочей области Log Analytics в своей собственной подписке.

Если рабочее пространство Log Analytics находится в одной и той же подписке Azure или в другой подписке, но в том же клиенте Azure Active Directory, следуйте шагам в статье [Просмотр журналов действий Azure](../log-analytics/log-analytics-activity.md), чтобы собрать журналы действий Azure.

## <a name="overview"></a>Обзор

Стратегия, используемая в этом сценарии, заключается в том, чтобы журнал действий Azure отправлял события в [концентратор событий](../event-hubs/event-hubs-what-is-event-hubs.md), в котором [приложение логики](../logic-apps/logic-apps-overview.md) отправляет их в рабочее пространство Log Analytics. 

![изображение потока данных из журнала действий в Log Analytics](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Преимущества этого подхода:
- Низкая задержка, так как журнал действий Azure передается потоком в концентратор событий.  Затем приложение логики запускается и отправляет данные в Log Analytics. 
- Требуется минимальный объем кода и не нужно развертывать инфраструктуру сервера.

В этой статье описано, как выполнить следующие действия:
1. Создайте концентратор событий. 
2. Экспортировать журналы действий в концентратор событий с помощью профиля экспорта журнала действий Azure.
3. Создать приложение логики для считывания событий из концентратора событий и отправки их в Log Analytics.

## <a name="requirements"></a>Требования
Ниже приведены требования относительно ресурсов Azure, используемых в этом сценарии.

- Пространство имен концентратора событий не должно быть в той же подписке, в которой создаются журналы. Пользователю, который настраивает этот параметр, должно быть предоставлено соответствующее разрешение на доступ к обеим подпискам. Если у вас несколько подписок в одном и том же клиенте Azure Active Directory, вы можете отправлять журналы действий для всех подписок в один концентратор событий.
- Приложение логики может находиться в подписке, отличной от подписки концентратора событий, и не должно принадлежать к одному и тому же клиенту Azure Active Directory. Приложение логики считывает события из концентратора событий, используя общий ключ доступа концентратора событий.
- Рабочее пространство Log Analytics может находиться в другой подписке и клиенте Azure Active Directory, нежели приложение логики, но для простоты мы рекомендуем, чтобы они принадлежали к одной подписке. Приложение логики отправляет данные в Log Analytics, используя идентификатор рабочей области и ключ Log Analytics.



## <a name="step-1---create-an-event-hub"></a>Шаг 1. Создание концентратора событий

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. На портале Azure выберите **Создать**> **Интернет вещей** > **Концентраторы событий**.

   ![Новый концентратор событий в Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. В диалоговом окне **создания пространства имен** введите новое пространство имен или выберите существующее. Система немедленно проверяет, доступно ли оно.

   ![изображение диалогового окна создания концентратора событий](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Выберите ценовую категорию ("Базовый" или "Стандартный"), подписку Azure, группу ресурсов и расположение для нового ресурса.  Щелкните **Создать** , чтобы создать пространство имен. Полная подготовка ресурсов для системы может занять несколько минут.
6. Выберите в списке пространство имен, которое вы только что создали.
7. В колонке **Политики общего доступа** щелкните **RootManageSharedAccessKey**.

   ![изображение политик общего доступа к концентраторам событий](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. Нажмите кнопку копирования, чтобы скопировать строку подключения **RootManageSharedAccessKey** в буфер обмена. 

   ![изображение общего ключа доступа к концентраторам событий](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. Во временном расположении, например в Блокноте, сохраните копию имени концентратора событий, а также основную или вторичную строку подключения к концентратору событий. Приложению логики требуются эти значения.  Для строки подключения к концентратору событий можно использовать строку подключения **RootManageSharedAccessKey** или создать отдельную.  Используемая строка подключения должна начинаться с `Endpoint=sb://` и предназначаться для политики, которая имеет политику доступа **Управление**.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Шаг 2. Передача журналов действий в концентратор событий

Чтобы включить потоковую передачу журнала действий, выберите пространство имен концентратора событий и политику общего доступа для этого пространства имен. Концентратор событий создается в этом пространстве имен, когда происходит первое событие журнала действий. 

Вы можете использовать пространство имен концентратора событий, которое не входит в подписку, в которой создаются журналы, однако подписки должны принадлежать к одному клиенту Azure Active Directory. Пользователю, который настраивает этот параметр, должен быть предоставлен соответствующий уровень доступа RBAC к обеим подпискам. 

1. На портале Azure выберите **Монитор** > **Журнал действий**.
3. Нажмите кнопку **Экспорт** в верхней части страницы.

   ![изображение Azure Monitor в области навигации](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Выберите **подписку** для экспорта и щелкните **Выбрать все** в раскрывающемся списке **Регионы**, чтобы выбрать события для ресурсов во всех регионах. Установите флажок **Экспорт в концентратор событий**.
7. Щелкните **Пространство имен служебной шины** и выберите **подписку** с концентратором событий, **пространство имен концентратора событий** и **имя политики концентратора событий**.

    ![изображение страницы экспорта в концентратор событий](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Нажмите кнопку **ОК**, а затем **Сохранить**, чтобы сохранить эти параметры. Параметры будут немедленно применены к подписке

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Шаг 3. Создание приложения логики

Когда журналы действий записываются в концентратор событий, вы создаете приложение логики для сбора журналов из концентратора событий и их записи в Log Analytics.

Приложение логики включает в себя следующее:
- Триггер [соединителя концентратора событий](https://docs.microsoft.com/connectors/eventhubs/) для считывания из концентратора событий.
- [Действие "Анализ JSON"](../logic-apps/logic-apps-content-type.md) для извлечения событий JSON.
- [Действие составления](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) для преобразования JSON в объект.
- [Соединитель отправки данных Log Analytics](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) для публикации данных в Log Analytics.

   ![изображение триггера концентратора событий в Logic Apps](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Требования для приложения логики
Прежде чем создавать приложение логики, убедитесь, что у вас есть следующие сведения из предыдущих шагов:
- имя концентратора событий;
- строка подключения к концентратору событий (первичная или вторичная) для пространства имен концентратора событий;
- идентификатор рабочей области Log Analytics;
- общий ключ Log Analytics.

Чтобы получить имя концентратора событий и строку подключения, выполните действия, описанные в разделе [Проверка разрешений для пространства имен концентраторов событий и определение строки подключения](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Создание пустого приложения логики

1. На портале Azure выберите **Создать ресурс** > **Интеграция Enterprise** > **Приложение логики**.

    ![Новое приложение логики в Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Введите параметры в таблице, приведенной ниже.

    ![Создание приложения логики](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Параметр | ОПИСАНИЕ  |
   |:---|:---|
   | ИМЯ           | Уникальное имя для приложения логики. |
   | Подписка   | Выберите подписку Azure, в которой будет размещаться приложение логики. |
   | Группа ресурсов | Выберите для приложения логики существующую группу ресурсов или создайте новую. |
   | Расположение       | Выберите регион центра обработки данных для развертывания приложения логики. |
   | Служба Log Analytics  | Выберите, если нужно регистрировать состояние каждого запуска приложения логики в Log Analytics.  |

    
3. Нажмите кнопку **Создать**. При отображении уведомления **Развертывание прошло успешно** щелкните **Перейти к ресурсу**, чтобы открыть приложение логики.

4. В разделе **Шаблоны** выберите **Пустое приложение логики**. 

В конструкторе Logic Apps теперь отображаются доступные соединители и их триггеры, которые используются для запуска рабочего процесса приложения логики.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Добавление триггера концентратора событий

1. В конструкторе приложений логики введите в поле поиска текст фильтра *концентраторы событий*. Выберите триггер концентраторов событий с именем **При наличии событий в концентраторе событий**.

   ![изображение триггера концентратора событий в Logic Apps](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Когда появится запрос учетных данных, подключитесь к пространству имен концентраторов событий. Введите имя для своего подключения, а затем скопированную строку подключения.  Нажмите кнопку **Создать**.

   ![изображение добавления триггера концентратора событий в приложениях логики](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. После создания подключения измените параметры триггера. Для начала выберите **insights-operational-logs** из раскрывающегося списка **Имя концентратора событий**.

   ![Диалоговое окно "При наличии событий в концентраторе событий"](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Разверните **Показать дополнительные параметры** и измените **тип содержимого** на *приложение/json*

   ![Диалоговое окно конфигурации концентратора событий](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Добавление действия "Анализ JSON"

Выходные данные в концентратора событий содержат полезные данные JSON с массивом записей. Действие [Анализ JSON](../logic-apps/logic-apps-content-type.md) используется для извлечения только массива записей для отправки в Log Analytics.

1. Щелкните **Новый шаг** > **Добавить действие**.
2. В поле поиска для фильтрации введите *анализ json*. Выберите действие **Операции с данными: анализ JSON**.

   ![Добавление действия анализа json в приложениях для логики](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Щелкните поле **Содержимое**, а затем выберите *Текст*.

4. Скопируйте следующую схему и вставьте ее в поле **Схема**:  Эта схема совпадает с результатом действия концентратора событий.  

   ![Диалоговое окно анализа JSON](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> Образец полезных данных можно получить, щелкнув **Выполнить** и просмотрев **Необработанные выходные данные** в концентраторе событий.  Затем можно использовать эти выходные данные с помощью параметра **Использовать пример полезной нагрузки, чтобы создать схему** в действии **Анализ JSON** для создания схемы.

### <a name="add-compose-action"></a>Действие "Добавить compose"
Действие [Compose](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) принимает выходные данные JSON и создает объект, который может использоваться для анализа журналов действия.

1. Щелкните **Новый шаг** > **Добавить действие**.
2. Введите *compose* в качестве фильтра и затем выберите действие **Операции с данными: compose**.

    ![Действие "Добавить compose"](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Щелкните поле **Входные данные** и выберите **Текст** в разделе действия **Анализ JSON**.


### <a name="add-log-analytics-send-data-action"></a>Добавление действия отправки данных Log Analytics
Действие [Сборщик данных Azure Log Analytics](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) использует объект из действия Compose и отправляет его в Log Analytics.

1. Щелкните **Новый шаг** > **Добавить действие**.
2. Введите *log analytics* в качестве своего фильтра, а затем выберите действие **Сборщик данных Azure Log Analytics: отправить данные**.

   ![Добавление действия отправки данных Log Analytics в приложениях логики](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Введите имя подключения и вставьте **идентификатор рабочей области** и **ключ рабочей области** для рабочей области Log Analytics.  Нажмите кнопку **Создать**.

   ![Добавление подключения Log Analytics в приложениях логики](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. После создания подключения измените параметры в таблице ниже. 

    ![Настройка действия отправки данных](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Параметр        | Значение           | ОПИСАНИЕ  |
   |---------------|---------------------------|--------------|
   |Текст запроса JSON  | **Выходные данные** действия **Compose** | Извлекает записи из текста сообщения действия Compose. |
   | Имя пользовательского журнала | AzureActivity | Имя настраиваемой таблицы журналов, которую необходимо создать в Log Analytics для хранения импортированных данных. |
   | time-generated-field | Twitter в режиме реального | Не выбирайте поле **время** JSON — просто введите слово time. Если вы выберете поле JSON, конструктор поместит действие **отправить данные** в цикл *For Each*, что не требуется. |




10. Чтобы сохранить изменения, внесенные в приложение логики, нажмите кнопку **Сохранить**.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Шаг 4. Тестирование и устранение неполадок приложения логики
После завершения рабочего процесса вы можете выполнить тестирование в конструкторе, чтобы убедиться, что процесс работает без ошибок.

В конструкторе Logic Apps щелкните **Запуск**, чтобы протестировать приложение. На каждом шаге в приложении логики отображается значок состояния с белой галочкой в ​​зеленом круге, что указывает на успешное выполнение.

   ![Тестирование приложения логики](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Чтобы просмотреть подробные сведения о каждом шаге, щелкните имя шага, чтобы развернуть его. Щелкните **Показать необработанные входные данные** и **Показать необработанные выходные данные** для просмотра дополнительных сведений о данных, получаемых и отправляемых на каждом шаге.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Шаг 5. Просмотр журнала действий Azure в службе Log Analytics
Теперь следует проверить рабочее пространство Log Analytics, чтобы убедиться, что сбор данных выполняется как ожидалось.

1. На портале Azure выберите **Log Analytics**.
2. Выберите рабочую область, а затем плитку **Поиск по журналу**.
3. На панели поиска запросов введите `AzureActivity_CL` и нажмите кнопку поиска. Если вы дали пользовательскому журналу имя, отличное от *AzureActivity*, введите имя, которое вы выбрали, и добавьте `_CL`.

>[!NOTE]
> Когда новый пользовательский журнал впервые отправляется в Log Analytics, может потребоваться около часа, прежде чем он станет доступен для поиска.

>[!NOTE]
> Журналы действий записываются в пользовательскую таблицу и не отображаются в [решении журнала действий](./log-analytics-activity.md).


![Тестирование приложения логики](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Дополнительная информация

В этой статье вы создали приложение логики для считывания событий в журналах действий Azure из концентратора событий и отправки их в Log Analytics для анализа. Чтобы узнать больше о визуализации данных в Log Analytics, включая создание панелей мониторинга, просмотрите руководство по визуализации данных.

> [!div class="nextstepaction"]
> [Создание панелей мониторинга данных Log Analytics и предоставление общего доступа к ним](./log-analytics-tutorial-dashboards.md)
