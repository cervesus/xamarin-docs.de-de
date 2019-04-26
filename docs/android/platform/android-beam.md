---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 9fcabc90875dda28ecdd5d94f1ca2f263ffe4886
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60954191"
---
# <a name="android-beam"></a>Android Beam

Android-Funktionen handelt es sich um eine Near Field Communication (NFC)-Technologie, die in Android 4.0, die Clientanwendungen ermöglicht, Freigeben von Informationen über NFC in der Nähe eingeführt.

[![Diagramm zur Veranschaulichung zwei Geräte in unmittelbarer Nähe zueinander, die Freigabe von Informationen](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android Balken funktioniert, indem Nachrichten über NFC übertragen werden, wenn zwei Geräte im Bereich befinden. Geräte ungefähr 4cm voneinander können Daten mithilfe von Android Balken freigeben. Eine Aktivität auf einem Gerät eine Nachricht erstellt und gibt an, eine Aktivität (oder Aktivitäten), die mithilfe von Push übertragen sie verarbeiten können. Wenn die angegebene Aktivität im Vordergrund ist, und die Geräte im Bereich sind, pushen Android Balken die Nachricht auf das zweite Gerät. Ein Intent-Objekt wird aufgerufen, die die Daten der Nachricht enthält, auf dem Gerät empfangen.

Android unterstützt zwei Arten von Einstellung-Nachrichten mit Android-Funktionen:

-   `SetNdefPushMessage` – Bevor Android Balken initiiert wird, kann eine Anwendung aufrufen, SetNdefPushMessage zum Angeben einer NdefMessage mithilfe von Push übertragen über NFC und die Aktivität, die sie mithilfe von Push übertragen wird. Dieser Mechanismus wird am besten verwendet werden, wenn eine Nachricht nicht ändert, während eine Anwendung verwendet wird.

-   `SetNdefPushMessageCallback` – Wenn Sie Android Balken initiiert wird, kann eine Anwendung einen Rückruf zum Erstellen einer NdefMessage behandeln. Dieser Mechanismus ermöglicht es, für die Erstellung der Nachricht verzögert werden, bis der Geräte im Bereich befinden. Es unterstützt Szenarien, in dem die Nachricht variieren, basierend auf der Abläufe in der Anwendung.


In beiden Fällen zum Senden von Daten mit Android Balken, eine Anwendung sendet eine `NdefMessage`, verpacken die Daten in mehreren `NdefRecords`. Werfen wir einen Blick auf die wichtigsten Punkte, die berücksichtigt werden müssen, bevor wir Android Funktionen ausgelöst werden kann. Zunächst versuchen wir in Zusammenarbeit mit dem Rückruf-Stil zum Erstellen einer `NdefMessage`.


## <a name="creating-a-message"></a>Erstellen einer Nachricht

Wir können Registrieren von Rückrufen mit einer `NfcAdapter` Aktivität `OnCreate` Methode. Angenommen ein `NfcAdapter` mit dem Namen `mNfcAdapter` wird als Klassenvariable deklariert, eine in der Aktivität, Schreiben wir den folgenden Code, um den Rückruf zu erstellen, die die Nachricht erstellt wird:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Die Aktivität, die implementiert `NfcAdapter.ICreateNdefMessageCallback`, übergeben wird, um die `SetNdefPushMessageCallback` oben genannte Methode. Wenn Android Balken initiiert wird, kann das System ruft `CreateNdefMessage`, von dem die Aktivität erstellen, können eine `NdefMessage` wie unten dargestellt:

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

Auf der Empfängerseite Ruft das System einen Intent mit dem `ActionNdefDiscovered` Aktion, die aus dem können wir wie folgt die NdefMessage extrahieren:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Ein vollständiges Codebeispiel, das Android Funktionen, die Ausführung im nachstehenden Screenshot dargestellt verwendet finden Sie unter den [Android Balken Demo](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) im Katalog-Beispiel.

[![Beispiel-Screenshot aus der Funktionen der Android-demo](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>Verwandte Links

- [Android-Balken-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](https://developer.android.com/sdk/android-4.0.html)
