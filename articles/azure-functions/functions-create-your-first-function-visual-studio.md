---
title: "Создание первой функции в Azure с помощью Visual Studio | Документация Майкрософт"
description: "Создание и публикация в Azure простой функции, активируемой HTTP, с помощью инструментов функций Azure для Visual Studio."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "функции azure, функции, обработка событий, вычисления, независимая архитектура"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/11/2017
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 401230c6d7ef522a6a607fd03f798483f942a226
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="create-your-first-function-using-visual-studio"></a>Создание первой функции с помощью Visual Studio

Решение "Функции Azure" позволяет выполнять код в [бессерверной](https://azure.microsoft.com/overview/serverless-computing/) среде без необходимости создавать виртуальную машину или публиковать веб-приложение.

Изучив эту статью, вы научитесь использовать инструменты Visual Studio 2017 для Функций Azure и локально тестировать функцию hello world. Затем вы опубликуете код функции в Azure. Эти средства доступны как часть рабочей нагрузки Azure для разработки в Visual Studio 2017 версии 15.3 или более поздней.

![Код функций Azure в проекте Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

При желании вместо этого можно [просмотреть видео](#watch-the-video).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством установите следующие компоненты.

* [Visual Studio 2017 15.4](https://www.visualstudio.com/vs/) или более поздней версии, в том числе рабочая нагрузка **разработки в Azure**.

    ![Установка Visual Studio 2017 с рабочей нагрузкой разработки в Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Создание проекта функций Azure в Visual Studio

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

После создания проекта можно приступать к созданию первой функции.

## <a name="create-the-function"></a>Создание функции

1. Щелкните правой кнопкой мыши узел проекта в **обозревателе решений** и выберите **Добавить** > **Новый элемент**. Выберите **Функция Azure**, введите `HttpTriggerCSharp.cs` в поле **Имя** и нажмите кнопку **Добавить**.

2. Выберите **HttpTrigger**, выберите для параметра **Права доступа** значение **Анонимно** и нажмите кнопку **ОК**. Созданная функция доступна для HTTP-запроса из любого клиента. 

    ![Создание функции Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    Файл кода добавляется в проект, который содержит класс, реализующий код функции. Этот код основан на шаблоне, который получает значение имени и выводит сведения о нем. Атрибут **FunctionName** задает имя функции. Атрибут **HttpTrigger** указывает сообщение, которое активирует функцию. 

    ![Файл кода функции](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Созданную функцию, активируемую HTTP, можно протестировать на локальном компьютере.

## <a name="test-the-function-locally"></a>Локальное тестирование функции

Основные инструменты службы Функции Azure позволяют запускать проекты функций Azure на локальном компьютере разработчика. Вам будет предложено установить эти инструменты при первом запуске функции из Visual Studio.  

1. Чтобы проверить работу функции, нажмите клавишу F5. Если будет предложено, примите запрос от Visual Studio на скачивание и установку основных инструментов службы Функции Azure (CLI).  Кроме того, вам может понадобиться включить исключение брандмауэра, чтобы инструменты могли обрабатывать HTTP-запросы.

2. Скопируйте URL-адрес функции из выходных данных среды выполнения функций Azure.  

    ![Локальная среда выполнения Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Вставьте URL-адрес запроса в адресную строку браузера. Добавьте строку запроса `?name=<yourname>` в этот URL-адрес и выполните запрос. Ниже показан ответ в браузере на локальный запрос GET, возвращаемый функцией: 

    ![Ответ функции localhost в браузере](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Чтобы остановить отладку, нажмите кнопку **Остановить** на панели инструментов Visual Studio.

Убедитесь, что функция правильно работает на локальном компьютере. Затем опубликуйте проект в Azure.

## <a name="publish-the-project-to-azure"></a>Публикация проекта в Azure

Перед публикацией проекта убедитесь, что в вашей подписке Azure есть приложения-функция. Можно создать приложение-функцию непосредственно в Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Тестирование функции в Azure

1. Скопируйте базовый URL-адрес приложения-функции на странице профиля публикации. Замените часть `localhost:port` URL-адреса, который использовался при локальной проверке функции новым базовым URL-адресом. Как и ранее, добавьте строку запроса `?name=<yourname>` в этот URL-адрес и выполните запрос.

    URL-адрес, который вызывает функцию, активируемую HTTP, выглядит так:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Вставьте этот URL-адрес HTTP-запроса в адресную строку браузера. Ниже показан ответ в браузере на удаленный запрос GET, возвращаемый функцией: 

    ![Ответ функции в браузере](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)

## <a name="watch-the-video"></a>Просмотреть видео

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Дальнейшие действия

С помощью Visual Studio вы создали приложение-функцию C# с простой функцией, активируемой HTTP. 

+ Дополнительные сведения о настройке проекта для поддержки других типов триггеров и привязок см. в разделе [Настройка проекта для локальной разработки](functions-develop-vs.md#configure-the-project-for-local-development) в статье [Инструменты Функций Azure для Visual Studio](functions-develop-vs.md).
+ Дополнительные сведения о локальном тестировании и отладке с помощью основных инструментов функций Azure см. в статье [Как программировать и тестировать функции Azure в локальной среде](functions-run-local.md). 
+ Дополнительные сведения о разработке функций, таких как библиотек классов .NET, см. в статье [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Использование библиотек классов .NET с помощью Функций Azure). 

