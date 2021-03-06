---
title: "Поддержка брандмауэра Azure Cosmos DB и контроль доступа по протоколу IP | Документация Майкрософт"
description: "Сведения о настройке поддержки брандмауэра в учетных записях базы данных Azure Cosmos DB с помощью политик контроля доступа на основе IP-адресов."
keywords: "Контроль доступа на основе IP-адресов, поддержка брандмауэра"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: ankshah
ms.openlocfilehash: 85f5e0e076f92afc79ed8f4ce652bb0f31923113
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-firewall-support"></a>Поддержка брандмауэра для Azure Cosmos DB
Для защиты данных, хранящихся в учетных записях базы данных Azure Cosmos DB, в Cosmos DB реализована поддержка [модели авторизации](https://msdn.microsoft.com/library/azure/dn783368.aspx) на основе секретов. Проверка целостности данных в этой модели осуществляется с помощью надежного кода проверки подлинности сообщений, использующего хэш-функции (HMAC). Теперь, помимо модели авторизации на основе секрета, для поддержки входящего трафика брандмауэра база данных Azure Cosmos DB использует политики контроля доступа на основе IP-адресов. Эта модель похожа на использование правил брандмауэра в традиционной системе базы данных. Она предоставляет дополнительный уровень защиты для учетных записей базы данных Azure Cosmos DB. Кроме того, эта модель позволяет настроить доступ к учетной записи базы данных Azure Cosmos DB только из утвержденных компьютеров и (или) облачных служб. Для доступа к ресурсам Azure Cosmos DB из этих утвержденных компьютеров и служб пользователь (вызывающая сторона) по-прежнему должен предоставить допустимый маркер проверки подлинности.

## <a name="ip-access-control-overview"></a>Общие сведения о контроле доступа на основе IP-адресов
По умолчанию доступ к учетной записи базы данных Azure Cosmos DB можно получить через Интернет при условии, что запрос сопровождается допустимым маркером проверки подлинности. Чтобы настроить политики контроля доступа на основе IP-адресов, пользователь должен предоставить набор IP-адресов или их диапазонов в нотации CIDR, которые добавляются в список разрешенных клиентских IP-адресов для указанной учетной записи базы данных. После применения этой конфигурации все запросы, получаемые с машинами за пределами этого списка разрешенных блокируются на сервере.  На следующей схеме показан поток обработки подключения для управления доступом на основе IP-адресов.

![Схема подключения для контроля доступа на основе IP-адресов](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Настройка политики контроля доступа на основе IP-адресов
Политика управления доступом IP можно задать на портале Azure, или программным путем с использованием [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), или [API-интерфейса REST](/rest/api/documentdb/) путем обновления **ipRangeFilter** свойство. 

Чтобы задать политику управления доступом IP на портале Azure, перейдите на страницу учетной записи Azure Cosmos DB, щелкните **брандмауэра** перейдите в меню переходов **включить управление доступом IP** значение **ON**. 

![Снимок экрана, показывающий, как открыть страницу брандмауэра на портале Azure](./media/firewall-support/azure-portal-firewall.png)

После управления доступом IP, портал предоставляет параметры для включения доступа к порталу Azure, других служб Azure и текущий IP-адрес. В следующих разделах предоставляются дополнительные сведения об этих параметрах.

![Снимок экрана, показывающий, как настроить параметры брандмауэра на портале Azure](./media/firewall-support/azure-portal-firewall-configure.png)

> [!NOTE]
> После включения политики контроля доступа на основе IP-адресов для учетной записи базы данных Azure Cosmos DB все подключения к этой учетной записи с компьютеров, диапазоны IP-адресов которых не входят в список разрешенных, блокируются. При использовании этой модели просмотр операций с плоскостью данных на портале также блокируется, чтобы обеспечить целостность контроля доступа.

## <a name="connections-from-the-azure-portal"></a>Подключения на портале Azure 

При включении политики управления доступом IP программными средствами, необходимо добавить IP-адрес для портала Azure для **ipRangeFilter** свойство для сохранения доступа. IP-адрес портала:

|Регион|IP-адрес|
|------|----------|
|Все области, за исключением указанных ниже|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Германия|51.4.229.218|
|Китай|139.217.8.252|
|Штат в США (для обслуживания государственных организаций США)|52.244.48.71|

Чтобы включить доступ к порталу Azure на портале Azure задайте **разрешить доступ к порталу Azure** значение **ON** на портале Azure ( **включить управление доступом IP** значение должно быть присвоено **ON** для просмотра и изменения **разрешить доступ к порталу Azure** значение).

![Снимок экрана, показывающий Включение Azure доступа к порталу](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-other-azure-paas-services"></a>Соединения от других служб Azure PaaS 
В Azure служб PaaS, таких как Azure Stream analytics, Azure функции и службы приложений Azure используются совместно с Azure Cosmos DB. Для доступа к базе данных Azure Cosmos учетной записи базы данных из этих служб, чьи IP-адреса не удается получить добавьте IP-адрес 0.0.0.0 в список разрешенных IP-адресов, связанных с учетной записью базы данных Azure Cosmos DB, программным образом или установите **Разрешить доступ к службам Azure** значение ON на портале Azure ( **включить управление доступом IP** должно быть присвоено значение **ON** для просмотра и изменения **разрешить Доступ к службам Azure** значение). Это гарантирует доступность службы Azure PaaS учетной записи Azure Cosmos DB. 

![Снимок экрана, показывающий, как открыть страницу брандмауэра на портале Azure](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Соединения с вашей текущей IP-адреса

Для упрощения разработки портал Azure помогает определить и добавить IP-адрес клиентского компьютера в список разрешенных адресов, чтобы приложения, работающие на вашем компьютере, могли получить доступ к учетной записи Azure Cosmos DB. IP-адрес клиента определяется так, как он отображается на портале. Это может быть IP-адрес клиента вашего компьютера, а также IP-адрес вашего сетевого шлюза. Не забудьте удалить его до внесения изменений в рабочую среду.

![Снимок экрана, показывающий, как настроить параметры брандмауэра для текущего IP-адреса](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Подключения из облачных служб
В Azure облачные службы — это стандартный способ размещения логики службы среднего уровня с помощью Azure Cosmos DB. Чтобы разрешить доступ к учетной записи базы данных Azure Cosmos DB из облачной службы, общедоступный IP-адрес этой службы необходимо добавить в список разрешенных IP-адресов, связанных с учетной записью базы данных Azure Cosmos DB. Для этого следует [настроить политику контроля доступа на основе IP-адресов](#configure-ip-policy).  Это позволит обеспечить доступ к учетной записи базы данных Azure Cosmos DB для всех экземпляров ролей облачных служб. IP-адреса для ваших облачных служб можно получить на портале Azure, как показано на следующем снимке экрана:

![Снимок экрана с общедоступным IP-адресом для облачной службы на портале Azure](./media/firewall-support/public-ip-addresses.png)

При расширении своей облачной службы за счет добавления дополнительных экземпляров ролей эти новые экземпляры автоматически получают доступ к учетной записи базы данных Azure Cosmos DB, так как они входят в ту же самую облачную службу.

## <a name="connections-from-virtual-machines"></a>Подключения из виртуальных машин
[Виртуальные машины](https://azure.microsoft.com/services/virtual-machines/) или [масштабируемые наборы виртуальных машин](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) также можно использовать для размещения служб среднего уровня с помощью Azure Cosmos DB.  Чтобы настроить доступ к учетной записи базы данных Azure Cosmos DB из виртуальных машин, общедоступные IP-адреса виртуальной машины и (или) масштабируемого набора виртуальных машин необходимо добавить в список разрешенных IP-адресов для этой учетной записи. Для этого следует [настроить политику контроля доступа на основе IP-адресов](#configure-ip-policy). IP-адреса для виртуальных машин можно получить на портале Azure, как показано на следующем снимке экрана.

![Снимок экрана с общедоступным IP-адресом для виртуальной машины на портале Azure](./media/firewall-support/public-ip-addresses-dns.png)

При добавлении в группу дополнительных экземпляров виртуальных машин они автоматически получают доступ к вашей учетной записи базы данных Azure Cosmos DB.

## <a name="connections-from-the-internet"></a>Подключения через Интернет
При подключении к учетной записи базы данных Azure Cosmos DB с компьютера через Интернет IP-адрес или диапазон IP-адресов клиента необходимо добавить в список разрешенных IP-адресов для этой учетной записи. 

## <a name="troubleshooting-the-ip-access-control-policy"></a>Устранение неполадок с политикой контроля доступа на основе IP-адресов
### <a name="portal-operations"></a>Операции на портале
После включения политики контроля доступа на основе IP-адресов для учетной записи базы данных Azure Cosmos DB все подключения к этой учетной записи с компьютеров, диапазоны IP-адресов которых не входят в список разрешенных, блокируются. Поэтому если вы хотите включить данные портала плоскости такие операции, как просмотр коллекций и запрос документов, необходимо явно разрешить использование Azure доступа к порталу **брандмауэра** на портале. 

![Снимок экрана, показывающий, как разрешить доступ к порталу Azure](./media/firewall-support/enable-azure-portal.png)

### <a name="sdk--rest-api"></a>Пакет SDK и REST API
По соображениям безопасности при доступе с помощью пакета SDK и REST API с компьютеров, IP-адреса которых не входят в список разрешенных, вернется ответ с кодом 404 (объект не найден) без каких-либо дополнительных сведений. Проверьте список разрешенных IP-адресов и убедитесь, что у политики допустимая конфигурация для учетной записи базы данных Azure Cosmos DB.

## <a name="next-steps"></a>Дальнейшие действия
Сведения о советы по повышению производительности, связанные с сетью см. в разделе [советы по повышению производительности](performance-tips.md).

