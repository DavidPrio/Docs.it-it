---
title: "Aggiunta della funzionalità di ricerca"
author: rick-anderson
description: "Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 2d8a18365a0d46d6468d708e1cd02def071309b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="2ad07-103">Nota: SQLlite distingue tra maiuscole e minuscole, pertanto sarà necessario cercare "Fantasma" e non "fantasma".</span><span class="sxs-lookup"><span data-stu-id="2ad07-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="2ad07-104">Modifica il tag `<form>` nella vista Razor *Views\movie\Index.cshtml* per specificare `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="2ad07-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="2ad07-105">[Precedente - metodi e viste del controller](controller-methods-views.md)
[Successivo - Aggiungere un campo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="2ad07-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
