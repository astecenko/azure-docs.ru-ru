---
title: "Сокращение затрат с помощью предварительной оплаты виртуальных машин Azure | Документация Майкрософт"
description: "Сведения о зарезервированных экземплярах виртуальной машины Azure, которые помогут вам снизить затраты на виртуальные машины."
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2017
ms.author: vikdesai
ms.openlocfilehash: 96e9cf2fed0b22fd7aa7b9ffeab0e94738ce510d
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2017
---
# <a name="save-money-on-virtual-machines-with-reserved-virtual-machine-instances"></a>Сокращение затрат с помощью зарезервированных экземпляров виртуальных машин Azure 
Зарезервированные экземпляры виртуальных машин позволяют оплатить использование вычислительной мощности на один или три года вперед, чтобы получить скидку на виртуальные машины. 1-летние и 3-летние контракты с предварительной оплатой позволят сэкономить существенную сумму (до 72 %) по сравнению с оплатой виртуальных машин по мере использования. Зарезервированные экземпляры виртуальных машин просто предоставляют скидку и никак не влияют на состояние среды выполнения этих виртуальных машин.

Вы можете приобрести зарезервированный экземпляр виртуальной машины на [портале Azure](https://aka.ms/reservations). Дополнительные сведения см. в статье [Prepay for Virtual Machines with Reserved VM Instances](https://go.microsoft.com/fwlink/?linkid=861721) (Предоплата виртуальных машин и экономия при использовании зарезервированных экземпляров виртуальных машин).

## <a name="why-should-i-buy-a-reserved-virtual-machine-instance"></a>Для чего нужны зарезервированные экземпляры виртуальных машин?
Если у вас есть виртуальные машины, которые работают длительное время, приобретение зарезервированного экземпляра виртуальной машины позволит вам получить самую низкую цену на них. Например, если в регионе "западная часть США" постоянно работают четыре экземпляра уровня "Стандартный" D2, то без резервирования за них взимается обычная оплата по мере использования. Если вы приобретете зарезервированный экземпляр виртуальной машины для каждой из этих четырех виртуальных машин, вы немедленно получите скидку на них при выставлении счетов. Теперь к ним не будут применяться ставки оплаты по мере использования. 

## <a name="what-charges-does-a-reserved-virtual-machine-instance-cover"></a>Какие расходы покрывает зарезервированный экземпляр виртуальной машины?
Резервирование относится только к оплате инфраструктуры виртуальной машины Windows или Linux. Резервирование не включает оплату дополнительного программного обеспечения, сетей и хранилищ. Если вы используете виртуальные машины Windows, расходы на лицензирование вам поможет сократить [Преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-virtual-machine-instance"></a>Кто имеет право приобретать зарезервированный экземпляр виртуальной машины?
Зарезервированный экземпляр виртуальной машины доступен для пользователей Azure со следующими типами подписок:
-   тип предложения подписки "Enterprise Agreement" (MS-AZR-0017P);
-   тип предложения подписки [с оплатой по мере использования](https://azure.microsoft.com/offers/ms-azr-0003p/) (MS-AZR-003P).
Для приобретения зарезервированного экземпляра вы должна быть назначена роль владельца подписки. Чтобы приобрести резервирование для подписки Enterprise Agreement, администратор предприятия должен разрешить покупку резервирований на портале EA. По умолчанию эта возможность включена.

## <a name="how-is-a-reserved-virtual-machine-instances-purchase-billed"></a>Как оплачиваются зарезервированные экземпляры виртуальных машин?
Для покупки резервирований используется метод оплаты, привязанный к подписке. Если вы используете подписку Enterprise Agreement, стоимость резервирования вычитается из баланса денежных обязательств. Если у вас нет достаточного баланса денежных обязательств для покрытия полной стоимости резервирования, вам выставляется счет на соответствующую разницу.
Если вы используете подписку с оплатой по мере использования, нужная сумма немедленно снимается с кредитной карты, которая подключена к учетной записи. Если вы используете оплату по накладным, эта сумма будет включена в ближайшую накладную.

## <a name="how-is-the-purchased-reserved-virtual-machine-instance-discount-applied"></a>Как применяются скидки для зарезервированного экземпляра виртуальной машины?
Скидка для зарезервированного экземпляра виртуальной машины применяется к виртуальным машинам, которые соответствуют атрибутам, выбранным при покупке резервирования. Атрибуты включают в себя область, в которой выполняются виртуальные машины. Например, если вы хотите использовать зарезервированный экземпляр виртуальной машины для получения скидки на четыре виртуальные машины категории "Стандартный" D2 в регионе "западная часть США", выберите ту подписку, в которой работают эти виртуальные машины. Если виртуальные машины работают в разных подписках в одной учетной записи, выберите вариант совместного использования. Совместное использование позволяет применить скидки резервирования сразу в нескольких подписках.
Область зарезервированного экземпляра виртуальной машины можно изменить и после покупки. Соответствующая процедура описана в документации по управлению резервированиями.

Скидка резервирования применяется только к виртуальным машинам, размещенным в подписках Enterprise Agreement или с оплатой по мере использования. Виртуальные машины, работающие в подписке с любым другим типом, не могут воспользоваться скидкой резервирования. Из числа корпоративных подписок преимущества зарезервированного экземпляра недоступны для подписок "Разработка и тестирование Enterprise".

Влияние резервирования на оплату виртуальных машин подробно описано [в этой статье](https://go.microsoft.com/fwlink/?linkid=863405).

## <a name="what-happens-when-the-reservation-term-expires"></a>Что происходит, когда завершается срок резервирования?
При завершении срока резервирования скидка на оплату отменяется, и для виртуальной машины снова применяется оплата по мере использования. Резервирования не возобновляются автоматически. Чтобы сохранить эту скидку на оплату, необходимо приобрести новый зарезервированный экземпляр виртуальной машины. 

## <a name="sizes-and-regional-availability"></a>Доступность для размеров и регионов
Резервирования доступны почти для всех размеров виртуальных машин за некоторыми исключениями.
- Размеры виртуальных машин предварительной версии. Все размеры, которые находятся в предварительной версии, недоступны для приобретения зарезервированного экземпляра виртуальной машины.
- Облака. Зарезервированные экземпляры виртуальных машин недоступны для приобретения в этих регионах: государственные организации США, Германия и Китай. 
- Недостаточная квота. Если зарезервированный экземпляр виртуальной машины привязан к одной конкретной подписке, эта подписка должна иметь свободную квоту виртуальных процессоров для размещения этого зарезервированного экземпляра. Например, если целевая подписка имеет квоту в 10 виртуальных процессоров для семейства серии D, вы не сможете приобрести зарезервированный экземпляр виртуальной машины для 11 экземпляров Standard_D1. При проверке квоты для резервирования учитываются все виртуальные машины, уже размещенные в подписке. Предположим, что подписка имеет квоту в 10 виртуальных процессоров для семейства серии D. Если в этой подписке уже существуют два экземпляра категории Standard_D1, вы сможете приобрести для нее зарезервированный экземпляр виртуальной машины для 10 экземпляров Standard_D1. 
- Ограничения емкости. В редких случаях Azure может ограничить приобретение новых резервирований для некоторых размеров виртуальных машин из-за нехватки емкости в соответствующем регионе.

## <a name="next-steps"></a>Дальнейшие действия
Снижайте расходы на виртуальные машины, приобретая [зарезервированный экземпляр виртуальной машины](https://go.microsoft.com/fwlink/?linkid=861721). 

Если вам нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.
