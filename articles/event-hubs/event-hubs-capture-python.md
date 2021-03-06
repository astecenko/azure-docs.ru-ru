---
title: "Пошаговое руководство. Запись концентраторов событий Azure | Документация Майкрософт"
description: "Пример приложения, использующего пакет SDK Azure Python для демонстрации функции записи концентраторов событий."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: cdbb2baea2bc6c45908369ff821c264b66053d95
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Пошаговое руководство. Использование записи концентраторов событий с Python

Запись концентраторов событий — это функция концентраторов событий, которая позволяет автоматически передавать потоковые данные из концентратора событий в выбранную учетную запись хранилища BLOB-объектов Azure. Эта возможность упрощает выполнение пакетной обработки данных потоковой передачи в режиме реального времени. В этой статье мы расскажем, как использовать запись концентраторов событий с Python. Дополнительные сведения о записи концентраторов событий см. в [этом обзоре](event-hubs-capture-overview.md).

В приведенном примере для демонстрации функции записи используется [пакет SDK Azure для Python](https://azure.microsoft.com/develop/python/). Программа sender.py отправляет имитацию телеметрии окружающей среды концентраторам событий в формате JSON. Настройками концентратора событий предусмотрено использование функции записи для пакетной записи этих данных в хранилище BLOB-объектов. Затем capturereader.py считывает эти большие двоичные объекты и создает файл для дозаписи для каждого устройства, после чего записывает данные в CSV-файлы.

## <a name="what-will-be-accomplished"></a>Что будет выполнено

1. создать учетную запись хранилища BLOB-объектов Azure, содержащую контейнер больших двоичных объектов, с помощью портала Azure;
2. создать пространство имен концентратора событий с помощью портала Azure;
3. создать концентратор событий с включенной функцией записи с помощью портала Azure;
4. отправить данные в концентратор событий с помощью сценария Python;
5. прочитать файлы из записи и обработать их с помощью другого скрипта Python.

## <a name="prerequisites"></a>Технические условия

- Python 2.7.x.
- Подписка Azure
- Активные [пространство имен концентраторов событий и концентратор событий](event-hubs-create.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Создание учетной записи хранения Azure
1. Войдите на [портал Azure][Azure portal].
2. В области навигации слева на странице портала щелкните **Создать** > **Хранилище** > **Учетная запись хранения**.
3. Заполните поля в колонке учетной записи хранения и нажмите кнопку **Создать**.
   
   ![][1]
4. Получив сообщение об **успешном выполнении развертывания**, щелкните имя новой учетной записи хранения, затем в колонке **Основные компоненты** щелкните **BLOB-объекты**. В верхней части открывшейся колонки **Служба BLOB-объектов** щелкните **+ Container** (+ Контейнер). Присвойте контейнеру имя **capture**, затем закройте колонку **Служба BLOB-объектов**.
5. Щелкните **Ключи доступа** в колонке слева и скопируйте имя учетной записи хранения, а также значение **key1**. Сохраните на время эти значения в Блокноте или любом другом месте.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Создание сценария Python для отправки событий в концентратор событий
1. Откройте предпочитаемый редактор Python, например [Visual Studio Code][Visual Studio Code].
2. Создайте сценарий с именем **sender.py**. Этот скрипт отправляет 200 событий в ваш концентратор событий. События представляют собой простые данные показаний среды, пересылаемые в формате JSON.
3. Скопируйте приведенный ниже код и вставьте его в сценарий sender.py.
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. Обновите предыдущий код, указав имя пространства имен, значение ключа и имя концентратора событий, полученные при создании пространства имен концентратора событий.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Создание скрипта Python для чтения файлов записи

1. Заполните поля в колонке и щелкните **Создать**.
2. Создайте скрипт с именем **capturereader.py**. Он считывает собранные файлы и создает для каждого устройства отдельный файл для записи данных только по этому устройству.
3. Скопируйте приведенный ниже код и вставьте его в скрипт capturereader.py.
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          #content_length == 508 is an empty file, so only process content_length > 508 i.e. skip  empty files
          if blob.properties.content_length > 508:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Не забудьте вставить соответствующие значения имени вашей учетной записи хранения и ключа в вызов `startProcessing`.

## <a name="run-the-scripts"></a>Выполнение сценариев
1. Откройте окно командной строки с Python в соответствующем расположении, а затем выполните следующие команды для установки пакетов необходимых компонентов Python.
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Если вы используете более раннюю версию Azure или хранилища Azure, вам может понадобиться параметр **--upgrade**.
   
  Возможно, вам также потребуется выполнить следующую команду (необязательно для большинства систем).
   
  ```
  pip install cryptography
  ```
2. Укажите папку, в которой были сохранены скрипты sender.py и capturereader.py, и выполните следующую команду.
   
  ```
  start python sender.py
  ```
   
  Эта команда запускает новый процесс Python для выполнения скрипта sender.
3. Подождите несколько минут до запуска записи. Теперь введите следующую команду в исходном окне командной строки.
   
   ```
   python capturereader.py
   ```

   Этот обработчик записи загружает все большие двоичные объекты из учетной записи хранения или контейнера в локальный каталог. Он обрабатывает все непустые файлы и записывает результаты в CSV-файлы в локальном каталоге.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о концентраторах событий см. в следующих источниках:

* [Запись концентраторов событий Azure][Overview of Event Hubs Capture]
* [Примеры приложений, использующие концентраторы событий](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Обзор концентраторов событий Azure][Event Hubs overview].

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
