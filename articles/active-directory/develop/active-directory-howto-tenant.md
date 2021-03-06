---
title: "Как получить клиент Azure AD | Документация Майкрософт"
description: "Как получить клиент Azure Active Directory для регистрации и создания приложений."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 5874e6ce7d19c5106bc88ce9ff7fddd1842e0c3b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2017
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Как получить клиент Azure Active Directory
В Azure Active Directory (Azure AD) [клиент](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) представляет организацию.  Это выделенный экземпляр службы Azure AD, который она получает и которым владеет при подписке на облачные службы Майкрософт, такие как Azure, Office 365 или Microsoft Intune.  Каждый клиент Azure AD отделен от остальных клиентов Azure AD.  

Клиент содержит пользователей в организации и информацию о них, такую как пароли, данные профилей пользователей, разрешения и т. д.  Кроме того, он содержит группы, приложения и другие данные, относящиеся к организации и ее системе безопасности.

Чтобы разрешить пользователям Azure AD вход в ваше приложение, необходимо зарегистрировать его в своем клиенте.  Публикация приложения в клиенте Azure AD **абсолютно бесплатна**.  На самом деле большинство разработчиков создают несколько клиентов и приложений для экспериментов, разработки, промежуточного хранения и тестирования.  Организации, которые подписались на ваше приложение и используют его, при необходимости могут приобрести лицензию, если они хотят воспользоваться преимуществами расширенных функций каталога.

Как вы хотите получить клиент Azure AD?  Процесс может отличаться в зависимости от начальных условий:

* [Наличие существующей подписки Office 365](#use-an-existing-office-365-subscription)
* [Наличие существующей подписки Azure, связанной с учетной записью Майкрософт](#use-an-msa-azure-subscription)
* [Наличие существующей подписки Azure, связанной с учетной записью организации](#use-an-organizational-azure-subscription)
* [Нет ничего из перечисленного выше, и нужно создать клиент с нуля](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Использование существующей подписки Office 365
Если у вас есть подписка Office 365, у вас также есть клиент Azure AD. Вы можете войти на [портал Azure](https://portal.azure.com), используя учетную запись O365, и приступить к работе с Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Использование подписки Azure, связанной с учетной записью Майкрософт
Если ранее вы уже зарегистрировались для получения подписки Azure с помощью своей учетной записи Майкрософт, у вас уже есть клиент!  При входе на [портал Azure](https://portal.azure.com) автоматически выполняется вход в клиент, используемый по умолчанию. Вы можете использовать этот клиент по своему усмотрению, но может потребоваться создать учетную запись администратора организации.

Для этого выполните следующие действия.  Кроме того, вам может понадобиться создать новый клиент и его администратора по аналогичному принципу.

1. Войдите на [портал Azure](https://portal.azure.com), используя свою учетную запись.
2. На портале перейдите к разделу Azure Active Directory (он находится на панели навигации слева в разделе **Больше служб**).
3. Вы автоматически войдете в каталог по умолчанию. В противном случае, чтобы сменить каталог, щелкните имя своей учетной записи в правом верхнем углу.
4. В разделе **Быстрые задачи** выберите **Добавить пользователя**.
5. В форме добавления пользователя введите следующие данные:

   * имя — выберите соответствующие значения;
   * «Имя пользователя» — выберите имя пользователя для этого администратора;
   * профиль — введите соответствующие значения в полях имени, фамилии, должности и подразделения;
   * «Роль» — «Глобальный администратор»;
6. После заполнения формы добавления пользователя и после получения временного пароля для нового пользователя с правами администратора не забудьте записать его, так как вам понадобится войти под именем этого нового пользователя, чтобы изменить пароль. Кроме того, пароль можно отправить непосредственно пользователю на альтернативную почту.
7. Щелкните **Создать**, чтобы создать нового пользователя.
8. Чтобы изменить временный пароль, войдите на сайт [https://login.microsoftonline.com](https://login.microsoftonline.com), используя учетную запись нового пользователя, и измените пароль по запросу.

## <a name="use-an-organizational-azure-subscription"></a>Использование подписки организации Azure
Если вы уже ранее выполнили подписку Azure с помощью своей учетной записи организации, у вас уже есть клиент!  Его можно найти на [портале Azure](https://portal.azure.com), выбрав "Больше служб" и Azure Active Directory.  Вы можете использовать этот клиент по своему усмотрению.

## <a name="start-from-scratch"></a>Создание клиента с нуля
Если вам не понятно ничего из указанного выше, не беспокойтесь. Просто посетите [портал Azure](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory), чтобы создать каталог Azure AD. После выполнения процесса вы получите собственный клиент Azure AD с доменным именем, выбранным во время подписки.  Его можно найти на [портале Azure](https://portal.azure.com) в разделе **Azure Active Directory**, расположенном на панели навигации слева.
