---
title: Parámetros y argumentos
description: 'Obtenga información sobre la compatibilidad del lenguaje F # para definir parámetros y pasar argumentos a funciones, métodos y propiedades.'
ms.date: 08/15/2020
ms.openlocfilehash: 3c391ca37a1cf3bd150316943e5b06efa532b317
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740294"
---
# <a name="parameters-and-arguments"></a>Parámetros y argumentos

En este tema se describe la compatibilidad con lenguajes para definir parámetros y pasar argumentos a funciones, métodos y propiedades. Incluye información sobre cómo pasar por referencia y cómo definir y usar métodos que pueden tomar un número variable de argumentos.

## <a name="parameters-and-arguments"></a>Parámetros y argumentos

El término *parámetro* se usa para describir los nombres de los valores que se espera que se proporcionen. El término *argumento* se utiliza para los valores proporcionados para cada parámetro.

Los parámetros se pueden especificar en forma de tupla o en formato currificados, o en una combinación de ambos. Puede pasar argumentos mediante el uso de un nombre de parámetro explícito. Los parámetros de los métodos se pueden especificar como opcionales y se les asigna un valor predeterminado.

## <a name="parameter-patterns"></a>Patrones de parámetros

Los parámetros proporcionados a funciones y métodos son, en general, modelos separados por espacios. Esto significa que, en principio, cualquiera de los modelos descritos en [expresiones de coincidencia](match-expressions.md) se puede usar en una lista de parámetros para una función o un miembro.

Los métodos suelen usar la forma de tupla de pasar argumentos. Esto consigue un resultado más claro desde la perspectiva de otros lenguajes .NET, ya que el formato de tupla coincide con la forma en que se pasan los argumentos en los métodos de .NET.

La forma currificada se usa con más frecuencia con funciones creadas mediante `let` enlaces.

En el siguiente pseudocódigo se muestran ejemplos de argumentos de tupla y currificados.

```fsharp
// Tuple form.
member this.SomeMethod(param1, param2) = ...
// Curried form.
let function1 param1 param2 = ...
```

Los formularios combinados son posibles cuando algunos argumentos están en tuplas y otros no.

```fsharp
let function2 param1 (param2a, param2b) param3 = ...
```

También se pueden usar otros patrones en las listas de parámetros, pero si el patrón de parámetros no coincide con todas las entradas posibles, puede haber una coincidencia incompleta en tiempo de ejecución. La excepción `MatchFailureException` se genera cuando el valor de un argumento no coincide con los patrones especificados en la lista de parámetros. El compilador emite una advertencia cuando un patrón de parámetro permite coincidencias incompletas. Al menos un patrón es normalmente útil para las listas de parámetros, y es el patrón de carácter comodín. El patrón de caracteres comodín se usa en una lista de parámetros cuando simplemente se desea omitir los argumentos que se proporcionan. En el código siguiente se muestra el uso del patrón de carácter comodín en una lista de argumentos.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3801.fs)]

El patrón de carácter comodín puede ser útil cuando no se necesitan los argumentos pasados, como en el punto de entrada principal a un programa, cuando no está interesado en los argumentos de la línea de comandos que se proporcionan normalmente como una matriz de cadenas, como en el código siguiente.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3802.fs)]

Otros patrones que a veces se usan en argumentos son el `as` patrón y los patrones de identificador asociados a las uniones discriminadas y los patrones activos. Puede usar el modelo de Unión discriminada de un solo caso como se indica a continuación.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3803.fs)]

La salida es la siguiente.

```console
Data begins at 0 and ends at 4 in string Et tu, Brute?
Et tu
```

Los modelos activos pueden ser útiles como parámetros, por ejemplo, al transformar un argumento en un formato deseado, como en el ejemplo siguiente:

```fsharp
type Point = { x : float; y : float }

let (| Polar |) { x = x; y = y} =
    ( sqrt (x*x + y*y), System.Math.Atan (y/ x) )

let radius (Polar(r, _)) = r
let angle (Polar(_, theta)) = theta
```

Puede usar el `as` patrón para almacenar un valor coincidente como un valor local, como se muestra en la siguiente línea de código.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3805.fs)]

Otro patrón que se usa ocasionalmente es una función que deja el último argumento sin nombre proporcionando, como el cuerpo de la función, una expresión lambda que realiza inmediatamente una coincidencia de patrón en el argumento implícito. Un ejemplo de esto es la siguiente línea de código.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3804.fs)]

Este código define una función que toma una lista genérica y devuelve `true` si la lista está vacía, y `false` en caso contrario. El uso de estas técnicas puede dificultar la lectura del código.

En ocasiones, los patrones que implican coincidencias incompletas son útiles, por ejemplo, si sabe que las listas del programa tienen solo tres elementos, podría usar un patrón similar al siguiente en una lista de parámetros.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3806.fs)]

El uso de patrones con coincidencias incompletas se reserva mejor para el prototipo rápido y otros usos temporales. El compilador emitirá una advertencia para este tipo de código. Estos patrones no pueden cubrir el caso general de todas las entradas posibles y, por lo tanto, no son adecuadas para las API de componentes.

## <a name="named-arguments"></a>Argumentos con nombre

Los argumentos para los métodos se pueden especificar por posición en una lista de argumentos separados por comas, o bien se pueden pasar explícitamente a un método proporcionando el nombre, seguido de un signo igual y del valor que se va a pasar. Si se especifica proporcionando el nombre, pueden aparecer en un orden diferente al utilizado en la declaración.

Los argumentos con nombre pueden hacer que el código sea más legible y más adaptable a determinados tipos de cambios en la API, como una reordenación de los parámetros de método.

Los argumentos con nombre solo se permiten para métodos, no para `let` funciones enlazadas, valores de función o expresiones lambda.

En el ejemplo de código siguiente se muestra el uso de argumentos con nombre.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3807.fs)]

En una llamada a un constructor de clase, puede establecer los valores de las propiedades de la clase mediante una sintaxis similar a la de los argumentos con nombre. En el ejemplo siguiente se muestra esta sintaxis.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3506.fs)]

Para obtener más información, vea [constructores (F #)](members/constructors.md).

## <a name="optional-parameters"></a>Parámetros opcionales

Puede especificar un parámetro opcional para un método mediante un signo de interrogación delante del nombre del parámetro. Los parámetros opcionales se interpretan como el tipo de opción de F #, por lo que puede consultarlos de la manera habitual en que se consultan los tipos de opciones, mediante el uso de una `match` expresión con `Some` y `None` . Los parámetros opcionales solo se permiten en los miembros, no en las funciones creadas mediante `let` enlaces.

Puede pasar valores opcionales existentes al método por nombre de parámetro, como `?arg=None` o `?arg=Some(3)` `?arg=arg` . Esto puede ser útil al compilar un método que pasa argumentos opcionales a otro método.

También puede utilizar una función `defaultArg` , que establece un valor predeterminado de un argumento opcional. La `defaultArg` función toma el parámetro opcional como primer argumento y el valor predeterminado como segundo.

En el ejemplo siguiente se muestra el uso de parámetros opcionales.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3808.fs)]

La salida es la siguiente.

```console
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 4800 Duplex: Half Parity: false
Baud Rate: 300 Duplex: Half Parity: true
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 9600 Duplex: Full Parity: false
Baud Rate: 4800 Duplex: Half Parity: false
```

Para los fines de la interoperabilidad de C# y Visual Basic, puede usar los atributos `[<Optional; DefaultParameterValue<(...)>]` de F #, de modo que los llamadores verán un argumento como opcional. Esto es equivalente a definir el argumento como opcional en C# como en `MyMethod(int i = 3)` .

```fsharp
open System
open System.Runtime.InteropServices
type C =
    static member Foo([<Optional; DefaultParameterValue("Hello world")>] message) =
        printfn $"{message}"
```

También puede especificar un nuevo objeto como valor de parámetro predeterminado. Por ejemplo, en `Foo` su lugar, el miembro podría tener una `CancellationToken` entrada opcional como:

```fsharp
open System.Threading
open System.Runtime.InteropServices
type C =
    static member Foo([<Optional; DefaultParameterValue(CancellationToken())>] ct: CancellationToken) =
        printfn $"{ct}"
```

El valor dado como argumento para `DefaultParameterValue` debe coincidir con el tipo del parámetro. Por ejemplo, no se permite lo siguiente:

```fsharp
type C =
    static member Wrong([<Optional; DefaultParameterValue("string")>] i:int) = ()
```

En este caso, el compilador genera una advertencia y omitirá ambos atributos por completo. Tenga en cuenta que el valor predeterminado `null` debe anotarse de tipo, ya que, de lo contrario, el compilador deduce el tipo equivocado, es decir, `[<Optional; DefaultParameterValue(null:obj)>] o:obj` .

## <a name="passing-by-reference"></a>Pasar por referencia

Pasar un valor F # por referencia implica [byrefs](byrefs.md), que son tipos de puntero administrados. Instrucciones para el tipo que se va a usar es la siguiente:

- Use `inref<'T>` si solo necesita leer el puntero.
- Use `outref<'T>` si solo necesita escribir en el puntero.
- Use `byref<'T>` si necesita leer y escribir en el puntero.

```fsharp
let example1 (x: inref<int>) = printfn $"It's %d{x}"

let example2 (x: outref<int>) = x <- x + 1

let example3 (x: byref<int>) =
    printfn $"It's %d{x}"
    x <- x + 1

let test () =
    // No need to make it mutable, since it's read-only
    let x = 1
    example1 &x

    // Needs to be mutable, since we write to it
    let mutable y = 2
    example2 &y
    example3 &y // Now 'y' is 3
```

Dado que el parámetro es un puntero y el valor es mutable, cualquier cambio en el valor se conserva después de la ejecución de la función.

Puede usar una tupla como valor devuelto para almacenar los `out` parámetros en los métodos de la biblioteca de .net. Como alternativa, puede tratar el `out` parámetro como un `byref` parámetro. En el ejemplo de código siguiente se muestran ambas maneras.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-1/snippet3810.fs)]

## <a name="parameter-arrays"></a>Matrices de parámetros

En ocasiones, es necesario definir una función que tome un número arbitrario de parámetros de tipo heterogéneo. No sería práctico crear todos los métodos sobrecargados posibles para tener en cuenta todos los tipos que se podrían usar. Las implementaciones de .NET proporcionan compatibilidad con estos métodos a través de la característica de matriz de parámetros. Se puede proporcionar un método que toma una matriz de parámetros en su signatura con un número arbitrario de parámetros. Los parámetros se colocan en una matriz. El tipo de los elementos de la matriz determina los tipos de parámetros que se pueden pasar a la función. Si define la matriz de parámetros con `System.Object` como tipo de elemento, el código de cliente puede pasar valores de cualquier tipo.

En F #, las matrices de parámetros solo se pueden definir en métodos. No se pueden usar en funciones independientes o en funciones que se definen en módulos.

Una matriz de parámetros se define mediante el `ParamArray` atributo. El `ParamArray` atributo solo se puede aplicar al último parámetro.

En el código siguiente se muestra cómo llamar a un método .NET que toma una matriz de parámetros y la definición de un tipo en F # que tiene un método que toma una matriz de parámetros.

[!code-fsharp[Main](~/samples/snippets/fsharp/parameters-and-arguments-2/snippet3811.fs)]

Cuando se ejecuta en un proyecto, la salida del código anterior es la siguiente:

```console
a 1 10 Hello world 1 True
"a"
1
10.0
"Hello world"
1u
true
```

## <a name="see-also"></a>Consulte también

- [Miembros](./members/index.md)
