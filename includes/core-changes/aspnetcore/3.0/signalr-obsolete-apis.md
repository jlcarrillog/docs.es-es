---
ms.openlocfilehash: 2a65caedea2af65796267aa145e275ebff814bf8
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394185"
---
### <a name="signalr-usesignalr-and-useconnections-methods-marked-obsolete"></a><span data-ttu-id="51dfa-101">SignalR: los métodos UseSignalR y UseConnections se han marcado como obsoletos</span><span class="sxs-lookup"><span data-stu-id="51dfa-101">SignalR: UseSignalR and UseConnections methods marked obsolete</span></span>

<span data-ttu-id="51dfa-102">Los métodos `UseConnections` y `UseSignalR`, y las clases `ConnectionsRouteBuilder` y `HubRouteBuilder` están marcados como obsoletos en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="51dfa-102">The methods `UseConnections` and `UseSignalR` and the classes `ConnectionsRouteBuilder` and `HubRouteBuilder` are marked as obsolete in ASP.NET Core 3.0.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="51dfa-103">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="51dfa-103">Version introduced</span></span>

<span data-ttu-id="51dfa-104">3.0</span><span class="sxs-lookup"><span data-stu-id="51dfa-104">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="51dfa-105">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="51dfa-105">Old behavior</span></span>

<span data-ttu-id="51dfa-106">El enrutamiento del centro de conectividad de SignalR se configuraba mediante `UseSignalR` o `UseConnections`.</span><span class="sxs-lookup"><span data-stu-id="51dfa-106">SignalR hub routing was configured using `UseSignalR` or `UseConnections`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="51dfa-107">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="51dfa-107">New behavior</span></span>

<span data-ttu-id="51dfa-108">La forma antigua de configurar el enrutamiento ha quedado obsoleta y se ha reemplazado por el enrutamiento del punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="51dfa-108">The old way of configuring routing has been obsoleted and replaced with endpoint routing.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="51dfa-109">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="51dfa-109">Reason for change</span></span>

<span data-ttu-id="51dfa-110">El middleware se está trasladando al sistema de enrutamiento nuevo de puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="51dfa-110">Middleware is being moved to the new endpoint routing system.</span></span> <span data-ttu-id="51dfa-111">La forma anterior de agregar middleware está obsoleta.</span><span class="sxs-lookup"><span data-stu-id="51dfa-111">The old way of adding middleware is being obsoleted.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="51dfa-112">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="51dfa-112">Recommended action</span></span>

<span data-ttu-id="51dfa-113">Reemplace `UseSignalR` por `UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="51dfa-113">Replace `UseSignalR` with `UseEndpoints`:</span></span>

<span data-ttu-id="51dfa-114">**Código anterior:**</span><span class="sxs-lookup"><span data-stu-id="51dfa-114">**Old code:**</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<SomeHub>("/path");
});
```

<span data-ttu-id="51dfa-115">**Código nuevo:**</span><span class="sxs-lookup"><span data-stu-id="51dfa-115">**New code:**</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<SomeHub>("/path");
});
```

#### <a name="category"></a><span data-ttu-id="51dfa-116">Categoría</span><span class="sxs-lookup"><span data-stu-id="51dfa-116">Category</span></span>

<span data-ttu-id="51dfa-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51dfa-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="51dfa-118">API afectadas</span><span class="sxs-lookup"><span data-stu-id="51dfa-118">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder})?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.SignalR.HubRouteBuilder})?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.SignalR.HubRouteBuilder?displayProperty=fullName>

<!-- 

#### Affected APIs

- `M:Microsoft.AspNetCore.Builder.ConnectionsAppBuilderExtensions.UseConnections(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder})`
- `M:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR(Microsoft.AspNetCore.Builder.IApplicationBuilder,System.Action{Microsoft.AspNetCore.SignalR.HubRouteBuilder})`
- `T:Microsoft.AspNetCore.Http.Connections.ConnectionsRouteBuilder`
- `T:Microsoft.AspNetCore.SignalR.HubRouteBuilder`

-->