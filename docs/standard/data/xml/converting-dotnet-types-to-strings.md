---
title: Conversión de tipos de .NET en cadenas
ms.date: 03/30/2017
ms.assetid: dc2e2b65-f623-4dc3-938b-d2a054d6832c
ms.openlocfilehash: 9744224b4346b865a112b0eb6957f12553e1ec5f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830992"
---
# <a name="convert-net-types-to-strings"></a>Conversión de tipos de .NET en cadenas

Si quiere convertir un tipo de .NET en una cadena, use el método **ToString**. El método **ToString** devuelve una representación de cadena del tipo que se le pasa. En la tabla siguiente se enumeran los tipos de .NET que devuelven una cadena en un formato que se asigna a las especificaciones de los esquemas XML (XSD).  
  
|Tipo de .NET|Tipo de cadena devuelta|  
|-------------------------|--------------------------|  
|Booleano|"true", "false"|  
|Single.PositiveInfinity|"INF"|  
|Single.NegativeInfinity|"-INF"|  
|Double.PositiveInfinity|"INF"|  
|Double.NegativeInfinity|"-INF"|  
|DateTime|El formato es aaaa-MM-ddTHH:mm:sszzzzzz y sus subconjuntos.|  
|Timespan|El formato es PnYnMnTnHnMnS, por ejemplo, `P2Y10M15DT10H30M20S` corresponde a una duración de 2 años, 10 meses, 15 días, 10 horas, 30 minutos y 20 segundos.|  
  
## <a name="see-also"></a>Vea también

- [Conversión de tipos de datos XML](conversion-of-xml-data-types.md)
- [Conversión de cadenas en tipos de datos de .NET](converting-strings-to-dotnet-data-types.md)
