---
title: "Создание циклов и областей или индивидуальная обработка рабочих процессов в Azure Logic Apps | Документация Майкрософт"
description: "Узнайте о создании циклов для выполнения итерации по данным, группировании действий в области и индивидуальной обработке данных для запуска дополнительных рабочих процессов в Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9cdbe4a12a0b16341a1e52f176901045baf327b5
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a>Циклы, области действия и индивидуальная обработка приложений логики
  
Среда Logic Apps предоставляет несколько способов работы с массивами, коллекциями, пакетами и циклами в рабочем процессе.
  
## <a name="foreach-loop-and-arrays"></a>Цикл и массивы ForEach
  
Приложения логики позволяют запускать циклы с использованием набора данных и выполнять определенное действие с каждым из них.  Для перебора коллекции можно сделать через `foreach` действие.  В конструкторе, можно добавить для каждого цикла.  Выбрав массив для перебора, можно приступать к добавлению действий.  Вы можете добавить несколько действий для каждого цикла foreach.  Один раз в цикле, вы сможете указать, что должно произойти на каждое значение в массиве.

  Этот пример отправляет сообщение электронной почты для каждого адреса электронной почты, содержащий «microsoft.com». Если используется представление кода, можно указать для каждого цикла, как в следующем примере:

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  Объект `foreach` действия может выполнять итерацию массивов с тысячами сущностей.  Параллельного выполнения итераций по умолчанию.  В разделе [ограничения и конфигурации](logic-apps-limits-and-config.md) подробные сведения об ограничениях массива и параллелизм.

### <a name="sequential-foreach-loops"></a>Последовательные циклы ForEach

Чтобы цикл ForEach мог выполняться последовательно, необходимо добавить параметр операции `Sequential`.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Цикл Until
  
  Действие или серию действий можно выполнять, пока не будет выполнено определенное условие.  Наиболее распространенный сценарий использования пока цикл осуществляет вызов конечной точки, пока не будет получен ответ, который вы ищете.  В конструкторе можно добавить цикл until.  После добавления действия в цикл можно задать условия выхода, а также ограничения для цикла.
  
  В этом примере вызывает конечную точку HTTP, пока текст ответа имеет значение «Завершено».  Он завершается при любом: 
  
  * HTTP-ответ имеет состояние "Выполнен".
  * Предпринял один час
  * Действие было выполнено 100 раз.
  
  Если используется представление кода, можно указать до цикла, как в следующем примере:
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn и индивидуальная обработка

Иногда триггер может получить массив элементов, которые нужно обработать индивидуально и запустить для каждого собственный рабочий процесс.  Это debatching можно выполнить с помощью `spliton` команды.  По умолчанию, если ваш триггер swagger указывает полезных данных, который является массивом `spliton` добавляется. `spliton` Команда запускает выполнения каждого элемента в массиве.  SplitOn могут добавляться только к триггеру, в которой могут быть настроены вручную или переопределении. Одновременно использовать `spliton` и реализовать шаблон синхронных ответов невозможно.  Любой рабочий процесс, который называется имеет `response` действия в дополнение к `spliton` выполняется асинхронно и отправляет непосредственно `202 Accepted` ответа.  

  В этом примере получает массив элементов и debatches в каждой строке. В следующем примере SplitOn можно указать в представлении кода.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Области действия

Последовательности действий можно группировать, используя область действий.  Области можно использовать для реализации обработки исключений.  Добавьте новую область в конструкторе и начните добавлять в нее действия.  Можно определить области в представлении кода имеет следующий вид:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
