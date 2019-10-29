---
title: Arbeiten mit watchos-Text Eingaben in xamarin
description: Dieses Dokument beschreibt die watchos-Texteingabe in xamarin. Es erläutert die presenttextinputcontroller-Methode, scribgend, Plain Text, Emojis und Diktat.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 156a31e37d14ce3e3cbe7173ae97b608e9d4c32e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032654"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Arbeiten mit watchos-Text Eingaben in xamarin

Der Apple Watch stellt keine Tastatur für Benutzer zum Eingeben von Text bereit, unterstützt jedoch einige Überwachungs freundliche Alternativen:

- Auswahl aus einer vordefinierten Liste von Textoptionen
- Siri-Diktat,
- Auswählen eines Emoji
- Scribble Letter-by-Letter-Handschrifterkennung (eingeführt in watchos 3).

Der Simulator unterstützt derzeit kein Diktat, aber Sie können die anderen Optionen des Texteingabe Controllers, wie z. b. Scribble, auch noch testen, wie hier gezeigt:

![](text-input-images/textinput-sml.png "Testing the scribble option")

So akzeptieren Sie Texteingaben in einer Watch-App:

1. Erstellen Sie ein Zeichen folgen Array mit vordefinierten Optionen.
2. Aufrufen von `PresentTextInputController` mit dem Array, ob Emoji zulässig ist oder nicht, und eine `Action`, die aufgerufen wird, wenn der Benutzer fertig ist.
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

Die `WKTextInputMode`-Enumeration hat drei Werte:

- Lin
- Zuordnung
- "Zubei Zuweisung"

## <a name="plain"></a>Lin

Wenn der Klartext festgelegt ist, kann der Benutzer Folgendes auswählen:

- Diktat
- Scribble oder
- aus einer vordefinierten Liste, die von der Anwendung bereitstellt wird.

[![](text-input-images/plain-scribble-sml.png "Dictation, Scribble, or from a pre-defined list that the app supplies")](text-input-images/plain-scribble.png#lightbox)

Das Ergebnis wird immer als `NSObject` zurückgegeben, das in eine `string`umgewandelt werden kann.

## <a name="emoji"></a>Emoji

Es gibt zwei Arten von emoji:

- Reguläres Unicode-Emoji
- Animierte Bilder

Wenn der Benutzer ein Unicode-Emoji auswählt, wird er als Zeichenfolge zurückgegeben.

Wenn ein animiertes Bild Emoji ausgewählt ist, enthält der `result` im Abschluss Handler ein `NSData` Objekt, das den Emoji-`UIImage`enthält.

## <a name="accepting-dictation-only"></a>Nur akzeptieren von Diktat

So nehmen Sie den Benutzer direkt auf den Bildschirm für die diktierung, ohne Vorschläge anzuzeigen (oder die Scribble-Option):

- übergeben Sie ein leeres Array für die Vorschlagsliste, und
- Legen Sie `WatchKit.WKTextInputMode.Plain`fest.

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

![](text-input-images/dictation.png "When the user is speaking, the watch screen displays the text as it is understood")

Wenn Sie auf die Schaltfläche " **done** " klicken, wird der Text zurückgegeben.

## <a name="related-links"></a>Verwandte Links

- [Dokument Text und Bezeichnungen von Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
