---
title: Caricamento di file in ASP.NET Core
author: ardalis
description: Come utilizzare l'associazione del modello e lo streaming per caricare i file in ASP.NET MVC di base.
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: bc1cfe0d6ee88a0af49cdff9ce77ad42f57b95f7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="9d977-103">Caricamento di file in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d977-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="9d977-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9d977-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9d977-105">Azioni di ASP.NET MVC supportano il caricamento di uno o più file utilizzando un modello semplice di associazione per i file più piccoli o di flusso per i file di dimensioni maggiori.</span><span class="sxs-lookup"><span data-stu-id="9d977-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="9d977-106">Consente di visualizzare o scaricare l'esempio da GitHub</span><span class="sxs-lookup"><span data-stu-id="9d977-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="9d977-107">Il caricamento di file di piccole dimensioni con l'associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="9d977-107">Uploading small files with model binding</span></span>

<span data-ttu-id="9d977-108">Per caricare file di piccole dimensioni, è possibile utilizzare un modulo HTML più parte o creare una richiesta POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9d977-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="9d977-109">Un modulo di esempio utilizzando Razor, che supporta più i file caricati, è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9d977-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="9d977-110">Per supportare il caricamento di file, è necessario specificare moduli HTML un `enctype` di `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="9d977-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="9d977-111">Il `files` elemento input illustrato in precedenza supporta il caricamento di più file.</span><span class="sxs-lookup"><span data-stu-id="9d977-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="9d977-112">Omettere il `multiple` attributo per questo elemento di input per consentire solo un singolo file da caricare.</span><span class="sxs-lookup"><span data-stu-id="9d977-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="9d977-113">Il codice precedente viene eseguito il rendering in un browser come:</span><span class="sxs-lookup"><span data-stu-id="9d977-113">The above markup renders in a browser as:</span></span>

![Modulo di caricamento file](file-uploads/_static/upload-form.png)

<span data-ttu-id="9d977-115">I singoli file caricati nel server sono accessibili tramite [associazione di modelli](xref:mvc/models/model-binding) utilizzando il [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfaccia.</span><span class="sxs-lookup"><span data-stu-id="9d977-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="9d977-116">`IFormFile`presenta la struttura seguente:</span><span class="sxs-lookup"><span data-stu-id="9d977-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="9d977-117">Non si basano su o considerare attendibile il `FileName` proprietà senza convalida.</span><span class="sxs-lookup"><span data-stu-id="9d977-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="9d977-118">Il `FileName` proprietà deve essere utilizzata solo per scopi di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9d977-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="9d977-119">Durante il caricamento di file utilizzando l'associazione di modelli e `IFormFile` interfaccia, il metodo di azione può accettare un singolo `IFormFile` o `IEnumerable<IFormFile>` (o `List<IFormFile>`) che rappresenta i diversi file.</span><span class="sxs-lookup"><span data-stu-id="9d977-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="9d977-120">Nell'esempio seguente esegue il ciclo di uno o più file caricati, li salva nel file system locale e restituisce il numero totale e le dimensioni dei file caricati.</span><span class="sxs-lookup"><span data-stu-id="9d977-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="9d977-121">I file caricati utilizzando il `IFormFile` tecnica vengono memorizzati nel buffer in memoria o su disco nel server web prima di essere elaborato.</span><span class="sxs-lookup"><span data-stu-id="9d977-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="9d977-122">All'interno del metodo di azione, il `IFormFile` contenuto è accessibile come flusso.</span><span class="sxs-lookup"><span data-stu-id="9d977-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="9d977-123">Oltre ai file system locale, file possono essere trasmesso a [archiviazione Blob di Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="9d977-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="9d977-124">Per archiviare i dati di file binario in un database tramite Entity Framework, definire una proprietà di tipo `byte[]` nell'entità:</span><span class="sxs-lookup"><span data-stu-id="9d977-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="9d977-125">Specificare una proprietà viewmodel di tipo `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="9d977-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="9d977-126">`IFormFile`può essere utilizzato direttamente come un parametro di metodo di azione o una proprietà viewmodel, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9d977-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="9d977-127">Copia il `IFormFile` in un flusso e salvarlo nella matrice di byte:</span><span class="sxs-lookup"><span data-stu-id="9d977-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="9d977-128">Prestare attenzione quando si archiviano dati binari in database relazionali, come può influire negativamente sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="9d977-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="9d977-129">Caricamento di file di grandi dimensioni con il flusso</span><span class="sxs-lookup"><span data-stu-id="9d977-129">Uploading large files with streaming</span></span>

<span data-ttu-id="9d977-130">Se le dimensioni o la frequenza di caricamento file provoca problemi di risorse per l'app, si consideri il caricamento del file di flusso, anziché memorizzarlo nella sua interezza, come illustrato in precedenza l'approccio di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="9d977-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="9d977-131">Quando si utilizza `IFormFile` e associazione del modello è una quantità soluzione più semplice, streaming richiede un numero di passaggi per implementare correttamente.</span><span class="sxs-lookup"><span data-stu-id="9d977-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="9d977-132">Qualsiasi singolo file memorizzato nel buffer superiore a 64KB verrà spostati dalla memoria in un file temporaneo su disco nel server.</span><span class="sxs-lookup"><span data-stu-id="9d977-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="9d977-133">Le risorse (disco, RAM) utilizzate dal caricamento di file dipendono dal numero e dimensione di caricamento di file simultanee.</span><span class="sxs-lookup"><span data-stu-id="9d977-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="9d977-134">Questo non è tanto sulle prestazioni, sulla scala.</span><span class="sxs-lookup"><span data-stu-id="9d977-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="9d977-135">Se si tenta di caricamento di un numero eccessivo di buffer, del sito verrà arrestato in modo anomalo quando si esaurisce lo spazio di memoria o disco.</span><span class="sxs-lookup"><span data-stu-id="9d977-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="9d977-136">Nell'esempio seguente viene illustrato l'utilizzo di JavaScript/angolare per lo streaming a un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="9d977-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="9d977-137">Token non riproducibili del file viene generato utilizzando un attributo di filtro personalizzato e passato nelle intestazioni HTTP anziché nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9d977-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="9d977-138">Poiché il metodo di azione elabora i dati caricati direttamente, l'associazione di modelli è disabilitata per un altro filtro.</span><span class="sxs-lookup"><span data-stu-id="9d977-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="9d977-139">All'interno dell'azione, il contenuto del form viene letti utilizzando un `MultipartReader`, che legge ogni singolo `MultipartSection`, l'elaborazione del file o di archiviare il contenuto come appropriato.</span><span class="sxs-lookup"><span data-stu-id="9d977-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="9d977-140">Dopo tutte le sezioni sono stati letti, l'azione esegue il proprio associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="9d977-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="9d977-141">L'azione iniziale carica il form e consente di salvare un token non riproducibili in un cookie (tramite il `GenerateAntiforgeryTokenCookieForAjax` attributo):</span><span class="sxs-lookup"><span data-stu-id="9d977-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="9d977-142">L'attributo utilizza predefinito di ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) il supporto per impostare un cookie con un token di richiesta:</span><span class="sxs-lookup"><span data-stu-id="9d977-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="9d977-143">Angolare passa automaticamente un token non riproducibili in un'intestazione di richiesta denominata `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="9d977-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="9d977-144">L'app ASP.NET MVC di base è configurato per fare riferimento a questa intestazione nella sua configurazione *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9d977-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="9d977-145">Il `DisableFormValueModelBinding` attributo, illustrato di seguito, viene utilizzato per l'associazione di modelli per disabilitare il `Upload` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="9d977-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="9d977-146">Poiché l'associazione del modello è disattivato, la `Upload` metodo di azione non accetta parametri.</span><span class="sxs-lookup"><span data-stu-id="9d977-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="9d977-147">Interagisce direttamente con il `Request` proprietà `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="9d977-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="9d977-148">Oggetto `MultipartReader` viene utilizzato per leggere ogni sezione.</span><span class="sxs-lookup"><span data-stu-id="9d977-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="9d977-149">Il file viene salvato con un nome di file GUID e i dati di chiave/valore vengono archiviati un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="9d977-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="9d977-150">Una volta tutte le sezioni sono stati letti, il contenuto del `KeyValueAccumulator` vengono utilizzati per associare i dati del form a un tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="9d977-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="9d977-151">L'intero `Upload` metodo è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9d977-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="9d977-152">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="9d977-152">Troubleshooting</span></span>

<span data-ttu-id="9d977-153">Ecco alcuni problemi comuni riscontrati quando si lavora con il caricamento di file e le relative soluzioni possibili.</span><span class="sxs-lookup"><span data-stu-id="9d977-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="9d977-154">Errore imprevisto non trovato con IIS</span><span class="sxs-lookup"><span data-stu-id="9d977-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="9d977-155">L'errore seguente indica il caricamento del file supera il server configurato `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="9d977-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="9d977-156">L'impostazione predefinita è `30000000`, ovvero circa 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="9d977-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="9d977-157">Il valore può essere personalizzato modificando *Web. config*:</span><span class="sxs-lookup"><span data-stu-id="9d977-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="9d977-158">Questa impostazione si applica solo a IIS.</span><span class="sxs-lookup"><span data-stu-id="9d977-158">This setting only applies to IIS.</span></span> <span data-ttu-id="9d977-159">Il comportamento non si verifica per impostazione predefinita, quando si ospita sul Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9d977-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="9d977-160">Per ulteriori informazioni, vedere [limiti richiesta \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="9d977-160">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="9d977-161">Eccezione di riferimento null con IFormFile</span><span class="sxs-lookup"><span data-stu-id="9d977-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="9d977-162">Se il controller è accettazione caricati i file usando `IFormFile` ma trovare che il valore è sempre null, verificare che il form HTML consiste nello specificare un `enctype` valore `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="9d977-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="9d977-163">Se questo attributo non è impostato sul `<form>` elemento, non si verifica il caricamento del file e qualsiasi associazione `IFormFile` argomenti è null.</span><span class="sxs-lookup"><span data-stu-id="9d977-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
