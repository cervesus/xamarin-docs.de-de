---
title: Async (Übersicht)
description: 'Die neueste Version der C#-Sprache – Version 5 – eingeführt wurden zwei neue Schlüsselwörter, um asynchrone Vorgänge auszudrücken: Async und await. Diese Schlüsselwörter können Sie einfachen Code schreiben, der der Task Parallel Library zum Ausführen von lang ausgeführten Vorgänge (z. B. Netzwerkzugriff) nutzt in einem anderen Thread und einfacher Zugriff auf die Ergebnisse auf den Abschluss des Vorgangs. Die neuesten Versionen der Xamarin.iOS und Xamarin.Android asynchrone Unterstützung und "await": Dieses Dokument enthält erläuterungen und ein Beispiel zur Verwendung der neuen Syntax mit Xamarin.'
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 5d6bb9581a4429502d9a70385b3ee2ff056f30ee
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="async-support-overview"></a>Async Support Overview (Übersicht über die asynchrone Unterstützung)

_Die neueste Version der C#-Sprache – Version 5 – eingeführt wurden zwei neue Schlüsselwörter, um asynchrone Vorgänge auszudrücken: Async und await. Diese Schlüsselwörter können Sie einfachen Code schreiben, der der Task Parallel Library zum Ausführen von lang ausgeführten Vorgänge (z. B. Netzwerkzugriff) nutzt in einem anderen Thread und einfacher Zugriff auf die Ergebnisse auf den Abschluss des Vorgangs. Die neuesten Versionen der Xamarin.iOS und Xamarin.Android asynchrone Unterstützung und "await": Dieses Dokument enthält erläuterungen und ein Beispiel zur Verwendung der neuen Syntax mit Xamarin._

Die Xamarin asynchrone Unterstützung basiert auf der Grundlage Mono 3.0 und aktualisiert die API-Profil wird eine mobilgerätefreundliche-Version von Silverlight eine mobilgerätefreundliche-Version von .NET 4.5 ausgeführt werden.

## <a name="overview"></a>Übersicht

Dieses Dokument führt der neuen asynchronen und "await" Schlüsselwörter dann schrittweise einige einfache Beispiele, die Implementierung von asynchroner Methoden in Xamarin.iOS und Xamarin.Android.

Eine ausführlichere Diskussion der neuen asynchronen Funktionen von C#-5 (einschließlich viele Beispiele und verschiedene Verwendungsszenarien) finden Sie in der MSDN-Dokumentation [asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/vstudio/hh191443.aspx).

Die beispielanwendung wird eine einfache asynchrone Web-Anforderung (ohne Blockierung des Haupt-Threads), und klicken Sie dann aktualisiert die Benutzeroberfläche mit der heruntergeladenen HTML- und der Anzahl der Zeichen.

 [![](async-images/AsyncAwait_427x368.png "Die beispielanwendung wird eine einfache asynchrone Web-Anforderung ohne den Hauptthread zu blockieren und aktualisiert die Benutzeroberfläche mit der heruntergeladenen HTML- und der Anzahl der Zeichen")](async-images/AsyncAwait.png#lightbox)

Die Xamarin asynchrone Unterstützung basiert auf der Grundlage Mono 3.0 und aktualisiert die API-Profil wird eine mobilgerätefreundliche-Version von Silverlight eine mobilgerätefreundliche-Version von .NET 4.5 ausgeführt werden.

## <a name="requirements"></a>Anforderungen

C#-5-Funktionen erfordern Mono-3.0, die in 6.4 Xamarin.iOS und Xamarin.Android 4.8 enthalten ist. Sie werden aufgefordert, die so aktualisieren Sie Ihren Mono, Xamarin.iOS, Xamarin.Android sowie Xamarin.Mac nutzen.

## <a name="using-async-amp-await"></a>Verwenden von Async &amp; "await"

 `async` und `await` werden neue C#-Sprachfunktionen, die Arbeit in Verbindung mit der Task Parallel Library zu erleichtern, um lang andauernden Aufgaben auszuführen, ohne dass den Hauptthread der Anwendung blockiert Threads Code zu schreiben.

## <a name="async"></a>async

### <a name="declaration"></a>Deklaration

Die `async` Schlüsselwort befindet sich in einer Methodendeklaration (oder einen Lambda-Ausdruck oder eine anonyme Methode), um anzugeben, dass sie Code enthält, die asynchron ausgeführt werden kann ie. der Aufrufer-Thread nicht blockieren.

Eine Methode mit markiert `async` sollten enthalten mindestens einen await-Ausdruck oder Anweisung. Wenn kein `await`s in der Methode vorhanden sind, und klicken Sie dann sie synchron ausgeführt werden soll (identisch, als wäre es wurden keine `async` Modifizierer). Dies führt auch in eine compilerwarnung verwenden (jedoch nicht um einen Fehler).

### <a name="return-types"></a>Rückgabetypen

Eine asynchrone Methode sollte Zurückgeben einer `Task`, `Task<TResult>` oder `void`.

Geben Sie die `Task` Typ zurückgeben, wenn einen anderer Wert nicht von der Methode zurückgegeben werden.

Geben Sie `Task<TResult>` , wenn die Methode einen Wert zurückgeben muss, in dem `TResult` ist der Typ, der zurückgegeben wird (z. B. ein `int`, z. B.).

Die `void` Rückgabetyp wird hauptsächlich für Ereignishandler, die es erfordern verwendet. Code, der "void" zurückgebende asynchrone Methoden aufruft, kann nicht `await` auf das Ergebnis.

### <a name="parameters"></a>Parameter

Asynchrone Methoden können nicht deklarieren `ref` oder `out` Parameter.

## <a name="await"></a>await

Der Await-Operator kann auf eine Aufgabe in eine markierte als asynchrone Methode angewendet werden. Es bewirkt, dass die Methode, die an diesem Punkt die Ausführung anhalten und warten Sie, bis die Aufgabe abgeschlossen ist.

Mit "await" blockiert nicht den Thread des Aufrufers – vielmehr Steuerelement an den Aufrufer zurückgegeben wird. Dies bedeutet, dass der aufrufende Thread nicht blockiert wird, so z. B. der Benutzer Schnittstelle Thread nicht blockiert werden würde, wenn eine Aufgabe zu erwarten.

Wenn die Aufgabe abgeschlossen ist, wird die Methode fortgesetzt, an der gleichen Stelle im Code ausführen. Dazu gehören die Rückgabe an den wiederholen Sie den Bereich eines Try-Catch-finally-Blocks (sofern vorhanden). "await" kann nicht in einem catch- oder finally-block.

Erfahren Sie mehr über ["await" in MSDN](http://msdn.microsoft.com/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Ausnahmebehandlung

Ausnahmen, die innerhalb einer asynchronen Methode auftreten in der Aufgabe gespeichert und wird ausgelöst, wenn die Aufgabe ist `await`Ed. Diese Ausnahmen können abgefangen und in einem Try / Catch-Block verarbeitet werden.

## <a name="cancellation"></a>Abbruch

Asynchrone Methoden, die eine lange Zeit in Anspruch nehmen, sollte Abbruch unterstützen. Abbruch wird in der Regel wie folgt aufgerufen:

- Ein `CancellationTokenSource` Objekt erstellt wird.
- Die `CancellationTokenSource.Token` Instanz an eine abzubrechende asynchrone Methode übergeben wird.
- Abbruch angefordert wird, durch Aufrufen der `CancellationTokenSource.Cancel` Methode.

Der Vorgang abgebrochen wird dann und bestätigt den Abbruch.

Weitere Informationen über Abbrüche finden Sie unter [Gewusst wie: Abbrechen eine asynchrone Aufgabe](http://msdn.microsoft.com/library/vstudio/jj155761.aspx) auf MSDN.

## <a name="example"></a>Beispiel

Herunterladen der [Beispiel Xamarin-Projektmappe](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (für IOS- und Android) um ein praktisches Beispiel finden Sie unter `async` und `await` bei mobilen apps. Im Beispielcode wird in diesem Abschnitt ausführlicher erläutert.

### <a name="writing-an-async-method"></a>Schreiben einer Async-Methode

Die folgende Methode veranschaulicht das Codieren ein `async` Methode mit einer `await`Ed Aufgabe:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Beachten Sie folgende Punkte:

-  Die Deklaration der Methode enthält die `async` Schlüsselwort.
-  Der Rückgabetyp ist `Task<int>` damit Aufrufen von Code zugreifen kann die `int` -Wert, der in dieser Methode berechnet wird.
-  Die return-Anweisung ist `return exampleInt;` ist ein Integer-Objekt – die Tatsache, dass die der Methodenrückgabe `Task<int>` ist Teil der Sprache Verbesserungen.


### <a name="calling-an-async-method-1"></a>Aufrufen einer asynchronen Methode 1

Diese Schaltfläche click-Ereignis in dem Beispiel zu Android-Anwendung zum Aufrufen der Methode, die weiter oben erläuterten kann Handler gefunden werden:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Notizen:

-  Der anonyme Delegat hat das Präfix der Async-Schlüsselwort.
-  Die asynchrone Methode DownloadHomepage gibt einen Task zurück <int> in der SizeTask-Variablen gespeichert wird.
-  Der Code wartet auf die SizeTask-Variable auf.  *Dies* ist der Speicherort, der die Methode angehalten, und die Steuerung wird an den aufrufenden Code zurückgegeben, bis die asynchrone Aufgabe, die in einem eigenen Thread abgeschlossen ist.
-  Ausführung ist *nicht* anhalten, wenn der Task erstellt wird, in der ersten Zeile der Methode, die trotz der Aufgabe, die es erstellt wird. Der Await-Schlüsselwort Gibt an, den Speicherort, an dem Ausführung angehalten wird.
-  Wenn die asynchrone Aufgabe abgeschlossen ist, IntResult festgelegt ist und die Ausführung wird fortgeführt, für den ursprünglichen Thread, aus der Zeile "await".


### <a name="calling-an-async-method-2"></a>Aufrufen einer asynchronen Methode 2

In der iOS-beispielanwendung wird die im Beispiel zur Veranschaulichung der Funktion eines alternativen Ansatzes etwas anders geschrieben. Statt ein anonymes Delegaten verwendet dieses Beispiel deklariert eine `async` Ereignishandler, der wie eine reguläre Ereignishandler zugeordnet ist:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Anschließend wird die Ereignishandlermethode definiert, wie hier gezeigt:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Einige wichtige Punkte:

-  Die Methode wird als gekennzeichnet `async` gibt jedoch `void` . Dies wird normalerweise nur für Ereignishandler ausgeführt (Andernfalls würde Zurückgeben einer `Task` oder `Task<TResult>` ).
-  Der Code `await` s auf die `DownloadHomepage` -Methode direkt nach einer Zuordnung zu einer Variablen ( `intResult` ) im Gegensatz zum vorherigen Beispiel, in denen es eine mittlere verwendet `Task<int>` Variablen an, auf die Aufgabe.  *Dies* ist der Speicherort, auf dem Steuerelement an den Aufrufer bis zum Abschluss der asynchronen Methode in einem anderen Thread zurückgegeben wird.
-  Wenn die asynchrone Methode abgeschlossen ist, und gibt zurück, die Ausführung wird fortgesetzt, an der `await` was bedeutet, dass die ganze Zahl als Ergebnis zurückgegeben und dann in einem UI-Widget gerendert.


## <a name="summary"></a>Zusammenfassung

Verwenden von Async und await erheblich vereinfacht den Code erforderlich, um lang ausgeführter Vorgänge in Hintergrundthreads zu erzeugen, ohne den Hauptthread zu blockieren. Sie können auch die einfach die Ergebnisse zugreifen, wenn die Aufgabe abgeschlossen wurde.

Dieses Dokument hat einen Überblick über die neuen Programmiersprachen-Schlüsselworten und Beispiele für Xamarin.iOS und Xamarin.Android angegeben.



## <a name="related-links"></a>Verwandte Links

- [AsyncAwait (Beispiel)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Rückrufe als unsere Generationen Go-Anweisung](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Daten (iOS) (Beispiel)](https://developer.xamarin.com/samples/monotouch/Data/)
- ["HttpClient" (iOS) (Beispiel)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (Beispiel)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Webinar: C#-Async auf IOS- und Android (Video)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Asynchrone Programmierung mit Async und Await (MSDN)](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [Optimierung der Async-Anwendung (MSDN)](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- ["Await", und die Benutzeroberfläche und Deadlocks. Oh, mein! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Verarbeiten von Aufgaben nach Abschluss (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Task-based Asynchronous Pattern (TAP)](http://msdn.microsoft.com/library/hh873175.aspx) (Aufgabenbasiertes asynchrones Muster)
- [Asynchronie in C#-5 (Eric Blog) – über die Einführung der Schlüsselwörter](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
