---
title: "ネイティブなモバイル アプリケーションのバックエンド サービスを作成します。"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: 0737cb1c81b6a143090234f37e5567d5c605b99e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="6d122-102">ネイティブなモバイル アプリケーションのバックエンド サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d122-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="6d122-103">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6d122-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6d122-104">モバイル アプリは、ASP.NET Core バックエンド サービスと通信に簡単にできます。</span><span class="sxs-lookup"><span data-stu-id="6d122-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="6d122-105">表示またはバックエンド サービスのサンプル コードをダウンロード</span><span class="sxs-lookup"><span data-stu-id="6d122-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="6d122-106">サンプルのネイティブ モバイル アプリ</span><span class="sxs-lookup"><span data-stu-id="6d122-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="6d122-107">このチュートリアルでは、ネイティブ モバイル アプリをサポートするために ASP.NET Core MVC を使用してバックエンド サービスを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6d122-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="6d122-108">使用して、 [Xamarin フォーム ToDoRest アプリ](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/)をその native client と Android、iOS、Windows ユニバーサルおよび Window Phone のデバイス用の個別のネイティブ クライアントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6d122-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="6d122-109">ネイティブ アプリを作成する (および必要な空き Xamarin ツールをインストールする) にリンクされているチュートリアルに従ってしたりできるよう Xamarin サンプル ソリューションをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6d122-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="6d122-110">Xamarin サンプルには、この記事の ASP.NET Core アプリケーションに置き換えます (クライアントで必要な変更はありません)、ASP.NET Web API 2 services プロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6d122-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Android フォンで実行されているは Rest アプリケーションに](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="6d122-112">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="6d122-112">Features</span></span>

<span data-ttu-id="6d122-113">ToDoRest アプリには、一覧表示する、追加、削除、および作業項目の更新がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="6d122-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="6d122-114">各アイテムには、ID、名前やノートでは、まだ完了されてがあるかどうかを示すプロパティです。</span><span class="sxs-lookup"><span data-stu-id="6d122-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="6d122-115">アイテムのメイン ビュー、上記のようには、各項目の名前を一覧表示し、かどうかは、チェック マークの付いた実行を示します。</span><span class="sxs-lookup"><span data-stu-id="6d122-115">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="6d122-116">タップすると、`+`アイコンは、追加の項目 ダイアログを開きます。</span><span class="sxs-lookup"><span data-stu-id="6d122-116">Tapping the `+` icon opens an add item dialog:</span></span>

![追加の項目 ダイアログ ボックス](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="6d122-118">メイン リスト 画面で項目をタップすると、編集 ダイアログ ボックスで、項目の名前、メモ、および元に戻す設定を変更できる、または項目を削除することができますが開きます。</span><span class="sxs-lookup"><span data-stu-id="6d122-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![項目のダイアログ ボックスを編集します。](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="6d122-120">このサンプルは、既定では読み取り専用の操作を許可するように developer.xamarin.com でホストされているバックエンド サービスを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="6d122-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="6d122-121">お使いのコンピューターで実行されている次のセクションで作成された ASP.NET Core アプリケーションに対してを自分でテストするには、アプリを更新する必要があります`RestUrl`定数。</span><span class="sxs-lookup"><span data-stu-id="6d122-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="6d122-122">移動し、`ToDoREST`プロジェクトから開き、 *Constants.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6d122-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="6d122-123">置換、`RestUrl`マシンの ip アドレスを含む URL を使用してアドレス (localhost または 127.0.0.1、コンピューターからではなく、デバイス エミュレーターからこのアドレスが使用されるため)。</span><span class="sxs-lookup"><span data-stu-id="6d122-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="6d122-124">ポート番号も (5000) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6d122-124">Include the port number as well (5000).</span></span> <span data-ttu-id="6d122-125">デバイスで、サービスが動作するかをテストするために、アクティブなファイアウォールがこのポートへのアクセスをブロックしていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6d122-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="6d122-126">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d122-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="6d122-127">Visual Studio で新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d122-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="6d122-128">認証なしの Web API テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="6d122-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="6d122-129">プロジェクトに名前を*ToDoApi*です。</span><span class="sxs-lookup"><span data-stu-id="6d122-129">Name the project *ToDoApi*.</span></span>

![選択されている Web API プロジェクト テンプレートを使って新しい ASP.NET Web アプリケーション ダイアログ](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="6d122-131">アプリケーションは、5000 のポートに加えられたすべての要求に応答する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d122-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="6d122-132">更新*Program.cs*に含める`.UseUrls("http://*:5000")`これを実現します。</span><span class="sxs-lookup"><span data-stu-id="6d122-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="6d122-133">IIS Express、既定では、ローカル以外の要求を無視する背後ではなく、直接、アプリケーションを実行することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6d122-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="6d122-134">実行`dotnet run`コマンド プロンプトから、または、Visual Studio ツールバーのデバッグ ターゲット ドロップダウン リストから、アプリケーション名のプロファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="6d122-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="6d122-135">作業項目を表すためのモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d122-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="6d122-136">マークは必須フィールドを使用して、`[Required]`属性。</span><span class="sxs-lookup"><span data-stu-id="6d122-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="6d122-137">API のメソッドでは、データを操作するいくつかの方法が必要です。</span><span class="sxs-lookup"><span data-stu-id="6d122-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="6d122-138">使用して同じ`IToDoRepository`インターフェイスの元の Xamarin サンプルの使用。</span><span class="sxs-lookup"><span data-stu-id="6d122-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="6d122-139">このサンプルでは、実装はプライベート項目のコレクションだけを使用します。</span><span class="sxs-lookup"><span data-stu-id="6d122-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="6d122-140">実装を構成する*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6d122-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="6d122-141">作成する準備ができたら、この時点で、 *ToDoItemsController*です。</span><span class="sxs-lookup"><span data-stu-id="6d122-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="6d122-142">Api の web の作成の詳細を学習[構築 Your 最初 Web API Core MVC の ASP.NET と Visual Studio で](../tutorials/first-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="6d122-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="6d122-143">コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6d122-143">Creating the Controller</span></span>

<span data-ttu-id="6d122-144">新しいコント ローラーをプロジェクトに追加*ToDoItemsController*です。</span><span class="sxs-lookup"><span data-stu-id="6d122-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="6d122-145">Microsoft.AspNetCore.Mvc.Controller から継承してする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d122-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="6d122-146">追加、`Route`コント ローラーではじまるパスへの要求を処理することを示すために属性`api/todoitems`です。</span><span class="sxs-lookup"><span data-stu-id="6d122-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="6d122-147">`[controller]`ルート内の変数が、コント ローラーの名前に置き換えられます (を省略すると、`Controller`サフィックス)、およびグローバルのルートで特に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6d122-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="6d122-148">詳細については[ルーティング](../fundamentals/routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="6d122-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="6d122-149">コント ローラーが必要です、`IToDoRepository`を関数には、コント ローラーのコンス トラクターでは、この型のインスタンスを要求します。</span><span class="sxs-lookup"><span data-stu-id="6d122-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="6d122-150">実行時に、このインスタンスが行われますのフレームワークのサポートを使用して[依存性の注入](../fundamentals/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="6d122-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="6d122-151">この API は、データ ソースに対して CRUD (作成、読み取り、更新、削除) 操作を実行する 4 つの異なる HTTP 動詞をサポートします。</span><span class="sxs-lookup"><span data-stu-id="6d122-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="6d122-152">HTTP GET 要求に対応する読み取り操作は、これらの最も簡単なのです。</span><span class="sxs-lookup"><span data-stu-id="6d122-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="6d122-153">項目の読み取り</span><span class="sxs-lookup"><span data-stu-id="6d122-153">Reading Items</span></span>

<span data-ttu-id="6d122-154">GET 要求では項目の一覧を要求する、`List`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="6d122-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="6d122-155">`[HttpGet]`属性を`List`メソッドでは、このアクションが GET 要求を処理する必要がありますのみを示します。</span><span class="sxs-lookup"><span data-stu-id="6d122-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="6d122-156">このアクションのルートがコント ローラーで指定されたルートです。</span><span class="sxs-lookup"><span data-stu-id="6d122-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="6d122-157">必ずしも、ルートの一部として、アクションの名前を使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6d122-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="6d122-158">各アクションに一意で明確なルートがあることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d122-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="6d122-159">ルーティング属性は、コント ローラーと特定のルートを構築するメソッドのレベルの両方に適用できます。</span><span class="sxs-lookup"><span data-stu-id="6d122-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="6d122-160">`List`メソッド 200 OK の応答コードとそのすべての JSON としてシリアル化、ToDo 項目を返します。</span><span class="sxs-lookup"><span data-stu-id="6d122-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="6d122-161">などのさまざまなツールを使用して、新しい API メソッドをテストする[Postman](https://www.getpostman.com/docs/)、ここで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="6d122-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Todoitems とを示す、返される次の 3 つの項目の JSON 応答の本文の GET 要求を示す postman コンソール](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="6d122-163">項目の作成</span><span class="sxs-lookup"><span data-stu-id="6d122-163">Creating Items</span></span>

<span data-ttu-id="6d122-164">慣例により、新しいデータ項目の作成、HTTP POST 動詞にマップされます。</span><span class="sxs-lookup"><span data-stu-id="6d122-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="6d122-165">`Create`メソッドには、`[HttpPost]`属性が適用されているし、受け入れる、`ToDoItem`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="6d122-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="6d122-166">以降、 `item` 、通知の本文で渡される引数、このパラメーターで装飾されて、`[FromBody]`属性。</span><span class="sxs-lookup"><span data-stu-id="6d122-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="6d122-167">メソッドの内部有効性および前の存在、データ ストア内の項目がチェックして、リポジトリを使用して追加されますの問題が発生しなかった場合。</span><span class="sxs-lookup"><span data-stu-id="6d122-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="6d122-168">チェック`ModelState.IsValid`実行[モデルの検証](../mvc/models/validation.md)、し、ユーザー入力を受け付けるすべての API メソッドで行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d122-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="6d122-169">サンプルは、モバイル クライアントに渡されるエラー コードを含む列挙型を使用します。</span><span class="sxs-lookup"><span data-stu-id="6d122-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="6d122-170">要求の本文の JSON 形式で新しいオブジェクトを提供する POST 動詞を選択して Postman を使って新しいアイテムを追加することをテストします。</span><span class="sxs-lookup"><span data-stu-id="6d122-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="6d122-171">指定する要求ヘッダーを追加する必要がありますも、`Content-Type`の`application/json`します。</span><span class="sxs-lookup"><span data-stu-id="6d122-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![POST と応答を示す postman コンソール](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="6d122-173">メソッドは、応答で新しく作成されたアイテムを返します。</span><span class="sxs-lookup"><span data-stu-id="6d122-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="6d122-174">アイテムの更新</span><span class="sxs-lookup"><span data-stu-id="6d122-174">Updating Items</span></span>

<span data-ttu-id="6d122-175">レコードの変更は実行 HTTP PUT 要求を使用します。</span><span class="sxs-lookup"><span data-stu-id="6d122-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="6d122-176">この変更以外、`Edit`メソッドはほぼ同じである`Create`です。</span><span class="sxs-lookup"><span data-stu-id="6d122-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="6d122-177">レコードが見つからない場合、`Edit`アクションが返されます、 `NotFound` (404) 応答です。</span><span class="sxs-lookup"><span data-stu-id="6d122-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="6d122-178">Postman をテストするには、put 動詞を変更します。</span><span class="sxs-lookup"><span data-stu-id="6d122-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="6d122-179">要求の本文で更新されたオブジェクト データを指定します。</span><span class="sxs-lookup"><span data-stu-id="6d122-179">Specify the updated object data in the Body of the request.</span></span>

![PUT と応答を示す postman コンソール](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="6d122-181">このメソッドが戻る、 `NoContent` (204) 応答の整合性の既存の API が成功するとします。</span><span class="sxs-lookup"><span data-stu-id="6d122-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="6d122-182">アイテムの削除</span><span class="sxs-lookup"><span data-stu-id="6d122-182">Deleting Items</span></span>

<span data-ttu-id="6d122-183">レコードを削除する操作は、サービスに DELETE 要求を行うと、削除する項目の ID を渡すことによって行います。</span><span class="sxs-lookup"><span data-stu-id="6d122-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="6d122-184">存在しない項目の要求を受信する更新では、として`NotFound`応答します。</span><span class="sxs-lookup"><span data-stu-id="6d122-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="6d122-185">それ以外の場合、要求が成功した場合になります、 `NoContent` (204) 応答です。</span><span class="sxs-lookup"><span data-stu-id="6d122-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="6d122-186">削除機能をテストする場合は nothing に必要である、要求の本文に注意してください。</span><span class="sxs-lookup"><span data-stu-id="6d122-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![削除と応答を示す postman コンソール](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="6d122-188">Web API の一般的な規則</span><span class="sxs-lookup"><span data-stu-id="6d122-188">Common Web API Conventions</span></span>

<span data-ttu-id="6d122-189">アプリのバックエンド サービスを開発する際、一貫性のある一連の規則または横断的関心事を処理するためのポリシーを起動することができます。</span><span class="sxs-lookup"><span data-stu-id="6d122-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="6d122-190">たとえば前に、示したサービスで受信が見つかりませんでした。 特定のレコードの要求、 `NotFound` 、応答ではなく、`BadRequest`応答します。</span><span class="sxs-lookup"><span data-stu-id="6d122-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="6d122-191">同様に、コマンドが常に確認の種類のバインドされたモデルに渡されたこのサービスに加えられた`ModelState.IsValid`返されると、`BadRequest`の無効なモデルの種類。</span><span class="sxs-lookup"><span data-stu-id="6d122-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="6d122-192">独自の Api の一般的なポリシーを識別したら、通常をカプセル化できますで、[フィルター](../mvc/controllers/filters.md)です。</span><span class="sxs-lookup"><span data-stu-id="6d122-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="6d122-193">詳細については[ASP.NET Core MVC アプリケーションで共通の API ポリシーをカプセル化する](https://msdn.microsoft.com/magazine/mt767699.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6d122-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>