---
title: Die Xamarin.Forms-Klasse „TabbedPage“
description: Die TabbedPage-Klasse von Xamarin.Forms besteht aus einer Liste von Registerkarten und aus einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich. In diesem Artikel wird veranschaulicht, wie Sie mit einer TabbedPage-Klasse durch eine Auflistung von Seiten navigieren können.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 4e2d33276c865695d70abb2d8e00b3b80d446839
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207985"
---
# <a name="xamarinforms-tabbed-page"></a>Die Xamarin.Forms-Klasse „TabbedPage“

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)

_Die TabbedPage-Klasse von Xamarin.Forms besteht aus einer Liste von Registerkarten und aus einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich. In diesem Artikel wird veranschaulicht, wie Sie mit einer TabbedPage-Klasse durch eine Auflistung von Seiten navigieren können._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse auf jeder Plattform:

![](tabbed-page-images/tab1.png "Beispiel für TabbedPage")

Die folgenden Screenshots beziehen sich auf das Registerkartenformat auf jeder Plattform:

![](tabbed-page-images/tabbedpage-components.png "Komponenten einer TabbedPage-Registerkarte")

Das Layout einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse und der zugehörigen Registerkarten ist plattformabhängig:

- Unter iOS wird die Liste der Registerkarten am unteren Rand des Bildschirms angezeigt, und der Detailbereich befindet sich darüber. Jede Registerkarte enthält zudem ein Symbol, bei dem es sich um eine PNG-Bilddatei mit 30×30 Pixeln mit Transparenz für eine normale Auflösung, 60×60 Pixeln für eine hohe Auflösung und 90×90 Pixeln für die Auflösung des iPhone 6 Plus handeln sollte. Wenn mehr als fünf Registerkarten vorhanden sind, wird die Registerkarte *More* (Weitere) angezeigt, die verwendet werden kann, um auf die zusätzlichen Registerkarten zuzugreifen. Weitere Informationen zum Laden von Bildern in einer Xamarin.Forms-Anwendung finden Sie unter [Images in Xamarin.Forms (Bilder in Xamarin.Forms)](~/xamarin-forms/user-interface/images.md). Weitere Informationen zu den Anforderungen für Symbole finden Sie unter [Creating Tabbed Applications (Erstellen von Anwendungen mit Registerkarten)](~/ios/user-interface/controls/creating-tabbed-applications.md).

  > [!NOTE]
  > Beachten Sie, dass die `TabbedRenderer`-Klasse für iOS eine überschreibbare `GetIcon`-Methode enthält, die verwendet werden kann, um Registerkartensymbole aus einer angegebenen Quelle zu laden. Durch diese Überschreibung können Sie SVG-Bilder als Symbole in einer `TabbedPage`-Klasse verwenden. Darüber hinaus können die ausgewählte und nicht ausgewählte Versionen eines Symbols zur Verfügung gestellt werden.

- Unter Android wird die Liste der Registerkarten standardmäßig am oberen Rand des Bildschirms angezeigt, und der Detailbereich befindet sich darunter. Die Liste der Registerkarten kann jedoch mit einem plattformspezifischen Element an den unteren Rand des Bildschirms verschoben werden. Weitere Informationen finden Sie unter [Setting TabbedPage Toolbar Placement and Color (Festlegen der Position und Farbe von TabbedPage-Symbolleisten)](~/xamarin-forms/platform/android/tabbedpage-toolbar-placement-color.md).

  > [!NOTE]
  > Beachten Sie, dass für jede Registerkarte ein Symbol angezeigt wird, wenn Sie AppCompat unter Android verwenden. Außerdem enthält `TabbedPageRenderer`-Klasse von AppCompat unter Android eine überschreibbare `GetIconDrawable`-Methode, die verwendet werden kann, um Registerkartensymbole aus einer benutzerdefinierten `Drawable`-Klasse zu laden. Durch diese Überschreibung können SVG-Bilder als Symbole in einer `TabbedPage`-Klasse verwendet werden. Die Bilder funktionieren auf Registerkartenleisten am oberen und am unteren Rand. Alternativ kann die überschreibbare `SetTabIcon`-Methode verwendet werden, um Registerkartensymbole aus einer benutzerdefinierten `Drawable`-Klasse für Registerkartenleisten am oberen Rand zu laden.

- Bei Formfaktoren für Windows-Tablets werden die Registerkarten nicht immer angezeigt, und die Benutzer müssen nach unten wischen (oder mit der rechten Maustaste klicken, wenn eine Maus angeschlossen ist), um die Registerkarten in einer `TabbedPage`-Klasse anzuzeigen (siehe unten).

![](tabbed-page-images/windows-tabs.png "TabbedPage-Registerkarten unter Windows")

## <a name="creating-a-tabbedpage"></a>Erstellen einer TabbedPage-Klasse

Es gibt zwei Ansätze zum Erstellen einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse:

- [Füllen Sie](#Populating_a_TabbedPage_with_a_Page_Collection) die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse mit einer Collection von untergeordneten [`Page`](xref:Xamarin.Forms.Page)-Objekten auf, z.B. mit einer Collection aus [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen.
- [Weisen Sie](#Populating_a_TabbedPage_with_a_Template) der [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft eine Collection zu, und weisen Sie der [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zu, um Seiten für Objekte in der Collection zurückzugeben.

Durch beide Ansätze zeigt die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse die entsprechende Seite an, wenn der Benutzer eine Registerkarte auswählt.

> [!NOTE]
> Es wird empfohlen, die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse nur mit [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)- und [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen aufzufüllen. Dadurch wird eine konsistente Benutzerumgebung auf allen Plattformen gewährleistet.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Auffüllen einer TabbedPage-Klasse mit einer Seitenauflistung

Im folgenden XAML-Codebeispiel wird eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse erstellt, indem diese mit einer Collection von untergeordneten [`Page`](xref:Xamarin.Forms.Page)-Objekten aufgefüllt wird.

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

Im folgenden Codebeispiel wird die entsprechende [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse in C# dargestellt:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse wird mit zwei untergeordneten [`Page`](xref:Xamarin.Forms.Page)-Objekten aufgefüllt. Beim ersten untergeordneten Objekt handelt es sich um eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz, und die zweite Registerkarte ist eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse, die eine `ContentPage`-Instanz enthält.

> [!NOTE]
> Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse unterstützt keine Benutzeroberflächenvirtualisierung. Deshalb kann die Leistung beeinträchtigt werden, wenn die `TabbedPage`-Klasse zu viele untergeordnete Elemente enthält.

Auf den folgenden Screenshots wird die `TodayPage`-Instanz der [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse veranschaulicht, die auf der Registerkarte *Today* (Heute) angezeigt wird:

![](tabbed-page-images/today-page.png "ContentPage-Klasse in einer TabbedPage-Klasse")

Durch das Auswählen der Registerkarte *Schedule* (Zeitplan) wird die `SchedulePage`-Instanz der [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse angezeigt, die von einer Instanz der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse umschlossen ist. Dies ist auf dem folgenden Screenshot zu sehen:

![](tabbed-page-images/schedule-page.png "NavigationPage-Klasse in einer TabbedPage-Klasse")

Weitere Informationen zum Layout einer [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse finden Sie unter [Performing Navigation (Einrichten der Navigation)](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Es wird zwar empfohlen, eine [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse in einer [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse zu verwenden, allerdings sollten Sie keine `TabbedPage`-Klasse in einer `NavigationPage`-Klasse verwenden. Dies liegt darin begründet, dass `UITabBarController` unter iOS immer als Wrapper für `UINavigationController` fungiert. Weitere Informationen finden Sie unter [Combined View Controller Interfaces (Kombinierte Schnittstellen für View Controller)](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) in der iOS Developer Library.

#### <a name="navigation-inside-a-tab"></a>Navigation auf einer Registerkarte

Die Navigation kann auf der zweiten Registerkarte durchgeführt werden, indem die [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync*)-Methode in der [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation)-Eigenschaft der [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanz aufgerufen wird. Dies wird in folgendem Codebeispiel veranschaulicht:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Dies bewirkt, dass die `UpcomingAppointmentsPage`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird im folgenden Screenshot veranschaulicht:

![](tabbed-page-images/navigationpage.png "Navigation auf einer Registerkarte")

Weitere Informationen zur Navigation mithilfe der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Klasse finden Sie unter [Hierarchical Navigation (Hierarchische Navigation)](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Auffüllen einer TabbedPage-Klasse mit einer Vorlage

Das folgende XAML-Codebeispiel zeigt eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse, die durch Zuweisen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft erstellt wurde, um Seiten für Objekte in der Collection zurückzugeben:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
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

Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse wird mit Daten aufgefüllt, indem die [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft im Konstruktor für die CodeBehind-Datei festgelegt wird:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Im folgenden Codebeispiel wird die entsprechende [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse in C# dargestellt:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
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

Jede Registerkarte stellt eine [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse dar, die eine Reihe von [`StackLayout`](xref:Xamarin.Forms.StackLayout)- und [`Label`](xref:Xamarin.Forms.Label)-Instanzen verwendet, um die Daten der Registerkarte anzuzeigen. Auf den folgenden Screenshots ist der Inhalt der Registerkarte *Tamarin* dargestellt:

![](tabbed-page-images/tab3.png "Auffüllen einer TabbedPage-Klasse mit einer Vorlage")

Wenn Sie eine andere Registerkarte auswählen, wird deren Inhalt angezeigt.

> [!NOTE]
> Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse unterstützt keine Benutzeroberflächenvirtualisierung. Deshalb kann die Leistung beeinträchtigt werden, wenn die `TabbedPage`-Klasse zu viele untergeordnete Elemente enthält.

Weitere Informationen zur [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse finden Sie in [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) im Xamarin.Forms-Buch von Charles Petzold.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie mit einer TabbedPage-Klasse durch eine Auflistung von Seiten navigieren können. Die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)-Klasse von Xamarin.Forms besteht aus einer Liste von Registerkarten sowie einem größeren Detailbereich. Jede Registerkarte lädt Inhalt in den Detailbereich.


## <a name="related-links"></a>Verwandte Links

- [Page Varieties (Seitenvarianten)](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPage mit NavigationPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
