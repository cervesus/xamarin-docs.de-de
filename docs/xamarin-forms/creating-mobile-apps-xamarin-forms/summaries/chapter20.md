---
title: 'Zusammenfassung von Kapitel 20: Asynchrone Verarbeitung und Datei-E/A'
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 20: Asynchrone Verarbeitung und Datei-E/A'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 283273e6ee28cc5cd1a61169f38bfcd1dd1726d8
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771036"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Zusammenfassung von Kapitel 20: Asynchrone Verarbeitung und Datei-E/A

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)

> [!NOTE] 
> In den Anmerkungen auf dieser Seite wird erläutert, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

 Eine grafische Benutzeroberfläche muss sequenziell auf Benutzereingabeereignisse reagieren. Dies impliziert, dass die gesamte Verarbeitung von Benutzereingabeereignissen in einem einzigen Thread erfolgen muss, der häufig als *Hauptthread* oder *UI-Thread* bezeichnet wird.

Benutzer erwarten von grafischen Benutzeroberflächen eine kurze Reaktionszeit. Dies bedeutet, dass ein Programm Benutzereingabeereignisse schnell verarbeiten muss. Wenn dies nicht möglich ist, muss die Verarbeitung auf sekundäre Verarbeitungsthreads verlagert werden.

In verschiedenen Beispielprogrammen in diesem Buch wurde die [`WebRequest`](xref:System.Net.WebRequest)-Klasse verwendet. In dieser Klasse startet die [`BeginGetResponse`](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object))-Methode einen Arbeitsthread, der nach seinem Abschluss eine Rückruffunktion aufruft. Diese Rückruffunktion wird jedoch im Arbeitsthread ausgeführt, sodass das Programm die [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))-Methode aufrufen muss, um auf die Benutzerschnittstelle zuzugreifen.

> [!NOTE]
> Xamarin.Forms-Programme sollten eher [`HttpClient`](xref:System.Net.Http.HttpClient) anstelle von [`WebRequest`](xref:System.Net.WebRequest) für den Zugriff auf Dateien über das Internet verwenden. `HttpClient` unterstützt asynchrone Vorgänge.

Ein moderner Ansatz für die asynchrone Verarbeitung steht in .NET und C# zur Verfügung. Dies schließt die Klassen [`Task`](xref:System.Threading.Tasks.Task) und [`Task<TResult>`](xref:System.Threading.Tasks.Task`1), weitere Typen in den Namespaces [`System.Threading`](xref:System.Threading) und [`System.Threading.Tasks`](xref:System.Threading.Tasks) sowie die C# 5.0-Schlüsselwörter `async` und `await` ein. Darauf konzentriert sich dieses Kapitel.

## <a name="from-callbacks-to-await"></a>Von Rückrufen zu „await“

Die `Page`-Klasse selbst enthält drei asynchrone Methoden zur Anzeige von Warnfeldern:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) gibt ein `Task`-Objekt zurück.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) gibt ein `Task<bool>`-Objekt zurück.
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) gibt ein `Task<string>`-Objekt zurück.

Die `Task`-Objekte weisen darauf hin, dass diese Methoden das aufgabenbasierte asynchrone Muster (Task-based Asynchronous Pattern, TAP) verwenden. Diese `Task`-Objekte werden von der Methode schnell zurückgegeben. Die `Task<T>`-Rückgabewerte stellen eine „Zusage“ (Promise) dar, dass ein Wert vom Typ `TResult` zur Verfügung steht, wenn die Aufgabe abgeschlossen wurde. Der `Task`-Rückgabewert weist auf eine asynchrone Aktion hin, die ausgeführt wird, aber keinen Wert zurückgibt.

In all diesen Fällen ist `Task` abgeschlossen, wenn der Benutzer das Warnfeld schließt.  

### <a name="an-alert-with-callbacks"></a>Eine Warnung mit Rückrufen

Das Beispiel [**AlertCallbacks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) veranschaulicht, wie `Task<bool>`-Rückgabeobjekte und `Device.BeginInvokeOnMainThread`-Aufrufe mithilfe von Rückrufmethoden verarbeitet werden.

### <a name="an-alert-with-lambdas"></a>Warnung mit Lambda-Ausdrücken

Das Beispiel [**AlertLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) veranschaulicht, wie anonyme Lambda-Funktionen zum Verarbeiten von `Task`- und `Device.BeginInvokeOnMainThread`-Aufrufen verwendet werden.  

### <a name="an-alert-with-await"></a>Warnung mit „await“

Ein geradlinigerer Ansatz umfasst die in C# 5 eingeführten Schlüsselwörter `async` und `await`. Ihre Verwendung wird im Beispiel [**AlertAwait**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) veranschaulicht.

### <a name="an-alert-with-nothing"></a>Warnung mit „nothing“

Wenn die asynchrone Methode `Task` anstelle von `Task<TResult>` zurückgibt, muss das Programm keine dieser Techniken verwenden, wenn es nicht wissen muss, wann die asynchrone Aufgabe abgeschlossen ist. Dies wird im Beispiel [**NothingAlert**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) gezeigt.

### <a name="saving-program-settings-asynchronously"></a>Asynchrones Speichern der Programmeinstellungen

Das Beispiel [**SaveProgramChanges**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) veranschaulicht die Verwendung der [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync)-Methode von `Application` zum Speichern von Programmeinstellungen, wenn sich diese ohne Überschreiben der `OnSleep`-Methode ändern.

### <a name="a-platform-independent-timer"></a>Plattformunabhängiger Zeitgeber

Es ist möglich, mit [`Task.Delay`](xref:System.Threading.Tasks.Task.Delay(System.Int32)) einen plattformunabhängigen Zeitgeber zu erstellen. Dies wird im Beispiel [**TaskDelayClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) gezeigt.

## <a name="file-inputoutput"></a>Dateieingabe/-ausgabe

Traditionell war der .NET-Namespace [`System.IO`](xref:System.IO) die Quelle zur Unterstützung der Datei-E/A. Einige Methoden in diesem Namensraum unterstützen asynchrone Vorgänge, die meisten anderen jedoch nicht. Der Namensraum unterstützt außerdem mehrere einfache Methodenaufrufe, die anspruchsvolle Datei-E/A-Funktionen ausführen.

### <a name="good-news-and-bad-news"></a>Gute und schlechte Nachrichten

Alle von Xamarin.Forms unterstützten Plattformen bieten Unterstützung für den lokalen Speicher der Anwendung, also Speicher, der als privat für die Anwendung festgelegt ist.

Die Xamarin.iOS- und Xamarin.Android-Bibliotheken umfassen eine Version von .NET, die Xamarin explizit auf diese zwei Plattformen zugeschnitten hat. Sie enthalten Klassen von `System.IO`, die Sie zum Ausführen von Datei-E/A-Vorgängen mit dem lokalen Anwendungsspeicher in diesen zwei Plattformen nutzen können.

Wenn Sie jedoch in einer Xamarin.Forms-PCL nach diesen `System.IO`-Klassen suchen, werden Sie sie nicht finden. Das Problem ist, dass Microsoft die Datei-E/A für die Windows-Runtime-API komplett überarbeitet hat. Programme, die auf Windows 8.1, Windows Phone 8.1 und die universelle Windows-Plattform abzielen, verwenden `System.IO` nicht für Datei-E/A.

Dies bedeutet, dass Sie zur Implementierung der Datei-E/A weiterhin den [`DependencyService`](xref:Xamarin.Forms.DependencyService) verwenden müssen (erstmals erörtert in [**Kapitel 9: Plattformspezifische API-Aufrufe**](chapter09.md)).

> [!NOTE]
> Portable Klassenbibliotheken wurden durch .NET Standard 2.0-Bibliotheken ersetzt, und .NET Standard 2.0 unterstützt [`System.IO`](xref:System.IO)-Typen für alle Xamarin.Forms-Plattformen. Für die meisten Datei-E/A-Aufgaben ist es nicht länger nötig, einen `DependencyService` zu verwenden. Informationen zu einem moderneren Ansatz für die Datei-E/A finden Sie unter [Dateiverarbeitung in Xamarin.Forms](~/xamarin-forms/data-cloud/data/files.md).

### <a name="a-first-shot-at-cross-platform-file-io"></a>Ein erster Versuch einer plattformübergreifenden Datei-E/A

Das Beispiel [**TextFileTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) definiert eine [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs)-Schnittstelle für Datei-E/A und Implementierungen dieser Schnittstelle auf allen Plattformen. Die Windows-Runtime-Implementierungen funktionieren jedoch nicht mit den Methoden in dieser Schnittstelle, weil die Datei-E/A-Methoden der Windows-Runtime asynchron sind.

### <a name="accommodating-windows-runtime-file-io"></a>Verarbeiten von Datei-E/A-Vorgängen in der Windows-Runtime

Programme, die unter der Windows-Runtime ausgeführt werden, verwenden Klassen in den Namespaces [`Windows.Storage`](/uwp/api/Windows.Storage) und [`Windows.Storage.Streams`](/uwp/api/Windows.Storage.Streams) für Datei-E/A-Vorgänge, einschließlich des lokalen Speichers der Anwendung. Da Microsoft festgelegt hat, dass jeder Vorgang mit einer Dauer von mehr als 50 Millisekunden asynchron sein sollte, um den UI-Thread nicht zu blockieren, sind diese Datei-E/A-Methoden meist asynchron.

Der Code zur Veranschaulichung dieses neuen Ansatzes ist in einer Bibliothek enthalten, damit er von anderen Anwendungen verwendet werden kann.

## <a name="platform-specific-libraries"></a>Plattformspezifische Bibliotheken

Es ist vorteilhaft, wiederverwendbaren Code in Bibliotheken zu speichern. Dies ist natürlich schwieriger, wenn verschiedene Teile des wiederverwendbaren Codes für völlig unterschiedliche Betriebssysteme bestimmt sind.

Die Projektmappe [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) zeigt einen Ansatz. Diese Projektmappe enthält verschiedene Projekte:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), eine normale Xamarin.Forms-PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), eine iOS-Klassenbibliothek
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), eine Android-Klassenbibliothek
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), eine UWP-Klassenbibliothek
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), ein freigegebenes Projekt für Code, der allen Windows-Plattformen gemeinsam ist

Alle Plattformprojekte (mit Ausnahme von **Xamarin.FormsBook.Platform.WinRT**) enthalten Verweise auf **Xamarin.FormsBook.Platform**. Die drei Windows-Projekte enthalten einen Verweis auf **Xamarin.FormsBook.Platform.WinRT**.

Alle Projekte enthalten eine statische `Toolkit.Init`-Methode, um sicherzustellen, dass die Bibliothek geladen ist, wenn sie nicht direkt durch ein Projekt in einer Xamarin.Forms-Anwendungsprojektmappe referenziert wird.

Das Projekt **Xamarin.FormsBook.Platform** enthält die neue [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs)-Schnittstelle. Alle Methoden weisen jetzt Namen mit `Async`-Suffixen auf und geben `Task`-Objekte zurück.

Das **Xamarin.FormsBook.Platform.WinRT**-Projekt enthält die [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs)-Klasse für die Windows-Runtime.

Das **Xamarin.FormsBook.Platform.iOS**-Projekt enthält die [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs)-Klasse für iOS. Diese Methoden müssen ab sofort asynchron sein. Einige der Methoden verwenden die asynchronen Versionen der in `StreamWriter` und `StreamReader` definierten Methoden: [`WriteAsync`](xref:System.IO.StreamWriter.WriteAsync(System.String)) und [`ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync). Andere konvertieren ein Ergebnis mithilfe der [`FromResult`](xref:System.Threading.Tasks.Task.FromResult*)-Methode in ein `Task`-Objekt.

Das Projekt **Xamarin.FormsBook.Platform.Android** enthält eine ähnliche [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs)-Klasse für Android.

Das Projekt **Xamarin.FormsBook.Platform** enthält ebenfalls eine [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs)-Klasse, die die Verwendung des `DependencyService`-Objekts vereinfacht.

Um diese Bibliotheken zu verwenden, muss eine Anwendungsprojektmappe alle Projekte in der Projektmappe **Xamarin.FormsBook.Platform** enthalten, und jedes der Anwendungsprojekte muss einen Verweis auf die entsprechende Bibliothek in **Xamarin.FormsBook.Platform** enthalten.

Die Projektmappe [**TextFileAsync**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) veranschaulicht die Verwendung der **Xamarin.FormsBook.Platform**-Bibliotheken. Jedes der Projekte enthält einen Aufruf an `Toolkit.Init`. Die Anwendung nutzt asynchrone Datei-E/A-Funktionen.

### <a name="keeping-it-in-the-background"></a>Ausführung im Hintergrund

Methoden in Bibliotheken, die mehrere asynchrone Methoden aufrufen &mdash; z. B. die Methoden `WriteFileAsync` und `ReadFileASync` in der [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs)-Klasse der Windows-Runtime &mdash; können durch Verwendung der [`ConfigureAwait`](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean))-Methode etwas effizienter gestaltet werden, um einen Wechsel zurück zum Thread der Benutzerschnittstelle zu vermeiden.

### <a name="dont-block-the-ui-thread"></a>Keine Blockierung des UI-Treads!

Gelegentlich ist es verlockend, die Verwendung von `ContinueWith` oder `await` zu vermeiden, indem man die Eigenschaft [`Result`](xref:System.Threading.Tasks.Task`1.Result) auf die Methoden anwendet. Dies sollte vermieden werden, da es den UI-Thread blockieren oder sogar dazu führen kann, dass die Anwendung nicht mehr reagiert.

## <a name="your-own-awaitable-methods"></a>Eigene await-fähige Methoden

Sie können einigen Code asynchron ausführen, indem Sie ihn an eine der [`Task.Run`](xref:System.Threading.Tasks.Task.Run(System.Action))-Methoden übergeben. Sie können `Task.Run` innerhalb einer asynchronen Methode aufrufen, die einen Teil der Arbeit übernimmt.

Die verschiedenen `Task.Run`-Muster werden nachfolgend diskutiert.

### <a name="the-basic-mandelbrot-set"></a>Die grundlegende Mandelbrot-Menge

Um die Mandelbrot-Menge in Echtzeit zu zeichnen, umfasst die [**Xamarin.Forms.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek eine [`Complex`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs)-Struktur, die der im Namespace `System.Numerics` ähnlich ist.

Das [**MandelbrotSet**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet)-Beispiel enthält eine `CalculateMandeblotAsync`-Methode in der zugehörigen CodeBehind-Datei, die die grundlegende Schwarz-Weiß-Mandelbrot-Menge berechnet und [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) zu deren Platzierung auf einer Bitmap verwendet.

### <a name="marking-progress"></a>Melden des Fortschritts

Um den Fortschritt einer asynchronen Methode zu melden, können Sie eine [`Progress<T>`](xref:System.Progress`1)-Klasse instanziieren und Ihre asynchrone Methode so definieren, dass sie ein Argument vom Typ [`IProgress<T>`](xref:System.IProgress`1) aufweist. Dies wird im Beispiel [**MandelbrotProgress**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) gezeigt.

### <a name="cancelling-the-job"></a>Abbrechen des Auftrags

Sie können eine asynchrone Methode auch so schreiben, dass sie abgebrochen werden kann. Sie beginnen mit einer Klasse namens [`CancellationTokenSource`](xref:System.Threading.CancellationTokenSource). Die [`Token`](xref:System.Threading.CancellationTokenSource.Token)-Eigenschaft ist ein Wert vom Typ [`CancellationToken`](xref:System.Threading.CancellationToken). Dieser Wert wird an eine asynchrone Funktion übergeben. Ein Programm ruft die [`Cancel`](xref:System.Threading.CancellationTokenSource.Cancel)-Methode von `CancellationTokenSource` auf (im Allgemeinen als Reaktion auf eine Benutzeraktion), um die asynchrone Funktion abzubrechen.

Die asynchrone Methode kann in regelmäßigen Abständen die Eigenschaft [`IsCancellationRequested`](xref:System.Threading.CancellationToken.IsCancellationRequested) von `CancellationToken` überprüfen und beendet werden, wenn die Eigenschaft `true` lautet, oder einfach die Methode [`ThrowIfCancellationRequested`](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) aufrufen. In letzterem Fall wird die Methode mit einer [`OperationCancelledException`](xref:System.OperationCanceledException) beendet.

Das [**MandelbrotCancellation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation)-Beispiel veranschaulicht die Verwendung einer Funktion, die abgebrochen werden kann.

### <a name="an-mvvm-mandelbrot"></a>Mandelbrot (MVVM)

Das [**MandelbrotXF**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF)-Beispiel umfasst eine umfangreichere Benutzerschnittstelle und basiert größtenteils auf den Klassen [`MandelbrotModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) und [`MandelbrotViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs):

[![Drei Screenshots von MandelbrotXF](images/ch20fg13-small.png "Mandelbrot (MVVM)")](images/ch20fg13-large.png#lightbox "Mandelbrot (MVVM)")

## <a name="back-to-the-web"></a>Zurück zum Web

Die in einigen Beispielen verwendete [`WebRequest`](xref:System.Net.WebRequest)-Klasse nutzt ein altmodisches asynchrones Protokoll: APM (Asynchronous Programming Model). Sie können eine solche Klasse in das moderne TAP-Protokoll konvertieren, indem Sie eine der `FromAsync`-Methoden in der [`TaskFactory`](xref:System.Threading.Tasks.TaskFactory`1)-Klasse verwenden. Dies wird im Beispiel [**ApmToTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 20 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Kapitel 20 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Working with Files (Arbeiten mit Dateien)](~/xamarin-forms/data-cloud/data/files.md)
