---
title: "Core di ASP.NET MVC con Entity Framework Core - ereditarietà - 9 di 10"
author: tdykstra
description: "In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati, l'utilizzo di Entity Framework Core in un'applicazione ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: a4ae696bdd114ab9c36d1218f753fa3d515f2300
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a><span data-ttu-id="c2ef8-103">Ereditarietà - EF Core con l'esercitazione di base di ASP.NET MVC (9 di 10)</span><span class="sxs-lookup"><span data-stu-id="c2ef8-103">Inheritance - EF Core with ASP.NET Core MVC tutorial (9 of 10)</span></span>

<span data-ttu-id="c2ef8-104">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2ef8-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2ef8-105">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="c2ef8-106">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="c2ef8-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="c2ef8-107">Nell'esercitazione precedente, si gestite le eccezioni di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-107">In the previous tutorial, you handled concurrency exceptions.</span></span> <span data-ttu-id="c2ef8-108">In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-108">This tutorial will show you how to implement inheritance in the data model.</span></span>

<span data-ttu-id="c2ef8-109">Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per facilitare il riutilizzo del codice.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-109">In object-oriented programming, you can use inheritance to facilitate code reuse.</span></span> <span data-ttu-id="c2ef8-110">In questa esercitazione si modificherà il `Instructor` e `Student` classi in modo che derivano da un `Person` classe che contiene, ad esempio proprietà di base `LastName` che sono comuni a entrambi i docenti e studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-110">In this tutorial, you'll change the `Instructor` and `Student` classes so that they derive from a `Person` base class which contains properties such as `LastName` that are common to both instructors and students.</span></span> <span data-ttu-id="c2ef8-111">Non aggiungere o cambiare tutte le pagine web, ma si modificherà parte del codice e le modifiche verranno riflesse automaticamente nel database.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-111">You won't add or change any web pages, but you'll change some of the code and those changes will be automatically reflected in the database.</span></span>

## <a name="options-for-mapping-inheritance-to-database-tables"></a><span data-ttu-id="c2ef8-112">Opzioni per il mapping di ereditarietà alle tabelle di database</span><span class="sxs-lookup"><span data-stu-id="c2ef8-112">Options for mapping inheritance to database tables</span></span>

<span data-ttu-id="c2ef8-113">Il `Instructor` e `Student` classi nel modello di dati dell'istituto di istruzione di sono associate diverse proprietà che sono identiche:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-113">The `Instructor` and `Student` classes in the School data model have several properties that are identical:</span></span>

![Classi di Student e Instructor](inheritance/_static/no-inheritance.png)

<span data-ttu-id="c2ef8-115">Si supponga che si desidera eliminare il codice ridondante per le proprietà condivise dal `Instructor` e `Student` entità.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-115">Suppose you want to eliminate the redundant code for the properties that are shared by the `Instructor` and `Student` entities.</span></span> <span data-ttu-id="c2ef8-116">O si desidera scrivere un servizio che è possibile formattare i nomi senza caring se il nome proviene da un docente o uno studente.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-116">Or you want to write a service that can format names without caring whether the name came from an instructor or a student.</span></span> <span data-ttu-id="c2ef8-117">È possibile creare un `Person` classe base che contiene solo le proprietà condivise, quindi rendere la `Instructor` e `Student` le classi ereditano da tale classe di base, come illustrato nella figura seguente:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-117">You could create a `Person` base class that contains only those shared properties, then make the `Instructor` and `Student` classes inherit from that base class, as shown in the following illustration:</span></span>

![Student e Instructor le classi che derivano dalla classe di persona](inheritance/_static/inheritance.png)

<span data-ttu-id="c2ef8-119">Esistono diversi modi di che questa struttura di ereditarietà potrebbe essere rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-119">There are several ways this inheritance structure could be represented in the database.</span></span> <span data-ttu-id="c2ef8-120">È possibile usare una tabella di persona che include informazioni su studenti e insegnanti in un'unica tabella.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-120">You could have a Person table that includes information about both students and instructors in a single table.</span></span> <span data-ttu-id="c2ef8-121">Alcune delle colonne è stato possibile applicare solo per i docenti (HireDate), alcuni solo per gli studenti (EnrollmentDate), alcune su entrambi (LastName, FirstName).</span><span class="sxs-lookup"><span data-stu-id="c2ef8-121">Some of the columns could apply only to instructors (HireDate), some only to students (EnrollmentDate), some to both (LastName, FirstName).</span></span> <span data-ttu-id="c2ef8-122">In genere è una colonna del discriminatore per indicare che ogni riga rappresenta un tipo.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-122">Typically, you'd have a discriminator column to indicate which type each row represents.</span></span> <span data-ttu-id="c2ef8-123">La colonna del discriminatore, ad esempio, potrebbe essere "Istruttore" per i docenti e "Student" per studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-123">For example, the discriminator column might have "Instructor" for instructors and "Student" for students.</span></span>

![Esempio di tabella per gerarchia](inheritance/_static/tph.png)

<span data-ttu-id="c2ef8-125">Questo modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è denominato tabella per gerarchia ereditarietà della.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-125">This pattern of generating an entity inheritance structure from a single database table is called table-per-hierarchy (TPH) inheritance.</span></span>

<span data-ttu-id="c2ef8-126">In alternativa è possibile rendere il database più simile la struttura di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-126">An alternative is to make the database look more like the inheritance structure.</span></span> <span data-ttu-id="c2ef8-127">Ad esempio, è possibile avere solo i campi del nome della tabella Person e dispongono di tabelle di corsi e studenti separate con i campi delle date.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-127">For example, you could have only the name fields in the Person table and have separate Instructor and Student tables with the date fields.</span></span>

![Ereditarietà tabella per tipo](inheritance/_static/tpt.png)

<span data-ttu-id="c2ef8-129">Questo modello di una tabella di database per ogni classe di entità è denominato tabella per l'ereditarietà di tipo.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-129">This pattern of making a database table for each entity class is called table per type (TPT) inheritance.</span></span>

<span data-ttu-id="c2ef8-130">Ancora un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratta per le singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-130">Yet another option is to map all non-abstract types to individual tables.</span></span> <span data-ttu-id="c2ef8-131">Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguire il mapping alle colonne della tabella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-131">All properties of a class, including inherited properties, map to columns of the corresponding table.</span></span> <span data-ttu-id="c2ef8-132">Questo modello viene denominato ereditarietà della classe per ogni tabella-Concrete (TPC).</span><span class="sxs-lookup"><span data-stu-id="c2ef8-132">This pattern is called Table-per-Concrete Class (TPC) inheritance.</span></span> <span data-ttu-id="c2ef8-133">Se è implementato ereditarietà TPC per le classi di persona, Student e Instructor come illustrato in precedenza, le tabelle Student e Instructor avrà un aspetto non è diverse dopo l'implementazione dell'ereditarietà rispetto a quello precedente.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-133">If you implemented TPC inheritance for the Person, Student, and Instructor classes as shown earlier, the Student and Instructor tables would look no different after implementing inheritance than they did before.</span></span>

<span data-ttu-id="c2ef8-134">TPC e Implementerà i criteri di ereditarietà offrono in genere prestazioni migliori rispetto a criteri di ereditarietà tabella per tipo, in quanto possono generare modelli di tabella per tipo query join complessi.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-134">TPC and TPH inheritance patterns generally deliver better performance than TPT inheritance patterns, because TPT patterns can result in complex join queries.</span></span>

<span data-ttu-id="c2ef8-135">In questa esercitazione viene illustrato come implementare Implementerà l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-135">This tutorial demonstrates how to implement TPH inheritance.</span></span> <span data-ttu-id="c2ef8-136">Della tabella per gerarchia è il modello di ereditarietà solo che supporta il nucleo di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-136">TPH is the only inheritance pattern that the Entity Framework Core supports.</span></span>  <span data-ttu-id="c2ef8-137">È possibile eseguire è creare un `Person` classe, modificare il `Instructor` e `Student` classi da cui derivare `Person`, aggiungere la nuova classe per il `DbContext`e creare una migrazione.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-137">What you'll do is create a `Person` class, change the `Instructor` and `Student` classes to derive from `Person`, add the new class to the `DbContext`, and create a migration.</span></span>

> [!TIP] 
> <span data-ttu-id="c2ef8-138">Si consiglia di salvare una copia del progetto prima di apportare le modifiche seguenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-138">Consider saving a copy of the project before making the following changes.</span></span>  <span data-ttu-id="c2ef8-139">Se si verificano problemi ed è necessario ricominciare, sarà più facile iniziare dal progetto salvato anziché inversione passaggi da eseguire per questa esercitazione o a partire dall'inizio della serie intera.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-139">Then if you run into problems and need to start over, it will be easier to start from the saved project instead of reversing steps done for this tutorial or going back to the beginning of the whole series.</span></span>

## <a name="create-the-person-class"></a><span data-ttu-id="c2ef8-140">Creare la classe di persona</span><span class="sxs-lookup"><span data-stu-id="c2ef8-140">Create the Person class</span></span>

<span data-ttu-id="c2ef8-141">Nella cartella Models, creare Person e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-141">In the Models folder, create Person.cs and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a><span data-ttu-id="c2ef8-142">Creazione delle classi Student e Instructor ereditare da una persona</span><span class="sxs-lookup"><span data-stu-id="c2ef8-142">Make Student and Instructor classes inherit from Person</span></span>

<span data-ttu-id="c2ef8-143">In *Instructor.cs*, derivare la classe istruttore dalla classe di persona e rimuovere i campi chiavi e il nome.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-143">In *Instructor.cs*, derive the Instructor class from the Person class and remove the key and name fields.</span></span> <span data-ttu-id="c2ef8-144">Il codice sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-144">The code will look like the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

<span data-ttu-id="c2ef8-145">Apportare le stesse modifiche nel *Student.cs*.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-145">Make the same changes in *Student.cs*.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a><span data-ttu-id="c2ef8-146">Aggiungere il tipo di entità utente per il modello di dati</span><span class="sxs-lookup"><span data-stu-id="c2ef8-146">Add the Person entity type to the data model</span></span>

<span data-ttu-id="c2ef8-147">Aggiungere il tipo di entità utente per *SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-147">Add the Person entity type to *SchoolContext.cs*.</span></span> <span data-ttu-id="c2ef8-148">Le nuove righe vengono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-148">The new lines are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

<span data-ttu-id="c2ef8-149">Questo è tutto ciò che necessita di Entity Framework per configurare l'ereditarietà tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-149">This is all that the Entity Framework needs in order to configure table-per-hierarchy inheritance.</span></span> <span data-ttu-id="c2ef8-150">Come si vedrà, quando il database viene aggiornato, si otterrà una tabella Person invece le tabelle Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-150">As you'll see, when the database is updated, it will have a Person table in place of the Student and Instructor tables.</span></span>

## <a name="create-and-customize-migration-code"></a><span data-ttu-id="c2ef8-151">Creare e personalizzare il codice di migrazione</span><span class="sxs-lookup"><span data-stu-id="c2ef8-151">Create and customize migration code</span></span>

<span data-ttu-id="c2ef8-152">Salvare le modifiche e compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-152">Save your changes and build the project.</span></span> <span data-ttu-id="c2ef8-153">Quindi aprire la finestra di comando nella cartella del progetto e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-153">Then open the command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add Inheritance
```

<span data-ttu-id="c2ef8-154">Non vengono eseguiti il `database update` ancora di comando.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-154">Don't run the `database update` command yet.</span></span> <span data-ttu-id="c2ef8-155">Tale comando causerà perdita di dati perché elimina la tabella Instructor e rinominare la tabella di studenti alla persona.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-155">That command will result in lost data because it will drop the Instructor table and rename the Student table to Person.</span></span> <span data-ttu-id="c2ef8-156">È necessario fornire codice personalizzato per mantenere i dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-156">You need to provide custom code to preserve existing data.</span></span>

<span data-ttu-id="c2ef8-157">Aprire *migrazioni /\<timestamp > _Inheritance.cs* e sostituire il `Up` (metodo) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-157">Open *Migrations/\<timestamp>_Inheritance.cs* and replace the `Up` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

<span data-ttu-id="c2ef8-158">Questo codice si occupa delle attività di aggiornamento di database seguenti:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-158">This code takes care of the following database update tasks:</span></span>

* <span data-ttu-id="c2ef8-159">Rimuove i vincoli foreign key e gli indici che fanno riferimento alla tabella di studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-159">Removes foreign key constraints and indexes that point to the Student table.</span></span>

* <span data-ttu-id="c2ef8-160">Rinomina la tabella Instructor come persona e apporta modifiche necessarie archiviare i dati di Student:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-160">Renames the Instructor table as Person and makes changes needed for it to store Student data:</span></span>

* <span data-ttu-id="c2ef8-161">Aggiunge EnrollmentDate ammette valori null per studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-161">Adds nullable EnrollmentDate for students.</span></span>

* <span data-ttu-id="c2ef8-162">Aggiunge una colonna del discriminatore per indicare se una riga è per uno studente o un docente.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-162">Adds Discriminator column to indicate whether a row is for a student or an instructor.</span></span>

* <span data-ttu-id="c2ef8-163">Poiché le righe di student avrà date di assunzione, rende HireDate ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-163">Makes HireDate nullable since student rows won't have hire dates.</span></span>

* <span data-ttu-id="c2ef8-164">Aggiunge un campo temporaneo che verrà utilizzato per aggiornare le chiavi esterne che puntano a studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-164">Adds a temporary field that will be used to update foreign keys that point to students.</span></span> <span data-ttu-id="c2ef8-165">Quando si copia studenti nella tabella Person otterranno nuovi valori di chiave primari.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-165">When you copy students into the Person table they will get new primary key values.</span></span>

* <span data-ttu-id="c2ef8-166">Copia i dati della tabella di studenti nella tabella Person.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-166">Copies data from the Student table into the Person table.</span></span> <span data-ttu-id="c2ef8-167">In questo modo gli studenti a vengano assegnati nuovi valori di chiave primari.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-167">This causes students to get assigned new primary key values.</span></span>

* <span data-ttu-id="c2ef8-168">Consente di correggere i valori di chiave esterna che fanno riferimento a studenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-168">Fixes foreign key values that point to students.</span></span>

* <span data-ttu-id="c2ef8-169">Consente di ricreare i vincoli di chiave esterni e gli indici, ora puntarli alla tabella Person.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-169">Re-creates foreign key constraints and indexes, now pointing them to the Person table.</span></span>

<span data-ttu-id="c2ef8-170">(Se è stato usato GUID anziché l'intero come tipo di chiave primario, non sarebbe necessario modificare i valori di chiave primaria di studenti e alcuni di questi passaggi Impossibile sono stati omessi.)</span><span class="sxs-lookup"><span data-stu-id="c2ef8-170">(If you had used GUID instead of integer as the primary key type, the student primary key values wouldn't have to change, and several of these steps could have been omitted.)</span></span>

<span data-ttu-id="c2ef8-171">Eseguire il `database update` comando:</span><span class="sxs-lookup"><span data-stu-id="c2ef8-171">Run the `database update` command:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="c2ef8-172">(In un sistema di produzione apportate le modifiche corrispondenti al `Down` metodo nel caso qualora fosse necessario usarla per tornare alla versione del database precedente.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-172">(In a production system you would make corresponding changes to the `Down` method in case you ever had to use that to go back to the previous database version.</span></span> <span data-ttu-id="c2ef8-173">Per questa esercitazione non è possibile utilizzare il `Down` metodo.)</span><span class="sxs-lookup"><span data-stu-id="c2ef8-173">For this tutorial you won't be using the `Down` method.)</span></span>

> [!NOTE] 
> <span data-ttu-id="c2ef8-174">È possibile ottenere altri errori quando si apportano modifiche allo schema in un database che contiene dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-174">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="c2ef8-175">Se si verificano errori di migrazione che è possibile risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-175">If you get migration errors that you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="c2ef8-176">Con un nuovo database, non sono presenti dati per eseguire la migrazione e il comando update-database è più probabile che completata senza errori.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-176">With a new database, there's no data to migrate, and the update-database command is more likely to complete without errors.</span></span> <span data-ttu-id="c2ef8-177">Per eliminare il database, utilizzare sillaba SSOX o eseguire il `database drop` comando CLI.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-177">To delete the database, use SSOX or run the `database drop` CLI command.</span></span>

## <a name="test-with-inheritance-implemented"></a><span data-ttu-id="c2ef8-178">Eseguire il test con ereditarietà implementato</span><span class="sxs-lookup"><span data-stu-id="c2ef8-178">Test with inheritance implemented</span></span>

<span data-ttu-id="c2ef8-179">Eseguire l'app e provare diverse pagine.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-179">Run the app and try various pages.</span></span> <span data-ttu-id="c2ef8-180">Tutto ciò che funziona come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-180">Everything works the same as it did before.</span></span>

<span data-ttu-id="c2ef8-181">In **Esplora oggetti di SQL Server**, espandere **connessioni dati/SchoolContext** e quindi **tabelle**, noterai che le tabelle Student e Instructor sono state sostituite da un Tabella Person.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-181">In **SQL Server Object Explorer**, expand **Data Connections/SchoolContext** and then **Tables**, and you see that the Student and Instructor tables have been replaced by a Person table.</span></span> <span data-ttu-id="c2ef8-182">Aprire Progettazione tabelle Person e si noterà che contiene tutte le colonne utilizzate per le tabelle Student e Instructor.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-182">Open the Person table designer and you see that it has all of the columns that used to be in the Student and Instructor tables.</span></span>

![Tabella Person in sillaba SSOX](inheritance/_static/ssox-person-table.png)

<span data-ttu-id="c2ef8-184">Il pulsante destro della tabella Person e quindi fare clic su **Mostra dati tabella** per visualizzare la colonna del discriminatore.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-184">Right-click the Person table, and then click **Show Table Data** to see the discriminator column.</span></span>

![Tabella Person in sillaba SSOX - i dati della tabella](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a><span data-ttu-id="c2ef8-186">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="c2ef8-186">Summary</span></span>

<span data-ttu-id="c2ef8-187">Tabella per gerarchia ereditarietà per aver implementato il `Person`, `Student`, e `Instructor` classi.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-187">You've implemented table-per-hierarchy inheritance for the `Person`, `Student`, and `Instructor` classes.</span></span> <span data-ttu-id="c2ef8-188">Per ulteriori informazioni sull'ereditarietà in Entity Framework Core, vedere [ereditarietà](https://docs.microsoft.com/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="c2ef8-188">For more information about inheritance in Entity Framework Core, see [Inheritance](https://docs.microsoft.com/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="c2ef8-189">Nella prossima esercitazione verrà visualizzato come gestire un'ampia gamma di scenari di Entity Framework relativamente avanzati.</span><span class="sxs-lookup"><span data-stu-id="c2ef8-189">In the next tutorial you'll see how to handle a variety of relatively advanced Entity Framework scenarios.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2ef8-190">[Precedente](concurrency.md)
[Successivo](advanced.md)</span><span class="sxs-lookup"><span data-stu-id="c2ef8-190">[Previous](concurrency.md)
[Next](advanced.md)</span></span>  
