---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: aab121ed5f811baf38eed48cf891ccdf076eaf44
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "76723812"
---
# <a name="android-beam"></a>Android Beam

Android Beam ist eine NFC-Technologie (Near Field Communication), die in Android 4.0 eingeführt wurde und die es Anwendungen ermöglicht, Informationen über NFC auszutauschen, wenn unmittelbare Nähe gegeben ist.

[![Diagramm zur Veranschaulichung zweier Geräte in unmittelbarer Nähe, die Informationen austauschen](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android Beam funktioniert, indem Nachrichten über NFC übertragen werden, wenn zwei Geräte in Reichweite sind. Geräte, die ca. 4 cm voneinander entfernt sind, können über Android Beam Daten austauschen. Eine Aktivität auf einem Gerät erstellt eine Nachricht und gibt eine Aktivität (oder Aktivitäten) an, die in der Lage ist, die Nachricht zu senden. Wenn sich die angegebene Aktivität im Vordergrund befindet und die Geräte in Reichweite sind, sendet Android Beam die Nachricht an das zweite Gerät. Auf dem empfangenden Gerät wird ein Intent-Objekt aufgerufen, das die Nachrichtendaten enthält.

Android unterstützt zwei Arten, Nachrichten mit Android Beam festzulegen:

- `SetNdefPushMessage`: Bevor Android-Beam gestartet wird, kann eine Anwendung „SetNdefPushMessage“ aufrufen, um eine „NdefMessage“-Nachricht, die über NFC gesendet werden soll, und die Aktivität anzugeben, von der die Nachricht gesendet wird. Dieser Mechanismus wird am besten verwendet, wenn sich eine Nachricht nicht ändert, während eine Anwendung genutzt wird.

- `SetNdefPushMessageCallback`: Wenn Android Beam gestartet wird, kann eine Anwendung einen Rückruf verarbeiten, um eine „NdefMessage“-Nachricht zu erstellen. Dieser Mechanismus ermöglicht es, die Erstellung von Nachrichten zu verzögern, bis sich die Geräte in Reichweite befinden. Hiermit werden Szenarien unterstützt, in denen die Nachricht je nach dem, was in der Anwendung geschieht, variieren kann.

In beiden Fällen sendet eine Anwendung, damit Daten mit Android Beam gesendet werden können, eine `NdefMessage`-Nachricht, wobei die Daten in mehrere `NdefRecords` verpackt sind. Es folgen die wichtigsten Punkte, die erfüllt sein müssen, bevor Android Beam ausgelöst werden kann. Zunächst soll das Erstellen einer `NdefMessage`-Nachricht mit der Rückrufmethode erfolgen.

## <a name="creating-a-message"></a>Erstellen einer Nachricht

Rückrufe können mit einem `NfcAdapter`-Objekt in der `OnCreate`-Methode der Aktivität registriert werden. Angenommen, ein `NfcAdapter`-Objekt namens `mNfcAdapter` ist als Klassenvariable in der Aktivität deklariert. Dann kann der folgende Code geschrieben werden, um den Rückruf zu erstellen, der die Nachricht erstellt:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Die-Aktivität, in der `NfcAdapter.ICreateNdefMessageCallback` implementiert ist, wird an die obige `SetNdefPushMessageCallback`-Methode übergeben. Wenn Android Beam gestartet wird, ruft das System die `CreateNdefMessage`-Methode auf, aus der die Aktivität eine `NdefMessage`-Nachricht erstellen kann, wie nachstehend gezeigt:

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

Auf der empfangenden Seite ruft das System ein Intent-Objekt mit der `ActionNdefDiscovered`-Aktion auf, aus der die „NdefMessage“-Nachricht wie folgt extrahiert werden kann:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Ein umfassendes Codebeispiel, in dem Android Beam verwendet wird und dessen Ergebnis im folgenden Screenshot gezeigt ist, finden Sie in der [Android Beam-Demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) im Beispielkatalog.

[![Beispielscreenshots für die Android Beam-Demo](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Android Beam-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
