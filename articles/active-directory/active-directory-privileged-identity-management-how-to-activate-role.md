---
title: "Активация или деактивация роли | Документация Майкрософт"
description: "Сведения о том, как активировать роли для привилегированных пользователей с помощью приложения для управления привилегированными пользователями Azure."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8bd8a72653699df4f4953053d61c16e30a2a101d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Как активировать и деактивировать роли в компоненте управления привилегированными пользователями Azure AD
Компонент Azure Active Directory (AD) Privileged Identity Management упрощает управление привилегированным доступом пользователей к ресурсам в Azure AD и других веб-службах Майкрософт, включая Office 365 и Microsoft Intune.  

Если вам назначена роль временного администратора, то это значит, что вы можете активировать эту роль, когда вам потребуется выполнить привилегированные задачи. Например, если вы управляете компонентами Office 365 нерегулярно, корпоративные администраторы привилегированных ролей могут не назначить вам роль постоянного глобального администратора, так как эта роль влияет и на другие службы. Вместо этого они могут назначить вам более подходящие для работы с Azure AD роли, например роль администратора Exchange Online. При необходимости вы можете запросить активацию роли, чтобы получить административный доступ на определенный период времени.

Эта статья предназначена для администраторов, которым требуется активировать свои роли в компоненте управления привилегированными пользователями Azure AD. В ней описано, как активировать роль для включения необходимых разрешений, а затем деактивировать ее после завершения работы. Кроме того, для активации привилегированной роли администратора может потребоваться утверждение (в предварительной версии). Дополнительные сведения о [рабочих процессах утверждения в рамках управления привилегированными пользователями](./privileged-identity-management/azure-ad-pim-approval-workflow.md).

## <a name="add-the-privileged-identity-management-application"></a>Добавление приложения для управления привилегированными пользователями
Используйте приложение Azure AD Privileged Identity Management на [портале Azure](https://portal.azure.com/) , чтобы запросить активацию роли, даже если вы собираетесь работать на другом портале или в PowerShell. Если приложение Azure AD PIM еще не установлено на портале Azure, выполните следующие действия, чтобы приступить к работе.

1. Войдите на [портале Azure](https://portal.azure.com/).
2. Щелкните свое имя пользователя в правом верхнем углу портала Azure и выберите каталог, с которым будете работать.
3. Выберите **Другие службы** и в текстовое поле "Фильтр" введите **Azure AD Privileged Identity Management**.
4. Установите флажок **Закрепить на панели мониторинга** и нажмите кнопку **Создать**. Откроется приложение Privileged Identity Management.

## <a name="activate-a-role"></a>Активация роли
Когда вам потребуется какая-либо роль, вы можете запросить ее активацию, выбрав параметр **Мои роли** в левом столбце навигации приложения "Управление привилегированными пользователями" (PIM) Azure AD.

1. Войдите на [портал Azure](https://portal.azure.com/) и выберите элемент "Управление привилегированными пользователями Azure AD".
2. Выберите **Мои роли**. В верхней части страницы отобразится список соответствующих ролей, назначенных вам.
3. Выберите роль, которую нужно активировать.
4. Выберите **Активировать**. Отобразится колонка **Запросить активацию ролей** .
5. Для активации некоторых ролей требуется использование Многофакторной идентификации (MFA). Проверка подлинности выполняется один раз за сеанс.
   
    ![Проверка с помощью функции MFA перед активацией роли — снимок экрана][2]
6. В текстовом поле введите основание для запроса активации.  Для некоторых ролей требуется предоставить номер запроса на устранение неисправности.
7. Нажмите кнопку **ОК**.  Если роль не требует утверждения, она будет активирована и появится в списке активных ролей (непосредственно под списком назначенных ролей). Если [роль требует утверждения](./privileged-identity-management/azure-ad-pim-approval-workflow.md) для активации, в правом верхнем углу окна браузера ненадолго появится всплывающее уведомление о том, что запрос ожидает утверждения.

    ![Снимок экрана: уведомление о запросе, ожидающем утверждения][3]

## <a name="deactivate-a-role"></a>Деактивация роли
Активированная роль автоматически деактивируется через определенный период времени (соответствующую длительность).

Если вы завершили задачи администрирования раньше, роль также можно отключить вручную в приложении для управления привилегированными пользователями Azure AD.  Выберите **Мои роли**, выберите роль, которая больше не нужна, в группе **Активные назначения ролей**, а затем щелкните **Отключить**.  

## <a name="cancel-a-pending-request"></a>Отмена ожидающего запроса
Если вам не нужно активировать роль, требующую утверждения, вы можете в любой момент отменить запрос, ожидающий утверждения. Просто выберите параметр **Мои роли** в левом столбце навигации приложения "Управление привилегированными пользователями" (PIM) Azure AD.

1. Войдите на [портал Azure](https://portal.azure.com/) и выберите элемент "Управление привилегированными пользователями Azure AD".
2. Выберите **Мои роли**. В верхней части страницы отобразится список соответствующих ролей, назначенных вам.
3. Выберите роль.
4. Выберите баннер **Активация ожидает утверждения** в колонке сведений об активации роли.
5. Выберите **Отмена** в верхней части колонки **Ожидающие утверждения**.

   ![Снимок экрана: отмена ожидающего запроса][4]

## <a name="next-steps"></a>Дальнейшие действия
Ниже приведены ссылки на подробные сведения об управлении привилегированными пользователями Azure AD.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
