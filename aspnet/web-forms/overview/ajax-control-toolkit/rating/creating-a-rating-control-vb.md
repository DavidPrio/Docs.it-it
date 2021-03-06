---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Creazione di un controllo di valutazione (VB) | Documenti Microsoft
author: wenz
description: "Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcuni impegno di codifica, ma non è disponibile il..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ff3394865e084c5a24e7e79469a4a7d26aabb552
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-vb"></a>Creazione di un controllo di valutazione (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi. Questo in genere richiede alcuni impegno di codifica, ma abbiamo il Toolkit di controllo per l'eliminazione.


## <a name="overview"></a>Panoramica

Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi. Questo in genere richiede alcuni impegno di codifica, ma abbiamo il Toolkit di controllo per l'eliminazione.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario (almeno) due tipi di immagini: una per una piena elemento classificazione e uno per un elemento vuoto di classificazione. Un elemento di classificazione è in genere una stella o un smile. Per questo scenario, si trova tre file, smiley.png empty.png e smile done.png come parte dei download di codice sorgente per questa esercitazione.

Quindi, creare un nuovo file ASP.NET e iniziare, aggiungere un `ScriptManager` controllo:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Aggiungere quindi il `Rating` controllo di ASP.NET AJAX Control Toolkit. È necessario impostare per questo esempio gli attributi seguenti:

- `CurrentRating`la valutazione iniziale da utilizzare
- `MaxRating`la classificazione massima
- `EmptyStarCssClass`la classe CSS da utilizzare quando un elemento di classificazione (asterisco) è vuota
- `FilledStarCssClass`la classe CSS da utilizzare quando viene compilato un elemento di classificazione (asterisco)
- `StarCssClass`la classe CSS da usare per un stat visibile
- `WaitingStarCssClass`la classe CSS da usare durante una classificazione a stelle viene inviata al server

Di seguito è il markup che crea un controllo di classificazione con cinque elementi (smileys) di cui non viene riempita inizialmente:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Le tre classi CSS a cui fa riferimento a questo punto necessario mostrare i file di immagine appropriata, ovvero semplice con CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Assicurarsi di fornire la larghezza e l'altezza delle tre immagini, in caso contrario la visualizzazione potrebbe avere un aspetto leggermente messed backup.

Infine, il risultato della valutazione deve essere visualizzato all'utente (o almeno salvato in un database). Aggiungere un'etichetta per l'output di un messaggio di testo e un pulsante di invio per inviare nuovamente il form di classificazione per il server:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Nel codice sul lato server, accedere al controllo di valutazione tramite il relativo `ID` e quindi accedere ai relativi `CurrentRating` proprietà che è il numero di elementi di classificazione selezionati, in questo esempio un valore compreso tra 0 e 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Salvare la pagina e caricarla nel browser. Quando si passa il mouse sugli elementi di classificazione (inizialmente vuota), si verifica un effetto JavaScript: le modifiche di classificazione. Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente. Infine, quando si invia il form, il codice lato server restituisce la classificazione selezionata.


[![Creazione di un sistema di classificazione con quantità minima di codice](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Creazione di un sistema di classificazione con quantità minima di codice ([fare clic per visualizzare l'immagine ingrandita](creating-a-rating-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](creating-a-rating-control-cs.md)
