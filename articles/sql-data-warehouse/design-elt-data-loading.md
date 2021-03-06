---
title: "Разработка ELT для хранилища данных Azure SQL | Документы Microsoft"
description: "Объединять технологии для перемещения данных в Azure, а также загрузки данных в хранилище данных SQL для извлечения, загрузки и преобразования (ELT) процесс проектирования для хранилища данных SQL Azure."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 12/12/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: e94dca69c77c46034e318205279be5188e1371f5
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2017
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Проектирование извлечения, загрузки и преобразования (ELT) для хранилища данных Azure SQL

Объединять технологии для размещения данных в хранилище Azure и загрузка данных в хранилище данных SQL для извлечения, загрузки и преобразования (ELT) процесс проектирования для хранилища данных SQL Azure. В этой статье описаны технологии, которые поддерживает загрузку с помощью Polybase, а затем посвящен разработке ELT процесс, который использует PolyBase с помощью T-SQL для загрузки данных в хранилище данных SQL из хранилища Azure.

## <a name="what-is-elt"></a>Что такое ELT?

Извлечение, загрузки, и преобразование (ELT) — это процесс, с помощью которого перемещения данных из исходной системы в хранилище данных назначения. Этот процесс выполняется на регулярной основе, например ежечасно или ежедневно, чтобы получить только что созданный данных в хранилище данных. Разработке ELT процесс, который использует PolyBase для загрузки данных в хранилище данных SQL является идеальным способом получения данных из источника данных.

ELT сначала загружает и затем преобразует данные, а извлечения, преобразования и загрузки (ETL) преобразует данные перед его загрузкой. Выполнение ELT вместо ETL сохраняет стоимости предоставления ресурсов для преобразования данных перед его загрузкой. При использовании хранилища данных SQL, ELT использует система MPP для выполнения преобразования.

Хотя существует много разновидностей реализации ELT для хранилища данных SQL, это основные шаги:  

1. Извлеките исходные данные в текстовые файлы.
2. Перемещаться данные в хранилище больших двоичных объектов Azure или хранилища Озера данных Azure.
3. Подготовка данных для загрузки.
2. Загрузка данных в промежуточные таблицы с помощью PolyBase хранилища данных SQL.
3. Преобразования данных.
4. Вставьте данные в таблицы производства.


Учебник загрузки см. в разделе [PolyBase используется для загрузки данных из хранилища BLOB-объектов Azure в хранилище данных SQL Azure](load-data-from-azure-blob-storage-using-polybase.md).

Дополнительные сведения см. в разделе [Загрузка шаблонов блог](http://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Параметры для загрузки с помощью PolyBase

PolyBase — это технология, которая обращается к данным за пределами базы данных посредством языка T-SQL. Это лучший способ загрузки данных в хранилище данных SQL. С помощью PolyBase загрузки данных в параллельном режиме из источника данных непосредственно для вычислительных узлов. 

Чтобы загрузить данные с помощью PolyBase, можно использовать любой из этих параметров загрузки.

- [PolyBase с помощью T-SQL](load-data-from-azure-blob-storage-using-polybase.md) работает хорошо, когда данные хранятся в хранилище больших двоичных объектов Azure или хранилища Озера данных Azure. Предоставляет полный контроль над процесс загрузки, но также необходимо определить объекты внешних данных. Другие методы определения этих объектов в фоновом, как сопоставить исходные таблицы для целевых таблиц.  Для управления загружает T-SQL, которое можно использовать фабрики данных Azure, служб SSIS или функций Azure. 
- [PolyBase со службами SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md) работает хорошо, когда источник данных в SQL Server, SQL Server локально или в облаке. Служб SSIS определяет источника для сопоставления целевой таблицы, а также управляет нагрузки. При наличии пакетов служб SSIS, можно изменить пакеты, для работы с нового хранилища данных назначения. 
- [PolyBase с фабрикой данных Azure (ADF)](sql-data-warehouse-load-with-data-factory.md) представляет собой другое средство orchestration.  Он определяет конвейера и составление расписания заданий. 

### <a name="polybase-external-file-formats"></a>PolyBase внешние форматы файлов

PolyBase загружает данные из UTF-8, а в кодировке UTF-16 с разделителями текстовых файлов. Кроме файлов с разделителями он загружает из форматов файлов Hadoop RC-файла, ORC и Parquet. PolyBase можно загрузить данные из Gzip и высокой сжатых файлов. PolyBase в настоящее время не поддерживает расширенные ASCII, формат фиксированной ширины и вложенных форматы, такие как WinZip, JSON и XML.

### <a name="non-polybase-loading-options"></a>Параметры загрузки не PolyBase
Если данные не совместимо с PolyBase, можно использовать [bcp](sql-data-warehouse-load-with-bcp.md) или [API-Интерфейс SQLBulkCopy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). bcp загружает напрямую к хранилищу данных SQL, минуя хранилища больших двоичных объектов Azure и предназначена только для небольших нагрузок. Обратите внимание, что производительность нагрузки этих параметров выполняется значительно медленнее, чем PolyBase. 


## <a name="extract-source-data"></a>Извлечение исходных данных

Получение данных из исходной системы зависит от источника.  Целью является перемещение данных в текстовых файлов с разделителями. Если вы используете SQL Server, можно использовать [средство командной строки bcp](/sql/tools/bcp-utility) для экспорта данных.  

## <a name="land-data-to-azure-storage"></a>Земли данных в хранилище Azure

Чтобы получить данные в хранилище Azure, можно перенести его в [хранилища больших двоичных объектов Azure](../storage/blobs/storage-blobs-introduction.md) или [хранилища Озера данных Azure](../data-lake-store/data-lake-store-overview.md). В любом месте данные должны храниться в текстовые файлы. Polybase можно загрузить из любого места.

Ниже приведены средства и службы, которые можно использовать для перемещения данных в хранилище Azure.

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) служба повышает пропускную способность сети, производительности, а также предсказуемости. ExpressRoute — это служба, которая направляет данные с помощью выделенного частного подключения в Azure. Подключения ExpressRoute не передают данные через общедоступный Интернет. Соединения обеспечивают большую надежность, высокую скорость, меньшую задержку и большую безопасность по сравнению с обычными подключениями через общедоступный Интернет.
- [Служебную программу AZCopy](../storage/common/storage-use-azcopy.md) перемещает данные в хранилище Azure через общедоступный Интернет. Это работает, если ваш размера данных меньше, чем 10 ТБ. Чтобы выполнить загрузку на регулярной основе с помощью AZCopy, проверьте скорость сети для просмотра, если это допустимо. 
- [Фабрика данных Azure (ADF)](../data-factory/introduction.md) имеет шлюз, который можно установить на локальном сервере. Затем можно создать конвейер для перемещения данных из вашего локального сервера в хранилище Azure.

Дополнительные сведения см. в разделе [перемещение данных в и из хранилища Azure](../storage/common/storage-moving-data.md)


## <a name="prepare-data"></a>Подготовка данных

Может потребоваться подготовка и очистка данных в учетной записи до их загрузки в хранилище данных SQL. Подготовка данных может выполняться, пока данные хранятся в источнике, как экспортировать данные в текстовый файл, или после копирования данных в хранилище Azure.  Можно легко работать с данными, как можно раньше в процессе максимально.  

### <a name="define-external-tables"></a>Определение внешних таблиц
Перед загрузкой данных, необходимо определить внешних таблиц в хранилище данных. PolyBase использует внешние таблицы для определения и доступа к данным в хранилище Azure. Внешняя таблица аналогична обычной таблицы. Основное отличие заключается в точках внешней таблицы с данными, которые хранятся вне хранилища данных. 

Определение внешние таблицы включает в себя указание источника данных, формат текстовых файлов и определения таблиц. Ниже приведены в разделах синтаксиса T-SQL, которые понадобятся:
- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Пример создания внешних объектов см. в разделе [Создание внешних таблиц](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) шаг в учебнике «загрузки».

### <a name="format-text-files"></a>Файлы форматирования текста

После определения внешних объектов необходимо обеспечить соответствие строк текстовых файлов с внешней таблицы и определение формата файла. Данные в каждой строке текстового файла должно соответствовать определения таблицы.

Для форматирования текстовых файлов:

- Если будут поступать данные из источника нереляционных, необходимо преобразовать его в строки и столбцы. Является ли данные из реляционных и нереляционных источника, выравнивается определения столбцов для таблицы, в которую необходимо выполнить загрузку данных необходимо преобразование данных. 
- Форматировать данные в текстовый файл, чтобы соответствовать столбцы и типы данных в целевой таблице хранилища данных SQL. Неполное соответствие между типами данных в внешние текстовые файлы и таблицы хранилища данных, строки будут отклонены во время загрузки.
- Отдельные поля в текстовом файле с признаком конца.  Обязательно используйте символ или последовательность символов, не найден в источнике данных. Используйте терминатор с [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Загрузка в промежуточную таблицу
Для получения данных в хранилище данных, он подходит для первой загрузки данных в промежуточную таблицу. С помощью промежуточной таблицы, может обрабатывать ошибки, не мешая таблицы производства и избежать выполнения операции отката рабочей таблицы. Промежуточная таблица также предоставляет возможность использовать хранилище данных SQL для запуска преобразований перед вставкой данных в таблицы производства.

Чтобы загрузить с помощью T-SQL, выполните [создать ТАБЛИЦУ AS ВЫБЕРИТЕ (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) инструкции T-SQL. Эта команда вставки результатов инструкции select в новую таблицу. Когда инструкция выбирает из внешней таблицы, импорте внешних данных. 

В следующем примере ext. Дата является внешней таблицы. Все строки, импортируются в новую таблицу с именем dbo. Дата.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>Преобразования данных
Данных во время промежуточной таблицы, выполните преобразования, необходимые для рабочей нагрузки. Затем переместите данные в таблицы производства.

## <a name="insert-data-into-production-table"></a>Вставка данных в рабочую таблицу

INSERT INTO... Инструкция SELECT перемещает данные из промежуточной таблицы для постоянной таблицы. 

При разработке процесса ETL, попытайтесь запустить процесс на пример небольшого теста. Попробуйте выполнить извлечение 1000 строк из таблицы в файл, переместите его в Azure, а затем повторите их загрузки в промежуточную таблицу. 

## <a name="partner-loading-solutions"></a>Загрузка решения партнеров
Многие из наших партнеров предлагают решения для загрузки. Чтобы узнать больше, просмотреть список наших [решения партнеров](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Дальнейшие действия
Для загрузки руководства, в разделе [рекомендации для загрузки данных](guidance-for-loading-data.md).


