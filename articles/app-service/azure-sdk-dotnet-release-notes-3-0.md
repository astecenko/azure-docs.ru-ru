---
title: "Заметки о выпуске пакета Azure SDK для .NET 3.0 | Документы Майкрософт"
description: "Заметки о выпуске пакета Azure SDK для .NET 3.0"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: eea4e569ac2d0192ed7872d2fcb9bed03614832b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a>Заметки о выпуске пакета Azure SDK для .NET 3.0

В этой статье содержатся заметки о выпуске пакета Azure SDK версий 3.0 и 2.9.6 для .NET.

##<a name="azure-sdk-for-net-30-release-summary"></a>Сводные данные о выпуске пакета Azure SDK для .NET 3.0

Дата выпуска: 07.03.2017
 
В этом выпуске нет никаких критических изменений пакета Azure SDK 3.0. Для использования этого пакета SDK с имеющимися проектами облачной службы также не требуется выполнять обновление. Чтобы пакет SDK для Azure 3.0 мог использоваться без необходимости обновления, он устанавливается в те же каталоги, что и пакет SDK для Azure 2.9. Основной номер версии большинства компонентов не изменился (т. е. остался равным 2.9), но обновился их номер сборки.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- В Visual Studio 2017 этот выпуск пакета Azure SDK для .NET встроен в рабочую нагрузку Azure. Все средства, необходимые для разработки Azure, будут входить в Visual Studio 2017. Для Visual Studio 2015 пакет SDK по-прежнему будет доступен через WebPI. Мы вывели из использования выпуски пакета Azure SDK для .NET для Visual Studio 2013, так как теперь вышла версия Visual Studio 2017.

### <a name="azure-diagnostics"></a>Диагностика Azure

- Теперь сохраняется только частичная строка подключения к хранилищу для диагностики облачных служб с маркером вместо ключа. Фактический ключ к хранилищу данных теперь хранится в папке профиля пользователя, поэтому доступ к нему можно контролировать. Visual Studio будет считывать этот ключ для локальной отладки и процедуры публикации. 
- В ответ на описанные выше изменения группа разработчиков Visual Studio Online усовершенствовала шаблон задачи развертывания облачных служб Azure. Теперь пользователи могут указать ключ к хранилищу данных, чтобы настроить расширение системы диагностики во время публикации в Azure при непрерывной интеграции и развертывании.
- Мы сделали так, что стало возможно хранить безопасную строку подключения и разметку для системы диагностики Azure (WAD), чтобы устранить проблемы с конфигурацией в средах.
 
### <a name="windows-server-2016-virtual-machines"></a>Виртуальные машины Windows Server 2016

- Visual Studio теперь поддерживает развертывание облачных служб на виртуальных машинах с операционной системой из семейства версии 5 (Windows Server 2016). Для имеющихся облачных служб можно изменить параметры, чтобы использовать новую ОС из семейства версий. Если для создания облачных служб вы решили использовать .NET 4.6 или более поздней версии, по умолчанию для службы будет использоваться ОС из семейства версий 5.  Дополнительные сведения см. в статье [Таблица совместимости выпусков гостевых ОС Azure и пакетов SDK](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Известные проблемы

- В пакете SDK Azure 3.0 для .NET возникала проблема при удалении Visual Studio 2017 в параллельной конфигурации с Visual Studio 2015.  Если у вас установлен пакет SDK Azure для Visual Studio 2015, при удалении Visual Studio 2017 будут удалены эмулятор хранения Microsoft Azure и эмулятор вычислений Microsoft Azure.  Это приведет к ошибке при создании и отладке новых проектов облачных служб в Visual Studio 2015. Чтобы устранить эту проблему, переустановите пакет Azure SDK из установщика веб-платформы.  Проблема будет устранена в следующем обновлении Visual Studio 2017.  .

 
### <a name="azure-in-role-cache"></a>Кэш роли Azure 

- Поддержка кэша роли Azure завершилась 30 ноября 2016 года. Дополнительные сведения см. [здесь](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




