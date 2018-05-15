---
title: ASP.NET Core 的部分標記協助程式
author: scottaddie
description: 了解 ASP.NET Core 部分標記協助程式和其每個屬性在呈現部分檢視時所扮演的角色。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 670663b963f4207da793afff44d55b85ba58b7f8
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="af4fc-103">ASP.NET Core 的部分標記協助程式</span><span class="sxs-lookup"><span data-stu-id="af4fc-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="af4fc-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="af4fc-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="af4fc-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af4fc-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="af4fc-106">總覽</span><span class="sxs-lookup"><span data-stu-id="af4fc-106">Overview</span></span>

<span data-ttu-id="af4fc-107">部分標記協助程式用於在 Razor 頁面和 MVC 應用程式中呈現[部分檢視](xref:mvc/views/partial)。</span><span class="sxs-lookup"><span data-stu-id="af4fc-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="af4fc-108">請考慮它：</span><span class="sxs-lookup"><span data-stu-id="af4fc-108">Consider that it:</span></span>

* <span data-ttu-id="af4fc-109">需要 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="af4fc-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="af4fc-110">是 [HTML 協助程式語法](xref:mvc/views/partial#referencing-a-partial-view)的替代方法。</span><span class="sxs-lookup"><span data-stu-id="af4fc-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="af4fc-111">以非同步方式呈現部分檢視。</span><span class="sxs-lookup"><span data-stu-id="af4fc-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="af4fc-112">呈現部分檢視的 HTML 協助程式選項包括：</span><span class="sxs-lookup"><span data-stu-id="af4fc-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="af4fc-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="af4fc-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="af4fc-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="af4fc-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="af4fc-115">這整份文件的範例皆使用 *Product* 模型：</span><span class="sxs-lookup"><span data-stu-id="af4fc-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="af4fc-116">部分標記協助程式屬性的清查如下。</span><span class="sxs-lookup"><span data-stu-id="af4fc-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="af4fc-117">name</span><span class="sxs-lookup"><span data-stu-id="af4fc-117">name</span></span>

<span data-ttu-id="af4fc-118">`name` 屬性 (Attribute) 是必要項。</span><span class="sxs-lookup"><span data-stu-id="af4fc-118">The `name` attribute is required.</span></span> <span data-ttu-id="af4fc-119">它會指出要呈現之部分檢視的名稱或路徑。</span><span class="sxs-lookup"><span data-stu-id="af4fc-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="af4fc-120">當提供部分檢視名稱時，就會起始[檢視探索](xref:mvc/views/overview#view-discovery)程序。</span><span class="sxs-lookup"><span data-stu-id="af4fc-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="af4fc-121">提供明確的路徑時，則會略過該程序。</span><span class="sxs-lookup"><span data-stu-id="af4fc-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="af4fc-122">下列標記會使用明確的路徑，指出將從 *Shared* 資料夾載入 *_ProductPartial.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="af4fc-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="af4fc-123">使用 [for](#for) 屬性，模型就會傳遞到部分檢視以進行繫結。</span><span class="sxs-lookup"><span data-stu-id="af4fc-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="af4fc-124">for</span><span class="sxs-lookup"><span data-stu-id="af4fc-124">for</span></span>

<span data-ttu-id="af4fc-125">`for` 屬性會針對目前的模型指派一個要評估的 [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression)。</span><span class="sxs-lookup"><span data-stu-id="af4fc-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="af4fc-126">`ModelExpression` 可推斷 `@Model.` 語法。</span><span class="sxs-lookup"><span data-stu-id="af4fc-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="af4fc-127">例如，可以使用 `for="Product"`，而不是 `for="@Model.Product"`。</span><span class="sxs-lookup"><span data-stu-id="af4fc-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="af4fc-128">使用 `@` 符號來定義內嵌運算式會覆寫這個預設的推斷行為。</span><span class="sxs-lookup"><span data-stu-id="af4fc-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="af4fc-129">`for` 屬性不能與 [model](#model) 屬性一起使用。</span><span class="sxs-lookup"><span data-stu-id="af4fc-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="af4fc-130">下列標記將載入 *_ProductPartial.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="af4fc-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="af4fc-131">部分檢視會繫結至相關聯的頁面模型 `Product` 屬性：</span><span class="sxs-lookup"><span data-stu-id="af4fc-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="af4fc-132">model</span><span class="sxs-lookup"><span data-stu-id="af4fc-132">model</span></span>

<span data-ttu-id="af4fc-133">`model`　屬性指派要傳遞至部分檢視的模型執行個體。</span><span class="sxs-lookup"><span data-stu-id="af4fc-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="af4fc-134">`model` 屬性不能與 [for](#for) 屬性一起使用。</span><span class="sxs-lookup"><span data-stu-id="af4fc-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="af4fc-135">在下列標記中，新的 `Product` 物件將具現化，並傳遞給 `model` 屬性以進行繫結：</span><span class="sxs-lookup"><span data-stu-id="af4fc-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="af4fc-136">view-data</span><span class="sxs-lookup"><span data-stu-id="af4fc-136">view-data</span></span>

<span data-ttu-id="af4fc-137">`view-data`　屬性指派要傳遞至部分檢視的 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)。</span><span class="sxs-lookup"><span data-stu-id="af4fc-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="af4fc-138">下列標記會讓整個 ViewData 集合可存取部分檢視：</span><span class="sxs-lookup"><span data-stu-id="af4fc-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="af4fc-139">在上述程式碼中，`IsNumberReadOnly` 索引鍵值會設定為 `true`，並新增至 ViewData 集合。</span><span class="sxs-lookup"><span data-stu-id="af4fc-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="af4fc-140">因此，`ViewData["IsNumberReadOnly"]` 可以在下列部分檢視內進行存取：</span><span class="sxs-lookup"><span data-stu-id="af4fc-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="af4fc-141">在此範例中，`ViewData["IsNumberReadOnly"]` 的值決定 *Number* 欄位是否會顯示為唯讀。</span><span class="sxs-lookup"><span data-stu-id="af4fc-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af4fc-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="af4fc-142">Additional resources</span></span>

* [<span data-ttu-id="af4fc-143">部分檢視</span><span class="sxs-lookup"><span data-stu-id="af4fc-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="af4fc-144">弱型別資料 (ViewData 和 ViewBag)</span><span class="sxs-lookup"><span data-stu-id="af4fc-144">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)