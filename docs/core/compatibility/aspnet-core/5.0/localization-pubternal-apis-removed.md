---
title: 'Cambio importante: API Pubternal quitadas'
description: Obtenga información sobre el cambio importante en ASP.NET Core 5.0 donde se han quitado algunas API Pubternal de localización
author: scottaddie
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: ae647d66b716175536edb3c978b027ebb7d3ddac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95760127"
---
# <a name="localization-pubternal-apis-removed"></a>Localización: API "Pubternal" quitadas

Para mantener mejor la superficie de la API pública de ASP.NET Core, se han quitado algunas API de localización de :::no-loc text="\"pubternal\"":::. Una API de :::no-loc text="\"pubternal\""::: tiene un modificador de acceso `public` y se define en un espacio de nombres que implica una intención [interna](../../../../csharp/language-reference/keywords/internal.md).

Para obtener información, vea [dotnet/aspnetcore#22291](https://github.com/dotnet/aspnetcore/issues/22291).

## <a name="version-introduced"></a>Versión introducida

5.0 (versión preliminar 6)

## <a name="old-behavior"></a>Comportamiento anterior

Las siguientes API eran `public`:

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- El constructor `Microsoft.Extensions.Localization.ResourceManagerStringLocalizer` se sobrecarga al aceptar cualquiera de los tipos de parámetro siguientes:
  - `AssemblyWrapper`
  - `IResourceStringProvider`

## <a name="new-behavior"></a>Comportamiento nuevo

En la lista siguiente se describen los cambios:

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper` se ha convertido en `Microsoft.Extensions.Localization.AssemblyWrapper` y ahora es `internal`.
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider` se ha convertido en `Microsoft.Extensions.Localization.Internal.IResourceStringProvider` y ahora es `internal`.
- Las sobrecargas del constructor `Microsoft.Extensions.Localization.ResourceManagerStringLocalizer` al aceptar cualquiera de los tipos de parámetro siguientes ahora son `internal`:
  - `AssemblyWrapper`
  - `IResourceStringProvider`

## <a name="reason-for-change"></a>Motivo del cambio

Se explica de forma más detallada en [aspnet/Announcements#377](https://github.com/aspnet/Announcements/issues/377#issue-473651882). Los tipos :::no-loc text="\"pubternal\""::: se han quitado de la superficie de la API de `public`. Estos cambios adaptan más clases a la decisión de diseño mencionada. Las clases en cuestión estaban pensadas como puntos de extensión para las pruebas internas del equipo.

## <a name="recommended-action"></a>Acción recomendada

Aunque es improbable, algunas aplicaciones pueden depender intencional o accidentalmente de los tipos de :::no-loc text="\"pubternal\"":::. Consulte las secciones de [Nuevo comportamiento](#new-behavior) para determinar cómo realizar la migración para dejar de usar estos tipos.

Si ha identificado un escenario en el que la API pública permitía esto antes del cambio mencionado, pero no lo hace ahora, registre una incidencia en [dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues).

## <a name="affected-apis"></a>API afectadas

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.%23ctor%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `T:Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- `Overload:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.#ctor`

-->
