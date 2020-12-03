---
title: 'Instalación de .NET en Ubuntu: .NET'
description: Se muestran las diversas maneras de instalar el SDK y el entorno de ejecución de .NET en Ubuntu.
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 22ce3379e028f065528e1f507a2d8c1ae598f0e8
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031856"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-ubuntu"></a><span data-ttu-id="f07a4-103">Instalación del SDK y el entorno de ejecución de .NET en Ubuntu</span><span class="sxs-lookup"><span data-stu-id="f07a4-103">Install the .NET SDK or the .NET Runtime on Ubuntu</span></span>

<span data-ttu-id="f07a4-104">.NET es compatible con Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="f07a4-104">.NET is supported on Ubuntu.</span></span> <span data-ttu-id="f07a4-105">En este artículo se describe cómo instalar .NET en Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="f07a4-105">This article describes how to install .NET on Ubuntu.</span></span> <span data-ttu-id="f07a4-106">Cuando una versión de Ubuntu no es compatible, .NET deja de ser compatible con esa versión.</span><span class="sxs-lookup"><span data-stu-id="f07a4-106">When an Ubuntu version falls out of support, .NET is no longer supported with that version.</span></span> <span data-ttu-id="f07a4-107">Pero estas instrucciones pueden ayudarle a conseguir que .NET se ejecute en esas versiones, aunque no se admita.</span><span class="sxs-lookup"><span data-stu-id="f07a4-107">However, these instructions may help you to get .NET running on those versions, even though it isn't supported.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="f07a4-108">Distribuciones admitidas</span><span class="sxs-lookup"><span data-stu-id="f07a4-108">Supported distributions</span></span>

<span data-ttu-id="f07a4-109">En la tabla siguiente se muestra una lista de versiones de .NET actualmente compatibles y las versiones de Ubuntu en las que se admiten.</span><span class="sxs-lookup"><span data-stu-id="f07a4-109">The following table is a list of currently supported .NET releases and the versions of Ubuntu they're supported on.</span></span> <span data-ttu-id="f07a4-110">Estas versiones siguen siendo compatibles hasta que la versión de [.NET llegue al fin del soporte técnico](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) o la versión de [Ubuntu llegue al final del ciclo de vida](https://wiki.ubuntu.com/Releases).</span><span class="sxs-lookup"><span data-stu-id="f07a4-110">These versions remain supported until either the version of [.NET reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Ubuntu reaches end-of-life](https://wiki.ubuntu.com/Releases).</span></span>

- <span data-ttu-id="f07a4-111">El símbolo ✔️ indica que todavía se admite la versión de Ubuntu o de .NET.</span><span class="sxs-lookup"><span data-stu-id="f07a4-111">A ✔️ indicates that the version of Ubuntu or .NET is still supported.</span></span>
- <span data-ttu-id="f07a4-112">El símbolo ❌ indica que la versión de Ubuntu o de .NET no se admite en esa versión de Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="f07a4-112">A ❌ indicates that the version of Ubuntu or .NET isn't supported on that Ubuntu release.</span></span>
- <span data-ttu-id="f07a4-113">Si una versión de Ubuntu y una versión de .NET tienen un símbolo ✔️, esa combinación de sistema operativo y .NET se admite.</span><span class="sxs-lookup"><span data-stu-id="f07a4-113">When both a version of Ubuntu and a version of .NET have ✔️, that OS and .NET combination is supported.</span></span>

| <span data-ttu-id="f07a4-114">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="f07a4-114">Ubuntu</span></span>                   | <span data-ttu-id="f07a4-115">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-115">.NET Core 2.1</span></span> | <span data-ttu-id="f07a4-116">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-116">.NET Core 3.1</span></span> | <span data-ttu-id="f07a4-117">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-117">.NET 5.0</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="f07a4-118">✔️ [20.10](#2010-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-118">✔️ [20.10](#2010-)</span></span>       | <span data-ttu-id="f07a4-119">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-119">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-120">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-120">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-121">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-121">✔️ 5.0</span></span> |
| <span data-ttu-id="f07a4-122">✔️ [20.04 (LTS)](#2004-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-122">✔️ [20.04 (LTS)](#2004-)</span></span> | <span data-ttu-id="f07a4-123">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-123">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-124">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-124">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-125">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-125">✔️ 5.0</span></span> |
| <span data-ttu-id="f07a4-126">❌ [19.10](#1910-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-126">❌ [19.10](#1910-)</span></span>       | <span data-ttu-id="f07a4-127">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-127">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-128">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-128">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-129">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-129">✔️ 5.0</span></span> |
| <span data-ttu-id="f07a4-130">❌ [19.04](#1904-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-130">❌ [19.04](#1904-)</span></span>       | <span data-ttu-id="f07a4-131">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-131">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-132">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-132">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-133">❌ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-133">❌ 5.0</span></span> |
| <span data-ttu-id="f07a4-134">❌ [18.10](#1810-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-134">❌ [18.10](#1810-)</span></span>       | <span data-ttu-id="f07a4-135">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-135">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-136">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-136">❌ 3.1</span></span>        | <span data-ttu-id="f07a4-137">❌ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-137">❌ 5.0</span></span> |
| <span data-ttu-id="f07a4-138">✔️ [18.04 (LTS)](#1804-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-138">✔️ [18.04 (LTS)](#1804-)</span></span> | <span data-ttu-id="f07a4-139">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-139">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-140">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-140">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-141">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-141">✔️ 5.0</span></span> |
| <span data-ttu-id="f07a4-142">❌ [17.10](#1710-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-142">❌ [17.10](#1710-)</span></span>       | <span data-ttu-id="f07a4-143">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-143">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-144">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-144">❌ 3.1</span></span>        | <span data-ttu-id="f07a4-145">❌ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-145">❌ 5.0</span></span> |
| <span data-ttu-id="f07a4-146">❌ [17.04](#1704-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-146">❌ [17.04](#1704-)</span></span>       | <span data-ttu-id="f07a4-147">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-147">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-148">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-148">❌ 3.1</span></span>        | <span data-ttu-id="f07a4-149">❌ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-149">❌ 5.0</span></span> |
| <span data-ttu-id="f07a4-150">❌ [16.10](#1610-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-150">❌ [16.10](#1610-)</span></span>       | <span data-ttu-id="f07a4-151">❌ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-151">❌ 2.1</span></span>        | <span data-ttu-id="f07a4-152">❌ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-152">❌ 3.1</span></span>        | <span data-ttu-id="f07a4-153">❌ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-153">❌ 5.0</span></span> |
| <span data-ttu-id="f07a4-154">✔️ [16.04 (LTS)](#1604-)</span><span class="sxs-lookup"><span data-stu-id="f07a4-154">✔️ [16.04 (LTS)](#1604-)</span></span> | <span data-ttu-id="f07a4-155">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-155">✔️ 2.1</span></span>        | <span data-ttu-id="f07a4-156">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f07a4-156">✔️ 3.1</span></span>        | <span data-ttu-id="f07a4-157">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-157">✔️ 5.0</span></span> |

<span data-ttu-id="f07a4-158">Las versiones siguientes de .NET ya no se admiten.</span><span class="sxs-lookup"><span data-stu-id="f07a4-158">The following versions of .NET are no longer supported.</span></span> <span data-ttu-id="f07a4-159">Las descargas de estas siguen estando publicadas:</span><span class="sxs-lookup"><span data-stu-id="f07a4-159">The downloads for these still remain published:</span></span>

- <span data-ttu-id="f07a4-160">3.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-160">3.0</span></span>
- <span data-ttu-id="f07a4-161">2.2</span><span class="sxs-lookup"><span data-stu-id="f07a4-161">2.2</span></span>
- <span data-ttu-id="f07a4-162">2.0</span><span class="sxs-lookup"><span data-stu-id="f07a4-162">2.0</span></span>

## <a name="remove-preview-versions"></a><span data-ttu-id="f07a4-163">Eliminación de versiones preliminares</span><span class="sxs-lookup"><span data-stu-id="f07a4-163">Remove preview versions</span></span>

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="how-to-install-other-versions"></a><span data-ttu-id="f07a4-164">Procedimiento para instalar otras versiones</span><span class="sxs-lookup"><span data-stu-id="f07a4-164">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="2010-"></a><span data-ttu-id="f07a4-165">20.10 ✔️</span><span class="sxs-lookup"><span data-stu-id="f07a4-165">20.10 ✔️</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f07a4-166">.NET Core 2.1 todavía no está disponible en la fuente de paquetes.</span><span class="sxs-lookup"><span data-stu-id="f07a4-166">.NET Core 2.1 isn't yet available in the package feed.</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/20.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="2004-"></a><span data-ttu-id="f07a4-167">20.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="f07a4-167">20.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="1910-"></a><span data-ttu-id="f07a4-168">19.10 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-168">19.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1904-"></a><span data-ttu-id="f07a4-169">19.04 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-169">19.04 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1810-"></a><span data-ttu-id="f07a4-170">18.10 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-170">18.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1804-"></a><span data-ttu-id="f07a4-171">18.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="f07a4-171">18.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="1710-"></a><span data-ttu-id="f07a4-172">17.10 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-172">17.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1704-"></a><span data-ttu-id="f07a4-173">17.04 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-173">17.04 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1610-"></a><span data-ttu-id="f07a4-174">16.10 ❌</span><span class="sxs-lookup"><span data-stu-id="f07a4-174">16.10 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1604-"></a><span data-ttu-id="f07a4-175">16.04 ✔️</span><span class="sxs-lookup"><span data-stu-id="f07a4-175">16.04 ✔️</span></span>

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="apt-update-sdk-or-runtime"></a><span data-ttu-id="f07a4-176">SDK o entorno de ejecución de actualización de APT</span><span class="sxs-lookup"><span data-stu-id="f07a4-176">APT update SDK or runtime</span></span>

<span data-ttu-id="f07a4-177">Cuando hay disponible una nueva versión de revisión para .NET, basta con actualizarla mediante APT con los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f07a4-177">When a new patch release is available for .NET, you can simply upgrade it through APT with the following commands:</span></span>

```bash
sudo apt-get update
sudo apt-get upgrade
```

## <a name="apt-troubleshooting"></a><span data-ttu-id="f07a4-178">Solución de problemas de APT</span><span class="sxs-lookup"><span data-stu-id="f07a4-178">APT troubleshooting</span></span>

<span data-ttu-id="f07a4-179">En esta sección se proporciona información sobre los errores comunes que puede recibir al usar APT para instalar .NET.</span><span class="sxs-lookup"><span data-stu-id="f07a4-179">This section provides information on common errors you may get while using APT to install .NET.</span></span>

### <a name="unable-to-find-package"></a><span data-ttu-id="f07a4-180">No se puede encontrar el paquete</span><span class="sxs-lookup"><span data-stu-id="f07a4-180">Unable to find package</span></span>

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

### <a name="unable-to-locate--some-packages-could-not-be-installed"></a><span data-ttu-id="f07a4-181">No se puede encontrar el paquete \\ No se han podido instalar algunos paquetes</span><span class="sxs-lookup"><span data-stu-id="f07a4-181">Unable to locate \\ Some packages could not be installed</span></span>

[!INCLUDE [package-manager-failed-to-find-deb](includes/package-manager-failed-to-find-deb.md)]

```bash
sudo apt-get install -y gpg
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/ubuntu/{os-version}/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y {dotnet-package}
```

### <a name="failed-to-fetch"></a><span data-ttu-id="f07a4-182">No se pudo capturar el elemento</span><span class="sxs-lookup"><span data-stu-id="f07a4-182">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-deb](includes/package-manager-failed-to-fetch-deb.md)]

## <a name="snap"></a><span data-ttu-id="f07a4-183">Snap</span><span class="sxs-lookup"><span data-stu-id="f07a4-183">Snap</span></span>

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a><span data-ttu-id="f07a4-184">Dependencias</span><span class="sxs-lookup"><span data-stu-id="f07a4-184">Dependencies</span></span>

<span data-ttu-id="f07a4-185">Al realizar la instalación con un administrador de paquetes, estas bibliotecas se instalan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="f07a4-185">When you install with a package manager, these libraries are installed for you.</span></span> <span data-ttu-id="f07a4-186">Pero si instala manualmente .NET o publica una aplicación independiente, deberá asegurarse de que estas bibliotecas estén instaladas:</span><span class="sxs-lookup"><span data-stu-id="f07a4-186">But, if you manually install .NET or you publish a self-contained app, you'll need to make sure these libraries are installed:</span></span>

- <span data-ttu-id="f07a4-187">libc6</span><span class="sxs-lookup"><span data-stu-id="f07a4-187">libc6</span></span>
- <span data-ttu-id="f07a4-188">libgcc1</span><span class="sxs-lookup"><span data-stu-id="f07a4-188">libgcc1</span></span>
- <span data-ttu-id="f07a4-189">libgssapi-krb5-2</span><span class="sxs-lookup"><span data-stu-id="f07a4-189">libgssapi-krb5-2</span></span>
- <span data-ttu-id="f07a4-190">libicu52 (para 14.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-190">libicu52 (for 14.x)</span></span>
- <span data-ttu-id="f07a4-191">libicu55 (para 16.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-191">libicu55 (for 16.x)</span></span>
- <span data-ttu-id="f07a4-192">libicu60 (para 18.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-192">libicu60 (for 18.x)</span></span>
- <span data-ttu-id="f07a4-193">libicu66 (para 20.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-193">libicu66 (for 20.x)</span></span>
- <span data-ttu-id="f07a4-194">libssl1.0.0 (para 14.x, 16.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-194">libssl1.0.0 (for 14.x, 16.x)</span></span>
- <span data-ttu-id="f07a4-195">libssl1.1 (para 18.x, 20.x)</span><span class="sxs-lookup"><span data-stu-id="f07a4-195">libssl1.1 (for 18.x, 20.x)</span></span>
- <span data-ttu-id="f07a4-196">libstdc++6</span><span class="sxs-lookup"><span data-stu-id="f07a4-196">libstdc++6</span></span>
- <span data-ttu-id="f07a4-197">zlib1g</span><span class="sxs-lookup"><span data-stu-id="f07a4-197">zlib1g</span></span>

<span data-ttu-id="f07a4-198">Para las aplicaciones de .NET en las que se usa el ensamblado *System.Drawing.Common*, también se necesita la dependencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="f07a4-198">For .NET apps that use the *System.Drawing.Common* assembly, you also need the following dependency:</span></span>

- <span data-ttu-id="f07a4-199">libgdiplus (versión 6.0.1 o posteriores)</span><span class="sxs-lookup"><span data-stu-id="f07a4-199">libgdiplus (version 6.0.1 or later)</span></span>

  > [!WARNING]
  > <span data-ttu-id="f07a4-200">Puede instalar una versión reciente de *libgdiplus* agregando el repositorio Mono al sistema.</span><span class="sxs-lookup"><span data-stu-id="f07a4-200">You can install a recent version of *libgdiplus* by adding the Mono repository to your system.</span></span> <span data-ttu-id="f07a4-201">Para obtener más información, vea <https://www.mono-project.com/download/stable/>.</span><span class="sxs-lookup"><span data-stu-id="f07a4-201">For more information, see <https://www.mono-project.com/download/stable/>.</span></span>

## <a name="scripted-install"></a><span data-ttu-id="f07a4-202">Instalación con script</span><span class="sxs-lookup"><span data-stu-id="f07a4-202">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="f07a4-203">Instalación manual</span><span class="sxs-lookup"><span data-stu-id="f07a4-203">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="f07a4-204">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f07a4-204">Next steps</span></span>

- [<span data-ttu-id="f07a4-205">Tutorial: Creación de una aplicación de consola con el SDK de .NET mediante Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f07a4-205">Tutorial: Create a console application with .NET SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)
