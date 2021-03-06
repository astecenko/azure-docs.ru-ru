---
title: "Сетевое сопоставление между двумя регионами Azure в Azure Site Recovery | Документация Майкрософт"
description: "Azure Site Recovery координирует репликацию, отработку отказа и восстановление виртуальных машин и физических серверов. Узнайте о функции отработки отказа с выполнением переноса в Azure или в дополнительный центр обработки данных."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: bf3d557c77e3cb6ade6f1bb3773c807f9c8b43f6
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Сетевое сопоставление между двумя регионами Azure


В этой статье описывается, как сопоставлять виртуальные сети Azure из двух регионов Azure друг с другом. Сетевое сопоставление гарантирует, что при создании реплицированной виртуальной машины в целевом регионе Azure, создается в виртуальной сети, сопоставленный в виртуальной сети исходной виртуальной машины.  

## <a name="prerequisites"></a>Технические условия
Перед сопоставлением сетей, убедитесь, что вы создали [виртуальные сети Azure](../virtual-network/virtual-networks-overview.md) в обоих источников и целевых регионов Azure.

## <a name="map-networks"></a>Сопоставление сетей

Чтобы сопоставить виртуальную сеть Azure в одном регионе Azure с виртуальной сетью в другом регионе, выберите " Site Recovery Infrastructure (Инфраструктура Site Recovery) > Сетевое сопоставление" (для виртуальных машин Azure) и создайте сетевое сопоставление.

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


В следующем примере виртуальная машина работает в области Восточная Азия и реплицируется на Юго-Восточная Азия.

Выберите источник и целевая сеть и нажмите кнопку ОК, чтобы создать сетевое сопоставление Восточная Азия на Юго-Восточная Азия.

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Повторите описанный выше процесс, чтобы создать сетевое сопоставление Юго-Восточная Азия на Восточная Азия.

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Сетевое сопоставление при включении репликации

Если сетевое сопоставление не было сделано, если выполняется репликация виртуальной машины в первый раз из одного региона Azure в другую, можно выбрать целевую сеть в рамках одного процесса. Site Recovery создает сетевое сопоставление из исходного региона в целевой и наоборот (в зависимости от вашего выбора).   

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

По умолчанию Site Recovery создает сеть в целевом регионе, которая идентична исходной сети, и добавляет суффикс -asr к имени исходной сети. Щелкните "Настройка", чтобы выбрать созданную ранее сеть.

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Если сетевое сопоставление уже выполнено, изменить целевую виртуальную сеть при включении репликации нельзя. Чтобы ее изменить, измените имеющееся сетевое сопоставление.  

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Сетевое сопоставление](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Если вы изменили сетевое сопоставление из региона 1 в регион 2, убедитесь, что сетевое сопоставление из региона 2 в регион 1 было также изменено.
>
>


## <a name="subnet-selection"></a>Выбор подсети
Подсеть виртуальной машины целевую выбирается на основе имени подсети исходной виртуальной машины. Если подсеть с таким же именем, что и исходная виртуальная машина недоступна в целевой сети, подсети выбирается для целевой виртуальной машины. Если ни одна подсеть с тем же именем в целевой сети, затем по алфавиту первая подсеть, выбирается в качестве целевой подсети. Чтобы изменить подсеть, выберите параметры вычислений и сети виртуальной машины.

![Изменение подсети](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-адрес

IP-адрес для каждого сетевого интерфейса целевой виртуальной машины выбран следующим образом:

### <a name="dhcp"></a>DHCP
Если сетевой интерфейс исходной виртуальной машины использует DHCP, то сетевому интерфейсу целевой виртуальной машины будет также задан DHCP.

### <a name="static-ip"></a>Статический IP-адрес
Если сетевой интерфейс исходной виртуальной машины использует статический IP-адрес, сетевой интерфейс целевой виртуальной машины будет также его использовать. Статический IP-адрес можно выбрать следующим образом:

#### <a name="same-address-space"></a>Одинаковое пространство адресов

Если подсети исходной и целевой подсети имеют то же адресное пространство, IP-адрес сетевого интерфейса исходной виртуальной машины устанавливается как конечный IP-адрес. Если же IP-адрес недоступен, следующий доступный IP-адрес назначается конечный IP-адрес.

#### <a name="different-address-space"></a>Разное пространство адресов

Если подсети исходной и целевой подсети имеют разные адресные пространства, следующий доступный IP-адрес целевой подсети устанавливается как конечный IP-адрес.

Перейдите в вычислительных и сетевых параметров виртуальной машины можно изменить целевой IP-адрес каждого сетевого интерфейса.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о репликации виртуальных машин см. в [этой статье](site-recovery-azure-to-azure-networking-guidance.md).
