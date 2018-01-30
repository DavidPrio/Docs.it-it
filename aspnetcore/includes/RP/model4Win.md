<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="36470-101">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="36470-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="36470-102">Eseguire il codice seguente dalla riga di comando nella directory del progetto che contiene i file *Program.cs*, *Startup.cs* e *csproj*:</span><span class="sxs-lookup"><span data-stu-id="36470-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="36470-103">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="36470-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="36470-104">Aprire una shell dei comandi nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="36470-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="36470-105">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="36470-105">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="36470-106">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="36470-106">Exit Visual Studio and run the command again.</span></span>
