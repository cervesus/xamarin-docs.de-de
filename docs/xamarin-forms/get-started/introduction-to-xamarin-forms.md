---
title: "Einführung in Xamarin.Forms"
description: "Xamarin.Forms ist eine plattformübergreifende nativ gesicherte Benutzeroberflächen-Toolkit-Abstraktion, mit der Entwickler ohne großen Aufwand Benutzeroberflächen für Android, iOS, Windows und Windows Phone erstellen können. Die Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Anwendungen von Xamarin.Forms das Erscheinungsbild der einzelnen Plattformen beibehalten. Dieser Artikel enthält eine Einführung in Xamarin.Forms und Informationen zu den ersten Schritten beim Schreiben von Anwendungen."
ms.topic: article
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 593e720e4a6125e2ef4a1c9488186cb2c04dcd66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Einführung in Xamarin.Forms

_Xamarin.Forms ist eine plattformübergreifende nativ gesicherte Benutzeroberflächen-Toolkit-Abstraktion, mit der Entwickler ohne großen Aufwand Benutzeroberflächen für Android, iOS, Windows und Windows Phone erstellen können. Die Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Anwendungen von Xamarin.Forms das Erscheinungsbild der einzelnen Plattformen beibehalten. Dieser Artikel enthält eine Einführung in Xamarin.Forms und Informationen zu den ersten Schritten beim Schreiben von Anwendungen._

<a name="Overview" />

## <a name="overview"></a>Übersicht

Xamarin.Forms ist ein Framework, mit dem Entwickler schnell plattformübergreifende Benutzeroberflächen erstellen können. Es bietet eine eigene Abstraktion für die Benutzeroberfläche, die mit nativen Steuerelementen unter iOS, Android, Windows oder Windows Phone gerendert wird. Dies bedeutet, dass Anwendungen einen Großteil des zugehörigen Codes für die Benutzeroberfläche freigeben können und das native Erscheinungsbild der Zielplattform weiterhin erhalten bleibt.

Mit Xamarin.Forms werden ohne großen Aufwand Prototypen für Anwendungen erstellt, die sich im Laufe der Zeit zu komplexen Anwendungen entwickeln können. Da es sich bei Xamarin.Forms-Anwendungen um native Anwendungen handelt, weisen diese nicht die Einschränkungen anderer Toolkits auf, wie z.B. Browser-Sandkasten, begrenzte APIs oder schlechte Leistung. Mit Xamarin.Forms geschriebene Anwendungen können alle APIs oder Funktionen der zugrunde liegenden Plattform nutzen. Dazu zählen unter anderem CoreMotion, PassKit und StoreKit unter iOS; NFC und Google Play Services unter Android; und Tiles unter Windows. Darüber hinaus können Anwendungen erstellt werden, bei denen Teile der zugehörigen Benutzeroberfläche mit Xamarin.Forms erstellt werden, während andere Teile mit dem nativen Benutzeroberflächen-Toolkit erstellt werden.

Xamarin.Forms-Anwendungen sind genauso aufgebaut wie traditionelle plattformübergreifende Anwendungen. Bei dem gebräuchlichsten Ansatz wird der freigegebene Code über [Portable Bibliotheken](~/cross-platform/app-fundamentals/pcl.md) oder [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) untergebracht. Zudem werden plattformspezifische Anwendungen erstellt, die den freigegebenen Code nutzen.

Es gibt zwei Techniken für die Erstellung von Benutzeroberflächen in Xamarin.Forms. Bei der ersten Technik werden Benutzeroberflächen komplett mit C#-Quellcode erstellt. Bei der zweiten Technik wird *XAML* (Extensible Application Markup Language) verwendet. Dies ist eine deklarative Markupsprache für die Beschreibung von Benutzeroberflächen. Weitere Informationen zu XAML finden Sie unter [XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md).

In diesem Artikel werden die Grundlagen des Xamarin.Forms-Frameworks erläutert. Folgende Themen werden behandelt:

-  [Überprüfen einer Xamarin.Forms-Anwendung](#Examining_A_Xamarin.Forms_Application).
-  [Verwendung von Xamarin.Forms-Seiten und -Steuerelementen](#Views_and_Layouts).
-  [Anzeigen einer Liste von Daten](#Lists_in_Xamarin.Forms).
-  [Einrichten der Datenbindung](#Data_Binding).
-  [Navigieren zwischen Seiten](#Navigation).
-  [Nächste Schritte](#Next_Steps).

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Überprüfen einer Xamarin.Forms-Anwendung

In Visual Studio für Mac und Visual Studio erstellt die standardmäßige Xamarin.Forms-App-Vorlage die einfachste mögliche Xamarin.Forms-Projektmappe, die dem Benutzer Text anzeigt. Wenn Sie die Anwendung ausführen, sollte sie den folgenden Screenshots ähneln:

[ ![](introduction-to-xamarin-forms-images/image05-sml.png "Xamarin.Forms-Standardanwendung")](introduction-to-xamarin-forms-images/image05.png "Xamarin.Forms-Standardanwendung")

Alle Anzeigen in den Screenshots entsprechen einer *Seite* in Xamarin.Forms. Eine [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) stellt eine *Aktivität* unter Android, einen *Ansichtscontroller* unter iOS oder eine *Seite* auf der universellen Windows-Plattform (Windows Universal Platform, UWP) dar. In dem Beispiel in den oben stehenden Screenshots wird ein [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Objekt instanziiert, das für die Anzeige einer [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) verwendet wird.

Zur Maximierung der Wiederverwendung des Startcodes verfügen Xamarin.Forms-Anwendungen über eine einzelne Klasse namens `App`, welche die erste [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) instanziiert, die angezeigt wird. Ein Beispiel für die `App`-Klasse ist im folgenden Code zu sehen:

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

Dieser Code instanziiert ein neues [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Objekt, das eine einzelne [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) anzeigt, die auf der Seite vertikal und horizontal zentriert ist.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>Aufrufen der Xamarin.Forms-Startseite auf den einzelnen Plattformen

Damit diese [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) in einer Anwendung verwendet werden kann, müssen alle Anwendungsplattformen das Xamarin.Forms-Framework initialisieren und beim Start eine Instanz der [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) bereitstellen. Dieser Initialisierungsschritt variiert je nach Plattform und wird in den folgenden Abschnitten behandelt.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

Für das Aufrufen der Xamarin.Forms-Startseite in iOS enthält das Plattformprojekt die `AppDelegate`-Klasse, die wie im folgenden Codebeispiel veranschaulicht von der Klasse `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` erbt:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

Die `FinishedLoading`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die iOS-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor der Root View Controller durch den Aufruf auf die `LoadApplication`-Methode festgelegt wird.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Für das Aufrufen der Xamarin.Forms-Startseite in Android enthält das Plattformprojekt Code, der eine `Activity` mit dem Attribut `MainLauncher` erstellt, wobei die Aktivität wie im folgenden Codebeispiel veranschaulicht von der Klasse `FormsApplicationActivity` erbt:

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

Die `OnCreate`-Außerkraftsetzung initialisiert das Xamarin.Forms-Framework durch Aufrufen der `Init`-Methode. Dadurch wird die Android-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen, bevor die Xamarin.Forms-Anwendung geladen wird.

<a name="Launching_in_Windows_Phone" />

#### <a name="windows-phone-81-winrt"></a>Windows Phone 8.1 (WinRT)

In Windows-Runtime-Anwendungen wird die `Init`-Methode, die das Xamarin.Forms-Framework initialisiert, über die `App`-Klasse aufgerufen:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Dadurch wird die Windows Phone-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Die Xamarin.Forms-Startseite wird wie im folgenden Codebeispiel veranschaulicht über die `MainPage`-Klasse aufgerufen:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.NavigationCacheMode = NavigationCacheMode.Required;
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Die Xamarin.Forms-Anwendung wird mit der `LoadApplication`-Methode geladen.

Xamarin.Forms bietet auch Unterstützung für Windows 8.1. Informationen zum Konfigurieren dieser Projekttypen, finden Sie unter [Setup Windows Projects](~/xamarin-forms/platform/windows/installation/index.md) (Einrichten von Windows-Projekten).

#### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

In Anwendungen der universellen Windows-Plattform (Universal Windows Platform, UWP) wird die `Init`-Methode, die das Xamarin.Forms-Framework initialisiert, über die `App`-Klasse aufgerufen:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Dadurch wird die UWP-spezifische Implementierung von Xamarin.Forms in die Anwendung geladen. Die Xamarin.Forms-Startseite wird wie im folgenden Codebeispiel veranschaulicht über die `MainPage`-Klasse aufgerufen:

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Die Xamarin.Forms-Anwendung wird mit der `LoadApplication`-Methode geladen.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>Ansichten und Layouts

Es gibt vier Hauptsteuerelementgruppen, mit denen die Benutzeroberfläche einer Xamarin.Forms-Anwendung erstellt wird.

1. **Seiten**: Xamarin.Forms-Seiten stellen Anzeigen plattformübergreifender mobiler Anwendungen dar. Weitere Informationen zu Seiten finden Sie unter [Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md) (Xamarin.Forms-Seiten).
1. **Layouts**: Xamarin.Forms-Layouts sind Container, die zum Erstellen von Ansichten in Form von logischen Strukturen verwendet werden. Weitere Informationen zu Layouts finden Sie unter [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md) (Xamarin.Forms-Layouts).
1. **Ansichten**: Xamarin.Forms-Ansichten sind die Steuerelemente auf der Benutzeroberfläche, z.B. Bezeichnungen, Schaltflächen und Texteingabefelder. Weitere Informationen zu Ansichten finden Sie unter [Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md) (Xamarin.Forms-Ansichten).
1. **Zellen**: Xamarin.Forms-Zellen sind spezielle Elemente für Listenelemente und beschreiben, wie die einzelnen Elemente in einer Liste gezeichnet werden sollen. Weitere Informationen zu Zellen finden Sie unter [Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md) (Xamarin.Forms-Zellen).

Zur Laufzeit wird jedes Steuerelement seinem nativen Äquivalent zugeordnet. Dieses wird dann gerendert.

Steuerelemente werden in einem Layout gehostet. Nun wird die [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Klasse überprüft, die ein häufig verwendetes Layout implementiert.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

Das [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) vereinfacht die plattformübergreifende Anwendungsentwicklung, indem Steuerelemente in der Anzeige unabhängig von der Bildschirmgröße automatisch angeordnet werden. Alle untergeordneten Elemente werden nacheinander horizontal oder vertikal in der Reihenfolge positioniert, in der sie hinzugefügt wurden. Wie viel Speicherplatz das `StackLayout` verwendet, hängt davon ab, wie die Eigenschaften [`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) festgelegt sind. Das `StackLayout` versucht jedoch standardmäßig, den gesamten Bildschirm zu verwenden.

Der folgende XAML-Code zeigt ein Beispiel für die Verwendung eines [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) zur Anordnung von drei [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)-Steuerelementen:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

Das [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) setzt, wie in den folgenden Screenshots zu sehen ist, standardmäßig eine vertikale Ausrichtung voraus:

[ ![](introduction-to-xamarin-forms-images/image09-sml.png "Vertikales StackLayout")](introduction-to-xamarin-forms-images/image09.png "Vertikales StackLayout")

Die Ausrichtung des [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) kann wie im folgenden XAML-Codebeispiel veranschaulicht in eine horizontale Ausrichtung geändert werden:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

Die folgenden Screenshots zeigen das resultierende Layout:

[ ![](introduction-to-xamarin-forms-images/image10-sml.png "Horizontales StackLayout")](introduction-to-xamarin-forms-images/image10.png "Horizontales StackLayout")

Die Größe von Steuerelementen kann wie im folgenden XAML-Codebeispiel veranschaulicht über die Eigenschaften `HeightRequest` und `WidthRequest` festgelegt werden:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

Die folgenden Screenshots zeigen das resultierende Layout:

[ ![](introduction-to-xamarin-forms-images/image11-sml.png "Horizontales StackLayout mit LayoutOptions")](introduction-to-xamarin-forms-images/image11.png "Horizontales StackLayout mit LayoutOptions")

Weitere Informationen zur [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Klasse finden Sie unter [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Listen in Xamarin.Forms

Mit dem [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Steuerelement wird auf dem Bildschirm eine Sammlung von Elementen angezeigt. Jedes Element in der `ListView` ist in einer einzelnen Zelle enthalten. Eine `ListView` verwendet standardmäßig die integrierte [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)-Vorlage und rendert eine einzelne Textzeile.

Das folgende Codebeispiel zeigt ein einfaches Beispiel für die [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/):

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

Der folgende Screenshot zeigt die resultierende [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/):

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

Weitere Informationen zum [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Steuerelement finden Sie unter [ListView](~/xamarin-forms/user-interface/listview/index.md).

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>Binden an eine benutzerdefinierte Klasse

Das [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Steuerelement kann unter Verwendung der [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)-Standardvorlage auch benutzerdefinierte Objekte anzeigen.

Das folgende Codebeispiel zeigt die `TodoItem`-Klasse:

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

Das [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Steuerelement kann wie im folgenden Codebeispiel veranschaulicht aufgefüllt werden:

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

Für die Festlegung, welche `TodoItem`-Eigenschaft von der [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) angezeigt werden soll, kann eine Bindung wie im folgenden Codebeispiel veranschaulicht erstellt werden:

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

Dadurch wird eine Bindung erstellt, die den Pfad zur Eigenschaft `TodoItem.Name` angibt und zum vorher angezeigten Screenshot führt.

Weitere Informationen zur Bindung an eine benutzerdefinierte Klasse finden Sie unter [ListView Data Sources](~/xamarin-forms/user-interface/listview/data-and-databinding.md) (ListView-Datenquellen).

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>Auswählen eines Elements in einer ListView

Wenn ein Benutzer eine Zelle in einer [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) berührt, sollte als Reaktion darauf das [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)-Ereignis wie im folgenden Codebeispiel veranschaulicht behandelt werden:

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

Wenn sie auf einer [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) enthalten ist, kann die [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/)-Methode verwendet werden, um eine neue Seite mit integrierter Rückwärtsnavigation zu öffnen. Das [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)-Ereignis kann über die Eigenschaft [`e.SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) auf das der Zelle zugeordnete Objekt zugreifen, es an eine neue Seite binden und die neue Seite über `PushAsync` anzeigen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

Die einzelnen Plattformen implementieren eine integrierte Rückwärtsnavigation auf ihre eigene Art und Weise. Weitere Informationen finden Sie unter [Navigation](#Navigation).

Weitere Informationen zur [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Auswahl finden Sie unter [ListView Interactivity](~/xamarin-forms/user-interface/listview/interactivity.md) (ListView-Interaktivität).

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>Anpassen der Darstellung einer Zelle

Die Zellendarstellung kann angepasst werden, indem Unterklassen der [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)-Klasse erstellt werden und der Typ dieser Klasse auf die Eigenschaft [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) der [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) festgelegt wird.

Die im folgenden Screenshot dargestellte Zelle besteht aus einem [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)-Steuerelement und zwei [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)-Steuerelementen:

 ![](introduction-to-xamarin-forms-images/image14.png "ListView: Benutzerdefinierte Darstellung einer Zelle")

Zur Erstellung dieses benutzerdefinierten Layouts sollten für die [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)-Klasse wie im folgenden Codebeispiel veranschaulicht Unterklassen erstellt werden:

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

Mit diesem Code werden die folgenden Aufgaben ausgeführt:

-  Er fügt ein [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)-Steuerelement hinzu und bindet dieses an die Eigenschaft `ImageUri` des `Employee`-Objekts. Weitere Informationen zur Datenbindung finden Sie unter [Datenbindung](#Data_Binding).
-  Er erstellt ein [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) mit einer vertikalen Ausrichtung, das die zwei [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)-Steuerelemente enthalten soll. Die `Label`-Steuerelemente werden an die Eigenschaften `DisplayName` und `Twitter` des `Employee`-Objekts gebunden.
-  Er erstellt ein [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), welches das vorhandene [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) und `StackLayout` hostet. Er ordnet die zugehörigen untergeordneten Elemente mit horizontaler Ausrichtung an.

Nachdem die benutzerdefinierte Zelle erstellt wurde, kann sie von einem [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)-Steuerelement genutzt werden, indem sie in einer [`DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) umgebrochen wird. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

Dieser Code stellt eine `List` der `Employee` für die [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) bereit. Die einzelnen Zellen werden über die `EmployeeCell`-Klasse gerendert. Die `ListView` übergibt das `Employee`-Objekt an die `EmployeeCell` als [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Weitere Informationen zum Anpassen der Zellendarstellung finden Sie unter [Cell Appearance ](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) (Zellendarstellung).

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>Verwenden von XAML zum Erstellen und Anpassen einer Liste

Das XAML-Äquivalent der [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) im vorherigen Abschnitt wird im folgenden Codebeispiel veranschaulicht:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Mit diesem XAML-Code wird eine [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) definiert, die eine [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) enthält. Die Datenquelle der `ListView` wird über das Attribut [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) festgelegt. Das Layout der einzelnen Zeilen in der `ItemsSource` wird im Element [`ListView.ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) definiert.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung

Bei der Datenbindung werden zwei Objekte miteinander verbunden: die *Quelle* und das *Ziel*. Das *Quellobjekt* stellt die Daten bereit. Das *Zielobjekt* verwendet Daten aus dem Quellobjekt (und zeigt diese häufig an). Mit einer [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) (*Zielobjekt*) wird die zugehörige Eigenschaft [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) beispielsweise häufig an eine öffentliche Eigenschaft `string` in einem *Quellobjekt* gebunden. Das folgende Diagramm veranschaulicht die Bindungsbeziehung:

![](introduction-to-xamarin-forms-images/data-binding.png "Datenbindung")

Der Hauptvorteil der Datenbindung ist, dass Sie Daten zwischen Ihren Ansichten und der Datenquelle nicht mehr synchronisieren müssen. Änderungen im *Quellobjekt* werden automatisch mithilfe von Push intern vom Bindungsframework in das *Zielobjekt* übertragen, und Änderungen im Zielobjekt können optional wieder zurück in das *Quellobjekt* übertragen werden.

Die Datenbindung wird in zwei Schritten eingerichtet:

- Die Eigenschaft [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des *Zielobjekts* muss auf die *Quelle* festgelegt werden.
- Zwischen dem *Ziel* und der *Quelle* muss eine Bindung eingerichtet werden. In XAML wird dies mit der [`Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/)-Markuperweiterung erreicht. In C# wird dies mit der [`SetBinding`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)-Methode erreicht.

Weitere Informationen zur Datenbindung finden Sie unter [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) (Datenbindungsgrundlagen).

### <a name="xaml"></a>XAML

Der folgende Code ist ein Beispiel für die Durchführung einer Datenbindung in XAML:

```xaml
<Entry Text="{Binding FirstName}" ... />
```

Zwischen der Eigenschaft [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) und der Eigenschaft `FirstName` des *Quellobjekts* wird eine Bindung eingerichtet. Am `Entry`-Steuerelement vorgenommene Änderungen werden automatisch an das `employeeToDisplay`-Objekt weitergegeben. Gleichermaßen aktualisiert das Xamarin.Forms-Bindungsmodul auch die Inhalte des `Entry`-Steuerelements, wenn Änderungen an der Eigenschaft `employeeToDisplay.FirstName` vorgenommen werden. Dies wird als *bidirektionale Bindung* bezeichnet. Damit die bidirektionale Bindung funktioniert, muss die Modellklasse die `INotifyPropertyChanged`-Schnittstelle implementieren.

Obwohl die Eigenschaft [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) der `EmployeeDetailPage`-Klasse in XAML festgelegt werden kann, wird sie hier in CodeBehind auf die Instanz eines `Employee`-Objekts festgelegt:

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

Die Eigenschaft [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) der einzelnen *Zielobjekte* kann einzeln festgelegt werden, dies ist jedoch nicht erforderlich. `BindingContext` ist eine spezielle Eigenschaft, die von allen zugehörigen untergeordneten Elementen geerbt wird. Wenn der `BindingContext` auf der [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) auf eine `Employee`-Instanz festgelegt wird, verfügen folglich alle untergeordneten Elemente der `ContentPage` über den gleichen `BindingContext` und können eine Bindung an öffentliche Eigenschaften des `Employee`-Objekts vornehmen.

### <a name="c35"></a>C&#35;

Der folgende Code zeigt ein Beispiel für die Durchführung einer Datenbindung in C#:

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

Dem [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Konstruktor wird die Instanz eines `Employee`-Objekts übergeben. Er legt den [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) auf das Objekt fest, an das die Instanz gebunden werden soll. Ein [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)-Steuerelement wird instanziiert, und die Bindung zwischen der Eigenschaft [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) und der Eigenschaft `FirstName` des *Quellobjekts* wird festgelegt. Am `Entry`-Steuerelement vorgenommene Änderungen werden automatisch an das `employeeToDisplay`-Objekt weitergegeben. Gleichermaßen aktualisiert das Xamarin.Forms-Bindungsmodul auch die Inhalte des `Entry`-Steuerelements, wenn Änderungen an der Eigenschaft `employeeToDisplay.FirstName` vorgenommen werden. Dies wird als *bidirektionale Bindung* bezeichnet. Damit die bidirektionale Bindung funktioniert, muss die Modellklasse die `INotifyPropertyChanged`-Schnittstelle implementieren.

Die `SetBinding`-Methode nimmt zwei Parameter an. Der erste Parameter gibt Informationen zum Bindungstyp an. Über den zweiten Parameter werden Informationen zu Bindungselementen und zur Vorgehensweise bei der Bindung bereitgestellt. In den meisten Fällen ist der zweite Parameter bloß eine Zeichenfolge, die den Namen der Eigenschaft in der [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) enthält. Die folgende Syntax wird für eine direkte Bindung an die `BindingContext` verwendet:

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

Die Punktsyntax sagt Xamarin.Forms, dass statt der Eigenschaft `BindingContext` der [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) als Datenquelle verwendet werden soll. Dies ist hilfreich, wenn der `BindingContext` ein einfacher Typ ist, z.B. eine `string` oder eine `int`.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>Benachrichtigung der Eigenschaftenänderung

Das *Zielobjekt* empfängt standardmäßig nur bei Erstellung der Bindung den Wert des *Quellobjekts*. Das *Zielobjekt* muss bei einer Änderung des *Quellobjekts* benachrichtigt werden können, um die Benutzeroberfläche mit der Datenquelle synchronisiert zu halten. Dieser Mechanismus wird von der `INotifyPropertyChanged`-Schnittstelle bereitgestellt. Durch die Implementierung dieser Schnittstelle werden für alle datengebundenen Steuerelemente Benachrichtigungen bereitgestellt, wenn der zugrunde liegende Eigenschaftswert geändert wird.

Objekte, die `INotifyPropertyChanged` implementieren, müssen das `PropertyChanged`-Ereignis auslösen, wenn eine der zugehörigen Eigenschaften mit einem neuen Wert aktualisiert wird. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Bei einer Änderung der Eigenschaft `MyObject.FirstName` wird die `OnPropertyChanged`-Methode aufgerufen und das `PropertyChanged`-Ereignis ausgelöst. Das `PropertyChanged`-Ereignis wird nicht ausgelöst, wenn sich der Eigenschaftswert nicht ändert. Auf diese Weise werden unnötige Ereignisauslösungen vermieden.

Beachten Sie, dass der Parameter `propertyName` bei der `OnPropertyChanged`-Methode mit dem Attribut `CallerMemberName` gestaltet wird. Dadurch wird sichergestellt, dass beim Aufrufen der `OnPropertyChanged`-Methode mit einem `null`-Wert das Attribut `CallerMemberName` den Namen der Methode angibt, über die `OnPropertyChanged` aufgerufen wurde.

<a name="Navigation" />

## <a name="navigation"></a>Navigation

Xamarin.Forms stellt abhängig von dem verwendeten [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Typ eine Reihe unterschiedlicher Seitennavigationen bereit. Für [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Instanzen gibt es zwei Seitennavigationen:

- [Hierarchische Navigation](#Hierarchical_Navigation)
- [Modale Navigation](#Modal_Navigation)

Die [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)-, [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)-und die [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)-Klasse bieten alternative Navigationen. Weitere Informationen finden Sie unter [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md).

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>Hierarchische Navigation

Die [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse stellt eine hierarchische Navigation bereit, bei welcher der Benutzer wie gewünscht in der Vorwärts- und in der Rückwärtsrichtung durch Seiten navigieren kann. Die Klasse implementiert die Navigation als LIFO-Stapel (Last-In-First-out) von [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Objekten.

Bei der hierarchischen Navigation wird die [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Klasse verwendet, um durch einen Stapel von [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-Objekten zu navigieren. Für den Wechsel von einer auf die andere Seite überträgt eine Anwendung eine neue Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird. Wenn Sie zur vorherigen Seite zurückkehren möchten, entfernt die Anwendung die aktuelle Seite per Pop vom Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite.

Die erste Seite, die zu einem Navigationsstapel hinzugefügt wird, wird als *Stammseite* der Anwendung bezeichnet. Das folgende Codebeispiel zeigt, wie dies erreicht wird:

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

Für die Navigation zur `LoginPage` muss die [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/)-Methode für die Eigenschaft [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) der aktuellen Seite aufgerufen werden. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
await Navigation.PushAsync(new LoginPage());
```

Dies bewirkt, dass das neue `LoginPage`-Objekt mithilfe von Push auf den Navigationsstapel übertragen wird, wo es dann zur aktiven Seite wird.

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt. Wenn Sie programmgesteuert zur vorherigen Seite zurückkehren möchten, muss die `LoginPage`-Instanz die [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
await Navigation.PopAsync();
```

Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>Modale Navigation

Xamarin.Forms bietet Unterstützung für modale Seiten. Eine modale Seite ermutigt Benutzer, eine eigenständige Aufgabe auszuführen. Dabei kann erst dann die Ansicht gewechselt werden, wenn die Aufgabe abgeschlossen oder abgebrochen wurde.

Eine modale Seite kann jeder von Xamarin.Forms unterstützte [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Typ sein. Wenn eine modale Seite angezeigt werden soll, überträgt die Anwendung diese Seite mithilfe von Push in den Navigationsstapel, wo sie dann zur aktiven Seite wird. Wenn Sie zur vorherigen Seite zurückzukehren möchten, entfernt die Anwendung die aktuelle Seite per Pop vom Navigationsstapel, und die neue oberste Seite wird zur aktiven Seite.

Modale Navigationsmethoden werden von der Eigenschaft [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) für einen beliebigen [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-Typ verfügbar gemacht. Die Eigenschaft [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) macht zudem die Eigenschaft [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) verfügbar, über welche die modalen Seiten im Navigationsstapel abgerufen werden können. Es gibt jedoch kein Konzept für die modale Stapelbearbeitung oder das Entfernen per Pop, um bei der modalen Navigation zur Stammseite zurückzukehren. Grund dafür ist, dass diese Vorgänge auf den zugrunde liegenden Plattformen nicht allgemein unterstützt werden.

> [!NOTE]
> Für die Durchführung einer modalen Seitennavigation ist keine [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)-Instanz erforderlich.

Für die modale Navigation zur `LoginPage` muss die [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/)-Methode für die Eigenschaft [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) der aktuellen Seite wie im folgenden Codebeispiel veranschaulicht aufgerufen werden:

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

Dies bewirkt, dass die `LoginPage`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird.

Die aktive Seite kann durch Drücken der Schaltfläche *Zurück* an dem Gerät per Pop von dem Navigationsstapel entfernt werden, und zwar unabhängig davon, ob es sich um eine physische Schaltfläche an dem Gerät oder um eine Schaltfläche auf dem Bildschirm handelt. Wenn Sie programmgesteuert zur ursprünglichen Seite zurückkehren möchten, muss die `LoginPage`-Instanz die [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)-Methode aufrufen. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
await Navigation.PopModalAsync();
```

Dadurch wird die `LoginPage`-Instanz von dem Navigationsstapel entfernt, und die neue oberste Seite wird zur aktiven Seite.

Weitere Informationen zur modalen Navigation finden Sie unter [Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md) (Modale Seiten).

<a name="Next_Steps" />

## <a name="next-steps"></a>Nächste Schritte

Dieser einleitende Artikel soll Ihnen dabei helfen, mit dem Schreiben von Xamarin.Forms-Anwendungen zu beginnen. Vorgeschlagene nächste Schritte umfassen das Lesen über folgende Funktionen:

- Mit Steuerelementvorlagen können auf einfache Weise Designs für Anwendungsseiten zur Laufzeit entworfen und überarbeitet werden. Weitere Informationen finden Sie unter [Steuerelementvorlagen](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).
- Datenvorlagen bieten die Möglichkeit, die Darstellung von Daten für unterstützte Steuerelemente zu definieren. Weitere Informationen finden Sie unter [Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).
- Freigegebener Code kann über die [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)-Klasse auf eine native Funktion zugreifen. Weitere Informationen finden Sie unter [Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) (Zugreifen auf native Funktionen über DependencyService).
- Xamarin.Forms umfasst einen einfachen Messagingdienst zum Senden und Empfangen von Nachrichten. Dadurch wird die Kopplung zwischen Klassen reduziert. Weitere Informationen finden Sie unter [Publish and Subscribe with MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md) (Veröffentlichen und Abonnieren über MessagingCenter).
- Die einzelnen Seiten, Layouts und Steuerelemente werden auf jeder Plattform auf unterschiedliche Weise über eine `Renderer`-Klasse gerendert. Diese erstellt ein natives Steuerelement, ordnet dieses auf dem Bildschirm an und fügt das im freigegebenen Code angegebene Verhalten hinzu. Entwickler können ihre eigenen benutzerdefinierten `Renderer`-Klassen implementieren, um die Darstellung und/oder das Verhalten eines Steuerelements anzupassen. Weitere Informationen finden Sie unter [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) (Benutzerdefinierte Renderer).
- Zudem können durch Effekte native Steuerelemente auf den einzelnen Plattformen angepasst werden. Effekte werden in plattformspezifischen Projekten erstellt, indem Unterklassen für das [`PlatformEffect`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)-Steuerelement erstellt werden. Sie werden verarbeitet, indem sie an das entsprechende Xamarin.Forms-Steuerelement angefügt werden. Weitere Informationen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

Alternativ enthält das Buch „Creating Mobile Apps with Xamarin.Forms“ (Erstellen von mobilen Apps mit Xamarin.Forms) von Charles Petzold weitere Informationen zu Xamarin.Forms. Weitere Informationen finden Sie unter [Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) (Erstellen von mobilen Apps mit Xamarin.Forms).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Einführung in Xamarin.Forms und Informationen zu den ersten Schritten beim Schreiben von Anwendungen. Xamarin.Forms ist eine plattformübergreifende nativ gesicherte Benutzeroberflächen-Toolkit-Abstraktion, mit der Entwickler ohne großen Aufwand Benutzeroberflächen für Android, iOS, Windows und Windows Phone erstellen können. Die Benutzeroberflächen werden mit den nativen Steuerelementen der Zielplattform gerendert. Dadurch können Anwendungen von Xamarin.Forms das Erscheinungsbild der einzelnen Plattformen beibehalten.


## <a name="related-links"></a>Verwandte Links

- [XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md)
- [Steuerelementreferenz](~/xamarin-forms/user-interface/controls/index.md)
- [Benutzeroberfläche](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Beispiele für die ersten Schritte](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [Free Self-Guided Learning (Kostenloses eigenständiges Lernen) (Video)](https://university.xamarin.com/self-guided)
- [Hello, Xamarin.Forms iOS Workbook (Hallo, Xamarin.Forms (iOS-Arbeitsmappe))](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
