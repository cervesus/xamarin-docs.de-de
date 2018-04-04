---
title: Intent Services in Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="intent-services-in-xamarinandroid"></a>Intent Services in Xamarin.Android

## <a name="intent-services-overview"></a>Beabsichtigte (Übersicht)

Beide gestartet und der Dienst ausgeführt wird, auf den Hauptthread, d. h. um Leistung zu halten, smooth, ein Dienst, die asynchron auszuführende Arbeit muss gebunden. Eine der einfachsten Möglichkeiten, dieses Problem zu verringern ist mit einem _Worker Warteschlange Prozessor Muster_, in dem der auszuführenden Arbeit in einer Warteschlange platziert wird, die über einen einzelnen Thread gerade gewartet wird. 

Die [ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) ist eine Unterklasse von der `Service` -Klasse, die eine bestimmte Android Implementierung dieses Muster bereitstellt. Verwaltet es Message Queueing Arbeit, ein Arbeitsthread bedienen die Warteschlange gestartet aktivieren und Deaktivieren der Warteschlange für den Arbeitsthread ausgeführt werden, wie Anforderungen. Ein `IntentService` wird – selbst beenden und den Arbeitsthread zu entfernen, wenn keine weitere Arbeit in der Warteschlange vorhanden ist.
 
Arbeit wird an die Warteschlange übermittelt, durch das Erstellen einer `Intent` und deren Übergabe, die `Intent` auf die `StartService` Methode.

Es ist nicht möglich, Anhalten oder Unterbrechen der `OnHandleIntent` Methode `IntentService` Zuhause ist. Aufgrund dieses Designs ein `IntentService` sollten zustandslose beibehalten &ndash; es sollten Sie sich nicht auf eine aktive Verbindung oder für die Kommunikation vom Rest der Anwendung. Ein `IntentService` statelessly der arbeitsanforderungen verarbeiten soll.

Es gibt zwei Anforderungen für die Erstellung von Unterklassen von `IntentService`:

1. Den neuen Typ (erstellt durch Unterklassen `IntentService`) nur überschreibt die `OnHandleIntent` Methode.
2. Der Konstruktor für den neuen Typ muss eine Zeichenfolge, die verwendet wird, um den Arbeitsthread zu benennen, der die Anforderungen behandelt. Der Name der dieser Arbeitsthread wird hauptsächlich verwendet, wenn die Anwendung zu debuggen.

Der folgende code zeigt eine `IntentService` mit der überschriebenen Implementierung `OnHandleIntent` Methode:

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

Arbeit wird gesendet, um eine `IntentService` durch Instanziieren einer `Intent` und dem anschließenden Aufrufen der [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) Methode mit diese Absicht als Parameter. Die Absicht wird an den Dienst übergeben werden, als Parameter in der `OnHandleIntent` Methode. Dieser Codeausschnitt ist ein Beispiel für eine arbeitsanforderung an Priorität gesendet: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

Die `IntentService` können extrahieren Sie die Werte aus den Zweck, wie in diesem Codeausschnitt gezeigt:  

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
