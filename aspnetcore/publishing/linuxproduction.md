---
title: Hosting di ASP.NET Core in Linux con Nginx
description: Viene descritto come configurare Nginx come proxy inverso in Ubuntu 16.04 per inoltrare il traffico HTTP a un'applicazione Web ASP.NET Core in esecuzione su Kestrel.
keywords: ASP.NET Core, Linux, nginx, Ubuntu, proxy inverso
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="d83a2-104">Configurare un ambiente di hosting per ASP.NET Core in Linux con Nginx e distribuirlo</span><span class="sxs-lookup"><span data-stu-id="d83a2-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="d83a2-105">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="d83a2-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="d83a2-106">Questa guida spiega come configurare un ambiente ASP.NET Core pronto per la produzione in un server Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="d83a2-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="d83a2-107">**Nota:** per Ubuntu 14.04, è consigliabile supervisord come soluzione per il monitoraggio del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d83a2-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="d83a2-108">systemd non è disponibile in Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="d83a2-108">systemd is not available on Ubuntu 14.04.</span></span> [<span data-ttu-id="d83a2-109">Vedere la versione precedente di questo documento</span><span class="sxs-lookup"><span data-stu-id="d83a2-109">See previous version of this document</span></span>](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

<span data-ttu-id="d83a2-110">In questa guida:</span><span class="sxs-lookup"><span data-stu-id="d83a2-110">This guide:</span></span>

* <span data-ttu-id="d83a2-111">Posizionare un'applicazione ASP.NET Core esistente dietro un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="d83a2-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="d83a2-112">Configurare il server proxy inverso per l'inoltro delle richieste al server Web Kestrel</span><span class="sxs-lookup"><span data-stu-id="d83a2-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="d83a2-113">Verificare che l'applicazione Web venga eseguita all'avvio come daemon</span><span class="sxs-lookup"><span data-stu-id="d83a2-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="d83a2-114">Configurare uno strumento di gestione del processo per consentire il riavvio dell'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="d83a2-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d83a2-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d83a2-115">Prerequisites</span></span>

1. <span data-ttu-id="d83a2-116">Accesso a un server Ubuntu 16.04 con un account utente standard con privilegio sudo</span><span class="sxs-lookup"><span data-stu-id="d83a2-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="d83a2-117">Un'applicazione ASP.NET Core esistente</span><span class="sxs-lookup"><span data-stu-id="d83a2-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="d83a2-118">Copiare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="d83a2-118">Copy over your app</span></span>

<span data-ttu-id="d83a2-119">Eseguire `dotnet publish` dall'ambiente di sviluppo per creare un pacchetto per un'applicazione in una directory indipendente che può essere eseguita sul server.</span><span class="sxs-lookup"><span data-stu-id="d83a2-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="d83a2-120">Copiare l'applicazione ASP.NET Core sul server usando qualsiasi strumento (SCP, FTP e così via) che si possa integrare nel flusso di lavoro.</span><span class="sxs-lookup"><span data-stu-id="d83a2-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="d83a2-121">Testare l'applicazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d83a2-121">Test the app, for example:</span></span>
 - <span data-ttu-id="d83a2-122">Eseguire `dotnet yourapp.dll` dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="d83a2-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="d83a2-123">In un browser passare a `http://<serveraddress>:<port>` per verificare se l'applicazione funziona in Linux.</span><span class="sxs-lookup"><span data-stu-id="d83a2-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="d83a2-124">Configurare un server proxy inverso</span><span class="sxs-lookup"><span data-stu-id="d83a2-124">Configure a reverse proxy server</span></span>

<span data-ttu-id="d83a2-125">Un proxy inverso è una configurazione comune per la gestione delle applicazioni Web dinamiche.</span><span class="sxs-lookup"><span data-stu-id="d83a2-125">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="d83a2-126">Un proxy inverso termina la richiesta HTTP e la inoltra all'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d83a2-126">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="d83a2-127">Perché usare un server proxy inverso?</span><span class="sxs-lookup"><span data-stu-id="d83a2-127">Why use a reverse proxy server?</span></span>

<span data-ttu-id="d83a2-128">Kestrel funziona perfettamente per la gestione del contenuto dinamico da ASP.NET Core, tuttavia le parti relative al Web non sono così ricche di funzionalità come i server IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-128">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="d83a2-129">Un server proxy inverso è in grado di eseguire l'offload di attività come la gestione del contenuto statico, le richieste di memorizzazione nella cache, la compressione delle richieste e la terminazione SSL dal server HTTP.</span><span class="sxs-lookup"><span data-stu-id="d83a2-129">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="d83a2-130">Un server proxy inverso può risiedere in un computer dedicato o essere distribuito insieme a un server HTTP.</span><span class="sxs-lookup"><span data-stu-id="d83a2-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="d83a2-131">Ai fini di questa guida viene usata una singola istanza di Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="d83a2-132">Viene eseguito sullo stesso server, insieme al server HTTP.</span><span class="sxs-lookup"><span data-stu-id="d83a2-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="d83a2-133">In base alle proprie esigenze, è possibile scegliere un'impostazione diversa.</span><span class="sxs-lookup"><span data-stu-id="d83a2-133">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="d83a2-134">Poiché le richieste vengono inoltrate dal proxy inverso, usare il middleware `ForwardedHeaders` dal pacchetto `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="d83a2-135">Questo middleware aggiorna `Request.Scheme` usando l'intestazione `X-Forwarded-Proto`, in modo che gli URI di reindirizzamento e altri criteri di sicurezza funzionino correttamente.</span><span class="sxs-lookup"><span data-stu-id="d83a2-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d83a2-136">Quando si configura un server proxy inverso, il middleware di autenticazione richiede `UseForwardedHeaders` per essere eseguito per primo.</span><span class="sxs-lookup"><span data-stu-id="d83a2-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="d83a2-137">Questo ordine garantisce che il middleware di autenticazione sia in grado di usare i valori interessati e di generare gli URI di reindirizzamento corretti.</span><span class="sxs-lookup"><span data-stu-id="d83a2-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d83a2-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d83a2-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d83a2-139">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="d83a2-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d83a2-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d83a2-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d83a2-141">Richiamare il metodo `UseForwardedHeaders` (nel metodo `Configure` di *Startup.cs*) prima di chiamare `UseIdentity` e `UseFacebookAuthentication` o un middleware con schema di autenticazione simile:</span><span class="sxs-lookup"><span data-stu-id="d83a2-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a><span data-ttu-id="d83a2-142">Installare Nginx</span><span class="sxs-lookup"><span data-stu-id="d83a2-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="d83a2-143">Se si prevede di installare i moduli Nginx facoltativi, può essere necessario compilare Nginx dall'origine.</span><span class="sxs-lookup"><span data-stu-id="d83a2-143">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="d83a2-144">Usare `apt-get` per installare Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="d83a2-145">Il programma di installazione crea uno script di inizializzazione System V che esegue Nginx come daemon all'avvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="d83a2-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="d83a2-146">Poiché Nginx è stato installato per la prima volta, avviarlo in modo esplicito eseguendo:</span><span class="sxs-lookup"><span data-stu-id="d83a2-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="d83a2-147">Verificare che un browser visualizzi la pagina di destinazione predefinita per Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="d83a2-148">Configurare Nginx</span><span class="sxs-lookup"><span data-stu-id="d83a2-148">Configure Nginx</span></span>

<span data-ttu-id="d83a2-149">Per configurare Nginx come proxy inverso per inoltrare le richieste all'applicazione ASP.NET Core, modificare `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="d83a2-150">Aprirlo in un editor di testo e sostituire il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="d83a2-150">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="d83a2-151">Questo file di configurazione Nginx inoltra il traffico pubblico in ingresso dalla porta `80` alla porta `5000`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="d83a2-152">Dopo aver apportato le modifiche alla configurazione Nginx, è possibile eseguire `sudo nginx -t` per verificare la sintassi dei file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d83a2-152">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="d83a2-153">Se il test dei file di configurazione ha esito positivo, è possibile chiedere a Nginx di applicare le modifiche eseguendo `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-153">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="d83a2-154">Monitoraggio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d83a2-154">Monitoring our application</span></span>

<span data-ttu-id="d83a2-155">Nginx è ora configurato in modo da inoltrare le richieste effettuate a `http://yourhost:80` all'applicazione ASP.NET Core eseguita in Kestrel all'indirizzo `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-155">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="d83a2-156">Tuttavia, Nginx non è configurato per la gestione del processo Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d83a2-156">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="d83a2-157">È possibile usare *systemd* e creare un file del servizio per avviare e monitorare l'applicazione Web sottostante.</span><span class="sxs-lookup"><span data-stu-id="d83a2-157">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d83a2-158">*systemd* è un sistema di inizializzazione che offre molte funzionalità potenti per l'avvio, l'arresto e la gestione dei processi.</span><span class="sxs-lookup"><span data-stu-id="d83a2-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="d83a2-159">Creare il file del servizio</span><span class="sxs-lookup"><span data-stu-id="d83a2-159">Create the service file</span></span>

<span data-ttu-id="d83a2-160">Creare il file di definizione del servizio:</span><span class="sxs-lookup"><span data-stu-id="d83a2-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="d83a2-161">Di seguito è riportato un esempio di file del servizio per questa applicazione:</span><span class="sxs-lookup"><span data-stu-id="d83a2-161">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="d83a2-162">**Nota:** se l'utente *www-data* non viene usato dalla configurazione, è necessario creare prima l'utente definito qui e assegnargli correttamente la proprietà dei file.</span><span class="sxs-lookup"><span data-stu-id="d83a2-162">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="d83a2-163">Salvare il file e abilitare il servizio.</span><span class="sxs-lookup"><span data-stu-id="d83a2-163">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="d83a2-164">Avviare il servizio e verificare che sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d83a2-164">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="d83a2-165">Con il proxy inverso configurate e Kestrel gestito con systemd, l'applicazione Web è completamente configurata ed è possibile accedervi da un browser nel computer locale in `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-165">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d83a2-166">È anche possibile accedervi da un computer remoto, escludendo eventuali firewall che potrebbero impedirlo.</span><span class="sxs-lookup"><span data-stu-id="d83a2-166">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="d83a2-167">Se si esaminano le intestazioni di risposta, l'intestazione `Server` indica che l'applicazione ASP.NET Core è gestita da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d83a2-167">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="d83a2-168">Visualizzazione dei log</span><span class="sxs-lookup"><span data-stu-id="d83a2-168">Viewing logs</span></span>

<span data-ttu-id="d83a2-169">Poiché l'applicazione Web che usa Kestrel viene gestita con `systemd`, tutti gli eventi e i processi vengono registrati in un giornale di registrazione centralizzato.</span><span class="sxs-lookup"><span data-stu-id="d83a2-169">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d83a2-170">Tuttavia, questo giornale include tutte le voci per tutti i servizi e i processi gestiti da `systemd`.</span><span class="sxs-lookup"><span data-stu-id="d83a2-170">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="d83a2-171">Per visualizzare le voci specifiche di `kestrel-hellomvc.service`, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d83a2-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="d83a2-172">Per filtrare ulteriormente, opzioni come `--since today`, `--until 1 hour ago` o una combinazione delle stesse consentono di ridurre la quantità di voci restituite.</span><span class="sxs-lookup"><span data-stu-id="d83a2-172">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="d83a2-173">Protezione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d83a2-173">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="d83a2-174">Abilitare AppArmor</span><span class="sxs-lookup"><span data-stu-id="d83a2-174">Enable AppArmor</span></span>

<span data-ttu-id="d83a2-175">Linux Security Modules (LSM) è un framework che fa parte del kernel Linux a partire da Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="d83a2-175">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="d83a2-176">LSM supporta diverse implementazioni di moduli di protezione.</span><span class="sxs-lookup"><span data-stu-id="d83a2-176">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="d83a2-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) è un modulo LSM che implementa un sistema di controllo di accesso obbligatorio che consente di circoscrivere il programma a un set limitato di risorse.</span><span class="sxs-lookup"><span data-stu-id="d83a2-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="d83a2-178">Verificare che AppArmor sia abilitato e configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="d83a2-178">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="d83a2-179">Configurazione del firewall</span><span class="sxs-lookup"><span data-stu-id="d83a2-179">Configuring our firewall</span></span>

<span data-ttu-id="d83a2-180">Chiudere tutte le porte esterne che non sono in uso.</span><span class="sxs-lookup"><span data-stu-id="d83a2-180">Close off all external ports that are not in use.</span></span> <span data-ttu-id="d83a2-181">Il firewall UFW (Uncomplicated Firewall) offre un front-end per `iptables` attraverso un'interfaccia della riga di comando per la configurazione del firewall.</span><span class="sxs-lookup"><span data-stu-id="d83a2-181">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="d83a2-182">Verificare che `ufw` sia configurato per consentire il traffico su tutte le porte necessarie.</span><span class="sxs-lookup"><span data-stu-id="d83a2-182">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="d83a2-183">Protezione di Nginx</span><span class="sxs-lookup"><span data-stu-id="d83a2-183">Securing Nginx</span></span>

<span data-ttu-id="d83a2-184">La distribuzione predefinita di Nginx non abilita SSL.</span><span class="sxs-lookup"><span data-stu-id="d83a2-184">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="d83a2-185">Per abilitare ulteriori funzionalità di sicurezza, compilarle dal codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d83a2-185">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="d83a2-186">Scaricare il codice sorgente e installare le dipendenze di compilazione</span><span class="sxs-lookup"><span data-stu-id="d83a2-186">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="d83a2-187">Modificare il nome di risposta Nginx</span><span class="sxs-lookup"><span data-stu-id="d83a2-187">Change the Nginx response name</span></span>

<span data-ttu-id="d83a2-188">Modificare *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="d83a2-188">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="d83a2-189">Configurare le opzioni e compilare</span><span class="sxs-lookup"><span data-stu-id="d83a2-189">Configure the options and build</span></span>

<span data-ttu-id="d83a2-190">La libreria PCRE è obbligatoria per le espressioni regolari.</span><span class="sxs-lookup"><span data-stu-id="d83a2-190">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="d83a2-191">Le espressioni regolari sono usate nella direttiva ubicazione per ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="d83a2-191">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="d83a2-192">http_ssl_module aggiunge il supporto del protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d83a2-192">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="d83a2-193">È consigliabile usare un firewall per applicazioni Web, ad esempio *ModSecurity*, per la protezione avanzata dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d83a2-193">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="d83a2-194">Configurare SSL</span><span class="sxs-lookup"><span data-stu-id="d83a2-194">Configure SSL</span></span>

* <span data-ttu-id="d83a2-195">Configurare il server in modo che sia in ascolto del traffico HTTPS sulla porta `443` specificando un certificato valido emesso da un'autorità di certificazione attendibile.</span><span class="sxs-lookup"><span data-stu-id="d83a2-195">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="d83a2-196">Rafforzare la protezione adottando alcune delle procedure descritte nel file */etc/nginx/nginx.conf* che segue.</span><span class="sxs-lookup"><span data-stu-id="d83a2-196">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="d83a2-197">Gli esempi includono la scelta di una crittografia più complessa e il reindirizzamento di tutto il traffico su HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d83a2-197">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="d83a2-198">L'aggiunta di un'intestazione `HTTP Strict-Transport-Security` (HSTS) garantisce che tutte le richieste successive vengano effettuate dal esclusivamente via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d83a2-198">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="d83a2-199">Non aggiungere l'intestazione Strict-Transport-Security o scegliere un elemento `max-age` appropriato se si prevede di disabilitare SSL in futuro.</span><span class="sxs-lookup"><span data-stu-id="d83a2-199">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="d83a2-200">Aggiungere il file di configurazione */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="d83a2-200">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="d83a2-201">Aggiungere il file di configurazione */etc/nginx/proxy.conf*.</span><span class="sxs-lookup"><span data-stu-id="d83a2-201">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="d83a2-202">L'esempio contiene entrambe le sezioni `http` e `server` in un unico file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d83a2-202">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="d83a2-203">Proteggere Nginx dal clickjacking</span><span class="sxs-lookup"><span data-stu-id="d83a2-203">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="d83a2-204">Il clickjacking è una tecnica fraudolenta con cui i clic di un utente vengono "catturati"</span><span class="sxs-lookup"><span data-stu-id="d83a2-204">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="d83a2-205">e reindirizzati a un oggetto in un sito infetto.</span><span class="sxs-lookup"><span data-stu-id="d83a2-205">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="d83a2-206">Usare X-FRAME-OPTIONS per proteggere il sito.</span><span class="sxs-lookup"><span data-stu-id="d83a2-206">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="d83a2-207">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="d83a2-207">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d83a2-208">Aggiungere la riga `add_header X-Frame-Options "SAMEORIGIN";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-208">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d83a2-209">Analisi del tipo MIME</span><span class="sxs-lookup"><span data-stu-id="d83a2-209">MIME-type sniffing</span></span>

<span data-ttu-id="d83a2-210">Questa intestazione impedisce alla maggior parte dei browser di dedurre dall'analisi del tipo MIME un tipo di contenuto diverso da quello dichiarato, poiché l'intestazione indica al browser di non eseguire l'override del tipo di contenuto della risposta.</span><span class="sxs-lookup"><span data-stu-id="d83a2-210">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="d83a2-211">Con l'opzione `nosniff`, se il server indica che il contenuto è "text/html", il browser ne esegue il rendering come "text/html".</span><span class="sxs-lookup"><span data-stu-id="d83a2-211">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="d83a2-212">Modificare il file *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="d83a2-212">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d83a2-213">Aggiungere la riga `add_header X-Content-Type-Options "nosniff";` e salvare il file, quindi riavviare Nginx.</span><span class="sxs-lookup"><span data-stu-id="d83a2-213">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
