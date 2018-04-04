---
title: Threading
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6d178231cd45d3b251a26c47abd47bf22b6c2716
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="threading"></a>Threading

Die Xamarin.iOS-Laufzeit bietet Entwicklern Zugriff auf die .NET threading-APIs, beide explizit bei der Verwendung von Threads (`System.Threading.Thread, System.Threading.ThreadPool`) als auch implizit bei asynchronen Delegaten Muster oder BeginXXX-Methoden sowie die vollständigen Bereich von APIs, die Unterstützung der Task Parallel Library.



Xamarin empfiehlt dringend die Verwendung der [Task Parallel Library](http://msdn.microsoft.com/en-us/library/dd460717.aspx) (TPL) zum Erstellen von Anwendungen für einige Gründe:
-  TPL Standardplaner delegiert die Ausführung der Aufgabe an den Threadpool dem wiederum dynamisch die Anzahl der Threads, die erforderlich vergrößert wird, da der Prozess findet, ein Szenario, in denen zu viele Threads um CPU-Zeit konkurrieren letztendlich, zu vermeiden. 
-  Es ist einfacher, sind im Wesentlichen Vorgänge in Bezug auf die TPL-Tasks. Können Sie bequem bearbeitet werden, zu planen, ihre Ausführung zu serialisieren oder viele parallel mit einem umfangreichen Satz von APIs zu starten. 
-  Es ist die Grundlage für die Programmierung mit den neuen C#-Async spracherweiterungen. 


Der Threadpool wächst langsam die Anzahl der Threads nach Ihren Anforderungen basierend auf der Anzahl der CPU-Kerne verfügbar, auf das System, die Systemlast und die Anforderungen Ihrer Anwendung. Sie können diese Threadpool verwenden, entweder durch Aufrufen von Methoden in `System.Threading.ThreadPool` oder unter Verwendung des standardmäßigen `System.Threading.Tasks.TaskScheduler` (Teil der *parallele Frameworks*).

Normalerweise verwenden Entwickler Threads, wenn sie reaktionsfähige Anwendungen erstellen können müssen, und sie nicht die Ausführung der Schleife Hauptbenutzeroberfläche blockieren möchten.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Reagiert die Anwendungsentwicklung

Zugriff auf Elemente der Benutzeroberfläche sollte auf dem gleichen Thread beschränkt werden, die die Hauptschleife für Ihre Anwendung ausgeführt wird. Wenn Sie Änderungen an die Haupt-Benutzeroberfläche von einem anderen Thread vornehmen möchten, sollten Sie den Code mithilfe von Warteschlange [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), wie folgt:

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

Oben wird den Code innerhalb der Delegaten im Kontext des Hauptthreads, ohne dass alle Racebedingungen, die potenziell die Anwendung abstürzt konnte aufgerufen.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Threading und die Garbagecollection

Im Verlauf der Ausführung die Objective-C-Laufzeit erstellen und Freigeben von Objekten. Wenn Objekte werden für "Auto-Version" Objective-C-Laufzeit wird diese Objekte freigegeben gekennzeichnet der Thread die aktuelle `NSAutoReleasePool`. Xamarin.iOS erstellt eine `NSAutoRelease` Pool für jeden Thread aus der `System.Threading.ThreadPool` und für den Hauptthread. Diese Erweiterung werden alle Threads erstellt unter Verwendung des standardmäßigen TaskScheduler in System.Threading.Tasks behandelt.

Bei der Erstellung eigener Threads der Verwendung `System.Threading` ist erforderlich, geben Sie der Besitzer sind `NSAutoRelease` Pool, um zu verhindern, dass die Daten gelangen. Zu diesem Zweck müssen Sie lediglich umschließen Sie Ihr Thread in der folgenden Codeabschnitt:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Hinweis: Da Xamarin.iOS 5.2 Sie keine eigene bereitstellen `NSAutoReleasePool` mehr als eine automatisch für Sie bereitgestellt wird.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)
