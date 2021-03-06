---
title: "Получение данных об установленном на компьютерах программном обеспечении с помощью службы автоматизации Azure | Документация Майкрософт"
description: "Использование данных инвентаризации для определения установленного программного обеспечения на компьютерах в вашей среде."
services: automation
keywords: "данные инвентаризации, автоматизация, отслеживание"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: tutorial
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: bdd638d0612a8ddee1a0ddb4fd4579f8da14b887
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2018
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Получение данных об установленном программном обеспечении на виртуальных машинах Azure и других компьютерах

Из этого руководства вы узнаете, как получить сведения об установленном в среде программном обеспечении. Вы можете собирать и просматривать сведения об установленных на компьютерах программном обеспечении, файлах, управляющих программах, службах и разделах реестра Windows. Отслеживание конфигураций поможет вам локализовать операционные проблемы в любом сегменте среды и получить сведения о текущем состоянии компьютеров.

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * подключить виртуальную машину к решениям для отслеживания изменений и инвентаризации;
> * просмотреть установленное программное обеспечение;
> * найти установленное программное обеспечение в журналах инвентаризации.

## <a name="prerequisites"></a>Необходимые компоненты

Для работы с этим учебником необходимы указанные ниже компоненты.

* Подписка Azure. Если у вас ее нет, [активируйте преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) или [зарегистрируйте бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Учетная запись службы автоматизации](automation-offering-get-started.md) для хранения runbook наблюдателя, runbook действия и задачи наблюдателя.
* [Виртуальная машина](../virtual-machines/windows/quick-create-portal.md) для подключения.

## <a name="log-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу http://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Включение службы отслеживания изменений и инвентаризации

Чтобы выполнить задачи в сначала нужно настроить для виртуальной машины службу отслеживания изменений и инвентаризации. Если вы ранее настроили для этой виртуальной машины другую систему автоматизации, этот шаг можно пропустить.

1. В меню слева выберите **Виртуальные машины**, а затем выберите виртуальную машину в списке.
2. В меню слева в разделе **Операции** выберите **Инвентаризация**. Откроется страница **Включение отслеживания изменений и инвентаризации**.

Проверка выполняется, чтобы определить, включена ли инвентаризация для этой виртуальной машины.
При этом проверяется рабочее пространство Log Analytics и связанная учетная запись службы автоматизации, а также наличие решения в рабочей области.

Рабочая область [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) используется для сбора данных, созданных такими компонентами и службами, как "Инвентаризация".
Рабочая область предоставляет единое расположение для проверки и анализа данных из нескольких источников.

В процессе проверки также проверяется подготовка виртуальной машины с помощью Microsoft Monitoring Agent (MMA) и гибридной рабочей роли.
Этот агент позволяет взаимодействовать с виртуальной машиной и получать сведения об установленном программном обеспечении.
В процессе проверки также определяется, была ли виртуальная машина подготовлена с помощью Microsoft Monitoring Agent (MMA) и гибридной рабочей роли службы автоматизации.

Если предварительные условия не выполнены, появится баннер, с помощью которого можно будет включить решение.

![Баннер конфигурации для включения инвентаризации](./media/automation-tutorial-installed-software/enableinventory.png)

Щелкните этот баннер, чтобы включить решение.
Если проверка выявит, что отсутствует один из следующих компонентов, он будет автоматически добавлен:

* [Рабочая область Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json).
* [Автоматизация](./automation-offering-get-started.md)
* [Гибридная рабочая роль runbook](./automation-hybrid-runbook-worker.md) включена на виртуальной машине.

Теперь откроется экран **Отслеживание изменений и инвентаризация**. Настройте расположение, рабочую область Log Analytics и учетную запись службы автоматизации, затем нажмите кнопку **Включить**. Если эти поля неактивны и выделены серым цветом, то для этой виртуальной машины настроено другое решение автоматизации, а значит необходимо использовать ту же рабочую область и ту же учетную запись службы автоматизации.

![Окно включения службы отслеживания изменений](./media/automation-tutorial-installed-software/installed-software-enable.png)

Включение решения может занять до 15 минут. В это время не закрывайте окно браузера.
Когда решение будет включено, сведения об установленном программном обеспечении и изменениях на виртуальной машине начнут передаваться в Log Analytics.
На получение данных для анализа может уйти от 30 минут до 6 часов.

## <a name="view-installed-software"></a>просмотреть установленное программное обеспечение;

Когда вы включите решение для отслеживания изменений и инвентаризации, собранные данные вы сможете увидеть на странице **Инвентаризация**.

На виртуальной машине выберите **Инвентаризация** в разделе **Операции**.

На странице **Инвентаризация** щелкните вкладку **Программное обеспечение**.

На вкладке **Программное обеспечение** вы найдете таблицу со списком обнаруженного программного обеспечения. Программное обеспечение группируется по именам и версиям.

В таблице предоставляются обобщенные сведения о каждой строке с информацией о программном обеспечении. Эти сведения включают имя программного обеспечения, версию, издателя, время последнего обновления (для компьютера в этой группе) и число компьютеров (на которых установлено это программное обеспечение).

![Инвентаризация программного обеспечения](./media/automation-tutorial-installed-software/inventory-software.png)

Щелкните любую строку, чтобы просмотреть свойства программного обеспечения в этой строке и список имен компьютеров, на которых оно установлено.

Вы можете найти нужные программное обеспечение или группу, введя строку для поиска в текстовое поле прямо над списком программного обеспечения.
С помощью фильтра можно выполнять поиск по названию, версии и (или) издателю программного обеспечения.

Например, поиск по строке Contoso возвращает список программного обеспечения, у которого название, имя издателя или версия содержат подстроку Contoso.

## <a name="search-inventory-logs-for-installed-software"></a>Поиск установленного программного обеспечения в журналах инвентаризации

Служба инвентаризации сохраняет данные в журнале и отправляет их в Log Analytics. Чтобы выполнить поиск по этим журналам, выберите **Log Analytics** в верхней части окна **Инвентаризация**.

Данные об инвентаризации хранятся в типе **ConfigurationData**.
Ниже приводится пример запроса Log Analytics, который возвращает всех издателей, в имени которых содержится подстрока Microsoft, и число записей программного обеспечения (с группировкой по полям SoftwareName и Computer) для каждого издателя.

```
ConfigurationData
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
| where ConfigDataType == "Software"
| search Publisher:"Microsoft"
| summarize count() by Publisher
```

Дополнительные сведения о поиске по файлам журналов в Log Analytics см. в статье [Что такое Log Analytics?](https://docs.loganalytics.io/index)

### <a name="single-machine-inventory"></a>Инвентаризация одного компьютера

Чтобы просмотреть результаты инвентаризации программного обеспечения для одного компьютера, откройте на странице ресурсов виртуальной машины Azure сведения об инвентаризации или примените в Log Analytics фильтр по конкретному компьютеру. Ниже приводится пример запроса Log Analytics, который возвращает список программного обеспечения на компьютере с именем ContosoVM.

```
ConfigurationData
| where ConfigDataType == "Software" 
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
```

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как использовать решение для инвентаризации программного обеспечения и как выполнить следующие действия:

> [!div class="checklist"]
> * подключить виртуальную машину к решениям для отслеживания изменений и инвентаризации;
> * просмотреть установленное программное обеспечение;
> * найти установленное программное обеспечение в журналах инвентаризации.

См. дополнительные сведения о решении для отслеживания изменений и инвентаризации, чтобы получить дополнительные сведения.

> [!div class="nextstepaction"]
> [Change management and Inventory solution](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json) (Решение для управления изменениями и инвентаризации)