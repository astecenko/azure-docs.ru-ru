---
title: "Пример скрипта Azure CLI. Подключение веб-приложения к учетной записи хранения | Документация Майкрософт"
description: "Пример скрипта Azure CLI. Подключение веб-приложения к учетной записи хранения"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 0f60c4cd03a5728dc81cb43e02c8486b15221851
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2017
---
# <a name="connect-a-web-app-to-a-storage-account"></a>Подключение веб-приложения к учетной записи хранения

Этот образец скрипта создает учетную запись хранилища Azure и Azure веб-приложение. Учетная запись хранения связывает веб-приложения с помощью параметров приложения.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, необходимо Azure CLI версии 2.0 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-storage/connect-to-storage.sh "Azure Storage")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды для создания группы ресурсов, веб-приложения, учетной записи хранилища и все связанные ресурсы. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Заметки |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | Создает план службы приложений. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Создает веб-приложение Azure. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) | Создание учетной записи хранения. |
| [`az storage account show-connection-string`](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string) | Получите строку подключения для учетной записи хранения. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) | Создает или обновляет параметр приложения для веб-приложения Azure. Параметры приложения представляются в качестве переменных среды для приложения. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Дополнительные примеры скриптов Azure CLI для службы приложений см. в [документации по службе приложений Azure](../app-service-cli-samples.md).
