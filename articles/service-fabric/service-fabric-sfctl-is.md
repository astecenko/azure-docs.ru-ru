---
title: "Интерфейс командной строки Azure Service Fabric : sfctl is | Документация Майкрософт"
description: "Описание команд sfctl is интерфейса командной строки Azure Service Fabric."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: b611a447dd6669a09ca16c816de74acd7f3e8c7e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-is"></a>sfctl is
Запрос службы инфраструктуры и отправка команд для нее.

## <a name="commands"></a>Команды

|Get-Help|ОПИСАНИЕ|
| --- | --- |
|    command| Вызывает команду администрирования для заданного экземпляра службы инфраструктуры.|
|    query  | Вызывает запрос только для чтения к заданному экземпляру службы инфраструктуры.|


## <a name="sfctl-is-command"></a>sfctl is command
Вызывает команду администрирования для заданного экземпляра службы инфраструктуры.

Для кластеров с одним или несколькими экземплярами службы инфраструктуры этот API позволяет отправлять команды инфраструктуры конкретному экземпляру службы инфраструктуры. Доступные команды и соответствующий формат ответа различаются в зависимости от инфраструктуры, в которой выполняется кластер. Этот API поддерживает платформу Service Fabric. Он не предназначен для использования непосредственно в коде. .

### <a name="arguments"></a>Аргументы

|Аргумент|ОПИСАНИЕ|
| --- | --- |
| --command [обязательный параметр]| Текст команды для вызова. Содержимое команды зависит от инфраструктуры.  Значение по умолчанию: команда.|
| --service-id     | Идентификатор службы инфраструктуры. Это полное имя службы инфраструктуры без схемы универсального кода ресурса (URI) "fabric:". Этот параметр является обязательным только для кластеров, в которых выполняется более одного экземпляра службы инфраструктуры.|
| --timeout -t     | Время ожидания сервера в секундах.  Значение по умолчанию: 60.|

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|ОПИСАНИЕ|
| --- | --- |
| --debug          | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки.|
| --help -h        | Отображение этого справочного сообщения и выход.|
| --output -o      | Формат выходных данных.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.|
| --query          | Строка запроса JMESPath. Дополнительные сведения и примеры доступны на сайте http://jmespath.org.|
| --verbose        | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки.|

## <a name="sfctl-is-query"></a>sfctl is query
Вызывает запрос только для чтения к заданному экземпляру службы инфраструктуры.

Для кластеров с одним или несколькими экземплярами службы инфраструктуры этот API позволяет отправлять запросы к инфраструктуре конкретному экземпляру службы инфраструктуры. Доступные команды и соответствующий формат ответа различаются в зависимости от инфраструктуры, в которой выполняется кластер. Этот API поддерживает платформу Service Fabric. Он не предназначен для использования непосредственно в коде.

### <a name="arguments"></a>Аргументы

|Аргумент|ОПИСАНИЕ|
| --- | --- |
| --command [обязательный параметр]| Текст команды для вызова. Содержимое команды зависит от инфраструктуры.  Значение по умолчанию: запрос.|
| --service-id     | Идентификатор службы инфраструктуры. Это полное имя службы инфраструктуры без схемы универсального кода ресурса (URI) "fabric:". Этот параметр является обязательным только для кластеров, в которых выполняется более одного экземпляра службы инфраструктуры.|
| --timeout -t     | Время ожидания сервера в секундах.  Значение по умолчанию: 60.|

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|ОПИСАНИЕ|
| --- | --- |
| --debug          | Увеличение уровня детализации ведения журнала для отображения всех журналов отладки.|
| --help -h        | Отображение этого справочного сообщения и выход.|
| --output -o      | Формат выходных данных.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.|
| --query          | Строка запроса JMESPath. Дополнительные сведения доступны на сайте http://jmespath.org.|
| --verbose        | Повышение уровня детализации ведения журнала. Используйте параметр --debug, чтобы получить полные журналы отладки.|

## <a name="next-steps"></a>Дополнительная информация
- [Настройте](service-fabric-cli.md) интерфейс командной строки Service Fabric.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](/azure/service-fabric/scripts/sfctl-upgrade-application).