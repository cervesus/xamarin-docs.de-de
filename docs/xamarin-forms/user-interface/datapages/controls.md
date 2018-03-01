---
title: DataPages Steuerelemente-Referenz
ms.topic: article
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: fbbc7e8c93f8ed562381d9203060c862960f44b9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="datapages-controls-reference"></a>DataPages Steuerelemente-Referenz

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschau verfügbar")

> [!IMPORTANT]
> DataPages erfordert eine [Xamarin.Forms Design](~/xamarin-forms/user-interface/themes/index.md) Verweis auf das Rendern.


Der Xamarin.Forms-DataPages Nuget umfasst eine Reihe von Kontrollmechanismen, die datenquellenbindung nutzen können.

Um diese Steuerelemente nicht in XAML verwenden, stellen Sie sicher, der Namespace eingefügt wurde, z. B. finden Sie unter der `xmlns:pages` folgende Deklaration:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Die folgenden Beispiele enthalten `DynamicResource` Verweise an, die in das Projekt Ressourcen Wörterbuch funktioniert vorhanden sein müsste. Es gibt auch ein Beispiel zum Erstellen einer [benutzerdefiniertes Steuerelement](#custom)

## <a name="built-in-controls"></a>Integrierte Steuerelemente

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

Die `HeroImage` Steuerelement verfügt über vier Eigenschaften:

* Text
* Detail
* ImageSource
* Aspect

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "HeroImage Steuerelement unter Android") ![ ] (controls-images/heroimage-dark-android.png "HeroImage Steuerelement unter Android")

**iOS**

![](controls-images/heroimage-light-ios.png "HeroImage-Steuerelement auf iOS") ![ ] (controls-images/heroimage-dark-ios.png "HeroImage-Steuerelement für iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

Die `ListItem` Layout des Steuerelements ähnelt dem systemeigenen iOS und Android-Liste oder Tabelle Zeilen, jedoch auch als reguläre Ansicht verwendet werden können. Im Beispiel wird der Code, darunter gehostet gezeigt eine `StackLayout`, aber es kann auch in datengebundenen Scolling List-Steuerelemente verwendet werden.

Es gibt fünf Eigenschaften:

* Titel
* Detail
* ImageSource
* PlaceholdImageSource
* Aspect

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Diese Screenshots zeigen die `ListItem` auf IOS- und Android-Plattformen verwenden sowohl der hellen und dunklen Design:

**Android**

![](controls-images/listitem-light-android.png "ListItem-Steuerelement unter Android") ![ ] (controls-images/listitem-dark-android.png ""ListItem"-Steuerelement unter Android")

**iOS**

![](controls-images/listitem-light-ios.png "ListItem-Steuerelement auf iOS") ![ ] (controls-images/listitem-dark-ios.png "ListItem-Steuerelement für iOS")


## <a name="custom-control-example"></a>Beispiel für ein benutzerdefiniertes Steuerelement

Das Ziel von diesem benutzerdefinierten `CardView` Steuerelement ist, damit die systemeigene Android CardView ähneln.

Es werden drei Eigenschaften enthalten:

* Text
* Detail
* ImageSource

Das Ziel ist ein benutzerdefiniertes Steuerelement, das den folgenden Code aussieht (Beachten Sie, dass eine benutzerdefinierte `xmlns:local` ist erforderlich, die auf die aktuelle Assembly verweist):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Sie sollten die Screenshots unter Verwendung von Farben für die integrierte hellen und dunklen Design formuliert werden:

**Android**

![](controls-images/cardview-light-android.png "CardView benutzerdefinierte Steuerelement im Android") ![ ] (controls-images/cardview-dark-android.png "CardView-benutzerdefiniertes Steuerelement auf Android-Geräten")

**iOS**

![](controls-images/cardview-light-ios.png "CardView benutzerdefiniertes Steuerelement auf iOS") ![ ] (controls-images/cardview-dark-ios.png "CardView benutzerdefiniertes Steuerelement auf iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Erstellen von benutzerdefinierten CardView

1. [DataView-Unterklasse.](#1)
2. [Definieren Sie Schriftart, Layout und Ränder](#2)
3. [Erstellen Sie Stile für das Steuerelement untergeordnete Elemente.](#3)
4. [Erstellen Sie das Layout-Steuerelementvorlage](#4)
5. [Fügen Sie der Design-spezifischen Ressourcen hinzu](#5)
6. [Legen Sie das ControlTemplate für die CardView-Klasse](#6)
7. [Fügen Sie das Steuerelement zu einer Seite](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. DataView-Unterklasse.

Die C#-Unterklasse für `DataView` definiert die bindungsfähigen Eigenschaften für das Steuerelement.

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

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Definieren Sie Schriftart, Layout und Ränder

Der Steuerelement-Designer würde diese Werte als Teil des Entwurfs Benutzeroberfläche für das benutzerdefinierte Steuerelement herauszufinden. Clientplattform-spezifische Spezifikationen erforderlich ist, sind die `OnPlatform` Element verwendet wird.

Beachten Sie, die auf einige Werte verweisen `StaticResource`s – Diese definiert werden [Schritt 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewBackgroundColor">
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

<OnPlatform x:TypeArguments="Color" x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Erstellen Sie Stile für das Steuerelement untergeordnete Elemente.

Einen Verweis auf die Elemente, die um zu erstellenden die untergeordneten Elemente, die verwendet werden, in das benutzerdefinierte Steuerelement definiert:

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

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Erstellen Sie das Layout-Steuerelementvorlage

Der visuelle Entwurf des benutzerdefinierten Steuerelements wird in der Vorlage für ein Steuerelement mithilfe der oben definierten Ressourcen explizit deklariert:

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

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Fügen Sie der Design-spezifischen Ressourcen hinzu

Da dies ein benutzerdefiniertes Steuerelement ist, fügen Sie die Ressourcen, die das Design, das Sie verwenden das Ressourcenwörterbuch entsprechen:

##### <a name="light-theme-colors"></a>Farben Design "hell"

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Farben Design "dunkel"

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Legen Sie das ControlTemplate für die CardView-Klasse

Stellen Sie schließlich sicher erstellte C#-Klasse. [Schritt 1](#1) verwendet die Steuerelementvorlage in definierten [Schritt 4](#4) mithilfe einer `Style` `Setter` Element

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Fügen Sie das Steuerelement zu einer Seite

Die `CardView` Steuerelement kann jetzt auf eine Seite hinzugefügt werden. Das folgende Beispiel zeigt es in gehosteten eine `StackLayout`:

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
