---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Esercitazione su Razor Pages in ASP.NET Core. Include MVC Core, ASP.NET Core 2.x, l'introduzione allo sviluppo Web e Visual Studio 2017. Il documento offre una panoramica sull'uso di Razor Pages in ASP.NET Core per agevolare lo sviluppo di scenari incentrati sulle pagine.
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 059dc3a163c646877da40a73bcc9a75eb38fb345
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="6b5af-105">Introduzione a Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b5af-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="6b5af-106">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="6b5af-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="6b5af-107">Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.</span><span class="sxs-lookup"><span data-stu-id="6b5af-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="6b5af-108">Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere l'[introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="6b5af-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="6b5af-109">Questo documento offre un'introduzione a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b5af-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="6b5af-110">Non è un'esercitazione dettagliata.</span><span class="sxs-lookup"><span data-stu-id="6b5af-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="6b5af-111">Se si trovano alcune sezioni più difficili da seguire, vedere la [Guida introduttiva a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6b5af-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="6b5af-112">Prerequisiti di ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="6b5af-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="6b5af-113">Installare [.NET Core](https://www.microsoft.com/net/core) 2.0.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="6b5af-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="6b5af-114">Se si usa Visual Studio, installare [Visual Studio](https://www.visualstudio.com/vs/) 2017 versione 15.3 o versione successiva con i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b5af-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="6b5af-115">**Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="6b5af-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="6b5af-116">**Sviluppo multipiattaforma .NET Core**</span><span class="sxs-lookup"><span data-stu-id="6b5af-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="6b5af-117">Creazione di un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6b5af-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b5af-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6b5af-119">Per istruzioni dettagliate su come creare un progetto Razor Pages con Visual Studio, vedere la [guida introduttiva a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6b5af-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6b5af-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b5af-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6b5af-121">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b5af-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="6b5af-122">Aprire il file *CSPROJ* generato da Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="6b5af-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6b5af-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b5af-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="6b5af-124">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b5af-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6b5af-125">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="6b5af-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="6b5af-126">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b5af-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="6b5af-127">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-127">Razor Pages</span></span>

<span data-ttu-id="6b5af-128">La funzionalità Razor Pages è abilitata in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="6b5af-129">Si consideri una pagina di base: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="6b5af-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="6b5af-130">Il codice precedente è molto simile a un file di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="6b5af-131">Ciò che lo differenzia è la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="6b5af-132">`@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller.</span><span class="sxs-lookup"><span data-stu-id="6b5af-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="6b5af-133">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="6b5af-134">`@page` influisce sul comportamento di altri costrutti Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="6b5af-135">Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="6b5af-136">Il file *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="6b5af-137">Il file"code-behind" *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-137">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="6b5af-138">Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="6b5af-139">Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="6b5af-140">Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="6b5af-141">Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system.</span><span class="sxs-lookup"><span data-stu-id="6b5af-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="6b5af-142">Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="6b5af-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="6b5af-143">Percorso e nome file</span><span class="sxs-lookup"><span data-stu-id="6b5af-143">File name and path</span></span>               | <span data-ttu-id="6b5af-144">URL corrispondente</span><span class="sxs-lookup"><span data-stu-id="6b5af-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6b5af-145">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="6b5af-146">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="6b5af-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="6b5af-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="6b5af-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="6b5af-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="6b5af-150">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="6b5af-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="6b5af-151">Note:</span><span class="sxs-lookup"><span data-stu-id="6b5af-151">Notes:</span></span>

* <span data-ttu-id="6b5af-152">Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b5af-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="6b5af-153">`Index` è la pagina predefinita quando un URL non include una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="6b5af-154">Scrittura di un form di base</span><span class="sxs-lookup"><span data-stu-id="6b5af-154">Writing a basic form</span></span>

<span data-ttu-id="6b5af-155">Le funzionalità Razor Pages sono progettate in modo da semplificare i modelli normalmente usati con i Web browser.</span><span class="sxs-lookup"><span data-stu-id="6b5af-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="6b5af-156">L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="6b5af-157">Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="6b5af-158">Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="6b5af-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="6b5af-159">Il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="6b5af-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="6b5af-160">Il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="6b5af-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="6b5af-161">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="6b5af-162">Il file code-behind *Pages/Create.cshtml.cs* per la visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="6b5af-162">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="6b5af-163">Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="6b5af-164">La classe `PageModel` consente la separazione della logica di una pagina dalla relativa presentazione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="6b5af-165">Definisce i gestori di pagina per le richieste inviate alla pagina e i dati usati per il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="6b5af-166">Questa separazione consente di gestire le dipendenze della pagina tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e di eseguire [unit test](xref:testing/razor-pages-testing) delle pagine.</span><span class="sxs-lookup"><span data-stu-id="6b5af-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="6b5af-167">La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form.</span><span class="sxs-lookup"><span data-stu-id="6b5af-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="6b5af-168">È possibile aggiungere metodi gestore per qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b5af-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="6b5af-169">I gestori più comuni sono:</span><span class="sxs-lookup"><span data-stu-id="6b5af-169">The most common handlers are:</span></span>

* <span data-ttu-id="6b5af-170">`OnGet` per inizializzare lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="6b5af-171">Esempio di [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="6b5af-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="6b5af-172">`OnPost` per gestire gli invii di form.</span><span class="sxs-lookup"><span data-stu-id="6b5af-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="6b5af-173">Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="6b5af-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="6b5af-174">Il codice `OnPostAsync` nell'esempio precedente è simile a ciò che in genere si scrive in un controller.</span><span class="sxs-lookup"><span data-stu-id="6b5af-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="6b5af-175">Il codice precedente è tipico di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b5af-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="6b5af-176">La maggior parte delle primitive di MVC, ad esempio l'[associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation) e i risultati dell'azione vengono condivise.</span><span class="sxs-lookup"><span data-stu-id="6b5af-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="6b5af-177">Il metodo `OnPostAsync` precedente:</span><span class="sxs-lookup"><span data-stu-id="6b5af-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="6b5af-178">Il flusso di base di `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="6b5af-179">Verificare se sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="6b5af-179">Check for validation errors.</span></span>

*  <span data-ttu-id="6b5af-180">Se non sono presenti errori, salvare i dati e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="6b5af-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="6b5af-181">Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="6b5af-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="6b5af-182">La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali.</span><span class="sxs-lookup"><span data-stu-id="6b5af-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="6b5af-183">In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.</span><span class="sxs-lookup"><span data-stu-id="6b5af-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="6b5af-184">Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="6b5af-185">`RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b5af-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="6b5af-186">Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6b5af-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="6b5af-187">Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="6b5af-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="6b5af-188">Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="6b5af-189">`Page` restituisce un'istanza di `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="6b5af-190">La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="6b5af-191">`PageResult` è il tipo restituito <!-- Review  --> predefinito per un metodo gestore.</span><span class="sxs-lookup"><span data-stu-id="6b5af-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="6b5af-192">Un metodo gestore che restituisce `void` esegue il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="6b5af-193">La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="6b5af-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="6b5af-194">Per impostazione predefinita Razor Pages associa le proprietà solo ai verbi non GET.</span><span class="sxs-lookup"><span data-stu-id="6b5af-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="6b5af-195">Il binding alle proprietà può ridurre la quantità di codice da scrivere.</span><span class="sxs-lookup"><span data-stu-id="6b5af-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="6b5af-196">Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name" />`) e accettare l'input.</span><span class="sxs-lookup"><span data-stu-id="6b5af-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="6b5af-197">La home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="6b5af-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="6b5af-198">Il file code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="6b5af-199">Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:</span><span class="sxs-lookup"><span data-stu-id="6b5af-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="6b5af-200">[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usato l'attributo `asp-route-{value}` per generare un collegamento alla pagina di modifica.</span><span class="sxs-lookup"><span data-stu-id="6b5af-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="6b5af-201">Il collegamento contiene i dati della route con l'ID contatto.</span><span class="sxs-lookup"><span data-stu-id="6b5af-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="6b5af-202">Ad esempio `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="6b5af-203">Il file *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="6b5af-204">La prima riga contiene la direttiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="6b5af-205">Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="6b5af-206">Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="6b5af-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="6b5af-207">Il file *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="6b5af-208">Il file *Index.cshtml* contiene anche il markup per creare un pulsante di eliminazione per ogni contatto cliente:</span><span class="sxs-lookup"><span data-stu-id="6b5af-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="6b5af-209">Quando viene eseguito il rendering del pulsante di eliminazione in HTML, il relativo `formaction` include i parametri per:</span><span class="sxs-lookup"><span data-stu-id="6b5af-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="6b5af-210">L'ID di contatto cliente dall'attributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="6b5af-211">L'`handler` specificato dall'attributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="6b5af-212">Di seguito è riportato un esempio di pulsante di eliminazione di cui è stato eseguito il rendering con un ID di contatto cliente `1`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="6b5af-213">Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server.</span><span class="sxs-lookup"><span data-stu-id="6b5af-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="6b5af-214">Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="6b5af-215">Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="6b5af-216">Se `asp-page-handler` viene impostato su un valore diverso, come `remove`, viene selezionato un metodo gestore della pagina con il nome `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="6b5af-217">Il metodo `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="6b5af-218">Accetta l'`id` dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6b5af-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="6b5af-219">Interroga il database in merito al contatto del cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="6b5af-220">Se viene trovato il contatto cliente, viene rimosso dall'elenco dei contatti del cliente.</span><span class="sxs-lookup"><span data-stu-id="6b5af-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="6b5af-221">Il database viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6b5af-221">The database is updated.</span></span>
* <span data-ttu-id="6b5af-222">Chiama `RedirectToPage` per reindirizzare alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6b5af-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="6b5af-223">XSRF/CSRF e Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="6b5af-224">Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="6b5af-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="6b5af-225">La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b5af-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="6b5af-226">Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="6b5af-227">Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="6b5af-228">I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="6b5af-229">La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="6b5af-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="6b5af-230">Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="6b5af-231">Il [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="6b5af-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="6b5af-232">Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.</span><span class="sxs-lookup"><span data-stu-id="6b5af-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="6b5af-233">Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="6b5af-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="6b5af-234">Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="6b5af-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="6b5af-235">La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6b5af-236">**Nota**: il layout è nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="6b5af-237">Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="6b5af-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="6b5af-238">Un layout nella cartella *Pages* può essere usato da qualsiasi pagina Razor della cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="6b5af-239">Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="6b5af-240">*Views/Shared* è un modello destinato alle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="6b5af-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="6b5af-241">Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.</span><span class="sxs-lookup"><span data-stu-id="6b5af-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="6b5af-242">La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="6b5af-243">Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="6b5af-244">Aggiungere un file *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6b5af-245">`@namespace` viene spiegato in seguito nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="6b5af-246">La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="6b5af-247">Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:</span><span class="sxs-lookup"><span data-stu-id="6b5af-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="6b5af-248">La direttiva imposta lo spazio dei nomi per la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="6b5af-249">La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="6b5af-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="6b5af-250">Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="6b5af-251">Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="6b5af-252">Ad esempio, il file code-behind *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="6b5af-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="6b5af-253">Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="6b5af-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="6b5af-254">Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde al file code-behind.</span><span class="sxs-lookup"><span data-stu-id="6b5af-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="6b5af-255">La direttiva `@namespace` è stata progettata in modo che le classi C# aggiunte a un progetto e il codice generato dalle pagine *funzionino* senza dover aggiungere una direttiva `@using` per il file code-behind.</span><span class="sxs-lookup"><span data-stu-id="6b5af-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="6b5af-256">**Nota:** `@namespace` funziona anche con le normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="6b5af-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="6b5af-257">Il file di visualizzazione *Pages/Create.cshtml* originale:</span><span class="sxs-lookup"><span data-stu-id="6b5af-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="6b5af-258">Il file di visualizzazione *Pages/Create.cshtml* aggiornato:</span><span class="sxs-lookup"><span data-stu-id="6b5af-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="6b5af-259">Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="6b5af-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="6b5af-260">Generazione di URL per le pagine</span><span class="sxs-lookup"><span data-stu-id="6b5af-260">URL generation for Pages</span></span>

<span data-ttu-id="6b5af-261">La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="6b5af-262">L'applicazione ha la struttura di file o cartella seguente:</span><span class="sxs-lookup"><span data-stu-id="6b5af-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="6b5af-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="6b5af-263">*/Pages*</span></span>

  * <span data-ttu-id="6b5af-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="6b5af-265">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="6b5af-265">*/Customer*</span></span>

    * <span data-ttu-id="6b5af-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="6b5af-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="6b5af-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b5af-268">*Index.cshtml*</span></span>

<span data-ttu-id="6b5af-269">Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="6b5af-270">La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="6b5af-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="6b5af-271">La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="6b5af-272">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6b5af-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="6b5af-273">Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale (`/`), ad esempio `/Index`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="6b5af-274">Gli esempi di generazione di URL precedenti sono molto più ricco di funzionalità rispetto a livello di codice solo un URL.</span><span class="sxs-lookup"><span data-stu-id="6b5af-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="6b5af-275">La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="6b5af-276">La generazione di URL per le pagine supporta i nomi relativi.</span><span class="sxs-lookup"><span data-stu-id="6b5af-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="6b5af-277">La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b5af-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="6b5af-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="6b5af-278">RedirectToPage(x)</span></span>| <span data-ttu-id="6b5af-279">Pagina</span><span class="sxs-lookup"><span data-stu-id="6b5af-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6b5af-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="6b5af-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="6b5af-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6b5af-281">*Pages/Index*</span></span> |
| <span data-ttu-id="6b5af-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="6b5af-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="6b5af-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6b5af-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="6b5af-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="6b5af-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="6b5af-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6b5af-285">*Pages/Index*</span></span> |
| <span data-ttu-id="6b5af-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="6b5af-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="6b5af-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6b5af-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="6b5af-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono *nomi relativi*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="6b5af-289">Il parametro `RedirectToPage` è *combinato* con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="6b5af-290">Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa.</span><span class="sxs-lookup"><span data-stu-id="6b5af-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="6b5af-291">Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella.</span><span class="sxs-lookup"><span data-stu-id="6b5af-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="6b5af-292">Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.</span><span class="sxs-lookup"><span data-stu-id="6b5af-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="6b5af-293">TempData</span><span class="sxs-lookup"><span data-stu-id="6b5af-293">TempData</span></span>

<span data-ttu-id="6b5af-294">ASP.NET Core espone la proprietà [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="6b5af-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="6b5af-295">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="6b5af-295">This property stores data until it is read.</span></span> <span data-ttu-id="6b5af-296">I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="6b5af-297">`TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b5af-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="6b5af-298">L'attributo `[TempData]` è nuovo in ASP.NET Core 2.0 ed è supportato per controller e pagine.</span><span class="sxs-lookup"><span data-stu-id="6b5af-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="6b5af-299">Il codice seguente imposta il valore di `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="6b5af-300">Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="6b5af-301">Il file code-behind *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-301">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="6b5af-302">Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="6b5af-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="6b5af-303">Più gestori per pagina</span><span class="sxs-lookup"><span data-stu-id="6b5af-303">Multiple handlers per page</span></span>

<span data-ttu-id="6b5af-304">La pagina seguente genera markup per due gestori di pagina usando l'helper tag `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="6b5af-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="6b5af-305">Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso.</span><span class="sxs-lookup"><span data-stu-id="6b5af-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="6b5af-306">L'attributo `asp-page-handler` è correlato a `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="6b5af-307">`asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="6b5af-308">`asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="6b5af-308">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="6b5af-309">Il file code-behind:</span><span class="sxs-lookup"><span data-stu-id="6b5af-309">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="6b5af-310">Il codice precedente usa *metodi gestore denominati*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="6b5af-311">I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente).</span><span class="sxs-lookup"><span data-stu-id="6b5af-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="6b5af-312">Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="6b5af-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="6b5af-313">Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="6b5af-314">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="6b5af-315">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="6b5af-316">Personalizzazione del routing</span><span class="sxs-lookup"><span data-stu-id="6b5af-316">Customizing Routing</span></span>

<span data-ttu-id="6b5af-317">Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="6b5af-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="6b5af-318">È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="6b5af-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="6b5af-319">La route precedente inserisce il nome del gestore nel percorso URL anziché nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6b5af-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="6b5af-320">`?` che segue `handler` indica che il parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6b5af-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="6b5af-321">È possibile usare `@page` per aggiungere altri segmenti e parametri alla route di una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="6b5af-322">Tutti gli elementi presenti vengono **aggiunti** alla route predefinita della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b5af-322">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="6b5af-323">L'uso di un percorso assoluto o virtuale per modificare la route della pagina, ad esempio `"~/Some/Other/Path"`, non è supportato.</span><span class="sxs-lookup"><span data-stu-id="6b5af-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="6b5af-324">Configurazione e impostazioni</span><span class="sxs-lookup"><span data-stu-id="6b5af-324">Configuration and settings</span></span>

<span data-ttu-id="6b5af-325">Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:</span><span class="sxs-lookup"><span data-stu-id="6b5af-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="6b5af-326">Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b5af-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="6b5af-327">In questo modo si potrà garantire una maggiore estendibilità in futuro.</span><span class="sxs-lookup"><span data-stu-id="6b5af-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="6b5af-328">Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="6b5af-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="6b5af-329">[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="6b5af-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="6b5af-330">Vedere l'articolo [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) (Guida introduttiva a Razor Pages in ASP.NET Core), che si basa su questa introduzione.</span><span class="sxs-lookup"><span data-stu-id="6b5af-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="6b5af-331">Specificare che Razor Pages si trova nella radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="6b5af-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="6b5af-332">Per impostazione predefinita, la directory radice di Razor Pages è */Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b5af-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="6b5af-333">Aggiungere [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages è nella radice del contenuto ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) dell'app:</span><span class="sxs-lookup"><span data-stu-id="6b5af-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="6b5af-334">Specificare che Razor Pages è in una directory radice personalizzata</span><span class="sxs-lookup"><span data-stu-id="6b5af-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="6b5af-335">Aggiungere [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages si trova in una directory radice personalizzata nell'app (fornire un percorso relativo):</span><span class="sxs-lookup"><span data-stu-id="6b5af-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="6b5af-336">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="6b5af-336">See also</span></span>

* [<span data-ttu-id="6b5af-337">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6b5af-338">Convenzioni di autorizzazione di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b5af-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="6b5af-339">Route personalizzata di Razor Pages e provider di modelli di pagina</span><span class="sxs-lookup"><span data-stu-id="6b5af-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="6b5af-340">Unità di Razor Pages e test di integrazione</span><span class="sxs-lookup"><span data-stu-id="6b5af-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
