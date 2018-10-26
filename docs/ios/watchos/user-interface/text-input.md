---
title: Arbeiten mit WatchOS Texteingabe in Xamarin
description: Dieses Dokument beschreibt die WatchOS-Texteingabe in Xamarin. Es erläutert die PresentTextInputController Methode schreiben, nur-Text, Emojis und Diktat.
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 2092b12254008936f2c5b6a7d9dd610ff751e802
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122360"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Arbeiten mit WatchOS Texteingabe in Xamarin

Der Apple Watch-bietet keine Tastatur für Benutzer zur Eingabe von Text, aber einige Alternativen Watch-freundliche unterstützt:

- Wählen aus einer vordefinierten Liste von Optionen für Text,
- Siri Diktatmodus,
- Auswählen einer emoji
- Scribble-zeichenweise handschrifterkennung (eingeführt in WatchOS 3).

Der Simulator unterstützt derzeit keine Diktat, aber Sie können weiterhin testen, der Texteingabe des Controllers Weitere Optionen wie die Scribble, wie hier gezeigt:

![](text-input-images/textinput-sml.png "Testen die Scribble-option")

Um die Texteingabe in eine Watch-app zu akzeptieren:

1. Erstellen Sie ein Zeichenfolgenarray der vordefinierten Optionen.
2. Rufen Sie `PresentTextInputController` mit dem Array, ob Emoji oder nicht zulässig und ein `Action` , die aufgerufen wird, wenn der Benutzer abgeschlossen ist.
3. Testen Sie in der Abschlussaktion wird ausgeführt für das Ergebnis der Eingabe, und führen Sie die entsprechende Aktion in der app (möglicherweise durch das Festlegen einer Bezeichnung Textwerts).

Der folgende Codeausschnitt stellt drei vordefinierte Optionen für den Benutzer an:

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

- Diktatmodus,
- Scribble, oder
- aus einer vordefinierten Liste, die die Anwendung bereitstellt.

[![](text-input-images/plain-scribble-sml.png "Diktatmodus, Scribble oder aus einer vordefinierten Liste, die die app bereitstellt.")](text-input-images/plain-scribble.png#lightbox)

Das Ergebnis wird immer zurückgegeben, als ein `NSObject` , die umgewandelt werden kann, um eine `string`.

## <a name="emoji"></a>Emoji

Es gibt zwei Arten von Emoji:

- Reguläre Unicode-emoji
- Animierte Bilder

Wenn der Benutzer eine Unicode-Emoji auswählt, wird es als Zeichenfolge zurückgegeben.

Wenn ein animiertes Bild Emoji ausgewählt ist die `result` Handler enthält bei der Durchführung einer `NSData` -Objekt, das gewünschte Emoji enthält `UIImage`.

## <a name="accepting-dictation-only"></a>Akzeptieren nur Diktat

Den Benutzer direkt auf den Bildschirm Diktat nutzen, ohne Sie mit der Vorschläge (oder die Scribble-Option):

- übergeben Sie ein leeres Array für die Liste der Vorschläge, und
- Legen Sie `WatchKit.WKTextInputMode.Plain`.

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

Wenn der Benutzer erweitern, werden der Bildschirm sehen Sie sich der folgende Bildschirm angezeigt, die den Text enthält, wie sie verstanden werden (z. B. "Das ist ein Test") angezeigt:

![](text-input-images/dictation.png "Wenn der Benutzer erweitern, sehen Sie sich zeigt den Bildschirm an den Text wie er interpretiert werden kann")

Nachdem sie drücken Sie die **Fertig** Schaltfläche der Text zurückgegeben werden.



## <a name="related-links"></a>Verwandte Links

- [Apple Doc für Text und Bezeichnungen](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
