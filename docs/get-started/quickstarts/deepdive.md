---
title: 'title: "Xamarin.Forms – Ausführliche Erläuterungen zum Schnellstart" description: "In diesem Artikel werden die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms erläutert.'
description: 'Zu den behandelten Themen zählen die Struktur einer Xamarin.Forms-Anwendung, die Architektur- und Anwendungsgrundlagen sowie die Benutzeroberfläche." zone_pivot_groups: platform ms.topic: quickstart ms.prod: xamarin ms.custom: video ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 11/27/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.custom: video
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1bfb76f71a2ac9d8bc9ae84152501909000b9623
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84132521"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Xamarin.Forms Ausführliche Erläuterungen zum Schnellstart

Im [Xamarin.Forms-Schnellstart](~/get-started/index.yml) wurde die Notes-Anwendung erstellt. In diesem Artikel wird dieser Vorgang noch einmal genauer betrachtet, um ein Verständnis für die Funktionsweise von Xamarin.Forms-Anwendungen zu entwickeln.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Einführung in Visual Studio

In Visual Studio wird Code in *Projektmappen* und *Projekten* organisiert. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Notes-Anwendung besteht wie im folgenden Screenshot gezeigt aus einer Projektmappe mit vier Projekten:

![](deepdive-images/vs/solution.png "Visual Studio Solution Explorer")

Diese Projekte sind folgende:

- Notes: Dieses Projekt ist das .NET Standard-Klassenbibliotheksprojekt, das den gesamten freigegebenen Code und die gesamte freigegebene Benutzeroberfläche enthält.
- Notes.Android: Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für die Android-Anwendung.
- Notes.iOS: Dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für die iOS-Anwendung.
- Notes.UWP: Dieses Projekt enthält für die Universelle Windows-Plattform (UWP) spezifischen Code und ist der Einstiegspunkt für die UWP-Anwendung.

## <a name="anatomy-of-a-xamarinforms-application"></a>Struktur einer Xamarin.Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt des .NET Standard-Bibliotheksprojekts „Notes“ in Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard Project Contents")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **NuGet:** Die NuGet-Pakete für Xamarin.Forms und „sqlite-net-pcl“, die dem Projekt hinzugefügt wurden
- **SDK:** Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Einführung in Visual Studio für Mac

Die Codeorganisation in [Visual Studio für Mac](/visualstudio/mac/) baut auf Visual Studio auf und gliedert sich in *Projektmappen* und *Projekte*. Eine Projektmappe ist ein Container, der mindestens ein Projekt enthält. Ein Projekt kann z.B. eine Anwendung, eine unterstützende Bibliothek oder eine Testanwendung sein. Die Notes-Anwendung besteht wie im folgenden Screenshot gezeigt aus einer Projektmappe mit drei Projekten:

![](deepdive-images/vsmac/solution.png "Visual Studio for Mac Solution Pane")

Diese Projekte sind folgende:

- Notes: Dieses Projekt ist das .NET Standard-Klassenbibliotheksprojekt, das den gesamten freigegebenen Code und die gesamte freigegebene Benutzeroberfläche enthält.
- Notes.Android: Dieses Projekt enthält Android-spezifischen Code und ist der Einstiegspunkt für Android-Anwendungen.
- Notes.iOS: Dieses Projekt enthält iOS-spezifischen Code und ist der Einstiegspunkt für iOS-Anwendungen.

## <a name="anatomy-of-a-xamarinforms-application"></a>Struktur einer Xamarin.Forms-Anwendung

Der folgende Screenshot zeigt den Inhalt des .NET Standard-Bibliotheksprojekts „Notes“ in Visual Studio für Mac:

![](deepdive-images/vsmac/net-standard-project.png "Phoneword .NET Standard Library Project Contents")

Das Projekt verfügt über den Knoten **Abhängigkeiten**, der die Knoten **NuGet** und **SDK** enthält:

- **NuGet:** Die NuGet-Pakete für Xamarin.Forms und „sqlite-net-pcl“, die dem Projekt hinzugefügt wurden
- **SDK:** Das Metapaket `NETStandard.Library` verweist auf alle NuGet-Pakete, die .NET Standard definieren.

::: zone-end

Das Projekt besteht aus mehreren Dateien:

- **Data\NoteDatabase.cs:** Diese Klasse enthält Code, mit dem die Datenbank erstellt wird und Daten aus ihr gelesen bzw. in sie geschrieben und aus ihr gelöscht werden.
- **Models\Note.cs:** Diese Klasse definiert ein `Note`-Modell, dessen Instanzen Daten über jede Notiz in der Anwendung speichert.
- **App.xaml**: Das XAML-Markup für die `App`-Klasse, die ein Ressourcenverzeichnis für die Anwendung definiert
- **App.xaml.cs**: Das CodeBehind-Modell für die `App`-Klasse, das die erste Seite instanziiert, die von der Anwendung auf jeder Plattform angezeigt wird, und für die Behandlung von Ereignissen im App-Lebenszyklus zuständig ist
- **AssemblyInfo.cs:** Diese Datei enthält ein Anwendungsattribut für das Projekt, das auf Assemblyebene angewendet wird.
- **NotesPage.xaml:** das XAML-Markup für die `NotesPage`-Klasse, das die Benutzeroberfläche für die Seite definiert, die beim Start der Anwendung angezeigt wird
- **NotesPage.xaml.cs:** das CodeBehind für die `NotesPage`-Klasse, das die Geschäftslogik enthält, die ausgeführt wird, wenn der Benutzer mit der Seite interagiert
- **NoteEntryPage.xaml:** das XAML-Markup für die `NoteEntryPage`-Klasse, das die Benutzeroberfläche für die Seite definiert, die angezeigt wird, wenn der Benutzer eine Notiz eingibt
- **NoteEntryPage.xaml.cs:** das CodeBehind für die `NoteEntryPage`-Klasse, das die Geschäftslogik enthält, die ausgeführt wird, wenn der Benutzer mit der Seite interagiert

Weitere Informationen zum Aufbau einer Xamarin.iOS-Anwendung finden Sie unter [Hello, iOS: Deep Dive (Hallo iOS: Ausführliche Informationen)](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Weitere Informationen zum Aufbau einer Xamarin.Android-Anwendung finden Sie unter [Hello, Android: Deep Dive (Hallo Android: Ausführliche Informationen)](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Architektur und Anwendungsgrundlagen

Eine Xamarin.Forms-Anwendung ist genauso aufgebaut wie eine traditionelle plattformübergreifende Anwendung. Freigegebener Code wird in der Regel in einer .NET Standard-Bibliothek platziert und von plattformspezifischen Anwendungen genutzt. Die folgende Abbildung bietet für die Notes-Anwendung einen Überblick über diese Beziehung:

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Notes Architecture")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Notes Architecture")

::: zone-end

Um die Wiederverwendung von Startcode zu maximieren, enthalten Xamarin.Forms-Anwendungen eine einzelne Klasse namens `App`, die die erste Seite instanziiert, die von der Anwendung wie im folgenden Codebeispiel gezeigt auf jeder Plattform angezeigt wird:

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

Mit diesem Code wird die Eigenschaft `MainPage` der Klasse `App` auf eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Instanz festgelegt, die eine `NotesPage`-Instanz beinhaltet.

Außerdem enthält die Datei **AssemblyInfo.cs** ein einzelnes Anwendungsattribut, das auf Assemblyebene angewendet wird:

```csharp
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
```

Das Attribut [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) aktiviert den XAML-Compiler, sodass XAML direkt in der Zwischensprache kompiliert wird. Weitere Informationen finden Sie unter [XAML Compilation (XAML-Kompilierung)](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Starten der Anwendung auf den verschiedenen Plattformen

### <a name="ios"></a>iOS

Zum Aufrufen der Xamarin.Forms-Startseite in iOS definiert das Notes.iOS-Projekt die Klasse `AppDelegate`, die von der Klasse `FormsApplicationDelegate` erbt:

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

Mit der `FinishedLaunching`-Außerkraftsetzung wird das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode initialisiert. Dadurch wird die iOS-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor der Stammansichtscontroller durch den Aufruf der `LoadApplication`-Methode festgelegt wird.

### <a name="android"></a>Android

Zum Aufrufen der Xamarin.Forms-Startseite in Android enthält das Notes.Android-Projekt Code, der eine Aktivität (`Activity`) mit dem Attribut `MainLauncher` erstellt, wobei die Aktivität von der Klasse `FormsAppCompatActivity` erbt:

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

Mit der `OnCreate`-Außerkraftsetzung wird das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode initialisiert. Dadurch wird die Android-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor die Xamarin.Forms-Anwendung geladen wird.

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

In UWP-Anwendungen (Universelle Windows-Plattform) wird die `Init`-Methode, die das Xamarin.Forms-Framework initialisiert, über die `App`-Klasse aufgerufen:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Dadurch wird die UWP-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Die Xamarin.Forms-Startseite wird von der Klasse `MainPage` gestartet:

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
> UWP-Apps (Universelle Windows-Plattform) können mit Xamarin.Forms erstellt werden, jedoch nur mit Visual Studio unter Windows.

::: zone-end

## <a name="user-interface"></a>Benutzeroberfläche

Es gibt vier Hauptsteuerelementgruppen, mit denen die Benutzeroberfläche einer Xamarin.Forms-Anwendung erstellt wird:

1. **Seiten**: Xamarin.Forms-Seiten stellen Bildschirme plattformübergreifender mobiler Anwendungen dar. Die Notes-Anwendung verwendet die [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse, um einzelne Bildschirme anzuzeigen. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms-Seiten](~/xamarin-forms/user-interface/controls/pages.md).
1. **Ansichten**: Xamarin.Forms-Ansichten sind die auf der Benutzeroberfläche angezeigten Steuerelemente, z. B. Bezeichnungen, Schaltflächen und Texteingabefelder. Die fertige Notes-Anwendung verwendet die Ansichten [`ListView`](xref:Xamarin.Forms.ListView), [`Editor`](xref:Xamarin.Forms.Editor) und [`Button`](xref:Xamarin.Forms.Button). Weitere Informationen zu Ansichten finden Sie unter [Xamarin.Forms-Ansichten](~/xamarin-forms/user-interface/controls/views.md).
1. **Layouts**: Xamarin.Forms-Layouts sind Container, die zum Erstellen von Ansichten in Form von logischen Strukturen verwendet werden. Die Notes-Anwendung verwendet die Klasse [`StackLayout`](xref:Xamarin.Forms.StackLayout), um Ansichten in einem vertikalen Stapel anzuordnen, und die Klasse [`Grid`](xref:Xamarin.Forms.Grid), um Schaltflächen horizontal anzuordnen. Weitere Informationen zu Layouts finden Sie unter [Xamarin.Forms-Layouts](~/xamarin-forms/user-interface/controls/layouts.md).
1. **Zellen**: Xamarin.Forms-Zellen sind spezielle Elemente für Listenelemente und beschreiben, wie die einzelnen Elemente in einer Liste gezeichnet werden sollen. Die Notes-Anwendung verwendet die Klasse [`TextCell`](xref:Xamarin.Forms.TextCell), um für jede Zeile in der Liste zwei Elemente anzuzeigen. Weitere Informationen zu Zellen finden Sie unter [Xamarin.Forms-Zellen](~/xamarin-forms/user-interface/controls/cells.md).

Zur Laufzeit wird jedes Steuerelement seinem nativen Äquivalent zugeordnet. Dieses wird dann gerendert.

### <a name="layout"></a>Layout

Die Notes-Anwendung verwendet die Klasse [`StackLayout`](xref:Xamarin.Forms.StackLayout), um die plattformübergreifende Anwendungsentwicklung zu vereinfachen, indem Anzeigen unabhängig von der Bildschirmgröße automatisch angeordnet werden. Alle untergeordneten Elemente werden nacheinander horizontal oder vertikal in der Reihenfolge positioniert, in der sie hinzugefügt wurden. Wie viel Speicherplatz das `StackLayout` verwendet, hängt davon ab, wie die Eigenschaften [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) und [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) festgelegt sind. Das `StackLayout` versucht jedoch standardmäßig, den gesamten Bildschirm zu verwenden.

Der folgende XAML-Code zeigt ein Beispiel für die Verwendung einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse zur Gestaltung von `NoteEntryPage`:

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

Standardmäßig geht die Klasse [`StackLayout`](xref:Xamarin.Forms.StackLayout) von einer vertikalen Ausrichtung aus. Wenn aber eine horizontale Ausrichtung gewünscht wird, kann die Eigenschaft [`StackLayout.Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) auf den Enumerationsmember [`StackOrientation.Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal) festgelegt werden.

> [!NOTE]
> Die Größe der Ansichten kann mithilfe der Eigenschaften `HeightRequest` und `WidthRequest` festgelegt werden.

Weitere Informationen zur [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse finden Sie unter [StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).

### <a name="responding-to-user-interaction"></a>Reagieren auf eine Benutzerinteraktion

Ein in XAML definiertes Objekt kann ein Ereignis auslösen, das in der CodeBehind-Datei behandelt wird. Das folgende Codebeispiel zeigt die Methode `OnSaveButtonClicked` im CodeBehind für die Klasse `NoteEntryPage`, die als Reaktion auf das Ereignis [`Clicked`](xref:Xamarin.Forms.Button.Clicked) ausgeführt wird, das über die Schaltfläche *Speichern* ausgelöst wird.

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

Die Methode `OnSaveButtonClicked` speichert die Notiz in der Datenbank und wechselt zurück zur vorherigen Seite.

> [!NOTE]
> Die CodeBehind-Datei für eine XAML-Klasse kann auf ein Objekt zugreifen, das mit dem Namen, der ihm mit dem `x:Name`-Attribut zugewiesen wurde, in XAML definiert wurde. So wie bei C#-Variablen muss der diesem Attribut zugewiesene Wert mit einem Buchstaben oder Unterstrich beginnen und darf keine eingebetteten Leerzeichen enthalten.

Die Verknüpfung der Schaltfläche „Speichern“ mit der `OnSaveButtonClicked`-Methode erfolgt im XAML-Markup für die Klasse `NoteEntryPage`:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Listen

Über die Klasse [`ListView`](xref:Xamarin.Forms.ListView) wird eine Sammlung von Elementen vertikal in einer Liste angeordnet. Jedes Element in der Klasse `ListView` ist in einer einzelnen Zelle enthalten.

Das folgende Codebeispiel zeigt die Klasse [`ListView`](xref:Xamarin.Forms.ListView) über die `NotesPage`:

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

Das Layout jeder Zeile in der Klasse [`ListView`](xref:Xamarin.Forms.ListView) wird im Element [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) definiert und verwendet Datenbindung, um sämtliche Notizen anzuzeigen, die von der Anwendung abgerufen werden. Die Eigenschaft [`ListView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) ist in `NotesPage.xaml.cs` auf die Datenquelle festgelegt:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Dieser Code befüllt die Klasse [`ListView`](xref:Xamarin.Forms.ListView) mit sämtlichen Notizen, die in der Datenbank gespeichert sind.

Wenn eine Zeile in der Klasse [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird, wird das Ereignis [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) ausgelöst. Ein Ereignishandler mit dem Namen `OnListViewItemSelected` wird ausgeführt, wenn das Ereignis ausgelöst wird:

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

Das Ereignis [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) kann auf das Objekt zugreifen, das von der Eigenschaft [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) der Zelle zugeordnet wurde.

Weitere Informationen zur Klasse [`ListView`](xref:Xamarin.Forms.ListView) finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="navigation"></a>Navigation

Xamarin.Forms stellt abhängig vom verwendeten [`Page`](xref:Xamarin.Forms.Page)-Typ eine Reihe unterschiedlicher Seitennavigationen bereit. Für [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen kann die Navigation hierarchisch oder modal sein. Weitere Informationen zur modalen Navigation finden Sie unter [Modale Xamarin.Forms-Seiten](~/xamarin-forms/app-fundamentals/navigation/modal.md).

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-und die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse bieten alternative Navigationen. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).

Bei der hierarchischen Navigation wird die Klasse [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) verwendet, um in einen Stapel von [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten beliebig vor und zurück zu navigieren. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](xref:Xamarin.Forms.Page)-Objekten. Für den Wechsel von einer auf die andere Seite überträgt eine Anwendung eine neue Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird. Wenn Sie zur vorherigen Seite zurückkehren möchten, entfernt die Anwendung die aktuelle Seite per Pop vom Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite.

Die `NavigationPage`-Klasse platziert außerdem eine Navigationsleiste oben auf der Seite, in der ein Titel und eine für die Plattform angemessene **Zurück**-Schaltfläche, über die man zurück auf die vorherige Seite wechseln kann, angezeigt wird.

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird, wird als *Stammseite* der Anwendung bezeichnet. Das folgende Codebeispiel zeigt, wie dies in der Notes-Anwendung erreicht wird:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

Alle [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen (Inhaltsseiteninstanzen) beinhalten eine [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation)-Eigenschaft zum Ändern des Seitenstapels. Diese Methoden sollten nur angewendet werden, wenn die Anwendung eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) einschließt. Um zu `NoteEntryPage` navigieren zu können, muss die [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page))-Methode wie im folgenden Codebeispiel angewendet werden:

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

Dies bewirkt, dass das neue `NoteEntryPage`-Objekt auf den Navigationsstapel gepusht wird, wo es dann zur aktiven Seite wird.

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt. Um programmgesteuert zur ursprünglichen Seite zurückgehen zu können, muss das `NoteEntryPage`-Objekt die [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync)-Methode anwenden. Dies wird im folgenden Codebeispiel dargestellt:

```csharp
await Navigation.PopAsync();
```

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

## <a name="data-binding"></a>Datenbindung

Datenbindung wird verwendet, um zu vereinfachen, wie eine Xamarin.Forms-Anwendung Daten anzeigt und mit ihnen interagiert. Sie stellt eine Verbindung zwischen der Benutzeroberfläche und der zugrundeliegenden Anwendung her. Die [`BindableObject`](xref:Xamarin.Forms.BindableObject)-Klasse beinhaltet einen Großteil der Infrastruktur, um die Datenbindung zu unterstützen.

Bei der Datenbindung werden zwei Objekte miteinander verbunden: die *Quelle* und das *Ziel*. Das *Quellobjekt* stellt die Daten bereit. Das *Zielobjekt* verwendet Daten aus dem Quellobjekt (und zeigt diese häufig an). Mit einem [`Editor`](xref:Xamarin.Forms.Editor) (*Zielobjekt*) wird die zugehörige Eigenschaft [`Text`](xref:Xamarin.Forms.InputView.Text) beispielsweise häufig an eine öffentliche `string`-Eigenschaft in einem *Quellobjekt* gebunden. Das folgende Diagramm veranschaulicht die Bindungsbeziehung:

![](deepdive-images/data-binding.png "Data Binding")

Der Hauptvorteil der Datenbindung ist, dass Sie Daten zwischen Ihren Ansichten und der Datenquelle nicht mehr synchronisieren müssen. Änderungen im *Quellobjekt* werden automatisch mithilfe von Push intern vom Bindungsframework in das *Zielobjekt* übertragen, und Änderungen im Zielobjekt können optional wieder zurück in das *Quellobjekt* übertragen werden.

Die Datenbindung wird in zwei Schritten eingerichtet:

- Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) des *Zielobjekts* muss auf die *Quelle* festgelegt werden.
- Zwischen dem *Ziel* und der *Quelle* muss eine Bindung eingerichtet werden. In XAML wird dies mit der [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension)-Markuperweiterung erreicht.

In der Notes-Anwendung ist der [`Editor`](xref:Xamarin.Forms.Editor), der eine Notiz anzeigt, das Bindungsziel, während die Instanz `Note`, die als die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` festgelegt ist, die Bindungsquelle darstellt.

Die Eigenschaft `BindingContext` von `NoteEntryPage` wird wie im folgenden Codebeispiel gezeigt bei der Seitennavigation festgelegt:

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

In der Methode `OnNoteAddedClicked`, die ausgeführt wird, wenn der Anwendung eine neue Notiz hinzugefügt wird, ist die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) von `NoteEntryPage` auf eine neue `Note`-Instanz festgelegt. In der Methode `OnListViewItemSelected`, die ausgeführt wird, wenn eine vorhandene Notiz in der Klasse [`ListView`](xref:Xamarin.Forms.ListView) ausgewählt wird, wird die Eigenschaft `BindingContext` von `NoteEntryPage` auf die ausgewählte `Note`-Instanz festgelegt, auf die über die Eigenschaft [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) zugegriffen werden kann.

> [!IMPORTANT]
> Die Eigenschaft [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der einzelnen *Zielobjekte* kann einzeln festgelegt werden, dies ist jedoch nicht erforderlich. `BindingContext` ist eine spezielle Eigenschaft, die von allen zugehörigen untergeordneten Elementen geerbt wird. Wenn die Eigenschaft `BindingContext` auf der Seite [`ContentPage`](xref:Xamarin.Forms.ContentPage) auf eine `Note`-Instanz festgelegt wird, verfügen folglich alle untergeordneten Elemente der Seite `ContentPage` über die gleiche `BindingContext`-Eigenschaft und können eine Bindung an öffentliche Eigenschaften des `Note`-Objekts vornehmen.

Der [`Editor`](xref:Xamarin.Forms.Editor) auf der Seite `NoteEntryPage` bindet dann die Eigenschaft `Text` des Objekts `Note`:

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

Zwischen der Eigenschaft [`Editor.Text`](xref:Xamarin.Forms.InputView.Text) und der Eigenschaft `Text` des *Quellobjekts* wird eine Bindung eingerichtet. Am `Editor` vorgenommene Änderungen werden automatisch an das `Note`-Objekt weitergegeben. Gleichermaßen aktualisiert die Xamarin.Forms-Bindungs-Engine auch die Inhalte von `Editor`, wenn Änderungen an der Eigenschaft `Note.Text` vorgenommen werden. Dies wird als *bidirektionale Bindung* bezeichnet.

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Format

Xamarin.Forms-Anwendungen enthalten häufig mehrere visuelle Elemente, die identisch dargestellt werden. Es kann sehr eintönig und fehleranfällig sein, die Darstellung der visuellen Elemente einzeln festzulegen. Stattdessen können Formatvorlagen, die die Darstellung definieren, erstellt und anschließend auf die erforderlichen visuellen Elemente angewendet werden.

Die Klasse [`Style`](xref:Xamarin.Forms.Style) gruppiert eine Sammlung von Eigenschaftswerten in ein Objekt, das anschließend auf mehrere Instanzen von visuellen Elementen angewendet werden kann. Formatvorlagen werden entweder auf Anwendungs-, auf Ansichts- oder auf Seitenebene in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)-Klasse gespeichert. Die Entscheidung, wo Sie eine `Style`-Klasse definieren, hat Einfluss darauf, wo Sie sie verwenden können:

- Eine auf Anwendungsebene definierte [`Style`](xref:Xamarin.Forms.Style)-Instanz können Sie für die gesamte Anwendung nutzen.
- Eine auf Seitenebene definierte [`Style`](xref:Xamarin.Forms.Style)-Instanz können Sie nur auf die jeweilige Seite und deren untergeordneten Seiten anwenden.
- Eine auf Ansichtsebene definierte [`Style`](xref:Xamarin.Forms.Style)-Instanz können Sie nur auf die jeweilige Ansicht und deren untergeordneten Ansichten anwenden.

> [!IMPORTANT]
> Alle Formatvorlagen, die in der gesamten Anwendung verwendet werden, werden im Ressourcenverzeichnis der Anwendung gespeichert, damit eine Duplizierung verhindert wird. XAML-Code, der für eine Seite spezifisch ist, sollte jedoch nicht im Ressourcenverzeichnis der Anwendung enthalten sein, da die Ressourcen dann beim Starten der Anwendung analysiert werden und nicht, wenn dies auf einer Seite erforderlich ist.

Jede [`Style`](xref:Xamarin.Forms.Style)-Instanz enthält eine Sammlung mit mindestens einem [`Setter`](xref:Xamarin.Forms.Setter)-Objekt. Dabei weist jedes `Setter`-Objekt eine [`Property`](xref:Xamarin.Forms.Setter.Property) und einen [`Value`](xref:Xamarin.Forms.Setter.Value) auf. `Property` ist der Name der bindbaren Eigenschaft des Elements, auf das die Formatvorlage angewendet wird, und `Value` ist der Wert, der auf die Eigenschaft angewendet wird. Das folgende Codebeispiel zeigt eine Formatvorlage von `NoteEntryPage`:

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

Diese Formatvorlage wird auf alle [`Editor`](xref:Xamarin.Forms.Editor)-Instanzen auf der Seite angewendet.

Wenn Sie eine [`Style`](xref:Xamarin.Forms.Style)-Instanz erstellen, wird immer die Eigenschaft [`TargetType`](xref:Xamarin.Forms.Style.TargetType) benötigt.

> [!NOTE]
> Xamarin.Forms-Anwendungen werden normalerweise mithilfe von XAML-Formatvorlagen formatiert. Xamarin.Forms unterstützt jedoch auch das Formatieren von visuellen Elementen mithilfe von Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von Cascading Stylesheets (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Weitere Informationen zu XAML-Formatvorlagen finden Sie unter [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md).

### <a name="providing-platform-specific-styles"></a>Bereitstellen von plattformspezifischen Formatvorlagen

Die `OnPlatform`-Markuperweiterungen ermöglichen Ihnen das plattformspezifische Anpassen der Benutzeroberfläche:

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

Die [`Style`](xref:Xamarin.Forms.Style)-Instanz legt je nach verwendeter Plattform verschiedene [`Color`](xref:Xamarin.Forms.Color)-Werte für die Eigenschaften [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) und [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor) von [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) fest.

Weitere Informationen über XAML-Markuperweiterungen finden Sie unter [XAML Markup Extensions (XAML-Markuperweiterungen)](~/xamarin-forms/xaml/markup-extensions/index.md). Weitere Informationen zur Markuperweiterung `OnPlatform` finden Sie unter [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="testing-and-deployment"></a>Testen und Bereitstellen

Sowohl Visual Studio für Mac als auch Visual Studio stellen viele Optionen zum Testen und Bereitstellen einer Anwendung bereit. Das Debuggen von Anwendungen ist ein üblicher Vorgang im Lebenszyklus der Anwendungsentwicklung und trägt dazu bei, Codeprobleme zu diagnostizieren. Weitere Informationen finden Sie unter [Festlegen eines Haltepunkts](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Step Through Code (Detailliertes Durchlaufen von Code)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) und [Output Information to the Log Window (Ausgabeinformationen an das Protokollfenster)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Simulatoren eigenen sich hervorragend für die Bereitstellung und das Testen einer Anwendung. Außerdem bieten sie nützliche Funktionen zum Testen von Anwendungen. Allerdings verwenden Benutzer die endgültige Anwendung nicht in einem Simulator. Daher sollte die Anwendungen frühzeitig und häufig auf echten Geräten getestet werden. Weitere Informationen zur Bereitstellung von iOS-Geräten finden Sie unter [Device Provisioning (Gerätebereitstellung)](~/ios/get-started/installation/device-provisioning/index.md). Weitere Informationen zur Bereitstellung von Android-Geräten finden Sie unter [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden die Grundlagen der Anwendungsentwicklung mit Xamarin.Forms erläutert. Vorgeschlagene nächste Schritte umfassen das Lesen über folgende Funktionen:

- Es gibt vier Hauptsteuerelementgruppen, mit denen die Benutzeroberfläche einer Xamarin.Forms-Anwendung erstellt wird. Weitere Informationen finden Sie unter [Steuerelementreferenz](~/xamarin-forms/user-interface/controls/index.md).
- Bei der Datenbindung werden die Eigenschaften von zwei Objekten verknüpft. Dadurch werden Änderungen an einer Eigenschaft automatisch in der anderen widergespiegelt. Weitere Informationen finden Sie unter [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- Xamarin.Forms stellt abhängig vom verwendeten Seitentyp eine Reihe unterschiedlicher Seitennavigationen bereit. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).
- Stile können repetitives Markup reduzieren und erleichtern Änderungen an der Darstellung von Anwendungen. Weitere Informationen finden Sie unter [Formatieren von Xamarin.Forms-Apps](~/xamarin-forms/user-interface/styles/index.md).
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
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Beispiele für die ersten Schritte](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms%20get%20started)
- [Xamarin.Forms-API-Referenz](xref:Xamarin.Forms)
- [Free Self-Guided Learning (Kostenloses eigenständiges Lernen) (Video)](https://university.xamarin.com/self-guided/)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/Xamarin-Solution-Architecture-4-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
