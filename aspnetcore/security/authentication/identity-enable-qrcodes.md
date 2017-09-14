---
title: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
author: rick-anderson
description: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
keywords: Componenti di base di ASP.NET, MVC, generazione di codice a matrice, l'autenticatore, 2FA
ms.author: riande
manager: wpickett
ms.date: 7/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4eb260ecb3d1f1797bed5ba82ef3fc0628f05b6f
ms.sourcegitcommit: bb615e57e80ae496a63a1bf53a3a9cffab2fce9b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core

Nota: In questo argomento si applica a ASP.NET Core 2. x con pagine Razor.

ASP.NET Core viene fornito con supporto per le applicazioni per l'autenticazione singoli autenticatore. Due fattori (2FA) di autenticazione le app di autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono consigliato approch per 2FA settore. 2FA utilizzando TOTP è preferibile a 2FA SMS. Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma di nome utente e password. In genere un'app di autenticazione viene installata uno Smartphone.

I modelli di applicazione web ASP.NET Core supportano gli autenticatori, ma non forniscono supporto per la generazione di QRCode. I generatori QRCode facilitano l'installazione di 2FA. Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Aggiunta di codici a matrice alla pagina di configurazione 2FA

Queste istruzioni si usa *qrcode.js* dal repository https://davidshimjs.github.io/qrcodejs/.

* Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.

* Nel *Pages\Account\Manage\EnableAuthenticator.cshtml* file, individuare il `Scripts` sezione alla fine del file:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aggiornamento di `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e da una chiamata per generare il codice a matrice. Dovrebbe apparire come segue:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Eliminare il paragrafo in cui vengono forniti collegamenti a queste istruzioni.

Eseguire l'app e assicurarsi che è possibile scansionare il codice a matrice e convalidare il codice che dimostra l'autenticatore.

## <a name="change-the-site-name-in-the-qr-code"></a>Modificare il nome del sito nel codice a matrice

Il nome del sito nel codice QR viene ricavato dal nome del progetto che scelto durante la creazione del progetto. È possibile modificarla cercando il `GenerateQrCodeUri(string email, string unformattedKey)` metodo nel *EnableAuthenticator.cshtml.cs* file. 

Il codice predefinito con il modello è simile al seguente:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Il secondo parametro nella chiamata a `string.Format` è il nome del sito, ricavato il nome della soluzione. Può essere modificata in qualsiasi valore, ma deve essere sempre codificati nell'URL.

## <a name="using-a-different-qr-code-library"></a>Utilizzo di una libreria di codice a matrice diversi

È possibile sostituire la libreria di codice a matrice con la libreria preferita. Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.

L'URL formattato correttamente per il codice a matrice è disponibile nel:

* `AuthenticatorUri`proprietà del modello.
* `data-url`proprietà di `qrCodeData` elemento. 

Utilizzare `@Html.Raw` per accedere alla proprietà di modello in una vista (in caso contrario le e commerciali nell'url verranno codificate double e verrà ignorato il parametro etichetta del codice QR).