---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introduzione a ASP.NET MVC 5 | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui utilizzando Visual Studio 2015. Nuova esercitazione Usa ASP.NET Core MVC 6, che fornisce molti improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="9a408-104">Introduzione ad ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="9a408-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="9a408-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9a408-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 <span data-ttu-id="9a408-106">In questa esercitazione verrà fornite le nozioni di base della creazione di un'app web ASP.NET MVC 5 utilizzando [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9a408-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="9a408-107">Origine finale per l'esercitazione si trovano in [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="9a408-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>
 
 
 <span data-ttu-id="9a408-108">In questa esercitazione è stato scritto dal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="9a408-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>
 
 <span data-ttu-id="9a408-109">È necessario un account Azure per distribuire l'app in Azure:</span><span class="sxs-lookup"><span data-stu-id="9a408-109">You need an Azure account to deploy this app to Azure:</span></span>
 
 - <span data-ttu-id="9a408-110">È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a408-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="9a408-111">È possibile [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="9a408-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="9a408-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="9a408-112">Getting Started</span></span>

<span data-ttu-id="9a408-113">Avviare l'installazione ed eseguendo [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9a408-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="9a408-114">Visual Studio è un IDE, o un ambiente di sviluppo integrato.</span><span class="sxs-lookup"><span data-stu-id="9a408-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="9a408-115">Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni.</span><span class="sxs-lookup"><span data-stu-id="9a408-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="9a408-116">In Visual Studio è disponibile un elenco lungo il bordo inferiore che mostra le diverse opzioni disponibili per l'utente.</span><span class="sxs-lookup"><span data-stu-id="9a408-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="9a408-117">È inoltre disponibile un menu che consente a un altro modo per eseguire attività nell'IDE.</span><span class="sxs-lookup"><span data-stu-id="9a408-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="9a408-118">(Ad esempio, anziché selezionare **nuovo progetto** dal **avviare** pagina, è possibile utilizzare il menu e selezionare **File** &gt; **nuovo progetto**.)</span><span class="sxs-lookup"><span data-stu-id="9a408-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a><span data-ttu-id="9a408-119">Creazione di un'applicazione</span><span class="sxs-lookup"><span data-stu-id="9a408-119">Creating Your First Application</span></span>

<span data-ttu-id="9a408-120">Fare clic su **nuovo progetto**, quindi selezionare Visual c# a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="9a408-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="9a408-121">Denominare il progetto "MvcMovie" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a408-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="9a408-122">Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC** e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a408-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="9a408-123">Visual Studio utilizzato un modello predefinito per il progetto ASP.NET MVC che appena creato, in modo che sia un'applicazione funzionante ora senza eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="9a408-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="9a408-124">Si tratta di un semplice "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="9a408-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="9a408-125">progetto e 's un buon punto di partenza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9a408-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="9a408-126">Premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="9a408-126">Click F5 to start debugging.</span></span> <span data-ttu-id="9a408-127">F5 fa sì che Visual Studio avviare [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) ed eseguire l'app web.</span><span class="sxs-lookup"><span data-stu-id="9a408-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="9a408-128">Visual Studio, quindi, viene avviato un browser e verrà visualizzata la home page dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9a408-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="9a408-129">Si noti che la barra degli indirizzi del browser afferma `localhost:port#` e non è `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9a408-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9a408-130">Ciò accade perché `localhost` punta sempre al computer locale, che in questo caso è in esecuzione l'applicazione appena compilato.</span><span class="sxs-lookup"><span data-stu-id="9a408-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="9a408-131">Quando Visual Studio viene eseguito un progetto web, viene utilizzata una porta casuale per il server web.</span><span class="sxs-lookup"><span data-stu-id="9a408-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="9a408-132">Nell'immagine seguente, il numero di porta è 1234.</span><span class="sxs-lookup"><span data-stu-id="9a408-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="9a408-133">Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="9a408-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="9a408-134">Subito tale modello predefinito consente principale, contatti e sulle pagine.</span><span class="sxs-lookup"><span data-stu-id="9a408-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="9a408-135">L'immagine precedente non viene visualizzato il **Home**, **su** e **contatto** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="9a408-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="9a408-136">A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di navigazione per vedere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="9a408-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="9a408-137">L'applicazione fornisce inoltre il supporto per registrare e accedere.</span><span class="sxs-lookup"><span data-stu-id="9a408-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="9a408-138">Il passaggio successivo consiste nella modifica il funzionamento di questa applicazione e un po' informazioni su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9a408-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="9a408-139">Chiudere l'applicazione ASP.NET MVC e modificare il codice.</span><span class="sxs-lookup"><span data-stu-id="9a408-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="9a408-140">Per un elenco di esercitazioni corrente, vedere [MVC consigliato articoli](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="9a408-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="9a408-141">Vedere l'App in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="9a408-141">See this App Running on Azure</span></span>

<span data-ttu-id="9a408-142">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="9a408-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="9a408-143">È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="9a408-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="9a408-144">È necessario un account Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="9a408-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="9a408-145">Se si dispone già di un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9a408-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="9a408-146">[Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="9a408-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="9a408-147">[Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="9a408-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9a408-148">avanti</span><span class="sxs-lookup"><span data-stu-id="9a408-148">Next</span></span>](adding-a-controller.md)
