## <a name="what-is-blob-storage"></a>Что такое хранилище больших двоичных объектов
Хранилище BLOB-объектов Azure — это служба хранения большого количества неструктурированных данных объектов, таких как текстовые или двоичные данные, к которым можно получить доступ практически из любой точки мира по протоколу HTTP или HTTPS. Хранилища BLOB-объектов можно использовать для предоставления данных в открытом доступе всему миру или для хранения данных от приложений в частном порядке.

Наиболее частые способы использования хранилища BLOB-объектов:

* Обслуживающий изображения или документы непосредственно в браузере.
* Хранение файлов для распределенного доступа.
* Потоковая передача видео- и аудиопотоки.
* Хранение данных для резервного копирования и восстановления, аварийного восстановления и архивирования.
* Хранение данных для анализа с локальной или службы, размещенной в Azure.

## <a name="blob-service-concepts"></a>Основные понятия службы BLOB-объектов
Служба Blob-объектов содержит следующие компоненты:

![Диаграмма архитектуры служб больших двоичных объектов](./media/storage-blob-concepts-include/blob1.png)

* **Учетная запись хранения.** Доступ к службе хранилища Azure всегда осуществляется с помощью учетной записи хранения. Эта учетная запись может быть **учетную запись хранения общего назначения** или **BLOB-объектов учетной записи хранилища**, который специализируется для хранения объектов или больших двоичных объектов. См. дополнительные сведения [об учетных записях хранения Azure](../articles/storage/common/storage-create-storage-account.md).
* **Контейнер**. В контейнере группируется набор BLOB-объектов. Все BLOB-объекты должны содержаться в контейнере. Учетная запись может содержать неограниченное число контейнеров. Контейнер может хранить неограниченное количество больших двоичных объектов. Учтите, что все знаки в имени контейнера должны быть строчными.
* **BLOB-объект**. Файл любого типа и размера. Хранилище Azure предлагает три типа больших двоичных объектов: блочные большие двоичные объекты, добавлять большие двоичные объекты и страничные большие двоичные объекты.
  
    *Блочные BLOB-объекты* идеально подходят для хранения текстовых и двоичных файлов, таких как документы и файлы мультимедиа. Один блочный BLOB-объект может содержать до 50 000 блоков размером до 100 МБ каждый с общим размером немного более 4,75 ТБ (100 МБ X 50 000). 

    *Добавочные большие двоичные объекты* схожи с блочными BLOB-объектами, так как они состоят из блоков, однако они оптимизированы для операций добавления и поэтому полезны для сценариев ведения журнала. Один добавочный BLOB-объект может содержать до 50 000 блоков размером до 4 МБ каждый с общим размером немного более 195 ГБ (4 МБ X 50 000).
  
    *Страничные BLOB-объекты* могут иметь размер до 1 ТБ. Они более эффективны для частых операций чтения и записи. Виртуальные машины Azure используют страничные большие двоичные объекты как операционной системы и дисков данных.
  
    Дополнительные сведения об именовании контейнеров и больших двоичных объектов см. в разделе [именование и ссылки на контейнеры, большие двоичные объекты и метаданные](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

