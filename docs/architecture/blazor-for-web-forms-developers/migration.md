---
title: Migre desde formularios Web Forms de ASP.NET a Blazor
description: Aprenda a enfocar la migración de una aplicación de formularios Web Forms de ASP.NET existente a Blazor .
author: twsouthwick
ms.author: tasou
no-loc:
- Blazor
- WebAssembly
ms.date: 11/20/2020
ms.openlocfilehash: 893b6f851681ec540629fe160749b2622b6d5440
ms.sourcegitcommit: 2f485e721f7f34b87856a51181b5b56624b31fd5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509837"
---
# <a name="migrate-from-aspnet-web-forms-to-no-locblazor"></a><span data-ttu-id="956eb-103">Migre desde formularios Web Forms de ASP.NET a Blazor</span><span class="sxs-lookup"><span data-stu-id="956eb-103">Migrate from ASP.NET Web Forms to Blazor</span></span>

<span data-ttu-id="956eb-104">La migración de una base de código de formularios Web Forms de ASP.NET a Blazor es una tarea que requiere mucho tiempo y requiere planeación.</span><span class="sxs-lookup"><span data-stu-id="956eb-104">Migrating a code base from ASP.NET Web Forms to Blazor is a time-consuming task that requires planning.</span></span> <span data-ttu-id="956eb-105">En este capítulo se describe el proceso.</span><span class="sxs-lookup"><span data-stu-id="956eb-105">This chapter outlines the process.</span></span> <span data-ttu-id="956eb-106">Algo que puede facilitar la transición es asegurarse de que la aplicación se adhiera a una arquitectura de *N niveles* , donde el modelo de la aplicación (en este caso, los formularios Web Forms) es independiente de la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="956eb-106">Something that can ease the transition is to ensure the app adheres to an *N-tier* architecture, wherein the app model (in this case, Web Forms) is separate from the business logic.</span></span> <span data-ttu-id="956eb-107">Esta separación lógica de las capas permite desactivar lo que necesita pasar a .NET Core y Blazor .</span><span class="sxs-lookup"><span data-stu-id="956eb-107">This logical separation of layers makes it clear what needs to move to .NET Core and Blazor.</span></span>

<span data-ttu-id="956eb-108">En este ejemplo, se usa la aplicación de eShop disponible en [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) .</span><span class="sxs-lookup"><span data-stu-id="956eb-108">For this example, the eShop app available on [GitHub](https://github.com/dotnet-architecture/eShopOnBlazor) is used.</span></span> <span data-ttu-id="956eb-109">eShop es un servicio de catálogo que proporciona capacidades CRUD mediante la entrada de formulario y la validación.</span><span class="sxs-lookup"><span data-stu-id="956eb-109">eShop is a catalog service that provides CRUD capabilities via form entry and validation.</span></span>

<span data-ttu-id="956eb-110">¿Por qué se debe migrar a una aplicación en funcionamiento Blazor ?</span><span class="sxs-lookup"><span data-stu-id="956eb-110">Why should a working app be migrated to Blazor?</span></span> <span data-ttu-id="956eb-111">Muchas veces, no es necesario.</span><span class="sxs-lookup"><span data-stu-id="956eb-111">Many times, there's no need.</span></span> <span data-ttu-id="956eb-112">Los formularios Web Forms de ASP.NET seguirán siendo compatibles durante muchos años.</span><span class="sxs-lookup"><span data-stu-id="956eb-112">ASP.NET Web Forms will continue to be supported for many years.</span></span> <span data-ttu-id="956eb-113">Sin embargo, muchas de las características que Blazor proporciona solo se admiten en una aplicación migrada.</span><span class="sxs-lookup"><span data-stu-id="956eb-113">However, many of the features that Blazor provides are only supported on a migrated app.</span></span> <span data-ttu-id="956eb-114">Estas características incluyen:</span><span class="sxs-lookup"><span data-stu-id="956eb-114">Such features include:</span></span>

- <span data-ttu-id="956eb-115">Mejoras de rendimiento en el marco de trabajo como `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="956eb-115">Performance improvements in the framework such as `Span<T>`</span></span>
- <span data-ttu-id="956eb-116">Capacidad para ejecutarse como WebAssembly</span><span class="sxs-lookup"><span data-stu-id="956eb-116">Ability to run as WebAssembly</span></span>
- <span data-ttu-id="956eb-117">Compatibilidad entre plataformas para Linux y macOS</span><span class="sxs-lookup"><span data-stu-id="956eb-117">Cross-platform support for Linux and macOS</span></span>
- <span data-ttu-id="956eb-118">Implementación local de la aplicación o implementación de un marco compartido sin afectar a otras aplicaciones</span><span class="sxs-lookup"><span data-stu-id="956eb-118">App-local deployment or shared framework deployment without impacting other apps</span></span>

<span data-ttu-id="956eb-119">Si estas u otras características nuevas son lo suficientemente atractivas, puede haber un valor en la migración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="956eb-119">If these or other new features are compelling enough, there may be value in migrating the app.</span></span> <span data-ttu-id="956eb-120">La migración puede tomar distintas formas; puede ser toda la aplicación o solo determinados puntos de conexión que requieran los cambios.</span><span class="sxs-lookup"><span data-stu-id="956eb-120">The migration can take different shapes; it can be the entire app, or only certain endpoints that require the changes.</span></span> <span data-ttu-id="956eb-121">La decisión de migrar se basa en última instancia en los problemas empresariales que el desarrollador debe resolver.</span><span class="sxs-lookup"><span data-stu-id="956eb-121">The decision to migrate is ultimately based on the business problems to be solved by the developer.</span></span>

## <a name="server-side-versus-client-side-hosting"></a><span data-ttu-id="956eb-122">En el lado servidor frente al hospedaje del lado cliente</span><span class="sxs-lookup"><span data-stu-id="956eb-122">Server-side versus client-side hosting</span></span>

<span data-ttu-id="956eb-123">Como se describe en el capítulo [modelos de hospedaje](hosting-models.md) , una Blazor aplicación se puede hospedar de dos maneras diferentes: lado servidor y lado cliente.</span><span class="sxs-lookup"><span data-stu-id="956eb-123">As described in the [hosting models](hosting-models.md) chapter, a Blazor app can be hosted in two different ways: server-side and client-side.</span></span> <span data-ttu-id="956eb-124">El modelo de servidor usa ASP.NET Core las conexiones de Signalr para administrar las actualizaciones del DOM mientras se ejecuta cualquier código real en el servidor.</span><span class="sxs-lookup"><span data-stu-id="956eb-124">The server-side model uses ASP.NET Core SignalR connections to manage the DOM updates while running any actual code on the server.</span></span> <span data-ttu-id="956eb-125">El modelo de cliente se ejecuta como WebAssembly dentro de un explorador y no requiere ninguna conexión de servidor.</span><span class="sxs-lookup"><span data-stu-id="956eb-125">The client-side model runs as WebAssembly within a browser and requires no server connections.</span></span> <span data-ttu-id="956eb-126">Hay una serie de diferencias que pueden afectar a lo mejor para una aplicación específica:</span><span class="sxs-lookup"><span data-stu-id="956eb-126">There are a number of differences that may affect which is best for a specific app:</span></span>

- <span data-ttu-id="956eb-127">Ejecutar como WebAssembly no admite todas las características (como el subprocesamiento) en el momento actual</span><span class="sxs-lookup"><span data-stu-id="956eb-127">Running as WebAssembly doesn't support all features (such as threading) at the current time</span></span>
- <span data-ttu-id="956eb-128">La comunicación entre el cliente y el servidor puede producir problemas de latencia en el modo de servidor</span><span class="sxs-lookup"><span data-stu-id="956eb-128">Chatty communication between the client and server may cause latency issues in server-side mode</span></span>
- <span data-ttu-id="956eb-129">El acceso a las bases de datos y a los servicios internos o protegidos requiere un servicio independiente con hospedaje en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="956eb-129">Access to databases and internal or protected services require a separate service with client-side hosting</span></span>

<span data-ttu-id="956eb-130">En el momento de escribir este documento, el modelo del lado servidor se parece más al de los formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="956eb-130">At the time of writing, the server-side model more closely resembles Web Forms.</span></span> <span data-ttu-id="956eb-131">La mayor parte de este capítulo se centra en el modelo de hospedaje del lado servidor, ya que está listo para la producción.</span><span class="sxs-lookup"><span data-stu-id="956eb-131">Most of this chapter focuses on the server-side hosting model, as it's production-ready.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="956eb-132">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="956eb-132">Create a new project</span></span>

<span data-ttu-id="956eb-133">Este paso de migración inicial consiste en crear un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="956eb-133">This initial migration step is to create a new project.</span></span> <span data-ttu-id="956eb-134">Este tipo de proyecto se basa en los proyectos de estilo SDK de .NET y simplifica gran parte del texto reutilizable que se usó en los formatos de proyecto anteriores.</span><span class="sxs-lookup"><span data-stu-id="956eb-134">This project type is based on the SDK style projects of .NET and simplifies much of the boilerplate that was used in previous project formats.</span></span> <span data-ttu-id="956eb-135">Para obtener más información, consulte el capítulo sobre la [estructura del proyecto](project-structure.md).</span><span class="sxs-lookup"><span data-stu-id="956eb-135">For more detail, please see the chapter on [Project Structure](project-structure.md).</span></span>

<span data-ttu-id="956eb-136">Una vez creado el proyecto, instale las bibliotecas que se usaron en el proyecto anterior.</span><span class="sxs-lookup"><span data-stu-id="956eb-136">Once the project has been created, install the libraries that were used in the previous project.</span></span> <span data-ttu-id="956eb-137">En los proyectos de formularios Web Forms anteriores, es posible que haya usado el archivo *packages.config* para mostrar los paquetes de NuGet necesarios.</span><span class="sxs-lookup"><span data-stu-id="956eb-137">In older Web Forms projects, you may have used the *packages.config* file to list the required NuGet packages.</span></span> <span data-ttu-id="956eb-138">En el nuevo proyecto de estilo SDK, *packages.config* se ha reemplazado por `<PackageReference>` elementos en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="956eb-138">In the new SDK-style project, *packages.config* has been replaced with `<PackageReference>` elements in the project file.</span></span> <span data-ttu-id="956eb-139">Una ventaja de este enfoque es que todas las dependencias se instalan de forma transitiva.</span><span class="sxs-lookup"><span data-stu-id="956eb-139">A benefit to this approach is that all dependencies are installed transitively.</span></span> <span data-ttu-id="956eb-140">Solo se muestran las dependencias de nivel superior que le interesan.</span><span class="sxs-lookup"><span data-stu-id="956eb-140">You only list the top-level dependencies you care about.</span></span>

<span data-ttu-id="956eb-141">Muchas de las dependencias que está usando están disponibles para .NET, como Entity Framework 6 y log4net.</span><span class="sxs-lookup"><span data-stu-id="956eb-141">Many of the dependencies you're using are available for .NET, including Entity Framework 6 and log4net.</span></span> <span data-ttu-id="956eb-142">Si no hay ninguna versión de .NET o .NET Standard disponible, a menudo se puede usar la versión de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="956eb-142">If there's no .NET or .NET Standard version available, the .NET Framework version can often be used.</span></span> <span data-ttu-id="956eb-143">Puede que tu experiencia sea diferente.</span><span class="sxs-lookup"><span data-stu-id="956eb-143">Your mileage may vary.</span></span> <span data-ttu-id="956eb-144">Cualquier API usada que no esté disponible en .NET provocará un error de tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="956eb-144">Any API used that isn't available in .NET causes a runtime error.</span></span> <span data-ttu-id="956eb-145">Visual Studio le informa de estos paquetes.</span><span class="sxs-lookup"><span data-stu-id="956eb-145">Visual Studio notifies you of such packages.</span></span> <span data-ttu-id="956eb-146">Aparece un icono amarillo en el nodo **referencias** del proyecto en **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="956eb-146">A yellow icon appears on the project's **References** node in **Solution Explorer**.</span></span>

<span data-ttu-id="956eb-147">En el Blazor proyecto de eShop basado en, puede ver los paquetes que están instalados.</span><span class="sxs-lookup"><span data-stu-id="956eb-147">In the Blazor-based eShop project, you can see the packages that are installed.</span></span> <span data-ttu-id="956eb-148">Anteriormente, el archivo de *packages.config* mostraba todos los paquetes usados en el proyecto, lo que generaba un archivo de casi 50 líneas de longitud.</span><span class="sxs-lookup"><span data-stu-id="956eb-148">Previously, the *packages.config* file listed every package used in the project, resulting in a file almost 50 lines long.</span></span> <span data-ttu-id="956eb-149">Un fragmento de *packages.config* es:</span><span class="sxs-lookup"><span data-stu-id="956eb-149">A snippet of *packages.config* is:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  ...
  <package id="Microsoft.ApplicationInsights.Agent.Intercept" version="2.4.0" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.DependencyCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.PerfCounterCollector" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.Web" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" version="2.9.1" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.FriendlyUrls.Core" version="1.0.2" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.MSAjax" version="5.0.0" targetFramework="net472" />
  <package id="Microsoft.AspNet.ScriptManager.WebForms" version="5.0.0" targetFramework="net472" />
  ...
  <package id="System.Memory" version="4.5.1" targetFramework="net472" />
  <package id="System.Numerics.Vectors" version="4.4.0" targetFramework="net472" />
  <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Channels" version="4.5.0" targetFramework="net472" />
  <package id="System.Threading.Tasks.Extensions" version="4.5.1" targetFramework="net472" />
  <package id="WebGrease" version="1.6.0" targetFramework="net472" />
</packages>
```

<span data-ttu-id="956eb-150">El `<packages>` elemento incluye todas las dependencias necesarias.</span><span class="sxs-lookup"><span data-stu-id="956eb-150">The `<packages>` element includes all necessary dependencies.</span></span> <span data-ttu-id="956eb-151">Es difícil identificar cuál de estos paquetes se incluyen porque los necesita.</span><span class="sxs-lookup"><span data-stu-id="956eb-151">It's difficult to identify which of these packages are included because you require them.</span></span> <span data-ttu-id="956eb-152">Algunos `<package>` elementos se enumeran simplemente para satisfacer las necesidades de las dependencias que necesita.</span><span class="sxs-lookup"><span data-stu-id="956eb-152">Some `<package>` elements are listed simply to satisfy the needs of dependencies you require.</span></span>

<span data-ttu-id="956eb-153">El Blazor proyecto muestra las dependencias que necesita dentro de un `<ItemGroup>` elemento en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="956eb-153">The Blazor project lists the dependencies you require within an `<ItemGroup>` element in the project file:</span></span>

```xml
<ItemGroup>
    <PackageReference Include="Autofac" Version="4.9.3" />
    <PackageReference Include="EntityFramework" Version="6.4.4" />
    <PackageReference Include="log4net" Version="2.0.12" />
    <PackageReference Include="Microsoft.Extensions.Logging.Log4Net.AspNetCore" Version="2.2.12" />
</ItemGroup>
```

<span data-ttu-id="956eb-154">Un paquete NuGet que simplifica la vida de los desarrolladores de formularios Web Forms es el [paquete de compatibilidad de Windows](../../core/porting/windows-compat-pack.md).</span><span class="sxs-lookup"><span data-stu-id="956eb-154">One NuGet package that simplifies the life of Web Forms developers is the [Windows Compatibility Pack](../../core/porting/windows-compat-pack.md).</span></span> <span data-ttu-id="956eb-155">Aunque .NET es multiplataforma, algunas características solo están disponibles en Windows.</span><span class="sxs-lookup"><span data-stu-id="956eb-155">Although .NET is cross-platform, some features are only available on Windows.</span></span> <span data-ttu-id="956eb-156">Las características específicas de Windows se ponen a disposición mediante la instalación del paquete de compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="956eb-156">Windows-specific features are made available by installing the compatibility pack.</span></span> <span data-ttu-id="956eb-157">Entre los ejemplos de estas características se incluyen el registro, WMI y servicios de directorio.</span><span class="sxs-lookup"><span data-stu-id="956eb-157">Examples of such features include the Registry, WMI, and Directory Services.</span></span> <span data-ttu-id="956eb-158">El paquete agrega alrededor de 20.000 API y activa muchos servicios con los que es posible que ya esté familiarizado.</span><span class="sxs-lookup"><span data-stu-id="956eb-158">The package adds around 20,000 APIs and activates many services with which you may already be familiar.</span></span> <span data-ttu-id="956eb-159">El proyecto de eShop no requiere el paquete de compatibilidad; pero si los proyectos usan características específicas de Windows, el paquete facilita el trabajo de migración.</span><span class="sxs-lookup"><span data-stu-id="956eb-159">The eShop project doesn't require the compatibility pack; but if your projects use Windows-specific features, the package eases the migration efforts.</span></span>

## <a name="enable-startup-process"></a><span data-ttu-id="956eb-160">Habilitar proceso de inicio</span><span class="sxs-lookup"><span data-stu-id="956eb-160">Enable startup process</span></span>

<span data-ttu-id="956eb-161">El proceso de inicio de Blazor ha cambiado de los formularios Web Forms y sigue una configuración similar para otros servicios de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-161">The startup process for Blazor has changed from Web Forms and follows a similar setup for other ASP.NET Core services.</span></span> <span data-ttu-id="956eb-162">Cuando el lado servidor hospedado, Blazor los componentes se ejecutan como parte de una aplicación de ASP.net Core normal.</span><span class="sxs-lookup"><span data-stu-id="956eb-162">When hosted server-side, Blazor components are run as part of a normal ASP.NET Core app.</span></span> <span data-ttu-id="956eb-163">Cuando se hospeda en el explorador con WebAssembly , Blazor los componentes utilizan un modelo de hospedaje similar.</span><span class="sxs-lookup"><span data-stu-id="956eb-163">When hosted in the browser with WebAssembly, Blazor components use a similar hosting model.</span></span> <span data-ttu-id="956eb-164">La diferencia es que los componentes se ejecutan como un servicio independiente de cualquiera de los procesos de back-end.</span><span class="sxs-lookup"><span data-stu-id="956eb-164">The difference is the components are run as a separate service from any of the backend processes.</span></span> <span data-ttu-id="956eb-165">En cualquier caso, el inicio es similar.</span><span class="sxs-lookup"><span data-stu-id="956eb-165">Either way, the startup is similar.</span></span>

<span data-ttu-id="956eb-166">El archivo *global.asax.CS* es la página de inicio predeterminada para los proyectos de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="956eb-166">The *Global.asax.cs* file is the default startup page for Web Forms projects.</span></span> <span data-ttu-id="956eb-167">En el proyecto de eShop, este archivo configura el contenedor de inversión de control (IoC) y controla los distintos eventos de ciclo de vida de la aplicación o solicitud.</span><span class="sxs-lookup"><span data-stu-id="956eb-167">In the eShop project, this file configures the Inversion of Control (IoC) container and handles the various lifecycle events of the app or request.</span></span> <span data-ttu-id="956eb-168">Algunos de estos eventos se controlan con middleware (como `Application_BeginRequest` ).</span><span class="sxs-lookup"><span data-stu-id="956eb-168">Some of these events are handled with middleware (such as `Application_BeginRequest`).</span></span> <span data-ttu-id="956eb-169">Otros eventos requieren la invalidación de servicios específicos a través de la inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="956eb-169">Other events require the overriding of specific services via dependency injection (DI).</span></span>

<span data-ttu-id="956eb-170">Por ejemplo, el archivo *global.asax.CS* de eShop contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="956eb-170">By way of example, the *Global.asax.cs* file for eShop, contains the following code:</span></span>

```csharp
public class Global : HttpApplication, IContainerProviderAccessor
{
    private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

    static IContainerProvider _containerProvider;
    IContainer container;

    public IContainerProvider ContainerProvider
    {
        get { return _containerProvider; }
    }

    protected void Application_Start(object sender, EventArgs e)
    {
        // Code that runs on app startup
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);
        ConfigureContainer();
        ConfigDataBase();
    }

    /// <summary>
    /// Track the machine name and the start time for the session inside the current session
    /// </summary>
    protected void Session_Start(Object sender, EventArgs e)
    {
        HttpContext.Current.Session["MachineName"] = Environment.MachineName;
        HttpContext.Current.Session["SessionStartTime"] = DateTime.Now;
    }

    /// <summary>
    /// https://autofaccn.readthedocs.io/en/latest/integration/webforms.html
    /// </summary>
    private void ConfigureContainer()
    {
        var builder = new ContainerBuilder();
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
        builder.RegisterModule(new ApplicationModule(mockData));
        container = builder.Build();
        _containerProvider = new ContainerProvider(container);
    }

    private void ConfigDataBase()
    {
        var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);

        if (!mockData)
        {
            Database.SetInitializer<CatalogDBContext>(container.Resolve<CatalogDBInitializer>());
        }
    }

    protected void Application_BeginRequest(object sender, EventArgs e)
    {
        //set the property to our new object
        LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper();

        LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo();

        _log.Debug("Application_BeginRequest");
    }
}
```

<span data-ttu-id="956eb-171">El archivo anterior se convierte `Startup` en la clase en el lado servidor Blazor :</span><span class="sxs-lookup"><span data-stu-id="956eb-171">The preceding file becomes the `Startup` class in server-side Blazor:</span></span>

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    public IConfiguration Configuration { get; }

    public IWebHostEnvironment Env { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages();
        services.AddServerSideBlazor();

        if (Configuration.GetValue<bool>("UseMockData"))
        {
            services.AddSingleton<ICatalogService, CatalogServiceMock>();
        }
        else
        {
            services.AddScoped<ICatalogService, CatalogService>();
            services.AddScoped<IDatabaseInitializer<CatalogDBContext>, CatalogDBInitializer>();
            services.AddSingleton<CatalogItemHiLoGenerator>();
            services.AddScoped(_ => new CatalogDBContext(Configuration.GetConnectionString("CatalogDBContext")));
        }
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddLog4Net("log4Net.xml");

        if (Env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
        }

        // Middleware for Application_BeginRequest
        app.Use((ctx, next) =>
        {
            LogicalThreadContext.Properties["activityid"] = new ActivityIdHelper(ctx);
            LogicalThreadContext.Properties["requestinfo"] = new WebRequestInfo(ctx);
            return next();
        });

        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapBlazorHub();
            endpoints.MapFallbackToPage("/_Host");
        });

        ConfigDataBase(app);
    }

    private void ConfigDataBase(IApplicationBuilder app)
    {
        using (var scope = app.ApplicationServices.CreateScope())
        {
            var initializer = scope.ServiceProvider.GetService<IDatabaseInitializer<CatalogDBContext>>();

            if (initializer != null)
            {
                Database.SetInitializer(initializer);
            }
        }
    }
}
```

<span data-ttu-id="956eb-172">Un cambio importante que puede observar de los formularios Web Forms es el importancia de DI.</span><span class="sxs-lookup"><span data-stu-id="956eb-172">One significant change you may notice from Web Forms is the prominence of DI.</span></span> <span data-ttu-id="956eb-173">DI ha sido un principio de GUID en el diseño de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-173">DI has been a guiding principle in the ASP.NET Core design.</span></span> <span data-ttu-id="956eb-174">Admite la personalización de casi todos los aspectos del marco de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-174">It supports customization of almost all aspects of the ASP.NET Core framework.</span></span> <span data-ttu-id="956eb-175">Hay incluso un proveedor de servicios integrado que se puede usar para muchos escenarios.</span><span class="sxs-lookup"><span data-stu-id="956eb-175">There's even a built-in service provider that can be used for many scenarios.</span></span> <span data-ttu-id="956eb-176">Si se requiere más personalización, puede ser compatible con muchos proyectos de la comunidad.</span><span class="sxs-lookup"><span data-stu-id="956eb-176">If more customization is required, it can be supported by many community projects.</span></span> <span data-ttu-id="956eb-177">Por ejemplo, puede llevar a cabo la inversión de la biblioteca de DI de terceros.</span><span class="sxs-lookup"><span data-stu-id="956eb-177">For example, you can carry forward your third-party DI library investment.</span></span>

<span data-ttu-id="956eb-178">En la aplicación de eShop original, hay alguna configuración para la administración de sesiones.</span><span class="sxs-lookup"><span data-stu-id="956eb-178">In the original eShop app, there's some configuration for session management.</span></span> <span data-ttu-id="956eb-179">Puesto que el servidor Blazor usa ASP.net Core signalr para la comunicación, no se admite el estado de sesión, ya que las conexiones pueden producirse independientemente de un contexto http.</span><span class="sxs-lookup"><span data-stu-id="956eb-179">Since server-side Blazor uses ASP.NET Core SignalR for communication, the session state isn't supported as the connections may occur independent of an HTTP context.</span></span> <span data-ttu-id="956eb-180">Una aplicación que usa el estado de sesión requiere que se rediseñe antes de ejecutarse como una Blazor aplicación.</span><span class="sxs-lookup"><span data-stu-id="956eb-180">An app that uses the session state requires rearchitecting before running as a Blazor app.</span></span>

<span data-ttu-id="956eb-181">Para obtener más información sobre el inicio de la aplicación, consulte inicio de la [aplicación](app-startup.md).</span><span class="sxs-lookup"><span data-stu-id="956eb-181">For more information about app startup, see [App startup](app-startup.md).</span></span>

## <a name="migrate-http-modules-and-handlers-to-middleware"></a><span data-ttu-id="956eb-182">Migración de módulos y controladores HTTP a middleware</span><span class="sxs-lookup"><span data-stu-id="956eb-182">Migrate HTTP modules and handlers to middleware</span></span>

<span data-ttu-id="956eb-183">Los módulos y controladores HTTP son patrones comunes en formularios Web Forms para controlar la canalización de solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="956eb-183">HTTP modules and handlers are common patterns in Web Forms to control the HTTP request pipeline.</span></span> <span data-ttu-id="956eb-184">Clases que implementan `IHttpModule` o que `IHttpHandler` se pueden registrar y procesar solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="956eb-184">Classes that implement `IHttpModule` or `IHttpHandler` could be registered and process incoming requests.</span></span> <span data-ttu-id="956eb-185">Los formularios Web Forms configuran módulos y controladores en el archivo de *web.config* .</span><span class="sxs-lookup"><span data-stu-id="956eb-185">Web Forms configure modules and handlers in the *web.config* file.</span></span> <span data-ttu-id="956eb-186">Los formularios Web Forms también se basan principalmente en el control de eventos del ciclo de vida de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="956eb-186">Web Forms is also heavily based on app lifecycle event handling.</span></span> <span data-ttu-id="956eb-187">En su lugar, ASP.NET Core usa middleware.</span><span class="sxs-lookup"><span data-stu-id="956eb-187">ASP.NET Core uses middleware instead.</span></span> <span data-ttu-id="956eb-188">El middleware se registra en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="956eb-188">Middleware is registered in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="956eb-189">El orden de ejecución del middleware viene determinado por el orden de registro.</span><span class="sxs-lookup"><span data-stu-id="956eb-189">Middleware execution order is determined by the registration order.</span></span>

<span data-ttu-id="956eb-190">En la sección [Habilitar proceso de inicio](#enable-startup-process) , Web Forms generó un evento de ciclo de vida como el `Application_BeginRequest` método.</span><span class="sxs-lookup"><span data-stu-id="956eb-190">In the [Enable startup process](#enable-startup-process) section, a lifecycle event was raised by Web Forms as the `Application_BeginRequest` method.</span></span> <span data-ttu-id="956eb-191">Este evento no está disponible en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-191">This event isn't available in ASP.NET Core.</span></span> <span data-ttu-id="956eb-192">Una manera de lograr este comportamiento es implementar el middleware tal como se aprecia en el ejemplo de archivo *Startup.CS* .</span><span class="sxs-lookup"><span data-stu-id="956eb-192">One way to achieve this behavior is to implement middleware as seen in the *Startup.cs* file example.</span></span> <span data-ttu-id="956eb-193">Este middleware realiza la misma lógica y, a continuación, transfiere el control al siguiente controlador en la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="956eb-193">This middleware does the same logic and then transfers control to the next handler in the middleware pipeline.</span></span>

<span data-ttu-id="956eb-194">Para obtener más información sobre cómo migrar módulos y controladores, consulte [migración de controladores y módulos http a middleware de ASP.net Core](/aspnet/core/migration/http-modules).</span><span class="sxs-lookup"><span data-stu-id="956eb-194">For more information on migrating modules and handlers, see [Migrate HTTP handlers and modules to ASP.NET Core middleware](/aspnet/core/migration/http-modules).</span></span>

## <a name="migrate-static-files"></a><span data-ttu-id="956eb-195">Migrar archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="956eb-195">Migrate static files</span></span>

<span data-ttu-id="956eb-196">Para servir archivos estáticos (por ejemplo, HTML, CSS, imágenes y JavaScript), los archivos deben estar expuestos por middleware.</span><span class="sxs-lookup"><span data-stu-id="956eb-196">To serve static files (for example, HTML, CSS, images, and JavaScript), the files must be exposed by middleware.</span></span> <span data-ttu-id="956eb-197">La llamada al `UseStaticFiles` método habilita el servicio de archivos estáticos de la ruta de acceso raíz Web.</span><span class="sxs-lookup"><span data-stu-id="956eb-197">Calling the `UseStaticFiles` method enables the serving of static files from the web root path.</span></span> <span data-ttu-id="956eb-198">El directorio raíz Web predeterminado es *wwwroot*, pero se puede personalizar.</span><span class="sxs-lookup"><span data-stu-id="956eb-198">The default web root directory is *wwwroot*, but it can be customized.</span></span> <span data-ttu-id="956eb-199">Tal y como se incluye en el `Configure` método de la clase de eShop `Startup` :</span><span class="sxs-lookup"><span data-stu-id="956eb-199">As included in the `Configure` method of eShop's `Startup` class:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    ...

    app.UseStaticFiles();

    ...
}
```

<span data-ttu-id="956eb-200">El proyecto de eShop permite el acceso a archivos estáticos básicos.</span><span class="sxs-lookup"><span data-stu-id="956eb-200">The eShop project enables basic static file access.</span></span> <span data-ttu-id="956eb-201">Hay muchas personalizaciones disponibles para el acceso a archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="956eb-201">There are many customizations available for static file access.</span></span> <span data-ttu-id="956eb-202">Para obtener información sobre cómo habilitar archivos predeterminados o un explorador de archivos, consulte [archivos estáticos en ASP.net Core](/aspnet/core/fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="956eb-202">For information on enabling default files or a file browser, see [Static files in ASP.NET Core](/aspnet/core/fundamentals/static-files).</span></span>

## <a name="migrate-runtime-bundling-and-minification-setup"></a><span data-ttu-id="956eb-203">Migrar la configuración de agrupación y minificación de tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="956eb-203">Migrate runtime bundling and minification setup</span></span>

<span data-ttu-id="956eb-204">La agrupación y minificación son técnicas de optimización del rendimiento para reducir el número y el tamaño de las solicitudes de servidor para recuperar determinados tipos de archivo.</span><span class="sxs-lookup"><span data-stu-id="956eb-204">Bundling and minification are performance optimization techniques for reducing the number and size of server requests to retrieve certain file types.</span></span> <span data-ttu-id="956eb-205">JavaScript y CSS suelen someterse a una forma de agrupación o minificación antes de enviarse al cliente.</span><span class="sxs-lookup"><span data-stu-id="956eb-205">JavaScript and CSS often undergo some form of bundling or minification before being sent to the client.</span></span> <span data-ttu-id="956eb-206">En los formularios Web Forms de ASP.NET, estas optimizaciones se controlan en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="956eb-206">In ASP.NET Web Forms, these optimizations are handled at runtime.</span></span> <span data-ttu-id="956eb-207">Las convenciones de optimización se definen como un archivo *App_Start/bundleconfig.CS* .</span><span class="sxs-lookup"><span data-stu-id="956eb-207">The optimization conventions are defined an *App_Start/BundleConfig.cs* file.</span></span> <span data-ttu-id="956eb-208">En ASP.NET Core, se adopta un enfoque más declarativo.</span><span class="sxs-lookup"><span data-stu-id="956eb-208">In ASP.NET Core, a more declarative approach is adopted.</span></span> <span data-ttu-id="956eb-209">Un archivo enumera los archivos que se van a reducida, junto con la configuración específica de minificación.</span><span class="sxs-lookup"><span data-stu-id="956eb-209">A file lists the files to be minified, along with specific minification settings.</span></span>

<span data-ttu-id="956eb-210">Para obtener más información sobre la agrupación y la minificación, consulte [agrupación y minimizar de recursos estáticos en ASP.net Core](/aspnet/core/client-side/bundling-and-minification).</span><span class="sxs-lookup"><span data-stu-id="956eb-210">For more information on bundling and minification, see [Bundle and minify static assets in ASP.NET Core](/aspnet/core/client-side/bundling-and-minification).</span></span>

## <a name="migrate-aspx-pages"></a><span data-ttu-id="956eb-211">Migrar páginas ASPX</span><span class="sxs-lookup"><span data-stu-id="956eb-211">Migrate ASPX pages</span></span>

<span data-ttu-id="956eb-212">Una página de una aplicación de formularios Web Forms es un archivo con la extensión *. aspx* .</span><span class="sxs-lookup"><span data-stu-id="956eb-212">A page in a Web Forms app is a file with the *.aspx* extension.</span></span> <span data-ttu-id="956eb-213">Una página de formularios Web Forms a menudo se puede asignar a un componente en Blazor .</span><span class="sxs-lookup"><span data-stu-id="956eb-213">A Web Forms page can often be mapped to a component in Blazor.</span></span> <span data-ttu-id="956eb-214">Un Blazor componente se crea en un archivo con la extensión *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="956eb-214">A Blazor component is authored in a file with the *.razor* extension.</span></span> <span data-ttu-id="956eb-215">En el caso del proyecto de eShop, se convierten cinco páginas en una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="956eb-215">For the eShop project, five pages are converted to a Razor page.</span></span>

<span data-ttu-id="956eb-216">Por ejemplo, la vista de detalles consta de tres archivos en el proyecto de formularios Web Forms: *details. aspx*, *details.aspx.CS* y *details.aspx.Designer.CS*.</span><span class="sxs-lookup"><span data-stu-id="956eb-216">For example, the details view comprises three files in the Web Forms project: *Details.aspx*, *Details.aspx.cs*, and *Details.aspx.designer.cs*.</span></span> <span data-ttu-id="956eb-217">Al convertir a Blazor , el código subyacente y el marcado se combinan en *details. Razor*.</span><span class="sxs-lookup"><span data-stu-id="956eb-217">When converting to Blazor, the code-behind and markup are combined into *Details.razor*.</span></span> <span data-ttu-id="956eb-218">La compilación de Razor (equivalente a lo que se encuentra en los archivos *. Designer.CS* ) se almacena en el directorio *obj* y no es visible de forma predeterminada en **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="956eb-218">Razor compilation (equivalent to what's in *.designer.cs* files) is stored in the *obj* directory and isn't, by default, viewable in **Solution Explorer**.</span></span> <span data-ttu-id="956eb-219">La página de formularios Web Forms consta del siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="956eb-219">The Web Forms page consists of the following markup:</span></span>

```aspx-csharp
<%@ Page Title="Details" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Details.aspx.cs" Inherits="eShopLegacyWebForms.Catalog.Details" %>

<asp:Content ID="Details" ContentPlaceHolderID="MainContent" runat="server">
    <h2 class="esh-body-title">Details</h2>

    <div class="container">
        <div class="row">
            <asp:Image runat="server" CssClass="col-md-6 esh-picture" ImageUrl='<%#"/Pics/" + product.PictureFileName%>' />
            <dl class="col-md-6 dl-horizontal">
                <dt>Name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Name%>' />
                </dd>

                <dt>Description
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.Description%>' />
                </dd>

                <dt>Brand
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogBrand.Brand%>' />
                </dd>

                <dt>Type
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.CatalogType.Type%>' />
                </dd>
                <dt>Price
                </dt>

                <dd>
                    <asp:Label CssClass="esh-price" runat="server" Text='<%#product.Price%>' />
                </dd>

                <dt>Picture name
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.PictureFileName%>' />
                </dd>

                <dt>Stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.AvailableStock%>' />
                </dd>

                <dt>Restock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.RestockThreshold%>' />
                </dd>

                <dt>Max stock
                </dt>

                <dd>
                    <asp:Label runat="server" Text='<%#product.MaxStockThreshold%>' />
                </dd>

            </dl>
        </div>

        <div class="form-actions no-color esh-link-list">
            <a runat="server" href='<%# GetRouteUrl("EditProductRoute", new {id =product.Id}) %>' class="esh-link-item">Edit
            </a>
            |
            <a runat="server" href="~" class="esh-link-item">Back to list
            </a>
        </div>

    </div>
</asp:Content>
```

<span data-ttu-id="956eb-220">El código subyacente del marcado anterior incluye el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="956eb-220">The preceding markup's code-behind includes the following code:</span></span>

```csharp
using eShopLegacyWebForms.Models;
using eShopLegacyWebForms.Services;
using log4net;
using System;
using System.Web.UI;

namespace eShopLegacyWebForms.Catalog
{
    public partial class Details : System.Web.UI.Page
    {
        private static readonly ILog _log = LogManager.GetLogger(System.Reflection.MethodBase.GetCurrentMethod().DeclaringType);

        protected CatalogItem product;

        public ICatalogService CatalogService { get; set; }

        protected void Page_Load(object sender, EventArgs e)
        {
            var productId = Convert.ToInt32(Page.RouteData.Values["id"]);
            _log.Info($"Now loading... /Catalog/Details.aspx?id={productId}");
            product = CatalogService.FindCatalogItem(productId);

            this.DataBind();
        }
    }
}
```

<span data-ttu-id="956eb-221">Cuando se convierte en Blazor , la página de formularios Web Forms se convierte en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="956eb-221">When converted to Blazor, the Web Forms page translates to the following code:</span></span>

```razor
@page "/Catalog/Details/{id:int}"
@inject ICatalogService CatalogService
@inject ILogger<Details> Logger

<h2 class="esh-body-title">Details</h2>

<div class="container">
    <div class="row">
        <img class="col-md-6 esh-picture" src="@($"/Pics/{_item.PictureFileName}")">

        <dl class="col-md-6 dl-horizontal">
            <dt>
                Name
            </dt>

            <dd>
                @_item.Name
            </dd>

            <dt>
                Description
            </dt>

            <dd>
                @_item.Description
            </dd>

            <dt>
                Brand
            </dt>

            <dd>
                @_item.CatalogBrand.Brand
            </dd>

            <dt>
                Type
            </dt>

            <dd>
                @_item.CatalogType.Type
            </dd>
            <dt>
                Price
            </dt>

            <dd>
                @_item.Price
            </dd>

            <dt>
                Picture name
            </dt>

            <dd>
                @_item.PictureFileName
            </dd>

            <dt>
                Stock
            </dt>

            <dd>
                @_item.AvailableStock
            </dd>

            <dt>
                Restock
            </dt>

            <dd>
                @_item.RestockThreshold
            </dd>

            <dt>
                Max stock
            </dt>

            <dd>
                @_item.MaxStockThreshold
            </dd>

        </dl>
    </div>

    <div class="form-actions no-color esh-link-list">
        <a href="@($"/Catalog/Edit/{_item.Id}")" class="esh-link-item">
            Edit
        </a>
        |
        <a href="/" class="esh-link-item">
            Back to list
        </a>
    </div>

</div>

@code {
    private CatalogItem _item;

    [Parameter]
    public int Id { get; set; }

    protected override void OnInitialized()
    {
        Logger.LogInformation("Now loading... /Catalog/Details/{Id}", Id);

        _item = CatalogService.FindCatalogItem(Id);
    }
}
```

<span data-ttu-id="956eb-222">Observe que el código y el marcado están en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="956eb-222">Notice that the code and markup are in the same file.</span></span> <span data-ttu-id="956eb-223">Los servicios necesarios se hacen accesibles con el `@inject` atributo.</span><span class="sxs-lookup"><span data-stu-id="956eb-223">Any required services are made accessible with the `@inject` attribute.</span></span> <span data-ttu-id="956eb-224">Según la `@page` Directiva, se puede tener acceso a esta página en la `Catalog/Details/{id}` ruta.</span><span class="sxs-lookup"><span data-stu-id="956eb-224">Per the `@page` directive, this page can be accessed at the `Catalog/Details/{id}` route.</span></span> <span data-ttu-id="956eb-225">El valor del marcador de posición de la ruta se ha `{id}` restringido a un entero.</span><span class="sxs-lookup"><span data-stu-id="956eb-225">The value of the route's `{id}` placeholder has been constrained to an integer.</span></span> <span data-ttu-id="956eb-226">Como se describe en la sección [enrutamiento](pages-routing-layouts.md) , a diferencia de los formularios Web Forms, un componente Razor indica explícitamente su ruta y los parámetros que se incluyen.</span><span class="sxs-lookup"><span data-stu-id="956eb-226">As described in the [routing](pages-routing-layouts.md) section, unlike Web Forms, a Razor component explicitly states its route and any parameters that are included.</span></span> <span data-ttu-id="956eb-227">Es posible que muchos controles de formularios Web Forms no tengan homólogos exactos en Blazor .</span><span class="sxs-lookup"><span data-stu-id="956eb-227">Many Web Forms controls may not have exact counterparts in Blazor.</span></span> <span data-ttu-id="956eb-228">A menudo hay un fragmento de código HTML equivalente que tendrá el mismo propósito.</span><span class="sxs-lookup"><span data-stu-id="956eb-228">There's often an equivalent HTML snippet that will serve the same purpose.</span></span> <span data-ttu-id="956eb-229">Por ejemplo, el `<asp:Label />` control se puede reemplazar por un `<label>` elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="956eb-229">For example, the `<asp:Label />` control can be replaced with an HTML `<label>` element.</span></span>

### <a name="model-validation-in-no-locblazor"></a><span data-ttu-id="956eb-230">Validación de modelos en Blazor</span><span class="sxs-lookup"><span data-stu-id="956eb-230">Model validation in Blazor</span></span>

<span data-ttu-id="956eb-231">Si el código de formularios Web Forms incluye validación, puede transferir gran parte de lo que tiene con cambios poco a no.</span><span class="sxs-lookup"><span data-stu-id="956eb-231">If your Web Forms code includes validation, you can transfer much of what you have with little-to-no changes.</span></span> <span data-ttu-id="956eb-232">Una ventaja de ejecutar en Blazor es que se puede ejecutar la misma lógica de validación sin necesidad de JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="956eb-232">A benefit to running in Blazor is that the same validation logic can be run without needing custom JavaScript.</span></span> <span data-ttu-id="956eb-233">Las anotaciones de datos permiten la validación sencilla del modelo.</span><span class="sxs-lookup"><span data-stu-id="956eb-233">Data annotations enable easy model validation.</span></span>

<span data-ttu-id="956eb-234">Por ejemplo, la página *Create. aspx* tiene un formulario de entrada de datos con validación.</span><span class="sxs-lookup"><span data-stu-id="956eb-234">For example, the *Create.aspx* page has a data entry form with validation.</span></span> <span data-ttu-id="956eb-235">Un fragmento de código de ejemplo tendría el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="956eb-235">An example snippet would look like this:</span></span>

```aspx
<div class="form-group">
    <label class="control-label col-md-2">Name</label>
    <div class="col-md-3">
        <asp:TextBox ID="Name" runat="server" CssClass="form-control"></asp:TextBox>
        <asp:RequiredFieldValidator runat="server" ControlToValidate="Name" Display="Dynamic"
            CssClass="field-validation-valid text-danger" ErrorMessage="The Name field is required." />
    </div>
</div>
```

<span data-ttu-id="956eb-236">En Blazor , el marcado equivalente se proporciona en un archivo *Create. Razor* :</span><span class="sxs-lookup"><span data-stu-id="956eb-236">In Blazor, the equivalent markup is provided in a *Create.razor* file:</span></span>

```razor
<EditForm Model="_item" OnValidSubmit="@...">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="control-label col-md-2">Name</label>
        <div class="col-md-3">
            <InputText class="form-control" @bind-Value="_item.Name" />
            <ValidationMessage For="(() => _item.Name)" />
        </div>
    </div>

    ...
</EditForm>
```

<span data-ttu-id="956eb-237">El `EditForm` contexto incluye compatibilidad con la validación y se puede ajustar en torno a una entrada.</span><span class="sxs-lookup"><span data-stu-id="956eb-237">The `EditForm` context includes validation support and can be wrapped around an input.</span></span> <span data-ttu-id="956eb-238">Las anotaciones de datos son una manera común de agregar validación.</span><span class="sxs-lookup"><span data-stu-id="956eb-238">Data annotations are a common way to add validation.</span></span> <span data-ttu-id="956eb-239">Esta compatibilidad de validación se puede Agregar a través del `DataAnnotationsValidator` componente.</span><span class="sxs-lookup"><span data-stu-id="956eb-239">Such validation support can be added via the `DataAnnotationsValidator` component.</span></span> <span data-ttu-id="956eb-240">Para obtener más información sobre este mecanismo, consulte [ASP.net Core Blazor Forms and Validation](/aspnet/core/blazor/forms-validation).</span><span class="sxs-lookup"><span data-stu-id="956eb-240">For more information on this mechanism, see [ASP.NET Core Blazor forms and validation](/aspnet/core/blazor/forms-validation).</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="956eb-241">Migración de la configuración</span><span class="sxs-lookup"><span data-stu-id="956eb-241">Migrate configuration</span></span>

<span data-ttu-id="956eb-242">En un proyecto de formularios Web Forms, los datos de configuración se almacenan normalmente en el archivo *web.config* .</span><span class="sxs-lookup"><span data-stu-id="956eb-242">In a Web Forms project, configuration data is most commonly stored in the *web.config* file.</span></span> <span data-ttu-id="956eb-243">Se tiene acceso a los datos de configuración con `ConfigurationManager` .</span><span class="sxs-lookup"><span data-stu-id="956eb-243">The configuration data is accessed with `ConfigurationManager`.</span></span> <span data-ttu-id="956eb-244">A menudo, los servicios debían analizar objetos.</span><span class="sxs-lookup"><span data-stu-id="956eb-244">Services were often required to parse objects.</span></span> <span data-ttu-id="956eb-245">Con .NET Framework 4.7.2, composición se ha agregado a la configuración a través de `ConfigurationBuilders` .</span><span class="sxs-lookup"><span data-stu-id="956eb-245">With .NET Framework 4.7.2, composability was added to the configuration via `ConfigurationBuilders`.</span></span> <span data-ttu-id="956eb-246">Estos generadores permitían a los desarrolladores agregar varios orígenes para la configuración que luego se compone en tiempo de ejecución para recuperar los valores necesarios.</span><span class="sxs-lookup"><span data-stu-id="956eb-246">These builders allowed developers to add various sources for the configuration that was then composed at runtime to retrieve the necessary values.</span></span>

<span data-ttu-id="956eb-247">ASP.NET Core presentó un sistema de configuración flexible que le permite definir el origen de configuración o los orígenes usados por la aplicación y la implementación.</span><span class="sxs-lookup"><span data-stu-id="956eb-247">ASP.NET Core introduced a flexible configuration system that allows you to define the configuration source or sources used by your app and deployment.</span></span> <span data-ttu-id="956eb-248">La `ConfigurationBuilder` infraestructura que puede usar en la aplicación de formularios Web Forms se modelaba después de los conceptos usados en el sistema de configuración de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-248">The `ConfigurationBuilder` infrastructure that you may be using in your Web Forms app was modeled after the concepts used in the ASP.NET Core configuration system.</span></span>

<span data-ttu-id="956eb-249">El siguiente fragmento de código muestra cómo el proyecto de eShop de formularios Web Forms usa *web.config* para almacenar los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="956eb-249">The following snippet demonstrates how the Web Forms eShop project uses *web.config* to store configuration values:</span></span>

```xml
<configuration>
  <configSections>
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="CatalogDBContext" connectionString="Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
  </connectionStrings>
  <appSettings>
    <add key="UseMockData" value="true" />
    <add key="UseCustomizationData" value="false" />
  </appSettings>
</configuration>
```

<span data-ttu-id="956eb-250">Es habitual que los secretos, como las cadenas de conexión de base de datos, se almacenen en el *web.config*. Los secretos se conservan inevitablemente en ubicaciones no seguras, como el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="956eb-250">It's common for secrets, such as database connection strings, to be stored within the *web.config*. The secrets are inevitably persisted in unsecure locations, such as source control.</span></span> <span data-ttu-id="956eb-251">Con Blazor en ASP.net Core, la configuración basada en XML anterior se reemplaza por el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="956eb-251">With Blazor on ASP.NET Core, the preceding XML-based configuration is replaced with the following JSON:</span></span>

```json
{
  "ConnectionStrings": {
    "CatalogDBContext": "Data Source=(localdb)\\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;"
  },
  "UseMockData": true,
  "UseCustomizationData": false
}
```

<span data-ttu-id="956eb-252">JSON es el formato de configuración predeterminado; sin embargo, ASP.NET Core admite muchos otros formatos, incluido XML.</span><span class="sxs-lookup"><span data-stu-id="956eb-252">JSON is the default configuration format; however, ASP.NET Core supports many other formats, including XML.</span></span> <span data-ttu-id="956eb-253">También hay varios formatos admitidos por la comunidad.</span><span class="sxs-lookup"><span data-stu-id="956eb-253">There are also several community-supported formats.</span></span>

<span data-ttu-id="956eb-254">El constructor de la Blazor clase del proyecto `Startup` acepta una `IConfiguration` instancia a través de una técnica de di conocida como inserción del constructor:</span><span class="sxs-lookup"><span data-stu-id="956eb-254">The constructor in the Blazor project's `Startup` class accepts an `IConfiguration` instance through a DI technique known as constructor injection:</span></span>

```csharp
public class Startup
{
    public Startup(IConfiguration configuration, IWebHostEnvironment env)
    {
        Configuration = configuration;
        Env = env;
    }

    ...
}
```

<span data-ttu-id="956eb-255">De forma predeterminada, las variables de entorno, los archivos JSON (*appsettings.jsen* y *appSettings. { Environment}. JSON*) y las opciones de línea de comandos se registran como orígenes de configuración válidos en el objeto de configuración.</span><span class="sxs-lookup"><span data-stu-id="956eb-255">By default, environment variables, JSON files (*appsettings.json* and *appsettings.{Environment}.json*), and command-line options are registered as valid configuration sources in the configuration object.</span></span> <span data-ttu-id="956eb-256">Se puede tener acceso a los orígenes de configuración a través de `Configuration[key]` .</span><span class="sxs-lookup"><span data-stu-id="956eb-256">The configuration sources can be accessed via `Configuration[key]`.</span></span> <span data-ttu-id="956eb-257">Una técnica más avanzada consiste en enlazar los datos de configuración a los objetos mediante el patrón de opciones.</span><span class="sxs-lookup"><span data-stu-id="956eb-257">A more advanced technique is to bind the configuration data to objects using the options pattern.</span></span> <span data-ttu-id="956eb-258">Para obtener más información sobre la configuración y el patrón de opciones, vea [configuración en ASP.net Core](/aspnet/core/fundamentals/configuration/) y [patrón de opciones en ASP.net Core](/aspnet/core/fundamentals/configuration/options), respectivamente.</span><span class="sxs-lookup"><span data-stu-id="956eb-258">For more information on configuration and the options pattern, see [Configuration in ASP.NET Core](/aspnet/core/fundamentals/configuration/) and [Options pattern in ASP.NET Core](/aspnet/core/fundamentals/configuration/options), respectively.</span></span>

## <a name="migrate-data-access"></a><span data-ttu-id="956eb-259">Migración del acceso a datos</span><span class="sxs-lookup"><span data-stu-id="956eb-259">Migrate data access</span></span>

<span data-ttu-id="956eb-260">El acceso a datos es un aspecto importante de cualquier aplicación.</span><span class="sxs-lookup"><span data-stu-id="956eb-260">Data access is an important aspect of any app.</span></span> <span data-ttu-id="956eb-261">El proyecto de eShop almacena información de catálogo en una base de datos y recupera los datos con Entity Framework (EF) 6.</span><span class="sxs-lookup"><span data-stu-id="956eb-261">The eShop project stores catalog information in a database and retrieves the data with Entity Framework (EF) 6.</span></span> <span data-ttu-id="956eb-262">Dado que EF 6 es compatible con .NET 5,0, el proyecto puede seguir utilizándolo.</span><span class="sxs-lookup"><span data-stu-id="956eb-262">Since EF 6 is supported in .NET 5.0, the project can continue to use it.</span></span>

<span data-ttu-id="956eb-263">Los siguientes cambios relacionados con EF eran necesarios para eShop:</span><span class="sxs-lookup"><span data-stu-id="956eb-263">The following EF-related changes were necessary to eShop:</span></span>

- <span data-ttu-id="956eb-264">En .NET Framework, el `DbContext` objeto acepta una cadena con el formato *Name = ConnectionString* y utiliza la cadena de conexión de `ConfigurationManager.AppSettings[ConnectionString]` para conectarse.</span><span class="sxs-lookup"><span data-stu-id="956eb-264">In .NET Framework, the `DbContext` object accepts a string of the form *name=ConnectionString* and uses the connection string from `ConfigurationManager.AppSettings[ConnectionString]` to connect.</span></span> <span data-ttu-id="956eb-265">No se admite en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-265">In .NET Core, this isn't supported.</span></span> <span data-ttu-id="956eb-266">Se debe proporcionar la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="956eb-266">The connection string must be supplied.</span></span>
- <span data-ttu-id="956eb-267">Se ha tenido acceso a la base de datos de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="956eb-267">The database was accessed in a synchronous way.</span></span> <span data-ttu-id="956eb-268">Aunque esto funciona, la escalabilidad puede verse afectada.</span><span class="sxs-lookup"><span data-stu-id="956eb-268">Though this works, scalability may suffer.</span></span> <span data-ttu-id="956eb-269">Esta lógica debe moverse a un patrón asincrónico.</span><span class="sxs-lookup"><span data-stu-id="956eb-269">This logic should be moved to an asynchronous pattern.</span></span>

<span data-ttu-id="956eb-270">Aunque no hay la misma compatibilidad nativa para el enlace de conjuntos de DataSet, Blazor proporciona flexibilidad y capacidad con su compatibilidad con C# en una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="956eb-270">Although there isn't the same native support for dataset binding, Blazor provides flexibility and power with its C# support in a Razor page.</span></span> <span data-ttu-id="956eb-271">Por ejemplo, puede realizar cálculos y mostrar el resultado.</span><span class="sxs-lookup"><span data-stu-id="956eb-271">For example, you can perform calculations and display the result.</span></span> <span data-ttu-id="956eb-272">Para obtener más información sobre los patrones de datos en Blazor , vea el capítulo de [acceso a datos](data.md) .</span><span class="sxs-lookup"><span data-stu-id="956eb-272">For more information on data patterns in Blazor, see the [Data access](data.md) chapter.</span></span>

## <a name="architectural-changes"></a><span data-ttu-id="956eb-273">Cambios de arquitectura</span><span class="sxs-lookup"><span data-stu-id="956eb-273">Architectural changes</span></span>

<span data-ttu-id="956eb-274">Por último, hay algunas diferencias arquitectónicas importantes a tener en cuenta a la hora de migrar a Blazor .</span><span class="sxs-lookup"><span data-stu-id="956eb-274">Finally, there are some important architectural differences to consider when migrating to Blazor.</span></span> <span data-ttu-id="956eb-275">Muchos de estos cambios se aplican a cualquier elemento basado en .NET Core o en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-275">Many of these changes are applicable to anything based on .NET Core or ASP.NET Core.</span></span>

<span data-ttu-id="956eb-276">Dado que Blazor se basa en .net Core, existen consideraciones para garantizar la compatibilidad en .net Core.</span><span class="sxs-lookup"><span data-stu-id="956eb-276">Because Blazor is built on .NET Core, there are considerations in ensuring support on .NET Core.</span></span> <span data-ttu-id="956eb-277">Algunos de los cambios principales incluyen la eliminación de las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="956eb-277">Some of the major changes include the removal of the following features:</span></span>

- <span data-ttu-id="956eb-278">Varios AppDomains</span><span class="sxs-lookup"><span data-stu-id="956eb-278">Multiple AppDomains</span></span>
- <span data-ttu-id="956eb-279">Comunicación remota</span><span class="sxs-lookup"><span data-stu-id="956eb-279">Remoting</span></span>
- <span data-ttu-id="956eb-280">Seguridad de acceso del código (CAS)</span><span class="sxs-lookup"><span data-stu-id="956eb-280">Code Access Security (CAS)</span></span>
- <span data-ttu-id="956eb-281">Transparencia de seguridad</span><span class="sxs-lookup"><span data-stu-id="956eb-281">Security Transparency</span></span>

<span data-ttu-id="956eb-282">Para obtener más información sobre las técnicas para identificar los cambios necesarios para admitir la ejecución en .NET Core, vea [portar el código de .NET Framework a .net Core](../../core/porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="956eb-282">For more information on techniques to identify necessary changes to support running on .NET Core, see [Port your code from .NET Framework to .NET Core](../../core/porting/index.md).</span></span>

<span data-ttu-id="956eb-283">ASP.NET Core es una versión Reimaginada de ASP.NET y tiene algunos cambios que es posible que no parezcan inicialmente obvios.</span><span class="sxs-lookup"><span data-stu-id="956eb-283">ASP.NET Core is a reimagined version of ASP.NET and has some changes that may not initially seem obvious.</span></span> <span data-ttu-id="956eb-284">Los principales cambios son:</span><span class="sxs-lookup"><span data-stu-id="956eb-284">The main changes are:</span></span>

- <span data-ttu-id="956eb-285">Sin contexto de sincronización, lo que significa que no hay ningún `HttpContext.Current` , `Thread.CurrentPrincipal` u otros descriptores de acceso estáticos</span><span class="sxs-lookup"><span data-stu-id="956eb-285">No synchronization context, which means there's no `HttpContext.Current`, `Thread.CurrentPrincipal`, or other static accessors</span></span>
- <span data-ttu-id="956eb-286">Sin copias sombra</span><span class="sxs-lookup"><span data-stu-id="956eb-286">No shadow copying</span></span>
- <span data-ttu-id="956eb-287">Sin cola de solicitudes</span><span class="sxs-lookup"><span data-stu-id="956eb-287">No request queue</span></span>

<span data-ttu-id="956eb-288">Muchas operaciones en ASP.NET Core son asincrónicas, lo que permite una descarga más sencilla de las tareas enlazadas a e/s.</span><span class="sxs-lookup"><span data-stu-id="956eb-288">Many operations in ASP.NET Core are asynchronous, which allows easier off-loading of I/O-bound tasks.</span></span> <span data-ttu-id="956eb-289">Es importante no bloquear nunca mediante `Task.Wait()` o `Task.GetResult()` , lo que puede agotar rápidamente los recursos del grupo de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="956eb-289">It's important to never block by using `Task.Wait()` or `Task.GetResult()`, which can quickly exhaust thread pool resources.</span></span>

## <a name="migration-conclusion"></a><span data-ttu-id="956eb-290">Conclusión de la migración</span><span class="sxs-lookup"><span data-stu-id="956eb-290">Migration conclusion</span></span>

<span data-ttu-id="956eb-291">En este punto, ha visto muchos ejemplos de lo que se necesita para trasladar un proyecto de formularios Web Forms a Blazor .</span><span class="sxs-lookup"><span data-stu-id="956eb-291">At this point, you've seen many examples of what it takes to move a Web Forms project to Blazor.</span></span> <span data-ttu-id="956eb-292">Para obtener un ejemplo completo, vea el proyecto [eShopOn Blazor ](https://github.com/dotnet-architecture/eShopOnBlazor) .</span><span class="sxs-lookup"><span data-stu-id="956eb-292">For a full example, see the [eShopOnBlazor](https://github.com/dotnet-architecture/eShopOnBlazor) project.</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="956eb-293">Anterior</span><span class="sxs-lookup"><span data-stu-id="956eb-293">Previous</span></span>](security-authentication-authorization.md)
