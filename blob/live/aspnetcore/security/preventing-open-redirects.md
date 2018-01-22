---
title: "防止 ASP.NET Core 應用程式中開啟的重新導向攻擊 |Microsoft 文件"
author: ardalis
description: "示範如何防止對 ASP.NET Core 應用程式的開啟重新導向攻擊"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: e57ae429e9af54ade74485361ba591cb75c16752
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="42d9e-103">防止 ASP.NET Core 應用程式中開啟的重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="42d9e-103">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="42d9e-104">重新導向至指定的要求，例如查詢字串或表單資料透過 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。</span><span class="sxs-lookup"><span data-stu-id="42d9e-104">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="42d9e-105">這種竄改稱為開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="42d9e-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="42d9e-106">每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL，沒有遭到竄改。</span><span class="sxs-lookup"><span data-stu-id="42d9e-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="42d9e-107">ASP.NET Core 具有內建的功能，可協助保護應用程式，從開啟的重新導向 （也就是開啟的重新導向） 攻擊。</span><span class="sxs-lookup"><span data-stu-id="42d9e-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="42d9e-108">什麼是開啟的重新導向攻擊？</span><span class="sxs-lookup"><span data-stu-id="42d9e-108">What is an open redirect attack?</span></span>

<span data-ttu-id="42d9e-109">Web 應用程式經常將使用者重新導向至登入頁面存取需要驗證的資源時。</span><span class="sxs-lookup"><span data-stu-id="42d9e-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="42d9e-110">包含重新導向 typlically `returnUrl` querystring 參數，以便使用者可以傳回至原本要求的 URL 之後使用者成功登入。</span><span class="sxs-lookup"><span data-stu-id="42d9e-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="42d9e-111">使用者驗證之後，它們會重新導向至其原先要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="42d9e-111">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="42d9e-112">因為要求的 querystring 中指定的目的地 URL，惡意的使用者無法修改查詢字串。</span><span class="sxs-lookup"><span data-stu-id="42d9e-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="42d9e-113">遭竄改的查詢字串可能會允許將使用者重新導向至外部，惡意的站台的站台。</span><span class="sxs-lookup"><span data-stu-id="42d9e-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="42d9e-114">這項技術稱為開啟重新導向 （或重新導向） 攻擊。</span><span class="sxs-lookup"><span data-stu-id="42d9e-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="42d9e-115">範例攻擊</span><span class="sxs-lookup"><span data-stu-id="42d9e-115">An example attack</span></span>

<span data-ttu-id="42d9e-116">惡意使用者可以開發目的是讓使用者的認證或在您的應用程式上的機密資訊的惡意使用者存取的攻擊。</span><span class="sxs-lookup"><span data-stu-id="42d9e-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="42d9e-117">若要開始攻擊，它們說服使用者連結至網站的登入頁面上，按一下與`returnUrl`加入 URL querystring 值。</span><span class="sxs-lookup"><span data-stu-id="42d9e-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="42d9e-118">例如， [NerdDinner.com](http://nerddinner.com)範例應用程式 （寫入 ASP.NET mvc） 包含這類登入頁面。 這裡： ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``。</span><span class="sxs-lookup"><span data-stu-id="42d9e-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="42d9e-119">攻擊，然後遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="42d9e-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="42d9e-120">使用者按下連結``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn``(請注意，第二個 URL 是 nerddi**n**呃，不 nerddi**nn**er)。</span><span class="sxs-lookup"><span data-stu-id="42d9e-120">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="42d9e-121">使用者成功登入。</span><span class="sxs-lookup"><span data-stu-id="42d9e-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="42d9e-122">使用者重新導向 （由站台） 至``http://nerddiner.com/Account/LogOn``（看起來像真正的站台的惡意網站）。</span><span class="sxs-lookup"><span data-stu-id="42d9e-122">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="42d9e-123">使用者再次登入 （提供惡意網站他們的認證） 會重新導向至實際站台。</span><span class="sxs-lookup"><span data-stu-id="42d9e-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="42d9e-124">使用者將可能認為其第一次嘗試登入失敗，且其第二個是成功。</span><span class="sxs-lookup"><span data-stu-id="42d9e-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="42d9e-125">將最有可能仍不知道他們的認證遭洩漏。</span><span class="sxs-lookup"><span data-stu-id="42d9e-125">They'll most likely remain unaware their credentials have been compromised.</span></span>

![開啟的重新導向攻擊程序](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="42d9e-127">除了登入頁面中，有些網站會提供重新導向頁面或端點。</span><span class="sxs-lookup"><span data-stu-id="42d9e-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="42d9e-128">假設您的應用程式已開啟的重新導向的分頁``/Home/Redirect``。</span><span class="sxs-lookup"><span data-stu-id="42d9e-128">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="42d9e-129">攻擊者無法建立，比方說，連結會移至以電子郵件``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``。</span><span class="sxs-lookup"><span data-stu-id="42d9e-129">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="42d9e-130">一般使用者會查看 URL，並請參閱您的網站名稱開頭。</span><span class="sxs-lookup"><span data-stu-id="42d9e-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="42d9e-131">信任的會讓他們按一下連結。</span><span class="sxs-lookup"><span data-stu-id="42d9e-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="42d9e-132">開啟的重新導向至網路釣魚網站，看起來與您相同，然後就會傳送使用者和使用者便可能它們所認為的登入是您的網站。</span><span class="sxs-lookup"><span data-stu-id="42d9e-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="42d9e-133">保護開啟的重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="42d9e-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="42d9e-134">在開發 web 應用程式時，所有使用者提供資料都視為不受信任。</span><span class="sxs-lookup"><span data-stu-id="42d9e-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="42d9e-135">如果您的應用程式已重新導向 URL 的內容會根據使用者的功能，請確定的這類的重新導向只進行本機應用程式中 （或已知的 URL，可能需要提供於 querystring 中不含任何 URL）。</span><span class="sxs-lookup"><span data-stu-id="42d9e-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="42d9e-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="42d9e-136">LocalRedirect</span></span>

<span data-ttu-id="42d9e-137">使用``LocalRedirect``helper 方法的基底`Controller`類別：</span><span class="sxs-lookup"><span data-stu-id="42d9e-137">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="42d9e-138">``LocalRedirect``如果指定非本機 URL，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="42d9e-138">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="42d9e-139">否則，它的行為就像是``Redirect``方法。</span><span class="sxs-lookup"><span data-stu-id="42d9e-139">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="42d9e-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="42d9e-140">IsLocalUrl</span></span>

<span data-ttu-id="42d9e-141">使用[IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)方法來測試之前將重新導向的 Url:</span><span class="sxs-lookup"><span data-stu-id="42d9e-141">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="42d9e-142">下列範例會示範如何檢查 URL 是否本機，然後才將重新導向。</span><span class="sxs-lookup"><span data-stu-id="42d9e-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="42d9e-143">`IsLocalUrl`方法可防止使用者不小心重新導向至惡意網站。</span><span class="sxs-lookup"><span data-stu-id="42d9e-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="42d9e-144">您可以記錄的情況下，您必須是本機 URL 提供給非本機 URL 時所提供的 URL 的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="42d9e-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="42d9e-145">記錄重新導向 Url 可能有助於診斷重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="42d9e-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>