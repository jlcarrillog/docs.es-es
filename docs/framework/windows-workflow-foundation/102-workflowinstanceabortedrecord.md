---
title: 102 - WorkflowInstanceAbortedRecord
ms.date: 03/30/2017
ms.assetid: bde4378d-4eea-4907-aaf2-c1a2bc770a37
ms.openlocfilehash: a48e3e27787a9f6dd133240f20ed67230d837698
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275471"
---
# <a name="102---workflowinstanceabortedrecord"></a>102 - WorkflowInstanceAbortedRecord

## <a name="properties"></a>Propiedades  
  
|||  
|-|-|  
|Identificador|102|  
|Palabras clave|EndToEndMonitoring, Troubleshooting, HealthMonitoring, WFTracking|  
|Nivel|Advertencia|  
|Canal|Microsoft-Windows-Application Server-Applications/Analytic|  
  
## <a name="description"></a>Descripción  

 El participante de seguimiento de ETW emite este evento cuando una instancia de flujo de trabajo emite WorkflowInstanceAbortedRecord.  
  
## <a name="message"></a>Message  

 TrackRecord = WorkflowInstanceAbortedRecord, InstanceID = %1, RecordNumber = %2, EventTime = %3, ActivityDefinitionId = %4, Reason = %5, Annotations = %6, ProfileName = %7  
  
## <a name="details"></a>Detalles  
  
|Nombre del elemento de datos|Tipo del elemento de datos|Descripción|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|El id. de instancia del flujo de trabajo.|  
|RecordNumber|xs:long|El número de secuencia del registro emitido.|  
|EventTime|xs:dateTime|La hora en UTC cuando se emitió el evento.|  
|ActivityDefinitionId|xs:string|El nombre de la actividad raíz del flujo de trabajo.|  
|Motivo|xs:string|La razón por la que se anuló el flujo de trabajo.|  
|anotaciones|xs:string|Las anotaciones que se agregaron a este evento.  Los valores se almacenan en un elemento XML con el formato \<items> \< item  name = "annotationName" type="System.String"> annotationValue \</item> \</items> .  Si no se especifica ninguna anotación, la cadena contendrá \<items/> . El tamaño del evento ETW está limitado por el tamaño de búfer de ETW o la carga útil máxima para un evento ETW. Si el tamaño del evento supera los límites de ETW, el evento se trunca quitando las anotaciones y reemplazando el valor de anotación por \<items> ... \</items> .|  
|ProfileName|xs:string|El nombre o el perfil de seguimiento que dio como resultado que se emitiera este evento.|  
|HostReference|xs:string|En el caso de los servicios hospedados en web, este campo identifica de manera única el servicio en la jerarquía web.  Su formato se define como ' ruta de acceso virtual de la aplicación del nombre del sitio web&#124;ruta de acceso virtual del servicio&#124;ServiceName ' ejemplo: ' sitio web predeterminado/CalculatorApplication&#124;/CalculatorService.svc&#124;CalculatorService '|  
|AppDomain|xs:string|La cadena devuelta por AppDomain.CurrentDomain.FriendlyName.|
