Эти вопросы и ответы применимы к подключениям P2S с использованием классической модели развертывания.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Какие клиентские операционные системы можно использовать для подключения типа "точка — сеть"?

Поддерживаются следующие клиентские операционные системы:

* Windows 7 (32-разрядная и 64-разрядная версии)
* Windows Server 2008 R2 (только 64-разрядная версия)
* Windows 8 (32-разрядная и 64-разрядная версии)
* Windows 8.1 (32-разрядная и 64-разрядная версии)
* Windows Server 2012 (только 64-разрядная версия)
* Windows Server 2012 R2 (только 64-разрядная версия)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Можно ли использовать для подключения "точка — сеть" любой VPN-клиент, поддерживающий SSTP?

Нет. Поддержка предоставляется только для версий операционной системы Windows, перечисленных выше.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Сколько конечных точек VPN-клиента можно настроить в конфигурации "точка — сеть"?

Поддерживаются до 128 VPN-клиентов с возможностью одновременного подключения к виртуальной сети.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Можно ли использовать корневой ЦС собственной внутренней системы PKI для подключения "точка — сеть"?

Да. Раньше поддерживались только самозаверяющие корневые сертификаты. Вы по-прежнему можете загружать до 20 корневых сертификатов.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Можно ли просматривать прокси-серверы и брандмауэры с использованием возможности "точка — сеть"?

Да. Для туннелирования через брандмауэры используется протокол SSTP (Secure Socket Tunneling Protocol). Этот туннель будет отображаться как HTTPs-соединение.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Если перезапустить клиентский компьютер, настроенный для подключения "точка — сеть", произойдет ли автоматическое переподключение VPN?

По умолчанию клиентский компьютер не восстанавливает VPN-подключение автоматически.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Поддерживает ли подключение "точка — сеть" автоматическое повторное подключение и DDNS для VPN-клиентов?

В конфигурациях VPN "точка — сеть" в настоящее время не поддерживаются автоматическое повторное подключение и DDNS.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Могут ли сосуществовать в одной виртуальной сети конфигурации "сеть — сеть" и "точка — сеть"?

Да. Оба этих решения будут работать, если ваш шлюз использует тип VPN на основе маршрутов. В классической модели развертывания требуется динамический шлюз. Мы не поддерживаем подключения "точка — сеть" для VPN-шлюзов статической маршрутизации или шлюзов, использующих командлет `-VpnType PolicyBased`.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Можно ли настроить клиент "точка — сеть" для подключения к нескольким виртуальным сетям одновременно?

Да, это возможно. Но виртуальные сети не могут иметь перекрывающиеся префиксы IP-адресов и адресные пространства "точка — сеть" не должны перекрываться между виртуальными сетями.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>На какую пропускную способность можно рассчитывать в конфигурациях подключения "сеть — сеть" и "точка — сеть"?

Сложно поддерживать конкретную пропускную способность для туннелей VPN. IPsec и SSTP — это надежно зашифрованные протоколы VPN. Пропускная способность также зависит от задержки и пропускной способности между локальной сетью и Интернетом.
