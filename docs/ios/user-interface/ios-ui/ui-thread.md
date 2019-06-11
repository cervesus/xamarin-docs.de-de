---
title: Arbeiten mit UI-Thread in Xamarin.iOS
description: Dieses Dokument beschreibt das Arbeiten mit UI-Thread in Xamarin.iOS. Erläutert die Ausführung von UI-Threads, bietet ein Beispiel für die Hintergrund-Thread und untersucht, Async/await.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: e4485c485b708bdec06f7f1dc22f0bf33e07e982
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827745"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Arbeiten mit UI-Thread in Xamarin.iOS

Anwendungsbenutzerschnittstellen sind immer, auch in Multithread-Geräte – es gibt nur eine Darstellung des Bildschirms, und müssen alle Änderungen an der Anzeige durch einen einzigen 'Access Point' koordiniert werden. Dadurch wird verhindert, dass mehrere Threads versuchen, dasselbe Pixel (z. B.) zur gleichen Zeit zu aktualisieren.

Ihr Code sollte nur Änderungen an den Benutzer Steuerelemente der Benutzeroberfläche aus der primären (oder UI) Thread vornehmen. Alle Aktualisierungen der Benutzeroberfläche, die auf einem anderen Thread (z. B. einen Rückruf oder Hintergrund-Thread) auftreten können nicht auf dem Bildschirm gerendert zu erhalten, oder es können auch einen Absturz verursachen.

## <a name="ui-thread-execution"></a>Ausführung von UI-Threads

Wenn Sie das Erstellen von Steuerelementen in einer Sicht, oder behandeln ein Ereignisses vom Benutzer initiierte, z. B. eine Fingereingabe, wird der Code bereits im Kontext des UI-Threads ausgeführt.

Wenn Code in einem Hintergrundthread, in einem Task oder einen Rückruf ausführt ist es wahrscheinlich nicht auf dem Hauptbenutzeroberflächen-Thread ausgeführt. In diesem Fall sollten Sie den Code umschließen, in einem Aufruf von `InvokeOnMainThread` oder `BeginInvokeOnMainThread` wie folgt aus:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

Die `InvokeOnMainThread` Methode definiert ist, auf `NSObject` , damit sie von innerhalb jedes UIKit-Objekt (z. B. eine Sicht oder View Controller) definierten Methoden aufgerufen werden kann.

Beim Debuggen von Xamarin.iOS-Anwendungen, wird ein Fehler ausgelöst werden, wenn der Code versucht, den falschen Thread auf ein UI-Steuerelement. Dadurch können Sie zum Aufspüren und beheben diese Probleme mit der InvokeOnMainThread-Methode. Dies wird nur tritt auf, während des Debuggens und einen Fehler wird nicht in Releasebuilds ausgelöst. Die Fehlermeldung wird wie folgt angezeigt:

 ![](ui-thread-images/image10.png "Ausführung von UI-Threads")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Hintergrund-Thread-Beispiel

Es folgt ein Beispiel, das versucht, ein Steuerelement der Benutzeroberfläche zugreifen (eine `UILabel`) aus einem Hintergrundthread, der mit einem einfachen Thread:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Dass der Code eine Ausnahme auslöst, die `UIKitThreadAccessException` während des Debuggens. Umschließen Sie zum Beheben des Problems (und stellen Sie sicher, dass die Steuerelemente der Benutzeroberfläche nur aus dem Hauptbenutzeroberflächen-Thread zugegriffen wird), Code, verweist der UI-Steuerelemente in, einem `InvokeOnMainThread` Ausdruck wie folgt:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Sie müssen nicht dies für den Rest der Beispiele in diesem Dokument verwenden, aber es ist ein wichtiger Konzept zu beachten, wenn Ihre app netzwerkanforderungen stellt verwendet Notification Center oder andere Methoden, die einen Abschlusshandler erfordern, die auf einem anderen ausgeführt wird Thread.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Beispiel für "Async/await"

Bei Verwendung der C# 5 Async/await-Schlüsselwörtern `InvokeOnMainThread` ist nicht erforderlich, da bei der eine erwartete Aufgabe abgeschlossen ist die Methode, die für den aufrufenden Thread fortgesetzt.

Dieser Beispielcode (die in einem Methodenaufruf Verzögerung ausschließlich zu Demonstrationszwecken "awaits") zeigt eine asynchrone Methode, die im UI-Thread aufgerufen wird (es handelt es sich um einen Handler TouchUpInside). Da der UI-Thread die enthaltende Methode aufgerufen wird, UI-Vorgänge wie das Festlegen des Texts auf eine `UILabel` oder Einblenden von einer `UIAlertView` kann problemlos aufgerufen werden, nachdem in Hintergrundthreads asynchrone Vorgänge abgeschlossen haben.

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

Wenn eine asynchrone Methode aus einem Hintergrundthread (nicht dem Hauptbenutzeroberflächen-Thread) aufgerufen wird, klicken Sie dann `InvokeOnMainThread` wären immer noch erforderlich.


## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/monotouch/Controls/)
- [Threading](~/ios/app-fundamentals/threading.md)
