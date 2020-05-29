---
title: Das Xamarin.Forms flexlayout
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 031a846b7546c204d45c7437acd829d6cb49bfbb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137209"
---
# <a name="the-xamarinforms-flexlayout"></a>Das Xamarin.Forms flexlayout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_Verwenden Sie das flexlayout zum Stapeln oder Umpacken einer Auflistung untergeordneter Ansichten._

Der Xamarin.Forms [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) ist neu in Xamarin.Forms Version 3,0. Es basiert auf dem flexiblen CSS- [Box-Layoutmodul](https://www.w3.org/TR/css-flexbox-1/), das auch als " _flexlayout_ " oder " _Flex-Box_" bezeichnet wird, da es viele flexible Optionen zum Anordnen von untergeordneten Elementen im Layout enthält.

`FlexLayout`ähnelt der Xamarin.Forms [`StackLayout`](~/xamarin-forms/user-interface/layouts/stacklayout.md) in, dass Sie die untergeordneten Elemente horizontal und vertikal in einem Stapel anordnen kann. Der kann jedoch `FlexLayout` auch seine untergeordneten Elemente umwickeln, wenn zu viele für eine einzelne Zeile oder Spalte vorhanden sind, und verfügt außerdem über viele Optionen für Ausrichtung, Ausrichtung und Anpassung an verschiedene Bildschirmgrößen.

`FlexLayout`wird von abgeleitet [`Layout<View>`](xref:Xamarin.Forms.Layout`1) und erbt eine [`Children`](xref:Xamarin.Forms.Layout`1.Children) Eigenschaft vom Typ `IList<View>` .

`FlexLayout`definiert sechs öffentliche bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, die sich auf die Größe, die Ausrichtung und die Ausrichtung der untergeordneten Elemente auswirken. (Wenn Sie mit den angefügten bindbaren Eigenschaften nicht vertraut sind, finden Sie weitere Informationen im Artikel **[angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)**.) Diese Eigenschaften werden in den folgenden Abschnitten ausführlich beschrieben: Details zu **[den bindbaren Eigenschaften](#bindable-properties)** und **[die angefügten bindbaren Eigenschaften im](#attached-properties)** Detail. Dieser Artikel beginnt jedoch mit einem Abschnitt zu einigen **[häufigen Verwendungs Szenarios](#common-scenarios)** von `FlexLayout` , in denen viele dieser Eigenschaften informisch beschrieben werden. Am Ende des Artikels finden Sie Informationen zum kombinieren `FlexLayout` mit [CSS-Stylesheets](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Allgemeine Verwendungsszenarios

Das Beispielprogramm " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " enthält mehrere Seiten, die einige gängige Verwendungsmöglichkeiten von veranschaulichen `FlexLayout` und es Ihnen ermöglichen, mit ihren Eigenschaften zu experimentieren.

### <a name="using-flexlayout-for-a-simple-stack"></a>Verwenden von flexlayout für einen einfachen Stapel

Die Seite " **einfacher Stapel** " zeigt, wie `FlexLayout` ein durch einen ersetzt werden kann, `StackLayout` jedoch mit einfachem Markup. Alles in diesem Beispiel wird auf der XAML-Seite definiert. Die `FlexLayout` enthält vier untergeordnete Elemente:

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

`FlexLayout`In der **simplestackpage. XAML** -Datei werden drei Eigenschaften von angezeigt:

- Die- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) Eigenschaft wird auf einen Wert der- [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) Enumeration festgelegt. Der Standardwert lautet `Row`. Das Festlegen der-Eigenschaft auf `Column` bewirkt, dass die unter `FlexLayout` geordneten Elemente von in einer einzelnen Spalte von Elementen angeordnet werden.

    Wenn Elemente in einem `FlexLayout` in einer Spalte angeordnet sind, `FlexLayout` wird eine vertikale _Hauptachse_ und eine horizontale _Kreuz Achse_bezeichnet.

- Die [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) -Eigenschaft ist vom Typ [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) und gibt an, wie Elemente an der Kreuz Achse ausgerichtet werden. Die- `Center` Option bewirkt, dass jedes Element horizontal zentriert wird.

    Wenn Sie `StackLayout` `FlexLayout` für diese Aufgabe einen anstelle von verwendet haben, würden Sie alle Elemente zentrieren, indem Sie die- `HorizontalOptions` Eigenschaft der einzelnen Elemente in zuweisen `Center` . Die- `HorizontalOptions` Eigenschaft funktioniert nicht für untergeordnete Elemente eines `FlexLayout` , aber die einzelne- `AlignItems` Eigenschaft erreicht dasselbe Ziel. Wenn erforderlich, können Sie die `AlignSelf` angefügte bindbare Eigenschaft verwenden, um die- `AlignItems` Eigenschaft für einzelne Elemente zu überschreiben:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Bei dieser Änderung `Label` wird diese am linken Rand des positioniert, `FlexLayout` Wenn die Lesefolge von links nach rechts ist.

- Die [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) -Eigenschaft ist vom Typ [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) und gibt an, wie Elemente auf der Hauptachse angeordnet werden. Mit der-Option wird der `SpaceEvenly` gesamte restliche vertikale Leerraum gleichmäßig zwischen allen Elementen und oberhalb des ersten Elements und unterhalb des letzten Elements zugeordnet.

    Wenn Sie verwenden `StackLayout` , müssen Sie die- `VerticalOptions` Eigenschaft der einzelnen Elemente zuweisen, `CenterAndExpand` um einen ähnlichen Effekt zu erzielen. Die `CenterAndExpand` Option würde jedoch zweimal so viel Platz zwischen jedem Element als vor dem ersten Element und nach dem letzten Element zuordnen. Sie können die `CenterAndExpand` Option von imitieren `VerticalOptions` , indem Sie die- `JustifyContent` Eigenschaft von `FlexLayout` auf festlegen `SpaceAround` .

Diese `FlexLayout` Eigenschaften werden im Abschnitt " **[bindbare Eigenschaften](#bindable-properties)** " im folgenden Abschnitt ausführlicher erläutert.

### <a name="using-flexlayout-for-wrapping-items"></a>Verwenden von flexlayout zum Umwickeln von Elementen

Die Seite **Photo Wrapping** des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " veranschaulicht `FlexLayout` , wie die untergeordneten Elemente in zusätzliche Zeilen oder Spalten eingebunden werden können. Die XAML-Datei instanziiert den `FlexLayout` und weist ihm zwei Eigenschaften zu:

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

Die- `Direction` Eigenschaft dieser `FlexLayout` ist nicht festgelegt, sodass Sie die Standardeinstellung hat. dies `Row` bedeutet, dass die untergeordneten Elemente in Zeilen angeordnet sind und die Hauptachse horizontal ist.

Die- [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) Eigenschaft ist ein Enumerationstyp [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) . Wenn zu viele Elemente vorhanden sind, die in eine Zeile passen, bewirkt diese Eigenschafts Einstellung, dass die Elemente in die nächste Zeile eingebunden werden.

Beachten Sie, dass der ein untergeordnetes Element `FlexLayout` von ist `ScrollView` . Wenn zu viele Zeilen vorhanden sind, die auf die Seite passen, `ScrollView` verfügt über eine Standard `Orientation` Eigenschaft von `Vertical` und ermöglicht das vertikale Scrollen.

Die- `JustifyContent` Eigenschaft ordnet den verbliebenen Speicherplatz auf der Hauptachse (der horizontalen Achse) zu, sodass jedes Element von derselben Menge an Leerraum umgeben ist.

Die Code-Behind-Datei greift auf eine Auflistung von Beispiel Fotos zu und fügt Sie der-Auflistung `Children` von hinzu `FlexLayout` :

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

Die Seite " **Heiliges Grail-Layout** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " zeigt eine einfache Implementierung dieses Layouts mithilfe eines `FlexLayout` in einem anderen. Da diese Seite für ein Telefon im Hochformat konzipiert ist, sind die Bereiche Links und rechts vom Inhalts Bereich nur 50 Pixel breit:

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

Die Navigations-und Nebenbereiche werden mit einem `BoxView` auf der linken und rechten Seite gerendert.

Der erste `FlexLayout` in der XAML-Datei verfügt über eine vertikale Hauptachse und enthält drei untergeordnete Elemente, die in einer Spalte angeordnet sind. Dabei handelt es sich um den Header, den Text der Seite und die Fußzeile. Die Schraffur `FlexLayout` verfügt über eine horizontale Hauptachse, bei der drei untergeordnete Elemente in einer Zeile angeordnet sind.

In diesem Programm werden drei angefügte bindbare Eigenschaften veranschaulicht:

- Die `Order` angefügte bindbare Eigenschaft wird für den ersten festgelegt `BoxView` . Diese Eigenschaft ist eine ganze Zahl mit dem Standardwert 0. Sie können diese Eigenschaft verwenden, um die layoutreihenfolge zu ändern. Im allgemeinen bevorzugen Entwickler, dass der Inhalt der Seite im Markup vor den Navigationselementen und anderen Elementen angezeigt wird. `Order`Wenn Sie die-Eigenschaft für den ersten `BoxView` auf einen Wert festlegen, der kleiner als die anderen gleich geordneten Elemente ist, wird er als erstes Element in der Zeile angezeigt. Entsprechend können Sie sicherstellen, dass ein Element zuletzt angezeigt wird, indem Sie die- `Order` Eigenschaft auf einen Wert festlegen, der größer als die zugehörigen neben geordneten

- Die `Basis` angefügte bindbare Eigenschaft wird für die beiden `BoxView` Elemente festgelegt, um Ihnen eine Breite von 50 Pixeln zu übergeben. Diese Eigenschaft `FlexBasis` hat den Typ, eine-Struktur, die eine statische Eigenschaft des Typs mit `FlexBasis` dem Namen definiert `Auto` . Dies ist die Standardeinstellung. Mit können Sie `Basis` eine Pixelgröße oder einen Prozentsatz angeben, der angibt, wie viel Speicherplatz das Element auf der Hauptachse einnimmt. Sie wird als _Basis_ bezeichnet, da Sie eine Elementgröße angibt, die die Basis für alle nachfolgenden Layouts ist.

- Die `Grow` -Eigenschaft wird für das-Element und das untergeordnete Element festgelegt, `Layout` `Label` das den Inhalt darstellt. Diese Eigenschaft ist vom Datentyp `float` und hat den Standardwert 0. Wenn ein positiver Wert festgelegt wird, wird der gesamte verbleibende Platz entlang der Hauptachse diesem Element und den gleich geordneten Elementen mit positiven Werten von zugeordnet `Grow` . Der Speicherplatz wird proportional zu den Werten zugewiesen, ähnlich wie die Stern Angabe in einer `Grid` .

    Die erste `Grow` angefügte-Eigenschaft wird für die geschachtelte festgelegt `FlexLayout` , um anzugeben, dass dies `FlexLayout` den gesamten nicht verwendeten vertikalen Leerraum innerhalb der äußeren enthalten soll `FlexLayout` . Die zweite `Grow` angefügte-Eigenschaft wird auf dem festgelegt `Label` , der den Inhalt darstellt, und gibt an, dass dieser Inhalt den gesamten nicht verwendeten horizontalen Leerraum innerhalb des inneren einnimmt `FlexLayout` .

    Es gibt auch eine ähnliche `Shrink` angefügte bindbare Eigenschaft, die Sie verwenden können, wenn die Größe der untergeordneten Elemente die Größe von überschreitet, `FlexLayout` aber keine umfügung gewünscht ist.

### <a name="catalog-items-with-flexlayout"></a>Katalog Elemente mit "flexlayout"

Die Seite **Katalog Elemente** im Beispiel " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " ähnelt [Beispiel 1 in Abschnitt 1,1 der Spezifikation "CSS-flexlayoutbox](https://www.w3.org//TR/css-flexbox-1/#overview) " mit der Ausnahme, dass Sie eine horizontal scrollbare Reihe von Bildern und Beschreibungen von drei Affen anzeigt:

[![Die Seite "Katalog Elemente"](flex-layout-images/CatalogItems.png "Die Seite "Katalog Elemente"")](flex-layout-images/CatalogItems-Large.png#lightbox)

Jeder der drei Affen ist ein `FlexLayout` , der in einem enthalten ist, `Frame` der eine explizite Höhe und Breite erhält und der ebenfalls ein untergeordnetes Element eines größeren ist `FlexLayout` . In dieser XAML-Datei werden die meisten Eigenschaften der untergeordneten Elemente `FlexLayout` in Stilen angegeben, wobei es sich bei allen um einen impliziten Stil handelt:

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

Der implizite Stil für das `Image` umfasst Einstellungen für zwei angefügte bindbare Eigenschaften von `Flexlayout` :

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Die `Order` Einstellung &ndash; 1 bewirkt `Image` , dass das Element in jeder geschachtelten Ansicht zuerst angezeigt wird, `FlexLayout` unabhängig von seiner Position in der Auflistung Children. Die- `AlignSelf` Eigenschaft von `Center` bewirkt `Image` , dass innerhalb der zentriert wird `FlexLayout` . Dies überschreibt die Einstellung der `AlignItems` -Eigenschaft, die den Standardwert hat `Stretch` , was bedeutet, dass die untergeordneten Elemente `Label` und `Button` auf die vollständige Breite des gestreckt werden `FlexLayout` .

In jeder der drei `FlexLayout` Ansichten ist ein leeres vor `Label` dem `Button` , aber es hat die `Grow` Einstellung 1. Dies bedeutet, dass der gesamte zusätzliche vertikale Leerraum diesem Leerzeichen zugeordnet ist `Label` , wodurch die nach-unten-Taste gedrückt wird `Button` .

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Die bindbaren Eigenschaften im Detail

Nachdem Sie nun einige gängige Anwendungen von kennengelernt haben `FlexLayout` , können die Eigenschaften von ausführlicher unter `FlexLayout` sucht werden.
`FlexLayout`definiert sechs bindbare Eigenschaften, die Sie im `FlexLayout` Code oder in XAML festlegen, um die Ausrichtung und Ausrichtung zu steuern. (Eine dieser Eigenschaften, [`Position`](xref:Xamarin.Forms.FlexLayout.Position) , wird in diesem Artikel nicht behandelt.)

Sie können mit den fünf verbleibenden bindbaren Eigenschaften experimentieren, indem Sie die Seite " **Experiment** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " verwenden. Auf dieser Seite können Sie untergeordnete Elemente aus hinzufügen oder entfernen, `FlexLayout` um Kombinationen der fünf bindbaren Eigenschaften festzulegen. Alle untergeordneten Elemente von `FlexLayout` sind `Label` Ansichten verschiedener Farben und Größen, wobei die- `Text` Eigenschaft auf eine Zahl festgelegt ist, die ihrer Position in der Auflistung entspricht `Children` .

Wenn das Programm gestartet wird, werden in fünf `Picker` Ansichten die Standardwerte dieser fünf `FlexLayout` Eigenschaften angezeigt. Am `FlexLayout` unteren Rand des Bildschirms befinden sich drei untergeordnete Elemente:

[![Die Experiment Seite: Standard](flex-layout-images/ExperimentDefault.png "Die Seite "Experiment"-Standard")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Jede `Label` Ansicht verfügt über einen grauen Hintergrund, der den Platz anzeigt, der in `Label` der zugeordnet ist `FlexLayout` . Der Hintergrund der `FlexLayout` selbst ist Alice Blue. Es belegt den gesamten unteren Bereich der Seite, mit Ausnahme eines kleinen Randes auf der linken und rechten Seite.

<a name="direction" />

### <a name="the-direction-property"></a>Die Direction-Eigenschaft

Die- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction) Eigenschaft ist vom Typ [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) , eine Enumeration mit vier Membern:

- `Column`
- `ColumnReverse`(oder "Column-Reverse" in XAML)
- `Row`, der Standardwert
- `RowReverse`(oder "Row-Reverse" in XAML)

In XAML können Sie den Wert dieser Eigenschaft mit den Namen der Enumerationsmember in Kleinbuchstaben, Großbuchstaben oder gemischter Groß-/Kleinschreibung angeben, oder Sie können zwei zusätzliche Zeichen folgen verwenden, die in Klammern angezeigt werden, die mit den CSS-Indikatoren identisch sind. (Die "Column-Reverse"-und "Row-Reverse"-Zeichen folgen werden in der Klasse definiert, [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) die vom XAML-Parser verwendet wird.)

Hier ist die **Experiment** Seite (von links nach rechts), `Row` Richtung, `Column` Richtung und `ColumnReverse` Richtung:

[![Die Experiment Seite: Richtung](flex-layout-images/ExperimentDirection.png "Die Experiment Seitenrichtung")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Beachten Sie, dass `Reverse` die Elemente bei den Optionen ganz rechts oder unten beginnen.

<a name="wrap" />

### <a name="the-wrap-property"></a>Die Wrap-Eigenschaft

Die- [`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap) Eigenschaft ist vom Typ [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) , eine Enumeration mit drei Elementen:

- `NoWrap`, der Standardwert
- `Wrap`
- `Reverse`(oder "Wrap-Reverse" in XAML)

Von links nach rechts zeigen diese Bildschirme die `NoWrap` `Wrap` Optionen, und für 12 untergeordnete Elemente `Reverse` an:

[![Die Experiment Seite: Wrap](flex-layout-images/ExperimentWrap.png "Der Experiment Seitenumbruch")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Wenn die `Wrap` -Eigenschaft auf festgelegt ist `NoWrap` und die Hauptachse (wie in diesem Programm) eingeschränkt ist, und die Hauptachse nicht breit oder hoch genug ist, um alle untergeordneten Elemente zu erfüllen, `FlexLayout` versucht, die Elemente zu verkleinern, wie der IOS-Bildschirmfoto zeigt. Sie können die Verkleinerung der Elemente mit der [`Shrink`](#shrink) angefügten bindbare-Eigenschaft steuern.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Die Eigenschaft "justifycontent"

Die- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent) Eigenschaft ist vom Typ [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) , eine Enumeration mit sechs Membern:

- `Start`(oder "Flex-Start" in XAML), die Standardeinstellung
- `Center`
- `End`(oder "Flex-End" in XAML)
- `SpaceBetween`(oder "Leerzeichen zwischen" in XAML)
- `SpaceAround`(oder "Leerraum" in XAML)
- `SpaceEvenly`

Diese Eigenschaft gibt an, wie die Elemente auf der Hauptachse, d. h. die horizontale Achse in diesem Beispiel, in den Abstand gestellt werden:

[![Die Experiment Seite: Inhalt rechtfertigen](flex-layout-images/ExperimentJustifyContent.png "Das Experiment Seiten-rechtfertigen Inhalt")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

In allen drei Screenshots wird die- `Wrap` Eigenschaft auf festgelegt `Wrap` . Der `Start` Standardwert wird im vorherigen Android-Bildschirmfoto angezeigt. Der IOS-Bildschirmfoto zeigt die `Center` Option an: alle Elemente werden in den Mittelpunkt verschoben. Die drei weiteren Optionen, die mit dem Wort beginnen, weisen `Space` den zusätzlichen Platz zu, der nicht von den Elementen belegt wird. `SpaceBetween`ordnet den Raum gleichmäßig zwischen den Elementen zu. `SpaceAround`legt den gleichen Leerraum um jedes Element fest, während den `SpaceEvenly` gleichen Raum zwischen den einzelnen Elementen und vor dem ersten Element und hinter dem letzten Element in der Zeile einfügt.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Die alignitems-Eigenschaft

Die- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems) Eigenschaft ist vom Typ [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) , eine Enumeration mit vier Membern:

- `Stretch`, der Standardwert
- `Center`
- `Start`(oder "Flex-Start" in XAML)
- `End`(oder "Flex-End" in XAML)

Dies ist eine von zwei Eigenschaften (die andere ist [`AlignContent`](#align-content) ), die angibt, wie untergeordnete Elemente auf der Kreuz Achse ausgerichtet werden. In jeder Zeile werden die untergeordneten Elemente gestreckt (wie im vorherigen Screenshot dargestellt) oder am Anfang, Mittelpunkt oder Ende der einzelnen Elemente ausgerichtet, wie in den folgenden drei Screenshots gezeigt:

[![Die Experiment Seite: Elemente ausrichten](flex-layout-images/ExperimentAlignItems.png "Das Experiment Seiten-Ausrichten von Elementen")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Im IOS-Screenshot werden die Spitzen aller untergeordneten Elemente ausgerichtet. In den Android-Screenshots werden die Elemente vertikal basierend auf dem höchsten untergeordneten Element zentriert. Im UWP-Bildschirmfoto werden die untersten Elemente aller Elemente ausgerichtet.

Für jedes einzelne Element kann die `AlignItems` Einstellung mit der [`AlignSelf`](#align-self) angefügten bindbaren Eigenschaft überschrieben werden.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Die aligncontent-Eigenschaft

Die- [`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent) Eigenschaft ist vom Typ [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) , eine Enumeration mit sieben Membern:

- `Stretch`, der Standardwert
- `Center`
- `Start`(oder "Flex-Start" in XAML)
- `End`(oder "Flex-End" in XAML)
- `SpaceBetween`(oder "Leerzeichen zwischen" in XAML)
- `SpaceAround`(oder "Leerraum" in XAML)
- `SpaceEvenly`

Ebenso `AlignItems` richtet die-Eigenschaft auch untergeordnete Elemente `AlignContent` auf der quer Achse aus, wirkt sich aber auf ganze Zeilen oder Spalten aus:

[![Die Experiment Seite: Inhalt ausrichten](flex-layout-images/ExperimentAlignContent.png "Der Inhalt der Inhaltsseite")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

Im IOS-Bildschirmfoto befinden sich beide Zeilen im oberen Bereich. im Android-Screenshot befinden Sie sich in der Mitte. und im UWP-Screenshot befinden Sie sich im unteren Bereich. Die Zeilen können auch auf unterschiedliche Weise verteilt werden:

[![Die Experiment Seite: Inhalt ausrichten 2](flex-layout-images/ExperimentAlignContent2.png "Die Experiment Seite-ausrichten von Inhalt 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent`Hat keine Auswirkung, wenn nur eine Zeile oder Spalte vorhanden ist.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Details der angefügten bindbaren Eigenschaften

`FlexLayout`definiert fünf angefügte bindbare Eigenschaften. Diese Eigenschaften werden auf untergeordnete Elemente von festgelegt `FlexLayout` und beziehen sich nur auf das jeweilige untergeordnete Element.

<a name="align-self" />

### <a name="the-alignself-property"></a>Die alignself-Eigenschaft

Die [`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) angefügte bindbare Eigenschaft weist den Typ auf [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) , eine Enumeration mit fünf Membern:

- `Auto`, der Standardwert
- `Stretch`
- `Center`
- `Start`(oder "Flex-Start" in XAML)
- `End`(oder "Flex-End" in XAML)

Für alle untergeordneten Elemente von `FlexLayout` überschreibt diese Eigenschafts Einstellung die-Eigenschaft, die für [`AlignItems`](#align-items) das selbst festgelegt ist `FlexLayout` . Mit der Standardeinstellung von wird `Auto` die- `AlignItems` Einstellung verwendet.

Für ein- `Label` Element `label` mit dem Namen (oder Beispiel) können Sie die- `AlignSelf` Eigenschaft in Code wie dem folgenden festlegen:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Beachten Sie, dass es keinen Verweis auf das `FlexLayout` übergeordnete Element von gibt `Label` . In XAML legen Sie die-Eigenschaft wie folgt fest:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Die Order-Eigenschaft

Die- [`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty) Eigenschaft ist vom Typ `int` . Der Standardwert ist 0.

Mit der- `Order` Eigenschaft können Sie die Reihenfolge ändern, in der die untergeordneten Elemente von `FlexLayout` angeordnet werden. In der Regel ist die Anordnung der unter `FlexLayout` geordneten Elemente in der gleichen Reihenfolge, in der Sie in der Auflistung angezeigt werden `Children` . Sie können diese Reihenfolge außer Kraft setzen, indem Sie die `Order` angefügte bindbare Eigenschaft auf ein oder mehrere untergeordnete Elemente auf einen ganzzahligen Wert ungleich 0 festlegen. Ordnet dann die untergeordneten Elemente auf `FlexLayout` der Grundlage der Einstellung der-Eigenschaft für jedes untergeordnete Element an, aber untergeordnete Elemente `Order` mit derselben `Order` Einstellung werden in der Reihenfolge angeordnet, in der Sie in der Auflistung angezeigt werden `Children` .

### <a name="the-basis-property"></a>Die Basis Eigenschaft

Die [`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty) angefügte bindbare Eigenschaft gibt die Menge an Speicherplatz an, die einem untergeordneten Element von `FlexLayout` auf der Hauptachse zugewiesen wird. Die Größe, die von der- `Basis` Eigenschaft angegeben wird, ist die Größe entlang der Hauptachse des übergeordneten Elements `FlexLayout` . Daher `Basis` gibt die Breite eines untergeordneten Elements an, wenn die untergeordneten Elemente in Zeilen angeordnet sind, oder die Höhe, wenn die untergeordneten Elemente in Spalten angeordnet sind.

Die- `Basis` Eigenschaft hat den Typ [`FlexBasis`](xref:Xamarin.Forms.FlexBasis) , eine-Struktur. Die Größe kann entweder in geräteunabhängigen Einheiten oder als Prozentsatz der Größe des angegeben werden `FlexLayout` . Der Standardwert der `Basis` Eigenschaft ist die statische Eigenschaft `FlexBasis.Auto` . Dies bedeutet, dass die angeforderte Breite oder Höhe des untergeordneten Elements verwendet wird.

Im Code können Sie die `Basis` -Eigenschaft für einen `Label` mit dem `label` Namen auf 40 geräteunabhängige Einheiten wie folgt festlegen:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Das zweite Argument für den `FlexBasis` Konstruktor wird benannt `isRelative` und gibt an, ob die Größe relativ ( `true` ) oder absolut ( `false` ) ist. Das-Argument hat den Standardwert `false` , sodass Sie auch den folgenden Code verwenden können:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Eine implizite Konvertierung von `float` in `FlexBasis` ist definiert, sodass Sie Sie noch weiter vereinfachen können:

```csharp
FlexLayout.SetBasis(label, 40);
```

Sie können die Größe auf 25% des übergeordneten Elements wie folgt festlegen `FlexLayout` :

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

Auf der Seite " **Basis Experiment** " des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " können Sie mit der- `Basis` Eigenschaft experimentieren. Auf der Seite wird eine umschließende Spalte `Label` mit fünf Elementen mit wechselnden Hintergrund-und Vorder Grundfarben angezeigt. Mit zwei `Slider` -Elementen können Sie `Basis` Werte für das zweite und vierte angeben `Label` :

[![Die Basis Experiment Seite](flex-layout-images/BasisExperiment.png "Die Basis Experiment Seite")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Der IOS-Screenshot auf der linken Seite zeigt die beiden `Label` Elemente in geräteunabhängigen Einheiten. Im Android-Bildschirm wird angezeigt, dass Ihnen Höhen zugewiesen werden, die ein Bruchteil der Gesamthöhe des sind `FlexLayout` . Wenn das-Element `Basis` bei 100% festgelegt ist, ist das untergeordnete Element die Höhe von `FlexLayout` und wird mit der nächsten Spalte Umbruch und die gesamte Höhe dieser Spalte belegen, wie der UWP-Bildschirmfoto zeigt: Es wird angezeigt, als ob die fünf untergeordneten Elemente in einer Zeile angeordnet sind, aber Sie sind tatsächlich in fünf Spalten angeordnet.

### <a name="the-grow-property"></a>Die Grow-Eigenschaft

Die [`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty) angefügte bindbare Eigenschaft weist den Typ auf `int` . Der Standardwert ist 0, und der Wert muss größer oder gleich 0 sein.

Die `Grow` -Eigenschaft spielt eine Rolle, wenn die `Wrap` -Eigenschaft auf festgelegt ist `NoWrap` und die Zeile der untergeordneten Elemente eine Gesamtbreite kleiner als die Breite von hat `FlexLayout` , oder die-Spalte untergeordneter Elemente hat eine kürzere Höhe als `FlexLayout` . Die- `Grow` Eigenschaft gibt an, wie der übrig gebliebene Raum zwischen den untergeordneten Elementen angezeigt werden soll.

Auf der Seite zum **vergrößern** des Experiments `Label` werden fünf Elemente von abwechselnder Farben in einer-Spalte angeordnet, und mit zwei `Slider` -Elementen können Sie die `Grow` -Eigenschaft der zweiten und vierten anpassen `Label` . Der IOS-Bildschirm Abbildung ganz links zeigt die Standard `Grow` Eigenschaften 0 an:

[![Die Seite zum Vergrößern des Experiments](flex-layout-images/GrowExperiment.png "Die Seite zum Vergrößern des Experiments")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Wenn einem untergeordneten Element ein positiver Wert zugewiesen wird `Grow` , nimmt dieses untergeordnete Element den gesamten verbleibenden Platz in Kauf, wie im Android-Bildschirmfoto veranschaulicht. Dieser Speicherplatz kann auch unter zwei oder mehr untergeordneten Elementen zugeordnet werden. Im UWP-Bildschirm Abbildung wird die- `Grow` Eigenschaft der zweiten `Label` auf 0,5 festgelegt, während die- `Grow` Eigenschaft der vierten `Label` 1,5 ist, wodurch das vierte `Label` drei fache des restlichen Speicherplatzes als Sekunde ergibt `Label` .

Wie die untergeordnete Ansicht diesen Bereich verwendet, hängt vom jeweiligen Typ des untergeordneten Elements ab. Bei einem `Label` kann der Text `Label` mit den Eigenschaften und innerhalb des gesamten Speicherplatzes von positioniert werden `HorizontalTextAlignment` `VerticalTextAlignment` .

<a name="shrink" />

### <a name="the-shrink-property"></a>Die Shrink-Eigenschaft

Die [`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) angefügte bindbare Eigenschaft weist den Typ auf `int` . Der Standardwert ist 1, und der Wert muss größer oder gleich 0 sein.

Die `Shrink` -Eigenschaft spielt eine Rolle, wenn die `Wrap` -Eigenschaft auf festgelegt ist `NoWrap` und die Aggregat Breite einer Zeile mit untergeordneten Elementen größer ist als die Breite von `FlexLayout` , oder die Aggregat Höhe einer einzelnen Spalte untergeordneter Elemente ist größer als die Höhe des `FlexLayout` . Normalerweise `FlexLayout` werden diese untergeordneten Elemente angezeigt, wenn Sie Ihre Größe überschreiten. Die- `Shrink` Eigenschaft kann angeben, welche untergeordneten Elemente Priorität haben, wenn Sie in ihrer vollen Größe angezeigt werden.

Die Seite " **Versuch verkleinern** " erstellt ein-Element `FlexLayout` mit einer einzelnen Zeile mit fünf untergeordneten Elementen `Label` , die mehr Platz als die `FlexLayout` Breite benötigen. Der IOS-Screenshot auf der linken Seite zeigt alle `Label` Elemente mit den Standardwerten 1 an:

[![Die Seite "Versuch verkleinern"](flex-layout-images/ShrinkExperiment.png "Die Seite "Versuch verkleinern"")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Im Android-Screenshot wird der `Shrink` Wert für den zweiten `Label` auf 0 festgelegt, und der Wert `Label` wird in der vollständigen Breite angezeigt. Außerdem erhält der vierte `Label` `Shrink` Wert einen Wert, der größer als 1 ist und verkleinert wurde. Der UWP-Bildschirm Abbildung zeigt `Label` , dass beide Elemente den `Shrink` Wert 0 erhalten, damit Sie in ihrer vollen Größe angezeigt werden können, sofern dies möglich ist.

Sie können den `Grow` -Wert und den- `Shrink` Wert auf Situationen festlegen, in denen die untergeordneten Aggregat Größen manchmal kleiner als oder manchmal größer als die Größe von sind `FlexLayout` .

## <a name="css-styling-with-flexlayout"></a>CSS-Formatierung mit flexlayout

Sie können die [CSS](~/xamarin-forms/user-interface/styles/css/index.md) -Formatierungsfunktion verwenden, die mit Xamarin.Forms 3,0 in Verbindung mit eingeführt wurde `FlexLayout` . Die Seite **CSS-Katalog Elemente** des Beispiels " **[flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** " dupliziert das Layout der Seite " **Katalog Elemente** ", aber mit einem CSS-Stylesheet für viele der Stile:

[![Die Seite CSS-Katalog Elemente](flex-layout-images/CssCatalogItems.png "Die Seite CSS-Katalog Elemente")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Die ursprüngliche **catalogitemspage. XAML** -Datei enthält fünf `Style` Definitionen in Ihrem `Resources` Abschnitt mit 15 `Setter` Objekten. In der **csscatalogitemspage. XAML** -Datei, die auf zwei `Style` Definitionen mit nur vier Objekten reduziert wurde `Setter` . Diese Stile ergänzen das CSS-Stylesheet für Eigenschaften, die von der CSS-Formatierungs Xamarin.Forms Funktion derzeit nicht unterstützt werden:

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

`FlexLayout`Hier werden mehrere angefügte bindbare Eigenschaften referenziert. Im `label.empty` Selektor wird das-Attribut angezeigt `flex-grow` , das einen leeren formatiert, um einen leeren `Label` Bereich oberhalb von bereitzustellen `Button` . Der `image` Selektor enthält ein `order` -Attribut und ein- `align-self` Attribut, von denen beide den `FlexLayout` angefügten bindbaren Eigenschaften entsprechen.

Sie haben gesehen, dass Sie Eigenschaften direkt auf festlegen können, `FlexLayout` und Sie können angefügte bindbare Eigenschaften für die untergeordneten Elemente eines festlegen `FlexLayout` . Oder Sie können diese Eigenschaften indirekt mithilfe herkömmlicher XAML-basierter Stile oder CSS-Stile festlegen. Wichtig ist es, diese Eigenschaften zu kennen und zu verstehen. Durch diese Eigenschaften ist das `FlexLayout` wirklich flexibel.

## <a name="flexlayout-with-xamarinuniversity"></a>Flexlayout mit xamarin. University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms3,0-flexlayoutvideo**

## <a name="related-links"></a>Verwandte Links

- [Flexlayoutdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
