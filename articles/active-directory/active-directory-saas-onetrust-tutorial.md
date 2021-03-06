---
title: "Учебник: Интеграция Azure Active Directory с программным обеспечением конфиденциальности OneTrust | Документы Microsoft"
description: "Подробные сведения о настройке единого входа между Azure Active Directory и программное обеспечение для управления OneTrust конфиденциальности."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71c2b6d0-3d28-4130-a2c8-1e72ab3d5814
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jeedes
ms.openlocfilehash: 89d657e754f6908a3afbb615a2158c9d7f3380f9
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/16/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onetrust-privacy-management-software"></a>Учебник: Интеграция Azure Active Directory с программным обеспечением конфиденциальности OneTrust

В этом учебнике вы узнаете, как интегрировать программное обеспечение управления конфиденциальности OneTrust с Azure Active Directory (Azure AD).

Интеграция программного обеспечения управления конфиденциальности OneTrust с Azure AD предоставляет следующие преимущества:

- Можно управлять в Azure AD, имеющего доступ к программному обеспечению управления конфиденциальности OneTrust.
- Вы можете включить их учетные записи Azure AD пользователям автоматически получить вошедшего OneTrust программном обеспечении управления конфиденциальности (Single Sign-On).
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Технические условия

Чтобы настроить интеграцию Azure AD с программным обеспечением конфиденциальности OneTrust, необходимы следующие элементы:

- подписка Azure AD;
- Программное обеспечение управления конфиденциальности OneTrust единый вход включен подписки

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление программного обеспечения управления конфиденциальности OneTrust из галереи
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-onetrust-privacy-management-software-from-the-gallery"></a>Добавление программного обеспечения управления конфиденциальности OneTrust из галереи
Чтобы настроить интеграцию OneTrust конфиденциальности управления программного обеспечения в Azure AD, необходимо добавить программное обеспечение управления конфиденциальности OneTrust из коллекции в список управляемых приложений SaaS.

**Чтобы добавить программное обеспечение управления конфиденциальности OneTrust из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **программное обеспечение управления конфиденциальности OneTrust**выберите **программное обеспечение управления конфиденциальности OneTrust** из панели результатов щелкните **добавить** кнопку, чтобы добавить приложение.

    ![Программное обеспечение для управления OneTrust конфиденциальность в списке результатов](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе настройки и тестирования в Azure AD единого входа с программным обеспечением управления конфиденциальности OneTrust основании тестового пользователя с именем «Britta Simon».

Azure AD для единого входа для работы, необходимо знать пользователя аналога в программное обеспечение управления конфиденциальности OneTrust для пользователя в Azure AD. Другими словами связи между пользователя Azure AD и соответствующих пользователей в программном обеспечении управления конфиденциальности OneTrust необходимо установить.

В программном обеспечении управления OneTrust конфиденциальности, присвойте значение **имя пользователя** в Azure AD в качестве значения **Username** для установления связи.

Для настройки и проверки Azure AD единого входа с программным обеспечением конфиденциальности OneTrust, необходимые для выполнения следующих блоков.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя программное обеспечение управления конфиденциальности OneTrust](#create-a-onetrust-privacy-management-software-test-user)**  — на аналог Саймон Britta в программное обеспечение управления OneTrust конфиденциальности, связанного с представлением Azure AD пользователя.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе включения Azure AD единого входа на портале Azure и настройки единого входа в приложении OneTrust программное обеспечение управления конфиденциальности.

**Чтобы настроить Azure AD единого входа с программным обеспечением конфиденциальности OneTrust, выполните следующие действия.**

1. На портале Azure на **программное обеспечение управления конфиденциальности OneTrust** странице интеграции приложения щелкните **единого входа**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_samlbase.png)

3. На **URL-адреса и домена программного обеспечения управления конфиденциальности OneTrust** выполните следующие действия, если вы хотите настроить приложение в **IDP** инициировал режим:

    ![Домена программного обеспечения управления OneTrust конфиденциальности и URL-адреса единого входа сведения](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_url.png)

    a. В текстовом поле **Идентификатор** введите URL-адрес: `https://www.onetrust.com/saml2`

    Б. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<subdomain>.onetrust.com/auth/consumerservice`.

4. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Домена программного обеспечения управления OneTrust конфиденциальности и URL-адреса единого входа сведения](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<subdomain>.onetrust.com/auth/login`
     
    > [!NOTE] 
    > Эти значения приведены в качестве примера. Обновите эти значения в фактический URL-адрес ответа и URL-адрес входа. Обратитесь к [группа поддержки клиентского программного обеспечения управления конфиденциальности OneTrust](mailto:support@onetrust.com) для получения этих значений. 

5. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_certificate.png) 

6. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/active-directory-saas-onetrust-tutorial/tutorial_general_400.png)

7. Для настройки единого входа на **программное обеспечение управления конфиденциальности OneTrust** стороны, необходимо отправить загруженного **метаданные в формате XML** для [группа поддержки программное обеспечение управления конфиденциальности OneTrust](mailto:support@onetrust.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/active-directory-saas-onetrust-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/active-directory-saas-onetrust-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/active-directory-saas-onetrust-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/active-directory-saas-onetrust-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-a-onetrust-privacy-management-software-test-user"></a>Создание тестового пользователя программное обеспечение для управления OneTrust конфиденциальности

Цель этого раздела является создание пользователя с именем Саймон Britta в программное обеспечение управления OneTrust конфиденциальности. Программное обеспечение управления конфиденциальности OneTrust поддерживает подготовку just-in-time, который по умолчанию включен. В этом разделе никакие действия с вашей стороны не требуются. При попытке получить доступ к программному обеспечению управления конфиденциальности OneTrust, если он еще не существует, создается новый пользователь.

>[!Note]
>Если необходимо вручную создать пользователя, обратитесь в службу [группа поддержки программное обеспечение управления конфиденциальности OneTrust](mailto:support@onetrust.com).

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе включите Саймон Britta для использования Azure единого входа путем предоставления доступа к программному обеспечению управления конфиденциальности OneTrust.

![Назначение роли пользователя][200] 

**Чтобы назначить Саймон Britta OneTrust программном обеспечении управления конфиденциальности, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **программное обеспечение управления конфиденциальности OneTrust**.

    ![Программное обеспечение управления конфиденциальности OneTrust ссылку в список приложений](./media/active-directory-saas-onetrust-tutorial/tutorial_onetrust_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

При нажатии кнопки программное обеспечение управления конфиденциальности OneTrust плитки на панели доступа, вы должны получить автоматически вошедшего в приложение OneTrust программное обеспечение управления конфиденциальности.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onetrust-tutorial/tutorial_general_203.png

