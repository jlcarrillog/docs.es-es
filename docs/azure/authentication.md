---
title: Descripción de la autenticación en las bibliotecas de Azure para .NET
description: Explica las distintas formas de autenticarse con el SDK de Azure para .NET.
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: 5ed29d5485dc7f59bcc757c8903116edf6bd5d7d
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174886"
---
# <a name="authenticate-with-the-azure-sdk-for-net"></a><span data-ttu-id="2b9e9-103">Autenticación con SDK de Azure para .NET</span><span class="sxs-lookup"><span data-stu-id="2b9e9-103">Authenticate with the Azure SDK for .NET</span></span>

## <a name="recommended-azureidentity"></a><span data-ttu-id="2b9e9-104">Se recomienda: Azure.Identity</span><span class="sxs-lookup"><span data-stu-id="2b9e9-104">Recommended: Azure.Identity</span></span>

<span data-ttu-id="2b9e9-105">Los paquetes más recientes de SDK de Azure para .NET usan un paquete de autenticación común para autenticarse, `Azure.Identity`.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-105">The latest packages in the Azure SDK for .NET use a common authentication package to authenticate, `Azure.Identity`.</span></span> <span data-ttu-id="2b9e9-106">Se recomienda usar `Azure.Identity` sobre otros mecanismos de autenticación que se describen más adelante en este documento.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-106">Using `Azure.Identity` is recommended over other authentication mechanisms described later in this document.</span></span> <span data-ttu-id="2b9e9-107">Los paquetes que admiten las credenciales proporcionadas por `Azure.Identity` tienen identificadores de paquete que comienzan por *Azure*.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-107">Packages supporting the credentials provided by `Azure.Identity` have package identifiers starting with *Azure.*</span></span> <span data-ttu-id="2b9e9-108">[Para obtener más información, consulte las versiones más recientes de SDK de Azure para .NET](https://azure.github.io/azure-sdk/releases/latest/index.html#net).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-108">[For more information, see the latest releases in the Azure SDK for .NET](https://azure.github.io/azure-sdk/releases/latest/index.html#net).</span></span>

<span data-ttu-id="2b9e9-109">Para obtener instrucciones completas sobre cómo usar `Azure.Identity` en el proyecto, consulte la documentación de [Azure Identity Client para .NET](/dotnet/api/overview/azure/identity-readme).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-109">For complete instructions on using `Azure.Identity` in your project, see the documentation for [Azure Identity client for .NET](/dotnet/api/overview/azure/identity-readme).</span></span>

> [!TIP]
> <span data-ttu-id="2b9e9-110">Consulte la [muestra de Azure Identity, Resource Management y Storage](/samples/dotnet/samples/azure-identity-resource-management-storage/) para obtener ejemplos del uso de Azure Identity para administrar y acceder a los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-110">See the [Azure Identity, Resource Management, and Storage sample](/samples/dotnet/samples/azure-identity-resource-management-storage/) for examples of using Azure Identity to manage and access Azure resources.</span></span>

<span data-ttu-id="2b9e9-111">Para autenticar con bibliotecas que no son compatibles con Azure.Identity, consulte el resto de este tema.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-111">To authenticate with libraries that don't support Azure.Identity, see the rest of this topic.</span></span>

## <a name="access-azure-resources"></a><span data-ttu-id="2b9e9-112">Acceso a recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="2b9e9-112">Access Azure resources</span></span>

<span data-ttu-id="2b9e9-113">Para interactuar con los recursos de Azure, como recuperar un secreto de Key Vault o almacenar un blob en Storage, muchas bibliotecas de servicios de Azure requieren una cadena de conexión o claves para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-113">To interact with Azure resources, such as retrieving a secret from Key Vault or storing a blob in Storage, many Azure service libraries require a connection string or keys for authentication.</span></span> <span data-ttu-id="2b9e9-114">Por ejemplo, SQL Database usa una [cadena de conexión SQL estándar](https://docs.microsoft.com/azure/azure-sql/database/connect-query-dotnet-core).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-114">For example, SQL Database uses a [standard SQL connection string](https://docs.microsoft.com/azure/azure-sql/database/connect-query-dotnet-core).</span></span> <span data-ttu-id="2b9e9-115">Las cadenas de conexión de servicio se usan en otros servicios de Azure como [CosmosDB](/azure/cosmos-db/), [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache) y [Service Bus](/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-115">Service connection strings are used in other Azure services like [CosmosDB](/azure/cosmos-db/), [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-dotnet-how-to-use-azure-redis-cache), and [Service Bus](/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues).</span></span> <span data-ttu-id="2b9e9-116">Puede obtener esas cadenas mediante Azure Portal, la CLI o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-116">You can get those strings using the Azure portal, CLI, or PowerShell.</span></span> <span data-ttu-id="2b9e9-117">También puede usar las bibliotecas de administración de Azure para .NET para consultar recursos para generar cadenas de conexión en el código.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-117">You can also use the Azure management libraries for .NET to query resources to build connection strings in your code.</span></span>

<span data-ttu-id="2b9e9-118">Los métodos para usar una cadena de conexión varían según el producto.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-118">The methods for using a connection string vary by product.</span></span> <span data-ttu-id="2b9e9-119">[Consulte la documentación de su producto Azure](/azure/?product=featured).</span><span class="sxs-lookup"><span data-stu-id="2b9e9-119">[Refer to the documentation for your Azure product](/azure/?product=featured).</span></span>

## <a name="manage-azure-resources"></a><span data-ttu-id="2b9e9-120">Administración de recursos de Azure</span><span class="sxs-lookup"><span data-stu-id="2b9e9-120">Manage Azure resources</span></span>

[!include[Create service principal](includes/create-sp.md)]

<span data-ttu-id="2b9e9-121">Ahora que ya está creada la entidad de servicio, hay dos opciones disponibles para autenticarse en la entidad de servicio para crear y administrar los recursos.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-121">Now that the service principal is created, two options are available to authenticate to the service principal to create and manage resources.</span></span>

<span data-ttu-id="2b9e9-122">Para las dos opciones, tendrá que agregar los siguientes paquetes NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-122">For both options you will need to add the following NuGet packages to your project.</span></span>

```powershell
Install-Package Microsoft.Azure.Management.Fluent
Install-Package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="authenticate-with-token-credentials"></a><span data-ttu-id="2b9e9-123">Autenticación con credenciales de token</span><span class="sxs-lookup"><span data-stu-id="2b9e9-123">Authenticate with token credentials</span></span>

<span data-ttu-id="2b9e9-124">El primer método consiste en generar el objeto de credencial de token en el código.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-124">The first method is to build the token credential object in code.</span></span> <span data-ttu-id="2b9e9-125">Debe almacenar las credenciales de forma segura en un archivo de configuración, el Registro o Azure KeyVault.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-125">You should store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

```csharp
var credentials = SdkContext.AzureCredentialsFactory
    .FromServicePrincipal(clientId,
        clientSecret,
        tenantId,
        AzureEnvironment.AzureGlobalCloud);
```

<span data-ttu-id="2b9e9-126">Use los valores de *clientId*, *clientSecret* y *tenantId* de la salida JSON de la creación de la entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-126">Use the *clientId*, *clientSecret*, and *tenantId* values from the JSON output when you created the service principal.</span></span>

<span data-ttu-id="2b9e9-127">Después, cree el objeto `Azure` de punto de entrada para empezar a trabajar con la API:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-127">Then create the entry point `Azure` object to start working with the API:</span></span>

```csharp
var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```

### <a name="file-based-authentication"></a><a name="mgmt-file"></a><span data-ttu-id="2b9e9-128">Autenticación basada en archivo</span><span class="sxs-lookup"><span data-stu-id="2b9e9-128">File-based authentication</span></span>

<span data-ttu-id="2b9e9-129">La autenticación basada en archivo permite colocar las credenciales de la entidad de servicio en un archivo de texto sin formato y protegerlo en el sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="2b9e9-129">File-based authentication allows you to put the service principal credentials in a plain text file and secure it within the file system.</span></span>

[!include[File-based authentication](includes/file-based-auth.md)]

<span data-ttu-id="2b9e9-130">Lea el contenido del archivo y cree el objeto `Azure` de punto de entrada para empezar a trabajar con la API:</span><span class="sxs-lookup"><span data-stu-id="2b9e9-130">Read the contents of the file and create the entry point `Azure` object to start working with the API:</span></span>

```csharp
// pull in the location of the authentication properties file from the environment
var credentials = SdkContext.AzureCredentialsFactory
    .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

var azure = Microsoft.Azure.Management.Fluent.Azure
    .Configure()
    .Authenticate(credentials)
    .WithDefaultSubscription();
```