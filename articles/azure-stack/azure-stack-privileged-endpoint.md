---
title: "Использование привилегированной конечной точки в Azure Stack | Документация Майкрософт"
description: "Здесь описано использование привилегированной конечной точки в Azure Stack (для оператора Azure Stack)."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 80c3f248edb40b66e3177c512f3caf77295c6c5d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2018
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Использование привилегированной конечной точки в Azure Stack

*Область применения: интегрированные системы Azure Stack и комплект разработки Azure Stack*

Для повседневных задач управления оператору Azure Stack следует использовать портал администрирования, PowerShell или API-интерфейсы Azure Resource Manager. Но для выполнения некоторых менее распространенных операций понадобится использовать *привилегированную конечную точку*. Эта конечная точка является предварительно настроенной удаленной консолью PowerShell, которая предоставляет достаточно возможностей для выполнения требуемой задачи. Конечная точка использует PowerShell JEA (достаточно для администрирования) для предоставления ограниченного набора командлетов. Для доступа к привилегированной конечной точке и вызова ограниченного набора командлетов используется учетная запись с низким уровнем привилегий. Учетные записи администратора не требуются. Написание скриптов для обеспечения дополнительной безопасности не допускается.

Привилегированную конечную точку можно использовать для выполнения следующих задач:

- выполнение задач низкого уровня, таких как [сбор журналов диагностики](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool);
- выполнение многих задач по интеграции центра обработки данных после развертывания для интегрированных систем, например добавление DNS-серверов после развертывания, настройка интеграции Graph, интеграции служб федерации Active Directory (AD FS), смена сертификатов и т. д.;
- работа со службой поддержки для получения временного доступа высокого уровня для проведения глубокой диагностики интегрированной системы. 

В привилегированной конечной точке регистрируется каждое действие (и его соответствующие выходные данные), выполняемое в сеансе PowerShell. Это обеспечивает полную прозрачность и аудит операций. Эти файлы журналов можно сохранить для проведения аудита в дальнейшем.

> [!NOTE]
> В комплекте разработки Azure Stack (ASDK) некоторые команды, доступные в привилегированной конечной точке, можно выполнять непосредственно из сеанса PowerShell на узле комплекта разработки. Тем не менее вы можете проверить некоторые операции с использованием привилегированной конечной точки, например сбор данных журналов, так как это единственный доступный метод для выполнения определенных операций в среде интегрированных систем.

## <a name="access-the-privileged-endpoint"></a>Доступ к привилегированной конечной точке

Доступ к привилегированной конечной точке осуществляется через удаленный сеанс PowerShell на виртуальной машине, на которой размещается привилегированная конечная точка. В ASDK эта виртуальная машина называется AzS-ERCS01. Если вы используете интегрированную систему, на разных узлах виртуальной машины запущены три экземпляра привилегированной конечной точки (*Prefix*-ERCS01, *Prefix*-ERCS02 или *Prefix*-ERCS03) для обеспечения устойчивости. 

Перед началом этой процедуры для интегрированной системы убедитесь в наличии доступа к привилегированной конечной точке по IP-адресу или через DNS. После первоначального развертывания Azure Stack доступ к привилегированной конечной точке можно получить только по IP-адресу, так как интеграция DNS еще не настроена. Поставщик OEM предоставит вам JSON-файл с именем "AzureStackStampDeploymentInfo", содержащий IP-адреса привилегированной конечной точки.

Мы рекомендуем подключаться к привилегированной конечной точке только с узла жизненного цикла оборудования или выделенного заблокированного компьютера, например [рабочей станции с привилегированным доступом](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Выполните одно из следующих действий в зависимости от вашей среды:

    - В интегрированной системе выполните следующую команду, чтобы добавить привилегированную конечную точку в качестве доверенного узла на узел жизненного цикла оборудования или рабочую станцию с привилегированным доступом.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Если вы используете ADSK, войдите на узел комплекта разработки.

2. На узле жизненного цикла оборудования или рабочей станции с привилегированным доступом откройте сеанс Windows PowerShell с повышенными привилегиями. Выполните следующие команды для создания удаленного сеанса на виртуальной машине, на которой размещается привилегированная конечная точка:
 
    - В интегрированной системе:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      Параметр `ComputerName` может быть IP-адресом или DNS-именем одной из виртуальных машин, на которых размещена привилегированная конечная точка. 
    - При использовании ADSK:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   При появлении запроса используйте следующие учетные данные:

      - **Имя пользователя.** Укажите учетную запись CloudAdmin в формате **&lt;*домен Azure Stack*&gt;\cloudadmin**. (При использовании ASDK имя пользователя — **azurestack\cloudadmin**.)
      - **Пароль.** Введите пароль, который использовался во время установки учетной записи администратора домена AzureStackAdmin.
    
3.  После подключения запрос изменится на **[*IP-адрес или имя виртуальной машины ERCS*]: PS>** или на **[azs-ercs01]: PS>**, в зависимости от среды. Теперь запустите командлет `Get-Command`, чтобы просмотреть список доступных командлетов.

    ![Выходные данные командлета Get-Command, отображающие список доступных команд](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Многие из этих командлетов предназначены только для сред интегрированных систем (например, командлеты, относящиеся к интеграции центра обработки данных). В ASDK проверены следующие командлеты:

    - Clear-Host;
    - Close-PrivilegedEndpoint;
    - Exit-PSSession;
    - Get-AzureStackLog;
    - Get-AzureStackStampInformation;
    - Get-Command;
    - Get-FormatData;
    - Get-Help
    - Get-ThirdPartyNotices;
    - Measure-Object;
    - New-CloudAdminUser;
    - Out-Default;
    - Remove-CloudAdminUser;
    - Select-Object;
    - Set-CloudAdminUserPassword;
    - Test-AzureStack
    - Stop-AzureStack;
    - Get-ClusterLog.

4.  Так как написание скриптов не разрешается, нельзя заполнять значения параметров нажатием клавиши TAB. Чтобы получить список параметров для определенного командлета, выполните следующую команду:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    При попытке выполнить операцию скрипта любого типа она завершается с ошибкой **ScriptsNotAllowed**. Это ожидаемое поведение.

## <a name="close-the-privileged-endpoint-session"></a>Закрытие сеанса привилегированной конечной точки

 Как упоминалось ранее, в привилегированной конечной точке регистрируется каждое действие (и его соответствующие выходные данные), выполняемое в сеансе PowerShell. Следует закрыть сеанс с помощью командлета `Close-PrivilegedEndpoint`. Этот командлет закрывает конечную точку надлежащим образом, а также передает файлы журнала во внешнюю общую папку для хранения.

Чтобы закрыть сеанс конечной точки:

1. Создайте внешнюю общую папку, доступную для привилегированной конечной точки. В среде комплекта разработки можно просто создать общую папку на узле комплекта разработки.
2. Запустите командлет `Close-PrivilegedEndpoint`. 
3. Будет предложено ввести путь для хранения файла журнала расшифровки. Укажите созданную ранее общую папку в формате &#92;&#92;*имя_сервера*&#92;*имя_общей_папки*. Если не указать путь, командлет завершится сбоем и сеанс останется открытым. 

    ![Выходные данные командлета Close-PrivilegedEndpoint с указанием целевого пути к файлу расшифровки](media/azure-stack-privileged-endpoint/closeendpoint.png)

После того как файлы журнала расшифровки успешно переданы в общую папку, они автоматически удаляются из привилегированной конечной точки. Если завершить сеанс привилегированной конечной точки, используя командлеты `Exit-PSSession` либо `Exit`, или просто закрыть консоль PowerShell, журналы расшифровки не будут перенесены в общую папку. Они останутся в привилегированной конечной точке. При следующем запуске командлета `Close-PrivilegedEndpoint` и добавлении общей папки журналы расшифровки из предыдущих сеансов будут также перенесены.

## <a name="next-steps"></a>Дополнительная информация
[Azure Stack diagnostic tools](azure-stack-diagnostics.md) (Средства диагностики Azure Stack)







