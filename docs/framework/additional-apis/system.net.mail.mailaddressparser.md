---
title: Clase MailAddressParser (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.MailAddressParser
- System.Net.MailAddressParser.ParseAddress
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 2c17970614ff9b6f1e6793a064a956da7248d693
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990511"
---
# <a name="mailaddressparser-class"></a><span data-ttu-id="60f2a-102">Clase MailAddressParser</span><span class="sxs-lookup"><span data-stu-id="60f2a-102">MailAddressParser class</span></span>

<span data-ttu-id="60f2a-103">Analiza las direcciones de correo electrónico como se describe en RFC 2822.</span><span class="sxs-lookup"><span data-stu-id="60f2a-103">Parses email addresses as described in RFC 2822.</span></span> <span data-ttu-id="60f2a-104">No se puede heredar esta clase.</span><span class="sxs-lookup"><span data-stu-id="60f2a-104">This class cannot be inherited.</span></span>

```csharp
internal static class MailAddressParser
```

> [!WARNING]
> <span data-ttu-id="60f2a-105">Esta clase es interna y no está diseñada para usarse directamente en el código.</span><span class="sxs-lookup"><span data-stu-id="60f2a-105">This class is internal and is not meant to be used directly in your code.</span></span>
>
> <span data-ttu-id="60f2a-106">Microsoft no admite el uso de esta clase en una aplicación de producción en cualquier circunstancia.</span><span class="sxs-lookup"><span data-stu-id="60f2a-106">Microsoft does not support the use of this class in a production application under any circumstance.</span></span>

## <a name="parseaddress-method"></a><span data-ttu-id="60f2a-107">Método ParseAddress</span><span class="sxs-lookup"><span data-stu-id="60f2a-107">ParseAddress method</span></span>

<span data-ttu-id="60f2a-108">Analiza una única dirección de correo electrónico de la cadena especificada.</span><span class="sxs-lookup"><span data-stu-id="60f2a-108">Parses a single email address from the specified string.</span></span>

```csharp
internal static MailAddress ParseAddress(string data)
```

### <a name="parameters"></a><span data-ttu-id="60f2a-109">Parámetros</span><span class="sxs-lookup"><span data-stu-id="60f2a-109">Parameters</span></span>

<span data-ttu-id="60f2a-110">`data` <xref:System.String></span><span class="sxs-lookup"><span data-stu-id="60f2a-110">`data` <xref:System.String></span></span>

<span data-ttu-id="60f2a-111">Cadena que contiene una dirección de correo electrónico que se va a analizar.</span><span class="sxs-lookup"><span data-stu-id="60f2a-111">The string that contains an email address to be parsed.</span></span>

### <a name="return-value"></a><span data-ttu-id="60f2a-112">Valor devuelto</span><span class="sxs-lookup"><span data-stu-id="60f2a-112">Return value</span></span>

<xref:System.Net.Mail.MailAddress>

<span data-ttu-id="60f2a-113">Una dirección de correo electrónico válida.</span><span class="sxs-lookup"><span data-stu-id="60f2a-113">A valid email address.</span></span>

### <a name="exceptions"></a><span data-ttu-id="60f2a-114">Excepciones</span><span class="sxs-lookup"><span data-stu-id="60f2a-114">Exceptions</span></span>

<xref:System.FormatException?displayProperty=nameWithType>

<span data-ttu-id="60f2a-115">La dirección de correo electrónico no es válida.</span><span class="sxs-lookup"><span data-stu-id="60f2a-115">The email address is invalid.</span></span>

## <a name="requirements"></a><span data-ttu-id="60f2a-116">Requisitos</span><span class="sxs-lookup"><span data-stu-id="60f2a-116">Requirements</span></span>

<span data-ttu-id="60f2a-117">**Espacio de nombres:** <xref:System.Net></span><span class="sxs-lookup"><span data-stu-id="60f2a-117">**Namespace:** <xref:System.Net></span></span>

<span data-ttu-id="60f2a-118">**Ensamblado:** Sistema (en System.dll)</span><span class="sxs-lookup"><span data-stu-id="60f2a-118">**Assembly:** System (in System.dll)</span></span>