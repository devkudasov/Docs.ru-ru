---
uid: signalr/overview/older-versions/signalr-performance
title: "SignalR производительности (SignalR 1.x) | Документы Microsoft"
author: pfletcher
description: "SignalR производительности"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 52052ad202958eb5d648ceb64d9f06fb86ef3777
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance-signalr-1x"></a>SignalR производительности (SignalR 1.x)
====================
по [Патрик Флетчера](https://github.com/pfletcher)

> Описывается, как разрабатывать приложения для измерения и повысить производительность приложения SignalR.


Недавней презентации на SignalR производительности и масштабируемости, в разделе [масштабирование в Интернете в режиме реального времени с помощью ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

В этом разделе содержатся следующие подразделы.

- [Вопросы проектирования](#design)
- [Настройки сервера SignalR для повышения производительности](#tuning)
- [Диагностика проблем производительности](#troubleshooting)
- [С помощью счетчиков производительности SignalR](#perfcounters)
- [Использование других счетчиков производительности](#othercounters)
- [Другие ресурсы](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Вопросы проектирования

В этом разделе описываются шаблоны, которые могут быть применены во время разработки приложения SignalR, чтобы убедиться, что не выполняется ухудшить производительность путем создания ненужных сетевого трафика.

### <a name="throttling-message-frequency"></a>Частота запросов сообщений

Даже в приложении, которое отправляет сообщения с высокой частотой (например, приложения, игры в реальном времени) большинство приложений не требуется отправить несколько сообщений в секунду. Чтобы уменьшить объем трафика, создаваемый каждым клиентом, цикл обработки сообщений может быть реализован, очередей и отправки сообщений не более часто фиксированной ставке (то есть до определенного числа сообщений будут отправляться каждую секунду, если сообщения в это время в terval отправки). Образец приложения, регулирует сообщения для определенных коэффициент (от сервера и клиента), в разделе [высокой частотой в реальном времени с SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Уменьшение размера сообщения

Можно уменьшить размер сообщения SignalR, уменьшив размер сериализованных объектов. В серверном коде при отправке объект, содержащий свойства, которые не должны передаваться предотвратить эти свойства сериализацию с помощью `JsonIgnore` атрибута. Имена свойств, также сохраняются в сообщении; имена свойств может быть сокращено с помощью `JsonProperty` атрибута. В следующем образце кода показано, как исключить свойство отправку клиенту и сокращение имена свойств:

**Код сервера .NET, демонстрирующее атрибут JsonIgnore запрет данных, отправляемых клиенту, а атрибут JsonProperty, чтобы уменьшить размер сообщения**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Чтобы сохранить удобочитаемость и удобства поддержки в клиентском коде сокращенное свойство имена могут быть имена, преобразованное в понятную после получения сообщения. В следующем образце кода демонстрируется один из возможных способов переназначение сокращенные имена больше из них, путем определения контракта сообщения (сопоставление) и с помощью `reMap` функции для применения к класса оптимизированного сообщений контракта:

**Клиентский код JavaScript, который указывает, что сокращено имена свойств для восприятия имена**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Имена может быть сокращено в сообщениях от клиента к серверу, используя тот же метод.

Объем памяти (объем памяти, используемый для сообщения) сообщения объект также может повысить производительность. Например если полный спектр `int` не является обязательным, `short` или `byte` можно использовать вместо него.

Поскольку сообщения хранятся в канале сообщений в памяти сервера, уменьшение размера сообщения также могут устранения неполадок памяти сервера.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Настройки сервера SignalR для повышения производительности

Можно использовать следующие параметры конфигурации для настройки сервера для повышения производительности в приложении SignalR. Общие сведения о том, как повысить производительность приложений ASP.NET см. в разделе [повышение производительности ASP.NET](https://msdn.microsoft.com/en-us/library/ff647787.aspx).

**Параметры конфигурации SignalR**

- **DefaultMessageBufferSize**: по умолчанию SignalR сохраняет 1000 сообщений в памяти на концентраторе на один сеанс. Если используются большие сообщения, это может создать проблемы с памятью, которые могут быть облегчено уменьшая это значение. Этот параметр можно задать в `Application_Start` обработчик событий в приложении ASP.NET или `Configuration` метод класса запуска OWIN в резидентного приложения. В следующем образце показано, как уменьшить это значение, чтобы сократить использование памяти приложения, чтобы сократить объем используемой памяти сервера:

    **Код сервера .NET в файле Global.asax для уменьшения размера буфера сообщения по умолчанию**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Параметры конфигурации IIS**

- **Максимальное количество параллельных запросов приложения**: увеличение числа одновременных IIS запросов будет увеличить ресурсы сервера, доступных для обслуживания запросов. Значение по умолчанию — 5000; Чтобы увеличить этот параметр, выполните следующие команды в командную строку с повышенными привилегиями:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Параметры конфигурации ASP.NET**

Этот раздел содержит параметры конфигурации, которые могут быть установлены в `aspnet.config` файла. Этот файл находится в одном из двух мест, в зависимости от платформы:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Следующие параметры ASP.NET, которые могут повысить производительность SignalR.

- **Максимальное число параллельных запросов на один ЦП**: увеличение значения этого параметра может устранить узкие места по производительности. Чтобы увеличить этот параметр, добавьте следующий параметр конфигурации в `aspnet.config` файла:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Предел очереди запросов**: Если превышает общее число подключений `maxConcurrentRequestsPerCPU` параметр ASP.NET запустится регулирование запросов с использованием очереди. Чтобы увеличить размер очереди, можно увеличить `requestQueueLimit` параметр. Чтобы сделать это, добавьте следующий параметр конфигурации в `processModel` узел в `config/machine.config` (а не `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Диагностика проблем производительности

Этот раздел описывает способы поиска узких мест производительности в приложении.

### <a name="verifying-that-websocket-is-being-used"></a>Проверка того, что используется WebSocket

SignalR можно использовать различные транспорты для обмена данными между клиентом и сервером, WebSocket предлагает значительное преимущество в производительности и следует использовать, если клиент и сервер поддерживают его. Чтобы определить, если клиент и сервер соответствует требованиям для WebSocket, в разделе [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports). Чтобы определить, какой транспорт используется в приложении, можно использовать инструменты разработчика браузера и изучите журналы, чтобы увидеть, какой транспорт используется для соединения. Сведения об использовании средства разработки браузера в Internet Explorer и Chrome см. в разделе [транспортов и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>С помощью счетчиков производительности SignalR

В этом разделе описывается включение и использование счетчиков производительности SignalR, найден в `Microsoft.AspNet.SignalR.Utils` пакета.

### <a name="installing-signalrexe"></a>Установка signalr.exe

Счетчики производительности можно добавить на сервер, с помощью служебной программы, называется SignalR.exe. Чтобы установить эту программу, выполните следующие действия.

1. В приложении Visual Studio выберите **средства**, **диспетчер пакетов библиотеки**, **управление пакетами NuGet для решения...**
2. Поиск **signalr.utils**и выберите установку.

    ![](signalr-performance/_static/image1.png)
3. Примите лицензионное соглашение для установки пакета.
4. Будет установлен для SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Установка счетчиков производительности с SignalR.exe

Чтобы установить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующими параметрами:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Чтобы удалить счетчиков производительности SignalR, запустите SignalR.exe в командную строку со следующими параметрами:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Счетчики производительности SignalR

Служебные программы пакет устанавливает следующие счетчики производительности. Счетчики «Общее» измеряет число событий с момента последнего пул приложений или сервера перезапуска.

**Метрики подключения**

Следующие метрики измерения возникающих событий время существования подключения. Дополнительные сведения см. в разделе [понимание и обработка события времени жизни соединений](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Подключений**
- **Переподключить подключений**
- **Disonnected подключения**
- **Текущих подключений**

**Метрики сообщения**

Следующие метрики измерения трафика сообщения, сформированного SignalR.

- **Всего получено сообщений подключения**
- **Всего отправлено сообщений подключения**
- **Подключение сообщений, полученных в секунду**
- **Подключение отправленных сообщений в секунду**

**Метрики шины сообщений**

Следующие метрики измерения трафика через внутренний сообщений SignalR, очереди, в которой размещаются всех входящих и исходящих сообщений SignalR. Сообщение **опубликовано** при его отправке или широковещательных пакетов. Объект **подписчика** в этом контексте подписка на канал сообщений; это должно равняться количеству клиентов, а также сам сервер. **Выделенных рабочих** — это компонент, который отправляет данные для активных подключений; **занятых рабочих** , активно отправляет сообщение.

- **Канал сообщений сообщений в очереди полученных**
- **Канал сообщений сообщений, полученных в секунду**
- **Всего опубликованных Message шины сообщений**
- **Канал сообщений сообщений, опубликованных в секунду**
- **Текущий подписчиков шины сообщений**
- **Всего подписчиков шины сообщений**
- **Подписчики шины сообщений в секунду**
- **Канал сообщений выделенных рабочих процессов**
- **Сообщение шины рабочих процессов**
- **Текущий разделы шины сообщений**

**Ошибка метрик**

Следующие метрики измерения ошибок, создаваемых трафиком сообщений SignalR. **Разрешение концентратора** ошибки возникают, когда не удается разрешить концентратора или метода концентратора. **Вызов концентратора** ошибки, исключения, возникшие во время вызова метода концентратора. **Транспорт** ошибки являются соединения ошибок, возникающих во время HTTP-запроса или ответа.

- **Ошибок: Общее число всех**
- **Ошибки: All/сек**
- **Ошибок: Общее число концентратора разрешения**
- **Ошибок: Разрешение концентратора в секунду**
- **Ошибок: Общее число вызовов концентратора**
- **Ошибки: Концентратора вызовов/сек**
- **Ошибок: Общее число транспорта**
- **Ошибки: Транспорта в секунду**

**Метрики горизонтального масштабирования**

Следующие метрики измерения трафика и ошибки, созданные поставщиком горизонтального масштабирования. Объект **поток** в этом контексте единица масштабирования, используемый поставщиком горизонтального масштабирования; это действие таблицы, если используется SQL Server, раздел, если используется Service Bus и подписки, если используется Redis. По умолчанию используется только один поток, но его можно увеличить через конфигурацию на SQL Server и Service Bus. Объект **буферизация** поток является тот, который вошел в состоянии сбоя, когда поток находится в состоянии сбоя, все сообщения, отправляемые на объединительную плату не будет работать сразу, пока поток больше не произошла ошибка. **Длина очереди отправки** — количество сообщений, которые разнесены, но еще не отправлены.

- **Канал сообщений горизонтального масштабирования сообщений, полученных в секунду**
- **Всего потоков горизонтального масштабирования**
- **Откройте потоки горизонтального масштабирования**
- **Буферизация потоков горизонтального масштабирования**
- **Общее число ошибок горизонтального масштабирования**
- **Ошибок горизонтального масштабирования в секунду**
- **Длина очереди отправки горизонтального масштабирования**

Дополнительные сведения об измерении этих счетчиков см. в разделе [SignalR горизонтального масштабирования с Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Использование других счетчиков производительности

Следующие счетчики производительности также можно использовать для наблюдения за производительностью приложения.

**Память**

- .NET CLR память # байтов во всех кучах (для w3wp)

**ASP.NET**

- ASP.net\текущих запросов
- ASP.NET\Queued
- ASP.NET\Rejected

**ЦП**

- Information\Processor загруженности процессора

**TCP/IP**

- TCP версии 6 и подключений
- TCP версии 4 подключений

**Веб-службы**

- Веб-служба\текущих подключений
- Веб-Service\Maximum подключений

**Работа с потоками**

- Потоки CLR .NET\# текущих логических потоков
- .NET CLR LocksAnd потоков\# текущих физических потоков

<a id="otherresources"></a>

## <a name="other-resources"></a>Другие ресурсы

Дополнительные сведения о производительности ASP.NET, контроля и настройки см. в следующих разделах:

- [Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)
- [Использование потоков ASP.NET в IIS 7.5, IIS 7.0 и IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;пул приложений&gt; элемент (веб-параметров)](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
