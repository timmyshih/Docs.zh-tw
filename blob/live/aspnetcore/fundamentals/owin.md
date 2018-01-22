---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: "探索 ASP.NET Core 支援的方式開啟 Web 介面的.NET (OWIN)，可讓 web 應用程式来從 web 伺服器。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e819037e2ebd1566c778879516e20de8dc7603ea
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="c7112-103">若要開啟.NET (OWIN) 的網頁介面的簡介</span><span class="sxs-lookup"><span data-stu-id="c7112-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="c7112-104">由[Steve Smith](https://ardalis.com/)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c7112-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c7112-105">ASP.NET Core 支援Open Web Interface for .NET (OWIN)。</span><span class="sxs-lookup"><span data-stu-id="c7112-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="c7112-106">OWIN 可讓 Web 應用程式獨立於網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7112-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="c7112-107">它會定義可以在管線中用來處理要求和回應相關聯的中介軟體的標準方式。</span><span class="sxs-lookup"><span data-stu-id="c7112-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="c7112-108">與 OWIN 應用程式、 伺服器和中介軟體的 ASP.NET Core 應用程式和中介軟體可以交互操作。</span><span class="sxs-lookup"><span data-stu-id="c7112-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="c7112-109">OWIN 提供脫鉤層，可讓兩個架構來使用不同的物件模型一起使用。</span><span class="sxs-lookup"><span data-stu-id="c7112-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="c7112-110">`Microsoft.AspNetCore.Owin`套件提供兩個配接器實作：</span><span class="sxs-lookup"><span data-stu-id="c7112-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="c7112-111">Owin ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7112-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="c7112-112">ASP.NET Core 的 OWIN</span><span class="sxs-lookup"><span data-stu-id="c7112-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="c7112-113">這可讓 ASP.NET Core 之上 OWIN 相容伺服器/主控件，或執行 ASP.NET Core 之上其他 OWIN 相容元件的裝載。</span><span class="sxs-lookup"><span data-stu-id="c7112-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="c7112-114">注意： 使用這些介面卡隨附的效能成本。</span><span class="sxs-lookup"><span data-stu-id="c7112-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="c7112-115">使用僅 ASP.NET 核心元件的應用程式不應該使用 Owin 封裝或介面卡。</span><span class="sxs-lookup"><span data-stu-id="c7112-115">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="c7112-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7112-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="c7112-117">在 ASP.NET 管線中執行 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="c7112-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="c7112-118">ASP.NET Core OWIN 支援部署為一部分`Microsoft.AspNetCore.Owin`封裝。</span><span class="sxs-lookup"><span data-stu-id="c7112-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="c7112-119">您可以匯入您的專案的 OWIN 支援安裝此套件。</span><span class="sxs-lookup"><span data-stu-id="c7112-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="c7112-120">OWIN 中介軟體符合[OWIN 規格](http://owin.org/spec/spec/owin-1.0.0.html)，這項工作需要`Func<IDictionary<string, object>, Task>`設定介面和特定的索引鍵 (例如`owin.ResponseBody`)。</span><span class="sxs-lookup"><span data-stu-id="c7112-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="c7112-121">下列簡單的 OWIN 中介軟體會顯示"Hello World":</span><span class="sxs-lookup"><span data-stu-id="c7112-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="c7112-122">範例簽章傳回`Task`並接受`IDictionary<string, object>`依 OWIN。</span><span class="sxs-lookup"><span data-stu-id="c7112-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="c7112-123">下列程式碼示範如何加入`OwinHello`（如上所示） 具有的 ASP.NET 管線中介軟體`UseOwin`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="c7112-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="c7112-124">您可以設定 OWIN 管線中進行其他動作。</span><span class="sxs-lookup"><span data-stu-id="c7112-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="c7112-125">僅能在第一次寫入至回應資料流之前修改回應標頭。</span><span class="sxs-lookup"><span data-stu-id="c7112-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="c7112-126">多個呼叫`UseOwin`建議您不要使用基於效能的考量。</span><span class="sxs-lookup"><span data-stu-id="c7112-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="c7112-127">如果群組在一起，OWIN 元件會以最能運作。</span><span class="sxs-lookup"><span data-stu-id="c7112-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="c7112-128">使用 OWIN 架構的伺服器上的 ASP.NET 裝載</span><span class="sxs-lookup"><span data-stu-id="c7112-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="c7112-129">OWIN 伺服器可以裝載 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7112-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="c7112-130">這類伺服器[Nowin](https://github.com/Bobris/Nowin)，.NET OWIN web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="c7112-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="c7112-131">在本文範例中，包括了參考 Nowin 並使用它來建立的專案`IServer`能夠自我裝載的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="c7112-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="c7112-132">`IServer`是需要的介面`Features`屬性和`Start`方法。</span><span class="sxs-lookup"><span data-stu-id="c7112-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="c7112-133">`Start`會負責設定和啟動伺服器，在此情況下透過一系列 fluent 應用程式開發的應用程式開發介面呼叫從 IServerAddressesFeature 設定剖析的位址。</span><span class="sxs-lookup"><span data-stu-id="c7112-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="c7112-134">請注意，fluent 應用程式開發的設定`_builder`變數會指定要求交由`appFunc`稍早在方法中定義。</span><span class="sxs-lookup"><span data-stu-id="c7112-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="c7112-135">這`Func`呼叫的每個要求來處理傳入的要求。</span><span class="sxs-lookup"><span data-stu-id="c7112-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="c7112-136">我們也可以加入`IWebHostBuilder`輕鬆加入及設定 Nowin 伺服器的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="c7112-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="c7112-137">這個位置，所有的要求執行 ASP.NET 應用程式擴充功能呼叫中使用此自訂伺服器*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7112-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="c7112-138">深入了解 ASP.NET[伺服器](servers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="c7112-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="c7112-139">OWIN 架構的伺服器上執行 ASP.NET Core，並使用 WebSockets 的支援</span><span class="sxs-lookup"><span data-stu-id="c7112-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="c7112-140">如何 OWIN 架構伺服器功能的另一個範例可以利用 ASP.NET Core 是 WebSockets 等功能的存取。</span><span class="sxs-lookup"><span data-stu-id="c7112-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="c7112-141">在上述範例中使用.NET OWIN web 伺服器的 Web 通訊端中，建立以 ASP.NET Core 應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="c7112-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="c7112-142">下列範例顯示簡單的 web 應用程式支援 Web 通訊端及回應透過 Websocket 傳送的所有項目。</span><span class="sxs-lookup"><span data-stu-id="c7112-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="c7112-143">這[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)已設定為使用相同`NowinServer`唯一的差別是應用程式中的設定方式與上一個為其`Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="c7112-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="c7112-144">使用測試[簡單 websocket 用戶端](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)示範應用程式：</span><span class="sxs-lookup"><span data-stu-id="c7112-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web 通訊端測試用戶端](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="c7112-146">OWIN 環境</span><span class="sxs-lookup"><span data-stu-id="c7112-146">OWIN environment</span></span>

<span data-ttu-id="c7112-147">您可以建構的 OWIN 環境 using `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="c7112-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="c7112-148">OWIN 索引鍵</span><span class="sxs-lookup"><span data-stu-id="c7112-148">OWIN keys</span></span>

<span data-ttu-id="c7112-149">OWIN 取決於`IDictionary<string,object>`通訊內的 HTTP 要求/回應交換資訊的物件。</span><span class="sxs-lookup"><span data-stu-id="c7112-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="c7112-150">ASP.NET Core 實作下面所列的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="c7112-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="c7112-151">請參閱[規格的主要、 延伸](http://owin.org/#spec)，和[OWIN 索引鍵指導方針和一般索引鍵](http://owin.org/spec/spec/CommonKeys.html)。</span><span class="sxs-lookup"><span data-stu-id="c7112-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="c7112-152">要求資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="c7112-152">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="c7112-153">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-153">Key</span></span>               | <span data-ttu-id="c7112-154">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-154">Value (type)</span></span> | <span data-ttu-id="c7112-155">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="c7112-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="c7112-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="c7112-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="c7112-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="c7112-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="c7112-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="c7112-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="c7112-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="c7112-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="c7112-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="c7112-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="c7112-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="c7112-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="c7112-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="c7112-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="c7112-164">要求資料 (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="c7112-164">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="c7112-165">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-165">Key</span></span>               | <span data-ttu-id="c7112-166">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-166">Value (type)</span></span> | <span data-ttu-id="c7112-167">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="c7112-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="c7112-169">Optional</span><span class="sxs-lookup"><span data-stu-id="c7112-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="c7112-170">回應資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="c7112-170">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="c7112-171">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-171">Key</span></span>               | <span data-ttu-id="c7112-172">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-172">Value (type)</span></span> | <span data-ttu-id="c7112-173">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="c7112-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="c7112-175">Optional</span><span class="sxs-lookup"><span data-stu-id="c7112-175">Optional</span></span> |
| <span data-ttu-id="c7112-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="c7112-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="c7112-177">Optional</span><span class="sxs-lookup"><span data-stu-id="c7112-177">Optional</span></span> |
| <span data-ttu-id="c7112-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="c7112-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="c7112-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="c7112-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="c7112-180">其他資料 (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="c7112-180">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="c7112-181">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-181">Key</span></span>               | <span data-ttu-id="c7112-182">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-182">Value (type)</span></span> | <span data-ttu-id="c7112-183">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="c7112-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="c7112-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="c7112-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="c7112-186">一般索引鍵</span><span class="sxs-lookup"><span data-stu-id="c7112-186">Common Keys</span></span>

| <span data-ttu-id="c7112-187">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-187">Key</span></span>               | <span data-ttu-id="c7112-188">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-188">Value (type)</span></span> | <span data-ttu-id="c7112-189">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="c7112-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="c7112-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="c7112-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="c7112-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="c7112-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="c7112-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="c7112-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="c7112-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="c7112-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="c7112-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="c7112-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="c7112-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="c7112-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="c7112-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="c7112-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="c7112-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="c7112-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="c7112-199">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-199">Key</span></span>               | <span data-ttu-id="c7112-200">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-200">Value (type)</span></span> | <span data-ttu-id="c7112-201">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="c7112-202">sendfile.SendAsync</span></span> | <span data-ttu-id="c7112-203">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="c7112-204">每個要求</span><span class="sxs-lookup"><span data-stu-id="c7112-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="c7112-205">不透明 v0.3.0</span><span class="sxs-lookup"><span data-stu-id="c7112-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="c7112-206">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-206">Key</span></span>               | <span data-ttu-id="c7112-207">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-207">Value (type)</span></span> | <span data-ttu-id="c7112-208">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="c7112-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="c7112-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="c7112-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="c7112-211">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="c7112-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="c7112-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="c7112-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="c7112-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="c7112-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="c7112-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="c7112-215">Key</span><span class="sxs-lookup"><span data-stu-id="c7112-215">Key</span></span>               | <span data-ttu-id="c7112-216">值 （類型）</span><span class="sxs-lookup"><span data-stu-id="c7112-216">Value (type)</span></span> | <span data-ttu-id="c7112-217">描述</span><span class="sxs-lookup"><span data-stu-id="c7112-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="c7112-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="c7112-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="c7112-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="c7112-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="c7112-220">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="c7112-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="c7112-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="c7112-222">非規格</span><span class="sxs-lookup"><span data-stu-id="c7112-222">Non-spec</span></span> |
| <span data-ttu-id="c7112-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="c7112-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="c7112-224">請參閱[RFC6455 第 4.2.2 節](https://tools.ietf.org/html/rfc6455#section-4.2.2)步驟 5.5</span><span class="sxs-lookup"><span data-stu-id="c7112-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="c7112-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="c7112-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="c7112-226">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="c7112-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="c7112-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="c7112-228">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="c7112-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="c7112-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="c7112-230">請參閱[委派簽章](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="c7112-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="c7112-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="c7112-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="c7112-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="c7112-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="c7112-233">Optional</span><span class="sxs-lookup"><span data-stu-id="c7112-233">Optional</span></span> |
| <span data-ttu-id="c7112-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="c7112-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="c7112-235">Optional</span><span class="sxs-lookup"><span data-stu-id="c7112-235">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="c7112-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="c7112-236">Additional Resources</span></span>

* [<span data-ttu-id="c7112-237">中介軟體</span><span class="sxs-lookup"><span data-stu-id="c7112-237">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="c7112-238">伺服器</span><span class="sxs-lookup"><span data-stu-id="c7112-238">Servers</span></span>](servers/index.md)