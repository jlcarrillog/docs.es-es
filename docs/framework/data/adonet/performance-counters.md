---
title: Contadores de rendimiento
description: Use los contadores de rendimiento de ADO.NET para supervisar el estado de la aplicación y sus recursos de conexión mediante el monitor de rendimiento de Windows o mediante programación.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.openlocfilehash: dca259ef6d8a2229349d88a9fcae3298cb8a829c
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739469"
---
# <a name="performance-counters-in-adonet"></a><span data-ttu-id="db2b3-103">Contadores de rendimiento de ADO.NET</span><span class="sxs-lookup"><span data-stu-id="db2b3-103">Performance Counters in ADO.NET</span></span>

<span data-ttu-id="db2b3-104">ADO.NET 2.0 incorporó la compatibilidad expandida para los contadores de rendimiento que incluye la compatibilidad tanto con <xref:System.Data.SqlClient> como con <xref:System.Data.OracleClient>.</span><span class="sxs-lookup"><span data-stu-id="db2b3-104">ADO.NET 2.0 introduced expanded support for performance counters that includes support for both <xref:System.Data.SqlClient> and <xref:System.Data.OracleClient>.</span></span> <span data-ttu-id="db2b3-105">Los contadores de rendimiento <xref:System.Data.SqlClient> que estaban disponibles en las versiones anteriores de ADO.NET están en desuso y se han sustituido por los nuevos contadores de rendimiento que se describen aquí.</span><span class="sxs-lookup"><span data-stu-id="db2b3-105">The <xref:System.Data.SqlClient> performance counters available in previous versions of ADO.NET have been deprecated and replaced with the new performance counters discussed in this topic.</span></span> <span data-ttu-id="db2b3-106">Puede utilizar los contadores de rendimiento de ADO.NET para supervisar el estado de su aplicación y los recursos de conexión que emplea.</span><span class="sxs-lookup"><span data-stu-id="db2b3-106">You can use ADO.NET performance counters to monitor the status of your application and the connection resources that it uses.</span></span> <span data-ttu-id="db2b3-107">Los contadores de rendimiento se pueden controlar con el Monitor de rendimiento de Windows pero también se puede tener acceso a ellos mediante programación usando la clase <xref:System.Diagnostics.PerformanceCounter> del espacio de nombres <xref:System.Diagnostics>.</span><span class="sxs-lookup"><span data-stu-id="db2b3-107">Performance counters can be monitored by using Windows Performance Monitor or can be accessed programmatically using the <xref:System.Diagnostics.PerformanceCounter> class in the <xref:System.Diagnostics> namespace.</span></span>

## <a name="available-performance-counters"></a><span data-ttu-id="db2b3-108">Contadores de rendimiento disponibles</span><span class="sxs-lookup"><span data-stu-id="db2b3-108">Available Performance Counters</span></span>

 <span data-ttu-id="db2b3-109">Actualmente hay disponibles 14 contadores de rendimiento para <xref:System.Data.SqlClient> y <xref:System.Data.OracleClient>, tal y como se describe en la siguiente tabla.</span><span class="sxs-lookup"><span data-stu-id="db2b3-109">Currently there are 14 different performance counters available for <xref:System.Data.SqlClient> and <xref:System.Data.OracleClient> as described in the following table.</span></span> <span data-ttu-id="db2b3-110">Tenga en cuenta que los nombres de los contadores individuales no se localizan en las versiones regionales de Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="db2b3-110">Note that the names for the individual counters are not localized across regional versions of the Microsoft .NET Framework.</span></span>

|<span data-ttu-id="db2b3-111">Contador de rendimiento</span><span class="sxs-lookup"><span data-stu-id="db2b3-111">Performance counter</span></span>|<span data-ttu-id="db2b3-112">Descripción</span><span class="sxs-lookup"><span data-stu-id="db2b3-112">Description</span></span>|
|-------------------------|-----------------|
|`HardConnectsPerSecond`|<span data-ttu-id="db2b3-113">El número de conexiones por segundo que se establecen con un servidor de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="db2b3-113">The number of connections per second that are being made to a database server.</span></span>|
|`HardDisconnectsPerSecond`|<span data-ttu-id="db2b3-114">El número de desconexiones por segundo que se producen con un servidor de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="db2b3-114">The number of disconnects per second that are being made to a database server.</span></span>|
|`NumberOfActiveConnectionPoolGroups`|<span data-ttu-id="db2b3-115">El número de conjuntos de grupos de conexiones únicas que están activos.</span><span class="sxs-lookup"><span data-stu-id="db2b3-115">The number of unique connection pool groups that are active.</span></span> <span data-ttu-id="db2b3-116">Este contador depende del número de cadenas de conexión única que haya en el AppDomain.</span><span class="sxs-lookup"><span data-stu-id="db2b3-116">This counter is controlled by the number of unique connection strings that are found in the AppDomain.</span></span>|
|`NumberOfActiveConnectionPools`|<span data-ttu-id="db2b3-117">El número total de grupos de conexiones.</span><span class="sxs-lookup"><span data-stu-id="db2b3-117">The total number of connection pools.</span></span>|
|`NumberOfActiveConnections`|<span data-ttu-id="db2b3-118">El número de conexiones activas que se están utilizando actualmente.</span><span class="sxs-lookup"><span data-stu-id="db2b3-118">The number of active connections that are currently in use.</span></span> <span data-ttu-id="db2b3-119">**Nota:**  Este contador de rendimiento no está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="db2b3-119">**Note:**  This performance counter is not enabled by default.</span></span> <span data-ttu-id="db2b3-120">Para habilitar este contador de rendimiento, consulte [activación de contadores desactivados de forma predeterminada](#ActivatingOffByDefault).</span><span class="sxs-lookup"><span data-stu-id="db2b3-120">To enable this performance counter, see [Activating Off-By-Default Counters](#ActivatingOffByDefault).</span></span>|
|`NumberOfFreeConnections`|<span data-ttu-id="db2b3-121">El número de conexiones que se pueden utilizar en los grupos de conexiones.</span><span class="sxs-lookup"><span data-stu-id="db2b3-121">The number of connections available for use in the connection pools.</span></span> <span data-ttu-id="db2b3-122">**Nota:**  Este contador de rendimiento no está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="db2b3-122">**Note:**  This performance counter is not enabled by default.</span></span> <span data-ttu-id="db2b3-123">Para habilitar este contador de rendimiento, consulte [activación de contadores desactivados de forma predeterminada](#ActivatingOffByDefault).</span><span class="sxs-lookup"><span data-stu-id="db2b3-123">To enable this performance counter, see [Activating Off-By-Default Counters](#ActivatingOffByDefault).</span></span>|
|`NumberOfInactiveConnectionPoolGroups`|<span data-ttu-id="db2b3-124">El número de conjuntos de grupos de conexiones únicas que están marcados para ser eliminados.</span><span class="sxs-lookup"><span data-stu-id="db2b3-124">The number of unique connection pool groups that are marked for pruning.</span></span> <span data-ttu-id="db2b3-125">Este contador depende del número de cadenas de conexión única que haya en el AppDomain.</span><span class="sxs-lookup"><span data-stu-id="db2b3-125">This counter is controlled by the number of unique connection strings that are found in the AppDomain.</span></span>|
|`NumberOfInactiveConnectionPools`|<span data-ttu-id="db2b3-126">El número de grupos de conexiones inactivas que no han tenido ninguna actividad recientemente y que están a la espera de ser eliminadas.</span><span class="sxs-lookup"><span data-stu-id="db2b3-126">The number of inactive connection pools that have not had any recent activity and are waiting to be disposed.</span></span>|
|`NumberOfNonPooledConnections`|<span data-ttu-id="db2b3-127">El número de conexiones activas que no están agrupadas.</span><span class="sxs-lookup"><span data-stu-id="db2b3-127">The number of active connections that are not pooled.</span></span>|
|`NumberOfPooledConnections`|<span data-ttu-id="db2b3-128">El número de conexiones activas que administra la infraestructura de agrupación de conexiones.</span><span class="sxs-lookup"><span data-stu-id="db2b3-128">The number of active connections that are being managed by the connection pooling infrastructure.</span></span>|
|`NumberOfReclaimedConnections`|<span data-ttu-id="db2b3-129">El número de conexiones que se han reclamado a través de la recolección de elementos no utilizados si la aplicación no llamó a `Close` o `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="db2b3-129">The number of connections that have been reclaimed through garbage collection where `Close` or `Dispose` was not called by the application.</span></span> <span data-ttu-id="db2b3-130">Si no se cierran o eliminan explícitamente las conexiones, el rendimiento se verá perjudicado.</span><span class="sxs-lookup"><span data-stu-id="db2b3-130">Not explicitly closing or disposing connections hurts performance.</span></span>|
|`NumberOfStasisConnections`|<span data-ttu-id="db2b3-131">El número de conexiones que están actualmente en espera de que se finalice una acción y que por lo tanto no pueden ser utilizadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="db2b3-131">The number of connections currently awaiting completion of an action and which are therefore unavailable for use by your application.</span></span>|
|`SoftConnectsPerSecond`|<span data-ttu-id="db2b3-132">El número de conexiones activas que se están extrayendo del grupo de conexiones.</span><span class="sxs-lookup"><span data-stu-id="db2b3-132">The number of active connections being pulled from the connection pool.</span></span> <span data-ttu-id="db2b3-133">**Nota:**  Este contador de rendimiento no está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="db2b3-133">**Note:**  This performance counter is not enabled by default.</span></span> <span data-ttu-id="db2b3-134">Para habilitar este contador de rendimiento, consulte [activación de contadores desactivados de forma predeterminada](#ActivatingOffByDefault).</span><span class="sxs-lookup"><span data-stu-id="db2b3-134">To enable this performance counter, see [Activating Off-By-Default Counters](#ActivatingOffByDefault).</span></span>|
|`SoftDisconnectsPerSecond`|<span data-ttu-id="db2b3-135">El número de conexiones activas que se devuelven al grupo de conexiones.</span><span class="sxs-lookup"><span data-stu-id="db2b3-135">The number of active connections that are being returned to the connection pool.</span></span> <span data-ttu-id="db2b3-136">**Nota:**  Este contador de rendimiento no está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="db2b3-136">**Note:**  This performance counter is not enabled by default.</span></span> <span data-ttu-id="db2b3-137">Para habilitar este contador de rendimiento, consulte [activación de contadores desactivados de forma predeterminada](#ActivatingOffByDefault).</span><span class="sxs-lookup"><span data-stu-id="db2b3-137">To enable this performance counter, see [Activating Off-By-Default Counters](#ActivatingOffByDefault).</span></span>|

### <a name="connection-pool-groups-and-connection-pools"></a><span data-ttu-id="db2b3-138">Conjuntos de grupos de conexiones y grupos de conexiones</span><span class="sxs-lookup"><span data-stu-id="db2b3-138">Connection Pool Groups and Connection Pools</span></span>

 <span data-ttu-id="db2b3-139">Si utiliza la autenticación de Windows (seguridad integrada) debe supervisar los contadores de rendimiento `NumberOfActiveConnectionPoolGroups` y `NumberOfActiveConnectionPools`.</span><span class="sxs-lookup"><span data-stu-id="db2b3-139">When using Windows Authentication (integrated security), you must monitor both the `NumberOfActiveConnectionPoolGroups` and `NumberOfActiveConnectionPools` performance counters.</span></span> <span data-ttu-id="db2b3-140">El motivo es que los conjuntos de grupos de conexiones se corresponden con cadenas de conexión única.</span><span class="sxs-lookup"><span data-stu-id="db2b3-140">The reason is that connection pool groups map to unique connection strings.</span></span> <span data-ttu-id="db2b3-141">Si se usa seguridad integrada, los grupos de conexiones se asignan a cadenas de conexión y además crean grupos diferentes para cada identidad de Windows.</span><span class="sxs-lookup"><span data-stu-id="db2b3-141">When integrated security is used, connection pools map to connection strings and additionally create separate pools for individual Windows identities.</span></span> <span data-ttu-id="db2b3-142">Por ejemplo, si Alfredo y Julia, los dos dentro del mismo AppDomain, utilizan la cadena de conexión `"Data Source=MySqlServer;Integrated Security=true"`, se crea un conjunto de grupos de conexiones para la cadena de conexión y dos grupos adicionales, uno para Alfredo y otro para Julia.</span><span class="sxs-lookup"><span data-stu-id="db2b3-142">For example, if Fred and Julie, each within the same AppDomain, both use the connection string `"Data Source=MySqlServer;Integrated Security=true"`, a connection pool group is created for the connection string, and two additional pools are created, one for Fred and one for Julie.</span></span> <span data-ttu-id="db2b3-143">Si John y Marta usan una cadena de conexión con un inicio de sesión de SQL Server idéntico, `"Data Source=MySqlServer;User Id=lowPrivUser;Password=[PLACEHOLDER]"` , solo se creará un único grupo para la identidad **lowPrivUser** .</span><span class="sxs-lookup"><span data-stu-id="db2b3-143">If John and Martha use a connection string with an identical SQL Server login, `"Data Source=MySqlServer;User Id=lowPrivUser;Password=[PLACEHOLDER]"`, then only a single pool is created for the **lowPrivUser** identity.</span></span>

<a name="ActivatingOffByDefault"></a>

### <a name="activating-off-by-default-counters"></a><span data-ttu-id="db2b3-144">Activar contadores desactivados de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="db2b3-144">Activating Off-By-Default Counters</span></span>

 <span data-ttu-id="db2b3-145">Los contadores de rendimiento `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` y `SoftConnectsPerSecond` están desactivados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="db2b3-145">The performance counters `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond`, and `SoftConnectsPerSecond` are off by default.</span></span> <span data-ttu-id="db2b3-146">Agregue la siguiente información al archivo de configuración de la aplicación para habilitarlos:</span><span class="sxs-lookup"><span data-stu-id="db2b3-146">Add the following information to the application's configuration file to enable them:</span></span>

```xml
<system.diagnostics>
  <switches>
    <add name="ConnectionPoolPerformanceCounterDetail"
         value="4"/>
  </switches>
</system.diagnostics>
```

## <a name="retrieving-performance-counter-values"></a><span data-ttu-id="db2b3-147">Recuperar los valores de los contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="db2b3-147">Retrieving Performance Counter Values</span></span>

 <span data-ttu-id="db2b3-148">La siguiente aplicación de consola muestra cómo recuperar valores de los contadores de rendimiento en su aplicación.</span><span class="sxs-lookup"><span data-stu-id="db2b3-148">The following console application shows how to retrieve performance counter values in your application.</span></span> <span data-ttu-id="db2b3-149">Las conexiones deben estar abiertas y activas para que se devuelva información de todos los contadores de rendimiento de ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="db2b3-149">Connections must be open and active for information to be returned for all of the ADO.NET performance counters.</span></span>

> [!NOTE]
> <span data-ttu-id="db2b3-150">En este ejemplo se usa la base de datos de ejemplo **AdventureWorks** que se incluye con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db2b3-150">This example uses the sample **AdventureWorks** database included with SQL Server.</span></span> <span data-ttu-id="db2b3-151">Las cadenas de conexión que se incluyen en el código de ejemplo presuponen que la base de datos está instalada y disponible en el equipo local con el nombre de instancia SqlExpress y que se han creado inicios de sesión de SQL Server que coinciden con los proporcionados en las cadenas de conexión.</span><span class="sxs-lookup"><span data-stu-id="db2b3-151">The connection strings provided in the sample code assume that the database is installed and available on the local computer with an instance name of SqlExpress, and that you have created SQL Server logins that match those supplied in the connection strings.</span></span> <span data-ttu-id="db2b3-152">Quizá deba habilitar inicios de sesión de SQL Server si su servidor se ha configurado usando la configuración de seguridad predeterminada, que solo admite la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="db2b3-152">You may need to enable SQL Server logins if your server is configured using the default security settings which allow only Windows Authentication.</span></span> <span data-ttu-id="db2b3-153">Modifique las cadenas de conexión según sea necesario para su entorno.</span><span class="sxs-lookup"><span data-stu-id="db2b3-153">Modify the connection strings as necessary to suit your environment.</span></span>

### <a name="example"></a><span data-ttu-id="db2b3-154">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="db2b3-154">Example</span></span>

```vb
Option Explicit On
Option Strict On

Imports System.Data.SqlClient
Imports System.Diagnostics
Imports System.Runtime.InteropServices

Class Program

    Private PerfCounters(9) As PerformanceCounter
    Private connection As SqlConnection = New SqlConnection

    Public Shared Sub Main()
        Dim prog As Program = New Program
        ' Open a connection and create the performance counters.
        prog.connection.ConnectionString = _
           GetIntegratedSecurityConnectionString()
        prog.SetUpPerformanceCounters()
        Console.WriteLine("Available Performance Counters:")

        ' Create the connections and display the results.
        prog.CreateConnections()
        Console.WriteLine("Press Enter to finish.")
        Console.ReadLine()
    End Sub

    Private Sub CreateConnections()
        ' List the Performance counters.
        WritePerformanceCounters()

        ' Create 4 connections and display counter information.
        Dim connection1 As SqlConnection = New SqlConnection( _
           GetIntegratedSecurityConnectionString)
        connection1.Open()
        Console.WriteLine("Opened the 1st Connection:")
        WritePerformanceCounters()

        Dim connection2 As SqlConnection = New SqlConnection( _
           GetSqlConnectionStringDifferent)
        connection2.Open()
        Console.WriteLine("Opened the 2nd Connection:")
        WritePerformanceCounters()

        Console.WriteLine("Opened the 3rd Connection:")
        Dim connection3 As SqlConnection = New SqlConnection( _
           GetSqlConnectionString)
        connection3.Open()
        WritePerformanceCounters()

        Dim connection4 As SqlConnection = New SqlConnection( _
           GetSqlConnectionString)
        connection4.Open()
        Console.WriteLine("Opened the 4th Connection:")
        WritePerformanceCounters()

        connection1.Close()
        Console.WriteLine("Closed the 1st Connection:")
        WritePerformanceCounters()

        connection2.Close()
        Console.WriteLine("Closed the 2nd Connection:")
        WritePerformanceCounters()

        connection3.Close()
        Console.WriteLine("Closed the 3rd Connection:")
        WritePerformanceCounters()

        connection4.Close()
        Console.WriteLine("Closed the 4th Connection:")
        WritePerformanceCounters()
    End Sub

    Private Enum ADO_Net_Performance_Counters
        NumberOfActiveConnectionPools
        NumberOfReclaimedConnections
        HardConnectsPerSecond
        HardDisconnectsPerSecond
        NumberOfActiveConnectionPoolGroups
        NumberOfInactiveConnectionPoolGroups
        NumberOfInactiveConnectionPools
        NumberOfNonPooledConnections
        NumberOfPooledConnections
        NumberOfStasisConnections
        ' The following performance counters are more expensive to track.
        ' Enable ConnectionPoolPerformanceCounterDetail in your config file.
        '     SoftConnectsPerSecond
        '     SoftDisconnectsPerSecond
        '     NumberOfActiveConnections
        '     NumberOfFreeConnections
    End Enum

    Private Sub SetUpPerformanceCounters()
        connection.Close()
        Me.PerfCounters(9) = New PerformanceCounter()

        Dim instanceName As String = GetInstanceName()
        Dim apc As Type = GetType(ADO_Net_Performance_Counters)
        Dim i As Integer = 0
        Dim s As String = ""
        For Each s In [Enum].GetNames(apc)
            Me.PerfCounters(i) = New PerformanceCounter()
            Me.PerfCounters(i).CategoryName = ".NET Data Provider for SqlServer"
            Me.PerfCounters(i).CounterName = s
            Me.PerfCounters(i).InstanceName = instanceName
            i = (i + 1)
        Next
    End Sub

    Private Declare Function GetCurrentProcessId Lib "kernel32.dll" () As Integer

    Private Function GetInstanceName() As String
        'This works for Winforms apps.
        Dim instanceName As String = _
           System.Reflection.Assembly.GetEntryAssembly.GetName.Name

        ' Must replace special characters like (, ), #, /, \\
        Dim instanceName2 As String = _
           AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _
           .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")

        'For ASP.NET applications your instanceName will be your CurrentDomain's
        'FriendlyName. Replace the line above that sets the instanceName with this:
        'instanceName = AppDomain.CurrentDomain.FriendlyName.ToString.Replace("(", "[") _
        '    .Replace(")", "]").Replace("#", "_").Replace("/", "_").Replace("\\", "_")

        Dim pid As String = GetCurrentProcessId.ToString
        instanceName = (instanceName + ("[" & (pid & "]")))
        Console.WriteLine("Instance Name: {0}", instanceName)
        Console.WriteLine("---------------------------")
        Return instanceName
    End Function

    Private Sub WritePerformanceCounters()
        Console.WriteLine("---------------------------")
        For Each p As PerformanceCounter In Me.PerfCounters
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue)
        Next
        Console.WriteLine("---------------------------")
    End Sub

    Private Shared Function GetIntegratedSecurityConnectionString() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;Integrated Security=True;" &
          "Initial Catalog=AdventureWorks")
    End Function

    Private Shared Function GetSqlConnectionString() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Data Source=.\SqlExpress;User Id=LowPriv;Password=[PLACEHOLDER];" &
          "Initial Catalog=AdventureWorks")
    End Function

    Private Shared Function GetSqlConnectionStringDifferent() As String
        ' To avoid storing the connection string in your code,
        ' you can retrieve it from a configuration file.
        Return ("Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" & _
          "User Id=LowPriv;Password=[PLACEHOLDER];")
    End Function
End Class
```

```csharp
using System;
using System.Data.SqlClient;
using System.Diagnostics;
using System.Runtime.InteropServices;

class Program
{
    PerformanceCounter[] PerfCounters = new PerformanceCounter[10];
    SqlConnection connection = new SqlConnection();

    static void Main()
    {
        Program prog = new Program();
        // Open a connection and create the performance counters.
        prog.connection.ConnectionString =
           GetIntegratedSecurityConnectionString();
        prog.SetUpPerformanceCounters();
        Console.WriteLine("Available Performance Counters:");

        // Create the connections and display the results.
        prog.CreateConnections();
        Console.WriteLine("Press Enter to finish.");
        Console.ReadLine();
    }

    private void CreateConnections()
    {
        // List the Performance counters.
        WritePerformanceCounters();

        // Create 4 connections and display counter information.
        SqlConnection connection1 = new SqlConnection(
              GetIntegratedSecurityConnectionString());
        connection1.Open();
        Console.WriteLine("Opened the 1st Connection:");
        WritePerformanceCounters();

        SqlConnection connection2 = new SqlConnection(
              GetSqlConnectionStringDifferent());
        connection2.Open();
        Console.WriteLine("Opened the 2nd Connection:");
        WritePerformanceCounters();

        SqlConnection connection3 = new SqlConnection(
              GetSqlConnectionString());
        connection3.Open();
        Console.WriteLine("Opened the 3rd Connection:");
        WritePerformanceCounters();

        SqlConnection connection4 = new SqlConnection(
              GetSqlConnectionString());
        connection4.Open();
        Console.WriteLine("Opened the 4th Connection:");
        WritePerformanceCounters();

        connection1.Close();
        Console.WriteLine("Closed the 1st Connection:");
        WritePerformanceCounters();

        connection2.Close();
        Console.WriteLine("Closed the 2nd Connection:");
        WritePerformanceCounters();

        connection3.Close();
        Console.WriteLine("Closed the 3rd Connection:");
        WritePerformanceCounters();

        connection4.Close();
        Console.WriteLine("Closed the 4th Connection:");
        WritePerformanceCounters();
    }

    private enum ADO_Net_Performance_Counters
    {
        NumberOfActiveConnectionPools,
        NumberOfReclaimedConnections,
        HardConnectsPerSecond,
        HardDisconnectsPerSecond,
        NumberOfActiveConnectionPoolGroups,
        NumberOfInactiveConnectionPoolGroups,
        NumberOfInactiveConnectionPools,
        NumberOfNonPooledConnections,
        NumberOfPooledConnections,
        NumberOfStasisConnections
        // The following performance counters are more expensive to track.
        // Enable ConnectionPoolPerformanceCounterDetail in your config file.
        //     SoftConnectsPerSecond
        //     SoftDisconnectsPerSecond
        //     NumberOfActiveConnections
        //     NumberOfFreeConnections
    }

    private void SetUpPerformanceCounters()
    {
        connection.Close();
        this.PerfCounters = new PerformanceCounter[10];
        string instanceName = GetInstanceName();
        Type apc = typeof(ADO_Net_Performance_Counters);
        int i = 0;
        foreach (string s in Enum.GetNames(apc))
        {
            this.PerfCounters[i] = new PerformanceCounter();
            this.PerfCounters[i].CategoryName = ".NET Data Provider for SqlServer";
            this.PerfCounters[i].CounterName = s;
            this.PerfCounters[i].InstanceName = instanceName;
            i++;
        }
    }

    [DllImport("kernel32.dll", SetLastError = true)]
    static extern int GetCurrentProcessId();

    private string GetInstanceName()
    {
        //This works for Winforms apps.
        string instanceName =
            System.Reflection.Assembly.GetEntryAssembly().GetName().Name;

        // Must replace special characters like (, ), #, /, \\
        string instanceName2 =
            AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(', '[')
            .Replace(')', ']').Replace('#', '_').Replace('/', '_').Replace('\\', '_');

        // For ASP.NET applications your instanceName will be your CurrentDomain's
        // FriendlyName. Replace the line above that sets the instanceName with this:
        // instanceName = AppDomain.CurrentDomain.FriendlyName.ToString().Replace('(','[')
        // .Replace(')',']').Replace('#','_').Replace('/','_').Replace('\\','_');

        string pid = GetCurrentProcessId().ToString();
        instanceName = instanceName + "[" + pid + "]";
        Console.WriteLine("Instance Name: {0}", instanceName);
        Console.WriteLine("---------------------------");
        return instanceName;
    }

    private void WritePerformanceCounters()
    {
        Console.WriteLine("---------------------------");
        foreach (PerformanceCounter p in this.PerfCounters)
        {
            Console.WriteLine("{0} = {1}", p.CounterName, p.NextValue());
        }
        Console.WriteLine("---------------------------");
    }

    private static string GetIntegratedSecurityConnectionString()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Data Source=.\SqlExpress;Integrated Security=True;" +
          "Initial Catalog=AdventureWorks";
    }
    private static string GetSqlConnectionString()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Data Source=.\SqlExpress;User Id=LowPriv;Password=[PLACEHOLDER];" +
          "Initial Catalog=AdventureWorks";
    }

    private static string GetSqlConnectionStringDifferent()
    {
        // To avoid storing the connection string in your code,
        // you can retrieve it from a configuration file.
        return @"Initial Catalog=AdventureWorks;Data Source=.\SqlExpress;" +
          "User Id=LowPriv;Password=[PLACEHOLDER];";
    }
}
```

## <a name="see-also"></a><span data-ttu-id="db2b3-155">Vea también</span><span class="sxs-lookup"><span data-stu-id="db2b3-155">See also</span></span>

- [<span data-ttu-id="db2b3-156">Conectarse a un origen de datos</span><span class="sxs-lookup"><span data-stu-id="db2b3-156">Connecting to a Data Source</span></span>](connecting-to-a-data-source.md)
- [<span data-ttu-id="db2b3-157">Agrupación de conexiones de OLE DB, ODBC y Oracle</span><span class="sxs-lookup"><span data-stu-id="db2b3-157">OLE DB, ODBC, and Oracle Connection Pooling</span></span>](ole-db-odbc-and-oracle-connection-pooling.md)
- <span data-ttu-id="db2b3-158">[Contadores de rendimiento para ASP.NET](/previous-versions/aspnet/fxk122b4(v=vs.100))</span><span class="sxs-lookup"><span data-stu-id="db2b3-158">[Performance Counters for ASP.NET](/previous-versions/aspnet/fxk122b4(v=vs.100))</span></span>
- [<span data-ttu-id="db2b3-159">Generar perfiles en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="db2b3-159">Runtime Profiling</span></span>](../../debug-trace-profile/runtime-profiling.md)
- <span data-ttu-id="db2b3-160">[Introducción a los umbrales de rendimiento de supervisión](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))</span><span class="sxs-lookup"><span data-stu-id="db2b3-160">[Introduction to Monitoring Performance Thresholds](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))</span></span>
- [<span data-ttu-id="db2b3-161">Información general de ADO.NET</span><span class="sxs-lookup"><span data-stu-id="db2b3-161">ADO.NET Overview</span></span>](ado-net-overview.md)
