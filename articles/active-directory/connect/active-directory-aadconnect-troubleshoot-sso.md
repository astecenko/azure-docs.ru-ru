---
title: "Azure Active Directory Connect: устранение неполадок с простым единым входом | Документы Майкрософт"
description: "В этом разделе описывается устранение неполадок с простым единым входом Azure Active Directory."
services: active-directory
keywords: "что такое Azure AD Connect, установка Active Directory, необходимые компоненты для Azure AD, единый вход"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: billmath
ms.openlocfilehash: aa28431c5926656ae97ded3f23b83f2a91c60487
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/05/2018
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Устранение неполадок с простым единым входом Azure Active Directory

Эта статья поможет вам найти информацию об устранении распространенных неполадок с простым единым входом Azure Active Directory (Azure AD).

## <a name="known-problems"></a>Известные проблемы

- В некоторых случаях включение простого единого входа может занять до 30 минут.
- Если отключить и повторно включить прозрачную единый вход для клиента, пользователи не получат единого входа до их билеты кэшированных Kerberos, как правило, действительны в течение 10 часов, истек срок действия.
- Браузер Edge не поддерживается.
- При запуске клиентов Office, особенно на общих компьютерах, выдаются дополнительные приглашения на вход в систему для пользователей. Пользователи должны часто вводить свои имена пользователей, но не пароли.
- После успешного единого входа пользователю не предоставляется возможность выбрать вариант **Оставаться в системе**. Из-за этого сценарии сопоставления SharePoint и OneDrive не поддерживаются.
- Простой единый вход не работает в конфиденциальном режиме просмотра в Firefox.
- Простой единый вход не работает в Internet Explorer с включенным режимом повышенной защиты.
- Простой единый вход не работает в браузерах на мобильных устройствах с iOS и Android.
- При синхронизации 30 лесов Active Directory или больше простой единый вход через Azure AD Connect включить невозможно. Чтобы избежать этого, можно [вручную включить](#manual-reset-of-azure-ad-seamless-sso) эту функцию на своем клиенте.
- Добавление URL-адресов службы Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) в зону "Надежные сайты" вместо зоны "Местная интрасеть" *блокирует вход пользователей*.

## <a name="check-the-status-of-the-feature"></a>Проверка состояния функции

Убедитесь, что функция простого единого входа по-прежнему **включена** в клиенте. Можно проверить состояние, перейдя в область **Azure AD Connect** в [центре администрирования Azure Active Directory](https://aad.portal.azure.com/).

![Центр администрирования Active Directory Azure — область "Azure AD Connect"](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Причины сбоя единого входа в центре администрирования Azure Active Directory (требуется лицензия Premium)

Если к клиенту привязана лицензия Azure AD Premium, вы можете также изучить [отчет о действиях при входе](../active-directory-reporting-activity-sign-ins.md) в [центре администрирования Azure Active Directory](https://aad.portal.azure.com/).

![Центр администрирования Azure Active Directory — отчет о действиях входа](./media/active-directory-aadconnect-sso/sso9.png)

Перейдите в раздел **Azure Active Directory** > **Вход в систему** в [центре администрирования Azure Active Directory](https://aad.portal.azure.com/) и щелкните конкретное действие входа пользователя. Найдите поле **КОД ОШИБКИ ВХОДА**. Сопоставьте значение этого поля c причиной сбоя и способом разрешения с помощью следующей таблицы.

|Код ошибки входа|Причина ошибки входа|Способы устранения:
| --- | --- | ---
| 81001 | Билет Kerberos пользователя слишком большой. | Сократите список членства пользователя в группах и повторите попытку.
| 81002 | Не удалось проверить билет Kerberos пользователя. | Изучите [контрольный список по устранению неполадок](#troubleshooting-checklist).
| 81003 | Не удалось проверить билет Kerberos пользователя. | Изучите [контрольный список по устранению неполадок](#troubleshooting-checklist).
| 81004 | Проверка подлинности Kerberos завершилась сбоем. | Изучите [контрольный список по устранению неполадок](#troubleshooting-checklist).
| 81008 | Не удалось проверить билет Kerberos пользователя. | Изучите [контрольный список по устранению неполадок](#troubleshooting-checklist).
| 81009 | Не удалось проверить билет Kerberos пользователя. | Изучите [контрольный список по устранению неполадок](#troubleshooting-checklist).
| 81010 | Не удалось выполнить простой единый вход, так как срок действия билета Kerberos пользователя истек или этот билет недопустим. | Пользователю следует войти с устройства, присоединенного к домену, в корпоративной сети.
| 81011 | Не удалось найти объект-пользователь на основе сведений в билете Kerberos пользователя. | Используйте Azure AD Connect для синхронизации сведений о пользователе с Azure AD.
| 81012 | Пользователь, пытающийся выполнить вход в Azure AD, и пользователь устройства отличаются. | Пользователю нужно войти с другого устройства.
| 81013 | Не удалось найти объект-пользователь на основе сведений в билете Kerberos пользователя. |Используйте Azure AD Connect для синхронизации сведений о пользователе с Azure AD. 

## <a name="troubleshooting-checklist"></a>Контрольный список по устранению неполадок

Используйте следующий контрольный список для устранения неполадок простого единого входа.

- Проверьте, включена ли функция простого единого входа в Azure AD Connect. Если функцию не удается включить (например, из-за заблокированного порта), обеспечьте соблюдение всех [предварительных требований](active-directory-aadconnect-sso-quick-start.md#step-1-check-the-prerequisites).
- Если вы включили оба [присоединения Azure AD](../active-directory-azureadjoin-overview.md) и эффективная единого входа для клиента, убедитесь, что проблема не связана с присоединения Azure AD. Единого входа из присоединения Azure AD имеет приоритет над прозрачную единого входа, если устройство не зарегистрированные в Azure AD и домену. С помощью единого входа из присоединения Azure AD пользователь видит Плитка вход с надписью «Подключение к Windows».
- Убедитесь, что оба URL-адреса службы Azure AD (https://autologon.microsoftazuread-sso.com и https://aadg.windows.net.nsatc.net) указаны в параметрах зоны интрасети пользователя.
- Убедитесь, что корпоративное устройство присоединено к домену Active Directory.
- Убедитесь, что пользователь вошел в систему с этого устройства с помощью доменной учетной записи Active Directory.
- Убедитесь, что учетная запись пользователя относится к лесу Active Directory, в котором настроен простой единый вход.
- Убедитесь, что устройство подключено к корпоративной сети.
- Убедитесь, что время на устройстве синхронизировано с временем в Active Directory и на контроллерах доменов, и что разница между временем на контроллерах домена не превышает пяти минут.
- Выведите список билетов Kerberos на устройстве с помощью команды `klist` из командной строки. Убедитесь в наличии билетов, выданных для учетной записи компьютера `AZUREADSSOACCT`. Билеты Kerberos пользователей обычно допустимы для 10 часов. В Active Directory могут быть заданы другие параметры.
- Если отключить и снова включить прозрачную единого входа для клиента, пользователи не получат единого входа до их кэшированных билетов Kerberos, истек срок действия.
- Удалите имеющиеся билеты Kerberos с устройства с помощью команды `klist purge` и повторите попытку.
- Чтобы определить наличие проблем, связанных с JavaScript, просмотрите журналы консоли из браузера (в разделе **Средства разработчика**).
- Просмотрите [журналы контроллеров домена](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Журналы контроллеров домена

Если на контроллере домена включен аудит успешных попыток, то каждый раз, когда пользователь входит в систему с использованием простого единого входа, в журнал событий заносится запись безопасности. Эти события безопасности можно найти с помощью приведенного ниже запроса. (Найдите событие **4769**, связанное с учетной записью компьютера **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Ручной сброс функции

Если устранение неполадок не помогло, можно сбросить вручную эту функцию на клиенте. Выполните следующие действия на локальном сервере, на котором выполняется Azure AD Connect.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Шаг 1. Импортируйте модуль PowerShell для простого единого входа

1. Скачайте и установите [помощник по входу в Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Скачайте и установите [64-разрядную версию модуля Azure Active Directory для Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Перейдите в папку `%programfiles%\Microsoft Azure Active Directory Connect`.
4. Импортируйте модуль PowerShell для простого единого входа с помощью следующей команды: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Шаг 2. Получение списка лесов Active Directory, для которых включен простой единый вход

1. Откройте сеанс PowerShell от имени администратора. В PowerShell вызовите `New-AzureADSSOAuthenticationContext`. При запросе введите учетные данные глобального администратора своего клиента.
2. Вызовите `Get-AzureADSSOStatus`. Эта команда выводит список лесов Active Directory (см. список "Домены"), в которых включена эта функция.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>Шаг 3. Отключение простого единого входа для каждого леса Active Directory, в котором настроена эта функция

1. Вызовите `$creds = Get-Credential`. При запросе введите свои учетные данные администратора домена для нужного леса Active Directory.
2. Вызовите `Disable-AzureADSSOForest -OnPremCredentials $creds`. Эта команда удаляет учетную запись компьютера `AZUREADSSOACCT` с локального контроллера домена для конкретного леса Active Directory.
3. Повторите предыдущие шаги для каждого леса Active Directory, в котором настроена эта функция.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Шаг 4. Включение простого единого входа для каждого леса Active Directory

1. Вызовите `Enable-AzureADSSOForest`. При запросе введите свои учетные данные администратора домена для нужного леса Active Directory.
2. Повторите предыдущие шаги для каждого леса Active Directory, в котором должна быть настроена эта функция.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Шаг 5. Включить функцию в своем клиенте

Чтобы включить эту функцию в клиенте, вызовите `Enable-AzureADSSO` и введите **true** в командной строке `Enable:`.
