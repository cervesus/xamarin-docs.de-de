---
title: Xamarin.Forms-Karussellseite
description: Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren. In diesem Artikel wird gezeigt, wie Sie mit einer CarouselPage durch eine Sammlung von Seiten navigieren können.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 48c009b836ac109e0d54cd2fdb036c46e17c4387
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121502"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms-Karussellseite

_Die Xamarin.Forms-CarouselPage ist eine Seite, auf der Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren. In diesem Artikel wird gezeigt, wie Sie mit einer CarouselPage durch eine Sammlung von Seiten navigieren können._

## <a name="overview"></a>Übersicht

Die folgenden Screenshots zeigen eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse auf jeder Plattform:

![](carousel-page-images/thirdpage.png "CarouselPage, drittes Element")

Das Layout einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse ist auf jeder Plattform identisch. Sie können durch Wischen von rechts nach links vorwärts durch die Sammlung navigieren oder durch Wischen von links nach rechts rückwärts durch die Sammlung navigieren. Die folgenden Screenshots zeigen die erste Seite in einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Instanz:

![](carousel-page-images/firstpage.png "CarouselPage, erstes Element")

Durch Wischen von rechts nach links gelangen Sie wie in den folgenden Screenshots gezeigt auf die zweite Seite:

![](carousel-page-images/secondpage.png "CarouselPage, zweites Element")

Durch erneutes Wischen von rechts nach links gelangen Sie zur dritten Seite, durch Wischen von links nach rechts hingegen auf die vorherige Seite.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Erstellen einer CarouselPage

Es gibt zwei Ansätze zum Erstellen einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse:

- [Auffüllen](#Populating_a_CarouselPage_with_a_Page_Collection) der `CarouselPage`-Klasse mit einer Collection untergeordneter [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen.
- [Zuweisen](#Populating_a_CarouselPage_with_a_Template) einer Sammlung zur [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft sowie Zuweisen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft, um [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen für Objekte in der Sammlung zurückzugeben.

Bei beiden Ansätzen zeigt die `CarouselPage`-Klasse dann Seite um Seite an, wobei durch Wischen zur nächsten anzuzeigenden Seite gewechselt werden kann.

> [!NOTE]
> Eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse kann nur mit [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen oder `ContentPage`-Ableitungen aufgefüllt werden.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Auffüllen einer CarouselPage mit einer Seitensammlung

Das folgende XAML-Codebeispiel zeigt eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse mit drei [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen:

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

Im folgenden Codebeispiel wird die entsprechende Benutzeroberfläche in C# dargestellt:

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

Jede [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse zeigt einfach eine [`Label`](xref:Xamarin.Forms.Label)-Klasse für eine bestimmte Farbe und eine [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse dieser Farbe an.

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse unterstützt keine Benutzeroberflächenvirtualisierung. Deshalb kann die Leistung beeinträchtigt werden, wenn die `CarouselPage`-Klasse zu viele untergeordnete Elemente enthält.

Wenn eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse in die [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)-Seite einer [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse eingebettet ist, sollte die [`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)-Eigenschaft auf `false` festgelegt werden, um Gestenkonflikte zwischen `CarouselPage` und `MasterDetailPage` zu vermeiden.

Weitere Informationen zur [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse finden Sie in [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) im Xamarin.Forms-Handbuch von Charles Petzold.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Auffüllen einer CarouselPage mit einer Vorlage

Das folgende XAML-Codebeispiel zeigt eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse, die durch Zuweisen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft erstellt wurde, um Seiten für Objekte in der Collection zurückzugeben:

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

Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse wird mit Daten aufgefüllt, indem die [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft im Konstruktor für die CodeBehind-Datei festgelegt wird:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

Im folgenden Codebeispiel wird die entsprechende [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse in C# dargestellt:

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

Jede [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Klasse zeigt einfach eine [`Label`](xref:Xamarin.Forms.Label)-Klasse für eine bestimmte Farbe und eine [`BoxView`](xref:Xamarin.Forms.BoxView)-Klasse dieser Farbe an.

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse unterstützt keine Benutzeroberflächenvirtualisierung. Deshalb kann die Leistung beeinträchtigt werden, wenn die `CarouselPage`-Klasse zu viele untergeordnete Elemente enthält.

Wenn eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse in die [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)-Seite einer [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse eingebettet ist, sollte die [`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)-Eigenschaft auf `false` festgelegt werden, um Gestenkonflikte zwischen `CarouselPage` und `MasterDetailPage` zu vermeiden.

Weitere Informationen zur [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse finden Sie in [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) im Xamarin.Forms-Handbuch von Charles Petzold.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie Sie mit einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) durch eine Sammlung von Seiten navigieren können. Die `CarouselPage` ist eine Seite, die Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren.


## <a name="related-links"></a>Verwandte Links

- [Page Varieties (Seitenvarianten)](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
