---
ms.openlocfilehash: 4788f5b80306116e63ee56584d65b862ce0606ee
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496539"
---
### <a name="rsacng-and-dsacng-are-once-again-usable-in-partial-trust-scenarios"></a>Se puede volver a usar RSACng y DSACng en escenarios de confianza parcial

#### <a name="details"></a>Detalles

En algunos casos, CngLightup (que se usa en varias API de criptografía de nivel superior, como <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType>) y <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> se basan en la plena confianza. Estos casos incluyen P/Invoke sin permisos <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType> de aserción y rutas de código en las que <xref:System.Security.Cryptography.CngKey?displayProperty=nameWithType> tiene peticiones de permiso para <xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode?displayProperty=nameWithType>. A partir de .NET Framework 4.6.2, CngLightup se usaba para cambiar a <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> siempre que era posible. Como resultado, las aplicaciones de confianza parcial que usaban <xref:System.Security.Cryptography.Xml.EncryptedXml?displayProperty=nameWithType> correctamente comenzaron a producir errores e iniciar <xref:System.Security.SecurityException> excepciones. Este cambio agrega las aserciones necesarias para que todas las funciones en las que se usa CngLightup tengan los permisos necesarios.

#### <a name="suggestion"></a>Sugerencia

Si este cambio en .NET Framework 4.6.2 ha afectado negativamente a las aplicaciones de confianza parcial, actualice a .NET Framework 4.7.1.

| Nombre    | Valor       |
|:--------|:------------|
| Ámbito   |Borde|
|Versión|4.6.2|
|Tipo|Tiempo de ejecución|

#### <a name="affected-apis"></a>API afectadas

- <xref:System.Security.Cryptography.DSACng.%23ctor(System.Security.Cryptography.CngKey)>
- <xref:System.Security.Cryptography.DSACng.Key?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSACng.LegalKeySizes?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSACng.CreateSignature(System.Byte[])?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSACng.VerifySignature(System.Byte[],System.Byte[])?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSACng.%23ctor(System.Security.Cryptography.CngKey)>
- <xref:System.Security.Cryptography.RSACng.Key?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSACng.Decrypt(System.Byte[],System.Security.Cryptography.RSAEncryptionPadding)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSACng.SignHash(System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Security.Cryptography.DSACng.#ctor(System.Security.Cryptography.CngKey)`
- `P:System.Security.Cryptography.DSACng.Key`
- `P:System.Security.Cryptography.DSACng.LegalKeySizes`
- `M:System.Security.Cryptography.DSACng.CreateSignature(System.Byte[])`
- `M:System.Security.Cryptography.DSACng.VerifySignature(System.Byte[],System.Byte[])`
- `M:System.Security.Cryptography.RSACng.#ctor(System.Security.Cryptography.CngKey)`
- `P:System.Security.Cryptography.RSACng.Key`
- `M:System.Security.Cryptography.RSACng.Decrypt(System.Byte[],System.Security.Cryptography.RSAEncryptionPadding)`
- `M:System.Security.Cryptography.RSACng.SignHash(System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)`

-->
