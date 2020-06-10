---
title: 'Teil 5: Praktische Strategien für die Codefreigabe'
description: In diesem Dokument werden praktische Code Freigabe Strategien für Szenarien wie Datenbanken, Dateizugriff, Netzwerk Vorgänge und asynchroner Code behandelt.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: fd0e48c8f954ba926c5e1b5dc3a1c9bf6aab8c54
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571193"
---
# <a name="part-5---practical-code-sharing-strategies"></a>Teil 5: Praktische Strategien für die Codefreigabe

Dieser Abschnitt enthält Beispiele für das Freigeben von Code für gängige Anwendungsszenarien.

## <a name="data-layer"></a>Datenschicht

Die Datenschicht besteht aus einer Speicher-Engine und Methoden zum Lesen und Schreiben von Informationen. Aus Leistungs-, Flexibilität-und plattformübergreifender Kompatibilität wird die SQLite-Datenbank-Engine für plattformübergreifende xamarin-Anwendungen empfohlen.
Es wird auf einer Vielzahl von Plattformen ausgeführt, einschließlich Windows, Android, IOS und Mac.

### <a name="sqlite"></a>SQLite

SQLite ist eine Open-Source-Datenbankimplementierung. Die Quelle und die Dokumentation finden Sie unter [sqlite.org](https://www.sqlite.org/). SQLite-Unterstützung ist auf jeder mobilen Plattform verfügbar:

- **IOS** – in das Betriebssystem integriert.
- **Android** – integriert in das Betriebssystem seit Android 2,2 (API-Ebene 10).
- **Windows** – Weitere Informationen finden Sie unter [sqlite für universelle Windows-Plattform-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Auch wenn die Datenbank-Engine auf allen Plattformen verfügbar ist, unterscheiden sich die systemeigenen Methoden für den Zugriff auf die Datenbank. Sowohl IOS als auch Android bieten integrierte APIs für den Zugriff auf SQLite, die von xamarin. IOS oder xamarin. Android verwendet werden könnten. die Verwendung der nativen SDK-Methoden bietet jedoch keine Möglichkeit, Code freizugeben (außer den SQL-Abfragen selbst, vorausgesetzt, dass Sie als Zeichen folgen gespeichert sind). Ausführliche Informationen zur nativen Datenbankfunktionalität finden `CoreData` Sie unter in der IOS-oder Android- `SQLiteOpenHelper` Klasse. da diese Optionen nicht plattformübergreifend sind, gehen Sie über den Rahmen dieses Dokuments hinaus.

### <a name="adonet"></a>ADO.NET

Sowohl xamarin. IOS als auch xamarin. Android unterstützen `System.Data` und `Mono.Data.Sqlite` (Weitere Informationen finden Sie in der xamarin. IOS- [Dokumentation](~/ios/data-cloud/system.data.md) ).
Mit diesen Namespaces können Sie ADO.NET-Code schreiben, der auf beiden Plattformen funktioniert. Bearbeiten Sie die Verweise des Projekts, um `System.Data.dll` und `Mono.Data.Sqlite.dll` hinzuzufügen, und fügen Sie diese using-Anweisungen zum Code hinzu:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Der folgende Beispielcode funktioniert dann:

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

Reale Implementierungen von ADO.net werden offensichtlich auf verschiedene Methoden und Klassen aufgeteilt (dieses Beispiel dient nur zu Demonstrationszwecken).

### <a name="sqlite-net--cross-platform-orm"></a>SQLite-net – plattformübergreifende ORM

Ein ORM (oder Objekt relationaler Mapper) versucht, die Speicherung von Daten zu vereinfachen, die in Klassen modelliert werden. Anstatt manuell SQL-Abfragen zu schreiben, mit denen Tabellen erstellt oder Daten, die manuell aus Klassen Feldern und-Eigenschaften extrahiert werden, hinzugefügt, eingefügt und gelöscht werden, fügt ein ORM eine Codeebene hinzu, die dies für Sie erledigt. Wenn Sie die Struktur der Klassen mithilfe von Reflektion untersuchen, kann ein ORM automatisch Tabellen und Spalten erstellen, die einer Klasse entsprechen, und Abfragen generieren, um die Daten zu lesen und zu schreiben. Dies ermöglicht Anwendungscode das einfache senden und Abrufen von Objektinstanzen an den ORM, der alle SQL-Vorgänge im Hintergrund übernimmt.

SQLite-NET fungiert als einfaches ORM, das es Ihnen ermöglicht, Ihre Klassen in SQLite zu speichern und abzurufen. Dadurch wird die Komplexität des plattformübergreifenden SQLite-Zugriffs durch eine Kombination aus Compilerdirektiven und anderen Tricks ausgeblendet.

Features von SQLite-net:

- Tabellen werden durch das Hinzufügen von Attributen zu Modellklassen definiert.
- Eine-Daten Bank Instanz wird durch eine Unterklasse von `SQLiteConnection` , die Hauptklasse in der SQLite-NET-Bibliothek, repräsentiert.
- Daten können mithilfe von-Objekten eingefügt, abgefragt und gelöscht werden. Es sind keine SQL-Anweisungen erforderlich (obwohl Sie ggf. SQL-Anweisungen schreiben können).
- Grundlegende LINQ-Abfragen können für die von SQLite-net zurückgegebenen Sammlungen durchgeführt werden.

Der Quellcode und die Dokumentation für SQLite-net sind unter [SQLite-NET auf GitHub](https://github.com/praeclarum/sqlite-net) verfügbar und wurden in beiden Fallstudien implementiert. Im folgenden finden Sie ein einfaches Beispiel für SQLite-NET-Code (aus der *Tasky pro* -Fallstudie).

Zuerst verwendet die- `TodoItem` Klasse Attribute zum Definieren eines Felds als Primärschlüssel für die Datenbank:

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

Dadurch kann eine `TodoItem` Tabelle mit der folgenden Codezeile (und ohne SQL-Anweisungen) auf einer-Instanz erstellt werden `SQLiteConnection` :

```csharp
CreateTable<TodoItem> ();
```

Die Daten in der Tabelle können auch mit anderen Methoden auf dem bearbeitet werden `SQLiteConnection` (auch ohne SQL-Anweisungen):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Ausführliche Beispiele finden Sie im Quellcode der Fallstudie.

## <a name="file-access"></a>Dateizugriff

Der Dateizugriff ist ein wichtiger Bestandteil einer beliebigen Anwendung. Gängige Beispiele für Dateien, die Teil einer Anwendung sein können, sind u. a.:

- SQLite-Datenbankdateien.
- Benutzergenerierte Daten (Text, Bilder, Sound, Video).
- Heruntergeladene Daten zum Zwischenspeichern (Bilder, HTML-oder PDF-Dateien).

### <a name="systemio-direct-access"></a>System.IO direkter Zugriff

Sowohl xamarin. IOS als auch xamarin. Android ermöglichen den Dateisystem Zugriff mithilfe von Klassen im- `System.IO` Namespace.

Jede Plattform hat unterschiedliche Zugriffsbeschränkungen, die berücksichtigt werden müssen:

- IOS-Anwendungen werden in einem Sandkasten mit sehr eingeschränktem Dateisystem Zugriff ausgeführt. Apple legt außerdem fest, wie Sie das Dateisystem verwenden sollten, indem Sie bestimmte Standorte angeben, die gesichert werden (und andere nicht). Weitere Informationen finden Sie im Handbuch [Arbeiten mit dem Datei System in xamarin. IOS](~/ios/app-fundamentals/file-system.md) .
- Android schränkt auch den Zugriff auf bestimmte Verzeichnisse ein, die mit der Anwendung verknüpft sind, unterstützt jedoch auch externe Medien (z. b. SD-Karten) und den Zugriff auf freigegebene Daten.
- Windows Phone 8 (Silverlight) lässt keinen direkten Dateizugriff zu – Dateien können nur mithilfe von manipuliert werden `IsolatedStorage` .
- Windows 8.1 WinRT-und Windows 10-UWP-Projekte bieten nur asynchrone Datei Vorgänge über `Windows.Storage` APIs, die sich von den anderen Plattformen unterscheiden.

#### <a name="example-for-ios-and-android"></a>Beispiel für IOS und Android

Im folgenden finden Sie ein einfaches Beispiel für das Schreiben und Lesen einer Textdatei.
`Environment.GetFolderPath`Die Verwendung von ermöglicht das Ausführen desselben Codes unter IOS und Android, die jeweils ein gültiges Verzeichnis basierend auf Ihren Dateisystem Konventionen zurückgeben.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

Weitere Informationen zu IOS-spezifischen File System-Funktionen finden Sie unter xamarin. IOS [Arbeiten mit dem Datei System](~/ios/app-fundamentals/file-system.md) Dokument. Beachten Sie beim Schreiben von Platt Form übergreifendem Datei Zugriffs Code, dass bei einigen Dateisystemen die Groß-/Kleinschreibung beachtet wird und verschiedene Verzeichnis Trennzeichen vorhanden sind. Es wird empfohlen, `Path.Combine()` beim Erstellen von Datei-oder Verzeichnis Pfaden immer die gleiche Groß-/Kleinschreibung für Dateinamen und die-Methode zu verwenden.

### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows. Storage für Windows 8 und Windows 10

Das *Erstellen Mobile Apps mit dem xamarin. Forms* - [Buch](https://developer.xamarin.com/r/xamarin-forms/book/) 
 [Kapitel 20. Async und Datei-e/](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) a enthalten [Beispiele für Windows 8.1 und Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Mithilfe [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) von ist es möglich, Dateien auf diesen Plattformen mithilfe der unterstützten APIs zu lesen und zu verwenden:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Weitere Informationen finden Sie im [Buchkapitel](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) .

<a name="Isolated_Storage"></a>

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Isolierter Speicher auf Windows Phone 7 & 8 (Silverlight)

Isolierter Speicher ist eine gängige API zum Speichern und Laden von Dateien über alle IOS-, Android-und älteren Windows Phone Plattformen.

Dies ist der Standardmechanismus für den Dateizugriff in Windows Phone (Silverlight), der in xamarin. IOS und xamarin. Android implementiert wurde, damit allgemeiner Datei Zugriffs Code geschrieben werden kann. Auf die- `System.IO.IsolatedStorage` Klasse kann über alle drei Plattformen in einem frei [gegebenen Projekt](~/cross-platform/app-fundamentals/shared-projects.md)verwiesen werden.

Weitere Informationen Windows Phone finden Sie unter [Übersicht über den isolierten Speicher](https://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) .

Die APIs für den isolierten Speicher sind in [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)nicht verfügbar. Eine Alternative für die PCL ist das [pclstorage-nuget](https://pclstorage.codeplex.com/) .

### <a name="cross-platform-file-access-in-pcls"></a>Plattformübergreifender Dateizugriff in pcls

Außerdem gibt es eine PCL-kompatible nuget-– [pclstorage](https://www.nuget.org/packages/PCLStorage/) –, die plattformübergreifenden Dateizugriff für xamarin-unterstützte Plattformen und die neuesten Windows-APIs bietet.

## <a name="network-operations"></a>Netzwerkvorgänge

Die meisten mobilen Anwendungen verfügen über eine Netzwerkkomponente, z. b.:

- Herunterladen von Bildern, Videos und Audiodaten (z. b. Miniaturansichten, Fotos, Musik).
- Herunterladen von Dokumenten (z.b. HTML, PDF).
- Hochladen von Benutzerdaten (z. b. Fotos oder Text).
- Zugreifen auf Webdienste oder Drittanbieter-APIs (einschließlich SOAP, XML oder JSON).

Der .NET Framework bietet verschiedene Klassen für den Zugriff auf Netzwerkressourcen: `HttpClient` , `WebClient` und `HttpWebRequest` .

### <a name="httpclient"></a>HttpClient

Die `HttpClient` -Klasse im `System.Net.Http` -Namespace ist in xamarin. IOS, xamarin. Android und den meisten Windows-Plattformen verfügbar. Es gibt eine [Microsoft HTTP-Client Bibliothek](https://www.nuget.org/packages/Microsoft.Net.Http/) , mit der Sie diese API in Portable Klassenbibliotheken (und Windows Phone 8 Silverlight) einbringen können.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

Die- `WebClient` Klasse stellt eine einfache API zum Abrufen von Remote Daten von Remote Servern bereit.

Universelle Windows-Plattform Vorgänge *müssen* Async sein, auch wenn xamarin. IOS und xamarin. Android synchrone Vorgänge unterstützen (was für Hintergrundthreads möglich ist).

Der Code für einen einfachen asynchronen `WebClient` Vorgang lautet wie folgt:

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

 `WebClient`verfügt auch über `DownloadFileCompleted` und `DownloadFileAsync` zum Abrufen von Binärdaten.

<a name="HttpWebRequest"></a>

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest`bietet mehr Anpassungen als `WebClient` und erfordert daher mehr Code für die Verwendung von.

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

 <a name="Reachability"></a>

### <a name="reachability"></a>Reachability

Mobile Geräte arbeiten unter einer Vielzahl von Netzwerkbedingungen, von schnellen Wi-Fi-oder 4G-Verbindungen zu schlechten Empfangsbereichen und langsamen Daten Verknüpfungen. Aus diesem Grund empfiehlt es sich, zu erkennen, ob das Netzwerk verfügbar ist, und wenn dies der Fall ist, welcher Netzwerktyp verfügbar ist, bevor versucht wird, eine Verbindung mit Remote Servern herzustellen.

Zu den Aktionen, die eine Mobile App in diesen Fällen ausführen kann, gehören:

- Wenn das Netzwerk nicht verfügbar ist, sollten Sie den Benutzer informieren. Wenn Sie Sie manuell deaktiviert haben (z. b. Der Flugzeug Modus oder das Ausschalten des WLAN), dann können Sie das Problem beheben.
- Wenn die Verbindung 3G ist, können sich Anwendungen anders Verhalten (z. b. Apple lässt nicht zu, dass apps, die größer als 20 MB sind, über 3G heruntergeladen werden). Anwendungen könnten diese Informationen verwenden, um den Benutzer über übermäßig lange Downloadzeiten beim Abrufen großer Dateien zu warnen.
- Auch wenn das Netzwerk verfügbar ist, empfiehlt es sich, die Konnektivität mit dem Zielserver zu überprüfen, bevor andere Anforderungen initiiert werden. Dadurch wird verhindert, dass die Netzwerk Vorgänge der APP wiederholt ablaufen und dem Benutzer eine informative Fehlermeldung angezeigt wird.

## <a name="webservices"></a>WebServices

Weitere Informationen finden Sie in unserer Dokumentation zum [Arbeiten mit Webdiensten](~/cross-platform/data-cloud/web-services/index.md), die den Zugriff auf Rest-, SOAP-und WCF-Endpunkte mit xamarin. IOS behandelt. Es ist möglich, Webdienst Anforderungen Hand zu erstellen und die Antworten zu analysieren. es sind jedoch Bibliotheken verfügbar, um dies zu vereinfachen, einschließlich Azure, restsharp und servicestack. Sogar grundlegende WCF-Vorgänge können in xamarin-apps aufgerufen werden.

### <a name="azure"></a>Azure

Microsoft Azure ist eine cloudplattform, die eine Vielzahl von Diensten für Mobile Apps bereitstellt, einschließlich Datenspeicherung und-Synchronisierung und Pushbenachrichtigungen.

Besuchen Sie [Azure.Microsoft.com](https://azure.microsoft.com/) , um es kostenlos zu testen.

### <a name="restsharp"></a>RestSharp

Restsharp ist eine .NET-Bibliothek, die in Mobile Anwendungen integriert werden kann, um einen Rest-Client bereitzustellen, der den Zugriff auf Webdienste vereinfacht. Dies erleichtert die Bereitstellung einer einfachen API, um Daten anzufordern und die Rest-Antwort zu analysieren. Restsharp kann nützlich sein.

Die [restsharp-Website](http://restsharp.org/) enthält eine [Dokumentation](https://github.com/restsharp/RestSharp/wiki) zur Implementierung eines Rest-Clients mithilfe von restsharp.
Restsharp bietet xamarin. IOS-und xamarin. Android-Beispiele auf [GitHub](https://github.com/restsharp/RestSharp/).

Es gibt auch einen xamarin. IOS-Code Ausschnitt in unserer [Webdienst Dokumentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack"></a>

### <a name="servicestack"></a>ServiceStack

Im Gegensatz zu restsharp ist servicestack sowohl eine serverseitige Lösung zum Hosten eines Webdiensts als auch eine Client Bibliothek, die in mobilen Anwendungen implementiert werden kann, um auf diese Dienste zuzugreifen.

Die [servicestack-Website](http://servicestack.net/) erläutert den Zweck des Projekts und Links zu Dokument-und Codebeispielen. Die Beispiele umfassen eine komplette serverseitige Implementierung eines Webdiensts sowie verschiedene Client seitige Anwendungen, die darauf zugreifen können.

### <a name="wcf"></a>WCF

Mithilfe von xamarin-Tools können Sie einige Windows Communication Foundation (WCF)-Dienste nutzen. Im Allgemeinen unterstützt xamarin die gleiche Client seitige Teilmenge von WCF, die im Lieferumfang der Silverlight-Laufzeit enthalten ist. Dies schließt die gängigsten Codierungs-und Protokollimplementierungen von WCF: Text codierte SOAP-Nachrichten über das HTTP-Transportprotokoll mithilfe von ein `BasicHttpBinding` .

Aufgrund der Größe und Komplexität des WCF-Frameworks können aktuelle und zukünftige Dienst Implementierungen vorhanden sein, die außerhalb des Bereichs liegen, der von der Client-Subset-Domäne von xamarin unterstützt wird. Außerdem erfordert die WCF-Unterstützung die Verwendung von Tools, die nur in einer Windows-Umgebung zur Generierung des Proxys verfügbar sind.

 <a name="Threading"></a>

## <a name="threading"></a>Threading

Die Reaktionsfähigkeit der Anwendung ist für mobile Anwendungen wichtig – Benutzer erwarten, dass Anwendungen schnell geladen und durchgeführt werden. Ein "fixierter" Bildschirm, der die Annahme von Benutzereingaben nicht mehr anzeigt, scheint, dass die Anwendung abgestürzt ist, daher ist es wichtig, den UI-Thread nicht mit langen blockierenden aufrufen zu verknüpfen, wie z. b. Netzwerk Anforderungen oder langsame lokale Vorgänge (z. b. Entzippen einer Datei). Insbesondere sollte der Startprozess keine Aufgaben mit langer Ausführungsdauer enthalten – alle mobilen Plattformen beenden eine APP, die zu lange dauert, bis Sie geladen werden kann.

Dies bedeutet, dass Ihre Benutzeroberfläche eine "Fortschrittsanzeige" oder eine anderweitig verwendbare Benutzeroberfläche implementieren muss, die schnell angezeigt werden kann, sowie asynchrone Aufgaben zum Durchführen von Hintergrund Vorgängen. Das Ausführen von Hintergrundaufgaben erfordert die Verwendung von Threads. das bedeutet, dass die Hintergrundaufgaben eine Möglichkeit benötigen, um mit dem Haupt Thread zu kommunizieren, um den Fortschritt anzuzeigen, oder wann Sie abgeschlossen wurden.

 <a name="Parallel_Task_Library"></a>

### <a name="parallel-task-library"></a>Parallele Task Bibliothek

Aufgaben, die mit der parallel-Task Bibliothek erstellt werden, können asynchron ausgeführt werden und werden in Ihrem aufrufenden Thread zurückgegeben, sodass Sie sehr nützlich sind, um Vorgänge mit langer Ausführungszeit auszulösen, ohne die Benutzeroberfläche

Ein einfacher paralleler Task Vorgang könnte wie folgt aussehen:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Der Schlüssel ist `TaskScheduler.FromCurrentSynchronizationContext()` , der die SynchronizationContext. Current-Eigenschaft des Threads, der die-Methode aufruft (hier der Haupt Thread, der ausgeführt wird `MainThreadMethod` ) als Möglichkeit zum Mars Hallen von Aufrufen an diesen Thread wieder verwendet. Dies bedeutet, wenn die-Methode im UI-Thread aufgerufen wird, wird der `ContinueWith` Vorgang im UI-Thread wieder ausgeführt.

Wenn der Code Tasks aus anderen Threads startet, verwenden Sie das folgende Muster, um einen Verweis auf den UI-Thread zu erstellen, und der Task kann dennoch einen Rückruf dafür ausführen:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread"></a>

### <a name="invoking-on-the-ui-thread"></a>Aufrufen von im UI-Thread

Für Code, der die parallele Aufgaben Bibliothek nicht verwendet, verfügt jede Plattform über eine eigene Syntax für das Marshalling von Vorgängen zurück zum UI-Thread:

- **IOS** -–`owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** -–`owner.RunOnUiThread(action)`
- **Xamarin. Forms** –`Device.BeginInvokeOnMainThread(action)`
- **Windows** –`Deployment.Current.Dispatcher.BeginInvoke(action)`

Die IOS-und Android-Syntax erfordert, dass eine Context-Klasse verfügbar ist. Dies bedeutet, dass der Code dieses Objekt an alle Methoden übergeben muss, die einen Rückruf für den UI-Thread erfordern.

Um UI-Thread Aufrufe in frei gegebenem Code durchführen zu können, befolgen Sie das [idispatchonuithread-Beispiel](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (Courtesy of [@follesoe](https://twitter.com/follesoe) ). Deklarieren Sie eine Schnittstelle im freigegebenen Code, und Programmieren Sie, `IDispatchOnUIThread` und implementieren Sie dann die plattformspezifischen Klassen, wie hier gezeigt:

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

 <a name="Platform_and_Device_Capabilities_and_Degradation"></a>

## <a name="platform-and-device-capabilities-and-degradation"></a>Plattform-und Gerätefunktionen und-Beeinträchtigung

Weitere spezifische Beispiele für den Umgang mit verschiedenen Funktionen finden Sie in der Dokumentation zu den Plattformfunktionen. Es befasst sich mit dem Erkennen verschiedener Funktionen und der ordnungsgemäßen deverarbeitung einer Anwendung, um eine gute Benutzer Funktionalität zu gewährleisten, selbst wenn die APP nicht mit dem vollen Potenzial arbeiten kann.
