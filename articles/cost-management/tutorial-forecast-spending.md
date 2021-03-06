---
title: "Прогнозирование затрат с помощью службы \"Управление затратами Azure\" | Документация Майкрософт"
description: "Прогнозирование затрат с помощью исторических данных о потреблении и затратах."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: d8b0cd2a3e5f9829f0844783aad22d375eb9d7a8
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2017
---
# <a name="forecast-future-spending"></a>Прогнозирование будущих затрат

Служба "Управление затратами Azure" от Cloudyn помогает прогнозировать будущие затраты, используя исторические данные о потреблении и затратах. В отчетах Cloudyn можно просмотреть все данные о прогнозировании затрат. В примерах этого руководства рассматривается прогнозирование затрат с использованием отчетов. Из этого руководства вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Прогнозирование будущих затрат.

## <a name="forecast-future-spending"></a>Прогнозирование будущих затрат

Cloudyn включает в себя отчеты о прогнозировании затрат, помогающие предсказывать расходы на основе потребления с течением времени. Основное их назначение — гарантировать, что тенденции расходов не будут превышать ожидания вашей организации. Используемые отчеты — Current Month Projected Cost (Прогнозируемые затраты на текущий месяц) и Annual Projected Cost (Прогнозируемые затраты на год). В обоих отчетах отображаются прогнозируемые будущие затраты, если потребление сопоставимо с последними 30 днями потребления.

В отчете Current Month Projected Cost (Прогнозируемые затраты на текущий месяц) отображается стоимость используемых служб. Для отображения прогнозируемых затрат используются затраты с начала месяца и за предыдущий месяц. В меню отчетов в верхней части портала выберите **Cost (Затраты)** > **Projection and Budget (Прогнозирование и бюджет)** > **Current Month Projected Cost (Прогнозируемые затраты на текущий месяц)**. Пример приведен на следующем рисунке.

![Прогнозируемые затраты на текущий месяц](./media/tutorial-forecast-spending/project-month01.png)

В примере вы увидите, какие службы создают больше всего затрат. Затраты на Azure были ниже, чем на AWS. Если нужно просмотреть подробные сведения о прогнозируемых затратах на виртуальные машины Azure, в списке **Filter** (Фильтр) выберите **Azure/VM** (Azure/виртуальная машина).

![Прогнозируемые затраты на текущий месяц для виртуальной машины Azure](./media/tutorial-forecast-spending/project-month02.png)

Выполните те же базовые шаги (см. выше) для отображения ежемесячных прогнозируемых затрат на другие интересующие вас службы.

Отчет Annual Projected Cost (Прогнозируемые затраты на год) показывает экстраполированные затраты на ваши службы на следующие 12 месяцев.

В меню отчетов в верхней части портала выберите **Cost (Затраты)** > **Projection and Budget (Прогнозирование и бюджет)** > **Annual Projected Cost (Прогнозируемые затраты на год)**. Пример приведен на следующем рисунке.

![Отчет о прогнозируемых затратах на год](./media/tutorial-forecast-spending/project-annual01.png)

В примере вы увидите, какие службы создают больше всего затрат. Как и в примере за месяц, расходы на Azure были ниже, чем на AWS. Если нужно просмотреть подробные сведения о прогнозируемых затратах на виртуальные машины Azure, в списке **Filter** (Фильтр) выберите **Azure/VM** (Azure/виртуальная машина).

![Прогнозируемые затраты на год на виртуальные машины](./media/tutorial-forecast-spending/project-annual02.png)

На приведенном выше рисунке прогнозируемые затраты на год на виртуальные машины Azure составляют 28 374 $.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * Прогнозирование будущих затрат.


Перейдите к следующему руководству, чтобы научиться управлять затратами с помощью отчетов о распределении затрат и виртуальных счетах.

> [!div class="nextstepaction"]
> [Управление затратами с использованием отчетов о распределении затрат и виртуальных счетах](tutorial-manage-costs.md)
