---
title: "Teil 5 – praktische Strategien für die Codefreigabe"
ms.topic: article
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 4c102181a1d2c345e0376f53f1f343cbc7be5551
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>Teil 5 – praktische Strategien für die Codefreigabe

Dieser Abschnitt enthält Beispiele für Informationen zum Freigeben von Code für allgemeine Anwendungsszenarien.



## <a name="data-layer"></a>Datenschicht

Die Datenschicht besteht aus einem Speichermodul und Methoden zum Lesen und Schreiben von Informationen. Für die Leistung, Flexibilität und plattformübergreifende Kompatibilität der SQLite wird Datenbankmodul für Xamarin plattformübergreifende Anwendungen empfohlen.
Es wird in einer Vielzahl von Plattformen, einschließlich Windows, Android, iOS und Mac ausgeführt


### <a name="sqlite"></a>SQLite

SQLite ist eine Open Source-Datenbank-Implementierung. Die Quelle und die Dokumentation finden Sie unter [SQLite.org](http://www.sqlite.org/). SQLite-Unterstützung ist auf den verschiedenen mobilen Plattformen verfügbar:

-  **iOS** – für das Betriebssystem integriert.
- **Android** – seit Android 2.2 (API-Ebene 10) für das Betriebssystem integriert.
- **Windows** – finden Sie unter der [SQLite für universelle Windows-Plattform-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


Sogar mit dem Datenbankmodul, die auf allen Plattformen verfügbar werden die systemeigenen Methoden für den Datenbankzugriff anderen. Sowohl IOS- und Android bieten integrierte APIs den Zugriff von Xamarin.iOS oder Xamarin.Android auf SQLite, die verwendet werden können, jedoch mit den systemeigenen SDK Methoden bietet keine Möglichkeit zum Freigeben von Code (außer vielleicht die SQL-Abfragen selbst, vorausgesetzt, dass sie als Zeichenfolgen gespeichert) . Weitere Informationen zu systemeigenen Funktionalität Datenbanksuche für `CoreData` in iOS oder Android `SQLiteOpenHelper` Klasse, da diese Optionen nicht plattformübergreifend sind, sind sie würde den Rahmen dieses Dokuments sprengen.



### <a name="adonet"></a>ADO.NET

Unterstützung von Xamarin.iOS und Xamarin.Android `System.Data` und `Mono.Data.Sqlite` (finden Sie unter der Xamarin.iOS [Dokumentation](~/ios/data-cloud/system.data.md) für Weitere Informationen).
Mithilfe dieser Namespaces können Sie ADO.NET-Code zu schreiben, die für beide Plattformen verwendet werden kann. Bearbeiten Sie den Verweisen des Projekts enthalten `System.Data.dll` und `Mono.Data.Sqlite.dll` und fügen Sie diese mithilfe von Anweisungen für Ihren Code hinzu:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Der folgende Beispielcode funktioniert:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Praktische Implementierungen von ADO.NET werden offensichtlich über verschiedene Methoden und Klassen, die (in diesem Beispiel wird nur zu Demonstrationszwecken) aufgeteilt.



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – Cross-Platform ORM

Ein ORM (oder das Objekt objektrelationale Zuordnungen) versucht, vereinfachen Sie die Speicherung von Daten in Klassen modelliert. Anstatt manuell Schreiben von SQL-Abfragen, Tabellen zu erstellen, oder wählen Sie INSERT- und DELETE-Daten, die manuell aus extrahiert werden-Klasse Felder und Eigenschaften, die ein ORM Sicherheitsebene des Codes, die für Sie. Verwenden Reflektion, um die Struktur Ihrer Klassen zu untersuchen, kann ein ORM automatisch erstellen, Tabellen und Spalten, die eine Klasse übereinstimmen, und Generieren von Abfragen, um die Daten lesen und schreiben. Dadurch wird ein Anwendungscode einfach gesendet und Objektinstanzen, die alle SQL-Vorgänge hinter den Kulissen übernimmt ORM abgerufen werden.

SQLite-NET fungiert als eine einfache Verwendung von ORM, mit denen Sie zum Speichern und Abrufen der Klassen in SQLite. Es verbirgt die Komplexität der cross-Platform SQLite Zugriff mit einer Kombination von Compiler-Direktiven und andere Tricks.

SQLite-NET-Funktionen:

-  Tabellen werden durch Hinzufügen von Attributen zu Modellklassen definiert.
-  Eine Datenbankinstanz wird dargestellt, indem Sie eine Unterklasse von `SQLiteConnection` , die Hauptklasse in der SQLite-Netzwerkbibliothek.
-  Daten können abgefragt und gelöschte Objekte mit eingefügt werden. Keine SQL-Anweisungen sind erforderlich, (obwohl Sie SQL-Anweisungen schreiben können, falls erforderlich).
-  Grundlegende Linq-Abfragen können für die SQLite-NET zurückgegebenen Sammlungen ausgeführt werden.


Der Quellcode und SQLite-NET-Dokumentation finden Sie unter [SQLite-Net auf Github](https://github.com/praeclarum/sqlite-net) und wurde in beiden Fallstudien implementiert. Ein einfaches Beispiel SQLite-NET-Code (aus der *Tasky Pro* Fallstudie) wird unten gezeigt.

Zunächst wird die `TodoItem` Klasse mithilfe von Attributen um ein Felds zur werden ein Datenbank-Primärschlüssel zu definieren:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Dies ermöglicht eine `TodoItem` Tabelle mit den folgenden Code (und keine SQL-Anweisungen) erstellt werden, auf eine `SQLiteConnection` Instanz:

```csharp
CreateTable<TodoItem> ();
```

Daten in der Tabelle können auch mit anderen Methoden bearbeitet werden, auf die `SQLiteConnection` (erneut, ohne dass die SQL-Anweisungen):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Finden Sie Ausführliche Beispiele für die Fallstudie-Quellcode.



## <a name="file-access"></a>Dateizugriff

Der Dateizugriff ist sicher, dass ein wesentlicher Bestandteil des jeder Anwendung sein. Allgemeine Beispiele für Dateien, die Teil einer Anwendung gehören möglicherweise:

-  SQLite Datenbankdateien.
-  Vom Benutzer generierte Daten (Text, Bilder, Audio-, Video).
-  Heruntergeladenen Daten für das caching (Bilder, html oder PDF-Dateien).




### <a name="systemio-direct-access"></a>System.IO-Direct-Zugriff

Xamarin.iOS und Xamarin.Android zulassen Dateisystemzugriff mithilfe von Klassen in der `System.IO` Namespace.

Jede Plattform weist unterschiedliche Zugriffsberechtigungen Einschränkungen auf, die berücksichtigt werden müssen:

-  iOS-Anwendungen, die in einer Sandbox mit sehr eingeschränkten Dateisystemzugriff ausgeführt werden. Weitere Apple bestimmt, wie Sie die im Dateisystem verwenden sollten, indem bestimmte Speicherorte, die gesichert werden (und andere nicht). Finden Sie in der [arbeiten mit dem Dateisystem in Xamarin.iOS](~/ios/app-fundamentals/file-system.md) Weitere-Handbuch.
-  Android beschränkt außerdem den Zugriff auf bestimmte Verzeichnisse, die im Zusammenhang mit der Anwendung, aber sie unterstützt auch externe Medien eingesetzt werden (z. b. SD-Karten) und den Zugriff auf freigegebene Daten.
-  Windows Phone 8 (Silverlight) lassen sich nicht auf direkten Dateizugriff – Dateien können nur bearbeitet werden mit `IsolatedStorage`.
-  WinRT für Windows 8.1 und Windows 10-UWP-Projekte bieten nur asynchrone Dateivorgänge über `Windows.Storage` APIs, die von den anderen Plattformen unterscheiden.

#### <a name="example-for-ios-and-android"></a>Beispiel für iOS und Android

Nachfolgend finden Sie ein einfaches Beispiel, das schreibt und liest eine Textdatei.
Mithilfe von `Environment.GetFolderPath` den gleichen Code für die Ausführung auf IOS- und Android, die jeweils ein gültiges Verzeichnis basierend auf deren Filesystem-Konventionen zurückgeben können.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Finden Sie in der Xamarin.iOS [arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md) Dokument Weitere Informationen zu iOS-spezifische Filesystem-Funktionen. Wenn plattformübergreifende Datei Zugriffscode schreiben, denken Sie daran, dass einige Dateisysteme / Kleinschreibung beachtet Groß- und anderen Verzeichnistrennzeichen. Es wird empfohlen, immer die gleiche Groß-/Kleinschreibung bei Dateinamen verwenden und die `Path.Combine()` Methode, wenn die Datei oder das Verzeichnis Pfade zu erstellen.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage für Windows 8 und Windows 10

Die *Erstellen mobiler Apps mit Xamarin.Forms* [Buch](https://developer.xamarin.com/r/xamarin-forms/book/)
[Kapitel 20. Async und Datei-e/a-](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) enthält [Beispiele für Windows 8.1 und Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Mit einem [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) es ist möglich, lesen und Dateien auf diesen Plattformen, die mit den unterstützten APIs:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Finden Sie in der [Buchkapitel](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) Weitere Details.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Isolierter Speicher auf Windows Phone 7 und 8 (Silverlight)

Isolierte Speicherung ist eine allgemeine API zum Speichern und Laden von Dateien in allen iOS, Android und älteren Windows Phone-Plattformen.

Es ist der standardmäßige Mechanismus für den Zugriff auf Dateien im Windows Phone (Silverlight), die in Xamarin.iOS und Xamarin.Android gemeinsamer Code der Datei-Zugriff zu ermöglichen, zu schreibenden implementiert wurde. Die `System.IO.IsolatedStorage` Klasse verwiesen werden kann, und alle drei Plattformen in einer [freigegebenes Projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Finden Sie in der [isolierten Speicher (Übersicht) für Windows Phone](http://msdn.microsoft.com/en-us/library/windowsphone/develop/ff402541(v=vs.105).aspx) für Weitere Informationen.

Die APIs für den isolierten Speicher sind nicht verfügbar in [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md). Eine Alternative zur PCL ist die [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Plattformübergreifende Dateizugriff in PCLs

Es gibt auch eine PCL-kompatiblen Nuget – [PCLStorage](https://www.nuget.org/packages/PCLStorage/) –, plattformübergreifende Dateizugriff Einrichtungen für Xamarin-unterstützte Plattformen und die neuesten Windows-APIs.


## <a name="network-operations"></a>Netzwerkoperationen

Die meisten mobile Anwendungen müssen Netzwerkkomponente, beispielsweise:

-  Herunterladen der Bilder, video und audio (z. b. Miniaturansichten, Fotos, Musik).
-  Herunterladen von Dokumenten (z. b. HTML, PDF).
-  Hochladen von Benutzerdaten (z. B. Fotos oder Text).
-  Beim Zugriff auf Webdienste oder 3rd Party APIs (einschließlich SOAP, XML oder JSON).


.NET Framework stellt mehrere unterschiedliche Klassen für den Zugriff auf Netzwerkressourcen: `HttpClient`, `WebClient`, und `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

Die `HttpClient` -Klasse in der `System.Net.Http` Namespace in den meisten Windows-Plattformen, Xamarin.iOS und Xamarin.Android verfügbar ist. Es ist ein [Microsoft HTTP Client Library-NuGet-](https://www.nuget.org/packages/Microsoft.Net.Http/) , die verwendet werden kann, um diese API in portablen Klassenbibliotheken (und Windows Phone 8 Silverlight) einzufügen.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

Die `WebClient` -Klasse bietet eine einfache API zum Abrufen von Remotedaten von Remoteservern.

Universelle Windows-Platofrm Vorgänge *müssen* werden asynchrone, obwohl Xamarin.iOS und Xamarin.Android synchrone Vorgänge unterstützt (die in Hintergrundthreads ausgeführt werden können).

Der Code für eine einfache asychronen `WebClient` Vorgang ist:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` verfügt auch über `DownloadFileCompleted` und `DownloadFileAsync` zum Abrufen von Binärdaten.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` bietet weitere Anpassung als `WebClient` und erfordert daher mehr Code verwenden.

Der Code für eine einfache synchrone `HttpWebRequest` Vorgang ist:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Es ist ein Beispiel, in unserem [Web Services-Dokumentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Erreichbarkeit

Mobile Geräte, die unter einer Vielzahl von netzwerkbedingungen aus schnellen Wi-Fi oder 4G-Verbindungen an schlechter Empfang von Bereichen und langsame EDGE-datenverknüpfungen ausgeführt werden. Aus diesem Grund ist es empfiehlt sich zu ermitteln, ob das Netzwerk verfügbar ist, und wenn also, welche Art von Netzwerk verfügbar ist, bevor Sie versuchen, für die Verbindung zu Remoteservern.

In diesen Situationen könnte zu eine mobile app führen Aktionen zählen:

-  Wenn das Netzwerk nicht verfügbar ist, informieren Sie den Benutzer. Wenn sie manuell (z. b. deaktiviert haben Flugzeugmodus oder Deaktivieren von Wi-Fi) und dann das Problem zu beheben.
-  Wenn die Verbindung 3 G, Anwendungen möglicherweise ein anderes Verhalten (z. B. Apple lässt keine apps mehr als 20Mb in mehr als 3 G heruntergeladen werden). Anwendungen könnten diese Informationen verwenden, um den Benutzer eine übermäßige Download warnt Zeitpunkte, große Dateien abrufen.
-  Auch wenn das Netzwerk verfügbar ist, ist es empfiehlt sich, die Konnektivität mit dem Zielserver zu überprüfen, bevor andere Anforderungen initiiert. Dies wird wiederholt verhindern, dass die app Netzwerkvorgänge ein Timeout auftritt und ermöglichen außerdem eine ausführlichere Fehlermeldung an, die dem Benutzer angezeigt werden.


Besteht eine [Xamarin.iOS Beispiel](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) verfügbar (die basiert auf der Apple- [Erreichbarkeit Beispielcode](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) um netzwerkverfügbarkeit zu erkennen.


## <a name="webservices"></a>WebServices

Finden Sie in unserer Dokumentation auf [arbeiten mit Webdiensten](~/cross-platform/data-cloud/web-services/index.md), der kompletten Zugriff auf REST, SOAP- und WCF-Endpunkte, die mithilfe von Xamarin.iOS. Es ist möglich, webdienstanforderungen Hand Abmeldenachricht und analysieren Sie die Antworten, jedoch Bibliotheken, dies viel einfacher, einschließlich Azure, RestSharp und ServiceStack verfügbar sind. Auch grundlegende WCF-Dienstvorgänge können im Xamarin-apps zugegriffen werden.

### <a name="azure"></a>Azure

Microsoft Azure ist eine Cloudplattform, die eine Vielzahl von Diensten für mobile apps, einschließlich der datenspeicherung und Synchronisierung und Push-Benachrichtigungen bereitstellt.

Besuchen Sie [azure.microsoft.com](https://azure.microsoft.com/) kostenlos ausprobieren.

### <a name="restsharp"></a>RestSharp

RestSharp ist eine .NET-Bibliothek, die in mobilen Anwendungen einen REST-Client bereitstellen, der Zugriff auf Webdienste vereinfacht aufgenommen werden kann. Es wird durch die Bereitstellung einer einfachen API zum Anfordern von Daten und analysieren die REST-Antwort. RestSharp kann hilfreich sein.

Die [RestSharp Website](http://restsharp.org/) enthält [Dokumentation](https://github.com/restsharp/RestSharp/wiki) zum Implementieren eines REST-Client RestSharp verwenden.
RestSharp bietet Beispiele für die Xamarin.iOS und Xamarin.Android auf [Github](https://github.com/restsharp/RestSharp/).

Es gibt auch ein Codeausschnitt Xamarin.iOS in unserer [Web Services-Dokumentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

Im Gegensatz zu RestSharp ist die ServiceStack sowohl eine serverseitige Lösung zum Hosten eines Webdiensts sowie eine Clientbibliothek, die in mobilen Anwendungen Zugriff auf diese Dienste implementiert werden kann.

Die [ServiceStack Website](http://servicestack.net/) Dokument sowie Codebeispiele erklärt den Zweck des Projekts und Links. Die Beispiele enthalten eine vollständige serverseitige Implementierung von einem Webdienst sowie verschiedene clientseitige Anwendungen zur Verfügung, die darauf zugreifen können.

Besteht eine [Xamarin.iOS Beispiel](http://www.servicestack.net/monotouch/remote-info/) auf der Website ServiceStack und einen Codeausschnitt in unserer [Web Services-Dokumentation](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Xamarin-Tools können Sie einige Windows Communication Foundation (WCF)-Dienste zu nutzen. Im Allgemeinen unterstützt Xamarin dieselbe clientseitige Teilmenge von WCF, die mit der Silverlight-Laufzeit bereitgestellt wird. Dies schließt die am häufigsten verwendeten Codierungs- und Protokolldetails Implementierungen von WCF: Text-codierte SOAP-Nachrichten über den HTTP-transport-Protokoll der `BasicHttpBinding`.

Aufgrund der Größe und Komplexität des WCF-Frameworks möglicherweise aktueller und künftiger dienstimplementierungen, die außerhalb des Bereichs, unterstützt von Xamarin Client-Teilmenge Domäne fallen werden. WCF-Unterstützung erfordert außerdem die Verwendung von Tools nur in einer Windows-Umgebung für den Proxy zu generieren.

 <a name="Threading" />


## <a name="threading"></a>Threading

Reaktionsfähigkeit der Anwendung für mobile Anwendungen wichtig ist – Benutzer erwarten, dass Anwendungen zu laden und schnell ausführen können. Eine "fixierte" Bildschirm, den beendet wird, die Benutzereingaben akzeptieren, um anzugeben, dass die Anwendung abgestürzt ist, daher ist es wichtig, nicht im UI-Thread mit langer blockierende Aufrufe z. B. netzwerkanforderungen oder langsamen lokalen Vorgänge (z. B. Entzippen eine Datei) Verarbeitungsressourcen angezeigt wird. Insbesondere der Startprozess dürfen nicht für lang andauernden Aufgaben – lle mobile Plattformen werden kill eine app, die zu lange dauert.

Dies bedeutet, dass Ihre Benutzeroberfläche implementieren sollten, eine Statusanzeige oder andernfalls 'nutzbare' UI, der schnell angezeigt werden und asynchrone Aufgaben zum Ausführen von Hintergrundvorgängen im. Ausführen von Hintergrundaufgaben erfordert die Verwendung von Threads, d. h. den Hintergrund Aufgaben Anforderungen eine Methode für die Kommunikation mit den Hauptthread, um den Fortschritt, oder wenn durchgeführt.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Task Parallel Library

Aufgaben mit der Task Parallel Library können asynchron ausgeführt und deren aufrufenden Thread, wodurch das sehr nützlich, bei dem lang ausgeführter Vorgänge auslösen, ohne Blockierung die Benutzeroberfläche zurückgeben.

Eine einfache parallel Taskvorgang kann wie folgt aussehen:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Der Schlüssel ist `TaskScheduler.FromCurrentSynchronizationContext()` der wird wiederverwendet, die SynchronizationContext.Current des Threads, die beim Aufrufen der Methode (hier der Haupt-Thread, der ausgeführt wird `MainThreadMethod`) als eine Möglichkeit, zurück zu diesem Thread marshallen. Dies bedeutet, wenn die Methode für den UI-Thread aufgerufen wird, führt er die `ContinueWith` Vorgang wieder an den UI-Thread.

Wenn der Code Aufgaben von anderen Threads gestartet wird, verwenden Sie Folgendes Muster, um einen Verweis auf den UI-Thread zu erstellen, und der Task kann trotzdem aufgerufen:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Auf den UI-Thread aufrufen

Für Code, der Task Parallel Library verwendet wird nicht, hat jede Plattform eine eigene Syntax für Marshalling Vorgänge an den UI-Thread:

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
-  **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS- und Android-Syntax erfordert eine 'Context'-Klasse verfügbar sein, was bedeutet, dass der Code muss alle Methoden dieses Objekts übergeben, die einen Rückruf an den UI-Thread erforderlich ist.

Um UI-Thread-Aufrufe im freigegebenen Code vornehmen, führen Sie die [IDispatchOnUIThread Beispiel](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (gestellt von [ @follesoe ](http://jonas.follesoe.no/)). Deklarieren und Programmieren, um eine `IDispatchOnUIThread` Schnittstelle im freigegebenen Code, und klicken Sie dann die Clientplattform-spezifische Klassen implementieren, wie hier gezeigt:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Xamarin.Forms-Entwickler sollten verwenden [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) im gemeinsamen Code (freigegebenen Projekten oder PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Plattform und Gerätefunktionen und Beeinträchtigung.

Weitere werden spezifische Beispiele für den Umgang mit unterschiedlichen Funktionen in der Dokumentation Plattformfunktionen angegeben. Sie behandelt erkennen, unterschiedliche Funktionen und Leistungseinbußen eine Anwendung für eine gute benutzererfahrung zu gewährleisten, selbst wenn die app ausgeführt, um seine volles Potenzial werden kann.
