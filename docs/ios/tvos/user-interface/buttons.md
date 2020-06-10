---
title: Arbeiten mit tvos-Schaltflächen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit Schaltflächen in einer tvos-App arbeiten, die mit xamarin erstellt wurde. Darin wird erläutert, wie Sie mit Schaltflächen im Code und in Storyboards arbeiten und wie Sie eine Schaltfläche formatieren.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/07/2017
ms.openlocfilehash: 63aa344ec94730ebe448aba090e2d91af9da64b5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574040"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Arbeiten mit tvos-Schaltflächen in xamarin

Verwenden Sie eine Instanz der- `UIButton` Klasse, um eine Fokussier Bare, auswählbare Schaltfläche in einem tvos-Fenster zu erstellen. Wenn der Benutzer eine Schaltfläche auswählt, sendet er eine Aktions Meldung an das Zielobjekt, damit Ihre xamarin. tvos-App auf die Eingabe des Benutzers antwortet.

[![](buttons-images/buttons01.png "Example buttons")](buttons-images/buttons01.png#lightbox)

Weitere Informationen zum Arbeiten mit dem Fokus und zum Navigieren mit der Siri-Remote Dokumentation finden Sie in der Dokumentation [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri-Remote-und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) .

<a name="About-Buttons"></a>

## <a name="about-buttons"></a>Informationen zu Schaltflächen

In tvos werden Schaltflächen für App-spezifische Aktionen verwendet und können einen Titel, ein Symbol oder beides enthalten. Wenn der Benutzer die Benutzeroberfläche der App mithilfe von [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)navigiert, verschiebt sich der Fokus auf die angegebene Schaltfläche, sodass Text-und Hintergrundfarben geändert werden. Ein Schatten wird auch auf die Schaltfläche zum Hinzufügen eines 3D-Effekts angewendet, sodass er über dem Rest der Benutzeroberfläche angezeigt wird.

[![](buttons-images/buttons01.png "Example buttons")](buttons-images/buttons01.png#lightbox)

Apple hat die folgenden Vorschläge zum Arbeiten mit Schaltflächen:

- **Verwenden Sie entweder einen Titel oder ein Symbol** , während sowohl ein Symbol als auch ein Titel in eine Schaltfläche eingeschlossen werden können, da Leerzeichen begrenzt sind. versuchen Sie nicht, beides zu kombinieren.
- **Destruktive Schaltflächen eindeutig markieren** : Wenn die Schaltfläche eine zerstörerische Aktion ausführt (z. b. das Löschen einer Datei), markieren Sie Sie mit dem Text und/oder dem Symbol eindeutig als solche. Zerstörerische Aktionen sollten immer eine [Warnung](~/ios/tvos/user-interface/alerts.md) darstellen, die den Benutzer auffordert, die Aktion einzuschränken.
- **Keine Schaltflächen verwenden** : die Menü Schaltfläche auf der Siri-Remote Seite wird verwendet, um zum vorherigen Bildschirm zurückzukehren. Die einzige Ausnahme von dieser Regel ist für in-App-Käufe oder destruktive Aktionen, bei denen eine Schaltfläche **Abbrechen** angezeigt werden soll.

Weitere Informationen zum Arbeiten mit dem Fokus und der Navigation finden Sie in unserer Dokumentation zum [Arbeiten mit der Navigation und dem Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) .

<a name="Button-Icons"></a>

### <a name="button-icons"></a>Schaltflächen Symbole

Apple schlägt vor, dass Sie für Ihre Schaltflächen Symbole einfache, stark erkennbare Bilder verwenden. Übermäßig komplexe Symbole sind auf einem TV-Bildschirm im Raum auf dem Bildschirm schwer zu erkennen. versuchen Sie also, die einfachste Darstellung zu verwenden, um den Eindruck zu vermitteln. Verwenden Sie nach Möglichkeit standardmäßig bekannte Bilder für Symbole (z. b. eine Lupe für die Suche).

<a name="Button-Titles"></a>

### <a name="button-titles"></a>Schaltflächen Titel

Apple hat beim Erstellen der Titel für Ihre Schaltflächen die folgenden Vorschläge:

- **Beschreibungstext unterhalb** von Symbol Schaltflächen anzeigen: Wenn möglich, platzieren Sie nach Möglichkeit den Text der Schaltfläche mit den folgenden Symbolen, um den Zweck der Schaltfläche weiter zu erhöhen.
- **Verwenden Sie Verben oder Verb Ausdrücke für den Titel** , und geben Sie die Aktion, die ausgeführt wird, wenn der Benutzer auf die Schaltfläche klickt, eindeutig an.
- **Verwenden** Sie die Groß-/Kleinschreibung von Titeln, mit Ausnahme von Artikeln, antaktionen oder vorab Positionen (höchstens vier Buchstaben), sollte jedes Wort des Schaltflächen Titels groß geschrieben werden.
- **Verwenden Sie einen kurzen, zu-Punkt-Titel** : Verwenden Sie das kürzeste mögliche Sprache, um die Aktion der Schaltfläche zu beschreiben.

<a name="Buttons-and-Storyboards"></a>

## <a name="buttons-and-storyboards"></a>Schaltflächen und Storyboards

Die einfachste Möglichkeit, mit Schaltflächen in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des Xamarin Designer für IOS der Benutzeroberfläche der APP hinzuzufügen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** , und legen Sie Sie in der Ansicht ab: 

    [![](buttons-images/storyboard01.png "A button")](buttons-images/storyboard01.png#lightbox)
1. Im **Eigenschaften-Explorer**können Sie mehrere Eigenschaften der Schaltfläche anpassen, z. b. **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02.png "Button properties")](buttons-images/storyboard02.png#lightbox)
1. Wechseln Sie als nächstes zur **Registerkarte Ereignisse** , und richten Sie ein **Ereignis** über die **Schaltfläche** ein, und nennen Sie es `ButtonPressed` : 

    [![](buttons-images/storyboard03.png "The Events Tab")](buttons-images/storyboard03.png#lightbox)
1. Sie werden automatisch zu der Ansicht gewechselt, in der `ViewController.cs` Sie die neue Aktion mithilfe der nach- **oben** -und **nach-unten** -Taste in Ihrem Code platzieren können: 

    [![](buttons-images/storyboard04.png "Placing a new Action in code")](buttons-images/storyboard04.png#lightbox)
1. Drücken Sie die **Eingabe** Taste, um den Speicherort auszuwählen: 

    [![](buttons-images/storyboard05.png "The code editor")](buttons-images/storyboard05.png#lightbox)
1. Speichern Sie die Änderungen an allen Dateien.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zur Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** , und legen Sie Sie in der Ansicht ab: 

    [![](buttons-images/storyboard01vs.png "A button")](buttons-images/storyboard01vs.png#lightbox)
1. Im **Eigenschaften-Explorer**können Sie mehrere Eigenschaften der Schaltfläche anpassen, z. b. **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02vs.png "The Properties Explorer")](buttons-images/storyboard02vs.png#lightbox)
1. Wechseln Sie als nächstes zur **Registerkarte Ereignisse** , und richten Sie ein **Ereignis** über die **Schaltfläche** ein, und nennen Sie es `ButtonPressed` : 

    [![](buttons-images/storyboard03vs.png "The Events Tab")](buttons-images/storyboard03vs.png#lightbox)
1. Speichern Sie die Änderungen an allen Dateien.

Bearbeiten Sie die Ansichts Controller `ViewController.cs` Datei (Beispieldatei), und fügen Sie den folgenden Code hinzu, um die ausgewählte Schaltfläche zu behandeln:

```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Solange die-Eigenschaft einer Schaltfläche `Enabled` ist `true` und Sie nicht von einem anderen Steuerelement oder einer anderen Ansicht abgedeckt wird, kann Sie mithilfe von Siri Remote als Element im Fokus erstellt werden. Wenn der Benutzer die Schaltfläche auswählt und auf die Berührungs Oberfläche klickt, `ButtonPressed` wird die oben definierte Aktion ausgeführt.

> [!IMPORTANT]
> Obwohl es möglich ist, bei `TouchUpInside` `UIButton` der Erstellung eines **Ereignis Handlers**Aktionen wie einem im IOS-Designer zuzuweisen, wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Beim Erstellen von **Aktionen** für tvos-Benutzeroberflächen Elemente sollte immer der Standard **Aktionstyp** verwendet werden.

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code"></a>

## <a name="buttons-and-code"></a>Schaltflächen und Code

Optional kann ein `UIButton` in c#-Code erstellt und der Ansicht der tvos-app hinzugefügt werden. Beispiel:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

Wenn Sie ein neues `UIButton` im Code erstellen, geben Sie dessen `UIButtonType` als einen der folgenden an:

- **System** : Dies ist der Standardtyp der von tvos dargestellten Schaltfläche und ist der Typ, den Sie am häufigsten verwenden werden.
- **Detaildisclosure** : zeigt den Typ der Schaltfläche "Ausschalten" an, die verwendet wird, um ausführliche Informationen auszublenden oder anzuzeigen.
- **Infodark** : eine dunkle, ausführliche infoschaltfläche hat ein "i" in einem Kreis angezeigt.
- **Infolight** : eine helle ausführliche infoschaltfläche, die ein "i" in einem Kreis angezeigt wird.
- **AddContact** : zeigt die Schaltfläche als Schaltfläche zum Hinzufügen eines Kontakts an.
- **Benutzer** definiert: Hiermit können Sie verschiedene Merkmale der Schaltfläche anpassen.

Als nächstes definieren Sie die Größe und den Speicherort der Schaltfläche auf dem Bildschirm. Beispiel:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Legen Sie dann den Titel für die Schaltfläche fest. `UIButtons`unterscheiden sich von den meisten Steuer `UIKit` Elementen darin, dass Sie einen-Zustand aufweisen, sodass Sie den Titel nicht einfach ändern können, sondern Sie müssen ihn für eine bestimmte ändern `UIControlState` . Beispiel:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Verwenden Sie als nächstes das- `AllEvents` Ereignis, um zu sehen, wann der Benutzer auf die Schaltfläche geklickt hat. Beispiel:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Zum Schluss fügen Sie der Ansicht die Schaltfläche hinzu, um Sie anzuzeigen:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Obwohl es möglich ist, Aktionen wie `TouchUpInside` zu einem zuzuweisen `UIButton` , wird es nie aufgerufen, da Apple TV keinen Touchscreen hat oder touchereignisse unterstützt. Sie sollten immer Ereignisse wie z. b. **allevents** oder **primaryaktionauslösen**verwenden.

<a name="Styling-a-Button"></a>

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

tvos bietet mehrere Eigenschaften von `UIButton` , die verwendet werden können, um den Titel und den Stil mit Dingen wie Hintergrundfarbe und Bildern bereitzustellen.

<a name="Button-Titles"></a>

### <a name="button-titles"></a>Schaltflächen Titel

Wie bereits erwähnt, `UIButtons` unterscheiden sich von den meisten Steuer `UIKit` Elementen darin, dass Sie über einen Zustand verfügen, sodass Sie den Titel nicht einfach ändern können, sondern Sie müssen ihn für eine bestimmte ändern `UIControlState` . Beispiel:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Mithilfe der-Methode können Sie die Titel Farbe für die Schaltfläche Festlegen `SetTitleColor` . Beispiel:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Und Sie können den Schatten des Titels mithilfe von anpassen `SetTitleShadowColor` . Beispiel:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Wenn die Schaltfläche mit folgendem Code markiert wird *, können* Sie den Titel Schatten so *festlegen, dass* er von der einschaltfläche in einen Wert geändert wird:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Außerdem können Sie attributierten Text als Titel der Schaltfläche verwenden. Beispiel:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Schaltflächen Bilder

Einem `UIButton` kann ein Bild angefügt sein, und es kann ein Bild als Hintergrund verwendet werden.

Um das Hintergrundbild einer Schaltfläche für einen angegebenen festzulegen `UIControlState` , verwenden Sie den folgenden Code:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Legen Sie die- `AdjustsImageWhenHiglighted` Eigenschaft auf fest `true` , um das Bild heller zu zeichnen, wenn die Schaltfläche markiert wird (Dies ist die Standardeinstellung). Legen Sie die- `AdjustsImageWhenDisabled` Eigenschaft auf fest `true` , um das Bild dunkler zu zeichnen, wenn die Schaltfläche deaktiviert ist (Dies ist auch der Standardwert).

Um das auf der Schaltfläche angezeigte Bild festzulegen, verwenden Sie den folgenden Code:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Verwenden Sie die- `TintColor` Eigenschaft, um ein farbtint festzulegen, das sowohl auf den Titel als auch auf das Bild der Schaltfläche angewendet wird. Für Schaltflächen des `Custom` Typs hat diese Eigenschaft keine Auswirkung. Sie müssen das `TintColor` Verhalten selbst implementieren.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Schaltflächen in einer xamarin. tvos-App behandelt. Es wurde gezeigt, wie mit Schaltflächen im IOS-Designer gearbeitet wird und wie Schaltflächen in c#-Code erstellt werden. Schließlich haben Sie erfahren, wie Sie den Titel einer Schaltfläche ändern und den Stil und die Darstellung ändern können.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
