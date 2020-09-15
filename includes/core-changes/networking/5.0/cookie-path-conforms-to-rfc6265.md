---
ms.openlocfilehash: 7140f6d4cac063088b3663ab98d292104218b542
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89465523"
---
### <a name="cookie-path-handling-now-conforms-to-rfc-6265"></a><span data-ttu-id="a18b3-101">La administración de rutas de acceso de cookies ahora se ajusta a RFC 6265.</span><span class="sxs-lookup"><span data-stu-id="a18b3-101">Cookie Path handling now conforms to RFC 6265</span></span>

<span data-ttu-id="a18b3-102">Los algoritmos de control de rutas de acceso que se definen en [RFC 6265](https://tools.ietf.org/html/rfc6265) se implementaron para las clases <xref:System.Net.Cookie> y <xref:System.Net.CookieContainer>.</span><span class="sxs-lookup"><span data-stu-id="a18b3-102">Path-handling algorithms defined in [RFC 6265](https://tools.ietf.org/html/rfc6265) were implemented for the <xref:System.Net.Cookie> and <xref:System.Net.CookieContainer> classes.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="a18b3-103">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="a18b3-103">Version introduced</span></span>

<span data-ttu-id="a18b3-104">5.0</span><span class="sxs-lookup"><span data-stu-id="a18b3-104">5.0</span></span>

#### <a name="change-description"></a><span data-ttu-id="a18b3-105">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="a18b3-105">Change description</span></span>

- <span data-ttu-id="a18b3-106">Ya no es necesario que la propiedad <xref:System.Net.Cookie.Path> sea un prefijo de la ruta de acceso de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a18b3-106">The <xref:System.Net.Cookie.Path> property is no longer required to be a prefix of the request path.</span></span>
- <span data-ttu-id="a18b3-107">El cálculo del valor predeterminado de <xref:System.Net.Cookie.Path> y los algoritmos de coincidencia de ruta de acceso se implementaron tal y como se define en la [sección 5.1.4](https://tools.ietf.org/html/rfc6265#section-5.1.4) de RFC 6265.</span><span class="sxs-lookup"><span data-stu-id="a18b3-107">The calculation of the default value of <xref:System.Net.Cookie.Path> and path-matching algorithms were implemented as defined in [section 5.1.4](https://tools.ietf.org/html/rfc6265#section-5.1.4) of RFC 6265.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="a18b3-108">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="a18b3-108">Recommended action</span></span>

<span data-ttu-id="a18b3-109">En la mayoría de los casos, no tiene que realizar ninguna acción.</span><span class="sxs-lookup"><span data-stu-id="a18b3-109">In most cases, you won't need to take any action.</span></span> <span data-ttu-id="a18b3-110">Sin embargo, si el código dependía de la validación de <xref:System.Net.Cookie.Path>, deberá implementar la validación necesaria en el código.</span><span class="sxs-lookup"><span data-stu-id="a18b3-110">However, if your code was dependent on <xref:System.Net.Cookie.Path> validation, you'll need to implement required validation in your code.</span></span> <span data-ttu-id="a18b3-111">Si el código dependía del cálculo de valores predeterminados para <xref:System.Net.Cookie.Path>, considere la posibilidad de proporcionar el valor <xref:System.Net.Cookie.Path> manualmente en lugar de usar el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a18b3-111">If your code was dependent on default value calculation for <xref:System.Net.Cookie.Path>, consider supplying the <xref:System.Net.Cookie.Path> value manually instead of using the default value.</span></span>

#### <a name="category"></a><span data-ttu-id="a18b3-112">Categoría</span><span class="sxs-lookup"><span data-stu-id="a18b3-112">Category</span></span>

<span data-ttu-id="a18b3-113">Redes</span><span class="sxs-lookup"><span data-stu-id="a18b3-113">Networking</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="a18b3-114">API afectadas</span><span class="sxs-lookup"><span data-stu-id="a18b3-114">Affected APIs</span></span>

- <xref:System.Net.Cookie?displayProperty=fullName>
- <xref:System.Net.CookieContainer?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Net.Cookie`
- `T:System.Net.CookieContainer`

-->