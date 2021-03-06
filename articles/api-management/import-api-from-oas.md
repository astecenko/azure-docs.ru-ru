---
title: "Импорт спецификации OpenAPI с помощью портала Azure | Документация Майкрософт"
description: "Сведения об импорте спецификации OpenAPI с помощью службы управления API."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Импорт спецификации OpenAPI

В этой статье показано, как импортировать API серверной части "Спецификация OpenAPI", размещенный по адресу http://conferenceapi.azurewebsites.net?format=json. Этот API серверной части предоставляется корпорацией Майкрософт и размещен в Azure. Также здесь объясняется, как проверить API службы управления API Azure.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Импорт API серверной части "Спецификация OpenAPI"
> * Проверка API на портале Azure
> * Проверка API на портале разработчика

## <a name="prerequisites"></a>Предварительные требования

Выполните задачи в кратком руководстве по [созданию экземпляра службы управления API Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Импорт и публикация API серверной части

1. Выберите **Интерфейсы API** в разделе **Управление API**.
2. Выберите **Спецификация OpenAPI** из списка **Добавление нового API**.
    ![Спецификация OpenAPI](./media/import-api-from-oas/oas-api.png)
3. Введите соответствующие параметры. Все значения параметров API вы можете указать прямо во время создания. Кроме того, некоторые из них можно настроить позднее на вкладке **Параметры**. <br/> Если на нажать клавиши **Табуляция**, часть полей (или даже все поля) заполняется информацией, полученной от указанной службы серверной части.

    ![Создание API](./media/api-management-get-started/create-api.png)

    |Настройка|Значение|Описание|
    |---|---|---|
    |**Спецификация OpenAPI**|http://conferenceapi.azurewebsites.net?format=json|Определяет службу, которая реализует API-интерфейс. Портал управления API направит запросы по этому адресу.|
    |**Отображаемое имя**|*Demo Conference API*|Если нажать клавишу табуляции после ввода URL-адреса службы, служба управления API заполнит это поле информацией из объекта json. <br/>Это имя отображается на портале разработчика.|
    |**Имя**|*demo-conference-api*|Содержит уникальное имя API. <br/>Если нажать клавишу табуляции после ввода URL-адреса службы, служба управления API заполнит это поле информацией из объекта json.|
    |**Описание**|Необязательное описание API.|Если нажать клавишу табуляции после ввода URL-адреса службы, служба управления API заполнит это поле информацией из объекта json.|
    |**Суффикс URL-адреса API**|*conference*|Этот суффикс добавляется к основному URL-адресу службы управления API. API Management отличает интерфейсы API по их суффиксу. Следовательно, суффикс должен быть уникальным для каждого API для заданного издателя.|
    |**Схема URL-адресов**|*HTTPS*|Определяет, какие протоколы можно использовать для доступа к этому API. |
    |**Продукты**|*Unlimited*| Чтобы опубликовать API, его нужно связать с определенным продуктом. Чтобы добавить новый API к продукту (необязательно), введите здесь название продукта. Этот шаг можно повторить несколько раз, чтобы добавить API в несколько продуктов.<br/>Продуктами называют ассоциации из одного или нескольких API. Вы можете объединить в них некоторое количество API и предлагать их разработчикам через портал разработчика. Чтобы получить доступ к API, разработчику нужно подписаться на соответствующий продукт. При этом он получит ключ подписки, который подходит для любого API в этом продукте. Создавая экземпляр службы управления API, вы автоматически становитесь его администратором. Поэтому вы по умолчанию будете подписаны на все продукты.<br/> По умолчанию каждый экземпляр службы управления API создается с двумя демонстрационными продуктами: **Начальный** и **Без ограничений**. |

4. Нажмите кнопку **Создать**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Проверка нового API службы управления API на портале Azure

Операции можно вызывать на портале Azure, где предоставляется удобный способ для просмотра и проверки операций API.  

1. Выберите API, созданный на предыдущем шаге.
2. Откройте вкладку **Тест**.

    ![Проверка API](./media/api-management-get-started/test-api.png)
1. Щелкните **GetSpeakers**.

    Эта страница отображает поля для параметров запроса, но в нашем примере параметры не используются. Также на этой странице отображаются поля для заголовков. Один из заголовков, Ocp-Apim-Subscription-Key, содержит ключ подписки для продукта, связанного с этим API. Как создатель экземпляра службы управления API, вы автоматически являетесь администратором, поэтому сведения о ключе будут заполнены автоматически. 
4. Нажмите кнопку **Отправить**.

    Служба серверной части вернет ответ **200 ОК** и некоторые данные.

## <a name="call-operation"> </a>Вызов операции с портала разработчика

Также операции можно вызвать через **портал разработчиков**, чтобы проверить API. 

1. Выберите API, который вы создали на шаге "Импорт и публикация API серверной части".
2. Щелкните **Портал разработчика**.

    ![Проверка через портал разработчика](./media/api-management-get-started/developer-portal.png)

    Откроется сайт портала разработчика.
3. Выберите **API**.
4. Выберите **Demo Conference API**.
5. Щелкните **GetSpeakers**.
    
    Эта страница отображает поля для параметров запроса, но в нашем примере параметры не используются. Также на этой странице отображаются поля для заголовков. Один из заголовков, Ocp-Apim-Subscription-Key, содержит ключ подписки для продукта, связанного с этим API. Как создатель экземпляра службы управления API, вы автоматически являетесь администратором, поэтому сведения о ключе будут заполнены автоматически.
6. Щелкните **Попробовать**.
7. Нажмите кнопку **Отправить**.
    
    После вызова операции портал разработчика отображает **состояние ответа**, **заголовки ответа** и **содержимое ответа**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Преобразование и защита опубликованного API](transform-api.md)
