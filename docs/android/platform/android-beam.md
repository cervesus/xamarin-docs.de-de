---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 83fa64ca207358b712341e1923a3a9a67a449e1f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524734"
---
# <a name="android-beam"></a>Android Beam

Android-Beam ist eine NFC-Technologie (Near Field Communication), die in Android 4,0 eingeführt wurde und die es Anwendungen ermöglicht, Informationen über NFC zu teilen, wenn Sie sich in der Nähe

[![Diagramm zur Veranschaulichung zweier Geräte in unmittelbarer Nähe der Freigabe Informationen](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android-Strahl funktioniert, indem Nachrichten über NFC übertragen werden, wenn sich zwei Geräte im Bereich befinden. Geräte, die 4 cm voneinander sind, können Daten mit dem Android-Strahl gemeinsam verwenden. Eine Aktivität auf einem Gerät erstellt eine Meldung und gibt eine Aktivität (oder Aktivitäten) an, die die pushübertragung verarbeiten kann. Wenn sich die angegebene Aktivität im Vordergrund befindet und sich die Geräte im Gültigkeitsbereich befinden, überträgt der Android-Strahl die Nachricht per Push an das zweite Gerät. Auf dem empfangenden Gerät wird eine Absicht aufgerufen, die die Nachrichten Daten enthält.

Android unterstützt zwei Möglichkeiten zum Festlegen von Nachrichten mit Android-Strahl:

- `SetNdefPushMessage`-Bevor der Android-Strahl gestartet wird, kann eine Anwendung setndefpushmessage aufrufen, um eine ndefmessage anzugeben, die per Push über NFC übertragen werden soll, und die Aktivität, die Sie überträgt. Dieser Mechanismus wird am besten verwendet, wenn eine Nachricht nicht geändert wird, während eine Anwendung verwendet wird.

- `SetNdefPushMessageCallback`-Wenn der Android-Strahl initiiert wird, kann eine Anwendung einen Rückruf zum Erstellen einer ndefmessage verarbeiten. Dieser Mechanismus ermöglicht, dass die Nachrichten Erstellung verzögert wird, bis sich die Geräte im Gültigkeitsbereich befinden. Es unterstützt Szenarien, in denen die Nachricht je nach den Ereignissen in der Anwendung variieren kann.


In beiden Fällen sendet `NdefMessage`eine Anwendung zum Senden von Daten mit dem Android-Strahl eine, die die Daten in mehreren `NdefRecords`Paketen verpackt. Werfen wir einen Blick auf die wichtigsten Punkte, die behoben werden müssen, bevor wir den Android-Strahl lösen können. Zuerst arbeiten wir mit dem Rückruf Stil der Erstellung eines `NdefMessage`.


## <a name="creating-a-message"></a>Erstellen einer Nachricht

Wir können Rückrufe mit `NfcAdapter` in der- `OnCreate` Methode der Aktivität registrieren. Wenn z. b. `NfcAdapter` angenommen `mNfcAdapter` wird, dass ein mit dem Namen als Klassen Variable in der-Aktivität deklariert ist, können wir den folgenden Code schreiben, um den Rückruf zu erstellen, der die Nachricht erstellt:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Die Aktivität, die implementiert `NfcAdapter.ICreateNdefMessageCallback`, wird an die `SetNdefPushMessageCallback` oben genannte Methode weitergeleitet. Wenn der Android-Strahl initiiert wird, ruft `CreateNdefMessage`das System auf, von dem die-Aktivität wie unten gezeigt ein `NdefMessage` erstellen kann:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>Empfangen einer Nachricht

Auf der Empfangsseite Ruft das System eine Absicht mit der `ActionNdefDiscovered` Aktion auf, aus der wir die ndefmessage wie folgt extrahieren können:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Ein umfassendes Codebeispiel, das den Android-Strahl verwendet, der im folgenden Screenshot gezeigt wird, finden Sie in der [Android-Strahl-Demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) im Beispiel Katalog.

[![Beispiel-Screenshots aus der Android-Strahl-Demo](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>Verwandte Links

- [Android-Strahl-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
- [Einführung in Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4,0-Plattform](https://developer.android.com/sdk/android-4.0.html)
