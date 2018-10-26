---
title: Intent-Dienste in Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1301f34ad1f7a0069c542ba81bf237a673fd239d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112857"
---
# <a name="intent-services-in-xamarinandroid"></a>Intent-Dienste in Xamarin.Android

## <a name="intent-services-overview"></a>Übersicht über die Intent-Dienste

Beide Schritte und gebundene Dienste führen Sie auf den Hauptthread, was bedeutet, dass ein Dienst um Leistung smooth beizubehalten, die Arbeit asynchron auszuführen muss. Eine der einfachsten Möglichkeiten, um dieses Problem zu beheben ist mit einem _Muster "Prozessor" Worker_, in dem die zu erledigenden Arbeit in einer Warteschlange platziert wird, die von einem einzelnen Thread verarbeitet wird. 

Die [ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) ist eine Unterklasse von der `Service` Klasse, die eine Android-spezifische Implementierung dieses Musters bereitstellt. Verwaltet Arbeit Queueing service die Warteschlange, ein Arbeitsthread gestartet aktivieren und Deaktivieren der Warteschlange für den Arbeitsthread ausgeführt werden, Abrufen von Anforderungen. Ein `IntentService` unauffällig selbst beendet und den Arbeitsthread zu entfernen, wenn keine weiteren Aufgaben in der Warteschlange vorhanden ist.
 
Arbeit an die Warteschlange gesendet wird, durch das Erstellen einer `Intent` und deren Übergabe, die `Intent` auf die `StartService` Methode.

Es ist nicht möglich, beenden oder unterbrechen die `OnHandleIntent` Methode `IntentService` und sie arbeitet. Aufgrund dieses Designs ein `IntentService` zustandslosen beibehalten werden sollen &ndash; es, auf eine aktive Verbindung oder Kommunikation mit dem Rest der Anwendung nicht empfehlenswert. Ein `IntentService` statelessly arbeitsanforderungen verarbeiten soll.

Es gibt zwei Anforderungen für Unterklassen `IntentService`:

1. Der neue Typ (erstellt, indem Unterklassen `IntentService`) nur Außerkraftsetzungen der `OnHandleIntent` Methode.
2. Der Konstruktor für den neuen Typ muss es sich um eine Zeichenfolge, die verwendet wird, um den Arbeitsthread zu benennen, der die Anforderungen behandelt. Der Name der dieser Arbeitsthread wird hauptsächlich beim Debuggen der Anwendung.

Der folgende code zeigt eine `IntentService` -Implementierung mit der überschriebenen `OnHandleIntent` Methode:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Arbeit wird gesendet, um eine `IntentService` durch Instanziieren einer `Intent` und dem anschließenden Aufrufen der [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) -Methode mit diese Absicht als Parameter. Die Absicht wird an den Dienst übergeben werden, als Parameter in der `OnHandleIntent` Methode. Dieser Codeausschnitt ist ein Beispiel für eine Aufgaben-Anforderung an einen Intent gesendet: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

Die `IntentService` können die Werte von der Absicht, extrahieren, wie im folgenden Codeausschnitt gezeigt:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>Verwandte Links

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
