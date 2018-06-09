---
title: Schriftarten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie an Schriftartinformationen für Steuerelemente, die Text in Xamarin.Forms Anwendungen angezeigt wird.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: fd45528446c9d3d4bdfa1b8f9f4010babb2ad044
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245630"
---
# <a name="fonts-in-xamarinforms"></a>Schriftarten in Xamarin.Forms

Dieser Artikel beschreibt wie Xamarin.Forms Schriftartattribute (einschließlich Gewichtung und Größe) angeben können, auf die Steuerelemente zur Anzeige von Text. Informationen zur Schriftart kann [im Code spezifizierte](#Setting_Font_in_Code) oder [in Xaml angegeben](#Setting_Font_in_Xaml).
Es ist auch möglich, verwenden Sie eine [benutzerdefinierte Schriftart](#Using_a_Custom_Font).

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>Festlegen von Schriftart in Code

Verwenden Sie die drei schriftartbezogene Eigenschaften aller Steuerelemente, die Text anzeigen:

- **FontFamily** &ndash; der `string` Schriftartname.
- **FontSize** &ndash; den Schriftgrad als eine `double`.
- **FontAttributes** &ndash; eine Zeichenfolge, die Angabe von Formatinformationen wie *Kursiv* und **fett** (mithilfe der `FontAttributes` Enumeration in c#).

Dieser Code zeigt, wie Sie eine Bezeichnung zu erstellen, und geben Sie den Schriftgrad und die Gewichtung vervielfacht, um anzuzeigen:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Schriftgrad

Die `FontSize` Eigenschaft kann für die Instanz in einen double-Wert festgelegt werden:

```csharp
label.FontSize = 24;
```

Sie können auch die `NamedSize` Enumeration besitzt vier integrierte Optionen; Xamarin.Forms wählt die optimale Größe für jede Plattform.

-  **Micro**
-  **Kleine**
-  **Mittel**
-  **Große**


Die `NamedSize` Enumeration kann überall verwendet eine `FontSize` kann angegeben werden, mithilfe der `Device.GetNamedSize` Methode, um den Wert zu konvertieren einer `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Schriftartattribute

Schriftart Formate wie z. B. **fett** und *Kursiv* kann festgelegt werden, auf die `FontAttributes` Eigenschaft. Die folgenden Werte werden derzeit unterstützt:

-  **Keine**
-  **Fett**
-  **Kursiv**

Die `FontAttribute` Enumeration kann wie folgt verwendet werden (Sie können ein einzelnes Attribut angeben oder `OR` sie zusammen):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

Einige Xamarin.Forms-Steuerelemente (z. B. `Label`) unterstützen auch verschiedene Schriftartattribute innerhalb einer Zeichenfolge mithilfe der `FormattedString` Klasse. Ein `FormattedString` besteht aus einem oder mehr `Span`s, von denen jede eine eigene Formatierung Attribute aufweisen.

Die `Span` -Klasse verfügt über die folgenden Attribute:

* **Text** &ndash; anzuzeigende Wert
* **FontFamily** &ndash; den Namen der Schriftart
* **FontSize** &ndash; den Schriftgrad
* **FontAttributes** &ndash; Formatinformationen wie *Kursiv* und **fett**
* **Foreground Color** &ndash; Textfarbe
* **BackgroundColor** &ndash; Hintergrundfarbe

Ein Beispiel für das Erstellen und Anzeigen von einem `FormattedString` wird unten gezeigt &ndash; Hinweis, der er der Bezeichnungen zugewiesen ist `FormattedText` Eigenschaft und nicht die `Text` Eigenschaft.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>Festlegen von Schriftart Info plattformspezifischen

Alternativ können Sie die `Device.RuntimePlatform` Eigenschaft kann zum Festlegen von verschiedenen Schriftartnamen auf jeder Plattform verwendet werden, wie in diesem Code gezeigt:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Ist eine gute Informationsquelle Schriftart für iOS [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Festlegen der Schriftartformats in Xaml

Xamarin.Forms steuert, Anzeigetext alle verfügen über eine `Font` -Eigenschaft, die in Xaml festgelegt werden kann. Die einfachste Möglichkeit, die Schriftart in Xaml festgelegt ist die Verwendung der benannten Größe-Enumerationswerte fest, wie im folgenden Beispiel gezeigt:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Es ist ein integrierte Konverter für die `Font` Clustereigenschaft, mit der alle schriftarteinstellungen als Zeichenfolgenwert in Xaml ausgedrückt werden. Die folgenden Beispiele zeigen, wie Sie Schriftartattribute und Größen in Xaml angeben können:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

Angeben von mehreren `Font` Einstellungen, die erforderlichen Einstellungen in einer einzigen Schriftart Attributzeichenfolge zu kombinieren. Die Schriftart Attributzeichenfolge formatiert werden sollen, als `"[font-face],[attributes],[size]"`. Die Reihenfolge der Parameter ist wichtig, alle Parameter sind optional, und mehrere `attributes` kann beispielsweise angegeben werden:

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

Die `FormattedString` -Klasse kann auch in XAML verwendet werden, wie hier gezeigt:

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) kann auch in XAML verwendet werden, um eine andere Schriftart auf jeder Plattform zu rendern. Im folgenden Beispiel wird eine benutzerdefinierte Schriftart für iOS (<span style="font-family:MarkerFelt-Thin">MarkerFelt dünn</span>) und gibt nur Größe/Attribute auf anderen Plattformen:

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

Wenn Sie eine benutzerdefinierte Schriftart angeben, wird immer eine gute Idee, verwenden Sie `OnPlatform`, wie es schwierig ist, eine Schriftart zu suchen, die auf allen Plattformen verfügbar ist.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>Verwenden eine benutzerdefinierte Schriftart

Mithilfe einer Schriftart als integrierte Schriftarten erfordert einige plattformspezifischen-Codierung. Diese bildschirmabbildung zeigt die benutzerdefinierte Schriftartdatei **Hummern** aus [Google Open Source-Schriftarten](https://www.google.com/fonts) mit Xamarin.Forms gerendert.

 [![Benutzerdefinierte Schriftart auf IOS- und Android](fonts-images/custom-sml.png "benutzerdefinierte Schriftarten Beispiel")](fonts-images/custom.png#lightbox "benutzerdefinierte Schriftarten-Beispiel")

Im folgenden sind die erforderlichen Schritte für jede Plattform erläutert. Wenn Sie benutzerdefinierte Schriftart-Dateien mit einer Anwendung einschließen, achten Sie darauf, dass Sie überprüfen, ob die Schriftart-Lizenz für die Verteilung zulässt.

### <a name="ios"></a>iOS

Es ist möglich, eine benutzerdefinierte Schriftart anzeigen, indem Sie zunächst sicherstellen, dass es geladen wird, und klicken Sie dann verweisen darauf anhand des Namens der Entwicklung Xamarin.Forms verwenden `Font` Methoden.
Befolgen Sie die Anweisungen in [diesem Blogbeitrag](http://blog.xamarin.com/custom-fonts-in-ios/):

1. Fügen Sie die Schriftartdatei mit **Buildvorgang: BundleResource**, und
2. Update der **"Info.plist"** Datei (**Schriftarten, die von der Anwendung bereitgestellten**, oder `UIAppFonts`, Schlüsselzeit), klicken Sie dann
3. Darauf verweisen Sie anhand des Namens, wo Sie eine Schriftart in Xamarin.Forms definieren.

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms für Android können eine benutzerdefinierte Schriftart verweisen, die durch einen bestimmten Benennungsstandard folgen dem Projekt hinzugefügt wurde. Zunächst fügen die Schriftartdatei, die **Bestand** Ordner in das Anwendungsprojekt und *Buildvorgang: AndroidAsset*. Verwenden Sie den vollständigen Pfad und *Schriftartname* getrennt durch ein Nummernzeichen (#) als den Schriftartnamen in Xamarin.Forms, wie der folgende Codeausschnitt veranschaulicht:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms für Windows-Plattformen kann es sich um eine benutzerdefinierte Schriftart verweisen, die durch einen bestimmten Benennungsstandard folgen dem Projekt hinzugefügt wurde. Zuerst die Schriftartdatei zum Hinzufügen der **/Assets/Schriftarten/** Ordner, in das Anwendungsprojekt und Festlegen der <span class="UIItem">erstellen Aktionsinhalt:</span>. Verwenden Sie den vollständigen Pfad und Schriftart Dateinamen, gefolgt von einem Nummernzeichen (#) und die <span class="UIItem">Schriftartname</span>, wie der folgende Codeausschnitt veranschaulicht:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Beachten Sie, dass die Datei Schriftartname und Schriftartname unterscheiden können. Um den Schriftartnamen für Windows zu ermitteln, mit der rechten Maustaste der .ttf-Datei, und wählen Sie **Vorschau**. Der Name der Schriftart kann dann über das untenstehende Vorschaufenster ermittelt werden.

Der allgemeine Code für die Anwendung ist damit fertiggestellt. Nun wird plattformspezifischer Telefonwählcode als [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) implementiert.

### <a name="xaml"></a>XAML

Sie können auch [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) in XAML zum Rendern einer benutzerdefinierten Schriftart:

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

Xamarin.Forms stellt einfache Standardeinstellungen Sie können Text einfach für alle unterstützten Plattformen Größe. Darüber hinaus können Sie die Schriftart und-Größe geben &ndash; auch anders für jede Plattform &ndash; Wenn eine präzisere Kontrolle erforderlich ist. Die `FormattedString` -Klasse kann verwendet werden, so erstellen Sie eine Zeichenfolge, die mit anderen Schriftart-Spezifikationen, die mithilfe der `Span` Klasse.

Informationen zur Schriftart kann auch in Xaml, die über eine ordnungsgemäß formatierte Schriftartattribute angegeben werden oder die `FormattedString` Element mit `Span` untergeordneten Elemente.


## <a name="related-links"></a>Verwandte Links

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
