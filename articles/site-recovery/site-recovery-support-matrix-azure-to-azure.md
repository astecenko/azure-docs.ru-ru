---
title: "Таблица поддержки Azure Site Recovery для репликации из одного региона Azure в другой | Документация Майкрософт"
description: "В этой статье перечислены операционные системы и конфигурации, поддерживаемые Azure Site Recovery для репликации виртуальных машин Azure из одного региона в другой в целях аварийного восстановления."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/08/2017
ms.author: sujayt
ms.openlocfilehash: c15583b9420355bb7c35bd107b899c59e80e3741
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/13/2018
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-to-azure"></a>Таблица поддержки Azure Site Recovery для репликации из одного региона Azure в другой


>[!NOTE]
>
> Репликация Azure Site Recovery для виртуальных машин Azure сейчас доступна в предварительной версии.

В этой статье перечислены операционные системы и компоненты, поддерживаемые Azure Site Recovery при репликации и восстановлении виртуальных машин Azure из одного региона в другой.

## <a name="user-interface-options"></a>Параметры пользовательского интерфейса

**Пользовательский интерфейс** |  **Поддерживается или не поддерживается**
--- | ---
**Портал Azure** | Поддерживаются
**Классический портал.** | Не поддерживается
**PowerShell** | Сейчас не поддерживается
**REST API** | Сейчас не поддерживается
**ИНТЕРФЕЙС КОМАНДНОЙ СТРОКИ** | Сейчас не поддерживается


## <a name="resource-move-support"></a>Поддержка перемещения ресурсов

**Тип перемещения ресурсов** | **Поддерживается или не поддерживается** | **Примечания**  
--- | --- | ---
**Перемещение хранилища между группами ресурсов** | Не поддерживается |Перемещение хранилища служб восстановления между группами ресурсов невозможно.
**Перемещение вычислительных ресурсов, ресурсов хранилищ и сетевых ресурсов между группами ресурсов** | Не поддерживается |При перемещении виртуальной машины (или связанных с ней компонентов, например, хранилища или сети) после включения репликации, необходимо отключить репликацию, а затем включить ее еще раз.



## <a name="support-for-deployment-models"></a>Поддержка моделей развертывания

**Модель развертывания** | **Поддерживается или не поддерживается** | **Примечания**  
--- | --- | ---
**Классический** | Поддерживаются | Классическую виртуальную машину можно реплицировать или восстанавливать только как классическую виртуальную машину, ее нельзя восстановить в качестве виртуальной машины диспетчера ресурсов. Развертывание классической виртуальной машины без виртуальной сети напрямую в регион Azure не поддерживается.
**Resource Manager** | Поддерживаются |

>[!NOTE]
>
> 1. Репликация виртуальных машин Azure между подписками в рамках сценариев аварийного восстановления не поддерживается.
> 2. Перенос виртуальных машин Azure между подписками не поддерживается.
> 3. Перенос виртуальных машин Azure в пределах одного региона не поддерживается.
> 4. Перенос виртуальных машин Azure из классической модели развертывания в модель Resource Manager не поддерживается.

## <a name="support-for-replicated-machine-os-versions"></a>Поддержка версий ОС для реплицируемых виртуальных машин

Поддержка ниже относится к любой рабочей нагрузке, выполняемой в указанной ОС.

#### <a name="windows"></a>Windows

- Windows Server 2016 (основные серверные компоненты и сервер с возможностями рабочего стола)*;
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 как минимум с пакетом обновления 1 (SP1)

>[!NOTE]
>
> \* Nano Server Windows Server 2016 не поддерживается.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3, 7.4.
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2, 7.3, 7.4.
- Сервер Ubuntu версии 14.04 LTS [(поддерживаемые версии ядра)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Сервер Ubuntu версии 16.04 LTS [(поддерживаемые версии ядра)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7
- Debian 8;
- Oracle Enterprise Linux 6.4, 6.5 с ядром, совместимым с Red Hat, или с ядром Unbreakable Enterprise Kernel Release 3 (UEK3).
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4

(Обновление реплицируемых компьютеров с SLES 11 SP3 до SLES 11 SP4 не поддерживается. Если реплицируемый компьютер обновлен с SLES 11 SP3 до SLES 11 SP4, вам потребуется отключить репликацию и включить повторную защиту компьютера после обновления.)

>[!NOTE]
>
> На серверах Ubuntu с поддержкой входа и аутентификации на основе пароля, а также пакета cloud-init для настройки облачных виртуальных машин, вход на основе пароля может быть отключен после отработки отказа (в зависимости от конфигурации cloud-init). Вход на основе пароля можно повторно включить на виртуальной машине. Для этого на портале Azure сбросьте пароль в меню параметров виртуальной машины (в разделе "Поддержка и устранение неполадок"), на которой выполнялась отработка отказа.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Поддерживаемые версии ядра Ubuntu для виртуальных машин Azure

**Выпуск** | **Версия службы Mobility Service** | **Версия ядра** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-generic–3.13.0-121-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>4.4.0-21-generic–4.4.0-81-generic |
14.04 LTS | 9.11 | с 3.13.0-24-generic по 3.13.0-125-generic<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>с 4.4.0-21-generic по 4.4.0-83-generic |
14.04 LTS | 9.12 | с 3.13.0-24-generic по 3.13.0-132-generic<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>с 4.4.0-21-generic по 4.4.0-96-generic |
14.04 LTS | 9.13 | С 3.13.0-24-generic по 3.13.0-137-generic,<br/>3.16.0-25-generic to 3.16.0-77-generic,<br/>3.19.0-18-generic to 3.19.0-80-generic,<br/>4.2.0-18-generic to 4.2.0-42-generic,<br/>с 4.4.0-21-generic по 4.4.0-104-generic. |
16.04 LTS | 9.10 | 4.4.0-21-generic–4.4.0-81-generic,<br/>4.8.0-34-generic–4.8.0-56-generic,<br/>4.10.0-14-generic–4.10.0-24-generic |
16.04 LTS | 9.11 | с 4.4.0-21-generic по 4.4.0-83-generic<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-27-generic |
16.04 LTS | 9.12 | с 4.4.0-21-generic по 4.4.0-96-generic<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-35-generic |
16.04 LTS | 9.13 | С 4.4.0-21-generic по 4.4.0-104-generic,<br/>с 4.8.0-34-generic по 4.8.0-58-generic<br/>с 4.10.0-14-generic по 4.10.0-42-generic. |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Поддерживаемые файловые системы и конфигурации гостевого хранилища на виртуальных машинах Azure под управлением ОС Linux

* Файловые системы — ext3, ext4, ReiserFS (только Suse Linux Enterprise Server), XFS.
* Диспетчер томов: LVM2.
* Программное обеспечение Multipath — Device Mapper

## <a name="region-support"></a>Поддержка регионов

Виртуальные машины можно реплицировать и восстанавливать между любыми двумя регионами в том же географическом кластере.

**Географический кластер** | **Регионы Azure**
-- | --
Америка | Восточная Канада, центральная Канада, юго-центральный регион США, западно-центральная часть США, восточная часть США, восточная часть США 2, западная часть США, западная часть США 2, центральная часть США, северо-центральная часть США
Европа | Западная часть Соединенного Королевства, южная часть Соединенного Королевства, Северная Европа, Западная Европа
Азия | Южная Индия, центральная Индия, Юго-Восточная Азия, Восточная Азия, Восточная Япония, Западная Япония, Центральная Корея, Южная Корея
Австралия   | Восточная Австралия, Юго-Восточная Австралия
Azure Government    | Виргиния (для обслуживания государственных организаций США), Айова (для обслуживания государственных организаций США), Аризона (для обслуживания государственных организаций США), Техас (для обслуживания государственных организаций США), восточная часть США (US DOD), центральная часть США, МО (US DOD)
Германия | Центральная Германия и северо-восточная Германия
Китай | Восточная часть Китая, Северная часть Китая

>[!NOTE]
>
> Для региона "Южная Бразилия" выполнять репликацию, отработку отказа и восстановление размещения можно только в следующие регионы: юго-центральная часть США, западно-центральная часть США, восточная часть США, восточная часть США 2, западная часть США, западная часть США 2 и северо-центральный регион США.


## <a name="support-for-compute-configuration"></a>Поддержка конфигурации вычислений

**Конфигурация** | **Поддержка** | **Примечания**
--- | --- | ---
Размер | Виртуальная машина Azure любого размера с 2 ядрами ЦП и 1 ГБ ОЗУ | Дополнительные сведения см.в статье [Размеры виртуальных машин Azure](../virtual-machines/windows/sizes.md)
Группы доступности | Поддерживаются | При использовании параметров по умолчанию на шаге включения репликации на портале, группа доступности автоматически создается на основе конфигурации исходного региона. Группу доступности можно в любое время изменить на панели "Replicated item (Реплицированный элемент) > Параметры > Вычисления и сеть > Группа доступности".
Виртуальные машины с лицензией в программе преимуществ гибридного использования | Поддерживаются | Если у исходной виртуальной машины включена лицензия программы преимуществ гибридного использования, то тестовая отработка отказа или отработка отказа виртуальной машины также использует лицензию программы преимуществ гибридного использования.
наборы для масштабирования виртуальных машин | Не поддерживается |
Образы из коллекции Azure, опубликованные корпорацией Майкрософт | Поддерживаются | Поддерживаются до тех пор, пока виртуальная машина работает в операционной системе, поддерживаемой Site Recovery.
Образы из коллекции Azure, опубликованные сторонним производителем | Поддерживаются | Поддерживается, пока виртуальная машина работает в операционной системе, поддерживаемой Site Recovery.
Пользовательские образы, опубликованные сторонним производителем | Поддерживаются | Поддерживается, пока виртуальная машина работает в операционной системе, поддерживаемой Site Recovery.
Виртуальные машины, перенесенные с помощью Site Recovery | Поддерживаются | Если эти компьютеры VMware или физические компьютеры были перенесены с помощью Azure Site Recovery, необходимо удалить более раннюю версию службы Mobility Service и перезагрузить компьютер перед репликацией его в другой регион Azure.

## <a name="support-for-storage-configuration"></a>Поддержка конфигурации хранилища

**Конфигурация** | **Поддержка** | **Примечания**
--- | --- | ---
Максимальный размер диска ОС | 2048 ГБ | Дополнительные сведения см.в разделе [Диски, используемые виртуальными машинами](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms).
Максимальный размер данных на диске | 4095 ГБ | Дополнительные сведения см.в разделе [Диски, используемые виртуальными машинами](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms).
Количество дисков данных | До 64 (поддерживается определенными размерами виртуальных машин Azure) | Дополнительные сведения см.в статье [Размеры виртуальных машин Azure](../virtual-machines/windows/sizes.md)
Временный диск | Всегда исключается из репликации | Временный диск всегда исключается из репликации. Согласно руководству по Azure, постоянные данные не следует помещать на временный диск. Дополнительные сведения см. в разделе [Временный диск](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk).
Частота изменения данных на диске | Максимум 6 Мбит/с на диск | Если средняя частота изменения данных на диске постоянно превышает 6 Мбит/с, репликация может занять некоторое время. Если же это эпизодический случай, и частота изменения данных только на некоторое время превышает 6 Мбит/с, репликация завершится вовремя. В этом случае вы можете увидеть немного отложенные точки восстановления.
Диски для стандартных учетных записей хранения | Поддерживаются |
Диск для учетных записей хранения уровня Premium | Поддерживаются | Если диски виртуальной машины распределены между стандартными учетными записями хранения и учетными записями хранения Premium, для каждого диска можно выбрать различные целевые учетные записи хранения, чтобы обеспечить такую же конфигурацию хранилища в целевом регионе.
Управляемые диски уровня "Стандартный" | Не поддерживается |  
Управляемые диски уровня "Премиум" | Не поддерживается |
Дисковые пространства | Поддерживаются |         
Шифрование неактивных данных (SSE) | Поддерживаются | Для кэша и целевых учетных записей хранения можно выбрать учетную запись хранения с включенным шифрованием неактивных данных.     
Шифрование дисков Azure | Не поддерживается |
"Горячее" добавление или удаление диска | Не поддерживается | При добавлении или удалении диска данных его с виртуальной машины, вам необходимо сначала отключить репликацию, а затем включить ее снова.
Исключение диска | Не поддерживается|   Временный диск исключается по умолчанию.
LRS | Поддерживаются |
GRS | Поддерживаются |
RA-GRS | Поддерживаются |
ZRS | Не поддерживается |  
"Холодное" и "горячее" хранилище | Не поддерживается | Диски виртуальных машин не поддерживаются "холодными" и "горячими" хранилищами
Конечные точки службы виртуальной сети (брандмауэры службы хранилища Azure и виртуальные сети)  | Нет  | Разрешение доступа к определенным виртуальным сетям Azure для учетных записей хранения кэша, используемых для хранения реплицируемых данных, не поддерживается. 
Учетные записи хранения общего назначения версии 2 ("горячего" и "холодного" уровней) | Нет  | Значительное увеличение затрат на транзакции по сравнению с учетными записями хранения общего назначения версии 1.

>[!IMPORTANT]
> Обязательно определите целевые показатели масштабируемости и производительности дисков виртуальных машин [Linux](../virtual-machines/linux/disk-scalability-targets.md) или [Windows](../virtual-machines/windows/disk-scalability-targets.md), чтобы избежать проблем с производительностью. Если использовать параметры по умолчанию, Site Recovery будет создавать необходимые диски и учетные записи хранения на основе исходной конфигурации. Если вы настраиваете и выбираете собственные параметры, учитывайте целевые показатели масштабирования и производительности дисков исходных виртуальных машин.

## <a name="support-for-network-configuration"></a>Поддержка конфигурации сети
**Конфигурация** | **Поддержка** | **Примечания**
--- | --- | ---
Сетевой интерфейс | Не более максимального числа сетевых интерфейсов, поддерживаемых виртуальной машиной Azure определенного размера | Сетевые интерфейсы создаются при создании виртуальной машины как часть операции отработки отказа или тестовой отработки отказа. Количество сетевых интерфейсов на восстановленной виртуальной машине зависит от количества сетевых интерфейсов на исходной виртуальной машине во время включения репликации. Добавление или удаление сетевого интерфейса после включения репликации не влияет на число сетевых интерфейсов на восстановленной виртуальной машине.
Балансировщик нагрузки для Интернета | Поддерживаются | Необходимо связать предварительно настроенный балансировщик нагрузки с помощью сценария автоматизации Azure в плане восстановления.
Внутренний балансировщик нагрузки | Поддерживаются | Необходимо связать предварительно настроенный балансировщик нагрузки с помощью сценария автоматизации Azure в плане восстановления.
Общедоступный IP-адрес| Поддерживаются | Существующий общедоступный IP-адрес необходимо связать с сетевым интерфейсом или создать новый и связать его с сетевым интерфейсом используя сценарий автоматизации Azure в плане восстановления.
Группы безопасности сети в сетевом интерфейсе (Resource Manager)| Поддерживаются | Необходимо связать группы безопасности сети с сетевым интерфейсом, используя сценарий автоматизации Azure из плана восстановления.  
Группы безопасности сети для подсети (классическая модель и Resource Manager)| Поддерживаются | Необходимо связать группы безопасности сети с сетевым интерфейсом, используя сценарий автоматизации Azure из плана восстановления.
Группы безопасности сети для виртуальной машины (классическая модель)| Поддерживаются | Необходимо связать группы безопасности сети с сетевым интерфейсом, используя сценарий автоматизации Azure из плана восстановления.
Зарезервированный IP-адрес (Статический IP-адрес) или сохранение исходного IP-адреса | Поддерживаются | Если сетевой интерфейс исходной виртуальной машины имеет статическую IP-конфигурацию, а в целевой подсети доступен такой же IP-адрес, он будет назначен восстановленной виртуальной машине. Если в целевой подсети нет такого же IP-адреса, один из доступных IP-адресов в подсети будет зарезервирован для этой виртуальной машины. Вы можете указать фиксированный IP-адрес по своему выбору на панели "Replicated item (Реплицированный элемент) > Параметры > Compute and Network (Вычисления и сеть) > Сетевые интерфейсы". Выбранному сетевому интерфейсу можно указать подсеть и IP-адрес по своему выбору.
Динамический IP-адрес| Поддерживаются | Если сетевой интерфейс исходной виртуальной машины имеет динамическую IP-конфигурацию, сетевой интерфейс на восстановленной виртуальной машине также по умолчанию будет динамическим. Вы можете указать фиксированный IP-адрес по своему выбору на панели "Replicated item (Реплицированный элемент) > Параметры > Compute and Network (Вычисления и сеть) > Сетевые интерфейсы". Выбранному сетевому интерфейсу можно указать подсеть и IP-адрес по своему выбору.
Интеграция диспетчера трафика | Поддерживаются | Диспетчер трафика можно предварительно настроить так, чтобы трафик перенаправлялся в конечную точку в исходном регионе на регулярной основе и в конечную точку в целевом регионе в случае отработки отказа.
Служба доменных имен, управляемая Azure | Поддерживаются |
Пользовательский DNS  | Поддерживаются |    
Прокси-сервер, не прошедший проверку подлинности | Поддерживаются | Дополнительные сведения см.в статье [Руководство по организации сети для репликации виртуальных машин Azure](site-recovery-azure-to-azure-networking-guidance.md).    
Прокси-сервер, прошедший проверку подлинности | Не поддерживается | Если виртуальная машина использует прокси-сервер, прошедший проверку подлинности для исходящих подключений, ее нельзя реплицировать с помощью Azure Site Recovery.    
VPN типа "сеть — сеть" с локальной средой (с или без ExpressRoute)| Поддерживаются | Убедитесь, что определяемые пользователем маршруты и сетевые группы безопасности настраиваются так, чтоб трафик Site Recovery не перенаправлялся в локальную среду. Дополнительные сведения см.в статье [Руководство по организации сети для репликации виртуальных машин Azure](site-recovery-azure-to-azure-networking-guidance.md).  
Подключение типа "виртуальная сеть — виртуальная сеть" | Поддерживаются | Дополнительные сведения см.в статье [Руководство по организации сети для репликации виртуальных машин Azure](site-recovery-azure-to-azure-networking-guidance.md).  


## <a name="next-steps"></a>Дополнительная информация
- Дополнительные сведения о репликации виртуальных машин см. в [этой статье](site-recovery-azure-to-azure-networking-guidance.md).
- Включите защиту рабочих нагрузок, [выполнив репликацию виртуальных машин Azure](site-recovery-azure-to-azure.md).
