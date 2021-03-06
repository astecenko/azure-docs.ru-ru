---
title: "Импорт SOAP API и преобразование в REST с помощью портала Azure | Документация Майкрософт"
description: "Сведения о том, как импортировать SOAP API и преобразовать его в REST с помощью службы управления API."
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
ms.openlocfilehash: 74935f11d5bf3c83aef46c1d41fccd81ad312acb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2017
---
# <a name="import-a-soap-api-and-convert-to-rest"></a>Импорт SOAP API и преобразование в REST

Из этой статьи вы узнаете, как импортировать SOAP API и преобразовать его в REST. Также здесь показано, как проверить API службы управления API.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Импорт SOAP API и преобразование в REST
> * Проверка API на портале Azure
> * Проверка API на портале разработчика

## <a name="prerequisites"></a>Предварительные требования

Выполните задачи в кратком руководстве по [созданию экземпляра службы управления API Azure](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Импорт и публикация API серверной части

1. Выберите **Интерфейсы API** в разделе **УПРАВЛЕНИЕ API**.
2. Выберите **WSDL** в списке **Add a new API** (Добавление нового API).

    ![SOAP API](./media/restify-soap-api/wsdl-api.png)
3. В поле **Спецификация WSDL** введите URL-адрес, по которому располагается SOAP API.
4. Нажмите переключатель **SOAP в REST**. Если выбран этот параметр, в службе управления API выполняется попытка автоматического преобразования между XML и JSON. В этом случае объектам-получателям следует вызывать API в режиме REST, в котором возвращается JSON. Служба управления API преобразует каждый запрос в вызов SOAP.

    ![SOAP в REST](./media/restify-soap-api/soap-to-rest.png)

5. Нажмите клавишу TAB.

    Следующие поля будут заполнены информацией из SOAP API: "Отображаемое имя", "Имя", "Описание".
6. Добавьте суффикс URL-адреса API. Суффиксом называется имя, которое идентифицирует конкретный API в экземпляре службы управления API. Он должен быть уникальным в пределах экземпляра службы управления API.
9. Чтобы опубликовать API, его нужно связать с определенным продуктом. В нашем случае используется продукт *Без ограничений*.  Если вы планируете опубликовать API и предоставить разработчикам доступ к нему, добавьте его в продукт. Это можно сделать во время создания API или в любое время позже.

    Продуктами называют ассоциации из одного или нескольких API. Вы можете объединить в них определенное количество API и предлагать их разработчикам через портал разработчика. Чтобы получить доступ к API, разработчику нужно подписаться на соответствующий продукт. Затем разработчик получит ключ подписки, подходящий ко всем API-интерфейсам в этом продукте. Создавая экземпляр службы управления API, вы автоматически становитесь его администратором. Поэтому вы по умолчанию будете подписаны на все продукты.

    По умолчанию каждый экземпляр API управления поставляется с двумя демонстрационными продуктами:

    * **Starter**
    * **Unlimited**   
10. Нажмите кнопку **Создать**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Проверка нового API службы управления API на портале Azure

Операции можно вызывать непосредственно с портала Azure, где предоставляется удобный способ просмотра и проверки операций API.  

1. Выберите API, созданный на предыдущем шаге.
2. Откройте вкладку **Тест**.
3. Выберите какую-либо операцию.

    На странице отобразятся поля для параметров запроса и для заголовков. Один из заголовков, Ocp-Apim-Subscription-Key, содержит ключ подписки для продукта, связанного с этим API. Как создатель экземпляра службы управления API, вы автоматически являетесь администратором, поэтому сведения о ключе будут заполнены автоматически. 
1. Нажмите кнопку **Отправить**.

    Служба серверной части вернет ответ **200 ОК** и некоторые данные.

## <a name="call-operation"> </a>Вызов операции с портала разработчика

Также операции можно вызвать через **портал разработчиков**, чтобы проверить API. 

1. Выберите API, созданный на шаге "Импорт и публикация API серверной части".
2. Нажмите **Портал разработчика**.

    Откроется сайт портала разработчика.
3. Выберите созданный **API**.
4. Выберите операцию, которую необходимо проверить.
5. Щелкните **Попробовать**.
6. Нажмите кнопку **Отправить**.
    
    После вызова операции портал разработчика отображает **состояние ответа**, **заголовки ответа** и **содержимое ответа**.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Преобразование и защита опубликованного API](transform-api.md)