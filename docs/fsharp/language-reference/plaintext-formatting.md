---
title: Formato de texto sin formato
description: 'Aprenda a usar printf y otro formato de texto sin formato en aplicaciones y scripts de F #.'
ms.date: 07/22/2020
ms.openlocfilehash: 6b14633e074961757d0f0cd258d1b1667f5fd8ee
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854924"
---
# <a name="plain-text-formatting"></a><span data-ttu-id="bb87c-103">Formato de texto sin formato</span><span class="sxs-lookup"><span data-stu-id="bb87c-103">Plain text formatting</span></span>

<span data-ttu-id="bb87c-104">F # admite el formato de texto sin formato con comprobación de tipos mediante `printf` ,, `printfn` `sprintf` y las funciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="bb87c-104">F# supports type-checked formatting of plain text using `printf`, `printfn`, `sprintf`, and related functions.</span></span>
<span data-ttu-id="bb87c-105">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-105">For example,</span></span>

```console
dotnet fsi

> printfn "Hello %s, %d + %d is %d" "world" 2 2 (2+2);;
```

<span data-ttu-id="bb87c-106">proporciona la salida</span><span class="sxs-lookup"><span data-stu-id="bb87c-106">gives the output</span></span>

```fsharp
Hello world, 2 + 2 is 4
```

<span data-ttu-id="bb87c-107">F # también permite dar formato de texto sin formato a los valores estructurados.</span><span class="sxs-lookup"><span data-stu-id="bb87c-107">F# also allows structured values to be formatted as plain text.</span></span>
<span data-ttu-id="bb87c-108">Por ejemplo, considere el siguiente ejemplo en el que se da formato a la salida como una presentación de tuplas de tipo matriz.</span><span class="sxs-lookup"><span data-stu-id="bb87c-108">For example, consider the following example that formats the output as a matrix-like display of tuples.</span></span>

```console
dotnet fsi

> printfn "%A" [ for i in 1 .. 5 -> [ for j in 1 .. 5 -> (i, j) ] ];;

[[(1, 1); (1, 2); (1, 3); (1, 4); (1, 5)];
 [(2, 1); (2, 2); (2, 3); (2, 4); (2, 5)];
 [(3, 1); (3, 2); (3, 3); (3, 4); (3, 5)];
 [(4, 1); (4, 2); (4, 3); (4, 4); (4, 5)];
 [(5, 1); (5, 2); (5, 3); (5, 4); (5, 5)]]
```

<span data-ttu-id="bb87c-109">El formato de texto sin formato estructurado se activa cuando se usa el `%A` formato en `printf` cadenas de formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-109">Structured plain text formatting is activated when you use the `%A` format in `printf` formatting strings.</span></span>
<span data-ttu-id="bb87c-110">También se activa al dar formato a la salida de valores de F # Interactive, donde la salida incluye información adicional y también se puede personalizar.</span><span class="sxs-lookup"><span data-stu-id="bb87c-110">It's also activated when formatting the output of values in F# interactive, where the output includes extra information and is additionally customizable.</span></span>
<span data-ttu-id="bb87c-111">El formato de texto sin formato también se puede observar a través de cualquier llamada a `x.ToString()` en valores de registro y Unión de F #, incluidos los que se producen implícitamente en la depuración, el registro y otras herramientas.</span><span class="sxs-lookup"><span data-stu-id="bb87c-111">Plain text formatting is also observable through any calls to `x.ToString()` on F# union and record values, including those that occur implicitly in debugging, logging, and other tooling.</span></span>

## <a name="checking-of-printf-format-strings"></a><span data-ttu-id="bb87c-112">Comprobando `printf` cadenas de formato</span><span class="sxs-lookup"><span data-stu-id="bb87c-112">Checking of `printf`-format strings</span></span>

<span data-ttu-id="bb87c-113">Se informará de un error en tiempo de compilación si `printf` se usa una función de formato con un argumento que no coincida con los especificadores de formato printf en la cadena de formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-113">A compile-time error will be reported if a `printf` formatting function is used with an argument that doesn't match the printf format specifiers in the format string.</span></span>  <span data-ttu-id="bb87c-114">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-114">For example,</span></span>

```fsharp
sprintf "Hello %s" (2+2)
```

<span data-ttu-id="bb87c-115">proporciona la salida</span><span class="sxs-lookup"><span data-stu-id="bb87c-115">gives the output</span></span>

```console
  sprintf "Hello %s" (2+2)
  ----------------------^

stdin(3,25): error FS0001: The type 'string' does not match the type 'int'
```

<span data-ttu-id="bb87c-116">Técnicamente hablando, al usar `printf` y otras funciones relacionadas, una regla especial en el compilador de F # comprueba el literal de cadena pasado como la cadena de formato, asegurándose de que los argumentos subsiguientes aplicados son del tipo correcto para que coincidan con los especificadores de formato usados.</span><span class="sxs-lookup"><span data-stu-id="bb87c-116">Technically speaking, when using `printf` and other related functions, a special rule in the F# compiler checks the string literal passed as the format string, ensuring the subsequent arguments applied are of the correct type to match the format specifiers used.</span></span>

## <a name="format-specifiers-for-printf"></a><span data-ttu-id="bb87c-117">Especificadores de formato para`printf`</span><span class="sxs-lookup"><span data-stu-id="bb87c-117">Format specifiers for `printf`</span></span>

<span data-ttu-id="bb87c-118">Las especificaciones de formato de los `printf` formatos son cadenas con `%` marcadores que indican el formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-118">Format specifications for `printf` formats are strings with `%` markers that indicate format.</span></span> <span data-ttu-id="bb87c-119">Los marcadores de posición de formato se componen de `%[flags][width][.precision][type]` que el tipo se interpreta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="bb87c-119">Format placeholders consist of `%[flags][width][.precision][type]` where the type is interpreted as follows:</span></span>

| <span data-ttu-id="bb87c-120">Especificador de formato</span><span class="sxs-lookup"><span data-stu-id="bb87c-120">Format specifier</span></span>   | <span data-ttu-id="bb87c-121">Tipos (s)</span><span class="sxs-lookup"><span data-stu-id="bb87c-121">Type(s)</span></span>        | <span data-ttu-id="bb87c-122">Observaciones</span><span class="sxs-lookup"><span data-stu-id="bb87c-122">Remarks</span></span>                      |
|:-------------------|:---------------|:-----------------------------|
| `%b`               | <span data-ttu-id="bb87c-123">bool</span><span class="sxs-lookup"><span data-stu-id="bb87c-123">bool</span></span>      | <span data-ttu-id="bb87c-124">Con formato `true` o`false`</span><span class="sxs-lookup"><span data-stu-id="bb87c-124">Formatted as `true` or `false`</span></span>                |
| `%s`               | <span data-ttu-id="bb87c-125">string</span><span class="sxs-lookup"><span data-stu-id="bb87c-125">string</span></span>    | <span data-ttu-id="bb87c-126">Con formato de contenido sin escape</span><span class="sxs-lookup"><span data-stu-id="bb87c-126">Formatted as its unescaped contents</span></span>         |
| `%c`               | <span data-ttu-id="bb87c-127">char</span><span class="sxs-lookup"><span data-stu-id="bb87c-127">char</span></span>      | <span data-ttu-id="bb87c-128">Con formato del literal de carácter</span><span class="sxs-lookup"><span data-stu-id="bb87c-128">Formatted as the character literal</span></span>  |
| <span data-ttu-id="bb87c-129">`%d`, `%i`</span><span class="sxs-lookup"><span data-stu-id="bb87c-129">`%d`, `%i`</span></span>         | <span data-ttu-id="bb87c-130">un tipo entero básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-130">a basic integer type</span></span> | <span data-ttu-id="bb87c-131">Con formato de entero decimal, con signo si el tipo entero básico está firmado</span><span class="sxs-lookup"><span data-stu-id="bb87c-131">Formatted as a decimal integer, signed if the basic integer type is signed</span></span> |
| `%u`               | <span data-ttu-id="bb87c-132">un tipo entero básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-132">a basic integer type</span></span> | <span data-ttu-id="bb87c-133">Con formato de entero decimal sin signo</span><span class="sxs-lookup"><span data-stu-id="bb87c-133">Formatted as an unsigned decimal integer</span></span>   |
| <span data-ttu-id="bb87c-134">`%x`, `%X`</span><span class="sxs-lookup"><span data-stu-id="bb87c-134">`%x`, `%X`</span></span>         | <span data-ttu-id="bb87c-135">un tipo entero básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-135">a basic integer type</span></span> | <span data-ttu-id="bb87c-136">Con formato de número hexadecimal sin signo (a-f o A-F para dígitos hexadecimales, respectivamente)</span><span class="sxs-lookup"><span data-stu-id="bb87c-136">Formatted as an unsigned hexadecimal number (a-f or A-F for hex digits respectively)</span></span>  |
|  `%o`              | <span data-ttu-id="bb87c-137">un tipo entero básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-137">a basic integer type</span></span> | <span data-ttu-id="bb87c-138">Con formato de número octal sin signo</span><span class="sxs-lookup"><span data-stu-id="bb87c-138">Formatted as an unsigned octal number</span></span> |
| <span data-ttu-id="bb87c-139">`%e`, `%E`</span><span class="sxs-lookup"><span data-stu-id="bb87c-139">`%e`, `%E`</span></span>         | <span data-ttu-id="bb87c-140">un tipo de punto flotante básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-140">a basic floating point type</span></span> | <span data-ttu-id="bb87c-141">Con formato como un valor con signo que tiene el formato `[-]d.dddde[sign]ddd` donde d es un solo dígito decimal, dddd es uno o más dígitos decimales, DDD tiene exactamente tres dígitos decimales y sign es `+` o`-`</span><span class="sxs-lookup"><span data-stu-id="bb87c-141">Formatted as a signed value having the form `[-]d.dddde[sign]ddd` where d is a single decimal digit, dddd is one or more decimal digits, ddd is exactly three decimal digits, and sign is `+` or `-`</span></span> |
| `%f`               | <span data-ttu-id="bb87c-142">un tipo de punto flotante básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-142">a basic floating point type</span></span> | <span data-ttu-id="bb87c-143">Con formato como un valor con signo que tiene el formato `[-]dddd.dddd` , donde `dddd` es uno o más dígitos decimales.</span><span class="sxs-lookup"><span data-stu-id="bb87c-143">Formatted as a signed value having the form `[-]dddd.dddd`, where `dddd` is one or more decimal digits.</span></span> <span data-ttu-id="bb87c-144">El número de dígitos que hay delante del separador decimal depende de la magnitud del número y el número de dígitos que hay detrás del separador decimal depende de la precisión solicitada.</span><span class="sxs-lookup"><span data-stu-id="bb87c-144">The number of digits before the decimal point depends on the magnitude of the number, and the number of digits after the decimal point depends on the requested precision.</span></span> |
| <span data-ttu-id="bb87c-145">`%g`, `%G`</span><span class="sxs-lookup"><span data-stu-id="bb87c-145">`%g`, `%G`</span></span> | <span data-ttu-id="bb87c-146">un tipo de punto flotante básico</span><span class="sxs-lookup"><span data-stu-id="bb87c-146">a basic floating point type</span></span> |  <span data-ttu-id="bb87c-147">Con formato con como valor con signo impreso en `%f` o en `%e` formato, lo que sea más compacto para el valor y la precisión especificados.</span><span class="sxs-lookup"><span data-stu-id="bb87c-147">Formatted using as a signed value printed in `%f` or `%e` format, whichever is more compact for the given value and precision.</span></span> |
| `%M` | <span data-ttu-id="bb87c-148">un `System.Decimal` valor</span><span class="sxs-lookup"><span data-stu-id="bb87c-148">a `System.Decimal` value</span></span>  |    <span data-ttu-id="bb87c-149">Con formato mediante el `"G"` especificador de formato para`System.Decimal.ToString(format)`</span><span class="sxs-lookup"><span data-stu-id="bb87c-149">Formatted using the `"G"` format specifier for `System.Decimal.ToString(format)`</span></span> |
| `%O` | <span data-ttu-id="bb87c-150">cualquier valor</span><span class="sxs-lookup"><span data-stu-id="bb87c-150">any value</span></span>  |   <span data-ttu-id="bb87c-151">Formateado mediante la conversión boxing del objeto y la llamada a su `System.Object.ToString()` método</span><span class="sxs-lookup"><span data-stu-id="bb87c-151">Formatted by boxing the object and calling its `System.Object.ToString()` method</span></span> |
| `%A` | <span data-ttu-id="bb87c-152">cualquier valor</span><span class="sxs-lookup"><span data-stu-id="bb87c-152">any value</span></span>  |   <span data-ttu-id="bb87c-153">Con formato de [texto sin formato estructurado](plaintext-formatting.md) con la configuración de diseño predeterminada</span><span class="sxs-lookup"><span data-stu-id="bb87c-153">Formatted using [structured plain text formatting](plaintext-formatting.md) with the default layout settings</span></span> |
| `%a` | <span data-ttu-id="bb87c-154">cualquier valor</span><span class="sxs-lookup"><span data-stu-id="bb87c-154">any value</span></span>  |   <span data-ttu-id="bb87c-155">Requiere dos argumentos: una función de formato que acepte un parámetro de contexto y el valor, y el valor concreto que se va a imprimir.</span><span class="sxs-lookup"><span data-stu-id="bb87c-155">Requires two arguments: a formatting function accepting a context parameter and the value, and the particular value to print</span></span> |
| `%t` | <span data-ttu-id="bb87c-156">cualquier valor</span><span class="sxs-lookup"><span data-stu-id="bb87c-156">any value</span></span>  |   <span data-ttu-id="bb87c-157">Requiere un argumento: una función de formato que acepte un parámetro de contexto que genere o devuelva el texto adecuado</span><span class="sxs-lookup"><span data-stu-id="bb87c-157">Requires one argument: a formatting function accepting a context parameter that either outputs or returns the appropriate text</span></span> |

<span data-ttu-id="bb87c-158">Los tipos enteros básicos son `byte` ( `System.Byte` ), `sbyte` ( `System.SByte` ), `int16` ( `System.Int16` ), `uint16` ( `System.UInt16` ), `int32` ( `System.Int32` ), `uint32` ( `System.UInt32` ), `int64` ( `System.Int64` ), `uint64` ( `System.UInt64` ), `nativeint` ( `System.IntPtr` ) y `unativeint` ( `System.UIntPtr` ).</span><span class="sxs-lookup"><span data-stu-id="bb87c-158">Basic integer types are `byte` (`System.Byte`), `sbyte` (`System.SByte`), `int16` (`System.Int16`), `uint16` (`System.UInt16`), `int32` (`System.Int32`), `uint32` (`System.UInt32`), `int64` (`System.Int64`), `uint64` (`System.UInt64`), `nativeint`  (`System.IntPtr`), and `unativeint`  (`System.UIntPtr`).</span></span>
<span data-ttu-id="bb87c-159">Los tipos de punto flotante básicos son `float` ( `System.Double` ) y `float32` ( `System.Single` ).</span><span class="sxs-lookup"><span data-stu-id="bb87c-159">Basic floating point types are `float` (`System.Double`) and `float32` (`System.Single`).</span></span>

<span data-ttu-id="bb87c-160">El ancho opcional es un entero que indica el ancho mínimo del resultado.</span><span class="sxs-lookup"><span data-stu-id="bb87c-160">The optional width is an integer indicating the minimal width of the result.</span></span> <span data-ttu-id="bb87c-161">Por ejemplo, `%6d` imprime un entero, prefijando espacios en blanco para rellenar al menos seis caracteres.</span><span class="sxs-lookup"><span data-stu-id="bb87c-161">For instance, `%6d` prints an integer, prefixing it with spaces to fill at least six characters.</span></span> <span data-ttu-id="bb87c-162">Si el ancho es `*` , se toma un argumento entero adicional para especificar el ancho correspondiente.</span><span class="sxs-lookup"><span data-stu-id="bb87c-162">If width is `*`, then an extra integer  argument is taken to specify the corresponding width.</span></span>

<span data-ttu-id="bb87c-163">Las marcas válidas son:</span><span class="sxs-lookup"><span data-stu-id="bb87c-163">Valid flags are:</span></span>

| <span data-ttu-id="bb87c-164">Marca</span><span class="sxs-lookup"><span data-stu-id="bb87c-164">Flag</span></span>   | <span data-ttu-id="bb87c-165">Efecto</span><span class="sxs-lookup"><span data-stu-id="bb87c-165">Effect</span></span>        | <span data-ttu-id="bb87c-166">Observaciones</span><span class="sxs-lookup"><span data-stu-id="bb87c-166">Remarks</span></span>                      |
|:-------------------|:---------------|:-----------------------------|
| `0`  | <span data-ttu-id="bb87c-167">Agregue ceros en lugar de espacios para crear el ancho necesario</span><span class="sxs-lookup"><span data-stu-id="bb87c-167">Add zeros instead of spaces to make up the required width</span></span> |    |
| `-` |  <span data-ttu-id="bb87c-168">Justificar a la izquierda el resultado dentro del ancho especificado</span><span class="sxs-lookup"><span data-stu-id="bb87c-168">Left justify the result within the specified width</span></span> |   |
| `+`  | <span data-ttu-id="bb87c-169">Agregue un `+` carácter si el número es positivo (para que coincida con un `-` signo para los negativos)</span><span class="sxs-lookup"><span data-stu-id="bb87c-169">Add a `+` character if the number is positive (to match a `-` sign for negatives)</span></span> |   |
| <span data-ttu-id="bb87c-170">carácter de espacio</span><span class="sxs-lookup"><span data-stu-id="bb87c-170">space character</span></span> | <span data-ttu-id="bb87c-171">Agregue un espacio adicional si el número es positivo (para que coincida con un signo "-" para los negativos)</span><span class="sxs-lookup"><span data-stu-id="bb87c-171">Add an extra space if the number is positive (to match a '-' sign for negatives)</span></span> |

<span data-ttu-id="bb87c-172">La `#` marca printf no es válida y se indicará un error en tiempo de compilación si se usa.</span><span class="sxs-lookup"><span data-stu-id="bb87c-172">The printf `#` flag is invalid and a compile-time error will be reported if it is used.</span></span>

<span data-ttu-id="bb87c-173">Se da formato a los valores mediante una referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="bb87c-173">Values are formatted using invariant culture.</span></span> <span data-ttu-id="bb87c-174">La configuración de la referencia cultural es irrelevante para `printf` dar formato, excepto cuando afecta a los resultados de `%O` y el `%A` formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-174">Culture settings are irrelevant to `printf` formatting except when they affect the results of `%O` and `%A` formatting.</span></span> <span data-ttu-id="bb87c-175">Para obtener más información, vea [formato de texto sin formato estructurado](plaintext-formatting.md).</span><span class="sxs-lookup"><span data-stu-id="bb87c-175">For more information, see [structured plain text formatting](plaintext-formatting.md).</span></span>

## <a name="a-formatting"></a><span data-ttu-id="bb87c-176">`%A`formato</span><span class="sxs-lookup"><span data-stu-id="bb87c-176">`%A` formatting</span></span>

<span data-ttu-id="bb87c-177">El `%A` especificador de formato se usa para dar formato a los valores de una manera legible y también puede ser útil para notificar información de diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="bb87c-177">The `%A` format specifier is used to format values in a human-readable way, and can also be useful for reporting diagnostic information.</span></span>

### <a name="primitive-values"></a><span data-ttu-id="bb87c-178">Valores primitivos</span><span class="sxs-lookup"><span data-stu-id="bb87c-178">Primitive values</span></span>

<span data-ttu-id="bb87c-179">Al dar formato a texto sin formato mediante el `%A` especificador, se da formato a los valores numéricos de F # con su sufijo y referencia cultural de todos los idiomas.</span><span class="sxs-lookup"><span data-stu-id="bb87c-179">When formatting plain text using the `%A` specifier, F# numeric values are formatted with their suffix and invariant culture.</span></span> <span data-ttu-id="bb87c-180">Los valores de punto flotante se formatean con 10 lugares de precisión de punto flotante.</span><span class="sxs-lookup"><span data-stu-id="bb87c-180">Floating point values are formatted using 10 places of floating point precision.</span></span> <span data-ttu-id="bb87c-181">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-181">For example,</span></span>

```fsharp
printfn "%A" (1L, 3n, 5u, 7, 4.03f, 5.000000001, 5.0000000001)
```

<span data-ttu-id="bb87c-182">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-182">produces</span></span>

```console
(1L, 3n, 5u, 7, 4.03000021f, 5.000000001, 5.0)
```

<span data-ttu-id="bb87c-183">Al usar el `%A` especificador, se da formato a las cadenas con comillas.</span><span class="sxs-lookup"><span data-stu-id="bb87c-183">When using the `%A` specifier, strings are formatted using quotes.</span></span> <span data-ttu-id="bb87c-184">Los códigos de escape no se agregan y, en su lugar, se imprimen los caracteres sin formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-184">Escape codes are not added and instead the raw characters are printed.</span></span> <span data-ttu-id="bb87c-185">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-185">For example,</span></span>

```fsharp
printfn "%A" ("abc", "a\tb\nc\"d")

```

<span data-ttu-id="bb87c-186">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-186">produces</span></span>

```console
("abc", "a      b
c"d")
```

### <a name="net-values"></a><span data-ttu-id="bb87c-187">Valores de .NET</span><span class="sxs-lookup"><span data-stu-id="bb87c-187">.NET values</span></span>

<span data-ttu-id="bb87c-188">Al dar formato a texto sin formato mediante el `%A` especificador, se da formato a los objetos de .net que no son de F # mediante `x.ToString()` el uso de la configuración predeterminada de .net especificada por `System.Globalization.CultureInfo.CurrentCulture` y `System.Globalization.CultureInfo.CurrentUICulture` .</span><span class="sxs-lookup"><span data-stu-id="bb87c-188">When formatting plain text using the `%A` specifier, non-F# .NET objects are formatted by using `x.ToString()` using the default settings of .NET given by `System.Globalization.CultureInfo.CurrentCulture` and `System.Globalization.CultureInfo.CurrentUICulture`.</span></span>  <span data-ttu-id="bb87c-189">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-189">For example,</span></span>

```fsharp
open System.Globalization

let date = System.DateTime(1999, 12, 31)

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("de-DE")
printfn "Culture 1: %A" date

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("en-US")
printfn "Culture 2: %A" date
```

<span data-ttu-id="bb87c-190">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-190">produces</span></span>

```console
Culture 1: 31.12.1999 00:00:00
Culture 2: 12/31/1999 12:00:00 AM
```

### <a name="structured-values"></a><span data-ttu-id="bb87c-191">Valores estructurados</span><span class="sxs-lookup"><span data-stu-id="bb87c-191">Structured values</span></span>

<span data-ttu-id="bb87c-192">Al dar formato a texto sin formato mediante el `%A` especificador, la sangría de bloque se usa para las listas y tuplas de F #.</span><span class="sxs-lookup"><span data-stu-id="bb87c-192">When formatting plain text using the `%A` specifier, block indentation is used for F# lists and tuples.</span></span> <span data-ttu-id="bb87c-193">Esto se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="bb87c-193">This shown in the previous example.</span></span>
<span data-ttu-id="bb87c-194">También se utiliza la estructura de matrices, incluidas las matrices multidimensionales.</span><span class="sxs-lookup"><span data-stu-id="bb87c-194">The structure of arrays is also used, including multi-dimensional arrays.</span></span>  <span data-ttu-id="bb87c-195">Las matrices unidimensionales se muestran con `[| ... |]` Sintaxis.</span><span class="sxs-lookup"><span data-stu-id="bb87c-195">Single-dimensional arrays are shown with `[| ... |]` syntax.</span></span> <span data-ttu-id="bb87c-196">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-196">For example,</span></span>

```fsharp
printfn "%A" [| for i in 1 .. 20 -> (i, i*i) |]
```

<span data-ttu-id="bb87c-197">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-197">produces</span></span>

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25); (6, 36); (7, 49); (8, 64); (9, 81);
  (10, 100); (11, 121); (12, 144); (13, 169); (14, 196); (15, 225); (16, 256);
  (17, 289); (18, 324); (19, 361); (20, 400)|]
```

<span data-ttu-id="bb87c-198">El ancho de impresión predeterminado es 80.</span><span class="sxs-lookup"><span data-stu-id="bb87c-198">The default print width is 80.</span></span>  <span data-ttu-id="bb87c-199">Este ancho se puede personalizar mediante el uso de un ancho de impresión en el especificador de formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-199">This width can be customized by using a print width in the format specifier.</span></span> <span data-ttu-id="bb87c-200">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-200">For example,</span></span>

```fsharp
printfn "%10A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%20A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%50A" [| for i in 1 .. 5 -> (i, i*i) |]
```

<span data-ttu-id="bb87c-201">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-201">produces</span></span>

```console
[|(1, 1);
  (2, 4);
  (3, 9);
  (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4);
  (3, 9); (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
```

<span data-ttu-id="bb87c-202">Si se especifica un ancho de impresión de 0, no se utilizará ningún ancho de impresión.</span><span class="sxs-lookup"><span data-stu-id="bb87c-202">Specifying a print width of 0 results in no print width being used.</span></span> <span data-ttu-id="bb87c-203">Se producirá una sola línea de texto, excepto donde las cadenas incrustadas en la salida contengan saltos de línea.</span><span class="sxs-lookup"><span data-stu-id="bb87c-203">A single line of text will result, except where embedded strings in the output contain line breaks.</span></span>  <span data-ttu-id="bb87c-204">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="bb87c-204">For example</span></span>

```fsharp
printfn "%0A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%0A" [| for i in 1 .. 5 -> "abc\ndef" |]
```

<span data-ttu-id="bb87c-205">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-205">produces</span></span>

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
[|"abc
def"; "abc
def"; "abc
def"; "abc
def"; "abc
def"|]
```

<span data-ttu-id="bb87c-206">Se usa un límite de profundidad de 4 para los valores de secuencia ( `IEnumerable` ), que se muestran como `seq { ...}` .</span><span class="sxs-lookup"><span data-stu-id="bb87c-206">A depth limit of 4 is used for sequence (`IEnumerable`) values, which are shown as `seq { ...}`.</span></span> <span data-ttu-id="bb87c-207">Se usa un límite de profundidad de 100 para los valores de lista y matriz.</span><span class="sxs-lookup"><span data-stu-id="bb87c-207">A depth limit of 100 is used for list and array values.</span></span>
<span data-ttu-id="bb87c-208">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-208">For example,</span></span>

```fsharp
printfn "%A" (seq { for i in 1 .. 10 -> (i, i*i) })
```

<span data-ttu-id="bb87c-209">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-209">produces</span></span>

```console
seq [(1, 1); (2, 4); (3, 9); (4, 16); ...]
```

<span data-ttu-id="bb87c-210">La sangría de bloque también se usa para la estructura de los valores de registro y Unión públicos.</span><span class="sxs-lookup"><span data-stu-id="bb87c-210">Block indentation is also used for the structure of public record and union values.</span></span> <span data-ttu-id="bb87c-211">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-211">For example,</span></span>

```fsharp
type R = { X : int list; Y : string list }

printfn "%A" { X =  [ 1;2;3 ]; Y = ["one"; "two"; "three"] }
```

<span data-ttu-id="bb87c-212">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-212">produces</span></span>

```console
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

<span data-ttu-id="bb87c-213">Si `%+A` se utiliza, la estructura privada de los registros y las uniones también se revela mediante la reflexión.</span><span class="sxs-lookup"><span data-stu-id="bb87c-213">If `%+A` is used, then the private structure of records and unions is also revealed by using reflection.</span></span> <span data-ttu-id="bb87c-214">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="bb87c-214">For example</span></span>

```fsharp
type internal R =
    { X : int list; Y : string list }
    override _.ToString() = "R"

let internal data = { X = [ 1;2;3 ]; Y = ["one"; "two"; "three"] }

printfn "external view:\n%A" data

printfn "internal view:\n%+A" data

```

<span data-ttu-id="bb87c-215">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-215">produces</span></span>

```console
external view:
R

internal view:
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

### <a name="large-cyclic-or-deeply-nested-values"></a><span data-ttu-id="bb87c-216">Valores grandes, cíclicos o profundamente anidados</span><span class="sxs-lookup"><span data-stu-id="bb87c-216">Large, cyclic, or deeply nested values</span></span>

<span data-ttu-id="bb87c-217">Los valores estructurados grandes tienen el formato de un número máximo de nodos de objetos globales de 10000.</span><span class="sxs-lookup"><span data-stu-id="bb87c-217">Large structured values are formatted to a maximum overall object node count of 10000.</span></span>
<span data-ttu-id="bb87c-218">Los valores profundamente anidados tienen el formato de una profundidad de 100.</span><span class="sxs-lookup"><span data-stu-id="bb87c-218">Deeply nested values are formatted to a depth of 100.</span></span>  <span data-ttu-id="bb87c-219">En ambos casos, `...` se usa para Elide parte de la salida.</span><span class="sxs-lookup"><span data-stu-id="bb87c-219">In both cases `...` is used to elide some of the output.</span></span>  <span data-ttu-id="bb87c-220">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-220">For example,</span></span>

```fsharp
type Tree =
    | Tip
    | Node of Tree * Tree

let rec make n =
    if n = 0 then
        Tip
    else
        Node(Tip, make (n-1))

printfn "%A" (make 1000)
```

<span data-ttu-id="bb87c-221">genera una salida grande con algunas partes eliden:</span><span class="sxs-lookup"><span data-stu-id="bb87c-221">produces a large output with some parts elided:</span></span>

```console
Node(Tip, Node(Tip, ....Node (..., ...)...))
```

<span data-ttu-id="bb87c-222">Los ciclos se detectan en los gráficos de objetos y `...` se utilizan en los lugares donde se detectan los ciclos.</span><span class="sxs-lookup"><span data-stu-id="bb87c-222">Cycles are detected in the object graphs and `...` is used at places where cycles are detected.</span></span> <span data-ttu-id="bb87c-223">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="bb87c-223">For example</span></span>

```fsharp
type R = { mutable Links: R list }
let r = { Links = [] }
r.Links <- [r]
printfn "%A" r
```

<span data-ttu-id="bb87c-224">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-224">produces</span></span>

```console
{ Links = [...] }
```

### <a name="lazy-null-and-function-values"></a><span data-ttu-id="bb87c-225">Valores de función Lazy, NULL y</span><span class="sxs-lookup"><span data-stu-id="bb87c-225">Lazy, null, and function values</span></span>

<span data-ttu-id="bb87c-226">Los valores diferidos se imprimen como `Value is not created` o texto equivalente cuando el valor aún no se ha evaluado.</span><span class="sxs-lookup"><span data-stu-id="bb87c-226">Lazy values are printed as `Value is not created` or equivalent text when the value has not yet been evaluated.</span></span>

<span data-ttu-id="bb87c-227">Los valores NULL se imprimen como `null` a menos que el tipo estático del valor se determine como un tipo de Unión donde `null` es una representación permitida.</span><span class="sxs-lookup"><span data-stu-id="bb87c-227">Null values are printed as `null` unless the static type of the value is determined to be a union type where `null` is a permitted representation.</span></span>

<span data-ttu-id="bb87c-228">Los valores de función de F # se imprimen como su nombre de cierre generado internamente, por ejemplo, `<fun:it@43-7>` .</span><span class="sxs-lookup"><span data-stu-id="bb87c-228">F# function values are printed as their internally generated closure name, for example, `<fun:it@43-7>`.</span></span>

### <a name="customize-plain-text-formatting-with-structuredformatdisplay"></a><span data-ttu-id="bb87c-229">Personalizar el formato de texto sin formato con`StructuredFormatDisplay`</span><span class="sxs-lookup"><span data-stu-id="bb87c-229">Customize plain text formatting with `StructuredFormatDisplay`</span></span>

<span data-ttu-id="bb87c-230">Cuando se usa el `%A` especificador, se respeta la presencia del `StructuredFormatDisplay` atributo en las declaraciones de tipo.</span><span class="sxs-lookup"><span data-stu-id="bb87c-230">When using the `%A` specifier, the presence of the `StructuredFormatDisplay` attribute on type declarations is respected.</span></span>  <span data-ttu-id="bb87c-231">Se puede usar para especificar texto suplente y la propiedad para mostrar un valor.</span><span class="sxs-lookup"><span data-stu-id="bb87c-231">This can be used to specify surrogate text and property to display a value.</span></span> <span data-ttu-id="bb87c-232">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bb87c-232">For example:</span></span>

```fsharp
[<StructuredFormatDisplay("Counts({Clicks})")>]
type Counts = { Clicks:int list}

printfn "%20A" {Clicks=[0..20]}
```

<span data-ttu-id="bb87c-233">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-233">produces</span></span>

```console
Counts([0; 1; 2; 3;
        4; 5; 6; 7;
        8; 9; 10; 11;
        12; 13; 14;
        15; 16; 17;
        18; 19; 20])
```

### <a name="customize-plain-text-formatting-by-overriding-tostring"></a><span data-ttu-id="bb87c-234">Personalizar el formato de texto sin formato invalidando`ToString`</span><span class="sxs-lookup"><span data-stu-id="bb87c-234">Customize plain text formatting by overriding `ToString`</span></span>

<span data-ttu-id="bb87c-235">La implementación predeterminada de `ToString` es observable en programación en F #.</span><span class="sxs-lookup"><span data-stu-id="bb87c-235">The default implementation of `ToString` is observable in F# programming.</span></span> <span data-ttu-id="bb87c-236">A menudo, los resultados predeterminados no son adecuados para su uso en la presentación de información orientada al programador o en el resultado del usuario y, como resultado, es habitual reemplazar la implementación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="bb87c-236">Often, the default results aren't suitable for use in either programmer-facing information display or user output, and as a result it is common to override the default implementation.</span></span>  

<span data-ttu-id="bb87c-237">De forma predeterminada, los tipos de registro y Unión de F # invalidan la implementación de `ToString` con una implementación de que usa `sprintf "%+A"` .</span><span class="sxs-lookup"><span data-stu-id="bb87c-237">By default, F# record and union types override the implementation of `ToString` with an implementation that uses `sprintf "%+A"`.</span></span>  <span data-ttu-id="bb87c-238">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-238">For example,</span></span>

```fsharp
type Counts = { Clicks:int list }

printfn "%s" ({Clicks=[0..10]}.ToString())
```

<span data-ttu-id="bb87c-239">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-239">produces</span></span>

```console
{ Clicks = [0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10] }
```

<span data-ttu-id="bb87c-240">En el caso de los tipos de clase, no se proporciona ninguna implementación predeterminada de `ToString` y se usa el valor predeterminado de .net, que indica el nombre del tipo.</span><span class="sxs-lookup"><span data-stu-id="bb87c-240">For class types, no default implementation of `ToString` is provided and the .NET default is used, which reports the name of the type.</span></span> <span data-ttu-id="bb87c-241">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="bb87c-241">For example,</span></span>

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Default structured print gives this:\n%A" data
printfn "Default ToString gives:\n%s" (data.ToString())
```

<span data-ttu-id="bb87c-242">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-242">produces</span></span>

```console
Default structured print gives this:
[MyClassType; MyClassType]
Default ToString gives:
[MyClassType; MyClassType]
```

<span data-ttu-id="bb87c-243">Agregar una invalidación para `ToString` puede dar un mejor formato.</span><span class="sxs-lookup"><span data-stu-id="bb87c-243">Adding an override for `ToString` can give better formatting.</span></span>

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks
   override _.ToString() = sprintf "MyClassType(%0A)" clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Now structured print gives this:\n%A" data
printfn "Now ToString gives:\n%s" (data.ToString())
```

<span data-ttu-id="bb87c-244">Obtiene</span><span class="sxs-lookup"><span data-stu-id="bb87c-244">produces</span></span>

```console
Now structured print gives this:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
Now ToString gives:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
```

## <a name="f-interactive-structured-printing"></a><span data-ttu-id="bb87c-245">F# interactivo impresión estructurada</span><span class="sxs-lookup"><span data-stu-id="bb87c-245">F# Interactive structured printing</span></span>

<span data-ttu-id="bb87c-246">F# interactivo ( `dotnet fsi` ) utiliza una versión extendida de formato de texto sin formato estructurado para informar de los valores y permite la personalización adicional.</span><span class="sxs-lookup"><span data-stu-id="bb87c-246">F# Interactive (`dotnet fsi`) uses an extended version of structured plain text formatting to report values and allows additional customizability.</span></span> <span data-ttu-id="bb87c-247">Para obtener más información, vea [F# interactivo](fsharp-interactive-options.md).</span><span class="sxs-lookup"><span data-stu-id="bb87c-247">For more information, see [F# Interactive](fsharp-interactive-options.md).</span></span>

## <a name="customize-debug-displays"></a><span data-ttu-id="bb87c-248">Personalizar las visualizaciones de depuración</span><span class="sxs-lookup"><span data-stu-id="bb87c-248">Customize debug displays</span></span>

<span data-ttu-id="bb87c-249">Los depuradores para .NET respetan el uso de atributos como `DebuggerDisplay` y `DebuggerTypeProxy` , y afectan a la presentación estructurada de objetos en las ventanas de inspección del depurador.</span><span class="sxs-lookup"><span data-stu-id="bb87c-249">Debuggers for .NET respect the use of attributes such as `DebuggerDisplay` and `DebuggerTypeProxy`, and these affect the structured display of objects in debugger inspection windows.</span></span>
<span data-ttu-id="bb87c-250">El compilador de F # generaba automáticamente estos atributos para los tipos de Unión y de registro discriminados, pero no los tipos de clase, interfaz o estructura.</span><span class="sxs-lookup"><span data-stu-id="bb87c-250">The F# compiler automatically generated these attributes for discriminated union and record types, but not class, interface, or struct types.</span></span>

<span data-ttu-id="bb87c-251">Estos atributos se omiten en el formato de texto sin formato de F #, pero puede ser útil implementar estos métodos para mejorar las visualizaciones al depurar tipos de F #.</span><span class="sxs-lookup"><span data-stu-id="bb87c-251">These attributes are ignored in F# plain text formatting, but it can be useful to implement these methods to improve displays when debugging F# types.</span></span>

## <a name="see-also"></a><span data-ttu-id="bb87c-252">Vea también</span><span class="sxs-lookup"><span data-stu-id="bb87c-252">See also</span></span>

- [<span data-ttu-id="bb87c-253">Cadenas</span><span class="sxs-lookup"><span data-stu-id="bb87c-253">Strings</span></span>](strings.md)
- [<span data-ttu-id="bb87c-254">Informes</span><span class="sxs-lookup"><span data-stu-id="bb87c-254">Records</span></span>](records.md)
- [<span data-ttu-id="bb87c-255">Uniones discriminadas</span><span class="sxs-lookup"><span data-stu-id="bb87c-255">Discriminated Unions</span></span>](discriminated-unions.md)
- [<span data-ttu-id="bb87c-256">F# interactivo</span><span class="sxs-lookup"><span data-stu-id="bb87c-256">F# Interactive</span></span>](fsharp-interactive-options.md)