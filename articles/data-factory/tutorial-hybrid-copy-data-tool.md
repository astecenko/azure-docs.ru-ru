---
title: "Копирование локальных данных с помощью средства копирования данных Azure | Документация Майкрософт"
description: "Создание фабрики данных Azure и применение средства копирования данных для копирования данных из локальной базы данных SQL Server в хранилище BLOB-объектов Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Копирование данных из локальной базы данных SQL Server в хранилище BLOB-объектов Azure с помощью средства копирования данных
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Версия 1 — общедоступная](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Версия 2 — предварительная](tutorial-hybrid-copy-data-tool.md)

В этом руководстве вы создадите фабрику данных с помощью портала Azure. После этого вы примените средство копирования данных для копирования данных из локальной базы данных SQL Server в хранилище BLOB-объектов Azure.

> [!NOTE]
> - Если вы еще не работали с фабрикой данных Azure, ознакомьтесь со статьей [Введение в фабрику данных Azure](introduction.md).
>
> - Эта статья относится к версии 2 фабрики данных, которая в настоящее время доступна в предварительной версии. Если вы используете общедоступную версию 1 службы фабрики данных, см. статью о [начале работы со службой фабрики данных версии 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

В этом руководстве вы выполните следующие шаги:

> [!div class="checklist"]
> * Создадите фабрику данных.
> * Создание конвейера с помощью средства копирования данных
> * Мониторинг конвейера и выполнения действий.

## <a name="prerequisites"></a>предварительным требованиям
### <a name="azure-subscription"></a>Подписка Azure.
Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

### <a name="azure-roles"></a>Роли Azure
Чтобы создать экземпляры фабрики данных, нужно назначить учетной записи пользователя, используемой для входа в Azure, роль *участника*, *владельца* либо *администратора* подписки Azure. 

Чтобы просмотреть имеющиеся разрешения в подписке, на портале Azure выберите "Имя пользователя" в правом верхнем углу и выберите **Разрешения**. Если у вас есть доступ к нескольким подпискам, выберите соответствующую подписку. Примеры инструкций по назначению пользователю роли см. в статье [о добавлении ролей](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 и 2017
В этом руководстве используйте локальную базу данных SQL Server в качестве *исходного* хранилища данных. Конвейер фабрики данных, созданный в рамках этого руководства, копирует данные из этой локальной базы данных SQL Server (источника) в хранилище BLOB-объектов Azure (приемник). Затем создайте таблицу с именем **emp** в базе данных SQL Server и вставьте в нее несколько примеров записей. 

1. Запустите среду SQL Server Management Studio. Если она еще не установлена на вашем компьютере, скачайте ее со страницы [Скачивание SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Подключитесь к экземпляру SQL Server с помощью учетных данных. 

3. Создайте пример базы данных. В представлении в виде дерева щелкните правой кнопкой мыши элемент **Базы данных** и выберите пункт **Новая база данных**. 
 
4. В окне **Новая база данных** введите имя базы данных и нажмите кнопку **ОК**. 

5. Чтобы создать таблицу **emp** и добавить в нее демонстрационные данные, запустите в базе данных следующий скрипт запроса. Для этого щелкните правой кнопкой мыши созданную базу данных в представлении в виде дерева и выберите действие **Новый запрос**.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Учетная запись хранения Azure
В этом руководстве используйте учетную запись хранения Azure общего назначения (хранилища BLOB-объектов Azure) в качестве целевого (принимающего) хранилища данных. Если у вас нет учетной записи хранения Azure общего назначения, изучите инструкции [по созданию учетной записи хранения](../storage/common/storage-create-storage-account.md#create-a-storage-account). Конвейер фабрики данных, созданный в рамках этого руководства, копирует данные из локальной базы данных SQL Server (источника) в это хранилище BLOB-объектов Azure (приемник). 

#### <a name="get-storage-account-name-and-account-key"></a>Получение имени и ключа учетной записи хранения
В этом руководстве вы будете использовать имя и ключ своей учетной записи хранения Azure. Выполните следующие действия, чтобы получить имя и ключ учетной записи хранения. 

1. Войдите на [портал Azure](https://portal.azure.com), используя имя пользователя и пароль Azure. 

2. В левой области выберите **Больше служб**, отфильтруйте содержимое по ключевому слову **хранение**, а затем выберите **Учетные записи хранения**.

    ![Поиск учетной записи хранения](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. В списке учетных записей хранения найдите с помощью фильтра свою учетную запись хранения (при необходимости), а затем выберите эту учетную запись. 

4. В окне **Учетная запись хранения** выберите параметр **Ключи доступа**.

    ![Получение имени и ключа учетной записи хранения](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Скопируйте значения полей **Имя учетной записи хранения** и **key1**. Затем вставьте их в Блокнот или другой редактор для дальнейшего использования в руководстве. 

#### <a name="create-the-adftutorial-container"></a>Создание контейнера adftutorial 
В этом разделе вы создадите контейнер больших двоичных объектов с именем **adftutorial** в хранилище BLOB-объектов Azure. 

1. В окне **Учетная запись хранения** перейдите на вкладку **Обзор** и выберите **Blobs** (Большие двоичные объекты). 

    ![Выбор параметра "BLOB-объекты"](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. В окне **Служба BLOB-объектов** выберите **Контейнер**. 

    ![Кнопка добавления контейнера](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. В окне **Новый контейнер** в поле **Имя** введите **adftutorial**, а затем нажмите кнопку **ОК**. 

    ![Ввод имени контейнера](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. В списке контейнеров выберите **adftutorial**.  

    ![Выбор контейнера](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Не закрывайте окно **контейнера** **adftutorial**. Она понадобится для проверки выходных данных в конце этого руководства. Фабрика данных автоматически создает выходную папку в этом контейнере, поэтому ее не нужно создавать.

    ![Окно контейнера](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Создание фабрики данных

1. В меню слева щелкните **Создать**, выберите **Данные+аналитика** и щелкните **Фабрика данных**. 
   
   ![Создать -> Фабрика данных](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. На странице **Новая фабрика данных** введите **ADFTutorialDataFactory** в поле **Имя**. 
      
     ![Страница "Новая фабрика данных"](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   Имя фабрики данных Azure должно быть **глобально уникальным**. Если вы увидите следующую ошибку для имени, введите другое имя фабрики данных (например, ваше_имя_ADFTutorialDataFactory). Ознакомьтесь со статьей [Фабрика данных — правила именования](naming-rules.md), чтобы узнать правила именования для артефактов фабрики данных.
  
     ![Страница "Новая фабрика данных"](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Выберите **подписку** Azure, в рамках которой вы хотите создать фабрику данных. 
4. Для **группы ресурсов** выполните одно из следующих действий.
     
      - Выберите **Использовать существующую**и укажите существующую группу ресурсов в раскрывающемся списке. 
      - Выберите **Создать новую**и укажите имя группы ресурсов.   
         
      Сведения о группах ресурсов см. в статье, где описывается [использование групп ресурсов для управления ресурсами Azure](../azure-resource-manager/resource-group-overview.md).  
4. Выберите **V2 (Preview)** (V2 (предварительная версия)) для **версии**.
5. Укажите **расположение** фабрики данных. В раскрывающемся списке отображаются только поддерживаемые расположения. Хранилища данных (служба хранилища Azure, база данных SQL Azure и т. д.) и вычисления (HDInsight и т. д.), используемые фабрикой данных, могут располагаться в других расположениях и (или) регионах.
6. Кроме того, установите флажок **Закрепить на панели мониторинга**.     
7. Нажмите кнопку **Создать**.
8. На панели мониторинга вы увидите приведенный ниже элемент с состоянием **Deploying data factory** (Развертывание фабрики данных). 

    ![Элемент Deploying data factory (Развертывание фабрики данных)](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Когда завершится создание, вы увидите страницу **Фабрика данных**, как показано на рисунке ниже.
   
   ![Домашняя страница фабрики данных](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Щелкните плитку **Создание и мониторинг**, чтобы открыть на отдельной вкладке пользовательский интерфейс фабрики данных Azure. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Создание конвейера с помощью средства копирования данных

1. На странице начала работы щелкните плитку **Копировать данные**, чтобы запустить средство копирования данных. 

   ![Плитка средства копирования данных](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. На странице **Свойства** для средства копирования данных укажите **CopyFromOnPremSqlToAzureBlobPipeline** в качестве **имени задачи** и нажмите кнопку **Далее**. Средство копирования данных создаст конвейер с именем, которое вы ввели в этом поле. 
    
   ![Страница свойств](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. На странице **Исходное хранилище данных** выберите **SQL Server** и нажмите кнопку **Далее**. Возможно, список придется прокрутить вниз, чтобы найти **SQL Server**. 

   ![Страница исходного хранилища данных](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Введите **SqlServerLinkedService** в поле **Имя подключения** и щелкните ссылку **Create Integration Runtime** (Создать среду выполнения интеграции). Вам следует создать локальную среду выполнения интеграции, скачать ее на локальный компьютер и зарегистрировать ее в службе фабрики данных. Локальная среда выполнения интеграции копирует данные из локальной среды в облако Azure и обратно.

   ![Ссылка для создания среды выполнения интеграции](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. В диалоговом окне **Create Integration Runtime** (Создание среды выполнения интеграции) введите **TutorialIntegrationRuntime** в поле **Имя** и щелкните **Создать**. 

   ![Диалоговое окно создания среды выполнения интеграции](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Щелкните **Launch express setup on this computer** (Запустить экспресс-установку на этом компьютере). Это действие устанавливает среду выполнения интеграции на локальном компьютере и регистрирует ее в службе фабрики данных. Кроме того, вы можете использовать режим установки вручную: скачайте файл установки, запустите его и примените ключ для регистрации среды выполнения интеграции. 

    ![Ссылка для создания среды выполнения интеграции](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Запустите загруженное приложение. В окне будет отображаться текущее **состояние** экспресс-установки. 

    ![Состояние экспресс-установки](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Убедитесь, что в поле **Среда выполнения интеграции** выбран вариант **TutorialIntegrationRuntime**

    ![Выбранная среда выполнения интеграции](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. В окне **Specify the on-premises SQL Server database** (Выбор локальной базы данных SQL Server) выполните следующие действия. 

    1. Введите **OnPremSqlLinkedService** в поле **Имя подключения**.
    2. Введите имя локального SQL Server в поле **Имя сервера**.
    3. Введите имя локальной базы данных в поле **Имя базы данных**. 
    4. Выберите нужный вариант аутентификации в поле **Тип проверки подлинности**.
    5. Введите имя пользователя, имеющего доступ к локальному SQL Server, в поле **Имя пользователя**.
    6. Введите **пароль** для этого пользователя. 
10. На странице **Select tables from which to copy the data or use a custom query** (Выберите таблицу, из которых нужно скопировать данные, или введите пользовательский запрос) выберите в списке значение **[dbo].[emp]** и нажмите кнопку **Далее**. 

    ![Выбор таблицы emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. На странице **Целевое хранилище данных** выберите **Хранилище BLOB-объектов Azure** и нажмите кнопку **Далее**.

    ![Выбор хранилища BLOB-объектов Azure](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. На странице **Specify the Azure Blob storage account** (Выберите учетную запись хранения BLOB-объектов Azure) выполните следующие действия. 

    1. Введите **AzureStorageLinkedService** в поле **Имя подключения**.
    2. Выберите учетную запись хранения Azure в выпадающем списке **Имя учетной записи хранения**. 
    3. Нажмите кнопку **Далее**.

        ![Выбор хранилища BLOB-объектов Azure](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. На странице **Choose the output file or folder** (Выберите целевой файл или папку) выполните следующие действия. 
    
    1. Введите **adftutorial/fromonprem** в поле **Путь к папке**. Для работы с этим руководством вы ранее создали контейнер **adftutorial**. Если указанная целевая папка не существует, служба фабрики данных автоматически создаст ее. Можно также нажать кнопку **Обзор**, чтобы открыть хранилище BLOB-объектов и просмотреть в нем контейнеры и папки. Обратите внимание, что по умолчанию указано имя целевого файла **dbo.emp**.
        
        ![Выбор целевого файла или папки](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. На странице **Параметры формата файла** нажмите кнопку **Далее**. 

    ![Страница параметров формата файла](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. На странице **Параметры** нажмите кнопку **Далее**. 

    ![Страница «Параметры»](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. На странице **Сводка** проверьте значения всех параметров и нажмите кнопку **Далее**. 

    ![Страница "Сводка"](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. На странице **Развертывания** нажмите кнопку **Отслеживать**, чтобы отслеживать созданный конвейер или задачу.

    ![Страница развертывания](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. На вкладке **Мониторинг** вы можете просмотреть состояние созданного конвейера. Ссылки в столбце **Действия** дают возможность просмотреть запуски действий, связанные с этим запуском конвейера, и (или) повторно запустить конвейер. 

    ![Запуски конвейера](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Щелкните ссылку **View Activity Runs** (Просмотр запусков действий) в столбце **Действия**, чтобы просмотреть запуски действий, связанные с этим запуском конвейера. Чтобы просмотреть сведения об операции копирования, щелкните ссылку **Сведения** (значок очков) в столбце **Действия**. Чтобы вернуться в режим просмотра запусков конвейера, щелкните ссылку **Конвейеры** вверху.

    ![Выполнение действия](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Убедитесь, что целевой файл появился в папке **fromonprem** конвейера **adftutorial**.   
 
    ![Целевой BLOB-объект](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Щелкните вкладку **Изменить** слева, чтобы переключиться в режим редактора. В этом редакторе вы можете изменять параметры связанных служб, наборов данных и конвейеров, созданных с помощью средства. Щелкните **Код**, чтобы просмотреть код JSON для сущности, открытой в настоящий момент в редакторе. Дополнительные сведения о редактировании этих сущностей с помощью пользовательского интерфейса фабрики данных вы найдете в [версии этого руководства для портала Azure](tutorial-copy-data-portal.md).

    ![Вкладка редактирования](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Дополнительная информация
Этот пример конвейера копирует данные из локальной базы данных SQL Server в хранилище BLOB-объектов Azure. Вы научились выполнять следующие задачи: 

> [!div class="checklist"]
> * Создадите фабрику данных.
> * Создание конвейера с помощью средства копирования данных
> * Мониторинг конвейера и выполнения действий.

Список хранилищ данных, поддерживаемых фабрикой данных, см. в разделе [Поддерживаемые хранилища данных и форматы](copy-activity-overview.md#supported-data-stores-and-formats).

Чтобы узнать о копировании данных в пакетном режиме из источника в место назначения, перейдите к следующему руководству:

> [!div class="nextstepaction"]
>[Копирование нескольких таблиц в пакетном режиме с помощью фабрики данных Azure](tutorial-bulk-copy-portal.md)