---
description: 'where (restricción de tipo genérico): Referencia de C#'
title: 'where (restricción de tipo genérico): Referencia de C#'
ms.date: 04/15/2020
f1_keywords:
- whereconstraint
- whereconstraint_CSharpKeyword
- classconstraint_CSharpKeyword
- structconstraint_CSharpKeyword
- enumconstraint_CSharpKeyword
helpviewer_keywords:
- where (generic type constraint) [C#]
ms.openlocfilehash: ff2d50b2148ea62e5bef5eceda547a976e4abf02
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687318"
---
# <a name="where-generic-type-constraint-c-reference"></a>where (restricción de tipo genérico) (Referencia de C#)

La cláusula `where` en una definición genérica especifica restricciones en los tipos que se usan como argumentos para los parámetros de tipo en un tipo genérico, método, delegado o función local. Las restricciones pueden especificar interfaces o clases base, o bien requerir que un tipo genérico sea una referencia, un valor o un tipo no administrado. Declaran las funcionalidades que debe tener el argumento de tipo.

Por ejemplo, se puede declarar una clase genérica, `AGenericClass`, de modo que el parámetro de tipo `T` implemente la interfaz <xref:System.IComparable%601>:

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#1)]

> [!NOTE]
> Para obtener más información sobre la cláusula where en una expresión de consulta, vea [where (Cláusula)](where-clause.md).

La cláusula `where` también puede incluir una restricción de clase base. La restricción de clase base indica que un tipo que se va a usar como argumento de tipo para ese tipo genérico tiene la clase especificada como clase base, o bien es la clase base. Si se usa la restricción de clase base, debe aparecer antes que cualquier otra restricción de ese parámetro de tipo. Algunos tipos no están permitidos como restricción de clase base: <xref:System.Object>, <xref:System.Array> y <xref:System.ValueType>. Antes de C# 7.3, tampoco se permitían <xref:System.Enum>, <xref:System.Delegate> ni <xref:System.MulticastDelegate> como restricciones de clase base. En el ejemplo siguiente se muestran los tipos que ahora se pueden especificar como clase base:

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#2)]

En un contexto que admite un valor NULL en C# 8.0 y versiones posteriores, se aplica la nulabilidad del tipo de clase base. Si la clase base no acepta valores NULL (por ejemplo, `Base`), el argumento de tipo no debe aceptar valores NULL. Si la clase base admite un valor NULL (por ejemplo, `Base?`), el argumento de tipo puede ser un tipo de referencia que acepte o no valores NULL. El compilador emite una advertencia si el argumento de tipo es un tipo de referencia que admite un valor NULL cuando la clase base no acepta valores NULL.

La cláusula `where` puede especificar que el tipo es `class` o `struct`. La restricción `struct` elimina la necesidad de especificar una restricción de clase base de `System.ValueType`. El tipo `System.ValueType` no se puede usar como restricción de clase base. En el ejemplo siguiente se muestran las restricciones `class` y `struct`:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#3)]

En un contexto que admite un valor NULL en C# 8.0 y versiones posteriores, la restricción `class` requiere que un tipo sea un tipo de referencia que no acepte valores NULL. Para permitir tipos de referencia que admitan un valor NULL, use la restricción `class?`, que permite tipos de referencia que aceptan y que no aceptan valores NULL.

La cláusula `where` puede incluir la restricción `notnull`. La restricción `notnull` limita el parámetro de tipo a tipos que no aceptan valores NULL. Ese tipo puede ser un [tipo de valor](../builtin-types/value-types.md) o un tipo de referencia que no acepta valores NULL. La restricción `notnull` está disponible a partir C# 8.0 para el código compilado en un contexto [`nullable enable`](../../nullable-references.md#nullable-contexts). A diferencia de otras restricciones, si un argumento de tipo infringe la restricción `notnull`, el compilador genera una advertencia en lugar de un error. Las advertencias solo se generan en un contexto `nullable enable`.

> [!IMPORTANT]
> Las declaraciones genéricas que incluyen la restricción `notnull` se pueden usar en un contexto donde se desconoce que se aceptan valores NULL, pero el compilador no aplica la restricción.

[!code-csharp[using the nonnull constraint](snippets/GenericWhereConstraints.cs#NotNull)]

La cláusula `where` también podría incluir una restricción `unmanaged`. La restricción `unmanaged` limita el parámetro de tipo a los tipos conocidos como [tipos no administrados](../builtin-types/unmanaged-types.md). La restricción `unmanaged` hace que sea más fácil escribir código de interoperabilidad de bajo nivel en C#. Esta restricción habilita las rutinas reutilizables en todos los tipos no administrados. La restricción `unmanaged` no se puede combinar con las restricciones `class` o `struct`. La restricción `unmanaged` exige que el tipo sea `struct`:

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#4)]

La cláusula `where` también podría incluir una restricción de constructor, `new()`. Esta restricción hace posible crear una instancia de un parámetro de tipo con el operador `new`. La [restricción new()](new-constraint.md) permite que el compilador sepa que cualquier argumento de tipo especificado debe tener accesible un constructor sin parámetros. Por ejemplo:

[!code-csharp[using the new constraint](snippets/GenericWhereConstraints.cs#5)]

La restricción `new()` aparece en último lugar en la cláusula `where`. La restricción `new()` no se puede combinar con las restricciones `struct` o `unmanaged`. Todos los tipos que cumplan esas restricciones deben tener un constructor sin parámetros accesible, lo que hace que la restricción `new()` sea redundante.

Con varios parámetros de tipo, use una cláusula `where` para cada parámetro de tipo, por ejemplo:

[!code-csharp[using multiple where constraints](snippets/GenericWhereConstraints.cs#6)]

También puede asociar restricciones a parámetros de tipo de métodos genéricos, como se muestra en el ejemplo siguiente:

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#7)]

Observe que la sintaxis para describir las restricciones de parámetro de tipo en delegados es igual que la de métodos:

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#8)]

Para obtener información sobre los delegados genéricos, vea [Delegados genéricos](../../programming-guide/generics/generic-delegates.md).

Para obtener más información sobre la sintaxis y el uso de restricciones, vea [Restricciones de tipos de parámetros](../../programming-guide/generics/constraints-on-type-parameters.md).

## <a name="c-language-specification"></a>Especificación del lenguaje C#

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Consulte también

- [Referencia de C#](../index.md)
- [Guía de programación de C#](../../programming-guide/index.md)
- [Introducción a los genéricos](../../programming-guide/generics/index.md)
- [new (restricción)](./new-constraint.md)
- [Restricciones de tipos de parámetros](../../programming-guide/generics/constraints-on-type-parameters.md)
