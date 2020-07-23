---
title: Xamarin.Forms-CarouselPage
description: Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren. In diesem Artikel wird gezeigt, wie Sie mit einer CarouselPage durch eine Sammlung von Seiten navigieren können.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c234af1a5d47446149c92a71e9ce592dc0366b8f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937461"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms-CarouselPage

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)

_Die Xamarin.Forms-CarouselPage ist eine Seite, die Benutzer hin und her wischen können, um durch Inhaltsseiten wie durch einen Katalog zu navigieren. In diesem Artikel wird gezeigt, wie Sie mit einer CarouselPage durch eine Sammlung von Seiten navigieren können._

> [!IMPORTANT]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse wurde durch die [`CarouselView`](xref:Xamarin.Forms.CarouselView)-Klasse ersetzt. Diese bietet ein scrollbares Layout, bei dem Benutzer durch Wischen durch eine Auflistung von Elementen navigieren können. Weitere Informationen zu `CarouselView` finden Sie unter [Xamarin.Forms CarouselView](~/xamarin-forms/user-interface/carouselview/index.md).

Die folgenden Screenshots zeigen eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse auf jeder Plattform:

![CarouselPage, drittes Element](carousel-page-images/thirdpage.png)

Das Layout einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse ist auf jeder Plattform identisch. Sie können durch Wischen von rechts nach links vorwärts durch die Sammlung navigieren oder durch Wischen von links nach rechts rückwärts durch die Sammlung navigieren. Die folgenden Screenshots zeigen die erste Seite in einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Instanz:

![CarouselPage, erstes Element](carousel-page-images/firstpage.png)

Durch Wischen von rechts nach links gelangen Sie wie in den folgenden Screenshots gezeigt auf die zweite Seite:

![CarouselPage, zweites Element](carousel-page-images/secondpage.png)

Durch erneutes Wischen von rechts nach links gelangen Sie zur dritten Seite, durch Wischen von links nach rechts hingegen auf die vorherige Seite.

> [!NOTE]
> Die [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse unterstützt keine Benutzeroberflächenvirtualisierung. Deshalb kann die Leistung beeinträchtigt werden, wenn die `CarouselPage`-Klasse zu viele untergeordnete Elemente enthält.

Wenn eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse in die [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)-Seite einer [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Klasse eingebettet ist, sollte die [`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)-Eigenschaft auf `false` festgelegt werden, um Gestenkonflikte zwischen `CarouselPage` und `MasterDetailPage` zu vermeiden.

Weitere Informationen zur [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse finden Sie in [Kapitel 25](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) im Xamarin.Forms-Buch von Charles Petzold.

## <a name="create-a-carouselpage"></a>Erstellen einer CarouselPage

Es gibt zwei Ansätze zum Erstellen einer [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse:

- [Auffüllen](#populate-a-carouselpage-with-a-page-collection) der `CarouselPage`-Klasse mit einer Collection untergeordneter [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen.
- [Zuweisen](#populate-a-carouselpage-with-a-template) einer Sammlung zur [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)-Eigenschaft sowie Zuweisen einer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)-Klasse zur [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)-Eigenschaft, um [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen für Objekte in der Sammlung zurückzugeben.

Bei beiden Ansätzen zeigt die `CarouselPage`-Klasse dann Seite um Seite an, wobei durch Wischen zur nächsten anzuzeigenden Seite gewechselt werden kann.

> [!NOTE]
> Eine [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)-Klasse kann nur mit [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Instanzen oder `ContentPage`-Ableitungen aufgefüllt werden.

### <a name="populate-a-carouselpage-with-a-page-collection"></a>Auffüllen einer CarouselPage mit einer Page-Sammlung

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

### <a name="populate-a-carouselpage-with-a-template"></a>Auffüllen einer CarouselPage mit einer Vorlage

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

## <a name="related-links"></a>Verwandte Links

- [Page Varieties (Seitenvarianten)](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)
- [CarouselPageTemplate (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
