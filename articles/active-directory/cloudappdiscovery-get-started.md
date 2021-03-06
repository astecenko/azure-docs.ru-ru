---
title: "Настройка службы Cloud App Discovery в Azure Active Directory | Документация Майкрософт"
description: "Поиск и администрирование приложений с помощью Cloud App Discovery для предоставления ценных сведений об использовании облачных и теневых ИТ."
services: active-directory
keywords: "cloud app discovery, управление приложениями"
documentationcenter: 
author: curtand
manager: mtillman
tags: ignite
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: nigu
ms.openlocfilehash: 4a0cb1b7793c846f98ae4e89b99b4bda984cd5e4
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2017
---
# <a name="set-up-cloud-app-discovery-in-azure-ad"></a>Настройка Cloud App Discovery в Azure AD

В основе Cloud App Discovery в Azure AD теперь лежит интеграция с данными, доступными в Microsoft Cloud App Security. Чтобы предоставить вам актуальные сведения об использовании облачных и теневых ИТ, Cloud App Discovery сравнивает журналы трафика с каталогом Cloud App Security из более чем 15 000 облачных приложений. В этой статье описывается процедура настройки и содержатся ссылки на подробные сведения о каждом шаге. Здесь также предоставлены сведения о реализации поддержки данных брандмауэра и прокси-сервера, а также файлов журнала.

## <a name="prerequisites"></a>Технические условия

* Для использования этого продукта ваша организация должна иметь лицензию Azure AD Premium P1. Дополнительные сведения см. в разделе [Цены на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
* Для настройки Cloud App Discovery вам нужно быть глобальным администратором или читателем безопасности в Azure Active Directory.

## <a name="setup-steps"></a>Процедура настройки

1. [Настройте отчеты о моментальных снимках](cloudappdiscovery-set-up-snapshots.md) для проверки формата журнала. Убедитесь, что журналы передают важные сведения в Cloud App Discovery. Они также позволяют детальнее проанализировать журналы трафика, которые вы вручную отправляете из брандмауэров и с прокси-серверов.

2. [Настройте непрерывные отчеты](https://docs.microsoft.com/cloud-app-security/discovery-docker) для анализа всех журналов, пересылаемых из вашей сети с помощью сборщика журналируемых данных Cloud App Security. Их можно использовать для определения новых приложений и тенденций использования.

3. Если сейчас журналы не поддерживаются, [настройте пользовательское средство синтаксического анализа журналов](https://docs.microsoft.com/cloud-app-security/custom-log-parser), чтобы модуль Cloud App Discovery смог их проанализировать.
  
## <a name="log-processing-flow"></a>Процедура обработки журналов

В зависимости от объема данных создание отчетов может занять от нескольких минут до нескольких часов. При этом анализируется следующее:

* **Отправка** — журналы веб-трафика из сети передаются на портал.
* **Синтаксический анализ** — Cloud App Security анализирует и извлекает данные из журналов трафика с помощью выделенного средства синтаксического анализа для каждого источника данных.
* **Анализ** — данные трафика анализируются с использованием каталога Cloud App Security для определения более чем 15 000 облачных приложений. В ходе анализа также определяются активные пользователи и IP-адреса.
* **Создание отчета** — Cloud App Security создает отчет о данных, извлеченных из файлов журналов, и предоставляет его Cloud App Discovery.

> [!NOTE]
> * Данные непрерывного отчета анализируются два раза в день.
> * Сборщик журналируемых данных сжимает данные перед отправкой. Исходящий трафик через сборщик журналируемых данных составляет примерно 10 % от размера полученных журналов трафика.

## <a name="using-traffic-logs-for-cloud-app-discovery"></a>Использование журналов трафика для Cloud App Discovery

Cloud App Discovery использует данные из журналов трафика. Чем больше подробностей вы можете добавить в журнал, тем полнее формируемое представление. Cloud App Discovery требуются данные о веб-трафике со следующими атрибутами:

* Дата транзакции
* Исходный IP-адрес
* Исходный пользователь **рекомендуется**
* Конечный IP-адрес
* Конечный URL-адрес **рекомендуется** (URL-адреса повышают точность при обнаружении облачных приложений по сравнению с IP-адресами)
* Общий объем данных
* Объем отправленных или скачанных данных для получения сведений о характере использования облачных приложений
* Предпринятое действие (разрешено/заблокировано)

Cloud App Discovery не может отображать или анализировать атрибуты, не включенные в журналы. Например, стандартный формат журнала у **брандмауэра Cisco ASA** не содержит **объем переданных байтов за одну транзакцию**, **имя пользователя** или **конечный URL-адрес** , а только конечный IP-адрес. Таким образом, в вашем распоряжении может быть меньше сведений об облачных приложениях из этого источника данных. Для брандмауэров Cisco ASA установите уровень информации 6.1.

Чтобы создать отчет Cloud App Discovery, журналы трафика должны соответствовать следующим условиям:

1.  Источник данных является [поддерживаемым брандмауэром или прокси-сервером](#supported-firewalls-and-proxies).
2.  Формат журнала совпадает с ожидаемым стандартным форматом. Это проверяется во время отправки. Сведения об оптимизации вашего формата журнала см. в разделе [Создание отчетов о снимках для Cloud App Discovery](cloudappdiscovery-set-up-snapshots.md).
3.  События не старше 90 дней.
4.  Файл журнала допустим и включает информацию об исходящем трафике.

## <a name="supported-firewalls-and-proxy-servers"></a>Поддерживаемые брандмауэры и прокси-серверы

* Barracuda — брандмауэр веб-приложений (W3C)
* Blue Coat Proxy SG — журнал доступа (W3C)
* Check Point
* Мощь этих средств есть Cisco ASA
* Брандмауэр Cisco ASA (для брандмауэров Cisco ASA установите уровень информации 6)
* Cisco IronPort WSA
* Cisco ScanSafe
* Cisco Meraki — журнал URL-адресов
* Clavister NGFW (Syslog)
* Dell Sonicwall
* Fortinet Fortigate
* Juniper SRX
* Juniper SSG
* McAfee Secure Web Gateway
* Microsoft Forefront Threat Management Gateway (W3C)
* Брандмауэр из серии Palo Alto
* Sophos SG
* Sophos Cyberoam
* Squid (Common)
* Squid (Native)
* Websense — Web Security Solutions — подробный отчет для проведения расследований (CSV)
* Websense — Web Security Solutions — отчет об интернет-активности (CEF)
* Zscaler

> [!NOTE]
> Cloud App Discovery поддерживает адреса IPv4 и IPv6.

Если ваш журнал не поддерживается, выберите значение **Другой** для параметра **Источник данных** и укажите устройство и журнал, который вы пытаетесь отправить. Ваш журнал рассматривает группа облачных аналитиков Cloud App Security. В случае добавления вашего типа журнала мы отправим вам соответствующее уведомление, но вместо этого вы можете определить настраиваемое средство синтаксического анализа, соответствующее вашему формату журнала. Дополнительные сведения см. в статье [Использование настраиваемого средства синтаксического анализа журналов](https://docs.microsoft.com/cloud-app-security/custom-log-parser)

## <a name="data-attributes-according-to-vendor-documentation"></a>Атрибуты данных (в соответствии с документацией поставщика)

| Источник данных         | URL-адрес целевого приложения | IP-адрес целевого приложения | Имя пользователя | Исходный IP-адрес | Общий трафик | Отправлено байтов |
|-----------------------------------------|----------------|---------------|----------|-----------|---------------|----------------|
| Barracuda                               | **Да**        | **Да**       | **Да**  | **Да**   | Нет             | Нет              |
| Blue Coat                               | **Да**        | Нет             | **Да**  | **Да**   | **Да**       | **Да**        |
| Контрольная точка                              | Нет              | **Да**       | Нет        | **Да**   | Нет             | Нет              |
| Cisco ASA                               | Нет              | **Да**       | Нет        | **Да**   | **Да**       | Нет              |
| Cisco FWSM                              | Нет              | **Да**       | Нет        | **Да**   | **Да**       | Нет              |
| Cisco Ironport WSA                      | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |
| Cisco Meraki                            | **Да**        | **Да**       | Нет        | **Да**   | Нет             | Нет              |
| Clavister NGFW (Syslog)                 | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |
| Dell SonicWall                          | **Да**        | **Да**       | Нет        | **Да**   | **Да**       | **Да**        |
| Fortigate                               | Нет              | **Да**       | Нет        | **Да**   | **Да**       | **Да**        |
| Juniper SRX                             | Нет              | **Да**       | Нет        | **Да**   | **Да**       | **Да**        |
| Juniper SSG                             | Нет              | **Да**       | Нет        | **Да**   | **Да**       | **Да**        |
| McAfee SWG                              | **Да**        | Нет             | Нет        | **Да**   | **Да**       | **Да**        |
| MS TMG                                  | **Да**        | Нет             | **Да**  | **Да**   | **Да**       | **Да**        |
| Palo Alto Networks                      | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |
| Sophos                                  | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | Нет              |
| Squid (Common)                          | **Да**        | Нет             | **Да**  | **Да**   | Нет             | **Да**        |
| Squid (Native)                          | **Да**        | Нет             | **Да**  | **Да**   | Нет             | **Да**        |
| Websense — отчет для проведения расследований (CSV)   | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |
| Websense — журнал интернет-активности (CEF)  | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |
| Zscaler                                 | **Да**        | **Да**       | **Да**  | **Да**   | **Да**       | **Да**        |


## <a name="next-steps"></a>Дальнейшие действия
Используйте следующие ссылки, чтобы продолжить настройку системы Cloud App Discovery в Azure AD.

* [Создание отчетов о моментальных снимках](cloudappdiscovery-set-up-snapshots.md)
* [Настройка непрерывного создания отчетов](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Использование настраиваемого средства синтаксического анализа журналов](https://docs.microsoft.comcommit/cloud-app-security/custom-log-parser)
