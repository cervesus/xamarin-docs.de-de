---
title: Zusammenfassung der Kapitel 20. Asynchrone Verarbeitung und Datei-e/a
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 20. Asynchrone Verarbeitung und Datei-e/a'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: d606432174807498fd458470647109de4fa0b6b4
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156729"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Zusammenfassung der Kapitel 20. Asynchrone Verarbeitung und Datei-e/a

> [!NOTE] 
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

 Eine grafische Benutzeroberfläche muss Benutzereingaben reagieren auf Ereignisse sequenziell. Dies bedeutet, dass die gesamte Verarbeitung von Ereignissen von Benutzereingaben in einem einzigen Thread, der häufig dem Namen erfolgen muss die *Hauptthread* oder *UI-Thread*.

Benutzer erwarten eine grafische Benutzeroberflächen zu reagieren. Dies bedeutet, dass ein Programm Benutzereingabe-Ereignisse schnell verarbeiten muss. Wenn dies nicht möglich ist, muss die wird bei der Verarbeitung zu sekundären Ausführungsthreads verbannt werden.

Mehrere Beispielprogramme in diesem Buch verwendet die [ `WebRequest` ](xref:System.Net.WebRequest) Klasse. In dieser Klasse die [ `BeginGetReponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) Methode startet einen Arbeitsthread, der eine Callback-Funktion aufruft, sobald dieser abgeschlossen ist. Jedoch diese Callback-Funktion wird in der Arbeitsthread ausgeführt, sodass die Anwendung aufrufen muss [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) Methode auf der Benutzeroberfläche zugreifen.

> [!NOTE]
> Xamarin.Forms-Programme verwenden sollten [ `HttpClient` ](xref:System.Net.Http.HttpClient) statt [ `WebRequest` ](xref:System.Net.WebRequest) für den Zugriff auf Dateien über das Internet. `HttpClient` asynchrone Vorgänge unterstützt.

Ein moderner Ansatz für die asynchrone Verarbeitung ist in .NET und c# verfügbar. Dies umfasst die [ `Task` ](xref:System.Threading.Tasks.Task) und [ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1) Klassen und anderen Typen in der [ `System.Threading` ](xref:System.Threading) und [ `System.Threading.Tasks` ](xref:System.Threading.Tasks) Namespaces sowie die c# 5.0 `async` und `await` Schlüsselwörter. Das ist was Schwerpunkt in diesem Kapitel.

## <a name="from-callbacks-to-await"></a>Von Rückrufe zu "await"

Die `Page` Klasse selbst enthält drei asynchrone Methoden zum Anzeigen von Warnungen Feldern:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) Gibt eine `Task` Objekt
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Gibt eine `Task<bool>` Objekt
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) Gibt eine `Task<string>` Objekt

Die `Task` Objekte anzugeben, dass diese Methoden implementieren Sie die Task-based Asynchronous Pattern, TAP genannt. Diese `Task` Objekte schnell von der Methode zurückgegeben. Die `Task<T>` zurückgeben, Werte bilden eine "Zusage", die einen Wert vom Typ `TResult` verfügbar sein wird, wenn die Aufgabe abgeschlossen ist. Die `Task` Rückgabewert gibt an einer asynchronen Aktion abgeschlossen hat aber kein Wert zurückgegeben wird.

In allen diesen Fällen die `Task` ist abgeschlossen, wenn der Benutzer das Warnung schließt.  

### <a name="an-alert-with-callbacks"></a>Eine Warnung mit Rückrufen

Die [ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) Beispiel Behandlung veranschaulicht `Task<bool>` Objekte zurückgegeben und `Device.BeginInvokeOnMainThread` Aufrufe, die Verwendung von Rückrufmethoden.

### <a name="an-alert-with-lambdas"></a>Eine Warnung mit Lambda-Ausdrücken

Die [ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) Beispiel veranschaulicht, wie anonymer Lambda-Funktionen für die Behandlung `Task` und `Device.BeginInvokeOnMainThread` aufrufen.  

### <a name="an-alert-with-await"></a>Eine Warnung mit "await"

Ein einfacherer Ansatz umfasst die `async` und `await` Schlüsselwörter in c# 5 eingeführt. Die [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) Beispiel veranschaulicht ihre Verwendung.

### <a name="an-alert-with-nothing"></a>Eine Warnung mit "nothing"

Wenn die asynchrone Methode zurückgibt `Task` statt `Task<TResult>`, und klicken Sie dann das Programm muss keine der folgenden Methoden verwenden, wenn sie nicht wissen, wenn die asynchrone Aufgabe abgeschlossen ist. Die [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) veranschaulicht dies.

### <a name="saving-program-settings-asynchronously"></a>Speichern die Einstellungen des Programms asynchron

Die [ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) Beispiel veranschaulicht die Verwendung von der [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) -Methode der `Application` , Einstellungen für das Programm zu speichern, da sie ohne ändern Überschreiben der `OnSleep` Methode.

### <a name="a-platform-independent-timer"></a>Ein plattformunabhängiges timer

Es ist möglich, verwenden Sie [ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32)) auf einen plattformunabhängigen Zeitgeber zu erstellen. Die [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) veranschaulicht dies.

## <a name="file-inputoutput"></a>Datei-e/a

In der Vergangenheit .NET [ `System.IO` ](xref:System.IO) Namespace wurde die Quelle der e/a-Unterstützung. Obwohl einige Methoden in diesem Namespace asynchrone Vorgänge zu unterstützen, werden in den meisten nicht. Der Namespace unterstützt auch mehrere einfache Methodenaufrufe, die anspruchsvolle Datei e/a-Funktionen ausführen.

### <a name="good-news-and-bad-news"></a>Gute Neuigkeiten und schlecht

Alle Plattformen, die vom lokalen Speicher für Xamarin.Forms unterstützen Anwendung unterstützt &mdash; Speicher, der die Anwendung privat ist.

Die Xamarin.iOS und Xamarin.Android-Bibliotheken enthalten eine Version von .NET, mit der Xamarin ausdrücklich für diese beiden Plattformen angepasst ist. Diese umfassen Klassen aus `System.IO` , dass Sie zum Ausführen von Datei-e/a mit lokalem Speicher der Anwendung in dieser beiden Plattformen verwenden können.

Allerdings bei der Suche für diese `System.IO` Klassen in einer Xamarin.Forms-PCL, Sie wird nicht gefunden werden. Das Problem ist Microsoft vollständig überarbeitete Datei e/a-enthält die Windows-Runtime-API. Verwenden Sie Programme, die für Windows 8.1, Windows Phone 8.1 und die universelle Windows-Plattform nicht `System.IO` für Datei e/a.

Dies bedeutet, dass Sie verwenden, müssen die [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (zuerst in beschriebenen [ **Kapitel 9. Clientplattform-spezifische API-Aufrufe** ](chapter09.md) Datei-e/a zu implementieren.

> [!NOTE]
> Portable Bibliotheken der Klasse durch .NET Standard 2.0-Bibliotheken ersetzt wurden, und unterstützt .NET Standard 2.0 [ `System.IO` ](xref:System.IO) Typen für alle Xamarin.Forms-Plattformen. Es ist nicht mehr erforderlich, verwenden eine `DependencyService` für die meisten e/a-Aufgaben. Finden Sie unter [Dateibehandlung in Xamarin.Forms](~/xamarin-forms/app-fundamentals/files.md) einen moderneren Ansatz für Datei e/a.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Eine erste Abschlag Cross-Platform-Datei-e/a

Die [ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) Beispiel definiert eine [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) Schnittstelle für Datei-e/a und Implementierungen dieser Schnittstelle in allen Plattformen. Funktionieren jedoch nicht die Windows-Runtime-Implementierungen mit den Methoden dieser Benutzeroberfläche, da die Windows-Runtime-Datei-e/a-Methoden asynchron sind.

### <a name="accommodating-windows-runtime-file-io"></a>Sodass die Windows-Runtime-Datei-e/a

Programme, die unter der Windows-Runtime ausgeführt werden. verwenden Sie Klassen in der [ `Windows.Storage` ](/uwp/api/Windows.Storage) und [ `Windows.Storage.Streams` ](/uwp/api/Windows.Storage.Streams) Namespaces für Datei-e/a, einschließlich lokalen Anwendungsspeicher. Da Microsoft, dass alle Vorgänge festgestellt, die erfordern, dass mehr als 50 Millisekunden asynchron, um zu vermeiden, die UI-Thread blockiert werden soll, sind diese Methoden der Datei-e/a-hauptsächlich asynchron.

Der Code, dem dieser neuen Ansatz veranschaulicht werden in einer Bibliothek, damit sie von anderen Anwendungen verwendet werden kann.

## <a name="platform-specific-libraries"></a>Plattformspezifische Bibliotheken

Es ist vorteilhaft, wiederverwendbaren Code in Bibliotheken zu speichern. Dies ist natürlich noch schwieriger, wenn es sich bei verschiedenen Teile von wiederverwendbarem Code völlig unterschiedliche Betriebssysteme sind.

Die [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung veranschaulicht einen Ansatz. Diese Lösung umfasst sieben verschiedene Projekte:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), a normal Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), eine iOS-Klassenbibliothek
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), an Android class library
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), eine universelle Windows-Klassenbibliothek
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), a shared project for code that is common to all the Windows platforms

Die einzelnen Plattformprojekte (mit Ausnahme von **Xamarin.FormsBook.Platform.WinRT**) verfügen über Verweise auf **Xamarin.FormsBook.Platform**. Die drei Windows-Projekte haben einen Verweis auf **Xamarin.FormsBook.Platform.WinRT**.

Alle Projekte enthalten einen statischen `Toolkit.Init` Methode, um sicherzustellen, dass die Bibliothek geladen wird, wenn sie nicht direkt von einem Projekt in einer Projektmappe der Xamarin.Forms-Anwendung verwiesen wird.

Die **Xamarin.FormsBook.Platform** -Projekt enthält das neue [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) Schnittstelle. Alle Methoden haben nun die Namen mit `Async` Suffixen und Rückgabe `Task` Objekte.

Die **Xamarin.FormsBook.Platform.WinRT** -Projekt enthält die [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) -Klasse für die Windows-Runtime.

Die **Xamarin.FormsBook.Platform.iOS** -Projekt enthält die [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) -Klasse für iOS. Diese Methoden müssen nun asynchron sein. Einige der Methoden verwenden, der asynchronen Versionen der Methoden, die in definierten `StreamWriter` und `StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String)) und [ `ReadToEndAsync` ](xref:System.IO.StreamReader.ReadToEndAsync). Andere konvertieren ein Ergebnis, um eine `Task` -Objekt unter Verwendung der [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*) Methode.

Die **Xamarin.FormsBook.Platform.Android** -Projekt enthält eine ähnliche [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) -Klasse für Android.

Die **Xamarin.FormsBook.Platform** Projekt enthält außerdem eine [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) -Klasse, die die Verwendung von vereinfacht die `DependencyService` Objekt.

Um diese Bibliotheken verwenden zu können, muss eine Lösung zur enthalten alle Projekte in der **Xamarin.FormsBook.Platform** Lösung, und jedes der Anwendungsprojekte müssen einen Verweis auf die entsprechende Bibliothek in  **Xamarin.FormsBook.Platform**.

Die [ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) Lösung veranschaulicht, wie die **Xamarin.FormsBook.Platform** Bibliotheken. Jedes der Projekte hat einen Aufruf von `Toolkit.Init`. Die Anwendung nutzt die asynchrone Datei e/a-Funktionen.

### <a name="keeping-it-in-the-background"></a>Da diese im Hintergrund

Methoden in Bibliotheken, die mehrere asynchrone Methoden aufrufen &mdash; wie z. B. die `WriteFileAsync` und `ReadFileASync` Methoden in der Windows-Runtime [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) Klasse &mdash; kann hergestellt werden eine effizientere mithilfe der [ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) Methode, um zu vermeiden, wechseln zurück an den UI-Thread.

### <a name="dont-block-the-ui-thread"></a>Blockieren Sie nicht den UI-Thread.

Manchmal ist es verlockend, vermeiden Sie die Verwendung von `ContinueWith` oder `await` mithilfe der [ `Result` ](xref:System.Threading.Tasks.Task`1.Result) Eigenschaft für die Methoden. Dies sollte vermieden werden, für den UI-Thread blockieren oder sogar hängen von der Anwendung kann.

## <a name="your-own-awaitable-methods"></a>Eigene awaitable-Methoden

Sie können Code asynchron ausführen, durch Übergabe an eines der [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action)) Methoden. Rufen Sie `Task.Run` innerhalb einer Async-Methode, die einige der Aufwand behandelt.

Die verschiedenen `Task.Run` Muster werden weiter unten erläutert.

### <a name="the-basic-mandelbrot-set"></a>Die grundlegende Mandelbrot-Menge

Zum Zeichnen der Mandelbrot-Menge in Echtzeit die [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek verfügt über eine [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) ähnliche Struktur wie in der `System.Numerics` Namespace.

Die [ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) Beispiel verfügt über eine `CalculateMandeblotAsync` -Methode in der CodeBehind-Datei, die die grundlegende Schwarz-Weiß Mandelbrot-Menge berechnet und verwendet [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)auf eine Bitmap ausgedrückt.

### <a name="marking-progress"></a>Markieren von Status

Um den Fortschritt von einer asynchronen Methode instanziieren Sie ein [ `Progress<T>` ](xref:System.Progress`1) Klasse und definieren Sie die asynchrone Methode, um ein Argument des Typs [ `IProgress<T>` ](xref:System.IProgress`1). Dies wird veranschaulicht, der [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) Beispiel.

### <a name="cancelling-the-job"></a>Den Auftrag wird abgebrochen.

Sie können auch eine asynchrone Methode zum abbrechbar ist schreiben. Beginnen Sie mit einer Klasse namens [ `CancellationTokenSource` ](xref:System.Threading.CancellationTokenSource). Die [ `Token` ](xref:System.Threading.CancellationTokenSource.Token) Eigenschaft ist ein Wert vom Typ [ `CancellationToken` ](xref:System.Threading.CancellationToken). Dies wird an die asynchrone Funktion übergeben. Ein Programm ruft die [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel) -Methode der `CancellationTokenSource` (in der Regel als Reaktion auf eine Aktion vom Benutzer) zum Abbrechen der asynchronen Funktion.

Die asynchrone Methode kann in regelmäßigen Abständen überprüfen. die [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested) Eigenschaft `CancellationToken` und beenden, wenn die Eigenschaft `true`, oder rufen Sie einfach die [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) Methode im der Fall, dass der Methode endet mit einem [ `OperationCancelledException` ](xref:System.OperationCanceledException).

Die [ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) Beispiel veranschaulicht die Verwendung einer Funktion abgebrochen werden können.

### <a name="an-mvvm-mandelbrot"></a>Eine MVVM-Mandelbrot

Die [ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) Beispiel verfügt über eine umfangreichere Benutzeroberfläche, und sie basiert größtenteils auf eine [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) und [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)Klassen:

[![Dreifacher Screenshot der Mandelbrot-X-F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>An das web

Die [ `WebRequest` ](xref:System.Net.WebRequest) Klasse zur Verwendung in einigen Beispielen verwendet ein altmodisches asynchrones Protokoll als APM oder Asynchronous Programming Model. Sie können eine solche Klasse konvertieren, für das moderne mithilfe einer der TAP-Protokoll der `FromAsync` Methoden in der [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1) Klasse. Die [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) veranschaulicht dies.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 20 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Kapitel 20-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Working with Files (Arbeiten mit Dateien)](~/xamarin-forms/app-fundamentals/files.md)
