---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Compilazione dinamica di un controllo utilizzando il codice JavaScript (c#) | Documenti Microsoft
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3b23e16f2e4dd26f50eb3e07f35d078dd19a19
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Compilazione dinamica di un controllo utilizzando il codice JavaScript (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` controllo ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo da chiamare `DynamicPopulateExtender` controllo. Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo invia una parte delle informazioni di contesto con ogni chiamata al servizio web. Ecco il codice (file `DynamicPopulate.cs.asmx`) che consente di recuperare la data corrente in uno dei tre formati:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo sito ASP.NET e iniziare con il controllo ScriptManager di AJAX ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio mediante il controllo HTML con lo stesso nome, o `<asp:Label />` controllo web) che in un secondo momento visualizzerà il risultato della chiamata al servizio web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Successivamente, includere un `DynamicPopulateExtender` controllano e forniscono informazioni sui servizi web, controllo di destinazione, ma non il nome del controllo che attiva la popolazione questa operazione verrà eseguita in un secondo momento utilizzando JavaScript personalizzato.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Ora per la parte di JavaScript. Il `$find()` funzione, definito dalla libreria ASP.NET AJAX, restituisce un riferimento a oggetti lato server di ASP.NET AJAX Control Toolkit, ad esempio `DynamicPopulateExtender`. Nel file corrente, `$find("dpe")` restituisce un riferimento a quella `DynamicPopulateExtender` controllo nella pagina. Espone un metodo denominato `populate()` che attiva il processo di compilazione dinamica. Il `populate()` metodo richiede un argomento: la chiave di contesto che verrà utilizzata come argomento per il `getDate()` metodo web. Quindi, ad esempio, `$find("dpe").populate("format1")` popolerebbe il file etichetta con la data corrente nel formato mese-giorno-anno.

Per semplificare l'esempio più flessibile, l'utente può scegliere tra diversi formati di Data. Per ognuna di esse, viene visualizzato un pulsante di opzione. Una volta l'utente fa clic su un pulsante di opzione, il codice JavaScript compila in modo dinamico l'etichetta con il formato della data selezionata. Ecco i pulsanti di opzione:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Si noti che nel contesto di un pulsante di opzione, l'espressione JavaScript `this.value` fa riferimento al valore del pulsante corrente, che possono essere esattamente le stesse informazioni il `getDate()` metodo utilizzare.


[![Un clic sul pulsante Recupera la data dal server, nel formato specificato](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Un clic sul pulsante Recupera la data dal server, nel formato specificato ([fare clic per visualizzare l'immagine ingrandita](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](dynamically-populating-a-control-cs.md)
[Successivo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
