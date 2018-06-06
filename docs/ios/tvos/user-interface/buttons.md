---
title: Arbeiten mit tvos. außerdem wurden Schaltflächen in Xamarin
description: Dieses Dokument beschreibt, wie mit den Schaltflächen in einer app für tvos. außerdem wurden mit Xamarin erstellten arbeiten. Es wird erläutert, wie mit den Schaltflächen im Code und in Storyboards arbeiten, und sie untersucht, wie auf eine Schaltfläche zu formatieren.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 3de732e9eee696ce21ffc5526afd44f29695a313
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789384"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Arbeiten mit tvos. außerdem wurden Schaltflächen in Xamarin

Eine Instanz der `UIButton` -Klasse zum Erstellen von schaltflächenstilen den Fokus erhalten kann, auswählbare in einem Fenster tvos. außerdem wurden. Wenn der Benutzer eine Schaltfläche auswählt, sendet er eine Aktionsnachricht auf das Zielobjekt Ihrer Xamarin.tvOS app reagieren, die dem Benutzer den eingabeendpunkt zulassen.

[![](buttons-images/buttons01.png "Beispiel-Schaltflächen")](buttons-images/buttons01.png#lightbox)

Weitere Informationen zum Arbeiten mit Fokus, und Navigieren mit der Remoteinstanz Siri finden Sie unter unsere [arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

<a name="About-Buttons" />

## <a name="about-buttons"></a>Informationen über Schaltflächen

In tvos. außerdem wurden Schaltflächen für app-spezifisches Aktionen verwendet werden und können einen Titel, ein Symbol oder beides enthalten. Wie der Benutzer der app-Benutzeroberfläche mithilfe navigiert der [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), Fokus verlagert sich auf die Schaltfläche mit den angegebenen somit Text- und Hintergrundfarben zu ändern. Ein Schatten wird auch auf die Schaltfläche "Hinzufügen eines 3D-Effekt somit steigen über den Rest der Benutzeroberfläche angezeigt" angewendet.

[![](buttons-images/buttons01.png "Beispiel-Schaltflächen")](buttons-images/buttons01.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit Schaltflächen:

- **Verwenden Sie einen Titel oder ein Symbol** – während der beiden ein Symbol und ein Titel in einer Schaltfläche eingeschlossen werden kann, ist begrenzt so versuchen Sie nicht, um beide zu kombinieren.
- **Deutlich destruktiven Schaltflächen** – Wenn die Schaltfläche destruktiv ausführt (z. B. das Löschen einer Datei), Aktion markieren klar als solche mit Text und Symbol. Destruktiv sollte immer vorhanden. eine [Warnung](~/ios/tvos/user-interface/alerts.md) den Benutzer auffordert, die Aktion zu beschränken.
- **Keine Verwendung wieder Schaltflächen** -auf der Remoteinstanz Siri die Menüschaltfläche wird verwendet, um zum vorherigen Bildschirm zurückzukehren. Die einzige Ausnahme dieser Regel wird für In-App-Einkäufe oder destruktiv, in denen ein **"Abbrechen"** Schaltfläche angezeigt werden soll.

Weitere Informationen zum Arbeiten mit einer Fokussierung und Navigation finden Sie unter unsere [arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) Dokumentation.

<a name="Button-Icons" />

### <a name="button-icons"></a>Schaltflächensymbole

Apple schlägt vor, Ihnen die Verwendung von einfachen, erkennbaren Bilder für Ihre Schaltflächensymbole. Übermäßig komplexe Symbole sind schwer zu erkennen, die auf einem Bildschirm TV über den Raum auf eine Couch, daher versuchen, verwenden Sie die einfachste Darstellung möglich, über das Konzept zu erhalten. Nach Möglichkeit Standard verwenden, ebenfalls wissen von Images für Symbole (z. B. für die Suche Vergrößerungsglas).

<a name="Button-Titles" />

### <a name="button-titles"></a>Schaltfläche Titel

Apple hat die folgenden Vorschläge aus, wenn die Titel für die Schaltflächen zu erstellen:

- **Beschreibender Text unten Symbole Schaltflächen anzeigen** – Wenn möglich, direkt deaktivieren, beschreibenden Text unterhalb des Symbols für nur den Zweck der Schaltfläche für weitere Get Schaltflächen.
- **Verwenden von Verben oder verbale Ausdrücke für den Titel** -deutlich Status die Aktion, die platzieren, wenn der Benutzer auf die Schaltfläche klickt.
- **Verwenden Sie Titelformatgroß-/** – mit Ausnahme von Artikeln, Konjunktionen oder Präpositionen (vier Buchstaben oder weniger), jedes Wort des Titels für die Schaltfläche sollte groß geschrieben werden.
- **Verwenden Sie einen kurzen, zu Punkt Titel** -verwenden Sie die kürzeste mögliche Fachsprache aus, um die Schaltfläche Aktion beschreiben.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Schaltflächen und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit Schaltflächen in einer app Xamarin.tvOS werden diese Benutzeroberfläche der Anwendung, die mit der Xamarin-Designer für iOS hinzufügen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)


1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** und legen Sie sie in der Sicht: 

    [![](buttons-images/storyboard01.png "Eine Schaltfläche")](buttons-images/storyboard01.png#lightbox)
1. In der **Eigenschaften-Explorer**, Sie können mehrere Eigenschaften der Schaltfläche z. B. Anpassen der **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02.png "Eigenschaften von Schaltflächen")](buttons-images/storyboard02.png#lightbox)
1. Als Nächstes wechseln Sie zu der **Registerkarte "Ereignisse"** und über das Netzwerk ein **Ereignis** aus der **Schaltfläche** und Aufruf der `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Die Registerkarte \"Ereignisse\"")](buttons-images/storyboard03.png#lightbox)
1. Sie werden automatisch auf verlagert die `ViewController.cs` anzeigen, wo Sie die neue Aktion in Ihrem Code platzieren können, die **einrichten** und **nach unten** Pfeiltasten: 

    [![](buttons-images/storyboard04.png "Platzieren eine neue Aktion im code")](buttons-images/storyboard04.png#lightbox)
1. Drücken Sie die **EINGABETASTE** um den Speicherort auszuwählen: 

    [![](buttons-images/storyboard05.png "Der Code-editor")](buttons-images/storyboard05.png#lightbox)
1. Speichern Sie die Änderungen auf alle Dateien.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung.
1. Ziehen Sie eine **Schaltfläche** aus der **Bibliothek** und legen Sie sie in der Sicht: 

    [![](buttons-images/storyboard01vs.png "Eine Schaltfläche")](buttons-images/storyboard01vs.png#lightbox)
1. In der **Eigenschaften-Explorer**, Sie können mehrere Eigenschaften der Schaltfläche z. B. Anpassen der **Titel** und **Textfarbe**: 

    [![](buttons-images/storyboard02vs.png "Im Eigenschaften-Explorer")](buttons-images/storyboard02vs.png#lightbox)
1. Als Nächstes wechseln Sie zu der **Registerkarte "Ereignisse"** und über das Netzwerk ein **Ereignis** aus der **Schaltfläche** und Aufruf der `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Die Registerkarte \"Ereignisse\"")](buttons-images/storyboard03vs.png#lightbox)
1. Speichern Sie die Änderungen auf alle Dateien.



Bearbeiten Sie die View-Controller (Beispiel `ViewController.cs`) Datei, und fügen Sie den folgenden Code zum Behandeln der Schaltfläche ausgewählt wird:


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

Solange einer Schaltfläche `Enabled` Eigenschaft ist `true` und fällt nicht von einem anderen Steuerelement oder Sicht, die im Fokus-Element, das mithilfe von Siri Remote vorgenommen werden. Wenn der Benutzer wählt die Schaltfläche aus und klickt auf die Oberfläche berühren der `ButtonPressed` oben definierte Aktion ausgeführt werden würde.

> [!IMPORTANT]
> Während es möglich ist, weisen Sie Aktionen wie z. B. `TouchUpInside` auf eine `UIButton` in der iOS-Designer beim Erstellen einer **Ereignishandler**, es wird nie aufgerufen werden, da Apple TV Fingereingabe Bildschirm oder Berührungsereignisse unterstützen keine. Sie sollten immer die Standardeinstellung verwenden **Aktionstyp** für die Erstellung **Aktionen** für tvos. außerdem wurden Elemente der Benutzeroberfläche.




Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Schaltflächen und Code

Optional ein `UIButton` im C#-Code erstellt und die tvos. außerdem wurden app Ansicht hinzugefügt werden können. Zum Beispiel:

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

Beim Erstellen einer neuen `UIButton` Geben Sie im Code, dessen `UIButtonType` als eines der folgenden:

- **System** -Dies ist der Standardtyp der Schaltfläche ", präsentiert von tvos. außerdem wurden, ist der Typ, die am häufigsten verwendet werden.
- **DetailDisclosure** -zeigt einen "nach unten drehen"-Typ der Schaltfläche "verwendet, um ausführliche Informationen anzeigen oder ausblenden.
- **InfoDark** -einem dunklen detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **InfoLight** -eine helle detaillierte Informationen, die Schaltfläche ein "i" in einem Kreis angezeigt.
- **AddContact** -Schaltfläche als eine Schaltfläche zum Hinzufügen von Kontakten angezeigt.
- **Benutzerdefinierte** -können Sie mehrere Merkmale der Schaltfläche anpassen.

Als Nächstes definieren Sie die auf dem Bildschirm Größe und Position der Schaltfläche. Beispiel:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Schalten Sie dann den Titel für die Schaltfläche aus. `UIButtons` unterscheiden sich von denen meisten `UIKit` Steuerelemente insofern, dass sie einen Status aufweisen, damit Sie einfach nur können nicht den Titel ändern, müssen Sie ändern, für einen angegebenen `UIControlState`. Zum Beispiel:

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

Schließlich fügen Sie die Schaltfläche, auf die Ansicht, um diese anzuzeigen:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> Es ist zwar möglich, weisen Sie Aktionen wie z. B. `TouchUpInside` auf eine `UIButton`, es wird nie aufgerufen werden, da Apple TV Fingereingabe Bildschirm oder Berührungsereignisse unterstützen keine. Sie sollten immer Ereignisse verwenden, z. B. **AllEvents** oder **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

tvos. außerdem wurden enthält verschiedene Eigenschaften einer `UIButton` , die verwendet werden kann, so geben Sie den Titel hinzu und formatieren es mit Dinge Hintergrund Farben und Bilder.

<a name="Button-Titles" />

### <a name="button-titles"></a>Schaltfläche Titel

Wie oben veranschaulichte Filtermenü `UIButtons` unterscheiden sich von denen meisten `UIKit` Steuerelemente insofern, dass sie einen Status aufweisen, damit Sie einfach nur können nicht, ändern Sie den Titel, müssen Sie ihn ändern einer bestimmten `UIControlState`. Zum Beispiel:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Sie können festlegen, dass die Farbe für die Schaltfläche mit den `SetTitleColor` Methode. Zum Beispiel:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

Und Sie können des Titels anpassen Shadowing mithilfe der `SetTitleShadowColor`. Zum Beispiel:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Sie können festlegen, dass den Schatten Titel zum Ändern von *Engraved* auf *Relief* markiert ist die Schaltfläche "" ist mit dem folgenden Code:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Darüber hinaus können Sie mit der Schaltfläche Titel attributierte Text. Zum Beispiel:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Bilder

Ein `UIButton` können ein Image angefügt und kann ein Bild als Hintergrund verwenden.

Festzulegende das Hintergrundbild des eine Schaltfläche für einen bestimmten `UIControlState`, verwenden Sie den folgenden Code:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Legen Sie die `AdjustsImageWhenHiglighted` Eigenschaft `true` heller Bild gezeichnet werden soll, wenn die Schaltfläche "" markiert ist (Dies ist die Standardeinstellung). Legen Sie die `AdjustsImageWhenDisabled` Eigenschaft `true` dunkler Bild gezeichnet werden soll, wenn die Schaltfläche deaktiviert ist (erneut, dies ist die Standardeinstellung).

Um das Bild angezeigt, auf die Schaltfläche "" festzulegen, verwenden Sie den folgenden Code ein:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Verwenden der `TintColor` Eigenschaft Farbton festlegen, die auf den Titel und das Bild der Schaltfläche angewendet wird. Für Schaltflächen für die `Custom` Typ dieser Eigenschaft hat keine Auswirkungen, die Sie implementieren müssen die `TintColor` Verhalten selbst.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Schaltflächen innerhalb einer Xamarin.tvOS-app. Arbeiten mit den Schaltflächen in der iOS-Designer und zum Erstellen von Schaltflächen in C#-Code bleiben. Schließlich wurde es gezeigt, wie eine Schaltfläche Titel ändern, und ändern Sie den Stil und die Darstellung.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
