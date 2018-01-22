---
title: "ASP.NET Core 1.1 の概要"
author: rick-anderson
description: "ASP.NET Core 1.1 を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: 7f178618508e1a1e9c49d8ace619b9f942998dae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="655ed-103">ASP.NET Core 1.1 の概要</span><span class="sxs-lookup"><span data-stu-id="655ed-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="655ed-104">ここで説明する手順は ASP.NET Core 1.1 用です。</span><span class="sxs-lookup"><span data-stu-id="655ed-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="655ed-105">最新バージョンについては、</span><span class="sxs-lookup"><span data-stu-id="655ed-105">Looking for the latest version?</span></span> <span data-ttu-id="655ed-106">[このチュートリアルの最新バージョン](xref:getting-started)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="655ed-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="655ed-107">[.NET Core 1.0.5 および 1.1.2 SDK 1.0.4 ダウンロード ページ](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="655ed-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="655ed-108">新しい .NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="655ed-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="655ed-109">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="655ed-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="655ed-110">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="655ed-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="655ed-111">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="655ed-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="655ed-112">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="655ed-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="655ed-113">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="655ed-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="655ed-114">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="655ed-114">Run the app.</span></span>

   <span data-ttu-id="655ed-115">必要に応じて、まず `dotnet run` コマンドでアプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="655ed-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="655ed-116">`http://localhost:5000` を参照します</span><span class="sxs-lookup"><span data-stu-id="655ed-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="655ed-117">次の手順</span><span class="sxs-lookup"><span data-stu-id="655ed-117">Next steps</span></span>

<span data-ttu-id="655ed-118">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="655ed-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="655ed-119">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="655ed-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="655ed-120">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="655ed-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="655ed-121">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="655ed-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>