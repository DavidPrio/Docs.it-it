---
title: Formattazione di dati di risposta in ASP.NET MVC di base
author: ardalis
description: Informazioni su come formattare i dati di risposta in ASP.NET MVC di base.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddda494e0db22031af9d20325e14e8458756cbfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="0b5fb-103">Introduzione alla formattazione dei dati di risposta in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="0b5fb-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="0b5fb-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0b5fb-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0b5fb-105">ASP.NET MVC di base include il supporto incorporato per la formattazione di dati di risposta, utilizzando formati a lunghezza fissa o in risposta a specifiche del client.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="0b5fb-106">[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="0b5fb-107">Risultati dell'azione specifica di formato</span><span class="sxs-lookup"><span data-stu-id="0b5fb-107">Format-Specific Action Results</span></span>

<span data-ttu-id="0b5fb-108">Alcuni tipi di risultati di azione sono specifici di un particolare formato, ad esempio `JsonResult` e `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="0b5fb-109">Azioni possono restituire risultati specifici che vengono sempre formattati in modo particolare.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="0b5fb-110">Ad esempio, restituendo un `JsonResult` restituisce i dati in formato JSON, indipendentemente dalle preferenze del client.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="0b5fb-111">Analogamente, restituendo un `ContentResult` restituirà dati stringa in formato testo normale (come verrà semplicemente che restituisce una stringa).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="0b5fb-112">Un'azione non è necessario restituire qualsiasi tipo particolare. MVC supporta qualsiasi valore restituito dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="0b5fb-113">Se un'operazione restituisce un `IActionResult` implementazione e il controller eredita da `Controller`, gli sviluppatori dispongono di molti metodi di supporto corrispondente a molte delle opzioni.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="0b5fb-114">Risultati di azioni che restituiscono oggetti che non sono `IActionResult` tipi verranno serializzati utilizzando il metodo appropriato `IOutputFormatter` implementazione.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="0b5fb-115">Per restituire i dati in un formato specifico da un controller da cui eredita il `Controller` classe di base, utilizzare il metodo helper predefinite `Json` per restituire JSON e `Content` per testo normale.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="0b5fb-116">Il metodo di azione deve restituire il tipo di risultato specifico (ad esempio, `JsonResult`) o `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="0b5fb-117">Restituzione di dati in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="0b5fb-118">Risposta di esempio da questa azione:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-118">Sample response from this action:</span></span>

![Scheda di rete di strumenti di sviluppo in Microsoft Edge che mostra il tipo di contenuto della risposta è application/json](formatting/_static/json-response.png)

<span data-ttu-id="0b5fb-120">Si noti che il tipo di contenuto della risposta è `application/json`, come illustrato nell'elenco di richieste di rete e nella sezione intestazioni di risposta.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="0b5fb-121">Si noti inoltre l'elenco di opzioni visualizzate dal browser nell'intestazione Accept nella sezione intestazioni di richiesta (in questo caso, Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="0b5fb-122">La tecnica corrente Ignora questa intestazione. Seguire il è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="0b5fb-123">Per restituire dati in formato testo normale, utilizzare `ContentResult` e `Content` helper:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="0b5fb-124">Una risposta da questa azione:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-124">A response from this action:</span></span>

![Scheda di rete di strumenti di sviluppo in Microsoft Edge che mostra il tipo di contenuto della risposta è text/plain.](formatting/_static/text-response.png)

<span data-ttu-id="0b5fb-126">Si noti in questo caso il `Content-Type` restituita è `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="0b5fb-127">È anche possibile ottenere lo stesso comportamento utilizzando solo un tipo di risposta di stringa:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="0b5fb-128">Per le azioni non semplice con più restituire tipi o le opzioni (ad esempio, in base al risultato di operazioni eseguite diversi codici di stato HTTP), preferisce `IActionResult` come tipo restituito.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="0b5fb-129">Negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="0b5fb-129">Content Negotiation</span></span>

<span data-ttu-id="0b5fb-130">Negoziazione di contenuto (*conneg* breve) si verifica quando il client specifica un [intestazione Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="0b5fb-131">Il formato predefinito utilizzato da ASP.NET MVC di base è JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="0b5fb-132">Negoziazione del contenuto viene implementata da `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="0b5fb-133">Viene generato anche nel codice di stato risultati specifici dell'azione restituiti da metodi di supporto (che sono tutti basati sul `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="0b5fb-134">È inoltre possibile restituire un tipo di modello (una classe sono stati definiti come tipo di trasferimento di dati) e il framework esegue automaticamente il wrapping in un `ObjectResult` automaticamente.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="0b5fb-135">Usa il metodo di azione seguenti la `Ok` e `NotFound` metodi helper:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="0b5fb-136">Verrà restituita una risposta in formato JSON a meno che non è stato richiesto un altro formato e il server può restituire il formato richiesto.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="0b5fb-137">È possibile utilizzare uno strumento come [Fiddler](http://www.telerik.com/fiddler) per creare una richiesta che include un'intestazione Accept e specificare un altro formato.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="0b5fb-138">In questo caso, se il server ha un *formattatore* che può generare una risposta nel formato richiesto, il risultato verrà restituito nel formato preferito di client.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Console di Fiddler che mostra un oggetto creato manualmente ottenere una richiesta con un valore di intestazione Accept di application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="0b5fb-140">Nella schermata precedente, creazione di Fiddler è stata utilizzata per generare una richiesta, specificare `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="0b5fb-141">Per impostazione predefinita, ASP.NET MVC Core supporta solo JSON, pertanto anche quando viene specificato un altro formato, il risultato restituito è ancora in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="0b5fb-142">Si noterà come aggiungere i formattatori aggiuntivi nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="0b5fb-143">Le azioni del controller possono restituire POCOs (Plain Old CLR Object), nel qual caso ASP.NET MVC creerà automaticamente un `ObjectResult` per l'utente che esegue il wrapping dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="0b5fb-144">Il client verrà visualizzato l'oggetto serializzato formattato (il valore predefinito è il formato JSON, XML o in altri formati, è possibile configurare).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="0b5fb-145">Se l'oggetto restituito è `null`, il framework restituirà un `204 No Content` risposta.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="0b5fb-146">Restituzione di un tipo di oggetto:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="0b5fb-147">Nell'esempio, una richiesta per un alias valido autore riceverà una risposta 200 OK con i dati dell'autore.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="0b5fb-148">Una richiesta di un alias valido riceverà una risposta 204 Nessun contenuto.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="0b5fb-149">Screenshot che mostra la risposta in formato XML e JSON è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="0b5fb-150">Processo di negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="0b5fb-150">Content Negotiation Process</span></span>

<span data-ttu-id="0b5fb-151">Contenuto *negoziazione* avviene solo se un `Accept` intestazione è presente nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="0b5fb-152">Quando una richiesta contiene un'intestazione accept, il framework permette di enumerare i tipi di supporto nell'intestazione accept in ordine di preferenza e tenterà di trovare un formattatore in grado di produrre una risposta in uno dei formati specificati dall'intestazione accept.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="0b5fb-153">Nel caso in cui non viene trovato alcun formattatore che può soddisfare la richiesta del client, il framework tenterà di trovare il primo formattatore in grado di produrre una risposta (a meno che lo sviluppatore ha configurato l'opzione su `MvcOptions` per restituire 406 inaccettabile invece).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="0b5fb-154">Se la richiesta specifica di XML, ma il formattatore XML non è stato configurato, verrà utilizzato il formattatore JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="0b5fb-155">Più in generale, se non è configurato alcun formattatore che può fornire il formato richiesto, quindi viene utilizzato il primo formattatore in grado di formattare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="0b5fb-156">Se non viene specificata alcuna intestazione, il primo formattatore in grado di gestire l'oggetto deve essere restituito da utilizzare per serializzare la risposta.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="0b5fb-157">In questo caso, non è eseguita qualsiasi negoziazione - consiste nel determinare quale formato utilizzerà il server.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="0b5fb-158">Se l'intestazione Accept contiene `*/*`, l'intestazione verrà ignorata a meno che non `RespectBrowserAcceptHeader` è impostata su true in `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="0b5fb-159">Browser e negoziazione del contenuto</span><span class="sxs-lookup"><span data-stu-id="0b5fb-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="0b5fb-160">A differenza delle tipiche client API, web browser tendono a fornire `Accept` le intestazioni che includono un'ampia gamma di formati, inclusi i caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="0b5fb-161">Per impostazione predefinita, quando il framework rileva che la richiesta proviene da un browser, verrà ignorata il `Accept` intestazione e invece restituito il contenuto dell'applicazione configurata per il formato predefinito (JSON se non diversamente configurato).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="0b5fb-162">Ciò offre un'esperienza più coerenza quando si utilizza diversi browser per utilizzare le API.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="0b5fb-163">Se si preferisce intestazioni accept browser onore applicazione, è possibile configurare questo come parte della configurazione del MVC impostando `RespectBrowserAcceptHeader` a `true` nel `ConfigureServices` metodo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="0b5fb-164">Configurazione di formattatori</span><span class="sxs-lookup"><span data-stu-id="0b5fb-164">Configuring Formatters</span></span>

<span data-ttu-id="0b5fb-165">Se l'applicazione deve supportare formati aggiuntivi oltre il valore predefinito di JSON, è possibile aggiungere pacchetti NuGet e configurare MVC per supportarli.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="0b5fb-166">Sono disponibili i formattatori separati per l'input e output.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="0b5fb-167">Formattatori di input vengono utilizzati da [associazione di modelli](model-binding.md); formattatori di output vengono utilizzati per formattare le risposte.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="0b5fb-168">È inoltre possibile configurare [formattatori personalizzato](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="0b5fb-169">Aggiunta di supporto del formato XML</span><span class="sxs-lookup"><span data-stu-id="0b5fb-169">Adding XML Format Support</span></span>

<span data-ttu-id="0b5fb-170">Per aggiungere supporto per la formattazione XML, installare il `Microsoft.AspNetCore.Mvc.Formatters.Xml` pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="0b5fb-171">Aggiungere il XmlSerializerFormatters alla configurazione di MVC in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="0b5fb-172">In alternativa, è possibile aggiungere solo il formattatore di output:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="0b5fb-173">Questi due approcci eseguirà la serializzazione dei risultati mediante `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="0b5fb-174">Se si preferisce, è possibile utilizzare il `System.Runtime.Serialization.DataContractSerializer` aggiungendo il formattatore associato:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="0b5fb-175">Dopo aver aggiunto il supporto per la formattazione XML, i metodi del controller devono restituire il formato appropriato in base alla richiesta `Accept` intestazione, come questa Fiddler riportato:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Console Fiddler: scheda The non elaborato per la richiesta viene mostrato il valore dell'intestazione Accept è application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="0b5fb-178">È possibile visualizzare nella scheda controlli che è stata effettuata la richiesta GET non elaborato con un `Accept: application/xml` intestazione set.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="0b5fb-179">Nel riquadro di risposta viene mostrato il `Content-Type: application/xml` , intestazione e il `Author` oggetto è stato serializzato in XML.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="0b5fb-180">Utilizzare la scheda Composer per modificare la richiesta per specificare `application/json` nel `Accept` intestazione.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="0b5fb-181">Eseguire la richiesta e risposta verrà formattata come JSON:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Console Fiddler: scheda The non elaborato per la richiesta viene mostrato il valore dell'intestazione Accept è application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="0b5fb-184">In questa schermata, è possibile visualizzare la richiesta imposta un'intestazione di `Accept: application/json` e la risposta specifica lo stesso come relativo `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="0b5fb-185">Il `Author` oggetto viene visualizzato nel corpo della risposta, in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="0b5fb-186">Utilizzo forzato di un particolare formato</span><span class="sxs-lookup"><span data-stu-id="0b5fb-186">Forcing a Particular Format</span></span>

<span data-ttu-id="0b5fb-187">Se si desidera limitare i formati di risposta per un'azione specifica, è possibile, è possibile applicare il `[Produces]` filtro.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="0b5fb-188">Il `[Produces]` filtro specifica i formati di risposta per un'azione specifica (o controller).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="0b5fb-189">Come la maggior parte [filtri](../controllers/filters.md), questa può essere applicata l'azione, un controller o un ambito globale.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="0b5fb-190">Il `[Produces]` filtro forzerà tutte le azioni all'interno di `AuthorsController` per restituire le risposte in formato JSON, anche se altri formattatori sono stati configurati per l'applicazione e il client fornito un `Accept` intestazione richiesta di un altro disponibili formato.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="0b5fb-191">Vedere [filtri](../controllers/filters.md) per ulteriori informazioni, incluse le modalità applicare filtri a livello globale.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="0b5fb-192">Formattatori di casi speciali</span><span class="sxs-lookup"><span data-stu-id="0b5fb-192">Special Case Formatters</span></span>

<span data-ttu-id="0b5fb-193">Alcuni casi speciali vengono implementati mediante i formattatori predefiniti.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="0b5fb-194">Per impostazione predefinita, `string` verranno formattati come tipi restituiti *text/plain* (*text/html* se richiesto tramite `Accept` intestazione).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="0b5fb-195">Questo comportamento può essere rimossa mediante la rimozione di `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="0b5fb-196">La rimozione di formattatori nel `Configure` metodo *Startup.cs* (mostrato sotto).</span><span class="sxs-lookup"><span data-stu-id="0b5fb-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="0b5fb-197">Le azioni con un oggetto modello restituiscono tipo non restituirà un 204 Nessun contenuto risposta quando viene restituito `null`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="0b5fb-198">Questo comportamento può essere rimossa mediante la rimozione di `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="0b5fb-199">Nell'esempio di codice rimuove il `TextOutputFormatter` e `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="0b5fb-200">Senza il `TextOutputFormatter`, `string` restituire tipi 406-pagina non valida, ad esempio.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="0b5fb-201">Si noti che se esiste un formattatore XML, verrà formattata `string` tipi restituiti, se il `TextOutputFormatter` viene rimosso.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="0b5fb-202">Senza il `HttpNoContentOutputFormatter`, oggetti null vengono formattati mediante il formattatore configurato.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="0b5fb-203">Ad esempio, il formattatore JSON limitino a restituire una risposta con un corpo di `null`, mentre il formattatore XML verrà restituito un elemento XML vuoto con l'attributo `xsi:nil="true"` impostato.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="0b5fb-204">Mapping di URL di formato di risposta</span><span class="sxs-lookup"><span data-stu-id="0b5fb-204">Response Format URL Mappings</span></span>

<span data-ttu-id="0b5fb-205">I client possono richiedere un formato particolare come parte dell'URL, ad esempio nella stringa di query o parte del percorso, o mediante un'estensione di file di formato, ad esempio XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="0b5fb-206">Il mapping da un percorso della richiesta deve essere specificato nella route che utilizza l'API.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="0b5fb-207">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0b5fb-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="0b5fb-208">Questa route consentirebbe il formato richiesto di specificare come un'estensione di file facoltativo.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="0b5fb-209">Il `[FormatFilter]` attributo verifica l'esistenza del valore di formato di `RouteData` e verrà eseguito il mapping il formato della risposta al formattatore appropriato quando viene creata la risposta.</span><span class="sxs-lookup"><span data-stu-id="0b5fb-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="0b5fb-210">Route</span><span class="sxs-lookup"><span data-stu-id="0b5fb-210">Route</span></span>                      | <span data-ttu-id="0b5fb-211">Formattatore</span><span class="sxs-lookup"><span data-stu-id="0b5fb-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="0b5fb-212">Il formattatore di output predefinito</span><span class="sxs-lookup"><span data-stu-id="0b5fb-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="0b5fb-213">Il formattatore JSON (se configurata)</span><span class="sxs-lookup"><span data-stu-id="0b5fb-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="0b5fb-214">Il formattatore XML (se configurata)</span><span class="sxs-lookup"><span data-stu-id="0b5fb-214">The XML formatter (if configured)</span></span>  |
