---
title: Xamarin. Forms-Schnellstart Deep Dive
description: In diesem Artikel werden die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms erläutert. Zu den behandelten Themen zählen die Struktur einer Xamarin.Forms-Anwendung, die Architektur- und Anwendungsgrundlagen sowie die Benutzeroberfläche.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 997c9e023a743b8e5128ffc566e50da63652f945
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739003"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin. Forms-Schnellstart Deep Dive

In der [xamarin. Forms-Schnellstart](~/get-started/index.yml)Anleitung wurde die Notes-Anwendung erstellt. In diesem Artikel wird dieser Vorgang noch einmal genauer betrachtet, um ein Verständnis für die Funktionsweise von Xamarin.Forms-Anwendungen zu erarbeiten.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

In Visual Studio wird Code in *Projektmappen* und *Projekten* organisiert. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Notes-Anwendung besteht aus einer Projekt Mappe, die vier Projekte enthält, wie im folgenden Screenshot zu sehen:

![](deepdive-images/vs/solution.png "Projektmappen-Explorer von Visual Studio")

Diese Projekte sind folgende:

- Hinweise – dieses Projekt ist das .NET Standard Bibliotheksprojekt, das den gesamten freigegebenen Code und die freigegebene Benutzeroberfläche enthält.
- Notes. Android – dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-Anwendung.
- Notes. IOS – dieses Projekt enthält IOS-spezifischen Code und ist der Einstiegspunkt für die IOS-Anwendung.
- Notes. UWP – dieses Projekt enthält universelle Windows-Plattform (UWP) spezifischen Code und ist der Einstiegspunkt für die UWP-Anwendung.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie einer xamarin. Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt der Notizen .NET Standard Bibliotheksprojekt in Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Inhalt des Phoneword-Projekts von .NET Standard")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **Nuget** &ndash; die nuget-Pakete xamarin. Forms und SQLite-net-PCL, die dem Projekt hinzugefügt wurden.
- **SDK**: Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Die Codeorganisation in [Visual Studio für Mac](/visualstudio/mac/) baut auf Visual Studio auf und gliedert sich in *Projektmappen* und *Projekte*. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Notes-Anwendung besteht aus einer Projekt Mappe, die drei Projekte enthält, wie im folgenden Screenshot zu sehen:

![](deepdive-images/vsmac/solution.png "Visual Studio für Mac: Projektmappenbereich")

Diese Projekte sind folgende:

- Hinweise – dieses Projekt ist das .NET Standard Bibliotheksprojekt, das den gesamten freigegebenen Code und die freigegebene Benutzeroberfläche enthält.
- Notes. Android – dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für Android-Anwendungen.
- Notes. IOS – dieses Projekt enthält IOS-spezifischen Code und ist der Einstiegspunkt für IOS-Anwendungen.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomie einer xamarin. Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt der Notizen .NET Standard Bibliotheksprojekt in Visual Studio für Mac:

![](deepdive-images/vsmac/net-standard-project.png "Inhalt des Phoneword .NET Standard-Bibliotheksprojekts")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **Nuget** &ndash; die nuget-Pakete xamarin. Forms und SQLite-net-PCL, die dem Projekt hinzugefügt wurden.
- **SDK**: Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end

Das Projekt besteht aus mehreren Dateien:

- **Data\notedatabase.cs** – diese Klasse enthält Code zum Erstellen der Datenbank, zum Lesen von Daten, zum Schreiben von Daten in die Datenbank und zum Löschen von Daten aus der Datenbank.
- **Models\note.cs** – diese Klasse definiert ein `Note` Modell, dessen Instanzen Daten zu jeder Notiz in der Anwendung speichern.
- **App.xaml**: Das XAML-Markup für die `App`-Klasse, die ein Ressourcenverzeichnis für die Anwendung definiert
- **App.xaml.cs**: Das CodeBehind-Modell für die `App`-Klasse, das die erste Seite instanziiert, die von der Anwendung auf jeder Plattform angezeigt wird, und für die Behandlung von Ereignissen im App-Lebenszyklus zuständig ist
- **AssemblyInfo.cs** – diese Datei enthält ein Anwendungs Attribut für das Projekt, das auf Assemblyebene angewendet wird.
- **NotesPage. XAML** – das XAML-Markup für `NotesPage` die-Klasse, die die Benutzeroberfläche für die Seite definiert, die beim Starten der Anwendung angezeigt wird.
- **NotesPage.XAML.cs** – der Code Behind für die `NotesPage` -Klasse, die die Geschäftslogik enthält, die ausgeführt wird, wenn der Benutzer mit der Seite interagiert.
- **Noteentrypage. XAML** – das XAML-Markup für `NoteEntryPage` die-Klasse, die die Benutzeroberfläche für die Seite definiert, die angezeigt wird, wenn der Benutzer eine Notiz eingibt.
- **NoteEntryPage.XAML.cs** – der Code Behind für die `NoteEntryPage` -Klasse, die die Geschäftslogik enthält, die ausgeführt wird, wenn der Benutzer mit der Seite interagiert.

Weitere Informationen zum Aufbau einer Xamarin.iOS-Anwendung finden Sie unter [Hello, iOS: Deep Dive (Hallo iOS: Ausführliche Informationen)](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Weitere Informationen zum Aufbau einer Xamarin.Android-Anwendung finden Sie unter [Hello, Android: Deep Dive (Hallo Android: Ausführliche Informationen)](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektur-und Anwendungs Grundlagen

Eine Xamarin.Forms-Anwendung ist genauso aufgebaut wie eine traditionelle plattformübergreifende Anwendung. Freigegebener Code wird in der Regel in einer .NET Standard-Bibliothek platziert und von plattformspezifischen Anwendungen genutzt. Das folgende Diagramm zeigt eine Übersicht über diese Beziehung für die Notes-Anwendung:

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Notes-Architektur")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Notes-Architektur")

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

Mit diesem Code wird `MainPage` die-Eigenschaft `App` der-Klasse [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) auf eine-Instanz fest `NotesPage` gelegt, deren Inhalt eine-Instanz ist.

Außerdem enthält die Datei **AssemblyInfo.cs** ein einzelnes Anwendungs Attribut, das auf Assemblyebene angewendet wird:

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

Das [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) -Attribut schaltet den XAML-Compiler ein, sodass XAML direkt in die zwischen Sprache kompiliert wird. Weitere Informationen finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Starten der Anwendung auf jeder Plattform

### <a name="ios"></a>iOS

Zum Starten der ersten xamarin. Forms-Seite in ios definiert das Projekt Notes. IOS die `AppDelegate` Klasse, die von der `FormsApplicationDelegate` -Klasse erbt:

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

Zum Starten der xamarin. Forms-Startseite in Android enthält das Projekt Notes. Android Code, der eine `Activity` mit dem `MainLauncher` -Attribut erstellt, wobei die Aktivität von der `FormsAppCompatActivity` -Klasse erbt:

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

Dadurch wird die UWP-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Die ursprüngliche xamarin. Forms-Seite wird von der `MainPage` -Klasse gestartet:

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
> Universelle Windows-Plattform-Apps können mit xamarin. Forms erstellt werden, jedoch nur mithilfe von Visual Studio unter Windows.

::: zone-end

## <a name="user-interface"></a>Benutzeroberfläche

Zum Erstellen der Benutzeroberfläche einer xamarin. Forms-Anwendung sind vier Haupt Steuerungsgruppen vorhanden:

1. **Seiten**: Xamarin.Forms-Seiten stellen Anzeigen plattformübergreifender mobiler Anwendungen dar. Die Notes-Anwendung verwendet [`ContentPage`](xref:Xamarin.Forms.ContentPage) die-Klasse, um einzelne Bildschirme anzuzeigen. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md) (Xamarin.Forms-Seiten).
1. **Ansichten**: Xamarin.Forms-Ansichten sind die Steuerelemente auf der Benutzeroberfläche, z.B. Bezeichnungen, Schaltflächen und Texteingabefelder. Die fertige Notes-Anwendung verwendet [`ListView`](xref:Xamarin.Forms.ListView)die [`Editor`](xref:Xamarin.Forms.Editor)Sichten, [`Button`](xref:Xamarin.Forms.Button) und. Weitere Informationen zu Ansichten finden Sie unter [Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md) (Xamarin.Forms-Ansichten).
1. **Layouts**: Xamarin.Forms-Layouts sind Container, die zum Erstellen von Ansichten in Form von logischen Strukturen verwendet werden. Die Notes-Anwendung verwendet [`StackLayout`](xref:Xamarin.Forms.StackLayout) die-Klasse zum Anordnen von Sichten in einem vertikalen [`Grid`](xref:Xamarin.Forms.Grid) Stapel und die-Klasse, um Schaltflächen horizontal anzuordnen. Weitere Informationen zu Layouts finden Sie unter [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md) (Xamarin.Forms-Layouts).
1. **Zellen**: Xamarin.Forms-Zellen sind spezielle Elemente für Listenelemente und beschreiben, wie die einzelnen Elemente in einer Liste gezeichnet werden sollen. Die Notes-Anwendung verwendet [`TextCell`](xref:Xamarin.Forms.TextCell) das, um für jede Zeile in der Liste zwei Elemente anzuzeigen. Weitere Informationen zu Zellen finden Sie unter [Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md) (Xamarin.Forms-Zellen).

Zur Laufzeit wird jedes Steuerelement seinem nativen Äquivalent zugeordnet. Dieses wird dann gerendert.

### <a name="layout"></a>Layout

Die Notes-Anwendung verwendet [`StackLayout`](xref:Xamarin.Forms.StackLayout) das, um die plattformübergreifende Anwendungsentwicklung zu vereinfachen, indem Sichten unabhängig von der Bildschirmgröße automatisch auf dem Bildschirm angeordnet werden. Alle untergeordneten Elemente werden nacheinander horizontal oder vertikal in der Reihenfolge positioniert, in der sie hinzugefügt wurden. Wie viel Speicherplatz das `StackLayout` verwendet, hängt davon ab, wie die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) festgelegt sind. Das `StackLayout` versucht jedoch standardmäßig, den gesamten Bildschirm zu verwenden.

Der folgende XAML-Code zeigt ein Beispiel für die [`StackLayout`](xref:Xamarin.Forms.StackLayout) `NoteEntryPage`Verwendung eines zum Layout von:

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

Standardmäßig geht [`StackLayout`](xref:Xamarin.Forms.StackLayout) die vertikale Ausrichtung davon aus. Sie kann jedoch in eine horizontale Ausrichtung geändert werden, indem die [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) -Eigenschaft auf den [`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) -Enumerationsmember festgelegt wird.

> [!NOTE]
> Die Größe der Sichten kann über die `HeightRequest` Eigenschaften und `WidthRequest` festgelegt werden.

Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

Ein in XAML definiertes Objekt kann ein Ereignis auslösen, das in der CodeBehind-Datei behandelt wird. Im folgenden Codebeispiel wird die `OnSaveButtonClicked` -Methode im Code-Behind für die `NoteEntryPage` -Klasse veranschaulicht, die als Reaktion auf das [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis ausgeführt wird, das auf der Schaltfläche " *Speichern* " ausgelöst wird.

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

Die `OnSaveButtonClicked` -Methode speichert den Hinweis in der-Datenbank und navigiert zurück zur vorherigen Seite.

> [!NOTE]
> Die CodeBehind-Datei für eine XAML-Klasse kann auf ein Objekt zugreifen, das mit dem Namen, der ihm mit dem `x:Name`-Attribut zugewiesen wurde, in XAML definiert wurde. So wie bei C#-Variablen muss der diesem Attribut zugewiesene Wert mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Die Verknüpfung der Schaltfläche "Speichern" `OnSaveButtonClicked` zur-Methode tritt im XAML-Markup `NoteEntryPage` für die-Klasse auf:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Listen

Der [`ListView`](xref:Xamarin.Forms.ListView) ist dafür verantwortlich, eine Auflistung von Elementen in einer Liste vertikal anzuzeigen. Jedes Element in der `ListView` ist in einer einzelnen Zelle enthalten.

Im folgenden Codebeispiel wird die [`ListView`](xref:Xamarin.Forms.ListView) aus dem `NotesPage`veranschaulicht:

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

Das Layout der einzelnen Zeilen in der [`ListView`](xref:Xamarin.Forms.ListView) wird innerhalb des [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) -Elements definiert und verwendet die Datenbindung, um alle von der Anwendung abgerufenen Notizen anzuzeigen. Die [`ListView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) -Eigenschaft wird auf die Datenquelle in `NotesPage.xaml.cs`festgelegt:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Dieser Code füllt den [`ListView`](xref:Xamarin.Forms.ListView) mit allen in der Datenbank gespeicherten Notizen auf.

Wenn eine Zeile in der [`ListView`](xref:Xamarin.Forms.ListView)ausgewählt wird, wird das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis ausgelöst. Ein Ereignishandler mit dem `OnListViewItemSelected`Namen wird ausgeführt, wenn das Ereignis ausgelöst wird:

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

Das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis kann auf das-Objekt, das der Zelle zugeordnet war [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) , über die-Eigenschaft zugreifen.

Weitere Informationen [`ListView`](xref:Xamarin.Forms.ListView) zur-Klasse finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="navigation"></a>Navigation

Xamarin.Forms stellt abhängig von dem verwendeten [`Page`](xref:Xamarin.Forms.Page)-Typ eine Reihe unterschiedlicher Seitennavigationen bereit. Für [`ContentPage`](xref:Xamarin.Forms.ContentPage) Instanzen kann die Navigation hierarchisch oder modal sein. Weitere Informationen zur modalen Navigation finden Sie unter [xamarin. Forms Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md).

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-und die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse bieten alternative Navigationen. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).

In der hierarchischen Navigation [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) wird die-Klasse verwendet, um nach Belieben [`ContentPage`](xref:Xamarin.Forms.ContentPage) durch einen Stapel von Objekten zu navigieren. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten. Für den Wechsel von einer auf die andere Seite überträgt eine Anwendung eine neue Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird. Wenn Sie zur vorherigen Seite zurückkehren möchten, entfernt die Anwendung die aktuelle Seite per Pop vom Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite.

Die `NavigationPage`-Klasse platziert außerdem eine Navigationsleiste oben auf der Seite, in der ein Titel und eine für die Plattform angemessene **Zurück**-Schaltfläche, über die man zurück auf die vorherige Seite wechseln kann, angezeigt wird.

Die erste Seite, die einem Navigations Stapel hinzugefügt wird, wird als Stamm Seite der Anwendung bezeichnet. das folgende Codebeispiel zeigt, wie dies in der Notes-Anwendung erfolgt:

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

Dies bewirkt, dass `NoteEntryPage` das neue-Objekt auf dem Navigations Stapel abgelegt wird, wo es zur aktiven Seite wird.

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt. Um programmgesteuert zur ursprünglichen Seite zurückgehen zu können, muss das `NoteEntryPage`-Objekt die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode anwenden. Dies wird im folgenden Codebeispiel dargestellt:

```csharp
await Navigation.PopAsync();
```

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

## <a name="data-binding"></a>Datenbindung

Datenbindung wird verwendet, um die Vorgänge zu vereinfachen, über die in einer Xamarin.Forms-Anwendung Daten angezeigt werden und interagieren. Sie stellt eine Verbindung zwischen der Benutzeroberfläche und der zugrundeliegenden Anwendung her. Die [`BindableObject`](xref:Xamarin.Forms.BindableObject)-Klasse beinhaltet einen Großteil der Infrastruktur, um die Datenbindung zu unterstützen.

Bei der Datenbindung werden zwei Objekte miteinander verbunden: die *Quelle* und das *Ziel*. Das *Quellobjekt* stellt die Daten bereit. Das *Zielobjekt* verwendet Daten aus dem Quellobjekt (und zeigt diese häufig an). So bindet z. [`Editor`](xref:Xamarin.Forms.Editor) b. ein (*Ziel* Objekt) seine [`Text`](xref:Xamarin.Forms.Editor.Text) -Eigenschaft in der `string` Regel an eine öffentliche Eigenschaft in einem *Quell* Objekt. Das folgende Diagramm veranschaulicht die Bindungsbeziehung:

![](deepdive-images/data-binding.png "Datenbindung")

Der Hauptvorteil der Datenbindung ist, dass Sie Daten zwischen Ihren Ansichten und der Datenquelle nicht mehr synchronisieren müssen. Änderungen im *Quellobjekt* werden automatisch mithilfe von Push intern vom Bindungsframework in das *Zielobjekt* übertragen, und Änderungen im Zielobjekt können optional wieder zurück in das *Quellobjekt* übertragen werden.

Das Einrichten der Datenbindung ist ein zweistufiger Prozess:

- Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) des *Zielobjekts* muss auf die *Quelle* festgelegt werden.
- Zwischen dem *Ziel* und der *Quelle* muss eine Bindung eingerichtet werden. In XAML wird dies mit der [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension)-Markuperweiterung erreicht.

In der Notes-Anwendung ist das Bindungs Ziel das [`Editor`](xref:Xamarin.Forms.Editor) , das einen Hinweis anzeigt, während `Note` die `NoteEntryPage` Instanz, die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) als festgelegt ist, die Bindungs Quelle ist.

`BindingContext` Der`NoteEntryPage` von wird während der Seitennavigation festgelegt, wie im folgenden Codebeispiel gezeigt:

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

In der `OnNoteAddedClicked` -Methode, die ausgeführt wird, wenn der Anwendung ein neuer Hinweis hinzugefügt wird [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) , `NoteEntryPage` wird der von auf eine `Note` neue-Instanz festgelegt. In der `OnListViewItemSelected` -Methode, die ausgeführt wird, wenn ein vorhandener Hinweis [`ListView`](xref:Xamarin.Forms.ListView)in ausgewählt wird `BindingContext` , `NoteEntryPage` wird der der auf die ausgewählte `Note` Instanz festgelegt, auf die über [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) die-Eigenschaft zugegriffen wird.

> [!IMPORTANT]
> Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der einzelnen *Zielobjekte* kann einzeln festgelegt werden, dies ist jedoch nicht erforderlich. `BindingContext` ist eine spezielle Eigenschaft, die von allen zugehörigen untergeordneten Elementen geerbt wird. `BindingContext` Wenn für die [`ContentPage`](xref:Xamarin.Forms.ContentPage) auf eine `Note` - `BindingContext` `Note` Instanz festgelegt ist, habenalleuntergeordnetenElementevondenselbenundkönnenanöffentlicheEigenschaftendes-Objektsgebundenwerden.`ContentPage`

Der [`Editor`](xref:Xamarin.Forms.Editor) in `Text` bindetdann`Note` an die-Eigenschaft des-Objekts: `NoteEntryPage`

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

Zwischen der Eigenschaft [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) und der Eigenschaft `Text` des *Quellobjekts* wird eine Bindung eingerichtet. Änderungen, die `Editor` in vorgenommen werden, werden automatisch an `Note` das-Objekt weitergegeben. Wenn Änderungen an der `Note.Text` -Eigenschaft vorgenommen werden, aktualisiert auch die xamarin. Forms-Bindungs-Engine den Inhalt `Editor`von. Dies wird als *bidirektionale Bindung* bezeichnet.

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Format

Xamarin. Forms-Anwendungen enthalten häufig mehrere visuelle Elemente, die identisch sind. Das Festlegen der Darstellung der einzelnen visuellen Elemente kann sich wiederholen und fehleranfällig sein. Stattdessen können Stile erstellt werden, die die Darstellung definieren und dann auf die erforderlichen visuellen Elemente angewendet werden.

Die [`Style`](xref:Xamarin.Forms.Style) -Klasse gruppiert eine Auflistung von Eigenschafts Werten in ein Objekt, das dann auf mehrere visuelle Element Instanzen angewendet werden kann. Stile werden in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Anwendungs-, Seiten-oder Ansichts Ebene gespeichert. Die Auswahl des Speicher Orts `Style` für die Verwendung von wirkt sich auf die Verwendung von aus:

- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die auf Anwendungsebene definiert werden, können in der gesamten Anwendung angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)auf der Seitenebene definierte Instanzen können auf die Seite und ihre untergeordneten Elemente angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)auf der Ansichts Ebene definierte Instanzen können auf die Ansicht und deren untergeordnete Elemente angewendet werden.

> [!IMPORTANT]
> Alle Stile, die in der gesamten Anwendung verwendet werden, werden im Ressourcen Wörterbuch der Anwendung gespeichert, um Duplizierungen zu vermeiden. XAML-Code, der für eine Seite spezifisch ist, sollte jedoch nicht in das Ressourcen Wörterbuch der Anwendung eingeschlossen werden, da die Ressourcen beim Anwendungsstart analysiert werden, anstatt Sie für eine Seite zu benötigen.

Jede [`Style`](xref:Xamarin.Forms.Style) Instanz enthält eine Auflistung von einem oder mehreren [`Setter`](xref:Xamarin.Forms.Setter) -Objekten, wobei `Setter` jede eine [`Property`](xref:Xamarin.Forms.Setter.Property) -und [`Value`](xref:Xamarin.Forms.Setter.Value)eine-Instanz enthält. Der `Property` ist der Name der bindbaren Eigenschaft des Elements, auf das der Stil angewendet wird, `Value` und ist der Wert, der auf die Eigenschaft angewendet wird. Das folgende Codebeispiel zeigt einen Stil aus `NoteEntryPage`:

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

Dieser Stil wird auf alle [`Editor`](xref:Xamarin.Forms.Editor) Instanzen auf der Seite angewendet.

Beim Erstellen eines [`Style`](xref:Xamarin.Forms.Style)ist die [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft immer erforderlich.

> [!NOTE]
> Das Formatieren einer xamarin. Forms-Anwendung erfolgt in der Regel mithilfe von XAML-Stilen. Xamarin. Forms unterstützt jedoch auch das Formatieren visueller Elemente mithilfe Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter Formatieren von [xamarin. Forms-apps mit Cascading Stylesheets (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

### <a name="providing-platform-specific-styles"></a>Bereitstellen plattformspezifischer Stile

Die `OnPlatform` Markup Erweiterungen ermöglichen es Ihnen, die Darstellung der Benutzeroberfläche plattformspezifisch anzupassen:

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

Dadurch [`Style`](xref:Xamarin.Forms.Style) werden unter [`Color`](xref:Xamarin.Forms.Color) schiedliche Werte für [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) die-Eigenschaft [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)und die- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor) Eigenschaft von festgelegt, abhängig von der verwendeten Plattform.

Weitere Informationen über XAML-Markuperweiterungen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterungen)](~/xamarin-forms/xaml/markup-extensions/index.md). Weitere Informationen `OnPlatform` zur Markup Erweiterung finden Sie unter [onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="testing-and-deployment"></a>Tests und Bereitstellung

Sowohl Visual Studio für Mac als auch Visual Studio stellen viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. Das Debuggen von Anwendungen ist ein üblicher Vorgang im Lebenszyklus der Anwendungsentwicklung und trägt dazu bei, Codeprobleme zu diagnostizieren. Weitere Informationen finden Sie unter [Festlegen eines Haltepunkts](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Step Through Code (Detailliertes Durchlaufen von Code)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) und [Output Information to the Log Window (Ausgabeinformationen an das Protokollfenster)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Simulatoren eigenen sich hervorragend für die Bereitstellung und das Testen einer Anwendung. Außerdem bieten sie nützliche Funktionen zum Testen von Anwendungen. Allerdings verwenden Benutzer die endgültige Anwendung nicht in einem Simulator. Daher sollte die Anwendungen frühzeitig und häufig auf echten Geräten getestet werden. Weitere Informationen zur Bereitstellung von iOS-Geräten finden Sie unter [Device Provisioning (Gerätebereitstellung)](~/ios/get-started/installation/device-provisioning/index.md). Weitere Informationen zur Bereitstellung von Android-Geräten finden Sie unter [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Nächste Schritte

Dieser ausführliche Einblick hat die Grundlagen der Anwendungsentwicklung mithilfe von xamarin. Forms untersucht. Vorgeschlagene nächste Schritte umfassen das Lesen über folgende Funktionen:

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

- [Extensible Application Markup Language (XAML)](~/xamarin-forms/xaml/index.yml)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Steuerelementreferenz](~/xamarin-forms/user-interface/controls/index.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Beispiele für die ersten Schritte](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms%20get%20started)
- [Xamarin.Forms-API-Referenz](xref:Xamarin.Forms)
- [Free Self-Guided Learning (Kostenloses eigenständiges Lernen) (Video)](https://university.xamarin.com/self-guided/)
