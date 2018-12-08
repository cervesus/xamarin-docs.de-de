---
title: Schriftarten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Schriftartinformationen für Steuerelemente angeben, in denen Text in Xamarin.Forms-Anwendungen angezeigt wird.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 1b90a3184b89ba9147525a87b52e048bbb59f5af
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061149"
---
# <a name="fonts-in-xamarinforms"></a>Schriftarten in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)

Dieser Artikel beschreibt wie mit Xamarin.Forms können Sie die Schriftartattribute (einschließlich Gewichtung und Größe) angeben, für Steuerelemente, die Text anzeigen. Schriftartinformationen möglich [im Code angegebenen](#Setting_Font_in_Code) oder [in XAML angegebenen](#Setting_Font_in_Xaml).
Es ist auch möglich, verwenden Sie eine [benutzerdefinierte Schriftart](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Wenn Sie die Schriftart im Code

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

Sie können auch die `NamedSize` Enumeration, die vier integrierten Optionen; Xamarin.Forms wählt die optimale Größe für jede Plattform.

-  **Micro**
-  **Kleine**
-  **Mittel**
-  **Große**

Die `NamedSize` Enumeration möglich verwendet, wo eine `FontSize` kann angegeben werden, mithilfe der `Device.GetNamedSize` Methode, um den Wert konvertiert werden soll eine `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Schriftartattribute

Schriftart formatiert z. B. **fett** und *Kursiv* kann festgelegt werden, auf die `FontAttributes` Eigenschaft. Die folgenden Werte werden zurzeit unterstützt:

-  **Keine**
-  **Fett**
-  **Kursiv**

Die `FontAttribute` Enumeration kann wie folgt verwendet werden (Sie können angeben, dass ein einzelnes Attribut oder `OR` sie):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>Festlegen von Schriftartinformationen pro Plattform

Sie können auch die `Device.RuntimePlatform` Eigenschaft kann zum Festlegen von verschiedenen Schriftartnamen auf jeder Plattform verwendet werden, wie im folgenden Code gezeigt:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Ist eine gute Quelle für die Schriftartinformationen für iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Festlegen der Schrift in XAML

Xamarin.Forms-Steuerelemente, Anzeigetext, die alle verfügen über eine `Font` -Eigenschaft, die in XAML festgelegt werden kann. Die einfachste Möglichkeit zum Festlegen der Schriftart in XAML ist die Verwendung der benannten Größe-Enumerationswerte fest, wie im folgenden Beispiel gezeigt:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Es ist ein integrierte Konverter für die `Font` -Eigenschaft, die alle Einstellungen für die Systemschriftart in XAML als Zeichenfolge ausgedrückt werden kann. Die folgenden Beispiele zeigen, wie Sie Schriftartattribute und Größe in XAML angeben können:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Angeben mehrerer `Font` Einstellungen kombinieren Sie die erforderlichen Einstellungen in einem einzelnen `Font` Attribut-Zeichenfolge. Die Schriftart-Attribut-Zeichenfolge formatiert werden sollen, als `"[font-face],[attributes],[size]"`. Die Reihenfolge der Parameter ist wichtig, alle Parameter sind optional, und mehrere `attributes` kann angegeben werden, z.B.:

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) kann auch in XAML verwendet werden um zu eine andere Schriftart auf jeder Plattform zu rendern. Im folgenden Beispiel wird eine benutzerdefinierte Schriftart unter iOS (<span style="font-family:MarkerFelt-Thin">MarkerFelt-dünn</span>) und gibt nur die Größe/Attribute auf den anderen Plattformen:

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

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Verwenden eine benutzerdefinierte Schriftart

Mit einer Schriftart außer den integrierten Schriftarten erfordert einige plattformspezifische codieren. Dieser Screenshot zeigt die benutzerdefinierte Schriftart **Lobster** aus [Googles Open-Source-Schriftarten](https://www.google.com/fonts) mit Xamarin.Forms gerendert.

 [![Benutzerdefinierte Schriftart unter iOS und Android](fonts-images/custom-sml.png "Beispiel für benutzerdefinierte Schriftarten")](fonts-images/custom.png#lightbox "benutzerdefinierte Schriftarten-Beispiel")

Die für jede Plattform erforderlichen Schritte werden unten beschrieben. Wenn Sie benutzerdefinierte Schriftart-Dateien mit einer Anwendung einschließen, achten Sie darauf, dass Sie sicherstellen, dass die Schriftart-Lizenz für die Verteilung ermöglicht.

### <a name="ios"></a>iOS

Es ist möglich, zeigen eine benutzerdefinierte Schriftart, indem zunächst sichergestellt, dass es geladen wird, und verweisen auf diese Namen in der Xamarin.Forms `Font` Methoden.
Befolgen Sie die Anweisungen in [in diesem Blogbeitrag](http://blog.xamarin.com/custom-fonts-in-ios/):

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

Xamarin.Forms für Windows-Plattformen kann es sich um eine benutzerdefinierte Schriftart verweisen, die durch einen bestimmten Benennungsstandard folgen dem Projekt hinzugefügt wurde. Fügen Sie zuerst die Schriftartdatei, die **befände/Schriftarten/** Ordner im Projekt-Anwendung und legen die <span class="UIItem">erstellen: der Inhalt der Aktion</span>. Verwenden Sie dann den vollständigen Pfad und der Schriftart Dateinamen, gefolgt von einem Nummernzeichen (#) und die <span class="UIItem">Schriftartname</span>, wie der folgende Codeausschnitt veranschaulicht:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Beachten Sie, dass die Schriftart, Namen und den Namen der Datei kann unterschiedlich sein. Um den Schriftartnamen für Windows zu ermitteln, mit der rechten Maustaste der .ttf-Datei, und wählen Sie **Vorschau**. Der Name der Schriftart kann dann aus dem Fenster "Vorschau" ermittelt werden.

Der allgemeine Code für die Anwendung ist damit fertiggestellt. Nun wird plattformspezifischer Telefonwählcode als [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) implementiert.

### <a name="xaml"></a>XAML

Sie können auch [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) in XAML, um eine benutzerdefinierte Schriftart zu rendern:

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

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms stellt simplen Standardeinstellungen, Sie können die Größe von Text ganz einfach für alle unterstützten Plattformen. Darüber hinaus können Sie die Schriftart und-Größe geben &ndash; auch anders für jede Plattform &ndash; Wenn eine präzisere Kontrolle erforderlich ist.

Schriftartinformationen kann auch in XAML verwenden ordnungsgemäß formatierte Schriftartattribute angegeben werden.

## <a name="related-links"></a>Verwandte Links

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
