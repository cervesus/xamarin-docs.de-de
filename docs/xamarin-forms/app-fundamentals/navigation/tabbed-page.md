---
title: 'Xamarin.Forms: TabbedPage'
description: Die „TabbedPage“-Klasse von Xamarin.Forms besteht aus einer Liste mit Registerkarten und einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich. In diesem Artikel wird veranschaulicht, wie Sie mit einer TabbedPage-Klasse durch eine Auflistung von Seiten navigieren können.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 38389867ba52e63d8310e3b59d7838f58e8cf488
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137513"
---
# <a name="xamarinforms-tabbedpage"></a>Xamarin.Forms: TabbedPage

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse von Xamarin.Forms besteht aus einer Liste mit Registerkarten sowie einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich. Die folgenden Screenshots zeigen eine `TabbedPage` unter iOS und Android:

[![Screenshot einer TabbedPage mit drei Registerkarten unter iOS und Android](tabbed-page-images/tabbedpage-today.png "TabbedPage mit drei Registerkarten")](tabbed-page-images/tabbedpage-today-large.png#lightbox "TabbedPage mit drei Registerkarten")

Unter iOS wird die Liste der Registerkarten am unteren Rand des Bildschirms angezeigt, und der Detailbereich befindet sich darüber. Jede Registerkarte besteht aus einem Titel und einem Symbol, bei dem es sich um eine PNG-Datei mit einem Alphakanal handeln sollte. Im Hochformat werden die Symbole der Registerkartenleiste über den Registerkartentiteln angezeigt. Im Querformat werden die Symbole und Titel nebeneinander angezeigt. Außerdem kann je nach Gerät und Ausrichtung eine reguläre oder kompakte Registerkartenleiste angezeigt werden. Wenn mehr als fünf Registerkarten vorhanden sind, wird die Registerkarte **More** (Weitere) angezeigt, die verwendet werden kann, um auf die zusätzlichen Registerkarten zuzugreifen. Weitere Informationen zu den Symbolanforderungen finden Sie unter [Tab Bar Icon Size](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons#tab-bar-icon-size) (Größe von Symbolen auf Registerkartenleisten) auf developer.apple.com.

> [!TIP]
> Die `TabbedRenderer`-Klasse für iOS enthält eine überschreibbare `GetIcon`-Methode, die verwendet werden kann, um Registerkartensymbole aus einer angegebenen Quelle zu laden. Durch diese Überschreibung können Sie SVG-Bilder als Symbole in einer `TabbedPage`-Klasse verwenden. Darüber hinaus können die ausgewählte und nicht ausgewählte Versionen eines Symbols zur Verfügung gestellt werden.

Unter Android wird die Liste der Registerkarten am oberen Rand des Bildschirms angezeigt, und der Detailbereich befindet sich darunter. Jede Registerkarte besteht aus einem Titel und einem Symbol, bei dem es sich um eine PNG-Datei mit einem Alphakanal handeln sollte. Die Registerkarten können jedoch mit einem plattformspezifischen Element an den unteren Rand des Bildschirms verschoben werden. Wenn mehr als fünf Registerkarten vorhanden sind und die Registerkartenliste sich am unteren Rand des Bildschirms befindet, wird eine Registerkarte *More* (Weitere) angezeigt, die für den Zugriff auf zusätzliche Registerkarten verwendet werden kann. Weitere Informationen zu den Symbolanforderungen finden Sie unter [Tabs](https://material.io/components/tabs/#) (Registerkarten) auf material.io und [Support different pixel densities](https://developer.android.com/training/multiscreen/screendensities) (Unterstützung verschiedener Pixeldichten) auf developer.android.com. Weitere Informationen zum Verschieben der Registerkarten an den unteren Bildschirmrand finden Sie unter [Setting TabbedPage Toolbar Placement and Color](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md) (Festlegen der TabbedPage-Symbolleistenanordnung und -farbe).

> [!TIP]
> Die `TabbedPageRenderer`-Klasse von AppCompat unter Android enthält eine überschreibbare `GetIconDrawable`-Methode, die verwendet werden kann, um Registerkartensymbole aus einer benutzerdefinierten `Drawable`-Klasse zu laden. Durch diese Überschreibung können SVG-Bilder als Symbole in einer `TabbedPage`-Klasse verwendet werden. Die Bilder funktionieren auf Registerkartenleisten am oberen und am unteren Rand. Alternativ kann die überschreibbare `SetTabIcon`-Methode verwendet werden, um Registerkartensymbole aus einer benutzerdefinierten `Drawable`-Klasse für Registerkartenleisten am oberen Rand zu laden.

Auf der universellen Windows-Plattform (UWP) wird die Liste der Registerkarten am oberen Rand des Bildschirms angezeigt, und der Detailbereich befindet sich darunter. Jede Registerkarte umfasst einen Titel. Den Registerkarten können allerdings mit einem plattformspezifischen Element Symbole hinzugefügt werden. Weitere Informationen finden Sie unter [TabbedPage Icons on Windows](~/xamarin-forms/platform/windows/tabbedpage-icons.md) (TabbedPage-Symbole unter Windows).

## <a name="create-a-tabbedpage"></a>Erstellen einer TabbedPage

Es gibt zwei Ansätze zum Erstellen einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse:

- Füllen Sie die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse mit einer Sammlung von untergeordneten [`Page`](xref:Xamarin.Forms.Page)-Objekten auf, z. B. einer Sammlung von [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten. Weitere Informationen finden Sie unter [Populate a TabbedPage with a Page Collection](#populate-a-tabbedpage-with-a-page-collection) (Füllen einer TabbedPage mit einer Seitensammlung).
- Weisen Sie der [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft eine Sammlung und der [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) zu, um Seiten für Objekte in der Sammlung zurückzugeben. Weitere Informationen finden Sie unter [Populate a TabbedPage with a template](#populate-a-tabbedpage-with-a-template) (Füllen einer TabbedPage mithilfe einer Vorlage).

Durch beide Ansätze zeigt die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse die entsprechende Seite an, wenn der Benutzer eine Registerkarte auswählt.

> [!IMPORTANT]
> Es wird empfohlen, die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse nur mit [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)- und [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen aufzufüllen. Dadurch wird eine konsistente Benutzerumgebung auf allen Plattformen gewährleistet.

Darüber hinaus definiert [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) die folgenden Eigenschaften:

- [`BarBackgroundColor`](xref:Xamarin.Forms.TabbedPage.BarBackgroundColor) vom Typ [`Color`](xref:Xamarin.Forms.Color): die Hintergrundfarbe der Registerkartenleiste
- [`BarTextColor`](xref:Xamarin.Forms.TabbedPage.BarTextColor) vom Typ [`Color`](xref:Xamarin.Forms.Color): die Textfarbe auf der Registerkartenleiste
- [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) vom Typ [`Color`](xref:Xamarin.Forms.Color): die Registerkartenfarbe, wenn ausgewählt
- [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) vom Typ [`Color`](xref:Xamarin.Forms.Color): die Registerkartenfarbe, wenn nicht ausgewählt

Alle diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften formatiert werden können und die Ziele von Datenverbindungen sein können.

> [!WARNING]
> In einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) wird jedes [`Page`](xref:Xamarin.Forms.Page)-Objekt bei der Erstellung von `TabbedPage` erstellt. Dies kann insbesondere dann das Benutzererlebnis beeinträchtigen, wenn die `TabbedPage` die Stammseite der Anwendung ist. Die Xamarin.Forms-Shell ermöglicht es aber, dass Seiten, auf die über eine Registerkartenleiste zugegriffen wird, bei Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Xamarin.Forms-Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## <a name="populate-a-tabbedpage-with-a-page-collection"></a>Auffüllen einer TabbedPage mit einer Seitensammlung

Sie können eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) mit einer Sammlung von untergeordneten [`Page`](xref:Xamarin.Forms.Page)-Objekten auffüllen, z. B. einer Sammlung von [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekten. Dies wird erreicht, indem der [`TabbedPage.Children`](xref:Xamarin.Forms.MultiPage`1.Children*)-Sammlung die `Page`-Objekte hinzugefügt werden. Dies kann in XAML folgendermaßen durchgeführt werden:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" IconImageSource="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

> [!NOTE]
> Die [`Children`](xref:Xamarin.Forms.MultiPage`1.Children*)-Eigenschaft der [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1)-Klasse, von der [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) abgeleitet ist, ist die `ContentProperty` von `MultiPage<T>`. Daher ist es in XAML nicht notwendig, die [`Page`](xref:Xamarin.Forms.Page)-Objekte explizit der `Children`-Eigenschaft zuzuweisen.

Der entsprechende C#-Code lautet:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    NavigationPage navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.IconImageSource = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

In diesem Beispiel wird die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) mit zwei [`Page`](xref:Xamarin.Forms.ContentPage)-Objekten aufgefüllt. Beim ersten untergeordneten Objekt handelt es sich um ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, und beim zweiten um eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), die ein `ContentPage`-Objekt enthält.

Die folgenden Screenshots zeigen ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt in einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage):

[![Screenshot einer TabbedPage mit drei Registerkarten unter iOS und Android](tabbed-page-images/tabbedpage-today.png "TabbedPage mit drei Registerkarten")](tabbed-page-images/tabbedpage-today-large.png#lightbox "TabbedPage mit drei Registerkarten")

Durch das Auswählen einer anderen Registerkarte wird das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt angezeigt, das die Registerkarte darstellt:

[![Screenshot einer TabbedPage mit Registerkarten unter iOS und Android](tabbed-page-images/tabbedpage-week.png "TabbedPage mit Registerkarten")](tabbed-page-images/tabbedpage-week-large.png#lightbox "TabbedPage mit Registerkarten")

Auf der Registerkarte **Schedule** (Zeitplan) ist das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt von einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Objekt umschlossen.

> [!WARNING]
> Während eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) in eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) eingefügt werden kann, wird davon abgeraten, eine `TabbedPage` in eine `NavigationPage` einzufügen. Dies liegt darin begründet, dass `UITabBarController` unter iOS immer als Wrapper für `UINavigationController` fungiert. Weitere Informationen finden Sie unter [Combined View Controller Interfaces (Kombinierte Schnittstellen für View Controller)](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) in der iOS Developer Library.

## <a name="navigate-within-a-tab"></a>Navigieren auf einer Registerkarte

Die Navigation kann auf einer Registerkarte ausgeführt werden, wenn das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt von einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Objekt umschlossen wird. Dies wird durch die Aufrufen der [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*)-Methode in der [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation)-Eigenschaft des [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekts erreicht:

```csharp
await Navigation.PushAsync (new UpcomingAppointmentsPage ());
```

Die Seite, die das Ziel der Navigation ist, wird der [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*)-Methode als Argument angegeben. In diesem Beispiel wird die Seite `UpcomingAppointmentsPage` auf den Navigationsstapel gepusht und dort zur aktiven Seite:

[![Screenshot der Navigation auf einer Registerkarte unter iOS und Android](tabbed-page-images/tabbedpage-upcoming.png "TabbedPage-Navigation auf einer Registerkarte")](tabbed-page-images/tabbedpage-upcoming-large.png#lightbox "TabbedPage-Navigation auf einer Registerkarte")

Weitere Informationen zur Navigation mithilfe der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse finden Sie unter [Hierarchical Navigation (Hierarchische Navigation)](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="populate-a-tabbedpage-with-a-template"></a>Auffüllen einer TabbedPage mithilfe einer Vorlage

Eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) kann mit Seiten aufgefüllt werden, indem der [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft eine Sammlung von Daten zugewiesen wird. Dazu wird der [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) zugewiesen, die als Vorlage der Daten in Form von [`Page`](xref:Xamarin.Forms.Page)-Objekten fungiert. Dies kann in XAML folgendermaßen durchgeführt werden:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage"
            ItemsSource="{x:Static local:MonkeyDataModel.All}">            
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" IconImageSource="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

Der entsprechende C#-Code lautet:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() =>
    {
      var nameLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label
      {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage
      {
        IconImageSource = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children =
          {
            nameLabel,
            image,
            new StackLayout
            {
              Padding = new Thickness (50, 10),
              Children =
              {
                new StackLayout
                {
                  Orientation = StackOrientation.Horizontal,
                  Children =
                  {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                // ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

In diesem Beispiel enthält jede Registerkarte ein [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das [`Image`](xref:Xamarin.Forms.Image)-Objekte und [`Label`](xref:Xamarin.Forms.Label)-Objekte für das Anzeigen von Daten für die Registerkarte verwendet:

[![Screenshot einer auf einer Vorlage basierenden TabbedPage unter iOS und Android](tabbed-page-images/tabbedpage-template.png "Auf Vorlage basierende TabbedPage")](tabbed-page-images/tabbedpage-template-large.png#lightbox "Auf Vorlage basierende TabbedPage")

Durch das Auswählen einer anderen Registerkarte wird das [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt angezeigt, das die Registerkarte darstellt.

## <a name="related-links"></a>Verwandte Links

- [TabbedPage mit NavigationPage (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage)
- [TabbedPage (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage)
- [Hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [Page Varieties (Seitenvarianten)](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPage-API](xref:Xamarin.Forms.TabbedPage)
