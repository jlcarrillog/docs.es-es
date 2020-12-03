---
title: 'Cambio importante: Azure: los paquetes de integración de Azure con Microsoft como prefijo se han quitado'
description: 'Obtenga información sobre el cambio importante en ASP.NET Core 5.0 titulado Azure: los paquetes de integración de Azure con Microsoft como prefijo se han quitado'
author: scottaddie
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: b9f685b8c9fdcd73044f78840f2732809a0de710
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95760256"
---
# <a name="azure-microsoft-prefixed-azure-integration-packages-removed"></a>Azure: los paquetes de integración de Azure con Microsoft como prefijo se han quitado

Los siguientes paquetes `Microsoft.*` que proporcionan la integración entre ASP.NET Core y SDK de Azure no se incluyen en ASP.NET Core 5.0:

* [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/), que integra [Azure Key Vault](/azure/key-vault/) en el [sistema de configuración](/aspnet/core/fundamentals/configuration/).
* [Microsoft.AspNetCore.DataProtection.AzureKeyVault](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureKeyVault/), que integra Azure Key Vault en el [sistema de protección de datos de ASP.NET Core](/aspnet/core/security/data-protection/introduction).
* [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/), que integra [Azure Blob Storage](/azure/storage/blobs/) en el sistema de protección de datos de ASP.NET Core.

Para obtener información sobre esta incidencia, consulte [dotnet/aspnetcore#19570](https://github.com/dotnet/aspnetcore/issues/19570).

## <a name="version-introduced"></a>Versión introducida

5.0 (versión preliminar 1)

## <a name="old-behavior"></a>Comportamiento anterior

Los paquetes `Microsoft.*` integraban servicios de Azure con las API de protección de datos y configuración.

## <a name="new-behavior"></a>Comportamiento nuevo

Los nuevos paquetes `Azure.*` integran servicios de Azure con las API de protección de datos y configuración.

## <a name="reason-for-change"></a>Motivo del cambio

El cambio se realizó porque los paquetes `Microsoft.*`:

* Estaban usando versiones obsoletas del SDK de Azure. Las actualizaciones sencillas no eran posibles porque las nuevas versiones del SDK de Azure incluían cambios importantes.
* Estaban vinculados a la programación de versión de .NET Core. La transferencia de la propiedad de los paquetes al equipo del SDK de Azure habilita las actualizaciones de paquetes a medida que se actualiza el SDK de Azure.

## <a name="recommended-action"></a>Acción recomendada

En ASP.NET Core 2.1 o proyectos posteriores, reemplace el `Microsoft.*` anterior por los nuevos paquetes `Azure.*`.

| Antiguo | Nuevo |
|--|--|
| `Microsoft.AspNetCore.DataProtection.AzureKeyVault` | [Azure.Extensions.AspNetCore.DataProtection.Keys](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Keys) |
| `Microsoft.AspNetCore.DataProtection.AzureStorage` | [Azure.Extensions.AspNetCore.DataProtection.Blobs](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.DataProtection.Blobs) |
| `Microsoft.Extensions.Configuration.AzureKeyVault` | [Azure.Extensions.AspNetCore.Configuration.Secrets](https://www.nuget.org/packages/Azure.Extensions.AspNetCore.Configuration.Secrets) |

Los nuevos paquetes usan una nueva versión del SDK de Azure que incluye cambios importantes. Los patrones de uso general no se modifican. Algunas sobrecargas y opciones pueden diferir para adaptarse a los cambios en las API del SDK de Azure subyacentes.

Los paquetes anteriores:

* Serán compatibles con el equipo de ASP.NET Core durante la vigencia de .NET Core 2.1 y 3.1.
* No se incluirán en .NET 5.

Al actualizar su proyecto a .NET 5, realice la transición a los paquetes `Azure.*` para mantener la compatibilidad.

## <a name="affected-apis"></a>API afectadas

- <xref:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.ProtectKeysWithAzureKeyVault%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.PersistKeysToAzureBlobStorage%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.Extensions.Configuration.AzureKeyVaultConfigurationExtensions.AddAzureKeyVault`
- `Overload:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.ProtectKeysWithAzureKeyVault`
- `Overload:Microsoft.AspNetCore.DataProtection.AzureDataProtectionBuilderExtensions.PersistKeysToAzureBlobStorage`

-->
