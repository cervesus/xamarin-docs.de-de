---
title: Async Support Overview (Übersicht über die asynchrone Unterstützung)
description: Dieses Dokument beschreibt die Programmierung mit Async und await, der in eingeführte Konzepte C# 5, um das Schreiben von asynchronen Codes vereinfachen.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: cca147f0c5dd1a217f464ffbed2a1ad2618c9b80
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830193"
---
# <a name="async-support-overview"></a>Async Support Overview (Übersicht über die asynchrone Unterstützung)

_C#5 eingeführte zwei Schlüsselwörter zur Vereinfachung der asynchronen Programmieren: Async und await. Diese Schlüsselwörter können Sie einfachen Code schreiben, der die Task Parallel Library zur Ausführung der lang andauernde Operations (z. B. Netzwerkzugriff) nutzt in einem anderen Thread aus, und einfach Zugriff auf die Ergebnisse nach Abschluss. Die neuesten Versionen von Xamarin.iOS und Xamarin.Android unterstützen Async und await: Dieses Dokument enthält Beschreibungen und ein Beispiel der Verwendung der neuen Syntax mit Xamarin._

Xamarin Async-Unterstützung basiert auf der Grundlage von Mono 3.0 und die API-Profil wird eine Mobilgeräte-Version von Silverlight auf eine Mobiltelefone optimierte Version von .NET 4.5 aktualisiert.

## <a name="overview"></a>Übersicht

Dieses Dokument führt das neue Async und await-Schlüsselwörtern und führt einige einfache Beispiele ansehen, das Implementieren von asynchroner Methoden in Xamarin.iOS und Xamarin.Android.

Für eine umfassendere Erläuterung der neuen asynchronen Features der C# 5 (einschließlich viele Beispiele und verschiedene Verwendungsszenarien) finden Sie im Artikel [asynchrone Programmierung](https://docs.microsoft.com/dotnet/csharp/async).

Die beispielanwendung wird eine einfache asynchrone webanforderung (ohne der Hauptthread blockiert), und klicken Sie dann aktualisiert die Benutzeroberfläche mit dem heruntergeladenen html und die Anzahl der Zeichen.

 [![](async-images/AsyncAwait_427x368.png "Die beispielanwendung führt eine einfache asynchrone webanforderung, ohne den Hauptthread zu blockieren und aktualisiert die Benutzeroberfläche mit dem heruntergeladenen html und die Anzahl der Zeichen")](async-images/AsyncAwait.png#lightbox)

Xamarin Async-Unterstützung basiert auf der Grundlage von Mono 3.0 und die API-Profil wird eine Mobilgeräte-Version von Silverlight auf eine Mobiltelefone optimierte Version von .NET 4.5 aktualisiert.

## <a name="requirements"></a>Anforderungen

C#5 Funktionen erfordern die Mono-3.0, die im 6.4 für Xamarin.iOS und Xamarin.Android 4.8 enthalten ist. Sie werden aufgefordert, die zum Aktualisieren Ihrer Mono, Xamarin.iOS, Xamarin.Android- und Xamarin.Mac um nutzen.

## <a name="using-async-amp-await"></a>Verwenden von Async &amp; "await"

 `async` und `await` unerfahren C# Sprachfunktionen, die Arbeit in Verbindung mit der Task Parallel Library zu lang andauernden Aufgaben ausführen, ohne den Hauptthread der Anwendung blockiert Threads Code schreiben, erleichtern.

## <a name="async"></a>async

### <a name="declaration"></a>Deklaration

Die `async` Schlüsselwort befindet sich in einer Methodendeklaration (oder auf einen Lambda-Ausdruck oder eine anonyme Methode), um anzugeben, dass sie Code enthält, die asynchron ausgeführt wird, kann ie. nicht den Thread des Aufrufers zu blockieren.

Eine Methode gekennzeichnet, mit `async` sollten enthalten mindestens einen await-Ausdruck oder Anweisung. Wenn kein `await`s in der Methode vorhanden sind, und klicken Sie dann sie synchron ausgeführt werden soll (gleich als seien keine `async` Modifizierer). Dies führt auch in eine compilerwarnung angezeigt (jedoch kein Fehler).

### <a name="return-types"></a>Rückgabetypen

Eine asynchrone Methode zurückgeben soll eine `Task`, `Task<TResult>` oder `void`.

Geben Sie die `Task` Typ zurückgeben, wenn die Methode keinen anderer Wert zurückgegeben werden.

Geben Sie `Task<TResult>` , wenn die Methode einen Wert zurückgeben muss, in denen `TResult` ist der Typ, der zurückgegeben wird (z. B. eine `int`, z. B.).

Die `void` -Rückgabetyp wird hauptsächlich für Ereignishandler, die dies erfordern verwendet. Code, der "void" zurückgebende asynchrone Methoden aufruft, kann nicht `await` auf dem Ergebnis.

### <a name="parameters"></a>Parameter

Asynchrone Methoden können nicht deklarieren `ref` oder `out` Parameter.

## <a name="await"></a>await

Der Operator "await" kann auf eine Aufgabe innerhalb einer Methode als asynchron markierten angewendet werden. Es bewirkt, dass die Methode, die an diesem Punkt die Ausführung angehalten, und warten Sie, bis die Aufgabe abgeschlossen ist.

Mit "await" blockiert nicht den Thread des Aufrufers – anstatt die Steuerung wieder an den Aufrufer. Dies bedeutet, dass der aufrufende Thread nicht blockiert ist, so z. B. der Benutzer Benutzeroberflächenthread nicht blockiert werden würde, wenn eine Aufgabe zu warten.

Wenn die Aufgabe abgeschlossen ist, wird die Methode fortgesetzt, an der gleichen Stelle im Code ausführen. Dies schließt dem Try-Bereich eines Try-Catch-finally-Blocks zurückgeben, (sofern vorhanden). "await" kann nicht in einem catch- oder finally-block.

Erfahren Sie mehr über ["await"](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await) in Microsoft Docs.

## <a name="exception-handling"></a>Ausnahmebehandlung

Ausnahmen, die in einer asynchronen Methode auftreten in der Aufgabe gespeichert und wird ausgelöst, wenn der Task ist `await`Ed. Diese Ausnahmen abgefangen, und in einem Try / Catch-Block verarbeitet werden können.

## <a name="cancellation"></a>Abbruch

Asynchrone Methoden, die eine lange Zeit in Anspruch nehmen, sollte Abbruch unterstützen. Abbruch wird in der Regel wie folgt aufgerufen:

- Ein `CancellationTokenSource` Objekt erstellt wird.
- Die `CancellationTokenSource.Token` Instanz an eine abzubrechende asynchrone Methode übergeben wird.
- Abbruch angefordert wird, durch den Aufruf der `CancellationTokenSource.Cancel` Methode.

Dann wird der Vorgang abgebrochen und bestätigt den Abbruch.

Weitere Informationen zum Abbrechen finden Sie unter [Fine-Tuning Your Async Application (C#) (Abstimmen der asynchronen Anwendung (C#))](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application).

## <a name="example"></a>Beispiel

Herunterladen der [Beispiel Xamarin-Projektmappe](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (für iOS und Android), ein funktionsfähiges Beispiel finden Sie unter `async` und `await` in mobilen apps. Im Beispielcode wird in diesem Abschnitt ausführlicher erläutert.

### <a name="writing-an-async-method"></a>Schreiben einer Async-Methode

Die folgende Methode veranschaulicht von Code eine `async` -Methode mit einer `await`Ed Aufgabe:

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

Beachten Sie Folgendes ein:

-  Die Deklaration der Methode enthält die `async` Schlüsselwort.
-  Der Rückgabetyp ist `Task<int>` damit Aufrufen von Code zugreifen kann, die `int` -Wert, der in dieser Methode berechnet wird.
-  Die return-Anweisung ist `return exampleInt;` Dies ist ein Integer-Objekt – die Tatsache, die die Methode zurückgibt `Task<int>` ist Teil der Datei sprachverbesserungen.


### <a name="calling-an-async-method-1"></a>Aufrufen einer asynchronen Methode 1

Diese Schaltflächen-Klickereignis Handler finden Sie in der Beispiel zu Android-Anwendung, die weiter oben erläuterten Methode aufzurufen:

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
-  Die asynchrone Methode DownloadHomepage gibt einen Task zurück <int> , die in der SizeTask-Variablen gespeichert ist.
-  Der Code "awaits" in der SizeTask-Variable.  *Dies* ist der Speicherort, der die Methode angehalten, und die Steuerung wird an den aufrufenden Code zurückgegeben, bis die asynchrone Aufgabe, die in einem eigenen Thread abgeschlossen ist.
-  Ausführung wird *nicht* anhalten, wenn die Aufgabe in der ersten Zeile der Methode, obwohl es erstellten Aufgabe erstellt wird. Das Schlüsselwort "await" gibt den Speicherort, in dem Ausführung angehalten wird.
-  Wenn die asynchrone Aufgabe abgeschlossen ist, IntResult festgelegt ist, und die Ausführung wird fortgeführt, auf dem ursprünglichen Thread, von der "await" aus.


### <a name="calling-an-async-method-2"></a>Aufrufen einer asynchronen Methode 2

In der iOS-beispielanwendung wird im Beispiel etwas anders geschrieben, um einen alternativen Ansatz zu veranschaulichen. Anstatt ein anonymes Delegaten verwenden das folgende Beispiel deklariert eine `async` -Ereignishandler, der wie ein reguläres Ereignis-Handler zugeordnet ist:

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

-  Die Methode als gekennzeichnet ist `async` gibt jedoch `void` . Dies erfolgt in der Regel nur für Ereignishandler (Andernfalls würde Sie zurückgeben einer `Task` oder `Task<TResult>` ).
-  Der Code `await` s für die `DownloadHomepage` Methode direkt auf eine Zuweisung zu einer Variablen ( `intResult` ) im Gegensatz zum vorherigen Beispiel, in denen es eine mittlere verwendet `Task<int>` Variablen an, auf die Aufgabe.  *Dies* ist der Speicherort, in dem Steuerelement an den Aufrufer bis zum Abschluss der asynchronen Methode in einem anderen Thread zurückgegeben wird.
-  Wenn die asynchrone Methode abgeschlossen ist, und gibt zurück, die Ausführung wird fortgesetzt, an die `await` was bedeutet, dass das ganzzahlige Ergebnis zurückgegeben wird, und klicken Sie dann in einem UI-Widget gerendert.


## <a name="summary"></a>Zusammenfassung

Verwenden von Async und await deutlich vereinfacht den erforderlichen Code für die lang andauernde Vorgänge in Hintergrundthreads zu erzeugen, ohne den Hauptthread zu blockieren. Sie erleichtern auch auf die Ergebnisse zugreifen, wenn die Aufgabe abgeschlossen wurde.

In diesem Dokument erhält einen Überblick über die neuen Schlüsselwörter und Beispiele für Xamarin.iOS und Xamarin.Android.



## <a name="related-links"></a>Verwandte Links

- [AsyncAwait (Beispiel)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Rückrufe als unsere Generationen Anweisung finden Sie unter](https://tirania.org/blog/archive/2013/Aug-15.html)
- [Daten (iOS) (Beispiel)](https://developer.xamarin.com/samples/monotouch/Data/)
- ["HttpClient" (iOS) (Beispiel)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (Beispiel)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Asynchrone Programmierung](https://docs.microsoft.com/dotnet/csharp/async)
- [Feinabstimmung der Async-Anwendung (C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)
- ["Await", und die Benutzeroberfläche und Deadlocks. Oh, mein!](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [Verarbeitung von Aufgaben nach Abschluss)](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/library/hh873175.aspx) (Aufgabenbasiertes asynchrones Muster)
- [Asynchronie in C# 5 (Lipperts-Blog) – über die Einführung der Schlüsselwörter](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
