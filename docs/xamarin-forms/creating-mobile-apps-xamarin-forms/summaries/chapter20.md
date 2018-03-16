---
title: Zusammenfassung der Kapitel 20. Async und Datei-e/a
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ac316bc2cef04a80958c047427845dbdcc4137f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Zusammenfassung der Kapitel 20. Async und Datei-e/a

 Eine grafische Benutzeroberfläche muss sequenziell auf Benutzereingaben Ereignisse reagieren. Dies bedeutet, dass alle Verarbeitung von Benutzereingaben Ereignissen in einem einzelnen Thread, die häufig aufgerufen vorliegen muss die *Hauptthread* oder *UI-Thread*.

Benutzer erwarten, dass grafische Benutzeroberflächen reaktionsfähig ist. Dies bedeutet, dass ein Programm Benutzereingaben Ereignisse schnell verarbeiten muss. Wenn dies nicht möglich ist, muss Sie wird bei der Verarbeitung zum sekundären Ausführungsthreads gelandet.

Mehrere Beispielprogramme in diesem Buch verwendet die [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Klasse. In dieser Klasse die [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) Methode startet einen Arbeitsthread, der eine Rückruffunktion aufgerufen wird, wenn es vollständig ist. Allerdings, Callback-Funktion wird in den Arbeitsthread ausgeführt, damit das Programm aufrufen muss [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) Methode, um die Benutzeroberfläche zugreifen.

Ein moderner Ansatz für die asynchrone Verarbeitung ist in .NET bzw. c# verfügbar. Führen Sie dazu die [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) und [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) Klassen und anderen Typen in den [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) und [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) Namespaces sowie der C#-5.0 `async` und `await` Schlüsselwörter. Das ist, was in diesem Kapitel liegt der Schwerpunkt auf.

## <a name="from-callbacks-to-await"></a>Von Rückrufen zu "await"

Die `Page` Klasse selbst enthält drei asynchrone Methoden zum Anzeigen von Warnungen Feldern:

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Gibt eine `Task` Objekt
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Gibt eine `Task<bool>` Objekt
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Gibt eine `Task<string>` Objekt

Die `Task` Objekte anzugeben, dass diese Methoden implementieren Sie die Task-based Asynchronous Pattern, TAP genannt. Diese `Task` Objekte schnell von der Methode zurückgegeben. Die `Task<T>` zurückgeben, Werte bilden eine "Zusage", die einen Wert vom Typ `TResult` verfügbar sein wird, wenn die Aufgabe abgeschlossen ist. Die `Task` -Rückgabewert zeigt einer asynchronen Aktion abgeschlossen jedoch kein Wert zurückgegeben wird.

In all diesen Fällen die `Task` ist abgeschlossen, wenn der Benutzer das Warnung schließt.  

### <a name="an-alert-with-callbacks"></a>Eine Warnung mit Rückrufen

Die [ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) Beispiel veranschaulicht die Behandlung `Task<bool>` -Objekte zurückgegeben und `Device.BeginInvokeOnMainThread` Aufrufe mithilfe von Rückrufmethoden.

### <a name="an-alert-with-lambdas"></a>Eine Warnung mit Lambda-Ausdrücke

Die [ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) Beispiel veranschaulicht, wie anonyme Lambda-Funktionen für die Behandlung von `Task` und `Device.BeginInvokeOnMainThread` aufrufen.  

### <a name="an-alert-with-await"></a>Eine Warnung mit der "await"

Eine einfachere Möglichkeit besteht darin, die `async` und `await` Schlüsselwörter, die in c# 5 eingeführt. Die [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) Beispiel veranschaulicht ihre Verwendung.

### <a name="an-alert-with-nothing"></a>Eine Warnung mit ' Nothing '

Wenn die asynchrone Methode zurückgibt `Task` statt `Task<TResult>`, und klicken Sie dann das Programm muss keiner der folgenden Methoden verwenden, wenn sie nicht wissen, wann die asynchrone Aufgabe abgeschlossen ist. Die [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) Beispiel veranschaulicht dies.

### <a name="saving-program-settings-asynchronously"></a>Speichern die Einstellungen für das Programm asynchron

Die [ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) Beispiel veranschaulicht die Verwendung von der [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Methode `Application` Einstellungen für das Programm zu speichern, da sich diese ohne ändern Überschreiben der `OnSleep` Methode.

### <a name="a-platform-independent-timer"></a>Ein plattformunabhängiges timer

Es ist möglich, verwenden Sie [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) um ein plattformunabhängiges Timer erstellen. Die [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) Beispiel veranschaulicht dies.

## <a name="file-inputoutput"></a>Datei-e/a

Bisher .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) Namespace wurde die Quelle der Datei-e/a-Support. Auch wenn einige Methoden in diesem Namespace asynchrone Vorgänge unterstützt, jedoch die meisten nicht. Der Namespace unterstützt auch mehrere einfacher Methodenaufrufe, die komplexe Datei-e/a-Funktionen ausführen.

### <a name="good-news-and-bad-news"></a>Gute Neuigkeiten und schlechte Nachrichten

Alle Plattformen, die vom lokalen Speicher für Xamarin.Forms unterstützen Anwendung unterstützt &mdash; Speicher, der für die Anwendung privat ist.

Die Xamarin.iOS und Xamarin.Android-Bibliotheken enthalten eine Version von .NET, die dieser beiden Plattformen ausdrücklich Xamarin zugeschnitten ist. Dazu gehören Klassen aus `System.IO` , mit der Datei-e/a mit lokalem Speicher der Anwendung in dieser beiden Plattformen ausgeführt werden können.

Jedoch, wenn Sie für diese Suche `System.IO` Klassen in einer PCL Xamarin.Forms, Sie wird nicht gefunden werden. Das Problem ist, vollständig neu gestaltete Microsoft-Datei-e/a für die Windows-Runtime-API. Verwenden Sie Programmen, die mit Windows 8.1, Windows Phone 8.1 und universellen Windows-Plattform nicht `System.IO` für Datei-e/a.

Dies bedeutet, dass Sie verwenden, müssen die [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (zuerst in behandelten [ **Kapitel 9. Clientplattform-spezifische API-Aufrufe** ](chapter09.md) Datei-e/a zu implementieren.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Eine erste Screenshot auf plattformübergreifende Datei-e/a

Die [ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) Beispiel definiert eine [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) für Datei-e/a und Implementierungen dieser Schnittstelle in allen Plattformen unterstützt. Jedoch arbeiten nicht die Windows-Runtime-Implementierungen mit den Methoden dieser Benutzeroberfläche, da die Windows-Runtime-Datei-e/a-Methoden asynchron.

### <a name="accommodating-windows-runtime-file-io"></a>Unterstützung von Windows-Runtime-Datei-e/a

Programme, die unter Windows-Runtime ausgeführt werden. verwenden Sie Klassen in der [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) und [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) -Namespaces für Datei-e/a, einschließlich der lokalen Speicher der Anwendung. Da Microsoft, dass jeder Vorgang festgestellt, die erfordern, dass mehr als 50 Millisekunden asynchron, um zu vermeiden, den UI-Thread blockiert werden soll, sind diese Methoden der Datei-e/a-hauptsächlich asynchron.

Der Code für dieses neue Konzept veranschaulichen werden in einer Bibliothek, damit sie von einer anderen Anwendung verwendet werden kann.

## <a name="platform-specific-libraries"></a>Plattformspezifische Bibliotheken

Es empfiehlt sich zum Speichern von wiederverwendbaren Codes in Bibliotheken. Dies ist natürlich schwieriger, wenn unterschiedliche Arten von wiederverwendbarem Code für vollkommen andere Betriebssysteme sind.

Die [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung veranschaulicht eine Methode. Diese Lösung enthält sieben verschiedene Projekten:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), a normal Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), an iOS class library
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), a Universal Windows class library
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), a PCL for Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), a PCL for Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), a shared project for code that is common to all the Windows platforms

Alle einzelnen Plattformprojekte (mit Ausnahme von **Xamarin.FormsBook.Platform.WinRT**) verfügen über Verweise auf **Xamarin.FormsBook.Platform**. Die drei Windows-Projekte besitzen einen Verweis auf **Xamarin.FormsBook.Platform.WinRT**.

Alle Projekte enthalten einen statischen `Toolkit.Init` Methode, um sicherzustellen, dass die Bibliothek geladen wird, wenn er nicht direkt von einem Projekt in einer Xamarin.Forms-Anwendungsprojektmappe referenziert wird.

Die **Xamarin.FormsBook.Platform** Projekt enthält das neue [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) Schnittstelle. Alle Methoden verfügen jetzt über Namen mit `Async` Suffixe und der Rückgabewert `Task` Objekte.

Die **Xamarin.FormsBook.Platform.WinRT** Projekt enthält die [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Windows-Runtime-Klasse.

Die **Xamarin.FormsBook.Platform.iOS** Projekt enthält die [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) iOS-Klasse. Diese Methoden müssen nun asynchron sein. Einige der Methoden verwenden Sie der asynchronen Versionen der Methoden, die in definierten `StreamWriter` und `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) und [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). Andere konvertieren ein Ergebnis, um eine `Task` -Objekt unter Verwendung der [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) Methode.

Die **Xamarin.FormsBook.Platform.Android** Projekt enthält ein ähnliches [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) Android-Klasse.

Die **Xamarin.FormsBook.Platform** Projekt enthält außerdem eine [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) -Klasse, die die Verwendung von vereinfacht die `DependencyService` Objekt.

Um diese Bibliotheken verwenden zu können, muss eine Lösung enthalten alle Projekte in der **Xamarin.FormsBook.Platform** Lösung und jede Anwendung Projekte müssen einen Verweis auf die entsprechende Bibliothek in  **Xamarin.FormsBook.Platform**.

Die [ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) Lösung veranschaulicht, wie die **Xamarin.FormsBook.Platform** Bibliotheken. Jedes der Projekte hat einen Aufruf von `Toolkit.Init`. Die Anwendung nutzt die asynchrone Datei e/a-Funktionen.

### <a name="keeping-it-in-the-background"></a>Da diese im Hintergrund

Methoden in Bibliotheken, die mehrere asynchrone Methoden aufrufen &mdash; wie z. B. die `WriteFileAsync` und `ReadFileASync` Methoden in der Windows-Runtime [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Klasse &mdash; können vorgenommen werden, in einem gewissen eine effizientere mithilfe der [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) Methode zur Vermeidung zurück an den Benutzeroberflächenthread wechseln.

### <a name="dont-block-the-ui-thread"></a>Blockieren Sie nicht im UI-Thread!

Es ist verlockend, vermeiden Sie die Verwendung von `ContinueWith` oder `await` mithilfe der [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) -Eigenschaft für die Methoden anwenden. Dies sollte vermieden werden, für die UI-Thread blockieren oder sogar hängen von der Anwendung kann.

## <a name="your-own-awaitable-methods"></a>Eigene awaitable-Methoden

Sie können Code asynchron ausführen, durch Übergabe eines der [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) Methoden. Sie können Aufrufen `Task.Run` innerhalb einer Async-Methode, die einige der Aufwand behandelt.

Die verschiedenen `Task.Run` Muster werden unten erläutert.

### <a name="the-basic-mandelbrot-set"></a>Der grundlegende Mandelbrot

Zum Zeichnen der Mandelbrot in Echtzeit, legen die [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek hat eine [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) ähnlich demjenigen in der Struktur der `System.Numerics` Namespace.

Die [ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) Beispiel verfügt über eine `CalculateMandeblotAsync` Methode in einer Code-Behind-Datei, die der grundlegenden schwarzweiße Mandelbrot berechnet und verwendet [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs), in einer Bitmap eingefügt werden soll.

### <a name="marking-progress"></a>Markieren von Status

Um den Fortschritt von einer asynchronen Methode instanziiert werden können eine [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) Klasse, und definieren Sie die asynchrone Methode, um ein Argument des Typs haben [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). Dies wird dargestellt, der [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) Beispiel.

### <a name="cancelling-the-job"></a>Abbrechen des Auftrags

Sie können auch eine asynchrone Methode, um abbrechbar ist schreiben. Sie beginnen mit einer Klasse mit dem Namen [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). Die [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) Eigenschaft ist ein Wert vom Typ [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). Dies wird an die asynchrone Funktion übergeben. Ein Programm aufruft, die [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) Methode `CancellationTokenSource` (in der Regel als Reaktion auf eine Aktion vom Benutzer) zum Abbrechen des asynchronen Funktion.

Die asynchrone Methode kann in regelmäßigen Abständen Überprüfen der [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) Eigenschaft `CancellationToken` und beendet, wenn die Eigenschaft kann `true`, oder rufen Sie einfach die [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) Methode im wobei die Methode endet mit einem [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

Die [ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) Beispiel veranschaulicht die Verwendung einer Funktion abgebrochen werden können.

### <a name="an-mvvm-mandelbrot"></a>Ein Mandelbrot MVVM

Die [ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) Beispiel verfügt über eine erweiterte Benutzeroberfläche und basiert größtenteils auf eine [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) und [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)Klassen:

[![Dreifacher Screenshot des Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "Mandelbrot MVVM")

## <a name="back-to-the-web"></a>Im Web an

Die [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Klasse zur Verwendung in einigen Beispielen verwendet ein altmodisch asynchrone Protokoll, die Asynchronous Programming Model oder APM genannt. Sie können eine solche Klasse konvertieren, auf das moderne TAP-Protokoll mithilfe eines der der `FromAsync` Methoden in der [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) Klasse. Die [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) Beispiel veranschaulicht dies.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 20 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Kapitel 20-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Working with Files (Arbeiten mit Dateien)](~/xamarin-forms/app-fundamentals/files.md)
