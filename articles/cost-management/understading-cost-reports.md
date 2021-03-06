---
title: "Интерпретация отчетов о затратах в службе \"Управление затратами Azure\" | Документация Майкрософт"
description: "Эта статья поможет вам понять базовую структуру и функции отчетов Cloudyn."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/27/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: df2108a6e2a01195340a09eacf1c56f9d738c923
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="understanding-cost-reports"></a>Интерпретация отчетов о затратах

Эта статья поможет вам понять базовую структуру и функции отчетов Cloudyn. Большинство отчетов Cloudyn интуитивно понятны и имеют однородный внешний вид. После прочтения этой статьи вы сможете использовать все отчеты. Многие стандартные возможности доступны в различных отчетах,что позволяет легко их находить. Вы можете настраивать отчеты и выбирать способ вычисления и отображения результатов.

## <a name="report-fields-and-options"></a>Параметры и поля отчета

Ниже приведен пример отчета по затратам на период времени. Большинство отчетов Cloudyn имеют аналогичный макет.

![пример отчета](./media/understanding-cost-reports/sample-report.png)

Каждая пронумерованная область на предыдущем снимке подробно описана далее.

1. **Date Range** (Диапазон дат)

    Используйте список диапазонов дат, чтобы определить интервал времени отчета, используя предустановку или пользовательские настройки.
2. **Saved Filter** (Сохраненный фильтр)

    Используйте список сохраненных фильтров, чтобы сохранить текущие группы и фильтры, применяемые к отчету. В отчетах о затратах и производительности доступны такие сохраненные фильтры:

      - анализ затрат;
      - распределение;
      - управление ресурсами.
      - Оптимизация

  Введите название фильтра и щелкните **Save** (Сохранить).

3. **Теги**

    Используйте область "Теги", чтобы выполнить группировку по категориям тегов. Теги, отображаемые в меню, являются тегами отдела Azure или места возникновения затрат. Они также могут быть тегами подписки и сущности затрат Cloudyn. Выберите теги для фильтрации результатов. Для фильтрации результатов также можно ввести название тега (ключевое слово).

    ![Выбор параметров](./media/understanding-cost-reports/select-options.png)

    Выберите **Добавить**, чтобы добавить новый фильтр.

    ![Добавление фильтра](./media/understanding-cost-reports/add-filter.png)

    Процесс группировки или фильтрации тегов не связан с ресурсами Azure или тегами группы ресурсов.

    Выполнить группировку и фильтрацию тегов по распределению затрат можно с помощью пункта меню **Groups** (Группы).

4. **Группы в отчетах**

    Используйте группы в отчетах анализа затрат, чтобы отобразить стандартные, детализированные категории на основе данных о выставлении счетов из отчета.  Однако в группах отчетов по распределению затрат отображаются категории представлений на основе тегов. Теговые категории определяются в модели распределения затрат и стандартных детализированных категориях из данных о выставлении счетов.

    ![теги групп](./media/understanding-cost-reports/groups-tags01.png)

    ![теги групп](./media/understanding-cost-reports/groups-tags02.png)

    Отчеты о распределении затрат и группы в теговых категориях групп могут включать в себя следующие компоненты:
      - Теги
      - теги группы ресурсов;
      - теги сущности затрат Cloudyn;
      - категории тегов подписки для распределения затрат.

  Примеры могут включать в себя следующие элементы:
     - место возникновения затрат;
     - Department
     - Приложение
     - Среда
     - код затрат.

5. **Фильтры**

    Используйте один или несколько фильтров, чтобы задать диапазоны выбранных значений. Чтобы задать фильтр, щелкните **Add** (Добавить), а затем выберите категории и значения фильтра.

6. **Cost Model** (Модель затрат)

    Используйте пункт меню Cost Model (Модель затрат), чтобы выбрать модель затрат, созданную ранее с помощью комплексного распределения затрат. Может потребоваться несколько моделей затрат Cloudyn (в зависимости от требований распределения затрат). Требования к распределению затрат у одних команд в организации могут отличаться от требований других. Каждая команда может иметь собственную модель затрат.

    Сведения о создании определения модели распределения затрат см. в разделе [Распределение затрат с помощью пользовательских тегов](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Amortization** (Амортизация)

    Используйте пункт меню Amortization (Амортизация), чтобы просмотреть тарифы на обслуживание, не связанные с использованием, или одноразово оплачиваемые расходы, равномерно распределенные по времени в течение их срока действия. Примеры одноразовой платы могут включать в себя:
    - годовую плату за поддержку;
    - годовую плату за компоненты обеспечения безопасности;
    - плату за покупку зарезервированных экземпляров;
    - некоторые элементы Azure Marketplace.

  В пункте меню Amortization (Амортизация) выберите **Amortized cost** (Амортизированная себестоимость) или **Actual Cost** (Фактическая себестоимость).

8. **Способы устранения:**

    Используйте пункт меню Resolution (Разрешение), чтобы выбрать разрешение по времени в указанном диапазоне дат. Разрешение по времени определяет, как единицы отображаются в отчете. Вы можете задать для разрешения следующие значения:
    - Ежедневно
    - Weekly (Еженедельно);
    - Ежемесячная
    - Quarterly (Ежеквартально);
    - Annual (Ежегодно).

9. **Allocation rules** (Правила распределения)

    Используйте пункт меню Allocation rules (Правила распределения), чтобы применить или отключить повторное вычисление стоимости на распределение затрат. Вы можете включить или отключить повторное вычисление при распределении затрат для данных о выставлении счетов. Повторное вычисление применяется к выбранным в отчете категориям. Это позволяет оценить влияние повторного вычисления при распределении затрат на необработанные данные о выставлении счетов.

10. **Uncategorized** (Без категории)

    Используйте пункт меню Uncategorized (Без категории), чтобы включить затраты без категории в отчет и исключить их из него.

11. **Show/hide fields** (Показать или скрыть поля)

    Параметр Show/hide fields (Показать или скрыть поля) не влияет на отчеты.

12.   **Форматы отображения**

    Используйте форматы отображения, чтобы выбрать различные представления графика или таблицы.

    ![форматы отображения](./media/understanding-cost-reports/display-formats.png)

13. **Multi-color** (Многоцветный)

    Используйте раскрывающееся меню Multi-color (Многоцветный), чтобы задать в отчете цвет для графика.

14. **Действия**

    Используйте элемент Actions (Действия), чтобы сохранить, экспортировать или запланировать отчет.

## <a name="next-steps"></a>Дальнейшие действия

- Если вы еще не завершили первое руководство по службе "Управление затратами", ознакомьтесь с ним в статье [Просмотр сведений об использовании и затратах](tutorial-review-usage.md).
