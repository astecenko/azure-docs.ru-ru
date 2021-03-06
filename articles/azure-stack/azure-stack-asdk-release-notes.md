---
title: "Заметки к выпуску пакета SDK для Microsoft Azure Stack | Документация Майкрософт"
description: "Улучшения, исправления и известные проблемы пакета средств разработки Azure Stack."
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: andredm
ms.openlocfilehash: 88247733c945de277e4d74b049d5edc6e4d97d3b
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Заметки к выпуску пакета SDK для Azure Stack

*Область применения: комплект разработки Azure Stack*

В этой статье содержатся сведения об улучшениях, исправлениях и известных проблемах пакета средств разработки Azure Stack. Если вы не знаете, какая версия используется, проверьте ее [с помощью портала](azure-stack-updates.md#determine-the-current-version).

## <a name="build-201801032"></a>Сборка 20180103.2

### <a name="new-features-and-fixes"></a>Новые функции и исправления

- Ознакомьтесь с разделом [Новые функции и исправления](azure-stack-update-1712.md#new-features-and-fixes), в котором содержатся заметки о выпуске обновлений 1712 Azure Stack для интегрированных систем Azure Stack.

    > [!IMPORTANT]
    > Некоторые из элементов, перечисленные в разделе **Новые функции и исправления**, применимы только к интегрированным системам Azure Stack.

### <a name="known-issues"></a>Известные проблемы
 
#### <a name="deployment"></a>Развертывание
- Во время развертывания для сервера времени необходимо указать IP-адрес.

#### <a name="infrastructure-management"></a>Управление инфраструктурой
- Не включайте резервное копирование инфраструктуры в колонке **Infrastructure backup** (Архивация инфраструктуры).
- IP-адрес контроллера управления основной платой (BMC) и модель не отображаются в списке основных сведений об узле единицы масштабирования. Такое поведение считается нормальным в среде SDK для Azure Stack.

#### <a name="portal"></a>Microsoft Azure
- На портале может появиться пустая панель мониторинга. Чтобы восстановить панель мониторинга, щелкните значок шестеренки в правом верхнем углу портала и выберите **Восстановление параметров по умолчанию**.
- При просмотре свойств группы ресурсов кнопка **Перемещение** отключена. Это ожидаемое поведение. Перемещение групп ресурсов между подписками в настоящее время не поддерживается.
-  Для любого рабочего процесса, где в раскрывающемся списке выбираются подписки, группа ресурсов или расположение, возникают следующие проблемы:

   - Может появиться пустая строка в верхней части списка. По-прежнему можно будет выбрать элемент должным образом.
   - Если список элементов в раскрывающемся списке короткий, возможно, вы не сможете просмотреть ни одно из имен элементов.
   - При наличии нескольких подписок пользователей раскрывающийся список группы ресурсов может быть пуст. 

   Чтобы обойти последние две проблемы, можно ввести имя группы подписки или ресурса (если известно) или вместо этого использовать PowerShell.

- Вы увидите предупреждение **Требуется активация**, это значит, что вам необходимо зарегистрировать пакет средств разработки Azure Stack. Это ожидаемое поведение.
- Если щелкнуть ссылку на **компонент** из любого предупреждения о **роли инфраструктуры**, открывается колонка **Обзор** и выполняется попытка загрузки, которая завершается ошибкой. Кроме того для колонки **Обзор** нет ограничения на время ожидания.
- Удаление подписки пользователя приводит к появлению потерянных ресурсов. Чтобы избежать этого, следует сначала удалить ресурсы пользователя или всю группу ресурсов и лишь затем удалять подписки пользователя.
- Вы не можете просматривать разрешения для подписки на порталах Azure Stack. В качестве временного решения разрешения можно проверить с помощью PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Некоторые элементы Marketplace удаляются в этом выпуске из-за проблем с совместимостью. Они будут включены снова после дальнейшей проверки.
- Пользователи могут просматривать весь Marketplace без подписки и видеть некоторые административные элементы, такие как планы и предложения. Эти элементы бесполезны для пользователей.
 
#### <a name="compute"></a>Среда выполнения приложений
- Пользователи могут создавать виртуальную машину с географически избыточным хранилищем. Но такой выбор конфигурации проводит к сбою при создании виртуальной машины. 
- Группу доступности виртуальной машины можно настроить только с одним доменом сбоя и одним доменом обновления.
- На Marketplace не предусмотрено создание масштабируемых наборов виртуальных машин. Масштабируемый набор можно создать с помощью шаблона.
- Параметры масштабирования для масштабируемых наборов виртуальной машины на портале недоступны. В качестве обходного решения используйте [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Из-за различий между версиями PowerShell используйте параметр `-Name` вместо параметра `-VMScaleSetName`.

#### <a name="networking"></a>Сеть
- Вы не можете создать подсистему балансировки нагрузки с общедоступным IP-адресом с помощью портала. В качестве временного решения для создания подсистемы балансировки нагрузки можно использовать PowerShell.
- При создании подсистемы балансировки сетевой нагрузки необходимо создать правило преобразования сетевых адресов (NAT). Если этого не сделать, вы получите ошибку при попытке добавить правило NAT после создания подсистемы балансировки нагрузки.
- Если в разделе **Сеть** вы щелкнете **Подключение** для настройки подключения VPN, в списке возможных подключений появится **Виртуальная сеть — виртуальная сеть**. Не выбирайте это подключение. В настоящее время поддерживается только подключение **Сеть — сеть (IPsec)**.
- Невозможно отменить сопоставление общедоступного IP-адреса c виртуальной машиной после создания виртуальной машины и ее сопоставления с этим IP-адресом. Будет выглядеть так, будто вам удалось отменить сопоставление, но ранее присвоенный общедоступный IP-адрес останется связанным с исходной виртуальной машиной. Это происходит, даже если присвоить этот IP-адрес новой виртуальной машине (это часто называют *переключением виртуального IP-адреса*). Все последующие попытки подключиться через этот IP-адрес к новой виртуальной машине приведут к подключению к исходной виртуальной машине, к которой он был привязан. В настоящее время для создания виртуальной машины необходимо использовать только новые общедоступные IP-адреса.
- Операторы Azure Stack могут не иметь возможности развертывать, удалять, изменять виртуальные сети или группы безопасности сети. Эта проблема в первую очередь проявляется при последующих попытках обновления одного и того же пакета. Она вызвана проблемой упаковки обновления, которая в настоящий момент анализируется.
- Служба внутренней балансировки нагрузки неправильно обрабатывает MAC-адреса внутренних виртуальных машин, что нарушает работу экземпляров Linux.
 
#### <a name="sqlmysql"></a>SQL и MySQL 
- После создания новых номеров SKU для SQL или MySQL может пройти до одного часа, прежде чем клиенты смогут создавать в них базы данных. 
- Создание элементов непосредственно на серверах размещения SQL и MySQL, если его выполняет не поставщик ресурсов, не поддерживается и может привести к несоответствию состояний.

#### <a name="app-service"></a>Служба приложений
- Прежде чем создавать первую функцию Azure в подписке, пользователь должен зарегистрировать поставщик ресурсов хранилища.
 
#### <a name="usage-and-billing"></a>Потребление и выставление счетов
- Вместо метки *TimeDate*, которая показывает время создания записи, в данных инструмента для отслеживания использования общедоступного IP-адреса отображается одно значение *EventDateTime* для каждой записи. В настоящее время эти данные непригодны для выполнения точного расчета и получения данных об использовании общедоступного IP-адреса.

#### <a name="identity"></a>Удостоверение

В развернутых средах служб федерации Azure Active Directory учетная запись **azurestack\azurestackadmin** больше не является владельцем подписки поставщика услуг по умолчанию. Вместо входа на **портал администрирования и/или в конечную точку adminmanagement** с **azurestack\azurestackadmin**, можно использовать учетную запись **azurestack\cloudadmin** для применения подписки поставщика по умолчанию и управления ею.

> [!IMPORTANT]
> Несмотря на то, что учетная запись **azurestack\cloudadmin** является владельцем подписки поставщика услуг по умолчанию в развернутых средах служб федерации Azure Active Directory, у нее нет разрешений на вход в узел по протоколу удаленного рабочего стола. Продолжайте использовать учетную запись **azurestack\azurestackadmin** или учетную запись локального администратора для входа в узел, доступа к нему и управления им по мере необходимости.

## <a name="build-201711221"></a>Сборка 20171122.1

### <a name="new-features-and-fixes"></a>Новые функции и исправления

- Ознакомьтесь с разделом [Новые функции и исправления](azure-stack-update-1711.md#new-features-and-fixes) заметок о выпуске обновлений 1711 Azure Stack для интегрированных систем Azure Stack.

    > [!IMPORTANT]
    > Некоторые из элементов, перечисленные в разделе **Новые функции и исправления**, применимы только к интегрированным системам Azure Stack.

### <a name="known-issues"></a>Известные проблемы
 
#### <a name="deployment"></a>Развертывание
- Во время развертывания для сервера времени необходимо указать IP-адрес.

#### <a name="infrastructure-management"></a>Управление инфраструктурой
- Не включайте резервное копирование инфраструктуры в колонке **Infrastructure backup** (Архивация инфраструктуры).
- IP-адрес контроллера управления основной платой (BMC) и модель не отображаются в списке основных сведений об узле единицы масштабирования. Такое поведение считается нормальным в среде SDK для Azure Stack.

#### <a name="portal"></a>Microsoft Azure
- На портале может появиться пустая панель мониторинга. Чтобы восстановить панель мониторинга, щелкните значок шестеренки в правом верхнем углу портала и выберите **Восстановление параметров по умолчанию**.
- При просмотре свойств группы ресурсов кнопка **Перемещение** отключена. Это ожидаемое поведение. Перемещение групп ресурсов между подписками в настоящее время не поддерживается.
-  Для любого рабочего процесса, где в раскрывающемся списке выбираются подписки, группа ресурсов или расположение, возникают следующие проблемы:

   - Может появиться пустая строка в верхней части списка. По-прежнему можно будет выбрать элемент должным образом.
   - Если список элементов в раскрывающемся списке короткий, возможно, вы не сможете просмотреть ни одно из имен элементов.
   - При наличии нескольких подписок пользователей раскрывающийся список группы ресурсов может быть пуст. 

   Чтобы обойти последние две проблемы, можно ввести имя группы подписки или ресурса (если известно) или вместо этого использовать PowerShell.

- Вы увидите предупреждение **Требуется активация**, это значит, что вам необходимо зарегистрировать пакет средств разработки Azure Stack. Это ожидаемое поведение.
- Если щелкнуть ссылку на **компонент** из любого предупреждения о **роли инфраструктуры**, открывается колонка **Обзор** и выполняется попытка загрузки, которая завершается ошибкой. Кроме того, для колонки **Overview** (Обзор) нет ограничения на время ожидания.
- Удаление подписки пользователя приводит к появлению потерянных ресурсов. Чтобы избежать этого, следует сначала удалить ресурсы пользователя или всю группу ресурсов и лишь затем удалять подписки пользователя.
- Вы не можете просматривать разрешения для подписки на порталах Azure Stack. В качестве временного решения разрешения можно проверить с помощью PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- При попытке добавить элементы в Azure Stack Marketplace с помощью параметра **Add from Azure** (Добавить из Azure) для скачивания могут отображаться не все элементы.
- Пользователи могут просматривать весь Marketplace без подписки и видеть некоторые административные элементы, такие как планы и предложения. Эти элементы бесполезны для пользователей.
 
#### <a name="compute"></a>Среда выполнения приложений
- Пользователи могут создавать виртуальную машину с географически избыточным хранилищем. Но такой выбор конфигурации проводит к сбою при создании виртуальной машины. 
- Группу доступности виртуальной машины можно настроить только с одним доменом сбоя и одним доменом обновления.
- На Marketplace не предусмотрено создание масштабируемых наборов виртуальных машин. Масштабируемый набор можно создать с помощью шаблона.
- Параметры масштабирования для масштабируемых наборов виртуальной машины на портале недоступны. В качестве обходного решения используйте [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Из-за различий между версиями PowerShell используйте параметр `-Name` вместо параметра `-VMScaleSetName`.

#### <a name="networking"></a>Сеть
- Вы не можете создать подсистему балансировки нагрузки с общедоступным IP-адресом с помощью портала. В качестве временного решения для создания подсистемы балансировки нагрузки можно использовать PowerShell.
- При создании подсистемы балансировки сетевой нагрузки необходимо создать правило преобразования сетевых адресов (NAT). Если этого не сделать, вы получите ошибку при попытке добавить правило NAT после создания подсистемы балансировки нагрузки.
- Если в разделе **Сеть** вы щелкнете **Подключение** для настройки подключения VPN, в списке возможных подключений появится **Виртуальная сеть — виртуальная сеть**. Не выбирайте это подключение. В настоящее время поддерживается только подключение **Сеть — сеть (IPsec)**.
- Невозможно отменить сопоставление общедоступного IP-адреса c виртуальной машиной после создания виртуальной машины и ее сопоставления с этим IP-адресом. Будет выглядеть так, будто вам удалось отменить сопоставление, но ранее присвоенный общедоступный IP-адрес останется связанным с исходной виртуальной машиной. Это происходит, даже если присвоить этот IP-адрес новой виртуальной машине (это часто называют *переключением виртуального IP-адреса*). Все последующие попытки подключиться через этот IP-адрес к новой виртуальной машине приведут к подключению к исходной виртуальной машине, к которой он был привязан. В настоящее время для создания виртуальной машины необходимо использовать только новые общедоступные IP-адреса.
- Операторы Azure Stack могут не иметь возможности развертывать, удалять, изменять виртуальные сети или группы безопасности сети. Эта проблема в первую очередь проявляется при последующих попытках обновления одного и того же пакета. Она вызвана проблемой упаковки обновления, которая в настоящий момент анализируется.
- Служба внутренней балансировки нагрузки неправильно обрабатывает MAC-адреса внутренних виртуальных машин, что нарушает работу экземпляров Linux.
 
#### <a name="sqlmysql"></a>SQL и MySQL 
- После создания новых номеров SKU для SQL или MySQL может пройти до одного часа, прежде чем клиенты смогут создавать в них базы данных. 
- Создание элементов непосредственно на серверах размещения SQL и MySQL, если его выполняет не поставщик ресурсов, не поддерживается и может привести к несоответствию состояний.

    > [!NOTE]
    > Дополнительные руководства по совместимости версий см. в статьях [Использование баз данных SQL в Microsoft Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-sql-resource-provider-deploy) и [Использование баз данных MySQL в Microsoft Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-mysql-resource-provider-deploy).

#### <a name="app-service"></a>Служба приложений
- Прежде чем создавать первую функцию Azure в подписке, пользователь должен зарегистрировать поставщик ресурсов хранилища.
 
#### <a name="usage-and-billing"></a>Потребление и выставление счетов
- Вместо метки *TimeDate*, которая показывает время создания записи, в данных инструмента для отслеживания использования общедоступного IP-адреса отображается одно значение *EventDateTime* для каждой записи. В настоящее время эти данные непригодны для выполнения точного расчета и получения данных об использовании общедоступного IP-адреса.

#### <a name="identity"></a>Удостоверение

В развернутых средах служб федерации Azure Active Directory учетная запись **azurestack\azurestackadmin** больше не является владельцем подписки поставщика услуг по умолчанию. Вместо входа на **портал администрирования и/или в конечную точку adminmanagement** с **azurestack\azurestackadmin**, можно использовать учетную запись **azurestack\cloudadmin** для применения подписки поставщика по умолчанию и управления ею.

> [!IMPORTANT]
> Несмотря на то, что учетная запись **azurestack\cloudadmin** является владельцем подписки поставщика услуг по умолчанию в развернутых средах служб федерации Azure Active Directory, у нее нет разрешений на вход в узел по протоколу удаленного рабочего стола. Продолжайте использовать учетную запись **azurestack\azurestackadmin** или учетную запись локального администратора для входа в узел, доступа к нему и управления им по мере необходимости.


## <a name="build-201710201"></a>Сборка 20171020.1

### <a name="improvements-and-fixes"></a>Улучшения и исправления

Чтобы узнать о списке улучшений и исправлений, предоставленных в сборке 20171020.1, ознакомьтесь с [соответствующим разделом](azure-stack-update-1710.md#improvements-and-fixes) в заметках о выпуске 1710 для интегрированных систем Azure Stack. Некоторые из элементов, перечисленные в разделе об улучшенном качестве и исправлениях, применимы только к интегрированным системам.

Были также устранены следующие проблемы:
- Устранена проблема отображения неизвестного состояния для поставщика вычислительных ресурсов.
- Устранена проблема, при которой квоты не отображались на портале администрирования после их создания и попытки просмотра дополнительных сведений о плане.

### <a name="known-issues"></a>Известные проблемы

#### <a name="powershell"></a>PowerShell
- Выпуск модуля AzureRM 1.2.11 PowerShell поставляется со списком критически важных изменений. Дополнительные сведения об обновлении, начиная с версии 1.2.10, см. в [руководстве по миграции](https://aka.ms/azspowershellmigration).
 
#### <a name="deployment"></a>Развертывание
- Во время развертывания для сервера времени необходимо указать IP-адрес.

#### <a name="infrastructure-management"></a>Управление инфраструктурой
- Не включайте резервное копирование инфраструктуры в колонке **Infrastructure backup** (Архивация инфраструктуры).
- IP-адрес контроллера управления основной платой (BMC) и модель не отображаются в списке основных сведений об узле единицы масштабирования. Такое поведение считается нормальным в среде SDK для Azure Stack.

#### <a name="portal"></a>Microsoft Azure
- На портале может появиться пустая панель мониторинга. Чтобы восстановить панель мониторинга, щелкните значок шестеренки в правом верхнем углу портала и выберите **Восстановление параметров по умолчанию**.
- При просмотре свойств группы ресурсов кнопка **Перемещение** отключена. Это ожидаемое поведение. Перемещение групп ресурсов между подписками в настоящее время не поддерживается.
-  Для любого рабочего процесса, где в раскрывающемся списке выбираются подписки, группа ресурсов или расположение, возникают следующие проблемы:

   - Может появиться пустая строка в верхней части списка. По-прежнему можно будет выбрать элемент должным образом.
   - Если список элементов в раскрывающемся списке короткий, возможно, вы не сможете просмотреть ни одно из имен элементов.
   - При наличии нескольких подписок пользователей раскрывающийся список группы ресурсов может быть пуст. 

   Чтобы обойти последние две проблемы, можно ввести имя группы подписки или ресурса (если известно) или вместо этого использовать PowerShell.

- Вы увидите предупреждение **Требуется активация**, это значит, что вам необходимо зарегистрировать пакет средств разработки Azure Stack. Это ожидаемое поведение.
- В дополнительных сведениях оповещения о **необходимости активации** не нужно использовать ссылку на компонент **AzureBridge**. В противном случае в колонке **Обзор** запустится непрерывная загрузка компонента, которая завершится сбоем.
- На портале администрирования в области **уведомлений** может появиться **ошибка при получении сведений о клиентах**. Это сообщение об ошибке можно игнорировать.
- Удаление подписки пользователя приводит к появлению потерянных ресурсов. Чтобы избежать этого, следует сначала удалить ресурсы пользователя или всю группу ресурсов и лишь затем удалять подписки пользователя.
- Вы не можете просматривать разрешения для подписки на порталах Azure Stack. В качестве временного решения разрешения можно проверить с помощью PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- При попытке добавить элементы в Azure Stack Marketplace с помощью параметра **Add from Azure** (Добавить из Azure) для скачивания могут отображаться не все элементы.
- Пользователи могут просматривать весь Marketplace без подписки и видеть некоторые административные элементы, такие как планы и предложения. Эти элементы бесполезны для пользователей.
 
#### <a name="compute"></a>Среда выполнения приложений
- Пользователи могут создавать виртуальную машину с географически избыточным хранилищем. Но такой выбор конфигурации проводит к сбою при создании виртуальной машины. 
- Группу доступности виртуальной машины можно настроить только с одним доменом сбоя и одним доменом обновления.
- На Marketplace не предусмотрено создание масштабируемых наборов виртуальных машин. Масштабируемый набор можно создать с помощью шаблона.
- Параметры масштабирования для масштабируемых наборов виртуальной машины на портале недоступны. В качестве обходного решения используйте [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Из-за различий между версиями PowerShell используйте параметр `-Name` вместо параметра `-VMScaleSetName`.

#### <a name="networking"></a>Сеть
- Вы не можете создать подсистему балансировки нагрузки с общедоступным IP-адресом с помощью портала. В качестве временного решения для создания подсистемы балансировки нагрузки можно использовать PowerShell.
- При создании подсистемы балансировки сетевой нагрузки необходимо создать правило преобразования сетевых адресов (NAT). Если этого не сделать, вы получите ошибку при попытке добавить правило NAT после создания подсистемы балансировки нагрузки.
- Если в разделе **Сеть** вы щелкнете **Подключение** для настройки подключения VPN, в списке возможных подключений появится **Виртуальная сеть — виртуальная сеть**. Не выбирайте это подключение. В настоящее время поддерживается только подключение **Сеть — сеть (IPsec)**.
- Невозможно отменить сопоставление общедоступного IP-адреса c виртуальной машиной после создания виртуальной машины и ее сопоставления с этим IP-адресом. Будет выглядеть так, будто вам удалось отменить сопоставление, но ранее присвоенный общедоступный IP-адрес останется связанным с исходной виртуальной машиной. Это происходит, даже если присвоить этот IP-адрес новой виртуальной машине (это часто называют *переключением виртуального IP-адреса*). Все последующие попытки подключиться через этот IP-адрес к новой виртуальной машине приведут к подключению к исходной виртуальной машине, к которой он был привязан. В настоящее время для создания виртуальной машины необходимо использовать только новые общедоступные IP-адреса.
 
#### <a name="sqlmysql"></a>SQL и MySQL 
- После создания новых номеров SKU для SQL или MySQL может пройти до одного часа, прежде чем клиенты смогут создавать в них базы данных. 
- Создание элементов непосредственно на серверах размещения SQL и MySQL, если его выполняет не поставщик ресурсов, не поддерживается и может привести к несоответствию состояний.

#### <a name="app-service"></a>Служба приложений
- Прежде чем создавать первую функцию Azure в подписке, пользователь должен зарегистрировать поставщик ресурсов хранилища.
 
#### <a name="usage-and-billing"></a>Потребление и выставление счетов
- Вместо метки *TimeDate*, которая показывает время создания записи, в данных инструмента для отслеживания использования общедоступного IP-адреса отображается одно значение *EventDateTime* для каждой записи. В настоящее время эти данные непригодны для выполнения точного расчета и получения данных об использовании общедоступного IP-адреса.