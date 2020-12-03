---
title: Reducir los reinicios del sistema durante las instalaciones de .NET Framework 4.5
description: Aprenda a reducir los reinicios del sistema durante las instalaciones de .NET Framework 4.5. Es posible que se necesite un reinicio si una aplicación de .NET 4 está en uso durante la instalación de .NET Framework 4.5.
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework, reducing system restarts
- installing .NET Framework
- installation [.NET Framework]
ms.assetid: 7aa8cb72-dee9-4716-ac54-b17b9ae8218f
ms.openlocfilehash: 44dc3277ae0b87a0182bb980ddc263f0793de9ed
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275055"
---
# <a name="reducing-system-restarts-during-net-framework-45-installations"></a>Reducir los reinicios del sistema durante las instalaciones de .NET Framework 4.5

El instalador de .NET Framework 4.5 usa el [Administrador de reinicio](/windows/win32/rstmgr/about-restart-manager) para evitar reinicios del sistema durante la instalación, siempre que sea posible. Si el programa de instalación de la aplicación instala .NET Framework, este puede comunicarse con el Administrador de reinicio para aprovechar esta característica. Para obtener más información, vea [Cómo: Obtener el progreso del instalador de .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md).

## <a name="reasons-for-a-restart"></a>Razones para reiniciar

 La instalación de .NET Framework 4.5 requiere un reinicio del sistema si una aplicación de .NET Framework 4 está en uso durante la instalación. Esto se debe a que .NET Framework 4.5 reemplaza los archivos de .NET Framework 4 y requiere que dichos archivos estén disponibles durante la instalación. En muchos casos, el reinicio se puede impedir de forma preventiva detectando y cerrando las aplicaciones de .NET framework 4 que están en uso. Sin embargo, algunas aplicaciones del sistema no se deben cerrar. En estos casos, no puede evitarse el reinicio.

## <a name="end-user-experience"></a>Experiencia de usuario final

 Un usuario final que realice una instalación completa de .NET Framework 4.5 tiene la oportunidad de evitar el reinicio del sistema si el instalador detecta que hay aplicaciones de .NET Framework 4 en uso. Un mensaje enumera todas las aplicaciones de .NET Framework 4 en ejecución y proporciona la opción de cerrarlas antes de la instalación. Si el usuario lo confirma, el instalador cierra estas aplicaciones y se evita el reinicio del sistema. Si el usuario no responde al mensaje dentro de un período de tiempo determinado, la instalación continúa sin cerrar ninguna aplicación.

 Si el Administrador de reinicio detecta una situación que requiera un reinicio del sistema aun cuando las aplicaciones en ejecución estén cerradas, el mensaje no se muestra.

 ![Cuadro de diálogo Cerrar aplicación con una lista de los programas en ejecución.](./media/reducing-system-restarts/close-application-dialog.png)

## <a name="using-a-chained-installer"></a>Usar un instalador encadenado

 Si desea redistribuir .NET Framework con la aplicación, pero desea utilizar su propio programa de instalación e interfaz de usuario, puede incluir (encadenar) el proceso de instalación de .NET Framework en el proceso de configuración. Para obtener más información sobre instalaciones encadenadas, vea [Deployment Guide for Developers](deployment-guide-for-developers.md) (Guía de implementación para desarrolladores). Para reducir reinicios del sistema en instalaciones encadenadas, el instalador de .NET Framework proporciona la lista de aplicaciones que se deben cerrar al programa de instalación. El programa de instalación debe proporcionar esta información al usuario a través de una interfaz de usuario como un cuadro de mensaje, obtener la respuesta del usuario y, a continuación, devolver esta respuesta al instalador de .NET Framework. Para obtener un ejemplo de un instalador encadenado, vea el artículo [Cómo: Obtener el progreso del instalador de .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md).

 Si usa un instalador encadenado, pero no desea proporcionar su propio cuadro de mensaje para cerrar las aplicaciones, puede usar las opciones `/showrmui` y `/passive` en la línea de comandos al encadenar el proceso de instalación de .NET Framework. Cuando usa ambas opciones, el programa de instalación muestra el cuadro de mensaje de cierre de aplicaciones, en el caso de que puedan cerrarse para evitar reiniciar el sistema. Este cuadro de mensaje se comporta igual en modo pasivo que con la interfaz de usuario completa. Vea [Deployment Guide for Developers](deployment-guide-for-developers.md) (Guía de implementación para desarrolladores) para conocer el conjunto completo de opciones de línea de comandos para el paquete redistribuible de .NET Framework.

## <a name="see-also"></a>Vea también

- [Implementación](index.md)
- [Guía de implementación para desarrolladores](deployment-guide-for-developers.md)
- [Cómo: Obtener el progreso del instalador de .NET Framework 4.5](how-to-get-progress-from-the-dotnet-installer.md)
