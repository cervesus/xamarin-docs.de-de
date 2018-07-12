---
title: Steuerelementreferenz DataPages
description: Dieser Artikel enthält die Steuerelemente, die in Xamarin.Forms DataPages NuGet-Paket verfügbar sind.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38866521"
---
# <a name="datapages-controls-reference"></a>Steuerelementreferenz DataPages

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschauversion")

> [!IMPORTANT]
> DataPages erfordert eine [Xamarin.Forms Design](~/xamarin-forms/user-interface/themes/index.md) Verweis auf das Rendern.


Das Xamarin.Forms-DataPages Nuget umfasst eine Reihe von Steuerelementen, die datenquellenbindung nutzen können.

Um diese Steuerelemente in XAML verwenden zu können, stellen Sie sicher, den Namespace eingefügt wurde, z. B. die `xmlns:pages` folgende Deklaration:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Die folgenden Beispiele enthalten `DynamicResource` Verweise im Ressourcen-Wörterbuch funktioniert des Projekts vorhanden sein muss. Es gibt auch ein Beispiel zum Erstellen einer [benutzerdefiniertes Steuerelement](#custom)

## <a name="built-in-controls"></a>Integrierte Steuerelemente

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

Die `HeroImage` Steuerelement verfügt über vier Eigenschaften:

* Text
* Detail
* "ImageSource"
* Aspekt

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "HeroImage-Steuerelement für Android") ![ ] (controls-images/heroimage-dark-android.png "HeroImage-Steuerelement für Android")

**iOS**

![](controls-images/heroimage-light-ios.png "HeroImage-Steuerelement für iOS") ![ ] (controls-images/heroimage-dark-ios.png "HeroImage-Steuerelement für iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

Die `ListItem` Layout des Steuerelements ähnelt natives iOS und Android-Liste oder Tabelle Zeilen, aber es auch als normalen Ansichten verwendet werden kann. Im Beispiel wird darunter Code gehostet gezeigt eine `StackLayout`, aber es kann auch in Listensteuerelementen datengebundenen Scolling verwendet werden.

Es gibt fünf Eigenschaften:

* Titel
* Detail
* "ImageSource"
* PlaceholdImageSource
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

Diese Screenshots zeigen die `ListItem` auf IOS- und Android-Plattformen verwenden sowohl der hellen und dunklen Design:

**Android**

![](controls-images/listitem-light-android.png "ListItem-Steuerelement für Android") ![ ] (controls-images/listitem-dark-android.png "ListItem-Steuerelement für Android")

**iOS**

![](controls-images/listitem-light-ios.png "ListItem-Steuerelement für iOS") ![ ] (controls-images/listitem-dark-ios.png "ListItem-Steuerelement für iOS")


## <a name="custom-control-example"></a>Beispiel für ein benutzerdefiniertes Steuerelement

Das Ziel von diesem benutzerdefinierten `CardView` Steuerelement ist, die den systemeigenen Android CardView-ähnelt.

Es werden drei Eigenschaften enthalten:

* Text
* Detail
* "ImageSource"

Das Ziel ist ein benutzerdefiniertes Steuerelement, das den folgenden Code aussehen (Beachten Sie, dass ein benutzerdefiniertes `xmlns:local` ist erforderlich, die auf die aktuelle Assembly verweist):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Es sollte wie in den Screenshots unter Verwendung von Farben, die für die integrierte hellen und dunklen Design aussehen:

**Android**

![](controls-images/cardview-light-android.png "CardView-benutzerdefinierte Steuerelement für Android") ![ ] (controls-images/cardview-dark-android.png "CardView-benutzerdefinierte Steuerelement für Android")

**iOS**

![](controls-images/cardview-light-ios.png "CardView-benutzerdefiniertes Steuerelement unter iOS") ![ ] (controls-images/cardview-dark-ios.png "CardView-benutzerdefiniertes Steuerelement unter iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Erstellen der benutzerdefinierten CardView-

1. [DataView-Unterklasse.](#1)
2. [Definieren Sie Schriftart, Layout und Rändern](#2)
3. [Erstellen Sie Formate für die untergeordneten Elemente des Steuerelements.](#3)
4. [Erstellen der Layout-Steuerelementvorlage](#4)
5. [Fügen Sie die designspezifischen Ressourcen](#5)
6. [Legen Sie das ControlTemplate für die CardView--Klasse](#6)
7. [Das Steuerelement zu einer Seite hinzufügen](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. DataView-Unterklasse.

Die C#-Unterklasse für `DataView` definiert, das die bindbaren Eigenschaften des Steuerelements.

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

#### <a name="2-define-font-layout-and-margins"></a>2. Definieren Sie Schriftart, Layout und Rändern

Der Steuerelement-Designer würden diese Werte als Teil der Entwurf der Benutzeroberfläche für das benutzerdefinierte Steuerelement herauszufinden. Clientplattform-spezifische Spezifikationen erforderlich ist, sind die `OnPlatform` Element wird verwendet.

Beachten Sie, die auf einige Werte verweisen `StaticResource`s. diese im definiert werden [Schritt 5](#5).

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

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Erstellen Sie Formate für die untergeordneten Elemente des Steuerelements.

Verweisen Sie auf alle Elemente, die definiert, die die untergeordneten Elemente, die verwendet werden, in das benutzerdefinierte Steuerelement erstellen:

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

#### <a name="4-create-the-control-layout-template"></a>4. Erstellen der Layout-Steuerelementvorlage

Das visuelle Design des benutzerdefinierten Steuerelements wird in der Steuerelementvorlage, die mithilfe der oben definierten Ressourcen explizit deklariert:

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

#### <a name="5-add-the-theme-specific-resources"></a>5. Fügen Sie die designspezifischen Ressourcen

Da es sich um ein benutzerdefiniertes Steuerelement handelt, fügen Sie die Ressourcen, die das Design, das Sie verwenden das Ressourcenverzeichnis entsprechen:

##### <a name="light-theme-colors"></a>Design "hell"-Farben

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Design "dunkel" Farben

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

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Legen Sie das ControlTemplate für die CardView--Klasse

Stellen Sie sicher die C#-Klasse, die im erstellten [Schritt 1](#1) verwendet die Steuerelementvorlage definiert, die [Schritt 4](#4) mit einer `Style` `Setter` Element

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Das Steuerelement zu einer Seite hinzufügen

Die `CardView` Steuerelement kann jetzt zu einer Seite hinzugefügt werden. Das folgende Beispiel zeigt hosten einem `StackLayout`:

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
