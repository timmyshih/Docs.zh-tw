---
title: "啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生"
author: rick-anderson
description: "啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生"
keywords: "ASP.NET Core、 MVC、 QR 代碼產生驗證器、 2FA"
ms.author: riande
manager: wpickett
ms.date: 7/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4eb260ecb3d1f1797bed5ba82ef3fc0628f05b6f
ms.sourcegitcommit: bb615e57e80ae496a63a1bf53a3a9cffab2fce9b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>啟用驗證器應用程式中 ASP.NET Core 的 QR 代碼產生

注意： 本主題適用於 ASP.NET Core 2.x 使用 Razor 頁面。

ASP.NET Core 隨附個別驗證的驗證器應用程式的支援。 兩個因素驗證 (2FA) 驗證器應用程式，使用以時間為基礎單次密碼演算法 (TOTP)，是建議的 2FA approch 產業。 2FA 使用 TOTP 最好 SMS 2FA。 驗證器應用程式提供的使用者必須確認使用者名稱和密碼之後輸入 6 to 8 位數代碼。 通常驗證器應用程式會安裝在智慧型手機上。

ASP.NET Core web 應用程式範本支援的驗證器，但不是提供支援 QRCode 產生。 QRCode 產生器可簡化 2FA 的安裝程式。 本文件將引導您完成新增[QR 代碼](https://wikipedia.org/wiki/QR_code)產生 2FA 組態頁面。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>將 QR 代碼加入至 [2FA 組態] 頁面

這些指示使用*qrcode.js*從 https://davidshimjs.github.io/qrcodejs/ 儲存機制。

* 下載[qrcode.js javascript 程式庫](https://davidshimjs.github.io/qrcodejs/)至`wwwroot\lib`專案資料夾中的。

* 在*Pages\Account\Manage\EnableAuthenticator.cshtml*檔案中，找出`Scripts`檔案結尾 」 一節：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新`Scripts`區段，即可將參考加入`qrcodejs`您加入程式庫，並呼叫，產生的 QR 代碼。 它看起來應該如下：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* 刪除的段落，連結至這些指示。

執行您的應用程式並確定您可以用來掃描該 QR 代碼，並驗證驗證器證明程式碼。

## <a name="change-the-site-name-in-the-qr-code"></a>變更站台名稱在 QR 代碼

將 QR 代碼中的站台名稱是取自您一開始建立專案時，選擇專案名稱。 您可以藉由尋找來加以變更`GenerateQrCodeUri(string email, string unformattedKey)`方法中的*EnableAuthenticator.cshtml.cs*檔案。 

從範本的預設程式碼看起來如下：

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

呼叫中的第二個參數`string.Format`是您的站台名稱，從您的方案名稱。 它可變更為任何值，但它必須一律是 URL 編碼。

## <a name="using-a-different-qr-code-library"></a>使用不同的 QR 代碼程式庫

您可以使用您慣用的程式庫來取代 QR 代碼程式庫。 包含 HTML`qrCode`您可以在其中放置 QR 代碼透過任何機制的項目會提供您的程式庫。

將 QR 代碼的正確格式的 URL 位於:

* `AuthenticatorUri`模型的屬性。
* `data-url`中的屬性`qrCodeData`項目。 

使用`@Html.Raw`存取檢視中的模型屬性 （否則會雙重編碼 url 中的連字號和 QR 代碼的標籤參數將被忽略）。