---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: aab121ed5f811baf38eed48cf891ccdf076eaf44
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723812"
---
# <a name="android-beam"></a>Android Beam

Android-Beam ist eine NFC-Technologie (Near Field Communication), die in Android 4,0 eingeführt wurde und die es Anwendungen ermöglicht, Informationen über NFC zu teilen, wenn Sie sich in der Nähe

[![Diagramm, das zwei Geräte in unmittelbarer Nähe der Freigabe Informationen veranschaulicht](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android-Strahl funktioniert, indem Nachrichten über NFC übertragen werden, wenn sich zwei Geräte im Bereich befinden. Geräte, die 4 cm voneinander sind, können Daten mit dem Android-Strahl gemeinsam verwenden. Eine Aktivität auf einem Gerät erstellt eine Meldung und gibt eine Aktivität (oder Aktivitäten) an, die die pushübertragung verarbeiten kann. Wenn sich die angegebene Aktivität im Vordergrund befindet und sich die Geräte im Gültigkeitsbereich befinden, überträgt der Android-Strahl die Nachricht per Push an das zweite Gerät. Auf dem empfangenden Gerät wird eine Absicht aufgerufen, die die Nachrichten Daten enthält.

Android unterstützt zwei Möglichkeiten zum Festlegen von Nachrichten mit Android-Strahl:

- `SetNdefPushMessage`: bevor der Android-Strahl initiiert wird, kann eine Anwendung setndefpushmessage aufrufen, um eine ndefmessage anzugeben, die per Push über NFC übertragen werden soll, und die Aktivität, die Sie überträgt. Dieser Mechanismus wird am besten verwendet, wenn eine Nachricht nicht geändert wird, während eine Anwendung verwendet wird.

- `SetNdefPushMessageCallback`: Wenn Android-Strahl initiiert wird, kann eine Anwendung einen Rückruf zum Erstellen einer ndefmessage verarbeiten. Dieser Mechanismus ermöglicht, dass die Nachrichten Erstellung verzögert wird, bis sich die Geräte im Gültigkeitsbereich befinden. Es unterstützt Szenarien, in denen die Nachricht je nach den Ereignissen in der Anwendung variieren kann.

In beiden Fällen sendet eine Anwendung zum Senden von Daten mit dem Android-Strahl eine `NdefMessage`, wobei die Daten in mehreren `NdefRecords`verpackt werden. Werfen wir einen Blick auf die wichtigsten Punkte, die behoben werden müssen, bevor wir den Android-Strahl lösen können. Zuerst arbeiten wir mit dem Rückruf Stil der Erstellung eines `NdefMessage`.

## <a name="creating-a-message"></a>Erstellen einer Nachricht

Wir können Rückrufe mit einer `NfcAdapter` in der `OnCreate`-Methode der Aktivität registrieren. Wenn z. b. angenommen wird, dass eine `NfcAdapter` mit dem Namen `mNfcAdapter` als Klassen Variable in der-Aktivität deklariert ist, können wir den folgenden Code schreiben, um den Rückruf zu erstellen, mit dem die Nachricht erstellt wird:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Die-Aktivität, die `NfcAdapter.ICreateNdefMessageCallback`implementiert, wird an die obige `SetNdefPushMessageCallback`-Methode weitergeleitet. Wenn Android-Strahl initiiert wird, ruft das System `CreateNdefMessage`auf, von dem die Aktivität eine `NdefMessage` erstellen kann, wie unten dargestellt:

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
