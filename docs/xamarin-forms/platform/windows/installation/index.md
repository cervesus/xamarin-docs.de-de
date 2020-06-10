---
Title: "Einrichten von Windows-Projekten" Beschreibung: "ältere Xamarin.Forms Lösungen (oder die unter macOS erstellten) haben keine universelle Windows-Plattform Projekte. Daher wird in diesem Artikel erläutert, wie einer vorhandenen Projekt Mappe ein neues UWP-Projekt hinzugefügt wird Xamarin.Forms ."
ms. Prod: xamarin ms. assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 04/10/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="setup-windows-projects"></a>Einrichten von Windows-Projekten

_Hinzufügen neuer Windows-Projekte zu Xamarin.Forms einer vorhandenen Projekt Mappe_

Ältere Xamarin.Forms Lösungen (oder die unter macOS erstellten) haben keine universelle Windows-Plattform (UWP)-app-Projekte. Daher müssen Sie manuell ein UWP-Projekt hinzufügen, um eine Windows 10-app (UWP) zu erstellen.

## <a name="add-a-universal-windows-platform-app"></a>Hinzufügen einer universelle Windows-Plattform-App

**Visual Studio 2019** unter **Windows 10** wird empfohlen, um UWP-apps zu erstellen. Weitere Informationen zum universelle Windows-Plattform finden Sie unter Einführung [in die universelle Windows-Plattform](/windows/uwp/get-started/universal-application-platform-guide/).

UWP ist in Xamarin.Forms 2,1 und höher sowie in verfügbar Xamarin.Forms . Maps wird in 2,2 und höher unterstützt Xamarin.Forms .

Hilfreiche Tipps finden Sie im Abschnitt zur <a href="#troubleshooting">Problem</a> Behandlung.

Befolgen Sie diese Anweisungen, um eine UWP-App hinzuzufügen, die auf Windows 10-Telefonen,-Tablets und-Desktops ausgeführt werden kann:

 1. Klicken Sie mit der rechten Maustaste auf die Projekt Mappe, und wählen Sie **> neues Projekt hinzufügen** aus, und fügen Sie ein **leeres App-Projekt (Universal Windows)** hinzu:

  ![](universal-images/add-wu.png "Add New Project Dialog")

 2,2. Wählen Sie im Dialogfeld **Neues universelle Windows-Plattform Projekt** die minimal-und Ziel Versionen von Windows 10 aus, auf denen die app ausgeführt werden soll:

  ![](universal-images/target-version.png "New Universal Windows Platform Project Dialog")

 €. Klicken Sie mit der rechten Maustaste auf das UWP-Projekt, wählen Sie **nuget-Pakete verwalten** aus, und fügen Sie das Paket hinzu. **Xamarin.Forms** Stellen Sie sicher, dass die anderen Projekte in der Projekt Mappe ebenfalls auf dieselbe Version des Pakets aktualisiert werden Xamarin.Forms .

 0:. Stellen Sie sicher, dass das neue UWP-Projekt im Fenster **Build > Configuration Manager** erstellt wird (Dies ist wahrscheinlich nicht standardmäßig der Fall). Aktivieren Sie **die Felder** **Erstellen** und Bereitstellen für das universelle Projekt:

  [![](universal-images/configuration-sml.png "Configuration Manager Window")](universal-images/configuration.png#lightbox "Configuration Manager Window")

 5@@. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **> Verweis hinzufügen** aus, und erstellen Sie einen Verweis auf das Xamarin.Forms Anwendungsprojekt (.NET Standard oder Shared Project).

  ![](universal-images/addref-sml.png "Reference Manager Dialog")

 6. Bearbeiten Sie im UWP-Projekt **app.XAML.cs** , um den `Init` Methoden aufrufin der `OnLaunched` Methode um Zeile 52 einzuschließen:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 19.00. Bearbeiten Sie im UWP-Projekt **MainPage. XAML** , indem Sie den `Grid` im- `Page` Element enthaltenen entfernen.

 88. Fügen Sie in " **MainPage. XAML**" einen neuen `xmlns` Eintrag für hinzu `Xamarin.Forms.Platform.UWP` :

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 21.00. Ändern Sie in " **MainPage. XAML**" das Stamm `<Page` Element in `<forms:WindowsPage` :

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 €. Bearbeiten Sie im UWP-Projekt **MainPage.XAML.cs** , um den `: Page` vererbungsspezifizierer für den Klassennamen zu entfernen (da er jetzt `WindowsPage` aufgrund der im vorherigen Schritt vorgenommenen Änderung von geerbt wird):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11:. Fügen Sie in **MainPage.XAML.cs**den-Befehl `LoadApplication` im `MainPage` Konstruktor hinzu, um die APP zu starten Xamarin.Forms :

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

> [!NOTE]
> Das Argument für die- `LoadApplication` Methode ist die-Instanz, die `Xamarin.Forms.Application` im .NET Standard-Projekt definiert ist.

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12.12.2016. Fügen Sie alle lokalen Ressourcen hinzu (z. b. Bilddateien) aus den vorhandenen Platt Form Projekten, die erforderlich sind.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Ziel Aufruf Ausnahme" bei Verwendung von "mit .net Native Toolkette kompilieren"

Wenn Ihre UWP-App auf mehrere Assemblys verweist (z. b. Steuerelement Bibliotheken von Drittanbietern oder wenn Ihre APP selbst in mehrere Bibliotheken aufgeteilt ist), Xamarin.Forms kann möglicherweise keine Objekte aus diesen Assemblys laden (z. b. benutzerdefinierte Renderer).

Dies kann der Fall sein, wenn Sie die **Toolkette compile with .net Native** verwenden, die eine Option für UWP-apps im **Eigenschaften > Build > Allgemeines** Fenster für das Projekt ist.

Sie können dieses Problem beheben, indem Sie eine UWP-spezifische Überladung des `Forms.Init` Aufrufes in **app.XAML.cs** verwenden, wie im folgenden Code gezeigt. (Sie sollten durch eine tatsächliche Klasse ersetzen, auf die der `ClassInOtherAssembly` Code verweist):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Fügen Sie einen Eintrag für jede Assembly hinzu, die Sie als Verweis in der Projektmappen-Explorer hinzugefügt haben, entweder über einen direkten Verweis oder ein nuget.

#### <a name="dependency-services-and-net-native-compilation"></a>Abhängigkeits Dienste und .net Native Kompilierung

Releasebuilds mit .net Native Kompilierung können Fehler beim Auflösen von Abhängigkeits Diensten verursachen, die außerhalb der ausführbaren Datei der Hauptanwendung (z. b. in einem separaten Projekt oder einer separaten Bibliothek)

Verwenden Sie die- `DependencyService.Register<T>()` Methode, um Abhängigkeits Dienst Klassen manuell zu registrieren. Fügen Sie basierend auf dem obigen Beispiel die Register-Methode wie folgt hinzu:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
