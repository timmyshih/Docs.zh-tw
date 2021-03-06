---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273777"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>在 ASP.NET Core 中使用託管服務的背景工作

作者：[Luke Latham](https://github.com/guardrex)

在 ASP.NET Core 中，背景工作可實作為「託管服務」。 託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 啟動範圍服務的託管服務。 範圍服務可以使用相依性插入。
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

範例應用程式有兩個版本：

* Web 主機：Web 主機對於裝載 Web 應用程式非常有用。 本主題中顯示的範例程式碼是來自 Web 主機版本的範例。 如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。
* 泛型主機：泛型主機是 ASP.NET Core 2.1 的新功能。 如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。

::: moniker-end

## <a name="ihostedservice-interface"></a>IHostedService 介面

託管服務會實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。 此介面針對主機所管理的物件定義兩種方法：

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - 在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) 之後呼叫。 `StartAsync` 包含用來啟動背景工作的邏輯。

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - 當主機執行正常關機程序時觸發。 `StopAsync` 包含用來結束背景工作和處置任何受控資源的邏輯。 如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。

託管服務會在應用程式啟動時啟動一次，然後應用程式關閉時正常關閉。 若實作 [IDisposable](/dotnet/api/system.idisposable)，就可以在處置服務容器時處置資源。 如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](/dotnet/api/system.threading.timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

服務會在 `Startup.ConfigureServices` 中註冊：

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

若要在 `IHostedService` 內使用範圍服務，請建立一個範圍。 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在下列範例中，[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) 會插入至服務：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

這些服務會在 `Startup.ConfigureServices` 中註冊：

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

## <a name="queued-background-tasks"></a>排入佇列背景工作

背景工作佇列是以 .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([暫時排定為針對 ASP.NET Core 3.0 內建](https://github.com/aspnet/Hosting/issues/1280)) 為基礎：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

在 `QueueHostedService` 中，佇列中的背景工作 (`workItem`) 會從佇列清除並執行：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

這些服務會在 `Startup.ConfigureServices` 中註冊：

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

在索引頁面模型類別中，`IBackgroundTaskQueue` 已插入至建構函式，並指派給 `Queue`：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。 將會呼叫 `QueueBackgroundWorkItem` 以從佇列清除工作項目：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>其他資源

* [在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
