---
ms.openlocfilehash: 9c84c9f844a2a7efb9633c11634b069ef6b6033b
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804872"
---
### <a name="datagridview-no-longer-resets-fonts-for-customized-cell-styles"></a><span data-ttu-id="dcdc4-101">DataGridView ya no restablece las fuentes para los estilos de celda personalizados</span><span class="sxs-lookup"><span data-stu-id="dcdc4-101">DataGridView no longer resets fonts for customized cell styles</span></span>

<span data-ttu-id="dcdc4-102">Cuando la fuente ambiente cambia, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> ya no restablece las fuentes predeterminadas de estilo de celda para que coincidan con la fuente ambiente si se ha personalizado la fuente del estilo de celda.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-102">When the ambient font changes, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> no longer resets default cell style fonts to match the ambient font if the cell style font has been customized.</span></span>

#### <a name="change-description"></a><span data-ttu-id="dcdc4-103">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="dcdc4-103">Change description</span></span>

<span data-ttu-id="dcdc4-104">En versiones anteriores de .NET, si la fuente ambiente cambia, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> restablece e invalida las fuentes definidas por el usuario en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> y <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-104">In previous .NET versions, if the ambient font changes, <xref:System.Windows.Forms.DataGridViewElement.DataGridView> resets and overrides user-defined fonts in the <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle>, and <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> properties.</span></span>

<span data-ttu-id="dcdc4-105">A partir de .NET 5.0, si se configuran las opciones de fuente en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> o <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>, esas opciones se conservan incluso cuando cambia la fuente ambiente.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-105">Starting in .NET 5.0, if you configure font settings in the <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle>, or <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> properties, those settings are retained even when the ambient font changes.</span></span> <span data-ttu-id="dcdc4-106">Para cualquiera de estas propiedades en las que no se personalice la fuente, la fuente cambiará para que coincida con la configuración de la fuente ambiente.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-106">For any of these properties that you don't customize the font, the font will change to match the ambient font settings.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="dcdc4-107">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="dcdc4-107">Reason for change</span></span>

<span data-ttu-id="dcdc4-108">Con el [cambio de la fuente predeterminada en .NET Core 3.0](../../../../docs/core/compatibility/winforms.md#default-control-font-changed-to-segoe-ui-9-pt), también cambian los valores de fuente predeterminados para los distintos estilos de celda.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-108">With the [change of the default font in .NET Core 3.0](../../../../docs/core/compatibility/winforms.md#default-control-font-changed-to-segoe-ui-9-pt), the default font settings for the various cell styles also changed.</span></span> <span data-ttu-id="dcdc4-109">Este comportamiento no es el deseado para las aplicaciones que se basan en el estilo personalizado en sus controles <xref:System.Windows.Forms.DataGridViewElement.DataGridView> y ha impedido la migración de estas aplicaciones de .NET Framework a .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-109">This behavior is undesirable for apps that rely on custom styling in their <xref:System.Windows.Forms.DataGridViewElement.DataGridView> controls, and impeded the migration of these apps from .NET Framework to .NET 5.0.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="dcdc4-110">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="dcdc4-110">Version introduced</span></span>

<span data-ttu-id="dcdc4-111">.NET 5.0 RC2</span><span class="sxs-lookup"><span data-stu-id="dcdc4-111">.NET 5.0 RC2</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="dcdc4-112">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="dcdc4-112">Recommended action</span></span>

<span data-ttu-id="dcdc4-113">No es necesario que realice ninguna acción.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-113">No action is required on your part.</span></span> <span data-ttu-id="dcdc4-114">Pero si ha personalizado la fuente en las propiedades <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> o <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle>, y quiere que la fuente coincida con la fuente ambiente, establezca <xref:System.Windows.Forms.DataGridViewCellStyle.Font?displayProperty=nameWithType> en `null` para cada propiedad.</span><span class="sxs-lookup"><span data-stu-id="dcdc4-114">However, if you've customized the font in the <xref:System.Windows.Forms.DataGridView.DefaultCellStyle>, <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle>, or <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> properties and want the font to match the ambient font, set <xref:System.Windows.Forms.DataGridViewCellStyle.Font?displayProperty=nameWithType> to `null` for each property.</span></span>

#### <a name="category"></a><span data-ttu-id="dcdc4-115">Categoría</span><span class="sxs-lookup"><span data-stu-id="dcdc4-115">Category</span></span>

- <span data-ttu-id="dcdc4-116">Windows Forms</span><span class="sxs-lookup"><span data-stu-id="dcdc4-116">Windows Forms</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="dcdc4-117">API afectadas</span><span class="sxs-lookup"><span data-stu-id="dcdc4-117">Affected APIs</span></span>

- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Windows.Forms.DataGridView.DefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle`

-->