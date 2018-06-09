---
title: Xamarin.Forms-Seite im Registerformat
description: Der Xamarin.Forms TabbedPage besteht aus einer Liste von Registerkarten und ein größerer Detailbereich verfügbar, und jede Registerkarte Laden des Inhalts in den Bereich "Details". Dieser Artikel veranschaulicht, wie eine TabbedPage durch eine Auflistung von Seiten navigieren.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: b7e3eb8539704fccd713af45490c35a6196b072f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240378"
---
# <a name="xamarinforms-tabbed-page"></a>Xamarin.Forms-Seite im Registerformat

_Der Xamarin.Forms TabbedPage besteht aus einer Liste von Registerkarten und ein größerer Detailbereich verfügbar, und jede Registerkarte Laden des Inhalts in den Bereich "Details". Dieser Artikel veranschaulicht, wie eine TabbedPage durch eine Auflistung von Seiten navigieren._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) auf jeder Plattform:

![](tabbed-page-images/tab1.png "TabbedPage-Beispiel")

Die folgenden Screenshots konzentrieren sich auf die Registerkarte "-Format auf jeder Plattform:

![](tabbed-page-images/tabbedpage-components.png "Registerkarte \"TabbedPage\"-Komponenten")

Das Layout einer [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), die Registerkarten "und" ist abhängig von der Plattform:

- Klicken Sie auf iOS Registerkarten am unteren Rand des Bildschirms aufgeführt und Detailbereich oben ist. Jede Registerkarte enthält auch ein Symbol-Image, die eine 30 x 30 PNG Zwischenfarbe für normalen, 60 x 60 für hochauflösende und 90 x 90 für iPhone 6 ausgeführt werden sowie die Auflösung. Wenn mehr als fünf Registerkarten sind eine *weitere* Registerkarte wird angezeigt, die verwendet werden können um zusätzlichen Registerkarten zugreifen. Weitere Informationen über das Laden von Bildern in einer Xamarin.Forms-Anwendung finden Sie unter [arbeiten mit Bildern](~/xamarin-forms/user-interface/images.md). Weitere Informationen zu den Symbol-Anforderungen finden Sie unter [erstellen im Registerkartenformat Anwendungen](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Beachten Sie, dass die `TabbedRenderer` für iOS eine überschreibbare hat `GetIcon` -Methode, die mit der Registerkarte Symbole aus einer angegebenen Quelle geladen werden kann. Diese Außerkraftsetzung macht das SVG-Bilder als Symbole verwenden, auf eine `TabbedPage`. Darüber hinaus können ausgewählte und nicht ausgewählte Versionen eines Symbols bereitgestellt werden.

- Auf Android-Geräten Registerkarten am oberen Rand des Bildschirms aufgeführt, und der Detailbereich unterschreitet. Registerkartennamen werden automatisch aktiviert, und die Benutzer kann die Auflistung der Registerkarten einen Bildlauf durchführen, wenn zu viele auf einem Bildschirm passt.

    > [!NOTE]
  > Beachten Sie, dass bei Verwendung von AppCompat für Android jede Registerkarte auch ein Symbol angezeigt wird. Darüber hinaus die `TabbedPageRenderer` für Android AppCompat eine überschreibbare hat `SetTabIcon` -Methode, die verwendet werden kann, aus einer benutzerdefinierten Registerkarte Symbole laden `Drawable`. Diese Außerkraftsetzung macht das SVG-Bilder als Symbole verwenden, auf eine `TabbedPage`.

- Nicht im Windows-Tablet-Formfaktoren arbeiten, die Registerkarten sind immer sichtbar, und Benutzer müssen Wischen Sie (oder mit der rechten Maustaste, wenn sie eine Maus angefügt haben) zum Anzeigen von Registerkarten in einem `TabbedPage` (wie unten gezeigt).

![](tabbed-page-images/windows-tabs.png "TabbedPage Registerkarten unter Windows")

## <a name="creating-a-tabbedpage"></a>Erstellen eine TabbedPage

Zwei Ansätze können verwendet werden, um das Erstellen einer [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Auffüllen](#Populating_a_TabbedPage_with_a_Page_Collection) der [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) mit einer Auflistung von untergeordneten [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Objekte, z. B. eine Auflistung von [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen.
- [Weisen Sie](#Populating_a_TabbedPage_with_a_Template) eine Sammlung aus, der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) Eigenschaft und weisen eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) zu der [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) -Eigenschaft zum Zurückgeben von Seiten für Objekte in der Auflistung.

Mit beiden Ansätzen müssen die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) jeder Seite angezeigt, wie der Benutzer jede Registerkarte auswählt.

> [!NOTE]
> Es wird empfohlen, die eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) aufgefüllt werden soll, mit [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) und [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)nur Instanzen. Dies hilft, um eine konsistente benutzerumgebung für alle Plattformen sicherzustellen.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Auffüllen einer TabbedPage mit einem Auflistungs-Seite

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) erstellt, die durch Auffüllen mit einer Auflistung von untergeordneten [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Objekte:

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

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) in c# erstellt:

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

Die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) wird mit zwei untergeordneten aufgefüllt [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Objekte. Das erste untergeordnete Element ist ein [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Instanz, und die zweite Registerkarte ist eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) , enthält eine `ContentPage` Instanz.

> [!NOTE]
> Die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) die Virtualisierung der Benutzeroberfläche nicht unterstützt. Aus diesem Grund Leistung kann beeinträchtigt sein, falls die `TabbedPage` enthält zu viele untergeordnete Elemente.

Die folgenden Screenshots zeigen die `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Instanz, die auf angezeigt wird der *heute* Registerkarte:

![](tabbed-page-images/today-page.png "ContentPage in einem TabbedPage verwendet wird.")

Auswählen der *Zeitplan* Registerkarte zeigt die `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Instanz, die umschlossen wird eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Serverinstanz aus, und wird angezeigt, der bildschirmabbildung von folgenden:

![](tabbed-page-images/schedule-page.png "In einem TabbedPage NavigationPage")

Informationen zum Layout von einem [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), finden Sie unter [Navigation ausführen](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Während es zulässig ist, platzieren Sie eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) in einer [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), es wird nicht empfohlen, platzieren Sie eine `TabbedPage` in einer `NavigationPage`. Dies liegt daran, dass bei iOS kann eine `UITabBarController` immer dient als Wrapper für die `UINavigationController`. Weitere Informationen finden Sie unter [View-Controller-Schnittstellen kombiniert](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) in der iOS Developer Library.

#### <a name="navigation-inside-a-tab"></a>Navigation innerhalb einer Registerkarte

Navigation auf der zweiten Registerkarte ausgeführt werden kann, durch den Aufruf der [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) Methode für die [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Eigenschaft der [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanz wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

Dies bewirkt, dass die `UpcomingAppointmentsPage`-Instanz mithilfe von Push auf den Navigationsstapel übertragen wird, wo sie dann zur aktiven Seite wird. Dies wird in den folgenden Screenshots veranschaulicht:

![](tabbed-page-images/navigationpage.png "Navigation innerhalb einer Registerkarte")

Weitere Informationen zur Durchführung der Navigation mithilfe der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Klasse, finden Sie unter [hierarchische Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Auffüllen einer TabbedPage mit einer Vorlage

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) erstellt durch Zuweisen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) -Eigenschaft zum Zurückgeben von Seiten für Objekte in der Auflistung:

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

Die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) mit Daten aufgefüllt wird, durch Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) Eigenschaft im Konstruktor für die Code-Behind-Datei:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) in c# erstellt:

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

Jede Registerkarte zeigt ein [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) , verwendet eine Reihe von [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) und [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) -Instanzen, die Daten für die Registerkarte angezeigt. Die folgenden Screenshots zeigen den Inhalt für die *Tamarin* Registerkarte:

![](tabbed-page-images/tab3.png "Auffüllen einer TabbedPage mit einer Vorlage")

Eine andere Registerkarte klicken Sie dann auswählen, zeigt Inhalte für die Registerkarte.

> [!NOTE]
> Die [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) die Virtualisierung der Benutzeroberfläche nicht unterstützt. Aus diesem Grund Leistung kann beeinträchtigt sein, falls die `TabbedPage` enthält zu viele untergeordnete Elemente.

Weitere Informationen zu den [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), finden Sie unter [Chapter 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charless Xamarin.Forms Buch.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie eine TabbedPage zu verwenden, um durch eine Auflistung von Seiten zu navigieren. Der Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) besteht aus einer Liste von Registerkarten und ein größerer Detailbereich verfügbar, und jede Registerkarte Laden des Inhalts in den Bereich "Details".


## <a name="related-links"></a>Verwandte Links

- [Seite "-Varianten](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
