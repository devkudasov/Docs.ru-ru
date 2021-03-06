---
uid: signalr/overview/security/hub-authorization
title: "Проверка подлинности и авторизация для концентраторов SignalR | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов. В этом разделе версий программного обеспечения используется Visual Studio 2013 .NET 4.5 SignalR хранить..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>Проверка подлинности и авторизация для концентраторов SignalR
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> В этом разделе описывается ограничьте какие пользователи или роли доступ к методам концентраторов. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
> 
> Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Обзор

В этом разделе содержатся следующие подразделы.

- [Авторизовать атрибута](#authorizeattribute)
- [Требовать проверку подлинности для всех концентраторов](#requireauth)
- [Настраиваемые авторизации](#custom)
- [Передать сведения о проверке подлинности для клиентов](#passauth)
- [Параметры проверки подлинности для клиентов .NET](#authoptions)

    - [Файл cookie с помощью форм](#cookie)
    - [Проверка подлинности Windows](#windows)
    - [Заголовка соединения](#header)
    - [Сертификат](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Авторизовать атрибута

SignalR обеспечивает [авторизовать](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) атрибут, чтобы указать, какие пользователи или роли имеют доступ к концентратору или метод. Этот атрибут находится в `Microsoft.AspNet.SignalR` пространства имен. Можно применить `Authorize` атрибут концентратор или отдельные методы в концентраторе. При применении `Authorize` все методы в концентраторе применяется атрибут к классу hub требование проверки. В этом разделе приведены примеры различных типов требования к проверке подлинности, которые можно применить. Без `Authorize` атрибут привязан клиент может получить доступ к любому открытому методу на концентраторе.

Если вы определили роль с именем «Admin» в веб-приложении, можно указать только пользователям в этой роли можно получить доступ к концентратору следующим кодом.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Также можно указать, что концентратор содержит один метод, который доступен всем пользователям, а второй метод, который доступен только пользователям, прошедшим проверку, как показано ниже.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Следующие примеры сценариев разных авторизации:

- `[Authorize]`— только прошедшие проверку подлинности пользователей
- `[Authorize(Roles = "Admin,Manager")]`— только прошедшие проверку подлинности пользователей в указанные роли
- `[Authorize(Users = "user1,user2")]`— только прошедшие проверку подлинности пользователи, имеющие указанные имена пользователей
- `[Authorize(RequireOutgoing=false)]`— только прошедшие проверку подлинности пользователи могут вызывать концентратора, но вызовы с сервера обратно клиентам не ограничены по авторизации, например, когда только некоторые пользователи могут отправлять сообщения, но все остальные может получать сообщения. Свойство RequireOutgoing свойства может применяться только для всего hub, а не для отдельных пользователей методов в центре. Когда свойство RequireOutgoing не задано значение false, только пользователи, которые соответствуют требованиям к авторизации называются с сервера.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Требовать проверку подлинности для всех концентраторов

Можно потребовать проверку подлинности для всех концентраторам и методам концентраторов в приложении, вызвав [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) метод при запуске приложения. Этот метод можно использовать после нескольких концентраторов и принудительно требование проверки подлинности для всех из них. С помощью этого метода нельзя задать требования для ролей, пользователей или исходящих авторизации. Можно только указать, что доступ к методам концентратор является только пользователи, прошедшие проверку подлинности. Тем не менее можно применять атрибут авторизовать концентраторы или методам, чтобы указать дополнительные требования. Любое требование, указываемых в атрибут добавляется к основным требованием проверки подлинности.

Пример файла запуска, ограничивающее все методы концентратора пользователям, прошедшим проверку.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

При вызове метода `RequireAuthentication()` вызовет метод после обработки запрос SignalR, SignalR `InvalidOperationException` исключение. SignalR создает исключение, так как не удается добавить модуль HubPipeline после конвейера была вызвана. В предыдущем примере показан вызов `RequireAuthentication` метод в `Configuration` метод, который выполняется один раз перед обработкой первого запроса.

<a id="custom"></a>

## <a name="customized-authorization"></a>Настраиваемые авторизации

Если нужно настроить как авторизация, можно создать класс, производный от `AuthorizeAttribute` и Переопределите [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) метод. Для каждого запроса SignalR вызывает этот метод, чтобы определить, имеет ли пользователь разрешение для выполнения запроса. В переопределенном методе предоставляют необходимую логику для вашего сценария авторизации. Приведенный ниже показано, как осуществить авторизации с помощью удостоверений на основе утверждений.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Передать сведения о проверке подлинности для клиентов

Необходимо использовать сведения о проверке подлинности в коде, который выполняется на клиенте. При вызове методов на стороне клиента, передайте необходимые сведения. Например метод приложения разговора может передать как параметр имя пользователя как пользователь, отправляющий сообщение, как показано ниже.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Или можно создать объект для представления сведения для проверки подлинности и передать этот объект в качестве параметра, как показано ниже.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Никогда не следует передавать идентификатор соединения клиента один для других клиентов, т.к. пользователь-злоумышленник может использовать для имитации запроса от клиента.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Параметры проверки подлинности для клиентов .NET

При наличии клиент .NET, таких как консольное приложение, который взаимодействует с к концентратору, который будет ограничена для пользователей с проверкой подлинности, можно передать учетные данные проверки подлинности в файле cookie, заголовка соединения или сертификата. В примерах этого раздела показано, как эти различные методы для проверки подлинности пользователя. Они не являются полностью функциональной приложений SignalR. Дополнительные сведения о клиентах .NET с помощью SignalR см. в разделе [Справочник по API концентраторов - клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Куки-файл

Когда клиент .NET взаимодействует с к концентратору, который использует проверку подлинности форм ASP.NET, необходимо вручную задать файл cookie проверки подлинности соединения. Добавьте файл cookie, `CookieContainer` свойство [HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) объекта. В следующем примере показано консольное приложение, которое извлекает файл cookie проверки подлинности с веб-страницы и добавляет соединения этому файлу cookie.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

Консольное приложение отправляет учетные данные для **www.contoso.com/RemoteLogin** которого может ссылаться на пустую страницу, содержащую следующий файл кода.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Аутентификация Windows

При использовании проверки подлинности Windows, можно передать учетные данные текущего пользователя с помощью [DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx) свойство. Задать учетные данные для подключения к значению DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Заголовка соединения

Если приложение не использует файлы cookie, можно передать сведения о пользователе заголовка соединения. Например можно передать токен в заголовке соединения.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Затем в концентраторе, нужно проверить токен пользователя.

<a id="certificate"></a>

### <a name="certificate"></a>Сертификат

Можно передать сертификат клиента, чтобы убедиться, что пользователь. Добавить сертификат при создании подключения. Следующий пример демонстрирует только добавление сертификат клиента для подключения; он не отображается полное консольное приложение. Она использует [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx) класс, который предоставляет несколько способов создания сертификата.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
