---
title: Threading in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie die System.Threading-APIs in einer Xamarin.iOS-Anwendung verwendet wird. Es wird erläutert, die Task Parallel Library, Erstellen von reaktionsfähigen und Garbagecollection.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: 7dbb0044f09d5bc00f2393eb647efba05a061c3f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61399828"
---
# <a name="threading-in-xamarinios"></a>Threading in Xamarin.iOS

Die Xamarin.iOS-Runtime bietet Entwicklern Zugriff auf die .NET threading-APIs, sowohl explizit bei der Verwendung von Threads (`System.Threading.Thread, System.Threading.ThreadPool`) und implizit, wenn Sie mit der asynchronen Delegaten Muster oder BeginXXX-Methoden sowie die vollständige Auswahl von APIs, die Unterstützung der Task Parallel Library.



Xamarin empfiehlt dringend die Verwendung der [Task Parallel Library](https://msdn.microsoft.com/library/dd460717.aspx) (TPL) zum Erstellen von Anwendungen für verschiedene Gründe haben:
-  Der Standard-Scheduler TPL delegiert Ausführung der Aufgabe an den Threadpool, die wiederum dynamisch, die Anzahl der Threads, die nicht benötigt ausgeweitet wird, da der Vorgang stattfindet und zugleich ein Szenario, in denen zu viele Threads CPU-Zeit konkurrieren letztlich. 
-  Es ist einfacher zu bedenken, Vorgänge in Bezug auf die TPL-Tasks. Sie können ganz einfach bearbeiten diese, Planen sie, ihre Ausführung Serialisieren oder viele parallel mithilfe eines umfangreichen Satzes von APIs zu starten. 
-  Es ist die Grundlage für die Programmierung mit den neuen c# Async spracherweiterungen. 


Der Threadpool wächst langsam die Anzahl der Threads nach Bedarf basierend auf der Anzahl der CPU-Kerne zur Verfügung, auf das System, die Systemlast und die Anwendung bei steigendem Bedarf. Sie können diesen Threadpool verwenden, entweder durch Aufrufen von Methoden in `System.Threading.ThreadPool` oder unter Verwendung des standardmäßigen `System.Threading.Tasks.TaskScheduler` (Teil der *parallele Frameworks*).

In der Regel verwenden Entwickler Threads aus, wenn sie reaktionsfreudige Anwendungen erstellen können müssen und nicht sollen die Ausführung der Schleife Benutzeroberfläche zu blockieren.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Entwickeln-reaktionsschnelle Anwendungen

Zugriff auf Elemente der Benutzeroberfläche sollte auf dem gleichen Thread beschränkt werden, die die Hauptschleife für Ihre Anwendung ausgeführt wird. Wenn Sie die Benutzeroberfläche von einem anderen Thread ändern möchten, sollten Sie den Code mithilfe von Warteschlange [NSObject.InvokeOnMainThread](xref:Foundation.NSObject), wie folgt aus:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Die oben genannten Ruft den Code innerhalb der Delegaten im Kontext des Hauptthreads, ohne dass Racebedingungen, die möglicherweise die Anwendung abstürzen konnte.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Threading und Garbagecollection

Im Verlauf der Ausführung der Objective-C-Laufzeit erstellen und Freigeben von Objekten. Der Thread die aktuelle, wenn Objekte werden für "Auto-Release" fest, die Objective-C-Laufzeit wird diese Objekte freigegeben, gekennzeichnet `NSAutoReleasePool`. Xamarin.iOS erstellt einen `NSAutoRelease` Pool für jeden Thread aus dem `System.Threading.ThreadPool` und für den Hauptthread. Diese Erweiterung umfasst alle Threads mit der standardmäßige TaskScheduler System.Threading.Tasks erstellt.

Bei der Erstellung eigener Threads mit `System.Threading` ist erforderlich, geben Sie der Besitzer `NSAutoRelease` Pool, um zu verhindern, dass die Daten vor Verlust schützen. Zu diesem Zweck einfach einen Wrapper für Ihr Thread in den folgenden Codeausschnitt:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Hinweis: Da Xamarin.iOS 5.2 Sie müssen keine eigene bieten `NSAutoReleasePool` mehr, da eine automatisch für Sie bereitgestellt wird.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)
