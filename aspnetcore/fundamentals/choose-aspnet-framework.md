---
title: 在 ASP.NET 和 ASP.NET Core 之間進行選擇
author: rick-anderson
description: 了解如何在 ASP.NET 和 ASP.NET Core 之間進行選擇。
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297227"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="b5922-103">在 ASP.NET 和 ASP.NET Core 之間進行選擇</span><span class="sxs-lookup"><span data-stu-id="b5922-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="b5922-104">不論您要建立哪種 Web 應用程式，ASP.NET 都為您提供解決方案：從以 Windows Server 為目標的企業 Web 應用程式，到以 Linux 容器為目標的小型微服務，以及這兩者之間的所有項目。</span><span class="sxs-lookup"><span data-stu-id="b5922-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="b5922-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5922-105">ASP.NET Core</span></span>

<span data-ttu-id="b5922-106">ASP.NET Core 是一種開放原始碼、跨平台的架構，用於在 Windows、macOS 或 Linux 上建置現代化的雲端式 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5922-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="b5922-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b5922-107">ASP.NET</span></span>

<span data-ttu-id="b5922-108">ASP.NET 是一種成熟的架構，其提供建置企業級伺服器端 Web 應用程式所需的所有服務。</span><span class="sxs-lookup"><span data-stu-id="b5922-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="b5922-109">選取 Framework</span><span class="sxs-lookup"><span data-stu-id="b5922-109">Framework selection</span></span>

<span data-ttu-id="b5922-110">請檢閱下表來判斷哪一個架構最符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="b5922-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="b5922-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5922-111">ASP.NET Core</span></span> | <span data-ttu-id="b5922-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b5922-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="b5922-113">為 Windows、macOS 或 Linux 建置</span><span class="sxs-lookup"><span data-stu-id="b5922-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="b5922-114">為 Windows 建置</span><span class="sxs-lookup"><span data-stu-id="b5922-114">Build for Windows</span></span>|
|<span data-ttu-id="b5922-115">從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="b5922-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="b5922-116">另請參閱 [MVC](xref:mvc/overview)、[Web API](xref:tutorials/first-web-api) 和 [SignalR](xref:signalr/introduction)。</span><span class="sxs-lookup"><span data-stu-id="b5922-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="b5922-117">使用 [Web Forms](/aspnet/web-forms)、[SignalR](/aspnet/signalr)、[MVC](/aspnet/mvc)、[Web API](/aspnet/web-api/)、[WebHooks](/aspnet/webhooks/) 或 [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="b5922-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="b5922-118">每部電腦多個版本</span><span class="sxs-lookup"><span data-stu-id="b5922-118">Multiple versions per machine</span></span>|<span data-ttu-id="b5922-119">每部電腦一個版本</span><span class="sxs-lookup"><span data-stu-id="b5922-119">One version per machine</span></span>|
|<span data-ttu-id="b5922-120">在 Visual Studio、[Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) 或 [Visual Studio Code](https://code.visualstudio.com/) 中使用 C# 或 F# 進行開發</span><span class="sxs-lookup"><span data-stu-id="b5922-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="b5922-121">在 Visual Studio 中使用 C#、VB 或 F# 進行開發</span><span class="sxs-lookup"><span data-stu-id="b5922-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="b5922-122">效能比 ASP.NET 更高</span><span class="sxs-lookup"><span data-stu-id="b5922-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="b5922-123">效能良好</span><span class="sxs-lookup"><span data-stu-id="b5922-123">Good performance</span></span>|
|[<span data-ttu-id="b5922-124">選擇 .NET Framework 或.NET Core 執行階段</span><span class="sxs-lookup"><span data-stu-id="b5922-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="b5922-125">使用 .NET Framework 執行階段</span><span class="sxs-lookup"><span data-stu-id="b5922-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="b5922-126">ASP.NET Core 案例</span><span class="sxs-lookup"><span data-stu-id="b5922-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="b5922-127">從 ASP.NET Core 2.x 開始，[Razor 頁面](xref:razor-pages/index)是建立 Web UI 的建議方法。</span><span class="sxs-lookup"><span data-stu-id="b5922-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="b5922-128">網站</span><span class="sxs-lookup"><span data-stu-id="b5922-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="b5922-129">API</span><span class="sxs-lookup"><span data-stu-id="b5922-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="b5922-130">即時</span><span class="sxs-lookup"><span data-stu-id="b5922-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="b5922-131">ASP.NET 案例</span><span class="sxs-lookup"><span data-stu-id="b5922-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="b5922-132">網站</span><span class="sxs-lookup"><span data-stu-id="b5922-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="b5922-133">API</span><span class="sxs-lookup"><span data-stu-id="b5922-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="b5922-134">即時</span><span class="sxs-lookup"><span data-stu-id="b5922-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="b5922-135">資源</span><span class="sxs-lookup"><span data-stu-id="b5922-135">Resources</span></span>

* [<span data-ttu-id="b5922-136">ASP.NET 簡介</span><span class="sxs-lookup"><span data-stu-id="b5922-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="b5922-137">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="b5922-137">Introduction to ASP.NET Core</span></span>](xref:index)