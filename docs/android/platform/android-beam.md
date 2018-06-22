---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 89e668b8936db9a05fca2353b334b630b8363a74
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762735"
---
# <a name="android-beam"></a>Android Beam

Android Balken ist eine Near Field Communication (NFC)-Technologie, die in Android 4.0, die Clientanwendungen ermöglicht, Freigeben von Informationen über NFC in der Nähe eingeführt.

[![Diagramm zur Veranschaulichung zwei Geräte in der Nähe, Informationen gemeinsam nutzen](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android Balken funktioniert, indem Nachrichten über NFC übertragen, wenn zwei Geräte im Bereich befinden. Geräte ca. 4cm voneinander können Daten mithilfe von Android Balken freigeben. Eine Aktivität auf einem Gerät erstellt eine Nachricht und gibt an, eine Aktivität (oder Aktivitäten), können verarbeiten, legt es. Wenn die angegebene Aktivität im Vordergrund ist und die Geräte im Bereich werden, wird Android Balken die Nachricht mit dem zweiten Gerät per Push übertragen. Priorität wird aufgerufen, die die Nachrichtendaten enthält, auf dem Gerät empfangen.

Android unterstützt zwei Arten von Einstellung Nachrichten mit Android Balken:

-   `SetNdefPushMessage` – Bevor Android Balken initiiert wird, kann eine Anwendung aufrufen, SetNdefPushMessage zum Angeben einer NdefMessage Push über NFC und die Aktivität, die sie per Push übertragen wird. Dieser Mechanismus wird am besten verwendet werden, wenn eine Nachricht nicht ändern, während eine Anwendung verwendet wird.

-   `SetNdefPushMessageCallback` – Wenn Sie Android Balken initiiert wird, kann eine Anwendung einen Rückruf zum Erstellen einer NdefMessage behandeln. Dieser Mechanismus erlaubt, für die Erstellung der Nachricht verzögert wird, bis Sie Geräte im Bereich befinden. Szenarien, in dem die Nachricht variieren, basierend auf in der Anwendung geschieht unterstützt.


In beiden Fällen zum Senden von Daten mit Android Balken, eine Anwendung sendet eine `NdefMessage`, verpacken die Daten in mehreren `NdefRecords`. Werfen wir einen Blick auf die wichtigsten Punkte, die behoben werden müssen, bevor Android Balken ausgelöst werden kann. Erstens arbeiten wir mit dem Rückruf-Stil zum Erstellen einer `NdefMessage`.


## <a name="creating-a-message"></a>Erstellen einer Nachricht

Wir können registrieren, Rückrufe mit einer `NfcAdapter` in der Aktivitätssymbols `OnCreate` Methode. Vorausgesetzt, z. B. ein `NfcAdapter` mit dem Namen `mNfcAdapter` wird deklariert als Klassenvariable in der Aktivität, können Sie den folgenden Code aus, um den Rückruf zu erstellen, die die Nachricht zu erstellen, wird schreiben:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Die Aktivität mit dem implementiert `NfcAdapter.ICreateNdefMessageCallback`, übergeben wird, um die `SetNdefPushMessageCallback` oben genannten Methode. Wenn Android Balken initiiert wird, wird das System Aufrufen `CreateNdefMessage`, von dem die Aktivität der Währungsumrechnungsfunktionen erstellen kann ein `NdefMessage` wie unten dargestellt:

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

Auf der Empfängerseite Ruft das System die Priorität mit dem `ActionNdefDiscovered` Aktion, die aus der wir das NdefMessage wie folgt extrahieren:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Ein vollständiges Codebeispiel, das Android Balken, ausgeführt wird, im folgenden Screenshot gezeigt verwendet finden Sie unter der [Android Balken Demo](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) in der Beispiel-Katalog.

[![Beispiel-Screenshots aus der Android Balken-demo](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>Verwandte Links

- [Android Balken Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
