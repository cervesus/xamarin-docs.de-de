---
Title: "DataPages-Steuerelement Verweis" Beschreibung: "in diesem Artikel werden die Steuerelemente vorgestellt, die im Xamarin.Forms DataPages-nuget-Paket verfügbar sind."
ms. Prod: xamarin ms. assetid: 891615d0-e8bd-4acc-a7f 0-4c3725bcc31 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 12/01/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="datapages-controls-reference"></a>Verweis auf DataPages-Steuerelemente

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages erfordert einen Design Xamarin.Forms Verweis zum Rendering. Dies umfasst die Installation von [ Xamarin.Forms . Design. basieren](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) Sie auf das nuget-Paket in Ihrem Projekt, gefolgt von dem [ Xamarin.Forms . Design. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) oder [ Xamarin.Forms . Design. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) nuget-Pakete.

Die Xamarin.Forms DataPages nuget enthält eine Reihe von Steuerelementen, die die Datenquellen Bindung nutzen können.

Um diese Steuerelemente in XAML zu verwenden, stellen Sie sicher, dass der Namespace enthalten ist, z. b. in der `xmlns:pages` folgenden Deklaration:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

In den folgenden Beispielen sind Verweise enthalten, `DynamicResource` die im Ressourcen Wörterbuch des Projekts vorhanden sein müssen, um funktionieren zu können. Außerdem gibt es ein Beispiel für das Erstellen eines [benutzerdefinierten Steuer](#custom-control-example)Elements.

## <a name="built-in-controls"></a>Integrierte Steuerelemente

* [Heroimage](#heroimage)
* [ListItem](#listitem)

### <a name="heroimage"></a>Heroimage

Das- `HeroImage` Steuerelement verfügt über vier Eigenschaften:

* Text
* Detail
* ImageSource
* Aspekt

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Heroimage-Steuerelement unter Android") ![](controls-images/heroimage-dark-android.png "Heroimage-Steuerelement unter Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Heroimage-Steuerelement unter IOS") ![](controls-images/heroimage-dark-ios.png "Heroimage-Steuerelement unter IOS")

### <a name="listitem"></a>ListItem

Das `ListItem` Layout des Steuer Elements ähnelt der systemeigenen IOS-und Android-Liste oder Tabellenzeilen, kann jedoch auch als reguläre Ansicht verwendet werden. Im unten aufgeführten Beispielcode wird in einem gehostet angezeigt `StackLayout` , kann aber auch in Daten gebundenen scolte-Listen Steuerelementen verwendet werden.

Es gibt fünf Eigenschaften:

* Titel
* Detail
* ImageSource
* Placeholdimagesource
* Aspekt

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Diese Screenshots zeigen die `ListItem` auf IOS-und Android-Plattformen, die sowohl das helle als auch das dunkle Design verwenden:

**Android**

![](controls-images/listitem-light-android.png "ListItem-Steuerelement unter Android") ![](controls-images/listitem-dark-android.png "ListItem-Steuerelement unter Android")

**iOS**

![](controls-images/listitem-light-ios.png "ListItem-Steuerelement unter IOS") ![](controls-images/listitem-dark-ios.png "ListItem-Steuerelement unter IOS")

## <a name="custom-control-example"></a>Beispiel für einen benutzerdefinierten

Das Ziel dieses benutzerdefinierten `CardView` Steuer Elements ist, der nativen Android-Kartenansicht zu ähneln.

Sie enthält drei Eigenschaften:

* Text
* Detail
* ImageSource

Das Ziel ist ein benutzerdefiniertes Steuerelement, das wie der folgende Code aussieht (Beachten Sie, dass ein benutzerdefiniertes `xmlns:local` erforderlich ist, das auf die aktuelle Assembly verweist):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Es sollte wie in den nachfolgenden Screenshots aussehen, indem Farben verwendet werden, die den integrierten hellen und dunklen Designs entsprechen:

**Android**

![](controls-images/cardview-light-android.png "Benutzerdefiniertes Kartenansicht-Steuerelement unter Android") ![](controls-images/cardview-dark-android.png "Benutzerdefiniertes Kartenansicht-Steuerelement unter Android")

**iOS**

![](controls-images/cardview-light-ios.png "Benutzerdefiniertes Kartenansicht-Steuerelement unter IOS") ![](controls-images/cardview-dark-ios.png "Benutzerdefiniertes Kartenansicht-Steuerelement unter IOS")

### <a name="building-the-custom-cardview"></a>Aufbauen der benutzerdefinierten CardView

1. [DataView-Unterklasse](#1-dataview-subclass)
2. [Schriftart, Layout und Ränder definieren](#2-define-font-layout-and-margins)
3. [Erstellen von Stilen für die untergeordneten Elemente des Steuer Elements](#3-create-styles-for-the-controls-children)
4. [Erstellen der Steuerelement Layout-Vorlage](#4-create-the-control-layout-template)
5. [Hinzufügen der Design spezifischen Ressourcen](#5-add-the-theme-specific-resources)
6. [Festlegen von "ControlTemplate" für die CardView-Klasse](#6-set-the-controltemplate-for-the-cardview-class)
7. [Hinzufügen des Steuer Elements zu einer Seite](#7-add-the-control-to-a-page)

#### <a name="1-dataview-subclass"></a>1. DataView-Unterklasse

Die c#-Unterklasse von `DataView` definiert die bindbaren Eigenschaften für das Steuerelement.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

#### <a name="2-define-font-layout-and-margins"></a>2. Definieren von Schriftart, Layout und Rändern

Der Steuerelement-Designer würde diese Werte als Teil des Benutzeroberflächen Entwurfs für das benutzerdefinierte Steuerelement herausfinden. Wenn plattformspezifische Spezifikationen erforderlich sind, wird das- `OnPlatform` Element verwendet.

Beachten Sie, dass einige Werte auf `StaticResource` s verweisen – diese werden in [Schritt 5](#5-add-the-theme-specific-resources)definiert.

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

#### <a name="3-create-styles-for-the-controls-children"></a>3. Erstellen Sie Stile für die untergeordneten Elemente des Steuer Elements.

Verweisen auf alle Elemente, die für definiert sind, um die untergeordneten Elemente zu erstellen, die im benutzerdefinierten Steuerelement verwendet werden:

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

#### <a name="4-create-the-control-layout-template"></a>4. Erstellen der Steuerelement Layout-Vorlage

Das visuelle Design des benutzerdefinierten Steuer Elements wird mithilfe der oben definierten Ressourcen explizit in der Steuerelement Vorlage deklariert:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />

    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

#### <a name="5-add-the-theme-specific-resources"></a>5. Hinzufügen der Design spezifischen Ressourcen

Da es sich um ein benutzerdefiniertes Steuerelement handelt, fügen Sie die Ressourcen hinzu, die dem verwendeten Design entsprechen:

##### <a name="light-theme-colors"></a>Helle Design Farben

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Farben des dunklen Designs

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Festlegen von "ControlTemplate" für die CardView-Klasse

Stellen Sie schließlich sicher, dass die in [Schritt 1](#1-dataview-subclass) erstellte c#-Klasse die in [Schritt 4](#4-create-the-control-layout-template) definierte Steuerelement Vorlage mithilfe eines-Elements verwendet. `Style` `Setter`

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

#### <a name="7-add-the-control-to-a-page"></a>7. Hinzufügen des Steuer Elements zu einer Seite

Das `CardView` Steuerelement kann nun zu einer Seite hinzugefügt werden. Das folgende Beispiel zeigt, wie es in einem gehostet wird `StackLayout` :

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
