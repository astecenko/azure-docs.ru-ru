---
title: "Как использовать соединитель SharePoint Online в приложениях логики | Документация Майкрософт"
description: "Создание приложений логики с соединителем SharePoint Online для управления списками на сайте SharePoint."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: bdda380360cd2a7595fa05dfe3fc6504e76255b5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-sharepoint-online-connector"></a>Приступая к работе с соединителем SharePoint Online
Соединитель SharePoint Online можно использовать для управления списками SharePoint.  

Чтобы использовать [соединитель](apis-list.md), сначала нужно создать приложение логики. Вы можете начать с [создания приложения логики](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-sharepoint-online"></a>Подключение к SharePoint Online
Чтобы обеспечить доступ приложения логики к какой-либо службе, сначала необходимо создать *подключение* к этой службе. Таким образом вы установите [соединение](connectors-overview.md) между приложением логики и другой службой.  

### <a name="create-a-connection-to-sharepoint-online"></a>Создание подключения к SharePoint Online
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a>Использование триггера SharePoint Online
Триггер — это событие, которое можно использовать для запуска рабочего процесса, определенного в приложении логики. [Дополнительные сведения о триггерах](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a>Использование действия SharePoint Online
Действие — это операция, выполняемая рабочим процессом, определенным в приложении логики. [Дополнительные сведения о действиях](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a>Сведения о соединителях

Информацию о существующих ограничениях, а также о триггерах и действиях, определенных в Swagger, см. в статье со [сведениями о соединителях](/connectors/sharepoint/).

## <a name="next-steps"></a>Дальнейшие действия
[Создайте приложение логики](../logic-apps/quickstart-create-first-logic-app-workflow.md)

