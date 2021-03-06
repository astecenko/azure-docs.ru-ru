---
title: "Управление присоединенными к домену кластерами HDInsight в Azure | Документы Майкрософт"
description: "Узнайте, как установить и настроить присоединенные к домену кластеры HDInsight"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: saurinsh
ms.openlocfilehash: 4921e329c2ec8ce3d5bbf8a0851146e13d5f6cd3
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Настройка среды с присоединенной к домену песочницей HDInsight

Узнайте, как настроить кластер Azure HDInsight с изолированной службой Active Directory и [Apache Ranger](http://hortonworks.com/apache/ranger/), чтобы воспользоваться преимуществами политик строгой проверки подлинности и управления доступа на основе ролей (RBAC) с широкими возможностями. Дополнительные сведения см. в статье [Introduce Domain-joined HDInsight clusters](apache-domain-joined-introduction.md) (Введение в присоединенные к домену кластеры HDInsight).

Без присоединенного к домену кластера HDInsight каждый кластер может иметь только учетную запись пользователя Hadoop HTTP и SSH.  Многофакторную проверку подлинности пользователей можно выполнить с помощью:

-   изолированной службы Active Directory, выполняемой в Azure IaaS;
-   Azure Active Directory;
-   Active Directory, выполняемой в локальной среде клиента.

В этой статье описано использование изолированной службы Active Directory, выполняемой в Azure IaaS. Это простейшая архитектура, которой может следовать клиент, чтобы обеспечить многопользовательскую поддержку в HDInsight. В данной статье рассматриваются два подхода к этой конфигурации:

- Вариант 1. Использование одного шаблона управления ресурсами Azure для создания изолированной службы Active Directory и кластера HDInsight.
- Вариант 2. Весь процесс разбивается на следующие действия:
    - создание приложения Active Directory с помощью шаблона;
    - установка протоколов LDAP;
    - создание пользователей и групп AD;
    - Создание кластера HDInsight

> [!IMPORTANT]
> Диспетчер Oozie не включен в присоединенном к домену кластере HDInsight.

## <a name="prerequisite"></a>Предварительные требования
* Подписка Azure.

## <a name="option-1-one-step-approach"></a>Вариант 1. Одноэтапный подход
В этом разделе вы откроете шаблон управления ресурсами Azure на портале Azure. Этот шаблон используется для создания изолированной службы Active Directory и кластера HDInsight. Сейчас можно создать присоединенный к домену кластер Hadoop, Spark и кластер интерактивных запросов.

1. Щелкните следующее изображение, чтобы открыть шаблон на портале Azure. Шаблон находится в [шаблонах быстрого запуска Azure](https://azure.microsoft.com/resources/templates/).
   
    Чтобы создать кластер Spark:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Чтобы создать кластер интерактивных запросов:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Чтобы создать кластер Hadoop:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Введите значения, установите флажок **Я принимаю указанные выше условия**, выберите **Закрепить на панели мониторинга**, а затем щелкните **Purchase** (Приобрести). Наведите указатель мыши на знак объяснения рядом с полями, чтобы просмотреть описания. Большая часть значений заполнены. Можно использовать собственные значения или значения по умолчанию.

    - **Группа ресурсов**. Введите имя группы ресурсов Azure.
    - **Расположение**. Выберите близкое к вам расположение.
    - **Имя новой учетной записи хранения**. Введите имя учетной записи хранения Azure. Новая учетная запись хранения используется PDC, BDC и кластером HDInsight в качестве учетной записи хранения по умолчанию.
    - **Имя администратора**. Введите имя администратора домена.
    - **Пароль администратора**. Введите пароль администратора домена.
    - **Имя домена**. Имя по умолчанию — *contoso.com*.  Если вы измените доменное имя, необходимо также обновить поля **Сертификат защищенного LDAP** и **Organizational Unit DN** (Доменное имя подразделения).
    - **Имя кластера**. Введите имя кластера HDInsight.
    - **Тип кластера**. Не меняйте это значение. Если вы хотите изменить тип кластера, используйте специальный шаблон на последнем шаге.

    Некоторые значения жестко закодированы в шаблоне. Например, число экземпляров рабочих ролей узла равно двум.  Чтобы изменить жестко закодированные значения, щелкните **Изменить шаблон**.

    ![Присоединенный к домену кластер HDInsight: изменение шаблона](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

После успешного создания шаблона в группе ресурсов будет 23 ресурса.

## <a name="option-2-multi-step-approach"></a>Вариант 2. Многоэтапный подход

Этот раздел состоит из четырех основных шагов:

1. создание приложения Active Directory с помощью шаблона;
2. установка протоколов LDAP;
3. создание пользователей и групп AD;
4. Создание кластера HDInsight

### <a name="create-an-active-directory"></a>Создание Active Directory

Шаблон Azure Resource Manager упрощает создание ресурсов Azure. В этом разделе используется [шаблон быстрого запуска Azure](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) для создания нового леса и домена с двумя виртуальными машинами. Две виртуальные машины выступают в качестве основного и резервного контроллеров домена.

**Создание домена с двумя контроллерами домена**

1. Щелкните следующее изображение, чтобы открыть шаблон на портале Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Шаблон выглядит следующим образом:

    ![Присоединенный к домену кластер HDInsight: создание виртуальных машин, домена, леса](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Введите следующие значения.

    - **Подписка**— выберите подписку Azure.
    - **Имя группы ресурсов**. Введите имя группы ресурсов.  Группа ресурсов используется для управления связанными с проектом ресурсами Azure.
    - **Расположение**. Выберите близкое к вам расположение Azure.
    - **Имя администратора**. Это имя администратора домена. Этот пользователь не связан с учетной записью пользователя HTTP кластера HDInsight. Это учетная запись, используемая в этом руководстве.
    - **Пароль администратора**. Введите пароль для администратора домена.
    - **Имя домена**. Имя домена должно быть двухкомпонентным. Например, contoso.com, contoso.local или hdinsight.test.
    - **Префикс DNS**. Введите префикс DNS.
    - **PDC RDP Port** (Порт RDP PDC). (В этом руководстве используйте значение по умолчанию.)
    - **BDC RDP Port** (Порт RDP BDC). (В этом руководстве используйте значение по умолчанию.)
    - **artifacts location** (Расположение артефактов). (В этом руководстве используйте значение по умолчанию.)
    - **artifacts location SAS token** (Маркер SAS расположения артефактов). (В этом руководстве оставьте это поле пустым.)

Процесс создания ресурсов занимает около 20 минут.

### <a name="setup-ldaps"></a>Настройка протоколов LDAP

Протокол LDAP используется для чтения и записи в AD.

**Подключение к PDC с помощью удаленного рабочего стола**

1. Войдите на [портале Azure](https://portal.azure.com).
2. Откройте группу ресурсов, а затем виртуальную машину основного контроллера домена (PDC). Имя PDC по умолчанию — adPDC. 
3. Щелкните **Подключить**, чтобы подключиться к PDC с помощью удаленного рабочего стола.

    ![Присоединенный к домену кластер HDInsight: подключение к PDC с помощью удаленного рабочего стола](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Добавление служб сертификатов Active Directory**

4. Откройте **диспетчер сервера**, если он еще не открыт.
5. Щелкните **Управление**, а затем — **Добавить роли и компоненты**.

    ![Присоединенный к домену кластер HDInsight: добавление ролей и компонентов](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. На странице "Перед началом работы" нажмите кнопку **Далее**.
6. Выберите **Установка ролей или компонентов**, а затем нажмите кнопку **Далее**.
7. Выберите PDC и нажмите кнопку **Далее**.  Имя PDC по умолчанию — adPDC.
8. Выберите **службы сертификатов Active Directory**.
9. В появившемся диалоговом окне щелкните **Добавить компоненты**.
10. Следуйте указаниям мастера и в остальной части процедуры используйте параметры по умолчанию.
11. Нажмите кнопку **Закрыть**, чтобы закрыть мастер.

**Настройка сертификата AD**

1. В диспетчере сервера щелкните желтый значок уведомления, а затем **Настроить службы сертификации Active Directory**.

    ![Присоединенный к домену кластер HDInsight: настройка сертификата AD](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Щелкните **Службы ролей** слева, выберите **Центр сертификации**, а затем нажмите кнопку **Далее**.
3. Следуйте указаниям мастера и в остальной части процедуры используйте параметры по умолчанию (щелкните **Настроить** на последнем шаге).
4. Нажмите кнопку **Закрыть**, чтобы закрыть мастер.

### <a name="optional-create-ad-users-and-groups"></a>(Необязательно.) Создание пользователей и групп AD

**Создание пользователей и групп в AD**
1. Подключитесь к PDC с помощью удаленного рабочего стола.
1. Откройте оснастку **Пользователи и компьютеры Active Directory**.
2. Выберите имя домена на панели слева.
3. Щелкните значок **Создание нового пользователя в текущем контейнере** в верхнем меню.

    ![Присоединенный к домену кластер HDInsight: создание пользователей](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Следуйте инструкциям, чтобы создать несколько пользователей. Например, hiveuser1 и hiveuser2.
5. Щелкните значок **Создание новой группы в текущем контейнере** в верхнем меню.
6. Следуйте инструкциям, чтобы создать группу с именем **HDInsightUsers**.  Эта группа используется при создании кластера HDInsight далее в этом руководстве.

> [!IMPORTANT]
> Перед созданием присоединенного к домену кластера HDInsight необходимо перезагрузить виртуальную машину PDC.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Создание кластера HDInsight в виртуальной сети

В этом разделе используется портал Azure для добавления кластера HDInsight в виртуальную сеть, созданную с помощью шаблона Resource Manager ранее в этом руководстве. В этой статье описываются только конкретные сведения о конфигурации кластера, присоединенного к домену.  Основные сведения см. в статье [Создание кластеров под управлением Linux в HDInsight с помощью портала Azure](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**Создание присоединенного к домену кластера HDInsight**

1. Выполните вход на [портал Azure](https://portal.azure.com).
2. Откройте группу ресурсов, созданную ранее в этом руководстве с помощью шаблона Resource Manager.
3. Добавьте в группу ресурсов кластер HDInsight.
4. Выберите параметр **Custom** (Настраиваемые):

    ![Присоединенный к домену кластер HDInsight: настраиваемый вариант создания](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    При использовании варианта настраиваемой конфигурации доступно шесть разделов: "Основы", "Хранилище", "Приложение", "Размер кластера", "Дополнительные параметры" и "Сводка".
5. В разделе **Основы**:

    - "Тип кластера". Выберите **Enterprise Security Package** (Пакет безопасности предприятия). Сейчас пакет безопасности предприятия можно включить только для следующих типов кластера: Hadoop, кластер интерактивных запросов и Spark.

        ![Присоединенный к домену кластер HDInsight: пакет безопасности предприятия](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - "Имя пользователя для входа в кластер". Это пользователь HTTP для Hadoop. Эта учетная запись отличается от учетной записи администратора домена.
    - "Группа ресурсов". Выберите группу ресурсов, созданную ранее с помощью шаблона Resource Manager.
    - "Расположение". Расположение должно совпадать с указанным при создании виртуальной сети и контроллеров домена с помощью шаблона Resource Manager.

6. В разделе **Дополнительные параметры** выполните следующие действия.

    - Параметры домена:

        ![Присоединенный к домену кластер HDInsight: дополнительные параметры домена](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - "Имя домена". Введите доменное имя, которое использовалось при [создании Active Directory](#create-an-active-directory).
        - Domain user name (Имя пользователя домена). Введите имя администратора домена, которое использовалось при [создании Active Directory](#create-an-active-directory).
        - "Подразделение". Для примера см. снимок экрана.
        - "URL-адрес LDAPS". Для примера см. снимок экрана.
        - "Доступ к группе пользователей". Введите имя группы пользователей, созданное при [создании пользователей и групп AD](#optionally-createad-users-and-groups)
    - "Виртуальная сеть". Выберите виртуальную сеть, которая была создана в разделе [Создание Active Directory](#create-an-active-directory). Имя по умолчанию, используемое в шаблоне, — **adVNET**.
    - "Подсеть". Имя по умолчанию, используемое в шаблоне, — **adSubnet**.



После завершения работы с этим руководством кластер можно удалить. В случае с HDInsight ваши данные хранятся в службе хранилища Azure, что позволяет безопасно удалить неиспользуемый кластер. Плата за кластеры HDInsight взимается, даже когда они не используются. Поскольку стоимость кластера во много раз превышает стоимость хранилища, экономически целесообразно удалять неиспользуемые кластеры. Инструкции по удалению кластера см. в статье [Управление кластерами Hadoop в HDInsight с помощью портала Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Дополнительная информация
* Сведения о настройке политик Hive и выполнении запросов Hive для присоединенного к домену кластера HDInsight см. [здесь](apache-domain-joined-run-hive.md).
* Сведения о подключении к присоединенным к домену кластерам HDInsight с использованием SSH см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

