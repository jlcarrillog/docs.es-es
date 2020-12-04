---
title: Reglas de rendimiento (análisis de código)
description: Obtenga información sobre las reglas de rendimiento del análisis de código.
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.performancerules
helpviewer_keywords:
- rules, performance
- performance rules
- performance, rules
- managed code analysis rules, performance rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: 4409cc46eb73f13f8e59d7a51899da27035bb6af
ms.sourcegitcommit: 2e4adc490c1d2a705a0592b295d606b10b9f51f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "96592402"
---
# <a name="performance-rules"></a>Reglas de rendimiento

Las reglas de rendimiento admiten aplicaciones y bibliotecas de alto rendimiento.

## <a name="in-this-section"></a>En esta sección

| Regla | Descripción |
| - | - |
| [CA1802: Utilizar literales cuando sea apropiado](ca1802.md) | Un campo se declara estático y de solo lectura (Shared y ReadOnly en Visual Basic) y se inicializa con un valor que se calcula en tiempo de compilación. Dado que el valor asignado al campo de destino es calculable en tiempo de compilación, cambie la declaración a un campo const (const en Visual Basic) para que el valor se calcule en tiempo de compilación en lugar de en tiempo de ejecución. |
| [CA1805: no inicializar innecesariamente](ca1805.md) | El tiempo de ejecución de .NET inicializa todos los campos de los tipos de referencia a sus valores predeterminados antes de ejecutar el constructor. En la mayoría de los casos, la inicialización explícita de un campo a su valor predeterminado es redundante, lo que aumenta los costos de mantenimiento y puede degradar el rendimiento (por ejemplo, con un mayor tamaño de ensamblado). |
| [CA1806: No omitir resultados del método](ca1806.md) | Se crea un nuevo objeto, pero nunca se utiliza, o se llama a un método que crea y devuelve una nueva cadena y la nueva cadena nunca se utiliza, o un modelo de objetos componentes (COM) o un método P/Invoke devuelve un código de error o HRESULT que nunca se usa. |
| [CA1810: Inicializar campos estáticos de tipo de referencia insertados](ca1810.md) | Cuando un tipo declara un constructor estático explícito, el compilador Just-In-Time (JIT) agrega una comprobación a cada constructor de instancia y a cada método estático del tipo para asegurarse de que se ha llamado anteriormente al constructor estático. Las comprobaciones del constructor estático pueden reducir el rendimiento. |
| [CA1812: Evitar las clases internas sin instancia](ca1812.md) | El código del ensamblado no crea una instancia del tipo del nivel de ensamblado. |
| [CA1813: Evitar los atributos no sellados](ca1813.md) | .NET proporciona métodos para recuperar atributos personalizados. De forma predeterminada, estos métodos buscan la jerarquía de herencia de atributo. La acción de sellar el atributo elimina la búsqueda en la jerarquía de herencia y puede mejorarse el rendimiento. |
| [CA1814: Preferir matrices escalonadas antes que multidimensionales](ca1814.md) | Una matriz escalonada es una matriz cuyos elementos son matrices. Las matrices que componen los elementos pueden tener distintos tamaños, lo que puede dar lugar a menos espacio desperdiciado para algunos conjuntos de datos. |
| [CA1815: Invalidar Equals y el operador Equals en los tipos de valores](ca1815.md) | Para los tipos de valor, la implementación heredada de Equals utiliza la biblioteca de reflexión y compara el contenido de todos los campos. Mediante el cálculo, la reflexión es cara y no es necesario comparar cada campo para comprobar si hay igualdad. Si espera que los usuarios comparen u ordenen instancias, o utilicen instancias como claves de tabla hash, el tipo de valor debe implementar Equals. |
| [CA1819: Las propiedades no deben devolver matrices](ca1819.md) | Las matrices devueltas por propiedades no están protegidas contra escritura, aunque la propiedad sea de solo lectura. Para mantener la matriz inviolable, la propiedad debe devolver una copia de la matriz. Por lo general, los usuarios no entienden las implicaciones de rendimiento adversas que se originan al llamar a este tipo de propiedad. |
| [CA1820: Comprobar si las cadenas están vacías mediante la longitud de cadena](ca1820.md) | El uso de la propiedad String.Length o del método String.IsNullOrEmpty para comparar cadenas es mucho más rápido que el uso de Equals. |
| [CA1821: Quitar finalizadores vacíos](ca1821.md) | Siempre que pueda, evite los finalizadores debido a la sobrecarga de rendimiento adicional necesaria para el seguimiento de la duración del objeto. Un finalizador vacío supone una sobrecarga adicional sin ninguna ventaja. |
| [CA1822: Marcar miembros como estáticos](ca1822.md) | Los miembros que no tienen acceso a los datos de instancia o a los métodos de instancia de llamada se pueden marcar como estáticos (compartidos en Visual Basic). Después de marcar los métodos como static, el compilador emite los sitios de llamada no virtuales para estos miembros. Esto puede proporcionar una mejora apreciable del rendimiento del código en el que el rendimiento es fundamental. |
| [CA1823: Evitar los campos privados sin utilizar](ca1823.md) | Se detectaron campos privados a los que no parece que se tenga acceso en el ensamblado. |
| [CA1824: Marcar los ensamblados con NeutralResourcesLanguageAttribute](ca1824.md) | El atributo NeutralResourcesLanguage informa al Administrador de recursos del lenguaje que se usó para mostrar los recursos de una referencia cultural neutra para un ensamblado. Esto mejora el rendimiento de la búsqueda del primer recurso que se carga y puede reducir el espacio de trabajo. |
| [CA1825: Evitar asignaciones de matrices de longitud cero](ca1825.md) | Al inicializar una matriz de longitud cero, se produce una asignación de memoria innecesaria. En su lugar, use la instancia de matriz vacía asignada estáticamente mediante una llamada a <xref:System.Array.Empty%2A?displayProperty=nameWithType> . La asignación de memoria se comparte entre todas las invocaciones de este método. |
| [CA1826: Usar la propiedad en lugar del método Linq Enumerable](ca1826.md) | <xref:System.Linq.Enumerable> El método LINQ se usó en un tipo que admite una propiedad equivalente y más eficaz. |
| [CA1827: No usar Count/LongCount si se puede usar Any](ca1827.md) | <xref:System.Linq.Enumerable.Count%2A><xref:System.Linq.Enumerable.LongCount%2A>se usó el método o, donde el <xref:System.Linq.Enumerable.Any%2A> método sería más eficaz. |
| [CA1828: No usar CountAsync/LongCountAsync si se puede usar AnyAsync](ca1828.md) | <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.CountAsync%2A><xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.LongCountAsync%2A>se usó el método o, donde el <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.AnyAsync%2A> método sería más eficaz. |
| [CA1829: Usar la propiedad Length/Count en lugar del método Enumerable.Count](ca1829.md) | <xref:System.Linq.Enumerable.Count%2A> El método LINQ se usó en un tipo que admite una propiedad equivalente, más eficaz `Length` o `Count` . |
| [CA1830: Preferir las sobrecargas de método Append e Insert fuertemente tipadas en StringBuilder](ca1830.md) | <xref:System.Text.StringBuilder.Append%2A> y <xref:System.Text.StringBuilder.Insert%2A> proporcionan sobrecargas para varios tipos más allá de System. String.  Siempre que sea posible, prefiera las sobrecargas fuertemente tipadas sobre el uso de ToString () y la sobrecarga basada en cadena. |
| [CA1831: Usar AsSpan en lugar de indizadores basados en intervalos para una cadena cuando proceda](ca1831.md) | Cuando se usa un indexador de intervalo en una cadena y se asigna implícitamente el valor a un &lt; tipo char ReadOnlySpan &gt; , se utilizará el método en <xref:System.String.Substring%2A?#System_String_Substring_System_Int32_System_Int32_> lugar de <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_> , que genera una copia de la parte solicitada de la cadena. |
| [CA1832: Usar AsSpan o AsMemory en lugar de indizadores basados en intervalos para obtener la parte ReadOnlySpan o ReadOnlyMemory de una matriz](ca1832.md) | Cuando se usa un indexador de intervalo en una matriz y se asigna implícitamente el valor a un <xref:System.ReadOnlySpan%601> <xref:System.ReadOnlyMemory%601> tipo o, se utilizará el método en <xref:System.Runtime.CompilerServices.RuntimeHelpers.GetSubArray%2A> lugar de <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_> , que genera una copia de la parte solicitada de la matriz. |
| [CA1833: Usar AsSpan o AsMemory en lugar de indizadores basados en intervalos para obtener la parte Span o Memory de una matriz](ca1833.md) | Cuando se usa un indexador de intervalo en una matriz y se asigna implícitamente el valor a un <xref:System.Span%601> <xref:System.Memory%601> tipo o, se utilizará el método en <xref:System.Runtime.CompilerServices.RuntimeHelpers.GetSubArray%2A> lugar de <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_> , que genera una copia de la parte solicitada de la matriz. |
| [CA1834: Usar StringBuilder.Append(char) para cadenas de un solo carácter](ca1834.md) | <xref:System.Text.StringBuilder> tiene una `Append` sobrecarga que toma `char` como argumento. Prefiera llamar `char` a la sobrecarga para mejorar el rendimiento. |
| [CA1835: preferir las sobrecargas basadas en Memory' para ' ReadAsync ' y ' WriteAsync '](ca1835.md) | ' Stream ' tiene una sobrecarga ' ReadAsync ' que toma un ' byte de memoria &lt; &gt; ' como primer argumento y una sobrecarga ' WriteAsync ' que toma un ' ReadOnlyMemory &lt; byte &gt; ' como primer argumento. Prefiera llamar a las sobrecargas basadas en memoria, que son más eficaces. |
| [CA1836: preferir `IsEmpty` `Count` cuando esté disponible](ca1836.md) | Propiedad preferida `IsEmpty` que es más eficaz `Count` que `Length` , <xref:System.Linq.Enumerable.Count%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> o <xref:System.Linq.Enumerable.LongCount%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> para determinar si el objeto contiene o no elementos. |
| [CA1837: use `Environment.ProcessId` en lugar de `Process.GetCurrentProcess().Id`](ca1837.md) | `Environment.ProcessId` es más sencillo y más rápido que `Process.GetCurrentProcess().Id` . |
| [CA1838: evitar `StringBuilder` parámetros para P/Invoke](ca1838.md) | La serialización de `StringBuilder` siempre crea una copia de búfer nativa, lo que da lugar a varias asignaciones para una operación de serialización. |
