---
title: "Состояние пользователей в службе Многофакторной идентификации Azure Microsoft Azure"
description: "Узнайте подробнее о состояниях пользователей в службе Многофакторной идентификации Azure."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: ad8d531d633eb65fe90404fdab0499b8e5332db6
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Как настроить требование двухфакторной проверки подлинности пользователя или группы

Двухфакторную проверку подлинности можно требовать, используя один из двух подходов. Первый способ — включить многофакторную идентификацию Azure (MFA) для каждого отдельного пользователя. В этом случае пользователи выполняют двухфакторную проверку подлинности при каждом входе (за некоторыми исключениями, к примеру, если они входят из доверенных IP-адресов или включена функция _запоминания устройств_). Второй способ — настроить политику условного доступа, которая требует прохождения двухфакторной проверки подлинности при определенных условиях.

>[!TIP] 
>Выберите один из этих методов, чтобы требовать прохождения двухфакторной проверки подлинности, но не оба. Включение Azure MFA для пользователя переопределяет все политики условного доступа.

## <a name="which-option-is-right-for-you"></a>Какой способ подходит для вас

**Включение Azure MFA путем изменения состояния пользователя** — это традиционный подход к применению двухфакторной проверки подлинности. Он подходит и для Azure MFA в облаке, и для сервера Azure MFA. Все пользователи, для которых включена такая аутентификация, выполняют двухфакторную проверку подлинности при каждом входе. Включение такой аутентификации для пользователя переопределяет все политики условного доступа, которые могут действовать для него. 

**Включение Azure MFA с политикой условного доступа** — более гибкий подход к применению двухфакторной проверки подлинности. Он подходит только для Azure MFA в облаке, при этом _условный доступ_ — [платная функция Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Можно создать политики условного доступа, применяемые как к группам, так и к отдельным пользователям. Для групп с высоким риском можно задать больше ограничений, чем для групп с низким риском. Или вы можете сделать двухфакторную проверку подлинности обязательной только для облачных приложений с высоким риском, пропуская ее для приложений с низким риском. 

В обоих случаях при первом входе пользователей после включения обязательной проверки подлинности им предлагается зарегистрироваться в MFA. Оба варианта также позволяют настроить [параметры MFA](multi-factor-authentication-whats-next.md).

## <a name="enable-azure-mfa-by-changing-user-status"></a>Включение Azure MFA путем изменения состояния пользователя

Учетные записи пользователей в службе Многофакторной идентификации Azure имеют три различных состояния:

| Status | ОПИСАНИЕ | Затронутые приложения, не использующие браузер | Затронутые приложения, использующие браузер | Затронутая современная аутентификация |
|:---:|:---:|:---:|:--:|:--:|
| Отключено |Состояние по умолчанию для нового пользователя, не зарегистрированного в Azure MFA. |Нет  |Нет  |Нет  |
| Включено |Пользователь указан в Azure MFA, но не зарегистрирован. Ему будет предложено зарегистрироваться при следующем входе в систему. |Нет.  Они будут продолжать работать, пока не завершится регистрация. | Да. После окончания сеанса требуется регистрация в Azure MFA.| Да. После окончания срока действия маркера доступа требуется регистрация в Azure MFA. |
| Принудительно |Пользователь указан и зарегистрирован для использования Azure MFA. |Да.  Для приложений нужны пароли приложений. |Да. Аутентификация в Azure MFA требуется при входе в систему. | Да. Аутентификация в Azure MFA требуется при входе в систему. |

Состояние пользователя отражает, указал ли администратор его в Azure MFA и завершил ли пользователь процесс регистрации.

Все пользователи начинают с состояния *Отключено*. При указании пользователей в Azure MFA их состояние меняется на *Включено*. После включения пользователи входят в систему и завершают регистрацию, затем их состояние меняется на *Enforced* (Принудительно).  

### <a name="view-the-status-for-a-user"></a>Просмотр состояния пользователя

Чтобы открыть страницу, на которой можно просмотреть состояние пользователей и управлять им, выполните следующее.

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Последовательно выберите **Azure Active Directory** > **Пользователи и группы** > **Все пользователи**.
3. Выберите **Многофакторная идентификация**.
   ![Выберите Многофакторную идентификации](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Откроется новая страница, где будет показано состояние пользователя.
   ![Состояние пользователя Многофакторной идентификации (снимок экрана)](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Изменение состояния пользователя

1. Выполните шаги выше, чтобы перейти на страницу **пользователей** MFA.
2. Найдите пользователя, для которого требуется включить Azure MFA. Возможно, потребуется изменить представление в верхней части страницы. 
   ![Поиск пользователя (снимок экрана)](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Установите флажок рядом с именем пользователя.
4. Справа, в разделе **быстрых действий**, щелкните **Включить** или **Отключить**.
   ![Включение MFA для выбранного пользователя (снимок экрана)](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >Пользователи с состоянием *Включено* автоматически перейдут в состояние *Enforced* (Принудительно) при регистрации в службе Azure MFA. Не изменяйте состояние пользователя вручную на *Enforced* (Принудительно). 

5. Подтвердите свой выбор во всплывающем окне, которое откроется. 

После включения уведомите пользователей по электронной почте. Сообщите, что им будет предложено зарегистрироваться при следующем входе. Кроме того, если ваша организация использует небраузерные приложения, не поддерживающие современную аутентификацию, потребуется создать пароли приложений. Можно также добавить ссылку на [руководство для пользователей Azure MFA](./end-user/multi-factor-authentication-end-user.md), чтобы им было проще приступить к работе.

### <a name="use-powershell"></a>Использование PowerShell
Чтобы изменить состояние пользователя с помощью [Azure AD PowerShell](/powershell/azure/overview), измените `$st.State`. Существуют три возможных состояния:

* Включено
* Принудительно
* Отключено  

Не переводите пользователей непосредственно в состояние *Применено*. Если сделать это, приложения, не использующие браузер, перестанут работать, так как пользователь не прошел регистрацию в Azure MFA и не получил [пароль приложения](multi-factor-authentication-whats-next.md#app-passwords). 

PowerShell — удобный инструмент для массового включения пользователей. Создайте сценарий PowerShell, который обходит список пользователей и включает их.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Следующий сценарий является примером.

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Включение Azure MFA с политикой условного доступа

_Условный доступ_ — платная функция Azure Active Directory, предусматривающая множество вариантов конфигурации. Ниже описан один из способов создания политики. Дополнительные сведения см. в разделе [Условный доступ в Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Выберите **Azure Active Directory** > **Условный доступ**.
3. Выберите **Новая политика**.
4. В разделе **Назначения** выберите **Пользователи и группы**. На вкладках **Включить** и **Исключить** укажите пользователей, подпадающих под политику.
5. В разделе **Назначения** выберите **Облачные приложения**. Выберите параметр **Все облачные приложения**, чтобы включить все облачные приложения.
6. В разделе **Элементы управления доступом** выберите **Предоставление**. Выберите **Требовать многофакторную проверку подлинности**.
7. Задайте для параметра **Включить политику** значение **Вкл.**, а затем щелкните **Сохранить**.

Другие параметры в политике условного доступа позволяют точно указать, когда требуется применять двухфакторную проверку подлинности. Например, можно создать политику для такой ситуации: подрядчики пытаются обратиться к нашему приложению для закупок из ненадежных сетей на устройствах, не присоединенных к домену, им требуется пройти двухфакторную проверку подлинности. 

## <a name="next-steps"></a>Дополнительная информация

- Получите советы в статье [Рекомендации по работе с условным доступом](../active-directory/active-directory-conditional-access-best-practices.md).

- Управляйте параметрами MFA для [пользователей и их устройств](multi-factor-authentication-manage-users-and-devices.md).
