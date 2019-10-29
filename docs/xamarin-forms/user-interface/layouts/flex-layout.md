---
title: Xamarin. Forms-flexlayout
description: Verwenden Sie das flexlayout zum Stapeln oder Umpacken einer Auflistung untergeordneter Ansichten.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 75249966c6506bc33ea06c7cfa9c398bd7eb8045
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029481"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin. Forms-flexlayout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_Verwenden Sie das flexlayout zum Stapeln oder Umpacken einer Auflistung untergeordneter Ansichten._

Die xamarin. Forms- [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ist neu in xamarin. Forms Version 3,0. Es basiert auf dem flexiblen CSS- [Box-Layoutmodul](https://www.w3.org/TR/css-flexbox-1/), das auch als " _flexlayout_ " oder " _Flex-Box_" bezeichnet wird, da es viele flexible Optionen zum Anordnen von untergeordneten Elementen im Layout enthält.

`FlexLayout` ähnelt der [`StackLayout`](~/xamarin-forms/user-interface/layouts/stack-layout.md) xamarin. Forms darin, dass die untergeordneten Elemente horizontal und vertikal in einem Stapel angeordnet werden können. Der `FlexLayout` kann jedoch auch seine untergeordneten Elemente umwickeln, wenn zu viele für eine einzelne Zeile oder eine einzelne Spalte vorhanden sind, und verfügt außerdem über viele Optionen für Ausrichtung, Ausrichtung und Anpassung an verschiedene Bildschirmgrößen.

`FlexLayout` von [`Layout<View>`](xref:Xamarin.Forms.Layout`1) abgeleitet und erbt eine [`Children`](xref:Xamarin.Forms.Layout`1.Children) -Eigenschaft vom Typ `IList<View>`.

`FlexLayout` definiert sechs öffentliche bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, die sich auf die Größe, die Ausrichtung und die Ausrichtung der untergeordneten Elemente auswirken. (Wenn Sie mit den angefügten bindbaren Eigenschaften nicht vertraut sind, finden Sie weitere Informationen im Artikel **[angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)** .) Diese Eigenschaften werden in den folgenden Abschnitten ausführlich beschrieben: Details zu **[den bindbaren Eigenschaften](#bindable-properties)** und **[die angefügten bindbaren Eigenschaften im](#attached-properties)** Detail. Dieser Artikel beginnt jedoch mit einem Abschnitt zu einigen **[gängigen Verwendungs Szenarios](#common-scenarios)** von `FlexLayout`, in denen viele dieser Eigenschaften informisch beschrieben werden. Am Ende des Artikels erfahren Sie, wie Sie `FlexLayout` mit [CSS-Stylesheets](~/xamarin-forms/user-interface/styles/css/index.md)kombinieren.

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Häufige Verwendungs Szenarien

Das Beispielprogramm " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " enthält mehrere Seiten, die einige gängige Verwendungsmöglichkeiten von `FlexLayout` veranschaulichen und es Ihnen ermöglichen, mit ihren Eigenschaften zu experimentieren.

### <a name="using-flexlayout-for-a-simple-stack"></a>Verwenden von flexlayout für einen einfachen Stapel

Die Seite "Simple Stack" zeigt, wie `FlexLayout` ein `StackLayout`, aber mit **einfachem** Markup ersetzen kann. Alles in diesem Beispiel wird auf der XAML-Seite definiert. Die `FlexLayout` enthält vier untergeordnete Elemente:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

Dies ist die Seite, die unter IOS, Android und der universelle Windows-Plattform ausgeführt wird:

[![Die einfache Stapel Seite](flex-layout-images/SimpleStack.png "Die einfache Stapel Seite")](flex-layout-images/SimpleStack-Large.png#lightbox)

In der **simplestackpage. XAML** -Datei werden drei Eigenschaften `FlexLayout` angezeigt:

- Die [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) -Eigenschaft wird auf einen Wert der [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) -Enumeration festgelegt. Der Standardwert ist `Row`. Das Festlegen der-Eigenschaft auf `Column` bewirkt, dass die untergeordneten Elemente des `FlexLayout` in einer einzelnen Spalte von Elementen angeordnet werden.

    Wenn Elemente in einer `FlexLayout` in einer Spalte angeordnet sind, wird die `FlexLayout` als vertikale _Hauptachse_ und horizontale _Kreuz Achse_bezeichnet.

- Die [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) -Eigenschaft ist vom Typ [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) und gibt an, wie Elemente an der Kreuz Achse ausgerichtet werden. Die Option `Center` bewirkt, dass jedes Element horizontal zentriert wird.

    Wenn Sie einen `StackLayout` anstelle eines `FlexLayout` für diese Aufgabe verwendet haben, würden Sie alle Elemente zentrieren, indem Sie die Eigenschaft `HorizontalOptions` der einzelnen Elemente `Center` zuweisen. Die `HorizontalOptions`-Eigenschaft funktioniert nicht für untergeordnete Elemente eines `FlexLayout`, aber die einzelnen `AlignItems`-Eigenschaften erreichen dasselbe Ziel. Wenn erforderlich, können Sie die Eigenschaft `AlignSelf` angehängter Bindung verwenden, um die `AlignItems`-Eigenschaft für einzelne Elemente zu überschreiben:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Bei dieser Änderung wird diese `Label` am linken Rand des `FlexLayout` positioniert, wenn die Lesefolge von links nach rechts ist.

- Die [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) -Eigenschaft ist vom Typ [`FlexJustify`](xref:Xamarin.Forms.FlexJustify)und gibt an, wie Elemente auf der Hauptachse angeordnet werden. Mit der Option `SpaceEvenly` werden alle verbliebenen vertikalen Leerzeichen gleichmäßig zwischen allen Elementen und oberhalb des ersten Elements und unterhalb des letzten Elements zugeordnet.

    Wenn Sie einen `StackLayout` verwendet haben, müssen Sie die `VerticalOptions`-Eigenschaft der einzelnen Elemente `CenterAndExpand` zuweisen, um einen ähnlichen Effekt zu erzielen. Die Option `CenterAndExpand` würde jedoch zweimal so viel Platz zwischen jedem Element als vor dem ersten Element und nach dem letzten Element zuordnen. Sie können die `CenterAndExpand`-Option von `VerticalOptions` imitieren, indem Sie die Eigenschaft `JustifyContent` von `FlexLayout` auf `SpaceAround` festlegen.

Diese `FlexLayout` Eigenschaften werden im folgenden Abschnitt im Abschnitt " **[bindbare Eigenschaften im Detail](#bindable-properties)** " ausführlich erläutert.

### <a name="using-flexlayout-for-wrapping-items"></a>Verwenden von flexlayout zum Umwickeln von Elementen

Die Seite **Photo Wrapping** des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " veranschaulicht, wie `FlexLayout` die untergeordneten Elemente in zusätzliche Zeilen oder Spalten umschließen können. Die XAML-Datei instanziiert die `FlexLayout` und weist ihr zwei Eigenschaften zu:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

Die `Direction`-Eigenschaft dieser `FlexLayout` ist nicht festgelegt, sodass Sie die Standardeinstellung `Row` hat, was bedeutet, dass die untergeordneten Elemente in Zeilen angeordnet sind und die Hauptachse horizontal ist.

Die [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) -Eigenschaft ist ein Enumerationstyp [`FlexWrap`](xref:Xamarin.Forms.FlexWrap). Wenn zu viele Elemente vorhanden sind, die in eine Zeile passen, bewirkt diese Eigenschafts Einstellung, dass die Elemente in die nächste Zeile eingebunden werden.

Beachten Sie, dass der `FlexLayout` ein untergeordnetes Element eines `ScrollView` ist. Wenn zu viele Zeilen vorhanden sind, die auf die Seite passen, verfügt die `ScrollView` über die Standard `Orientation` Eigenschaft `Vertical` und ermöglicht das vertikale Scrollen.

Die `JustifyContent`-Eigenschaft ordnet verbleibenden Speicherplatz auf der Hauptachse (der horizontalen Achse) zu, sodass jedes Element von derselben Menge an Leerraum umgeben ist.

Die Code-Behind-Datei greift auf eine Auflistung von Beispiel Fotos zu und fügt Sie der `Children`-Auflistung der `FlexLayout` hinzu:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

Hier ist das Programm, das ausgeführt wird, und progressiv von oben nach unten:

[![Die Seite "Foto Wrapping"](flex-layout-images/PhotoWrapping.png "Die Seite "Foto Wrapping"")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Seitenlayout mit flexlayout

Im Webdesign gibt es ein Standardlayout, das als [_Heiliger Gral_](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) bezeichnet wird, da es sich um ein Layout-Format handelt, das sehr wünschenswert ist, aber häufig mit der Perfektion schwer zu erkennen ist. Das Layout besteht aus einem Header am oberen Rand der Seite und einer Fußzeile im unteren Bereich, die beide auf die vollständige Breite der Seite ausdehnen. Der Mittelpunkt der Seite ist der Hauptinhalt, häufig aber mit einem Spalten förmigen Menü links neben dem Inhalt und ergänzenden Informationen (manchmal auch als _neben_ Bereich bezeichnet) auf der rechten Seite. [Abschnitt 5.4.1 der flexiblen CSS-boxlayoutspezifikation](https://www.w3.org/TR/css-flexbox-1/#order-accessibility) beschreibt, wie das Heilige Gral-Layout mit einem flexfeld realisiert werden kann.

Die Seite " **Heiliges Grail-Layout** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " zeigt eine einfache Implementierung dieses Layouts mithilfe eines `FlexLayout` das in einem anderen-Objekt eingebettet ist. Da diese Seite für ein Telefon im Hochformat konzipiert ist, sind die Bereiche Links und rechts vom Inhalts Bereich nur 50 Pixel breit:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

Hier wird ausgeführt:

[![Die Seite "Heiliges Grail-Layout"](flex-layout-images/HolyGrailLayout.png "Die Seite "Heiliges Grail-Layout"")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Die Navigations-und Nebenbereiche werden mit einer `BoxView` auf der linken und rechten Seite gerendert.

Der erste `FlexLayout` in der XAML-Datei verfügt über eine vertikale Hauptachse und enthält drei untergeordnete Elemente, die in einer Spalte angeordnet sind. Dabei handelt es sich um den Header, den Text der Seite und die Fußzeile. Der `FlexLayout` der Schraffur verfügt über eine horizontale Hauptachse mit drei untergeordneten Elementen in einer Zeile.

In diesem Programm werden drei angefügte bindbare Eigenschaften veranschaulicht:

- Die `Order` angefügte bindbare Eigenschaft wird für den ersten `BoxView` festgelegt. Diese Eigenschaft ist eine ganze Zahl mit dem Standardwert 0. Sie können diese Eigenschaft verwenden, um die layoutreihenfolge zu ändern. Im allgemeinen bevorzugen Entwickler, dass der Inhalt der Seite im Markup vor den Navigationselementen und anderen Elementen angezeigt wird. Wenn Sie die `Order`-Eigenschaft auf dem ersten `BoxView` auf einen Wert festlegen, der kleiner als die anderen gleich geordneten Elemente ist, wird er als erstes Element in der Zeile angezeigt. Entsprechend können Sie sicherstellen, dass ein Element zuletzt angezeigt wird, indem Sie die `Order`-Eigenschaft auf einen Wert festlegen, der größer als die zugehörigen gleich geordneten Elemente

- Die `Basis` angefügte bindbare-Eigenschaft wird für die beiden `BoxView` Elemente festgelegt, um Ihnen eine Breite von 50 Pixeln zu übergeben. Diese Eigenschaft hat den Typ `FlexBasis`, eine-Struktur, die eine statische Eigenschaft des Typs `FlexBasis` mit dem Namen `Auto` definiert. Dies ist die Standardeinstellung. Mit `Basis` können Sie eine Pixelgröße oder einen Prozentsatz angeben, der angibt, wie viel Speicherplatz das Element auf der Hauptachse einnimmt. Sie wird als _Basis_ bezeichnet, da Sie eine Elementgröße angibt, die die Basis für alle nachfolgenden Layouts ist.

- Die `Grow`-Eigenschaft wird für die `Layout` und für das untergeordnete `Label`, das den Inhalt darstellt, festgelegt. Diese Eigenschaft hat den Typ `float` und hat den Standardwert 0. Wenn ein positiver Wert festgelegt wird, wird der gesamte verbleibende Platz entlang der Hauptachse diesem Element und gleich geordneten Elementen mit positiven Werten `Grow` zugeordnet. Der Speicherplatz wird proportional zu den Werten zugewiesen, ähnlich wie die Stern Angabe in einem `Grid`.

    Der erste `Grow` angefügte-Eigenschaft wird für den geschachtelten `FlexLayout` festgelegt, der angibt, dass dieser `FlexLayout` den gesamten nicht verwendeten vertikalen Leerraum innerhalb der äußeren `FlexLayout` belegen soll. Die zweite `Grow` angefügte Eigenschaft wird für die `Label` festgelegt, die den Inhalt darstellt, und gibt an, dass dieser Inhalt den gesamten nicht verwendeten horizontalen Raum innerhalb der inneren `FlexLayout` belegen soll.

    Es gibt auch eine ähnliche `Shrink` angefügte bindbare Eigenschaft, die Sie verwenden können, wenn die Größe der untergeordneten Elemente die Größe des `FlexLayout` überschreitet, aber keine umfügung gewünscht ist.

### <a name="catalog-items-with-flexlayout"></a>Katalog Elemente mit "flexlayout"

Die Seite **Katalog Elemente** im Beispiel " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " ähnelt [Beispiel 1 in Abschnitt 1,1 der Spezifikation "CSS-flexlayoutbox](https://www.w3.org//TR/css-flexbox-1/#overview) " mit der Ausnahme, dass Sie eine horizontal scrollbare Reihe von Bildern und Beschreibungen von drei Affen anzeigt. :

[![Die Seite "Katalog Elemente"](flex-layout-images/CatalogItems.png "Die Seite "Katalog Elemente"")](flex-layout-images/CatalogItems-Large.png#lightbox)

Jede der drei Affen ist eine `FlexLayout`, die in einem `Frame` enthalten ist, dem eine explizite Höhe und Breite zugewiesen ist, und der auch ein untergeordnetes Element eines größeren `FlexLayout` ist. In dieser XAML-Datei werden die meisten Eigenschaften der `FlexLayout` untergeordneten Elemente in Stilen angegeben, aber nur eines davon ist ein impliziter Stil:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Der implizite Stil für die `Image` umfasst Einstellungen von zwei angefügten bindbaren Eigenschaften `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Die `Order` Einstellung &ndash;1 bewirkt, dass das `Image`-Element in jeder der geschachtelten `FlexLayout` Ansichten angezeigt wird, unabhängig von der Position in der Children-Auflistung. Die `AlignSelf`-Eigenschaft von `Center` bewirkt, dass die `Image` innerhalb der `FlexLayout` zentriert werden. Dies überschreibt die Einstellung der `AlignItems`-Eigenschaft, die den Standardwert `Stretch` aufweist. Dies bedeutet, dass die `Label` und `Button` untergeordneten Elemente auf die vollständige Breite des `FlexLayout` gestreckt werden.

In jeder der drei `FlexLayout` Ansichten ist ein leeres `Label` vor dem `Button`, verfügt aber über die `Grow` Einstellung 1. Dies bedeutet, dass der gesamte zusätzliche vertikale Bereich diesem leeren `Label` zugeordnet wird, wodurch die `Button` effektiv auf den unteren Rand übertragen wird.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Die bindbaren Eigenschaften im Detail

Nachdem Sie nun einige gängige Anwendungen von `FlexLayout` kennengelernt haben, können Sie die Eigenschaften von `FlexLayout` im Detail untersucht werden.
`FlexLayout` definiert sechs bindbare Eigenschaften, die Sie für die `FlexLayout` selbst festlegen, entweder im Code oder in XAML, um die Ausrichtung und Ausrichtung zu steuern. (Eine dieser Eigenschaften, [`Position`](xref:Xamarin.Forms.FlexLayout.Position), wird in diesem Artikel nicht behandelt.)

Sie können mit den fünf verbleibenden bindbaren Eigenschaften experimentieren, indem Sie die Seite " **Experiment** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " verwenden. Auf dieser Seite können Sie untergeordnete Elemente einer `FlexLayout` hinzufügen oder entfernen und Kombinationen der fünf bindbaren Eigenschaften festlegen. Alle untergeordneten Elemente der `FlexLayout` sind `Label` Sichten verschiedener Farben und Größen, wobei die `Text`-Eigenschaft auf eine Zahl festgelegt ist, die ihrer Position in der `Children` Auflistung entspricht.

Wenn das Programm gestartet wird, werden in fünf `Picker` Ansichten die Standardwerte dieser fünf `FlexLayout` Eigenschaften angezeigt. Der `FlexLayout` unten auf dem Bildschirm enthält drei untergeordnete Elemente:

[![Die Experiment Seite: Standard](flex-layout-images/ExperimentDefault.png "Die Seite "Experiment"-Standard")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Jede der `Label` Ansichten verfügt über einen grauen Hintergrund, der den Speicherplatz in der `FlexLayout` anzeigt, der `Label` zugeordnet ist. Der Hintergrund des `FlexLayout` selbst ist Alice Blue. Es belegt den gesamten unteren Bereich der Seite, mit Ausnahme eines kleinen Randes auf der linken und rechten Seite.

<a name="direction" />

### <a name="the-direction-property"></a>Die Direction-Eigenschaft

Die [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) -Eigenschaft ist vom Typ [`FlexDirection`](xref:Xamarin.Forms.FlexDirection), eine Enumeration mit vier Membern:

- `Column`
- `ColumnReverse` (oder "Column-Reverse" in XAML)
- `Row`, der Standardwert
- `RowReverse` (oder "Zeilen Rückgängigmachen" in XAML)

In XAML können Sie den Wert dieser Eigenschaft mit den Namen der Enumerationsmember in Kleinbuchstaben, Großbuchstaben oder gemischter Groß-/Kleinschreibung angeben, oder Sie können zwei zusätzliche Zeichen folgen verwenden, die in Klammern angezeigt werden, die mit den CSS-Indikatoren identisch sind. (Die "Column-Reverse"-und "Row-Reverse"-Zeichen folgen werden in der [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) Klasse definiert, die vom XAML-Parser verwendet wird.)

Hier ist die **Experiment** Seite (von links nach rechts), die `Row` Richtung, `Column` Richtung und `ColumnReverse` Richtung:

[![Die Experiment Seite: Richtung](flex-layout-images/ExperimentDirection.png "Die Experiment Seitenrichtung")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Beachten Sie, dass die Elemente für die `Reverse` Optionen rechts oder unten beginnen.

<a name="wrap" />

### <a name="the-wrap-property"></a>Die Wrap-Eigenschaft

Die [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) -Eigenschaft ist vom Typ [`FlexWrap`](xref:Xamarin.Forms.FlexWrap), eine Enumeration mit drei Elementen:

- `NoWrap`, der Standardwert
- `Wrap`
- `Reverse` (oder "Wrap-Reverse" in XAML)

Von links nach rechts zeigen diese Bildschirme die Optionen `NoWrap`, `Wrap` und `Reverse` für 12 untergeordnete Elemente an:

[![Die Experiment Seite: Wrap](flex-layout-images/ExperimentWrap.png "Der Experiment Seitenumbruch")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Wenn die `Wrap`-Eigenschaft auf `NoWrap` festgelegt ist und die Hauptachse eingeschränkt ist (wie in diesem Programm) und die Hauptachse nicht breit oder hoch genug ist, um alle untergeordneten Elemente zu erfüllen, versucht der `FlexLayout`, die Elemente zu verkleinern. , wie der IOS-Screenshot veranschaulicht. Sie können die Verkleinerung der Elemente mit der [`Shrink`](#shrink) angefügten bindbare-Eigenschaft steuern.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Die Eigenschaft "justifycontent"

Die [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) -Eigenschaft ist vom Typ [`FlexJustify`](xref:Xamarin.Forms.FlexJustify), eine Enumeration mit sechs Membern:

- `Start` (oder "Flex-Start" in XAML), Standard
- `Center`
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Leerraum" in XAML)
- `SpaceEvenly`

Diese Eigenschaft gibt an, wie die Elemente auf der Hauptachse, d. h. die horizontale Achse in diesem Beispiel, in den Abstand gestellt werden:

[![Die Experiment Seite: Inhalt rechtfertigen](flex-layout-images/ExperimentJustifyContent.png "Das Experiment Seiten-rechtfertigen Inhalt")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

In allen drei Screenshots wird die `Wrap`-Eigenschaft auf `Wrap` festgelegt. Der `Start` Standard wird im vorherigen Android-Bildschirmfoto angezeigt. Der IOS-Bildschirmfoto zeigt die `Center` Option an: alle Elemente werden in den Mittelpunkt verschoben. Die drei weiteren Optionen, die mit dem Wort beginnen, `Space` den zusätzlichen, nicht von den Elementen belegten Speicherplatz zuzuordnen. `SpaceBetween` ordnet den Raum gleichmäßig zwischen den Elementen zu.  `SpaceAround` fügt den gleichen Raum um jedes Element, während `SpaceEvenly` den gleichen Raum zwischen den einzelnen Elementen und vor dem ersten Element und hinter dem letzten Element in der Zeile einfügt.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Die alignitems-Eigenschaft

Die [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) -Eigenschaft ist vom Typ [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems), eine Enumeration mit vier Membern:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Dies ist eine von zwei Eigenschaften (die andere wird [`AlignContent`](#align-content)), die angibt, wie untergeordnete Elemente auf der Kreuz Achse ausgerichtet werden. In jeder Zeile werden die untergeordneten Elemente gestreckt (wie im vorherigen Screenshot dargestellt) oder am Anfang, Mittelpunkt oder Ende der einzelnen Elemente ausgerichtet, wie in den folgenden drei Screenshots gezeigt:

[![Die Experiment Seite: Elemente ausrichten](flex-layout-images/ExperimentAlignItems.png "Das Experiment Seiten-Ausrichten von Elementen")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Im IOS-Screenshot werden die Spitzen aller untergeordneten Elemente ausgerichtet. In den Android-Screenshots werden die Elemente vertikal basierend auf dem höchsten untergeordneten Element zentriert. Im UWP-Bildschirmfoto werden die untersten Elemente aller Elemente ausgerichtet.

Für jedes einzelne Element kann die `AlignItems` Einstellung mit der [`AlignSelf`](#align-self) angefügten bindbaren Eigenschaft überschrieben werden.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Die aligncontent-Eigenschaft

Die [`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent) -Eigenschaft ist vom Typ [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent), eine Enumeration mit sieben Membern:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Leerraum" in XAML)
- `SpaceEvenly`

Wie `AlignItems` richtet die `AlignContent`-Eigenschaft auch untergeordnete Elemente auf der quer Achse aus, wirkt sich aber auf ganze Zeilen oder Spalten aus:

[![Die Experiment Seite: Inhalt ausrichten](flex-layout-images/ExperimentAlignContent.png "Der Inhalt der Inhaltsseite")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

Im IOS-Bildschirmfoto befinden sich beide Zeilen im oberen Bereich. im Android-Screenshot befinden Sie sich in der Mitte. und im UWP-Screenshot befinden Sie sich im unteren Bereich. Die Zeilen können auch auf unterschiedliche Weise verteilt werden:

[![Die Experiment Seite: Inhalt ausrichten 2](flex-layout-images/ExperimentAlignContent2.png "Die Experiment Seite-ausrichten von Inhalt 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

Der `AlignContent` hat keine Auswirkung, wenn nur eine Zeile oder Spalte vorhanden ist.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Details der angefügten bindbaren Eigenschaften

`FlexLayout` definiert fünf angefügte bindbare Eigenschaften. Diese Eigenschaften werden auf untergeordnete Elemente der `FlexLayout` festgelegt und beziehen sich nur auf das jeweilige untergeordnete Element.

<a name="align-self" />

### <a name="the-alignself-property"></a>Die alignself-Eigenschaft

Die [`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) angefügte bindbare-Eigenschaft ist vom Typ [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent), eine Enumeration mit fünf Membern:

- `Auto`, der Standardwert
- `Stretch`
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Für alle untergeordneten Elemente des `FlexLayout` überschreibt diese Eigenschafts Einstellung die [`AlignItems`](#align-items) -Eigenschaft, die auf dem `FlexLayout` selbst festgelegt ist. Die Standardeinstellung `Auto` bedeutet, dass die `AlignItems` Einstellung verwendet werden soll.

Für ein `Label` Element mit dem Namen `label` (oder Beispiel) können Sie die `AlignSelf`-Eigenschaft in Code wie dem folgenden festlegen:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Beachten Sie, dass es keinen Verweis auf das übergeordnete `FlexLayout` des `Label` gibt. In XAML legen Sie die-Eigenschaft wie folgt fest:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Die Order-Eigenschaft

Die [`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty) -Eigenschaft ist vom Typ `int`. Der Standardwert ist 0.

Mit der `Order`-Eigenschaft können Sie die Reihenfolge ändern, in der die untergeordneten Elemente des `FlexLayout` angeordnet werden. In der Regel sind die untergeordneten Elemente eines `FlexLayout` die gleiche Reihenfolge wie in der `Children` Auflistung. Sie können diese Reihenfolge außer Kraft setzen, indem Sie die `Order` angefügte bindbare Eigenschaft auf ein oder mehrere untergeordnete Elemente auf einen ganzzahligen Wert ungleich 0 festlegen. Der `FlexLayout` ordnet dann seine untergeordneten Elemente auf der Grundlage der Einstellung der Eigenschaft `Order` für jedes untergeordnete Element an, aber untergeordnete Elemente mit derselben `Order` Einstellung werden in der Reihenfolge angeordnet, in der Sie in der `Children` Auflistung angezeigt werden.

### <a name="the-basis-property"></a>Die Basis Eigenschaft

Die [`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty) angefügte bindbare Eigenschaft gibt die Menge an Speicherplatz an, die einem untergeordneten Element des `FlexLayout` auf der Hauptachse zugeordnet ist. Die von der `Basis`-Eigenschaft angegebene Größe ist die Größe entlang der Hauptachse der übergeordneten `FlexLayout`. Daher gibt `Basis` die Breite eines untergeordneten Elements an, wenn die untergeordneten Elemente in Zeilen angeordnet sind, oder die Höhe, wenn die untergeordneten Elemente in Spalten angeordnet sind.

Die `Basis`-Eigenschaft ist vom Typ [`FlexBasis`](xref:Xamarin.Forms.FlexBasis), eine-Struktur. Die Größe kann entweder in geräteunabhängigen Einheiten oder als Prozentsatz der Größe des `FlexLayout` angegeben werden. Der Standardwert der `Basis`-Eigenschaft ist die statische Eigenschaft `FlexBasis.Auto`. Dies bedeutet, dass die angeforderte Breite oder Höhe des untergeordneten Elements verwendet wird.

Im Code können Sie die `Basis`-Eigenschaft für eine `Label` mit dem Namen `label` auf 40 geräteunabhängige Einheiten wie folgt festlegen:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Das zweite Argument für den `FlexBasis`-Konstruktor wird `isRelative` benannt und gibt an, ob die Größe relativ (`true`) oder absolut (`false`) ist. Das-Argument hat den Standardwert `false`, sodass Sie auch den folgenden Code verwenden können:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Eine implizite Konvertierung von `float` in `FlexBasis` ist definiert, sodass Sie Sie noch weiter vereinfachen können:

```csharp
FlexLayout.SetBasis(label, 40);
```

Sie können die Größe auf 25% des `FlexLayout` übergeordneten Elements festlegen, wie im folgenden Beispiel:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Dieser Bruch Wert muss zwischen 0 und 1 liegen.

In XAML können Sie eine Zahl für eine Größe in geräteunabhängigen Einheiten verwenden:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Oder Sie können einen Prozentsatz im Bereich von 0% bis 100% angeben:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

Auf der Seite " **Basis Experiment** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " können Sie mit der `Basis`-Eigenschaft experimentieren. Auf der Seite wird eine umschließende Spalte mit fünf `Label` Elementen mit wechselnden Hintergrund-und Vorder Grundfarben angezeigt. Mit zwei `Slider`-Elementen können Sie `Basis` Werte für das zweite und vierte `Label` angeben:

[![Die Basis Experiment Seite](flex-layout-images/BasisExperiment.png "Die Basis Experiment Seite")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Der IOS-Screenshot auf der linken Seite zeigt die beiden `Label` Elemente in geräteunabhängigen Einheiten in Höhe von Höhen. Im Android-Bildschirm wird angezeigt, dass Ihnen Höhen zugewiesen werden, die ein Bruchteil der Gesamthöhe des `FlexLayout` sind. Wenn die `Basis` auf 100% festgelegt ist, ist das untergeordnete Element die Höhe des `FlexLayout`und wird mit der nächsten Spalte Umbruch und die gesamte Höhe dieser Spalte belegen, wie der UWP-Bildschirmfoto zeigt: Sie wird angezeigt, als ob die fünf untergeordneten Elemente in einer Zeile angeordnet sind. , aber Sie sind tatsächlich in fünf Spalten angeordnet.

### <a name="the-grow-property"></a>Die Grow-Eigenschaft

Die [`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty) angefügte bindbare Eigenschaft ist vom Typ `int`. Der Standardwert ist 0, und der Wert muss größer oder gleich 0 sein.

Die `Grow`-Eigenschaft spielt eine Rolle, wenn die `Wrap`-Eigenschaft auf `NoWrap` festgelegt ist und die Zeile der untergeordneten Elemente eine Gesamtbreite hat, die kleiner als die Breite der `FlexLayout` ist, oder die Spalte untergeordneter Elemente hat eine kürzere Höhe als die `FlexLayout`. Die `Grow`-Eigenschaft gibt an, wie der übrig gebliebene Leerraum zwischen den untergeordneten Elementen angezeigt werden soll.

Auf der Seite "wachsen" des **Experiments** werden fünf `Label` Elemente von wechselnden Farben in einer Spalte angeordnet, und mit zwei `Slider` Elementen können Sie die `Grow`-Eigenschaft des zweiten und vierten `Label` anpassen. Der IOS-Screenshot ganz links zeigt die standardmäßigen `Grow` Eigenschaften von 0:

[![Die Seite zum Vergrößern des Experiments](flex-layout-images/GrowExperiment.png "Die Seite zum Vergrößern des Experiments")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Wenn einem untergeordneten Element ein positiver `Grow` Wert zugewiesen wird, nimmt das untergeordnete Element den gesamten verbleibenden Platz in Kauf, wie im Android-Bildschirmfoto veranschaulicht. Dieser Speicherplatz kann auch unter zwei oder mehr untergeordneten Elementen zugeordnet werden. Im UWP-Bildschirmfoto wird die `Grow`-Eigenschaft des zweiten `Label` auf 0,5 festgelegt, während die `Grow`-Eigenschaft der vierten `Label` 1,5 ist. Dadurch erhält der vierte `Label` dreimal so viel wie der zweite `Label`.

Wie die untergeordnete Ansicht diesen Bereich verwendet, hängt vom jeweiligen Typ des untergeordneten Elements ab. Bei einem `Label` kann der Text im gesamten Bereich der `Label` mithilfe der Eigenschaften `HorizontalTextAlignment` und `VerticalTextAlignment` positioniert werden.

<a name="shrink" />

### <a name="the-shrink-property"></a>Die Shrink-Eigenschaft

Die [`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) angefügte bindbare Eigenschaft ist vom Typ `int`. Der Standardwert ist 1, und der Wert muss größer oder gleich 0 sein.

Die `Shrink`-Eigenschaft spielt eine Rolle, wenn die `Wrap`-Eigenschaft auf `NoWrap` festgelegt ist und die Aggregat Breite einer Zeile mit untergeordneten Elementen größer ist als die Breite der `FlexLayout`, oder die Aggregat Höhe einer einzelnen Spalte mit untergeordneten Elementen ist größer als die Höhe von. der `FlexLayout`. Normalerweise werden die `FlexLayout` diese untergeordneten Elemente anzeigen, indem Sie Ihre Größe überschreiten. Die `Shrink`-Eigenschaft kann angeben, welche untergeordneten Elemente Priorität haben, wenn Sie in ihrer vollen Größe angezeigt werden.

Die Seite " **Versuch verkleinern** " erstellt eine `FlexLayout` mit einer einzelnen Zeile mit fünf `Label` untergeordneten Elementen, die mehr Platz als die `FlexLayout` Breite benötigen. Der IOS-Screenshot auf der linken Seite zeigt alle `Label` Elemente mit den Standardwerten 1 an:

[![Die Seite "Versuch verkleinern"](flex-layout-images/ShrinkExperiment.png "Die Seite "Versuch verkleinern"")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Im Android-Screenshot wird der `Shrink` Wert für die zweite `Label` auf 0 festgelegt, und die `Label` wird in der vollständigen Breite angezeigt. Außerdem erhält der vierte `Label` einen `Shrink` Wert, der größer als 1 ist, und verkleinert. Der UWP-Bildschirm Abbildung zeigt, dass beide `Label` Elemente den `Shrink` Wert 0 erhalten, damit Sie in ihrer vollen Größe angezeigt werden können, falls dies möglich ist.

Sie können die Werte für `Grow` und `Shrink` auf Situationen festlegen, in denen die aggregierten untergeordneten Größen manchmal kleiner als oder manchmal größer als die Größe des `FlexLayout` sein können.

## <a name="css-styling-with-flexlayout"></a>CSS-Formatierung mit flexlayout

Sie können die [CSS](~/xamarin-forms/user-interface/styles/css/index.md) -Formatierungsfunktion verwenden, die mit xamarin. Forms 3,0 in Verbindung mit `FlexLayout` eingeführt wurde. Die Seite **CSS-Katalog Elemente** des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " dupliziert das Layout der Seite " **Katalog Elemente** ", aber mit einem CSS-Stylesheet für viele der Stile:

[![Die Seite CSS-Katalog Elemente](flex-layout-images/CssCatalogItems.png "Die Seite CSS-Katalog Elemente")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Die ursprüngliche **catalogitemspage. XAML** -Datei enthält fünf `Style` Definitionen im `Resources` Abschnitt mit 15 `Setter` Objekten. In der **csscatalogitemspage. XAML** -Datei, die auf zwei `Style` Definitionen mit nur vier `Setter` Objekten reduziert wurde. Diese Stile ergänzen das CSS-Stylesheet für Eigenschaften, die von der xamarin. Forms-CSS-Formatierungsfunktion derzeit nicht unterstützt werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Auf das CSS-Stylesheet wird in der ersten Zeile des `Resources` Abschnitts verwiesen:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Beachten Sie auch, dass zwei Elemente in jedem der drei Elemente `StyleClass` Einstellungen einschließen:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Diese beziehen sich auf Selektoren im **catalogitemsstyles. CSS** -Stylesheet:

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

An dieser Stelle wird auf mehrere `FlexLayout` angefügte bindbare Eigenschaften verwiesen. Im `label.empty` Selector sehen Sie das `flex-grow`-Attribut, das eine leere `Label` formatiert, um einen leeren Bereich oberhalb der `Button` bereitzustellen. Der `image` Selektor enthält ein `order` Attribut und ein `align-self` Attribut, die beide `FlexLayout` zugeordneten bindbaren Eigenschaften entsprechen.

Sie haben gesehen, dass Sie Eigenschaften direkt für die `FlexLayout` festlegen können, und Sie können angefügte bindbare Eigenschaften für die untergeordneten Elemente einer `FlexLayout` festlegen. Oder Sie können diese Eigenschaften indirekt mithilfe herkömmlicher XAML-basierter Stile oder CSS-Stile festlegen. Wichtig ist es, diese Eigenschaften zu kennen und zu verstehen. Diese Eigenschaften machen die `FlexLayout` wirklich flexibel.

## <a name="flexlayout-with-xamarinuniversity"></a>Flexlayout mit xamarin. University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin. Forms 3,0-flexlayoutvideo**

## <a name="related-links"></a>Verwandte Links

- [Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
