---
title: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a>Aggiunta di un modello a un'app di pagine Razor

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Aggiungere una stringa di connessione del database

Aggiungere una stringa di connessione al file *appsettings.json*.

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrare il contesto del database

Registrare il contesto del database con il contenitore [Dependency Injection](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) nel file *Startup.cs*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Compilare il progetto per verificare che non ci siano errori.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale

In questa sezione viene usata la Console di Gestione pacchetti per:

* Aggiungere il pacchetto di generazione codice Web di Visual Studio. Questo pacchetto è necessario per eseguire il motore di scaffolding.
* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

In PMC, immettere i comandi seguenti:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.

Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a>Eseguire il test dell'app

* Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).
* Eseguire il test del collegamento **Crea**.

 ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.

Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database:

L'esercitazione successiva illustra i file creati tramite scaffolding.

>[!div class="step-by-step"]
[Indietro: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)    
