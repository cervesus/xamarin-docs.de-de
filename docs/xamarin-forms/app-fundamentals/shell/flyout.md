---
title: Xamarin.Forms-Shell-Flyout
description: Das Flyout ist das Hauptmenü für eine Shell-Anwendung, und der Zugriff erfolgt über ein Symbol oder durch Wischen von einer Seite des Bildschirms. Das Flyout besteht aus einem optionalen Header, Flyout-Elementen und optionalen Menüelementen.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2019
ms.openlocfilehash: 4049b3bdfdd6077dcfa151df9553722e63def0ba
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303870"
---
# <a name="xamarinforms-shell-flyout"></a>Xamarin.Forms-Shell-Flyout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Das Flyout ist das Hauptmenü für eine Shell-Anwendung, und der Zugriff erfolgt über ein Symbol oder durch Wischen von einer Seite des Bildschirms. Das Flyout besteht aus einem optionalen Header, Flyout-Elementen und optionalen Menüelementen:

![Screenshot: Shell-Flyout mit Anmerkungen](flyout-images/flyout-annotated.png "Flyout mit Anmerkungen")

Falls erforderlich, kann die Hintergrundfarbe des Flyouts über die bindbare Eigenschaft `Shell.FlyoutBackgroundColor` auf eine [`Color`](xref:Xamarin.Forms.Color) festgelegt werden. Diese Eigenschaft kann auch über ein Cascading Stylesheet (CSS) festgelegt werden. Weitere Informationen finden Sie unter [Spezifische Eigenschaften der Xamarin.Forms-Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="flyout-icon"></a>Flyout-Symbol

Standardmäßig verfügen Shell-Anwendungen über ein Hamburger-Symbol, über das das Flyout durch Drücken geöffnet wird. Dieses Symbol kann durch Festlegen der bindbaren Eigenschaft `Shell.FlyoutIcon` vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource) auf ein passendes Symbol geändert werden:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>Flyout-Verhalten

Auf das Flyout kann über das Hamburger-Symbol oder durch Streichen von der Seite des Bildschirms zugegriffen werden. Allerdings kann dieses Verhalten geändert werden, indem die angefügte Eigenschaft `Shell.FlyoutBehavior` auf eines der `FlyoutBehavior`-Enumerationsmember festgelegt wird:

- `Disabled` – Gibt an, dass das Flyout vom Benutzer nicht geöffnet werden kann.
- `Flyout` – Gibt an, dass das Flyout vom Benutzer geöffnet und geschlossen werden kann. Dies ist der Standardwert der `FlyoutBehavior`-Eigenschaft.
- `Locked` – Gibt an, dass das Flyout vom Benutzer nicht geschlossen werden kann und dass es keine Inhalte überlappt.

Im folgenden Beispiel wird die Deaktivierung des Flyouts veranschaulicht:

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> Die angefügte Eigenschaft `FlyoutBehavior` kann auf `Shell`, `FlyoutItem`, `ShellContent` und Seitenobjekte festgelegt werden, um das Standardverhalten des Flyouts zu überschreiben.

Darüber hinaus kann das Flyout programmgesteuert geöffnet und geschlossen werden, indem die bindbare Eigenschaft `Shell.FlyoutIsPresented` auf einen `boolean`-Wert festgelegt wird, der angibt, ob das Flyout aktuell sichtbar ist:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>Flyout-Header

Der Flyout-Header ist der Inhalt, der optional oben im Flyout angezeigt wird, wobei seine Darstellung durch ein `object` definiert wird, das über den Eigenschaftswert `Shell.FlyoutHeader` festgelegt werden kann:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

Der `FlyoutHeader`-Typ wird im folgenden Beispiel gezeigt:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

Dies führt zu folgendem Flyout-Header:

![Screenshot: Flyout-Header](flyout-images/flyout-header.png "Flyout-Header")

Alternativ kann die Darstellung des Flyout-Headers definiert werden, indem die `Shell.FlyoutHeaderTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird:

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

Standardmäßig wird der Flyout-Header im Flyout fixiert, während der Inhalt unten scrollt, wenn genügend Elemente vorhanden sind. Allerdings kann dieses Verhalten geändert werden, indem die bindbare Eigenschaft `Shell.FlyoutHeaderBehavior` auf eines der `FlyoutHeaderBehavior`-Enumerationsmember festgelegt wird:

- `Default` – Gibt an, dass das plattformspezifische Standardverhalten verwendet wird. Dies ist der Standardwert der `FlyoutHeaderBehavior`-Eigenschaft.
- `Fixed` – Gibt an, dass der Flyout-Header jederzeit sichtbar und unverändert bleibt.
- `Scroll` – Gibt an, dass der Flyout-Header beim Durchblättern der Elemente durch den Benutzer aus der Sicht des Benutzers heraus scrollt.
- `CollapseOnScroll` – Gibt an, dass der Flyout-Header beim Durchblättern der Elemente durch den Benutzer zu einem Titel reduziert wird.

Das folgende Beispiel zeigt, wie der Flyout-Header beim Durchblättern der Elemente durch den Benutzer reduziert wird:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-background-image"></a>Flyout-Hintergrundbild

Das Flyout kann über ein optionales Hintergrundbild verfügen, das unterhalb des Flyout-Headers und hinter allen Flyout-Elementen und Menüelementen angezeigt wird. Das Hintergrundbild kann angegeben werden, indem die bindbare `FlyoutBackgroundImage`-Eigenschaft vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource)auf eine Datei, eine eingebettete Ressource, einen URI oder einen Stream festgelegt wird.

Das Seitenverhältnis des Hintergrundbilds kann konfiguriert werden, indem die bindbare `FlyoutBackgroundImageAspect`-Eigenschaft vom Typ [`Aspect`](xref:Xamarin.Forms.Aspect)auf einen der `Aspect`-Enumerationsmember festgelegt wird:

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): Beschneidet das Bild so, dass es den Anzeigebereich ausfüllt und gleichzeitig das Seitenverhältnis beibehalten wird.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): Wendet das Letterboxformat auf das Bild an, wenn erforderlich, damit das Bild in den Anzeigebereich passt, wobei abhängig davon, ob das Bild breit oder hoch ist, Leerraum am oberen bzw. unteren Rand oder seitlich hinzugefügt wird.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): Streckt das Bild so, dass es den Anzeigebereich vollständig und genau ausfüllt. Dies kann zu einer Bildverzerrung führen.

Standardmäßig wird die `FlyoutBackgroundImageAspect`-Eigenschaft auf `AspectFit` festgelegt.

Das folgende Beispiel zeigt, wie diese Eigenschaften festgelegt werden:

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

Dies führt dazu, dass im Flyout ein Hintergrundbild angezeigt wird:

![Screenshot: Flyout-Hintergrundbild](flyout-images/flyout-backgroundimage.png "Flyout-Hintergrundbild")

## <a name="flyout-items"></a>Flyout-Elemente

Wenn das Navigationsmuster für eine Anwendung einen Flyout enthält, muss das `Shell`-Objekt mit Unterklassen ein oder mehrere `FlyoutItem`-Objekte enthalten, wobei jedes `FlyoutItem`-Objekt ein Element auf dem Flyout darstellt. Jedes `FlyoutItem`-Objekt muss ein untergeordnetes Element des `Shell`-Objekts sein.

Das folgende Beispiel erstellt ein Flyout, das einen Flyout-Header und zwei Flyout-Elemente enthält:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

In diesem Beispiel kann jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt nur über Flyout-Elemente aufgerufen werden:

[![Screenshot: Shell-App mit zwei Seiten und Flyout-Elementen unter iOS und Android](flyout-images/two-page-app-flyout.png "Shell-App mit zwei Seiten und Flyout-Elementen")](flyout-images/two-page-app-flyout-large.png#lightbox "Shell-App mit zwei Seiten und Flyout-Elementen")

> [!NOTE]
> Wenn kein Flyout-Header vorhanden ist, werden Flyout-Elemente ganz oben im Flyout angezeigt. Andernfalls werden sie unterhalb des Headers Flyout angezeigt.

Shell verfügt über implizite Konvertierungsoperatoren, die es ermöglichen, die visuelle Hierarchie der Shell zu vereinfachen, ohne zusätzliche Ansichten in die visuelle Struktur einzufügen. Dies ist möglich, weil ein `Shell`-Objekt mit Unterklassen immer nur `FlyoutItem`-Objekte oder ein `TabBar`-Objekt enthalten darf, die immer nur `Tab`-Objekte enthalten dürfen, die wiederum immer nur `ShellContent`-Objekte enthalten dürfen. Diese impliziten Konvertierungsoperatoren können verwendet werden, um die Objekte `FlyoutItem`, `Tab` und `ShellContent` aus dem vorherigen Beispiel zu entfernen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

Diese implizite Konvertierung umschließt jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt automatisch in `ShellContent`-Objekten, die in `Tab`-Objekte umschlossen sind, die in `FlyoutItem`-Objekte umschlossen sind.

> [!IMPORTANT]
> In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein untergeordnetes Element eines `ShellContent`-Objekts ist, während des Anwendungsstarts erstellt. Hinzufügen weiterer `ShellContent`-Objekte mit diesem Ansatz führt dazu, dass zusätzliche Seiten während des Anwendungsstarts erstellt werden, was zu einer schlechten Starterfahrung führen kann. Mit der Shell können Seiten jedoch auch nach Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Effizientes Laden von Seiten](tabs.md#efficient-page-loading) im Handbuch über die [Registerkarten der Xamarin.Forms-Shell](tabs.md).

### <a name="flyoutitem-class"></a>FlyoutItem-Klasse

Die `FlyoutItem`-Klasse umfasst die folgenden Eigenschaften, die die Darstellung und das Verhalten des Flyoutelements steuern:

- `FlyoutDisplayOptions` vom Typ `FlyoutDisplayOptions`: Definiert, wie das Element und seine untergeordneten Elemente im Flyout angezeigt werden. Der Standardwert ist `AsSingleItem`.
- `CurrentItem` vom Typ `Tab`: das ausgewählte Element.
- `Items` vom Typ `IList<Tab>`: Definiert alle Registerkarten in einem `FlyoutItem`-Objekt.
- `FlyoutIcon` vom Typ `ImageSource`: das für das Element verwendete Symbol. Wenn diese Eigenschaft nicht festgelegt ist, wird der `Icon`-Eigenschaftswert verwendet.
- `Icon` vom Typ `ImageSource`: Definiert das Symbol, das in Teilen des Chroms angezeigt wird, die nicht das Flyout sind.
- `IsChecked` vom Typ `boolean`: Definiert, ob das Element im Flyout derzeit ausgewählt ist.
- `IsEnabled` vom Typ `boolean`: Definiert, ob das Element im Chrom ausgewählt werden kann.
- `IsTabStop` vom Typ `bool`: Gibt an, ob ein `FlyoutItem`-Objekt in der Tabstoppnavigation enthalten ist. Der Standardwert ist `true`, und wenn er `false` ist, wird das `FlyoutItem` von der Infrastruktur der Tabstoppnavigation ignoriert, unabhängig davon, ob ein `TabIndex` festgelegt ist.
- `TabIndex` vom Typ `int`: Gibt an, in welcher Reihenfolge die `FlyoutItem`-Objekte den Fokus erhalten, wenn der Benutzer durch Drücken der TAB-TASTE durch Elemente navigiert. Der Standardwert der Eigenschaft ist 0.
- `Title` vom Typ `string`: der Titel, der in der Benutzeroberfläche angezeigt wird.
- `Route` vom Typ `string`: die Zeichenfolge, mit der das Element adressiert wird.

Alle diese Eigenschaften, mit Ausnahme der `Route`-Eigenschaft, werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Datenverbindungen sein können.

> [!NOTE]
> Alle `FlyoutItem`-Objekte in einem Shell-Objekt mit Unterklassen werden zur `Shell.Items`-Sammlung hinzugefügt, die die Liste der im Flyout angezeigten Elemente definiert.

Darüber hinaus stellt die `FlyoutItem`-Klasse die folgenden überschreibbaren Methoden zur Verfügung:

- `OnTabIndexPropertyChanged`: Wird aufgerufen, wenn sich die `TabIndex`-Eigenschaft ändert.
- `OnTabStopPropertyChanged`: Wird aufgerufen, wenn sich die `IsTabStop`-Eigenschaft ändert.
- `TabIndexDefaultValueCreator`: Gibt einen `int`-Wert zurück und wird aufgerufen, um den Standardwert der `TabIndex`-Eigenschaft festzulegen.
- `TabStopDefaultValueCreator`: Gibt einen `bool`-Wert zurück und wird aufgerufen, um den Standardwert der `TabStop`-Eigenschaft festzulegen.

## <a name="flyout-vertical-scroll"></a>Flyout mit vertikalem Scrollen

Standardmäßig kann ein Flyout vertikal gescrollt werden, wenn die Flyoutelemente nicht in das Flyout passen. Dieses Verhalten kann geändert werden, indem die bindbare Eigenschaft `Shell.FlyoutVerticalScrollMode` auf eines der `ScrollMode`-Enumerationsmember festgelegt wird:

- `Disabled`: Gibt an, dass das vertikale Scrollen deaktiviert wird.
- `Enabled`: Gibt an, dass das vertikale Scrollen aktiviert wird.
- `Auto`: Gibt an, dass das vertikale Scrollen aktiviert wird, wenn die Flyoutelemente nicht in das Flyout passen. Dies ist der Standardwert der `Shell.FlyoutVerticalScrollMode`-Eigenschaft.

Im folgenden Beispiel wird gezeigt, wie Sie das vertikale Scrollen deaktivieren:

```xaml
<Shell ...
       FlyoutVerticalScrollMode="Disabled"
    ...
</Shell>
```

## <a name="flyout-display-options"></a>Anzeigeoptionen für Flyouts

Die `FlyoutDisplayOptions`-Enumeration definiert die folgenden Member:

- `AsSingleItem`: Gibt an, dass das Element als ein einzelnes Element angezeigt wird.
- `AsMultipleItems`: Gibt an, dass das Element und seine untergeordneten Elemente im Flyout als eine Gruppe von Elementen angezeigt werden.

Durch Festlegen der `FlyoutItem.FlyoutDisplayOptions`-Eigenschaft auf `AsMultipleItems` wird ein Flyout-Element für jedes `Tab` innerhalb eines `FlyoutItem`-Objekts erstellt:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

In diesem Beispiel werden Flyout-Elemente für das `Tab`-Objekt, das ein untergeordnetes Element des `FlyoutItem`-Objekts ist, und die `ShellContent`-Objekte, die untergeordnete Elemente des `FlyoutItem`-Objekts sind, erstellt. Grund hierfür ist, dass jedes `ShellContent`-Objekt, das ein untergeordnetes Element des `FlyoutItem`-Objekts ist, automatisch in ein `Tab`-Objekt umschlossen wird. Darüber hinaus wird ein Flyout-Element für das endgültige `ShellContent`-Objekt erstellt, das in ein `Tab`-Objekt und dann in ein `FlyoutItem`-Objekt umschlossen wird.

Dies führt zu folgenden Flyout-Elementen:

[![Screenshot: Flyout mit FlyoutItem-Objekten unter iOS und Android](flyout-images/flyout-reduced.png "Shell-Flyout mit FlyoutItem-Objekten")](flyout-images/flyout-reduced-large.png#lightbox "Shell-Flyout mit FlyoutItem-Objekten")

## <a name="define-flyoutitem-appearance"></a>Definieren der FlyoutItem-Darstellung

Die Darstellung jedes `FlyoutItem` kann angepasst werden, indem die angefügte `Shell.ItemTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird:

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding FlyoutIcon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

Dieses Beispiel stellt die Titel der einzelnen `FlyoutItem`-Objekte in Kursivschrift dar:

[![Screenshot: FlyoutItem-Objekte als Vorlage unter iOS und Android](flyout-images/flyoutitem-templated.png "FlyoutItem-Objekte als Shell-Vorlagen")](flyout-images/flyoutitem-templated-large.png#lightbox "FlyoutItem-Objekte als Shell-Vorlagen")


Da `Shell.ItemTemplate` eine angefügte Eigenschaft ist, können verschiedene Vorlagen an bestimmte `FlyoutItem`-Objekte angefügt werden.

> [!NOTE]
> Die Shell stellt die Eigenschaften `Title` und `FlyoutIcon` für den [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) der `ItemTemplate` zur Verfügung.


### <a name="default-template-for-flyoutitems-and-menuitems"></a>Standardvorlage für FlyoutItems und MenuItems
Shell verwendet intern die folgende Vorlage für die Standardimplementierung. Wenn Sie lediglich kleine Änderungen an den vorhandenen Layouts vornehmen, ist dies ein hervorragender Ausgangspunkt. Dies veranschaulicht auch die Features des Visual State-Managers für Flyout-Elemente. Die gleiche Vorlage kann auch für MenuItems verwendet werden.

```xaml
<DataTemplate x:Key="FlyoutTemplates">
    <Grid HeightRequest="{x:OnPlatform Android=50}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                    </VisualState>
                    <VisualState x:Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="#F2F2F2" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="{x:OnPlatform Android=54, iOS=50}"></ColumnDefinition>
            <ColumnDefinition Width="*"></ColumnDefinition>
        </Grid.ColumnDefinitions>
        <Image Source="{Binding FlyoutIcon}"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            HeightRequest="{x:OnPlatform Android=24, iOS=22}"
            WidthRequest="{x:OnPlatform Android=24, iOS=22}">
        </Image>
        <Label VerticalOptions="Center"
                Text="{Binding Title}"
                FontSize="{x:OnPlatform Android=14, iOS=Small}"
                FontAttributes="Bold" Grid.Column="1">
            <Label.TextColor>
                <OnPlatform x:TypeArguments="Color">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="#D2000000" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.TextColor>
            <Label.Margin>
                <OnPlatform x:TypeArguments="Thickness">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="20, 0, 0, 0" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.Margin>
            <Label.FontFamily>
                <OnPlatform x:TypeArguments="x:String">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="sans-serif-medium" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.FontFamily>
        </Label>
    </Grid>
</DataTemplate>
```

## <a name="flyoutitem-tab-order"></a>FlyoutItem-Aktivierreihenfolge

Standardmäßig entspricht die Aktivierreihenfolge von `FlyoutItem`-Objekten der gleichen Reihenfolge, in der sie in XAML aufgelistet sind, oder in der sie programmgesteuert einer untergeordneten Sammlung hinzugefügt wurden. Diese Reihenfolge ist die Reihenfolge, in der die `FlyoutItem`-Objekte mithilfe einer Tastatur navigiert werden, und oft ist diese Standardreihenfolge die beste Reihenfolge.

Die Standardaktivierreihenfolge kann durch Festlegen der `FlyoutItem.TabIndex`-Eigenschaft geändert werden, die angibt, in welcher Reihenfolge die `FlyoutItem`-Objekte den Fokus erhalten, wenn der Benutzer durch Drücken der TAB-TASTE durch Elemente navigiert. Der Standardwert der Eigenschaft ist 0. Er kann auf jeden beliebigen `int`-Wert festgelegt werden.

Die folgenden Regeln gelten, wenn die Standardaktivierreihenfolge verwendet oder die `TabIndex`-Eigenschaft festgelegt wird:

- `FlyoutItem`-Objekte mit einem `TabIndex` gleich 0 werden, basierend auf ihrer Deklarationsreihenfolge in XMAL oder untergeordneten Sammlungen, der Aktivierreihenfolge hinzugefügt.
- `FlyoutItem`-Objekte mit einem `TabIndex` größer 0 werden, basierend auf ihrem `TabIndex`-Wert, der Aktivierreihenfolge hinzugefügt.
- `FlyoutItem`-Objekte mit einem `TabIndex` kleiner 0 werden der Aktivierreihenfolge hinzugefügt und werden vor einem NULL-Wert angezeigt.
- Konflikte bei einem `TabIndex` werden durch Deklaration der Reihenfolge aufgelöst.

Nach dem Definieren einer Aktivierreihenfolge wird durch das Drücken der TAB-TASTE eine Fokusschleife aktiviert, die `FlyoutItem`-Objekte in aufsteigender `TabIndex`-Reihenfolge durchläuft und von vorne beginnt, sobald das letzte Objekt erreicht ist.

Zusätzlich zum Festlegen der Aktivierreihenfolge von `FlyoutItem`-Objekten kann es notwendig sein, einige Objekte aus der Aktivierreihenfolge auszuschließen. Dies kann mit der `FlyoutItem.IsTabStop`-Eigenschaft erreicht werden, die angibt, ob ein `FlyoutItem` in der Tabstoppnavigation eingeschlossen ist. Der Standardwert ist `true`, und wenn er `false` ist, wird das `FlyoutItem` von der Infrastruktur der Tabstoppnavigation ignoriert, unabhängig davon, ob ein `TabIndex` festgelegt ist.

## <a name="set-the-current-flyoutitem"></a>Festlegen des aktuellen FlyoutItem-Objekts

Die `Shell`-Klasse verfügt über eine bindbare Eigenschaft mit dem Namen `CurrentItem` vom Typ `FlyoutItem`, die das derzeit ausgewählte `FlyoutItem`-Objekt darstellt. Wenn eine Shell-Anwendung zum ersten Mal ausgeführt wird, wird diese Eigenschaft auf das erste `FlyoutItem`-Objekt des über Unterklassen verfügenden `Shell`-Objekts festgelegt. Allerdings kann die Eigenschaft wie im folgenden Beispiel gezeigt, auf ein anderes `FlyoutItem`-Objekt festgelegt werden:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

Dieser Code legt das `ShellContent`-Objekt namens `aboutItem` als `CurrentItem`-Eigenschaft fest, wodurch es angezeigt wird. In diesem Beispiel wird eine implizite Konvertierung zum Umschließen des `ShellContent`-Objekts in ein `Tab`-Objekt verwendet, das in ein `FlyoutItem`-Objekt umschlossen ist.

Der entsprechende C#-Code lautet:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>Menüelemente

Menüelemente können optional zum Flyout hinzugefügt werden, und jedes Menüelement wird durch ein [`MenuItem`](xref:Xamarin.Forms.MenuItem)-Objekt dargestellt. Die Position von `MenuItem`-Objekten auf dem Flyout hängt von der Deklarationsreihenfolge in der visuellen Shell-Hierarchie ab. Daher werden alle `MenuItem`-Objekte, die vor `FlyoutItem`-Objekten deklariert wurden, oben im Flyout, und alle `MenuItem`-Objekte, die nach `FlyoutItem`-Objekten deklariert wurden, unten im Flyout angezeigt.

> [!NOTE]
> Die `MenuItem`-Klasse verfügt über ein [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked)-Ereignis und eine [`Command`](xref:Xamarin.Forms.MenuItem.Command)-Eigenschaft. Daher ermöglichen `MenuItem`-Objekte Szenarien, die eine Aktion als Reaktion auf das Antippen eines `MenuItem`-Objekts ausführen. Diese Szenarien sind beispielsweise das Navigieren und das Öffnen eines Webbrowsers auf einer bestimmten Webseite.

[`MenuItem`](xref:Xamarin.Forms.MenuItem)-Objekte können wie im folgenden Beispiel gezeigt dem Flyout hinzugefügt werden:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

Dieser Code fügt zwei unter allen Flyout-Elementen [`MenuItem`](xref:Xamarin.Forms.MenuItem)-Objekte zum Flyout hinzu:

[![Screenshot: Flyout mit MenuItem-Objekten unter iOS und Android](flyout-images/flyout.png "Shell-Flyout mit MenuItem-Objekten")](flyout-images/flyout-large.png#lightbox "Shell-Flyout mit MenuItem-Objekten")

Das erste [`MenuItem`](xref:Xamarin.Forms.MenuItem)-Objekt führt einen `ICommand` namens `RandomPageCommand` aus, der zu einer zufälligen Seite in der Anwendung navigiert. Das zweite `MenuItem`-Objekt führt einen `ICommand` namens `HelpCommand` aus, der die durch die Eigenschaft `CommandParameter` angegebene URL in einem Webbrowser öffnet.

> [!NOTE]
> Das [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) jedes `MenuItem`-Objekts wird von dem über Unterklassen verfügende `Shell`-Objekt geerbt.

## <a name="define-menuitem-appearance"></a>Definieren der Darstellung des MenuItem-Objekts

Die Darstellung jedes `MenuItem` kann angepasst werden, indem die angefügte `Shell.MenuItemTemplate`-Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) festgelegt wird:

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

In diesem Beispiel wird jedem `MenuItem`-Objekt die `MenuItemTemplate` auf Shell-Ebene hinzugefügt, wodurch die Titel der einzelnen `MenuItem`-Objekte in Kursivschrift angezeigt werden:

[![Screenshot: MenuItem-Objekte als Vorlage unter iOS und Android](flyout-images/menuitem-templated.png "MenuItem-Objekte als Shell-Vorlagen")](flyout-images/menuitem-templated-large.png#lightbox "MenuItem-Objekte als Shell-Vorlagen")

> [!NOTE]
> Die Shell stellt die Eigenschaften [`Text`](xref:Xamarin.Forms.MenuItem.Text) und [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) für die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Klasse von `MenuItemTemplate` zur Verfügung. Sie können auch `Title` anstelle von `Text` und `Icon` anstelle von `IconImageSource` verwenden. Dadurch können Sie dieselbe Vorlage für Menüelemente und Flyout-Elemente wiederverwenden.

Da `Shell.MenuItemTemplate` eine angefügte Eigenschaft ist, können verschiedene Vorlagen an bestimmte `MenuItem`-Objekte angefügt werden:

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              Icon="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell">
        <Shell.MenuItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </Shell.MenuItemTemplate>
    </MenuItem>
</Shell>
```


> [!NOTE]
> Sie können dieselbe Vorlage, die Sie für [Flyout-Elemente](#default-template-for-flyoutitems-and-menuitems) verwendet haben, auch für Menüelemente verwenden.

In diesem Beispiel wird die `MenuItemTemplate` der Shell-Ebene an das erste `MenuItem`-Objekt und die Inline-`MenuItemTemplate` an das zweite `MenuItem`-Objekt angefügt.

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
