### <a name="prerequisites"></a>Предварительные требования
Вам понадобится учетная запись [служебной шины](https://azure.microsoft.com/services/service-bus/).  

Прежде чем использовать учетную запись служебной шины Azure в приложении логики, необходимо авторизовать его для подключения к этой учетной записи. К счастью, это можно легко сделать из приложения логики на портале Azure.  

Ниже приведены указания по авторизации приложения логики для подключения к учетной записи служебной шины.  

1. Чтобы создать подключение к служебной шине, в конструкторе приложений логики в раскрывающемся списке выберите параметр **Показать API, управляемые Майкрософт**. Затем в поле поиска введите данные **служебной шины**. Выберите триггер или действие, которые хотите использовать.  
    ![Подключение к служебной шине, изображение 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Если вы ранее не создавали подключения к служебной шине, вам будет предложено ввести учетные данные служебной шины. Эти учетные данные используются для авторизации приложения логики, чтобы оно могло подключиться к вашей учетной записи служебной шины и получить доступ к ее данным. Соединителю служебной шины требуется строка подключения для пространства имен служебной шины. Кроме того, требуется разрешение на **управление**. Чтобы определить, для чего предназначена строка подключения (для пространства имен или какой-либо определенной сущности), проверьте наличие параметра `EntityPath`. Если он есть, строка подключения будет неправильной для приложения логики.  
    ![Строка подключения к служебной шине](./media/connectors-create-api-servicebus/connectionstring.png)
3. После получения строку подключения для пространства имен можно использовать для подключения к API в приложениях логики.  
    ![Подключение к служебной шине. Изображение 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Подключение создано, и теперь вы можете перейти к другим действиям в приложении логики.  
    ![Подключение к служебной шине. Изображение 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

