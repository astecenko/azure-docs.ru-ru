---
title: "Управление схемой базы данных SQL Azure в примере мультитенантного приложения | Документация Майкрософт"
description: "Управление схемой для нескольких клиентов в мультитенантном приложении, использующем базу данных SQL Azure"
keywords: "руководство по базе данных sql"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: c3eaa4d490b61b746e427d2fe2640ae5cdd6032c
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Управление схемой для нескольких клиентов в мультитенантном приложении, использующем Базу данных SQL Azure

В [первом руководстве по SaaS-приложению Wingtip Tickets c однотенантной БД](saas-dbpertenant-get-started-deploy.md) показано, как приложение может подготовить клиентскую базу данных и зарегистрировать ее в каталоге. Как и любое приложение, SaaS-приложение Wingtip Tickets c однотенантной БД будет развиваться со временем. При этом предполагаются некоторые изменения в базе данных, таких как новые или измененные схемы, справочные данные и стандартные задачи обслуживания баз данных для обеспечения оптимальной производительности приложения. С приложением SaaS эти изменения необходимо развертывать скоординировано в потенциально большом количестве баз данных клиентов. Чтобы эти изменения были внесены в будущие базы данных клиентов, их нужно включить в процесс подготовки.

В этом руководстве рассматривается два сценария: развертывание обновления справочных данных на всех клиентах и перенастройка индекса для таблицы, содержащей справочные данные. [Задания обработки эластичных БД](sql-database-elastic-jobs-overview.md) используются для выполнения этих операций на всех клиентах, а *"золотая"* база данных клиента используется как шаблон для новых баз данных.

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]

> * Создание учетной записи заданий.
> * Выполнение запроса к нескольким клиентам.
> * Обновление данных во всех клиентских базах данных.
> * Создание индекса для таблицы во всех базах данных клиента.


Для работы с этим руководством выполните следующие предварительные требования:

* Развернуть SaaS-приложение Wingtip Tickets c однотенантной базой данных. См. инструкции из руководства по быстрому [развертыванию SaaS-приложение Wingtip Tickets c однотенантной БД](saas-dbpertenant-get-started-deploy.md).
* Установите Azure PowerShell. Дополнительные сведения см. в статье [Начало работы с Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Установите последнюю версию SQL Server Management Studio (SSMS). [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (Скачивание SQL Server Management Studio (SSMS))

> [!NOTE]
> В этом руководстве используются функции службы базы данных SQL в ограниченной предварительной версии (задания обработки эластичных баз данных). Если вы хотите пройти это руководство, отправьте идентификатор вашей подписки по адресу SaaSFeedback@microsoft.com с темой "Задания обработки эластичных баз данных (предварительная версия)". Получив подтверждение о включении подписки, [скачайте и установите предварительную версию командлетов заданий](https://github.com/jaredmoo/azure-powershell/releases). Это ограниченная предварительная версия, поэтому если у вас возникнут вопросы или необходимость получить поддержку, напишите по адресу SaaSFeedback@microsoft.com.

## <a name="introduction-to-saas-schema-management-patterns"></a>Общие сведения о шаблонах управления схемой SaaS

Шаблон SaaS, в котором предусмотрено использование одного клиента на базу данных, имеет множество преимуществ, связанных с изоляцией данных. Но в то же время он усложняет обслуживание многих баз данных и управление ими. [Задания обработки эластичных БД](sql-database-elastic-jobs-overview.md) упрощают администрирование уровня данных SQL и управление им. Задания позволяют вам безопасно и надежно выполнять задачи (скрипты T-SQL) в группах баз данных независимо от действий пользователя. Этот метод можно использовать для развертывания схемы и обычных изменений справочных данных по всем клиентам в приложении. Задания обработки эластичных БД также могут использоваться для обслуживания *"золотой"* копии базы данных, которая используется для создания клиентов, обеспечивая ее самыми последними схемами и справочными данными.

![экран](media/saas-tenancy-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Ограниченная предварительная версия заданий обработки эластичных БД

Существует новая версия заданий обработки эластичных БД, которая является интегрированным компонентом баз данных SQL и не требует дополнительных служб или компонентов. Сейчас новая версия заданий обработки эластичных БД находится на этапе ограниченной предварительной версии. Эта ограниченная предварительная версия поддерживает создание учетных записей заданий для PowerShell и создание и управление заданиями для T-SQL.

> [!NOTE]
> В этом руководстве используются функции службы базы данных SQL в ограниченной предварительной версии (задания обработки эластичных баз данных). Если вы хотите пройти это руководство, отправьте идентификатор вашей подписки по адресу SaaSFeedback@microsoft.com с темой "Задания обработки эластичных баз данных (предварительная версия)". Получив подтверждение о включении подписки, [скачайте и установите предварительную версию командлетов заданий](https://github.com/jaredmoo/azure-powershell/releases). Это ограниченная предварительная версия, поэтому если у вас возникнут вопросы или необходимость получить поддержку, напишите по адресу SaaSFeedback@microsoft.com.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Получение скриптов для SaaS-приложения Wingtip Tickets c однотенантной БД

Сценарии для приложения SaaS Wingtip Tickets c мультитенантной базой данных и исходный код этого приложения вы найдете в репозитории GitHub [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Инструкции по скачиванию и разблокированию сценариев приложения SaaS Wingtip Tickets см. в статье [Общие рекомендации по работе с примерами приложений SaaS Wingtip Tickets](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="create-a-job-account-database-and-new-job-account"></a>Создание базы данных учетных записей заданий и учетной записи задания

В этом руководстве требуется создать базу данных учетных записей задания и учетную запись с помощью PowerShell. Подобно MSDB и агенту SQL, задания обработки эластичных БД используют базу данных SQL Azure для хранения определений заданий, сведений об их состоянии и журнала. Создав учетную запись задания, вы можете сразу создавать и отслеживать задания.

1. **В среде PowerShell ISE** выберите …\\Learning Modules\\Schema Management\\*Demo-SchemaManagement.ps1*.
1. Нажмите клавишу **F5** для запуска скрипта.

Скрипт *Demo-SchemaManagement.ps1* вызовет скрипт *Deploy-SchemaManagement.ps1* для создания базы данных *S2* с именем **jobaccount** в каталоге сервера. Затем он создаст учетную запись задания, передавая базу данных jobaccount как параметр для вызова создания учетной записи задания.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Создание задания и развертывание новых справочных данных на всех клиентах

Каждая база данных клиента включает набор типов объектов, которые определяют вид событий, проводимых на этом объекте. В этом упражнении вы развернете обновление на все базы данных клиентов, чтобы добавить два дополнительных типа объекта: *Мотоциклетные гонки* и *Плавательный клуб*. Эти типы объектов соответствуют фоновому изображению, которое отображается в приложении событий клиента.

Щелкните раскрывающееся меню с типами объектов и убедитесь, что доступно только 10 из них, а вариантов "Мотоциклетные гонки" и "Плавательный клуб" нет в списке.

Давайте создадим задание для обновления таблицы *VenueTypes* во всех базах данных клиентов и добавим новые типы объектов.

Чтобы создать задание, мы используем набор системно хранимых процедур заданий, созданный в базе данных jobaccount при создании учетной записи задания.

1. Откройте среду SSMS и подключитесь к серверу каталогов: catalog-dpt-\<user\>.database.windows.net server
1. Кроме того, подключитесь к серверу клиента: tenants1-dpt-\<user\>.database.windows.net
1. Перейдите к базе данных *contosoconcerthall* на сервере *tenants1-dpt-\<user\>* и запросите таблицу *VenueTypes*, чтобы убедиться, что объекты *Мотоциклетные гонки* и *Плавательный клуб* **отсутствуют** в списке результатов.
1. В среде SSMS выберите …\\Learning Modules\\Schema Management\\DeployReferenceData.sql
1. Измените инструкцию: SET @wtpUser = &lt;user&gt;, заменив значение User значением, использованным при развертывании SaaS-приложения Wingtip Tickets c однотенантной БД.
1. Убедитесь, что вы подключены к базе данных jobaccount, и нажмите клавишу **F5** для запуска скрипта.

В сценарии *DeployReferenceData.sql* обратите внимание на следующее:
* **sp\_add\_target\_group** создает целевую группу с именем DemoServerGroup. Теперь нам нужно добавить целевых участников.
* **sp\_add\_target\_group\_member** добавляет тип целевого элемента *server*, который считает, что все базы данных на сервере (обратите внимание, что сервер tenants1-dpt-&lt;User&gt; содержит клиентские базы данных) во время выполнения задания должны быть включены в задание. Также эта процедура добавляет тип целевого элемента *database*, в частности эталонную базу данных (basetenantdb), которая находится на сервере catalog-dpt-&lt;User&gt;, и еще один тип целевого элемента группы *database*, чтобы включить базу данных adhocanalytics, которая будет использоваться в следующем руководстве.
* **sp\_add\_job** — создает задание с названием "Развертывание справочных данных".
* **sp\_add\_jobstep** создает шаг задания, содержащий текст команды T-SQL для обновления в справочной таблице VenueTypes.
* Остальные представления в скрипте отображают сведения о существовании объектов и отслеживают выполнение задания мониторинга. С помощью этих запросов можно просмотреть значение состояния в столбце **lifecycle**, чтобы определить, когда задание будет успешно завершено во всех базах данных клиента и двух дополнительных базах данных, содержащих ссылочную таблицу.

В среде SSMS перейдите к базе данных *contosoconcerthall* на сервере *tenants1-dpt-\<user\>* и запросите таблицу *VenueTypes*, чтобы убедиться, что объекты *Мотоциклетные гонки* и *Плавательный клуб* теперь **есть** в списке результатов.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Создание задания для управления индексом справочной таблицы

Как и предыдущее упражнение, это упражнение создает задание для пересоздания индекса первичного ключа справочной таблицы — обычная операция управления базой данной, которую администратор может выполнить после передачи больших объемов данных в таблицу.

Создайте задание, используя те же системно хранимые процедуры задания.

1. Откройте среду SSMS и подключитесь к серверу catalog-dpt-&lt;User&gt;.database.windows.net server
1. Откройте файл \\Learning Modules\\Schema Management\\OnlineReindex.sql
1. Щелкните правой кнопкой мыши, выберите "Подключение" и подключитесь к серверу catalog-dpt-&lt;User&gt;.database.windows.net server, если вы еще не сделали это.
1. Убедитесь, что вы подключены к базе данных jobaccount, и нажмите клавишу F5 для запуска скрипта.

В сценарии *OnlineReindex.sql* обратите внимание на следующее:
* **sp\_add\_job** создает новое задание с именем Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885
* **sp\_add\_jobstep** создает шаг задания, содержащий текст команды T-SQL для обновления индекса
* Остальные представления в скрипте отслеживают выполнение задания. С помощью этих запросов можно просмотреть значение состояния в столбце **lifecycle**, чтобы определить, когда задание будет успешно завершено во всех целевых элементах группы.



## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]

> * Создание учетной записи заданий для запроса нескольких клиентов.
> * Обновление данных во всех клиентских базах данных.
> * Создание индекса для таблицы во всех базах данных клиента.

Далее см. статью [Выполнение запросов автоматизированной системы отчетности к нескольким базам данных SQL Azure](saas-tenancy-adhoc-analytics.md).


## <a name="additional-resources"></a>Дополнительные ресурсы

* [Дополнительные руководства по использованию развертывания SaaS-приложения Wingtip Tickets c однотенантной БД](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Управление масштабируемыми облачными базами данных](sql-database-elastic-jobs-overview.md)
* [Создание развернутых баз данных SQL Azure и управление ими с помощью заданий обработки эластичных БД (предварительная версия)](sql-database-elastic-jobs-create-and-manage.md)