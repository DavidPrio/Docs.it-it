---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Creazione di Unit test per applicazioni ASP.NET MVC (c#) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce un parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 56c981363f1905c1c9869dbaf2adb6b5ac1c28a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Creazione di Unit test per applicazioni ASP.NET MVC (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scarica il PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce una visualizzazione particolare, restituisce un set specifico di dati oppure un altro tipo di risultato dell'azione.


L'obiettivo di questa esercitazione è dimostrare come è possibile scrivere unit test per i controller in ASP.NET MVC applicazioni. È descritto come creare tre tipi diversi di unit test. È illustrato come testare la vista restituita da un'azione del controller, come i dati della visualizzazione restituito da un'azione del controller di test e come testare o meno un'azione del controller si viene reindirizzati a una seconda azione del controller.

## <a name="creating-the-controller-under-test"></a>Creazione di Controller sottoposta a Test

Iniziamo creando il controller che si desidera testare. Il controller, denominato il `ProductController`, è contenuto in elenco 1.

**Elenco 1:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`. Entrambi i metodi di azione restituiscono una visualizzazione. Si noti che il `Details()` azione accetta un parametro denominato ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test della visualizzazione restituito da un Controller

Si supponga che si desidera testare o meno il `ProductController` restituisce della vista destra. Si desidera assicurarsi che quando il `ProductController.Details()` azione viene richiamata, viene restituita la visualizzazione dei dettagli. La classe di test nel listato 2 contiene uno unit test per la visualizzazione restituita da test di `ProductController.Details()` azione.

**Elenco di 2:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`. Questo metodo contiene tre righe di codice. La prima riga di codice crea una nuova istanza di `ProductController` classe. La seconda riga di codice richiama il controller `Details()` metodo di azione. Infine, l'ultima riga di codice controlli o meno la visualizzazione restituita dal `Details()` azione è la visualizzazione dei dettagli.

Il `ViewResult.ViewName` proprietà rappresenta il nome della visualizzazione restituito da un controller. Un avviso di grandi dimensioni sul test di questa proprietà. Esistono due modi che un controller può restituire una visualizzazione. Un controller può restituire in modo esplicito una visualizzazione simile al seguente:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

In alternativa, il nome della visualizzazione può essere dedotto dal nome dell'azione del controller simile al seguente:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Questa azione del controller restituisce inoltre una visualizzazione denominata `Details`. Tuttavia, il nome della visualizzazione viene dedotto dal nome dell'azione. Se si desidera verificare il nome della vista, è necessario restituire in modo esplicito il nome della visualizzazione dall'azione del controller.

È possibile eseguire lo unit test nel listato 2 immettendo la combinazione di tasti **Ctrl-R, A** o facendo clic di **eseguire tutti i test nella soluzione** pulsante (vedere Figura 1). Se il test viene superato, verrà visualizzato nella finestra Risultati Test nella figura 2.


[![Esegui tutti i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Figura 01**: esecuzione di tutti i test nella soluzione ([fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Operazione completata](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Figura 02**: esito positivo. ([Fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Verifica dei dati della visualizzazione restituito da un Controller

Controller MVC passa i dati in una vista utilizzando uno strumento denominato  *`View Data`* . Ad esempio, si supponga che si desidera visualizzare i dettagli per un determinato prodotto quando si richiama il `ProductController Details()` azione. In tal caso, è possibile creare un'istanza di un `Product` classe (definito nel modello) e passare l'istanza di `Details` visualizzazione sfruttando `View Data`.

Modificato `ProductController` listato 3 include una versione aggiornata `Details()` azione che restituisce un prodotto.

**Elenco di 3:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

Prima di tutto, il `Details()` azione viene creata una nuova istanza di `Product` classe che rappresenta un computer portatile. Successivamente, l'istanza del `Product` classe viene passata come secondo parametro per il `View()` metodo.

È possibile scrivere unit test per verificare se i dati previsti sono contenuto nella visualizzazione dati. Lo unit test in test listato 4 o meno un prodotto che rappresenta un computer portatile viene restituito quando si chiama il `ProductController Details()` metodo di azione.

**Elenco di 4:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

Listato 4 il `TestDetailsView()` metodo verifica i dati della visualizzazione restituito richiamando il `Details()` metodo. Il `ViewData` esposto come proprietà nel `ViewResult` restituito richiamando il `Details()` metodo. Il `ViewData.Model` proprietà contiene il prodotto passato alla visualizzazione. Il test di verifica semplicemente che il prodotto contenuto nei dati di visualizzazione con il nome di computer portatile.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Verifica il risultato dell'azione restituito da un Controller

Un'azione del controller più complessa può restituire diversi tipi di risultati dell'azione in base ai valori dei parametri passati all'azione del controller. Un'azione del controller può restituire un'ampia gamma di tipi di risultati dell'azione tra una `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.

Ad esempio, l'elemento modificato `Details()` azione listato 5 restituisce il `Details` visualizzare quando si passa un Id di prodotto valido per l'azione. Se si passa un prodotto valido Id - Id con un valore minore di - 1, quindi si viene reindirizzati al `Index()` azione.

**Elenco di 5:`ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

È possibile testare il comportamento del `Details()` azione con lo unit test nel listato 6. Lo unit test nel listato 6 verifica che si verrà reindirizzati al `Index` visualizzare quando viene passato un Id con il valore -1 per il `Details()` metodo.

**Elenco di 6:`ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Quando si chiama il `RedirectToAction()` azione del controller di metodo in un'azione del controller, restituisce un `RedirectToRouteResult`. I controlli di test se il `RedirectToRouteResult` reindirizzerà l'utente a un'azione del controller denominata `Index`.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come creare unit test per le azioni del controller MVC. In primo luogo, è stato descritto come verificare se la vista destra verrà restituita da un'azione del controller. Si è appreso come utilizzare il `ViewResult.ViewName` proprietà per verificare il nome di una vista.

Successivamente, viene esaminato come è possibile testare il contenuto di `View Data`. È stato descritto come controllare se il prodotto di destra è stato restituito `View Data` dopo la chiamata a un'azione del controller.

Infine, è illustrato come è possibile verificare se vengono restituiti tipi diversi di risultati dell'azione da un'azione del controller. È stato descritto come verificare se un controller restituisce un `ViewResult` o `RedirectToRouteResult`.

>[!div class="step-by-step"]
[Successivo](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
