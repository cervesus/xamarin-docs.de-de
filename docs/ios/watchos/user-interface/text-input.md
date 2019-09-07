---
title: Arbeiten mit watchos-Text Eingaben in xamarin
description: Dieses Dokument beschreibt die watchos-Texteingabe in xamarin. Es erläutert die presenttextinputcontroller-Methode, scribgend, Plain Text, Emojis und Diktat.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: a0e45c51ba5460da87b80f21d4e9e54c13deabde
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766778"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Arbeiten mit watchos-Text Eingaben in xamarin

Der Apple Watch stellt keine Tastatur für Benutzer zum Eingeben von Text bereit, unterstützt jedoch einige Überwachungs freundliche Alternativen:

- Auswahl aus einer vordefinierten Liste von Textoptionen
- Siri-Diktat,
- Auswählen eines Emoji
- Scribble Letter-by-Letter-Handschrifterkennung (eingeführt in watchos 3).

Der Simulator unterstützt derzeit kein Diktat, aber Sie können die anderen Optionen des Texteingabe Controllers, wie z. b. Scribble, auch noch testen, wie hier gezeigt:

![](text-input-images/textinput-sml.png "Testen der Scribble-Option")

So akzeptieren Sie Texteingaben in einer Watch-App:

1. Erstellen Sie ein Zeichen folgen Array mit vordefinierten Optionen.
2. Aufrufen `PresentTextInputController` mit dem Array, unabhängig davon, ob Emoji zulässig ist oder nicht `Action` , und ein, das aufgerufen wird, wenn der Benutzer beendet ist.
3. Testen Sie in der Abschlussaktion das Eingabe Ergebnis, und führen Sie die entsprechende Aktion in der APP aus (möglicherweise wird der Textwert einer Bezeichnung festgelegt).

Der folgende Code Ausschnitt stellt dem Benutzer drei vordefinierte Optionen zur Folge:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

Die `WKTextInputMode` -Enumeration hat drei Werte:

- Lin
- Zuordnung
- AllowAnimatedEmoji

## <a name="plain"></a>Lin

Wenn der Klartext festgelegt ist, kann der Benutzer Folgendes auswählen:

- Diktat
- Scribble oder
- aus einer vordefinierten Liste, die von der Anwendung bereitstellt wird.

[![](text-input-images/plain-scribble-sml.png "Diktat, Scribble oder eine vordefinierte Liste, die die APP bereitstellt")](text-input-images/plain-scribble.png#lightbox)

Das Ergebnis wird immer als `NSObject` zurückgegeben, das in einen `string`umgewandelt werden kann.

## <a name="emoji"></a>Emoji

Es gibt zwei Arten von emoji:

- Reguläres Unicode-Emoji
- Animierte Bilder

Wenn der Benutzer ein Unicode-Emoji auswählt, wird er als Zeichenfolge zurückgegeben.

Wenn ein animiertes Bild Emoji ausgewählt `result` ist, enthält der im Vervollständigungs Handler ein `NSData` Objekt, das das Emoji `UIImage`enthält.

## <a name="accepting-dictation-only"></a>Nur akzeptieren von Diktat

So nehmen Sie den Benutzer direkt auf den Bildschirm für die diktierung, ohne Vorschläge anzuzeigen (oder die Scribble-Option):

- übergeben Sie ein leeres Array für die Vorschlagsliste, und
- legen `WatchKit.WKTextInputMode.Plain`Sie fest.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Wenn der Benutzer spricht, zeigt der Bildschirm überwachen den folgenden Bildschirm an, der den Text enthält (z. b. "This is a Test"):

![](text-input-images/dictation.png "Wenn der Benutzer spricht, zeigt der Bildschirm \"überwachen\" den Text so an, wie er verstanden wurde.")

Wenn Sie auf die Schaltfläche " **done** " klicken, wird der Text zurückgegeben.

## <a name="related-links"></a>Verwandte Links

- [Dokument Text und Bezeichnungen von Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
