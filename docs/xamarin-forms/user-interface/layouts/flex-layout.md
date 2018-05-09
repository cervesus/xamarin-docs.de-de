---
title: Die Xamarin.Forms-FlexLayout
description: Verwenden Sie FlexLayout für Stapeln oder eine Auflistung von untergeordneten Ansichten wrapping an.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 4aa2ea21c9cf2e9e646465ab7ad4aa0a01de433e
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="the-xamarinforms-flexlayout"></a>Die Xamarin.Forms-FlexLayout

_Verwenden Sie FlexLayout für Stapeln oder eine Auflistung von untergeordneten Ansichten wrapping an._

Der Xamarin.Forms [ `FlexLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexLayout/) Neuigkeiten gibt es in Xamarin.Forms Version 3.0. Basiert auf den CSS-Code [Flexible Box Layout Module](http://www.w3.org/TR/css-flexbox-1/), das häufig als bezeichnet _flex Layout_ oder _-Box-Flex_, also aufgerufen werden, da sie viele flexible Optionen zum Anordnen von untergeordneten Elementen enthält innerhalb des Layouts.

`FlexLayout` ähnelt der Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) , da er seine untergeordneten Elemente horizontal und vertikal in einem Stapel anordnen kann. Allerdings die `FlexLayout` kann auch seine untergeordneten Elemente umschlossen, wenn zu viele in eine einzelne Zeile oder Spalte passt, und auch verfügt über viele Optionen für die Ausrichtung, Ausrichtung und Anpassung an verschiedenen Bildschirmgrößen.

`FlexLayout` leitet sich von [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) und erbt einen [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) Eigenschaft vom Typ `IList<View>`.

`FlexLayout` definiert sechs öffentliche bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften. (Wenn Sie nicht mit angefügten bindbare Eigenschaften vertraut sind, finden Sie im Artikel  **[angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)**.) Alle diese Eigenschaften werden in den folgenden Abschnitten ausführlich beschrieben, auf **[die sechs bindbaren Eigenschaften](#bindable-properties)** und **[die fünf angefügte bindbare Eigenschaften](#attached-properties)**. Allerdings in diesem Artikel beginnt mit einem Abschnitt auf einigen **[allgemeine Anwendungen](#common-applications)** von `FlexLayout` , viele dieser Eigenschaften informell beschreibt. Gegen Ende des Artikels, sehen Sie, wie Sie kombinieren `FlexLayout` mit [CSS-Stylesheets](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-applications" />

## <a name="common-applications"></a>Allgemeine Anwendung

Die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispielprogramm enthält mehrere Seiten, einige allgemeine von Verwendungsmöglichkeiten, Demonstate `FlexLayout` und ermöglicht Ihnen das Experimentieren mit seinen Eigenschaften.

### <a name="using-flexlayout-for-a-simple-stack"></a>Verwenden für einen einfachen Stapel FlexLayout

Die **einfache Stapel** zeigt wie Seite `FlexLayout` ersetzen kann eine `StackLayout` jedoch mit einfacher Markup. Alles, was in diesem Beispiel wird in der XAML-Seite definiert. Die `FlexLayout` enthält vier untergeordneten Elemente:

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

So sieht die Seite, die unter iOS, Android und universellen Windows-Plattform aus:

[![Stack-Seite für die einfache](flex-layout-images/SimpleStack.png "einfachen Stack-Seite")](flex-layout-images/SimpleStack-Large.png#lightbox)

Drei Eigenschaften des `FlexLayout` in angezeigt werden, die **SimpleStackPage.xaml** Datei:

- Die [ `Direction` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Direction/) Eigenschaftensatz wird auf den Wert der [ `FlexDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirection/) Enumeration. Die Standardeinstellung ist `Row`. Festlegen der Eigenschaft auf `Column` bewirkt, dass die untergeordneten Elemente der `FlexLayout` in einer einzelnen Spalte Elemente angeordnet werden.

    Wenn Elemente in eine `FlexLayout` in einer Spalte angeordnet sind die `FlexLayout` hat ist eine vertikale _Hauptachse_ und einem horizontalen _cross-Achse_.

- Die [ `AlignItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignItems/) -Eigenschaft ist vom Typ [ `FlexAlignItems` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignItems/) und gibt an, wie die Elemente auf der querachse ausgerichtet sind. Die `Center` Option bewirkt, dass jedes Element horizontal zentriert werden soll.

    Bei Verwendung von wurden eine `StackLayout` anstelle eines `FlexLayout` für diese Aufgabe zentrieren Sie in diesem Fall alle Elemente durch Zuweisen der `HorizontalOptions` Eigenschaft jedes Elements um `Center`. Die `HorizontalOptions` Eigenschaft funktioniert nicht für untergeordnete Elemente des eine `FlexLayout`, aber dieser einzelnen `AlignItems` Eigenschaft wird das gleiche Ziel erreicht. Wenn Sie möchten, können Sie mithilfe der `AlignSelf` -bindbare Eigenschaft überschreiben die `AlignItems` Eigenschaft für einzelne Elemente:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Durch diese Änderung dieser `Label` befindet sich am linken Rand der `FlexLayout` Wenn ist die Lesefolge von links nach rechts.

- Die [ `JustifyContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.JustifyContent/) -Eigenschaft ist vom Typ [ `FlexJustify` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexJustify/), und gibt an, wie die Elemente auf der Hauptachse angeordnet werden. Die `SpaceEvenly` Option ordnet alle verbliebenen vertikalen Platz gleichmäßig auf alle Elemente des ersten Elements, und geben Sie unterhalb des letzten Elements.

    Bei Verwendung von wurden eine `StackLayout`, müssen Sie zum Zuweisen der `VerticalOptions` Eigenschaft jedes Elements um `CenterAndExpand` ein ähnliches Ergebnis erzielen. Aber die `CenterAndExpand` Option würde doppelt so viel Platz zwischen den einzelnen Elementen als vor dem ersten Element und nach dem letzten Element zuzuordnen. Imitieren, Sie können die `CenterAndExpand` Option `VerticalOptions` durch Festlegen der `JustifyContent` Eigenschaft `FlexLayout` auf `SpaceAround`.

Diese `FlexLayout` Eigenschaften werden ausführlicher im Abschnitt **[die sechs bindbaren Eigenschaften](#bindable-properties)** unten.

### <a name="using-flexlayout-for-wrapping-items"></a>Verwenden FlexLayout für Elemente umschließen

Die **Foto Wrapping** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel wird veranschaulicht, wie `FlexLayout` kann seinen untergeordneten Elementen zusätzliche Zeilen oder Spalten zu umschließen. Die XAML-Datei instanziiert den `FlexLayout` und weist zwei Eigenschaften:

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

Die `Direction` -Eigenschaft dieser `FlexLayout` nicht festgelegt ist, weshalb sie die Standardeinstellung von `Row`, was bedeutet, dass die untergeordneten Elemente in Zeilen angeordnet sind und Hauptachse horizontal ist.

Die [ `Wrap` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Wrap/) Eigenschaft eines Enumerationstyps ist [ `FlexWrap` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexWrap/). Wenn zu viele Elemente auf einer Zeile passt vorhanden sind, wird die Einstellung dieser Eigenschaft die Elemente auf die nächste Zeile umbrochen.

Beachten Sie, dass die `FlexLayout` ist ein untergeordnetes Element von einem `ScrollView`. Wenn es sind zu viele Zeilen auf der Seite passt die `ScrollView` verfügt über einen standardmäßigen `Orientation` Eigenschaft `Vertical` und ermöglicht einen vertikalen Bildlauf ausführen.

Die `JustifyContent` Eigenschaft weist verbleibende Speicherplatz auf der Haupt-Achse (x-Achse), damit jedes Element die gleiche Menge an Leerzeichen umgeben ist.

Code-Behind-Datei greift auf eine Auflistung von Fotos Beispiel und fügt diese der `Children` Auflistung von der `FlexLayout`:

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
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
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
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
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

So sieht das Programm ausgeführt wird, auf die drei Plattformen progressiv durch einen Bildlauf von oben nach unten aus:

[![Die Seite "Wrapping Foto"](flex-layout-images/PhotoWrapping.png "die Foto-Wrapping-Seite")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="holy-grail-layout-with-flexlayout"></a>Layout der wichtigste Aspekt mit FlexLayout

Webdesign aufgerufen besteht ein Standardlayout der [ _wichtigste Aspekt_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) , da es sich um ein Layoutformat, die sehr vorteilhaft, aber häufig schwer handelt zu bedenken, dass mit perfekt ist. Das Layout besteht aus einem Header am oberen Rand der Seite und eine Fußzeile im unteren beiden Erweitern auf die gesamte Breite der Seite. Die Mitte der Seite einnimmt ist der Hauptinhalt jedoch häufig mit einem spaltenförmigen Menü auf der linken Seite des Inhalts und zusätzliche (bezeichnet ein _reserviert_ Bereich) auf der rechten Seite. [Abschnitt 5.4.1 der CSS-Flexible Box-Layout-Spezifikation](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) wird beschrieben, wie das Layout der wichtigste Aspekt durch ein Feld Flex realisiert werden kann.

Die **Layout der wichtigste Aspekt** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel zeigt eine einfache Implementierung der dieses Layout mit einem `FlexLayout` in einer anderen geschachtelt. Da auf dieser Seite für ein Telefon im Hochformat ausgelegt ist, sind die Bereiche, links und rechts des Inhaltsbereichs nur 50 Pixel breit:

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

Hier ist er auf den drei Plattformen ausgeführt:

[![Der wichtigste Aspekt Layoutseite](flex-layout-images/HolyGrailLayout.png "der wichtigste Aspekt-Layoutseite")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Die Navigation und Aside-Bereiche werden gerendert, mit einem `BoxView` auf der linken und rechten Seite.

Die erste `FlexLayout` in der XAML-Datei hat eine vertikale Hauptachse und enthält drei untergeordneten Elemente in einer Spalte angeordnet. Dies sind die Header, der Text der Seite und die Fußzeile. Die geschachtelte `FlexLayout` verfügt über ein Hauptfenster Horizontalachse mit drei untergeordneten Elemente in einer Zeile angeordnet.

An diesem Programm werden drei angefügte bindbare Eigenschaften veranschaulicht:

- Die `Order` angefügte bindbare Eigenschaft festgelegt ist auf dem ersten `BoxView`. Diese Eigenschaft ist eine ganze Zahl mit einem Standardwert von 0. Sie können diese Eigenschaft verwenden, so ändern Sie das Layout-Reihenfolge. In der Regel Entwickler bevorzugen den Inhalt der Seite im Markup vor die Navigationselemente angezeigt werden und zur Seite gedrängt Elemente. Festlegen der `Order` Eigenschaft auf der ersten `BoxView` auf einen Wert kleiner als die anderen gleichgeordneten bewirkt, dass es als erstes Element in der Zeile angezeigt. Auf ähnliche Weise können Sie sicherstellen, dass ein Element zuletzt, durch Festlegen angezeigt der `Order` -Eigenschaft auf einen größeren Wert als den nebengeordneten Elementen.

- Die `Basis` angefügte bindbare Eigenschaft wird festgelegt, auf die beiden `BoxView` Elemente geben eine Breite von 50 Pixel. Diese Eigenschaft ist vom Typ `FlexBasis`, eine Struktur, die eine statische Eigenschaft vom Typ definiert `FlexBasis` mit dem Namen `Auto`, dies ist die Standardeinstellung. Sie können `Basis` an eine Größe in Pixel oder einen Prozentwert, der gibt an, wie viel Speicherplatz belegt das Element auf der Hauptachse. Sie wird aufgerufen, eine _Basis_ , da es sich um eine Elementgröße angibt, der die Grundlage für alle nachfolgenden Layout ist.

- Die `Grow` Eigenschaftensatz wird auf der geschachtelten `Layout` und klicken Sie auf die `Label` untergeordnetes Element, das den Inhalt darstellt. Diese Eigenschaft ist vom Typ `float` und hat den Standardwert 0. Auf einen positiven Wert festgelegt, der verbleibende Speicherplatz auf der Haupt-Achse auf dieses Element und gleichgeordneten Elementen mit positiven Werten von zugeordnet ist `Grow`. Der Speicherplatz ist proportional zugewiesen, auf die Werte etwas wie die Stern-Spezifikation in einem `Grid`.

    Die erste `Grow` gesetzte angehängte Eigenschaft auf die geschachtelte `FlexLayout`gibt an, die von diesem `FlexLayout` besteht darin, alle nicht verwendeten vertikale Platz in der äußeren einnehmen `FlexLayout`. Die zweite `Grow` angefügten Eigenschaft festgelegt ist, auf die `Label` , das den Inhalt, der angibt, dass dieser Inhalt ist, um alle nicht verwendeten horizontalen Platz in der inneren einnehmen darstellt `FlexLayout`.

    Es gibt auch ein ähnliches `Shrink` -bindbare Eigenschaft, die Sie verwenden können, wenn die Größe der untergeordneten Elemente die Größe der überschreitet die `FlexLayout` jedoch Wrapping nicht erwünscht ist.

### <a name="catalog-items-with-flexlayout"></a>Katalogelemente mit FlexLayout

Die **Katalogelemente** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel ähnelt [Example 1 in Abschnitt 1.1 der CSS-Layout im Feld Flex-Spezifikation](http://www.w3.org/TR/css-flexbox-1/#overview)mit dem Unterschied, dass eine horizontal bildlauffähigem Reihe von Bildern und Beschreibungen der drei Affen angezeigt:

[![Der Katalog Elemente](flex-layout-images/CatalogItems.png "des Katalogs Elemente")](flex-layout-images/CatalogItems-Large.png#lightbox)

Jede der drei Affen ist ein `FlexLayout` in enthaltenen eine `Frame` , erhält eine explizite Höhe und Breite und die ist auch ein untergeordnetes Element von einem größeren `FlexLayout`. In diesem XAML-Datei, die meisten Eigenschaften von der `FlexLayout` untergeordneten Elemente werden in Formaten, alle bis auf eins davon eine implizite Format ist angegeben:

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

Die implizite Stil für die `Image` beinhaltet Einstellungen von zwei angefügte bindbare Eigenschaften des `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Die `Order` Einstellung des &ndash;1 Ursachen der `Image` Element in jedem geschachtelten zuerst angezeigt werden `FlexLayout` Ansichten unabhängig von seiner Position innerhalb der Auflistung der untergeordneten Elemente. Die `AlignSelf` Eigenschaft `Center` bewirkt, dass die `Image` innerhalb zentriert werden soll die `FlexLayout`. Dies überschreibt die Einstellung der der `AlignItems` -Eigenschaft, die einen Standardwert besitzt der `Stretch`. Dies bedeutet, die die `Label` und `Button` untergeordnete Elemente werden gestreckt, um die gesamte Breite des der `FlexLayout`.

Innerhalb jeder der drei `FlexLayout` anzeigt, eine leere `Label` vorausgeht der `Button`, verfügt aber über eine `Grow` 1 festlegen. Dies bedeutet, dass alle zusätzlichen vertikalen Spae, werden diesem leer gelassen zugeordnet ist `Label`, die effektiv legt die `Button` unten.

<a name="bindable-properties" />

## <a name="the-six-bindable-properties"></a>Die sechs bindbaren Eigenschaften

Nun, dass Sie einige häufige Anwendungsbereiche gesehen haben `FlexLayout`, die Eigenschaften des `FlexLayout` genauer untersucht werden können. 
`FlexLayout` definiert sechs bindbare Eigenschaften, die Sie festlegen, auf die `FlexLayout` selbst, entweder im Code oder in XAML, aber die `Position` Eigenschaft wird in diesem Artikel nicht behandelt.

Sie experimentieren können mit dem fünf verbleibenden bindbare Eigenschaften, die mithilfe der **experimentieren** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel. Auf dieser Seite können Sie zum Hinzufügen oder entfernen die untergeordneten Elemente von einem `FlexLayout` und Kombinationen von den fünf bindbaren Eigenschaften festlegen. Die untergeordneten Elemente des der `FlexLayout` sind `Label` Ansichten verschiedener Farben und Größen, mit der `Text` -Eigenschaft auf eine Zahl entspricht, seine Position in der `Children` Auflistung.

Wenn das Programm gestartet wird, fünf `Picker` Ansichten anzeigen, die Standardwerte für diese fünf `FlexLayout` Eigenschaften. Die `FlexLayout` im unteren Rand des Bildschirms enthält drei untergeordneten Elemente:

[![Die Seite Experiment: Standard](flex-layout-images/ExperimentDefault.png "Seite Experiment - Standard")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Jede der `Label` Ansichten besitzt einen grauen Hintergrund, die den Speicherplatz an, die belegt zeigt `Label` innerhalb der `FlexLayout`. Der Hintergrund der `FlexLayout` selbst ist Alice Blau. Mit Ausnahme einer little Rand auf der linken und rechten nimmt dieses den gesamten unteren Bereich der Seite.

<a name="direction" />

### <a name="the-direction-property"></a>Die Direction-Eigenschaft

Die [ `Direction` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Direction/) -Eigenschaft ist vom Typ [ `FlexDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirection/), eine Enumeration mit vier Mitglieder:

- `Column`
- `ColumnReverse` (oder "Spalte-Reverse" in XAML)
- `Row`, der Standardwert
- `RowReverse` (oder "Zeile umkehren" in XAML)

In XAML werden können, geben Sie den Wert dieser Eigenschaft mit dem Enumerationsmembernamen in Kleinbuchstaben, Großbuchstaben, oder gemischter Groß-/Kleinschreibung, oder Sie können zwei zusätzliche Zeichenfolgen, die in Klammern, die identisch mit der CSS-Indikatoren werden angezeigt. (Die Zeichenfolgen "Spalte umkehren" und "Zeile umkehren" werden definiert, der [ `FlexDirectionTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexDirectionTypeConverter/) Klasse, die von der Verwendung von XAML-Parser verwendet.)

Hier wird die **Experiment** Seite "(von links nach rechts), zeigt der `Row` Richtung `Column` Richtung und `ColumnReverse` Richtung:

[![Die Seite Experiment: Richtung](flex-layout-images/ExperimentDirection.png "der Experiment-Seite - Richtung")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Beachten Sie, dass für die `Reverse` Startoptionen, die Elemente am rechten oder unteren.

<a name="wrap" />

### <a name="the-wrap-property"></a>Der Wrap-Eigenschaft

Die [ `Wrap` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Wrap/) -Eigenschaft ist vom Typ [ `FlexWrap` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexWrap/), eine Enumeration mit drei Membern:

- `NoWrap`, der Standardwert
- `Wrap`
- `Reverse` (oder "Wrap-Reverse" in XAML)

Von links nach rechts, diese Bildschirme zeigen die `NoWrap`, `Wrap` und `Reverse` Optionen für 12 untergeordnete Elemente:

[![Das Experiment-Seite: Umschließen](flex-layout-images/ExperimentWrap.png "Seite Experiment - umschließen")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Wenn die `Wrap` -Eigenschaftensatz auf `NoWrap` Hauptachse (wie in diesem Programm) eingeschränkt ist und die Hauptachse ist nicht breit oder hoch genug ist, an alle untergeordneten Elemente der `FlexLayout` Wiederholungsversuche an, die Elemente, die kleiner als der iOS-Screenshot veranschaulicht. Sie können steuern, die Shrinkness der Elemente mit dem [ `Shrink` ](#shrink) angefügte bindbare Eigenschaft.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Die JustifyContent-Eigenschaft

Die [ `JustifyContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.JustifyContent/) -Eigenschaft ist vom Typ [ `FlexJustify` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexJustify/), eine Enumeration mit sechs Elemente:

- `Start` (oder "Flex-Start" in XAML), Standard
- `Center`
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Speicherplatz around" in XAML)
- `SpaceEvenly`

Diese Eigenschaft gibt an, wie die Elemente auf der Hauptachse verteilt sind, die die horizontale Achse in diesem Beispiel ist:

[![Der Seite "Experiment": Rechtfertigen Inhalt](flex-layout-images/ExperimentJustifyContent.png "Seite Experiment - Justify Inhalt")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

In allen drei Screenshots der `Wrap` -Eigenschaftensatz auf `Wrap`. Die `Start` Standardwert wird in der vorherigen Android Screenshot dargestellt. Hier der iOS-Screenshot zeigt die `Center` Option: alle Elemente werden in der Mitte verschoben. Drei andere Optionen ab, mit dem Wort `Space` den zusätzlichen Speicherplatz belegt wird nicht von den Elementen zuzuordnen. `SpaceBetween` Ordnet den Speicherplatz gleichmäßig zwischen den Elementen; `SpaceAround` Puts gleich Abstands um jedes Element während `SpaceEvenly` Puts gleich Leerzeichen zwischen den einzelnen Elementen und vor dem ersten Element und nach dem letzten Element in der Zeile.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Die AlignItems-Eigenschaft

Die [ `AlignItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignItems/) -Eigenschaft ist vom Typ [ `FlexAlignItems` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignItems/), eine Enumeration mit vier Mitglieder:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Dies ist eine von zwei Eigenschaften (das andere ist [ `AlignContent` ](#align-content)), der angibt, wie die untergeordneten Elemente auf der querachse ausgerichtet sind. Innerhalb jeder Zeile werden die untergeordneten Elemente gestreckt (wie im vorhergehenden Screenshot gezeigt) oder auf die Start-, Center, oder der jedes Element ausgerichtet werden, wie in den folgenden drei Screenshots dargestellt:

[![Die Seite Experiment: Richten Sie Elemente](flex-layout-images/ExperimentAlignItems.png "richten Sie das Experiment-Seite - Elemente")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

In der iOS-Screenshot sind die oberen Bereiche aller untergeordneten Elemente ausgerichtet. Die Elemente werden in der Android Screenshots vertikal zentriert basierend auf der höchsten untergeordneten. Im Screenshot UWP werden alle Elemente der unten ausgerichtet.

Für jedes einzelne Element der `AlignItems` Einstellung kann überschrieben werden, mit der [ `AlignSelf` ](#align-self) -bindbare Eigenschaft.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Die AlignContent-Eigenschaft

Die [ `AlignContent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignContent/) -Eigenschaft ist vom Typ [ `FlexAlignContent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignContent/), eine Enumeration mit sieben Elementen:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Speicherplatz around" in XAML)
- `SpaceEvenly`

Wie `AlignItems`die `AlignContent` Eigenschaft auch untergeordnete Elemente auf der querachse ausgerichtet, sondern wirkt sich auf ganze Zeilen oder Spalten:

[![Die Seite Experiment: Ausrichten von Inhalt](flex-layout-images/ExperimentAlignContent.png "Seite Experiment - Ausrichten von Inhalt")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

Beide Zeilen sind in der iOS-Screnshot oben; in der Android-Screenshot sind sie in der Mitte; und im Screenshot universelle Windows-Plattform, die sie am unteren sind. Die Zeilen können auch auf verschiedene Weise verteilt werden:

[![Die Seite Experiment: Ausrichten Content 2](flex-layout-images/ExperimentAlignContent2.png "richten Sie das Experiment-Seite - Content 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

Die `AlignContent` hat keine Auswirkungen, wenn nur eine Zeile oder Spalte vorhanden ist.

<a name="attached-properties" />

## <a name="the-five-attached-bindable-properties"></a>Die fünf angefügte bindbare Eigenschaften

`FlexLayout` definiert fünf angefügte bindbare Eigenschaften an. Diese Eigenschaften werden festgelegt, auf die untergeordneten Elemente der `FlexLayout` und beziehen sich nur auf diese bestimmten untergeordneten.

<a name="align-self" />

### <a name="the-alignself-property"></a>Die AlignSelf-Eigenschaft

Die [ `AlignSelf` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.AlignSelf/) angefügte bindbare Eigenschaft ist vom Typ [ `FlexAlignSelf` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlexAlignContent/), eine Enumeration mit fünf Elementen:

- `Auto`, der Standardwert
- `Stretch`
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Für ein einzelnes untergeordnetes Element der `FlexLayout`, diese Eigenschaft festlegen von Außerkraftsetzungen die [ `AlignItems` ](#align-items) Eigenschaftensatz auf die `FlexLayout` selbst. Die Standardeinstellung von `Auto` bedeutet, dass verwendet die `AlignItems` Einstellung.

Für eine `Label` Element mit dem Namen `label` (oder Beispiel), können Sie festlegen, die `AlignSelf` -Eigenschaft im Code wie folgt:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

Beachten Sie, dass es ist kein Verweis auf die `FlexLayout` übergeordnet der `Label`. In XAML legen Sie die Eigenschaft wie folgt:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Der Order-Eigenschaft

Die [ `Order` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Order/) -Eigenschaft ist vom Typ `int`. Der Standardwert ist 0.

Die `Order` -Eigenschaft können Sie die Reihenfolge ändern, die die untergeordneten Elemente der `FlexLayout` angeordnet sind. In der Regel die untergeordneten Elemente des eine `FlexLayout` angeordnet sind, entspricht der Reihenfolge, die sie in angezeigt werden die `Children` Auflistung. Sie können diese Reihenfolge außer Kraft setzen, durch Festlegen der `Order` bindbare Eigenschaft auf einen Wert ungleich NULL ganze Zahl auf eine oder mehrere untergeordnete Elemente angefügt. Die `FlexLayout` ordnet dann seinen untergeordneten Elementen basierend auf der Einstellung von der `Order` -Eigenschaft für jede untergeordnete jedoch untergeordnete Elemente mit dem gleichen `Order` Einstellung in der in angezeigten Reihenfolge angeordnet sind die `Children` Auflistung.

### <a name="the-basis-property"></a>Die Basis-Eigenschaft

Die [ `Basis` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Basis/) angefügte bindbare Eigenschaft gibt die Menge des Speicherplatzes, der ein untergeordnetes Element des zugeordnet ist die `FlexLayout` auf der Hauptachse. Die angegebene Größe durch die `Basis` Eigenschaft ist die Größe der wichtigsten Achse des übergeordneten Elements `FlexLayout`. Das heißt, `Basis` gibt die Breite eines untergeordneten Elements an, wenn die untergeordneten Elemente in Zeilen oder die Höhe angeordnet werden, wenn die untergeordneten Elemente in den Spalten angeordnet sind.

Die `Basis` -Eigenschaft ist vom Typ [ `FlexBasis` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexBasis/), eine Struktur. Die Größe kann in geräteunabhängigen Einheiten oder als Prozentsatz der Größe der angegeben werden, wenn die `FlexLayout`. Der Standardwert der `Basis` Eigenschaft ist die statische Eigenschaft `FlexBasis.Auto`, was bedeutet, dass das untergeordnete Element angefordert der Breite oder Höhe wird verwendet.

Sie können im Code Festlegen der `Basis` -Eigenschaft für eine `Label` mit dem Namen `label` 40 geräteunabhängigen Einheiten wie folgt:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Das zweite Argument der `FlexBasis` Konstruktor heißt `isRelative` und gibt an, ob die Größe des relativen ist (`true`) oder absolut (`false`). Das Argument hat den Standardwert `false`, sodass Sie auch den folgenden Code verwenden können:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Eine implizite Konvertierung von `float` zu `FlexBasis` ist definiert, sodass er noch weiter zu vereinfachen:

```csharp
FlexLayout.SetBasis(label, 40);
```

Sie können die Größe festlegen, um 25 % des der `FlexLayout` übergeordneten wie folgt:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Diese Dezimalstellenwert muss im Bereich von 0 bis 1.

In XAML können Sie eine Zahl für eine Größe in geräteunabhängigen Einheiten:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Oder Sie können einen Prozentsatz angeben, in dem Bereich von 0 bis 100 %:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

Die **Basis experimentieren** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel können Sie zum Experimentieren mit der `Basis` Eigenschaft. Die Seite zeigt eine eingeschlossene Spalte von fünf `Label` mit Hintergrund-und Vordergrundfarben abwechselnde Elemente. Zwei `Slider` Elemente können Sie angeben, `Basis` Werte für den zweiten und vierten `Label`:

[![Die Basis experimentieren Seite](flex-layout-images/BasisExperiment.png "Basis experimentieren, Seite")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Der iOS-Screenshot auf der linken Seite zeigt die beiden `Label` Elemente wird Höhe in geräteunabhängigen Einheiten angegeben. Die Android Abbildung sehen sie die Höhe, die einem Bruchteil der gesamte Höhe angegeben wird die `FlexLayout`. Wenn die `Basis` bei 100 % festgelegt wird, dann wird das untergeordnete Element die Höhe des ist die `FlexLayout`, und in der nächsten Spalte umbrochen und belegen die gesamte Höhe der Spalte, wie der uwp-Screenshot veranschaulicht wird: er angezeigt wird, als ob die fünf untergeordnete Elemente in einer Zeile angeordnet sind , jedoch sind tatsächlich in fünf Spalten angeordnet.

### <a name="the-grow-property"></a>Die Vergrößerung der Eigenschaft

Die [ `Grow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Grow/) -Eigenschaft ist vom Typ `int`. Der Standardwert ist 0, und der Wert muss größer als oder gleich 0 sein.

Die `Grow` -Eigenschaft spielt eine Rolle bei der `Wrap` -Eigenschaftensatz auf `NoWrap` und die Zeile der untergeordneten Elemente verfügt über eine gesamte Breite kleiner als die Breite des der `FlexLayout`, oder die Spalte der untergeordneten Elemente hat eine kürzere Höhe als die `FlexLayout`. Die `Grow` Eigenschaft gibt an, wie den verbleibende Speicherplatz für die untergeordneten Elemente aufteilen.

In der **Vergrößerung Experiment** Seite fünf `Label` Elemente abwechselnde Farben sind in einer Spalte und zwei angeordnet `Slider` Elemente können Sie Anpassen der `Grow` Eigenschaft des zweiten und vierten `Label`. Der iOS-Screenshot ganz links zeigt die standardmäßige `Grow` Eigenschaften 0:

[![Die Seite "Experiment Grow"](flex-layout-images/GrowExperiment.png "der Seite "Experiment vergrößern"")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Wenn alle ein untergeordnetes Element eine Positive gewährt wird `Grow` Wert sind, werden diese untergeordneten den verbleibenden Platz einnimmt, wie Android Screenshot veranschaulicht. Dieser Speicherplatz kann auch zwischen zwei oder mehr untergeordnete Elemente zugeordnet werden. Im uwp-Screenshot der `Grow` Eigenschaft des zweiten `Label` auf 0,5 festgelegt ist während der `Grow` Eigenschaft des vierten `Label` 1,5, wodurch das vierte erhalten `Label` dreimal so viel von der verbleibende Speicherplatz als der zweite `Label`.

Wie die untergeordnete Ansicht diesen Speicherplatz verwendet, hängt von den betreffenden Typ des untergeordneten Elements ab. Für eine `Label`, kann der Text innerhalb der Gesamtspeicherplatz der positioniert werden die `Label` mithilfe der Eigenschaften `HorizontalTextAlignment` und `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Die Shrink-Eigenschaft

Die [ `Shrink` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FlexLayout.Shrink/) -Eigenschaft ist vom Typ `int`. Der Standardwert ist 1, und der Wert muss größer als oder gleich 0 sein.

Die `Shrink` Eigenschaft spielt eine Rolle bei der `Wrap` -Eigenschaftensatz auf `NoWrap` und die aggregate Breite einer Zeile der untergeordneten Elemente ist größer als die Breite des der `FlexLayout`, oder der Höhe von einer einzelnen Spalte der untergeordneten Elemente ist größer als der Höhe der `FlexLayout`. Normalerweise die `FlexLayout` werden diese untergeordneten Elemente anzeigen, indem Sie ihre Größe constricting. Die `Shrink` Eigenschaft kann angeben, welche untergeordneten Elemente "Priorität" in der angezeigt wird, auf ihre vollständige Größe zugewiesen werden.

Die **verkleinern Experiment** -Seite erstellt eine `FlexLayout` mit einer einzelnen Zeile von fünf `Label` untergeordnete Elemente, die mehr Speicherplatz als erforderlich die `FlexLayout` Breite. Der iOS-Screenshot auf der linken Seite zeigt alle der `Label` Elemente mit dem Standardwert 1:

[![Die verkleinern experimentieren Seite](flex-layout-images/ShrinkExperiment.png "die verkleinern experimentieren, Seite")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

In der Android-Screenshot der `Shrink` Wert für die zweite `Label` auf festgelegt ist, 0, und die `Label` wird in der Breite angezeigt. Darüber hinaus die vierte `Label` erhält eine `Shrink` -Wert größer als 1, und es verkleinert wurde. Der universelle Windows-Plattform-Screenshot zeigt sowohl `Label` Elemente angegeben wird eine `Shrink` Wert von 0 bis in ihre vollständige Größe angezeigt werden Wenn für das zulassen kann.

Legen Sie sowohl die `Grow` und `Shrink` um Situationen zu berücksichtigen, wo die aggregierte untergeordnete Größen kleiner als oder manchmal größer als die Größe der eventuell, Werte der `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>CSS-Stilen mit FlexLayout

Sie können die [CSS-Stilen](~/xamarin-forms/user-interface/styles/css/index.md) Xamarin.Forms 3.0 in Verbindung mit eingeführtes Feature `FlexLayout`. Die **CSS Katalogelemente** auf der Seite der **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel enthält das Layout der der **Katalogelemente** Seite, jedoch mit einer CSS Stylesheet für viele der Formatvorlagen:

[![Die CSS-Katalog Seite "Elemente"](flex-layout-images/CssCatalogItems.png "CSS Katalog Elemente (Seite)")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Die ursprüngliche **CatalogItemsPage.xaml** Datei verfügt über fünf `Style` Definitionen in seiner `Resources` Abschnitt mit 15 `Setter` Objekte. In der **CssCatalogItemsPage.xaml** -Datei, die auf zwei reduziert wurde `Style` Definitionen mit nur vier `Setter` Objekte. Diese Stile ergänzen die CSS-Stylesheet für Eigenschaften, die die Xamarin.Forms CSS-Stilen-Funktion derzeit verarbeitet werden kann:

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

Die CSS-Stylesheet verwiesen wird, in der ersten Zeile von der `Resources` Abschnitt:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Beachten Sie außerdem, dass zwei Elemente in jedem der drei Elemente umfassen `StyleClass` Einstellungen:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Diese beziehen sich auf Selektoren in der **CatalogItemsStyles.css** Stylesheet:

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

Mehrere `FlexLayout` angefügte bindbare Eigenschaften hier verwiesen wird. In der `label.empty` Selektor, sehen Sie die `flex-grow` -Attribut, das eine leere Stile `Label` eine leere Stelle, die oben genannten Bereitstellen der `Button`. Die `image` Selektor enthält ein `order` Attribut und einem `align-self` -Attribut, beide entsprechen `FlexLayout` bindbare Eigenschaften zugeordnet.

Haben Sie gesehen, dass Sie Eigenschaften festlegen können, direkt auf die `FlexLayout` und Sie können angeschlossene bindbare Eigenschaften festlegen, auf die untergeordneten Elemente einer `FlexLayout`. Oder Sie können diese Eigenschaften, die indirekt mit herkömmlichen XAML-basierte oder CSS-Stile festlegen. Wichtig ist, die mit bekannten und verstehen, diese Eigenschaften. Sie sind macht die `FlexLayout` tatsächlich flexibel. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout mit Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 Flex Layout von [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte links

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
