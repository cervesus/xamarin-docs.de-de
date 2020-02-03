---
title: 'Teil 5: Praktische Strategien für die Codefreigabe'
description: In diesem Dokument wird erläutert, praktische Strategien für Szenarien wie z. B. Datenbanken, den Zugriff auf Dateien, Netzwerkvorgänge und asynchronem Code für die Codefreigabe.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 0e37e138607fb0e00fbdc463ac7c53facf81395d
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723630"
---
# <a name="part-5---practical-code-sharing-strategies"></a>Teil 5: Praktische Strategien für die Codefreigabe

Dieser Abschnitt enthält Beispiele zum Freigeben von Code für häufige Anwendungsszenarien.

## <a name="data-layer"></a>Datenschicht

Die Datenschicht besteht aus einem Speicher-Engine und Methoden zum Lesen und Schreiben von Informationen. Für die Leistung, Flexibilität und plattformübergreifende Kompatibilität der SQLite wird die Datenbank-Engine für plattformübergreifende Xamarin-Anwendungen empfohlen.
Ausführung auf einer Vielzahl von Plattformen einschließlich Windows, Android, iOS und Mac.

### <a name="sqlite"></a>SQLite

SQLite ist eine Open-Source-Datenbank-Implementierung. Die Quelle und die Dokumentation finden Sie unter [sqlite.org](https://www.sqlite.org/). SQLite-Unterstützung ist auf jeder mobilen Plattform verfügbar:

- **IOS** – in das Betriebssystem integriert.
- **Android** – integriert in das Betriebssystem seit Android 2,2 (API-Ebene 10).
- **Windows** – Weitere Informationen finden Sie unter [sqlite für universelle Windows-Plattform-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Auch bei der Datenbank-Engine, die auf allen Plattformen verfügbar ist unterscheiden die systemeigenen Methoden für den Datenbankzugriff. Sowohl iOS und Android bieten integrierte APIs zum Zugreifen auf SQLite verwendet werden kann, die von Xamarin.iOS oder Xamarin.Android, jedoch mit den systemeigenen SDK-Methoden bietet keine Möglichkeit zum Freigeben von Code (außer vielleicht die SQL-Abfragen selbst, vorausgesetzt, dass diese als Zeichenfolgen gespeichert sind) . Weitere Informationen zu systemeigenen Datenbankfunktionen finden Sie unter `CoreData` in der `SQLiteOpenHelper`-Klasse von IOS oder Android. Da diese Optionen nicht plattformübergreifend sind, gehen Sie über den Rahmen dieses Dokuments hinaus.

### <a name="adonet"></a>ADO.NET

Sowohl xamarin. IOS als auch xamarin. Android unterstützen `System.Data` und `Mono.Data.Sqlite` (Weitere Informationen finden Sie in der xamarin. IOS- [Dokumentation](~/ios/data-cloud/system.data.md) ).
Mit diesen Namespaces können Sie, ADO.NET-Code zu schreiben, die auf beiden Plattformen funktioniert. Bearbeiten Sie die Verweise des Projekts so, dass Sie `System.Data.dll` und `Mono.Data.Sqlite.dll` enthalten, und fügen Sie diese using-Anweisungen zum Code hinzu:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Der folgende Code funktioniert:

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

Real-World-Implementierungen von ADO.NET werden offensichtlich aufgeteilt werden über verschiedene Methoden und Klassen (in diesem Beispiel ist nur zu Demonstrationszwecken).

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET-Cross-Platform ORM

Versucht, ein ORM (oder Object-Relational Mapper) zur Vereinfachung der Speicherung von Daten in Klassen modelliert. Anstatt manuell Schreiben von SQL-Abfragen, Tabellen erstellen oder wählen Sie INSERT- und DELETE-Daten, die manuell aus extrahierten Klasse Felder und Eigenschaften, die ein ORM erhöht den Code, der für Sie ausführt. Verwenden Reflektion, um die Struktur der Klassen zu untersuchen, kann ein ORM automatisch erstellen, Tabellen und Spalten, die eine Klasse übereinstimmen und Abfragen zum Lesen und schreiben die Daten generieren. Dadurch können Anwendungscode, um einfach senden und Abrufen von Objektinstanzen an den-ORM, der die SQL-Vorgänge intern abnimmt.

SQLite-NET-fungiert als einer einfachen ORM, mit denen Sie zum Speichern und Abrufen von Klassen in SQLite. Es Blendet die Komplexität der plattformübergreifenden Zugriff auf SQLite mit einer Kombination von Compiler-Direktiven und andere Tricks.

SQLite-NET-Funktionen:

- Tabellen werden durch Hinzufügen von Attributen zu ViewModel-Klassen definiert.
- Eine Daten Bank Instanz wird durch eine Unterklasse von `SQLiteConnection` dargestellt, der Hauptklasse in der SQLite-NET-Bibliothek.
- Daten können abgefragt und gelöschte Objekte mit eingefügt werden. SQL-Anweisungen müssen (obwohl Sie SQL-Anweisungen schreiben können, falls erforderlich).
- Grundlegende Linq-Abfragen können auf die vom SQLite-NET-Sammlungen ausgeführt werden.

Der Quellcode und die Dokumentation für SQLite-net sind unter [SQLite-NET auf GitHub](https://github.com/praeclarum/sqlite-net) verfügbar und wurden in beiden Fallstudien implementiert. Im folgenden finden Sie ein einfaches Beispiel für SQLite-NET-Code (aus der *Tasky pro* -Fallstudie).

Zuerst verwendet die `TodoItem`-Klasse Attribute zum Definieren eines Felds als Primärschlüssel der Datenbank:

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

Dadurch kann eine `TodoItem` Tabelle mit der folgenden Codezeile (und ohne SQL-Anweisungen) auf einer `SQLiteConnection` Instanz erstellt werden:

```csharp
CreateTable<TodoItem> ();
```

Die Daten in der Tabelle können auch mit anderen Methoden auf dem `SQLiteConnection` manipuliert werden (auch ohne die Verwendung von SQL-Anweisungen):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Finden Sie vollständige Beispiele für die Fallstudie-Quellcode.

## <a name="file-access"></a>Dateizugriff

Dateizugriff ist sicher, dass ein wichtiger Bestandteil jeder Anwendung sein. Allgemeine Beispiele für Dateien, die Teil einer Anwendung gehören möglicherweise:

- SQLite-Datenbankdateien.
- Von Benutzern generierte Daten (Text, Bilder, Audio-, Video).
- Heruntergeladenen Daten für die Zwischenspeicherung (Bilder, html oder PDF-Dateien).

### <a name="systemio-direct-access"></a>System.IO-Direct-Zugriff

Sowohl xamarin. IOS als auch xamarin. Android ermöglichen den Dateisystem Zugriff mithilfe von Klassen im `System.IO`-Namespace.

Jede Plattform besitzt Zugriff auf unterschiedliche Einschränkungen, die berücksichtigt werden müssen:

- iOS-Anwendungen, die in einer Sandbox mit sehr eingeschränkten Dateisystemzugriff ausgeführt werden. Weitere Apple schreibt vor, wie Sie das Dateisystem verwenden sollten, durch Angeben von bestimmten Standorten, die gesichert werden (und andere, die nicht). Weitere Informationen finden Sie im Handbuch [Arbeiten mit dem Datei System in xamarin. IOS](~/ios/app-fundamentals/file-system.md) .
- Android schränkt auch den Zugriff auf bestimmte Verzeichnisse, die im Zusammenhang mit der Anwendung, aber sie unterstützt auch externe Medien (z. b. SD-Karten) und den Zugriff auf freigegebene Daten.
- Windows Phone 8 (Silverlight) lässt keinen direkten Dateizugriff zu – Dateien können nur mithilfe `IsolatedStorage`manipuliert werden.
- Windows 8.1 WinRT-und Windows 10-UWP-Projekte bieten nur asynchrone Datei Vorgänge über `Windows.Storage`-APIs, die sich von den anderen Plattformen unterscheiden.

#### <a name="example-for-ios-and-android"></a>Beispiel für iOS und Android

Ein sehr einfaches Beispiel, das schreibt und eine Textdatei ist unten dargestellt.
Wenn Sie `Environment.GetFolderPath` verwenden, kann derselbe Code unter IOS und Android ausgeführt werden, die jeweils ein gültiges Verzeichnis basierend auf Ihren Dateisystem Konventionen zurückgeben.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

Weitere Informationen zu IOS-spezifischen File System-Funktionen finden Sie unter xamarin. IOS [Arbeiten mit dem Datei System](~/ios/app-fundamentals/file-system.md) Dokument. Cross-Platform-Datei schreiben, denken Sie daran, dass einige Dateisysteme Groß-/Kleinschreibung beachtet und anderen Verzeichnistrennzeichen. Es empfiehlt sich, bei der Erstellung von Datei-oder Verzeichnis Pfaden immer die gleiche Groß-/Kleinschreibung für Dateinamen und die `Path.Combine()`-Methode zu verwenden.

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage für Windows 8 und Windows 10

Die *Mobile Apps erstellen mit xamarin. Forms* [Book](https://developer.xamarin.com/r/xamarin-forms/book/)
[Kapitel 20. Async und Datei-e/](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) a enthalten [Beispiele für Windows 8.1 und Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Mithilfe eines [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) es möglich, Dateien auf diesen Plattformen mithilfe der unterstützten APIs zu lesen und zu verwenden:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Weitere Informationen finden Sie im [Buchkapitel](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) .

<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Isolierter Speicher auf Windows Phone 7 und 8 (Silverlight)

Isolierte Speicherung ist eine allgemeine API für das Speichern und Laden von Dateien für alle iOS-, Android- und älteren Windows Phone-Plattformen.

Es ist der standardmäßige Mechanismus für den Zugriff auf Dateien im Windows Phone (Silverlight), die in Xamarin.iOS und Xamarin.Android zu gemeinsamen Dateizugriff Code geschrieben werden implementiert wurde. Auf die `System.IO.IsolatedStorage` Klasse kann auf allen drei Plattformen in einem frei [gegebenen Projekt](~/cross-platform/app-fundamentals/shared-projects.md)verwiesen werden.

Weitere Informationen Windows Phone finden Sie unter [Übersicht über den isolierten Speicher](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) .

Die APIs für den isolierten Speicher sind in [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)nicht verfügbar. Eine Alternative für die PCL ist das [pclstorage-nuget](https://pclstorage.codeplex.com/) .

### <a name="cross-platform-file-access-in-pcls"></a>Plattformübergreifende Dateizugriff in PCLs

Außerdem gibt es eine PCL-kompatible nuget-– [pclstorage](https://www.nuget.org/packages/PCLStorage/) –, die plattformübergreifenden Dateizugriff für xamarin-unterstützte Plattformen und die neuesten Windows-APIs bietet.

## <a name="network-operations"></a>Netzwerkvorgänge

Die meisten mobile Anwendungen müssen die Netzwerkkomponente, z.B.:

- Herunterladen von Abbildern, Video- und Audiostreams (z. b. Miniaturansichten, Fotos, Musik).
- Herunterladen von Dokumenten (z. b. HTML, PDF-DATEI).
- Hochladen von Benutzerdaten (z. B. Fotos oder Text).
- Zugriff auf Webdienste oder 3rd Party APIs (einschließlich SOAP, XML oder JSON).

Der .NET Framework bietet verschiedene Klassen für den Zugriff auf Netzwerkressourcen: `HttpClient`, `WebClient`und `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

Die `HttpClient`-Klasse im `System.Net.Http`-Namespace ist in xamarin. IOS, xamarin. Android und den meisten Windows-Plattformen verfügbar. Es gibt eine [Microsoft HTTP-Client Bibliothek](https://www.nuget.org/packages/Microsoft.Net.Http/) , mit der Sie diese API in Portable Klassenbibliotheken (und Windows Phone 8 Silverlight) einbringen können.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

Die `WebClient`-Klasse stellt eine einfache API zum Abrufen von Remote Daten von Remote Servern bereit.

Universelle Windows-Plattform Vorgänge *müssen* Async sein, auch wenn xamarin. IOS und xamarin. Android synchrone Vorgänge unterstützen (was für Hintergrundthreads möglich ist).

Der Code für eine einfache asynchrone `WebClient` Operation lautet wie folgt:

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

`HttpWebRequest` bietet mehr Anpassungen als `WebClient` und erfordert daher mehr Code für die Verwendung von.

Der Code für einen einfachen synchronen `HttpWebRequest` Vorgang lautet wie folgt:

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

In unserer [Webdienst Dokumentation](~/cross-platform/data-cloud/web-services/index.md)finden Sie ein Beispiel.

 <a name="Reachability" />

### <a name="reachability"></a>Erreichbarkeit

Mobile Geräte, die eine Vielzahl von netzwerkbedingungen schnell Wi-Fi-oder 4G-Verbindungen an eine schlechte Empfangsbereichen und langsamen Verbindungen von EDGE-Daten ausgeführt werden. Aus diesem Grund ist es empfehlenswert, zu ermitteln, ob das Netzwerk verfügbar ist, und wenn Ja, welche Art von Netzwerk verfügbar ist, bevor zu Remoteservern eine Verbindung mit.

Aktionen, die in diesen Situationen eine mobile app benötigt möglicherweise gehören:

- Wenn das Netzwerk nicht verfügbar ist, der Benutzer ist. Wenn sie ihn (z. b. manuell deaktiviert haben Flugzeugmodus, oder deaktivieren Wi-Fi) und dann das Problem zu beheben.
- Wenn die Verbindung 3 G beträgt, Anwendungen anderes Verhalten aufweisen (z. B. Apple lässt keine apps mehr als 20Mb über 3 G heruntergeladen werden). Anwendungen können diese Informationen verwenden, um warnen den Benutzer über eine übermäßige Download ein Timeout beim Abrufen von großer Dateien.
- Auch wenn das Netzwerk verfügbar ist, ist es empfiehlt sich, die Konnektivität mit dem Zielserver zu überprüfen, bevor andere Anforderungen initiiert. Dies zu verhindern, dass der app Netzwerkvorgänge zeitüberschreitungen wiederholt und können auch eine informativere Fehlermeldung, die dem Benutzer angezeigt werden.

## <a name="webservices"></a>WebServices

Weitere Informationen finden Sie in unserer Dokumentation zum [Arbeiten mit Webdiensten](~/cross-platform/data-cloud/web-services/index.md), die den Zugriff auf Rest-, SOAP-und WCF-Endpunkte mit xamarin. IOS behandelt. Es ist möglich, webdienstanforderungen Hand-selbst zu erstellen und Analysieren von Antworten, jedoch stehen Ihnen Bibliotheken zur Verfügung, um dies viel einfacher, einschließlich Azure, RestSharp und ServiceStack. Selbst grundlegende WCF-Dienstvorgänge können in Xamarin-apps zugegriffen werden.

### <a name="azure"></a>Azure

Microsoft Azure ist eine Cloudplattform, die eine Vielzahl von Diensten für mobile apps, einschließlich der datenspeicherung und -Synchronisierung und Pushbenachrichtigungen bereitstellt.

Besuchen Sie [Azure.Microsoft.com](https://azure.microsoft.com/) , um es kostenlos zu testen.

### <a name="restsharp"></a>RestSharp

RestSharp ist eine .NET-Bibliothek, die in mobilen Anwendungen mit einen REST-Client bereitstellen, der Zugriff auf Webdienste vereinfacht einbezogen werden kann. Sie können, indem Sie eine einfache API zum Anfordern von Daten und Analyse der REST-Antwort bereitgestellt. RestSharp kann nützlich sein.

Die [restsharp-Website](http://restsharp.org/) enthält eine [Dokumentation](https://github.com/restsharp/RestSharp/wiki) zur Implementierung eines Rest-Clients mithilfe von restsharp.
Restsharp bietet xamarin. IOS-und xamarin. Android-Beispiele auf [GitHub](https://github.com/restsharp/RestSharp/).

Es gibt auch einen xamarin. IOS-Code Ausschnitt in unserer [Webdienst Dokumentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />

### <a name="servicestack"></a>ServiceStack

Im Gegensatz zu RestSharp ist die ServiceStack sowohl eine serverseitige Lösung zum Hosten von einem Webdienst als auch für eine Clientbibliothek, die in mobilen Anwendungen den Zugriff auf diese Dienste implementiert werden kann.

Die [servicestack-Website](http://servicestack.net/) erläutert den Zweck des Projekts und Links zu Dokument-und Codebeispielen. Die Beispiele umfassen eine vollständige Implementierung von serverseitigen Webdienst als auch für verschiedene Client-Side-Anwendungen, die darauf zugreifen können.

### <a name="wcf"></a>WCF

Xamarin-Tools können Sie einige Windows Communication Foundation (WCF)-Dienste nutzen. Im Allgemeinen unterstützt Xamarin jedoch die gleiche Client-Side Teilmenge von WCF, die in der Silverlight-Laufzeit enthalten ist. Dies umfasst die gängigsten Codierungs-und Protokollimplementierungen von WCF: Text codierte SOAP-Nachrichten über das HTTP-Transportprotokoll mithilfe der `BasicHttpBinding`.

Aufgrund der Größe und Komplexität des WCF-Frameworks es gibt möglicherweise aktuellen und zukünftigen dienstimplementierungen, die außerhalb des unterstützten Bereichs von Xamarin Client-Teilmenge Domäne liegen werden. Unterstützung der WCF erfordert außerdem die Verwendung von Tools, die nur verfügbar, in einer Windows-Umgebung in den Proxy zu generieren.

 <a name="Threading" />

## <a name="threading"></a>Threading

Reaktionsfähigkeit der Anwendung ist wichtig für mobile Anwendungen – erwarten, dass Benutzer Anwendungen laden, und führen Sie schnell. Eine "fixierte"-Bildschirm, den beendet wird, die Benutzereingaben akzeptiert angezeigt werden, um anzugeben, dass die Anwendung ist abgestürzt, daher ist es wichtig, nicht im UI-Thread mit langer blockierenden Aufrufe wie z. B. netzwerkanforderungen oder langsame lokale Vorgänge (z. B. Entpacken eine Datei) zu verknüpfen. Insbesondere der Startprozess sollten keine lang andauernden Aufgaben – bricht alle mobile Plattformen eine app, die beim Laden zu lange dauert.

Dies bedeutet, dass Ihre Benutzeroberfläche eine Statusanzeige oder andernfalls 'verwendet' Benutzeroberfläche, die schnell anzeigen und asynchronen Aufgaben zum Durchführen von Hintergrundoperationen implementieren soll. Ausführen von Hintergrundaufgaben erfordert die Verwendung von Threads, was bedeutet den Hintergrund-Aufgaben-Anforderungen auf eine Möglichkeit für die Kommunikation mit den Hauptthread zum Anzeigen des Status oder wann diese abgeschlossen haben.

 <a name="Parallel_Task_Library" />

### <a name="parallel-task-library"></a>Bibliothek für parallele Tasks

Aufgaben mit der Task Parallel Library können asynchron ausgeführt und ihre aufrufenden Thread, sodass sie sehr nützlich für das Auslösen von Vorgängen mit langen Laufzeiten ohne Blockierung der Benutzeroberfläche zurück.

Ein Vorgang für die einfache parallele Aufgabe könnte folgendermaßen aussehen:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Der Schlüssel ist `TaskScheduler.FromCurrentSynchronizationContext()`, der "SynchronizationContext. Current" des Threads, der die-Methode aufruft (hier der Haupt Thread, der `MainThreadMethod`ausgeführt wird) als Möglichkeit zum Mars Hallen von Aufrufen an diesen Thread verwendet. Dies bedeutet, dass, wenn die-Methode im UI-Thread aufgerufen wird, der `ContinueWith` Vorgang wieder im UI-Thread ausgeführt wird.

Wenn der Code Aufgaben von anderen Threads gestartet wird, verwenden Sie Folgendes Muster, um einen Verweis auf den UI-Thread zu erstellen, und die Aufgabe kann immer noch einen Rückruf aus:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />

### <a name="invoking-on-the-ui-thread"></a>Klicken Sie auf der UI-Thread aufrufen

Bei Code, der Task Parallel Library verwendet wird nicht, gelten für jede Plattform eine eigene Syntax für Marshalling-Vorgänge an den UI-Thread:

- **IOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** – `owner.RunOnUiThread(action)`
- **Xamarin. Forms** – `Device.BeginInvokeOnMainThread(action)`
- **Windows:** `Deployment.Current.Dispatcher.BeginInvoke(action)`

IOS- und Android-Syntax erfordert eine 'Context'-Klasse zur Verfügung stehen, was bedeutet, dass der Code muss alle Methoden dieses Objekts übergeben, die einen Rückruf für den UI-Thread erfordern.

Um UI-Thread Aufrufe in frei gegebenem Code auszuführen, befolgen Sie das [idispatchonuithread-Beispiel](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (Courtesy of [@follesoe](https://twitter.com/follesoe)). Deklarieren Sie an einer `IDispatchOnUIThread`-Schnittstelle im freigegebenen Code und Programmieren Sie, und implementieren Sie dann die plattformspezifischen Klassen, wie hier gezeigt:

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

Xamarin. Forms-Entwickler sollten [`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) in Common Code (freigegebene Projekte oder PCL) verwenden.

 <a name="Platform_and_Device_Capabilities_and_Degradation" />

## <a name="platform-and-device-capabilities-and-degradation"></a>Plattform und Gerätefunktionen und Beeinträchtigung

Weitere spezifische Beispiele für den Umgang mit unterschiedlichen Funktionen in der Dokumentation zu Plattformfunktionen erhalten. Er behandelt die unterschiedliche Funktionen und wie Sie Ihre Anforderungen ordnungsgemäß herabstufen eine Anwendung eine gute benutzererfahrung, bereitstellen, auch wenn die app auf ihr volles Potenzial betrieben werden, kann nicht erkannt.
