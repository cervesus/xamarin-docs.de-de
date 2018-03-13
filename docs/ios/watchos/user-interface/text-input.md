---
title: Arbeiten mit Texteingabe
ms.topic: article
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 170131a2449b37acfa411eeca54f7aa921b0d9e4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-text-input"></a>Arbeiten mit Texteingabe

Der Apple Watch bietet keine Tastatur für Benutzer zur Eingabe von Text, aber einige Alternativen Watch-freundliche unterstützt wird:

- In einer vordefinierten Liste von Optionen auszuwählen,
- Siri diktieren
- Bei der Auswahl einer emoji
- Scribble Buchstaben von Buchstaben handschrifterkennung (eingeführt in WatchOS 3).

Der Simulator unterstützt derzeit keine diktieren, aber Sie können dennoch testen die Texteingabe des Controllers andere Optionen, z. B. Scribble, wie hier gezeigt:

![](text-input-images/textinput-sml.png "Testen die Scribble-option")

Texteingabe in eine Watch-app zu akzeptieren:

1. Erstellen Sie ein Zeichenfolgenarray der vordefinierten Optionen.
2. Rufen Sie `PresentTextInputController` mit dem Array angibt, ob Emoji oder nicht zulassen und eine `Action` , die aufgerufen wird, wenn der Benutzer abgeschlossen ist.
3. Klicken Sie in die Abschlussaktion für das Ergebnis der Eingabe testen Sie, und ergreifen Sie entsprechende Maßnahmen, in der app (möglicherweise Textwert eine Bezeichnung festlegen).

Der folgende Codeausschnitt zeigt drei vordefinierte Optionen für den Benutzer an:

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

Die `WKTextInputMode` -Enumeration besitzt drei Werte:

- Nur
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Nur

Wenn der einfache Modus festgelegt ist, kann der Benutzer auswählen:

- Diktieren
- Scribble, oder
- aus einer vordefinierten Liste, die die Anwendung bereitstellt.

[![](text-input-images/plain-scribble-sml.png "Diktieren Scribble, oder aus einer vordefinierten Liste, das die app bereitstellt.")](text-input-images/plain-scribble.png#lightbox)

Das Ergebnis wird immer zurückgegeben, als ein `NSObject` , die umgewandelt werden kann, um eine `string`.

## <a name="emoji"></a>Emoji

Es gibt zwei Arten von Emoji:

- Reguläre Unicode-emoji
- Animierten Bildern

Wenn der Benutzer eine Unicode-Emoji auswählt, wird sie als Zeichenfolge zurückgegeben.

Wenn eine animierte Grafik Emoji ausgewählt ist die `result` bei der Durchführung Handler enthält ein `NSData` Objekt, das die Emoji enthält `UIImage`.

## <a name="accepting-dictation-only"></a>Akzeptieren nur diktieren

Den Benutzer direkt auf dem Bildschirm diktieren ausführen und Vorschläge (oder die Scribble-Option) wird nicht angezeigt:

- ein leeres Array für die Vorschlagsliste übergeben und
- Set `WatchKit.WKTextInputMode.Plain`.

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

Wenn der Benutzer gesehen wird, zeigt der Bildschirm Überwachen der folgende Bildschirm angezeigt, die den Text enthält, wie er im Klaren sind (z. B. "Das ist ein Test"):

![](text-input-images/dictation.png "Wenn der Benutzer spricht, überwachen zeigt den Bildschirm an den Text wie er im Klaren sind")

Sobald sie drücken Sie die **Fertig** Schaltfläche der Text zurückgegeben werden.



## <a name="related-links"></a>Verwandte Links

- [Apple Text und Bezeichnungen Doc](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
