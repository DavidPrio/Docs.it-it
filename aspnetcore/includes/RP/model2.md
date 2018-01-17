Aggiungere le proprietà seguenti alla classe `Movie`:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

Il campo `ID` è richiesto dal database per la chiave primaria.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Aggiungere una classe di contesto dei dati

Aggiungere la classe derivata `DbContext` seguente denominata *MovieContext.cs* alla cartella *Models*: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

Il codice precedente crea una proprietà `DbSet` per il set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.
