---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: "Веб-рекомендации по разработке (Создание реальных облачных приложений в Azure) | Документы Microsoft"
author: MikeWasson
description: "Построение реального мира облачными приложениями с помощью Azure электронная книга основан на разработанный Скотт Гатри презентации. Объясняет, 13 шаблоны и рекомендации, которые он может..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: a40a3779ddc416e141dd27b665f43830a43590b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web рекомендации по разработке (Создание реальных облачных приложений в Azure)
====================
по [Mike Wasson](https://github.com/MikeWasson), [Рик Андерсон](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Загрузка решение проект](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) или [Загрузите электронную](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Построение реального мира облачными приложениями с помощью Azure** электронная книга основана на представления, разработанный Скотт Гатри. Объясняются 13 шаблоны и рекомендации, которые помогут вам быть успешной разработки веб-приложений для облака. Сведения об электронных книг см. в разделе [первой главы](introduction.md).


Первые три шаблоны были аспектам гибкого процесса разработки; остальные являются об архитектуре и кода. Такой — это совокупность рекомендации по разработке web:

- [Веб-серверов](#stateless) за подсистемой балансировки нагрузки смарт.
- [Избежать состояния сеанса](#sessionstate) (или, если ее не избежать, используйте распределенного кэша, а не базы данных).
- [Использование CDN](#cdn) пограничного кэша статических файлов ОС (образы, сценарии).
- [Поддержка асинхронного выполнения .NET 4.5 использовать](#async) во избежание блокирующих вызовов.

Эти методы являются допустимыми для всех веб-разработки, не только для облачных приложений, но они особенно важно для облачных приложений. Они работают совместно для обеспечения оптимального использования гибкие масштабирование, предлагаемых облачной среде. При несоблюдении этих рекомендаций, вы будете запускать в ограничения при попытке масштабирования приложения.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Без учета состояния веб-уровень за подсистемой балансировки нагрузки смарт-

*Без учета состояния веб-уровень* означает не храните данные приложения в веб-сервере памяти или файлов системы. Поддержание уровень сети без сохранения состояния позволяет улучшить взаимодействие клиента и сэкономить деньги:

- Если оно находится за подсистемой балансировки нагрузки веб-уровня без сохранения состояния, может быстро отвечать на изменения в приложения трафика динамически, добавив или удалив серверы. В облачной среде, где вы платите только за ресурсы сервера для, пока они фактически используются эту возможность реагировать на изменения в требование можно перевести на сэкономить значительные средства.
- Без учета состояния веб-уровня с точки зрения архитектуры намного проще масштабировать приложение. Слишком, дает возможность отвечать на масштабирование потребностей быстрее и меньше затраты на разработку и тестирование в процессе.
- Серверах облака, как и локальные серверы, необходимо исправить и перезагрузить время от времени; и если веб-уровня без сохранения состояния, перенаправление трафика, когда сервер временно отключается не будут вызывать ошибки или непредвиденное поведение.

Большинство реальных приложений требуется для хранения состояния сеанса web; самое главное — не хранить его на веб-сервере. Можно хранить состояние другими способами, например, на клиенте в файлах cookie или за пределы серверную процесса в состояние сеанса ASP.NET с помощью поставщика кэша. Вы можете хранить файлы в [хранилища больших двоичных объектов Windows Azure](unstructured-blob-storage.md) вместо локальной файловой системе.

В качестве примера того, как просто масштабирование приложения в Windows Azure Web Sites, если уровень сети без сохранения состояния, в разделе **шкалы** вкладки для Windows Azure веб-сайта на портале управления:

![Вкладка "масштаб"](web-development-best-practices/_static/image1.png)

Если вы хотите добавить веб-серверы, можно просто перетащите экземпляр счетчика ползунок вправо. Задайте для него значение 5 и нажмите кнопку **Сохранить**, и за несколько секунд при наличии 5 веб-серверы обработки трафика веб-сайта Windows Azure.

![Пять экземпляров](web-development-best-practices/_static/image2.png)

Легко можно задать число экземпляров до 3 или до 1. Можно сократить, начинается экономит деньги немедленно, так как Windows Azure не с почасовой оплатой по минутам.

Также можно настроить Windows Azure автоматически увеличивать или уменьшать количество веб-серверов, в зависимости от использования ЦП. В следующем примере при ЦП становится меньше 60%, приведет к уменьшению число веб-серверов для как минимум 2 и если потребление ресурсов ЦП становится выше 80%, не более 4 будет увеличено количество веб-серверов.

![Масштабирование по ЦП](web-development-best-practices/_static/image3.png)

Или, что делать, если вы знаете, что веб-узла будет только занятости в рабочее время? Чтобы узнать Windows Azure для запуска нескольких серверов во время дневного и уменьшаться в односерверной числе по вечерам, nights и выходных дней. Следующий ряд снимков экрана показано, как настроить веб-сайт для запуска на одном сервере в нерабочие часы и 4 сервера во время работы с 8: 00 до 17: 00.

![Масштабирование по расписанию.](web-development-best-practices/_static/image4.png)

![Задать расписания](web-development-best-practices/_static/image5.png)

![Дневное расписание](web-development-best-practices/_static/image6.png)

![Расписание weeknight](web-development-best-practices/_static/image7.png)

![Расписание выходные дни](web-development-best-practices/_static/image8.png)

И Конечно все это можно сделать в сценариях, а также как и на портале.

Возможность масштабировать приложения практически неограниченное в Windows Azure, при условии, что позволяет избежать препятствия на пути к динамически Добавление или удаление сервера виртуальных машин, сохраняя без сохранения состояния веб-уровня.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Избежать состояния сеанса

Часто не имеет смысла в реальных облачного приложения не хранить определенные виды состояния сеанса пользователя, но некоторые подходы влияют на производительность и масштабируемость сильнее, чем другие. Если необходимо хранить состояние, лучше всего сохранить небольшой объем состояния и в файлах cookie. Если это невозможно, следующее оптимальное решение — использовать состояние сеанса ASP.NET с поставщиком для [распределенного кэша в памяти](distributed-caching.md#sessionstate). Наихудшее решение с точки зрения масштабируемости и производительности — использовать базу данных резервные поставщика состояний сеансов.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Использование CDN для кэширования статического файла ресурсов

CDN — сокращение от сети доставки содержимого. Укажите статический файл ресурсов, таких как изображения и файлы скриптов поставщику CDN, а поставщик кэширует эти файлы в центрах обработки данных по всему миру, чтобы везде, где пользователи обращаются к приложению, они получают относительно быстрого ответа и небольшую задержку для кэшированные активы. Это ускоряет общее время загрузки веб-узла и уменьшает нагрузку на веб-серверах. CDN особенно важны, если производится доступ широко распространенный географически аудитории.

Windows Azure имеет CDN, а другие CDN можно использовать в приложении, которое выполняется в Windows Azure или в любой среде веб-размещения.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Используйте Поддержка асинхронного выполнения .NET 4.5, чтобы избежать блокирующих вызовов

.NET 4.5 усиленной языки программирования C# и VB, чтобы сделать ее гораздо проще асинхронная обработка задачи. Преимущества асинхронного программирования не только для параллельной обработки ситуациях, например, если вы хотите одновременно запустить несколько вызовов веб-службы. Она также позволяет более эффективно выполнять на веб-сервере и надежных в условиях высокой нагрузки. Веб-сервер имеет только ограниченное число потоков, доступных и в условиях высокой нагрузки при использовании, все потоки входящих запросов имеют ожидание, пока не освободятся потоков. Если код приложения не обрабатывает задач, таких как базы данных запросы и вызовы веб-службы асинхронно, много потоков без необходимости блокировали сервер ожидает ответа ввода-вывода. Это ограничивает объем трафика, который может обработать сервер в условиях высокой нагрузки. В асинхронном программировании потоков, ожидающих веб-службы или базы данных для возвращения данных освобождаются до обслуживания новых запросов до данных принимается. Занят веб-сервер сотни и тысячи запросов затем может обрабатываться немедленно которых может ожидание потоков можно освободить.

Как показано выше, так же легко сократить количество обработки веб-сайта, как увеличить их веб-серверов. Поэтому если сервер может обеспечить более высокую пропускную способность, нет необходимости как многие из них и позволяет сократить расходы, так как требуется меньшее количество серверов для объем данного трафика, чем в противном случае.

Для веб-форм, MVC и веб-API; в ASP.NET 4.5 включена поддержка асинхронной модели программирования .NET 4.5 в Entity Framework 6, а в [API хранилища Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Поддержка асинхронного выполнения в ASP.NET 4.5

В ASP.NET 4.5 Поддержка асинхронного программирования был добавлен не только для языка, но и для платформы MVC, веб-форм и веб-API. Например метод действия контроллера ASP.NET MVC получает данные из веб-запроса и передает данные в представление, которое затем создает HTML-код, отправляемого в браузер. Часто метод действия должен получить данные из базы данных или веб-службы для их отображения на веб-странице или сохранять данные, вводимые на веб-странице. В этих случаях можно легко сделать асинхронного метода действия: вместо возвращения *ActionResult* возвращаемого вами объекта *задачи&lt;ActionResult&gt;*  и пометьте метод с *async* ключевое слово. Внутри метода Если строка кода запускает операцию, которая включает в себя время ожидания его пометить с помощью ключевого слова await.

Вот простое действие метод, который вызывает метод репозитория для запроса к базе данных.

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

А вот тот же метод, который асинхронно обрабатывает вызов базы данных:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

На самом деле, компилятор создает соответствующие асинхронного кода. Когда приложение отправляет вызов `FindTaskByIdAsync`, ASP.NET делает `FindTask` запроса, а затем освобождает рабочий поток и делает его доступным для обработки нового запроса. Когда `FindTask` выполняется запрос, поток перезапускается для продолжения обработки кода, которое идет после этого вызова. Во время промежутке между `FindTask` отправляет запрос и возвращаются данные после поток для выполнения требуемых задач, которые в противном случае может быть объединено для ответа.

Есть некоторые издержки для асинхронного кода, но в условиях низкой нагрузкой, эти издержки не незначительно, при в условиях высокой нагрузки, вы можете обрабатывать запросы, которые в противном случае будет храниться доступных потоков.

Ее можно сделать асинхронного программирования с ASP.NET версии 1.1, но это был непростым, ошибкам и трудно отлаживать. Теперь, когда мы упростили создание кода для него в ASP.NET 4.5, нет причин больше это сделать.

### <a name="async-support-in-entity-framework-6"></a>Поддержка асинхронного выполнения в Entity Framework 6

Как часть Поддержка асинхронного 4.5 мы выпустили поддержку асинхронных вызовов веб-служб, сокеты и файловой системы ввода-вывода, но чаще всего используется шаблон для веб-приложений нажатия базы данных, и наши данные библиотеки не поддерживает async. Теперь Entity Framework 6 добавляет поддержку async для доступа к базе данных.

В Entity Framework 6, все методы, вызывающие запроса или команды, отправляемые в базу данных имеют асинхронных версий. В данном примере показана версия async *найти* метод.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

И поддержка асинхронного выполнения этой работы не только для операции вставки, удаления, обновления и простой поиск, она также работает с помощью запросов LINQ.

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Отсутствует `Async` версии `ToList` метод так как в этом коде метод, который завершает запрос, отправляемые в базу данных. `Where` И `OrderByDescending` методы только Настройка запроса, пока `ToListAsync` метод выполняет запрос и сохраняет ответ в `result` переменной.

## <a name="summary"></a>Сводка

Можно реализовать web рекомендации по разработке описанную в любой веб-программирования framework и любой облачной среде, но у нас есть средства в ASP.NET и Windows Azure, чтобы упростить процесс. При выполнении этих шаблонов можно легко масштабировать ваш веб-уровня и будет свести к минимуму расходы, поскольку каждый сервер будет обрабатывать больше трафика.

[Следующей главе](single-sign-on.md) рассматривает как облаке позволяет сценариев единого входа.

## <a name="resources"></a>Ресурсы

Дополнительные сведения см. следующие ресурсы.

Веб-серверов:

- [Шаблоны и методики - автоматическое масштабирование рекомендации Майкрософт](https://msdn.microsoft.com/en-us/library/dn589774.aspx).
- [Отключение ARR экземпляра сопоставление в Windows Azure веб-сайтов](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). В блоге по Erez Benari описывает сходство сеансов в Windows Azure Web Sites.

CDN:

- [Отказоустойчивость: Построение масштабируемых, надежных облачные службы](https://channel9.msdn.com/Series/FailSafe). Серия видеоматериалов девять частей, Ульрих Хоманн, Marc Mercuri и Марк Симмс. См. обсуждение CDN в серии 3, начиная с 1:34:00.
- [Корпорация Майкрософт шаблоны и методики статического содержимого размещение шаблон](https://msdn.microsoft.com/en-us/library/dn589776.aspx)
- [Обзоры CDN](http://www.cdnreviews.com/). Общие сведения о многих CDN.

Асинхронное программирование:

- [Использование асинхронных методов в ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Учебник по Рик Андерсон.
- [Асинхронное программирование с использованием Async и Await (C# и Visual Basic)](https://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx). MSDN техническом документе объясняется обоснование для асинхронного программирования, как она работает в ASP.NET 4.5 и написание кода для его реализации.
- [Entity Framework асинхронный запрос и сохранение](https://msdn.microsoft.com/en-us/data/jj819165)
- [Как создать веб-приложений ASP.NET с помощью асинхронного](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Видеопрезентация с Роуэном Миллер. Включает в себя графические демонстрацию асинхронного программирования может упростить значительное увеличение пропускной способности веб сервера в условиях высокой нагрузки.
- [Отказоустойчивость: Построение масштабируемых, надежных облачные службы](https://channel9.msdn.com/Series/FailSafe). Серия видеоматериалов девять частей, Ульрих Хоманн, Marc Mercuri и Марк Симмс. Обсуждения о влиянии асинхронного программирования на масштабируемость серии 4 и 8 серии см.
- [Магическое использования асинхронных методов в ASP.NET 4.5, а также важные бреши](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Запись блога Скотт Хансельман в первую очередь, об использовании асинхронных приложений веб-форм ASP.NET.

Web Дополнительные рекомендации по разработке см. следующие ресурсы:

- [Исправление его образец приложения - рекомендации](the-fix-it-sample-application.md#bestpractices). Приложение, чтобы эта электронная книга приведено несколько рекомендаций, которые были реализованы в приложении исправить.
- [Контрольный список Web Developer](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[Назад](continuous-integration-and-continuous-delivery.md)
[Вперед](single-sign-on.md)
