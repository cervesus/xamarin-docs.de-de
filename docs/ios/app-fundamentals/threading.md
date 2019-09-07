---
title: Threading in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie die System. Threading-APIs in einer xamarin. IOS-Anwendung verwendet werden. Dabei werden die Task Parallel Library, das entwickeln reaktionsfähiger Anwendungen und die Garbage Collection erläutert.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: d8267d4def0f7c24c660dfb4d301c111a92bb0b9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767155"
---
# <a name="threading-in-xamarinios"></a>Threading in xamarin. IOS

Die xamarin. IOS-Laufzeit ermöglicht Entwicklern den Zugriff auf die .NET-Threading-APIs, sowohl explizit`System.Threading.Thread, System.Threading.ThreadPool`bei der Verwendung von Threads () als auch implizit, wenn die asynchronen delegatmuster oder die BeginXxx-Methoden sowie der gesamte Bereich von APIs verwendet werden, die die Task Parallel Library.

Xamarin empfiehlt dringend, den [Task Parallel Library](https://msdn.microsoft.com/library/dd460717.aspx) (TPL) zum Entwickeln von Anwendungen aus verschiedenen Gründen zu verwenden:
- Der standardmäßige TPL-Planer delegiert die Task Ausführung an den Thread Pool, der wiederum dynamisch die Anzahl der für den Prozess benötigten Threads vergrößert, während ein Szenario vermieden wird, bei dem zu viele Threads am Ende der CPU-Zeit konkurrieren. 
- Es ist einfacher, Vorgänge in Bezug auf TPL-Aufgaben zu übernehmen. Sie können Sie problemlos manipulieren, planen, ihre Ausführung Serialisieren oder viele parallel mit einem umfangreichen Satz von APIs starten. 
- Es ist die Grundlage für die Programmierung mit den C# neuen Async-Spracherweiterungen. 

Der Thread Pool vergrößert die Anzahl der Threads nach Bedarf, basierend auf der Anzahl der im System verfügbaren CPU-Kerne, der System Auslastung und der Anwendungsanforderungen. Sie können diesen Thread Pool entweder durch Aufrufen von Methoden in `System.Threading.ThreadPool` oder mithilfe des Standard `System.Threading.Tasks.TaskScheduler` mäßigen (Teil des *parallelen Frameworks*) verwenden.

In der Regel verwenden Entwickler Threads, wenn Sie reaktionsschnelle Anwendungen erstellen müssen, und Sie möchten die Haupt-UI-Lauf Zeitschleife nicht blockieren.

 <a name="Developing_Responsive_Applications" />

## <a name="developing-responsive-applications"></a>Entwickeln von reaktionsfähigen Anwendungen

Der Zugriff auf Benutzeroberflächen Elemente sollte auf denselben Thread beschränkt sein, der die Hauptschleife für Ihre Anwendung ausführen soll. Wenn Sie von einem Thread Änderungen an der Hauptbenutzer Oberfläche vornehmen möchten, sollten Sie den Code wie folgt mithilfe von [NSObject. invokeonmainthread](xref:Foundation.NSObject)in die Warteschlange stellen:

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

Der obige Code Ruft den Code innerhalb des Delegaten im Kontext des Haupt Threads auf, ohne Racebedingungen zu verursachen, die möglicherweise einen Absturz ihrer Anwendung verursachen.

 <a name="Threading_and_Garbage_Collection" />

## <a name="threading-and-garbage-collection"></a>Threading und Garbage Collection

Im Verlauf der Ausführung werden Objekte von der Ziel-C-Laufzeit erstellt und freigegeben. Wenn Objekte für "Automatisches Release" gekennzeichnet sind, gibt die Ziel-C-Laufzeit diese Objekte an den aktuellen `NSAutoReleasePool`des Threads frei. Xamarin. IOS erstellt einen `NSAutoRelease` Pool für jeden Thread `System.Threading.ThreadPool` aus und für den Haupt Thread. Dadurch werden alle Threads behandelt, die mit dem Standard Task Scheduler in System. Threading. Tasks erstellt wurden.

Wenn Sie Ihre eigenen Threads mithilfe `System.Threading` von erstellen, müssen Sie einen eigenen `NSAutoRelease` Pool bereitstellen, um zu verhindern, dass die Daten nicht mehr vorhanden sind. Umschließen Sie zu diesem Zweck einfach den Thread in den folgenden Code Ausschnitt:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Hinweis: Seit xamarin. IOS 5,2 müssen Sie keine eigenen `NSAutoReleasePool` bereitstellen, da eine automatisch für Sie bereitgestellt wird.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit dem UI-Thread](~/ios/user-interface/ios-ui/ui-thread.md)
