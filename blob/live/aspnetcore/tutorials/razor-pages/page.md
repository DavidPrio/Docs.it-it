---
title: Pagine Razor create in ASP.NET Core tramite scaffolding
author: rick-anderson
description: Illustra le pagine Razor generate tramite scaffolding.
keywords: ASP.NET Core, pagine Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: e42e7e469e411d2d4bc1bd1b3a3995a77c355ebd
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="f29b1-104">Pagine Razor create in ASP.NET Core tramite scaffolding</span><span class="sxs-lookup"><span data-stu-id="f29b1-104">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f29b1-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f29b1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f29b1-106">In questa esercitazione vengono esaminate le pagine Razor create tramite scaffolding nell'argomento dell'esercitazione precedente [Aggiunta di un modello](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="f29b1-106">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="f29b1-107">[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) l'esempio.</span><span class="sxs-lookup"><span data-stu-id="f29b1-107">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="f29b1-108">Pagine di creazione, eliminazione, dettagli e modifica.</span><span class="sxs-lookup"><span data-stu-id="f29b1-108">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="f29b1-109">Esaminare il file code-behind *Pages/Movies/Index.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="f29b1-109">Examine the *Pages/Movies/Index.cshtml.cs* code-behind file: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="f29b1-110">Le pagine Razor vengono derivate da `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-110">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="f29b1-111">Per convenzione, la classe derivata `PageModel` viene denominata `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-111">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="f29b1-112">Il costruttore usa l'[inserimento delle dipendenze](xref:fundamentals/dependency-injection) per aggiungere `MovieContext` alla pagina.</span><span class="sxs-lookup"><span data-stu-id="f29b1-112">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="f29b1-113">Tutte le pagine create tramite scaffolding seguono questo schema.</span><span class="sxs-lookup"><span data-stu-id="f29b1-113">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="f29b1-114">Vedere [Codice asincrono](xref:data/ef-rp/intro#asynchronous-code) per altre informazioni sulla programmazione asincrona con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f29b1-114">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="f29b1-115">Quando per la pagina viene eseguita una richiesta, il metodo `OnGetAsync` restituisce un elenco di filmati alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-115">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="f29b1-116">Nella pagina Razor viene chiamato `OnGetAsync`o `OnGet` per inizializzare lo stato della pagina.</span><span class="sxs-lookup"><span data-stu-id="f29b1-116">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="f29b1-117">In questo caso, `OnGetAsync` ottiene un elenco di filmati da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="f29b1-117">In this case, `OnGetAsync` gets a list of movies to display.</span></span>

<span data-ttu-id="f29b1-118">Esaminare la pagina Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f29b1-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="f29b1-119">Con Razor è possibile passare da HTML a C# o scegliere un markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="f29b1-120">Quando il simbolo `@` è seguito da una [parola chiave riservata Razor ](xref:mvc/views/razor#razor-reserved-keywords), la transizione avviene in un markup specifico per Razor, altrimenti in C#.</span><span class="sxs-lookup"><span data-stu-id="f29b1-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="f29b1-121">La direttiva Razor `@page` trasforma il file in un'azione MVC &mdash; in modo che possa gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="f29b1-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="f29b1-122">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="f29b1-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="f29b1-123">`@page` è un esempio di transizione nel markup specifico per Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="f29b1-124">Vedere [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintassi Razor) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="f29b1-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="f29b1-125">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="f29b1-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="f29b1-126">L'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="f29b1-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="f29b1-127">L'espressione lambda viene controllata anziché valutata.</span><span class="sxs-lookup"><span data-stu-id="f29b1-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="f29b1-128">Non sussiste pertanto violazione di accesso quando `model`, `model.Movie`, o `model.Movie[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="f29b1-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="f29b1-129">Quando invece l'espressione lambda viene valutata (ad esempio con `@Html.DisplayFor(modelItem => item.Title)`), vengono valutati i valori proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="f29b1-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="f29b1-130">La direttiva @model</span><span class="sxs-lookup"><span data-stu-id="f29b1-130">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="f29b1-131">La direttiva `@model` specifica il tipo di modello passato alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="f29b1-132">Nell'esempio precedente la riga `@model` rende disponibile la classe derivata `PageModel` alla pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="f29b1-133">Il modello viene usato negli [helper HTML](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) `@Html.DisplayNameFor` e `@Html.DisplayName` nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f29b1-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="f29b1-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
###ViewData e layout</span><span class="sxs-lookup"><span data-stu-id="f29b1-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="f29b1-135">Esaminare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f29b1-135">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="f29b1-136">Il codice evidenziato sopra è un esempio di transizione Razor in C#.</span><span class="sxs-lookup"><span data-stu-id="f29b1-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="f29b1-137">Tra i caratteri `{` e `}` è racchiuso un blocco di codice C#.</span><span class="sxs-lookup"><span data-stu-id="f29b1-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="f29b1-138">La classe di base `PageModel` ha una proprietà del dizionario `ViewData` che può essere usata per aggiungere i dati da passare a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f29b1-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="f29b1-139">Gli oggetti vengono aggiunti al dizionario `ViewData` usando uno schema chiave/valore.</span><span class="sxs-lookup"><span data-stu-id="f29b1-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="f29b1-140">Nell'esempio precedente la proprietà "Title" viene aggiunta al dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="f29b1-141">La proprietà "Title" viene usata nel file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f29b1-141">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="f29b1-142">Il markup seguente illustra le prime righe del file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f29b1-142">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="f29b1-143">La riga `@*Markup removed for brevity.*@` è un commento Razor.</span><span class="sxs-lookup"><span data-stu-id="f29b1-143">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="f29b1-144">A differenza dei commenti HTML (`<!-- -->`), i commenti Razor non vengono inviati al client.</span><span class="sxs-lookup"><span data-stu-id="f29b1-144">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="f29b1-145">Eseguire l'app e i collegamenti di test nel progetto (**Home**, **Informazioni**, **Contatto**, **Crea**, **Modifica** ed **Elimina**).</span><span class="sxs-lookup"><span data-stu-id="f29b1-145">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="f29b1-146">In ogni pagina viene impostato il titolo che è possibile visualizzare nella scheda del browser. Quando una pagina viene specificata come segnalibro, il titolo viene usato per il segnalibro.</span><span class="sxs-lookup"><span data-stu-id="f29b1-146">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="f29b1-147">*Pages/Index.cshtml* e *Pages/Movies/Index.cshtml* hanno attualmente lo stesso titolo, ma è possibile modificarli in modo che i valori siano diversi.</span><span class="sxs-lookup"><span data-stu-id="f29b1-147">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="f29b1-148">La proprietà `Layout` viene impostata nel file *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f29b1-148">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="f29b1-149">Il markup precedente imposta il file di layout su *Pages/_Layout.cshtml* per tutti i file Razor contenuti nella cartella *Pagine*.</span><span class="sxs-lookup"><span data-stu-id="f29b1-149">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="f29b1-150">Vedere [Layout](xref:mvc/razor-pages/index#layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="f29b1-150">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="f29b1-151">Aggiornare il layout</span><span class="sxs-lookup"><span data-stu-id="f29b1-151">Update the layout</span></span>

<span data-ttu-id="f29b1-152">Modificare l'elemento `<title>` nel file *Pages/_Layout.cshtml* per usare una stringa più corta.</span><span class="sxs-lookup"><span data-stu-id="f29b1-152">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="f29b1-153">Trovare l'elemento di ancoraggio seguente nel file *Pages/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f29b1-153">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="f29b1-154">Sostituire l'elemento precedente con il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="f29b1-154">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="f29b1-155">L'elemento di ancoraggio precedente è un [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="f29b1-155">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f29b1-156">In questo caso, si tratta dell'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f29b1-156">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="f29b1-157">L'attributo e il valore dell'helper tag `asp-page="/Movies/Index"` creano un collegamento alla pagina Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-157">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="f29b1-158">Salvare le modifiche e testare l'app selezionando il collegamento **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="f29b1-158">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="f29b1-159">Visualizzare il file [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) in GitHub.</span><span class="sxs-lookup"><span data-stu-id="f29b1-159">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="f29b1-160">Pagina di creazione code-behind</span><span class="sxs-lookup"><span data-stu-id="f29b1-160">The Create code-behind page</span></span>

<span data-ttu-id="f29b1-161">Esaminare il file code-behind *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f29b1-161">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="f29b1-162">Il metodo `OnGet` inizializza lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="f29b1-162">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="f29b1-163">La pagina di creazione non possiede uno stato da inizializzare.</span><span class="sxs-lookup"><span data-stu-id="f29b1-163">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="f29b1-164">Il metodo `Page` crea un oggetto `PageResult` che esegue il rendering della pagina *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f29b1-164">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="f29b1-165">La proprietà `Movie` usa l'attributo `[BindProperty]` per consentire l'[associazione di modelli](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="f29b1-165">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="f29b1-166">Dopo che il modulo di creazione registra i valori, il runtime di ASP.NET Core associa i valori registrati al modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-166">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="f29b1-167">Il metodo `OnPostAsync` viene eseguito quando la pagina inserisce i dati del modulo:</span><span class="sxs-lookup"><span data-stu-id="f29b1-167">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="f29b1-168">Se il modello contiene errori, il modulo viene nuovamente visualizzato insieme ai dati del modulo inviati.</span><span class="sxs-lookup"><span data-stu-id="f29b1-168">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="f29b1-169">La maggior parte degli errori del modello sono riscontrabili sul lato client prima che il modulo sia inviato.</span><span class="sxs-lookup"><span data-stu-id="f29b1-169">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="f29b1-170">La registrazione di un valore per il campo data non convertibile in una data è un esempio di errore del modello.</span><span class="sxs-lookup"><span data-stu-id="f29b1-170">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="f29b1-171">Durante l'esercitazione saranno poi trattate nel dettaglio la convalida lato client e la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="f29b1-171">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="f29b1-172">Se non sono presenti errori nel modello, i dati vengono salvati e il browser viene reindirizzato alla pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="f29b1-172">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="f29b1-173">Pagina di creazione di Razor</span><span class="sxs-lookup"><span data-stu-id="f29b1-173">The Create Razor Page</span></span>

<span data-ttu-id="f29b1-174">Esaminare il file della pagina Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f29b1-174">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="f29b1-175">Per visualizzare il tag `<form method="post">`, Visual Studio usa un tipo di carattere distintivo specifico per gli helper tag.</span><span class="sxs-lookup"><span data-stu-id="f29b1-175">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="f29b1-176">L'elemento `<form method="post">` è un [helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f29b1-176">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="f29b1-177">L'helper tag del modulo include automaticamente un [token antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="f29b1-177">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Visualizzazione di VS17 della pagina Create.cshtml](page/_static/th.png)

<span data-ttu-id="f29b1-179">Il motore di scaffolding crea un markup Razor per ogni campo nel modello (tranne l'ID) simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f29b1-179">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="f29b1-180">Gli [helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` e ` <span asp-validation-for`) visualizzano gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="f29b1-180">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="f29b1-181">Il tema della convalida viene trattato in modo più dettagliato successivamente in questa serie.</span><span class="sxs-lookup"><span data-stu-id="f29b1-181">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="f29b1-182">L'[helper tag di etichetta](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera la didascalia dell'etichetta e l'attributo `for` per la proprietà `Title`.</span><span class="sxs-lookup"><span data-stu-id="f29b1-182">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="f29b1-183">L'[helper tag di input](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa gli attributi [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f29b1-183">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="f29b1-184">L'esercitazione successiva illustra SQL Server LocalDB e il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="f29b1-184">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f29b1-185">[Previous: Adding a model](xref:tutorials/razor-pages/model) (Precedente: Aggiunta di un modello)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql) (Successivo: SQL Server LocalDB)</span><span class="sxs-lookup"><span data-stu-id="f29b1-185">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
