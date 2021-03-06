---
title: "Как выполнить резервное копирование и восстановление сервера в базе данных Azure для MySQL | Документация Майкрософт"
description: "Как выполнить резервное копирование и восстановление сервера в базе данных Azure для MySQL с помощью Azure CLI."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 44b3c68b8df4006d3fe087e5ad4118d7616d3d9a
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-cli"></a>Как выполнить резервное копирование и восстановление сервера в базе данных Azure для MySQL с помощью Azure CLI

Используйте базу данных Azure для MySQL, чтобы восстановить базу данных сервера с более ранней точки во времени за период от 7 до 35 дней.

## <a name="prerequisites"></a>Предварительные требования
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [Сервер и база данных Azure для MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Если вы решили установить и использовать Azure CLI локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы проверить версию, в командной строке Azure CLI введите `az --version`. Чтобы выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Резервное копирование выполняется автоматически
При использовании базы данных Azure для MySQL служба базы данных автоматически создает резервную копию службы каждые пять минут. 

Для уровня "Базовый" доступны резервные копии за 7 дней. Для уровня "Стандартный" — за 35 дней. Дополнительные сведения см. в описании для [ценовых категорий для базы данных Azure для MySQL](concepts-service-tiers.md).

С помощью этой функции автоматического резервного копирования можно восстановить сервер и его базы данных до более ранней даты или точки во времени.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Восстановление базы данных до предшествующей точки во времени с помощью Azure CLI
База данных Azure для MySQL позволяет восстановить сервер до предшествующей точки во времени. Восстановленные данные копируются на новый сервер, а имеющийся сервер остается неизменным. Например, если таблица была случайно удалена в полдень, ее можно восстановить до момента сразу перед полуднем. Затем вы можете извлечь отсутствующие таблицы и данные из восстановленной копии сервера. 

Чтобы восстановить сервер, используйте команду Azure CLI [az mysql server restore](/cli/azure/mysql/server#az_mysql_server_restore).

### <a name="run-the-restore-command"></a>Выполнение команды restore

Для восстановления сервера в командной строке Azure CLI введите следующую команду:

```azurecli-interactive
az mysql server restore --resource-group myResourceGroup --name myserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server myserver4demo
```

Для команды `az mysql server restore` обязательны указанные ниже параметры.
| Настройка | Рекомендуемое значение | Описание  |
| --- | --- | --- |
| resource-group | myResourceGroup |  Группа ресурсов, в которой находится исходный сервер.  |
| name | myserver-restored | Имя нового сервера, созданного командой restore. |
| restore-point-in-time | 2017-04-13T13:59:00Z | Выберите точку во времени, до которой необходимо выполнить восстановление. Значения даты и времени должны находиться в пределах срока хранения резервной копии исходного сервера. Используйте формат даты и времени ISO8601. Можно использовать местный часовой пояс, например `2017-04-13T05:59:00-08:00`. Также можно использовать формат UTC Zulu, например `2017-04-13T13:59:00Z`. |
| source-server | myserver4demo | Имя или идентификатор исходного сервера, с которого необходимо выполнить восстановление. |

При восстановлении сервера до более ранней точки во времени будет создан новый сервер. Исходный сервер и его базы данных, начиная с указанного момента во времени, копируются на новый сервер.

Значения расположения и ценовой категории для восстановленного сервера совпадают со значениями исходного сервера. 

Команда `az mysql server restore` выполняется в синхронном режиме. После завершения восстановления сервера его можно снова использовать, чтобы повторить процесс восстановления до другого момента во времени. 

После завершения восстановления найдите новый сервер, чтобы убедиться, что данные были восстановлены, как и ожидалось.

## <a name="next-steps"></a>Дальнейшие действия
[Библиотеки подключений для базы данных Azure для MySQL](concepts-connection-libraries.md)
