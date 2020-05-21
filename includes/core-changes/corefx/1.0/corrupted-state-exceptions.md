---
ms.openlocfilehash: 394f2daebad7b6af94bee4d7876796e87fe8ab19
ms.sourcegitcommit: 8b02d42f93adda304246a47f49f6449fc74a3af4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2020
ms.locfileid: "82135636"
---
### <a name="handling-corrupted-state-exceptions-is-not-supported"></a><span data-ttu-id="f17a9-101">No se admite el control de excepciones de estado dañado</span><span class="sxs-lookup"><span data-stu-id="f17a9-101">Handling corrupted state exceptions is not supported</span></span>

<span data-ttu-id="f17a9-102">No se admite el control de excepciones de estado de proceso dañado en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f17a9-102">Handling corrupted-process-state exceptions in .NET Core is not supported.</span></span>

#### <a name="change-description"></a><span data-ttu-id="f17a9-103">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="f17a9-103">Change description</span></span>

<span data-ttu-id="f17a9-104">Anteriormente, las excepciones de estado de proceso dañado podían detectarse y controlarse mediante controladores de excepciones de código administrado, por ejemplo, al usar una instrucción [try-catch](../../../../docs/csharp/language-reference/keywords/try-catch.md) en C#.</span><span class="sxs-lookup"><span data-stu-id="f17a9-104">Previously, corrupted-process-state exceptions could be caught and handled by managed code exception handlers, for example, by using a [try-catch](../../../../docs/csharp/language-reference/keywords/try-catch.md) statement in C#.</span></span>

<span data-ttu-id="f17a9-105">A partir de .NET Core 1.0, las excepciones de estado de proceso dañado no se pueden controlar mediante código administrado.</span><span class="sxs-lookup"><span data-stu-id="f17a9-105">Starting in .NET Core 1.0, corrupted-process-state exceptions cannot be handled by managed code.</span></span> <span data-ttu-id="f17a9-106">Common Language Runtime no entrega las excepciones de estado de proceso dañado al código administrado.</span><span class="sxs-lookup"><span data-stu-id="f17a9-106">The common language runtime doesn't deliver corrupted-process-state exceptions to managed code.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="f17a9-107">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="f17a9-107">Version introduced</span></span>

<span data-ttu-id="f17a9-108">1.0</span><span class="sxs-lookup"><span data-stu-id="f17a9-108">1.0</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="f17a9-109">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="f17a9-109">Recommended action</span></span>

<span data-ttu-id="f17a9-110">Para no tener que controlar las excepciones de estado de proceso dañado, aborde las situaciones que conducen a ellas.</span><span class="sxs-lookup"><span data-stu-id="f17a9-110">Avoid the need to handle corrupted-process-state exceptions by addressing the situations that lead to them instead.</span></span> <span data-ttu-id="f17a9-111">Si es absolutamente necesario controlar las excepciones de estado de proceso dañado, escriba el controlador de excepciones en código de C o C++.</span><span class="sxs-lookup"><span data-stu-id="f17a9-111">If it's absolutely necessary to handle corrupted-process-state exceptions, write the exception handler in C or C++ code.</span></span>

#### <a name="category"></a><span data-ttu-id="f17a9-112">Categoría</span><span class="sxs-lookup"><span data-stu-id="f17a9-112">Category</span></span>

<span data-ttu-id="f17a9-113">Bibliotecas de Core .NET</span><span class="sxs-lookup"><span data-stu-id="f17a9-113">Core .NET libraries</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="f17a9-114">API afectadas</span><span class="sxs-lookup"><span data-stu-id="f17a9-114">Affected APIs</span></span>

- <xref:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute?displayProperty=fullName>
- [<span data-ttu-id="f17a9-115">Elemento legacyCorruptedStateExceptionsPolicy</span><span class="sxs-lookup"><span data-stu-id="f17a9-115">legacyCorruptedStateExceptionsPolicy element</span></span>](~/docs/framework/configure-apps/file-schema/runtime/legacycorruptedstateexceptionspolicy-element.md)

<!--

#### Affected APIs

- `T:System.Runtime.ExceptionServices.HandleProcessCorruptedStateExceptionsAttribute`

-->