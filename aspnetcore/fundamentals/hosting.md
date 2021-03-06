---
title: Hosting in ASP.NET Core
author: guardrex
description: Informazioni sull'host Web in ASP.NET Core, responsabile della gestione dell'avvio e della durata delle app.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 78209c8d34fa1a2a164ae333d625feca1e145e89
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="hosting-in-aspnet-core"></a>Hosting in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Le app ASP.NET Core configurano e avviano un *host*. L'host è responsabile della gestione dell'avvio e della durata delle app. L'host configura almeno un server e una pipeline di elaborazione delle richieste.

## <a name="setting-up-a-host"></a>Impostazione di un host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Questa operazione viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`. Nei modelli di progetto `Main` si trova in *Program.cs*. Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder` esegue le attività seguenti:

* Configura [Kestrel](servers/kestrel.md) come server Web. Per le opzioni predefinite Kestrel, vedere la [sezione Opzioni Kestrel in Introduzione all'implementazione di server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).
* Imposta la radice del contenuto sul percorso restituito da [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Carica le configurazioni facoltative da:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Segreti dell'utente](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development`.
  * Variabili di ambiente.
  * Argomenti della riga di comando.
* Configura la [registrazione](xref:fundamentals/logging/index) per l'output della console e del debug. La registrazione include le regole di [filtro dei log](xref:fundamentals/logging/index#log-filtering) specificate nella sezione di configurazione della registrazione di un file *appsettings.json* o *appsettings.{Environment}.json*.
* Se eseguito in IIS, abilita l'[integrazione di IIS](xref:host-and-deploy/iis/index). Configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Il modulo crea un proxy inverso tra IIS e Kestrel. Configura inoltre l'app per l'[acquisizione degli errori di avvio](#capture-startup-errors). Per le opzioni predefinite IIS, vedere la [sezione Opzioni IIS in Host ASP.NET Core in Windows con IIS](xref:host-and-deploy/iis/index#iis-options).

La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC. Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto. Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).

Per altre informazioni sulla configurazione delle app, vedere [Configurazione in ASP.NET Core](xref:fundamentals/configuration/index).

> [!NOTE]
> In alternativa all'uso del metodo `CreateDefaultBuilder` statico, un approccio supportato in ASP.NET Core 2.x consiste nel creare un host da [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). Per altre informazioni, vedere la scheda ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Creare un host usando un'istanza di [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder). La creazione di un host viene in genere eseguita nel punto di ingresso dell'app, il metodo `Main`. Nei modelli di progetto `Main` si trova in *Program.cs*:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder` richiede un [server che implementa IServer](servers/index.md). I server incorporati sono [Kestrel](servers/kestrel.md) e [HTTP.sys](servers/httpsys.md) (prima della versione ASP.NET Core 2.0, HTTP.sys era chiamato [WebListener](xref:fundamentals/servers/weblistener)). In questo esempio, il [metodo di estensione UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifica il server Kestrel.

La *radice del contenuto* determina la posizione in cui l'host cerca i file dei contenuti, ad esempio i file di visualizzazione MVC. La radice di contenuto predefinita viene ottenuta per `UseContentRoot` tramite [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1). Quando l'app viene avviata dalla cartella radice del progetto, la cartella radice del progetto viene usata come radice del contenuto. Si tratta dell'impostazione predefinita usata in [Visual Studio](https://www.visualstudio.com/) e nei [nuovi modelli dotnet](/dotnet/core/tools/dotnet-new).

Per usare IIS come proxy inverso, chiamare [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) come parte della compilazione dell'host. A differenza di [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1), `UseIISIntegration` non configura un *server*. `UseIISIntegration` configura il percorso di base e la porta su cui il server esegue l'ascolto quando viene usato il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per creare un proxy inverso tra Kestrel e IIS. Per usare IIS con ASP.NET Core, è necessario specificare `UseKestrel` e `UseIISIntegration`. `UseIISIntegration` viene attivato solo quando si esegue IIS o IIS Express. Per altre informazioni, vedere [Introduzione al modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

Un'implementazione minima che configura un host (e un'app ASP.NET Core) include la specifica di un server e la configurazione della pipeline delle richieste dell'app:

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

Quando si configura un host, è possibile specificare i metodi [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) e [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1). Se viene specificata una classe `Startup`, la classe deve definire un metodo `Configure`. Per altre informazioni, vedere [Avvio dell'applicazione in ASP.NET Core](startup.md). Se vengono effettuate più chiamate a `ConfigureServices`, le chiamate vengono aggiunte l'una all'altra. Più chiamate a `Configure` o `UseStartup` in `WebHostBuilder` sostituiscono le impostazioni precedenti.

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) si basa sugli approcci seguenti per impostare i valori di configurazione dell'host:

* Configurazione del generatore di host, che include le variabili di ambiente nel formato `ASPNETCORE_{configurationKey}`. Ad esempio `ASPNETCORE_URLS`.
* Metodi espliciti, ad esempio `CaptureStartupErrors`.
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) e chiave associata. Quando si imposta un valore con `UseSetting`, il valore viene impostato come stringa indipendentemente dal tipo.

L'host usa l'ultima opzione che imposta un valore. Per altre informazioni, vedere [Override della configurazione](#overriding-configuration) nella sezione successiva.

### <a name="capture-startup-errors"></a>Acquisizione degli errori di avvio

Questa impostazione controlla l'acquisizione degli errori di avvio.

**Chiave**: captureStartupErrors  
**Tipo**: *bool* (`true` o `1`)  
**Impostazione predefinita**: il valore predefinito è `false` a meno che l'app non venga eseguita con Kestrel in IIS, in tal caso il valore predefinito è `true`.  
**Impostare usando**: `CaptureStartupErrors`  
**Variabile di ambiente**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Quando è `false`, gli errori durante l'avvio causano la chiusura dell'host. Quando è `true`, l'host acquisisce le eccezioni durante l'avvio e tenta di avviare il server.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>Radice del contenuto

Questa impostazione determina la posizione da cui ASP.NET Core inizia la ricerca dei file di contenuto, ad esempio delle visualizzazioni MVC. 

**Chiave**: contentRoot  
**Tipo**: *string*  
**Impostazione predefinita**: il valore predefinito corrisponde alla cartella contenente l'assembly dell'app.  
**Impostare usando**: `UseContentRoot`  
**Variabile di ambiente**: `ASPNETCORE_CONTENTROOT`

La radice del contenuto viene usata anche come percorso di base per l'[impostazione della radice Web](#web-root). Se il percorso non esiste, l'host non verrà avviato.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>Errori dettagliati

Determina l'acquisizione degli errori dettagliati.

**Chiave**: detailedErrors  
**Tipo**: *bool* (`true` o `1`)  
**Impostazione predefinita**: false  
**Impostare usando**: `UseSetting`  
**Variabile di ambiente**: `ASPNETCORE_DETAILEDERRORS`

Quando è abilitata (o quando l'<a href="#environment">ambiente</a> è impostato su `Development`), l'app acquisisce le eccezioni dettagliate.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>Ambiente

Imposta l'ambiente dell'app.

**Chiave**: environment  
**Tipo**: *string*  
**Impostazione predefinita**: Production  
**Impostare usando**: `UseEnvironment`  
**Variabile di ambiente**: `ASPNETCORE_ENVIRONMENT`

L'ambiente può essere impostato su qualsiasi valore. I valori definiti dal framework includono `Development`, `Staging` e `Production`. Nei valori non viene fatta distinzione tra maiuscole e minuscole. Per impostazione predefinita, l'*ambiente* viene letto dalla variabile di ambiente `ASPNETCORE_ENVIRONMENT`. Quando si usa [Visual Studio](https://www.visualstudio.com/), le variabili di ambiente possono essere impostate nel file *launchSettings.json*. Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>Assembly di avvio dell'hosting

Imposta gli assembly di avvio dell'hosting dell'app.

**Chiave**: hostingStartupAssemblies  
**Tipo**: *string*  
**Impostazione predefinita**: stringa vuota  
**Impostare usando**: `UseSetting`  
**Variabile di ambiente**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Una stringa delimitata da punto e virgola di assembly di avvio dell'hosting da caricare all'avvio. Questa funzionalità è stata introdotta in ASP.NET Core 2.0.

Sebbene il valore di configurazione predefinito sia una stringa vuota, gli assembly di avvio dell'hosting includono sempre l'assembly dell'app. Quando vengono specificati, gli assembly di avvio dell'hosting vengono aggiunti all'assembly dell'app per essere caricati quando l'app compila i servizi comuni durante l'avvio.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità non è disponibile in ASP.NET Core 1.x.

---

### <a name="prefer-hosting-urls"></a>Preferire gli URL di hosting

Indica se l'host deve eseguire l'ascolto sugli URL configurati con `WebHostBuilder` anziché su quelli configurati con l'implementazione `IServer`.

**Chiave**: preferHostingUrls  
**Tipo**: *bool* (`true` o `1`)  
**Impostazione predefinita**: true  
**Impostare usando**: `PreferHostingUrls`  
**Variabile di ambiente**: `ASPNETCORE_PREFERHOSTINGURLS`

Questa funzionalità è stata introdotta in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità non è disponibile in ASP.NET Core 1.x.

---

### <a name="prevent-hosting-startup"></a>Impedire l'avvio dell'hosting

Impedisce il caricamento automatico degli assembly di avvio dell'hosting, inclusi gli assembly di avvio dell'hosting configurati dall'assembly dell'app. Per altre informazioni, vedere [Aggiungere le funzionalità dell'app da un assembly esterno utilizzando IHostingStartup](xref:host-and-deploy/ihostingstartup).

**Chiave**: preventHostingStartup  
**Tipo**: *bool* (`true` o `1`)  
**Impostazione predefinita**: false  
**Impostare usando**: `UseSetting`  
**Variabile di ambiente**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

Questa funzionalità è stata introdotta in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità non è disponibile in ASP.NET Core 1.x.

---

### <a name="server-urls"></a>URL del server

Indica gli indirizzi IP o gli indirizzi host con le porte e protocolli su cui il server deve eseguire l'ascolto per le richieste.

**Chiave**: urls  
**Tipo**: *string*  
**Impostazione predefinita**: http://localhost:5000  
**Impostare usando**: `UseUrls`  
**Variabile di ambiente**: `ASPNETCORE_URLS`

Impostare su un elenco di prefissi URL separati da punto e virgola (;) ai quali il server deve rispondere. Ad esempio `http://localhost:123`. Usare "\*" per indicare che il server deve eseguire l'ascolto per le richieste su tutti gli indirizzi IP o nomi host usando la porta e il protocollo specificati (ad esempio, `http://*:5000`). Il protocollo (`http://` o `https://`) deve essere incluso con ogni URL. I formati supportati variano a seconda del server.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel ha una propria API di configurazione degli endpoint. Per altre informazioni, vedere l'introduzione all'[implementazione del server Web Kestrel in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>Timeout di arresto

Specifica il tempo di attesa per l'arresto dell'host Web.

**Chiave**: shutdownTimeoutSeconds  
**Tipo**: *int*  
**Impostazione predefinita**: 5  
**Impostare usando**: `UseShutdownTimeout`  
**Variabile di ambiente**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Sebbene la chiave accetti *int* con `UseSetting` (ad esempio, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), il metodo di estensione `UseShutdownTimeout` accetta `TimeSpan`. Questa funzionalità è stata introdotta in ASP.NET Core 2.0.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Questa funzionalità non è disponibile in ASP.NET Core 1.x.

---

### <a name="startup-assembly"></a>Assembly di avvio

Determina l'assembly per la ricerca della classe `Startup`.

**Chiave**: startupAssembly  
**Tipo**: *string*  
**Impostazione predefinita**: assembly dell'app  
**Impostare usando**: `UseStartup`  
**Variabile di ambiente**: `ASPNETCORE_STARTUPASSEMBLY`

È possibile fare riferimento all'assembly per nome (`string`) o per tipo (`TStartup`). Se vengono chiamati più metodi `UseStartup`, l'ultimo metodo ha la precedenza.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Radice Web

Imposta il percorso relativo degli asset statici dell'app.

**Chiave**: webroot  
**Tipo**: *string*  
**Impostazione predefinita**: se non è specificato, il valore predefinito è "(Content Root)/wwwroot", se il percorso esiste. Se il percorso non esiste, viene usato un provider di file no-op.  
**Impostare usando**: `UseWebRoot`  
**Variabile di ambiente**: `ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>Override della configurazione

Usare [Configuration](xref:fundamentals/configuration/index) per configurare l'host. Nell'esempio seguente la configurazione dell'host è specificata facoltativamente in un file *hosting.json*. Qualsiasi configurazione caricata dal file *hosting.json* può essere sostituita dagli argomenti della riga di comando. La configurazione compilata (in `config`) viene usata per configurare l'host con `UseConfiguration`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

Override della configurazione specificata da `UseUrls` con la configurazione *hosting.json*, quindi con la configurazione degli argomenti della riga di comando:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> Il metodo di estensione [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) non è attualmente in grado di analizzare una sezione di configurazione restituita da `GetSection` (ad esempio, `.UseConfiguration(Configuration.GetSection("section"))`. Il metodo `GetSection` filtra le chiavi di configurazione per la sezione richiesta, ma lascia il nome di sezione sulle chiavi (ad esempio, `section:urls`, `section:environment`). Il metodo `UseConfiguration` prevede che le chiavi corrispondano alle chiavi `WebHostBuilder` (ad esempio, `urls`, `environment`). La presenza del nome di sezione sulle chiavi impedisce ai valori della sezione di configurare l'host. Questo problema verrà risolto in una delle prossime versioni. Per altre informazioni e soluzioni alternative, vedere [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).

Per specificare l'host eseguito in un URL specifico, il valore desiderato può essere passato da un prompt dei comandi durante l'esecuzione di `dotnet run`. L'argomento della riga di comando esegue l'override del valore `urls`dal file *hosting.json*, mentre il server esegue l'ascolto sulla porta 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>Avvio dell'host

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**Run**

Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:

```csharp
host.Run();
```

**Start**

Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

L'app può inizializzare e avviare un nuovo host usando i valori predefiniti preconfigurati di `CreateDefaultBuilder` con un metodo pratico statico. Questi metodi avviano il server senza l'output della console e con l'attesa [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) di un'interruzione (Ctrl-C/SIGINT o SIGTERM):

**Start(RequestDelegate app)**

Start con `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!" `WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.

**Start(string url, RequestDelegate app)**

Start con un URL e `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato di **Start(app RequestDelegate)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.

**Start(Action<IRouteBuilder> routeBuilder)**

Usare un'istanza di `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) per usare il middleware di routing:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Usare le richieste del browser seguenti con l'esempio:

| Richiesta                                    | Risposta                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | Genera un'eccezione con la stringa "ooops!" |
| `http://localhost:5000/throw`              | Genera un'eccezione con la stringa "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.

**Start(string url, Action<IRouteBuilder> routeBuilder)**

Usare un URL e un'istanza di `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato di **Start(Action<IRouteBuilder> routeBuilder)**, ad eccezione del fatto che l'app risponde in `http://localhost:8080`.

**StartWith(Action<IApplicationBuilder> app)**

Specificare un delegato per configurare `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Creare una richiesta nel browser a `http://localhost:5000` per ricevere la risposta "Hello World!" `WaitForShutdown` rimane bloccato fino a quando non viene eseguita un'interruzione (Ctrl-C/SIGINT o SIGTERM). L'app visualizza il messaggio `Console.WriteLine` e attende la pressione di un tasto per chiudersi.

**StartWith(string url, Action<IApplicationBuilder> app)**

Specificare un URL e un delegato per configurare `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Produce lo stesso risultato di **StartWith(Action<IApplicationBuilder> app)**, ad eccezione del fatto che l'app risponde su `http://localhost:8080`.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**Run**

Il metodo `Run` avvia l'app Web e blocca il thread di chiamata fino all'arresto dell'host:

```csharp
host.Run();
```

**Start**

Eseguire l'host in modo non bloccante chiamandone il metodo `Start`:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

Se viene passato un elenco di URL al metodo `Start`, viene eseguito l'ascolto sugli URL specificati:


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a>Interfaccia IHostingEnvironment

L'[interfaccia IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fornisce informazioni sull'ambiente di hosting Web dell'app. Usare l'[inserimento di un costruttore](xref:fundamentals/dependency-injection) per ottenere `IHostingEnvironment` per poterne usare le proprietà e i metodi di estensione:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

È possibile usare un [approccio basato su convenzione](xref:fundamentals/environments#startup-conventions) per configurare l'app all'avvio in base all'ambiente. In alternativa, inserire `IHostingEnvironment` nel costruttore `Startup` per l'utilizzo in `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Oltre al metodo di estensione `IsDevelopment`, `IHostingEnvironment` offre i metodi `IsStaging`, `IsProduction` e `IsEnvironment(string environmentName)`. Per informazioni dettagliate, vedere [Uso di più ambienti](xref:fundamentals/environments).

Il servizio `IHostingEnvironment` può anche essere inserito direttamente nel metodo `Configure` per configurare la pipeline di elaborazione:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` può essere inserito nel metodo `Invoke` durante la creazione di [middleware](xref:fundamentals/middleware#writing-middleware) personalizzato:

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>Interfaccia IApplicationLifetime

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) consente attività post-avvio e di arresto. Tre proprietà nell'interfaccia sono token di annullamento usati per registrare i metodi `Action` che definiscono gli eventi di avvio e arresto. È presente anche un metodo `StopApplication`.

| Token di annullamento    | Attivato quando&#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | L'host è stato completamente avviato. |
| `ApplicationStopping` | L'host sta eseguendo un arresto normale. È possibile che le richieste siano ancora in esecuzione. L'arresto è bloccato fino al completamento di questo evento. |
| `ApplicationStopped`  | L'host sta completando un arresto normale. Tutte le richieste devono essere elaborate. L'arresto è bloccato fino al completamento di questo evento. |

| Metodo            | Operazione                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | Richiede la chiusura dell'applicazione corrente. |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a>Troubleshooting System.ArgumentException

**Si applica solo ad ASP.NET Core 2.0**

È possibile creare un host inserendo `IStartup` direttamente nel contenitore di inserimento delle dipendenze anziché chiamando `UseStartup` o `Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

Se l'host viene creato in questo modo, è possibile che si verifichi l'errore seguente:

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

Ciò si verifica perché è necessario [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (assembly corrente) per cercare `HostingStartupAttributes`. Se l'app inserisce manualmente `IStartup` nel contenitore di inserimento delle dipendenze, aggiungere la chiamata seguente a `WebHostBuilder` con il nome dell'assembly specificato:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

In alternativa, aggiungere `Configure` fittizio a `WebHostBuilder`, che imposta `applicationName`(`ApplicationKey`) automaticamente:

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**NOTA**: quest'operazione è necessaria solo con la versione ASP.NET Core 2.0 e solo quando l'app non esegue la chiamata a `UseStartup` o `Configure`.

Per altre informazioni, vedere [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (Annunci: Microsoft.Extensions.PlatformAbstractions è stato rimosso (commento)) e [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs) (Esempio di StartupInjection).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Host in Windows con IIS](xref:host-and-deploy/iis/index)
* [Host in Linux con Nginx](xref:host-and-deploy/linux-nginx)
* [Host in Linux con Apache](xref:host-and-deploy/linux-apache)
* [Host in un servizio di Windows](xref:host-and-deploy/windows-service)
