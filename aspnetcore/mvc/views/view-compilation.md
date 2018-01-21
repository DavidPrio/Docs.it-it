---
title: La precompilazione e la compilazione di visualizzazione razor
author: rick-anderson
description: Un documento di riferimento che descrivono come abilitare la compilazione di visualizzazione MVC Razor e precompilazione nelle applicazioni ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilazione di visualizzazione Razor e precompilazione in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Visualizzazioni Razor vengono compilate in fase di esecuzione quando viene richiamata la vista. ASP.NET Core 1.1.0 e versione successiva possono facoltativamente compilare visualizzazioni Razor e distribuirli con l'app&mdash;un processo noto come la precompilazione. Per impostazione predefinita, i modelli di progetto ASP.NET Core 2. x abilitare precompilazione.

> [!IMPORTANT]
> Precompilazione di visualizzazione Razor non è attualmente disponibile quando si esegue un [distribuzione indipendente (dimensione a modifica lenta)](/dotnet/core/deploying/#self-contained-deployments-scd) Core ASP.NET 2.0. La funzionalità sarà disponibile per dimensioni a modifica lenta quando rilascia 2.1. Per ulteriori informazioni, vedere [la compilazione di visualizzazione non riesce durante la compilazione incrociata per Linux in Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Considerazioni sulla precompilazione:

* La precompilazione di visualizzazioni comporta un più piccolo pubblicato bundle e tempi di avvio.
* È possibile modificare i file Razor dopo la precompilazione di viste. Le viste modificate non saranno presenti nel bundle pubblicato. 

Per distribuire precompilate viste:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se il progetto è destinato a .NET Framework, includere un riferimento pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se il progetto è destinato a .NET Core, non sono necessarie modifiche.

I modelli di progetto ASP.NET Core 2. x viene impostata in modo implicito `MvcRazorCompileOnPublish` a `true` per impostazione predefinita, ovvero il nodo può essere rimosso in modo sicuro dal *csproj* file. Se si preferisce, è necessario essere espliciti sono non crea alcun problema nell'impostazione di `MvcRazorCompileOnPublish` proprietà `true`. Nell'esempio *csproj* ed evidenzia questa impostazione:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Impostare `MvcRazorCompileOnPublish` a `true`e includere un riferimento pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Nell'esempio *csproj* ed evidenzia queste impostazioni:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Preparare l'app per un [distribuzione dipendenti dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) eseguendo un comando simile al seguente nella radice del progetto:

```console
dotnet publish -c Release
```

Oggetto *< Nome_Progetto >. PrecompiledViews.dll* file contenente le visualizzazioni Razor compilate, viene generato quando la precompilazione ha esito positivo. Ad esempio, la schermata seguente illustra il contenuto di *cshtml* all'interno di *WebApplication1.PrecompiledViews.dll*:

![Visualizzazioni Razor all'interno di DLL](view-compilation/_static/razor-views-in-dll.png)
