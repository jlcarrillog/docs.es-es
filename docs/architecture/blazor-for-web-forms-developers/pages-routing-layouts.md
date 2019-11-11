---
title: Páginas, enrutamiento y diseños
description: Obtenga información acerca de cómo crear páginas en extraordinarias, trabajar con el enrutamiento del lado cliente y administrar diseños de página.
author: danroth27
ms.author: daroth
ms.date: 09/19/2019
ms.openlocfilehash: 693eee270a46ccb56ed5fef8fced1d4a1cf1974f
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "73841238"
---
# <a name="pages-routing-and-layouts"></a><span data-ttu-id="f5e25-103">Páginas, enrutamiento y diseños</span><span class="sxs-lookup"><span data-stu-id="f5e25-103">Pages, routing, and layouts</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="f5e25-104">Las aplicaciones de formularios Web Forms de ASP.NET se componen de páginas definidas en archivos *. aspx* .</span><span class="sxs-lookup"><span data-stu-id="f5e25-104">ASP.NET Web Forms apps are composed of pages defined in *.aspx* files.</span></span> <span data-ttu-id="f5e25-105">La dirección de cada página se basa en su ruta de acceso física del archivo en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5e25-105">Each page's address is based on its physical file path in the project.</span></span> <span data-ttu-id="f5e25-106">Cuando un explorador realiza una solicitud a la página, el contenido de la página se representa dinámicamente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="f5e25-106">When a browser makes a request to the page, the contents of the page are dynamically rendered on the server.</span></span> <span data-ttu-id="f5e25-107">Las cuentas de representación para el marcado HTML de la página y sus controles de servidor.</span><span class="sxs-lookup"><span data-stu-id="f5e25-107">The rendering accounts for both the page's HTML markup and its server controls.</span></span>

<span data-ttu-id="f5e25-108">En increíble, cada página de la aplicación es un componente, que normalmente se define en un archivo *. Razor* , con una o varias rutas especificadas.</span><span class="sxs-lookup"><span data-stu-id="f5e25-108">In Blazor, each page in the app is a component, typically defined in a *.razor* file, with one or more specified routes.</span></span> <span data-ttu-id="f5e25-109">El enrutamiento se produce principalmente en el lado cliente sin implicar una solicitud de servidor específica.</span><span class="sxs-lookup"><span data-stu-id="f5e25-109">Routing mostly happens client-side without involving a specific server request.</span></span> <span data-ttu-id="f5e25-110">En primer lugar, el explorador realiza una solicitud a la dirección raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5e25-110">The browser first makes a request to the root address of the app.</span></span> <span data-ttu-id="f5e25-111">Un componente de `Router` raíz de la aplicación extraordinariamente controla las solicitudes de navegación de interceptación y el componente correcto.</span><span class="sxs-lookup"><span data-stu-id="f5e25-111">A root `Router` component in the Blazor app then handles intercepting navigation requests and them to the correct component.</span></span>

<span data-ttu-id="f5e25-112">El increíble también admite la *vinculación profunda*.</span><span class="sxs-lookup"><span data-stu-id="f5e25-112">Blazor also supports *deep linking*.</span></span> <span data-ttu-id="f5e25-113">La vinculación profunda se produce cuando el explorador realiza una solicitud a una ruta específica distinta de la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5e25-113">Deep linking occurs when the browser makes a request to a specific route other than the root of the app.</span></span> <span data-ttu-id="f5e25-114">Las solicitudes de vínculos profundos que se envían al servidor se enrutan a la aplicación extraordinaria, que luego enruta el lado cliente de la solicitud al componente correcto.</span><span class="sxs-lookup"><span data-stu-id="f5e25-114">Requests for deep links sent to the server are routed to the Blazor app, which then routes the request client-side to the correct component.</span></span>

<span data-ttu-id="f5e25-115">Una página simple de formularios Web Forms de ASP.NET podría contener el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="f5e25-115">A simple page in ASP.NET Web Forms might contain the following markup:</span></span>

<span data-ttu-id="f5e25-116">*Nombre. aspx*</span><span class="sxs-lookup"><span data-stu-id="f5e25-116">*Name.aspx*</span></span>

```aspx-csharp
<%@ Page Title="Name" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Name.aspx.cs" Inherits="WebApplication1.Name" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <div>
        What is your name?<br />
        <asp:TextBox ID="TextBox1" runat="server"></asp:TextBox>
        <asp:Button ID="Button1" runat="server" Text="Submit" OnClick="Button1_Click" />
    </div>
    <div>
        <asp:Literal ID="Literal1" runat="server" />
    </div>
</asp:Content>
```

<span data-ttu-id="f5e25-117">*Name.aspx.cs*</span><span class="sxs-lookup"><span data-stu-id="f5e25-117">*Name.aspx.cs*</span></span>

```csharp
public partial class Name : System.Web.UI.Page
{
    protected void Button1_Click1(object sender, EventArgs e)
    {
        Literal1.Text = "Hello " + TextBox1.Text;
    }
}
```

<span data-ttu-id="f5e25-118">La página equivalente en una aplicación extraordinaria sería similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="f5e25-118">The equivalent page in a Blazor app would look like this:</span></span>

<span data-ttu-id="f5e25-119">*Nombre. Razor*</span><span class="sxs-lookup"><span data-stu-id="f5e25-119">*Name.razor*</span></span>

```razor
@page "/Name"
@layout MainLayout

<div>
    What is your name?<br />
    <input @bind="text" />
    <button @onclick="OnClick">Submit</button>
</div>
<div>
    @if (name != null)
    {
        @:Hello @name
    }
</div>

@code {
    string text;
    string name;

    void OnClick() {
        name = text;
    }
}
```

## <a name="create-pages"></a><span data-ttu-id="f5e25-120">Crear páginas</span><span class="sxs-lookup"><span data-stu-id="f5e25-120">Create pages</span></span>

<span data-ttu-id="f5e25-121">Para crear una página en increíble, cree un componente y agregue la Directiva de Razor `@page` para especificar la ruta del componente.</span><span class="sxs-lookup"><span data-stu-id="f5e25-121">To create a page in Blazor, create a component and add the `@page` Razor directive to specify the route for the component.</span></span> <span data-ttu-id="f5e25-122">La Directiva `@page` toma un único parámetro, que es la plantilla de ruta que se va a agregar a ese componente.</span><span class="sxs-lookup"><span data-stu-id="f5e25-122">The `@page` directive takes a single parameter, which is the route template to add to that component.</span></span>

```razor
@page "/counter"
```

<span data-ttu-id="f5e25-123">El parámetro de la plantilla de ruta es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="f5e25-123">The route template parameter is required.</span></span> <span data-ttu-id="f5e25-124">A diferencia de los formularios Web Forms de ASP.NET, la ruta a un componente increíblemente *no se* deduce de su ubicación de archivo (aunque puede ser una característica agregada en el futuro).</span><span class="sxs-lookup"><span data-stu-id="f5e25-124">Unlike ASP.NET Web Forms, the route to a Blazor component *isn't* inferred from its file location (although that may be a feature added in the future).</span></span>

<span data-ttu-id="f5e25-125">La sintaxis de la plantilla de ruta es la misma sintaxis básica que se usa para el enrutamiento en ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="f5e25-125">The route template syntax is the same basic syntax used for routing in ASP.NET Web Forms.</span></span> <span data-ttu-id="f5e25-126">Los parámetros de ruta se especifican en la plantilla mediante llaves.</span><span class="sxs-lookup"><span data-stu-id="f5e25-126">Route parameters are specified in the template using braces.</span></span> <span data-ttu-id="f5e25-127">Más increíble enlazará los valores de ruta a los parámetros de componente con el mismo nombre (sin distinción de mayúsculas y minúsculas).</span><span class="sxs-lookup"><span data-stu-id="f5e25-127">Blazor will bind route values to component parameters with the same name (case-insensitive).</span></span>

```razor
@page "/product/{id}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public string Id { get; set; }
}
```

<span data-ttu-id="f5e25-128">También puede especificar restricciones en el valor del parámetro Route.</span><span class="sxs-lookup"><span data-stu-id="f5e25-128">You can also specify constraints on the value of the route parameter.</span></span> <span data-ttu-id="f5e25-129">Por ejemplo, para restringir el identificador de producto a un `int`:</span><span class="sxs-lookup"><span data-stu-id="f5e25-129">For example, to constrain the product ID to be an `int`:</span></span>

```razor
@page "/product/{id:int}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public int Id { get; set; }
}
```

<span data-ttu-id="f5e25-130">Para obtener una lista completa de las restricciones de ruta admitidas por el increíbles, consulte [restricciones de ruta](/aspnet/core/blazor/routing#route-constraints).</span><span class="sxs-lookup"><span data-stu-id="f5e25-130">For a full list of the route constraints supported by Blazor, see [Route constraints](/aspnet/core/blazor/routing#route-constraints).</span></span>

## <a name="router-component"></a><span data-ttu-id="f5e25-131">Componente de enrutador</span><span class="sxs-lookup"><span data-stu-id="f5e25-131">Router component</span></span>

<span data-ttu-id="f5e25-132">El componente `Router` controla el enrutamiento en increíble.</span><span class="sxs-lookup"><span data-stu-id="f5e25-132">Routing in Blazor is handled by the `Router` component.</span></span> <span data-ttu-id="f5e25-133">El componente de `Router` se usa normalmente en el componente raíz de la aplicación (*app. Razor*).</span><span class="sxs-lookup"><span data-stu-id="f5e25-133">The `Router` component is typically used in the app's root component (*App.razor*).</span></span>

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

<span data-ttu-id="f5e25-134">El componente `Router` detecta los componentes enrutables en el `AppAssembly` especificado y en la `AdditionalAssemblies`especificada opcionalmente.</span><span class="sxs-lookup"><span data-stu-id="f5e25-134">The `Router` component discovers the routable components in the specified `AppAssembly` and in the optionally specified `AdditionalAssemblies`.</span></span> <span data-ttu-id="f5e25-135">Cuando el explorador navega, el `Router` intercepta la navegación y representa el contenido de su parámetro `Found` con el `RouteData` extraído si una ruta coincide con la dirección; de lo contrario, el `Router` representa su parámetro `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="f5e25-135">When the browser navigates, the `Router` intercepts the navigation and renders the contents of its `Found` parameter with the extracted `RouteData` if a route matches the address, otherwise the `Router` renders its `NotFound` parameter.</span></span>

<span data-ttu-id="f5e25-136">El componente `RouteView` controla la representación del componente coincidente especificado por el `RouteData` con su diseño si tiene uno.</span><span class="sxs-lookup"><span data-stu-id="f5e25-136">The `RouteView` component handles rendering the matched component specified by the `RouteData` with its layout if it has one.</span></span> <span data-ttu-id="f5e25-137">Si el componente coincidente no tiene un diseño, se utiliza el `DefaultLayout` opcionalmente especificado.</span><span class="sxs-lookup"><span data-stu-id="f5e25-137">If the matched component doesn't have a layout, then the optionally specified `DefaultLayout` is used.</span></span>

<span data-ttu-id="f5e25-138">El componente `LayoutView` representa su contenido secundario dentro del diseño especificado.</span><span class="sxs-lookup"><span data-stu-id="f5e25-138">The `LayoutView` component renders its child content within the specified layout.</span></span> <span data-ttu-id="f5e25-139">Veremos los diseños más detalladamente más adelante en este capítulo.</span><span class="sxs-lookup"><span data-stu-id="f5e25-139">We'll look at layouts more in detail later in this chapter.</span></span>

## <a name="navigation"></a><span data-ttu-id="f5e25-140">Navegación</span><span class="sxs-lookup"><span data-stu-id="f5e25-140">Navigation</span></span>

<span data-ttu-id="f5e25-141">En los formularios Web Forms de ASP.NET, se desencadena la navegación a una página diferente devolviendo una respuesta de redirección al explorador.</span><span class="sxs-lookup"><span data-stu-id="f5e25-141">In ASP.NET Web Forms, you trigger navigation to a different page by returning a redirect response to the browser.</span></span> <span data-ttu-id="f5e25-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f5e25-142">For example:</span></span>

```csharp
protected void NavigateButton_Click(object sender, EventArgs e)
{
    Response.Redirect("Counter");
}
```

<span data-ttu-id="f5e25-143">Normalmente, no es posible devolver una respuesta de redirección en increíble.</span><span class="sxs-lookup"><span data-stu-id="f5e25-143">Returning a redirect response isn't typically possible in Blazor.</span></span> <span data-ttu-id="f5e25-144">El increíbles no usa un modelo de solicitud-respuesta.</span><span class="sxs-lookup"><span data-stu-id="f5e25-144">Blazor doesn't use a request-reply model.</span></span> <span data-ttu-id="f5e25-145">Sin embargo, puede desencadenar directamente las navegaciones del explorador, como se puede hacer con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f5e25-145">You can, however, trigger browser navigations directly, as you can with JavaScript.</span></span>

<span data-ttu-id="f5e25-146">Increíble proporciona un servicio `NavigationManager` que se puede usar para:</span><span class="sxs-lookup"><span data-stu-id="f5e25-146">Blazor provides a `NavigationManager` service that can be used to:</span></span>

- <span data-ttu-id="f5e25-147">Obtener la dirección del explorador actual</span><span class="sxs-lookup"><span data-stu-id="f5e25-147">Get the current browser address</span></span>
- <span data-ttu-id="f5e25-148">Obtención de la dirección base</span><span class="sxs-lookup"><span data-stu-id="f5e25-148">Get the base address</span></span>
- <span data-ttu-id="f5e25-149">Navegación de desencadenador</span><span class="sxs-lookup"><span data-stu-id="f5e25-149">Trigger navigations</span></span>
- <span data-ttu-id="f5e25-150">Recibir una notificación cuando cambie la dirección</span><span class="sxs-lookup"><span data-stu-id="f5e25-150">Get notified when the address changes</span></span>

<span data-ttu-id="f5e25-151">Para navegar a una dirección diferente, utilice el método `NavigateTo`:</span><span class="sxs-lookup"><span data-stu-id="f5e25-151">To navigate to a different address, use the `NavigateTo` method:</span></span>

```razor
@page "/"
@inject NavigationManager NavigationManager

<button @onclick="Navigate">Navigate</button>

@code {
    void Navigate() {
        NavigationManager.NavigateTo("counter");
    }
}
```

<span data-ttu-id="f5e25-152">Para obtener una descripción de todos los miembros de `NavigationManager`, vea [aplicaciones auxiliares de URI y de estado de navegación](/aspnet/core/blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="f5e25-152">For a description of all `NavigationManager` members, see [URI and navigation state helpers](/aspnet/core/blazor/routing#uri-and-navigation-state-helpers).</span></span>

## <a name="base-urls"></a><span data-ttu-id="f5e25-153">Direcciones URL base</span><span class="sxs-lookup"><span data-stu-id="f5e25-153">Base URLs</span></span>

<span data-ttu-id="f5e25-154">Si la aplicación increíblemente se implementa en una ruta de acceso base, debe especificar la dirección URL base en los metadatos de la página con la etiqueta `<base>` para la propiedad enrutamiento a trabajo.</span><span class="sxs-lookup"><span data-stu-id="f5e25-154">If your Blazor app is deployed under a base path, then you need to specify the base URL in the page metadata using the `<base>` tag for routing to work property.</span></span> <span data-ttu-id="f5e25-155">Si la página host de la aplicación se representa en el servidor mediante Razor, puede usar la sintaxis de `~/` para especificar la dirección base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5e25-155">If the host page for the app is server-rendered using Razor, then you can use the `~/` syntax to specify the app's base address.</span></span> <span data-ttu-id="f5e25-156">Si la página host es HTML estático, debe especificar explícitamente la dirección URL base.</span><span class="sxs-lookup"><span data-stu-id="f5e25-156">If the host page is static HTML, then you need to specify the base URL explicitly.</span></span>

```html
<base href="~/" />
```

## <a name="page-layout"></a><span data-ttu-id="f5e25-157">Diseño de página</span><span class="sxs-lookup"><span data-stu-id="f5e25-157">Page layout</span></span>

<span data-ttu-id="f5e25-158">El diseño de página en formularios Web Forms de ASP.NET se controla mediante páginas maestras.</span><span class="sxs-lookup"><span data-stu-id="f5e25-158">Page layout in ASP.NET Web Forms is handled by Master Pages.</span></span> <span data-ttu-id="f5e25-159">Las páginas maestras definen una plantilla con uno o varios marcadores de posición de contenido que las páginas individuales pueden proporcionar.</span><span class="sxs-lookup"><span data-stu-id="f5e25-159">Master Pages define a template with one or more content placeholders that can then be supplied by individual pages.</span></span> <span data-ttu-id="f5e25-160">Las páginas maestras se definen en los archivos *. Master* y comienzan con la Directiva `<%@ Master %>`.</span><span class="sxs-lookup"><span data-stu-id="f5e25-160">Master Pages are defined in *.master* files and start with the `<%@ Master %>` directive.</span></span> <span data-ttu-id="f5e25-161">El contenido de los archivos *. Master* se codifica como lo haría con una página *. aspx* , pero con la adición de controles `<asp:ContentPlaceHolder>` para marcar dónde pueden proporcionar contenido las páginas.</span><span class="sxs-lookup"><span data-stu-id="f5e25-161">The content of the *.master* files is coded as you would an *.aspx* page, but with the addition of `<asp:ContentPlaceHolder>` controls to mark where pages can supply content.</span></span>

<span data-ttu-id="f5e25-162">*Site. Master*</span><span class="sxs-lookup"><span data-stu-id="f5e25-162">*Site.master*</span></span>

```aspx-csharp
<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="WebApplication1.SiteMaster" %>

<!DOCTYPE html>
<html lang="en">
<head runat="server">
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%: Page.Title %> - My ASP.NET Application</title>
    <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
</head>
<body>
    <form runat="server">
        <div class="container body-content">
            <asp:ContentPlaceHolder ID="MainContent" runat="server">
            </asp:ContentPlaceHolder>
            <hr />
            <footer>
                <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
            </footer>
        </div>
    </form>
</body>
</html>
```

<span data-ttu-id="f5e25-163">En increíble, se controla el diseño de página mediante componentes de diseño.</span><span class="sxs-lookup"><span data-stu-id="f5e25-163">In Blazor, you handle page layout using layout components.</span></span> <span data-ttu-id="f5e25-164">Los componentes de diseño heredan de `LayoutComponentBase`, que define una única propiedad `Body` de tipo `RenderFragment`, que se puede utilizar para representar el contenido de la página.</span><span class="sxs-lookup"><span data-stu-id="f5e25-164">Layout components inherit from `LayoutComponentBase`, which defines a single `Body` property of type `RenderFragment`, which can be used to render the contents of the page.</span></span>

<span data-ttu-id="f5e25-165">*MainLayout. Razor*</span><span class="sxs-lookup"><span data-stu-id="f5e25-165">*MainLayout.razor*</span></span>

```razor
@inherits LayoutComponentBase
<h1>Main layout</h1>
<div>
    @Body
</div>
```

<span data-ttu-id="f5e25-166">Cuando se representa la página con un diseño, la página se representa en el contenido del diseño especificado en la ubicación en la que el diseño representa su `Body` propiedad.</span><span class="sxs-lookup"><span data-stu-id="f5e25-166">When the page with a layout is rendered, the page is rendered within the contents of the specified layout at the location where the layout renders its `Body` property.</span></span>

<span data-ttu-id="f5e25-167">Para aplicar un diseño a una página, use la Directiva `@layout`:</span><span class="sxs-lookup"><span data-stu-id="f5e25-167">To apply a layout to a page, use the `@layout` directive:</span></span>

```razor
@layout MainLayout
```

<span data-ttu-id="f5e25-168">Puede especificar el diseño de todos los componentes de una carpeta y subcarpetas mediante un archivo *_Imports. Razor* .</span><span class="sxs-lookup"><span data-stu-id="f5e25-168">You can specify the layout for all components in a folder and subfolders using an *_Imports.razor* file.</span></span> <span data-ttu-id="f5e25-169">También puede especificar un diseño predeterminado para todas las páginas mediante el [componente del enrutador](#router-component).</span><span class="sxs-lookup"><span data-stu-id="f5e25-169">You can also specify a default layout for all your pages using the [Router component](#router-component).</span></span>

<span data-ttu-id="f5e25-170">Las páginas maestras pueden definir varios marcadores de posición de contenido, pero los diseños de increíble solo tienen una única propiedad de `Body`.</span><span class="sxs-lookup"><span data-stu-id="f5e25-170">Master Pages can define multiple content placeholders, but layouts in Blazor only have a single `Body` property.</span></span> <span data-ttu-id="f5e25-171">Esta limitación de los componentes de diseño de Extraordinariaidad se solucionará en una futura versión.</span><span class="sxs-lookup"><span data-stu-id="f5e25-171">This limitation of Blazor layout components will hopefully be addressed in a future release.</span></span>

<span data-ttu-id="f5e25-172">Las páginas maestras en formularios Web Forms de ASP.NET se pueden anidar.</span><span class="sxs-lookup"><span data-stu-id="f5e25-172">Master Pages in ASP.NET Web Forms can be nested.</span></span> <span data-ttu-id="f5e25-173">Es decir, una página maestra también puede usar una página maestra.</span><span class="sxs-lookup"><span data-stu-id="f5e25-173">That is, a Master Page may also use a Master Page.</span></span> <span data-ttu-id="f5e25-174">Los componentes de diseño de increíbles también pueden estar anidados.</span><span class="sxs-lookup"><span data-stu-id="f5e25-174">Layout components in Blazor may be nested too.</span></span> <span data-ttu-id="f5e25-175">Puede aplicar un componente de diseño a un componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="f5e25-175">You can apply a layout component to a layout component.</span></span> <span data-ttu-id="f5e25-176">El contenido del diseño interno se representará dentro del diseño exterior.</span><span class="sxs-lookup"><span data-stu-id="f5e25-176">The contents of the inner layout will be rendered within the outer layout.</span></span>

<span data-ttu-id="f5e25-177">*ChildLayout. Razor*</span><span class="sxs-lookup"><span data-stu-id="f5e25-177">*ChildLayout.razor*</span></span>

```razor
@layout MainLayout
<h2>Child layout</h2>
<div>
    @Body
</div>
```

<span data-ttu-id="f5e25-178">*Index. Razor*</span><span class="sxs-lookup"><span data-stu-id="f5e25-178">*Index.razor*</span></span>

```razor
@page "/"
@layout ChildLayout
<p>I'm in a nested layout!</p>
```

<span data-ttu-id="f5e25-179">La salida representada de la página sería:</span><span class="sxs-lookup"><span data-stu-id="f5e25-179">The rendered output for the page would then be:</span></span>

```html
<h1>Main layout</h1>
<div>
    <h2>Child layout</h2>
    <div>
        <p>I'm in a nested layout!</p>
    </div>
</div>
```

<span data-ttu-id="f5e25-180">Normalmente, los diseños de más increíblemente no definen los elementos HTML raíz de una página (`<html>`, `<body>`, `<head>`, etc.).</span><span class="sxs-lookup"><span data-stu-id="f5e25-180">Layouts in Blazor don't typically define the root HTML elements for a page (`<html>`, `<body>`, `<head>`, and so on).</span></span> <span data-ttu-id="f5e25-181">En su lugar, los elementos HTML raíz se definen en la página del host de una aplicación, que se usa para representar el contenido HTML inicial de la aplicación (consulte [bootstrap](project-structure.md#bootstrap-blazor)).</span><span class="sxs-lookup"><span data-stu-id="f5e25-181">The root HTML elements are instead defined in a Blazor app's host page, which is used to render the initial HTML content for the app (see [Bootstrap Blazor](project-structure.md#bootstrap-blazor)).</span></span> <span data-ttu-id="f5e25-182">La página host puede representar varios componentes raíz para la aplicación con el marcado circundante.</span><span class="sxs-lookup"><span data-stu-id="f5e25-182">The host page can render multiple root components for the app with surrounding markup.</span></span>

<span data-ttu-id="f5e25-183">Los componentes de extraordinarias, incluidas las páginas, no pueden representar etiquetas `<script>`.</span><span class="sxs-lookup"><span data-stu-id="f5e25-183">Components in Blazor, including pages, can't render `<script>` tags.</span></span> <span data-ttu-id="f5e25-184">Esta restricción de representación existe porque `<script>` etiquetas se cargan una vez y, después, no se pueden cambiar.</span><span class="sxs-lookup"><span data-stu-id="f5e25-184">This rendering restriction exists because `<script>` tags get loaded once and then can't be changed.</span></span> <span data-ttu-id="f5e25-185">Puede producirse un comportamiento inesperado si intenta representar las etiquetas dinámicamente mediante sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="f5e25-185">Unexpected behavior may occur if you try to render the tags dynamically using Razor syntax.</span></span> <span data-ttu-id="f5e25-186">En su lugar, se deben agregar todas las etiquetas de `<script>` a la página host de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5e25-186">Instead, all `<script>` tags should be added to the app's host page.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="f5e25-187">[Anterior](components.md)
>[Siguiente](state-management.md)</span><span class="sxs-lookup"><span data-stu-id="f5e25-187">[Previous](components.md)
[Next](state-management.md)</span></span>