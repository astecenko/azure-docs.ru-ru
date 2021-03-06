---
title: "Использование Azure DNS для частных доменов | Документация Майкрософт"
description: "Обзор службы размещения частных DNS в Microsoft Azure."
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: 
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: 95cf8ab2bd34e698e12452e062687219bad49eb6
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="using-azure-dns-for-private-domains"></a>Использование Azure DNS для частных доменов
Служба доменных имен, или DNS, отвечает за преобразование (или разрешение) имени службы в IP-адрес. Azure DNS является службой размещения для доменов DNS, выполняющей разрешение имен на базе инфраструктуры Microsoft Azure.  Помимо DNS для доменов, доступных в Интернете, теперь Azure DNS в режиме предварительного просмотра поддерживает DNS для частных доменов.  
 
Azure DNS предоставляет надежную и защищенную службу DNS для разрешения доменных имен в виртуальных сетях, позволяя обойтись без поддержки собственной системы DNS. Частные зоны DNS позволяют использовать пользовательские доменные имена, а не только предоставленные Azure имена, как ранее.  Пользовательские доменные имена помогут адаптировать архитектуру виртуальной сети к потребностям вашей организации. Поддерживается разрешение имен для виртуальных машин в пределах виртуальной сети и между виртуальными сетями. Кроме того, вы можете настроить имена зон по схеме "расщепление горизонта", чтобы присваивать частной и общедоступной DNS-зонам одно и то же имя.

![Общие сведения о DNS](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="benefits"></a>Преимущества

* **Избавляет от необходимости создавать собственное решение DNS.** Ранее многие клиенты создавали пользовательские решения DNS для управления зонами DNS в виртуальной сети.  Теперь управление зонами DNS можно выполнять на базе инфраструктуры Azure и больше не нужно создавать пользовательские решения DNS и управлять ими.

* **Поддерживает все стандартные типы записей DNS.**  Azure DNS поддерживает записи DNS типов A, AAAA, CNAME, MX, NS, PTR, SOA, SRV и TXT.

* **Автоматическое управление записями для имени узла.** Помимо размещения пользовательских записей DNS, Azure автоматически хранит записи для имен узлов всех виртуальных машин в указанных виртуальных сетях.  Это позволяет оптимизировать использование доменных имен, не создавая пользовательские решения DNS и не изменяя код приложений.

* **Разрешение имен узлов между виртуальными сетями.** В отличие от имен узлов, которые предоставляются Azure, частные зоны DNS можно совместно использовать в нескольких виртуальных сетях.  Эта возможность упрощает работу в многосетевой топологии и процессы обнаружения служб, например при использовании пиринга между виртуальными сетями.

* **Привычные средства и взаимодействие с пользователем.** Чтобы вы быстрее ознакомились с новым предложением, для него реализованы широко известные инструменты Azure DNS (PowerShell, шаблоны Resource Manager и REST API).

* **Поддержка "расщепления горизонта" DNS.** Azure DNS позволяет создавать зоны так, чтобы запросы одного и того же имени возвращали разные ответы внутри виртуальной сети и из Интернета.  Типичный сценарий для "расщепления горизонта" DNS — отдельная версия службы для использования внутри виртуальной сети.


## <a name="pricing"></a>Цены

В период использования предварительной версии частные зоны DNS будут обслуживаться бесплатно. После выпуска общедоступной версии к этой функции будет применена модель ценообразования, сходная с ценообразованием для существующего предложения Azure DNS. 


## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как создать [частную зону DNS](./private-dns-getstarted-powershell.md) в Azure DNS.

Дополнительные сведения о записях и зонах DNS см. в [обзоре зон и записей DNS](dns-zones-records.md).

Дополнительные сведения о некоторых других ключевых [сетевых возможностях](../networking/networking-overview.md) Azure.

