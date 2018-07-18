---
title: Xamarin.Forms-Seite im Registerformat
description: Die Xamarin.Forms "tabbedpage" besteht aus einer Liste der Registerkarten und einen größeren Detailbereich, mit jeder Registerkarte Inhalt wird in den Bereich "Details" geladen. In diesem Artikel veranschaulicht, wie eine "tabbedpage" durch eine Auflistung von Seiten navigieren.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 3eb978780222da2050fc91dfa41c68ef4bd3b6f4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996294"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms-Seite im Registerformat

_Die Xamarin.Forms "tabbedpage" besteht aus einer Liste der Registerkarten und einen größeren Detailbereich, mit jeder Registerkarte Inhalt wird in den Bereich "Details" geladen. In diesem Artikel veranschaulicht, wie eine "tabbedpage" durch eine Auflistung von Seiten navigieren._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) auf jeder Plattform:

![](tabbed-page-images/tab1.png "\"Tabbedpage\"-Beispiel")

In den folgenden Screenshots beziehen sich auf die Registerkarte "-Format auf jeder Plattform:

![](tabbed-page-images/tabbedpage-components.png "Registerkarte \"tabbedpage\" \"-Komponenten")

Das Layout einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), und die zugehörigen Registerkarten ist abhängig von der Plattform:

- Unter iOS die Liste der Registerkarten am unteren Rand des Bildschirms angezeigt, und Detailbereich höher ist. Jede Registerkarte enthält auch ein Symbol-Image der eine 30 x 30 PNG mit Transparenz für die Auflösung von normalen, 60 x 60 für hohe Auflösung und 90 x 90, für das iPhone 6 sein soll und Auflösung. Wenn mehr als fünf Registerkarten, es gibt eine *weitere* Registerkarte wird angezeigt, die können verwendet werden, um zusätzlichen Registerkarten zugreifen. Weitere Informationen zum Laden von Bildern in einer Xamarin.Forms-Anwendung finden Sie unter [arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md). Weitere Informationen zu den Symbol-Anforderungen finden Sie unter [erstellen im Registerkartenformat Anwendungen](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Beachten Sie, dass die `TabbedRenderer` für iOS eine überschreibbare hat `GetIcon` -Methode, die mit der Registerkarte Symbole aus einer angegebenen Quelle geladen werden kann. Diese Außerkraftsetzung ist es möglich, SVG-Bildern als Symbole auf einem `TabbedPage`. Darüber hinaus können die ausgewählte und nicht ausgewählte Versionen eines Symbols bereitgestellt werden.

- Unter Android werden die Liste der Registerkarten am oberen Rand des Bildschirms wird standardmäßig angezeigt und Detailbereich unterschreitet. Allerdings kann die Registerkartenliste am unteren Rand der Bildschirm mit einer plattformspezifischen verschoben werden. Weitere Informationen finden Sie unter [Einstellung "tabbedpage" Symbolleiste Platzierung und die Farbe](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar).

    > [!NOTE]
  > Beachten Sie, dass bei Verwendung von AppCompat auf Android jede Registerkarte auch ein Symbol angezeigt wird. Darüber hinaus die `TabbedPageRenderer` für Android AppCompat eine überschreibbare hat `SetTabIcon` -Methode, die verwendet werden kann, zum Laden von Registerkarte Symbole aus einem benutzerdefinierten `Drawable`. Diese Außerkraftsetzung ist es möglich, SVG-Bildern als Symbole auf einem `TabbedPage`.

- Auf Windows-Tablet-Formfaktoren arbeiten, die Registerkarten werden nicht immer angezeigt, und Benutzer müssen Wischen Sie (oder mit der rechten Maustaste, wenn sie eine Maus angefügt haben), zeigen Sie die Registerkarten in einem `TabbedPage` (wie unten gezeigt).

![](tabbed-page-images/windows-tabs.png "Registerkarten \"tabbedpage\" Windows")

## <a name="creating-a-tabbedpage"></a>Erstellen einer "tabbedpage"

Zwei Ansätze können verwendet werden, um das Erstellen einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

- [Füllen Sie](#Populating_a_TabbedPage_with_a_Page_Collection) der [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) mit einer Auflistung von untergeordneten [ `Page` ](xref:Xamarin.Forms.Page) Objekte, z. B. eine Auflistung von [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Instanzen.
- [Weisen Sie](#Populating_a_TabbedPage_with_a_Template) einer Auflistung, die die [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) -Eigenschaft, und weisen eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) auf die [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Seiten für die zurückzugebende Eigenschaft Objekte in der Auflistung.

Mit beiden Ansätzen müssen die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) jeder Seite wird angezeigt, wie der Benutzer auf jeder Registerkarte auswählt.

> [!NOTE]
> Es wird empfohlen, eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) gefüllt werden soll, mit [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) und [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)nur Instanzen. Dadurch wird um eine konsistente benutzererfahrung auf allen Plattformen zu gewährleisten.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Auffüllen einer "tabbedpage" mit einer Auflistung von Datenseiten

Das folgende XAML-Code-Beispiel zeigt eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) erstellt, die durch Auffüllen der Gruppe mit der eine Auflistung von untergeordneten [ `Page` ](xref:Xamarin.Forms.Page) Objekte:

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

Das folgende Codebeispiel zeigt die entsprechende [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) in c# erstellt wurde:

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

Die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) wird mit zwei untergeordneten aufgefüllt [ `Page` ](xref:Xamarin.Forms.Page) Objekte. Das erste untergeordnete Element ist ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz und der zweiten Registerkarte ist eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) , enthält eine `ContentPage` Instanz.

> [!NOTE]
> Die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI-Virtualisierung wird nicht unterstützt. Aus diesem Grund Leistung möglicherweise betroffen, wenn die `TabbedPage` enthält zu viele untergeordnete Elemente.

Die folgenden Screenshots zeigen die `TodayPage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz, die auf angezeigt wird der *heute* Registerkarte:

![](tabbed-page-images/today-page.png "ContentPage in einer \"tabbedpage\"")

Auswählen der *Zeitplan* Registerkarte zeigt die `SchedulePage` [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz, die umschlossen wird eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz, und sehen Sie in der bildschirmabbildung von folgenden:

![](tabbed-page-images/schedule-page.png "\"NavigationPage\" in einer \"tabbedpage\"")

Informationen über das Layout einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), finden Sie unter [Navigation ausführen](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Es akzeptabel, platzieren Sie zwar eine [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) in eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), es wird nicht empfohlen, platzieren ein `TabbedPage` in eine `NavigationPage`. Dies liegt daran, unter iOS, einem `UITabBarController` immer dient als Wrapper für die `UINavigationController`. Weitere Informationen finden Sie unter [View Controller-Schnittstellen kombiniert](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) in der iOS Developer Library.

#### <a name="navigation-inside-a-tab"></a>Navigation auf einer Registerkarte

Navigation auf der zweiten Registerkarte ausgeführt werden kann, durch den Aufruf der [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync*) Methode für die [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) Eigenschaft der [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Instanz wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Dies bewirkt, dass die `UpcomingAppointmentsPage`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird in den folgenden Screenshots gezeigt:

![](tabbed-page-images/navigationpage.png "Navigation auf einer Registerkarte")

Weitere Informationen zum Ausführen der Navigation mithilfe der [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Klasse, finden Sie unter [hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Auffüllen einer "tabbedpage" mithilfe einer Vorlage

Das folgende Beispiel zeigt für die XAML-Code eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) erstellt, die durch das Zuweisen von einer [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) auf die [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Seiten für die zurückzugebende Eigenschaft Objekte in der Auflistung:

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

Die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) mit Daten aufgefüllt wird, durch Festlegen der [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) Eigenschaft im Konstruktor für die Code-Behind-Datei:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Das folgende Codebeispiel zeigt die entsprechende [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) in c# erstellt wurde:

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

Jede Registerkarte zeigt ein [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) , verwendet eine Reihe von [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen zum Anzeigen von Daten für die Registerkarte. Die folgenden Screenshots zeigen den Inhalt für die *Tamarin* Registerkarte:

![](tabbed-page-images/tab3.png "Auffüllen einer \"tabbedpage\" mithilfe einer Vorlage")

Wählen dann eine andere Registerkarte zeigt Inhalt für die Registerkarte an.

> [!NOTE]
> Die [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) UI-Virtualisierung wird nicht unterstützt. Aus diesem Grund Leistung möglicherweise betroffen, wenn die `TabbedPage` enthält zu viele untergeordnete Elemente.

Weitere Informationen zu den [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), finden Sie unter [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles petzolds Xamarin.Forms Buch.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie eine "tabbedpage" verwenden, um durch eine Auflistung von Seiten zu navigieren. Die Xamarin.Forms [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) besteht aus einer Liste der Registerkarten und einen größeren Detailbereich, und jede Registerkarte Inhalt wird in den Bereich "Details" geladen.


## <a name="related-links"></a>Verwandte Links

- [Seitenvarianten](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- ["Tabbedpage" (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](xref:Xamarin.Forms.TabbedPage)
