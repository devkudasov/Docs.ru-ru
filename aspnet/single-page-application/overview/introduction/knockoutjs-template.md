---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "Одностраничного приложения: Шаблон использованием KnockoutJS | Документы Microsoft"
author: MikeWasson
description: "Шаблон маскирования"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>Одностраничного приложения: Использованием KnockoutJS шаблон
====================
по [Mike Wasson](https://github.com/MikeWasson)

> Шаблон MVC маскирования является частью ASP.NET и 2012.2 средства Web
> 
> [Загрузите ASP.NET и веб-инструменты 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Обновления ASP.NET и 2012.2 средства Web содержит шаблон одностраничного приложения (SPA) для ASP.NET MVC 4. Этот шаблон предназначен для начала работы быстрого создания интерактивные клиентские веб-приложения.

«Одностраничного приложения» (SPA) — это общий термин для веб-приложения, которое загружает одной HTML-страницы и затем обновляет страницу динамически, вместо загрузки новой страницы. После загрузки начальной страницы SPA обращается к серверу через запросы AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX нет ничего нового, но на сегодняшний день существует платформ JavaScript, которые упрощают создание и обслуживание больших сложных приложений SPA. Кроме того HTML 5 и CSS3 упрощая процесс для создания многофункциональных пользовательских интерфейсов.

Чтобы приступить к работе, SPA шаблон создает пример приложения «Список дел». В этом учебнике мы сделаете обзором шаблона. Сначала мы будет Проверьте список дел самого приложения, а затем проверьте элементы технологии работы.

## <a name="create-a-new-spa-template-project"></a>Создание нового проекта шаблона SPA

Требования:

- Visual Studio 2012 или Visual Studio Express 2012 для Web
- Обновление средств ASP.NET Web 2012.2. Можно установить обновление [здесь](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Запустите Visual Studio и выберите **новый проект** с начальной страницы. Или из **файл** последовательно выберите пункты **New** и затем **проекта**.

В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#**выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Имя проекта и нажмите кнопку **ОК**.

![](knockoutjs-template/_static/image2.png)

В **новый проект** мастера выберите **одностраничного приложения**.

![](knockoutjs-template/_static/image3.png)

Нажмите клавишу F5, чтобы выполнить сборку приложения и запустить его. При первом запуске приложения оно отображает экран входа.

![](knockoutjs-template/_static/image4.png)

Нажмите кнопку &quot;зарегистрироваться&quot; связь и создайте нового пользователя.

![](knockoutjs-template/_static/image5.png)

После входа приложение создает список Todo по умолчанию с двумя элементами. Нажмите кнопку «Добавить список дел» для добавления нового списка.

![](knockoutjs-template/_static/image6.png)

Список переименования, добавление элементов в списке и отмечать. Кроме того, можно удалить элементы или удалять весь список. Изменения автоматически сохраняются в базе данных на сервере (фактически LocalDB на этом этапе, поскольку вы используете приложение локально).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Архитектура шаблона SPA

На этой диаграмме показаны основные строительные блоки для приложения.

![](knockoutjs-template/_static/image8.png)

На стороне сервера ASP.NET MVC служит HTML, а также обрабатывает проверку подлинности на основе форм.

Веб-API ASP.NET обрабатывает все запросы, связанные с ToDoLists и ToDoItems, включая получение, создания, обновления и удаления. Клиент обменивается данными с веб-API в формате JSON.

Entity Framework (EF) — это уровень O/надежный обмен Сообщениями. Он является посредником между объектно ориентированного приложений ASP.NET и основной базы данных. База данных использует LocalDB, но вы можете изменить в файле Web.config. Обычно использовать LocalDB для локальной разработки и затем развернуть в базе данных SQL на сервере, с помощью EF сначала код миграции.

На стороне клиента библиотеки Knockout.js обрабатывает обновления страницы из запросы AJAX. Маскировать использует привязку данных для синхронизации на странице с использованием последних данных. В этом случае не нужно писать код, который описывается данным JSON и обновляет модели DOM. Вместо этого вы поместите декларативные атрибуты в формате HTML, сообщить маскирования способ представления данных.

Большим преимуществом этой архитектуры является его отделяет слой представления от логики приложения. Можно создать часть веб-API не зная ничего о вида веб-страницы. На стороне клиента создать «представление модели» для представления данных и модели представления использует маскирования для привязки к элементу HTML. Что позволяет легко изменять HTML без изменения модели представления. (Мы рассмотрим маскирования чуть позже.)

## <a name="models"></a>Модели

В проекте Visual Studio папку модели содержит модели, используемые на стороне сервера. (На стороне клиента также существуют модели, мы получаем теми.)

![](knockoutjs-template/_static/image9.png)

**TodoItem TodoList**

Это модели Entity Framework Code First для базы данных. Обратите внимание, что эти модели свойства, указывающие на друг с другом. `ToDoList`содержит коллекцию ToDoItems и каждый `ToDoItem` имеет ссылку родительского ToDoList. Эти свойства называются свойства навигации, и они представляют отношение один ко многим список дел и его элементы задачи.

`ToDoItem` Класс также использует **[ForeignKey]** атрибут, чтобы указать, что `ToDoListId` является внешним ключом в `ToDoList` таблицы. Это заставляет EF, чтобы добавить ограничение внешнего ключа к базе данных.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto TodoListDto**

Эти классы определяют данные, которые будут отправляться на клиенте. «DTO» означает «объект передачи данных». DTO определяет, каким образом сущности будет сериализовано в JSON. Как правило существует несколько причин для использования DTO:

- Чтобы контролировать, какие свойства сериализуются. DTO может содержать подмножество свойств из модели домена. Это можно сделать, по соображениям безопасности (для скрытия конфиденциальных данных) или просто для сокращения объема данных, отправляемых.
- Чтобы изменить форму данных, например, с целью выравнивания требуется более сложная структура данных.
- Чтобы сохранить все бизнес-логики из DTO (Разделение областей ответственности).
- Если для какой-либо причине не может быть сериализован модели домена. Например, циклические ссылки может вызвать проблемы при сериализации объекта существует способов решения этой проблемы в веб-API (см. [обработки циклических ссылок объекта](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); но просто с помощью DTO устраняет проблемы полностью.

В шаблоне SPA DTO содержит те же данные модели домена. Тем не менее они полезны по-прежнему так, как они Избегайте циклических ссылок из свойства навигации, и они показывают общий шаблон DTO.

**AccountModels.cs**

Этот файл содержит модели для членства узла. `UserProfile` Класс определяет схему для профилей пользователей в членство в базы данных. (В этом случае единственное является идентификатор пользователя и имя пользователя.) Другие классы модели в этом файле используются для создания формы регистрации и входа в систему пользователя.

## <a name="entity-framework"></a>Entity Framework

Шаблон SPA использует EF Code First. В шаблон разработки Code First определяющие модели сначала в коде и затем EF использует модель для создания базы данных. Можно также использовать EF с существующей базы данных ([Database First](https://msdn.microsoft.com/en-us/data/jj206878.aspx)).

`TodoItemContext` Класс в папке «Models» является производным от **DbContext**. Этот класс предоставляет между моделями и EF «связующего». `TodoItemContext` Содержит `ToDoItem` коллекции и `TodoList` коллекции. Запрос в базу данных, просто написать запрос LINQ на этих коллекций. Например Вот как можно выбрать все списки задач для пользователя «Alice».

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Можно также добавлять новые элементы к коллекции, обновлять элементы, или удалить элементы из коллекции и сохранение изменений для базы данных.

## <a name="aspnet-web-api-controllers"></a>Контроллеры веб-API ASP.NET

В ASP.NET Web API контроллеры являются объектами, которые обрабатывают запросы HTTP. Как уже упоминалось, SPA шаблон использует веб-API для включения операций CRUD в `ToDoList` и `ToDoItem` экземпляры. Контроллеры расположены в папке Controllers находится решения.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Обрабатывает HTTP-запросы для заданий
- `TodoListController`: Обрабатывает HTTP-запросов для списков дел.

Эти имена важны, так как веб-API, соответствует пути URI к имени контроллера. (Чтобы узнать, как веб-API направляет HTTP-запросы к контроллерам, см. [маршрутизации в ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Давайте рассмотрим `ToDoListController` класса. Он содержит член данных:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Используется для связи с EF, как описано выше. Методы контроллера реализации операций CRUD. Веб-API сопоставляет HTTP-запросы от клиента к методам контроллера, как показано ниже:

| HTTP-запроса | Метод контроллера | Описание |
| --- | --- | --- |
| GET/api/todo | `GetTodoLists` | Получает коллекцию списков действий. |
| GET/api/todo/*идентификатор* | `GetTodoList` | Возвращает список дел по Идентификатору |
| PUT/api/todo/*идентификатор* | `PutTodoList` | Обновляет список дел. |
| POST/api/todo | `PostTodoList` | Создает новый список дел. |
| Удаление/api/todo/*идентификатор* | `DeleteTodoList` | Удаляет список TODO. |

Обратите внимание, что URL-адреса, для некоторых операций содержит заполнители для значение идентификатора. Например, чтобы удалить в список с Идентификатором 42, URI имеет следующий вид `/api/todo/42`.

Дополнительные сведения об использовании веб-API для операций CRUD см. в разделе [Создание веб-API, поддерживает операции CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Код для этого контроллера выполняется очень просто. Ниже приведены некоторые интересные моменты:

- `GetTodoLists` Метод использует запрос LINQ для фильтрации результатов по Идентификатору пользователя, вошедшего в систему. Таким образом, пользователь видит только данные, к которой принадлежит ему. Кроме того, обратите внимание, что инструкция Select используется для преобразования `ToDoList` экземпляров в `TodoListDto` экземпляров.
- Методы PUT и POST Проверьте состояние модели перед изменением базы данных. Если **ModelState.IsValid** имеет значение false, эти методы возвращают HTTP 400 Bad Request. Дополнительные сведения о модели проверки в веб-API в [проверка модели](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Класс контроллера также снабжен **[Authorize]** атрибута. Этот атрибут проверяет, является ли HTTP-запрос проходит проверку подлинности. Если запрос не прошел проверку подлинности, клиент получает HTTP 401 несанкционированный доступ. Дополнительные сведения о проверки подлинности в [проверки подлинности и авторизации в ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Класс очень похож на `TodoListController`. Главное отличие — что он не определяет никаких методов GET, так как клиент получит заданий вместе с каждой список дел.

## <a name="mvc-controllers-and-views"></a>MVC контроллеры и представления

Контроллеров MVC также находятся в папке Controllers находится решения. `HomeController`Отображает основной HTML для приложения. Для контроллера Home оно определено в Views/Home/Index.cshtml. Домашняя страница отображает различное содержимое в зависимости от того, вошел ли пользователь:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Когда пользователи находятся в системе, они видят основной пользовательский Интерфейс. В противном случае они увидят панель имени входа. Обратите внимание, что этот условный визуализация происходит на стороне сервера. Никогда не пытаются скрываться конфиденциальное содержимое на стороне клиента & #8212anything, отправляется в ответе HTTP является видимым для тем, кто смотрит необработанные сообщения HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript на стороне клиента и Knockout.js

Теперь давайте рассмотрим со стороны сервера приложения для клиента. Шаблон SPA использует сочетание jQuery и Knockout.js создать smooth, интерактивный пользовательский Интерфейс. Knockout.js — это библиотека JavaScript, который позволяет с легкостью выполнить привязку к данным HTML. Knockout.js использует шаблон под названием «Model-View-ViewModel.»

- Модель — это данные домена (ToDo списки и элементы ToDo).
- Представление является HTML-документа.
- Модель представления является объект JavaScript, хранящего данные модели. Модель представления — абстракция код пользовательского интерфейса. Он не имеет сведений о представление HTML. Вместо этого он представляет абстрактные функции, представления, такие как «список элементов ToDo».

Представление данных привязан к модели представления. Обновления для модели представления автоматически отражаются в представлении. Привязки работают в обратном направлении. События в модели DOM (например, щелкает) привязаны к функциям в модели представления которых активировать вызовы AJAX.

Шаблон SPA организует JavaScript на стороне клиента в три уровня:

- TODO.DataContext.js: отправляет запросы AJAX.
- TODO.Model.js: Определяет моделей.
- TODO.ViewModel.js: Определяет модели представления.

![](knockoutjs-template/_static/image11.png)

Эти файлы расположены в папке решения скрипты или приложения.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** обрабатывает все вызовы AJAX контроллеров веб-API. (Вызовы AJAX для протоколирования определены в другом месте, ajaxlogin.js.)

**TODO.Model.js** определяет модели (браузер) на стороне клиента для списков дел. Существует два класса модели: todoItem и todoList.

Многие свойства в классах модели имеют тип «ko.observable». Наблюдаемые объекты являются как маскирования делает свою работу. Из [документации маскирования](http://knockoutjs.com/documentation/introduction.html): наблюдаемым является «объект JavaScript, можно уведомлять подписчиков об изменениях.» При изменении значения наблюдаемого маскирования обновляет HTML-элементов, связанных с этими наблюдаемые объекты. Например todoItem имеет наблюдаемые объекты для свойств title и isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Можно также подписаться на наблюдаемый объект в коде. Например класс todoItem подписывается на изменения в свойства «isDone» и «title»:

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Модель представления**

Представление модели определяется в todo.viewmodel.js. Модель представления является центральной точки, где приложение связывает элементы HTML страницы данных домена. В шаблоне SPA модели представления содержит массив наблюдаемый todoLists. Следующий код в модель представления сообщает маскирования применение привязки:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML и привязка данных

Основной HTML для страницы определяется в Views/Home/Index.cshtml. Так как мы используем привязки данных, код HTML является только шаблон, что фактически возвращает отображены. Использует маскирования *декларативный* привязки. Элементы страницы при привязке к данным путем добавления атрибута «привязка к данным» к элементу. Ниже приведен очень простой пример, взяты из маскирования документации:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

В этом примере маскирования обновляет содержимое  **&lt;span&gt;**  элемент со значением `myItems.count()`. При каждом изменении этого значения маскирования обновляется документ.

Маскировать предоставляет ряд различными типами привязок. Ниже приведены некоторые привязки, используемые в шаблоне SPA.

- **foreach**: позволяет итерации цикла и применить ту же разметку для каждого элемента в списке. Используется для отображения списков дел и заданий для выполнения. В пределах **foreach**, привязки применяется к элементам списка.
- **отображается**: используемая для переключения видимости. Скрытие разметки, если коллекция пуста или отобразить сообщение об ошибке.
- **значение**: используется для заполнения значений формы.
- **Нажмите кнопку**: привязывает события щелчка кнопкой мыши функцию, в модели представления.

## <a name="anti-csrf-protection"></a>Защита CSRF защиты

Подделки межсайтовых запросов (CSRF) — это атака, при которой вредоносный сайт отправляет запрос уязвимым сайта, где пользователь вошел в данный момент в. Чтобы предотвратить атаки CSRF, ASP.NET MVC использует *маркеров защиты от подделки*, также называется токены проверки запроса. Идея заключается в том, что сервер переводит случайным образом созданный маркер в веб-страницу. Когда клиент отправляет данные на сервер, его необходимо включить это значение в сообщении запроса.

Маркеров защиты от подделки работать, поскольку вредоносный страницы не удается прочитать токенов пользователя, из-за политики для одного источника. (Политика одного источника не документы, размещенные на двух разных сайтах доступ друг к другу с).

ASP.NET MVC предоставляет встроенную поддержку для маркеров защиты от подделки через [AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx) класса и [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx) атрибута. В настоящее время эта функция не встроена в веб-API. Однако шаблон SPA включает пользовательскую реализацию для веб-API. Этот код, определенный в `ValidateHttpAntiForgeryTokenAttribute` класс, который находится в папке фильтры решения. Дополнительные сведения о противодействия CSRF в веб-API см. в разделе [атак Предотвращение межсайтовой подделки запросов (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Заключение

Чтобы приступить к работе быстро создание современных интерактивных веб-приложений предназначен шаблон SPA. Он использует Knockout.js библиотеки для разделения из данных и приложений логики презентации (разметка HTML). Но маскирования не только библиотеки JavaScript, которые можно использовать для создания SPA. Если вы хотите изучить некоторые другие параметры, взгляните на [шаблоны, созданные сообществом разработчиков SPA](../templates/index.md).
