---
title: "ASP.NET MVC を持つコアを EF Core - データ モデルの 10 の 5"
author: tdykstra
description: "このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 5b5645936504333573950b5bd17f5a037ffd984f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="c2e3c-103">EF コア ASP.NET Core MVC のチュートリアル (10 の 5) に、複雑なデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-103">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="c2e3c-104">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2e3c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2e3c-105">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="c2e3c-106">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="c2e3c-107">前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルを使用していた。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="c2e3c-108">このチュートリアルでは複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="c2e3c-109">完了したら、エンティティ クラスは、次の図に示す完成したデータ モデルを構成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="c2e3c-111">属性を使用して、データ モデルをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="c2e3c-112">このセクションでは、書式設定、検証、およびデータベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="c2e3c-113">作成する次のセクションでのいくつか追加することによって完全な学校のデータ モデル属性をクラスは、既に作成して、モデル内の残りのエンティティ型の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="c2e3c-114">データ型の属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-114">The DataType attribute</span></span>

<span data-ttu-id="c2e3c-115">学生の登録日のすべての web ページ現在表示、日付と時刻が、このフィールドの関心のあるは、日付。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="c2e3c-116">データの注釈属性を使用することができますいずれかのコード変更データを表示するすべてのビューの表示形式を修正します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="c2e3c-117">属性を追加することを実行する方法の例を参照する、`EnrollmentDate`プロパティに、`Student`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="c2e3c-118">*Models/Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティ、次の例で示すように。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="c2e3c-119">`DataType` 属性は、データベースの組み込み型よりも具体的なデータ型を指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-119">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="c2e3c-120">ここでのみが必要を追跡する、日付、日付と時刻がありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="c2e3c-121">`DataType`日付、時刻、PhoneNumber、通貨、EmailAddress などの多くのデータ型の列挙体を提供します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="c2e3c-122">また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="c2e3c-123">たとえば、`mailto:` リンクを `DataType.EmailAddress` に作成したり、HTML5 をサポートするブラウザーで `DataType.Date` に日付セレクターを提供したりできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="c2e3c-124">`DataType`属性は、HTML 5 を出力`data-`HTML 5 ブラウザーを理解することができます (データと読みます dash) の属性です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="c2e3c-125">`DataType`属性は、いずれかの検証を渡さないようにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-125">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="c2e3c-126">`DataType.Date` は、表示される日付の書式を指定しません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-126">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="c2e3c-127">既定では、サーバーの CultureInfo に基づく既定の形式に従ってデータ フィールドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="c2e3c-128">`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="c2e3c-129">`ApplyFormatInEditMode` の設定では、編集用にテキスト ボックスに値を表示するときにも適用する必要がある書式設定を指定します </span><span class="sxs-lookup"><span data-stu-id="c2e3c-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="c2e3c-130">(たくない一部のフィールドなどの通貨値の可能性がありますしないこと、テキスト ボックス内の通貨記号を編集するためです。)</span><span class="sxs-lookup"><span data-stu-id="c2e3c-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="c2e3c-131">使用することができます、`DisplayFormat`自体が、属性が使用することをお勧めでは一般に、`DataType`も属性します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="c2e3c-132">`DataType`属性を画面に表示する方法ではなく、データのセマンティクスを伝達し、次の利点を得られないことを示します`DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="c2e3c-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="c2e3c-133">ブラウザーが HTML5 機能を有効にすることができます (たとえば、予定表コントロールでは、ロケールに応じた通貨記号、電子メールへのリンクを表示する、いくつかのクライアント側入力検証などです。)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="c2e3c-134">ブラウザーの既定では、ロケールに基づいて正しい書式を使ってデータがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="c2e3c-135">詳細については、次を参照してください。、 [\<入力 > タグ ヘルパーのドキュメント](../../mvc/views/working-with-forms.md#the-input-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="c2e3c-136">アプリを実行する、受講者インデックス ページに移動し、登録の日付の時刻が表示されていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="c2e3c-137">学生のモデルを使用する任意のビューの場合は true。 同じになります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-137">The same will be true for any view that uses the Student model.</span></span>

![受講者時刻なしの日付が表示されたページをインデックスします。](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="c2e3c-139">StringLength 属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-139">The StringLength attribute</span></span>

<span data-ttu-id="c2e3c-140">データ検証ルールおよび属性を使用して検証エラー メッセージを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="c2e3c-141">`StringLength`属性は、データベースの最大長を設定し、クライアント側とサーバー側の提供 ASP.NET MVC の検証。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="c2e3c-142">この属性には、文字列の長さを指定することも、最小値データベース スキーマに影響がありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="c2e3c-143">ユーザーが名前の 50 個を超える文字を入力しないことを確認するとします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="c2e3c-144">この制限を追加する追加`StringLength`属性を`LastName`と`FirstMidName`プロパティ、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="c2e3c-145">`StringLength`属性は、名前に空白文字を入力するからユーザーを妨げることはありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="c2e3c-146">使用することができます、`RegularExpression`入力に制限を適用する属性。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="c2e3c-147">たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="c2e3c-148">`MaxLength`属性と同様の機能を提供する、`StringLength`属性が、クライアント側では提供しません検証します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="c2e3c-149">データベース モデルがデータベース スキーマの変更を必要とするように変更されました。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="c2e3c-150">追加したデータベースにアプリケーション UI を使用してデータを失うことがなく、スキーマを更新するのにには移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="c2e3c-151">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-151">Save your changes and build the project.</span></span> <span data-ttu-id="c2e3c-152">プロジェクト フォルダー内のコマンド ウィンドウを開くし、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="c2e3c-153">`migrations add`コマンドをデータ損失が発生する可能性があります、変更は、最大の長さの 2 つの列より短いために警告します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="c2e3c-154">移行という名前のファイルを作成する*\<タイムスタンプ > _MaxLengthOnNames.cs*です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="c2e3c-155">このファイルには内のコードが含まれています、`Up`メソッドを現在のデータ モデルと一致するデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="c2e3c-156">`database update`コマンドは、そのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="c2e3c-157">移行ファイル名にプレフィックスのタイムスタンプは、Entity Framework によって、移行を並べ替えに使用されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="c2e3c-158">データベースの更新コマンドを実行する前に複数の移行を作成することができ、移行のすべてが作成された順序で適用されます、します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="c2e3c-159">アプリを実行する、選択、**受講者** タブで、をクリックして**新規作成**、50 文字以下のいずれかの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="c2e3c-160">クリックすると、**作成**クライアント側の検証エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-160">When you click **Create**, client side validation shows an error message.</span></span>

![学生のインデックス文字列の長さエラーを示すページ](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="c2e3c-162">列の属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-162">The Column attribute</span></span>

<span data-ttu-id="c2e3c-163">クラスとプロパティをデータベースにマップする方法を制御するのに属性を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="c2e3c-164">名前を使用するいたと仮定します`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="c2e3c-165">選択した名前を指定するデータベース列が、`FirstName`データベースに対してアドホック クエリを記述するユーザーがその名前に慣れているため、します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="c2e3c-166">このマッピングを作成するには、使用することができます、`Column`属性。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="c2e3c-167">`Column`属性を指定するデータベースが作成されるとき、列の`Student`をマップするテーブル、`FirstMidName`プロパティの名前は`FirstName`します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="c2e3c-168">つまり、ときに、コードを指す`Student.FirstMidName`、データから得られますまたはで更新される、`FirstName`の列、`Student`テーブル。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="c2e3c-169">列名を指定しない場合、プロパティ名と同じ名前が付与されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-169">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="c2e3c-170">*Student.cs*ファイルに追加し、`using`の声明`System.ComponentModel.DataAnnotations.Schema`に列名の属性を追加し、`FirstMidName`プロパティ、次の強調表示されたコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="c2e3c-171">追加、`Column`属性がサポートするモデルを変更、 `SchoolContext`、ので、データベースと一致しません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="c2e3c-172">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-172">Save your changes and build the project.</span></span> <span data-ttu-id="c2e3c-173">プロジェクト フォルダー内のコマンド ウィンドウを開くし、別の移行を作成する次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="c2e3c-174">**SQL Server オブジェクト エクスプ ローラー**をダブルクリックして、Student テーブル デザイナーを開き、**学生**テーブル。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![移行した後で SSOX students テーブル](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="c2e3c-176">最初の 2 つの移行を適用する前に、名前の列は、nvarchar (max) 型のでした。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="c2e3c-177">これは nvarchar (50) し、列名は、firstname FirstMidName から変更されました。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-177">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="c2e3c-178">次のセクションでのすべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="c2e3c-179">学生エンティティの最終変更</span><span class="sxs-lookup"><span data-stu-id="c2e3c-179">Final changes to the Student entity</span></span>

![学生エンティティ](complex-data-model/_static/student-entity.png)

<span data-ttu-id="c2e3c-181">*Models/Student.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="c2e3c-182">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-182">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="c2e3c-183">必須の属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-183">The Required attribute</span></span>

<span data-ttu-id="c2e3c-184">`Required`属性は、名前のプロパティの必須フィールドです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="c2e3c-185">`Required`値の型などの非 null 許容の型の属性は必要ありません (DateTime、int、double、float などです。)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-185">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="c2e3c-186">Null にすることはできません型は、必要なフィールドとして自動的に扱われます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="c2e3c-187">削除することで、`Required`属性し、の最小の長さのパラメーターに置き換えて、`StringLength`属性。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="c2e3c-188">表示属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-188">The Display attribute</span></span>

<span data-ttu-id="c2e3c-189">`Display`属性、テキスト ボックスのキャプションにするように指定「姓」、"Last Name"、「完全名」、およびプロパティ名の代わりに「登録日」(を単語に分割する領域を持つなし) 各インスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="c2e3c-190">計算 FullName プロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-190">The FullName calculated property</span></span>

<span data-ttu-id="c2e3c-191">`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="c2e3c-192">存在し、get アクセサーだけ、したがって`FullName`データベース内の列が生成されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="c2e3c-193">Instructor エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-193">Create the Instructor Entity</span></span>

![Instructor エンティティ](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="c2e3c-195">作成*Models/Instructor.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="c2e3c-196">いくつかのプロパティは、Student とインストラクター エンティティで同じことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="c2e3c-197">[実装の継承](inheritance.md)このシリーズの後でチュートリアルでは、冗長性を回避するのには、このコードをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="c2e3c-198">作成することも、1 行に複数の属性を配置することができます、`HireDate`次のように属性します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="c2e3c-199">CourseAssignments と OfficeAssignment ナビゲーションのプロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="c2e3c-200">`CourseAssignments`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="c2e3c-201">インストラクターは任意の数のコースを教えることができますので、`CourseAssignments`コレクションとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="c2e3c-202">ナビゲーション プロパティは、複数のエンティティを保持できる、その型が一覧にエントリを追加、削除すると、更新できるにあります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="c2e3c-203">指定できます`ICollection<T>`またはなど型`List<T>`または`HashSet<T>`です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="c2e3c-204">指定した場合`ICollection<T>`、EF の作成、`HashSet<T>`既定のコレクション。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="c2e3c-205">これらは、なぜ理由`CourseAssignment`エンティティが、多対多リレーションシップに関するセクションで後述します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="c2e3c-206">Contoso 大学のビジネス ルールの状態、インストラクターが最大で 1 つのオフィスを持つことができますのみため、`OfficeAssignment`プロパティは 1 つの OfficeAssignment エンティティ (を office が割り当てられていない場合は null にすることがあります) を保持します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="c2e3c-207">OfficeAssignment エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment エンティティ](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="c2e3c-209">作成*Models/OfficeAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="c2e3c-210">キー属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-210">The Key attribute</span></span>

<span data-ttu-id="c2e3c-211">リレーションシップがある 0 または 1 を 1 つ、インストラクターと OfficeAssignment エンティティの間です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="c2e3c-212">オフィス割り当てのみに、割り当てられているインストラクターに関連してあり、したがって、主キーも Instructor エンティティへの外部のキー。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="c2e3c-213">Entity Framework は、自動的に、その名前が ID または classnameID 名前付け規則に従っていないためこのエンティティの主キーとして InstructorID を認識できません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="c2e3c-214">したがって、`Key`属性は、キーとして識別するために使用します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="c2e3c-215">使用することも、`Key`エンティティが、独自のプライマリ キーを持っていて classnameID または ID 以外何か、プロパティの名前を指定の属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="c2e3c-216">既定では、リレーションシップの列があるために、EF はデータベースによって生成された非としてキーを扱います。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="c2e3c-217">講師ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-217">The Instructor navigation property</span></span>

<span data-ttu-id="c2e3c-218">Instructor エンティティに null 許容`OfficeAssignment`ナビゲーション プロパティ (インストラクター オフィス割り当てがあるない) ため、OfficeAssignment エンティティが、null 非許容と`Instructor`ナビゲーション プロパティ (ため、オフィス割り当てできません講師--がないと存在`InstructorID`が null 非許容の)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="c2e3c-219">Instructor エンティティに関連する OfficeAssignment エンティティがある場合、各エンティティは、ナビゲーション プロパティで、もう 1 つへの参照があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="c2e3c-220">配置すること、 `[Required]` 、インストラクター ナビゲーション プロパティに関連のインストラクターである必要がありますがこれを行うためがないことを指定する属性、 `InstructorID` (これは、このテーブルへのキーではも) 外部キーが null 非許容されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="c2e3c-221">Course エンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-221">Modify the Course Entity</span></span>

![Course エンティティ](complex-data-model/_static/course-entity.png)

<span data-ttu-id="c2e3c-223">*Models/Course.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="c2e3c-224">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-224">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="c2e3c-225">Course エンティティは、外部キーのプロパティを持つ`DepartmentID`が、関連する部門エンティティとそれを`Department`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="c2e3c-226">Entity Framework は、関連エンティティのナビゲーション プロパティがある場合、データ モデルに外部キーのプロパティを追加する必要ありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="c2e3c-227">EF は自動的には、必要に応じて、データベース内の外部キーを作成し、作成[プロパティをシャドウ](https://docs.microsoft.com/ef/core/modeling/shadow-properties)にします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-227">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="c2e3c-228">簡素化されより効率的な更新プログラムを必ずデータ モデルの外部キーを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="c2e3c-229">たとえば、コースを編集するエンティティをフェッチするときに、Department エンティティが null、読み込まない場合ので、course エンティティを更新するときに必要があります、Department エンティティを最初に取得します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="c2e3c-230">ときに外部キー プロパティ`DepartmentID`が含まれるデータ モデルで更新する前に、Department エンティティをフェッチする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="c2e3c-231">DatabaseGenerated 属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="c2e3c-232">`DatabaseGenerated`属性が、`None`上のパラメーター、`CourseID`プロパティは、主キーの値は、ユーザーによって提供されるではなくデータベースによって生成されたを指定します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="c2e3c-233">既定では、Entity Framework は、主キーの値がデータベースによって生成されると仮定します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="c2e3c-234">ほとんどのシナリオに必要な要素です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="c2e3c-235">ただし、コースのエンティティを 1 つの部門別の部門の 2000年シリーズの一連の 1000 ようコースのユーザーが指定した番号を使用し、します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="c2e3c-236">`DatabaseGenerated`属性を使用して日付を記録するために使用するデータベース列の場合、行の作成または更新されるように、既定値を生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="c2e3c-237">詳細については、次を参照してください。[生成プロパティ](https://docs.microsoft.com/ef/core/modeling/generated-properties)です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="c2e3c-238">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="c2e3c-239">外部キーのプロパティと、Course エンティティでのナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="c2e3c-240">コースを 1 つの部門に割り当てられるがあるため、`DepartmentID`外部キーと`Department`上記の理由によりナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="c2e3c-241">コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="c2e3c-242">コースに複数のインストラクターを教育する可能性がありますので、`CourseAssignments`ナビゲーション プロパティがコレクション (型`CourseAssignment`については説明[後](#many-to-many-relationships))。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="c2e3c-243">部門 エンティティを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-243">Create the Department entity</span></span>

![部門 エンティティ](complex-data-model/_static/department-entity.png)


<span data-ttu-id="c2e3c-245">作成*Models/Department.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="c2e3c-246">列の属性</span><span class="sxs-lookup"><span data-stu-id="c2e3c-246">The Column attribute</span></span>

<span data-ttu-id="c2e3c-247">以前を使用して、`Column`列名のマッピングを変更する属性。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="c2e3c-248">部門 エンティティのコードで、 `Column` SQL データ型マッピングの列を定義するときは、データベースで SQL Server の money 型の使用ができるように変更する属性が使用されています。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="c2e3c-249">列マッピングは一般的に必要ですが、Entity Framework のプロパティを定義する CLR 型に基づく適切な SQL Server データ型の選択されるためです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="c2e3c-250">CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="c2e3c-251">ここでは、列に通貨の値が保持していると、money データ型は方が適していることを知る。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="c2e3c-252">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="c2e3c-253">外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="c2e3c-254">部門、管理者がない可能性があり、管理者は、常に、インストラクター。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="c2e3c-255">したがって、`InstructorID`プロパティが Instructor エンティティへの外部キーとして含まれると、後に疑問符 () が追加、`int`タイプの null 値許容とプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="c2e3c-256">ナビゲーション プロパティの名前が`Administrator`Instructor エンティティを保持しているが、します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="c2e3c-257">部門は、コースのナビゲーション プロパティがあるために、複数のコースにすることがあります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="c2e3c-258">慣例により、Entity Framework null 非許容の外部キーおよび多対多リレーションシップの連鎖削除を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="c2e3c-259">これは、結果、循環 cascade delete ルールは、移行を追加しようとすると、例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="c2e3c-260">たとえば、Department.InstructorID プロパティは、null 値許容と定義されていないのに、EF よう、部門ではないどのような動作を削除すると、インストラクターを削除する連鎖削除規則に構成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="c2e3c-261">ビジネス ルールが必要な場合、`InstructorID`プロパティを null 非許容、ステートメントを使用して、次 fluent API をリレーションシップで連鎖削除を無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="c2e3c-262">登録のエンティティを変更します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-262">Modify the Enrollment entity</span></span>

![登録エンティティ](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="c2e3c-264">*Models/Enrollment.cs*、次のコードで以前に追加したコードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="c2e3c-265">外部キー プロパティとナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="c2e3c-266">外部キーのプロパティとナビゲーション プロパティは、次のリレーションシップを反映します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="c2e3c-267">登録レコードが、1 つのコースがあるため、`CourseID`外部キーのプロパティと`Course`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="c2e3c-268">登録レコードが 1 つの受講者には、`StudentID`外部キーのプロパティと`Student`ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="c2e3c-269">多対多リレーションシップ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="c2e3c-270">Student とコースのエンティティ間に多対多のリレーションシップがあるし、登録エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="c2e3c-271">"ペイロード"で、Enrollment テーブルに (この場合は、主キーおよびグレード プロパティ) 内の結合テーブルの外部キーだけでなく、追加のデータが含まれているを意味します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="c2e3c-272">次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="c2e3c-273">(この図は、EF の Entity Framework のパワー ツールを使用して生成された 6.x 以外の場合は、チュートリアルの一部ではない、ダイアグラムを作成、使用されているだけを例としては、ここです)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![受講者コース多対多の関係](complex-data-model/_static/student-course.png)

<span data-ttu-id="c2e3c-275">各リレーションシップの線は、もう一方の端、一対多のリレーションシップを示す一方の端と、アスタリスク (\*) で 1 がします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="c2e3c-276">場合は、Enrollment テーブルには、評価情報が含まれていなかった、CourseID と学生 Id の 2 つの外部キーを保持することはのみです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="c2e3c-277">その場合は、なりますペイロードせず、多対多の結合テーブル (または純粋な結合テーブル)、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="c2e3c-278">インストラクターとコース エンティティはそのような多対多リレーションシップを持ち、ペイロードしないテーブルの結合として機能するエンティティ クラスを作成するのには、次の手順。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="c2e3c-279">(EF 6.x サポートされていますが、多対多のリレーションシップ、EF コアの暗黙の結合テーブルはありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="c2e3c-280">詳細については、次を参照してください、 [EF コア GitHub リポジトリにディスカッション](https://github.com/aspnet/EntityFramework/issues/1368)。)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="c2e3c-281">CourseAssignment エンティティ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-281">The CourseAssignment entity</span></span>

![CourseAssignment エンティティ](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="c2e3c-283">作成*Models/CourseAssignment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="c2e3c-284">エンティティ名を参加させる</span><span class="sxs-lookup"><span data-stu-id="c2e3c-284">Join entity names</span></span>

<span data-ttu-id="c2e3c-285">講師-コース多対多リレーションシップのデータベースに必要な結合テーブルを保持しているエンティティ セットで表されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="c2e3c-286">結合のエンティティの名前を が一般的`EntityName1EntityName2`、ここではある`CourseInstructor`です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="c2e3c-287">ただし、リレーションシップを説明する名前を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="c2e3c-288">データ モデルでは、簡単な起動し、拡大、いいえペイロード結合を頻繁にペイロードを後で取得します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="c2e3c-289">エンティティのわかりやすい名前で開始する場合は、後で、名前を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="c2e3c-290">理想的には、結合エンティティは、ビジネス ドメイン内、自身の自然な (場合によって 1 つの単語) 名があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="c2e3c-291">たとえば、ブックと顧客は、評価を通じてリンクでした。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="c2e3c-292">このリレーションシップの`CourseAssignment`よりの方が適しています`CourseInstructor`です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="c2e3c-293">複合キー</span><span class="sxs-lookup"><span data-stu-id="c2e3c-293">Composite key</span></span>

<span data-ttu-id="c2e3c-294">外部キーがない null 許容、まとめて一意にテーブルの各行を識別する、独立したプライマリ キーの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="c2e3c-295">*InstructorID*と*CourseID*プロパティは複合主キーとして機能する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="c2e3c-296">EF に複合主キーを識別する唯一の方法を使用して、 *fluent API* (属性を使用して、そのことはできません)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="c2e3c-297">次のセクションで、複合主キーを構成する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="c2e3c-298">複合キーにより、複数の行、1 つのコースおよび講師が 1 つに対して複数の行を保持できますが、することは いる同じインストラクターとコースに複数の行。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="c2e3c-299">`Enrollment`結合エンティティは、この種の重複も有効であるために自身のプライマリ キーを定義します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="c2e3c-300">このような重複を防ぐためには、外部のキー フィールドで一意のインデックスを追加または構成することが`Enrollment`に複合主キーのような`CourseAssignment`します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="c2e3c-301">詳細については、次を参照してください。[インデックス](https://docs.microsoft.com/ef/core/modeling/indexes)です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="c2e3c-302">データベース コンテキストを更新します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-302">Update the database context</span></span>

<span data-ttu-id="c2e3c-303">次の強調表示されたコードを追加、 *Data/SchoolContext.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="c2e3c-304">このコードは、新しいエンティティを追加し、CourseAssignment エンティティの複合主キーを構成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="c2e3c-305">属性に fluent API の代替手段</span><span class="sxs-lookup"><span data-stu-id="c2e3c-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="c2e3c-306">内のコード、`OnModelCreating`のメソッド、`DbContext`クラスの使用、 *fluent API* EF の動作を構成します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="c2e3c-307">API が呼び出されます"fluent"からこの例のように、1 つのステートメントには一連のメソッド呼び出しをまとめてを組み合わせるで多くの場合、使用されているため、 [EF の主要なマニュアル](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="c2e3c-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="c2e3c-308">このチュートリアルで使用している fluent API のみデータベース マッピング属性を使用して行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="c2e3c-309">ただし、ほとんどの書式、検証、および属性を使用して行うことができますマッピング規則を指定するのに fluent API を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="c2e3c-310">などのいくつかの属性`MinimumLength`fluent API では適用できません。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="c2e3c-311">前に述べたよう`MinimumLength`スキーマを変更しないクライアントとサーバー側の検証規則のみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="c2e3c-312">"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者</span><span class="sxs-lookup"><span data-stu-id="c2e3c-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="c2e3c-313">をしたいが存在している fluent API を使用してのみ実行できるいくつかのカスタマイズ属性および fluent API が混在することができますが、一般に推奨これら 2 つの方法のいずれかを選択し、使用可能な一貫している限りです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="c2e3c-314">両方を使用して行う場合は、競合がある場所に Fluent API が属性をオーバーライドする注意してください。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-314">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="c2e3c-315">Fluent API と属性の詳細については、次を参照してください。[構成の方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="c2e3c-316">エンティティの図の関係を示す</span><span class="sxs-lookup"><span data-stu-id="c2e3c-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="c2e3c-317">次の図は、Entity Framework のパワー ツールが、完成した School モデルを作成するダイアグラムを示します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![エンティティの図](complex-data-model/_static/diagram.png)

<span data-ttu-id="c2e3c-319">一対多リレーションシップの線以外 (1 ~ \*)、インストラクターと OfficeAssignment エンティティと 0-または-1-対多リレーションシップの線の間の 0 または 1 を 1 つのリレーションシップの線 (1 対 0..1) をここで確認できます (0..1 対 *) の間で、インストラクターと Department エンティティ。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="c2e3c-320">データベースにテスト データをシードします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="c2e3c-321">コードで置き換え、 *Data/DbInitializer.cs*を作成した新しいエンティティのシードにデータを提供するために次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="c2e3c-322">最初のチュートリアルで説明したとおり、このコードのほとんどは単に新しいエンティティ オブジェクトを作成し、プロパティをテストするために必要に応じてにサンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="c2e3c-323">多対多のリレーションシップの処理方法に注意してください: コード内のエンティティを作成することでリレーションシップを作成する、`Enrollments`と`CourseAssignment`エンティティ セットに参加します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="c2e3c-324">移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-324">Add a migration</span></span>

<span data-ttu-id="c2e3c-325">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-325">Save your changes and build the project.</span></span> <span data-ttu-id="c2e3c-326">プロジェクト フォルダー内のコマンド ウィンドウを開き、入力、`migrations add`コマンド (しない update database コマンドが、まだ)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="c2e3c-327">データ損失の可能性に関する警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="c2e3c-328">実行しようとする場合、`database update`コマンドの時点で (これを行わないまだ) 次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="c2e3c-329">"FK_dbo FOREIGN KEY 制約と競合して ALTER TABLE ステートメント。Course_dbo です。Department_DepartmentID"です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="c2e3c-330">競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部門"、列 'DepartmentID' です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="c2e3c-331">既存のデータと移行を実行するときに、外部キー制約を満たすためにデータベースにスタブ データを挿入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="c2e3c-332">生成されたコードに、`Up`メソッドは、Course テーブルに null 非許容 DepartmentID 外部キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="c2e3c-333">既にある場合の行 Course テーブルに、コードを実行すると、 `AddColumn` SQL Server が null にすることはできません、列内の値を認識していないために、操作が失敗します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="c2e3c-334">このチュートリアルでは、新しいデータベースでして移行を実行しますが、実稼働アプリケーションでは、既存のデータを処理する次の手順を実行する方法の例を表示するため、移行を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="c2e3c-335">この移行は、新しい列に、既定値を提供するコードを変更する必要がある既存のデータを操作および明細部分を作成するには、既定の部門として機能するには、"Temp"という名前です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="c2e3c-336">その結果、既存のコース行はすべてに関連する後に"Temp"部門、`Up`メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="c2e3c-337">開く、 *{timestamp}_ComplexDataModel.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="c2e3c-338">Course テーブルに、[DepartmentID] 列を追加するコードの行をコメントにします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="c2e3c-339">Department テーブルを作成するコードの後に、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="c2e3c-340">実稼働アプリケーションでは、コードまたは部門の行を追加して、コースの行を新しい部門行に関連するスクリプトを記述するとします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="c2e3c-341">不要になった"Temp"部門または Course.DepartmentID 列での既定値です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="c2e3c-342">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="c2e3c-343">接続文字列を変更し、データベースを更新</span><span class="sxs-lookup"><span data-stu-id="c2e3c-343">Change the connection string and update the database</span></span>

<span data-ttu-id="c2e3c-344">今すぐに新しいコードがある、`DbInitializer`シード エンティティのデータを新しいを空のデータベースに追加するクラスです。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="c2e3c-345">EF 新しい空のデータベースを作成するために、内の接続文字列でデータベースの名前を変更する*される appsettings.json* ContosoUniversity3 を使用しているコンピューターで使用していない別の名前。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="c2e3c-346">変更を保存*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e3c-347">データベース名を変更する代わりに、データベースを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="c2e3c-348">使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="c2e3c-349">データベース名を変更または、データベースを削除した後、実行、`database update`コマンドをコマンド ウィンドウで、移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="c2e3c-350">アプリケーションを実行、`DbInitializer.Initialize`メソッドを実行し、新しいデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="c2e3c-351">以前に行ったよう、SSOX でデータベースを開き、展開、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="c2e3c-352">(以前の状態から開く SSOX まだの場合にをクリックして、**更新**ボタンをクリックします)。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![SSOX 内のテーブル](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="c2e3c-354">データベースのシードを設定する初期化子のコードをトリガーするアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="c2e3c-355">右クリックし、 **CourseAssignment**テーブルを選択して**ビュー データ**にデータが入っていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![SSOX で CourseAssignment データ](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="c2e3c-357">まとめ</span><span class="sxs-lookup"><span data-stu-id="c2e3c-357">Summary</span></span>

<span data-ttu-id="c2e3c-358">複雑なデータ モデルと対応するデータベースがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="c2e3c-359">次のチュートリアルでは、学びます関連のデータにアクセスする方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="c2e3c-359">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2e3c-360">[前へ](migrations.md)
[次へ](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="c2e3c-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  