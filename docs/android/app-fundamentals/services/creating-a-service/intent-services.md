---
title: Beabsichtigte Dienste in xamarin. Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: acf8824c7a575bca37301a409bdf6d5f42cca622
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488061"
---
# <a name="intent-services-in-xamarinandroid"></a>Beabsichtigte Dienste in xamarin. Android

Sowohl gestartete als auch gebundene Dienste werden im Haupt Thread ausgeführt. Dies bedeutet, dass ein Dienst die Arbeit asynchron ausführen muss, um die Leistung zu gewährleisten. Eine der einfachsten Möglichkeiten, dieses Problem zu beheben, liegt bei einem _Prozessor Muster_für workerwarteschlangen, bei dem die zu erledende Arbeit in einer Warteschlange platziert wird, die von einem einzelnen Thread bedient wird.

Der [`IntentService`](xref:Android.App.IntentService) ist eine Unterklasse der `Service` Klasse, die eine Android-spezifische Implementierung dieses Musters bereitstellt. Sie verwaltet die Warteschlangen Arbeit, startet einen Arbeits Thread, um die Warteschlange zu verarbeiten, und zieht Anforderungen aus der Warteschlange ab, die im Arbeits Thread ausgeführt werden. Ein `IntentService` wird in Ruhe genommen und entfernt den Arbeits Thread, wenn keine weiteren Aufgaben in der Warteschlange vorhanden sind.

Die Arbeit wird an die Warteschlange übermittelt, indem ein `Intent` erstellt und dann an die `StartService`-Methode `Intent` übergeben wird.

Es ist nicht möglich, die `OnHandleIntent` Methode `IntentService` zu stoppen oder zu unterbrechen, während Sie funktioniert. Aufgrund dieses Entwurfs sollte eine `IntentService` Zustands frei beibehalten werden, &ndash; Sie sich nicht auf eine aktive Verbindung oder Kommunikation mit dem Rest der Anwendung verlassen sollte. Ein-`IntentService` soll die Arbeitsanforderungen Status fähig verarbeiten.

Es gibt zwei Anforderungen an die Unterklassen `IntentService`:

1. Der neue Typ (erstellt durch Unterklassen `IntentService`) überschreibt nur die `OnHandleIntent`-Methode.
2. Der Konstruktor für den neuen Typ erfordert eine Zeichenfolge, die verwendet wird, um den Arbeits Thread zu benennen, der die Anforderungen verarbeitet. Der Name dieses Arbeitsthreads wird hauptsächlich verwendet, wenn die Anwendung debuggt wird.

Der folgende Code zeigt eine `IntentService` Implementierung mit der überschriebenen `OnHandleIntent`-Methode:

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

Die Arbeit wird an eine `IntentService` gesendet, indem ein `Intent` instanziiert und dann die [`StartService`](xref:Android.Content.Context.StartService*) -Methode mit dieser Absicht als Parameter aufgerufen wird. Die Absicht wird an den Dienst als Parameter in der `OnHandleIntent`-Methode übergeben. Dieser Code Ausschnitt ist ein Beispiel für das Senden einer Arbeits Anforderung an eine Absicht: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.PutPutExtra("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

Der `IntentService` kann die Werte aus der Absicht extrahieren, wie im folgenden Code Ausschnitt gezeigt:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");

    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```

## <a name="related-links"></a>Verwandte Themen

- [IntentService](xref:Android.App.IntentService)
- [StartService](xref:Android.Content.Context.StartService*)
