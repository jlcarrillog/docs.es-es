---
title: 'Cambio importante: Deserialize necesita una cadena de un solo carácter'
description: Obtenga información sobre el cambio importante en .NET 5.0, donde JsonSerializer.Deserialize necesita una cadena de un solo carácter.
ms.date: 10/18/2020
ms.openlocfilehash: 780f2928d776ecb6db9a7fc05a720e889eb363e7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95760249"
---
# <a name="jsonserializerdeserialize-requires-single-character-string"></a><span data-ttu-id="c5236-103">JsonSerializer.Deserialize requiere una cadena de un solo carácter</span><span class="sxs-lookup"><span data-stu-id="c5236-103">JsonSerializer.Deserialize requires single-character string</span></span>

<span data-ttu-id="c5236-104">Cuando el parámetro de tipo es <xref:System.Char>, el argumento de cadena para <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> debe contener un solo carácter para que la deserialización se realice correctamente.</span><span class="sxs-lookup"><span data-stu-id="c5236-104">When the type parameter is <xref:System.Char>, the string argument to <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> must contain a single character for deserialization to succeed.</span></span>

## <a name="change-description"></a><span data-ttu-id="c5236-105">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="c5236-105">Change description</span></span>

<span data-ttu-id="c5236-106">En versiones anteriores de .NET, si se pasa una cadena de varios caracteres a <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> y el parámetro de tipo es <xref:System.Char>, la deserialización se realiza correctamente y solo se deserializa el primer carácter.</span><span class="sxs-lookup"><span data-stu-id="c5236-106">In previous .NET versions, if you pass a multi-character string to <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> and the type parameter is <xref:System.Char>, the deserialization succeeds and only the first character is deserialized.</span></span>

<span data-ttu-id="c5236-107">En .NET 5.0 y versiones posteriores, cuando el parámetro de tipo es <xref:System.Char>, si se pasa algo distinto a una cadena de un solo carácter, se inicia una excepción <xref:System.Text.Json.JsonException>.</span><span class="sxs-lookup"><span data-stu-id="c5236-107">In .NET 5.0 and later, when the type parameter is <xref:System.Char>, passing anything other than a single-character string causes a <xref:System.Text.Json.JsonException> to be thrown.</span></span>

```csharp
// .NET Core 3.0 and 3.1: Returns the first character 'a'.
// .NET 5.0 and later: Throws JsonException because payload has more than one character.
JsonSerializer.Deserialize<char>("\"abc\"");

// Correct usage.
JsonSerializer.Deserialize<char>("\"a\"");
```

## <a name="version-introduced"></a><span data-ttu-id="c5236-108">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="c5236-108">Version introduced</span></span>

<span data-ttu-id="c5236-109">5.0</span><span class="sxs-lookup"><span data-stu-id="c5236-109">5.0</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="c5236-110">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="c5236-110">Reason for change</span></span>

<span data-ttu-id="c5236-111"><xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> analiza el texto que representa un único valor JSON en una instancia del tipo especificado por el parámetro de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="c5236-111"><xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType> parses text that represents a single JSON value into an instance of the type specified by the generic type parameter.</span></span> <span data-ttu-id="c5236-112">El análisis solo se realiza correctamente si la carga proporcionada es válida para el parámetro de tipo genérico especificado.</span><span class="sxs-lookup"><span data-stu-id="c5236-112">Parsing should only succeed when the provided payload is valid for the specified generic type parameter.</span></span> <span data-ttu-id="c5236-113">Para un tipo de valor <xref:System.Char>, una carga válida es una cadena de un solo carácter.</span><span class="sxs-lookup"><span data-stu-id="c5236-113">For a <xref:System.Char> value type, a valid payload is a single character string.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c5236-114">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="c5236-114">Recommended action</span></span>

<span data-ttu-id="c5236-115">Al analizar una cadena en un tipo <xref:System.Char> mediante <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType>, asegúrese de que la cadena consta de un solo carácter.</span><span class="sxs-lookup"><span data-stu-id="c5236-115">When parsing a string into a <xref:System.Char> type using <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=nameWithType>, make sure the string consists of a single character.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="c5236-116">API afectadas</span><span class="sxs-lookup"><span data-stu-id="c5236-116">Affected APIs</span></span>

- <xref:System.Text.Json.JsonSerializer.Deserialize%60%601(System.String,System.Text.Json.JsonSerializerOptions)?displayProperty=fullName>

<!--

### Affected APIs

- `M:System.Text.Json.JsonSerializer.Deserialize``1(System.String,System.Text.Json.JsonSerializerOptions)`

### Category

Serialization

-->
