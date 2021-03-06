---
title: "Преобразование \"Получение столбца по образцу\" с использованием Azure Machine Learning Workbench"
description: "Справочный документ по преобразованию \"Получение столбца по образцу\""
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 6febd3f12248a96f54415a91fcf0513ef7412e78
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2017
---
# <a name="derive-column-by-example-transformation"></a>Преобразование "Получение столбца по образцу"

Преобразование **Получение столбца по образцу** позволяет создать столбец, производный от одного или нескольких существующих столбцов, на основе предоставляемого пользователем образца результата. Для получения столбца может использоваться любое сочетание поддерживаемых преобразований для строк, дат и чисел. 

Поддерживаются указанные ниже преобразования для строк, дат и чисел.

**Преобразования строк** 

Извлечение подстроки (в том числе интеллектуальное извлечение чисел и дат), объединение, преобразование регистра, сопоставление констант.

**Преобразования дат** 

Изменение формата даты, извлечение частей даты, сопоставление времени с интервалами.

Для преобразования дат соблюдаются обычные стандарты с несколькими важными ограничениями:
* не поддерживаются часовые пояса;
* не поддерживаются некоторые распространенные форматы:
    * формат недели года ISO 8601 (например, "2009-W53-7"); 
    * формат времени UNIX EPOCH;
* все форматы должны задаваться с учетом регистра (например, "4am" не распознается как время, в отличие от "4AM").

**Преобразования чисел** 

Округление, округление в меньшую сторону, округление в большую сторону, группировка по интервалам, добавление нулей или пробелов, деление и умножение на 1000.

**Составные преобразования** 

Любое сочетание преобразований для строк, дат и чисел.

## <a name="how-to-use-this-transformation"></a>Как использовать это преобразование

Чтобы использовать это преобразование, выполните инструкции ниже.
1. Выберите один или несколько столбцов, из которых нужно получить значения. 
2. В меню **Transforms** (Преобразования) выберите **Derive Column by Example** (Получение столбца по образцу). Или щелкните правой кнопкой мыши заголовок любого из выбранных столбцов и в контекстном меню выберите **Derive Column by Example** (Получение столбца по образцу). Откроется редактор преобразования, и рядом с крайним выбранным столбцом справа будет добавлен новый столбец. Заголовки выбранных столбцов отмечены флажками. Убирая или добавляя эти флажки, вы можете изменить набор выбранных столбцов.
3. Введите в новый столбец образец *выходных данных* и нажмите клавишу ВВОД. Workbench анализирует столбец с входными данными и предоставленные выходные данные, чтобы синтезировать программу для преобразования входных значений в выходные по образцу. Синтезированная программа применяется для всех строк в наборе данных. В неоднозначных и сложных случаях может потребоваться несколько образцов. Метод предоставления нескольких образцов зависит от выбранного режима работы (базовый или расширенный).
4. Просмотрите выходные данные и нажмите кнопку **ОК**, чтобы принять преобразование.

### <a name="transform-editor-basic-mode"></a>Редактор преобразования: базовый режим

В базовом режиме поддерживается встроенная правка в сетке данных. Чтобы предоставить образцы выходных данных, перейдите к нужной ячейке и введите значение. 

Workbench анализирует эти данные. Затем пользователю предлагается проверить спорные случаи. При анализе данных в заголовке редактора преобразования отображается текст **Analyzing Data** (Анализ данных). Когда анализ будет завершен, в заголовке отобразится текст **No Suggestions** (Предложения отсутствуют) или **Review next suggested row** (Проверка следующей предлагаемой строки). Вы можете переходить между спорными случаями, щелкая элемент **Review next suggested row** (Проверка следующей предлагаемой строки). Если значение для этой строки будет неверным, введите в нее правильное значение в качестве дополнительного образца. 

### <a name="transform-editor-advanced-mode"></a>Редактор преобразования: расширенный режим

Расширенный режим предоставляет больше возможностей для получения столбца по образцу. Все образцы отображаются рядом. Также вы можете просмотреть сразу все спорные случаи, щелкнув **Show suggested examples** (Показать предлагаемые примеры). 

В расширенном режиме вы можете указать в качестве образца любую строку, дважды щелкнув ее в сетке. Когда строка будет перенесена в список образцов, вы можете изменить данные в исходных столбцах, чтобы создать искусственный пример. Так можно добавить случаи, которые отсутствуют в исходном наборе данных.

Пользователь может переключаться между **базовым** и **расширенным** режимами, щелкая ссылки в редакторе преобразования.

### <a name="editing-existing-transformation"></a>Редактирование существующего преобразования

Пользователь может изменить текущее преобразование **Derive Column By Example** (Получение столбца по образцу), выбрав параметр **Edit** (Правка) на этапе преобразования. Щелкнув **Edit** (Правка), вы откроете редактор преобразования в **расширенном режиме**. Отобразятся все образцы, предоставленные при создании преобразования.

## <a name="examples-of-string-transformations-by-example"></a>Примеры преобразований строк по образцу


>[!NOTE] 
>**Полужирным** выделены значения, которые были предоставлены в качестве образцов для преобразования в этом наборе данных.


### <a name="s1-extracting-file-names-from-file-paths"></a>S1. Извлечение имен файлов из путей к файлам

Количество образцов, которые потребовались в этом примере: 2

|Входные данные|Выходные данные|
|:-----|:-----|
|C:\Python35\Tools\pynche\TypeinViewer.py|**TypeinViewer.py**|
|C:\Python35\Tools\pynche\webcolors.txt|webcolors.txt|
|C:\Python35\Tools\pynche\websafe.txt|websafe.txt|
|C:\Python35\Tools\pynche\X\rgb.txt|rgb.txt|
|C:\Python35\Tools\pynche\X\xlicense.txt|xlicense.txt|
|C:\Python35\Tools\Scripts\2to3.py|2to3.py|
|C:\Python35\Tools\Scripts\analyze_dxp.py|**analyze_dxp.py**|
|C:\Python35\Tools\Scripts\byext.py|byext.py|
|C:\Python35\Tools\Scripts\byteyears.py|byteyears.py|
|C:\Python35\Tools\Scripts\checkappend.py|checkappend.py|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. Извлечение строк с преобразованием регистра

Количество образцов, которые потребовались в этом примере: 3

|Входные данные|Выходные данные|
|:-----|:-----|
|REINDEER CT & DEAD END;  NEW HANOVER; Station 332; 2015-12-10 @ 17:10:52;|**New Hanover**|
|BRIAR PATH & WHITEMARSH LN;  HATFIELD TOWNSHIP; Station 345; 2015-12-10 @ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 @ 14:39:21-Station:STA27;|**Norristown**|
|AIRY ST & SWEDE ST;  NORRISTOWN; Station 308A; 2015-12-10 @ 16:47:36;|**Norristown**|
|CHERRYWOOD CT & DEAD END;  LOWER POTTSGROVE; Station 329; 2015-12-10 @ 16:56:52;|Lower Pottsgrove|
|CANNON AVE & W 9TH ST;  LANSDALE; Station 345; 2015-12-10 @ 15:39:04;|Lansdale|
|LAUREL AVE & OAKDALE AVE;  HORSHAM; Station 352; 2015-12-10 @ 16:46:48;|Horsham|
|COLLEGEVILLE RD & LYWISKI RD;  SKIPPACK; Station 336; 2015-12-10 @ 16:17:05;|Skippack|
|MAIN ST & OLD SUMNEYTOWN PIKE;  LOWER SALFORD; Station 344; 2015-12-10 @ 16:51:42;|Lower Salford|
|BLUEROUTE  & RAMP I476 NB TO CHEMICAL RD; PLYMOUTH; 2015-12-10 @ 17:35:41;|Plymouth|
|RT202 PKWY & KNAPP RD; MONTGOMERY; 2015-12-10 @ 17:33:50;|Montgomery|
|BROOK RD & COLWELL LN; PLYMOUTH; 2015-12-10 @ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. Извлечение строк с преобразованием формата даты

Количество образцов, которые потребовались в этом примере: 1

|Входные данные|Выходные данные|
|:-----|:-----|
|MONTGOMERY AVE & WOODSIDE RD;  LOWER MERION; Station 313; 2015-12-11 @ 04:11:35;|**12 Nov 2015 4AM**|
|DREYCOTT LN & W LANCASTER AVE;  LOWER MERION; Station 313; 2015-12-11 @ 01:29:52;|12 Nov 2015 1AM|
|E LEVERING MILL RD & CONSHOHOCKEN STATE RD; LOWER MERION; 2015-12-11 @ 07:29:58;|12 Nov 2015 7AM|
|PENN VALLEY RD & MANOR RD;  LOWER MERION; Station 313; 2015-12-10 @ 20:53:30;|12 Oct 2015 8PM|
|BELMONT AVE & OVERHILL RD; LOWER MERION; 2015-12-10 @ 23:02:27;|12 Oct 2015 11PM|
|W MONTGOMERY AVE & PENNSWOOD RD; LOWER MERION; 2015-12-10 @ 19:25:22;|12 Oct 2015 7PM|
|ROSEMONT AVE & DEAD END;  LOWER MERION; Station 313; 2015-12-10 @ 18:43:07;|12 Oct 2015 6PM|
|AVIGNON DR & DEAD END; LOWER MERION; 2015-12-10 @ 20:01:29-Station:STA24;|12 Oct 2015 8PM|

### <a name="s4-concatenating-strings"></a>S4. Объединение строк

Количество образцов, которые потребовались в этом примере: 1

>[!NOTE] 
>В этом примере в выходном столбце пробелы представлены специальным символом "·".

|Имя|Средний инициал|Фамилия|Выходные данные|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda··Lohmann|
|Claudio|A|Chew|**Claudio·A·Chew**|
|Sarah-Jane|S|Smith|Sarah-Jane·S·Smith|
|Brandi||Blumenthal|Brandi··Blumenthal|
|Jesusita|R|Journey|Jesusita·R·Journey|
|Hermina||Hults|Hermina··Hults|
|Anne-Marie|W|Jones|Anne-Marie·W·Jones|
|Rico||Ropp|Rico··Ropp|
|Lauren-May||Fullmer|Lauren-May··Fullmer|
|Marc|T|Maine|Marc·T·Maine|
|Angie||Adelman|Angie··Adelman|
|John-Paul||Smith|John-Paul··Smith|
|Song|W|Staller|Song·W·Staller|
|Jill||Jefferies|Jill··Jefferies|
|Ruby-Grace|M|Simmons|Ruby-Grace·M·Simmons|

### <a name="s5-generating-initials"></a>S5. Вычисление инициалов

Количество образцов, которые потребовались в этом примере: 2

|Полное имя|Выходные данные|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio Chew|C.C.|
|Sarah-Jane Smith|S.S.|
|Brandi Blumenthal|B.B.|
|Jesusita Journey|J.J.|
|Hermina Hults|H.H.|
|Anne-Marie Jones|A.J.|
|Rico Ropp|R.R.|
|Lauren-May Fullmer|L.F.|
|Marc Maine|M.M.|
|Angie Adelman|A.A.|
|John-Paul Smith|**J.S.**|
|Song Staller|S.S.|
|Jill Jefferies|J.J.|
|Ruby-Grace Simmons|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Сопоставление значений констант

Количество образцов, которые потребовались в этом примере: 3

|Официальный пол|Выходные данные|
|:-----|:-----:|
|Male|**0**|
|Female|**1**|
|Unknown|**2**|
|Female|1|
|Female|1|
|Male|0|
|Unknown|2|
|Male|0|
|Female|1|

## <a name="examples-of-number-transformations-by-example"></a>Примеры преобразований чисел по образцу

>[!NOTE] 
>**Полужирным** выделены значения, которые были предоставлены в качестве образцов для преобразования в этом наборе данных.


### <a name="n1-rounding-to-nearest-10"></a>N1. Округление до ближайших десятков

Количество образцов, которые потребовались в этом примере: 1

|Входные данные|Выходные данные|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. Округление в меньшую сторону до десятков

Количество образцов, которые потребовались в этом примере: 2

|Входные данные|Выходные данные|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. Округление до 0,05

Количество образцов, которые потребовались в этом примере: 2

|Входные данные|Выходные данные|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Группирование

Количество образцов, которые потребовались в этом примере: 1

|Входные данные|Выходные данные|
|-----:|:-----:|
|20.16|**20-25**|
|14,32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. Увеличение в 1000 раз

Количество образцов, которые потребовались в этом примере: 1

|Входные данные|Выходные данные|
|-----:|-----:|
|-243|**-243000**|
|-12.5|-12500|
|-2345.23292|-2345232.92|
|-1202.3433|-1202343.3|
|1202.3433|1202343.3|

### <a name="n6-padding"></a>N6. Добавление заполнения

Количество образцов, которые потребовались в этом примере: 1

|Код|Выходные данные|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Примеры преобразований дат по образцу

>[!NOTE] 
>**Полужирным** выделены значения, которые были предоставлены в качестве образцов для преобразования в этом наборе данных.


### <a name="d1-extracting-date-parts"></a>D1. Извлечение частей даты

Эти части даты извлекались с помощью различных преобразований по образцу в одном и том же наборе данных. Строки, выделенные полужирным, представляют образцы, которые предоставлялись для соответствующих преобразований.

|Datetime|День недели|Дата|Месяц|Год|Hour|Минута|Секунда|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**Fri**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17-Jan-1990 13:32:01|Wed|17|Янв|1990|13.|32|01|
|14-Feb-2034 05:36:07|Tue|14|Feb|2034|5|36|07|
|14-Mar-2002 13:16:16|Thu|14|Мар|2002|13.|16|16|
|21-Jan-1985 05:44:43|Mon|21|Янв|1985|5|44|**43**|
|16-Aug-1985 01:11:56|Fri|16|Aug|1985|1|11|56|
|20-Dec-2033 18:36:29|Tue|20|Dec|2033|18|36|29|
|16-Jul-1984 10:21:59|Mon|16|Jul|1984|10|21|59|
|13-Jan-2038 10:59:36|Wed|13.|Янв|2038|10|59|36|
|14-Aug-1982 15:13:54|Sat|14|Aug|1982|15|13.|54|
|22-Nov-2030 08:18:08|Fri|22|Ноя|2030|8|18|08|
|21-Oct-1997 08:42:58|Tue|21|Окт|1997|8|42|58|
|28-Nov-2006 14:19:15|Tue|28|Ноя|2006|14|19|15|
|29-Apr-2031 04:59:45|Tue|29|Апр|2031|4.|59|45|
|29-Jan-2032 02:38:36|Thu|29|Янв|2032|2|38|36|
|11-May-2028 15:31:52|Thu|11|May|2028|15|31|52|
|15-Jul-1977 12:45:39|Fri|15|Jul|1977|12|45|11,9|
|27-Jan-2029 05:55:41|Sat|27|Янв|2029|5|55|41|
|03-Mar-2024 10:17:49|Sun|3|Мар|2024|10|17|49|
|14-Apr-2010 00:23:13|Wed|14|Апр|2010|0|23|13.|

### <a name="d2-formatting-dates"></a>D2. Форматирование дат

Форматирование дат выполнялось с помощью различных преобразований по образцу в одном и том же наборе данных. Строки, выделенные полужирным, представляют образцы, которые предоставлялись для соответствующих преобразований.

|Datetime|Формат 1|Формат 2|Формат 3|Формат 4|Формат 5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**1/31/2031**|**Friday, January 31, 2031**|**01312031 5:54**|**31/1/2031 5:54 AM**|**Q1 2031**|
|17-Jan-1990 13:32:01|1/17/1990|Wednesday, January 17, 1990|01171990 13:32|17/1/1990 1:32 PM|Q1 1990|
|14-Feb-2034 05:36:07|2/14/2034|Tuesday, February 14, 2034|02142034 5:36|14/2/2034 5:36 AM|Q1 2034
|14-Mar-2002 13:16:16|3/14/2002|Thursday, March 14, 2002|03142002 13:16|14/3/2002 1:16 PM|Q1 2002
|21-Jan-1985 05:44:43|1/21/1985|Monday, January 21, 1985|01211985 5:44|21/1/1985 5:44 AM|Q1 1985
|16-Aug-1985 01:11:56|8/16/1985|Friday, August 16, 1985|08161985 1:11|16/8/1985 1:11 AM|Q3 1985
|20-Dec-2033 18:36:29|12/20/2033|Tuesday, December 20, 2033|12202033 18:36|20/12/2033 6:36 PM|Q4 2033
|16-Jul-1984 10:21:59|7/16/1984|Monday, July 16, 1984|07161984 10:21|16/7/1984 10:21 AM|Q3 1984
|13-Jan-2038 10:59:36|1/13/2038|Wednesday, January 13, 2038|01132038 10:59|13/1/2038 10:59 AM|Q1 2038
|14-Aug-1982 15:13:54|8/14/1982|Saturday, August 14, 1982|08141982 15:13|14/8/1982 3:13 PM|Q3 1982
|22-Nov-2030 08:18:08|11/22/2030|Friday, November 22, 2030|11222030 8:18|22/11/2030 8:18 AM|Q4 2030
|21-Oct-1997 08:42:58|10/21/1997|Tuesday, October 21, 1997|10211997 8:42|21/10/1997 8:42 AM|Q4 1997
|28-Nov-2006 14:19:15|11/28/2006|Tuesday, November 28, 2006|11282006 14:19|28/11/2006 2:19 PM|Q4 2006
|29-Apr-2031 04:59:45|4/29/2031|Tuesday, April 29, 2031|04292031 4:59|29/4/2031 4:59 AM|Q2 2031
|29-Jan-2032 02:38:36|1/29/2032|Thursday, January 29, 2032|01292032 2:38|29/1/2032 2:38 AM|Q1 2032
|11-May-2028 15:31:52|5/11/2028|Thursday, May 11, 2028|05112028 15:31|11/5/2028 3:31 PM|Q2 2028
|15-Jul-1977 12:45:39|7/15/1977|Friday, July 15, 1977|07151977 12:45|15/7/1977 12:45 PM|Q3 1977
|27-Jan-2029 05:55:41|1/27/2029|Saturday, January 27, 2029|01272029 5:55|27/1/2029 5:55 AM|Q1 2029
|03-Mar-2024 10:17:49|3/3/2024|Sunday, March 3, 2024|03032024 10:17|3/3/2024 10:17 AM|Q1 2024
|14-Apr-2010 00:23:13|4/14/2010|Wednesday, April 14, 2010|04142010 0:23|14/4/2010 12:23 AM|Q2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Сопоставление времени с интервалами

Сопоставление времени с интервалами выполнялось с помощью различных преобразований по образцу в одном и том же наборе данных. Строки, выделенные полужирным, представляют образцы, которые предоставлялись для соответствующих преобразований.

|Datetime|Интервал (секунды)|Интервал (минуты)|Интервал (два часа)|Интервал (30 минут)|
|-----:|-----:|-----:|-----:|-----:|
|31-Jan-2031 05:54:18|**0-20**|**45-60**|**5AM-7AM**|**5:30-6:00**|
|17-Jan-1990 13:32:01|**0-20**|30-45|1PM-3PM|13:30-14:00|
|14-Feb-2034 05:36:07|0-20|30-45|5AM-7AM|5:30-6:00|
|14-Mar-2002 13:16:16|0-20|15-30|1PM-3PM|13:00-13:30|
|21-Jan-1985 05:44:43|40-60|30-45|5AM-7AM|5:30-6:00|
|16-Aug-1985 01:11:56|40-60|0-15|1AM-3AM|1:00-1:30|
|20-Dec-2033 18:36:29|20-40|30-45|5PM-7PM|18:30-19:00|
|16-Jul-1984 10:21:59|40-60|15-30|9AM-11AM|10:00-10:30|
|13-Jan-2038 10:59:36|20-40|45-60|9AM-11AM|10:30-11:00|
|14-Aug-1982 15:13:54|40-60|0-15|3PM-5PM|15:00-15:30|
|22-Nov-2030 08:18:08|0-20|15-30|7AM-9AM|8:00-8:30|
|21-Oct-1997 08:42:58|40-60|30-45|7AM-9AM|8:30-9:00|
|28-Nov-2006 14:19:15|0-20|15-30|1PM-3PM|14:00-14:30|
|29-Apr-2031 04:59:45|40-60|45-60|3AM-5AM|4:30-5:00|
|29-Jan-2032 02:38:36|20-40|30-45|1AM-3AM|2:30-3:00|
|11-May-2028 15:31:52|40-60|30-45|3PM-5PM|15:30-16:00|
|15-Jul-1977 12:45:39|20-40|45-60|11AM-1PM|12:30-13:00|
|27-Jan-2029 05:55:41|40-60|45-60|5AM-7AM|5:30-6:00|
|03-Mar-2024 10:17:49|40-60|15-30|9AM-11AM|10:00-10:30|
|14-Apr-2010 00:23:13|0-20|15-30|11PM-1AM|0:00-0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Примеры составных преобразований по образцу

|продолжительность поездки|время начала|идентификатор начальной станции|координаты начальной станции: широта|координаты начальной станции: долгота|тип пользователя|столбец|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Subscriber|**A Subscriber picked a bike from station 107, lat/long (42.363,-71.088), on Jan 08, 2016 at around 4PM. The trip duration was 61 minutes**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Клиент|A Customer picked a bike from station 74, lat/long (42.373,-71.119), on Jan 17, 2016 at around 9AM. The trip duration was 61 minutes|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 25, 2016 at around 8AM. The trip duration was 62 minutes|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Subscriber|A Subscriber picked a bike from station 107, lat/long (42.363,-71.088), on Jan 08, 2016 at around 10AM. The trip duration was 63 minutes|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Subscriber|A Subscriber picked a bike from station 68, lat/long (42.365,-71.103), on Jan 15, 2016 at around 7PM. The trip duration was 64 minutes|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Subscriber|A Subscriber picked a bike from station 115, lat/long (42.388,-71.119), on Jan 22, 2016 at around 6PM. The trip duration was 64 minutes|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Subscriber|A Subscriber picked a bike from station 178, lat/long (42.360,-71.101), on Jan 18, 2016 at around 9AM. The trip duration was 68 minutes|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 14, 2016 at around 8AM. The trip duration was 69 minutes|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Subscriber|A Subscriber picked a bike from station 141, lat/long (42.364,-71.082), on Jan 13, 2016 at around 10PM. The trip duration was 69 minutes|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Subscriber|A Subscriber picked a bike from station 176, lat/long (42.387,-71.119), on Jan 15, 2016 at around 8AM. The trip duration was 69 minutes|


## <a name="technical-notes"></a>Технические примечания

### <a name="conditional-transformations"></a>Условные преобразования
Иногда невозможно найти одно преобразование, которое подходит для всех предоставленных образцов. В таких случаях при преобразовании "Получение столбца по образцу" выполняется попытка сгруппировать входные данные по какому-нибудь шаблону, а затем вычислить разные преобразования для каждой группы. Это называется **условное преобразование**. **Условное преобразование** применяется только для преобразований с одним входным столбцом. 

### <a name="reference"></a>Справочные материалы
Дополнительные сведения о технологии преобразования строк по образцу можно найти в [этой публикации](https://www.microsoft.com/en-us/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).
