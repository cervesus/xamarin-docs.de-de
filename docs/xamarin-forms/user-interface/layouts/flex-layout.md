---
title: Die Xamarin.Forms-FlexLayout
description: Verwenden Sie FlexLayout für Stapeln oder eine Auflistung von untergeordneten Ansichten umschließen.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 7626c49b2267cbd087a16c310f1b85aea7139823
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61303454"
---
# <a name="the-xamarinforms-flexlayout"></a>Die Xamarin.Forms-FlexLayout

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)

_Verwenden Sie FlexLayout für Stapeln oder eine Auflistung von untergeordneten Ansichten umschließen._

Die Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) ist neu in Xamarin.Forms-Version 3.0. Es basiert auf dem CSS [Flexible Box-Layout-Module](http://www.w3.org/TR/css-flexbox-1/), häufig als bezeichnet _flex Layout_ oder _-Box-Flex_, daraus, dass es viele flexible Optionen zum Anordnen von untergeordneten Elementen enthält innerhalb des Layouts.

`FlexLayout` ist vergleichbar mit der Xamarin.Forms [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) , da es seine untergeordneten Elemente horizontal und vertikal in einem Stapel anordnen kann. Allerdings die `FlexLayout` verfügt auch über viele Optionen für die Ausrichtung, Ausrichtung und zur Anpassung an verschiedene Bildschirmgrößen und kann auch seine untergeordneten Elemente umschließen, wenn zu viele in eine einzelne Zeile oder Spalte passen.

`FlexLayout` leitet sich von [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) und erbt einen [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) Eigenschaft vom Typ `IList<View>`.

`FlexLayout` definiert sechs öffentliche bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, die die Größe, Ausrichtung und Ausrichtung der untergeordneten Elemente zu beeinträchtigen. (Wenn Sie nicht mit angefügten bindbare Eigenschaften vertraut sind, finden Sie im Artikel  **[angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)**.) Diese Eigenschaften werden in den folgenden Abschnitten ausführlich beschrieben, auf **[die bindbaren Eigenschaften im Detail](#bindable-properties)** und  **[angefügten bindbare Eigenschaften im Detail](#attached-properties)**. Allerdings in diesem Artikel beginnt mit einem Abschnitt auf einigen **[allgemeine Verwendungsszenarien](#common-scenarios)** von `FlexLayout` , viele dieser Eigenschaften informell beschreibt. Am Ende des Artikels, sehen Sie, wie Sie kombinieren `FlexLayout` mit [CSS-Stylesheets](~/xamarin-forms/user-interface/styles/css/index.md).

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>Allgemeine Verwendungsszenarien

Die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispielprogramm enthält mehrere Seiten, die veranschaulichen einige gebräuchliche Verwendungen von `FlexLayout` und ermöglicht Ihnen das Experimentieren mit seinen Eigenschaften.

### <a name="using-flexlayout-for-a-simple-stack"></a>Verwenden für einen einfachen Stapel FlexLayout

Die **einfache Stack** zeigt wie Seite `FlexLayout` ersetzen kann eine `StackLayout` jedoch einfacher Markup. Alles, was in diesem Beispiel wird in der XAML-Seite definiert. Die `FlexLayout` enthält vier untergeordnete Elemente:

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

Hier ist diese Seite, die unter iOS, Android und die universelle Windows-Plattform:

[![Die einfache Stack Seite](flex-layout-images/SimpleStack.png "einfachen Stack-Seite")](flex-layout-images/SimpleStack-Large.png#lightbox)

Drei Eigenschaften des `FlexLayout` werden angezeigt, der **SimpleStackPage.xaml** Datei:

- Die [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) -Eigenschaftensatz auf den Wert der [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) Enumeration. Die Standardeinstellung ist `Row`. Festlegen der Eigenschaft auf `Column` bewirkt, dass die untergeordneten Elemente der `FlexLayout` in einer einzelnen Spalte Elemente angeordnet werden.

    Wenn Elemente in eine `FlexLayout` werden in einer Spalte angeordnet der `FlexLayout` hat eine vertikale _Hauptachse_ und eine horizontale _cross-Achse_.

- Die [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Eigenschaft ist vom Typ [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) und gibt an, wie Elemente auf der querachse ausgerichtet sind. Die `Center` Option bewirkt, dass jedes Element horizontal zentriert werden soll.

    Bei der Verwendung einer `StackLayout` anstelle eines `FlexLayout` für diesen Task, würden Sie alle Elemente zentrieren, durch Zuweisen der `HorizontalOptions` -Eigenschaft der einzelnen Elemente zu `Center`. Die `HorizontalOptions` Eigenschaft funktioniert nicht für untergeordnete Elemente des eine `FlexLayout`, aber der einzelnen `AlignItems` Eigenschaft wird das gleiche Ziel erreicht. Wenn Sie möchten, können Sie mithilfe der `AlignSelf` angefügte bindbare Eigenschaft überschreiben die `AlignItems` -Eigenschaft für einzelne Elemente:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    Mit dieser Änderung dieser `Label` befindet sich am linken Rand der `FlexLayout` Wenn ist die Lesefolge von links-nach-rechts.

- Die [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Eigenschaft ist vom Typ [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), und gibt an, wie Elemente auf der Hauptachse angeordnet sind. Die `SpaceEvenly` Option ordnet alle übrig gebliebene vertikalen Platz gleichmäßig auf alle Elemente, das erste Element, und geben Sie unterhalb des letzten Elements.

    Bei der Verwendung einer `StackLayout`, müssten Sie weisen die `VerticalOptions` -Eigenschaft der einzelnen Elemente zu `CenterAndExpand` auf einen ähnlichen Effekt zu erzielen. Aber die `CenterAndExpand` Option würde die doppelt so viel Platz zwischen den einzelnen Elementen als vor dem ersten Element und nach dem letzten Element zuzuordnen. Können Sie imitieren die `CenterAndExpand` Option `VerticalOptions` durch Festlegen der `JustifyContent` Eigenschaft `FlexLayout` zu `SpaceAround`.

Diese `FlexLayout` Eigenschaften werden ausführlicher im Abschnitt **[die bindbaren Eigenschaften im Detail](#bindable-properties)** unten.

### <a name="using-flexlayout-for-wrapping-items"></a>Verwenden FlexLayout für das Umschließen von Elementen

Die **Foto Wrapping** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel wird veranschaulicht, wie `FlexLayout` können die untergeordneten Elemente, die zusätzlichen Zeilen oder Spalten einschließen. Die XAML-Datei instanziiert den `FlexLayout` und weist zwei Eigenschaften:

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

Die `Direction` -Eigenschaft dieses `FlexLayout` nicht festgelegt ist, es gelten somit die Standardeinstellung von `Row`, was bedeutet, dass die untergeordneten Elemente werden in Zeilen angeordnet, und die wichtigsten Achse horizontal ist.

Die [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Eigenschaft weist einen Enumerationstyp [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap). Wenn zu viele Elemente in einer Zeile passen vorhanden sind, wird die Einstellung dieser Eigenschaft die Elemente, die in die nächste Zeile umbrochen werden.

Beachten Sie, dass die `FlexLayout` ist ein untergeordnetes Element von einem `ScrollView`. Wenn zu viele Zeilen auf der Seite passen die `ScrollView` hat den Standardwert `Orientation` Eigenschaft `Vertical` und ermöglicht es, einen vertikalen Bildlauf ausführen.

Die `JustifyContent` Eigenschaft ordnet übrig gebliebenen Platz auf der Hauptachse (der horizontalen Achse), sodass jedes Element, indem Sie die gleiche Menge an Leerzeichen eingeschlossen ist.

Code-Behind-Datei greift auf eine Sammlung von Beispiel-Fotos und fügt sie der `Children` Auflistung von der `FlexLayout`:

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

Hier ist das Programm ausgeführt wird, schrittweise von oben nach unten gescrollt:

[![Die Foto-Seite "Wrapping"](flex-layout-images/PhotoWrapping.png "der Foto-Wrapping-Seite")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>Seitenlayout mit FlexLayout

Ist es ein Standardlayout beim Webdesign wird aufgerufen, die [ _Kokosnuss_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) da es sich um ein Layoutformat handelt, der sehr angenehmen, aber mit Perfektion dabei häufig schwierig ist. Das Layout besteht aus einem Header am oberen Rand der Seite und einer Fußzeile im unteren Bereich beide Erweitern auf die gesamte Breite der Seite. Die Mitte der Seite einnimmt ist der Hauptinhalt, häufig mit einem spaltenbasierten Menü auf der linken Seite des Inhalts und zusätzliche (bezeichnet ein _reserviert_ Bereich) auf der rechten Seite. [Abschnitt 5.4.1 der CSS-Layout für flexibles Feld Spezifikation](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) wird beschrieben, wie das Layout der Heilige Gral mit aktiviertem Kontrollkästchen für eine flexible realisiert werden kann.

Die **Kokosnuss Layout** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel zeigt eine einfache Implementierung dieses Layout mit einem `FlexLayout` in einer anderen geschachtelt. Da auf dieser Seite für ein Telefon im Hochformat ausgelegt ist, sind die Bereiche, links und rechts vom Bereich nur 50 Pixel breit:

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

[![Der Heilige Gral Layoutseite](flex-layout-images/HolyGrailLayout.png "der Heilige Gral-Layoutseite")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

Die Navigation und Aside-Bereiche werden gerendert, mit einer `BoxView` auf der linken und rechten Seite.

Die erste `FlexLayout` in der XAML-Datei hat eine main vertikale Achse und enthält drei untergeordnete Elemente in einer Spalte angeordnet. Dies sind die Header, der den Text der Seite, und die Fußzeile. Die geschachtelte `FlexLayout` verfügt über eine main-Horizontalachse mit drei untergeordneten Elemente in einer Zeile angeordnet sind.

In diesem Programm werden die drei angefügte bindbare Eigenschaften veranschaulicht:

- Die `Order` angefügte bindbare Eigenschaft festgelegt ist auf dem ersten `BoxView`. Diese Eigenschaft ist eine ganze Zahl, Standardwert 0. Sie können diese Eigenschaft verwenden, um die Layoutreihenfolge ändern. In der Regel Entwickler bevorzugen den Inhalt der Seite, um die im Markup vor den Navigationselementen erscheinen und reserviert Elemente. Festlegen der `Order` Eigenschaft am ersten `BoxView` auf einen Wert kleiner als der andere gleichgeordnete Elemente bewirkt, dass es als das erste Element in der Zeile angezeigt werden. Auf ähnliche Weise können Sie sicherstellen, dass ein Element durch Festlegen von letzten angezeigt wird der `Order` Eigenschaft auf einen Wert größer als ihre Geschwister.

- Die `Basis` angefügte bindbare Eigenschaft festgelegt ist, auf die beiden `BoxView` Elemente, die ihnen eine Breite von 50 Pixeln gewähren. Diese Eigenschaft ist vom Typ `FlexBasis`, eine Struktur, eine statische Eigenschaft des Typs definiert `FlexBasis` mit dem Namen `Auto`, dies ist die Standardeinstellung. Sie können `Basis` an eine Größe in Pixel oder als Prozentsatz, der gibt an, wie viel Speicherplatz wird das Element auf der Hauptachse belegt. Es heißt eine _Basis_ , da es sich um eine Elementgröße angibt, die die Grundlage für alle nachfolgenden Layout ist.

- Die `Grow` -Eigenschaftensatz auf den geschachtelten `Layout` und klicken Sie auf die `Label` untergeordnetes Element, das den Inhalt darstellt. Diese Eigenschaft ist vom Typ `float` und hat den Standardwert 0. Wenn auf einen positiven Wert festgelegt, der verbleibende Speicherplatz auf der Haupt-Achse auf dieses Element und nebengeordneten Elementen mit positiven Werte der zugeordnet wird `Grow`. Der Speicherplatz ist proportional reserviert, auf die Werte, wie etwa die Stern-Spezifikation in einem `Grid`.

    Die erste `Grow` angefügte Eigenschaft festgelegt ist auf den geschachtelten `FlexLayout`, um anzugeben, das von diesem `FlexLayout` besteht darin, alle nicht verwendeten vertikalen Speicherplatz innerhalb der äußeren belegen `FlexLayout`. Die zweite `Grow` angefügte Eigenschaft festgelegt ist, auf die `Label` , die den Inhalt, der angibt, dass dieser Inhalt ist, belegt Sie alle nicht verwendeten horizontalen Bereich in der inneren darstellt `FlexLayout`.

    Es gibt auch eine ähnliche `Shrink` angefügte bindbare Eigenschaft, die Sie verwenden können, wenn die Größe der untergeordneten Elemente die Größe des überschreitet die `FlexLayout` aber Wrapping nicht erwünscht.

### <a name="catalog-items-with-flexlayout"></a>Katalogelemente mit FlexLayout

Die **Katalogelemente** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel ähnelt [Beispiel 1 in Abschnitt 1.1 der CSS-Flex Layout im Spezifikation](http://www.w3.org/TR/css-flexbox-1/#overview)mit dem Unterschied, dass eine horizontal bildlauffähigem Reihe von Bildern und Beschreibungen der drei Affen angezeigt:

[![Der Katalog Elemente Seite](flex-layout-images/CatalogItems.png "Katalog Elemente Seite")](flex-layout-images/CatalogItems-Large.png#lightbox)

Jede der drei Affen ist eine `FlexLayout` innerhalb einer `Frame` erhält, die eine explizite Höhe und Breite aus, und dies ist auch ein untergeordnetes Element von einem größeren `FlexLayout`. In dieser XAML-Datei, die meisten Eigenschaften von den `FlexLayout` untergeordnete Elemente werden in Stilen, aber es ist ein impliziter Stil aller angegeben:

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

Die impliziter Stil für die `Image` enthält Einstellungen, die von zwei angefügte bindbaren Eigenschaften des `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

Die `Order` -Einstellung &ndash;1 bewirkt, dass die `Image` Element zuerst in den einzelnen des geschachtelten angezeigt werden `FlexLayout` Ansichten unabhängig von seiner Position in der Auflistung untergeordneter Elemente. Die `AlignSelf` Eigenschaft `Center` bewirkt, dass die `Image` in zentriert werden soll die `FlexLayout`. Dies überschreibt die Einstellung der der `AlignItems` -Eigenschaft, die einen Standardwert besitzt der `Stretch`. Dies bedeutet, die die `Label` und `Button` untergeordnete Elemente werden gestreckt, um die gesamte Breite des der `FlexLayout`.

In jedem der drei `FlexLayout` anzeigt, ein Leerzeichen `Label` vorangestellt ist die `Button`, verfügt aber über eine `Grow` 1 festlegen. Dies bedeutet, dass alle sehr von vertikalen Speicherplatz, um diese Einstellung leer zugewiesen wurde, `Label`, die effektiv überträgt die `Button` unten.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>Die bindbare Eigenschaften im detail

Nun, da Sie einige allgemeine Anwendungen gesehen haben `FlexLayout`, die Eigenschaften des `FlexLayout` ausführlicher besprochen werden können.
`FlexLayout` definiert die sechs bindbare Eigenschaften, die Sie festlegen, auf die `FlexLayout` selbst im Code oder XAML, steuerelementausrichtung und Ausrichtung. (Eine dieser Eigenschaften, [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position), wird in diesem Artikel nicht behandelt.)

Sie können experimentieren mit den fünf verbleibenden bindbare Eigenschaften, die mit der **experimentieren** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel. Auf dieser Seite können Sie zum Hinzufügen oder entfernen die untergeordneten Elemente von einem `FlexLayout` und Kombinationen aus den fünf bindbaren Eigenschaften festlegen. Alle untergeordneten Elemente der `FlexLayout` sind `Label` Ansichten verschiedener Farben und Größen, mit der `Text` -Eigenschaft auf eine Zahl entsprechend seiner Position in der `Children` Auflistung.

Beim Starten des Programms um fünf `Picker` Ansichten anzeigen, die Standardwerte für diese fünf `FlexLayout` Eigenschaften. Die `FlexLayout` im unteren Bereich des Bildschirms enthält drei untergeordnete Elemente:

[![Die Seite "Experiment": Standard](flex-layout-images/ExperimentDefault.png "Experimentseite - Standard")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

Jede der `Label` Ansichten besitzt einen grauen Hintergrund, die den Speicherplatz auf, zeigt `Label` innerhalb der `FlexLayout`. Der Hintergrund der `FlexLayout` selbst Aliceblau ist. Es nimmt den gesamten unteren Bereich der Seite mit Ausnahme von einer kleinen Rand auf die Links und rechts.

<a name="direction" />

### <a name="the-direction-property"></a>Die Direction-Eigenschaft

Die [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) Eigenschaft ist vom Typ [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), eine Enumeration mit vier Mitglieder:

- `Column`
- `ColumnReverse` (oder "Spalte-rückwärts" in XAML)
- `Row`, der Standardwert
- `RowReverse` (oder "Zeile rückgängig gemacht" in XAML)

In XAML können Sie geben den Wert dieser Eigenschaft, die den Elementnamen Enumeration in Kleinbuchstaben, Großbuchstaben, mit gemischter Groß-/Kleinschreibung, oder Sie auch zwei zusätzliche Zeichenfolgen, die in Klammern, die identisch mit der CSS-Indikatoren werden angezeigt. (Die Zeichenfolgen "Spalte-Reverse" und "Zeile rückgängig gemacht" werden definiert, der [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) Klasse, die vom XAML-Parser verwendet.)

Hier ist die **Experiment** -Seite (von links nach rechts), mit der `Row` Richtung `Column` Richtung und `ColumnReverse` Richtung:

[![Die Seite "Experiment": Richtung](flex-layout-images/ExperimentDirection.png "Experimentseite - Richtung")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

Beachten Sie, dass für die `Reverse` Startoptionen, die Elemente am rechten oder unteren.

<a name="wrap" />

### <a name="the-wrap-property"></a>Der Wrap-Eigenschaft

Die [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) Eigenschaft ist vom Typ [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), eine Enumeration mit drei Elementen:

- `NoWrap`, der Standardwert
- `Wrap`
- `Reverse` (oder "Wrap-rückwärts" in XAML)

Von links nach rechts, diese Seiten anzeigen. dem `NoWrap`, `Wrap` und `Reverse` Optionen für 12 untergeordnete Elemente:

[![Die Seite "Experiment": Umschließen](flex-layout-images/ExperimentWrap.png "umschließen Sie die Seite \"Experiment\" -")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

Wenn die `Wrap` -Eigenschaftensatz auf `NoWrap` Hauptachse beschränkt (wie in diesem Programm) und die Hauptachse ist nicht breit oder groß genug, um alle untergeordneten Elemente, passen die `FlexLayout` versucht, die Elemente zu erstellen, die kleiner als der iOS-Screenshot veranschaulicht. Sie können steuern, die Shrinkness Elemente mit den [ `Shrink` ](#shrink) bindbare Eigenschaft angefügt.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>Die JustifyContent-Eigenschaft

Die [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) Eigenschaft ist vom Typ [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), eine Enumeration mit sechs Elemente:

- `Start` (oder "Flex-Start" in XAML), Standard
- `Center`
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Speicherplatz around" in XAML)
- `SpaceEvenly`

Diese Eigenschaft gibt an, wie die Elemente auf der Hauptachse verteilt sind, die der horizontalen Achse in diesem Beispiel ist:

[![Die Seite "Experiment": Ausrichten von Inhalt](flex-layout-images/ExperimentJustifyContent.png "Experimentseite - rechtfertigen Inhalt")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

In allen drei Screenshots die `Wrap` -Eigenschaftensatz auf `Wrap`. Die `Start` standardmäßig in den vorherigen Screenshot für Android angezeigt wird. Hier der iOS-Screenshot zeigt die `Center` Option: alle Elemente in die Mitte verschoben werden. Drei weitere Optionen ab, mit dem Wort `Space` den zusätzlichen Speicherplatz belegt wird nicht von den Elementen zuzuordnen. `SpaceBetween` Ordnet den gleichmäßig zwischen den Elementen; `SpaceAround` Puts gleich Abstands um jedes Element, während `SpaceEvenly` Puts gleich Leerzeichen zwischen den einzelnen Elementen, und vor dem ersten Element und nach dem letzten Element in der Zeile.

<a name="align-items" />

### <a name="the-alignitems-property"></a>Die AlignItems-Eigenschaft

Die [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) Eigenschaft ist vom Typ [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), eine Enumeration mit vier Mitglieder:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Dies ist eine von zwei Eigenschaften (das andere ist [ `AlignContent` ](#align-content)), der angibt, wie die untergeordneten Elemente auf der querachse ausgerichtet werden. Innerhalb jeder Zeile werden die untergeordneten Elemente gestreckt (wie im vorherigen Screenshot gezeigt) oder auf den Start, zentriert oder Ende jedes Element ausgerichtet werden, wie in den folgenden drei Screenshots gezeigt:

[![Die Seite "Experiment": Ausrichten von Elementen](flex-layout-images/ExperimentAlignItems.png "Experimentseite - ausrichten, Elemente")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

Im Screenshot iOS werden alle untergeordneten Elemente den oberen Bereich ausgerichtet. In den Android-Screenshots werden die Elemente vertikal zentriert basierend auf der höchsten untergeordneten. Im Screenshot UWP werden alle Elemente der unten ausgerichtet.

Für jedes einzelne Element das `AlignItems` Einstellung kann überschrieben werden, mit der [ `AlignSelf` ](#align-self) bindbare Eigenschaft angefügt.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>Die AlignContent-Eigenschaft

Die [ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) Eigenschaft ist vom Typ [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), eine Enumeration mit sieben Elementen:

- `Stretch`, der Standardwert
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)
- `SpaceBetween` (oder "Leerzeichen zwischen" in XAML)
- `SpaceAround` (oder "Speicherplatz around" in XAML)
- `SpaceEvenly`

Wie `AlignItems`, `AlignContent` Eigenschaft auch untergeordnete Elemente auf der querachse entspricht, jedoch wirkt sich auf ganze Zeilen oder Spalten:

[![Die Seite "Experiment": Ausrichten von Inhalt](flex-layout-images/ExperimentAlignContent.png "Experimentseite - Ausrichten von Inhalt")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

Sind in der iOS-Screenshot beide Zeilen am Anfang; in der Android-Screenshot sind sie in der Mitte; und in der UWP-Screenshot sind am unteren Rand. Die Zeilen können auch auf verschiedene Weise verteilt werden:

[![Die Seite "Experiment":  Ausrichten von Inhalt 2](flex-layout-images/ExperimentAlignContent2.png "Experimentseite - ausrichten Content 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

Die `AlignContent` hat keine Auswirkungen, wenn nur eine Zeile oder Spalte vorhanden ist.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>Die angefügten bindbare Eigenschaften im detail

`FlexLayout` definiert fünf angefügte bindbare Eigenschaften. Diese Eigenschaften werden festgelegt, auf die untergeordneten Elemente der `FlexLayout` und beziehen sich nur auf diese bestimmten untergeordneten.

<a name="align-self" />

### <a name="the-alignself-property"></a>Die AlignSelf-Eigenschaft

Die [ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) angefügte bindbare Eigenschaft weist den Typ [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), eine Enumeration mit fünf Elementen:

- `Auto`, der Standardwert
- `Stretch`
- `Center`
- `Start` (oder "Flex-Start" in XAML)
- `End` (oder "Flex-End" in XAML)

Für ein einzelnes untergeordnetes Element der `FlexLayout`, diese Eigenschaft festlegen von Außerkraftsetzungen die [ `AlignItems` ](#align-items) -Eigenschaft festgelegt wird, auf die `FlexLayout` selbst. Die Standardeinstellung von `Auto` bedeutet, dass die `AlignItems` festlegen.

Für eine `Label` Element mit dem Namen `label` (oder Beispiel), Sie können festlegen, die `AlignSelf` Eigenschaft im Code wie folgt:

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

Beachten Sie, dass es ist kein Verweis auf die `FlexLayout` übergeordnete Element der `Label`. In XAML legen Sie die Eigenschaft wie folgt aus:

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Der Order-Eigenschaft

Die [ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) Eigenschaft ist vom Typ `int`. Der Standardwert ist 0.

Die `Order` Eigenschaft können Sie die Reihenfolge zu ändern, die die untergeordneten Elemente der `FlexLayout` angeordnet sind. In der Regel die untergeordneten Elemente ein `FlexLayout` angeordnet sind, entspricht der Reihenfolge, die sie in angezeigt werden die `Children` Auflistung. Sie können diese Reihenfolge außer Kraft setzen, durch Festlegen der `Order` bindbare Eigenschaft auf einen Wert ungleich NULL ganze Zahl auf eine oder mehrere untergeordnete Elemente angefügt. Die `FlexLayout` dann ordnet der untergeordneten Elemente basierend auf der Einstellung der `Order` Eigenschaft für jede untergeordnete, aber die untergeordneten Elemente mit dem gleichen `Order` Einstellung werden in der Reihenfolge angeordnet, die sie in angezeigt werden die `Children` Auflistung.

### <a name="the-basis-property"></a>Die Basis-Eigenschaft

Die [ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) angefügte bindbare Eigenschaft gibt an, die Menge des Speicherplatzes, die ein untergeordnetes Element des zugeordnet wird, die `FlexLayout` auf der Haupt-Achse. Durch die angegebene Größe der `Basis` -Eigenschaft ist die Größe der wichtigsten Achse des übergeordneten Elements `FlexLayout`. Aus diesem Grund `Basis` gibt die Breite eines untergeordneten Elements an, wenn in Zeilen oder die Höhe der untergeordneten Elemente angeordnet werden, wenn die untergeordneten Elemente in Spalten angeordnet werden.

Die `Basis` Eigenschaft ist vom Typ [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), eine Struktur. Die Größe kann in beiden geräteunabhängige Einheiten oder als Prozentsatz der Größe der angegeben werden, wenn die `FlexLayout`. Der Standardwert der `Basis` Eigenschaft ist die statische Eigenschaft `FlexBasis.Auto`, was bedeutet, dass das untergeordnete Element angefordert der Breite oder Höhe wird verwendet.

Sie können im Code Festlegen der `Basis` -Eigenschaft für eine `Label` mit dem Namen `label` auf 40 geräteunabhängige Einheiten wie folgt:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

Das zweite Argument für die `FlexBasis` Konstruktor wird mit dem Namen `isRelative` und gibt an, ob die relative beträgt (`true`) oder absolut (`false`). Das Argument hat den Standardwert `false`, sodass Sie auch den folgenden Code verwenden können:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

Eine implizite Konvertierung von `float` zu `FlexBasis` definiert ist, damit Sie es noch weiter vereinfachen können:

```csharp
FlexLayout.SetBasis(label, 40);
```

Sie können die Größe festlegen, um 25 % der `FlexLayout` übergeordneten wie folgt:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

Dieser anteilige Wert muss im Bereich von 0 bis 1 sein.

In XAML können Sie eine Zahl für eine Größe in geräteunabhängigen Einheiten:

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

Oder Sie können einen Prozentsatz angeben, in dem Bereich von 0 bis 100 %:

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

Die **Basis experimentieren** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel ermöglicht Ihnen das Experimentieren mit der `Basis` Eigenschaft. Die Seite zeigt eine umschlossene Spalte mit maximal fünf `Label` Elemente mit abwechselnden Hintergrund-und Vordergrundfarben. Zwei `Slider` Elemente können Sie angeben, `Basis` Werte für die zweite und vierte `Label`:

[![Die Basis experimentieren Seite](flex-layout-images/BasisExperiment.png "Grundlage experimentieren Seite")](flex-layout-images/BasisExperiment-Large.png#lightbox)

Der iOS-Screenshot auf der linken Seite zeigt die beiden `Label` Elemente wird Höhe in geräteunabhängigen Einheiten angegeben. Der Android-Bildschirm zeigt ihnen wird die Höhe, die einem Bruchteil der Gesamthöhe des angegebenen die `FlexLayout`. Wenn die `Basis` bei 100 % festgelegt ist, und klicken Sie dann das untergeordnete Element die Höhe des ist der `FlexLayout`, und umschließen der nächsten Spalte und die gesamte Höhe der Spalte, belegen, wie der UWP-Screenshot veranschaulicht wird: Es wird angezeigt, die fünf untergeordnete Elemente werden in einer Zeile angeordnet, wobei jedoch sind tatsächlich in fünf Spalten angeordnet.

### <a name="the-grow-property"></a>Die Vergrößerung der Eigenschaft

Die [ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) angefügte bindbare Eigenschaft weist den Typ `int`. Der Standardwert ist 0, und der Wert muss größer als oder gleich 0 sein.

Die `Grow` Eigenschaft spielt eine Rolle bei der `Wrap` -Eigenschaftensatz auf `NoWrap` und die Zeile der untergeordneten Elemente verfügt über eine gesamte Breite kleiner als die Breite des der `FlexLayout`, oder die Spalte der untergeordneten Elemente hat eine kürzere Höhe als die `FlexLayout`. Die `Grow` Eigenschaft gibt an, wie die übrig gebliebenen Platz auf die die untergeordneten Elemente verteilen.

In der **wachsen Experiment** Seite fünf `Label` Elemente unterschiedlichen Farben werden in einer Spalte, und zwei angeordnet `Slider` Elemente können Sie anpassen, die `Grow` Eigenschaft des zweiten und vierten `Label`. Der iOS-Screenshot ganz links zeigt die standardmäßige `Grow` Eigenschaften von 0:

[![Die Seite "Experiment Grow"](flex-layout-images/GrowExperiment.png "Experimentseite erweitern")](flex-layout-images/GrowExperiment-Large.png#lightbox)

Wenn alle ein untergeordnetes Element ein positives Ergebnis angegeben ist `Grow` Wert, und klicken Sie dann dieses den verbleibenden Platz einnimmt, wie in der Android-Screenshot veranschaulicht wird. Dieser Speicherplatz kann auch zwischen zwei oder mehr untergeordnete Elemente zugeordnet werden. In der UWP-Screenshot der `Grow` -Eigenschaft der zweiten `Label` ist auf 0,5 festgelegt, während die `Grow` Eigenschaft des vierten `Label` ist 1.5, wodurch die vierte `Label` dreimal so viel von der übrig gebliebenen Platz als die zweite `Label`.

Wie sich die untergeordnete Ansicht auf diesen Speicherplatz verwendet, hängt von den bestimmten Typ des untergeordneten Elements ab. Für eine `Label`, der Text positioniert werden kann, innerhalb der gesamte Speicherplatz für die `Label` mithilfe der Eigenschaften `HorizontalTextAlignment` und `VerticalTextAlignment`.

<a name="shrink" />

### <a name="the-shrink-property"></a>Die Eigenschaft verkleinern

Die [ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) angefügte bindbare Eigenschaft weist den Typ `int`. Der Standardwert ist 1, und der Wert muss größer als oder gleich 0 sein.

Die `Shrink` Eigenschaft spielt eine Rolle bei der `Wrap` -Eigenschaftensatz auf `NoWrap` und die aggregierte Breite einer Zeile der untergeordneten Elemente ist größer als die Breite des der `FlexLayout`, oder der Höhe von einer einzelnen Spalte der untergeordneten Elemente ist größer als die Höhe der `FlexLayout`. Normalerweise die `FlexLayout` werden diese untergeordneten Elemente von constricting ihre Größen angezeigt. Die `Shrink` Eigenschaft kann darauf hinweisen, welche untergeordneten Aktivitäten "Priorität" in der angezeigt wird, auf ihre vollständige Größe angegeben werden.

Die **verkleinern Experiment** -Seite erstellt eine `FlexLayout` mit einer einzelnen Zeile mit maximal fünf `Label` untergeordnete Elemente, die erfordern mehr Speicherplatz als die `FlexLayout` Breite. Der iOS-Screenshot auf der linken Seite zeigt alle der `Label` Elemente mit dem Standardwert 1:

[![Die verkleinern experimentieren Seite](flex-layout-images/ShrinkExperiment.png "die verkleinern experimentieren Seite")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

In der Android-Screenshot der `Shrink` Wert für die zweite `Label` ist auf 0 festgelegt, sowie, `Label` in der Breite angezeigt wird. Darüber hinaus die vierte `Label` erhält eine `Shrink` -Wert größer als eins, und hat während der Verkleinerung. Der UWP-Screenshot zeigt beide `Label` Elemente angegeben wird ein `Shrink` -Wert von 0 bis sie ihre vollständige Größe angezeigt werden Wenn können möglich ist.

Sie können festlegen, sowohl die `Grow` und `Shrink` Werte, um Situationen zu berücksichtigen, in denen die Größe der aggregierten untergeordneten kleiner als oder sogar mehr als die Größe des eventuell, der `FlexLayout`.

## <a name="css-styling-with-flexlayout"></a>CSS-Stile mit FlexLayout

Sie können die [CSS-Stile](~/xamarin-forms/user-interface/styles/css/index.md) mit Xamarin.Forms 3.0 in Verbindung mit der fortlaufenden Standbyreplikation `FlexLayout`. Die **CSS-Katalogelemente** auf der Seite die **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** Beispiel, um das Layout der Duplikate der **Katalogelemente** Seite jedoch mit einer CSS- das Stylesheet für viele der Stile:

[![Das CSS Katalogseite Elemente](flex-layout-images/CssCatalogItems.png "CSS Katalog Elemente (Seite)")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

Die ursprüngliche **CatalogItemsPage.xaml** Datei verfügt über fünf `Style` Definitionen in der `Resources` Abschnitt mit 15 `Setter` Objekte. In der **CssCatalogItemsPage.xaml** Datei, die auf zwei reduziert wurde `Style` Definitionen mit nur vier `Setter` Objekte. Diese Stile ergänzen die CSS-Stylesheet für Eigenschaften, die das Xamarin.Forms-CSS-Stile Feature derzeit nicht unterstützt wird:

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

CSS-Stylesheet verwiesen wird, in der ersten Zeile von der `Resources` Abschnitt:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

Beachten Sie auch, dass zwei Elemente in jedem der drei Elemente enthalten `StyleClass` Einstellungen:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

Diese beziehen sich auf die Selektoren in die **CatalogItemsStyles.css** Stylesheet:

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

Mehrere `FlexLayout` angefügte bindbare Eigenschaften sind hier verwiesen wird. In der `label.empty` Selektor, sehen Sie die `flex-grow` -Attribut, das eine leere formatiert `Label` zu einer leeren Stelle, die oben genannten der `Button`. Die `image` Auswahl enthält ein `order` Attribut und einem `align-self` -Attribut, beide entsprechen `FlexLayout` bindbare Eigenschaften angefügt.

Sie haben gesehen, dass Sie Eigenschaften festlegen können, direkt auf die `FlexLayout` und Sie können die angefügte bindbare Eigenschaften festlegen, auf die untergeordneten Elemente ein `FlexLayout`. Oder Sie können diese Eigenschaften, die indirekt mit herkömmlichen XAML-basierte Stile oder CSS-Formatvorlagen festlegen. Wichtig ist, vertraut sein, diese Eigenschaften. Diese Eigenschaften sind, macht die `FlexLayout` wirklich flexibel.

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout mit Xamarin.University

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 Flex Layout mit Ausrichtung von [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
