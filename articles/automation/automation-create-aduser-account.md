---
title: "Создание учетной записи пользователя Azure AD | Документация Майкрософт"
description: "В этой статье объясняется, как создать учетные данные учетной записи пользователя Azure AD для модулей Runbook в службе автоматизации Azure для аутентификации в Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "пользователь azure active directory, управление службами azure, учетная запись пользователя azure ad"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Проверка подлинности модулей Runbook с помощью классического развертывания Azure и Resource Manager
В этой статье описываются действия по настройке учетной записи пользователя Azure AD для модулей Azure Automation Runbook, запущенных с классической моделью развертывания Azure или с ресурсами Azure Resource Manager.  Удостоверение аутентификации для модулей Runbook на основе Azure Resource Manager по-прежнему поддерживается, но мы советуем использовать в Azure учетную запись запуска от имени.       

## <a name="create-a-new-azure-active-directory-user"></a>Создание нового пользователя Azure Active Directory
1. Войдите на портал Azure как администратор службы для подписки Azure, которой вы хотите управлять.
2. Последовательно выберите **Azure Active Directory** > **Пользователи и группы** > **Все пользователи** > **Новый пользователь**.
3. Введите сведения о пользователе в соответствующих полях, например **Имя** и **Имя пользователя**.  
4. Запишите полное имя пользователя и временный пароль.
5. Выберите **Роль каталога**.
6. Назначьте роль глобального администратора или администратора с ограниченными правами.
7. Выйдите из Azure, а затем войдите снова, используя только что созданную учетную запись. Вам будет предложено изменить пароль пользователя.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Создание учетной записи службы автоматизации на портале Azure
В этом разделе перечисляются действия, с помощью которых на портале Azure можно создать учетную запись службы автоматизации Azure, которая будет использоваться для ресурсов управления модулями Runbook в режиме Azure Resource Manager.  

1. Войдите на портал Azure как администратор службы для подписки Azure, которой вы хотите управлять.
2. Выберите элемент **Учетные записи службы автоматизации**.
3. Выберите **Добавить**.<br><br>![Добавление учетной записи службы автоматизации](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. В колонке **Добавление учетной записи службы автоматизации** в поле **Имя** введите имя новой учетной записи службы автоматизации.
5. При наличии нескольких подписок укажите одну из них для новой учетной записи, а также новую или имеющуюся **группу ресурсов** и **расположение** центра обработки данных Azure.
6. Выберите параметр **Да** для **Create Azure Run As account** (Создать учетную запись запуска от имени в Azure) и нажмите кнопку **Создать**.  
   
    > [!NOTE]
    > Если вы решили не создавать учетную запись запуска от имени, выбрав значение **Нет**, в колонке **Добавление учетной записи службы автоматизации** появится предупреждение.  Хотя учетная запись создается и назначается роли **Участник** в подписке, она не будет содержать соответствующее удостоверение проверки подлинности в службе каталогов вашей подписки и поэтому не будет иметь доступ к ресурсам в вашей подписке.  Из-за этого модули Runbook, ссылающиеся на эту учетную запись, не смогут проходить проверку подлинности и выполнять задачи с ресурсами Azure Resource Manager.
    > 
    >

    <br>![Предупреждение в колонке "Добавление учетной записи службы автоматизации"](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Ход создания учетной записи службы автоматизации в Azure можно отслеживать в разделе **Уведомления** в меню.

После создания учетных данных вам нужно создать ресурс учетных данных, чтобы связать учетную запись службы автоматизации с созданной ранее учетной записью пользователя AD.  Помните: мы только создали учетную запись службы автоматизации; она не связана с удостоверением проверки подлинности.  Выполните действия, описанные в статье [Ресурсы учетных данных в службе автоматизации Azure](automation-credentials.md#creating-a-new-credential-asset), и введите в поле **Имя пользователя** значение в формате **домен\пользователь**.

## <a name="use-the-credential-in-a-runbook"></a>Использование учетных данных в Runbook
Учетные данные в модуле Runbook можно получить с помощью действия [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx), а затем использовать их в [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) для подключения к подписке Azure. Если это учетные данные администратора нескольких подписок Azure, вам также следует использовать [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) , чтобы указать соответствующую подписку. Как показано в примере Windows PowerShell ниже, она обычно будет отображаться в верхней части большинства модулей Runbook службы автоматизации Azure.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Эти строки должны повторяться после любой [контрольной точки](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) в Runbook. Если выполнение Runbook приостанавливается, а затем возобновляется другим работником, ему необходимо будет заново пройти аутентификацию.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о разных типах модулей Runbook и действиях по созданию собственных модулей Runbook см. в статье [Типы модулей Runbook в службе автоматизации Azure](automation-runbook-types.md).

