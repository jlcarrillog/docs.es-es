---
title: Programación asincrónica
description: 'Obtenga información sobre cómo F # proporciona compatibilidad limpia para asincronía basándose en un modelo de programación de nivel de lenguaje derivado de conceptos básicos de la programación funcional.'
ms.date: 08/15/2020
ms.openlocfilehash: 04b397ddbfb468aa3bc4ee245175d3ec9bdedb50
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739332"
---
# <a name="async-programming-in-f"></a><span data-ttu-id="d5676-103">Programación asincrónica en F\#</span><span class="sxs-lookup"><span data-stu-id="d5676-103">Async programming in F\#</span></span>

<span data-ttu-id="d5676-104">La programación asincrónica es un mecanismo fundamental para las aplicaciones modernas por diversos motivos.</span><span class="sxs-lookup"><span data-stu-id="d5676-104">Asynchronous programming is a mechanism that is essential to modern applications for diverse reasons.</span></span> <span data-ttu-id="d5676-105">Hay dos casos de uso principales en los que se encontrarán la mayoría de los desarrolladores:</span><span class="sxs-lookup"><span data-stu-id="d5676-105">There are two primary use cases that most developers will encounter:</span></span>

- <span data-ttu-id="d5676-106">Presentar un proceso de servidor que pueda dar servicio a un número significativo de solicitudes entrantes simultáneas, al tiempo que se minimizan los recursos del sistema ocupados mientras el procesamiento de solicitudes espera entradas de sistemas o servicios externos a ese proceso.</span><span class="sxs-lookup"><span data-stu-id="d5676-106">Presenting a server process that can service a significant number of concurrent incoming requests, while minimizing the system resources occupied while request processing awaits inputs from systems or services external to that process</span></span>
- <span data-ttu-id="d5676-107">Mantener una interfaz de usuario con capacidad de respuesta o un subproceso principal mientras progresa simultáneamente el trabajo en segundo plano</span><span class="sxs-lookup"><span data-stu-id="d5676-107">Maintaining a responsive UI or main thread while concurrently progressing background work</span></span>

<span data-ttu-id="d5676-108">Aunque el trabajo en segundo plano implica la utilización de varios subprocesos, es importante tener en cuenta los conceptos de asincronía y multithreading por separado.</span><span class="sxs-lookup"><span data-stu-id="d5676-108">Although background work often does involve the utilization of multiple threads, it's important to consider the concepts of asynchrony and multi-threading separately.</span></span> <span data-ttu-id="d5676-109">De hecho, son aspectos independientes y uno no implica el otro.</span><span class="sxs-lookup"><span data-stu-id="d5676-109">In fact, they are separate concerns, and one does not imply the other.</span></span> <span data-ttu-id="d5676-110">En este artículo se describen los conceptos independientes con más detalle.</span><span class="sxs-lookup"><span data-stu-id="d5676-110">This article describes the separate concepts in more detail.</span></span>

## <a name="asynchrony-defined"></a><span data-ttu-id="d5676-111">Asincronía definido</span><span class="sxs-lookup"><span data-stu-id="d5676-111">Asynchrony defined</span></span>

<span data-ttu-id="d5676-112">El punto anterior, que asincronía es independiente del uso de varios subprocesos, merece la pena explicar un poco más.</span><span class="sxs-lookup"><span data-stu-id="d5676-112">The previous point - that asynchrony is independent of the utilization of multiple threads - is worth explaining a bit further.</span></span> <span data-ttu-id="d5676-113">Hay tres conceptos que a veces están relacionados, pero estrictamente independientes entre sí:</span><span class="sxs-lookup"><span data-stu-id="d5676-113">There are three concepts that are sometimes related, but strictly independent of one another:</span></span>

- <span data-ttu-id="d5676-114">Simultaneidad Cuando se ejecutan varios cálculos en períodos de tiempo superpuestos.</span><span class="sxs-lookup"><span data-stu-id="d5676-114">Concurrency; when multiple computations execute in overlapping time periods.</span></span>
- <span data-ttu-id="d5676-115">Paralelismo cuando varios cálculos o varias partes de un único cálculo se ejecutan exactamente al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="d5676-115">Parallelism; when multiple computations or several parts of a single computation run at exactly the same time.</span></span>
- <span data-ttu-id="d5676-116">Asincronía Cuando uno o varios cálculos pueden ejecutarse por separado desde el flujo del programa principal.</span><span class="sxs-lookup"><span data-stu-id="d5676-116">Asynchrony; when one or more computations can execute separately from the main program flow.</span></span>

<span data-ttu-id="d5676-117">Los tres son conceptos ortogonales, pero se pueden reagrupar fácilmente, especialmente cuando se usan juntos.</span><span class="sxs-lookup"><span data-stu-id="d5676-117">All three are orthogonal concepts, but can be easily conflated, especially when they are used together.</span></span> <span data-ttu-id="d5676-118">Por ejemplo, puede que necesite ejecutar varios cálculos asincrónicos en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-118">For example, you may need to execute multiple asynchronous computations in parallel.</span></span> <span data-ttu-id="d5676-119">Esta relación no significa que el paralelismo o asincronía impliquen entre sí.</span><span class="sxs-lookup"><span data-stu-id="d5676-119">This relationship does not mean that parallelism or asynchrony imply one another.</span></span>

<span data-ttu-id="d5676-120">Si tiene en cuenta el etymology de la palabra "Asynchronous", hay dos partes implicadas:</span><span class="sxs-lookup"><span data-stu-id="d5676-120">If you consider the etymology of the word "asynchronous", there are two pieces involved:</span></span>

- <span data-ttu-id="d5676-121">"a", que significa "no".</span><span class="sxs-lookup"><span data-stu-id="d5676-121">"a", meaning "not".</span></span>
- <span data-ttu-id="d5676-122">"sincrónico", que significa "al mismo tiempo".</span><span class="sxs-lookup"><span data-stu-id="d5676-122">"synchronous", meaning "at the same time".</span></span>

<span data-ttu-id="d5676-123">Al colocar estos dos términos juntos, verá que "asincrónico" significa "no al mismo tiempo".</span><span class="sxs-lookup"><span data-stu-id="d5676-123">When you put these two terms together, you'll see that "asynchronous" means "not at the same time".</span></span> <span data-ttu-id="d5676-124">Eso es todo.</span><span class="sxs-lookup"><span data-stu-id="d5676-124">That's it!</span></span> <span data-ttu-id="d5676-125">No hay ninguna implicación de simultaneidad o paralelismo en esta definición.</span><span class="sxs-lookup"><span data-stu-id="d5676-125">There is no implication of concurrency or parallelism in this definition.</span></span> <span data-ttu-id="d5676-126">Esto también se aplica en la práctica.</span><span class="sxs-lookup"><span data-stu-id="d5676-126">This is also true in practice.</span></span>

<span data-ttu-id="d5676-127">En términos prácticos, los cálculos asincrónicos en F # están programados para ejecutarse independientemente del flujo principal del programa.</span><span class="sxs-lookup"><span data-stu-id="d5676-127">In practical terms, asynchronous computations in F# are scheduled to execute independently of the main program flow.</span></span> <span data-ttu-id="d5676-128">Esta ejecución independiente no implica simultaneidad ni paralelismo, ni implica que un cálculo siempre se produce en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="d5676-128">This independent execution doesn't imply concurrency or parallelism, nor does it imply that a computation always happens in the background.</span></span> <span data-ttu-id="d5676-129">De hecho, los cálculos asincrónicos pueden incluso ejecutarse sincrónicamente, dependiendo de la naturaleza del cálculo y del entorno en el que se ejecuta el cálculo.</span><span class="sxs-lookup"><span data-stu-id="d5676-129">In fact, asynchronous computations can even execute synchronously, depending on the nature of the computation and the environment the computation is executing in.</span></span>

<span data-ttu-id="d5676-130">La principal ventaja que debe tener es que los cálculos asincrónicos son independientes del flujo principal del programa.</span><span class="sxs-lookup"><span data-stu-id="d5676-130">The main takeaway you should have is that asynchronous computations are independent of the main program flow.</span></span> <span data-ttu-id="d5676-131">Aunque hay pocas garantías sobre cuándo o cómo se ejecuta un cálculo asincrónico, existen algunos enfoques para orquestarlos y programarlos.</span><span class="sxs-lookup"><span data-stu-id="d5676-131">Although there are few guarantees about when or how an asynchronous computation executes, there are some approaches to orchestrating and scheduling them.</span></span> <span data-ttu-id="d5676-132">En el resto de este artículo se exploran los conceptos básicos de F # asincronía y cómo usar los tipos, las funciones y las expresiones integradas en F #.</span><span class="sxs-lookup"><span data-stu-id="d5676-132">The rest of this article explores core concepts for F# asynchrony and how to use the types, functions, and expressions built into F#.</span></span>

## <a name="core-concepts"></a><span data-ttu-id="d5676-133">Conceptos principales</span><span class="sxs-lookup"><span data-stu-id="d5676-133">Core concepts</span></span>

<span data-ttu-id="d5676-134">En F #, la programación asincrónica se centra en torno a tres conceptos básicos:</span><span class="sxs-lookup"><span data-stu-id="d5676-134">In F#, asynchronous programming is centered around three core concepts:</span></span>

- <span data-ttu-id="d5676-135">El `Async<'T>` tipo, que representa un cálculo asincrónico que admite composición.</span><span class="sxs-lookup"><span data-stu-id="d5676-135">The `Async<'T>` type, which represents a composable asynchronous computation.</span></span>
- <span data-ttu-id="d5676-136">Las `Async` funciones del módulo, que permiten programar el trabajo asincrónico, componer los cálculos asincrónicos y transformar los resultados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="d5676-136">The `Async` module functions, which let you schedule asynchronous work, compose asynchronous computations, and transform asynchronous results.</span></span>
- <span data-ttu-id="d5676-137">`async { }` [Expresión de cálculo](../../language-reference/computation-expressions.md), que proporciona una sintaxis adecuada para compilar y controlar los cálculos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="d5676-137">The `async { }` [computation expression](../../language-reference/computation-expressions.md), which provides a convenient syntax for building and controlling asynchronous computations.</span></span>

<span data-ttu-id="d5676-138">Puede ver estos tres conceptos en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d5676-138">You can see these three concepts in the following example:</span></span>

```fsharp
open System
open System.IO

let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn $"File {fileName} has %d{bytes.Length} bytes"
    }

[<EntryPoint>]
let main argv =
    printTotalFileBytes "path-to-file.txt"
    |> Async.RunSynchronously

    Console.Read() |> ignore
    0
```

<span data-ttu-id="d5676-139">En el ejemplo, la `printTotalFileBytes` función es de tipo `string -> Async<unit>` .</span><span class="sxs-lookup"><span data-stu-id="d5676-139">In the example, the `printTotalFileBytes` function is of type `string -> Async<unit>`.</span></span> <span data-ttu-id="d5676-140">La llamada a la función no ejecuta realmente el cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-140">Calling the function does not actually execute the asynchronous computation.</span></span> <span data-ttu-id="d5676-141">En su lugar, devuelve un `Async<unit>` que actúa como *especificación* del trabajo que se va a ejecutar de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="d5676-141">Instead, it returns an `Async<unit>` that acts as a *specification* of the work that is to execute asynchronously.</span></span> <span data-ttu-id="d5676-142">Llama `Async.AwaitTask` a en su cuerpo, que convierte el resultado de <xref:System.IO.File.ReadAllBytesAsync%2A> en un tipo adecuado.</span><span class="sxs-lookup"><span data-stu-id="d5676-142">It calls `Async.AwaitTask` in its body, which converts the result of <xref:System.IO.File.ReadAllBytesAsync%2A> to an appropriate type.</span></span>

<span data-ttu-id="d5676-143">Otra línea importante es la llamada a `Async.RunSynchronously` .</span><span class="sxs-lookup"><span data-stu-id="d5676-143">Another important line is the call to `Async.RunSynchronously`.</span></span> <span data-ttu-id="d5676-144">Esta es una de las funciones de inicio del módulo Async que debe llamar si desea ejecutar realmente un cálculo asincrónico de F #.</span><span class="sxs-lookup"><span data-stu-id="d5676-144">This is one of the Async module starting functions that you'll need to call if you want to actually execute an F# asynchronous computation.</span></span>

<span data-ttu-id="d5676-145">Esta es una diferencia fundamental con el estilo C#/Visual Basic de `async` programación.</span><span class="sxs-lookup"><span data-stu-id="d5676-145">This is a fundamental difference with the C#/Visual Basic style of `async` programming.</span></span> <span data-ttu-id="d5676-146">En F #, los cálculos asincrónicos se pueden considerar como **tareas en frío**.</span><span class="sxs-lookup"><span data-stu-id="d5676-146">In F#, asynchronous computations can be thought of as **Cold tasks**.</span></span> <span data-ttu-id="d5676-147">Deben iniciarse explícitamente para ejecutarse realmente.</span><span class="sxs-lookup"><span data-stu-id="d5676-147">They must be explicitly started to actually execute.</span></span> <span data-ttu-id="d5676-148">Esto tiene algunas ventajas, ya que permite combinar y secuenciar el trabajo asincrónico de forma mucho más sencilla que en C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d5676-148">This has some advantages, as it allows you to combine and sequence asynchronous work much more easily than in C# or Visual Basic.</span></span>

## <a name="combine-asynchronous-computations"></a><span data-ttu-id="d5676-149">Combinar cálculos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="d5676-149">Combine asynchronous computations</span></span>

<span data-ttu-id="d5676-150">Este es un ejemplo que se basa en el anterior mediante la combinación de cálculos:</span><span class="sxs-lookup"><span data-stu-id="d5676-150">Here is an example that builds upon the previous one by combining computations:</span></span>

```fsharp
open System
open System.IO

let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn $"File {fileName} has %d{bytes.Length} bytes"
    }

[<EntryPoint>]
let main argv =
    argv
    |> Array.map printTotalFileBytes
    |> Async.Parallel
    |> Async.Ignore
    |> Async.RunSynchronously

    0
```

<span data-ttu-id="d5676-151">Como puede ver, la `main` función tiene bastantes llamadas más.</span><span class="sxs-lookup"><span data-stu-id="d5676-151">As you can see, the `main` function has quite a few more calls made.</span></span> <span data-ttu-id="d5676-152">Conceptualmente, hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d5676-152">Conceptually, it does the following:</span></span>

1. <span data-ttu-id="d5676-153">Transforme los argumentos de la línea de comandos en `Async<unit>` cálculos con `Array.map` .</span><span class="sxs-lookup"><span data-stu-id="d5676-153">Transform the command-line arguments into `Async<unit>` computations with `Array.map`.</span></span>
2. <span data-ttu-id="d5676-154">Cree un `Async<'T[]>` que programe y ejecute los `printTotalFileBytes` cálculos en paralelo cuando se ejecute.</span><span class="sxs-lookup"><span data-stu-id="d5676-154">Create an `Async<'T[]>` that schedules and runs the `printTotalFileBytes` computations in parallel when it runs.</span></span>
3. <span data-ttu-id="d5676-155">Cree un `Async<unit>` que ejecutará el cálculo en paralelo y omitirá su resultado.</span><span class="sxs-lookup"><span data-stu-id="d5676-155">Create an `Async<unit>` that will run the parallel computation and ignore its result.</span></span>
4. <span data-ttu-id="d5676-156">Ejecute explícitamente el último cálculo con `Async.RunSynchronously` y bloquee hasta que se complete.</span><span class="sxs-lookup"><span data-stu-id="d5676-156">Explicitly run the last computation with `Async.RunSynchronously` and block until it completes.</span></span>

<span data-ttu-id="d5676-157">Cuando se ejecuta este programa, `printTotalFileBytes` se ejecuta en paralelo para cada argumento de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d5676-157">When this program runs, `printTotalFileBytes` runs in parallel for each command-line argument.</span></span> <span data-ttu-id="d5676-158">Dado que los cálculos asincrónicos se ejecutan de forma independiente del flujo de programa, no hay ningún orden en el que impriman su información y terminen de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="d5676-158">Because asynchronous computations execute independently of program flow, there is no order in which they print their information and finish executing.</span></span> <span data-ttu-id="d5676-159">Los cálculos se programarán en paralelo, pero no se garantiza su orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d5676-159">The computations will be scheduled in parallel, but their order of execution is not guaranteed.</span></span>

## <a name="sequence-asynchronous-computations"></a><span data-ttu-id="d5676-160">Cálculos asincrónicos de secuencia</span><span class="sxs-lookup"><span data-stu-id="d5676-160">Sequence asynchronous computations</span></span>

<span data-ttu-id="d5676-161">Dado `Async<'T>` que es una especificación de trabajo en lugar de una tarea que ya se está ejecutando, puede realizar fácilmente transformaciones más complejas.</span><span class="sxs-lookup"><span data-stu-id="d5676-161">Because `Async<'T>` is a specification of work rather than an already-running task, you can perform more intricate transformations easily.</span></span> <span data-ttu-id="d5676-162">Este es un ejemplo que secuencia un conjunto de cálculos asincrónicos para que se ejecuten uno tras otro.</span><span class="sxs-lookup"><span data-stu-id="d5676-162">Here is an example that sequences a set of Async computations so they execute one after another.</span></span>

```fsharp
let printTotalFileBytes path =
    async {
        let! bytes = File.ReadAllBytesAsync(path) |> Async.AwaitTask
        let fileName = Path.GetFileName(path)
        printfn $"File {fileName} has %d{bytes.Length} bytes"
    }

[<EntryPoint>]
let main argv =
    argv
    |> Array.map printTotalFileBytes
    |> Async.Sequential
    |> Async.Ignore
    |> Async.RunSynchronously
    |> ignore
```

<span data-ttu-id="d5676-163">Esto programará `printTotalFileBytes` para que se ejecute en el orden de los elementos de en `argv` lugar de programarlos en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-163">This will schedule `printTotalFileBytes` to execute in the order of the elements of `argv` rather than scheduling them in parallel.</span></span> <span data-ttu-id="d5676-164">Dado que el siguiente elemento no se programará hasta que haya finalizado la ejecución del último cálculo, los cálculos se secuenciarán de modo que no se superpongan en su ejecución.</span><span class="sxs-lookup"><span data-stu-id="d5676-164">Because the next item will not be scheduled until after the last computation has finished executing, the computations are sequenced such that there is no overlap in their execution.</span></span>

## <a name="important-async-module-functions"></a><span data-ttu-id="d5676-165">Funciones importantes del módulo Async</span><span class="sxs-lookup"><span data-stu-id="d5676-165">Important Async module functions</span></span>

<span data-ttu-id="d5676-166">Al escribir código asincrónico en F #, normalmente interactuará con un marco que controla la programación de los cálculos.</span><span class="sxs-lookup"><span data-stu-id="d5676-166">When you write async code in F#, you'll usually interact with a framework that handles scheduling of computations for you.</span></span> <span data-ttu-id="d5676-167">Sin embargo, este no es siempre el caso, por lo que es conveniente conocer las distintas funciones de inicio para programar el trabajo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-167">However, this is not always the case, so it is good to learn the various starting functions to schedule asynchronous work.</span></span>

<span data-ttu-id="d5676-168">Dado que los cálculos asincrónicos de F # son una _especificación_ del trabajo en lugar de una representación del trabajo que ya se está ejecutando, deben iniciarse explícitamente con una función de inicio.</span><span class="sxs-lookup"><span data-stu-id="d5676-168">Because F# asynchronous computations are a _specification_ of work rather than a representation of work that is already executing, they must be explicitly started with a starting function.</span></span> <span data-ttu-id="d5676-169">Hay muchos [métodos de inicio asincrónico](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#section0) que son útiles en contextos diferentes.</span><span class="sxs-lookup"><span data-stu-id="d5676-169">There are many [Async starting methods](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#section0) that are helpful in different contexts.</span></span> <span data-ttu-id="d5676-170">En la siguiente sección se describen algunas de las funciones de inicio más comunes.</span><span class="sxs-lookup"><span data-stu-id="d5676-170">The following section describes some of the more common starting functions.</span></span>

### <a name="asyncstartchild"></a><span data-ttu-id="d5676-171">Async. Startchild (</span><span class="sxs-lookup"><span data-stu-id="d5676-171">Async.StartChild</span></span>

<span data-ttu-id="d5676-172">Inicia un cálculo secundario dentro de un cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-172">Starts a child computation within an asynchronous computation.</span></span> <span data-ttu-id="d5676-173">Esto permite ejecutar simultáneamente varios cálculos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="d5676-173">This allows multiple asynchronous computations to be executed concurrently.</span></span> <span data-ttu-id="d5676-174">El cálculo secundario comparte un token de cancelación con el cálculo primario.</span><span class="sxs-lookup"><span data-stu-id="d5676-174">The child computation shares a cancellation token with the parent computation.</span></span> <span data-ttu-id="d5676-175">Si se cancela el cálculo primario, también se cancela el cálculo de los elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="d5676-175">If the parent computation is canceled, the child computation is also canceled.</span></span>

<span data-ttu-id="d5676-176">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-176">Signature:</span></span>

```fsharp
computation: Async<'T> * timeout: ?int -> Async<Async<'T>>
```

<span data-ttu-id="d5676-177">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-177">When to use:</span></span>

- <span data-ttu-id="d5676-178">Cuando se desea ejecutar varios cálculos asincrónicos simultáneamente en lugar de uno en uno, pero no se programan en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-178">When you want to execute multiple asynchronous computations concurrently rather than one at a time, but not have them scheduled in parallel.</span></span>
- <span data-ttu-id="d5676-179">Cuando desea asociar la duración de un cálculo secundario al de un cálculo primario.</span><span class="sxs-lookup"><span data-stu-id="d5676-179">When you wish to tie the lifetime of a child computation to that of a parent computation.</span></span>

<span data-ttu-id="d5676-180">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-180">What to watch out for:</span></span>

- <span data-ttu-id="d5676-181">Iniciar varios cálculos con `Async.StartChild` no es lo mismo que programarlos en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-181">Starting multiple computations with `Async.StartChild` isn't the same as scheduling them in parallel.</span></span> <span data-ttu-id="d5676-182">Si desea programar cálculos en paralelo, use `Async.Parallel` .</span><span class="sxs-lookup"><span data-stu-id="d5676-182">If you wish to schedule computations in parallel, use `Async.Parallel`.</span></span>
- <span data-ttu-id="d5676-183">Si se cancela un cálculo primario, se desencadenará la cancelación de todos los cálculos secundarios que se iniciaron.</span><span class="sxs-lookup"><span data-stu-id="d5676-183">Canceling a parent computation will trigger cancellation of all child computations it started.</span></span>

### <a name="asyncstartimmediate"></a><span data-ttu-id="d5676-184">Async. StartImmediate (</span><span class="sxs-lookup"><span data-stu-id="d5676-184">Async.StartImmediate</span></span>

<span data-ttu-id="d5676-185">Ejecuta un cálculo asincrónico y comienza inmediatamente en el subproceso actual del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="d5676-185">Runs an asynchronous computation, starting immediately on the current operating system thread.</span></span> <span data-ttu-id="d5676-186">Esto resulta útil si necesita actualizar algo en el subproceso de llamada durante el cálculo.</span><span class="sxs-lookup"><span data-stu-id="d5676-186">This is helpful if you need to update something on the calling thread during the computation.</span></span> <span data-ttu-id="d5676-187">Por ejemplo, si un cálculo asincrónico debe actualizar una interfaz de usuario (por ejemplo, actualizar una barra de progreso), `Async.StartImmediate` debe usarse.</span><span class="sxs-lookup"><span data-stu-id="d5676-187">For example, if an asynchronous computation must update a UI (such as updating a progress bar), then `Async.StartImmediate` should be used.</span></span>

<span data-ttu-id="d5676-188">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-188">Signature:</span></span>

```fsharp
computation: Async<unit> * cancellationToken: ?CancellationToken -> unit
```

<span data-ttu-id="d5676-189">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-189">When to use:</span></span>

- <span data-ttu-id="d5676-190">Cuando es necesario actualizar algo en el subproceso que realiza la llamada en medio de un cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-190">When you need to update something on the calling thread in the middle of an asynchronous computation.</span></span>

<span data-ttu-id="d5676-191">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-191">What to watch out for:</span></span>

- <span data-ttu-id="d5676-192">El código del cálculo asincrónico se ejecutará en cualquier subproceso en el que se programe.</span><span class="sxs-lookup"><span data-stu-id="d5676-192">Code in the asynchronous computation will run on whatever thread one happens to be scheduled on.</span></span> <span data-ttu-id="d5676-193">Esto puede ser problemático si ese subproceso es sensiblemente confidencial, como un subproceso de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="d5676-193">This can be problematic if that thread is in some way sensitive, such as a UI thread.</span></span> <span data-ttu-id="d5676-194">En tales casos, `Async.StartImmediate` es probable que no sea apropiado usar.</span><span class="sxs-lookup"><span data-stu-id="d5676-194">In such cases, `Async.StartImmediate` is likely inappropriate to use.</span></span>

### <a name="asyncstartastask"></a><span data-ttu-id="d5676-195">Async. Startastask (</span><span class="sxs-lookup"><span data-stu-id="d5676-195">Async.StartAsTask</span></span>

<span data-ttu-id="d5676-196">Ejecuta un cálculo en el grupo de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="d5676-196">Executes a computation in the thread pool.</span></span> <span data-ttu-id="d5676-197">Devuelve un <xref:System.Threading.Tasks.Task%601> que se completará en el estado correspondiente una vez finalizado el cálculo (genera el resultado, produce la excepción o se cancela).</span><span class="sxs-lookup"><span data-stu-id="d5676-197">Returns a <xref:System.Threading.Tasks.Task%601> that will be completed on the corresponding state once the computation terminates (produces the result, throws exception, or gets canceled).</span></span> <span data-ttu-id="d5676-198">Si no se proporciona ningún token de cancelación, se usará el token de cancelación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d5676-198">If no cancellation token is provided, then the default cancellation token is used.</span></span>

<span data-ttu-id="d5676-199">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-199">Signature:</span></span>

```fsharp
computation: Async<'T> * taskCreationOptions: ?TaskCreationOptions * cancellationToken: ?CancellationToken -> Task<'T>
```

<span data-ttu-id="d5676-200">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-200">When to use:</span></span>

- <span data-ttu-id="d5676-201">Cuando necesita llamar a una API de .NET que espera que <xref:System.Threading.Tasks.Task%601> represente el resultado de un cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-201">When you need to call into a .NET API that expects a <xref:System.Threading.Tasks.Task%601> to represent the result of an asynchronous computation.</span></span>

<span data-ttu-id="d5676-202">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-202">What to watch out for:</span></span>

- <span data-ttu-id="d5676-203">Esta llamada asignará un `Task` objeto adicional, lo que puede aumentar la sobrecarga si se usa con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="d5676-203">This call will allocate an additional `Task` object, which can increase overhead if it is used often.</span></span>

### <a name="asyncparallel"></a><span data-ttu-id="d5676-204">Async. Parallel</span><span class="sxs-lookup"><span data-stu-id="d5676-204">Async.Parallel</span></span>

<span data-ttu-id="d5676-205">Programa una secuencia de cálculos asincrónicos que se van a ejecutar en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-205">Schedules a sequence of asynchronous computations to be executed in parallel.</span></span> <span data-ttu-id="d5676-206">El grado de paralelismo se puede optimizar o limitar opcionalmente especificando el `maxDegreesOfParallelism` parámetro.</span><span class="sxs-lookup"><span data-stu-id="d5676-206">The degree of parallelism can be optionally tuned/throttled by specifying the `maxDegreesOfParallelism` parameter.</span></span>

<span data-ttu-id="d5676-207">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-207">Signature:</span></span>

```fsharp
computations: seq<Async<'T>> * ?maxDegreesOfParallelism: int -> Async<'T[]>
```

<span data-ttu-id="d5676-208">Cuándo usarlo</span><span class="sxs-lookup"><span data-stu-id="d5676-208">When to use it:</span></span>

- <span data-ttu-id="d5676-209">Si necesita ejecutar un conjunto de cálculos al mismo tiempo y no depende de su orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d5676-209">If you need to run a set of computations at the same time and have no reliance on their order of execution.</span></span>
- <span data-ttu-id="d5676-210">Si no necesita resultados de los cálculos programados en paralelo hasta que todos se hayan completado.</span><span class="sxs-lookup"><span data-stu-id="d5676-210">If you don't require results from computations scheduled in parallel until they have all completed.</span></span>

<span data-ttu-id="d5676-211">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-211">What to watch out for:</span></span>

- <span data-ttu-id="d5676-212">Solo se puede tener acceso a la matriz de valores resultante una vez finalizados todos los cálculos.</span><span class="sxs-lookup"><span data-stu-id="d5676-212">You can only access the resulting array of values once all computations have finished.</span></span>
- <span data-ttu-id="d5676-213">Los cálculos se ejecutarán cuando acaben de programarse.</span><span class="sxs-lookup"><span data-stu-id="d5676-213">Computations will be run whenever they end up getting scheduled.</span></span> <span data-ttu-id="d5676-214">Este comportamiento significa que no se puede confiar en su orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d5676-214">This behavior means you cannot rely on their order of their execution.</span></span>

### <a name="asyncsequential"></a><span data-ttu-id="d5676-215">Async. Sequential</span><span class="sxs-lookup"><span data-stu-id="d5676-215">Async.Sequential</span></span>

<span data-ttu-id="d5676-216">Programa una secuencia de cálculos asincrónicos que se van a ejecutar en el orden en que se pasan.</span><span class="sxs-lookup"><span data-stu-id="d5676-216">Schedules a sequence of asynchronous computations to be executed in the order that they are passed.</span></span> <span data-ttu-id="d5676-217">Se ejecutará el primer cálculo, después el siguiente, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="d5676-217">The first computation will be executed, then the next, and so on.</span></span> <span data-ttu-id="d5676-218">No se ejecutarán cálculos en paralelo.</span><span class="sxs-lookup"><span data-stu-id="d5676-218">No computations will be executed in parallel.</span></span>

<span data-ttu-id="d5676-219">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-219">Signature:</span></span>

```fsharp
computations: seq<Async<'T>> -> Async<'T[]>
```

<span data-ttu-id="d5676-220">Cuándo usarlo</span><span class="sxs-lookup"><span data-stu-id="d5676-220">When to use it:</span></span>

- <span data-ttu-id="d5676-221">Si tiene que ejecutar varios cálculos en orden.</span><span class="sxs-lookup"><span data-stu-id="d5676-221">If you need to execute multiple computations in order.</span></span>

<span data-ttu-id="d5676-222">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-222">What to watch out for:</span></span>

- <span data-ttu-id="d5676-223">Solo se puede tener acceso a la matriz de valores resultante una vez finalizados todos los cálculos.</span><span class="sxs-lookup"><span data-stu-id="d5676-223">You can only access the resulting array of values once all computations have finished.</span></span>
- <span data-ttu-id="d5676-224">Los cálculos se ejecutarán en el orden en que se pasan a esta función, lo que puede significar que habrá más tiempo antes de que se devuelvan los resultados.</span><span class="sxs-lookup"><span data-stu-id="d5676-224">Computations will be run in the order that they are passed to this function, which can mean that more time will elapse before the results are returned.</span></span>

### <a name="asyncawaittask"></a><span data-ttu-id="d5676-225">Async. Awaittask (</span><span class="sxs-lookup"><span data-stu-id="d5676-225">Async.AwaitTask</span></span>

<span data-ttu-id="d5676-226">Devuelve un cálculo asincrónico que espera <xref:System.Threading.Tasks.Task%601> a que se complete el determinado y devuelve su resultado como un `Async<'T>`</span><span class="sxs-lookup"><span data-stu-id="d5676-226">Returns an asynchronous computation that waits for the given <xref:System.Threading.Tasks.Task%601> to complete and returns its result as an `Async<'T>`</span></span>

<span data-ttu-id="d5676-227">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-227">Signature:</span></span>

```fsharp
task: Task<'T> -> Async<'T>
```

<span data-ttu-id="d5676-228">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-228">When to use:</span></span>

- <span data-ttu-id="d5676-229">Cuando se utiliza una API de .NET que devuelve un <xref:System.Threading.Tasks.Task%601> en un cálculo asincrónico de F #.</span><span class="sxs-lookup"><span data-stu-id="d5676-229">When you are consuming a .NET API that returns a <xref:System.Threading.Tasks.Task%601> within an F# asynchronous computation.</span></span>

<span data-ttu-id="d5676-230">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-230">What to watch out for:</span></span>

- <span data-ttu-id="d5676-231">Las excepciones se incluyen en <xref:System.AggregateException> la siguiente Convención de la biblioteca de Parallel Task y este comportamiento es diferente de la forma en que F # Async normalmente muestra excepciones.</span><span class="sxs-lookup"><span data-stu-id="d5676-231">Exceptions are wrapped in <xref:System.AggregateException> following the convention of the Task Parallel Library, and this behavior is different from how F# async generally surfaces exceptions.</span></span>

### <a name="asynccatch"></a><span data-ttu-id="d5676-232">Async. Catch</span><span class="sxs-lookup"><span data-stu-id="d5676-232">Async.Catch</span></span>

<span data-ttu-id="d5676-233">Crea un cálculo asincrónico que ejecuta un determinado `Async<'T>` y devuelve un `Async<Choice<'T, exn>>` .</span><span class="sxs-lookup"><span data-stu-id="d5676-233">Creates an asynchronous computation that executes a given `Async<'T>`, returning an `Async<Choice<'T, exn>>`.</span></span> <span data-ttu-id="d5676-234">Si el especificado `Async<'T>` se completa correctamente, `Choice1Of2` se devuelve un con el valor resultante.</span><span class="sxs-lookup"><span data-stu-id="d5676-234">If the given `Async<'T>` completes successfully, then a `Choice1Of2` is returned with the resultant value.</span></span> <span data-ttu-id="d5676-235">Si se produce una excepción antes de que se complete, `Choice2of2` se devuelve un con la excepción generada.</span><span class="sxs-lookup"><span data-stu-id="d5676-235">If an exception is thrown before it completes, then a `Choice2of2` is returned with the raised exception.</span></span> <span data-ttu-id="d5676-236">Si se usa en un cálculo asincrónico que se compone de muchos cálculos y uno de esos cálculos produce una excepción, el cálculo de la englobación se detendrá por completo en su totalidad.</span><span class="sxs-lookup"><span data-stu-id="d5676-236">If it is used on an asynchronous computation that is itself composed of many computations, and one of those computations throws an exception, the encompassing computation will be stopped entirely.</span></span>

<span data-ttu-id="d5676-237">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-237">Signature:</span></span>

```fsharp
computation: Async<'T> -> Async<Choice<'T, exn>>
```

<span data-ttu-id="d5676-238">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-238">When to use:</span></span>

- <span data-ttu-id="d5676-239">Cuando se realiza el trabajo asincrónico que puede producir un error con una excepción y se desea controlar esa excepción en el llamador.</span><span class="sxs-lookup"><span data-stu-id="d5676-239">When you are performing asynchronous work that may fail with an exception and you want to handle that exception in the caller.</span></span>

<span data-ttu-id="d5676-240">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-240">What to watch out for:</span></span>

- <span data-ttu-id="d5676-241">Cuando se usan cálculos asincrónicos combinados o secuenciados, el cálculo de la englobación se detendrá por completo si uno de sus cálculos "internos" produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="d5676-241">When using combined or sequenced asynchronous computations, the encompassing computation will fully stop if one of its "internal" computations throws an exception.</span></span>

### <a name="asyncignore"></a><span data-ttu-id="d5676-242">Async. ignore</span><span class="sxs-lookup"><span data-stu-id="d5676-242">Async.Ignore</span></span>

<span data-ttu-id="d5676-243">Crea un cálculo asincrónico que ejecuta el cálculo especificado y omite su resultado.</span><span class="sxs-lookup"><span data-stu-id="d5676-243">Creates an asynchronous computation that runs the given computation and ignores its result.</span></span>

<span data-ttu-id="d5676-244">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-244">Signature:</span></span>

```fsharp
computation: Async<'T> -> Async<unit>
```

<span data-ttu-id="d5676-245">Cuándo usarlo:</span><span class="sxs-lookup"><span data-stu-id="d5676-245">When to use:</span></span>

- <span data-ttu-id="d5676-246">Cuando tiene un cálculo asincrónico cuyo resultado no es necesario.</span><span class="sxs-lookup"><span data-stu-id="d5676-246">When you have an asynchronous computation whose result is not needed.</span></span> <span data-ttu-id="d5676-247">Esto es análogo al `ignore` código para el código no asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-247">This is analogous to the `ignore` code for non-asynchronous code.</span></span>

<span data-ttu-id="d5676-248">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-248">What to watch out for:</span></span>

- <span data-ttu-id="d5676-249">Si debe usar `Async.Ignore` porque desea utilizar `Async.Start` u otra función que requiera `Async<unit>` , considere la posibilidad de descartar el resultado.</span><span class="sxs-lookup"><span data-stu-id="d5676-249">If you must use `Async.Ignore` because you wish to use `Async.Start` or another function that requires `Async<unit>`, consider if discarding the result is okay.</span></span> <span data-ttu-id="d5676-250">Evite descartar los resultados solo para ajustarse a una firma de tipo.</span><span class="sxs-lookup"><span data-stu-id="d5676-250">Avoid discarding results just to fit a type signature.</span></span>

### <a name="asyncrunsynchronously"></a><span data-ttu-id="d5676-251">Async. RunSynchronously</span><span class="sxs-lookup"><span data-stu-id="d5676-251">Async.RunSynchronously</span></span>

<span data-ttu-id="d5676-252">Ejecuta un cálculo asincrónico y espera su resultado en el subproceso que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="d5676-252">Runs an asynchronous computation and awaits its result on the calling thread.</span></span> <span data-ttu-id="d5676-253">Esta llamada está bloqueando.</span><span class="sxs-lookup"><span data-stu-id="d5676-253">This call is blocking.</span></span>

<span data-ttu-id="d5676-254">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-254">Signature:</span></span>

```fsharp
computation: Async<'T> * timeout: ?int * cancellationToken: ?CancellationToken -> 'T
```

<span data-ttu-id="d5676-255">Cuándo usarlo</span><span class="sxs-lookup"><span data-stu-id="d5676-255">When to use it:</span></span>

- <span data-ttu-id="d5676-256">Si lo necesita, úselo solo una vez en una aplicación: en el punto de entrada de un archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="d5676-256">If you need it, use it only once in an application - at the entry point for an executable.</span></span>
- <span data-ttu-id="d5676-257">Cuando no le interesa el rendimiento y desea ejecutar un conjunto de otras operaciones asincrónicas a la vez.</span><span class="sxs-lookup"><span data-stu-id="d5676-257">When you don't care about performance and want to execute a set of other asynchronous operations at once.</span></span>

<span data-ttu-id="d5676-258">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-258">What to watch out for:</span></span>

- <span data-ttu-id="d5676-259">La llamada a `Async.RunSynchronously` bloquea el subproceso que realiza la llamada hasta que se completa la ejecución.</span><span class="sxs-lookup"><span data-stu-id="d5676-259">Calling `Async.RunSynchronously` blocks the calling thread until the execution completes.</span></span>

### <a name="asyncstart"></a><span data-ttu-id="d5676-260">Async. Start</span><span class="sxs-lookup"><span data-stu-id="d5676-260">Async.Start</span></span>

<span data-ttu-id="d5676-261">Inicia un cálculo asincrónico en el grupo de subprocesos que devuelve `unit` .</span><span class="sxs-lookup"><span data-stu-id="d5676-261">Starts an asynchronous computation in the thread pool that returns `unit`.</span></span> <span data-ttu-id="d5676-262">No espera el resultado.</span><span class="sxs-lookup"><span data-stu-id="d5676-262">Doesn't wait for its result.</span></span> <span data-ttu-id="d5676-263">Los cálculos anidados iniciados con `Async.Start` se inician independientemente del cálculo primario que los llamó.</span><span class="sxs-lookup"><span data-stu-id="d5676-263">Nested computations started with `Async.Start` are started independently of the parent computation that called them.</span></span> <span data-ttu-id="d5676-264">Su duración no está asociada a ningún cálculo primario.</span><span class="sxs-lookup"><span data-stu-id="d5676-264">Their lifetime is not tied to any parent computation.</span></span> <span data-ttu-id="d5676-265">Si se cancela el cálculo primario, no se cancelan los cálculos secundarios.</span><span class="sxs-lookup"><span data-stu-id="d5676-265">If the parent computation is canceled, no child computations are canceled.</span></span>

<span data-ttu-id="d5676-266">Signature:</span><span class="sxs-lookup"><span data-stu-id="d5676-266">Signature:</span></span>

```fsharp
computation: Async<unit> * cancellationToken: ?CancellationToken -> unit
```

<span data-ttu-id="d5676-267">Use solo cuando:</span><span class="sxs-lookup"><span data-stu-id="d5676-267">Use only when:</span></span>

- <span data-ttu-id="d5676-268">Tiene un cálculo asincrónico que no produce un resultado ni requiere procesamiento de uno.</span><span class="sxs-lookup"><span data-stu-id="d5676-268">You have an asynchronous computation that doesn't yield a result and/or require processing of one.</span></span>
- <span data-ttu-id="d5676-269">No es necesario saber cuándo se completa un cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-269">You don't need to know when an asynchronous computation completes.</span></span>
- <span data-ttu-id="d5676-270">No le importa en qué subproceso se ejecuta un cálculo asincrónico.</span><span class="sxs-lookup"><span data-stu-id="d5676-270">You don't care which thread an asynchronous computation runs on.</span></span>
- <span data-ttu-id="d5676-271">No es necesario tener en cuenta ni notificar las excepciones resultantes de la tarea.</span><span class="sxs-lookup"><span data-stu-id="d5676-271">You don't have any need to be aware of or report exceptions resulting from the task.</span></span>

<span data-ttu-id="d5676-272">Qué debe ver:</span><span class="sxs-lookup"><span data-stu-id="d5676-272">What to watch out for:</span></span>

- <span data-ttu-id="d5676-273">Las excepciones producidas por los cálculos iniciados con `Async.Start` no se propagan al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="d5676-273">Exceptions raised by computations started with `Async.Start` aren't propagated to the caller.</span></span> <span data-ttu-id="d5676-274">La pila de llamadas se desenredará por completo.</span><span class="sxs-lookup"><span data-stu-id="d5676-274">The call stack will be completely unwound.</span></span>
- <span data-ttu-id="d5676-275">Cualquier trabajo (como llamar a `printfn` ) iniciado con `Async.Start` no hará que el efecto se produzca en el subproceso principal de la ejecución de un programa.</span><span class="sxs-lookup"><span data-stu-id="d5676-275">Any work (such as calling `printfn`) started with `Async.Start` won't cause the effect to happen on the main thread of a program's execution.</span></span>

## <a name="interoperate-with-net"></a><span data-ttu-id="d5676-276">Interoperar con .NET</span><span class="sxs-lookup"><span data-stu-id="d5676-276">Interoperate with .NET</span></span>

<span data-ttu-id="d5676-277">Puede estar trabajando con una biblioteca de .NET o código base de C# que utiliza la programación asincrónica de estilo [Async/Await](../../../standard/async.md).</span><span class="sxs-lookup"><span data-stu-id="d5676-277">You may be working with a .NET library or C# codebase that uses [async/await](../../../standard/async.md)-style asynchronous programming.</span></span> <span data-ttu-id="d5676-278">Dado que C# y la mayoría de las bibliotecas de .NET usan los <xref:System.Threading.Tasks.Task%601> <xref:System.Threading.Tasks.Task> tipos y como abstracciones principales en lugar de `Async<'T>` , debe cruzar un límite entre estos dos enfoques a asincronía.</span><span class="sxs-lookup"><span data-stu-id="d5676-278">Because C# and the majority of .NET libraries use the <xref:System.Threading.Tasks.Task%601> and <xref:System.Threading.Tasks.Task> types as their core abstractions rather than `Async<'T>`, you must cross a boundary between these two approaches to asynchrony.</span></span>

### <a name="how-to-work-with-net-async-and-taskt"></a><span data-ttu-id="d5676-279">Cómo trabajar con .NET Async y `Task<T>`</span><span class="sxs-lookup"><span data-stu-id="d5676-279">How to work with .NET async and `Task<T>`</span></span>

<span data-ttu-id="d5676-280">Trabajar con las bibliotecas asincrónicas .NET y códigos base que usan <xref:System.Threading.Tasks.Task%601> (es decir, los cálculos asincrónicos que tienen valores devueltos) es sencillo y tiene compatibilidad integrada con F #.</span><span class="sxs-lookup"><span data-stu-id="d5676-280">Working with .NET async libraries and codebases that use <xref:System.Threading.Tasks.Task%601> (that is, async computations that have return values) is straightforward and has built-in support with F#.</span></span>

<span data-ttu-id="d5676-281">Puede usar la `Async.AwaitTask` función para esperar un cálculo asincrónico de .net:</span><span class="sxs-lookup"><span data-stu-id="d5676-281">You can use the `Async.AwaitTask` function to await a .NET asynchronous computation:</span></span>

```fsharp
let getValueFromLibrary param =
    async {
        let! value = DotNetLibrary.GetValueAsync param |> Async.AwaitTask
        return value
    }
```

<span data-ttu-id="d5676-282">Puede usar la `Async.StartAsTask` función para pasar un cálculo asincrónico a un llamador de .net:</span><span class="sxs-lookup"><span data-stu-id="d5676-282">You can use the `Async.StartAsTask` function to pass an asynchronous computation to a .NET caller:</span></span>

```fsharp
let computationForCaller param =
    async {
        let! result = getAsyncResult param
        return result
    } |> Async.StartAsTask
```

### <a name="how-to-work-with-net-async-and-task"></a><span data-ttu-id="d5676-283">Cómo trabajar con .NET Async y `Task`</span><span class="sxs-lookup"><span data-stu-id="d5676-283">How to work with .NET async and `Task`</span></span>

<span data-ttu-id="d5676-284">Para trabajar con las API que usan <xref:System.Threading.Tasks.Task> (es decir, los cálculos asincrónicos de .net que no devuelven un valor), puede que necesite agregar una función adicional que convierta un `Async<'T>` en un <xref:System.Threading.Tasks.Task> :</span><span class="sxs-lookup"><span data-stu-id="d5676-284">To work with APIs that use <xref:System.Threading.Tasks.Task> (that is, .NET async computations that do not return a value), you may need to add an additional function that will convert an `Async<'T>` to a <xref:System.Threading.Tasks.Task>:</span></span>

```fsharp
module Async =
    // Async<unit> -> Task
    let startTaskFromAsyncUnit (comp: Async<unit>) =
        Async.StartAsTask comp :> Task
```

<span data-ttu-id="d5676-285">Ya existe una `Async.AwaitTask` que acepta <xref:System.Threading.Tasks.Task> como entrada.</span><span class="sxs-lookup"><span data-stu-id="d5676-285">There is already an `Async.AwaitTask` that accepts a <xref:System.Threading.Tasks.Task> as input.</span></span> <span data-ttu-id="d5676-286">Con esta y la función definida anteriormente `startTaskFromAsyncUnit` , puede iniciar y esperar <xref:System.Threading.Tasks.Task> tipos de un cálculo asincrónico de F #.</span><span class="sxs-lookup"><span data-stu-id="d5676-286">With this and the previously defined `startTaskFromAsyncUnit` function, you can start and await <xref:System.Threading.Tasks.Task> types from an F# async computation.</span></span>

## <a name="relationship-to-multi-threading"></a><span data-ttu-id="d5676-287">Relación con Multi-Threading</span><span class="sxs-lookup"><span data-stu-id="d5676-287">Relationship to multi-threading</span></span>

<span data-ttu-id="d5676-288">Aunque los subprocesos se mencionan en este artículo, hay dos aspectos importantes que hay que recordar:</span><span class="sxs-lookup"><span data-stu-id="d5676-288">Although threading is mentioned throughout this article, there are two important things to remember:</span></span>

1. <span data-ttu-id="d5676-289">No hay ninguna afinidad entre un cálculo asincrónico y un subproceso, a menos que se inicie explícitamente en el subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="d5676-289">There is no affinity between an asynchronous computation and a thread, unless explicitly started on the current thread.</span></span>
1. <span data-ttu-id="d5676-290">La programación asincrónica en F # no es una abstracción para multithreading.</span><span class="sxs-lookup"><span data-stu-id="d5676-290">Asynchronous programming in F# is not an abstraction for multi-threading.</span></span>

<span data-ttu-id="d5676-291">Por ejemplo, un cálculo puede ejecutarse realmente en el subproceso del llamador, dependiendo de la naturaleza del trabajo.</span><span class="sxs-lookup"><span data-stu-id="d5676-291">For example, a computation may actually run on its caller's thread, depending on the nature of the work.</span></span> <span data-ttu-id="d5676-292">Un cálculo también podría "saltar" entre subprocesos, por lo que se le prestará una pequeña cantidad de tiempo para realizar un trabajo útil entre los períodos de "en espera" (por ejemplo, cuando una llamada de red está en tránsito).</span><span class="sxs-lookup"><span data-stu-id="d5676-292">A computation could also "jump" between threads, borrowing them for a small amount of time to do useful work in between periods of "waiting" (such as when a network call is in transit).</span></span>

<span data-ttu-id="d5676-293">Aunque F # proporciona algunas funciones para iniciar un cálculo asincrónico en el subproceso actual (o explícitamente no en el subproceso actual), asincronía generalmente no está asociado a una estrategia de subprocesos determinada.</span><span class="sxs-lookup"><span data-stu-id="d5676-293">Although F# provides some abilities to start an asynchronous computation on the current thread (or explicitly not on the current thread), asynchrony generally is not associated with a particular threading strategy.</span></span>

## <a name="see-also"></a><span data-ttu-id="d5676-294">Consulte también</span><span class="sxs-lookup"><span data-stu-id="d5676-294">See also</span></span>

- [<span data-ttu-id="d5676-295">Modelo de programación asincrónica de F #</span><span class="sxs-lookup"><span data-stu-id="d5676-295">The F# Asynchronous Programming Model</span></span>](https://www.microsoft.com/research/publication/the-f-asynchronous-programming-model)
- [<span data-ttu-id="d5676-296">Guía de F # Async de jet. com</span><span class="sxs-lookup"><span data-stu-id="d5676-296">Jet.com's F# Async Guide</span></span>](https://medium.com/jettech/f-async-guide-eb3c8a2d180a)
- [<span data-ttu-id="d5676-297">Guía de programación asincrónica de F # para diversión y beneficios</span><span class="sxs-lookup"><span data-stu-id="d5676-297">F# for fun and profit's Asynchronous Programming guide</span></span>](https://fsharpforfunandprofit.com/posts/concurrency-async-and-parallel/)
- [<span data-ttu-id="d5676-298">Async en C# y F #: problemas asincrónicos en C #</span><span class="sxs-lookup"><span data-stu-id="d5676-298">Async in C# and F#: Asynchronous gotchas in C#</span></span>](http://tomasp.net/blog/csharp-async-gotchas.aspx/)
