---
title: "Изменения в конечной точке Azure AD версии 2.0 | Документация Майкрософт"
description: "Описание изменений, которые будут внесены в протоколы общедоступной предварительной версии модели приложения версии 2.0."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3d344529d1ac9891e5a961de630dea3c1e4caab4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a>Важные обновления протоколов проверки подлинности версии 2.0
Вниманию разработчиков! В течение следующих двух недель будут внесены некоторые изменения в протоколы проверки подлинности версии 2.0. Это могут быть критические изменения для любых приложений, написанных в течение периода действия предварительной версии.  

## <a name="who-does-this-affect"></a>Приложения, на которые повлияют обновления
Обновления повлияют на все приложения, которые предусматривают использование объединенной конечной точки проверки подлинности версии 2.0.

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Дополнительные сведения о конечной точке версии 2.0 можно найти [здесь](active-directory-appmodel-v2-overview.md).

Если приложение разработано с использованием конечной точки версии 2.0 путем написания кода непосредственно в протокол версии 2.0, а для проверки подлинности использовалось наше ПО промежуточного слоя (OpenID Connect или OAuth) или другие библиотеки сторонних производителей, понадобится протестировать проекты и, возможно, внести некоторые изменения.

## <a name="who-doesnt-this-affect"></a>Приложения, на которые не повлияют обновления
Обновления не повлияют на все приложения, написанные с помощью конечной точки проверки подлинности рабочей среды Azure AD.

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Этот протокол окончательный и не подлежит изменению.

Если для проверки подлинности в приложении используется **только** наша библиотека ADAL, вносить изменения также не потребуется.  При использовании библиотеки ADAL ваше приложение защищено от изменений.  

## <a name="what-are-the-changes"></a>Изменения
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Удаление значения x5t из заголовков маркера JWT
Для конечной точки версии 2.0 широко используются маркеры JWT, в которых содержится раздел параметров заголовка с соответствующими метаданными о маркере.  Если декодировать заголовок текущего маркера JWT, вы увидите нечто вроде этого:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Здесь свойства x5t и kid определяют открытый ключ, который следует использовать для проверки подписи маркера, полученной из конечной точки метаданных OpenID Connect.

Изменение заключается в удалении свойства x5t.  Вы можете по-прежнему использовать те же механизмы для проверки маркеров, но чтобы получить правильный открытый ключ, следует использовать только значение свойства kid, как указано в протоколе OpenID Connect. 

> [!IMPORTANT]
> **Задание. Убедитесь, что работа приложения не зависит от значения x5t.**
> 
> 

### <a name="removing-profileinfo"></a>Удаление profile_info
Ранее конечная точка версии 2.0 возвращала объект JSON в кодировке base64 в ответах маркера `profile_info`.  При запросе маркера доступа из конечной точки версии 2.0 по следующему адресу отправлялся запрос:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Ответ при этом выглядел как следующий объект JSON:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

В значении `profile_info` содержатся сведения о пользователе, выполнившем вход в приложение, а именно: отображаемое имя, имя, фамилия, адрес электронной почты, идентификатор и т. д.  `profile_info` преимущественно использовался для кэширования и отображения маркеров.

Теперь значение `profile_info` удалено. Не волнуйтесь, эти сведения по-прежнему предоставляются разработчикам, но в другом месте.  Теперь конечная точка версии 2.0 в каждом ответе маркера вместо `profile_info` возвращает `id_token`:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Чтобы получить сведения, которые ранее содержались в profile_info, можно декодировать id_token и выполнить его синтаксический анализ.  (id_token — это веб-маркер JSON (JWT) с содержимым, которое указано в OpenID Connect.)  Сделать это достаточно просто — нужно извлечь средний сегмент (текст) id_token и декодировать его из кодировки base64, чтобы получить доступ к находящемуся в нем объекту JSON.

В течение следующих двух недель вам необходимо кодировать приложение, чтобы получить сведения о пользователе из `id_token` или `profile_info` (в зависимости от наличия).  Таким образом после внесения изменений приложение может беспроблемно перейти с `profile_info` на `id_token`, не прекращая работу.

> [!IMPORTANT]
> **Задание. Убедитесь, что работа приложения не зависит от наличия значения `profile_info`.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Удаление id_token_expires_in
Подобно `profile_info`, из ответов также будет удален параметр `id_token_expires_in`.  До этого конечная точка версии 2.0 возвращала значение параметра `id_token_expires_in` вместе с каждым ответом id_token, например в ответе авторизации:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Или в ответе маркера:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Значение `id_token_expires_in` указывает количество секунд, в течение которых id_token остается допустимым.  Теперь значение `id_token_expires_in` будет полностью удалено.  Чтобы проверить допустимость id_token, вместо него можно использовать стандартные утверждения `nbf` и `exp` OpenID Connect.  Дополнительные сведения об этих утверждениях см. [справочнике по маркерам версии 2.0](active-directory-v2-tokens.md).

> [!IMPORTANT]
> **Задание. Убедитесь, что работа приложения не зависит от наличия значения `id_token_expires_in`.**
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a>Изменение утверждений, возвращенных с помощью области openid
Это изменение наиболее значимое, так как оно повлияет практически на все приложения, использующие конечную точку версии 2.0.  Многие приложения отправляют запросы в конечную точку версии 2.0, используя область `openid`, например:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Сейчас, когда пользователь предоставляет разрешение на доступ к области `openid`, приложение получает подробные сведения о пользователе в полученном id_token.  В этих утверждениях может содержаться его имя, предпочтительное имя пользователя, адрес электронной почты, идентификатор объекта и многое другое.

В этом обновлении изменяются данные, которые доступны приложению через область `openid`. Это обеспечивает соответствие спецификации OpenID Connect.  Теперь с помощью области `openid` пользователь сможет лишь войти в приложение и получить идентификатор для конкретного приложения в утверждении `sub` id_token.  Утверждения в id_token с доступом только к области `openid` не будут содержать личных сведений.  Примеры утверждений id_token:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Чтобы получить личные сведения о пользователе в приложении, приложение должно запросить дополнительные разрешения у пользователя.  Мы представляем две новые области из спецификации OpenID Connect, с помощью которых можно это сделать: `email` и `profile`.

* Область `email` достаточно проста. С ее помощью приложение получает доступ к основному адресу электронной почты пользователя через утверждение `email` в id_token.  Обратите внимание, что утверждение `email` не всегда представлено в id_tokens, а только если оно указано в профиле пользователя.
* С помощью области `profile` приложение может получить другие основные сведения о пользователе: имя, предпочтительное имя пользователя, идентификатор объекта и т. д.

Это позволяет писать код приложения с минимальным разглашением сведений. Вы можете запросить у пользователя только те сведения, которые необходимы приложению для выполнения задания.  Если вы хотите по-прежнему получать полный набор сведений о пользователе, необходимо включить в запросы авторизации все три области:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Приложение может немедленно начать отправку областей `email` и `profile`, а конечная точка версии 2.0 примет эти области и начнет запрашивать разрешения у пользователей при необходимости.  Однако изменения интерпретации области `openid` вступят в силу только через несколько недель.

> [!IMPORTANT]
> **Задание. Добавьте области `profile` и `email`, если приложению требуются сведения о пользователе.**  Обратите внимание, что при использовании ADAL эти разрешения запрашиваются по умолчанию. 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a>Удаление завершающей косой черты из значения издателя
До этого значение издателя, которое отображается в маркерах конечной точки версии 2.0, выглядело следующим образом:

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

guid здесь — идентификатор клиента Azure AD, который выдал маркер.  После внесения изменений значение издателя будет выглядеть так:

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

Это касается как обоих маркеров, так и документа обнаружения OpenID Connect.

> [!IMPORTANT]
> **Задание. Убедитесь, что во время проверки издателя приложение принимает значение издателя и с завершающей косой чертой, и без нее.**
> 
> 

## <a name="why-change"></a>Цель изменений
Эти изменения главным образом предназначены для соответствия стандартной спецификации OpenID Connect.  Мы надеемся, что таким образом можно свести к минимуму различия между интеграцией со службами идентификации Майкрософт и другими службами идентификации в этой отрасли.  Мы хотим предоставить разработчикам возможность использовать привычные библиотеки проверки подлинности с открытым исходным кодом без каких-либо изменений в связи с различиями служб Майкрософт.

## <a name="what-can-you-do"></a>Советы разработчикам
С сегодняшнего дня вы можете начать вносить все описанные выше изменения.  Незамедлительно сделайте следующее:

1. **Устраните все зависимости от параметра заголовка `x5t`.**
2. **Надлежащим образом выполните переход с `profile_info` на `id_token` в ответах маркеров.**
3. **Устраните все зависимости от параметра ответа `id_token_expires_in`.**
4. **Добавьте области `profile` и `email` в приложение, если ему необходимы основные сведения о пользователе.**
5. **Настройте принятие значений издателя в маркерах с завершающей косой чертой и без нее.**

Статья [Предварительная версия модели приложений 2.0: протоколы — OAuth 2.0 и OpenID Connect](active-directory-v2-protocols.md) уже обновлена с учетом этих изменений, так что вы можете использовать ее в качестве справочных материалов во время обновления кода.

Если в дальнейшем у вас возникнут вопросы по этим изменениям, вы можете связаться с нами в Twitter через канал @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Частота изменений протокола
Никакие критические изменения для протоколов проверки подлинности в дальнейшем не предусмотрены.  Мы намеренно объединили эти изменения в одной версии, чтобы вам в ближайшее время не пришлось выполнять этот процесс обновления снова.  Безусловно, мы продолжаем добавлять удобные функции в объединенную службу проверки подлинности версии 2.0, но эти изменения являются добавочными и не нарушают существующий код.

И наконец, мы бы хотели выразить благодарность за то, что вы опробовали на практике эту предварительную версию.  Аналитические сведения и опыт первых исследователей имели неоценимое значение, и мы надеемся, что вы продолжите делиться своим мнением и идеями.

Удачного программирования!

Подразделение Microsoft Identity

