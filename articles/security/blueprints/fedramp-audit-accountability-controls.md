---
title: "Служба автоматизации Azure Blueprint FedRAMP — аудит и система отчетности"
description: "Веб-приложения для FedRAMP — аудит и система отчетности"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: c5858e07-ca74-4526-b31f-e6b4e55bb594
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 83ef9cbb7652bf128d7758237a8e6fbeed6c6565
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
# <a name="audit-and-accountability-au"></a>Аудит и система отчетности (AU)

> [!NOTE]
> Эти директивы определены NIST и Министерством торговли США в составе специальной публикации NIST 800-53 редакции 4. Сведения о процедурах тестирования и рекомендации по каждой директиве см. в документе NIST 800-53 редакции 4.

## <a name="nist-800-53-control-au-1"></a>Директива NIST 800-53 AU-1

#### <a name="audit-and-accountability-policy-and-procedures"></a>Процедуры и политика аудита и системы отчетности

**AU-1**. Организация разрабатывает, документирует и распространяет среди [определенного организацией персонала или ролей] политику аудита и системы отчетности, которая охватывает назначение, сферу действия, роли, обязанности, обязательства по управлению, координацию между организационными подразделениями и соответствие; процедуры для содействия осуществлению политики аудита и системы отчетности и связанных с ней средств контроля аудита и системы отчетности; рассматривает и обновляет текущую политику аудита и системы отчетности с [определенной организацией частотой]; процедуры обеспечения аудита и системы отчетности с [определенной организацией частотой].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиентских корпоративных политик и процедур аудита и системы отчетности может быть достаточно для реализации этой директивы. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-2a"></a>Директива NIST 800-53 AU-2.a

#### <a name="audit-events"></a>События аудита

**AU-2.a**. Организация определяет, что информационная система поддерживает аудит следующих событий: [определенные организацией события для аудита].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Возможность аудита для этой службы Azure Blueprint обеспечивается службами Azure Monitor и Log Analytics в OMS. Azure Monitor предоставляет подробные журналы аудита о действиях, связанных с развернутыми ресурсами. Эти журналы, а также журналы на уровне операционной системы собираются службой Log Analytics и сохраняются в репозитории OMS. Служба Log Analytics сопоставляет данные аудита в отношении ресурсов, развернутых этим решением, и ее можно расширить до веб-приложения, развернутого клиентом. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-2b"></a>Директива NIST 800-53 AU-2.b

#### <a name="audit-events"></a>События аудита

**AU-2.b**. Организация координирует функцию аудита безопасности с другими корпоративными сущностями, которым требуются сведения, связанные с аудитом, чтобы улучшить взаимную поддержку и посодействовать выбору событий для аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент может использовать установленный процесс корпоративного уровня, определяющий события для аудита. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-2c"></a>Директива AU-2.c NIST 800-53

#### <a name="audit-events"></a>События аудита

**AU-2.c**. Организация предоставляет обоснование относительно причины выбора событий для аудита в качестве подходящих для поддержки расследования имевших место инцидентов безопасности.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | События, подвергшиеся аудиту, выполненному этой службой Azure Blueprint, включают информацию, достаточную для определения того, когда происходят события, определения источника события, результата события и другой подробной информации, необходимой при расследовании инцидентов безопасности. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-2d"></a>Директива NIST 800-53 AU-2.d

#### <a name="audit-events"></a>События аудита

**AU-2.d**. Организация определяет, что следующие события подлежат аудиту в информационной системе: [определенные организацией события для аудита (подмножество событий для аудита, определенных в директиве AU-2 a.) с частотой (или согласно ситуации) аудита для каждого определенного события].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | В рамках этого принципа Azure отслеживает события, собираемые журналами действий для развернутых ресурсов, журналами уровня ОС, журналами Active Directory и SQL Server. Клиенты могут выбирать дополнительные события для аудита в зависимости от требований. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-2-3"></a>Директива NIST 800-53 AU-2 (3)

#### <a name="audit-events--reviews-and-updates"></a>События аудита | Проверки и обновления

**AU-2 (3)**. Организация проверяет и обновляет события, подвергшиеся аудиту, с [определенной организацией частотой].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент может использовать установленную периодическую проверку на корпоративном уровне и обновлять процесс для определенного набора событий, подвергшихся аудиту. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-3"></a>Директива AU-3 NIST 800-53

#### <a name="content-of-audit-records"></a>Содержимое записей аудита

**AU-3**. Информационная система создает записи аудита, содержащие сведения, по которым можно определить, событие какого типа произошло, когда это событие произошло, где оно произошло, источник события, результат события, а также любые отдельные лица или объекты, связанные с событием.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint использует встроенные функции аудита Azure, Windows Server и SQL Server. Эти решения аудита регистрируют записи аудита со сведениями, достаточными для удовлетворения требований этой директивы. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-3-1"></a>Директива NIST 800-53 AU-3 (1)

#### <a name="content-of-audit-records--additional-audit-information"></a>Содержимое записей аудита | Дополнительные сведения об аудите

**AU-3 (1)**. Информационная система создает записи аудита, содержащие следующие сведения: [определенные организацией дополнительные, более подробные сведения].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | События журнала действий Azure используют подробную схему, содержащую поля для более чем 20 типов данных об аудите. Кроме журнала действий эта служба Azure Blueprint развертывает решение Log Analytics в OMS, которое поддерживает широкий набор источников данных, включая журналы Windows, Linux, журналы системы диагностики Azure и клиентские журналы.  |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-3-2"></a>Директива AU-3 (2) NIST 800-53

#### <a name="content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Содержимое записей аудита | Централизованное управление содержимым записей планового аудита

**AU-3 (2)**. Информационная система обеспечивает централизованное управление содержимым для регистрации в записях аудита, созданных [определенными организацией компонентами информационной системы], и их конфигурацию.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Все виртуальные машины, развернутые этой службой Azure Blueprint, присоединены к развернутому домену Active Directory. Все виртуальные машины, присоединенные к домену, реализуют групповую политику, которую можно настроить для централизованного управления конфигурацией системы аудита на уровне операционной системы. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-4"></a>Директива AU-4 NIST 800-53

#### <a name="audit-storage-capacity"></a>Емкость хранилища записей аудита

**AU-4**. Организация выделяет емкость для хранения записей аудита в соответствии с [определенными организацией требованиями к хранилищу записей аудита].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint выделяет достаточную емкость хранилища для хранения записей аудита в течение одного года. Все записи аудита собираются службой Log Analytics, настроенной для хранения этих записей в течение одного года. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-5a"></a>Директива AU-5.a NIST 800-53

#### <a name="response-to-audit-processing-failures"></a>Реагирование на сбои при обработке операций аудита

**AU-5.a**. Информационная система оповещает [определенный организацией персонал или роли] в случае сбоя обработки операции аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Данные о состоянии служб Azure Monitor и Log Analytics доступны на веб-сайте состояния Azure и в колонке работоспособности службы на портале Azure. С помощью Log Analytics можно настроить оповещения для уведомления о других типах сбоев обработки операций аудита. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-5b"></a>Директива AU-5.b NIST 800-53

#### <a name="response-to-audit-processing-failures"></a>Реагирование на сбои при обработке операций аудита

**AU-5.b**. С помощью информационной системы выполняются следующие дополнительные действия: [определенные организацией действия (например, завершение работы информационной системы, перезапись старых записей аудита, прекращение генерации записей аудита)].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Все записи аудита, созданные ресурсами, развернутыми этой службой Azure Blueprint, собираются Log Analytics и хранятся в течение одного года. За счет динамического выделения памяти для этого хранилища записей аудита обеспечивается достаточно емкости. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-5-1"></a>Директива AU-5 (1) NIST 800-53

#### <a name="response-to-audit-processing-failures--audit-storage-capacity"></a>Реагирование на сбои при обработке операций аудита | Емкость хранилища записей аудита

**AU-5 (1)**. Информационная система предупреждает [определенных организацией сотрудников, роли или расположения] в течение[определенного организацией периода времени], когда выделенный объем хранилища для записей аудита достигает [определенного организацией процента] от максимально выделенного объема хранилища для записей аудита в репозитории.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Все записи аудита, созданные ресурсами, развернутыми этой службой Azure Blueprint, собираются Log Analytics и хранятся в течение одного года. За счет динамического выделения памяти для этого хранилища записей аудита обеспечивается достаточно емкости. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-5-2"></a>Директива NIST 800-53 AU-5 (2)

#### <a name="response-to-audit-processing-failures--real-time-alerts"></a>Реагирование на сбои при обработке операций аудита | Оповещения в режиме реального времени

**AU-5 (2)**. Информационная система оповещает в течение [определенного организацией периода времени] [определенных организацией сотрудников, роли или расположения] при возникновении следующих событий сбоя аудита: [определенные организацией события сбоя аудита, требующие оповещений в режиме реального времени].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Данные о состоянии служб Azure доступны в колонке работоспособности службы на портале Azure. С помощью Log Analytics можно настроить оповещения для уведомления о других типах сбоев обработки операций аудита. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-6a"></a>Директива NIST 800-53 AU-6.a

#### <a name="audit-review-analysis-and-reporting"></a>Обзор аудита, его анализ и создание отчетов

**AU-6.a**. Организация проверяет и анализирует записи аудита информационной системы с [определенной организацией частотой] для обнаружения [определенной организацией неуместной или нетипичной активности].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент отвечает за просмотр и анализ записей аудита развертываемых им ресурсов (приложения, операционные системы, базы данных и программное обеспечение). |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-6b"></a>Директива NIST 800-53 AU-6.b

#### <a name="audit-review-analysis-and-reporting"></a>Обзор аудита, его анализ и создание отчетов

**AU-6.b**. Организация предоставляет отчет со сведениями [определяемым организацией сотрудникам или ролям].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент отвечает за предоставление отчета с результатами по неуместной или нетипичной активности (определенными с помощью директивы AU-06.a) в развернутых клиентом ресурсах. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-1"></a>Директива NIST 800-53 AU-6 (1)

#### <a name="audit-review-analysis-and-reporting--process-integration"></a>Обзор аудита, его анализ и создание отчетов | Интеграция процессов

**AU-6 (1)**. Организация использует автоматические механизмы для интеграции проверки аудита, его анализа и процессов отчетности для поддержки корпоративных процессов расследования подозрительных действий и соответствующего на них реагирования.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент может использовать централизованную проверку аудита, его анализ и возможности создания отчетов корпоративного уровня. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-3"></a>Директива NIST 800-53 AU-6 (3)

#### <a name="audit-review-analysis-and-reporting--correlate-audit-repositories"></a>Обзор аудита, анализ и создание отчетов | Сопоставление репозиториев аудита

**AU-6 (3)**. Организация анализирует и сопоставляет записи аудита в разных репозиториях, чтобы получить общую картину ситуации в масштабах всей организации.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует решение Log Analytics в OMS для централизации данных аудита в развернутых ресурсах, поддерживающих получение полных данных о ситуации в масштабах всей организации. Клиенты могут выбрать дальнейшую интеграцию Log Analytics с другими системами. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-4"></a>Директива NIST 800-53 AU-6 (4)

#### <a name="audit-review-analysis-and-reporting--central-review-and-analysis"></a>Обзор аудита, его анализ и создание отчетов | Централизованный обзор и анализ

**AU-6 (4)**. Информационная система предоставляет возможность централизованного обзора и анализа записей аудита из нескольких компонентов в системе.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует решение Log Analytics в OMS для централизации данных аудита на развернутых ресурсах, поддерживающих централизованную проверку, анализ и создание отчетов. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-5"></a>Директива AU-6 (5) NIST 800-53

#### <a name="audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>Обзор аудита, его анализ и создание отчетов | Интеграция, возможности сканирования и мониторинга

**AU-6 (5)**. Организация интегрирует анализ записей аудита с [(один или несколько вариантов): сведениями о сканировании уязвимостей; данными о производительности; сведениями о мониторинге информационной системы; [определенными организацией данными или сведениями, собранными из других источников]] для дальнейшей оптимизации процесса определения неуместной или нетипичной активности.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint развертывает решение "Безопасность и аудит" в OMS. Это решение обеспечивает полное представление о состоянии безопасности. Панель мониторинга "Безопасность и аудит" предоставляет общие сведения о состоянии развернутых ресурсов с использованием данных, доступных в развернутых решениях OMS, а также путем интеграции данных журнала и данных об уязвимостях, полученных на основе базовых показателей и оценки исправлений. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-6"></a>Директива NIST 800-53 AU-6 (6)

#### <a name="audit-review-analysis-and-reporting--correlation-with-physical-monitoring"></a>Обзор аудита, его анализ и создание отчетов | Сопоставление с физическим мониторингом

**AU-6 (6)**. Организация сопоставляет данные из записей аудита с данными, полученными путем мониторинга физического доступа, обеспечивая дальнейшую оптимизацию процесса определения подозрительных, неуместных, нетипичных или злонамеренных действий.

**Обязанности:** только Azure (`Azure Only`)

|||
|---|---|
| **Клиент** | Клиенты не имеют физического доступа к любым ресурсам системы в центрах обработки данных Azure. |
| **Поставщик (Microsoft Azure)** | Команда SIM Microsoft Azure использует связанные с физическим мониторингом данные и сопоставляет их с записями аудита, чтобы определить любое связанное логическое нарушение или подозрительное поведение при обнаружении инцидентов физического доступа. |


 ### <a name="nist-800-53-control-au-6-7"></a>Директива NIST 800-53 AU-6 (7)

#### <a name="audit-review-analysis-and-reporting--permitted-actions"></a>Обзор аудита, его анализ и создание отчетов | Разрешенные действия

**AU-6 (7)**. Организация указывает разрешенные действия для каждого [(один или несколько вариантов): процесса в информационной системе; роли; пользователя], связанного с проверкой, анализом и созданием отчета о результатах аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | В виртуальных машинах Windows, развернутых этой службой Azure Blueprint, реализованы разрешения на уровне операционной системы, ограничивающие действия пользователя с соответствующими данными аудита. В Azure пользователям или группам пользователей можно назначить роли (например, владельца, участника, читателя или другую настраиваемую роль) для ограничения действий, которые можно выполнить с какими-либо ресурсами или развернутыми решениями, включая Log Analytics.  |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-6-10"></a>Директива NIST 800-53 AU-6 (10)

#### <a name="audit-review-analysis-and-reporting--audit-level-adjustment"></a>Обзор аудита, его анализ и создание отчетов | Изменения на уровне аудита

**AU-6 (10)**. Организация регулирует уровень проверки аудита, его анализа и создания отчетов в информационной системе при изменениях риска с учетом сведений правоохранительных органов, аналитики или сведений других достоверных источников информации.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Клиент отвечает за изменение уровня проверки аудита, его анализа и создания отчетов для развернутых им ресурсов (включая приложения, операционные системы, базы данных и программное обеспечение) при изменениях риска с учетом сведений правоохранительных органов, аналитики или сведений других достоверных источников информации. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-7a"></a>Директива NIST 800-53 AU-7.a

#### <a name="audit-reduction-and-report-generation"></a>Сокращение аудита и создание отчетов

**AU-7.a**. Информационная система обеспечивает возможность сокращения аудита и создания отчетов, поддерживающую требования к проверке аудита, его анализу и созданию отчетов по запросу, а также расследование имевших место инцидентов безопасности.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует решение Log Analytics в OMS. Log Analytics предоставляет службы мониторинга для OMS за счет сбора данных из управляемых ресурсов и помещения их в центральный репозиторий. Собранные данные доступны для оповещения, анализа и экспорта. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-7b"></a>Директива NIST 800-53 AU-7.b

#### <a name="audit-reduction-and-report-generation"></a>Сокращение аудита и создание отчетов

**AU-7.b**. Информационная система обеспечивает возможность сокращения аудита и создания отчетов, которая не изменяет исходное содержимое или упорядочение по времени записей аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует решение Log Analytics в OMS. Log Analytics предоставляет службы мониторинга для OMS за счет сбора данных из управляемых ресурсов и помещения их в центральный репозиторий. При выполнении сбора службой Log Analytics содержимое и упорядочение по времени записей аудита не изменяется. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-7-1"></a>Директива NIST 800-53 AU-7 (1)

#### <a name="audit-reduction-and-report-generation--automatic-processing"></a>Сокращение аудита и создание отчетов | Автоматическая обработка

**AU-7 (1)**. Информационная система обеспечивает возможность обработки записей аудита соответствующих событий на основе [определенных организацией полей аудита в записях аудита].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует решение Log Analytics в OMS. Log Analytics предоставляет службы мониторинга для OMS за счет сбора данных из управляемых ресурсов и помещения их в центральный репозиторий. Собранные данные доступны для оповещения, анализа и экспорта. Служба Log Analytics содержит эффективный язык запросов для извлечения данных, хранящихся в репозитории. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-8a"></a>Директива NIST 800-53 AU-8.a

#### <a name="time-stamps"></a>Метки времени

**AU-8.a**. Информационная система использует внутренние системные часы для создания меток времени записей аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Ресурсы, развернутые этой службой Azure Blueprint, используют внутренние системные часы для создания меток времени записей аудита. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-8b"></a>Директива NIST 800-53 AU-8.b

#### <a name="time-stamps"></a>Метки времени

**AU-8.b**. Информационная система записывает метки времени для записей аудита, которые сопоставимы со всемирным координированным временем (UTC) или временем по Гринвичу (GMT) и соответствуют [определенной организацией степени детализации единицы измерения времени].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Ресурсы, развернутые этой службой Azure Blueprint, используют внутренние системные часы для создания меток времени записей аудита. Метки времени записываются в формате UTC. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-8-1a"></a>Директива NIST 800-53 AU-8 (1).a

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Метки времени | Синхронизация с достоверным источником времени

**AU-8 (1).a**. Информационная система сравнивает внутренние информационные системные часы с [определенным организацией достоверным источником времени] с [определенной организацией частотой].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Ресурсы, развернутые этой службой Azure Blueprint, используют внутренние системные часы для создания меток времени записей аудита. Внутренние системные часы настроены на синхронизацию с достоверным источником времени. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-8-1b"></a>Директива NIST 800-53 AU-8 (1).b

#### <a name="time-stamps--synchronization-with-authoritative-time-source"></a>Метки времени | Синхронизация с достоверным источником времени

**AU-8 (1).b**. Информационная система синхронизирует внутренние системные часы с достоверным источником времени, когда разница во времени превышает [определенный организацией период времени].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Ресурсы, развернутые этой службой Azure Blueprint, используют внутренние системные часы для создания меток времени записей аудита. Внутренние системные часы настроены на синхронизацию с достоверным источником времени. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-9"></a>Директива NIST 800-53 AU-9

#### <a name="protection-of-audit-information"></a>Защита сведений аудита

**AU-9**. Информационная система защищает сведения и инструменты аудита от несанкционированного доступа, изменения и удаления.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Элементы управления логическим доступом используются для защиты сведений и инструментов аудита в этой службе Azure Blueprint от несанкционированного доступа, изменения и удаления. Azure Active Directory применяет утвержденную процедуру логического доступа с использованием членства в группах на основе ролей. Возможность просмотра сведений аудита и использования инструментов аудита предоставляется только тем пользователям, которым требуются эти разрешения. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-9-2"></a>Директива NIST 800-53 AU-9 (2)

#### <a name="protection-of-audit-information--audit-backup-on-separate-physical-systems--components"></a>Защита сведений аудита | Резервное копирование сведений аудита в отдельных физических системах или компонентах

**AU-9 (2)**. Информационная система создает резервную копию записей аудита с [определенной организацией частотой] в системе или системном компоненте, физически отличающемся от системы или компонента, подвергнутых аудиту.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует службу Log Analytics в OMS. Развернутые виртуальные машины и учетные записи хранения системы диагностики Azure являются источниками, подключенными к Log Analytics и хранимыми отдельно от источника. Данные собираются OMS в режиме времени, приближенном к реальному. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-9-3"></a>Директива NIST 800-53 AU-9 (3)

#### <a name="protection-of-audit-information--cryptographic-protection"></a>Защита сведений аудита | Криптографическая защита

**AC-9 (3)**. Информационная система применяет механизмы шифрования для защиты целостности сведений и инструментов аудита.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует службу Log Analytics в OMS. Служба Log Analytics обеспечивает надежность источника входящих данных, проверяя сертификаты и целостность данных с помощью аутентификации Azure. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-9-4"></a>Директива NIST 800-53 AU-9 (4)

#### <a name="protection-of-audit-information--access-by-subset-of-privileged-users"></a>Защита сведений аудита | Доступ подмножества привилегированных пользователей

**AU-9 (4)**. Организация разрешает доступ к управлению функциями аудита только [определенному организацией подмножеству привилегированных пользователей].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Элементы управления логическим доступом используются для защиты сведений и инструментов аудита в этой службе Azure Blueprint от несанкционированного доступа, изменения и удаления. Azure Active Directory применяет утвержденную процедуру логического доступа с использованием членства в группах на основе ролей. Возможность просмотра сведений аудита и использования инструментов аудита предоставляется только тем пользователям, которым требуются эти разрешения.
 |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-10"></a>Директива NIST 800-53 AU-10

#### <a name="non-repudiation"></a>Неотрекаемость

**AU-10**. Информационная система защищает среду от пользователей (или процессов от имени таких пользователей), которые злонамеренно отрицают выполнение [определенных организацией действий, охватываемых положением о неотрекаемости].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Возможность аудита для этой службы Azure Blueprint обеспечивается службами Azure Monitor и Log Analytics в OMS. Azure Monitor предоставляет подробные журналы аудита о действиях, связанных с развернутыми ресурсами. Эти журналы, а также журналы на уровне операционной системы собираются службой Log Analytics и сохраняются в репозитории OMS. Они содержат подробные записи о событиях в информационной системе и обеспечивают неподдельность данных. Кроме того, доступ к данным журнала ограничивается с помощью управления доступом на основе ролей для предотвращения несанкционированного изменения или удаления данных журнала. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-11"></a>Директива NIST 800-53 AU-11

#### <a name="audit-record-retention"></a>Хранение записей аудита

**AU-11**. Организация сохраняет записи аудита на [определенный организацией период времени, согласованный с политикой хранения записей] для обеспечения поддержки расследования имевших место инцидентов безопасности и соответствия нормативным и корпоративным требованиям к хранению сведений.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует службу Log Analytics в OMS. Log Analytics предоставляет службы мониторинга для OMS за счет сбора данных из управляемых ресурсов и помещения их в центральный репозиторий. После сбора данные хранятся в течение одного года в соответствии с конфигурацией Log Analytics. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-12a"></a>Директива NIST 800-53 AU-12.a

#### <a name="audit-generation"></a>Создание записей аудита

**AU-12.a**. Информационная система предоставляет возможность создания записей аудита для событий аудита, определенных в директиве AU-2 a. в [определенных организацией компонентах информационной системы].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | В рамках этого принципа Azure отслеживает события, собираемые журналами действий для развернутых ресурсов, журналами уровня ОС, журналами Active Directory и SQL Server. Клиенты могут выбирать дополнительные события для аудита в зависимости от требований. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-12b"></a>Директива AU-12.b NIST 800-53

#### <a name="audit-generation"></a>Создание записей аудита

**AU-12.b**. Информационная система позволяет [определенным организацией сотрудникам или ролям] выбрать события для аудита определенными компонентами информационной системы.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Доступ к функциям аудита ограничивается с помощью управления доступом на основе ролей в Azure, а также на уровне операционной системы виртуальной машины. Пользователи с соответствующей авторизацией на основе ролей могут настроить конфигурацию событий, выбранных для аудита ресурсами, развернутыми этой службой Azure Blueprint. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ## <a name="nist-800-53-control-au-12c"></a>Директива NIST 800-53 AU-12.c

#### <a name="audit-generation"></a>Создание записей аудита

**AU-12.c**. Информационная система создает записи аудита для событий, определенных в директиве AU-2.d. с содержимым, определенным в директиве AU-3.

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | В рамках этого принципа Azure отслеживает события, собираемые журналами действий для развернутых ресурсов, журналами уровня ОС, журналами Active Directory и SQL Server. Клиенты могут выбирать дополнительные события для аудита в зависимости от требований. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-12-1"></a>Директива AU-12 (1) NIST 800-53

#### <a name="audit-generation--system-wide--time-correlated-audit-trail"></a>Создание аудита | Журнал аудита во всей системе с сопоставлением по времени

**AU-12 (1)**. Информационная система объединяет записи аудита из [определенных организацией системных компонентов] в системный журнал аудита (логический или физический), сопоставляемый по времени с [определенным организацией уровнем отказоустойчивости взаимосвязи между метками времени отдельных записей в журнале аудита].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Эта служба Azure Blueprint реализует службу Log Analytics в OMS. Log Analytics предоставляет службы мониторинга для OMS за счет сбора данных из управляемых ресурсов и помещения их в центральный репозиторий. Метки времени записей аудита не изменяются, поэтому журнал аудита сопоставляется по времени. |
| **Поставщик (Microsoft Azure)** | Не применяется |


 ### <a name="nist-800-53-control-au-12-3"></a>Директива NIST 800-53 AU-12 (3)

#### <a name="audit-generation--changes-by-authorized-individuals"></a>Создание записей аудита | Изменения авторизованными пользователями

**AU-12 (3)**. Информационная система обеспечивает возможность изменения аудита для [определенных организацией лиц или ролей] в [определенных организацией компонентах информационной системы] на основе [определенного организацией критерия для выбираемых событий] в [пределах определенных организацией пороговых значений времени].

**Обязанности:** только клиент (`Customer Only`)

|||
|---|---|
| **Клиент** | Доступ к функциям аудита ограничивается с помощью управления доступом на основе ролей в Azure, а также на уровне операционной системы виртуальной машины. Пользователи с соответствующей авторизацией на основе ролей могут настроить конфигурацию событий, выбранных для аудита ресурсами, развернутыми этой службой Azure Blueprint. |
| **Поставщик (Microsoft Azure)** | Не применяется |
