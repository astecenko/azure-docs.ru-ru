## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Реакция APIM прокси-сервера с использованием сертификата SSL в подтверждения TLS

### <a name="clients-calling-with-sni-header"></a>Клиенты, вызывающие с заголовком SNI
Если у клиента имеется одно или несколько пользовательских доменов, настроен прокси, APIM может отвечать на запросы HTTPS из пользовательских доменов (например, contoso.com) и домен по умолчанию (например, apim-service-name.azure-api.net). На основании информации в заголовке Указание имени сервера (SNI) APIM отвечает соответствующий сертификат.

### <a name="clients-calling-without-sni-header"></a>Клиенты, вызывающие без заголовка SNI
Если пользователь использует клиент, который не отправляет [SNI](https://tools.ietf.org/html/rfc6066#section-3) заголовок, APIM создает ответов в зависимости от следующую логику:

* Если у службы есть только один пользовательский домен настроен прокси, сертификат по умолчанию является сертификат, выданный для пользовательского домена прокси-сервера.
* Если служба настроена несколько пользовательских доменов для прокси-сервера (поддерживается только в **Premium** уровня), клиент можно указать, какой сертификат должен быть сертификат по умолчанию. Чтобы установить сертификат по умолчанию [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#definitions_hostnameconfiguration) свойство должно быть установлено значение true («defaultSslBinding»: «true»). Если клиент не задано свойство, сертификат по умолчанию является сертификат, выданный домен по умолчанию прокси-сервера, размещенного в *.azure-api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Поддержка запрос PUT или POST с большой объем данных

APIM прокси-сервер поддерживает запрос с большой объем данных, при использовании протокола HTTPS клиентских сертификатов (например, полезные данные > 40 КБ). Чтобы предотвратить замораживание запроса на сервере, клиентам можно задать свойство [«negotiateClientCertificate»: «true»](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#definitions_hostnameconfiguration) на имя узла прокси-сервера. Если свойство имеет значение true, клиент запроса сертификата во время подключения SSL/TLS, перед любой exchange запроса HTTP. Поскольку этот параметр применяется в **имя узла прокси-сервера** уровня, все запросы на подключение запрашивает сертификат клиента. Клиенты можно настроить до 20 пользовательских доменов для прокси-сервера (поддерживается только в **Premium** уровня) и обойти это ограничение.

