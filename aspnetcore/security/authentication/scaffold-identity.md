---
title: ASP.NET Core プロジェクトに scaffold Id
author: rick-anderson
description: ASP.NET Core プロジェクト内の Id をスキャフォールディングする方法を説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 7527d3c075fd845ac804d4cfd56469a0679ed7e8
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="b60dd-103">ASP.NET Core プロジェクトに scaffold Id</span><span class="sxs-lookup"><span data-stu-id="b60dd-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="b60dd-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b60dd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b60dd-105">ASP.NET Core 2.1 以降提供[ASP.NET Core Id](xref:security/authentication/identity)として、 [Razor クラス ライブラリ](xref:mvc/razor-pages/ui-class)です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="b60dd-106">Id を含むアプリケーションでは、選択的に Identity Razor クラス ライブラリ (RCL) に含まれるソース コードを追加する scaffolder を適用できます。</span><span class="sxs-lookup"><span data-stu-id="b60dd-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="b60dd-107">コードを変更して、動作を変更できるように、ソース コードを生成する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b60dd-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="b60dd-108">たとえば、scaffolder 登録で使用するコードを生成するように指示できます。</span><span class="sxs-lookup"><span data-stu-id="b60dd-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="b60dd-109">生成されたコードは、Identity RCL で同じコードよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="b60dd-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="b60dd-110">実行するアプリケーション**いない**含める認証 RCL Identity パッケージを追加する scaffolder を適用できます。</span><span class="sxs-lookup"><span data-stu-id="b60dd-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="b60dd-111">生成される Id コードを選択した場合のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="b60dd-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="b60dd-112">Scaffolder に必要なコードのほとんどが生成されますが、プロセスを完了するようにプロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b60dd-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="b60dd-113">このドキュメントでは、Identity スキャフォールディング更新を完了するために必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="b60dd-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="b60dd-114">Identity scaffolder を実行すると、 *ScaffoldingReadme.txt*プロジェクト ディレクトリにファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b60dd-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="b60dd-115">*ScaffoldingReadme.txt*ファイルには、Identity スキャフォールディング更新を完了する必要があるが何を一般的な手順が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b60dd-115">The *ScaffoldingReadme.txt* file contains general instructions on what's need to complete the Identity scaffolding update.</span></span> <span data-ttu-id="b60dd-116">このドキュメントには、読み取りよりも詳細な手順が含まれています、 *ScaffoldingReadme.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b60dd-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="b60dd-117">ファイルの相違点を表示し、変更を取り消すには、ソース管理システムを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b60dd-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="b60dd-118">Identity scaffolder を実行した後、変更を検査します。</span><span class="sxs-lookup"><span data-stu-id="b60dd-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="b60dd-119">空のプロジェクトに scaffold identity</span><span class="sxs-lookup"><span data-stu-id="b60dd-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b60dd-120">次の強調表示されている呼び出しを追加、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="b60dd-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="b60dd-121">既存の承認はしない Razor プロジェクトに scaffold identity</span><span class="sxs-lookup"><span data-stu-id="b60dd-121">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b60dd-122">ように構成された id *Areas/Identity/IdentityHostingStartup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b60dd-123">詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b60dd-124">`Configure`のメソッド、`Startup`クラス、呼び出し[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b60dd-124">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="b60dd-125">レイアウトの変更</span><span class="sxs-lookup"><span data-stu-id="b60dd-125">Layout changes</span></span>

<span data-ttu-id="b60dd-126">省略可能: 部分のログインを追加する (`_LoginPartial`) をレイアウト ファイル。</span><span class="sxs-lookup"><span data-stu-id="b60dd-126">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-individual-authorization"></a><span data-ttu-id="b60dd-127">個々 の承認で Razor プロジェクトに scaffold identity</span><span class="sxs-lookup"><span data-stu-id="b60dd-127">Scaffold identity into a Razor project with individual authorization</span></span>

<!--
dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="b60dd-128">いくつかの Id オプションを構成*Areas/Identity/IdentityHostingStartup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-128">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b60dd-129">詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-129">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="b60dd-130">既存の承認はしない MVC プロジェクトに scaffold identity</span><span class="sxs-lookup"><span data-stu-id="b60dd-130">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b60dd-131">省略可能: 部分のログインを追加する (`_LoginPartial`) に、 *Views/Shared/_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b60dd-131">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="b60dd-132">移動、 *Pages/Shared/_LoginPartial.cshtml*ファイルの名前を*Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b60dd-132">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="b60dd-133">ように構成された id *Areas/Identity/IdentityHostingStartup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="b60dd-133">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b60dd-134">詳細については、IHostingStartup を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b60dd-134">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b60dd-135">呼び出す[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b60dd-135">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-individual-authorization"></a><span data-ttu-id="b60dd-136">個々 の承認で MVC プロジェクトに scaffold identity</span><span class="sxs-lookup"><span data-stu-id="b60dd-136">Scaffold identity into an MVC project with individual authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="b60dd-137">削除、*ページ/共有*フォルダーとそのフォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="b60dd-137">Delete the *Pages/Shared* folder and the files in that folder.</span></span>