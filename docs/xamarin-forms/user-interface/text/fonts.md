---
title: Schriftarten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Schriftartinformationen für Steuerelemente angeben, in denen Text in Xamarin.Forms-Anwendungen angezeigt wird.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 62bc5d2012df4880e9257ac70d0fc2e0fca4af64
ms.sourcegitcommit: 619b32f4f96a255140963afcf87629ca690af93d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500441"
---
# <a name="fonts-in-xamarinforms"></a>Schriftarten in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

Dieser Artikel beschreibt wie mit Xamarin.Forms können Sie die Schriftartattribute (einschließlich Gewichtung und Größe) angeben, für Steuerelemente, die Text anzeigen. Schriftartinformationen möglich [im Code angegebenen](#Setting_Font_in_Code) oder [in XAML angegebenen](#Setting_Font_in_Xaml). Es ist auch möglich, eine [benutzerdefinierte Schriftart](#Using_a_Custom_Font)zu verwenden und [Schriftart Symbole anzuzeigen](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Festlegen der Schriftart im Code

Verwenden Sie die drei schriftartbezogene Eigenschaften aller Steuerelemente, die Text anzeigen:

- **FontFamily** &ndash; der `string` Schriftartname.
- **FontSize** &ndash; den Schriftgrad nach einem `double`.
- **FontAttributes** &ndash; eine Zeichenfolge, die Angabe von Formatinformationen wie *Kursiv* und **fett** (mithilfe der `FontAttributes` Aufzählung in c#).

Dieser Code zeigt, wie erstellen Sie eine Bezeichnung, und geben Sie den Schriftgrad und die Gewichtung angezeigt wird:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Schriftgrad

Die `FontSize` Eigenschaft kann auf einen double-Wert, für die Instanz festgelegt werden:

```csharp
label.FontSize = 24;
```

Der Größen Wert wird in geräteunabhängigen Einheiten gemessen. Weitere Informationen finden Sie unter [Maßeinheiten](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin. Forms definiert auch Felder in der [`NamedSize`](xref:Xamarin.Forms.NamedSize) -Enumeration, die bestimmte Schriftgrößen darstellen. Weitere Informationen zu benannten Schriftgrößen finden Sie unter [benannte Schriftgrößen](#named-font-sizes).

<a name="FontAttributes" />

### <a name="font-attributes"></a>Schriftart Attribute

Schriftart formatiert z. B. **fett** und *Kursiv* kann festgelegt werden, auf die `FontAttributes` Eigenschaft. Die folgenden Werte werden zurzeit unterstützt:

- **Keine**
- **Fett**
- **Kursiv**

Die `FontAttribute` Enumeration kann wie folgt verwendet werden (Sie können angeben, dass ein einzelnes Attribut oder `OR` sie):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Schriftart Informationen pro Plattform festlegen

Sie können auch die `Device.RuntimePlatform` Eigenschaft kann zum Festlegen von verschiedenen Schriftartnamen auf jeder Plattform verwendet werden, wie im folgenden Code gezeigt:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Ist eine gute Quelle für die Schriftartinformationen für iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Festlegen der Schriftart in XAML

Xamarin.Forms-Steuerelemente, Anzeigetext, die alle verfügen über eine `FontSize` -Eigenschaft, die in XAML festgelegt werden kann. Die einfachste Möglichkeit zum Festlegen der Schriftart in XAML ist die Verwendung der benannten Größe-Enumerationswerte fest, wie im folgenden Beispiel gezeigt:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Es ist ein integrierte Konverter für die `FontSize` -Eigenschaft, die alle Einstellungen für die Systemschriftart in XAML als Zeichenfolge ausgedrückt werden kann. Außerdem kann die `FontAttributes`-Eigenschaft zum Angeben von Schriftart Attributen verwendet werden:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) kann auch in XAML verwendet werden um zu eine andere Schriftart auf jeder Plattform zu rendern. Im folgenden Beispiel wird eine benutzerdefinierte Schriftart in ios (markerfelt-Thin) verwendet, und es werden nur Größe/Attribute auf den anderen Plattformen angegeben:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

Wenn Sie eine benutzerdefinierte Schriftart angeben, es ist immer eine gute Idee, `OnPlatform`, da es schwierig, eine Schriftart zu finden, die auf allen Plattformen verfügbar ist.

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

Mit einer Schriftart außer den integrierten Schriftarten erfordert einige plattformspezifische codieren. Dieser Screenshot zeigt die benutzerdefinierte Schriftart **Lobster** aus [Googles Open-Source-Schriftarten](https://www.google.com/fonts) mit Xamarin.Forms gerendert.

 [![Benutzerdefinierte Schriftart unter IOS und Android](fonts-images/custom-sml.png "Beispiel für benutzerdefinierte Schriftart")](fonts-images/custom.png#lightbox "Beispiel für benutzerdefinierte Schriftart")

Die für jede Plattform erforderlichen Schritte werden unten beschrieben. Wenn Sie benutzerdefinierte Schriftart-Dateien mit einer Anwendung einschließen, achten Sie darauf, dass Sie sicherstellen, dass die Schriftart-Lizenz für die Verteilung ermöglicht.

### <a name="ios"></a>iOS

Es ist möglich, zeigen eine benutzerdefinierte Schriftart, indem zunächst sichergestellt, dass es geladen wird, und verweisen auf diese Namen in der Xamarin.Forms `Font` Methoden.
Befolgen Sie die Anweisungen in [in diesem Blogbeitrag](https://devblogs.microsoft.com/xamarin/custom-fonts-in-ios/):

1. Fügen Sie die Schriftartdatei mit **Buildvorgang: BundleResource**, und
2. Update der **"Info.plist"** Datei (**von Anwendung bereitgestellte Schriftarten**, oder `UIAppFonts`Schlüssel), klicken Sie dann
3. Verweisen sie anhand des Namens, wenn Sie eine Schriftart in Xamarin.Forms definieren.

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms für Android kann es sich um eine benutzerdefinierte Schriftart verweisen, die durch einen bestimmten Benennungsstandard folgen dem Projekt hinzugefügt wurde. Fügen Sie zuerst die Schriftartdatei, die **Assets** Ordner im Projekt-Anwendung und legen *Buildvorgang: AndroidAsset*. Klicken Sie dann den vollständigen Pfad verwenden und *Schriftartname* getrennt durch ein Nummernzeichen (#) als der Name der Schriftart in Xamarin.Forms, wie der folgende Codeausschnitt veranschaulicht:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms für Windows-Plattformen kann es sich um eine benutzerdefinierte Schriftart verweisen, die durch einen bestimmten Benennungsstandard folgen dem Projekt hinzugefügt wurde. Fügen Sie zuerst die Schriftartdatei, die **befände/Schriftarten/** Ordner im Projekt-Anwendung und legen die **erstellen: der Inhalt der Aktion**. Verwenden Sie dann den vollständigen Pfad und der Schriftart Dateinamen, gefolgt von einem Nummernzeichen (#) und die **Schriftartname**, wie der folgende Codeausschnitt veranschaulicht:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Beachten Sie, dass die Schriftart, Namen und den Namen der Datei kann unterschiedlich sein. Um den Schriftartnamen für Windows zu ermitteln, mit der rechten Maustaste der .ttf-Datei, und wählen Sie **Vorschau**. Der Name der Schriftart kann dann aus dem Fenster "Vorschau" ermittelt werden.

### <a name="xaml"></a>XAML

Sie können auch [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) in XAML, um eine benutzerdefinierte Schriftart zu rendern:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

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

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Bindbare Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Bindbare Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
