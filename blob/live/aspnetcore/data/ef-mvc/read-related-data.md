---
title: "Основные ASP.NET MVC с основными EF - чтение связанных данных - 6 из 10"
author: tdykstra
description: "В этом учебнике вам чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации."
keywords: "Присоединяет ASP.NET Core, Entity Framework Core, связанные данные"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="9b772-104">Чтение связанных данных - Core EF учебнику ASP.NET Core MVC (6 из 10)</span><span class="sxs-lookup"><span data-stu-id="9b772-104">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="9b772-105">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9b772-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9b772-106">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b772-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="9b772-107">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="9b772-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="9b772-108">В предыдущем учебнике завершить модели данных School.</span><span class="sxs-lookup"><span data-stu-id="9b772-108">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="9b772-109">В этом учебнике предстоит чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-109">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="9b772-110">На следующих рисунках страницы, вы будете работать с.</span><span class="sxs-lookup"><span data-stu-id="9b772-110">The following illustrations show the pages that you'll work with.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="9b772-113">Упреждающая, явные и отложенной загрузки связанных данных</span><span class="sxs-lookup"><span data-stu-id="9b772-113">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="9b772-114">Существует несколько способов объектно-реляционного сопоставления (ORM) программного обеспечения, например Entity Framework можно загрузить связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="9b772-114">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="9b772-115">Безотложная загрузка.</span><span class="sxs-lookup"><span data-stu-id="9b772-115">Eager loading.</span></span> <span data-ttu-id="9b772-116">При чтении сущности связанные данные извлекаются вместе с ним.</span><span class="sxs-lookup"><span data-stu-id="9b772-116">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="9b772-117">Обычно в результате запроса одного соединения, который получает все данные, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="9b772-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="9b772-118">Указать упреждающую в Entity Framework Core, используя `Include` и `ThenInclude` методы.</span><span class="sxs-lookup"><span data-stu-id="9b772-118">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Пример Безотложная загрузка](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="9b772-120">Вы можете получить некоторые данные в отдельных запросах и EF» по исправлению» свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-120">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="9b772-121">То есть EF автоматически добавляет отдельно извлеченных сущностей, к которым они принадлежат свойств навигации ранее извлеченных объектов.</span><span class="sxs-lookup"><span data-stu-id="9b772-121">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="9b772-122">Для запроса, который получает данные, можно использовать `Load` метод вместо метода, который возвращает список или объекта, таких как `ToList` или `Single`.</span><span class="sxs-lookup"><span data-stu-id="9b772-122">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="9b772-124">Явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="9b772-124">Explicit loading.</span></span> <span data-ttu-id="9b772-125">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="9b772-125">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9b772-126">Написать код, который извлекает данные при необходимости.</span><span class="sxs-lookup"><span data-stu-id="9b772-126">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="9b772-127">Как в случае загрузки с отдельных запросов eager явную загрузку результатов в нескольких запросах отправляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9b772-127">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="9b772-128">Разница заключается в том, что с явная загрузка код задает свойства навигации для загрузки.</span><span class="sxs-lookup"><span data-stu-id="9b772-128">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="9b772-129">В Entity Framework Core 1.1 можно использовать `Load` метод для выполнения явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="9b772-129">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="9b772-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="9b772-130">For example:</span></span>

  ![Пример явная загрузка](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="9b772-132">Отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="9b772-132">Lazy loading.</span></span> <span data-ttu-id="9b772-133">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="9b772-133">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="9b772-134">Однако первой попытке доступа к свойству навигации, данные, необходимые для этого свойства навигации автоматически извлекается.</span><span class="sxs-lookup"><span data-stu-id="9b772-134">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="9b772-135">Отправляется запрос к базе данных каждый раз при попытке получить данные из свойства навигации в первый раз.</span><span class="sxs-lookup"><span data-stu-id="9b772-135">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="9b772-136">Entity Framework Core 1.0 не поддерживает отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="9b772-136">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="9b772-137">Особенности производительности</span><span class="sxs-lookup"><span data-stu-id="9b772-137">Performance considerations</span></span>

<span data-ttu-id="9b772-138">Если известно, что требуются связанные данные для каждой сущности, которые получены, активная загрузка часто обеспечивает оптимальную производительность, поскольку одного запроса, отправляемые в базу данных обычно более эффективно, чем отдельные запросы для каждой сущности, которые получены.</span><span class="sxs-lookup"><span data-stu-id="9b772-138">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="9b772-139">Например предположим, что для каждого отдела содержит десять связанных курсов.</span><span class="sxs-lookup"><span data-stu-id="9b772-139">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="9b772-140">Безотложная загрузка всех связанных данных приведет к запрос одним (join) и одним кругового пути к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9b772-140">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="9b772-141">Отдельный запрос для курсов для каждого отдела приведет к одиннадцать циклов приема-передачи в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9b772-141">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="9b772-142">При высокой задержки, дополнительных циклов обращения к базе данных, особенно к падению производительности.</span><span class="sxs-lookup"><span data-stu-id="9b772-142">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="9b772-143">С другой стороны в некоторых сценариях отдельные запросы более эффективно.</span><span class="sxs-lookup"><span data-stu-id="9b772-143">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="9b772-144">Безотложная загрузка всех связанных данных в одном запросе может привести к очень сложного соединения для создаваемого, который SQL Server не может эффективно обработать.</span><span class="sxs-lookup"><span data-stu-id="9b772-144">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="9b772-145">Или, если необходимо получить доступ к свойствам навигации сущности только для подмножества набора сущностей, который обработка отдельных запросов могут выполняться эффективнее, так как Безотложная загрузка всего заранее бы получить больше данных, чем необходимо.</span><span class="sxs-lookup"><span data-stu-id="9b772-145">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="9b772-146">Если важна производительность, рекомендуется проверить производительность оба способа, чтобы выбрать наиболее подходящую.</span><span class="sxs-lookup"><span data-stu-id="9b772-146">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="9b772-147">Создание страницы курсов, отображающий название отдела</span><span class="sxs-lookup"><span data-stu-id="9b772-147">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="9b772-148">Сущность курс включает свойство навигации, которое содержит сущности «отдел» отдела, назначенный курса.</span><span class="sxs-lookup"><span data-stu-id="9b772-148">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="9b772-149">Чтобы отобразить имя подразделения, назначенный в списке курсов, необходимо получить имя свойства из сущности «отдел», который находится в `Course.Department` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-149">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="9b772-150">Контроллер с именем CoursesController для типа сущности курса, используя те же параметры для создания **контроллер MVC с представлениями, использующий Entity Framework** scaffolder, которые были ранее для контроллера учащихся, как показано в на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="9b772-150">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Добавить контроллер курсов](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="9b772-152">Откройте *CoursesController.cs* и изучить `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="9b772-152">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="9b772-153">Автоматическое формирование шаблонов указал упреждающую для `Department` свойство навигации с помощью `Include` метод.</span><span class="sxs-lookup"><span data-stu-id="9b772-153">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="9b772-154">Замените `Index` метод следующим кодом, который использует более подходящее имя для `IQueryable` , возвращающий курса сущностей (`courses` вместо `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="9b772-154">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="9b772-155">Откройте *Views/Courses/Index.cshtml* и замените код шаблона с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="9b772-155">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="9b772-156">Изменения выделены:</span><span class="sxs-lookup"><span data-stu-id="9b772-156">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="9b772-157">Для формирования шаблонов код внесли следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="9b772-157">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="9b772-158">Изменить заголовок из индекса к курсам.</span><span class="sxs-lookup"><span data-stu-id="9b772-158">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="9b772-159">Добавлен **номер** столбца, отображающего `CourseID` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="9b772-159">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="9b772-160">По умолчанию первичные ключи не являются формирования шаблонов, так как обычно они не имеют смысла для конечных пользователей.</span><span class="sxs-lookup"><span data-stu-id="9b772-160">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="9b772-161">Однако в этом случае первичный ключ имеет смысл, и вы хотите отображать их.</span><span class="sxs-lookup"><span data-stu-id="9b772-161">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="9b772-162">Изменить **отдел** столбец, отображающий название отдела.</span><span class="sxs-lookup"><span data-stu-id="9b772-162">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="9b772-163">Код отображает `Name` свойства сущности «отдел», который загружается `Department` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="9b772-163">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="9b772-164">Запустите приложение и выберите **курсы** вкладку, чтобы просмотреть список с именами отдела.</span><span class="sxs-lookup"><span data-stu-id="9b772-164">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="9b772-166">Создание страницы инструкторов курсов и регистрации</span><span class="sxs-lookup"><span data-stu-id="9b772-166">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="9b772-167">В этом разделе вы создадите представления сущность инструктора и контроллер, чтобы отобразить страницу «преподаватели»:</span><span class="sxs-lookup"><span data-stu-id="9b772-167">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

<span data-ttu-id="9b772-169">Эта страница считывает и взаимосвязанные данные отображаются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9b772-169">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="9b772-170">Появится список инструкторов взаимосвязанные данные из OfficeAssignment сущности.</span><span class="sxs-lookup"><span data-stu-id="9b772-170">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="9b772-171">Сущности инструктора и OfficeAssignment являются отношением один к нулю или одному.</span><span class="sxs-lookup"><span data-stu-id="9b772-171">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="9b772-172">Вы воспользуетесь упреждающую OfficeAssignment сущностей.</span><span class="sxs-lookup"><span data-stu-id="9b772-172">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="9b772-173">Как упоминалось ранее, упреждающую обычно более эффективна при необходимости связанные данные для всех извлеченных строк таблицы первичного.</span><span class="sxs-lookup"><span data-stu-id="9b772-173">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="9b772-174">В этом случае будет отображаться office назначения для всех отображаемых инструкторов.</span><span class="sxs-lookup"><span data-stu-id="9b772-174">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="9b772-175">Когда пользователь выбирает инструктор, отображаются связанные сущности курса.</span><span class="sxs-lookup"><span data-stu-id="9b772-175">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="9b772-176">В отношении многие ко многим являются сущности инструктора и курса.</span><span class="sxs-lookup"><span data-stu-id="9b772-176">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="9b772-177">Безотложная загрузка будет использоваться для набора сущностей и их связанных сущностей отдела.</span><span class="sxs-lookup"><span data-stu-id="9b772-177">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="9b772-178">В этом случае отдельные запросы могут оказаться эффективными, поскольку требуется курсы только для выбранных инструктора.</span><span class="sxs-lookup"><span data-stu-id="9b772-178">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="9b772-179">Однако в этом примере показано, как использовать упреждающую свойств навигации в сущностях, которые сами в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-179">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="9b772-180">Когда пользователь выбирает курса, отображается взаимосвязанные данные из набора сущностей регистраций.</span><span class="sxs-lookup"><span data-stu-id="9b772-180">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="9b772-181">Сущности курс и регистрации находятся в отношении один ко многим.</span><span class="sxs-lookup"><span data-stu-id="9b772-181">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="9b772-182">Вы используете отдельные запросы для регистрации объектов и их связанные сущности студента.</span><span class="sxs-lookup"><span data-stu-id="9b772-182">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="9b772-183">Создать модель представлений для представления инструктора индекса</span><span class="sxs-lookup"><span data-stu-id="9b772-183">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="9b772-184">На странице инструкторов отображаются данные из трех разных таблицах.</span><span class="sxs-lookup"><span data-stu-id="9b772-184">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="9b772-185">Таким образом вы создадите модель представления, которая включает три свойства каждого хранения данных для одной из таблиц.</span><span class="sxs-lookup"><span data-stu-id="9b772-185">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="9b772-186">В *SchoolViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="9b772-186">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="9b772-187">Создание контроллера инструктора и представления</span><span class="sxs-lookup"><span data-stu-id="9b772-187">Create the Instructor controller and views</span></span>

<span data-ttu-id="9b772-188">Создайте инструкторов контроллер с действиями чтения и записи EF, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="9b772-188">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Добавление контроллера преподавателей](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="9b772-190">Откройте *InstructorsController.cs* и добавить с помощью инструкции для пространства имен ViewModels:</span><span class="sxs-lookup"><span data-stu-id="9b772-190">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="9b772-191">Замените метод индекс следующий код, чтобы сделать безотложной загрузкой связанных данных и поместить его в модели представления.</span><span class="sxs-lookup"><span data-stu-id="9b772-191">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="9b772-192">Этот метод принимает необязательный маршрутизации данных (`id`) и параметр строки запроса (`courseID`), содержащие значения ID выбранного инструктора и выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="9b772-192">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="9b772-193">Параметры, предоставляемые **выберите** гиперссылок на странице.</span><span class="sxs-lookup"><span data-stu-id="9b772-193">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="9b772-194">Код начинается с создания экземпляра модели представления и помещения его в списке инструкторов.</span><span class="sxs-lookup"><span data-stu-id="9b772-194">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="9b772-195">В коде задается упреждающую для `Instructor.OfficeAssignment` и `Instructor.CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-195">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="9b772-196">В `CourseAssignments` свойство, `Course` свойство загружен и в этом домене, `Enrollments` и `Department` свойства уже загружены и в каждом `Enrollment` сущности `Student` свойство загружено.</span><span class="sxs-lookup"><span data-stu-id="9b772-196">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="9b772-197">Так как представление всегда требует OfficeAssignment сущности, значительно эффективнее извлекать, в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="9b772-197">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="9b772-198">При инструктор выделено на веб-странице, так что один запрос лучше, чем несколько запросов только в том случае, если эта страница отображается чаще с курсом выбран чем без курса сущности не требуется.</span><span class="sxs-lookup"><span data-stu-id="9b772-198">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="9b772-199">Код повторяется `CourseAssignments` и `Course` так, как требуется два свойства из `Course`.</span><span class="sxs-lookup"><span data-stu-id="9b772-199">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="9b772-200">Первая строка `ThenInclude` вызывает возвращает `CourseAssignment.Course`, `Course.Enrollments`, и `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="9b772-200">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="9b772-201">На этом этапе в коде другой `ThenInclude` бы для свойства навигации `Student`, которой не требуется.</span><span class="sxs-lookup"><span data-stu-id="9b772-201">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="9b772-202">Но вызывающий `Include` начинает ее заново с `Instructor` свойства, поэтому вам придется проходить по цепочке еще раз, указав это время `Course.Department` вместо `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="9b772-202">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="9b772-203">Следующий код выполняется, когда был выбран инструктор.</span><span class="sxs-lookup"><span data-stu-id="9b772-203">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="9b772-204">Выбранный инструктора извлекается из списка инструкторов в модели представления.</span><span class="sxs-lookup"><span data-stu-id="9b772-204">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="9b772-205">Модель представления `Courses` свойство затем загружен с курса сущностей из этого инструктора `CourseAssignments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-205">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="9b772-206">`Where` Метод возвращает коллекцию, но в этом случае условия, переданные в результат метода только одну инструктора сущность возвращается.</span><span class="sxs-lookup"><span data-stu-id="9b772-206">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="9b772-207">`Single` Метод преобразует коллекцию в одной сущности инструктора предоставляет доступ к этой сущности `CourseAssignments` свойство.</span><span class="sxs-lookup"><span data-stu-id="9b772-207">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="9b772-208">`CourseAssignments` Свойство содержит `CourseAssignment` сущности, с которых будет осуществляться только связанные `Course` сущностей.</span><span class="sxs-lookup"><span data-stu-id="9b772-208">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="9b772-209">Вы используете `Single` метод в коллекции, если известно коллекции будет иметь только один элемент.</span><span class="sxs-lookup"><span data-stu-id="9b772-209">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="9b772-210">Один метод создает исключение, если возвращается пустая коллекция, переданы в него, или если существует более одного элемента.</span><span class="sxs-lookup"><span data-stu-id="9b772-210">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="9b772-211">Альтернативным вариантом является `SingleOrDefault`, который возвращает значение по умолчанию (null в данном случае), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="9b772-211">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="9b772-212">Однако в этом случае, по-прежнему приведет к исключению (попытки найти `Courses` свойство на указатель null), и сообщение об исключении менее четко указывает на причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="9b772-212">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="9b772-213">При вызове `Single` метод, можно также передать в предложении Where условие вместо вызова метода `Where` метод отдельно:</span><span class="sxs-lookup"><span data-stu-id="9b772-213">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="9b772-214">вместо следующего кода:</span><span class="sxs-lookup"><span data-stu-id="9b772-214">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="9b772-215">Далее Если был выбран курса, выбранного курса извлекается из списка курсов в модели представления.</span><span class="sxs-lookup"><span data-stu-id="9b772-215">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="9b772-216">Затем модели представления `Enrollments` свойство загружается с регистрации сущностей из этого курса `Enrollments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-216">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="9b772-217">Изменить представление Index инструктора</span><span class="sxs-lookup"><span data-stu-id="9b772-217">Modify the Instructor Index view</span></span>

<span data-ttu-id="9b772-218">В *Views/Instructors/Index.cshtml*, замените код шаблона в следующий код.</span><span class="sxs-lookup"><span data-stu-id="9b772-218">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="9b772-219">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="9b772-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="9b772-220">Следующие изменения, внесенные в существующий код:</span><span class="sxs-lookup"><span data-stu-id="9b772-220">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="9b772-221">Класс модели, чтобы изменить `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="9b772-221">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="9b772-222">Изменить заголовок страницы из **индекс** для **инструкторов**.</span><span class="sxs-lookup"><span data-stu-id="9b772-222">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="9b772-223">Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="9b772-223">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="9b772-224">(Так как один к нулю или одному связь, могут не существовать связанной сущности OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="9b772-224">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="9b772-225">Добавлен **курсы** столбец, отображающий курсы при изучении каждого инструктора.</span><span class="sxs-lookup"><span data-stu-id="9b772-225">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="9b772-226">В разделе [явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) для получения дополнительных сведений о данного синтаксиса razor.</span><span class="sxs-lookup"><span data-stu-id="9b772-226">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="9b772-227">Добавленный код, который динамически добавляет `class="success"` для `tr` инструктора выбранного элемента.</span><span class="sxs-lookup"><span data-stu-id="9b772-227">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="9b772-228">Это задает цвет фона для строк, выделенных с помощью класса начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="9b772-228">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="9b772-229">Добавлена новая гиперссылка с меткой **выберите** непосредственно перед другие ссылки в каждой строке, что вызывает идентификатор выбранного инструктора отправку `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="9b772-229">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="9b772-230">Запустите приложение и выберите **инструкторов** вкладки. На странице отображается свойство Location для связанных сущностей OfficeAssignment и по пустой ячейке таблице при нет связанной сущности OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="9b772-230">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Страница индекса инструкторов ничего не выбраны](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="9b772-232">В *Views/Instructors/Index.cshtml* файла после закрытия таблицы элемента (в конце файла), добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="9b772-232">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="9b772-233">Этот код отображает список связанных с инструктор при выборе инструктор курсов.</span><span class="sxs-lookup"><span data-stu-id="9b772-233">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="9b772-234">Этот код считывает `Courses` свойство модели представления для отображения списка курсов.</span><span class="sxs-lookup"><span data-stu-id="9b772-234">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="9b772-235">Он также предоставляет **выберите** гиперссылку, которая отправляет идентификатор курса для `Index` метода действия.</span><span class="sxs-lookup"><span data-stu-id="9b772-235">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="9b772-236">Обновите страницу и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="9b772-236">Refresh the page and select an instructor.</span></span> <span data-ttu-id="9b772-237">Вы увидите на таблице, которая отображает курсов, которые назначены выбранной инструктора, и для каждого курса вы увидите имя подразделения, назначенный.</span><span class="sxs-lookup"><span data-stu-id="9b772-237">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Инструкторов индекс страницы инструктора выбран](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="9b772-239">После только что добавленного блока кода добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="9b772-239">After the code block you just added, add the following code.</span></span> <span data-ttu-id="9b772-240">Это список учащихся, которые участвуют в курса при выборе этого курса.</span><span class="sxs-lookup"><span data-stu-id="9b772-240">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="9b772-241">Этот код считывает свойство регистраций модели представления для отображения списка учащихся, зарегистрированных в этом курсе.</span><span class="sxs-lookup"><span data-stu-id="9b772-241">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="9b772-242">Обновите страницу еще раз и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="9b772-242">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="9b772-243">Выберите курс, чтобы просмотреть список зарегистрированных студентов и их оценки.</span><span class="sxs-lookup"><span data-stu-id="9b772-243">Then select a course to see the list of enrolled students and their grades.</span></span>

![Инструктора страницы индекса инструкторов и выбранного курса](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="9b772-245">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="9b772-245">Explicit loading</span></span>

<span data-ttu-id="9b772-246">При получении списка инструкторов в *InstructorsController.cs*, указан упреждающую для `CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-246">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="9b772-247">Предположим, что ожидается пользователям лишь изредка могут понадобиться регистраций в выбранной инструктора и курс.</span><span class="sxs-lookup"><span data-stu-id="9b772-247">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="9b772-248">В этом случае может потребоваться загрузить данные регистрации только в том случае, если она запрошена.</span><span class="sxs-lookup"><span data-stu-id="9b772-248">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="9b772-249">Чтобы увидеть пример того, как сделать явная загрузка, замените `Index` метод следующим кодом, который удаляет упреждающую для регистрации и явно загружает свойства.</span><span class="sxs-lookup"><span data-stu-id="9b772-249">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="9b772-250">Изменения в коде, выделяются.</span><span class="sxs-lookup"><span data-stu-id="9b772-250">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="9b772-251">Новый код удаляет *ThenInclude* вызывает метод для регистрации данных из кода, которая извлекает сущности инструктора.</span><span class="sxs-lookup"><span data-stu-id="9b772-251">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="9b772-252">Если выбраны инструктора и курс, выделенный код извлекает сущности регистрации выбранного курса и студента сущности для каждой регистрации.</span><span class="sxs-lookup"><span data-stu-id="9b772-252">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="9b772-253">Запуск приложения, перейдите на страницу индекса инструкторов и вы увидите не отличается от отображаемых на странице, несмотря на то, что вы изменили извлечением данных.</span><span class="sxs-lookup"><span data-stu-id="9b772-253">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="9b772-254">Сводка</span><span class="sxs-lookup"><span data-stu-id="9b772-254">Summary</span></span>

<span data-ttu-id="9b772-255">Теперь использовалась упреждающую с одного запроса и несколько запросов на чтение связанных данных в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="9b772-255">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="9b772-256">В следующем уроке будет рассмотрено обновления связанных данных.</span><span class="sxs-lookup"><span data-stu-id="9b772-256">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="9b772-257">[Назад](complex-data-model.md)
>[Вперед](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="9b772-257">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  