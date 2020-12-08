---
ms.openlocfilehash: 28b882384760c8ac56c6d194bef6018c451fd03f
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96477757"
---
### <a name="wpf-grid-allocation-of-space-to-star-columns"></a>Asignación de espacio del elemento Grid de WPF a las columnas de asterisco

#### <a name="details"></a>Detalles

A partir de las aplicaciones que tienen como destino .NET Framework 4.7, WPF reemplaza el algoritmo que <xref:System.Windows.Controls.Grid> usa para asignar espacio a las columnas \*. Esto cambiará el ancho real que se asigna a las columnas \* en una serie de casos:

- Cuando una o más columnas \* también tienen un ancho mínimo o máximo que invalida la asignación proporcional para esa columna. (El ancho mínimo se puede derivar de una declaración MinWidth explícita, o bien de un mínimo implícito obtenido del contenido de la columna. El ancho máximo solo se puede definir de forma explícita, a partir de una declaración MaxWidth).
- Cuando una o más columnas \* declaran un peso \* extremadamente grande, superior a 10^298.
- Cuando los pesos \* son suficientemente distintos como para detectar una inestabilidad de punto flotante (desbordamiento, subdesbordamiento y pérdida de precisión).
- Cuando se habilita el redondeo del diseño, y el punto por pulgada de visualización efectivo es suficientemente alto.
En los primeros dos casos, los anchos producidos por el nuevo algoritmo pueden ser significativamente distintos de los producidos por el algoritmo antiguo; en el último caso, la diferencia será como máximo de uno o dos píxeles.<p/>El nuevo algoritmo corrige varios errores presentes en el algoritmo antiguo:

- La asignación total a columnas puede superar el ancho de la cuadrícula. Esto puede ocurrir cuando se asigna espacio a una columna cuya parte proporcional es inferior a su tamaño mínimo. El algoritmo asigna el tamaño mínimo, lo que disminuye el espacio disponible para otras columnas. Si no hay ninguna columna \* que asignar, la asignación total será demasiado grande.
- La asignación total puede no alcanzar el ancho de la cuadrícula. Este es el doble problema que presenta el n.º 1, que surge cuando se asigna a una columna cuya parte proporcional es superior al tamaño máximo, sin columnas \* para ocupar el margen de demora.
- Dos columnas \* pueden recibir ubicaciones no proporcionales a sus pesos de \*. Esta es una versión más ligera de n.º 1/n.º 2, que surge cuando se asigna a las columnas \* A, B y C (en ese orden), donde la parte proporcional de B infringe la limitación mínima o máxima. Como ocurría anteriormente, esto cambia el espacio disponible en la columna C, que obtiene menos (o más) asignación proporcional respecto a A.
- Las columnas con pesos extremadamente grandes (&gt; 10^298) se tratan como si tuvieran el peso 10^298. Las diferencias proporcionales entre ellas (y entre columnas con pesos ligeramente más reducidos) no se han respetado.
- Las columnas con pesos infinitos no se han administrado correctamente. [De hecho, no puede establecer un peso en infinito, pero esta es una restricción artificial. El código de ubicación estaba intentando administrarlo, pero de forma incorrecta].
- Algunos problemas leves al evitar el desbordamiento, el subdesbordamiento, la pérdida de precisión y problemas de punto flotante similares.
- Los ajustes del redondeo del diseño son incorrectos en los puntos por pulgada suficientemente elevados.
El nuevo algoritmo genera resultados que cumplen los siguientes criterios:<p/>R. El ancho real asignado a una columna * nunca es inferior al ancho mínimo ni superior al ancho máximo.<br/>B. Cada columna  <em>a la que no se le asigne su ancho mínimo o máximo, se le asigna un ancho proporcional a su peso de <em>. Para ser precisos, si dos columnas se declaran con un ancho x</em> e y</em> respectivamente, y si ninguna de ellas recibe su ancho mínimo o máximo, los anchos reales v y w asignados a las columnas tienen la misma proporción: v / w == x / y.<br/>C. El ancho total asignado a las columnas \* &quot;proporcionales&quot; es igual al espacio disponible después de la asignación a las columnas restringidas (columnas fijas, automáticas y \* a las que se les asigna su ancho mínimo o máximo). Debe ser cero, por ejemplo si la suma de los anchos mínimos supera el ancho disponible de la cuadrícula.<br/>D. Todas estas instrucciones se deben interpretar en relación con el diseño &quot;ideal&quot;. Cuando se aplica el redondeo del diseño, los anchos reales pueden diferir de los ideales como máximo un píxel.<br/>Se respetó el algoritmo antiguo (A), pero se produjo un error al respetar los demás criterios en los casos descritos anteriormente.<p/>Toda la información que se indica sobre las columnas y los anchos en este artículo se aplica también a las filas y las alturas.

#### <a name="suggestion"></a>Sugerencia

De forma predeterminada, las aplicaciones que tienen como destino versiones de .NET Framework a partir de .NET Framework 4.7 verán el algoritmo nuevo, mientras que las aplicaciones que tienen como destino .NET Framework 4.6.2 o versiones anteriores verán el algoritmo antiguo.<p/>Para invalidar el valor predeterminado, utilice el siguiente ajuste de configuración:

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.Grid.StarDefinitionsCanExceedAvailableSpace=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

El valor `true` selecciona el algoritmo antiguo y el valor `false` el nuevo.

| NOMBRE    | Valor       |
|:--------|:------------|
| Ámbito   | Secundaria       |
| Versión | 4.7         |
| Tipo    | Redestinación |
