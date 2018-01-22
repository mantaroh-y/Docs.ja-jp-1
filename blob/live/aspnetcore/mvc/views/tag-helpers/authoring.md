---
title: "ASP.NET Core のタグ ヘルパーの作成"
author: rick-anderson
description: "ASP.NET Core のタグ ヘルパーの作成方法を説明します。"
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9aaf40377e07e53fd0b7ebb177bcbb2df52b7553
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="a8ccf-103">ASP.NET Core、サンプルとチュートリアルの作成者タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a8ccf-103">Author Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="a8ccf-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a8ccf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a8ccf-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="a8ccf-106">タグ ヘルパーを概要します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="a8ccf-107">このチュートリアルでは、タグ ヘルパーのプログラミングの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="a8ccf-108">[タグ ヘルパーの概要](intro.md)タグ ヘルパーを提供する利点について説明します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="a8ccf-109">タグ ヘルパーを実装するクラス、`ITagHelper`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="a8ccf-110">ただし、タグ ヘルパーを作成する場合は、一般にから派生した`TagHelper`、これにアクセスするにより、`Process`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="a8ccf-111">いう新しい ASP.NET Core プロジェクトを作成する**AuthoringTagHelpers**です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="a8ccf-112">認証は、このプロジェクトの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="a8ccf-113">呼ばれるタグ ヘルパーを保持するフォルダーを作成する*TagHelpers*です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="a8ccf-114">*TagHelpers*フォルダーは*いない*必要に応じてが、妥当な規則です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-114">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="a8ccf-115">今すぐ始めましょういくつかの単純なタグ ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="a8ccf-116">最小タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a8ccf-116">A minimal Tag Helper</span></span>

<span data-ttu-id="a8ccf-117">このセクションでは、電子メール タグを更新するタグ ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="a8ccf-118">例:</span><span class="sxs-lookup"><span data-stu-id="a8ccf-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="a8ccf-119">サーバーは次のように、そのマークアップを変換するのに、電子メール タグ ヘルパーを使用します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="a8ccf-120">つまり、アンカー タグにより、この電子メールのリンク。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="a8ccf-121">これを行う場合は、ブログ エンジンを作成して、同じドメインにすべてマーケティング、サポート、およびその他の連絡先の電子メールを送信する際に必要する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="a8ccf-122">次の追加`EmailTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="a8ccf-123">**注:**</span><span class="sxs-lookup"><span data-stu-id="a8ccf-123">**Notes:**</span></span>
    
    * <span data-ttu-id="a8ccf-124">タグ ヘルパーの使用をルート クラスの名前を持つ要素を対象とする名前付け規則 (マイナス、 *TagHelper*クラス名の部分)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="a8ccf-125">この例では、ルート名で**電子メール**TagHelper は*電子メール*ので、`<email>`タグの対象となります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="a8ccf-126">後でこのメソッドをオーバーライドする方法を示しますが、この名前付け規則がほとんどタグ ヘルパーの動作する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="a8ccf-127">`EmailTagHelper` クラスは `TagHelper` から派生したものです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="a8ccf-128">`TagHelper`クラス タグ ヘルパーを記述するためのメソッドとプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="a8ccf-129">オーバーライドされた`Process`タグ ヘルパーが実行時にメソッドを制御します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="a8ccf-130">`TagHelper`クラスには、非同期バージョンも用意されています (`ProcessAsync`) 同じパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="a8ccf-131">コンテキスト パラメーターを`Process`(および`ProcessAsync`) 現在の HTML タグの実行に関連付けられている情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="a8ccf-132">出力パラメーターを`Process`(および`ProcessAsync`)、HTML タグとコンテンツの生成に使用される元のソースの代表的なステートフルな HTML 要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="a8ccf-133">クラス名のサフィックスを持つ**TagHelper**、これは*いない*、必要なことがベスト プラクティス規則と考えられますが、します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="a8ccf-134">クラスを宣言します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-134">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="a8ccf-135">させる、 `EmailTagHelper` 、すべての Razor ビューに使用可能なクラスを追加、`addTagHelper`ディレクティブを*Views/_ViewImports.cshtml*ファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="a8ccf-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="a8ccf-136">上記のコードは、使用可能なアセンブリ内のすべてのタグ ヘルパーを指定するのにワイルドカードの構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="a8ccf-137">後の最初の文字列`@addTagHelper`を読み込むタグ ヘルパーを指定します (使用する"\*"すべてのタグ ヘルパーの)、2 番目の文字列"AuthoringTagHelpers"アセンブリを指定するのには、タグ ヘルパーとします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="a8ccf-138">また、ワイルドカードの構文を使用して ASP.NET Core MVC タグ ヘルパーの 2 番目の行がもたらすことに注意してください (これらのヘルパーは、後ほど[タグ ヘルパーの概要](intro.md))。`@addTagHelper`タグ ヘルパーの Razor ビューを使用できるようにするディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="a8ccf-139">また、次に示すように、タグ ヘルパーの完全修飾名 (FQN) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="a8ccf-140">タグ ヘルパーに追加する、FQN を使用するビューを最初に追加する、FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、およびアセンブリ名では、(*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="a8ccf-141">ほとんどの開発者は、ワイルドカードの構文を使用するを選びます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="a8ccf-142">[タグ ヘルパーの概要](intro.md)タグ ヘルパーの追加、削除、階層、およびワイルドカードの構文の詳細を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="a8ccf-143">内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="a8ccf-144">アプリを実行して、アンカー マークアップを含む電子メール タグが置換されることを確認できるように HTML ソースの表示をお気に入りのブラウザーを使用して (たとえば、 `<a>Support</a>`)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="a8ccf-145">*サポート*と*マーケティング*は、リンクとして表示はありません、`href`ように機能する属性。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="a8ccf-146">解決する次のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-146">We'll fix that in the next section.</span></span>

<span data-ttu-id="a8ccf-147">注: HTML タグと属性、タグ、クラス名および属性で Razor、および c# のように小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-147">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="a8ccf-148">SetAttribute と SetContent</span><span class="sxs-lookup"><span data-stu-id="a8ccf-148">SetAttribute and SetContent</span></span>

<span data-ttu-id="a8ccf-149">このセクションで更新されます、`EmailTagHelper`電子メール用の有効なアンカー タグが作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-149">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="a8ccf-150">Razor ビューから情報に更新されます (の形式で、`mail-to`属性) と、アンカーの生成に使用します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-150">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="a8ccf-151">更新プログラム、`EmailTagHelper`を次のクラス。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-151">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="a8ccf-152">**注:**</span><span class="sxs-lookup"><span data-stu-id="a8ccf-152">**Notes:**</span></span>

* <span data-ttu-id="a8ccf-153">タグ ヘルパーのクラスおよびプロパティを pascal で名前に変換されます、 [kebab ケースを下げる](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-153">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="a8ccf-154">そのため、使用する、`MailTo`属性を使用する`<email mail-to="value"/>`と同等です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-154">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="a8ccf-155">最後の行は、最低限の機能のタグ ヘルパーの完了したコンテンツを設定します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-155">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="a8ccf-156">強調表示された行は、属性を追加するための構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-156">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="a8ccf-157">属性コレクションに現在存在しない限り、その方法は、属性"href"できます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-157">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="a8ccf-158">使用することも、`output.Attributes.Add`タグ ヘルパー属性をタグの属性のコレクションの末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-158">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="a8ccf-159">内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="a8ccf-159">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="a8ccf-160">アプリケーションを実行し、適切なリンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-160">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="a8ccf-161">電子メール タグ自己終了を記述する場合は (`<email mail-to="Rick" />`)、最終的な出力は自己終了になります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-161">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="a8ccf-162">開始タグのみを持つタグを記述する機能を有効にする (`<email mail-to="Rick">`) に次のクラスを装飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-162">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="a8ccf-163">自己終了電子メール タグ ヘルパーでは、出力ようになります`<a href="mailto:Rick@contoso.com" />`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-163">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="a8ccf-164">自己終了アンカー タグは、有効な HTML を作成するのにはしたいはありませんが、自己終了タグのヘルパーを作成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-164">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="a8ccf-165">タグ ヘルパーの種類の設定、`TagMode`タグを読み取った後のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-165">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="a8ccf-166">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="a8ccf-166">ProcessAsync</span></span>

<span data-ttu-id="a8ccf-167">このセクションでは非同期電子メール ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-167">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="a8ccf-168">置換、`EmailTagHelper`クラスを次のコードで。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-168">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="a8ccf-169">**注:**</span><span class="sxs-lookup"><span data-stu-id="a8ccf-169">**Notes:**</span></span>

    * <span data-ttu-id="a8ccf-170">このバージョンでは、非同期`ProcessAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-170">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="a8ccf-171">非同期の`GetChildContentAsync`を返します、`Task`を含む、`TagHelperContent`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-171">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="a8ccf-172">使用して、`output`パラメーターを HTML 要素の内容を取得します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-172">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="a8ccf-173">次の変更を行う、 *Views/Home/Contact.cshtml*ファイルのタグ ヘルパーは、ターゲット電子メールを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-173">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="a8ccf-174">アプリを実行して、有効な電子メールのリンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-174">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="a8ccf-175">RemoveAll、PreContent.SetHtmlContent および PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="a8ccf-175">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="a8ccf-176">次の追加`BoldTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-176">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="a8ccf-177">**注:**</span><span class="sxs-lookup"><span data-stu-id="a8ccf-177">**Notes:**</span></span>
    
    * <span data-ttu-id="a8ccf-178">`[HtmlTargetElement]`属性のパスを示す HTML 属性を含む任意の HTML 要素が"bold"という名前の属性パラメーターは一致と`Process`クラスでオーバーライド メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-178">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="a8ccf-179">この例で、`Process`メソッドは、"bold"属性を削除しとを含むマークアップを囲む`<strong></strong>`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-179">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="a8ccf-180">既存のタグはコンテンツを交換しないため、開始を書き込む必要があります`<strong>`とタグ付け、`PreContent.SetHtmlContent`メソッドと終了`</strong>`とタグ付け、`PostContent.SetHtmlContent`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-180">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="a8ccf-181">変更、 *About.cshtml*を含むビュー、`bold`属性の値。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-181">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="a8ccf-182">完成したコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-182">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="a8ccf-183">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-183">Run the app.</span></span> <span data-ttu-id="a8ccf-184">好みのブラウザーを使用するには、ソースを検査して、マークアップを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-184">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="a8ccf-185">`[HtmlTargetElement]`上の属性のみが対象"bold"の属性名を提供する HTML マークアップ。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-185">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="a8ccf-186">`<bold>`タグ ヘルパーによって要素が変更されていません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-186">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="a8ccf-187">コメント アウト、`[HtmlTargetElement]`属性行とそれは、対象とする既定`<bold>`タグ、フォームの HTML マークアップは、`<bold>`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-187">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="a8ccf-188">ただし、既定の名前付け規則は、クラス名に一致**太字**に TagHelper`<bold>`タグ。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-188">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="a8ccf-189">アプリを実行していることを確認、`<bold>`タグがタグ ヘルパーによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-189">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="a8ccf-190">複数のクラスを装飾`[HtmlTargetElement]`ターゲットの論理 OR で結果の属性です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-190">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="a8ccf-191">たとえば、次のコードを使用するには、太字のタグまたは太字属性は一致します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-191">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="a8ccf-192">同じステートメントに複数の属性を追加するときに、ランタイムとして扱います論理 and。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-192">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="a8ccf-193">たとえば、次のコードで HTML 要素必要がありますという名前が"bold"という名前の属性を"bold"(`<bold bold />`) 一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-193">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="a8ccf-194">使用することも、`[HtmlTargetElement]`を対象となる要素の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-194">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="a8ccf-195">場合の例については、`BoldTagHelper`ターゲットに`<MyBold>`タグ、次の属性を使用すると。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-195">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="a8ccf-196">タグ ヘルパーにモデルを渡す</span><span class="sxs-lookup"><span data-stu-id="a8ccf-196">Pass a model to a Tag Helper</span></span>

1.  <span data-ttu-id="a8ccf-197">追加、*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-197">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="a8ccf-198">次の `WebsiteContext` クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-198">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="a8ccf-199">次の追加`WebsiteInformationTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-199">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="a8ccf-200">**注:**</span><span class="sxs-lookup"><span data-stu-id="a8ccf-200">**Notes:**</span></span>
    
    * <span data-ttu-id="a8ccf-201">前述のように、タグ ヘルパーにより、c# を pascal クラス名とプロパティのでにタグ ヘルパー [kebab ケースを下げる](http://wiki.c2.com/?KebabCase)です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-201">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="a8ccf-202">そのため、使用する、 `WebsiteInformationTagHelper` Razor での記述`<website-information />`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-202">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="a8ccf-203">ターゲット要素に明示的に識別しない、`[HtmlTargetElement]`属性、そのため、既定の`website-information`の対象となります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-203">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="a8ccf-204">場合は、次の属性 (kebab ケースではありませんが、クラス名と一致することに注意) が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-204">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="a8ccf-205">低い kebab ケース タグ`<website-information />`は一致しません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-205">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="a8ccf-206">使用する場合、`[HtmlTargetElement]`属性、kebab ケース次に示すようを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-206">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="a8ccf-207">自己終了要素のコンテンツがあるありません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-207">Elements that are self-closing have no content.</span></span> <span data-ttu-id="a8ccf-208">この例では、Razor マークアップでは、自己終了タグですがタグ ヘルパーの作成は、[セクション](http://www.w3.org/TR/html5/sections.html#the-section-element)要素 (自己終了ではないと、対応するには、内のコンテンツを記述する、`section`要素)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-208">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="a8ccf-209">そのため、設定する必要があります`TagMode`に`StartTagAndEndTag`出力に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-209">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="a8ccf-210">行の設定をコメントする代わりに、`TagMode`と終了タグのマークアップを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-210">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="a8ccf-211">(例マークアップがこのチュートリアルの後半で提供されています。)</span><span class="sxs-lookup"><span data-stu-id="a8ccf-211">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="a8ccf-212">`$` (ドル記号) で、次の行を使用して、[補間文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="a8ccf-212">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="a8ccf-213">次のマークアップを追加、 *About.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-213">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="a8ccf-214">強調表示されたマークアップでは、web サイトの情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-214">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="a8ccf-215">次に示す Razor マークアップ。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-215">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="a8ccf-216">Razor、`info`属性は、文字列ではなく、クラスと c# コードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-216">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="a8ccf-217">しなくても、任意の文字列ではないタグ ヘルパー属性を記述する必要があります、`@`文字です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-217">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="a8ccf-218">アプリを実行し、バージョン情報を表示する web サイトの情報に移動します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-218">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="a8ccf-219">終了タグで、次のマークアップを使用してでは、行を削除する`TagMode.StartTagAndEndTag`タグ ヘルパーで。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-219">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="a8ccf-220">タグ ヘルパーを条件します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-220">Condition Tag Helper</span></span>

<span data-ttu-id="a8ccf-221">条件タグ ヘルパーは、true 値を渡されるときに出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-221">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="a8ccf-222">次の追加`ConditionTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-222">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="a8ccf-223">内容を置き換える、 *Views/Home/Index.cshtml*次のマークアップ ファイル。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-223">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="a8ccf-224">置換、`Index`メソッドで、`Home`コント ローラーを次のコード。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-224">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="a8ccf-225">アプリを実行して、ホーム ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-225">Run the app and browse to the home page.</span></span> <span data-ttu-id="a8ccf-226">条件付きでマークアップ`div`はレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-226">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="a8ccf-227">クエリ文字列の追加`?approved=true`URL に (たとえば、 `http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-227">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="a8ccf-228">`approved`true に設定され、条件付きマークアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-228">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="a8ccf-229">使用して、 [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子を太字タグ ヘルパーで行ったように文字列を指定するのではなく、対象の属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-229">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="a8ccf-230">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子は、コードを保護する必要があります、これまでリファクタリングを行うこと (名前を変更する可能性があります`RedCondition`)。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-230">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="a8ccf-231">タグ ヘルパーの競合を回避します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-231">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="a8ccf-232">このセクションでは、自動リンク タグ ヘルパーのペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-232">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="a8ccf-233">最初は、マークアップを含む、HTML アンカー タグ同じ URL を含む (および、URL へのリンクを生成するため) に HTTP で始まる URL で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-233">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="a8ccf-234">2 つ目は同じ操作を行いますは URL の www を開始します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-234">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="a8ccf-235">これら 2 つのヘルパーが密接に関連し、それらを今後リファクターする場合があります、ためにではこのままに同じファイルにします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-235">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="a8ccf-236">次の追加`AutoLinkerHttpTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-236">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="a8ccf-237">`AutoLinkerHttpTagHelper`クラス ターゲット`p`要素および使用[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)アンカーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-237">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="a8ccf-238">末尾に次のマークアップを追加、 *Views/Home/Contact.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-238">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="a8ccf-239">アプリケーションを実行し、タグ ヘルパー アンカーが正しく表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-239">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="a8ccf-240">更新プログラム、`AutoLinker`クラスに含める、 `AutoLinkerWwwTagHelper` www テキスト www の元のテキストが含まれているアンカー タグに変換します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-240">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="a8ccf-241">更新されたコードは、以下が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-241">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="a8ccf-242">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-242">Run the app.</span></span> <span data-ttu-id="a8ccf-243">通知 www テキストがリンクとしてレンダリングされますが、HTTP テキストではありません。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-243">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="a8ccf-244">両方のクラスでブレークポイントを配置した場合は、HTTP タグ ヘルパー クラスが最初に実行されることが確認できます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-244">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="a8ccf-245">問題には、タグ ヘルパーの出力がキャッシュされると、HTTP タグ ヘルパーからキャッシュされた出力 WWW タグ ヘルパーの実行時に上書きされます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-245">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="a8ccf-246">このチュートリアルで後ほどおでタグ ヘルパーが実行される順序を制御する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-246">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="a8ccf-247">コードは、次のように修正されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-247">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="a8ccf-248">Edition では、まず自動リンク タグ ヘルパーの次のコードで、ターゲットのコンテンツを取得しました。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-248">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="a8ccf-249">つまり、呼び出す`GetChildContentAsync`を使用して、`TagHelperOutput`に渡される、`ProcessAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-249">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="a8ccf-250">説明したように以前は、出力がキャッシュされるため、最後にタグ付け wins を実行するためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-250">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="a8ccf-251">次のコードに問題が修正されました。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-251">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="a8ccf-252">上記のコードは、コンテンツが変更されており場合は、出力バッファーからコンテンツを取得するかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-252">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="a8ccf-253">アプリを実行して、2 つのリンクが期待どおりに動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-253">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="a8ccf-254">この自動リンカー タグ ヘルパーが適切な表示される、中に微妙な問題があります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-254">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="a8ccf-255">WWW タグ ヘルパーが実行される場合、最初 www へのリンクは不正確になります。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-255">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="a8ccf-256">追加することで、コードを更新、`Order`オーバー ロードで、タグが実行される順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-256">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="a8ccf-257">`Order`プロパティが同じ要素を対象とするその他のタグ ヘルパーの基準とした実行順序を決定します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-257">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="a8ccf-258">既定の順序の値は 0 と低い値を持つインスタンスが最初に実行されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-258">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="a8ccf-259">上記のコードは、WWW タグ ヘルパーの前に、HTTP タグ ヘルパーが実行される保証されます。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-259">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="a8ccf-260">変更`Order`に`MaxValue`WWW タグの生成されたマークアップが正しいことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-260">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="a8ccf-261">検査し、子コンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-261">Inspect and retrieve child content</span></span>

<span data-ttu-id="a8ccf-262">タグ ヘルパーは、コンテンツを取得するいくつかのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-262">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="a8ccf-263">結果`GetChildContentAsync`に追加できます`output.Content`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-263">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="a8ccf-264">結果を調査して`GetChildContentAsync`で`GetContent`です。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-264">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="a8ccf-265">変更した場合`output.Content`、TagHelper 本体は実行またはない限り、表示されません`GetChildContentAsync`自動リンカー サンプルのようにします。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-265">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="a8ccf-266">複数回呼び出す`GetChildContentAsync`同じ値を返すし、再実行されません、`TagHelper`本文のキャッシュされた結果を使用しないことを示す場合は false にパラメーターを渡す場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="a8ccf-266">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating not use the cached result.</span></span>