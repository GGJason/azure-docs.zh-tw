---
title: 開始使用 .NET MVC 專案中的 Azure AD |Azure
description: 使用 Visual Studio 連線服務連線至 Azure AD 或建立 Azure AD 後，如何在 .NET MVC 專案中開始使用 Azure Active Directory
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: fe408e965c528db1d82b73ee7b20bbe3b3933657
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80886121"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>開始使用 Azure Active Directory (ASP.NET MVC 專案)

> [!div class="op_single_selector"]
> - [快速入門](vs-active-directory-dotnet-getting-started.md)
> - [發生了什麼事](vs-active-directory-dotnet-what-happened.md)

本文提供您在透過 Visual Studio 的 [專案] > [連線服務]**** 命令將 Active Directory 新增至 ASP.NET MVC 專案之後所需的其他指引。 如果您尚未將服務新增至專案，您可以隨時執行此動作。

請參閱[我的 MVC 專案發生什麼情形？](vs-active-directory-dotnet-what-happened.md)，了解您的專案在新增連線服務時所做的變更。

## <a name="requiring-authentication-to-access-controllers"></a>存取控制器之前需要驗證

專案中的所有控制器都會加上 `[Authorize]` 屬性作為裝飾。 此屬性要求使用者必須經過驗證，才能存取這些控制器。 若要允許以匿名方式存取控制器，請從控制器中移除此屬性。 如果您要以更精確地設定權限，請將此屬性套用至每一個需要授權的方法，而非套用至控制器類別。

## <a name="adding-signin--signout-controls"></a>加入 SignIn / SignOut 控制項

若要將 SignIn/SignOut 控制項新增至檢視，您可以使用 `_LoginPartial.cshtml` 部分檢視，將此功能新增至您的其中一個檢視。 以下是新增至標準 `_Layout.cshtml` 檢視的功能範例。 (請注意 div 中具有類別 navbar-collapse 的最後一個元素)：

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>後續步驟

- [Azure Active Directory 的驗證案例](authentication-scenarios.md)
- [將「使用 Microsoft 登入」新增至 ASP.NET Web 應用程式](quickstart-v2-aspnet-webapp.md)
