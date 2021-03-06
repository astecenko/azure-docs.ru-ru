
**Последнее обновление документа**: 6 января 18:30 по тихоокеанскому времени.

Недавно обнаружен [новый класс уязвимостей ЦП](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), известный как атаки упреждающего исполнения по сторонним каналам. В связи с этим у пользователей возникли вопросы, требующие разъяснения.  

Инфраструктура с изолированными друг от друга нагрузками, которая работает в Azure, является защищенной.  Это означает, что другим пользователям, работающим в Azure, не удастся осуществлять атаки на ваше приложение, воспользовавшись этой уязвимостью.

## <a name="keeping-your-operating-systems-up-to-date"></a>Поддержка актуальности операционной системы

Чтобы изолировать ваши приложения в Azure от действий других пользователей, которые работают в Azure, обновлять операционную систему не нужно. Но лучше всего постоянно поддерживать актуальность версий ОС. 

Рекомендуем выполнить следующие инструкции по обновлению операционной системы для указанных ниже предложений: 

<table>
<tr>
<th>Предложение</th> <th>Рекомендуемое действие </th>
</tr>
<tr>
<td>Облачные службы Azure </td>  <td>Включите автоматическое обновление и убедитесь, что используете последнюю версию гостевой ОС.</td>
</tr>
<tr>
<td>Виртуальные машины Linux в Azure</td> <td>Устанавливайте обновления от поставщика операционной системы по мере доступности. </td>
</tr>
<tr>
<td>Виртуальные машины Windows в Azure </td> <td>Перед установкой обновления ОС убедитесь, что используется поддерживаемое антивирусное приложение. Обратитесь к поставщику антивирусного программного обеспечения, чтобы получить сведения о совместимости.<p> Установите [выпущенную в январе свертку безопасности](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Прочие службы Azure PaaS</td> <td>Для клиентов, использующих эти службы, дополнительные действия не требуются. Azure автоматически выполняет обновление версий операционной системы. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Дополнительные рекомендации при выполнении кода без доверия 

Дополнительные действия не требуются, за исключением случаев, когда используется код без доверия. Если вы разрешаете выполнение кода без доверия (например, разрешаете одному из клиентов передать двоичный код или фрагмент кода, который затем выполняется в вашем приложении в облаке), сделайте следующее.  


### <a name="windows"></a>Windows 
Если вы используете Windows и размещаете код без доверия, включите функцию Windows для скрытия виртуального адреса ядра (KVA), которая обеспечит дополнительную защиту от уязвимостей упреждающего исполнения по сторонним каналам. По умолчанию эта функция отключена. При активации она может снизить производительность. Выполните инструкции из статьи [KB4072698 для Windows Server](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution), чтобы включить защиту на сервере. Если вы работаете с облачными службами Azure, убедитесь, что используется WA-GUEST-OS-5.15_201801-01 или WA-GUEST-OS-4.50_201801-01 (доступны начиная с 10 января), и включите раздел реестра при помощи задачи запуска.


### <a name="linux"></a>Linux
Если вы используете Linux и размещаете код без доверия, обновите Linux до последней версии с изоляцией страниц для таблиц, которые используются ядром (KPTI). При этом страницы таблиц, используемые ядром, отделяются от тех, которые относятся к пространству пользователя. Такие средства устранения рисков требуют обновления ОС Linux. Их можно получить у поставщика по мере доступности. Поставщик ОС сообщит, включены ли решения по обеспечению безопасности по умолчанию.








## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в разделе [Securing Azure customers from CPU vulnerability](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) (Защита клиентов Azure от уязвимости ЦП).