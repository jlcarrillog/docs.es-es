---
title: La expresión llama de forma recursiva a la propiedad contenedora '<propertyname>'
ms.date: 07/20/2015
f1_keywords:
- vbc42026
- BC42026
helpviewer_keywords:
- BC42026
ms.assetid: 4fde9db6-3bf3-48dc-8e05-981bf08969da
ms.openlocfilehash: f5b8b226d46afa0555559de0930f20025ba27f2b
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162770"
---
# <a name="bc42026-expression-recursively-calls-the-containing-property-propertyname"></a>BC42026: la expresión llama de forma recursiva a la propiedad contenedora ' \<propertyname> '

Una instrucción en el `Set` procedimiento de una definición de propiedad almacena un valor en el nombre de la propiedad.

 El enfoque recomendado para contener el valor de una propiedad es definir una `Private` variable en el contenedor de la propiedad y utilizarla en los `Get` procedimientos y `Set` . `Set`Después, el procedimiento debe almacenar el valor de entrada en esta `Private` variable.

 El `Get` procedimiento se comporta como un `Function` procedimiento, por lo que puede asignar un valor al nombre de la propiedad y devolver el control al encontrar la `End Get` instrucción. Sin embargo, el enfoque recomendado consiste en incluir la `Private` variable como valor en una [instrucción return](../statements/return-statement.md).

 El `Set` procedimiento se comporta como un `Sub` procedimiento, que no devuelve un valor. Por lo tanto, el nombre del procedimiento o de la propiedad no tiene ningún significado especial dentro de un `Set` procedimiento, y no se puede almacenar un valor en él.

 En el ejemplo siguiente se muestra el enfoque que puede producir este error, seguido del enfoque recomendado.

```vb
Public Class illustrateProperties
' The code in the following property causes this error.
    Public Property badProp() As Char
        Get
            Dim charValue As Char
            ' Insert code to update charValue.
            badProp = charValue
        End Get
        Set(ByVal Value As Char)
            ' The following statement causes this error.
            badProp = Value
            ' The value stored in the local variable badProp
            ' is not used by the Get procedure in this property.
        End Set
    End Property
' The following code uses the recommended approach.
    Private propValue As Char
    Public Property goodProp() As Char
        Get
            ' Insert code to update propValue.
            Return propValue
        End Get
        Set(ByVal Value As Char)
            propValue = Value
        End Set
    End Property
End Class
```

 De forma predeterminada, este mensaje es una advertencia. Para más información sobre cómo ocultar las advertencias o cómo tratarlas como errores, vea [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).

 **Identificador de error:** BC42026

## <a name="to-correct-this-error"></a>Para corregir este error

- Vuelva a escribir la definición de la propiedad para usar el enfoque recomendado, tal como se muestra en el ejemplo anterior.

## <a name="see-also"></a>Vea también

- [Procedimientos de propiedad](../../programming-guide/language-features/procedures/property-procedures.md)
- [Property Statement](../statements/property-statement.md)
- [Set (instrucción)](../statements/set-statement.md)
