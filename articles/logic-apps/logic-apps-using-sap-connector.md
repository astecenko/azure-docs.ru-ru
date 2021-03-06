---
title: "Подключение к локальной системе SAP в Azure Logic Apps | Документация Майкрософт"
description: "Использование локального шлюза данных для подключения к локальной системе SAP в рабочем процессе приложения логики"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Подключение к локальной системе SAP из приложения логики с помощью соединителя SAP 

Локальный шлюз данных позволяет управлять данными и безопасно обращаться к ресурсам, расположенным в локальной среде. В этом разделе показано, как приложения логики могут подключаться к локальной системе SAP. В этом примере приложение логики запрашивает IDOC по протоколу HTTP и отправляет ответ обратно.    

> [!NOTE]
> Текущие ограничения: 
> - Время ожидания приложения логики истекает, если все действия, необходимые для ответа, не завершаются в течение [времени ожидания запроса](./logic-apps-limits-and-config.md). В этом сценарии запросы могут быть заблокированы. 
> - Средство выбора файлов не отображает все доступные поля. В этом случае пути можно добавить вручную.

## <a name="prerequisites"></a>Предварительные требования

- Установите и настройте последнюю версию [локального шлюза данных](https://www.microsoft.com/download/details.aspx?id=53127) или версию не ниже 1.15.6150.1. Инструкция см. в статье [Подключение к локальному шлюзу данных для приложений логики](http://aka.ms/logicapps-gateway). Шлюз нужно установить на локальном компьютере, прежде чем продолжать процесс.

- Скачайте последнюю версию клиентской библиотеки SAP и установите ее на компьютере, на котором установлен шлюз данных. Можно использовать любую из следующих версий SAP: 
    - SAP Server
        - Любой сервер SAP, поддерживающий соединитель .NET (NCo) 3.0
 
    - SAP Client
        - Соединитель SAP .NET (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Добавление триггеров и действий для подключения к системе SAP

В соединителе SAP доступны действия, но не триггеры. Таким образом, в начале рабочего процесса необходимо использовать другой триггер. 

1. Добавьте триггер запроса и ответа, а затем выберите **Новый шаг**.

2. Выберите **Добавить действие**, а затем выберите нужный соединитель SAP. Для этого введите `SAP` в поле поиска:    

     ![Выберите сервер приложений SAP или сервер сообщений SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. Выберите [**сервер приложений SAP**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) или [**сервер сообщений SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm) в зависимости от конфигурации SAP. Если у вас нет существующего подключения, то вам будет предложено создать его.

   1. Выберите **подключение через локальный шлюз данных** и введите сведения о системе SAP:   

       ![Добавление строки подключения к SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. В разделе **Шлюз** выберите существующий шлюз или, чтобы установить новый шлюз, выберите **Установить шлюз**.

        ![Установка нового шлюза](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Введите все нужные сведения и нажмите **Создать**. 
   Logic Apps настраивает и проверяет подключение, гарантируя, что оно работает правильно.

4. Введите имя для подключения SAP.

5. Теперь вам будут доступны параметры SAP. Чтобы найти категорию IDOC, выберите ее в списке. Также можно вручную ввести путь и выбрать ответ HTTP в поле **body** (текст):

     ![Действие SAP](media/logic-apps-using-sap-connector/picture3.png)

6. Добавьте действие по созданию **HTTP-ответа**. Сообщение ответа должно передавать выходные данные SAP.

7. Сохраните приложение логики. Проверьте его, отправив IDOC с помощью URL-адреса триггера HTTP. После отправки IDOC дождитесь ответа от приложения логики:   

     > [!TIP]
     > Узнайте о возможностях [мониторинга приложений логики](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Теперь соединитель SAP добавлен в приложение логики, и вы можете ознакомиться с другими функциями.

- BAPI
- RFC

## <a name="get-help"></a>Получение справки

Чтобы задать вопросы, помочь другим пользователям и узнать, что делают другие пользователи, посетите [форум по Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Чтобы улучшить Azure Logic Apps и соединители, голосуйте за идеи или предлагайте собственные на [сайте обратной связи Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Дальнейшие действия

- Проверка, преобразование и другие функции в стиле BizTalk для [пакета интеграции Enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Подключение к локальным данным](../logic-apps/logic-apps-gateway-connection.md) из приложений логики
