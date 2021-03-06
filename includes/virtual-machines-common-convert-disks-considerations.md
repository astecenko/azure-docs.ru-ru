
* Процесс преобразования требует перезагрузки виртуальной машины, поэтому запланируйте перенос виртуальных машин на предварительно установленный период обслуживания. 

* Преобразование необратимо. 

* Не забудьте протестировать преобразование. Перенесите тестовую виртуальную машину перед выполнением миграции в рабочей среде.

* Во время преобразования виртуальная машина освобождается. При запуске после преобразования она получает новый IP-адрес. При необходимости виртуальной машине можно [назначить общедоступный IP-адрес](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Исходные VHD и учетная запись хранилища, используемые виртуальной машиной перед преобразованием, не удаляются. За их использование будет и дальше взиматься плата. Чтобы избежать выставления счетов за эти артефакты, удалите исходные большие двоичные объекты VHD, когда убедитесь, что преобразование завершено.

* Просмотрите Минимальная версия агента ВМ Azure, необходимые для поддержки процесса преобразования. Сведения о том, как проверить и обновить версию агента см. в разделе [Минимальная поддержка версий для агенты ВМ в Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)