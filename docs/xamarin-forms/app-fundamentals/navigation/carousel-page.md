---
title: Seite "Karussell"
description: Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer nebeneinander navigieren Sie können zum Navigieren durch die Seiten des Inhalts, wie einem Katalog. Dieser Artikel veranschaulicht, wie eine CarouselPage durch eine Auflistung von Seiten navigieren.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 035254f87e52801d5ff7419f9ad9d5503f060020
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="carousel-page"></a>Seite "Karussell"

_Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer nebeneinander navigieren Sie können zum Navigieren durch die Seiten des Inhalts, wie einem Katalog. Dieser Artikel veranschaulicht, wie eine CarouselPage durch eine Auflistung von Seiten navigieren._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) auf jeder Plattform:

![](carousel-page-images/thirdpage.png "CarouselPage Thid-Element")

Das Layout einer [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) auf jeder Plattform identisch ist. Seiten können durch ein Lesegerät von rechts nach links Navigieren durch die Auflistung und ein Lesegerät von links nach rechts, um rückwärts navigieren in der Auflistung über navigiert werden. Die folgenden Screenshots zeigen die erste Seite in einem [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Instanz:

![](carousel-page-images/firstpage.png "CarouselPage erste-Element")

Streifen von rechts nach links bewegt, die zweite Seite, wie in den folgenden Screenshots dargestellt:

![](carousel-page-images/secondpage.png "CarouselPage Sekunde-Element")

Ein Lesegerät erneut von rechts nach links verschiebt der dritten Seite Streifen von links nach rechts zur vorherigen Seite zurück.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Erstellen eine CarouselPage

Zwei Ansätze können verwendet werden, um das Erstellen einer [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Auffüllen](#Populating_a_CarouselPage_with_a_Page_Collection) der `CarouselPage` mit einer Auflistung von untergeordneten [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen.
- [Zuweisen](#Populating_a_CarouselPage_with_a_Template) eine Sammlung aus, die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) Eigenschaft und weisen eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) zurückzugebendeEigenschaft[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen für Objekte in der Auflistung.

Mit beiden Ansätzen müssen die `CarouselPage` zeigt dann jede Seite wiederum mit einem Streifen Interaktion verschieben zur nächsten Seite angezeigt werden. 

> [!NOTE]
> Ein [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) nur ausgefüllt werden, die mit [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen oder `ContentPage` ableitungen.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Auffüllen einer CarouselPage mit einem Auflistungs-Seite

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) , die zeigt drei [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Instanzen:

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

Jedes [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) zeigt einfach eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) für eine bestimmte Farbe und einem [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) dieser Farbe.

> [!NOTE]
> Die [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) die Virtualisierung der Benutzeroberfläche nicht unterstützt. Aus diesem Grund Leistung kann beeinträchtigt sein, falls die `CarouselPage` enthält zu viele untergeordnete Elemente.

Wenn eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) eingebettet ist, in der [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Eigenschaft sollte festgelegt werden, um `false` um Geste Konflikte vermeiden der `CarouselPage` und `MasterDetailPage`.

Weitere Informationen zu den [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), finden Sie unter [Chapter 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charless Xamarin.Forms Buch.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Auffüllen einer CarouselPage mit einer Vorlage

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) erstellt durch Zuweisen einer [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) auf die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) -Eigenschaft zum Zurückgeben von Seiten für Objekte in der Auflistung:

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

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) in c# erstellt:

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

Jedes [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) zeigt einfach eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) für eine bestimmte Farbe und einem [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) dieser Farbe.

> [!NOTE]
> Die [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) die Virtualisierung der Benutzeroberfläche nicht unterstützt. Aus diesem Grund Leistung kann beeinträchtigt sein, falls die `CarouselPage` enthält zu viele untergeordnete Elemente.

Wenn eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) eingebettet ist, in der [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) Eigenschaft sollte festgelegt werden, um `false` um Geste Konflikte vermeiden der `CarouselPage` und `MasterDetailPage`.

Weitere Informationen zu den [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), finden Sie unter [Chapter 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charless Xamarin.Forms Buch.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie eine [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) durch eine Auflistung von Seiten navigieren. Die `CarouselPage` ist eine Seite, die Benutzer nebeneinander navigieren Sie können zum Navigieren durch die Seiten des Inhalts, ähnlich wie ein Katalog.


## <a name="related-links"></a>Verwandte Links

- [Seite "-Varianten](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
