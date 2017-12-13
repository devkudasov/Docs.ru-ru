---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: "Использовать для инициализации базы данных Code First Migrations | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: df75a69644033cc76fee86b5a9692ab65beb4d01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="29b05-102">Использовать Code First Migrations для инициализации базы данных</span><span class="sxs-lookup"><span data-stu-id="29b05-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="29b05-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="29b05-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="29b05-104">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="29b05-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="29b05-105">В этом разделе используется [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) в EF в качестве начального базы данных с тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="29b05-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/en-us/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="29b05-106">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="29b05-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="29b05-107">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="29b05-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="29b05-108">Эта команда добавляет папку с именем миграции в проект, а также файл кода с именем Configuration.cs в папке миграции.</span><span class="sxs-lookup"><span data-stu-id="29b05-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="29b05-109">Откройте файл Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="29b05-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="29b05-110">Добавьте следующие **с помощью** инструкции.</span><span class="sxs-lookup"><span data-stu-id="29b05-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="29b05-111">Затем добавьте следующий код в **Configuration.Seed** метод:</span><span class="sxs-lookup"><span data-stu-id="29b05-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="29b05-112">В окне консоли диспетчера пакетов введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="29b05-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="29b05-113">Первая команда создает код, который создает базу данных, а вторая команда выполняет этот код.</span><span class="sxs-lookup"><span data-stu-id="29b05-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="29b05-114">База данных создается локально, с помощью [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="29b05-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/en-us/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="29b05-115">Просмотр API (необязательно)</span><span class="sxs-lookup"><span data-stu-id="29b05-115">Explore the API (Optional)</span></span>

<span data-ttu-id="29b05-116">Нажмите клавишу F5 для запуска приложения в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="29b05-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="29b05-117">Visual Studio запускает IIS Express и веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="29b05-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="29b05-118">Затем Visual Studio запускает браузер и открывает домашнюю страницу приложения.</span><span class="sxs-lookup"><span data-stu-id="29b05-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="29b05-119">При запуске веб-проекта Visual Studio назначается номер порта.</span><span class="sxs-lookup"><span data-stu-id="29b05-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="29b05-120">На рисунке ниже номер порта — 50524.</span><span class="sxs-lookup"><span data-stu-id="29b05-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="29b05-121">При запуске приложения вы увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="29b05-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="29b05-122">На домашней странице реализуется с помощью ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="29b05-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="29b05-123">В верхней части страницы есть ссылка с надписью «API».</span><span class="sxs-lookup"><span data-stu-id="29b05-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="29b05-124">Эта ссылка позволяет на страницу справки, автоматически созданный для веб-API.</span><span class="sxs-lookup"><span data-stu-id="29b05-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="29b05-125">(Чтобы узнать, как создается Эта страница справки и сведения о добавлении вашей собственной документации на страницу, в разделе [Создание страницах справки для веб-API ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Справка щелкайте ссылки на страницы для просмотра сведений об API-Интерфейсе, включая формат запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="29b05-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="29b05-126">API-Интерфейс позволяет операций CRUD в базе данных.</span><span class="sxs-lookup"><span data-stu-id="29b05-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="29b05-127">В следующей таблице показаны API.</span><span class="sxs-lookup"><span data-stu-id="29b05-127">The following summarizes the API.</span></span>

| <span data-ttu-id="29b05-128">Authors</span><span class="sxs-lookup"><span data-stu-id="29b05-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="29b05-129">Получение api и авторов</span><span class="sxs-lookup"><span data-stu-id="29b05-129">GET api/authors</span></span> | <span data-ttu-id="29b05-130">Получение всех авторов.</span><span class="sxs-lookup"><span data-stu-id="29b05-130">Get all authors.</span></span> |
| <span data-ttu-id="29b05-131">Api GET/авторы / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-131">GET api/authors/{id}</span></span> | <span data-ttu-id="29b05-132">Получить автора по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="29b05-132">Get an author by ID.</span></span> |
| <span data-ttu-id="29b05-133">POST/api/авторов</span><span class="sxs-lookup"><span data-stu-id="29b05-133">POST /api/authors</span></span> | <span data-ttu-id="29b05-134">Создание нового автора.</span><span class="sxs-lookup"><span data-stu-id="29b05-134">Create a new author.</span></span> |
| <span data-ttu-id="29b05-135">ПОМЕСТИТЕ /api/авторы / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="29b05-136">Обновите существующие автора.</span><span class="sxs-lookup"><span data-stu-id="29b05-136">Update an existing author.</span></span> |
| <span data-ttu-id="29b05-137">УДАЛИТЬ /api/авторы / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="29b05-138">Удалите автора.</span><span class="sxs-lookup"><span data-stu-id="29b05-138">Delete an author.</span></span> |

| <span data-ttu-id="29b05-139">Books</span><span class="sxs-lookup"><span data-stu-id="29b05-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="29b05-140">ПОЛУЧИТЬ /api/books</span><span class="sxs-lookup"><span data-stu-id="29b05-140">GET /api/books</span></span> | <span data-ttu-id="29b05-141">Получение всех книг.</span><span class="sxs-lookup"><span data-stu-id="29b05-141">Get all books.</span></span> |
| <span data-ttu-id="29b05-142">ПОЛУЧИТЬ /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-142">GET /api/books/{id}</span></span> | <span data-ttu-id="29b05-143">Получение книги по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="29b05-143">Get a book by ID.</span></span> |
| <span data-ttu-id="29b05-144">ОТПРАВЛЯТЬ/api/книги</span><span class="sxs-lookup"><span data-stu-id="29b05-144">POST /api/books</span></span> | <span data-ttu-id="29b05-145">Создайте новую книгу.</span><span class="sxs-lookup"><span data-stu-id="29b05-145">Create a new book.</span></span> |
| <span data-ttu-id="29b05-146">ПОМЕСТИТЕ /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="29b05-147">Обновите существующую книгу.</span><span class="sxs-lookup"><span data-stu-id="29b05-147">Update an existing book.</span></span> |
| <span data-ttu-id="29b05-148">УДАЛИТЬ /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="29b05-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="29b05-149">Удалите книгу.</span><span class="sxs-lookup"><span data-stu-id="29b05-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="29b05-150">Просмотр базы данных (необязательно)</span><span class="sxs-lookup"><span data-stu-id="29b05-150">View the Database (Optional)</span></span>

<span data-ttu-id="29b05-151">При выполнении команды Update-Database, EF создана база данных и вызывается `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="29b05-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="29b05-152">При локальном запуске приложения использует EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="29b05-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="29b05-153">В Visual Studio можно просматривать базы данных.</span><span class="sxs-lookup"><span data-stu-id="29b05-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="29b05-154">Из **представление** последовательно выберите пункты **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="29b05-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="29b05-155">В **соединение с сервером** диалоговое окно, в **имя сервера** "Правка", введите «(localdb) \v11.0».</span><span class="sxs-lookup"><span data-stu-id="29b05-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="29b05-156">Оставить **проверки подлинности** параметра «Проверка подлинности Windows».</span><span class="sxs-lookup"><span data-stu-id="29b05-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="29b05-157">Нажмите кнопку **Подключиться**.</span><span class="sxs-lookup"><span data-stu-id="29b05-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="29b05-158">Visual Studio подключится к LocalDB и показывает существующих баз данных в окне обозревателя объектов SQL Server.</span><span class="sxs-lookup"><span data-stu-id="29b05-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="29b05-159">Можно развернуть узлы для просмотра таблицы, созданные EF.</span><span class="sxs-lookup"><span data-stu-id="29b05-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="29b05-160">Чтобы просмотреть данные, щелкните правой кнопкой мыши таблицу и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="29b05-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="29b05-161">На следующем рисунке показан результаты для электронной таблицы.</span><span class="sxs-lookup"><span data-stu-id="29b05-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="29b05-162">Обратите внимание, что EF наполнения базы данных с данными начального значения и таблица содержит внешний ключ к таблице Authors.</span><span class="sxs-lookup"><span data-stu-id="29b05-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
<span data-ttu-id="29b05-163">[Назад](part-2.md)
[Вперед](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="29b05-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>