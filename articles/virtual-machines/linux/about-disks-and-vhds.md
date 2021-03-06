---
title: "Основные сведения о неуправляемых (страничные BLOB-объекты) и управляемых дисковых хранилищах для виртуальных машин Microsoft Azure под управлением Linux | Документация Майкрософт"
description: "Основные сведения о неуправляемых (страничные BLOB-объекты) и управляемых дисковых хранилищах для виртуальных машин Linux в Azure."
services: virtual-machines
author: iainfoulds
manager: jeconnoc
ms.service: virtual-machines
ms.workload: storage
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: iainfou
ms.openlocfilehash: 107e332a0f8c9d5a84a74de685ca458fb29caa8b
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2018
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Основные сведения о дисковых хранилищах для виртуальных машин Linux в Azure
Как и любой другой компьютер, виртуальные машины в Azure используют диски как место хранения операционной системы, приложений и данных. Все виртуальные машины Azure имеют по крайней мере два диска — диск операционной системы Linux и временный диск. Диск операционной системы создается из образа, а диск операционной системы и образ в действительности являются виртуальными жесткими дисками (VHD), расположенными в учетной записи хранения Azure. Кроме того, виртуальные машины могут иметь один или несколько дисков данных, которые также хранятся на виртуальных жестких дисках. 

В этой статье рассматриваются различные варианты применения дисков и сведения о том, как создать и использовать различные типы дисков. Также доступна версия этой статьи для [виртуальных машин Windows](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Диски, используемые виртуальными машинами

Давайте рассмотрим, как виртуальные машины используют диски.

## <a name="operating-system-disk"></a>Диск операционной системы
У каждой виртуальной машины есть один подключенный диск операционной системы. Он зарегистрирован как диск SATA и обозначается как /dev/sda по умолчанию. Максимальная емкость этого диска составляет 2048 ГБ. 

## <a name="temporary-disk"></a>Временный диск
У каждой виртуальной машины есть временный диск. Он выступает в качестве временного хранилища для приложений и процессов и предназначен только для хранения данных, таких как страничные файлы или файлы подкачки. Данные на временном диске могут быть потеряны во время [обслуживания](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) или при [повторном развертывании виртуальной машины](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Во время обычной перезагрузки виртуальной машины данные на временном диске должны сохраниться.

На виртуальных машинах Linux в качестве временного диска обычно используется **/dev/sdb**. Агент Linux для Azure форматирует этот диск и монтирует его в каталог **/mnt**. Размер временного диска зависит от размера виртуальной машины. Чтобы узнать больше, ознакомьтесь с [размерами виртуальных машин Linux](../windows/sizes.md).

Дополнительные сведения об использовании временного диска в Azure см. в статье [Общие сведения о временном диске на виртуальных машинах Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).

## <a name="data-disk"></a>Диск данных
Диск данных — виртуальный жесткий диск, подключенный к виртуальной машине для хранения данных приложений или других необходимых данных. Диски данных регистрируются как диски SCSI и обозначаются любой указанной буквой. Максимальная емкость каждого диска составляет 4095 ГБ. Размер виртуальной машины определяет, сколько дисков данных можно подключить и какой тип хранилища можно использовать для размещения дисков.

> [!NOTE]
> Чтобы узнать больше о емкости виртуальных машин, ознакомьтесь с [размерами виртуальных машин Linux](./sizes.md).
> 

Azure создает диск операционной системы при создании виртуальной машины из образа. Если используется образ, включающий диски данных, при создании виртуальной машины Azure также создает диски данных. В противном случае диски данных добавляются после создания виртуальной машины.

Диски данных можно добавить в виртуальную машину в любое время, **подключив** их к ней. Можно использовать виртуальный жесткий диск, отправленный или скопированный в учетную запись хранения или созданный Azure. Подключая диск данных, вы связываете VHD-файл с виртуальной машиной. VHD-файл "берется в аренду", поэтому его невозможно удалить из хранилища, пока он подключен.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Устранение неполадок
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Дополнительная информация
* [Подключите диск](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , чтобы увеличить емкость хранилища для виртуальной машины.
* [Создайте моментальный снимок](snapshot-copy-managed-disk.md).
* [Преобразуйте виртуальную машину для использования управляемых дисков](convert-unmanaged-to-managed-disks.md).

