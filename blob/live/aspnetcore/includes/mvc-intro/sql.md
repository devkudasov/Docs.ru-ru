# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="bdc52-101">Работа с SQLite в проекте ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="bdc52-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="bdc52-102">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="bdc52-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bdc52-103">Объект `MvcMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="bdc52-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bdc52-104">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bdc52-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="bdc52-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="bdc52-105">SQLite</span></span>

<span data-ttu-id="bdc52-106">На веб-сайте [SQLite](https://www.sqlite.org/) указывается следующее:</span><span class="sxs-lookup"><span data-stu-id="bdc52-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="bdc52-107">SQLite — это автономная внедряемая полнофункциональная общедоступная система управления базами данных SQL с высокой степенью надежности.</span><span class="sxs-lookup"><span data-stu-id="bdc52-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="bdc52-108">На данный момент SQLite является самой популярной СУБД в мире.</span><span class="sxs-lookup"><span data-stu-id="bdc52-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="bdc52-109">Для просмотра баз данных SQLite, а также управления ими можно использовать множество самых разных сторонних инструментов.</span><span class="sxs-lookup"><span data-stu-id="bdc52-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="bdc52-110">На следующем рисунке показан [DB Browser для SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="bdc52-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="bdc52-111">Если вы предпочитаете использовать другое средство для работы с SQLite, напишите нам в комментариях, чем именно оно нравится вам.</span><span class="sxs-lookup"><span data-stu-id="bdc52-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser для SQLite с базой данных movie](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="bdc52-113">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="bdc52-113">Seed the database</span></span>

<span data-ttu-id="bdc52-114">Создайте класс `SeedData` в папке *Models*.</span><span class="sxs-lookup"><span data-stu-id="bdc52-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="bdc52-115">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bdc52-115">Replace the generated code with the following:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="bdc52-116">Если в базе данных есть фильмы, возвращается инициализатор заполнения.</span><span class="sxs-lookup"><span data-stu-id="bdc52-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="bdc52-117">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="bdc52-117">Add the seed initializer</span></span>

<span data-ttu-id="bdc52-118">Добавьте инициализатор заполнения в метод `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bdc52-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a><span data-ttu-id="bdc52-119">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="bdc52-119">Test the app</span></span>

<span data-ttu-id="bdc52-120">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="bdc52-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="bdc52-121">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="bdc52-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="bdc52-122">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="bdc52-122">The app shows the seeded data.</span></span>

![Приложение MVC Movie с данными по фильмам, открытое в браузере](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)