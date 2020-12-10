---
description: Obtenga información sobre los tipos de valor, sus variedades y los que están integrados en C#.
title: Tipos de valor (Referencia de C#)
ms.date: 01/22/2020
f1_keywords:
- cs.valuetypes
helpviewer_keywords:
- value types [C#]
- types [C#], value types
- C# language, value types
ms.assetid: 471eb994-2958-49d5-a6be-19b4313f80a3
ms.openlocfilehash: 64c9e9eba2495531cfef8a603d53fb21c95c87a4
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599400"
---
# <a name="value-types-c-reference"></a>Tipos de valor (Referencia de C#)

Los *tipos de valor* y los [tipos de referencia](../keywords/reference-types.md) son las dos categorías principales de tipos de C#. Una variable de un tipo de valor contiene una instancia del tipo. Esto difiere de una variable de un tipo de referencia, que contiene una referencia a una instancia del tipo. De forma predeterminada, al [asignar](../operators/assignment-operator.md), pasar un argumento a un método o devolver el resultado de un método, se copian los valores de variable. En el caso de las variables de tipo de valor, se copian las instancias de tipo correspondientes. En el ejemplo siguiente se muestra ese comportamiento:

[!code-csharp[copy of values](snippets/shared/ValueTypes.cs#ValueTypeCopied)]

Como se muestra en el ejemplo anterior, las operaciones en una variable de tipo de valor solo afectan a esa instancia del tipo de valor, almacenado en la variable.

Si un tipo de valor contiene un miembro de datos de un tipo de referencia, solo se copia la referencia a la instancia del tipo de referencia al copiar una instancia de tipo de valor. Tanto la instancia de tipo de valor original como la copia tienen acceso a la misma instancia de tipo de referencia. En el ejemplo siguiente se muestra ese comportamiento:

[!code-csharp[shallow copy](snippets/shared/ValueTypes.cs#ShallowCopy)]

> [!NOTE]
> Para que el código sea menos propenso a errores y más sólido, defina y use tipos de valor inmutables. En este artículo se usan tipos de valor mutables solo con fines de demostración.

## <a name="kinds-of-value-types-and-type-constraints"></a>Clases de tipos de valor y restricciones de tipo

Un tipo de valor puede ser de una de las dos clases siguientes:

- un [tipo de estructura](struct.md), que encapsula los datos y la funcionalidad relacionada;
- un [tipo de enumeración](enum.md), que se define mediante un conjunto de constantes con nombre y representa una opción o una combinación de opciones.

Un [tipo de valor que admite valores NULL](nullable-value-types.md) `T?` representa todos los valores de su tipo de valor subyacente `T` y un valor [NULL](../keywords/null.md) adicional. No se puede asignar `null` a una variable de un tipo de valor, a menos que sea un tipo de valor que acepte valores NULL.

Puede usar la [restricción `struct`](../../programming-guide/generics/constraints-on-type-parameters.md) para especificar que un parámetro de tipo es un tipo de valor que no acepta valores NULL. Los tipos de estructura y enumeración satisfacen la restricción `struct`. A partir C# 7.3, puede usar `System.Enum` en una restricción de clase base (conocida como la [restricción de enumeración](../../programming-guide/generics/constraints-on-type-parameters.md#enum-constraints)) para especificar que un parámetro de tipo es un tipo de enumeración.

## <a name="built-in-value-types"></a>Tipos de valor integrados

C# proporciona los siguientes tipos de valor integrados, también conocidos como *tipos simples*:

- [Tipos numéricos integrales](integral-numeric-types.md)
- [Tipos numéricos de punto flotante](floating-point-numeric-types.md)
- [bool](bool.md), que representa un valor booleano
- [char](char.md), que representa un carácter Unicode UTF-16

Todos los tipos simples son tipos de estructuras y se diferencian de otros tipos de estructuras en que permiten determinadas operaciones adicionales:

- Se pueden usar literales para proporcionar un valor de un tipo simple. Por ejemplo, `'A'` es un literal del tipo `char` y `2001` es un literal del tipo `int`.

- Puede declarar constantes de los tipos simples con la palabra clave [const](../keywords/const.md). No es posible tener constantes de otros tipos de estructuras.

- Las expresiones constantes, cuyos operandos son todas constantes de los tipos simples, se evalúan en tiempo de compilación.

A partir de C# 7.0, C# admite [tuplas de valor](value-tuples.md). Una tupla de valor es un tipo de valor, pero no un tipo simple.

## <a name="c-language-specification"></a>Especificación del lenguaje C#

Para más información, vea las secciones siguientes de la [Especificación del lenguaje C#](~/_csharplang/spec/introduction.md):

- [Tipos de valor](~/_csharplang/spec/types.md#value-types)
- [Tipos simples](~/_csharplang/spec/types.md#simple-types)
- [Variables](~/_csharplang/spec/variables.md)

## <a name="see-also"></a>Vea también

- [Referencia de C#](../index.md)
- <xref:System.ValueType?displayProperty=nameWithType>
- [Tipos de referencia](../keywords/reference-types.md)
