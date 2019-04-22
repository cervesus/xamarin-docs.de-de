---
title: Tieferer Einblick in Xamarin.Forms-Schnellstart
description: In diesem Artikel werden die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms erläutert. Zu den behandelten Themen zählen die Struktur einer Xamarin.Forms-Anwendung, die Architektur- und Anwendungsgrundlagen sowie die Benutzeroberfläche.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 67b189254cc08fac0323b7df5fcbab5abd994c05
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855015"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Tieferer Einblick in Xamarin.Forms-Schnellstart

In der [Xamarin.Forms-Schnellstart](~/get-started/index.yml), die Anmerkungen zu dieser Anwendung erstellt wurde. In diesem Artikel wird dieser Vorgang noch einmal genauer betrachtet, um ein Verständnis für die Funktionsweise von Xamarin.Forms-Anwendungen zu erarbeiten.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

In Visual Studio wird Code in *Projektmappen* und *Projekten* organisiert. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Anmerkungen zu dieser Anwendung besteht aus einer Projektmappe mit vier Projekten, wie im folgenden Screenshot gezeigt:

![](deepdive-images/vs/solution.png "Projektmappen-Explorer von Visual Studio")

Diese Projekte sind folgende:

- Anmerkungen zu dieser Version: dieses Projekt ist das .NET Standard Library-Projekt, das alle freigegebenen Code und freigegebene Benutzeroberfläche enthält.
- Notes.Android: dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-Anwendung.
- Notes.iOS: dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für die iOS-Anwendung.
- Notes.UWP: dieses Projekt enthält universelle Windows-Plattform (UWP)-spezifischen Code und ist der Einstiegspunkt für die UWP-Anwendung.

## <a name="anatomy-of-a-xamarinforms-application"></a>Aufbau einer Xamarin.Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt des Anmerkungen zu dieser Version .NET Standard Library-Projekts in Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Inhalt des Phoneword-Projekts von .NET Standard")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **NuGet** &ndash; die Xamarin.Forms und Sqlite-Net-Pcl-NuGet-Pakete, die dem Projekt hinzugefügt wurden.
- **SDK**: Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Die Codeorganisation in [Visual Studio für Mac](/visualstudio/mac/) baut auf Visual Studio auf und gliedert sich in *Projektmappen* und *Projekte*. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Anmerkungen zu dieser Anwendung besteht aus einer Projektmappe mit drei Projekten, wie im folgenden Screenshot gezeigt:

![](deepdive-images/vsmac/solution.png "Visual Studio für Mac: Projektmappenbereich")

Diese Projekte sind folgende:

- Anmerkungen zu dieser Version: dieses Projekt ist das .NET Standard Library-Projekt, das alle freigegebenen Code und freigegebene Benutzeroberfläche enthält.
- Notes.Android: dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für Android-Anwendungen.
- Notes.iOS: dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für iOS-Anwendungen.

## <a name="anatomy-of-a-xamarinforms-application"></a>Aufbau einer Xamarin.Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt der Anmerkungen zu dieser Version .NET Standard Library-Projekt in Visual Studio für Mac:

![](deepdive-images/vsmac/net-standard-project.png "Inhalt des Phoneword .NET Standard-Bibliotheksprojekts")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **NuGet** &ndash; die Xamarin.Forms und Sqlite-Net-Pcl-NuGet-Pakete, die dem Projekt hinzugefügt wurden.
- **SDK**: Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end

Das Projekt besteht aus mehreren Dateien:

- **Data\NoteDatabase.cs** – diese Klasse enthält Code, um die Datenbank erstellen, Daten daraus zu lesen, Schreiben von Daten, und Daten daraus zu löschen.
- **Models\Note.cs** – diese Klasse definiert eine `Note` Modell, mit deren Instanzen werden Daten über jeden Hinweis in der Anwendung speichern.
- **App.xaml**: Das XAML-Markup für die `App`-Klasse, die ein Ressourcenverzeichnis für die Anwendung definiert
- **App.xaml.cs**: Das CodeBehind-Modell für die `App`-Klasse, das die erste Seite instanziiert, die von der Anwendung auf jeder Plattform angezeigt wird, und für die Behandlung von Ereignissen im App-Lebenszyklus zuständig ist
- **Datei "AssemblyInfo.cs"** – diese Datei enthält ein Application-Attribut für das Projekt, das auf Assemblyebene angewendet wird.
- **NotesPage.xaml** – die XAML-Markup für die `NotesPage` -Klasse, die die Benutzeroberfläche für die Seite angezeigt, wenn die Anwendung startet definiert.
- **NotesPage.xaml.cs** – der Code-Behind für die `NotesPage` -Klasse, die die Geschäftslogik, die ausgeführt wird enthält, wenn der Benutzer mit der Seite interagiert.
- **NoteEntryPage.xaml** – die XAML-Markup für die `NoteEntryPage` -Klasse, die die Benutzeroberfläche für die Seite angezeigt, wenn der Benutzer eine Notiz gibt definiert.
- **NoteEntryPage.xaml.cs** – der Code-Behind für die `NoteEntryPage` -Klasse, die die Geschäftslogik, die ausgeführt wird enthält, wenn der Benutzer mit der Seite interagiert.

Weitere Informationen zum Aufbau einer Xamarin.iOS-Anwendung finden Sie unter [Hello, iOS: Deep Dive (Hallo iOS: Ausführliche Informationen)](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Weitere Informationen zum Aufbau einer Xamarin.Android-Anwendung finden Sie unter [Hello, Android: Deep Dive (Hallo Android: Ausführliche Informationen)](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Grundlagen der Architektur und die Anwendung

Eine Xamarin.Forms-Anwendung ist genauso aufgebaut wie eine traditionelle plattformübergreifende Anwendung. Freigegebener Code wird in der Regel in einer .NET Standard-Bibliothek platziert und von plattformspezifischen Anwendungen genutzt. Das folgende Diagramm zeigt einen Überblick über diese Beziehung für die Anmerkungen zu dieser Anwendung:

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Anmerkungen zu dieser Architektur")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Anmerkungen zu dieser Architektur")

::: zone-end

Um die Wiederverwendung von Startcode zu maximieren, haben Xamarin.Forms-Anwendungen eine winzige Klasse namens `App`, die die erste Seite instanziiert, die von der Anwendung wie im folgenden Codebeispiel gezeigt auf jeder Plattform angezeigt wird:

```csharp
using Xamarin.Forms;

namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new NavigationPage(new NotesPage());
        }
        ...
    }
}
```

Dieser Code legt fest der `MainPage` Eigenschaft der `App` -Klasse eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) -Instanz, deren Inhalt eine `NotesPage` Instanz.

Darüber hinaus die **"AssemblyInfo.cs"** -Datei enthält eine einzelne Anwendung Attribut, die auf der Assemblyebene angewendet wird:

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

Die [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) Attribut aktiviert den XAML-Compiler, sodass XAML direkt in der Zwischensprache kompiliert wird. Weitere Informationen finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Starten der Anwendung auf jeder Plattform

### <a name="ios"></a>iOS

Um die Xamarin.Forms-Startseite in iOS zu starten, das Notes.iOS-Projekt definiert die `AppDelegate` erbt von der `FormsApplicationDelegate` Klasse:

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

Die `FinishedLaunching`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die iOS-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor der Root View Controller durch den Aufruf auf die `LoadApplication`-Methode festgelegt wird.

### <a name="android"></a>Android

Zum Starten der Xamarin.Forms-Startseite in Android enthält das Notes.Android-Projekt Code, der erstellt eine `Activity` mit der `MainLauncher` Attribut, mit der Aktivität erbt die `FormsAppCompatActivity` Klasse:

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

Die `OnCreate`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die Android-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor die Xamarin.Forms-Anwendung geladen wird.

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

In Anwendungen der universellen Windows-Plattform (Universal Windows Platform, UWP) wird die `Init`-Methode, die das Xamarin.Forms-Framework initialisiert, über die `App`-Klasse aufgerufen:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Dadurch wird die UWP-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Xamarin.Forms-Startseite wird gestartet, indem die `MainPage` Klasse:

```csharp
namespace Notes.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Notes.App());
        }
    }
}
```

Die Xamarin.Forms-Anwendung wird mit der `LoadApplication`-Methode geladen.

> [!NOTE]
> Apps der universellen Windows-Plattform können mit Xamarin.Forms erstellt, aber nur mit Visual Studio unter Windows sein.

::: zone-end

## <a name="user-interface"></a>Benutzeroberfläche

Es gibt vier hauptsteuerelementgruppen verwendet, um die Benutzeroberfläche einer Xamarin.Forms-Anwendung zu erstellen:

1. **Seiten**: Xamarin.Forms-Seiten stellen Anzeigen plattformübergreifender mobiler Anwendungen dar. Die Anmerkungen zu dieser Anwendung verwendet die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) zum Anzeigen der einzelnen Bildschirme. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md) (Xamarin.Forms-Seiten).
1. **Ansichten**: Xamarin.Forms-Ansichten sind die Steuerelemente auf der Benutzeroberfläche, z.B. Bezeichnungen, Schaltflächen und Texteingabefelder. Die fertige Anmerkungen zu dieser Anwendung verwendet die [ `ListView` ](xref:Xamarin.Forms.ListView), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Button` ](xref:Xamarin.Forms.Button) Ansichten. Weitere Informationen zu Ansichten finden Sie unter [Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md) (Xamarin.Forms-Ansichten).
1. **Layouts**: Xamarin.Forms-Layouts sind Container, die zum Erstellen von Ansichten in Form von logischen Strukturen verwendet werden. Die Anmerkungen zu dieser Anwendung verwendet die [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Klasse, um die Ansichten in einem vertikalen Stapel, anordnen und die [ `Grid` ](xref:Xamarin.Forms.Grid) Klasse, um Schaltflächen horizontal anordnen. Weitere Informationen zu Layouts finden Sie unter [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md) (Xamarin.Forms-Layouts).
1. **Zellen**: Xamarin.Forms-Zellen sind spezielle Elemente für Listenelemente und beschreiben, wie die einzelnen Elemente in einer Liste gezeichnet werden sollen. Die Anmerkungen zu dieser Anwendung verwendet die [ `TextCell` ](xref:Xamarin.Forms.TextCell) zwei Elemente für jede Zeile in der Liste angezeigt. Weitere Informationen zu Zellen finden Sie unter [Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md) (Xamarin.Forms-Zellen).

Zur Laufzeit wird jedes Steuerelement seinem nativen Äquivalent zugeordnet. Dieses wird dann gerendert.

### <a name="layout"></a>Layout

Die Anmerkungen zu dieser Anwendung verwendet die [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , plattformübergreifende Entwicklung zu vereinfachen, indem Sie Sichten, auf dem Bildschirm unabhängig von der Bildschirmgröße automatisch angeordnet. Alle untergeordneten Elemente werden nacheinander horizontal oder vertikal in der Reihenfolge positioniert, in der sie hinzugefügt wurden. Wie viel Speicherplatz das `StackLayout` verwendet, hängt davon ab, wie die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) festgelegt sind. Das `StackLayout` versucht jedoch standardmäßig, den gesamten Bildschirm zu verwenden.

Der folgende XAML-Code zeigt ein Beispiel für eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Layout der `NoteEntryPage`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

In der Standardeinstellung die [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) geht davon aus eine vertikale Ausrichtung. Allerdings kann es in eine horizontale Ausrichtung geändert werden, durch Festlegen der [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft, um die [ `StackOrientation.Horizontal` ](xref:Xamarin.Forms.StackOrientation.Horizontal) -Enumerationsmember.

> [!NOTE]
> Die Größe der Sichten kann festgelegt werden, über die `HeightRequest` und `WidthRequest` Eigenschaften.

Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

Ein in XAML definiertes Objekt kann ein Ereignis auslösen, das in der CodeBehind-Datei behandelt wird. Das folgende Codebeispiel zeigt die `OnSaveButtonClicked` -Methode in der der Code-Behind für die `NoteEntryPage` -Klasse, die als Reaktion auf ausgeführt wird die [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) ereignisauslösung der *speichern* Schaltfläche .

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

Die `OnSaveButtonClicked` Methode speichert den Hinweis in der Datenbank, und navigieren Sie zurück zur vorherigen Seite.

> [!NOTE]
> Die CodeBehind-Datei für eine XAML-Klasse kann auf ein Objekt zugreifen, das mit dem Namen, der ihm mit dem `x:Name`-Attribut zugewiesen wurde, in XAML definiert wurde. So wie bei C#-Variablen muss der diesem Attribut zugewiesene Wert mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Die Verknüpfung der speichern-Schaltfläche auf der `OnSaveButtonClicked` -Methode geschieht im XAML-Markup für die `NoteEntryPage` Klasse:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Listen

Die [ `ListView` ](xref:Xamarin.Forms.ListView) wird eine Auflistung der Elemente vertikal in einer Liste angezeigt. Jedes Element in der `ListView` ist in einer einzelnen Zelle enthalten.

Das folgende Codebeispiel zeigt die [ `ListView` ](xref:Xamarin.Forms.ListView) aus der `NotesPage`:

```xaml
<ListView x:Name="listView"
          Margin="{StaticResource PageMargin}"
          ItemSelected="OnListViewItemSelected">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Text}"
                      Detail="{Binding Date}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Das Layout der einzelnen Zeilen in der [ `ListView` ](xref:Xamarin.Forms.ListView) wird definiert, in der [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Element, und verwendet Datenbindung, um Hinweise anzuzeigen, die von der Anwendung abgerufen werden. Die [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) -Eigenschaftensatz auf die Datenquelle im `NotesPage.xaml.cs`:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Dieser Code füllt die [ `ListView` ](xref:Xamarin.Forms.ListView) mit Anmerkungen in der Datenbank gespeichert.

Wenn eine Zeile ausgewählt ist, der [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis ausgelöst wird. Ein Ereignishandler, die mit dem Namen `OnListViewItemSelected`, ausgeführt wird, wenn das Ereignis ausgelöst wird:

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

Die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis kann das Objekt, das die Zelle über zugeordnet war zugreifen, die [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) Eigenschaft.

Weitere Informationen zu den [ `ListView` ](xref:Xamarin.Forms.ListView) Klasse, finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="navigation"></a>Navigation

Xamarin.Forms stellt abhängig von dem verwendeten [`Page`](xref:Xamarin.Forms.Page)-Typ eine Reihe unterschiedlicher Seitennavigationen bereit. Für [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Instanzen Navigation kann hierarchische oder modal sein. Weitere Informationen zur modalen Navigation finden Sie unter [Xamarin.Forms modale Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md).

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-und die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse bieten alternative Navigationen. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).

Bei der hierarchischen Navigation der [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Klasse wird verwendet, um die Navigation in eines Stapels von [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Objekte, und in der Rückwärtsrichtung wie gewünscht. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten. Für den Wechsel von einer auf die andere Seite überträgt eine Anwendung eine neue Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird. Wenn Sie zur vorherigen Seite zurückkehren möchten, entfernt die Anwendung die aktuelle Seite per Pop vom Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite.

Die `NavigationPage`-Klasse platziert außerdem eine Navigationsleiste oben auf der Seite, in der ein Titel und eine für die Plattform angemessene **Zurück**-Schaltfläche, über die man zurück auf die vorherige Seite wechseln kann, angezeigt wird.

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird als bezeichnet die *Stamm* auf der Seite der Anwendung und im folgenden Codebeispiel wird veranschaulicht, wie dies in den Anmerkungen zu dieser Anwendung erreicht wird:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

Alle [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen (Inhaltsseiteninstanzen) beinhalten eine [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation)-Eigenschaft zum Ändern des Seitenstapels. Diese Methoden sollten nur angewendet werden, wenn die Anwendung eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) einschließt. Um zur `NoteEntryPage` navigieren zu können, muss die [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page))-Methode wie im folgenden Codebeispiel angewendet werden:

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

Dadurch wird das neue `NoteEntryPage` Objekt, das auf den Navigationsstapel übertragen werden, wo Sie dann zur aktiven Seite.

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt. Um programmgesteuert zur ursprünglichen Seite zurückgehen zu können, muss das `NoteEntryPage`-Objekt die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode anwenden. Dies wird im folgenden Codebeispiel dargestellt:

```csharp
await Navigation.PopAsync();
```

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

## <a name="data-binding"></a>Datenbindung

Datenbindung wird verwendet, um die Vorgänge zu vereinfachen, über die in einer Xamarin.Forms-Anwendung Daten angezeigt werden und interagieren. Sie stellt eine Verbindung zwischen der Benutzeroberfläche und der zugrundeliegenden Anwendung her. Die [`BindableObject`](xref:Xamarin.Forms.BindableObject)-Klasse beinhaltet einen Großteil der Infrastruktur, um die Datenbindung zu unterstützen.

Bei der Datenbindung werden zwei Objekte miteinander verbunden: die *Quelle* und das *Ziel*. Das *Quellobjekt* stellt die Daten bereit. Das *Zielobjekt* verwendet Daten aus dem Quellobjekt (und zeigt diese häufig an). Z. B. eine [ `Editor` ](xref:Xamarin.Forms.Editor) (*Ziel* Objekt) verbindet sich häufig die [ `Text` ](xref:Xamarin.Forms.Editor.Text) an eine öffentliche `string` -Eigenschaft in einem *Quelle* Objekt. Das folgende Diagramm veranschaulicht die Bindungsbeziehung:

![](deepdive-images/data-binding.png "Datenbindung")

Der Hauptvorteil der Datenbindung ist, dass Sie Daten zwischen Ihren Ansichten und der Datenquelle nicht mehr synchronisieren müssen. Änderungen im *Quellobjekt* werden automatisch mithilfe von Push intern vom Bindungsframework in das *Zielobjekt* übertragen, und Änderungen im Zielobjekt können optional wieder zurück in das *Quellobjekt* übertragen werden.

Einrichten von Data-Bindung ist ein zweistufiger Prozess:

- Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) des *Zielobjekts* muss auf die *Quelle* festgelegt werden.
- Zwischen dem *Ziel* und der *Quelle* muss eine Bindung eingerichtet werden. In XAML wird dies mit der [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension)-Markuperweiterung erreicht.

In der Anmerkungen zu dieser Anwendung ist das Bindungsziel der [ `Editor` ](xref:Xamarin.Forms.Editor) , die zeigt, dass eine Nachricht an, während die `Note` Instanz festgelegt werden, als die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` ist die Bindung Quelle.

Die `BindingContext` von der `NoteEntryPage` bei der Seitennavigation, festgelegt ist, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnNoteAddedClicked(object sender, EventArgs e)
{
    await Navigation.PushAsync(new NoteEntryPage
    {
        BindingContext = new Note()
    });
}

async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        await Navigation.PushAsync(new NoteEntryPage
        {
            BindingContext = e.SelectedItem as Note
        });
    }
}
```

In der `OnNoteAddedClicked` -Methode, die ausgeführt wird, wenn die Anwendung eine neue Notiz hinzugefügt wird, die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` festgelegt ist, um ein neues `Note` Instanz. In der `OnListViewItemSelected` -Methode, die ausgeführt wird, wenn eine vorhandene Anmerkung ausgewählt ist die [ `ListView` ](xref:Xamarin.Forms.ListView), `BindingContext` von der `NoteEntryPage` ist, die dem ausgewählten `Note` -Instanz, die über zugegriffen wird die [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) Eigenschaft.

> [!IMPORTANT]
> Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der einzelnen *Zielobjekte* kann einzeln festgelegt werden, dies ist jedoch nicht erforderlich. `BindingContext` ist eine spezielle Eigenschaft, die von allen zugehörigen untergeordneten Elementen geerbt wird. Aus diesem Grund, wenn die `BindingContext` auf die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) nastaven NA hodnotu eine `Note` Instanz, die alle untergeordneten Elemente von der `ContentPage` verfügen über denselben `BindingContext`, und können eine Bindung an öffentliche Eigenschaften des der `Note`Objekt.

Die [ `Editor` ](xref:Xamarin.Forms.Editor) in `NoteEntryPage` bindet dann an die `Text` Eigenschaft der `Note` Objekt:

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

Zwischen der Eigenschaft [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) und der Eigenschaft `Text` des *Quellobjekts* wird eine Bindung eingerichtet. Änderungen in der `Editor` wird automatisch weitergegeben werden, um die `Note` Objekt. Auf ähnliche Weise, wenn Änderungen, um vorgenommen werden die `Note.Text` -Eigenschaft, die Xamarin.Forms-Bindungs-Engine aktualisiert auch den Inhalt des der `Editor`. Dies wird als *bidirektionale Bindung* bezeichnet.

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Format

Xamarin.Forms-Anwendungen enthalten häufig mehrere visuelle Elemente, die eine identische Darstellung haben. Die Darstellung jedes visuelle Element festlegen kann sein, sich wiederholende und fehleranfällig. Stattdessen können Stile erstellt werden, die definieren die Darstellung, und klicken Sie dann auf die erforderlichen visual Elemente angewendet.

Die [ `Style` ](xref:Xamarin.Forms.Style) Klasse gruppiert eine Auflistung von Eigenschaftswerten in ein Objekt, das Sie dann auf mehrere Instanzen für visuelles Element angewendet werden kann. Stile werden gespeichert, einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), entweder auf Anwendungsebene, die auf der Seitenebene oder die Ebene anzeigen. Auswählen des Installationsorts für das Definieren einer `Style` wirkt sich auf, wo sie verwendet werden kann:

- [`Style`](xref:Xamarin.Forms.Style) auf Anwendungsebene definierte Instanzen können in der gesamten Anwendung angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style) auf Seitenebene definierte Instanzen können auf der Seite und seinen untergeordneten Elementen angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style) Instanzen, die auf der Ebene für die Sicht definiert, können an die Ansicht und seiner untergeordneten Elemente angewendet werden.

> [!IMPORTANT]
> Alle Formatvorlagen, die in der gesamten Anwendung verwendet werden, werden im Ressourcenverzeichnis der Anwendung, damit eine Duplizierung verhindert gespeichert. Jedoch sollte nicht XAML, die für eine Seite spezifisch ist im Ressourcenverzeichnis der Anwendung enthalten sein, wie die Ressourcen beim Start der Anwendung nicht durch einen im Bedarfsfall klicken Sie dann analysiert werden.

Jede [ `Style` ](xref:Xamarin.Forms.Style) Instanz enthält eine Auflistung einer oder mehreren [ `Setter` ](xref:Xamarin.Forms.Setter) Objekte mit jeweils `Setter` müssen eine [ `Property` ](xref:Xamarin.Forms.Setter.Property) und ein [`Value`](xref:Xamarin.Forms.Setter.Value). Die `Property` ist der Name der bindbare Eigenschaft des Elements, der Stil angewendet wird, und die `Value` ist der Wert, der auf die Eigenschaft angewendet wird. Das folgende Codebeispiel zeigt eine Formatvorlage aus `NoteEntryPage`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

Dieses Format gilt für alle [ `Editor` ](xref:Xamarin.Forms.Editor) Instanzen auf der Seite.

Beim Erstellen einer [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft ist immer erforderlich.

> [!NOTE]
> Formatieren einer Xamarin.Forms-Anwendung erfolgt normalerweise mithilfe von XAML-Stile. Xamarin.Forms unterstützt jedoch auch Stile visuelle Elemente, die mithilfe von Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter [Formatieren von Xamarin.Forms-apps mithilfe von Cascading Stylesheets (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Weitere Informationen zu XAML-Stile, finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

### <a name="providing-platform-specific-styles"></a>Bereitstellen von plattformspezifischen Stile

Die `OnPlatform` Markuperweiterungen können Sie UI-Darstellung auf einer Basis pro Plattform anpassen:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.App">
    <Application.Resources>
        ...
        <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
        <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
        <Color x:Key="iOSNavigationBarTextColor">Black</Color>
        <Color x:Key="AndroidNavigationBarTextColor">White</Color>

        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                       Android={StaticResource AndroidNavigationBarColor}}" />
             <Setter Property="BarTextColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                       Android={StaticResource AndroidNavigationBarTextColor}}" />           
        </Style>
        ...
    </Application.Resources>
</Application>
```

Dies [ `Style` ](xref:Xamarin.Forms.Style) legt anderen [ `Color` ](xref:Xamarin.Forms.Color) Werte für die [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) und [ `BarTextColor` ](xref:Xamarin.Forms.NavigationPage.BarTextColor) Eigenschaften des [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), je nachdem, auf der Plattform, die verwendet wird.

Weitere Informationen über XAML-Markuperweiterungen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterungen)](~/xamarin-forms/xaml/markup-extensions/index.md). Informationen zu den `OnPlatform` Markuperweiterung, finden Sie unter [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="testing-and-deployment"></a>Testen und bereitstellen

Sowohl Visual Studio für Mac als auch Visual Studio stellen viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. Das Debuggen von Anwendungen ist ein üblicher Vorgang im Lebenszyklus der Anwendungsentwicklung und trägt dazu bei, Codeprobleme zu diagnostizieren. Weitere Informationen finden Sie unter [Festlegen eines Haltepunkts](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Step Through Code (Detailliertes Durchlaufen von Code)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) und [Output Information to the Log Window (Ausgabeinformationen an das Protokollfenster)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Simulatoren eigenen sich hervorragend für die Bereitstellung und das Testen einer Anwendung. Außerdem bieten sie nützliche Funktionen zum Testen von Anwendungen. Allerdings verwenden Benutzer die endgültige Anwendung nicht in einem Simulator. Daher sollte die Anwendungen frühzeitig und häufig auf echten Geräten getestet werden. Weitere Informationen zur Bereitstellung von iOS-Geräten finden Sie unter [Device Provisioning (Gerätebereitstellung)](~/ios/get-started/installation/device-provisioning/index.md). Weitere Informationen zur Bereitstellung von Android-Geräten finden Sie unter [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Nächste Schritte

Tieferer Einblick in diese verfügt über die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms untersucht. Vorgeschlagene nächste Schritte umfassen das Lesen über folgende Funktionen:

- Es gibt vier Hauptsteuerelementgruppen, mit denen die Benutzeroberfläche einer Xamarin.Forms-Anwendung erstellt wird. Weitere Informationen finden Sie unter [Steuerelementreferenz](~/xamarin-forms/user-interface/controls/index.md).
- Bei der Datenbindung werden die Eigenschaften von zwei Objekten verknüpft. Dadurch werden Änderungen an einer Eigenschaft automatisch in der anderen widergespiegelt. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- Xamarin.Forms stellt abhängig von dem verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).
- Stile können repetitives Markup reduzieren und erleichtern Änderungen an der Darstellung von Anwendungen. Weitere Informationen finden Sie unter [Styling Xamarin.Forms Apps (Formatieren von Xamarin.Forms-Apps)](~/xamarin-forms/user-interface/styles/index.md).
- XAML-Markuperweiterungen erweitern die Leistungsfähigkeit und Flexibilität von XAML, indem Elementattribute auch aus anderen Quellen als literalen Textzeichenfolgen festgelegt werden. Weitere Informationen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterung)](~/xamarin-forms/xaml/markup-extensions/index.md).
- Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Ansichten zu definieren. Weitere Informationen finden Sie unter [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Die einzelnen Seiten, Layouts und Ansichten werden auf jeder Plattform auf unterschiedliche Weise über eine `Renderer`-Klasse gerendert. Diese erstellt ein natives Steuerelement, ordnet dieses auf dem Bildschirm an und fügt das im freigegebenen Code angegebene Verhalten hinzu. Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Weitere Informationen finden Sie unter [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) (Benutzerdefinierte Renderer).
- Zudem können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden. Effekte werden in plattformspezifischen Projekten erstellt, indem Unterklassen für die [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2)-Klasse erstellt werden. Sie werden verarbeitet, indem sie an das entsprechende Xamarin.Forms-Steuerelement angefügt werden. Weitere Informationen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).
- Freigegebener Code kann über die [`DependencyService`](xref:Xamarin.Forms.DependencyService)-Klasse auf eine native Funktion zugreifen. Weitere Informationen finden Sie unter [Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) (Zugreifen auf native Funktionen über DependencyService).

Alternativ enthält das Buch [_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) (Erstellen von mobilen Apps mit Xamarin.Forms) von Charles Petzold weitere Informationen zu Xamarin.Forms. Das Buch ist als PDF und in zahlreichen anderen E-Book-Formaten erhältlich.

## <a name="related-links"></a>Verwandte Links

- [eXtensible Application Markup Language (XAML)](~/xamarin-forms/xaml/index.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Steuerelementreferenz](~/xamarin-forms/user-interface/controls/index.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Beispiele für die ersten Schritte](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/)
- [Xamarin.Forms-API-Referenz](xref:Xamarin.Forms)
- [Free Self-Guided Learning (Kostenloses eigenständiges Lernen) (Video)](https://university.xamarin.com/self-guided/)
