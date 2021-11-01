# Mitschrift Video GitHub Actions - Part 1
https://youtu.be/R5ppadIsGbA

- erstelle Azure Account für Education
- erstelle beliebiges Repo auf github
- klone dieses leere Repo auf Rechner
  - git clone https://github.com/AllKindOfStuff/cicd_stropek.git 
  - vorhergehenden Link auf github.com kopieren
- erstelle z.B. ASP.NET Core Web API Anwendung in Visual Studio
  - kopiere den Inhalt des Projekts in das geklonte Repo am Rechner
- Starte VS Code aus dem geklonten Repo
- 14:50 installiere Plugin für gitignore und erstelle .gitignore Datei in Code. (autogenerate). Geht auch über www.gitignore.io     
- CI hinzufügen
  - In Code Plugin für github Actions installieren 
- Neuen Ordner .github in Code erstellen
  - in .github Ordner einen Ordner workflows erstellen   
    - Darin eine yaml Datei erstellen, Name kann freigewählt werden, z.B. ci-cd.yaml
- die yaml-Datei ist eine github Actions Specification
  - Es ist eine Art Rezept wie die Application gebaut werden soll
  - Erklärung zum Beispiel findest du in den Kommentaren der yaml Datei
  - Erstelle yaml -Datei
- Gehe zu https://github.com/marketplace
  - Filtere nach Actions (https://github.com/marketplace?type=actions)
  - Es gibt für alle möglichen Umgebungen verschiedene Actions (e.g. Java, .Net,....)
  - Suche nach Checkout V2 (https://github.com/marketplace/actions/checkout)
    - Schau dir den Abschnitt Usage an und kopiere **uses: actions/checkout@v2**
    - Trage dies in der yaml-Datei ein
  - Suche nach: Setup .NET Core SDK (https://github.com/marketplace/actions/setup-net-core-sdk)
    - Trage dies in der yaml-Datei ein
  - 31:45 Application bauen
  
    ```yaml 
    name: Restore dependencies 
    run: dotnet restore 
    name: Build the App 
    run: dotnet build -c Release --no-restore
    ```
   
- Push the Code to the Repo and the Build of the App will start
  - Check that on the github Webpage in the Actions Section


## Exkurs - how to build a .net Application in the CLI
  - Open a Terminal in Code (Folder of the solution)
  - run **dotnet restore** It fetches all the necessary nugget packages and installs them locally
  - run **dotnet build -c Release --no-restore** --> The application should build without any Errors
    - -c stands for configuration e.g. Release or Build
    - --no-restore makes it faster, because we did that step just before
    - fyi - The main difference between Build and Release in Software Testing is that Build is a version of a software the development team hands over to the testing team for testing purposes while Release is a software the testing team hands over to the customer. 
  - fyi: **dotnet --help** shows help, if needed 


# Mitschrift Video GitHub Actions - Part 1
https://youtu.be/ySVsLE0XWQA

## Add Unit Testing
- Open the solution in Visual Studio
- Neues Projekt hinzufügen. Rechtsklick auf Solution -> Hinzufügen -> Neues Projekt -> xUnit Test Project (.NET Core) for C# -> AnimalCountingDatabase.Tests
- Change to .Net 5. If you click on the Solution and the csproj file opens you can see that an older version of .net core is installed.
  - So Right Click the Solution -> Properties -> Change the Target Framwork to .NET 5.0 and save it
  - Also Right Click Solution -> Manage NuGet Packages -> an ceck if Updates are empty -> If not update them
  - Add a Test, e.g. 
    ```csharp
    public void Test1()
    {
        Assert.True(1 == 1);
    }
    ```
  - Make sure this Tests runs
  - Switch to Visual Studio Code again
  - Add a new step to the yaml file which runs the tests, find more detailed info here https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test
  ```yaml 
    - name: Run automated tests
      run: dotnet test -c Release --no-build
  ```
  - 06:50 check in everything and see if it runs on github
  - !!!!! Always write and run automated tests !!!!!!
