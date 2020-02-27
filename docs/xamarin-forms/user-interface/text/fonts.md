---
title: Schriftarten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Schriftartinformationen für Steuerelemente angeben, in denen Text in Xamarin.Forms-Anwendungen angezeigt wird.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/20/2020
ms.openlocfilehash: 75cf5acdb862a15d722075269b2f736264be680f
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636168"
---
# <a name="fonts-in-xamarinforms"></a>Schriftarten in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

Dieser Artikel beschreibt wie mit Xamarin.Forms können Sie die Schriftartattribute (einschließlich Gewichtung und Größe) angeben, für Steuerelemente, die Text anzeigen. Schriftart Informationen können [im Code](#Setting_Font_in_Code) oder [in XAML](#Setting_Font_in_Xaml)angegeben werden. Es ist auch möglich, eine [benutzerdefinierte Schriftart](#Using_a_Custom_Font)zu verwenden und [Schriftart Symbole anzuzeigen](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Festlegen der Schriftart im Code

Verwenden Sie die drei schriftartbezogene Eigenschaften aller Steuerelemente, die Text anzeigen:

- **FontFamily** &ndash; der `string` Schriftart Name.
- **FontSize** &ndash; die Schriftgröße als `double`.
- **Fontattributes** &ndash; eine Zeichenfolge, die Stil Informationen wie *kursiv* und **Fett** (mithilfe der `FontAttributes` Enumeration in C#) angibt.

Dieser Code zeigt, wie erstellen Sie eine Bezeichnung, und geben Sie den Schriftgrad und die Gewichtung angezeigt wird:

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

Die `FontSize`-Eigenschaft kann auf einen doppelten Wert festgelegt werden, z. a.:

```csharp
label.FontSize = 24;
```

Der Größen Wert wird in geräteunabhängigen Einheiten gemessen. Weitere Informationen finden Sie unter [Maßeinheiten](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin. Forms definiert auch Felder in der [`NamedSize`](xref:Xamarin.Forms.NamedSize) -Enumeration, die bestimmte Schriftgrößen darstellen. Weitere Informationen zu benannten Schriftgrößen finden Sie unter [benannte Schriftgrößen](#named-font-sizes).

<a name="FontAttributes" />

### <a name="font-attributes"></a>Schriftart Attribute

Schriftarten wie **Fett** und *kursiv* können für die `FontAttributes`-Eigenschaft festgelegt werden. Die folgenden Werte werden zurzeit unterstützt:

- **None**
- **Fett**
- **Kursiv**

Die `FontAttribute`-Enumeration kann wie folgt verwendet werden (Sie können ein einzelnes Attribut angeben oder Sie `OR`):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Schriftart Informationen pro Plattform festlegen

Alternativ kann die `Device.RuntimePlatform`-Eigenschaft verwendet werden, um unterschiedliche Schriftart Namen auf jeder Plattform festzulegen, wie in diesem Code veranschaulicht:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Eine gute Quelle für Schriftart Informationen für IOS ist [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Festlegen der Schriftart in XAML

Xamarin. Forms-Steuerelemente, die Text anzeigen, verfügen über eine `FontSize`-Eigenschaft, die in XAML festgelegt werden kann. Die einfachste Möglichkeit zum Festlegen der Schriftart in XAML ist die Verwendung der benannten Größe-Enumerationswerte fest, wie im folgenden Beispiel gezeigt:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Es gibt einen integrierten Konverter für die `FontSize`-Eigenschaft, mit der alle Schriftart Einstellungen als Zeichen folgen Wert in XAML ausgedrückt werden können. Außerdem kann die `FontAttributes`-Eigenschaft zum Angeben von Schriftart Attributen verwendet werden:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Die [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) -Eigenschaft kann auch in XAML verwendet werden, um eine andere Schriftart auf jeder Plattform zu erzeugen. Im folgenden Beispiel werden auf jeder Plattform verschiedene Schriftarten verwendet:

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

## <a name="named-font-sizes"></a>Benannte Schriftgrößen

Xamarin. Forms definiert Felder in der [`NamedSize`](xref:Xamarin.Forms.NamedSize) Enumeration, die bestimmte Schriftgrößen darstellen. In der folgenden Tabelle werden die `NamedSize` Mitglieder und ihre Standardgrößen für IOS, Android und die universelle Windows-Plattform (UWP) angezeigt:

| Member | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15,667 |
| `Small` | 13 | 14 | 18,667 |
| `Medium` | 16 | 17 | 22,667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Die Größen Werte werden in geräteunabhängigen Einheiten gemessen. Weitere Informationen finden Sie unter [Maßeinheiten](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Benannte Schriftgrößen können sowohl über XAML als auch über Code festgelegt werden. Außerdem kann die `Device.GetNamedSize`-Methode aufgerufen werden, um eine `double` zurückzugeben, die den benannten Schrift Grad darstellt:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> Unter IOS und Android werden benannte Schriftgrößen basierend auf den Barrierefreiheits Optionen des Betriebssystems automatisch skaliert. Dieses Verhalten kann unter IOS mit einem plattformspezifischen deaktiviert werden. Weitere Informationen finden Sie unter [Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>Benutzerdefinierte Schriftart verwenden

Benutzerdefinierte Schriftarten können dem freigegebenen xamarin. Forms-Projekt hinzugefügt und von Platt Form Projekten ohne zusätzliche Arbeit genutzt werden. Die Vorgehensweise hierfür ist wie folgt:

1. Fügen Sie die Schriftart dem freigegebenen xamarin. Forms-Projekt als eingebettete Ressource (**Buildaktion: EmbeddedResource**) hinzu.
1. Registrieren Sie die Schriftart Datei bei der Assembly in einer Datei, z. b. **AssemblyInfo.cs**, mithilfe des `ExportFont`-Attributs.

Das folgende Beispiel zeigt, wie die für die Assembly registrierte Hummer reguläre Schriftart angezeigt wird:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf")]
```

> [!NOTE]
> Die Schriftart kann sich in einem beliebigen Ordner im freigegebenen Projekt befinden, ohne den Ordnernamen beim Registrieren der Schriftart bei der Assembly angeben zu müssen.

Die Schriftart kann dann auf jeder Plattform verwendet werden, indem auf Ihren Namen ohne die Dateierweiterung verwiesen wird:

```xaml
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Der entsprechende C#-Code lautet:

```csharp
Label label = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};
```

Die folgenden Screenshots zeigen die benutzerdefinierte Schriftart:

[![Benutzerdefinierte Schriftart unter IOS und Android](fonts-images/custom-sml.png "Beispiel für benutzerdefinierte Schriftart")](fonts-images/custom.png#lightbox "Beispiel für benutzerdefinierte Schriftart")

## <a name="display-font-icons"></a>Schriftart Symbole anzeigen

Schriftart Symbole können von xamarin. Forms-Anwendungen angezeigt werden, indem die Schriftart Symbol Daten in einem `FontImageSource` Objekt angegeben werden. Diese Klasse, die von der [`ImageSource`](xref:Xamarin.Forms.ImageSource) -Klasse abgeleitet wird, verfügt über die folgenden Eigenschaften:

- `Glyph` – der Unicode-Zeichen Wert des Schriftart Symbols, der als `string`angegeben wird.
- `Size` – ein `double` Wert, der die Größe des gerenderten Schriftart Symbols in geräteunabhängigen Einheiten angibt. Der Standardwert ist 30. Außerdem kann diese Eigenschaft auf eine benannte Schriftgröße festgelegt werden.
- `FontFamily` – eine `string`, die die Schriftfamilie darstellt, zu der das Schriftart Symbol gehört.
- `Color` – ein optionaler [`Color`](xref:Xamarin.Forms.Color) Wert, der bei der Anzeige des Schriftart Symbols verwendet werden soll.

Diese Daten werden verwendet, um eine PNG-Datei zu erstellen, die von jeder Ansicht angezeigt werden kann, in der eine `ImageSource`angezeigt werden kann. Bei diesem Ansatz können Schriftart Symbole, wie z. b. Emojis, von mehreren Ansichten angezeigt werden, anstatt die Anzeige der Schriftart Symbole auf eine Ansicht zu beschränken, wie z. b. eine [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Schriftart Symbole können derzeit nur durch ihre Unicode-Zeichen Darstellung angegeben werden.

Das folgende XAML-Beispiel zeigt ein einzelnes Schriftart Symbol, das von einer [`Image`](xref:Xamarin.Forms.Image) Ansicht angezeigt wird:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Dieser Code zeigt ein Xbox-Symbol von der ionicons-Schriftfamilie in einer [`Image`](xref:Xamarin.Forms.Image) Ansicht an. Beachten Sie Folgendes: Obwohl das Unicode-Zeichen für dieses Symbol `\uf30c`ist, muss es in XAML mit Escapezeichen versehen werden und wird daher `&#xf30c;`. Der entsprechende C#-Code lautet:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

Die folgenden Screenshots aus dem Beispiel für [bindbare Layouts](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) zeigen, dass mehrere Schriftart Symbole von einem bindbaren Layout angezeigt werden:

![Screenshot der angezeigten Schriftart Symbole unter IOS und Android](fonts-images/font-image-source.png "In einer Bildansicht angezeigte Schriftart Symbole")

## <a name="related-links"></a>Verwandte Links

- [Fontssample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Bindbare Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Bindbare Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
