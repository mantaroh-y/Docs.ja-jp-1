---
title: "目的の文字列"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護 Api で目的の文字列を使用する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b1e95c9d0aa8195aa73fddfb97a4079e67a351bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-strings"></a><span data-ttu-id="42d7d-103">目的の文字列</span><span class="sxs-lookup"><span data-stu-id="42d7d-103">Purpose Strings</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="42d7d-104">コンポーネントから利用`IDataProtectionProvider`を一意に渡す必要があります*目的*パラメーターを`CreateProtector`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="42d7d-104">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="42d7d-105">目的*パラメーター*はように、暗号化のコンシューマー間の分離を提供するルートの暗号化キーが同じ場合でも、データ保護システムのセキュリティに固有です。</span><span class="sxs-lookup"><span data-stu-id="42d7d-105">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="42d7d-106">コンシューマーは、目的を示す、そのコンシューマーに一意の暗号のサブキーを派生するルートの暗号化キーと共に、目的の文字列が使用されます。</span><span class="sxs-lookup"><span data-stu-id="42d7d-106">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="42d7d-107">これにより、アプリケーションでその他のすべての暗号コンシューマーからコンシューマーを排除します。 その他のコンポーネントは、そのペイロードを読み取ることではありませんし、他のコンポーネントのペイロードを読み取ることができません。</span><span class="sxs-lookup"><span data-stu-id="42d7d-107">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="42d7d-108">この分離は、コンポーネントに対する攻撃の実行不可能な場合の全体カテゴリも表示します。</span><span class="sxs-lookup"><span data-stu-id="42d7d-108">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![目的の図の例](purpose-strings/_static/purposes.png)

<span data-ttu-id="42d7d-110">、上記の図で`IDataProtector`インスタンス A と B**できません**ペイロードを読み取る互いの、のみ独自です。</span><span class="sxs-lookup"><span data-stu-id="42d7d-110">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="42d7d-111">目的の文字列は、秘密にするがありません。</span><span class="sxs-lookup"><span data-stu-id="42d7d-111">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="42d7d-112">単に適切に動作の他のコンポーネントがこれまでしないことを目的の文字列と同じ意味で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="42d7d-112">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="42d7d-113">適切な原則として、この情報が競合することはありませんプラクティスと同様には、データ保護 Api を使用するコンポーネントの名前空間と型名を使用します。</span><span class="sxs-lookup"><span data-stu-id="42d7d-113">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="42d7d-114">Minting ベアラー トークンを担当する contoso 社製コンポーネントは、その目的の文字列として Contoso.Security.BearerToken を使用できます。</span><span class="sxs-lookup"><span data-stu-id="42d7d-114">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="42d7d-115">または、目的の文字列として Contoso.Security.BearerToken.v1 を - さらに、使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="42d7d-115">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="42d7d-116">バージョン番号を追加することにより、将来のバージョンを目的に応じて Contoso.Security.BearerToken.v2 を使用する、さまざまなバージョン互いから完全に分離ペイロード移動限りなります。</span><span class="sxs-lookup"><span data-stu-id="42d7d-116">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="42d7d-117">目的のパラメーターから`CreateProtector`文字列配列では、上記でした代わりとして指定されて`[ "Contoso.Security.BearerToken", "v1" ]`です。</span><span class="sxs-lookup"><span data-stu-id="42d7d-117">Since the purposes parameter to `CreateProtector` is a string array, the above could have been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="42d7d-118">これは、目的の階層を確立することができ、データ保護システムを使用するマルチ テナント シナリオの可能性を開きます。</span><span class="sxs-lookup"><span data-stu-id="42d7d-118">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="42d7d-119">コンポーネントは、目的のチェーンの入力の唯一のソースである信頼されていないユーザー入力を許可しません。</span><span class="sxs-lookup"><span data-stu-id="42d7d-119">Components should not allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="42d7d-120">たとえば、セキュリティで保護されたメッセージの格納を担当する Contoso.Messaging.SecureMessage コンポーネントを検討してください。</span><span class="sxs-lookup"><span data-stu-id="42d7d-120">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="42d7d-121">セキュリティで保護されたメッセージング コンポーネントを呼び出す場合`CreateProtector([ username ])`、悪意のあるユーザーを呼び出すコンポーネントを取得しようとするにユーザー名"Contoso.Security.BearerToken"を持つアカウントを作成する可能性があります、 `CreateProtector([ "Contoso.Security.BearerToken" ])`、誤ってしまい、セキュリティで保護されたメッセージング認証トークンとして認識でした言い換えるペイロードにシステムです。</span><span class="sxs-lookup"><span data-stu-id="42d7d-121">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="42d7d-122">メッセージング コンポーネントを向上させる目的でチェーンがなります`CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`、適切な分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="42d7d-122">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="42d7d-123">提供される分離との動作`IDataProtectionProvider`、 `IDataProtector`、その目的は次のとおり。</span><span class="sxs-lookup"><span data-stu-id="42d7d-123">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="42d7d-124">指定された`IDataProtectionProvider`オブジェクト、`CreateProtector`メソッドは、作成、`IDataProtector`両方にオブジェクトが一意に関連付けられている、`IDataProtectionProvider`メソッドに渡されたする目的でパラメーターを作成するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42d7d-124">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="42d7d-125">目的のパラメーターを null にするにはできません。</span><span class="sxs-lookup"><span data-stu-id="42d7d-125">The purpose parameter must not be null.</span></span> <span data-ttu-id="42d7d-126">(場合は、配列としての目的を指定すると、つまり長さ 0 の配列にはできません、配列のすべての要素が null にする必要があります。)空の文字列目的は、技術的に可能ですは避けることを推奨。</span><span class="sxs-lookup"><span data-stu-id="42d7d-126">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="42d7d-127">2 つの目的の引数は、同じ順序では、(序数の比較子を使用して)、同じ文字列が含まれている場合にのみに相当します。</span><span class="sxs-lookup"><span data-stu-id="42d7d-127">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="42d7d-128">1 つの目的の引数は、対応する 1 つの要素の目的で配列と同じです。</span><span class="sxs-lookup"><span data-stu-id="42d7d-128">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="42d7d-129">2 つ`IDataProtector`該当するショートカットからが作成される場合にのみ、オブジェクトが等しく`IDataProtectionProvider`と同等の目的でパラメーターを持つオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42d7d-129">Two `IDataProtector` objects are equivalent if and only if they are created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="42d7d-130">指定された`IDataProtector`オブジェクトへの呼び出し`Unprotect(protectedData)`元を返す`unprotectedData`場合にのみ`protectedData := Protect(unprotectedData)`同等の`IDataProtector`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42d7d-130">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="42d7d-131">いくつかのコンポーネントが意図的に別のコンポーネントと競合する呼ばれる目的の文字列を選択、ケース マイクロソフトいない検討しています。</span><span class="sxs-lookup"><span data-stu-id="42d7d-131">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="42d7d-132">このようなコンポーネントは基本的と見なされる悪意のあると、このシステムは、悪意のあるコードは、ワーカー プロセス内で既に実行されているセキュリティ保証を提供するものではありません。</span><span class="sxs-lookup"><span data-stu-id="42d7d-132">Such a component would essentially be considered malicious, and this system is not intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>