---
title: Arbeiten mit TvOS-Schaltflächen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Schaltflächen in einer TvOS-app mit Xamarin erstellt wurde. Es wird erläutert, wie mit den Schaltflächen im Code und in Storyboards zu arbeiten, und es wird untersucht, wie Sie eine Schaltfläche zu formatieren.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/07/2017
ms.openlocfilehash: 6d8fc1daaced24dccead78c4f9d0e5d0959b3755
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116341"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Arbeiten mit TvOS-Schaltflächen in Xamarin

Verwenden Sie eine Instanz von der `UIButton` Klasse, um eine Schaltfläche den Fokus erhalten kann, auswählbare in einem TvOS-Fenster zu erstellen. Wenn der Benutzer eine Schaltfläche auswählt, sendet er eine Aktionsnachricht, auf das Zielobjekt ermöglichen, Ihre Xamarin.tvOS-app-Antworten an den Benutzer die Eingabe.

[![](buttons-images/buttons01.png "Beispiel-Schaltflächen")](buttons-images/buttons01.png#lightbox)

Weitere Informationen zum Arbeiten mit Fokus, und Navigieren mit dem Remoterepository Siri, informieren Sie sich unsere [arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

<a name="About-Buttons" />

## <a name="about-buttons"></a>Informationen über Schaltflächen

In TvOS und Schaltflächen für app-spezifischen Aktionen verwendet werden und möglicherweise einen Titel, ein Symbol oder beides enthalten. Während der Benutzer navigiert der app Benutzeroberfläche mit dem [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), Fokus verlagert sich auf der angegebenen Schaltfläche somit Text- und Hintergrundfarben zu ändern. Ein Schatten wird auch auf die Schaltfläche zum Hinzufügen eines 3D-Effekt Dadurch steigen über den Rest der Benutzeroberfläche angezeigt werden angewendet.

[![](buttons-images/buttons01.png "Beispiel-Schaltflächen")](buttons-images/buttons01.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit Schaltflächen:

- **Verwenden Sie einen Titel oder ein Symbol** – bei der sowohl ein Symbol und ein Titel kann in eine Schaltfläche eingefügt werden, ist begrenzt versuchen Sie es also nicht, beide zu kombinieren.
- **Klar Mark destruktive Schaltflächen** : Wenn die Schaltfläche ein destruktiver ausführt, Aktion (z. B. beim Löschen einer Datei), markieren klar als solche mit Text und/oder ein Symbol. Schädlicher Aktionen sollten immer vorhanden. ein [Warnung](~/ios/tvos/user-interface/alerts.md) den Benutzer auffordert, die Aktion zu beschränken.
- **Keine Verwendung wieder Schaltflächen** -die Menü-Schaltfläche auf dem Remotecomputer Siri wird verwendet, um zum vorherigen Bildschirm zurückzukehren. Die einzige Ausnahme dieser Regel wird für In-App-Käufe oder schädlicher Aktionen, in denen eine **Abbrechen** Schaltfläche angezeigt werden soll.

Weitere Informationen zum Arbeiten mit Fokus und Navigation informieren Sie sich unsere [arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) Dokumentation.

<a name="Button-Icons" />

### <a name="button-icons"></a>Schaltfläche "-Symbole

Apple empfiehlt, dass Sie einfache, erkennbaren Images für Ihre Schaltflächensymbole verwenden. Kompliziert, als Symbole sind schwer zu erkennen, die auf einem TV-Bildschirm durch das Zimmer auf ein Sofa, also versuchen, die einfachste Darstellung möglich, die den Eindruck zu vermitteln. Wann immer möglich, verwenden von Standard, bekannten Bilder für Symbole (z. B. einer Lupe für die Suche).

<a name="Button-Titles" />

### <a name="button-titles"></a>Titel der Schaltfläche

Apple hat die folgenden Vorschläge aus, wenn die Titel für die Schaltflächen zu erstellen:

- **Beschreibender Text unter Symbole Schaltflächen anzeigen** – Wenn möglich, direkt löschen, beschreibenden Text unterhalb des Symbols für nur den Zweck der Schaltfläche für weitere Get Schaltflächen.
- **Verwenden Sie Verben "oder" Verb-Ausdrücke für den Titel** -klar die Aktion an, die platzieren, wenn der Benutzer auf die Schaltfläche klickt.
- **Titelformat Groß-/Kleinschreibung verwenden** – mit Ausnahme von Artikeln, Konjunktionen oder Präpositionen (vier Buchstaben oder weniger), jedes Wort, der den Titel der Schaltfläche sollte groß geschrieben werden.
- **Verwenden Sie einen kurzen, um den Punkt Titel** -verwenden Sie die kürzeste mögliche Sprache, um der Schaltfläche Aktion beschreiben.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Schaltflächen und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Schaltflächen in einer Xamarin.tvOS-app ist so fügen sie Benutzeroberfläches der app mit dem Xamarin-Designer für iOS hinzu.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)


1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** und legen Sie sie in der Ansicht: 

    [![](buttons-images/storyboard01.png "Eine Schaltfläche")](buttons-images/storyboard01.png#lightbox)
1. In der **Eigenschaften-Explorer**, können Sie mehrere Eigenschaften der Schaltfläche wie z. B. Anpassen der **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02.png "Eigenschaften von Schaltflächen")](buttons-images/storyboard02.png#lightbox)
1. Als Nächstes wechseln Sie zu der **Registerkarte "Ereignisse"** und über das Netzwerk ein **Ereignis** aus der **Schaltfläche** und nennen Sie sie `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Die Registerkarte \"Ereignisse\"")](buttons-images/storyboard03.png#lightbox)
1. Sie werden automatisch dahin umgeschaltet die `ViewController.cs` Ansicht Sie die neue Aktion in Ihrem Code mithilfe platzieren können der **einrichten** und **nach unten** Pfeiltasten: 

    [![](buttons-images/storyboard04.png "Platzieren eine neue Aktion in code")](buttons-images/storyboard04.png#lightbox)
1. Drücken Sie die **EINGABETASTE** um den Speicherort auszuwählen: 

    [![](buttons-images/storyboard05.png "Der Code-editor")](buttons-images/storyboard05.png#lightbox)
1. Speichern Sie die Änderungen auf alle Dateien an.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** und legen Sie sie in der Ansicht: 

    [![](buttons-images/storyboard01vs.png "Eine Schaltfläche")](buttons-images/storyboard01vs.png#lightbox)
1. In der **Eigenschaften-Explorer**, können Sie mehrere Eigenschaften der Schaltfläche wie z. B. Anpassen der **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02vs.png "Die Eigenschaften-Explorer")](buttons-images/storyboard02vs.png#lightbox)
1. Als Nächstes wechseln Sie zu der **Registerkarte "Ereignisse"** und über das Netzwerk ein **Ereignis** aus der **Schaltfläche** und nennen Sie sie `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Die Registerkarte \"Ereignisse\"")](buttons-images/storyboard03vs.png#lightbox)
1. Speichern Sie die Änderungen auf alle Dateien an.



Bearbeiten Sie Ihr View-Controller (Beispiel `ViewController.cs`)-Datei und fügen Sie den folgenden Code zum Behandeln der Schaltfläche ausgewählt wird:


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

So lange, wie einer Schaltfläche `Enabled` Eigenschaft `true` und nicht durch ein anderes Steuerelement oder die Sicht behandelt wird, das im Fokus-Element, das mit der entfernten Siri vorgenommen werden. Wenn der Benutzer die Schaltfläche klickt und die Touch-Oberfläche, die `ButtonPressed` oben definierte Aktion ausgeführt.

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Aktionen wie z. B. `TouchUpInside` auf eine `UIButton` im iOS-Designer beim Erstellen einer **-Ereignishandler**, es wird nie aufgerufen werden, da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Sie sollten immer die Standardeinstellung verwenden **Aktionstyp** Erstellung **Aktionen** für TvOS-Elemente der Benutzeroberfläche.




Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Schaltflächen und Code

Optional ein `UIButton` können erstellt werden, C# code und die TvOS-app-Ansicht hinzugefügt. Zum Beispiel:

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

Beim Erstellen einer neuen `UIButton` im Code Geben Sie die `UIButtonType` als eine der folgenden:

- **System** – Dies ist der Standardtyp der präsentiert von TvOS-Schaltfläche und der Typ, der am häufigsten verwenden werden.
- **DetailDisclosure** -zeigt einen "nach unten drehen"-Typ der Schaltfläche verwendet, um ausführliche Informationen anzeigen oder ausblenden.
- **InfoDark** -dunkelgrau detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **InfoLight** – ein Licht detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **AddContact** -Schaltfläche als eine Schaltfläche "Kontakt hinzufügen" angezeigt.
- **Benutzerdefinierte** -können Sie verschiedene Merkmale der Schaltfläche anpassen.

Als Nächstes definieren Sie die auf dem Bildschirm Größe und Position der Schaltfläche. Beispiel:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Legen Sie dann den Titel für die Schaltfläche aus. `UIButtons` unterscheiden sich von den meisten `UIKit` Steuerelemente, dass sie einen Zustand aufweisen, damit Sie einfach das ist nicht möglich, ändern Sie den Titel, müssen Sie so ändern sie für einen bestimmten `UIControlState`. Zum Beispiel:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Verwenden Sie als Nächstes die `AllEvents` Ereignis angezeigt, wenn der Benutzer auf die Schaltfläche geklickt hat. Beispiel:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Schließlich fügen Sie die Schaltfläche mit den an die Ansicht angezeigt:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Aktionen wie z. B. `TouchUpInside` zu einem `UIButton`, es wird nie aufgerufen werden, da Apple TV besitzt eine Fingereingabe Bildschirm oder Touch-Ereignissen zu unterstützen. Sie sollten immer Ereignisse verwenden, z. B. **AllEvents** oder **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

TvOS stellt mehrere Eigenschaften, die von einer `UIButton` , die verwendet werden kann, geben Sie den Titel und formatieren Sie es mit Dingen wie dem Hintergrund Farben und Bilder.

<a name="Button-Titles" />

### <a name="button-titles"></a>Titel der Schaltfläche

Wie oben beschrieben `UIButtons` unterscheiden sich von den meisten `UIKit` Steuerelemente, dass sie einen Zustand aufweisen, damit Sie einfach das ist nicht möglich, ändern Sie den Titel, müssen Sie so ändern sie für einen bestimmten `UIControlState`. Zum Beispiel:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Sie können festlegen, die Farbe für die Schaltfläche mit den `SetTitleColor` Methode. Zum Beispiel:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Und Sie können anpassen, dass des Titels des Schattenkopien mit der `SetTitleShadowColor`. Zum Beispiel:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Sie können festlegen, dass den Schatten Titel zum Ändern von *Engraved* zu *Relief* , wenn die Schaltfläche markiert wird, mit dem folgenden Code:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Darüber hinaus können Sie die attributierte Text als den Titel der Schaltfläche. Zum Beispiel:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Schaltfläche "-Images

Ein `UIButton` haben ein Image angefügt und können ein Bild als Hintergrund verwenden.

Das Hintergrundbild des eine Schaltfläche zum Festlegen einer bestimmten `UIControlState`, verwenden Sie den folgenden Code:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Legen Sie die `AdjustsImageWhenHiglighted` Eigenschaft `true` zum Zeichnen des Bilds heller, wenn Sie die Schaltfläche markiert wird (Dies ist die Standardeinstellung). Legen Sie die `AdjustsImageWhenDisabled` Eigenschaft `true` zum Zeichnen der dunkleren Image aus, wenn Sie die Schaltfläche deaktiviert ist (Dies ist wiederum die Standardeinstellung).

Um auf die Schaltfläche angezeigte Bild zu festzulegen, verwenden Sie den folgenden Code:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Verwenden der `TintColor` Eigenschaft, um eine Farbtons festzulegen, die sowohl den Titel und das Bild der Schaltfläche angewendet wird. Für Schaltflächen für die `Custom` Typ dieser Eigenschaft hat keine Auswirkungen, die Sie implementieren müssen die `TintColor` Verhalten sich selbst.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Schaltflächen innerhalb einer Xamarin.tvOS-app. Darüber hinaus wurde gezeigt, wie Sie mit den Schaltflächen in der iOS-Designer arbeiten sowie zum Erstellen von Schaltflächen in C# Code. Zum Schluss wird aufgezeigt, wie Sie eine Schaltfläche für den Titel ändern und die Formatvorlage und Darstellung.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
