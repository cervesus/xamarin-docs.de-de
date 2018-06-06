---
title: Arbeiten mit UI-Thread in Xamarin.iOS
description: Dieses Dokument beschreibt, wie im UI-Thread in Xamarin.iOS arbeiten. Erläutert die Ausführung der UI-Threads, enthält ein Beispiel für Background Thread und untersucht Async/await.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 4328b84625aff4c92d6e97029ced7dde747d4fc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790408"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Arbeiten mit UI-Thread in Xamarin.iOS

Benutzeroberflächen für die Anwendung sind immer Singlethread, sogar in Multithreaded-Geräte – es gibt nur eine Darstellung des Bildschirms, und alle Änderungen an der Anzeige über einen einzelnen "Zugriffspunkt" koordiniert werden müssen. Dadurch wird verhindert, dass mehrere Threads dasselbe Pixel (z. B.) gleichzeitig zu aktualisieren versuchen.

Den Code sollte nur Änderungen an den Benutzer Benutzeroberflächen-Steuerelemente aus dem Hauptknoten (oder UI)-Thread zu gestalten. Alle UI-Updates, die auf einem anderen Thread (z. B. einen Rückruf oder Hintergrund-Thread) auftreten möglicherweise nicht auf dem Bildschirm gerendert abrufen, oder Sie können auch verursacht einen Absturz.

## <a name="ui-thread-execution"></a>Ausführung von UI-Threads

Beim Erstellen von Steuerelementen in einer Ansicht, oder behandeln ein Ereignisses Benutzerinitiierte, z. B. die Fingereingabe, wird der Code im Kontext des UI-Thread bereits ausgeführt.

Wenn Code in einem Hintergrundthread, in eine Aufgabe oder einen Rückruf ausführt ist es wahrscheinlich nicht auf dem Hauptbenutzeroberflächen-Thread ausgeführt. In diesem Fall sollten Sie den Code umschließen, in einem Aufruf von `InvokeOnMainThread` oder `BeginInvokeOnMainThread` wie folgt:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

Die `InvokeOnMainThread` Methode definiert ist, auf `NSObject` , damit sie von innerhalb eines UIKit-Objekts (z. B. eine Sicht oder View Controller) definierten Methoden aufgerufen werden kann.

Während des Debuggens Xamarin.iOS Anwendungen, wird ein Fehler ausgelöst werden, wenn Ihr Code versucht, den falschen Thread ein UI-Steuerelement zugreifen. Dies hilft Ihnen das Auffinden und beheben diese Probleme mit der InvokeOnMainThread-Methode. Dies nur tritt auf, während des Debuggens und einen Fehler wird nicht in Releasebuilds ausgelöst. Die Fehlermeldung wird wie folgt angezeigt:

 ![](ui-thread-images/image10.png "Ausführung von UI-Threads")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Hintergrund-Thread-Beispiel

Hier ist ein Beispiel, die versucht, ein Steuerelement der Benutzeroberfläche zugreifen (eine `UILabel`) aus einem Hintergrundthread mithilfe einer einfachen Threads:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Dass Code auslöst, wird die `UIKitThreadAccessException` während des Debuggens. Umschließen Sie zum Beheben des Problems (und stellen Sie sicher, dass das Steuerelement der Benutzeroberfläche nur aus dem Hauptbenutzeroberflächen-Thread zugegriffen wird), Code, der UI-Steuerelemente in einer verweist auf ein `InvokeOnMainThread` Ausdruck wie folgt:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Müssen Sie nicht verwenden, die dies für den Rest der Beispiele in diesem Dokument, aber es ist ein wichtiges Konzept bei Ihrer app netzwerkanforderungen, werden verwendet, die mitteilungszentrale oder andere Methoden, die eine Abschlusshandler erfordern, die auf einem anderen ausgeführt wird Thread.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Async/Await-Beispiel

Bei Verwendung der C#-5-Async/await-Schlüsselwörter `InvokeOnMainThread` ist nicht erforderlich, da bei eine Aufgabe abgeschlossen ist die Methode, die im aufrufenden Thread fortgesetzt wird.

Dieser Beispielcode (die auf einen Methodenaufruf Verzögerung ausschließlich zu Demonstrationszwecken erwartet) zeigt eine Async-Methode, die im UI-Thread aufgerufen wird (es handelt sich um einen TouchUpInside-Handler ist). Da die enthaltende Methode für die UI-Thread aufgerufen wird, UI-Vorgänge wie das Festlegen des Texts auf eine `UILabel` oder das Anzeigen einer `UIAlertView` kann problemlos aufgerufen werden, nachdem in Hintergrundthreads asynchronen Vorgänge abgeschlossen sind.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Wenn eine Async-Methode, aus einem Hintergrundthread (nicht der Hauptbenutzeroberflächen-Thread aufgerufen wird) dann `InvokeOnMainThread` wären immer noch erforderlich.


## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
- [Threading](~/ios/app-fundamentals/threading.md)
