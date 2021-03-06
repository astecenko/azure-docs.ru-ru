1. Войдите на [портал Azure].
2. Щелкните **+Создать** > **Интернет+мобильные устройства** > **Мобильное приложение**, а затем введите имя серверной части мобильного приложения.
3. В поле **Группа ресурсов**выберите существующую группу ресурсов или создайте новую (с тем же именем, что и у приложения). 
   
    Выберите другой план службы приложений или создайте новый. Дополнительные сведения о планах службы приложений, а также о создании плана другой ценовой категории и в другом расположении см. в статье [Подробный обзор планов службы приложений Azure](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. В качестве **плана службы приложений**используется план по умолчанию (для [уровня "Стандартный"](https://azure.microsoft.com/pricing/details/app-service/)). Вы также можете [создать план](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan) или выбрать другой. Параметры плана службы приложений определяют [расположение, функции, стоимость и вычислительные ресурсы](https://azure.microsoft.com/pricing/details/app-service/) , связанные с вашим приложением. 
   
    Выбрав план, нажмите кнопку **Создать**. Будет создана серверная часть мобильного приложения, 
5. В колонке **Параметры** серверной части нового мобильного приложения щелкните **Быстрый запуск** > клиентская платформа приложения > **Connect a database** (Подключить базу данных). 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. В колонке **Add data connection** (Добавление подключения данных) щелкните **База данных SQL** > **Создать базу данных**, а затем введите **имя** базы данных в соответствующем поле, выберите ценовую категорию и щелкните **Сервер**.  Эту новую базу данных можно использовать повторно. Если в этом регионе у вас уже есть база данных, вы можете выбрать пункт **Использовать существующую базу данных**. Из-за затрат на пропускную способность и более длительных задержек мы не рекомендуем использовать базу данных в другом расположении.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. В колонке **Новый сервер** введите уникальное **имя сервера** в соответствующем поле, укажите имя для входа и пароль, установите флажок **Разрешить службам Azure доступ к серверу** и нажмите кнопку **ОК**. Будет создана база данных.
8. Вернитесь в колонку **Add data connection** (Добавление подключения данных), щелкните **Строка подключения**, введите имя для входа и пароль для базы данных, а затем нажмите кнопку **ОК**. Прежде чем продолжать, подождите несколько минут, пока будет выполнено развертывание базы данных.

<!-- URLs. -->
[портал Azure]: https://portal.azure.com/
