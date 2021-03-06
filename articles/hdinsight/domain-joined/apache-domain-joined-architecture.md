---
title: "Архитектура присоединенного к домену кластера Azure HDInsight | Документация Майкрософт"
description: "Сведения о планировании архитектуры присоединенного к домену кластера HDInsight."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2017
ms.author: saurinsh
ms.openlocfilehash: eca019fa5e7866ed6281e8cfee105ba1d99249bc
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Планирование архитектуры присоединенных к домену кластеров Hadoop в Azure HDInsight

Стандартная кластера HDInsight — это кластер одного пользователя. Он подходит для большинства организаций с небольшими отделами по работе с приложениями, создающими объемные рабочие нагрузки данных. Как Hadoop популярность, многие предприятия запущен перемещаются в сторону модели, в котором кластерами управляют ИТ-отделов и приложения с несколькими командами, кластеры общей папки. Таким образом, возможности работы с многопользовательскими кластерами стали одними из самых запрашиваемых в Azure HDInsight.

Вместо создания собственной многопользовательской проверки подлинности и авторизации HDInsight полагается на самого популярного поставщика удостоверений — Active Directory (AD). Функциональные возможности мощных безопасности в AD можно использовать для управления сетевой проверки подлинности в HDInsight. Интегрировав HDInsight с AD, вы сможете взаимодействовать с кластерами, используя свои учетные данные AD. Виртуальные машины в HDInsight, присоединенных к домену в AD, и это HDInsight сопоставление пользователь AD локального пользователя Hadoop, так что все службы, работающие на HDInsight (Ambari, Hive thrift Spark server круг, сервера и другие) работают незаметно для пользователя, прошедшего проверку подлинности. Затем, администраторы могут создавать политики надежный авторизации с помощью Apache круг обеспечивают управление доступом на основе ролей для ресурсов в HDInsight.


## <a name="integrate-hdinsight-with-active-directory"></a>Интеграция HDInsight с Active Directory

При установке HDInsight с помощью Active Directory, HDInsight узлы кластера находятся в домене присоединен к домену AD. Настройки параметров безопасности Kerberos для компонентов в кластере Hadoop. Для каждого из компонентов Hadoop субъекта-службы создается на Active Directory. Соответствующая машина участника также создается для каждого компьютера, который присоединен к домену. Эти субъекты-службы и субъекты машины может загромождать Active Directory. В результате он требуется для предоставления организационное подразделение (OU) в Active Directory, где располагаются эти субъекты. 

Таким образом, необходимо настроить среду с:

- Контроллер домена Active Directory с LDAPS настроен.
- Наличие подключения к виртуальной сети HDInsight к контроллеру домена Active Directory.
- Организационное подразделение, созданные на основе Active Directory.
- Учетная запись службы, имеющую разрешения на:

    - Создание субъектов-служб в Подразделении.
    - Присоединение компьютеров к домену и создание машины участников в Подразделении.

На следующем рисунке показан Подразделение, созданные в домене contoso.com. На снимке экрана показаны некоторые субъектов-служб и участников машины.

![Подразделения кластеров HDInsight объединить домена](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="three-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Три способа приведению контроллеры домена Active Directory

Существует три способа, можно добавить контроллеры домена Active Directory, чтобы создать кластеры HDInsight, присоединенных к домену. 

- **Azure доменных служб Active Directory**: Эта служба предоставляет управляемый домен Active Directory, полностью совместима с Windows Server Active Directory. Корпорация Майкрософт управляет доменом AD, применяет исправления к домену и осуществляет мониторинг домена. Вы можете развернуть свой кластер, не беспокоясь о поддержке контроллеров домена. Пользователей, групп и пароли синхронизируются из Azure Active Directory, позволив пользователям выполнять вход кластер, используя свои корпоративные учетные данные. Дополнительные сведения см. в разделе [HDInsight, присоединенных к домену, настроить кластеры с помощью доменных служб Active Directory Azure](./apache-domain-joined-configure-using-azure-adds.md).

- **Active Directory на виртуальных машинах Azure IaaS**: В этом параметре, развертывания и управления доменом Windows Server Active Directory на виртуальных машинах Azure IaaS. Дополнительные сведения см. в разделе [среде присоединены к домену "песочницы" Настройка домена](./apache-domain-joined-configure.md).

- **Локальной Active Directory**: В этом случае HDInsight интегрировать с локального контроллера домена Active Directory.


## <a name="next-steps"></a>Дальнейшие действия
* Сведения о настройке присоединенного к домену кластера HDInsight см. в [этой статье](apache-domain-joined-configure.md).
* Сведения об управлении присоединенными к домену кластерами HDInsight см. в [этой статье](apache-domain-joined-manage.md).
* Сведения о настройке политик Hive и выполнении запросов Hive см. в статье [Настройка политик Hive в присоединенном к домену кластере HDInsight (предварительная версия)](apache-domain-joined-run-hive.md).
* Сведения о выполнении запросов Hive с помощью SSH в присоединенных к домену кластерах HDInsight см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
