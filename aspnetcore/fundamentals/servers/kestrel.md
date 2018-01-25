---
title: Implementazione del server web kestrel in ASP.NET Core
author: tdykstra
description: Introduce Kestrel, il server web multipiattaforma per ASP.NET Core in base a libuv.
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e2b28f15e47789ac89213e57396060ee356ee33
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="fd857-103">Introduzione all'implementazione di server web Kestrel in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd857-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="fd857-104">Da [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), e [Stephen Halter](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="fd857-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="fd857-105">Kestrel è una multipiattaforma [server web per ASP.NET Core](index.md) in base a [libuv](https://github.com/libuv/libuv), una libreria dei / o asincroni multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="fd857-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="fd857-106">Kestrel è il server web che è incluso per impostazione predefinita nei modelli di progetto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd857-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="fd857-107">Kestrel supporta le funzionalità seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd857-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="fd857-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fd857-108">HTTPS</span></span>
  * <span data-ttu-id="fd857-109">Aggiornamento opaco usato per abilitare [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="fd857-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="fd857-110">Socket ad alte prestazioni dietro Nginx UNIX</span><span class="sxs-lookup"><span data-stu-id="fd857-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="fd857-111">Kestrel è supportata in tutte le piattaforme e le versioni che supporta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd857-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-113">[Consente di visualizzare o scaricare il codice di esempio per 2. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd857-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd857-115">[Consente di visualizzare o scaricare il codice di esempio per 1. x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([come scaricare](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd857-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="fd857-116">Quando utilizzare Kestrel con un proxy inverso</span><span class="sxs-lookup"><span data-stu-id="fd857-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-118">È possibile usare Kestrel da solo o con un *server proxy inverso*, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd857-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="fd857-119">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="fd857-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica direttamente con Internet senza un server proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd857-122">È possibile usare una delle due configurazioni &mdash; con o senza un server proxy inverso &mdash; anche se Kestrel viene esposto solo a una rete interna.</span><span class="sxs-lookup"><span data-stu-id="fd857-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd857-124">Se l'applicazione accetta le richieste solo da una rete interna, è possibile usare Kestrel da solo.</span><span class="sxs-lookup"><span data-stu-id="fd857-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel comunica direttamente con la rete interna](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="fd857-126">Se si espone l'applicazione a Internet, è necessario usare IIS, Nginx o Apache come *server proxy inverso*.</span><span class="sxs-lookup"><span data-stu-id="fd857-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="fd857-127">Un server proxy inverso riceve le richieste HTTP da Internet e le inoltra a Kestrel dopo alcune operazioni di gestione preliminari.</span><span class="sxs-lookup"><span data-stu-id="fd857-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel comunica indirettamente con Internet attraverso un server proxy inverso, ad esempio IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="fd857-129">Un proxy inverso è obbligatorio per le distribuzioni di edge (esposte per il traffico da Internet) per motivi di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="fd857-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="fd857-130">Nelle versioni 1. x di Kestrel non è disponibile una serie completa di difese contro gli attacchi.</span><span class="sxs-lookup"><span data-stu-id="fd857-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="fd857-131">Queste comprendono, ma non è limitata a timeout appropriato, limiti di dimensioni e i limiti di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="fd857-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="fd857-132">Uno scenario che richiede un proxy inverso è quando si dispone di più applicazioni che condividono lo stesso IP e porta in esecuzione in un singolo server.</span><span class="sxs-lookup"><span data-stu-id="fd857-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="fd857-133">Che non funziona con Kestrel direttamente poiché Kestrel non supporta lo stesso IP e porta tra più processi di condivisione.</span><span class="sxs-lookup"><span data-stu-id="fd857-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="fd857-134">Quando si configura Kestrel per l'ascolto su una porta, gestisce tutto il traffico per tale porta indipendentemente dall'intestazione host.</span><span class="sxs-lookup"><span data-stu-id="fd857-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="fd857-135">Un proxy inverso che può condividere porte è necessario inoltrare quindi a Kestrel su una porta e un IP univoco.</span><span class="sxs-lookup"><span data-stu-id="fd857-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="fd857-136">Anche se non è necessario un server proxy inverso, utilizzando uno potrebbe essere una buona scelta per altri motivi:</span><span class="sxs-lookup"><span data-stu-id="fd857-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="fd857-137">È possibile limitare la superficie esposta.</span><span class="sxs-lookup"><span data-stu-id="fd857-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="fd857-138">Fornisce un ulteriore livello facoltativo di configurazione e protezione avanzata.</span><span class="sxs-lookup"><span data-stu-id="fd857-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="fd857-139">Potrebbe essere migliore integrazione con l'infrastruttura esistente.</span><span class="sxs-lookup"><span data-stu-id="fd857-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="fd857-140">Semplifica il bilanciamento del carico e configurazione SSL.</span><span class="sxs-lookup"><span data-stu-id="fd857-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="fd857-141">Solo il server di proxy inverso richiede un certificato SSL e tale server può comunicare con il server applicazioni nella rete interna tramite HTTP semplice.</span><span class="sxs-lookup"><span data-stu-id="fd857-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="fd857-142">Come usare Kestrel nelle applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd857-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-144">Il [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) incluso nel pacchetto di [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="fd857-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="fd857-145">Modelli di progetto ASP.NET Core utilizzano Kestrel per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd857-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="fd857-146">In *Program.cs*, il codice chiama modello `CreateDefaultBuilder`, che chiama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) dietro le quinte.</span><span class="sxs-lookup"><span data-stu-id="fd857-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="fd857-147">Se è necessario configurare le opzioni Kestrel, chiamare `UseKestrel` in *Program.cs* come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="fd857-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd857-149">Installare il [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd857-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="fd857-150">Chiamare il [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metodo di estensione su `WebHostBuilder` nel `Main` metodo, specificando uno [opzioni Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) che è necessario, come illustrato nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="fd857-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="fd857-151">Opzioni kestrel</span><span class="sxs-lookup"><span data-stu-id="fd857-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-153">Il server web Kestrel dispone di opzioni di configurazione del vincolo che risultano particolarmente utili nelle distribuzioni con connessione Internet.</span><span class="sxs-lookup"><span data-stu-id="fd857-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="fd857-154">Ecco alcuni dei limiti di cui che è possibile impostare:</span><span class="sxs-lookup"><span data-stu-id="fd857-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="fd857-155">Numero massimo di connessioni client</span><span class="sxs-lookup"><span data-stu-id="fd857-155">Maximum client connections</span></span>
- <span data-ttu-id="fd857-156">Dimensione massima del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="fd857-156">Maximum request body size</span></span>
- <span data-ttu-id="fd857-157">Velocità minima dei dati del corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="fd857-157">Minimum request body data rate</span></span>

<span data-ttu-id="fd857-158">Impostare questi vincoli e gli altri il `Limits` proprietà del [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="fd857-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="fd857-159">Il `Limits` proprietà contiene un'istanza di [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) classe.</span><span class="sxs-lookup"><span data-stu-id="fd857-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="fd857-160">**Connessioni client massima**</span><span class="sxs-lookup"><span data-stu-id="fd857-160">**Maximum client connections**</span></span>

<span data-ttu-id="fd857-161">Per l'intera applicazione con il codice seguente, è possibile impostare il numero massimo di connessioni simultanee aperte TCP:</span><span class="sxs-lookup"><span data-stu-id="fd857-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="fd857-162">È previsto un limite separato per le connessioni che sono stati aggiornati da HTTP o HTTPS a un altro protocollo (ad esempio, in una richiesta WebSocket).</span><span class="sxs-lookup"><span data-stu-id="fd857-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="fd857-163">Dopo l'aggiornamento di una connessione, non è stato preso in considerazione il `MaxConcurrentConnections` limite.</span><span class="sxs-lookup"><span data-stu-id="fd857-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="fd857-164">Il numero massimo di connessioni è illimitato (null) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd857-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="fd857-165">**Dimensione massima del corpo della richiesta**</span><span class="sxs-lookup"><span data-stu-id="fd857-165">**Maximum request body size**</span></span>

<span data-ttu-id="fd857-166">Dimensioni predefinite del corpo massima della richiesta sono 30,000,000 byte, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="fd857-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="fd857-167">Il metodo consigliato per ignorare il limite in un'applicazione ASP.NET MVC di base consiste nell'utilizzare il [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attributo in un metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="fd857-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="fd857-168">Di seguito è riportato un esempio che illustra come configurare il vincolo per l'intera applicazione, ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="fd857-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="fd857-169">È possibile sostituire l'impostazione di una richiesta nel middleware specifica:</span><span class="sxs-lookup"><span data-stu-id="fd857-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="fd857-170">Se si tenta di configurare il limite per una richiesta dopo l'applicazione ha avviato la richiesta di lettura, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="fd857-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="fd857-171">È presente un `IsReadOnly` proprietà che indica se il `MaxRequestBodySize` proprietà è in stato di sola lettura, pertanto è troppo tardi per configurare il limite.</span><span class="sxs-lookup"><span data-stu-id="fd857-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="fd857-172">**Velocità di dati minima richiesta del corpo**</span><span class="sxs-lookup"><span data-stu-id="fd857-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="fd857-173">Kestrel controlla ogni secondo i vocabolari alla frequenza specificata in byte al secondo.</span><span class="sxs-lookup"><span data-stu-id="fd857-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="fd857-174">Se la velocità scende sotto il valore minimo, la connessione è scaduta. Il periodo di tolleranza è la quantità di tempo che Kestrel fornisce al client per aumentare la velocità di trasmissione fino a minimo. non è attivo il tasso di questo periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="fd857-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="fd857-175">Il periodo di prova consente di evitare l'eliminazione di connessioni che sono inizialmente l'invio dei dati a una velocità lenta a causa di TCP avvio lento.</span><span class="sxs-lookup"><span data-stu-id="fd857-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="fd857-176">La frequenza minima predefinita è 240 byte al secondo, con un periodo di tolleranza di 5 secondi.</span><span class="sxs-lookup"><span data-stu-id="fd857-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="fd857-177">Una frequenza minima si applica anche alla risposta.</span><span class="sxs-lookup"><span data-stu-id="fd857-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="fd857-178">Il codice per impostare il limite di richieste e il limite di risposta è lo stesso, ad eccezione che `RequestBody` o `Response` nei nomi di proprietà e di interfaccia.</span><span class="sxs-lookup"><span data-stu-id="fd857-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="fd857-179">Di seguito è riportato un esempio che illustra come configurare i tassi di dati minimo nel *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd857-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="fd857-180">È possibile configurare le frequenze per ogni richiesta nel middleware:</span><span class="sxs-lookup"><span data-stu-id="fd857-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="fd857-181">Per informazioni sulle altre opzioni Kestrel, vedere le classi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd857-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="fd857-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="fd857-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="fd857-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="fd857-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="fd857-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="fd857-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd857-186">Per informazioni sulle opzioni di Kestrel, vedere [KestrelServerOptions classe](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="fd857-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="fd857-187">Configurazione dell'endpoint</span><span class="sxs-lookup"><span data-stu-id="fd857-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-189">Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fd857-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fd857-190">Configurare i prefissi URL e delle porte per Kestrel in ascolto su chiamando `Listen` o `ListenUnixSocket` metodi su `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="fd857-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="fd857-191">(`UseUrls`, `urls` argomento della riga di comando, mentre la variabile di ambiente ASPNETCORE_URLS funzionano anche ma presentano le limitazioni indicate [più avanti in questo articolo](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="fd857-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="fd857-192">**Associare a un socket TCP**</span><span class="sxs-lookup"><span data-stu-id="fd857-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="fd857-193">Il `Listen` metodo associa a un socket TCP, e un'espressione lambda opzioni consente di configurare un certificato SSL:</span><span class="sxs-lookup"><span data-stu-id="fd857-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="fd857-194">Si noti come in questo esempio consente di configurare SSL per un determinato endpoint mediante [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="fd857-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="fd857-195">È possibile utilizzare la stessa API per configurare altre impostazioni Kestrel per determinati endpoint.</span><span class="sxs-lookup"><span data-stu-id="fd857-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="fd857-196">**Collegare a un socket di Unix**</span><span class="sxs-lookup"><span data-stu-id="fd857-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="fd857-197">Può restare in ascolto su un socket Unix per migliorare le prestazioni con Nginx, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="fd857-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="fd857-198">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="fd857-198">**Port 0**</span></span>

<span data-ttu-id="fd857-199">Se si specifica il numero di porta 0, Kestrel associa dinamicamente a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="fd857-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fd857-200">Nell'esempio seguente viene illustrato come determinare quale porta Kestrel effettivamente associato a in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="fd857-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="fd857-201">**Limitazioni di UseUrls**</span><span class="sxs-lookup"><span data-stu-id="fd857-201">**UseUrls limitations**</span></span>

<span data-ttu-id="fd857-202">È possibile configurare gli endpoint chiamando il `UseUrls` metodo o utilizzando il `urls` argomento della riga di comando o la variabile di ambiente ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="fd857-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="fd857-203">Questi metodi sono utili se si desidera che il codice per utilizzare server diverso da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="fd857-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="fd857-204">Tuttavia, è necessario tenere presenti queste limitazioni:</span><span class="sxs-lookup"><span data-stu-id="fd857-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="fd857-205">È possibile utilizzare SSL con questi metodi.</span><span class="sxs-lookup"><span data-stu-id="fd857-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="fd857-206">Se si utilizzano entrambi la `Listen` (metodo) e `UseUrls`, `Listen` eseguire l'override di endpoint il `UseUrls` endpoint.</span><span class="sxs-lookup"><span data-stu-id="fd857-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="fd857-207">**Configurazione dell'endpoint per IIS**</span><span class="sxs-lookup"><span data-stu-id="fd857-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="fd857-208">Se si utilizza IIS, i binding di URL per IIS esegue l'override di tutti i binding è impostata tramite la chiamata a `Listen` o `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd857-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="fd857-209">Per ulteriori informazioni, vedere [Introduzione a ASP.NET Core modulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="fd857-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd857-211">Per impostazione predefinita ASP.NET Core associa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="fd857-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="fd857-212">È possibile configurare i prefissi URL e delle porte per Kestrel in ascolto su utilizzando il `UseUrls` metodo di estensione, il `urls` argomento della riga di comando o il sistema di configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd857-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="fd857-213">Per ulteriori informazioni su questi metodi, vedere [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="fd857-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="fd857-214">Per informazioni sul funzionamento dell'associazione URL quando si utilizza IIS come un proxy inverso, vedere [ASP.NET Core modulo](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="fd857-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="fd857-215">Prefissi URL</span><span class="sxs-lookup"><span data-stu-id="fd857-215">URL prefixes</span></span>

<span data-ttu-id="fd857-216">Se si chiama `UseUrls` o utilizzare il `urls` argomento della riga di comando o una variabile di ambiente ASPNETCORE_URLS, i prefissi URL possono essere in uno dei formati seguenti.</span><span class="sxs-lookup"><span data-stu-id="fd857-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd857-218">Solo i prefissi URL HTTP sono validi. Kestrel non supporta SSL quando si configura associazioni URL utilizzando `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="fd857-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="fd857-219">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="fd857-220">0.0.0.0 è un caso speciale che viene associato a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd857-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="fd857-221">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="fd857-222">[:] equivale a IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="fd857-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="fd857-223">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="fd857-224">I nomi, host \*, e +, non sono speciali.</span><span class="sxs-lookup"><span data-stu-id="fd857-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="fd857-225">Tutto ciò che non è un indirizzo IP o "localhost" riconosciuta verrà associato a tutti i indirizzi IP IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd857-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fd857-226">Se è necessario associare i nomi host diversi per diverse applicazioni ASP.NET Core sulla stessa porta, utilizzare [HTTP.sys](httpsys.md) o in un server proxy inverso, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd857-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="fd857-227">Nome "Localhost" con l'IP di loopback o di numero di porta con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fd857-228">Quando `localhost` viene specificato, Kestrel tenta di associare le interfacce di loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="fd857-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fd857-229">Se la porta richiesta è in uso da un altro servizio in entrambe le interfacce loopback, Kestrel non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="fd857-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fd857-230">Se nessuna delle due interfacce di loopback non è disponibile per qualsiasi motivo (la maggior parte delle comune perché non è supportato IPv6), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="fd857-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="fd857-232">Indirizzo IPv4 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="fd857-233">0.0.0.0 è un caso speciale che viene associato a tutti gli indirizzi IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd857-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="fd857-234">Indirizzo IPv6 con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="fd857-235">[:] equivale a IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="fd857-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="fd857-236">Nome host con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="fd857-237">I nomi, host \*, e + non sono speciali.</span><span class="sxs-lookup"><span data-stu-id="fd857-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="fd857-238">Tutto ciò che non è un indirizzo IP o "localhost" riconosciuta associa a tutti i indirizzi IP IPv6 e IPv4.</span><span class="sxs-lookup"><span data-stu-id="fd857-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="fd857-239">Se è necessario associare i nomi host diversi per diverse applicazioni ASP.NET Core sulla stessa porta, utilizzare [WebListener](weblistener.md) o in un server proxy inverso, ad esempio IIS, Nginx o Apache.</span><span class="sxs-lookup"><span data-stu-id="fd857-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="fd857-240">Nome "Localhost" con l'IP di loopback o di numero di porta con numero di porta</span><span class="sxs-lookup"><span data-stu-id="fd857-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="fd857-241">Quando `localhost` viene specificato, Kestrel tenta di associare le interfacce di loopback IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="fd857-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="fd857-242">Se la porta richiesta è in uso da un altro servizio in entrambe le interfacce loopback, Kestrel non verrà avviato.</span><span class="sxs-lookup"><span data-stu-id="fd857-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="fd857-243">Se nessuna delle due interfacce di loopback non è disponibile per qualsiasi motivo (la maggior parte delle comune perché non è supportato IPv6), Kestrel registra un avviso.</span><span class="sxs-lookup"><span data-stu-id="fd857-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="fd857-244">Socket di UNIX</span><span class="sxs-lookup"><span data-stu-id="fd857-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="fd857-245">**Porta 0**</span><span class="sxs-lookup"><span data-stu-id="fd857-245">**Port 0**</span></span>

<span data-ttu-id="fd857-246">Se si specifica il numero di porta 0, Kestrel associa dinamicamente a una porta disponibile.</span><span class="sxs-lookup"><span data-stu-id="fd857-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="fd857-247">Associazione alla porta 0 è consentito per qualsiasi nome host o l'indirizzo IP, ad eccezione di `localhost` nome.</span><span class="sxs-lookup"><span data-stu-id="fd857-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="fd857-248">Nell'esempio seguente viene illustrato come determinare quale porta Kestrel effettivamente associato a in fase di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="fd857-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="fd857-249">**Prefissi URL per SSL**</span><span class="sxs-lookup"><span data-stu-id="fd857-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="fd857-250">Assicurarsi di includere i prefissi URL con `https:` se si chiama il `UseHttps` il metodo di estensione, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="fd857-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="fd857-251">Non può trovarsi sulla stessa porta HTTPS e HTTP.</span><span class="sxs-lookup"><span data-stu-id="fd857-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="fd857-252">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fd857-252">Next steps</span></span>

<span data-ttu-id="fd857-253">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="fd857-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd857-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd857-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="fd857-255">Applicazione di esempio per 2. x</span><span class="sxs-lookup"><span data-stu-id="fd857-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="fd857-256">Codice sorgente kestrel</span><span class="sxs-lookup"><span data-stu-id="fd857-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd857-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd857-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="fd857-258">Applicazione di esempio per 1. x</span><span class="sxs-lookup"><span data-stu-id="fd857-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="fd857-259">Codice sorgente kestrel</span><span class="sxs-lookup"><span data-stu-id="fd857-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
