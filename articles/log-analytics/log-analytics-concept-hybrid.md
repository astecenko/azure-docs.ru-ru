---
title: "Сбор данных для среды с помощью анализа журналов Azure | Документы Microsoft"
description: "Этот раздел поможет понять, как собирать данные и наблюдать за компьютерами, размещенных в локальном или других облачной среде с помощью аналитики журналов."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: magoedte
ms.openlocfilehash: 513855084c8b89d97b049f1df2ec24d0f9789afe
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2017
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Сбор данных с компьютеров в вашей среде с помощью аналитики журналов

Аналитика журналов Azure можно собирать и работают с данными из компьютеров Windows или Linux, размещенные в:

* [Виртуальные машины Azure](log-analytics-quick-collect-azurevm.md) с помощью расширения ВМ аналитика журналов 
* Центр обработки данных как физических серверах или виртуальных машин
* Виртуальные машины в облачной службе, например Amazon Web Services (AWS)

Компьютеры, размещаемых в среде может подключаться непосредственно к службе анализа журналов, или если эти компьютеры с помощью System Center Operations Manager 2012 R2 или 2016 уже ведется наблюдение, можно интегрировать операций управления группы управления с помощью аналитики журналов и Продолжайте обслуживание процессы операций службы и стратегии.  

## <a name="overview"></a>Обзор

![log-Analytics-Agent-Direct-Connect-Diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Прежде чем анализа и реагирования на собранных данных, необходимо сначала установить и подключить агенты для всех компьютеров, которые вы хотите отправлять данные в службе анализа журналов. Вы можете установить агенты на локальных компьютерах с помощью программы установки, в командной строке или с требуемого состояния (DSC) в службе автоматизации Azure. 

Агент для Linux и Windows взаимодействует в службе анализа журналов исходящих через TCP-порт 443 и при подключении компьютера к брандмауэр или прокси-сервера для обмена данными через Интернет, просмотрите [Настройка агента для использования с прокси-сервера или шлюз OMS](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) разобраться в изменениях конфигурации, которые необходимо применить. Если вы наблюдаете компьютер с System Center 2016 — Operations Manager или Operations Manager 2012 R2 может быть многосетевой в службе анализа журналов сбора данных и пересылать их в службе и по-прежнему могут контролироваться [Operations Manager ](log-analytics-om-agents.md). Компьютеры Linux наблюдением группы управления Operations Manager, интегрированных с помощью аналитики журналов не получают конфигурацию для источников данных и пересылать собранных данных через группы управления. Агент Windows может сообщать до четырех рабочие области, пока агент для Linux поддерживает только к одной рабочей области отчетов.  

Агент для Linux и Windows не только для подключения в службе анализа журналов, оно также поддерживает подключение в службе автоматизации Azure для размещения рабочей роли Runbook гибридных и решений управления, такие как отслеживание изменений и управления обновлениями.  Дополнительные сведения о гибридных рабочая роль Runbook см. в разделе [Azure Automation гибридной рабочей ролью Runbook](../automation/automation-offering-get-started.md#automation-architecture-overview).  

Если политики ИТ-безопасности запрещают подключение компьютеров в сети к Интернету, агент можно настроить для подключения к шлюзу OMS. Так он сможет получать сведения о конфигурации и отправлять собранные данные в зависимости от включенного решения. Дополнительные сведения и шаги по настройке агента Windows или Linux для взаимодействия через шлюз Служба аналитики журнала OMS см. в разделе [подключение компьютеров к OMS с помощью шлюза OMS](log-analytics-oms-gateway.md). 

## <a name="prerequisites"></a>Технические условия
Прежде чем начать, просмотрите следующие сведения, чтобы убедиться, что соответствует минимальным системным требованиям.

### <a name="windows-operating-system"></a>Операционная система Windows
Для агента Windows официально поддерживаются следующие версии операционной системы Windows:

* Windows Server 2008 с пакетом обновления 1 (SP1) или более поздней версии
* Windows 7 с пакетом обновления 1 и более поздних версий.

#### <a name="network-configuration"></a>Конфигурация сети
Сведения ниже список детали конфигурации прокси-сервера и брандмауэра, необходимые для взаимодействия с помощью аналитики журналов агента Windows. — Исходящим из локальной сети к службе анализа журналов. 

| Ресурс агента | порты; | Обход проверки HTTPS|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | Yes |
|*.oms.opinsights.azure.com | 443 | Yes | 
|*.blob.core.windows.net | 443 | Yes | 
|*.azure-automation.net | 443 | Yes | 

### <a name="linux-operating-systems"></a>Операционные системы Linux
Официально поддерживаются следующие дистрибутивы Linux.  Однако агент для Linux также можно запустить на других дистрибутивах, которых нет в списке.

* Amazon Linux 2012.09–2015.09 (x86/x64)
* CentOS Linux 5, 6 и 7 (x86/x64)
* Oracle Linux 5, 6 и 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 и 7 (x86/x64)
* Debian GNU/Linux 6, 7 и 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 и 12 (x86/x64)

#### <a name="network-configuration"></a>Конфигурация сети
Сведения ниже список детали конфигурации прокси-сервера и брандмауэра, необходимые для взаимодействия с помощью аналитики журналов агента Linux. — Исходящим из локальной сети к службе анализа журналов. 

|Ресурс агента| порты; |  
|------|---------|  
|*.ods.opinsights.azure.com | Порт 443|   
|*.oms.opinsights.azure.com | Порт 443|   
|*.blob.core.windows.net | Порт 443|   
|*.azure-automation.net | Порт 443|  

Агент для Linux поддерживает взаимодействие через прокси-сервер или шлюз OMS к службе анализа журналов с помощью протокола HTTPS.  Поддерживаются как анонимных, так и основные проверки подлинности (имя пользователя и пароль).  Прокси-сервер можно указать во время установки или при изменении файла конфигурации proxy.conf после установки.  

Значение конфигурации прокси-сервера имеет следующий синтаксис:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Если прокси-сервер требует проверки подлинности, агент Linux по-прежнему требуется предоставление псевдо пользователя и пароля. Это может быть любое имя пользователя или пароль.

|Свойство| ОПИСАНИЕ |
|--------|-------------|
|Протокол | HTTPS |
|user | Необязательное имя пользователя для аутентификации прокси-сервера |
|password | Необязательный пароль для аутентификации прокси-сервера |
|proxyhost | Адрес или полное доменное имя прокси-сервера или шлюза OMS |
|порт | Номер дополнительного порта для прокси сервера или OMS шлюза |

Например: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Если использовать специальные символы, такие как «@» в свой пароль, так как неправильно синтаксический анализ значения возникнет ошибка подключения прокси-сервера.  Чтобы обойти эту проблему, закодировать в URL-АДРЕСЕ, с помощью средства, такие как пароль [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Установка и настройка агента 
Подключение локальных компьютеров напрямую с помощью аналитики журналов может осуществляться разными способами в зависимости от требований. В следующей таблице выделены каждый метод, чтобы определить, что лучше всего работает в вашей организации.

|Источник | Метод | ОПИСАНИЕ|
|-------|-------------|-------------|
| Компьютер Windows|- [Ручная установка](log-analytics-agent-windows.md)<br>- [Azure Automation DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Шаблон диспетчера ресурсов с стек Azure](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Установка Microsoft Monitoring agent из командной строки или с помощью автоматического способа, таких как Azure Automation DSC [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), или с помощью шаблона диспетчера ресурсов Azure, если вы развернули Microsoft Стек Azure в центре обработки данных.| 
|Компьютер Linux| [Ручная установка](log-analytics-quick-collect-linux-computer.md)|Установите агент для Linux, вызов оболочки скрипта на сайте GitHub. | 
| System Center Operations Manager|[Интеграция Operations Manager с помощью аналитики журналов](log-analytics-om-agents.md) | Настроить интеграцию между Operations Manager и служба аналитики журналов для пересылки сбора данных с компьютеров Linux и Windows в группу управления.|  

## <a name="next-steps"></a>Дальнейшие действия

* Просмотрите [источники данных](log-analytics-data-sources.md) для понимания источники данных, доступные для сбора данных из операционной системы Windows или Linux. 

* Узнайте больше об [операциях поиска по журналу](log-analytics-log-searches.md) , которые можно применять для анализа данных, собираемых из источников данных и решений. 

* Узнайте больше о [решениях](log-analytics-add-solutions.md) , которые расширяют функции службы Log Analytics и собирают данные в репозиторий OMS.