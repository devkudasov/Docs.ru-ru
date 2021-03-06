---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "Использование асинхронных методов в ASP.NET 4.5 | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания асинхронного приложения веб-форм ASP.NET с помощью Visual Studio Express 2012 для Web, который является бесплатной..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 62a32db0984cfc2a1f5fd8f9196aad9259d74f93
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Использование асинхронных методов в ASP.NET 4.5
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания асинхронных приложений веб-форм ASP.NET с использованием [Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/11/en-us), это бесплатная версия Microsoft Visual Studio. Можно также использовать [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us). В этом учебнике включены следующие разделы.
> 
> - [Способ обработки запросов пулом потоков](#HowRequestsProcessedByTP)
> - [Выбор синхронные или асинхронные методы](#ChoosingSyncVasync)
> - [Образец приложения](#SampleApp)
> - [На странице синхронной барахло](#GizmosSynch)
> - [Создание асинхронных барахло страницы](#CreatingAsynchGizmos)
> - [Параллельное выполнение нескольких операций](#Parallel)
> - [Использование токена отмены](#CancelToken)
> - [Конфигурация сервера для вызовов веб-службы высокого уровня параллелизма и высокой задержки](#ServerConfig)
> 
> В этом учебнике в предоставляется полный пример  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) на [GitHub](https://github.com/) сайта.


ASP.NET 4.5 веб-страницы в сочетании [.NET 4.5](https://msdn.microsoft.com/en-us/library/w0x726c2(VS.110).aspx) позволяет регистрировать асинхронные методы, которые возвращают объект типа [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). .NET Framework 4 появился асинхронного программирования концепция, называемая [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) и ASP.NET 4.5 поддерживает [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx). Задачи представляются **задачи** типа и связанных типов в [System.Threading.Tasks](https://msdn.microsoft.com/en-us/library/system.threading.tasks.aspx) пространства имен. Сборки .NET Framework 4.5 эта поддержка асинхронной с [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) и [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевые слова, которые упрощают работу с [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) гораздо проще, чем предыдущие объектов асинхронные методы. [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ключевое слово является синтаксических сокращением, указывающие, фрагмент кода асинхронно ожидать другой фрагмент кода. [Async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевое слово представляет Указание, которое можно использовать для пометки методов, как асинхронные методы на основе задач. Сочетание **await**, **async**и **задачи** объекта он значительно упрощает написание асинхронного кода в .NET 4.5. Новая модель для асинхронных методов, называется *асинхронная модель на основе задач* (**КОСНИТЕСЬ**). В этом учебнике предполагается, имеющих опыт работы с помощью асинхронного программирования [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) и [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевые слова и [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) пространства имен.

Дополнительные сведения об использовании [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) и [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевые слова и [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) пространства имен, см. следующие ссылки.

- [Технический документ: Асинхронности в .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Часто задаваемые вопросы об Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Асинхронное программирование в Visual Studio](https://msdn.microsoft.com/en-us/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Способ обработки запросов пулом потоков

На веб-сервере .NET Framework поддерживается пул потоков, которые используются для обслуживания запросов ASP.NET. При получении запроса, поток из пула отправляется для обработки этого запроса. Если запрос обрабатывается синхронно, поток, обрабатывающий запрос является занят при запрос обрабатывается и что поток не может обслуживать другой запрос.   
  
Это может оказаться проблемой, так как пул потоков можно сделать достаточно большим, чтобы вместить много занятых потоков. Однако количество потоков в пуле потоков ограничено (максимум для .NET 4.5 по умолчанию — 5000). В больших приложениях, где высокого уровня параллелизма долго выполняющихся запросов все доступные потоки может быть загружен. Это состояние называется нехватка потоков. При наступлении такой ситуации веб-сервер помещает запросы в очередь. При переполнении очереди запросов веб-сервер отклоняет запросы с состоянием HTTP 503 (сервер перегружен). Пул потоков CLR имеет ограничения по включению новый поток. Если параллелизма непостоянным (то есть веб-сайте может неожиданно получить большое количество запросов) и все доступные запрос потоки заняты из-за внутренних вызовов с большой задержкой, скорость внедрения ограниченный потока можно задать для приложения очень плохо отвечать. Кроме того каждый новый поток, добавляемые в пул потоков имеет нагрузку (например, 1 МБ памяти). Веб-приложения с помощью синхронные методы для вызовов службы высокой задержкой, где пула потоков будет увеличиваться вплоть до .NET 4.5 по умолчанию максимальное число 5, 000 потоков потребует около 5 ГБ больше памяти, чем приложение может также запросы на обслуживание с помощью асинхронные методы и 50 потоков. При выполнении асинхронной работы, вы используете не всегда потока. Например, при внесении запрос асинхронного веб-службы ASP.NET не будут использоваться все потоки между **async** вызова метода и **await**. Использование пула потоков для обслуживания запросов с большой задержкой может привести к большой объем памяти и неэффективного использования серверного оборудования.

## <a name="processing-asynchronous-requests"></a>Обработка асинхронных запросов

Веб-приложения, которые разделе большое количество параллельных запросов при запуске или с непостоянным нагрузкой (где параллелизма увеличивается неожиданно) делая асинхронные вызовы веб-службы приведет к увеличению скорости реагирования приложения. Асинхронный запрос занимает столько же времени, что запрос является синхронным. Например если запрос выполняет веб-службы, в вызове, который требует две секунды для выполнения, запрос будет обрабатываться две секунды, является ли эта задача выполняется синхронно или асинхронно. Однако при асинхронном вызове, поток не заблокирован отвечать на другие запросы во время ожидания завершения первого запроса. Таким образом асинхронные запросы предупреждают роста пула очереди и поток запроса, если существует много одновременных запросов, которые вызывают длительные операции.

## <a id="ChoosingSyncVasync"></a>Выбор синхронные или асинхронные методы

В этом разделе перечислены рекомендации, когда использовать синхронные или асинхронные методы. Это только рекомендации. Проверьте каждое приложение, чтобы определить, является ли асинхронные методы проблем с производительностью.

В общем случае используйте синхронные методы для следующих условий:

- Операции являются простыми и кратковременными.
- Простота имеет более важной, чем эффективность.
- Операции основном являются операциями ЦП вместо операции, включающие широко используется диск или сетевые ресурсы. Использование асинхронных методов для операций ЦП не дает преимущества и приводит к дополнительному издержек.

 В общем случае используйте асинхронные методы для следующих условий:

- Выполняется вызов служб, которые могут быть использованы через асинхронных методов, и вы используете .NET 4.5 или более поздней.
- Операций, связанных с сетью или ввода-вывода вместо ЦП.
- Параллелизм имеет большее значение, чем простота кода.
- Вы хотите предоставить механизм, который позволяет пользователям отменить длительные запрос.
- Когда преимущество переключение потоков out весовые коэффициенты, затраты на переключение контекста. В общем случае следует метод асинхронной Если синхронный метод блокировку потока запросов ASP.NET во время выполнения нет работы. Путем вызова асинхронного потока запросов ASP.NET не блокируется не выполняют никакой работы при ожидании завершения запроса веб-службой.
- Тестирование показывает, что блокировка операций приводит к ухудшению производительности сайта и что IIS может обслуживать большее количество запросов с помощью асинхронных методов для этих заблокированных вызовов.

 Загружаемый пример показано, как эффективно использовать асинхронные методы. Предоставленный образец был разработан для обеспечения простого демонстрацию асинхронного программирования в ASP.NET 4.5. Образец не является ссылочной архитектуры для асинхронного программирования в ASP.NET. Программа-пример вызывает [веб-API ASP.NET](../../../web-api/index.md) методы, которые в свою очередь вызывают [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) для имитации вызовы долго выполняющихся веб-службы. Большинство приложений не продемонстрируют такие очевидные преимущества для использования асинхронных методов.   
  
Некоторые приложения требуют все методы выполняться асинхронно. Часто преобразование нескольких синхронных методов для асинхронных методов обеспечивает наилучший прирост эффективности объема работ.

## <a id="SampleApp"></a>Образец приложения

Можно загрузить пример приложения из [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) на [GitHub](https://github.com/) сайта. Репозиторий состоит из трех проектов.

- *WebAppAsync*: проект веб-форм ASP.NET, в котором веб-API **WebAPIpwg** службы. Большая часть кода для этого учебника — из этого проекта.
- *WebAPIpgw*: проект веб-API ASP.NET MVC 4, реализующий `Products, Gizmos and Widgets` контроллеров. Он предоставляет данные для *WebAppAsync* проекта и *Mvc4Async* проекта.
- *Mvc4Async*: проект ASP.NET MVC 4, который содержит код, используемый в другой учебник. Он выполняет вызовы веб-API **WebAPIpwg** службы.

## <a id="GizmosSynch"></a>На странице синхронной барахло

 В следующем коде показано `Page_Load` синхронный метод, который используется для отображения списка барахло. (Для данной статьи gizmo — это вымышленная механические устройство). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

В следующем коде показано `GetGizmos` метод gizmo службы.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos` Метод передает URI службы ASP.NET Web API HTTP, который возвращает список данных барахло. *WebAPIpgw* проект содержит реализацию веб-API `gizmos, widget` и `product` контроллеров.  
На рисунке показана страница барахло из образца проекта.

![Барахло](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Создание асинхронных барахло страницы

В образце используется новый [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) и [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ключевые слова (доступна в .NET 4.5 и Visual Studio 2012), чтобы позволить компилятору отвечает за поддержание сложные преобразования, необходимые для Асинхронное программирование. Компилятор позволяет писать код, используя создает поток синхронной управления C# и компилятор автоматически применяет преобразования, необходимые для использования обратных вызовов во избежание блокировки потоков.

Асинхронные страницы ASP.NET необходимо включить [страницы](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) директивы `Async` атрибута значение «true». В следующем коде показано [страницы](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) директивы `Async` атрибута значение «true» для *GizmosAsync.aspx* страницы.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

В следующем коде показано `Gizmos` синхронной `Page_Load` метод и `GizmosAsync` асинхронной страницы. Если браузер поддерживает [HTML 5 &lt;пометить&gt; элемент](http://www.w3.org/wiki/HTML/Elements/mark), вы увидите изменения в `GizmosAsync` в желтой выделения.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Асинхронная версия:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Следующие изменения были применены к возможным `GizmosAsync` страницы выполняться асинхронно.

- [Страницы](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) директивы должен иметь `Async` атрибута значение «true».
- `RegisterAsyncTask` Метод используется для регистрации асинхронная задача, содержащая код, который выполняется асинхронно.
- Новый `GetGizmosSvcAsync` метод помечен атрибутом [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевое слово, которое указывает компилятору создавать обратные вызовы для частей текста и автоматически создавать `Task` , возвращается.
- &quot;Асинхронный&quot; был добавлен к имени асинхронного метода. Добавление «Async» не является обязательным, но соглашение, используемое при создании асинхронных методов.
- Тип возвращаемого значения нового новый `GetGizmosSvcAsync` метод `Task`. Тип возвращаемого значения `Task` представляет выполняющуюся работу и предоставляет этот метод с дескриптором, через который ожидает завершения асинхронной операции.
- [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) была применена ключевое слово для вызова веб-службы.
- Был вызван API асинхронного веб-службы (`GetGizmosAsync`).

Внутри `GetGizmosSvcAsync` метод body другого асинхронного метода, `GetGizmosAsync` вызывается. `GetGizmosAsync`немедленно возвращает `Task<List<Gizmo>>` , со временем будет выполнена, когда данные станут доступны. Поскольку вы не хотите выполнять дополнительные действия, чтобы отобразить данные gizmo, код ожидает задачу (с помощью **await** ключевое слово). Можно использовать **await** ключевое слово только в методы, помеченные **async** ключевое слово.

**Await** ключевое слово не блокирует поток до завершения задачи. Подписывает остальную часть метода как обратный вызов в задаче и немедленно возвращает. При со временем завершения выполнения ожидающей задачи, будет этот обратный вызов и таким образом возобновить выполнение метод справа, где оно было остановлено. Дополнительные сведения об использовании [await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) и [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевые слова и [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) пространства имен, в разделе [async ссылки](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

В следующем коде показано `GetGizmos` и `GetGizmosAsync` методы.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Асинхронные изменения аналогичны внесенные **GizmosAsync** выше. 

- Сигнатура метода, аннотированных в [async](https://msdn.microsoft.com/en-us/library/hh156513(VS.110).aspx) ключевое слово, был изменен тип возвращаемого значения на `Task<List<Gizmo>>`, и *Async* был добавлен к имени метода.
- Асинхронная [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx) класс используется вместо синхронный [WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient.aspx) класса.
- [Await](https://msdn.microsoft.com/en-us/library/hh156528(VS.110).aspx) ключевое слово была применена к [HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/en-us/library/hh158944(VS.110).aspx) асинхронного метода.

На следующем рисунке представление асинхронной gizmo.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Представление браузеры данных барахло идентична представление, созданное с синхронный вызов. Единственное различие заключается в асинхронной версии может быть более производительным под высокой нагрузкой.

## <a name="registerasynctask-notes"></a>Метод RegisterAsyncTask заметок

Методы подключили с `RegisterAsyncTask` будет выполняться сразу после [PreRender](https://msdn.microsoft.com/en-us/library/ms178472.aspx). Также можно async void страницы события напрямую, как показано в следующем коде:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Недостатком async void событий является, разработчики, больше не имеет полный контроль над при выполнении события. Например если оба .aspx и. Определение основных `Page_Load` событий и один или оба из них являются асинхронными, порядок выполнения не может быть гарантирована. Indeterminiate порядке для обработчиков событий, отличных от (такие как `async void Button_Click` ) применяется. Для большинства разработчиков это приемлемо, но те, которым требуется полный контроль над порядок выполнения следует использовать только API, например `RegisterAsyncTask` , использовать методы, которые возвращают объект задачи.

## <a id="Parallel"></a>Параллельное выполнение нескольких операций

Асинхронные методы имеют значительное преимущество над синхронных методов, когда действие должно выполнить несколько независимых операций. В примере указано, синхронный страницы *PWG.aspx*(для продуктов, мини-приложений и барахло) отображает результаты три вызовы веб-службы для получения списка продуктов, мини-приложений и барахло. [Веб-API ASP.NET](../../../web-api/index.md) проекта, который предоставляет эти службы использует [Task.Delay](https://msdn.microsoft.com/en-us/library/hh139096(VS.110).aspx) имитировать задержки или медленные вызовы. Если задержка равно 500 миллисекунд асинхронного *PWGasync.aspx* страница занимает немного более чем 500 миллисекунд для выполнения во время синхронного `PWG` имеет версию, чем 1 500 миллисекунд. Синхронный *PWG.aspx* странице показан в следующем коде.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Асинхронная `PWGasync` ниже показан код программной части.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

На следующем рисунке показано представление, возвращенные асинхронной *PWGasync.aspx* страницы.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Использование токена отмены

Асинхронные методы возвращения `Task`являются можно отменить; они принимают [CancellationToken](https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(VS.110).aspx) параметр, если он указан с `AsyncTimeout` атрибут [страницы](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) директивы. В следующем коде показано *GizmosCancelAsync.aspx* страницы с временем ожидания в секунду.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

В следующем коде показано *GizmosCancelAsync.aspx.cs* файла.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

В образце приложения, предоставляемые, выбрав *GizmosCancelAsync* связывать вызовы *GizmosCancelAsync.aspx* страницы и демонстрирует выполнение отмены (по тайм-аута) асинхронного вызова. Поскольку случайных диапазона времени задержки, может потребоваться обновить страницу два раза получить сообщение об ошибке времени ожидания.

## <a id="ServerConfig"></a>Конфигурация сервера для вызовов веб-службы высокого уровня параллелизма и высокой задержки

Чтобы использовать преимущества асинхронного веб-приложения, может потребоваться внести некоторые изменения конфигурации сервера по умолчанию. Имейте в виду при настройке и нагрузочное тестирование асинхронного веб-приложения следующее.

- Windows 7, Windows Vista, Windows 8 и все клиентские операционные системы Windows иметь не более 10 параллельных запросов. Вам потребуется операционной системы Windows Server, чтобы ознакомиться с преимуществами асинхронных методов в условиях высокой нагрузки.
- Зарегистрируйте .NET 4.5 с IIS из командной строки с повышенными привилегиями выполните следующую команду:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 В разделе [средство регистрации ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx)
- Может потребоваться увеличить [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) предел очереди из значения по умолчанию 1000 до 5000. Если этот параметр имеет слишком мало, может появиться [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) отклонять запросы с состоянием HTTP 503. Чтобы изменить HTTP.sys предел очереди:

    - Откройте диспетчер служб IIS и перейдите в область группы приложений.
    - Щелкните целевой пул приложений правой кнопкой мыши и выберите **Дополнительные параметры**.  
        ![Дополнительно](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - В **Дополнительные параметры** диалоговом изменение *длина очереди* от 1000 до 5 000.  
        ![Длина очереди](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Обратите внимание, на рисунках выше, .NET framework указана как версии 4.0, несмотря на то, что пул приложений использует .NET 4.5. Чтобы понять это несоответствие, см.:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/en-us/library/bb822049(VS.110).aspx)
- Если приложение использует веб-службы или System.NET для взаимодействия с серверной части по протоколу HTTP можно может понадобиться увеличить [connectionManagement/maxconnection](https://msdn.microsoft.com/en-us/library/fb6y0fyc(VS.110).aspx) элемента. Для приложений ASP.NET это будет ограничена функцией автонастройки беспроводной сети в 12 раз больше число процессоров. Это означает, что в четырех proc можно иметь не более 12 \* 4 = 48 одновременных подключений к конечной точке IP. Так как это привязывается к [автонастройки](https://msdn.microsoft.com/en-us/library/7w2sway1(VS.110).aspx), самым простым способом, чтобы увеличить `maxconnection` в ASP.NET является установка приложения [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) программными средствами в из `Application_Start` метод в *global.asax* файла. См. в образце Загрузите пример.
- В .NET 4.5, значение по умолчанию 5000 для [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) должно быть нормально.

## <a name="contributors"></a>Авторы

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Брэд Вилсон](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
