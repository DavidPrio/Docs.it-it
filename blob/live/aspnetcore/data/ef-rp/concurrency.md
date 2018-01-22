---
title: Pagine Razor con Entity Framework Core, 8 - concorrenza - 8
author: rick-anderson
description: "In questa esercitazione viene illustrato come gestire i conflitti quando più utenti aggiornano la stessa entità nello stesso momento."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: a980669d49d332d7ef2ff5a18c73e9b269281287
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
<span data-ttu-id="51ba4-103">en-us /</span><span class="sxs-lookup"><span data-stu-id="51ba4-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="51ba4-104">Gestione dei conflitti di concorrenza - Core EF con pagine Razor (8 di 8)</span><span class="sxs-lookup"><span data-stu-id="51ba4-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="51ba4-105">Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), e [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="51ba4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="51ba4-106">In questa esercitazione viene illustrato come gestire i conflitti quando più utenti di aggiornare un'entità contemporaneamente (allo stesso tempo).</span><span class="sxs-lookup"><span data-stu-id="51ba4-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="51ba4-107">Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="51ba4-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="51ba4-108">Conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="51ba4-108">Concurrency conflicts</span></span>

<span data-ttu-id="51ba4-109">Si verifica un conflitto di concorrenza quando:</span><span class="sxs-lookup"><span data-stu-id="51ba4-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="51ba4-110">Un utente passa alla pagina di modifica per un'entità.</span><span class="sxs-lookup"><span data-stu-id="51ba4-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="51ba4-111">Un altro utente aggiorna la stessa entità prima di modifica del primo utente viene scritto il database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="51ba4-112">Se non è abilitato il rilevamento di concorrenza, quando si verificano aggiornamenti simultanei:</span><span class="sxs-lookup"><span data-stu-id="51ba4-112">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="51ba4-113">Ultimo aggiornamento wins.</span><span class="sxs-lookup"><span data-stu-id="51ba4-113">The last update wins.</span></span> <span data-ttu-id="51ba4-114">Vale a dire gli ultimi valori di aggiornamento vengono salvati al database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="51ba4-115">La prima degli aggiornamenti correnti andranno perse.</span><span class="sxs-lookup"><span data-stu-id="51ba4-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="51ba4-116">Concorrenza ottimistica</span><span class="sxs-lookup"><span data-stu-id="51ba4-116">Optimistic concurrency</span></span>

<span data-ttu-id="51ba4-117">Concorrenza ottimistica consente i conflitti di concorrenza consente di eseguire e quindi reagisce nel modo appropriato quando si verificano.</span><span class="sxs-lookup"><span data-stu-id="51ba4-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="51ba4-118">Ad esempio, Jane visita la pagina di modifica del reparto e il budget per il reparto inglese 350,000.00 $ diventa $0,00.</span><span class="sxs-lookup"><span data-stu-id="51ba4-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![La modifica di budget per 0](concurrency/_static/change-budget.png)

<span data-ttu-id="51ba4-120">Prima di Jane seleziona **salvare**, John visita la pagina stessa e il campo Data di inizio da 1/9/2007 a 1/9/2013.</span><span class="sxs-lookup"><span data-stu-id="51ba4-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Modifica la data di inizio a 2013](concurrency/_static/change-date.png)

<span data-ttu-id="51ba4-122">Jane seleziona **salvare** prima e vede cambia quando il browser viene visualizzata la pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="51ba4-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Budget modificato a zero](concurrency/_static/budget-zero.png)

<span data-ttu-id="51ba4-124">Seleziona John **salvare** in una pagina di modifica che mostra ancora un budget di $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="51ba4-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="51ba4-125">Le operazioni successive è determinata dalla modalità di gestione i conflitti di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51ba4-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="51ba4-126">Concorrenza ottimistica include le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="51ba4-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="51ba4-127">È possibile tenere traccia di quali proprietà di un utente ha modificato e aggiornare solo le colonne corrispondenti nel database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="51ba4-128">Nello scenario, non dati andrebbero persi.</span><span class="sxs-lookup"><span data-stu-id="51ba4-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="51ba4-129">Diverse proprietà sono state aggiornate con i due utenti.</span><span class="sxs-lookup"><span data-stu-id="51ba4-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="51ba4-130">La volta successiva che un utente che accede al dipartimento in lingua inglese, noteranno le modifiche di Jane sia di John.</span><span class="sxs-lookup"><span data-stu-id="51ba4-130">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="51ba4-131">Questo metodo di aggiornamento può ridurre il numero di conflitti che potrebbero comportare la perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="51ba4-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="51ba4-132">Questo approccio: \* non è possibile evitare la perdita di dati se vengono apportate modifiche competizione per la stessa proprietà.</span><span class="sxs-lookup"><span data-stu-id="51ba4-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="51ba4-133">\* È in genere non è pratico in un'app web.</span><span class="sxs-lookup"><span data-stu-id="51ba4-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="51ba4-134">È necessario mantenere lo stato del significativo per tenere traccia di recupero di tutti i valori e i nuovi valori.</span><span class="sxs-lookup"><span data-stu-id="51ba4-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="51ba4-135">Gestione di grandi quantità di stato può influiscono sulle prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="51ba4-136">\* È possibile aumentare la complessità delle app rispetto al rilevamento di concorrenza in un'entità.</span><span class="sxs-lookup"><span data-stu-id="51ba4-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="51ba4-137">È possibile consentire la modifica di John sovrascrivere la modifica di Jane.</span><span class="sxs-lookup"><span data-stu-id="51ba4-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="51ba4-138">La volta successiva che un utente che accede al reparto in lingua inglese, verranno visualizzate 1/9/2013 e il valore di $350,000.00 recuperato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-138">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="51ba4-139">Questo approccio è definito un *prevalenza del Client* o *ultimo in Wins* scenario.</span><span class="sxs-lookup"><span data-stu-id="51ba4-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="51ba4-140">(Tutti i valori dal client hanno la precedenza su ciò che si trova nell'archivio dati). Se non si configura questo codice per la gestione della concorrenza, prevalenza del Client viene eseguita automaticamente.</span><span class="sxs-lookup"><span data-stu-id="51ba4-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="51ba4-141">È possibile impedire la modifica di John vengano aggiornati nel database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="51ba4-142">In genere, l'app sarebbe: \* visualizzare un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="51ba4-143">\* Indica lo stato corrente dei dati.</span><span class="sxs-lookup"><span data-stu-id="51ba4-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="51ba4-144">\* Consente all'utente riapplicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="51ba4-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="51ba4-145">Si tratta di un *archivio Wins* scenario.</span><span class="sxs-lookup"><span data-stu-id="51ba4-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="51ba4-146">(I valori dell'archivio dati hanno la precedenza sui valori inviati dal client). Implementare lo scenario archivio Wins in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="51ba4-147">Questo metodo assicura che modifiche non vengono sovrascritti senza che l'utente viene generato un avviso.</span><span class="sxs-lookup"><span data-stu-id="51ba4-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="51ba4-148">La gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="51ba4-148">Handling concurrency</span></span> 

<span data-ttu-id="51ba4-149">Quando una proprietà è configurata come un [token di concorrenza](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="51ba4-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="51ba4-150">Core EF verifica che proprietà non è stata modificata dopo che è stata recuperata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="51ba4-151">Il controllo viene eseguito quando [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) o [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-151">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="51ba4-152">Se la proprietà è stata modificata dopo che è stato recuperato, un [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="51ba4-153">Il modello di database e dati deve essere configurato per supportare la generazione di `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="51ba4-154">Il rilevamento dei conflitti di concorrenza su una proprietà</span><span class="sxs-lookup"><span data-stu-id="51ba4-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="51ba4-155">È possibile rilevare i conflitti di concorrenza a livello di proprietà con il [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attributo.</span><span class="sxs-lookup"><span data-stu-id="51ba4-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="51ba4-156">L'attributo può essere applicato a più proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="51ba4-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="51ba4-157">Per ulteriori informazioni, vedere [dati annotazioni-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="51ba4-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="51ba4-158">Il `[ConcurrencyCheck]` attributo non viene utilizzato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-158">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="51ba4-159">Il rilevamento dei conflitti di concorrenza su una riga</span><span class="sxs-lookup"><span data-stu-id="51ba4-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="51ba4-160">Per rilevare i conflitti di concorrenza, un [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) rilevamento colonna viene aggiunto al modello.</span><span class="sxs-lookup"><span data-stu-id="51ba4-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="51ba4-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="51ba4-161">`rowversion` :</span></span>

* <span data-ttu-id="51ba4-162">È SQL Server specifico.</span><span class="sxs-lookup"><span data-stu-id="51ba4-162">Is SQL Server specific.</span></span> <span data-ttu-id="51ba4-163">Altri database potrebbero non fornire una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="51ba4-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="51ba4-164">Viene utilizzato per determinare che un'entità non è stata modificata perché è stato recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="51ba4-165">Il database genera una sequenza `rowversion` numero che sia stato incrementato ogni volta che la riga viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="51ba4-166">In un `Update` o `Delete` comando, il `Where` clausola include il valore recuperato di `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="51ba4-167">Se la riga aggiornata è stata modificata:</span><span class="sxs-lookup"><span data-stu-id="51ba4-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="51ba4-168">`rowversion`non corrisponde al valore di recupero.</span><span class="sxs-lookup"><span data-stu-id="51ba4-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="51ba4-169">Il `Update` o `Delete` comandi non trovano una riga perché la `Where` clausola include il recupero `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="51ba4-170">Oggetto `DbUpdateConcurrencyException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="51ba4-171">In Entity Framework Core, quando non le righe sono state aggiornate mediante un `Update` o `Delete` comando, viene generata un'eccezione di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51ba4-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="51ba4-172">Aggiungere una proprietà di rilevamento all'entità reparto</span><span class="sxs-lookup"><span data-stu-id="51ba4-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="51ba4-173">In *Models/Department.cs*, aggiungere una proprietà di rilevamento denominata RowVersion:</span><span class="sxs-lookup"><span data-stu-id="51ba4-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="51ba4-174">Il [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attributo specifica che questa colonna viene inclusa nel `Where` clausola di `Update` e `Delete` comandi.</span><span class="sxs-lookup"><span data-stu-id="51ba4-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="51ba4-175">L'attributo viene chiamato `Timestamp` perché le versioni precedenti di SQL Server utilizzato un database SQL `timestamp` il tipo di dati prima di SQL `rowversion` tipo sostituito.</span><span class="sxs-lookup"><span data-stu-id="51ba4-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="51ba4-176">L'API fluent è inoltre possibile specificare la proprietà di rilevamento:</span><span class="sxs-lookup"><span data-stu-id="51ba4-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="51ba4-177">Il codice seguente viene mostrata una parte di T-SQL generato da componenti di base di Entity Framework quando viene aggiornato il nome di reparto:</span><span class="sxs-lookup"><span data-stu-id="51ba4-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="51ba4-178">Precedente evidenziata codice mostra il `WHERE` clausola contenente `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="51ba4-179">Se il database `RowVersion` non sia uguale al `RowVersion` parametro (`@p2`), non vengono aggiornate righe.</span><span class="sxs-lookup"><span data-stu-id="51ba4-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="51ba4-180">Il codice evidenziato di seguito viene illustrato il T-SQL che consente di verificare esattamente una riga è stata aggiornata:</span><span class="sxs-lookup"><span data-stu-id="51ba4-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="51ba4-181">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) restituisce il numero di righe interessate dall'ultima istruzione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-181">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="51ba4-182">In nessun righe vengono aggiornate, EF Core genera una `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="51ba4-183">È possibile visualizzare che il nucleo di EF T-SQL genera l'errore nella finestra di output di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51ba4-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="51ba4-184">Aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="51ba4-184">Update the DB</span></span>

<span data-ttu-id="51ba4-185">Aggiunta di `RowVersion` Modifica proprietà modello di database, che richiede la migrazione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="51ba4-186">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="51ba4-186">Build the project.</span></span> <span data-ttu-id="51ba4-187">Nella finestra di comando, immettere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="51ba4-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="51ba4-188">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="51ba4-188">The preceding commands:</span></span>

* <span data-ttu-id="51ba4-189">Aggiunge il *migrazioni / {ora stamp}_RowVersion.cs* file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="51ba4-190">Gli aggiornamenti di *Migrations/SchoolContextModelSnapshot.cs* file.</span><span class="sxs-lookup"><span data-stu-id="51ba4-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="51ba4-191">L'aggiornamento aggiunge il codice evidenziato di seguito per il `BuildModel` metodo:</span><span class="sxs-lookup"><span data-stu-id="51ba4-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="51ba4-192">Esegue le migrazioni per aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="51ba4-193">Lo scaffolding il modello di reparto</span><span class="sxs-lookup"><span data-stu-id="51ba4-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="51ba4-194">Uscire da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51ba4-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="51ba4-195">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="51ba4-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="51ba4-196">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="51ba4-197">Il comando precedente delle strutture di `Department` modello.</span><span class="sxs-lookup"><span data-stu-id="51ba4-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="51ba4-198">Aprire il progetto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51ba4-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="51ba4-199">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="51ba4-199">Build the project.</span></span> <span data-ttu-id="51ba4-200">La compilazione genera errori simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="51ba4-201">Una modifica globale `_context.Department` a `_context.Departments` (ovvero, aggiungere una "s" `Department`).</span><span class="sxs-lookup"><span data-stu-id="51ba4-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="51ba4-202">7 occorrenze sono disponibili e aggiornate.</span><span class="sxs-lookup"><span data-stu-id="51ba4-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="51ba4-203">Aggiornare la pagina di indice reparti</span><span class="sxs-lookup"><span data-stu-id="51ba4-203">Update the Departments Index page</span></span>

<span data-ttu-id="51ba4-204">Il motore di scaffolding creato un `RowVersion` colonna per la pagina di indice, ma tale campo non deve essere visualizzata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="51ba4-205">In questa esercitazione, l'ultimo byte del `RowVersion` viene visualizzato per comprendere la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="51ba4-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="51ba4-206">L'ultimo byte non deve necessariamente essere univoco.</span><span class="sxs-lookup"><span data-stu-id="51ba4-206">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="51ba4-207">Non visualizzare una vera app `RowVersion` o l'ultimo byte della `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="51ba4-208">Aggiornare la pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="51ba4-208">Update the Index page:</span></span>

* <span data-ttu-id="51ba4-209">Sostituire l'indice con reparti.</span><span class="sxs-lookup"><span data-stu-id="51ba4-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="51ba4-210">Sostituire il markup che contiene `RowVersion` con l'ultimo byte della `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="51ba4-211">Sostituire FirstMidName con nome completo.</span><span class="sxs-lookup"><span data-stu-id="51ba4-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="51ba4-212">Il markup seguente mostra la pagina aggiornata:</span><span class="sxs-lookup"><span data-stu-id="51ba4-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="51ba4-213">Aggiornare il modello di pagina di modifica</span><span class="sxs-lookup"><span data-stu-id="51ba4-213">Update the Edit page model</span></span>

<span data-ttu-id="51ba4-214">Aggiornamento *pages\departments\edit.cshtml.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="51ba4-215">Per rilevare un problema di concorrenza, il [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) viene aggiornato con il `rowVersion` valore dall'entità è stato recuperato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="51ba4-216">Core EF genera un comando SQL UPDATE con una clausola WHERE contenente originale `RowVersion` valore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="51ba4-217">Se nessuna riga è interessata dal comando di aggiornamento (righe di non avere originale `RowVersion` valore), un `DbUpdateConcurrencyException` viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="51ba4-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="51ba4-218">Nel codice precedente, `Department.RowVersion` è il valore quando l'entità è stata recuperata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="51ba4-219">`OriginalValue`è il valore nel database quando `FirstOrDefaultAsync` in questo metodo è stato chiamato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="51ba4-220">Il codice seguente ottiene i valori del client (i valori a questo metodo di registrazione) e i valori DB:</span><span class="sxs-lookup"><span data-stu-id="51ba4-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="51ba4-221">Il codice seguente aggiunge un messaggio di errore personalizzati per i valori diversi da cosa è stata registrata per ogni colonna con DB `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="51ba4-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="51ba4-222">Seguenti evidenziato set di codice il `RowVersion` valore per il nuovo valore recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="51ba4-223">La volta successiva che l'utente fa clic **salvare**, solo gli errori di concorrenza che si verificano dopo l'ultima visualizzazione della pagina di modifica verrà rilevata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="51ba4-224">Il `ModelState.Remove` istruzione è necessaria perché `ModelState` è il vecchio `RowVersion` valore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="51ba4-225">Nella pagina Razor il `ModelState` valore per un campo ha la precedenza sui valori di proprietà del modello quando sono presenti entrambe.</span><span class="sxs-lookup"><span data-stu-id="51ba4-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="51ba4-226">Aggiornare la pagina di modifica</span><span class="sxs-lookup"><span data-stu-id="51ba4-226">Update the Edit page</span></span>

<span data-ttu-id="51ba4-227">Aggiornamento *Pages/Departments/Edit.cshtml* con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="51ba4-228">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-228">The preceding markup:</span></span>

* <span data-ttu-id="51ba4-229">Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="51ba4-230">Aggiunge una versione di riga nascosta.</span><span class="sxs-lookup"><span data-stu-id="51ba4-230">Adds a hidden row version.</span></span> <span data-ttu-id="51ba4-231">`RowVersion`è necessario aggiungere in modo postback associa il valore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="51ba4-232">Visualizza l'ultimo byte della `RowVersion` a scopo di debug.</span><span class="sxs-lookup"><span data-stu-id="51ba4-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="51ba4-233">Sostituisce `ViewData` con fortemente tipizzata `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="51ba4-234">Conflitti di concorrenza di test con la pagina di modifica</span><span class="sxs-lookup"><span data-stu-id="51ba4-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="51ba4-235">Aprire due istanze del browser di modifica su del reparto in lingua inglese:</span><span class="sxs-lookup"><span data-stu-id="51ba4-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="51ba4-236">Eseguire l'app e selezionare i reparti.</span><span class="sxs-lookup"><span data-stu-id="51ba4-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="51ba4-237">Fare doppio clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese e selezionare **aperto in una nuova scheda**.</span><span class="sxs-lookup"><span data-stu-id="51ba4-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="51ba4-238">Nella prima scheda, fare clic su di **modifica** collegamento ipertestuale per il reparto in lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="51ba4-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="51ba4-239">Le schede del due browser visualizzano le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="51ba4-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="51ba4-240">Modificare il nome nella prima scheda del browser e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="51ba4-240">Change the name in the first browser tab and click **Save**.</span></span>

![Reparto Modifica pagina 1 dopo la modifica](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="51ba4-242">Il browser viene mostrata la pagina di indice con il valore modificato e aggiornato rowVersion indicatore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="51ba4-243">Si noti l'indicatore rowVersion aggiornato, viene visualizzato con il secondo postback in altra scheda.</span><span class="sxs-lookup"><span data-stu-id="51ba4-243">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="51ba4-244">Modificare un campo diverso nella seconda scheda del browser.</span><span class="sxs-lookup"><span data-stu-id="51ba4-244">Change a different field in the second browser tab.</span></span>

![Reparto Modifica pagina 2 dopo la modifica](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="51ba4-246">Fare clic su **Salva**.</span><span class="sxs-lookup"><span data-stu-id="51ba4-246">Click **Save**.</span></span> <span data-ttu-id="51ba4-247">Vengono visualizzati messaggi di errore per tutti i campi che non corrispondono ai valori di database:</span><span class="sxs-lookup"><span data-stu-id="51ba4-247">You see error messages for all fields that don't match the DB values:</span></span>

![Messaggio di errore di pagina Modifica reparto](concurrency/_static/edit-error.png)

<span data-ttu-id="51ba4-249">Questa finestra del browser non si intende modificare il nome di campo.</span><span class="sxs-lookup"><span data-stu-id="51ba4-249">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="51ba4-250">Copiare e incollare il valore corrente (lingue) nel campo nome.</span><span class="sxs-lookup"><span data-stu-id="51ba4-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="51ba4-251">Scheda. La convalida lato client rimuove il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-251">Tab out. Client-side validation removes the error message.</span></span>

![Messaggio di errore di pagina Modifica reparto](concurrency/_static/cv.png)

<span data-ttu-id="51ba4-253">Fare clic su **salvare** nuovamente.</span><span class="sxs-lookup"><span data-stu-id="51ba4-253">Click **Save** again.</span></span> <span data-ttu-id="51ba4-254">Il valore che immesso nella seconda scheda browser viene salvato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="51ba4-255">Visualizzare i valori salvati nella pagina di indice.</span><span class="sxs-lookup"><span data-stu-id="51ba4-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="51ba4-256">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="51ba4-256">Update the Delete page</span></span>

<span data-ttu-id="51ba4-257">Aggiornare il modello di pagina di Delete con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="51ba4-258">La pagina di eliminazione rileva i conflitti di concorrenza quando l'entità è stato modificato dopo che è stata recuperata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="51ba4-259">`Department.RowVersion`è la versione di riga quando l'entità è stata recuperata.</span><span class="sxs-lookup"><span data-stu-id="51ba4-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="51ba4-260">Quando EF Core viene creato il comando SQL DELETE, include una clausola WHERE con `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="51ba4-261">Se i risultati del comando SQL DELETE in zero righe interessate:</span><span class="sxs-lookup"><span data-stu-id="51ba4-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="51ba4-262">Il `RowVersion` in SQL DELETE il comando non corrisponde a `RowVersion` nel database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="51ba4-263">Viene generata un'eccezione di DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="51ba4-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="51ba4-264">`OnGetAsync`viene chiamato con il `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="51ba4-265">Aggiornare la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="51ba4-265">Update the Delete page</span></span>

<span data-ttu-id="51ba4-266">Aggiornamento *Pages/Departments/Delete.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="51ba4-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="51ba4-267">Il markup precedente apporta le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="51ba4-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="51ba4-268">Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="51ba4-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="51ba4-269">Aggiunge un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-269">Adds an error message.</span></span>
* <span data-ttu-id="51ba4-270">Sostituisce FirstMidName con nome completo nel **amministratore** campo.</span><span class="sxs-lookup"><span data-stu-id="51ba4-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="51ba4-271">Le modifiche `RowVersion` per visualizzare l'ultimo byte.</span><span class="sxs-lookup"><span data-stu-id="51ba4-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="51ba4-272">Aggiunge una versione di riga nascosta.</span><span class="sxs-lookup"><span data-stu-id="51ba4-272">Adds a hidden row version.</span></span> <span data-ttu-id="51ba4-273">`RowVersion`è necessario aggiungere in modo postback associa il valore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="51ba4-274">Conflitti di concorrenza di test con la pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="51ba4-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="51ba4-275">Creare un test di reparto.</span><span class="sxs-lookup"><span data-stu-id="51ba4-275">Create a test department.</span></span>

<span data-ttu-id="51ba4-276">Aprire due istanze del browser di eliminazione sul reparto di test:</span><span class="sxs-lookup"><span data-stu-id="51ba4-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="51ba4-277">Eseguire l'app e selezionare i reparti.</span><span class="sxs-lookup"><span data-stu-id="51ba4-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="51ba4-278">Fare doppio clic su di **eliminare** collegamento ipertestuale per il reparto di test e selezionare **aperto in una nuova scheda**.</span><span class="sxs-lookup"><span data-stu-id="51ba4-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="51ba4-279">Fare clic su di **modifica** collegamento ipertestuale per il reparto di test.</span><span class="sxs-lookup"><span data-stu-id="51ba4-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="51ba4-280">Le schede del due browser visualizzano le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="51ba4-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="51ba4-281">Modificare il budget nella prima scheda del browser e fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="51ba4-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="51ba4-282">Il browser viene mostrata la pagina di indice con il valore modificato e aggiornato rowVersion indicatore.</span><span class="sxs-lookup"><span data-stu-id="51ba4-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="51ba4-283">Si noti l'indicatore rowVersion aggiornato, viene visualizzato con il secondo postback in altra scheda.</span><span class="sxs-lookup"><span data-stu-id="51ba4-283">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="51ba4-284">Eliminare il reparto di test dalla seconda scheda. Un errore di concorrenza viene visualizzato con i valori correnti dal database.</span><span class="sxs-lookup"><span data-stu-id="51ba4-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="51ba4-285">Fare clic su **eliminare** Elimina l'entità, a meno che non `RowVersion` è stata updated.department è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="51ba4-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="51ba4-286">Vedere [ereditarietà](xref:data/ef-mvc/inheritance) su come ereditare un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="51ba4-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="51ba4-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="51ba4-287">Additional resources</span></span>

* [<span data-ttu-id="51ba4-288">Token di concorrenza in EF Core</span><span class="sxs-lookup"><span data-stu-id="51ba4-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="51ba4-289">La gestione della concorrenza in EF Core</span><span class="sxs-lookup"><span data-stu-id="51ba4-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="51ba4-290">Precedente</span><span class="sxs-lookup"><span data-stu-id="51ba4-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
