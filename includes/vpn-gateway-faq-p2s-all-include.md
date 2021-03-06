### <a name="supportedclientos"></a>Какие клиентские операционные системы можно использовать для подключения типа "точка — сеть"?

Поддерживаются следующие клиентские операционные системы:

* Windows 7 (32-разрядная и 64-разрядная версии)
* Windows Server 2008 R2 (только 64-разрядная версия)
* Windows 8 (32-разрядная и 64-разрядная версии)
* Windows 8.1 (32-разрядная и 64-разрядная версии)
* Windows Server 2012 (только 64-разрядная версия)
* Windows Server 2012 R2 (только 64-разрядная версия)
* Windows Server 2016 (только 64-разрядная версия)
* Windows 10
* OSX версии 10.11 для Mac (El Capitan);
* macOS версии 10.12 для Mac (Sierra).

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Сколько конечных точек VPN-клиента можно настроить в конфигурации "точка — сеть"?

Поддерживаются до 128 VPN-клиентов с возможностью одновременного подключения к виртуальной сети.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Можно ли просматривать прокси-серверы и брандмауэры с использованием возможности "точка — сеть"?

Azure поддерживает два варианта VPN-подключения типа "точка — сеть":

* По протоколу SSTP (Secure Sockets Tunneling Protocol). SSTP — это разработанное корпорацией Майкрософт решение на основе SSL, которое позволяет проходить через брандмауэры, так как большинство брандмауэров открывают для SSL TCP-порт 443.

* По протоколу IKEv2 для VPN. IKEv2 для VPN — это стандартное решение для VPN-подключения IPsec, которое использует порты UDP 500 и 4500, а также протокол IP 50. Эти порты не всегда открываются в брандмауэре, поэтому есть вероятность, что IKEv2 для VPN не сможет пройти через прокси-серверы и брандмауэры.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Если перезапустить клиентский компьютер, настроенный для подключения "точка — сеть", произойдет ли автоматическое переподключение VPN?

По умолчанию клиентский компьютер не восстанавливает VPN-подключение автоматически.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Поддерживает ли подключение "точка — сеть" автоматическое повторное подключение и DDNS для VPN-клиентов?

В конфигурациях VPN "точка — сеть" в настоящее время не поддерживаются автоматическое повторное подключение и DDNS.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Могут ли сосуществовать в одной виртуальной сети конфигурации "сеть — сеть" и "точка — сеть"?

Да. Для модели развертывания с помощью Resource Manager требуется VPN-шлюз с маршрутизацией на основе маршрутов. В классической модели развертывания требуется динамический шлюз. Подключения типа "точка — сеть" для VPN-шлюзов со статической маршрутизацией или маршрутизацией на основе политик не поддерживаются.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Можно ли настроить клиент "точка — сеть" для подключения к нескольким виртуальным сетям одновременно?

Нет. Клиент для подключения типа "точка — сеть" может подключаться только к ресурсам в виртуальной сети, в которой находится шлюз виртуальной сети.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>На какую пропускную способность можно рассчитывать в конфигурациях подключения "сеть — сеть" и "точка — сеть"?

Сложно поддерживать конкретную пропускную способность для туннелей VPN. IPsec и SSTP — это надежно зашифрованные протоколы VPN. Пропускная способность также зависит от задержки и пропускной способности между локальной сетью и Интернетом. Для VPN-шлюза, который используется только для VPN-подключений типа "точка — сеть" по протоколу IKEv2, общая пропускная способность зависит от номера SKU шлюза. Дополнительные сведения о пропускной способности см. в статье о [номерах SKU шлюзов](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Можно ли использовать для подключения "точка — сеть" любой программный VPN-клиент, поддерживающий SSTP и (или) IKEv2?

Нет. Вы можете использовать только собственный VPN-клиент для SSTP в Windows и собственный VPN-клиент для IKEv2 в Mac. См. список поддерживаемых клиентских операционных систем.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Azure поддерживает протокол IKEv2 для VPN в Windows?

Пользователи могут подключаться к Azure с помощью встроенного VPN-клиента Windows, который поддерживает IKEv2. Но подключения IKEv2 с устройства Windows не будут работать в следующем случае.

  Если устройство пользователя содержит большое количество доверенных корневых сертификатов, размер полезных данных сообщения во время обмена данными IKE достаточно большой, что вызывает фрагментацию уровня IP-адресов. Фрагменты отклоняются на стороне Azure, что приводит к сбою подключения. Сложно оценить, при каком количестве сертификатов возникает эта проблема. В итоге не гарантируется, что подключения IKEv2 с устройств Windows будут работать. При настройке протоколов SSTP и IKEv2 в смешанной среде (состоящей из устройств Windows и Mac) профиль VPN в Windows всегда сначала пытается установить туннель IKEv2. Если происходит сбой из-за проблемы, описанной выше, то профиль возвращается к использованию протокола SSTP.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Какие еще платформы, кроме Windows и Mac, Azure поддерживает для VPN-подключений типа "точка — сеть"?

Для VPN-подключений типа "точка — сеть" Azure поддерживает только Windows и Mac.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>У меня уже развернут VPN-шлюз Azure. Можно включить RADIUS или IKEv2 VPN в его?

Да, можно включить эти новые функции для уже развернутых шлюзов с помощью Powershell или портал Azure, при условии, что шлюз SKU, в которых используется поддерживает RADIUS и/или IKEv2. Например VPN-шлюз Basic SKU не поддерживает RADIUS или IKEv2.
