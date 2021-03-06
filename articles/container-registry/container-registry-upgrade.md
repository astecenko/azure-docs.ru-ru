---
title: "Обновление реестра контейнера классический Azure"
description: "Воспользоваться преимуществами расширенный набор функций Basic, Standard и Premium управляется контейнера реестры обновление неуправляемые реестра классический контейнера."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 19090bb69d7165c1e904450dc93b925e23e44782
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-a-classic-container-registry"></a>Обновление реестра классический контейнера

Azure реестра контейнера (ACR) доступна на нескольких уровнях, [называется SKU](container-registry-skus.md). Первоначальный выпуск ACR предлагаемых одного SKU, Classic, не обеспечивает несколько возможностей, присущие Basic, Standard и Premium SKU (назывался *управляемых* реестров). В этой статье подробно описывается перенос неуправляемые классический реестра в один из управляемых продуктов, чтобы воспользоваться преимуществами их набор улучшенных функций.

## <a name="why-upgrade"></a>Зачем выполнять обновление

Из-за ограниченные возможности реестров классический неуправляемые рекомендуется все реестры классический не обновленные до Basic, Standard или Premium управляемого реестров. Эти номера SKU более высокого уровня более тесно интегрируют реестр с возможностями Azure.

Содержат управляемый реестров:

* Интеграция Azure Active Directory для [отдельного имени входа](container-registry-authentication.md#individual-login-with-azure-ad)
* поддержка удаления образов и тегов;
* [Георепликация](container-registry-geo-replication.md)
* [Объекты Webhook](container-registry-webhook.md)

Чаще всего реестр уровня "Классический" зависит от учетной записи хранения, которую Azure автоматически подготавливает в вашей подписке Azure при создании реестра. В отличие от этого, номера SKU уровней "Базовый", "Стандартный" и "Премиум" пользуются преимуществами *управляемого хранилища*. Иными словами, Azure прозрачно управляет хранилищем ваших образов (в подписке не создается отдельная учетная запись хранения).

Управляемый реестра хранилища предоставляет следующие преимущества:

* Образы контейнеров [зашифрованы при хранении](../storage/common/storage-service-encryption.md).
* Образы хранятся с использованием [геоизбыточного хранилища](../storage/common/storage-redundancy.md#geo-redundant-storage), что обеспечивает резервное копирование образов с репликацией в нескольких регионах.
* Возможность свободно [перемещение между SKU](container-registry-skus.md#changing-skus), позволяя более высокую пропускную способность, при выборе более высокого уровня SKU. С каждым номером SKU ACR может удовлетворить ваши требования к пропускной способности по мере роста ваших потребностей.
* Модель безопасности единой для реестра и ее хранилище предоставляет упрощенный rights management. Разрешения только для реестра контейнера, управлять без необходимости также управлять разрешениями для отдельную учетную запись хранилища.

## <a name="migration-considerations"></a>Рекомендации по переносу

При изменении реестра классический управляемого реестр, Azure необходимо скопировать все существующие образы контейнеров из учетной записи хранения созданы записи контроля доступа в вашей подписке учетной записи хранения управляется Azure. В зависимости от размеров реестра этот процесс может занять несколько минут до нескольких часов.

Во время преобразования все `docker push` операций блокируется, пока `docker pull` продолжает функционировать.

Не удаляйте и не изменять содержимое учетной записи хранения резервное классический реестра во время преобразования. Это может привести к повреждению контейнера изображений.

После завершения миграции учетной записи хранения в подписке, первоначально резервному реестра классический больше используется контроля доступа. После проверки успешности миграции, рассмотрите возможность удаления учетной записи хранилища, чтобы свести к минимуму затраты.

>[!IMPORTANT]
> Обновление от классической до одного из управляемых SKU является **односторонний процесс**. После преобразования классический реестра до выпуска Basic, Standard или Premium, нельзя вернуть на обычную. Можно Однако свободно перемещать между управляемого SKU с емкостью, достаточной для реестра.

## <a name="how-to-upgrade"></a>Обновление

Неуправляемые классический реестра можно обновить до одного из управляемых SKU несколькими способами. В следующих разделах описывается процесс использования [Azure CLI] [ azure-cli] и [портал Azure][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Обновления в интерфейсе командной строки Azure

Для обновления реестра классический в Azure CLI, выполнение [обновление записи контроля доступа az] [ az-acr-update] команду и укажите новый номер SKU для раздела реестра. В следующем примере классический реестра с именем *myclassicregistry* обновляется до Premium SKU:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

После завершения миграции вы увидите результат, аналогичный приведенному ниже. Обратите внимание, что `sku` — «Premium» и `storageAccount` пуст «,», указывающее, что Azure теперь управляет хранилище образов для этого раздела реестра.

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

При указании управляемого реестра которого Максимальная емкость меньше, чем размер реестра классический SKU, вы получите следующее сообщение об ошибке.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

При получении аналогичное сообщение об ошибке выполнения [обновление записи контроля доступа az] [ az-acr-update] еще раз и укажите, предлагаемые SKU, который следующим наибольшим уровня SKU, который может вместить изображений.

## <a name="upgrade-in-azure-portal"></a>Обновление на портале Azure

При обновлении реестра классический через портал Azure, Azure автоматически выбирает SKU самого низкого уровня, способное изображений. Например, если в реестре содержатся 12 Гиб в образах, Azure автоматически выбирает и преобразует реестра классический стандарту (максимум 100 Гиб).

Обновление реестра классический с помощью портала Azure, перейдите в реестре контейнеров **Обзор** и выберите **обновление управляемого реестр**.

![Кнопка на портале Azure пользовательского интерфейса обновления классический реестра][update-classic-01-upgrade]

Выберите **ОК** подтверждения обновления управляемого реестр.

![Классический реестра обновление подтверждения в пользовательском Интерфейсе портала Azure][update-classic-02-confirm]

Во время миграции портале указывает, что реестр **состояние подготовки** — *обновление*. Как упоминалось ранее, `docker push` операции недоступны во время миграции и не удаляйте или учетную запись хранения использовать классический реестра во время миграции — это обновление может привести к повреждению изображения.

![Классический реестра ход выполнения обновления на портале Azure пользовательского интерфейса][update-classic-03-updating]

По завершении миграции, **состояние подготовки** указывает *успешно*, и вы можете еще раз `docker push` в системный реестр.

![Классический реестра Обновите состояние завершения в пользовательском Интерфейсе портала Azure][update-classic-04-updated]

## <a name="next-steps"></a>Дальнейшие действия

Сразу обновления реестра до выпуска Basic, Standard или Premium классический Azure больше не использует учетную запись хранилища, первоначально резервного классический реестра. Чтобы сократить затраты, рассмотрите возможность удаления учетной записи хранилища или контейнере BLOB-объектов в учетной записи, которая содержит свои старые образы контейнера.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade\update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade\update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade\update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade\update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com