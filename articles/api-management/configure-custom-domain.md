---
title: "Настройка имени домена для экземпляра службы управления API Azure | Документация Майкрософт"
description: "В этом разделе описано, как настроить пользовательское имя домена для вашего экземпляра службы управления API Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 12/14/2017
ms.author: apimpm
ms.openlocfilehash: cf8a3eb502a808945e97822e10e44d38137d1161
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="configure-a-custom-domain-name"></a>Настройка пользовательского имени домена 

При создании экземпляра службы управления API (APIM) Azure назначает его поддомену домена azure-api.net (например, `apim-service-name.azure-api.net`). Но доступ к конечным точкам APIM можно предоставлять через собственное доменное имя, например **contoso.com**. В этом руководстве показано, как сопоставить существующее пользовательское DNS-имя с конечными точками, предоставленными экземпляром службы управления API Azure.

## <a name="prerequisites"></a>Технические условия

Чтобы выполнить действия, описанные в этой статье, необходимо следующее.

+ Активная подписка Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Экземпляр APIM. Дополнительные сведения см. в статье [Создание экземпляра службы управления API Azure](get-started-create-service-instance.md).
+ Пользовательское доменное имя, которое принадлежит вам. Используемое имя должно быть приобретено отдельно и размещено на DNS-сервере. Этот раздел не содержит инструкции по размещению пользовательского доменного имени.
+ Необходимо иметь действующий сертификат с открытым и закрытым ключами (PFX-файл). Субъект или альтернативное имя субъекта (SAN) должны соответствовать доменному имени (это позволяет APIM безопасно предоставлять URL-адреса по протоколу SSL).

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Настройка пользовательского доменного имени с помощью портала Azure

1. Перейдите к вашему экземпляру APIM на [портале Azure](https://portal.azure.com/).
2. Выберите **Личные домены и SSL**.
    
    Существует ряд конечных точек, которым можно назначить пользовательское доменное имя. Сейчас доступны следующие конечные точки: 
    + **Proxy** (по умолчанию: `<apim-service-name>.azure-api.net`), 
    + **Portal** (по умолчанию: `<apim-service-name>.portal.azure-api.net`),     
    + **Management** (по умолчанию: `<apim-service-name>.management.azure-api.net`), 
    + **SCM** (по умолчанию: `<apim-service-name>.scm.azure-api.net`).

    >[!NOTE]
    > Можно обновить все конечные точки или некоторые из них. Как правило, клиенты обновляют конечные точки **Proxy** (этот URL-адрес используется для вызова API, предоставляемого через службу управления API) и **Portal** (URL-адрес портала разработчика). Конечные точки **Management** и **SCM** используются для внутренних целей клиентами APIM, поэтому им реже назначается пользовательское доменное имя.
3. Выберите конечную точку, которую необходимо обновить. 
4. В окне справа выберите **Настраиваемое**.

    + В поле **Пользовательское доменное имя** укажите имя, которое следует использовать. Например, `api.contoso.com`. <br/>Поддерживаются также доменные имена с подстановочными знаками (например, *. domain.com).
    + В поле **Сертификат** укажите действительный PFX-файл, который требуется отправить. 
    + Если у сертификата есть пароль, введите его в поле **Пароль**.
1. Нажмите кнопку "Применить".

    >[!NOTE]
    >Процесс назначения сертификата может занять примерно 15 минут.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Дальнейшие действия

[Повышение категории и масштабирование службы](upgrade-and-scale.md)