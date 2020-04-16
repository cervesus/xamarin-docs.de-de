---
title: Schriftarten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie Schriftartinformationen zu Steuerelementen angeben, die Text in Xamarin.Forms-Anwendungen anzeigen.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: ca4d3b242fcc73bb73e8d6ab1f817eefcc2ade4d
ms.sourcegitcommit: 89b3e383a37db5b940f0c63bbfe9cb806dc7d5d1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81388802"
---
# <a name="fonts-in-xamarinforms"></a>Schriftarten in Xamarin.Forms

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

In diesem Artikel wird beschrieben, wie Sie mit Xamarin.Forms Schriftartattribute (einschließlich Gewicht ungegängen und größe) für Steuerelemente angeben können, die Text anzeigen. Schriftartinformationen können [im Code oder](#Setting_Font_in_Code) in [XAML angegeben](#Setting_Font_in_Xaml)werden. Es ist auch möglich, eine [benutzerdefinierte Schriftart](#use-a-custom-font)zu verwenden und [Schriftsymbole anzuzeigen.](#display-font-icons)

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Festlegen der Schriftart im Code

Verwenden Sie die drei schriftartbezogenen Eigenschaften aller Steuerelemente, die Text anzeigen:

- **FontFamily** &ndash; `string` den Schriftnamen.
- **FontSize** &ndash; die Schriftgröße `double`als .
- **FontAttributes** &ndash; eine Zeichenfolge, die Stilinformationen wie *Italic* und **Bold** angibt (unter Verwendung der `FontAttributes` Enumeration in C').

Dieser Code zeigt, wie Sie eine Beschriftung erstellen und die anzuzeigende Schriftgröße und -gewicht angeben:

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Schriftgrad

Die `FontSize` Eigenschaft kann auf einen doppelten Wert festgelegt werden, z. B.:

```csharp
label.FontSize = 24;
```

Der Größenwert wird in geräteunabhängigen Einheiten gemessen. Weitere Informationen finden Sie unter [Maßeinheiten](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin.Forms definiert auch Felder [`NamedSize`](xref:Xamarin.Forms.NamedSize) in der Enumeration, die bestimmte Schriftgrößen darstellen. Weitere Informationen zu benannten Schriftgrößen finden Sie unter [Benannte Schriftgrößen](#named-font-sizes).

<a name="FontAttributes" />

### <a name="font-attributes"></a>Schriftartattribute

Schriftarten wie **Fett** und *Kursiv* können `FontAttributes` auf der Eigenschaft festgelegt werden. Die folgenden Werte werden derzeit unterstützt:

- **None**
- **kühn**
- **Italic**

Die `FontAttribute` Enumeration kann wie folgt verwendet werden (Sie können ein einzelnes Attribut oder `OR` diese zusammen angeben):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Festlegen von Schriftinformationen pro Plattform

Alternativ kann `Device.RuntimePlatform` die Eigenschaft verwendet werden, um verschiedene Schriftartennamen auf jeder Plattform festzulegen, wie in diesem Code gezeigt:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Eine gute Quelle für Schriftartinformationen für iOS ist [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Festlegen der Schriftart in XAML

Xamarin.Forms-Steuerelemente, die `FontSize` Text anzeigen, verfügen alle über eine Eigenschaft, die in XAML festgelegt werden kann. Die einfachste Möglichkeit, die Schriftart in XAML festzulegen, besteht darin, die benannten Größenaufzählungswerte zu verwenden, wie in diesem Beispiel gezeigt:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Es gibt einen integrierten Konverter `FontSize` für die Eigenschaft, mit dem alle Schriftarteinstellungen als Zeichenfolgenwert in XAML ausgedrückt werden können. Darüber hinaus `FontAttributes` kann die Eigenschaft verwendet werden, um Schriftartattribute anzugeben:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Die [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) Eigenschaft kann auch in XAML verwendet werden, um eine andere Schriftart auf jeder Plattform zu rendern. Im folgenden Beispiel werden auf jeder Plattform unterschiedliche Schriftarten verwendet:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>Benannte Schriftgrade

Xamarin.Forms definiert Felder [`NamedSize`](xref:Xamarin.Forms.NamedSize) in der Enumeration, die bestimmte Schriftgrößen darstellen. Die folgende Tabelle `NamedSize` zeigt die Mitglieder und ihre Standardgrößen auf iOS, Android und der universellen Windows-Plattform (UWP):

| Member | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Die Größenwerte werden in geräteunabhängigen Einheiten gemessen. Weitere Informationen finden Sie unter [Maßeinheiten](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Benannte Schriftgrößen können sowohl über XAML als auch über Code festgelegt werden. Darüber hinaus `Device.GetNamedSize` kann die Methode aufgerufen `double` werden, um eine zurückzugeben, die die benannte Schriftgröße darstellt:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> Unter iOS und Android werden benannte Schriftgrößen basierend auf den Eingabehilfen des Betriebssystems automatisch skaliert. Dieses Verhalten kann unter iOS mit einem plattformspezifischen deaktiviert werden. Weitere Informationen finden Sie unter [Barrierefreiheitsskalierung für benannte Schriftgrößen unter iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

## <a name="use-a-custom-font"></a>Verwenden einer benutzerdefinierten Schriftart

Benutzerdefinierte Schriftarten können Ihrem freigegebenen Xamarin.Forms-Projekt hinzugefügt und von Plattformprojekten ohne zusätzliche Arbeit genutzt werden. Die Vorgehensweise hierfür ist wie folgt:

1. Fügen Sie die Schriftart Ihrem freigegebenen Xamarin.Forms-Projekt als eingebettete Ressource hinzu (**Build Action: EmbeddedResource**).
1. Registrieren Sie die Schriftartdatei mit dem **AssemblyInfo.cs** `ExportFont` Attribut in einer Datei wie AssemblyInfo.cs . Ein optionaler Alias kann ebenfalls angegeben werden.

> [!IMPORTANT]
> Eingebettete Schriftarten erfordern die Verwendung von Xamarin.Forms 4.5.0.530 oder höher.

Das folgende Beispiel zeigt die Lobster-Regular-Schriftart, die bei der Assembly registriert ist, zusammen mit einem Alias:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> Die Schriftart kann sich in einem beliebigen Ordner im freigegebenen Projekt befinden, ohne den Ordnernamen angeben zu müssen, wenn die Schriftart bei der Assembly registriert wird.

Die Schriftart kann dann auf jeder Plattform verwendet werden, indem sie auf ihren Namen verweist, ohne die Dateierweiterung:

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Alternativ kann es auf jeder Plattform verbraucht werden, indem auf seinen Alias verwiesen wird:

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

Der entsprechende C#-Code lautet:

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

Die folgenden Screenshots zeigen die benutzerdefinierte Schriftart:

[![Benutzerdefinierte Schriftart auf iOS und Android](fonts-images/custom-sml.png "Beispiel für benutzerdefinierte Schriftarten")](fonts-images/custom.png#lightbox "Beispiel für benutzerdefinierte Schriftarten")

> [!IMPORTANT]
> Unter Windows können der Name der Schriftartdatei und der Schriftartname unterschiedlich sein. Um den Schriftnamen unter Windows zu ermitteln, klicken Sie mit der rechten Maustaste auf die TTF-Datei, und wählen Sie **Vorschau**aus. Der Schriftartname kann dann aus dem Vorschaufenster ermittelt werden.

## <a name="display-font-icons"></a>Anzeigen von Schriftartsymbolen

Schriftartsymbole können von Xamarin.Forms-Anwendungen angezeigt werden, `FontImageSource` indem die Schriftsymboldaten in einem Objekt angegeben werden. Diese Klasse, die von [`ImageSource`](xref:Xamarin.Forms.ImageSource) der Klasse abstammt, hat die folgenden Eigenschaften:

- `Glyph`– der Unicode-Zeichenwert des Schriftartsymbols, der als `string`angegeben wird.
- `Size`– `double` ein Wert, der die Größe des gerenderten Schriftzeichens in geräteunabhängigen Einheiten angibt. Der Standardwert ist 30. Darüber hinaus kann diese Eigenschaft auf eine benannte Schriftgröße festgelegt werden.
- `FontFamily`– `string` eine Darstellung der Schriftfamilie, zu der das Schriftsymbol gehört.
- `Color`– ein [`Color`](xref:Xamarin.Forms.Color) optionaler Wert, der beim Anzeigen des Schriftartsymbols verwendet werden soll.

Diese Daten werden verwendet, um ein PNG zu erstellen, `ImageSource`das von jeder Ansicht angezeigt werden kann, die eine anzeigen kann. Dieser Ansatz ermöglicht die Anzeige von Schriftartensymbolen, z. B. Emojis, durch mehrere Ansichten, [`Label`](xref:Xamarin.Forms.Label)anstatt die Anzeige des Schriftzeichens auf eine einzelne Textdarstellungsansicht zu beschränken, z. B. eine .

> [!IMPORTANT]
> Schriftartsymbole können derzeit nur durch ihre Unicode-Zeichendarstellung angegeben werden.

Im folgenden XAML-Beispiel wird ein einzelnes Schriftartsymbol von einer [`Image`](xref:Xamarin.Forms.Image) Ansicht angezeigt:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Dieser Code zeigt in einer [`Image`](xref:Xamarin.Forms.Image) Ansicht ein XBox-Symbol aus der Schriftfamilie Ionicons an. Beachten Sie, dass das Unicode-Zeichen für dieses Symbol `\uf30c`zwar ein `&#xf30c;`Escapezeichen in XAML verwendet werden muss und daher zu wird. Der entsprechende C#-Code lautet:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

Die folgenden Screenshots aus dem Beispiel [bindbare Layouts](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) zeigen mehrere Schriftsymbole, die durch ein bindbares Layout angezeigt werden:

![Screenshot der angezeigten Schriftsymbole unter iOS und Android](fonts-images/font-image-source.png "Schriftsymbole, die in einer Bildansicht angezeigt werden")

## <a name="related-links"></a>Verwandte Links

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Bindbare Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Bindbare Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
