---
title: Async Support Overview (Übersicht über die asynchrone Unterstützung)
description: In diesem Dokument wird die Programmierung mit Async und Erwartung der in C# 5 eingeführten Konzepte beschrieben, um das Schreiben von asynchronem Code zu vereinfachen.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 4ade8fbb3ac596ef2da5d76b4efa751661cd8611
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646250"
---
# <a name="async-support-overview"></a>Async Support Overview (Übersicht über die asynchrone Unterstützung)

_C#5 hat zwei Schlüsselwörter zur Vereinfachung der asynchronen Programmierung eingeführt: Async und warten. Mithilfe dieser Schlüsselwörter können Sie einfachen Code schreiben, der die Task Parallel Library verwendet, um Vorgänge mit langer Ausführungsdauer (z. b. Netzwerk Zugriff) in einem anderen Thread auszuführen und problemlos auf die Ergebnisse beim Abschluss zuzugreifen. Die neuesten Versionen von xamarin. IOS und xamarin. Android unterstützen Async und warten: Dieses Dokument enthält Erläuterungen und ein Beispiel für die Verwendung der neuen Syntax mit xamarin._

Die Async-Unterstützung von xamarin basiert auf der Mono 3,0 Foundation und aktualisiert das API-Profil von der als Mobilgeräte freundlichen Version von Silverlight, die eine Mobil funkfreundliche Version von .NET 4,5 sein soll.

## <a name="overview"></a>Übersicht

Dieses Dokument enthält eine Einführung in die neuen Async-und-Erwartungs Schlüsselwörter und führt Sie durch einige einfache Beispiele für das Implementieren von asynchronen Methoden in xamarin. IOS und xamarin. Android.

Eine ausführlichere Erläuterung der neuen asynchronen Features von C# 5 (einschließlich vieler Beispiele und unterschiedlicher Verwendungs Szenarien) finden Sie im Artikel [asynchrone Programmierung](https://docs.microsoft.com/dotnet/csharp/async).

Die Beispielanwendung führt eine einfache asynchrone Webanforderung durch (ohne den Haupt Thread zu blockieren) und aktualisiert dann die Benutzeroberfläche mit der heruntergeladenen HTML-und Zeichen Anzahl.

 [![](async-images/AsyncAwait_427x368.png "Die Beispielanwendung erstellt eine einfache asynchrone Webanforderung, ohne den Haupt Thread zu blockieren, und aktualisiert dann die Benutzeroberfläche mit der heruntergeladenen HTML-und Zeichen Anzahl.")](async-images/AsyncAwait.png#lightbox)

Die Async-Unterstützung von xamarin basiert auf der Mono 3,0 Foundation und aktualisiert das API-Profil von der als Mobilgeräte freundlichen Version von Silverlight, die eine Mobil funkfreundliche Version von .NET 4,5 sein soll.

## <a name="requirements"></a>Anforderungen

C#5 Features erfordern Mono 3,0, das in xamarin. IOS 6,4 und xamarin. Android 4,8 enthalten ist. Sie werden aufgefordert, Ihr Mono, xamarin. IOS, xamarin. Android und xamarin. Mac zu aktualisieren, um Sie nutzen zu können.

## <a name="using-async-amp-await"></a>Verwenden von asynchroner Erwartung &amp;

 `async`und `await` sind neue C# sprach Features, die in Verbindung mit dem Task Parallel Library funktionieren, um das Schreiben von Thread Code zum Ausführen von Aufgaben mit langer Ausführungszeit zu vereinfachen, ohne den Hauptthread Ihrer Anwendung zu blockieren.

## <a name="async"></a>async

### <a name="declaration"></a>Deklaration

Das `async` Schlüsselwort wird in eine Methoden Deklaration eingefügt (oder in einer Lambda-oder anonymen Methode), um anzugeben, dass es Code enthält, der asynchron ausgeführt werden kann, d. h. den Thread des Aufrufers nicht blockieren.

Eine mit `async` markierte Methode sollte mindestens einen Erwartungs Ausdruck oder eine Anweisung enthalten. Wenn in `await`der Methode keine s vorhanden sind, wird sie synchron ausgeführt (das gleiche wie bei keinem `async` Modifizierer). Dies führt auch zu einer Compilerwarnung (aber nicht zu einem Fehler).

### <a name="return-types"></a>Rückgabetypen

Eine Async-Methode muss `Task`, `Task<TResult>` oder `void`zurückgeben.

Geben Sie `Task` den Rückgabetyp an, wenn die Methode keinen anderen Wert zurückgibt.

Geben `Task<TResult>` Sie an, ob die Methode einen Wert zurückgeben `TResult` muss, wobei der Typ ist, der zurück `int`gegeben wird (z. b. ein).

Der `void` Rückgabetyp wird hauptsächlich bei Ereignis Handlern verwendet, die dies erfordern. Code, der void-zurückgegebene asynchrone Methoden `await` aufruft, kann nicht auf das Ergebnis zurückzuführen sein.

### <a name="parameters"></a>Parameter

Asynchrone Methoden können keine `ref` - `out` oder-Parameter deklarieren.

## <a name="await"></a>await

Der Erwartungs Operator kann auf eine Aufgabe innerhalb einer Methode angewendet werden, die als "Async" markiert ist. Dadurch wird die Ausführung der-Methode an diesem Punkt beendet, und es wird gewartet, bis die Aufgabe abgeschlossen ist.

Die Verwendung von "warten" blockiert nicht den Thread des Aufrufers – stattdessen wird die Steuerung an den Aufrufer zurück Dies bedeutet, dass der aufrufende Thread nicht blockiert wird, sodass z. b. der Benutzeroberflächen Thread nicht blockiert wird, wenn auf eine Aufgabe gewartet wird.

Wenn die Aufgabe abgeschlossen ist, wird die Ausführung der-Methode an derselben Stelle im Code fortgesetzt. Dies schließt die Rückgabe zum Try-Bereich eines try-catch-that-Blocks ein (sofern vorhanden). "warten" kann nicht in einem Catch-oder schließlich-Block verwendet werden.

Weitere Informationen finden [Sie unter Microsoft-Dokumentation](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/await) .

## <a name="exception-handling"></a>Ausnahmebehandlung

Ausnahmen, die innerhalb einer Async-Methode auftreten, werden in der Aufgabe gespeichert und ausgelöst, `await`wenn die Aufgabe ausgeführt wird. Diese Ausnahmen können innerhalb eines try-catch-Blocks abgefangen und behandelt werden.

## <a name="cancellation"></a>Abbruch

Asynchrone Methoden, deren Abschluss lange dauert, sollten einen Abbruch unterstützen. In der Regel wird der Abbruch wie folgt aufgerufen:

- Ein `CancellationTokenSource` -Objekt wird erstellt.
- Die `CancellationTokenSource.Token` -Instanz wird an eine abbrechbare asynchrone Methode übermittelt.
- Der Abbruch wird durch Aufrufen der `CancellationTokenSource.Cancel` -Methode angefordert.

Der Task bricht dann sich selbst ab und bestätigt den Abbruch.

Weitere Informationen zum Abbrechen finden Sie unter [Fine-Tuning Your Async Application (C#) (Abstimmen der asynchronen Anwendung (C#))](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application).

## <a name="example"></a>Beispiel

Laden Sie die [xamarin-Beispiellösung](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (für IOS und Android) herunter, um ein funktionierendes `async` Beispiel `await` für und in Mobile Apps anzuzeigen. Der Beispielcode wird in diesem Abschnitt ausführlicher erläutert.

### <a name="writing-an-async-method"></a>Schreiben einer Async-Methode

Die folgende Methode veranschaulicht, wie Sie eine `async` -Methode mit `await`einer Ed-Aufgabe codieren:

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

Beachten Sie die folgenden Punkte:

-  Die Methoden Deklaration enthält `async` das-Schlüsselwort.
-  Der Rückgabetyp ist `Task<int>` , sodass der Aufruf `int` Code auf den Wert zugreifen kann, der in dieser Methode berechnet wird.
-  Die Return-Anweisung `return exampleInt;` ist ein ganzzahliges Objekt – die Tatsache, dass die `Task<int>` Methode zurückgibt, ist Teil der Verbesserungen der Sprache.


### <a name="calling-an-async-method-1"></a>Aufrufen einer Async-Methode 1

Dieser Click-Ereignishandler für die Schaltfläche befindet sich in der Android-Beispielanwendung, um die oben beschriebene Methode aufzurufen:

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

-  Der anonyme Delegat hat das Schlüsselwort "Async".
-  Die asynchrone Methode Download Homepage gibt eine Aufgabe <int> zurück, die in der sizetask-Variablen gespeichert ist.
-  Der Code wartet auf die sizetask-Variable.  *Dies* ist der Speicherort, an dem die Methode angehalten und die Steuerung an den aufrufenden Code zurückgegeben wird, bis die asynchrone Aufgabe in einem eigenen Thread abgeschlossen ist.
-  Die Ausführung wird *nicht* angehalten, wenn die Aufgabe in der ersten Zeile der-Methode erstellt wird, trotz der Aufgabe, die dort erstellt wird. Das Erwartungs Wort erwartet gibt den Speicherort an, an dem die Ausführung angehalten wird.
-  Wenn die asynchrone Aufgabe abgeschlossen ist, wird "intresult" festgelegt, und die Ausführung wird im ursprünglichen Thread von der Warteschlange fortgesetzt.


### <a name="calling-an-async-method-2"></a>Aufrufen einer Async-Methode 2

In der IOS-Beispielanwendung ist das Beispiel etwas anders geschrieben, um einen alternativen Ansatz zu veranschaulichen. Anstatt einen anonymen Delegaten zu verwenden, deklariert dieses `async` Beispiel einen Ereignishandler, der wie ein regulärer Ereignishandler zugewiesen wird:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Die Ereignishandlermethode wird dann wie hier gezeigt definiert:

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

-  Die Methode ist als `async` markiert, gibt jedoch zurück. `void` Dies erfolgt in der Regel nur bei Ereignis Handlern (andernfalls würden Sie `Task` oder `Task<TResult>` zurückgeben).
-  Der Code `await` für die `DownloadHomepage` Methode wird direkt für eine Zuweisung zu einer Variablen ( `intResult` ) im Gegensatz zum vorherigen Beispiel verwendet, bei dem `Task<int>` eine zwischen Variable verwendet wurde, um auf die Aufgabe zu verweisen.  *Dies* ist der Speicherort, an den die Steuerung an den Aufrufer zurückgegeben wird, bis die asynchrone Methode in einem anderen Thread abgeschlossen wurde.
-  Wenn die asynchrone Methode abgeschlossen und zurückgegeben wird, wird die Ausführung `await` bei der fortgesetzt, was bedeutet, dass das ganz Zahl Ergebnis zurückgegeben und dann in einem UI-Widget gerendert


## <a name="summary"></a>Zusammenfassung

Die Verwendung von "Async" und "warten" vereinfacht den Code, der erforderlich ist, um Vorgänge mit langer Ausführungsdauer für Hintergrundthreads zu erzeugen, ohne den Außerdem erleichtern Sie den Zugriff auf die Ergebnisse, wenn die Aufgabe abgeschlossen ist.

Dieses Dokument enthält eine Übersicht über die neuen sprach Schlüsselwörter und Beispiele für xamarin. IOS und xamarin. Android.



## <a name="related-links"></a>Verwandte Links

- [Asyncawait (Beispiel)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Rückrufe als "Gehe zu"-Anweisung unserer Generationen](https://tirania.org/blog/archive/2013/Aug-15.html)
- [Daten (IOS) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/data/)
- [HttpClient (IOS) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/httpclient/)
- [Mapkitsearch (IOS) (Beispiel)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Asynchrone Programmierung](https://docs.microsoft.com/dotnet/csharp/async)
- [Feinabstimmung der Async-Anwendung (C#)](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/fine-tuning-your-async-application)
- [Warten, Benutzeroberfläche und Deadlocks! Oh, mein!](https://devblogs.microsoft.com/pfxteam/await-and-ui-and-deadlocks-oh-my/)
- [Verarbeiten von Aufgaben nach Abschluss der Ausführung](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)
- [Task-based Asynchronous Pattern (TAP)](https://msdn.microsoft.com/library/hh873175.aspx) (Aufgabenbasiertes asynchrones Muster)
- [Asynchronität in C# 5 (Blog von Eric Lippert) – Informationen zur Einführung der Schlüsselwörter](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
