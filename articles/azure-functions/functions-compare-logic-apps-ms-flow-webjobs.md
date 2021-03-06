---
title: "Сравнение Microsoft Flow, Logic Apps, Функций и веб-заданий | Документация Майкрософт"
description: "Эта статья позволяет сравнить облачные службы интеграции Microsoft и выбрать наиболее оптимальный для вас вариант."
services: functions,app-service\logic
documentationcenter: na
author: ggailey777
manager: wpickett
tags: 
keywords: "microsoft flow, поток, logic apps, функции azure, функции, веб-задания azure, веб-задания, обработка событий, динамическое вычисление, независимая от сервера архитектура"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7ffe44828735a5687008ebc5a7d8d9f017f49daa
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Сравнение Microsoft Flow, Logic Apps, функций и веб-заданий Azure
В этой статье сравниваются следующие службы Microsoft Cloud, используемые для настройки интеграции и автоматизации бизнес-процессов:

* [Microsoft Flow](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Функции Azure](https://azure.microsoft.com/services/functions/)
* [веб-задания службы приложений Azure](../app-service/web-sites-create-web-jobs.md)

Все эти службы особенно полезны при объединении разрозненных систем. Они позволяют определять входные данные, действия, условия и выходные данные. Каждую отдельную службу можно запускать по расписанию или активировать по запросу. Каждая из них имеет уникальные преимущества. Поэтому цель сравнения служб — определить не лучшую из них, а самую подходящую в определенном сценарии. Обычно, чтобы быстро создать масштабируемое полнофункциональное решение интеграции, эти службы необходимо использовать вместе.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Сравнение Flow приложения логики;
Мы можем рассматривать Microsoft Flow и Azure Logic Apps вместе, так как обе эти службы являются службами интеграции на основе модели *configuration-first*. Они упрощают создание процессов и рабочих процессов, а также обеспечивают интеграцию с разными корпоративными приложениями и приложениями SaaS. 

* Служба Flow создана на основе Logic Apps.
* У них один и тот же конструктор рабочих процессов.
* [соединители](../connectors/apis-list.md) .

Служба Microsoft Flow помогает офисным сотрудникам самостоятельно выполнять простые операции интеграции (например, процесс утверждения для библиотеки документов SharePoint), не обращаясь к разработчикам или ИТ-специалистам. Однако Logic Apps обеспечивает расширенные интеграции (например, процессы B2B), где нужно выполнять разработку и операции корпоративного класса и применять методы обеспечения безопасности. Как правило, с течением времени бизнес-процессы становятся более сложными. Поэтому на первых этапах можно использовать поток, а затем при необходимости преобразовать его в приложение логики.

Сведения в следующей таблице помогут определить, какую из этих двух служб лучше всего использовать для определенной интеграции.

|  | Поток | приложения логики; |
| --- | --- | --- |
| Аудитория |Офисные сотрудники, бизнес-пользователи, администраторы SharePoint |Профессиональные интеграторы и разработчики, ИТ-специалисты |
| Сценарии |Самообслуживание |Расширенные интеграции |
| Средство разработки |В браузере и мобильном приложении, только пользовательский интерфейс |В браузере и [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md) доступно [представление кода](../logic-apps/logic-apps-author-definitions.md). |
| Управление жизненным циклом приложений (ALM) |Разработка и тестирование в непроизводственных средах, распространение в рабочей среде по готовности. |DevOps: система управления версиями, тестирование, поддержка, автоматизация и управление в [Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md) |
| Административные функции |Управление средами Flow и политиками защиты от потери данных (DLP), отслеживание лицензирования [https://admin.flow.microsoft.com](https://admin.flow.microsoft.com) |Управление группами ресурсов, подключениями, доступом и ведение журнала [https://portal.azure.com](https://portal.azure.com) |
| Безопасность |Журналы аудита безопасности и соответствия требованиям Office 365, защита от потери данных (DLP), [шифрование неактивных](https://wikipedia.org/wiki/Data_at_rest#Encryption) конфиденциальных данных и т. д. |Обеспечение безопасности Azure: [безопасность Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [центр безопасности](https://azure.microsoft.com/services/security-center/), [журналы аудита](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/) и многое другое. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Сравнение функций и Веб-задания
Функции Azure и веб-задания службы приложений Azure рассматриваются вместе, так как они являются службами интеграции на основе модели *code-first* и предназначены для разработчиков. Они позволяют выполнять сценарии или фрагменты кода в ответ на разные события, например [создание больших двоичных объектов хранилища](functions-bindings-storage.md) или отправку [запроса webhook](functions-bindings-http-webhook.md). Сходства между функциями Azure и веб-заданиями службы приложений Azure: 

* Созданы на основе [службы приложений Azure](../app-service/app-service-web-overview.md) и поддерживают одинаковые возможности, такие как [система управления версиями](../app-service/app-service-continuous-deployment.md), [аутентификация](../app-service/app-service-authentication-overview.md) и [мониторинг](../app-service/web-sites-monitor.md).
* Предназначены для разработчиков.
* Поддерживают стандартные языки программирования и скриптов.
* Поддерживают NuGet и NPM.

Функции были созданы на основе веб-заданий. В них сочетаются все лучшие возможности веб-заданий, а также добавлены некоторые улучшения, среди которых: 

* Модель [бессерверных](https://azure.microsoft.com/overview/serverless-computing/) приложений.
* Упрощенная разработка, тестирование и выполнение кода непосредственно в браузере.
* Встроенная интеграция с большим количеством служб Azure и сторонних служб, таких как [Объекты Webhook GitHub](https://developer.github.com/webhooks/creating/).
* Оплата за использование, не требуется платить за [план службы приложений](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Автоматизация, [динамическое масштабирование](functions-scale.md).
* Текущие клиенты службы приложений все еще могут использовать план службы приложений (чтобы воспользоваться малоиспользуемыми ресурсами).
* Интеграция с Logic Apps.

В следующей таблице приведены различия между функциями и веб-заданиями.

|  | Functions | Веб-задания |
| --- | --- | --- |
| Масштабирование |Масштабирование без настройки |Масштабирование в рамках плана службы приложений |
| Цены |Оплата за использование или в рамках плана службы приложений |В рамках плана службы приложений |
| Тип запуска |Активация, по расписанию (с помощью триггера таймера) |Активация, непрерывная работа, по расписанию |
| События триггера |[Таймер](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-cosmosdb.md), [концентраторы событий Azure](functions-bindings-event-hubs.md), [HTTP и веб-перехватчик (GitHub, Slack)](functions-bindings-http-webhook.md), [мобильные приложения службы приложений Azure](functions-bindings-mobile-apps.md), [концентраторы событий Azure](functions-bindings-event-hubs.md), [очереди и большие двоичные объекты службы хранилища Azure](functions-bindings-storage-blob.md), [очереди и разделы служебной шины Azure](functions-bindings-service-bus.md) |[Очереди и большие двоичные объекты службы хранилища Azure](functions-bindings-storage-blob.md), [очереди и разделы служебной шины Azure](functions-bindings-service-bus.md) |
| Разработка в браузере |Поддерживаются |Не поддерживается |
| C# |Поддерживаются |Поддерживаются |
| F# |Поддерживаются |Не поддерживается |
| JavaScript |Поддерживаются |Поддерживаются |
| Java |Предварительный просмотр | Не поддерживается |
| Bash |Экспериментальная возможность |Поддерживаются |
| Написание скриптов Windows (.cmd, .bat) |Экспериментальная возможность |Поддерживаются |
| PowerShell |Экспериментальная возможность |Поддерживаются |
| PHP |Экспериментальная возможность |Поддерживаются |
| Python |Экспериментальная возможность |Поддерживаются |
| TypeScript |Экспериментальная возможность |Не поддерживается |

Выбор между функциями и веб-заданиями зависит от того, какие процессы уже выполнялись в службе приложений. Если для службы приложений необходимо выполнить фрагменты кода и управлять ими совместно в среде разработки и операций, следует использовать веб-задания. Используйте функции в следующих сценариях.

* Вы хотите выполнить фрагменты кода для других служб Azure или приложений сторонних поставщиков.
* Вы хотите управлять кодом интеграции отдельно от приложений службы приложений.
* Вы хотите вызывать собственные фрагменты кода из Logic Apps. 

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Flow, Logic Apps и функции
Как уже отмечалось ранее, выбор конкретной службы зависит от определенной ситуации. 

* Для оптимизации простых бизнес-процессов используйте Flow.
* Если сценарий интеграции слишком сложен для Flow или требуются возможности DevOps, используйте Logic Apps.
* Если в рамках сценария интеграции требуется выполнить значительные преобразования или написать специализированный код, напишите функцию, а затем активируйте ее в качестве действия в приложении логики.

Приложение логики можно вызвать в потоке. Функцию также можно вызвать в приложении логики, а приложение логики — в функции. Интеграция между Flow, Logic Apps и функциями со временем улучшается. Вы можете создать что-нибудь в одной службе, а использовать в других. Поэтому все вложенные в эти три технологии средства оправданы.

## <a name="next-steps"></a>Дополнительная информация
Начните работу с каждой из этих служб, создав первый поток, приложение логики, приложение-функцию или веб-задание. Щелкните любую из следующих ссылок:

* [Начало работы с Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Создайте приложение логики](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Создание первой функции Azure](functions-create-first-azure-function.md)
* [Развертывание веб-заданий с помощью Visual Studio](../app-service/websites-dotnet-deploy-webjobs.md)

Дополнительные сведения об этих службах интеграции см. в следующих источниках:

* [Leveraging Azure Functions & Azure App Service for integration scenarios by Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/) (Использование функций Azure и службы приложений Azure в сценариях интеграции. Автор: Кристофер Андерсон (Christopher Anderson))
* [Integrations Made Simple by Charles Lamanna](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/) (Упрощенные возможности интеграции. Автор: Чарльз Ламанна (Charles Lamanna))
* [Logic Apps Live Webcast](http://aka.ms/logicappslive)
* [Часто задаваемые вопросы о Microsoft Flow](https://flow.microsoft.com/documentation/frequently-asked-questions/)

