---
title: "Группы доступности для классических виртуальных машин Linux | Документация Майкрософт"
description: "Настройка групп доступности для новой или существующей виртуальной машины Linux в классической модели развертывания с помощью портала Azure и Azure PowerShell."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: b8624315-beca-4ec7-8441-2e98b166b548
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: cynthn
ms.openlocfilehash: b6243614bf7cbd97a4ce8ceff5a3ba0d33fc42f4
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-configure-an-availability-set-for-linux-virtual-machines-in-the-classic-deployment-model"></a>Как настроить группу доступности для виртуальных машин Linux в классической модели развертывания
> [!IMPORTANT] 
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель Resource Manager и классическая модель](../../../resource-manager-deployment-model.md). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. [Группы доступности](../../azure-cli-arm-commands.md#azure-availset-commands-to-manage-your-availability-sets) можно также настраивать в развертываниях Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[!INCLUDE [virtual-machines-common-classic-configure-availability](../../../../includes/virtual-machines-common-classic-configure-availability.md)]