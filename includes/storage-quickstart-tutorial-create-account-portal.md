## <a name="create-a-storage-account-by-using-the-azure-portal"></a>Создание учетной записи хранения с помощью портала Azure

Сначала создайте учетную запись хранения общего назначения, которая будет использоваться в этом кратком руководстве. 

1. Перейдите на [портал Azure](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) и войдите в систему, используя свою учетную запись Azure. 
2. Введите уникальное имя учетной записи хранения. Помните о следующих правилах при назначении имени учетной записи хранения.
    - Имя должно содержать от 3 до 24 символов.
    - Имя может содержать только цифры и строчные буквы.
3. Установите следующие значения по умолчанию: 
    - Для параметра **Модель развертывания** установлено значение **Resource Manager**.
    - **Account kind** (Тип учетной записи) имеет значение **Общего назначения**.
    - **Производительность** имеет значение **Стандартный**.
    - **Репликация** имеет значение **Locally Redundant storage (LRS)** (Локально избыточное хранилище (LRS)).
4. Выберите свою подписку. 
5. Для **группы ресурсов** создайте еще одну группу ресурсов и присвойте ей уникальное имя. 
6. Выберите расположение, которое будет использоваться в вашей учетной записи хранения.
7. Выберите **Закрепить на панели мониторинга** и нажмите кнопку **Создать**, чтобы создать учетную запись хранения. 

Созданная учетная запись хранения закрепляется на панели мониторинга. Выберите ее, чтобы открыть. В разделе **Параметры** выберите **Ключи доступа**. Выберите первичный ключ и скопируйте связанную строку подключения в буфер обмена. Затем вставьте строку в текстовый редактор для последующего использования.
