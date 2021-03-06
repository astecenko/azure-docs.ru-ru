---
title: "Планировщик ресурсов Azure Site Recovery для сценариев развертывания виртуальных машин VMware в Azure | Документация Майкрософт"
description: "Руководство по использованию планировщика ресурсов Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: 36309eb85244435a853013448c83d125420c001c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2017
---
# <a name="cost-estimation-report-of-azure-site-recovery-deployment-planner"></a>Отчет об оценке затрат, созданный в планировщике развертывания Azure Site Recovery  

На страницах отчета [Рекомендации](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations) содержится сводка оценки затрат, а на странице оценки затрат — данные их подробного анализа. для каждой виртуальной машины. 

### <a name="cost-estimation-summary"></a>Результаты оценки затрат 
На следующих диаграммах представлена сводка общих затрат на аварийное восстановление (DR) в Azure для выбранного целевого региона и валюты, которые вы указали при создании отчета:
Результаты оценки затрат

![Результаты оценки затрат](media/site-recovery-vmware-deployment-planner-analyze-report/cost-estimation-summary-v2a.png)

Сводка поможет вам составить представление о затратах на хранилище, вычисления, сеть и лицензии при защите всех совместимых виртуальных машин в Azure с помощью Azure Site Recovery. Затраты рассчитываются только для совместимых, а не для всех профилируемых виртуальных машин.  
 
Можно просмотреть расходы за месяц или за год. Узнайте больше о поддерживаемых [целевых регионах](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-target-regions) и [валютах](./site-recovery-vmware-deployment-planner-cost-estimation.md#supported-currencies).

**Cost by components** (Затраты по составляющим). Расходы на аварийное восстановление разделены на четыре составляющие: стоимость вычислительных ресурсов, хранилища, сети и лицензии Azure Site Recovery. Затраты рассчитываются на основе данных потребления при репликации и тестировании DR. Сюда входит стоимость вычислительных ресурсов, хранилища (классы Premium и Standard), подключения ExpressRoute или VPN, настроенного между локальной средой и Azure, а также лицензии Azure Site Recovery.

**Cost by states** (Затраты по состояниям). Общие расходы на аварийное восстановление (DR) рассчитываются на основе двух состояний: репликации и тестирования DR. 

**Replication cost** (Затраты на репликацию). Стоимость репликации. Сюда входит стоимость хранилища, сети и лицензии Azure Site Recovery. 

**DR-Drill cost** (Затраты на тестирование DR). Стоимость тестовой отработки отказа. Azure Site Recovery запускает виртуальные машины при тестовой отработке отказа. Затраты на тестирование DR включают расходы на вычислительные ресурсы и хранилище для работающих виртуальных машин. 

**Azure storage cost per Month/Year** (Месячные или годовые затраты на хранилище). Это общие расходы на хранилище классов Premium и Standard при репликации и тестировании DR.

## <a name="detailed-cost-analysis"></a>Подробный анализ затрат
Цены Azure на использование вычислительных ресурсов, хранилища и т. д. зависят от региона. Вы можете создать отчет об оценке затрат с последними ценами Azure на основе вашей подписки, предложения, связанного с подпиской, а также для указанного целевого региона Azure в указанной валюте. По умолчанию в программе указаны регион Azure "Западная часть США 2" и доллар США. Если вы использовали другие регион и валюту, при следующем создании отчета об оценке затрат без указания идентификатора подписки, идентификатора предложения, целевого региона и валюты будут использоваться цены для последнего указанного целевого региона и последняя указанная валюта.
В этом разделе отображаются идентификаторы подписки и предложения, которые вы использовали при создании отчета.  Если эти данные не указаны, раздел будет пустым.

Ячейки отчета серого цвета доступны только для чтения. Ячейки белого цвета можно изменить в соответствии с вашими требованиями.

![Данные оценки затрат1](media/site-recovery-hyper-v-deployment-planner-cost-estimation/cost-estimation1-h2a.png)

### <a name="overall-dr-cost-by-components"></a>Общие затраты на DR по составляющим
В первом разделе показаны общие затраты на DR по составляющим и состояниям. 

**Compute** (Вычислительные ресурсы). Затраты на виртуальные машины IaaS, которые работают в Azure для аварийного восстановления. Сюда входят виртуальные машины, которые создаются в службе Azure Site Recovery при тестировании DR (тестовой отработке отказа) и виртуальные машины, которые работают в Azure, например SQL Server с группами доступности AlwaysOn, а также контроллеры домена и DNS-серверы.

**Хранилище.** Затраты на использование хранилища Azure для аварийного восстановления. Сюда входят расходы на использование хранилища для репликации и во время тестирования DR.
Сеть. Затраты на подключение ExpressRoute и VPN-подключение "сеть — сеть" для аварийного восстановления. 

**ASR license** (Лицензия ASR). Затраты на лицензию Azure Site Recovery для всех совместимых виртуальных машин. Если вы вручную ввели данные виртуальной машины в таблицу подробного анализа затрат, стоимость лицензии Azure Site Recovery для этой виртуальной машины также включена.

### <a name="overall-dr-cost-by-states"></a>Общие затраты на DR по состояниям
Общие затраты на аварийное восстановление рассчитываются на основе двух состояний: репликации и тестирования DR.

**Replication cost** (Затраты на репликацию). Стоимость репликации. Сюда входит стоимость хранилища, сети и лицензии Azure Site Recovery. 

**DR-Drill cost** (Затраты на тестирование DR). Стоимость тестирования аварийного восстановления. При тестировании DR в Azure Site Recovery запускаются виртуальные машины. Затраты на тестирование DR охватывают расходы на вычислительные ресурсы и хранилище для работающих виртуальных машин.
Общая годовая длительность тестирования DR = число операций тестирования DR x длительность каждой операции тестирования DR (в днях). Средние месячные затраты на тестирование DR = общие затраты на тестирование DR / 12.

### <a name="storage-cost-table"></a>Таблица затрат на хранилище
В этой таблице представлены затраты на хранилище классов Premium и Standard при репликации и тестировании DR со скидкой и без нее.

### <a name="site-to-azure-network"></a>Подключение "сеть — Azure"
Выберите соответствующий параметр согласно вашим требованиям. 

**ExpressRoute.** По умолчанию в программе выбирается ближайший план ExpressRoute, который соответствует пропускной способности сети, необходимой для разностной репликации. Вы можете изменить план в соответствии со своими требованиями.

**VPN-шлюз.** Выберите VPN-шлюз, если он есть в вашей среде. По умолчанию для него установлено значение "Не доступен".

**Целевой регион.** Указанный регион Azure для аварийного восстановления. Цена, используемая в отчете, рассчитывается для вычислительных ресурсов, хранилища, сети и лицензии на основе цен Azure для этого региона. 

### <a name="vm-running-on-azure"></a>Виртуальная машина, работающая в Azure
Если вы используете для DR контроллер домена, виртуальную машину DNS или виртуальную машину SQL Server с группами доступности Always On в Azure, вы можете указать число и размер виртуальных машин, чтобы расходы на вычислительные ресурсы для них включались в общие затраты на DR. 

### <a name="apply-overall-discount-if-applicable"></a>Применение общей скидки (при наличии)
Если вы являетесь партнером или клиентом Azure и имеете право на какую-либо скидку для всех цен Azure, можете использовать это поле. В средстве применяется скидка (в %) для всех составляющих.

### <a name="number-of-virtual-machines-type-and-compute-cost-per-year"></a>Количество типов виртуальных машин и годовые затраты на вычислительные ресурсы
В этой таблице представлено количество виртуальных машин с Windows и другими ОС и затраты на вычислительные ресурсы при тестировании DR для них.

### <a name="settings"></a>данных 
**Using managed disk** (Использование управляемого диска). Здесь указано, используется ли при тестировании DR управляемый диск. Значение по умолчанию — "Да". Если вы установили для параметра -UseManagedDisks значение "Нет", для расчета затрат используется цена на неуправляемый диск.

**Currency** (Валюта). Валюта, которая используется при создании отчета. Период для расчета затрат. Вы можете просмотреть все затраты за месяц или год. 

## <a name="detailed-cost-analysis-table"></a>Таблица подробного анализа затрат
![Подробный анализ затрат.](media/site-recovery-hyper-v-deployment-planner-cost-estimation/detailed-cost-analysis-h2a.png) В этой таблице перечислены отдельные затраты для каждой совместимой виртуальной машины. Кроме того, с ее помощью можно узнать оценочные затраты на DR в Azure для непрофилируемых виртуальных машин, добавив их данные вручную. Это полезно, если нужно оценить затраты в Azure на новое развертывание аварийного восстановления без подробного профилирования.
Чтобы добавить данные виртуальных машин вручную, сделайте следующее: 
1.  Нажмите кнопку "Вставить строку", чтобы вставить новую строку между строками "Начало" и "Конец".

2.  Укажите значения в следующих столбцах на основе приблизительного размера и числа виртуальных машин, которые соответствуют этой конфигурации: 

* число виртуальных машин, размер IaaS (по вашему выбору);
* тип хранилища (Standard или Premium);
* общий размер хранилища виртуальной машины (ГБ);
* число операций тестирования DR в год; 
* длительность каждой операции тестирования DR (в днях); 
* тип ОС;
* Избыточность данных 
* Программа преимуществ гибридного использования Azure

3.  Вы можете применить общее значение для всех виртуальных машин в таблице. Для этого нажмите кнопку Apply to all (Применить ко всем) для годового количества операций тестирования DR, длительности каждой такой операции (в днях), избыточности данных и преимущества гибридного использования Azure.

4.  Щелкните Re-calculate cost (Повторно рассчитать затраты), чтобы обновить сумму.

**Имя виртуальной машины.** Имя виртуальной машины.

**Number of VMs** (Число виртуальных машин). Количество виртуальных машин, которые соответствуют конфигурации. Можно обновить количество существующих виртуальных машин, если виртуальные машины аналогичной конфигурации не профилируются, но будут защищены.

**IaaS size (Recommendation)** (Размер IaaS (рекомендации)). Размер роли совместимой виртуальной машины, рекомендуемый в программе. 

**IaaS size (Your selection)** (Размер IaaS (по вашему выбору)). По умолчанию устанавливается то же значение, что и для рекомендуемого размера. Роль можно изменить в соответствии с требованиями. Затраты на вычислительные ресурсы зависят от выбранного размера роли виртуальной машины.

**Storage type** (Тип хранилища). Тип хранилища, который используется для виртуальной машины. Вы можете использовать хранилище класса Standard или Premium.

**VM total storage size (GB)** (Общий размер хранилища виртуальной машины (ГБ)). Общий объем хранилища для виртуальной машины.

**Number of DR-Drills in a year** (Годовое число операций тестирования DR). Сколько раз в год выполняется тестирование аварийного восстановления. Значение по умолчанию — 4 раза в год. Можно изменить число для отдельных виртуальных машин или применить новое значение ко всем виртуальным машинам. Для этого введите новое значение в верхней строке и нажмите кнопку Apply to all (Применить ко всем). На основе годового количества операций тестирования DR и длительности каждой такой операции рассчитываются общие затраты на тестирование аварийного восстановления.  

**Each DR-Drill duration (Days)** (Длительность каждой операции тестирования DR (в днях)). Продолжительность каждой операции тестирования аварийного восстановления. По умолчанию она составляет 7 дней через каждые 90 дней согласно условиям предоставления [преимущества Software Assurance для аварийного восстановления](https://azure.microsoft.com/en-in/pricing/details/site-recovery). Можно изменить число для отдельных виртуальных машин или применить новое значение ко всем виртуальным машинам. Для этого введите новое значение в верхней строке и нажмите кнопку Apply to all (Применить ко всем). Общие затраты на тестирование DR рассчитываются на основе годового количества операций тестирования DR и длительности каждой такой операции.
  
**Тип ОС.** Тип операционной системы виртуальной машины. Это может быть Windows или Linux. Если тип операционной системы — Windows, к этой виртуальной машине можно применить программу "Преимущество гибридного использования Azure". 

**Data redundancy** (Избыточность данных). Допустимы следующие значения: локально избыточное хранилище (LRS), геоизбыточное хранилище (GRS) и геоизбыточное хранилище с доступом на чтение (RA-GRS). Значение по умолчанию — LRS. Можно изменить тип определенных виртуальных машин на основе вашей учетной записи хранения или применить новый тип для всех виртуальных машин. Для этого измените тип в верхней строке и нажмите кнопку Apply to all (Применить ко всем).  Затраты на хранилище для репликации рассчитываются на основе стоимости выбранного варианта избыточности данных. 

**Преимущество гибридного использования Azure.** Вы можете использовать преимущество гибридного использования Azure для виртуальных машин Windows, если это применимо.  Значение по умолчанию — "Да". Можно изменить параметр для определенных виртуальных машин или обновить все виртуальные машины. Для этого нажмите кнопку Apply to all (Применить ко всем).

**Total Azure consumption** (Общее использование ресурсов Azure). Сюда входят затраты на вычислительные ресурсы, хранилище и лицензию Azure Site Recovery для аварийного восстановления. В зависимости от выбранного варианта выводятся затраты за месяц или за год.

**Steady state replication cost** (Затраты на репликацию для устойчивого состояния). Сюда входят расходы на хранилище для репликации.

**Total DR-Drill cost (average)** (Общие затраты на тестирование DR (среднее значение)) Сюда входят затраты на вычислительные ресурсы и хранилище для тестирования аварийного восстановления.

**ASR license cost** (Затраты на лицензию ASR). Стоимость лицензии Azure Site Recovery.

## <a name="supported-target-regions"></a>Поддерживаемые целевые регионы
Планировщик развертывания Azure Site Recovery обеспечивает оценку затрат для перечисленных ниже регионов Azure. Если ваш регион не указан в списке, можно использовать любой из следующих регионов, цены для которого наиболее приближены к ценам для вашего:

eastus, eastus2, westus, centralus, northcentralus, southcentralus, northeurope, westeurope, eastasia, southeastasia, japaneast, japanwest, australiaeast, australiasoutheast, brazilsouth, southindia, centralindia, westindia, canadacentral, canadaeast, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth. 

## <a name="supported-currencies"></a>Поддерживаемые валюты
При помощи планировщика развертывания Azure Site Recovery можно создать отчет с использованием любой из следующих валют:

|Валюта|Имя||Валюта|Имя||Валюта|Имя|
|---|---|---|---|---|---|---|---|
|ARS|Аргентинское песо ($)||AUD|Австралийский доллар ($)||BRL|Бразильский реал (R$)|
|CAD|Канадский доллар ($)||CHF|Швейцарский франк (chf)||DKK|Датская крона (kr)|
|EUR|Евро (€)||GBP|Британский фунт (£)||HKD|Гонконгский доллар (HK$)|
|IDR|Индонезийская рупия (Rp)||INR|Индийская рупия (₹)||JPY|Японская йена (¥)|
|KRW|Корейская вона (₩)||MXN|Мексиканское песо (MX$)||MYR|Малайзийский ринггит (RM$)|
|NOK|Норвежская крона (kr)||NZD|Новозеландский доллар ($)||RUB|Российский рубль (руб)|
|SAR|Саудовский риял (SR)||SEK|Шведская крона (kr)||TWD|Тайваньский доллар (NT$)|
|TRY|Турецкая лира (TL)||USD| Доллар США ($)||ZAR|Южноафриканский рэнд (R)|

## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь с дополнительными сведениями о защите [виртуальных машин VMware в Azure с помощью Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-vmware-to-azure).