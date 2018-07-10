---
title: Karussellsicht Xamarin.Forms-Startseite
description: Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer von Seite zu Seite navigieren können, Navigation durch Seiten, Inhalte, z. B. einem Katalog an. In diesem Artikel veranschaulicht, wie eine CarouselPage durch eine Auflistung von Seiten navigieren.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: b190498911867d29b63d839f56613fb1b80fe56f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935146"
---
# <a name="xamarinforms-carousel-page"></a>Karussellsicht Xamarin.Forms-Startseite

_Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer von Seite zu Seite navigieren können, Navigation durch Seiten, Inhalte, z. B. einem Katalog an. In diesem Artikel veranschaulicht, wie eine CarouselPage durch eine Auflistung von Seiten navigieren._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) auf jeder Plattform:

![](carousel-page-images/thirdpage.png "CarouselPage Thid-Element")

Das Layout einer [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) auf jeder Plattform identisch ist. Seiten können durch Wischen von rechts nach links, vorwärts durch die Auflistung zu navigieren, und durch Wischen nach links nach rechts, um rückwärts navigieren durch die Auflistung über navigiert werden. Die folgenden Screenshots zeigen die erste Seite in einem [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Instanz:

![](carousel-page-images/firstpage.png "CarouselPage erste-Element")

Wischen Sie nach von rechts nach links verschiebt der zweiten Seite an, wie in den folgenden Screenshots gezeigt:

![](carousel-page-images/secondpage.png "CarouselPage Sekunde-Element")

Wischen von rechts nach links erneut verschiebt, auf der dritten Seite Wischen von links nach rechts zur vorherigen Seite zurück.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Erstellen eine CarouselPage

Zwei Ansätze können verwendet werden, um das Erstellen einer [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Füllen Sie](#Populating_a_CarouselPage_with_a_Page_Collection) der `CarouselPage` mit einer Auflistung von untergeordneten [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen.
- [Weisen Sie](#Populating_a_CarouselPage_with_a_Template) einer Auflistung, die die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) -Eigenschaft, und weisen eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) zurückzugebendeEigenschaft[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen für Objekte in der Auflistung.

Mit beiden Ansätzen müssen die `CarouselPage` zeigt dann jede Seite wiederum mit einem streifeninteraktion Wechsel zur nächsten Seite angezeigt werden.

> [!NOTE]
> Ein [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) kann nur mit gefüllt werden [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Instanzen oder `ContentPage` ableitungen.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Auffüllen einer CarouselPage mit einer Auflistung von Datenseiten

Das folgende XAML-Code-Beispiel zeigt eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) angezeigt, die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

Das folgende Codebeispiel zeigt die entsprechende Benutzeroberfläche in c#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Jede [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) zeigt einfach eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) für eine bestimmte Farbe und einen [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) dieser Farbe.

> [!NOTE]
> Die [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI-Virtualisierung wird nicht unterstützt. Aus diesem Grund Leistung möglicherweise betroffen, wenn die `CarouselPage` enthält zu viele untergeordnete Elemente.

Wenn eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) eingebettet ist, in der [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Eigenschaft sollte festgelegt werden, um `false` , Geste Konflikte zu verhindern, dass die `CarouselPage` und `MasterDetailPage`.

Weitere Informationen zu den [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), finden Sie unter [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles petzolds Xamarin.Forms Buch.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Auffüllen einer CarouselPage mit einer Vorlage

Das folgende Beispiel zeigt für die XAML-Code eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) erstellt, die durch das Zuweisen von einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) Seiten für die zurückzugebende Eigenschaft Objekte in der Auflistung:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

Die [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) mit Daten aufgefüllt wird, durch Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) Eigenschaft im Konstruktor für die Code-Behind-Datei:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Das folgende Codebeispiel zeigt die entsprechende [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) in c# erstellt wurde:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Jede [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) zeigt einfach eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) für eine bestimmte Farbe und einen [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) dieser Farbe.

> [!NOTE]
> Die [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI-Virtualisierung wird nicht unterstützt. Aus diesem Grund Leistung möglicherweise betroffen, wenn die `CarouselPage` enthält zu viele untergeordnete Elemente.

Wenn eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) eingebettet ist, in der [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) Eigenschaft sollte festgelegt werden, um `false` , Geste Konflikte zu verhindern, dass die `CarouselPage` und `MasterDetailPage`.

Weitere Informationen zu den [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), finden Sie unter [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles petzolds Xamarin.Forms Buch.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie Sie mit einem [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) durch eine Auflistung von Seiten navigieren. Die `CarouselPage` ist eine Seite, die Benutzer von Seite zu Seite navigieren können, für die Navigation durch Seiten, Inhalte, ähnlich wie einen Katalog.


## <a name="related-links"></a>Verwandte Links

- [Seitenvarianten](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
