---
title: "Настройка устройства для службы подготовки устройств для Центра Интернета вещей Azure | Документация Майкрософт"
description: "Настройка устройства для подготовки через службу подготовки устройств для Центра Интернета вещей во время производства устройства."
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 835a54f147b9ea543df21e7dfeb226ac42aceda3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Настройка устройства для подготовки с помощью службы подготовки устройств для Центра Интернета вещей

В предыдущем руководстве вы узнали, как настроить службу подготовки устройств для Центра Интернета вещей, чтобы автоматически подготавливать устройства в Центре Интернета вещей. Это руководство содержит инструкции по настройке устройства во время производства, чтобы можно было настроить службу подготовки устройств для своего устройства на основе [аппаратного модуля безопасности (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security) и устройство могло подключаться к службе подготовки устройств при первоначальной загрузке. В этом руководстве рассматриваются такие процессы:

> [!div class="checklist"]
> * выбор аппаратного модуля безопасности;
> * создание клиентского пакета SDK, подготавливающего устройства, для выбранного HSM;
> * извлечение артефактов безопасности;
> * настройка конфигурации службы подготовки устройств на устройстве.

## <a name="prerequisites"></a>Технические условия

Прежде чем продолжить, создайте экземпляр службы подготовки устройств и Центр Интернета вещей с помощью инструкций, упомянутых в руководстве [Configure cloud resources for device provisioning with the IoT Hub Device Provisioning Service](./tutorial-set-up-cloud.md) (Настройка облачных ресурсов для подготовки устройств с помощью службы подготовки устройств для Центра Интернета вещей).


## <a name="select-a-hardware-security-module"></a>Выбор аппаратного модуля безопасности

[Клиентский пакет SDK службы подготовки устройств](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) поддерживает два типа аппаратных модулей безопасности (или HSM): 

- [Доверенный платформенный модуль (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module).
    - Доверенный платформенный модуль (TPM) является установленным стандартом для большинства платформ устройств на базе Windows, а также нескольких устройств на базе Linux и Ubuntu. Производитель устройства может выбрать этот HSM, если устройства выполняются под управлением любой из этих операционных систем, а также если необходим установленный стандарт HSM. С микросхемами доверенного платформенного модуля (TPM) можно только регистрировать каждое устройство отдельно в службе подготовки устройств. В целях разработки можно использовать симулятор доверенного платформенного модуля на компьютере разработки Windows или Linux.

- Аппаратные модули безопасности на основе [X.509](https://cryptography.io/en/latest/x509/). 
    - Аппаратные модули безопасности на основе X.509 — это относительно новые микросхемы, которые в настоящее время работают в Майкрософт с использованием микросхем RIoT или DICE, которые реализовывают сертификаты X.509. С помощью микросхем X.509 можно выполнять массовую регистрацию на портале. Они также поддерживают некоторые операционные системы, отличные от Windows, такие как embedOS. В целях разработки клиентский пакет SDK службы подготовки устройств поддерживает симулятор устройств X.509. 

Производителю устройств необходимо выбрать модули безопасности и микросхемы, основанные на одном из перечисленных выше типов. Другие типы аппаратных модулей безопасности в клиентском пакете SDK службы подготовки устройств в настоящее время не поддерживаются.   


## <a name="build-device-provisioning-client-sdk-for-the-selected-hsm"></a>Создание клиентского пакета SDK, подготавливающего устройства, для выбранного HSM

Клиентский пакет SDK службы подготовки устройств помогает реализовать выбранный механизм безопасности в программном обеспечении. Ниже показано, как использовать пакет SDK для выбранной микросхемы аппаратного модуля безопасности:

1. Если вы следовали [краткому руководству по созданию виртуального устройства](./quick-create-simulated-device.md), все готово к созданию пакета SDK. Если нет, выполните первые четыре шага из раздела [Подготовка среды разработки](./quick-create-simulated-device.md#setupdevbox). На этих шагах клонируется репозиторий GitHub для клиентского пакета SDK службы подготовки устройств, а также устанавливается средство сборки `cmake`. 

1. Создайте пакет SDK для типа HSM, выбранного для устройства, с помощью одной из следующих команд в командной строке:
    - Для устройств с доверенным платформенным модулем:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Для симулятора доверенного платформенного модуля:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

    - Для устройств X.509 и симулятора:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

1. Пакет SDK по умолчанию поддерживает устройства под управлением Windows или Ubuntu для доверенных платформенных модулей и аппаратных модулей безопасности X.509. Для таких поддерживаемых аппаратных модулей безопасности перейдите к разделу [Извлечение артефактов безопасности](#extractsecurity) ниже. 
 
## <a name="support-custom-tpm-and-x509-devices"></a>Поддержка пользовательского доверенного платформенного модуля и устройств X.509

Клиентский пакет SDK системы подготовки устройств не поддерживает по умолчанию любые доверенные платформенные модули или устройства X.509, которые не работают под управлением Windows или Ubuntu. Для таких устройств необходимо написать пользовательский код для конкретной микросхемы HSM, как показано ниже:

### <a name="develop-your-custom-repository"></a>Разработка пользовательского репозитория

1. Разработки библиотеки для доступа к имеющемуся аппаратному модулю безопасности. Для этого проекта необходимо создать статическую библиотеку пакета SDK для подготовки устройств.
1. Библиотека должна реализовывать функции, определенные в следующем файле заголовка: a. Для пользовательских TPM реализации функций, определенных в [документа пользовательских HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api).
    Б. Для пользовательских X.509 реализации функций, определенных в [документа пользовательских HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api). 

### <a name="integrate-with-the-device-provisioning-service-client"></a>Интеграция с клиентом службы подготовки устройств

После библиотеки успешно выполняет построение на собственный, можно переместить в центром IOT C-SDK и связать библиотеки:

1. Укажите пользовательский репозиторий GitHub HSM, путь к библиотеке и ее имя в следующей команде cmake:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
1. Откройте пакет SDK в Visual Studio и создайте его. 

    - Процесс построения будет компилироваться библиотека пакета SDK.
    - Пакет SDK попытается связать пользовательский HSM, определенный в команде cmake.

1. Запустите пример `\azure-iot-sdk-c\provisioning_client\samples\prov_dev_client_ll_sample\prov_dev_client_ll_sample.c`, чтобы проверить правильность реализации HSM.

<a id="extractsecurity"></a>
## <a name="extract-the-security-artifacts"></a>Извлечение артефактов безопасности

Следующим шагом является извлечение артефактов безопасности для аппаратного модуля безопасности на устройстве.

1. Для устройства с доверенным платформенным модулем необходимо узнать связанный с ним **ключ подтверждения** у производителя микросхемы доверенного платформенного модуля. Можно получить уникальный **ИД регистрации** для устройства TPM с помощью хэширования ключа подтверждения. 
2. Для устройства X.509 необходимо получить сертификаты, выпущенные для устройства — сертификаты конечных субъектов для отдельных регистраций устройств и корневые сертификаты для групповых регистраций устройств.

Такие артефакты безопасности необходимы для регистрации устройств в службе подготовки устройств. Служба подготовки ожидает загрузки этих устройств и подключается к ним в любой момент времени в будущем. Дополнительные сведения о том, как использовать эти артефакты безопасности для создания регистрации, см. в статье [How to manage device enrollments in the IoT Hub Device Provisioning Service](how-to-manage-enrollments.md) (Как управлять регистрацией устройств в службе подготовки устройств для Центра Интернета вещей). 

При первой загрузке устройства клиентский пакет SDK взаимодействует с микросхемой, извлекая артефакты безопасности с устройства, и проверяет регистрацию в службе подготовки устройств. 


## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Настройка конфигурации службы подготовки устройств на устройстве

Последний шаг в процессе производства устройства заключается в написании приложения, использующего клиентский пакет SDK службы подготовки устройств для регистрации устройства в службе. Этот пакет SDK предоставляет такие API для приложений:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Не забудьте инициализировать переменные `uri` и `id_scope`, как указано в разделе [Имитация последовательности первой загрузки для устройства](./quick-create-simulated-device.md#firstbootsequence) этого краткого руководства, перед их использованием. API регистрации клиента подготовки устройства `Prov_Device_LL_Create` подключается к глобальной службе подготовки устройств. *Область идентификаторов* создается службой и гарантирует уникальность. Она неизменяемая и используется для уникальной идентификации идентификаторов регистрации. `iothub_uri` позволяет API регистрации клиента подготовки устройства `IoTHubClient_LL_CreateFromDeviceAuth` подключаться к правильному Центру Интернета вещей. 


Эти API помогают устройству подключаться и регистрироваться в службе подготовки устройств, когда она загружается, получать сведения о Центре Интернета вещей и подключаться к нему. В файле `provisioning_client/samples/prov_client_ll_sample/prov_client_ll_sample.c` показано, как использовать эти API. Как правило, необходимо создать такую платформу для регистрации клиента:

```C
static const char* global_uri = "global.azure-devices-provisioning.net";
static const char* id_scope = "[ID scope for your provisioning service]";
...
static void register_callback(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* context)
{
    USER_DEFINED_INFO* user_info = (USER_DEFINED_INFO *)user_context;
    ...
    user_info. reg_complete = 1;
}
static void registation_status(DPS_REGISTRATION_STATUS reg_status, void* user_context)
{
}
int main()
{
    ...
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    prov_dev_security_init(hsm_type); // initialize your HSM 

    prov_transport = Prov_Device_HTTP_Protocol;
    
    PROV_CLIENT_LL_HANDLE handle = Prov_Device_LL_Create(global_uri, id_scope, prov_transport); // Create your provisioning client

    if (Prov_Client_LL_Register_Device(handle, register_callback, &user_info, register_status, &user_info) == IOTHUB_DPS_OK) {
        do {
        // The register_callback is called when registration is complete or fails
            Prov_Client_LL_DoWork(handle);
        } while (user_info.reg_complete == 0);
    }
    Prov_Client_LL_Destroy(handle); // Clean up the Provisioning client
    ...
    iothub_client = IoTHubClient_LL_CreateFromDeviceAuth(user_info.iothub_uri, user_info.device_id, transport); // Create your IoT hub client and connect to your hub
    ...
}
```

Вы можете уточнить клиентское приложение регистрации службы подготовки устройств сначала с помощью имитированного устройства, а затем — настройки службы тестирования. Когда приложение будет работать в тестовой среде, его можно создать для конкретного устройства и скопировать исполняемый файл в образ устройства. Не запускайте устройство. Сначала его необходимо [зарегистрировать в службе подготовки устройств](./tutorial-provision-device-to-hub.md#enrolldevice). Дополнительные сведения об этом процессе см. в следующих шагах. 

## <a name="clean-up-resources"></a>Очистка ресурсов

На этом этапе может понадобиться настроить службу подготовки устройств и Центр Интернета вещей на портале. Если вы хотите прервать настройку подготовки устройства и (или) приостановить использование любой из этих служб, рекомендуется выключить их, чтобы избежать лишних затрат.

1. В меню слева на портале Azure щелкните **Все ресурсы** и откройте службу подготовки устройств. В верхней части колонки **Все ресурсы** щелкните **Удалить**.  
1. В меню слева на портале Azure нажмите кнопку **Все ресурсы** и выберите свой Центр Интернета вещей. В верхней части колонки **Все ресурсы** щелкните **Удалить**.  


## <a name="next-steps"></a>Дальнейшие действия
Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * выбор аппаратного модуля безопасности;
> * создание клиентского пакета SDK, подготавливающего устройства, для выбранного HSM;
> * извлечение артефактов безопасности;
> * настройка конфигурации службы подготовки устройств на устройстве.

Перейдите к следующему руководству, чтобы узнать, как подготовить свое устройство в Центре Интернета вещей, зарегистрировав его в службе подготовки устройств для Центра Интернета вещей Azure, позволяющей выполнить автоматическую подготовку.

> [!div class="nextstepaction"]
> [Provision the device to an IoT hub using the Azure IoT Hub Device Provisioning Service](tutorial-provision-device-to-hub.md) (Подготовка устройства в Центре Интернета вещей с помощью службы подготовки устройств для Центра Интернета вещей).

