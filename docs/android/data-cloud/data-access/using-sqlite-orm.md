---
title: Verwenden von SQLite.NET mit Android
description: Die SQLite.NET PCL NuGet-Bibliothek bietet es sich um einen einfachen Zugriffsmechanismus für Xamarin.Android-apps.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/18/2018
ms.openlocfilehash: 878b0097fc0f62e6b90d948d8a15ab39db4b2f3e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732254"
---
# <a name="using-sqlitenet-with-android"></a>Verwenden von SQLite.NET mit Android

Die SQLite.NET-Bibliothek, die empfiehlt Xamarin ist ein sehr einfaches ORM, mit dem Sie leicht zu speichern und Abrufen von Objekten in der lokalen SQLite-Datenbank auf einem Android-Gerät. ORM steht für die Zuordnung von Objekt relationalen &ndash; eine API, mit dem Sie das Speichern und Abrufen von "Objekte" aus einer Datenbank ohne das Schreiben von SQL-Anweisungen.

Zum Einschließen der SQLite.NET-Bibliothek in einer Xamarin-app fügen Sie die folgenden NuGet-Paket zum Projekt hinzu:

- **Paketname:** Sqlite-Net-Pcl
- **Autor:** Frank A. Krueger
- **ID:** Sqlite-Net-Pcl
- **URL:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet-Paket](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet-Paket")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Stehen eine Reihe von verschiedenen SQLite-Paketen zur Verfügung – Achten Sie darauf, dass Sie die richtige auszuwählen (es möglicherweise nicht die obersten führen zu suchen).

Nachdem Sie die Bibliothek SQLite.NET verfügbar haben, führen Sie diese drei Schritte, um es verwenden, um eine Datenbank zuzugreifen:

1.  **Fügen Sie eine using Anweisung** &ndash; fügen Sie die folgende Anweisung, um die C#-Dateien, für den Datenzugriff erforderlich ist:

    ```csharp
    using SQLite;
    ```

2.  **Erstellen Sie eine leere Datenbank** &ndash; ein Datenbankverweis übergeben dem Dateipfad des Klassenkonstruktors SQLiteConnection erstellt werden. Sie müssen nicht überprüfen, ob die Datei bereits vorhanden ist &ndash; wird es automatisch erstellt werden, falls erforderlich, andernfalls wird die vorhandene Datenbankdatei geöffnet werden. Die `dbPath` Variable sollte entsprechend der weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Speichern von Daten** &ndash; nach der Erstellung eines Objekts SQLiteConnection Datenbankbefehle durch Aufrufen der Methoden, wie z. B. CreateTable und fügen Sie wie folgt ausgeführt:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Abrufen von Daten** &ndash; zum Abrufen eines Objekts (oder eine Liste von Objekten) verwenden die folgende Syntax:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Lernprogramm zu Data Access-Beispiel

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht wie folgt, wenn auf Android ausgeführt wird. Der Code wird veranschaulicht, wie einfache SQLite.NET Vorgänge ausführen und zeigt die Ergebnisse als Text im Hauptfenster der Anwendung.


**Android**

![Beispiel zu Android SQLite.NET](using-sqlite-orm-images/image3.png "Android SQLite.NET-Beispiel")

Das folgende Codebeispiel zeigt eine ganze Datenbankinteraktion mithilfe der SQLite.NET-Bibliothek, um die auf die zugrunde liegenden Datenbank zu kapseln.
Sie zeigt:

1.  Erstellen die Datenbankdatei

2.  Einfügen von Daten durch Objekte erstellen und diese dann speichern

3.  Abfragen der Daten

Sie müssen diese Namespaces enthalten:

```csharp
using SQLite; // from the github SQLite.cs class
```

Das letzte Lesezeichen erfordert, dass Sie Ihr Projekt SQLite hinzugefügt haben. Beachten Sie, dass die SQLite-Datenbanktabelle durch Hinzufügen von Attributen zu einer Klasse definiert ist (die `Stock` Klasse) anstatt einer CREATE TABLE-Befehl.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Mithilfe der `[Table]` Attribut ohne Angabe ein Tabelle Name-Parameter führt dazu, dass die zugrunde liegenden Datenbanktabelle haben den gleichen Namen wie die Klasse (in diesem Fall "Stock"). Der tatsächliche Tabellenname ist wichtig, wenn Sie SQL-Abfragen direkt gegen die Datenbank zu schreiben, anstatt die Verwendung von ORM-Datenzugriffsmethoden verwenden. Auf ähnliche Weise die `[Column("_id")]` Attribut ist optional, und wenn die Tabelle mit den gleichen Namen wie die Eigenschaften der Klasse fehlt eine Spalte hinzugefügt werden.

## <a name="sqlite-attributes"></a>SQLite-Attribute

Allgemeinen Attribute, die Sie anwenden können, in Ihren Klassen zu steuern, wie sie in der zugrunde liegenden Datenbank gespeichert werden, gehören:

-   **[PrimaryKey]**  &ndash; Dieses Attribut kann auf eine Ganzzahleigenschaft, die als Primärschlüssel der zugrunde liegenden Tabelle erzwingen angewendet werden. Zusammengesetzte Primärschlüssel werden nicht unterstützt.

-   **[AutoIncrement]**  &ndash; Dieses Attribut führt dazu, dass eine Ganzzahleigenschaft Wert automatisch inkrementierten für jedes neue Objekt in die Datenbank eingefügt werden.

-   **[Column(name)]**  &ndash; Die optionale Angabe `name` Parameter wird den Standardwert der zugrunde liegende Datenbankspalte Name (identisch mit der Eigenschaft wird) außer Kraft setzen.

-   **[Table(name)]**  &ndash; Markiert die Klasse als nicht in einer zugrunde liegenden SQLite-Tabelle gespeichert werden können. Angeben der optionale Name-Parameter wird den Standardwert der zugrunde liegenden Datenbanktabelle namens überschrieben (identisch mit der Klassenname ist).

-   **[MaxLength(value)]**  &ndash; Die Länge des eine Texteigenschaft zu beschränken, wenn, eine Datenbank einfügen versucht wird. Nutzung von Code sollte überprüfen Sie dies vor dem Einfügen des Objekts, wie dieses Attribut nur 'aktiviert ist' eine Datenbank INSERT- oder Updatevorgang wird ausgeführt.

-   **[Ignorieren]**  &ndash; Bewirkt, dass SQLite.NET, diese Eigenschaft ignoriert werden sollen.
    Dies ist besonders nützlich für Eigenschaften, die einen Typ aufweisen, der in der Datenbank gespeichert werden kann oder modellauflistungen bereit, die nicht automatisch aufgelöst werden können, SQLite sein.

-   **[Unique]**  &ndash; Wird sichergestellt, dass die Werte in der zugrunde liegenden Datenbankspalte eindeutig sind.


Die meisten dieser Attribute sind optional, SQLite Standardwerte für Tabellen- und Spaltennamen. Sie sollten immer einen ganze Zahl Primärschlüssel angeben, sodass Auswahl und Löschen von Abfragen für Ihre Daten effizient ausgeführt werden können.

## <a name="more-complex-queries"></a>Komplexere Abfragen

Die folgenden Methoden für `SQLiteConnection` genutzt werden, andere Data-Vorgänge auszuführen:

-   **Fügen Sie** &ndash; Fügt ein neues Objekt mit der Datenbank.

-   **Abrufen&lt;T&gt;**  &ndash; versucht, ein Objekt mit dem primären Schlüssel abzurufen.

-   **Tabelle&lt;T&gt;**  &ndash; aller Objekte in der Tabelle zurück.

-   **Löschen Sie** &ndash; Löscht ein Objekt mit dem primären Schlüssel.

-   **Abfrage&lt;T&gt;**  &ndash; Ausführen eine SQL-Abfrage, die eine Anzahl von Zeilen (als Objekte) zurückgibt.

-   **Führen Sie** &ndash; verwenden Sie diese Methode (und nicht `Query`) Wenn Sie Zeilen aus der SQL (z. B. INSERT, UPDATE und DELETE-Anweisungen) erwarten nicht.


### <a name="getting-an-object-by-the-primary-key"></a>Ein Objekt abrufen durch den primären Schlüssel

SQLite.Net stellt die Get-Methode, um ein einzelnes Objekt basierend auf den primären Schlüssel abzurufen.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Ein Objekt, das mithilfe von Linq auswählen

Unterstützen von Methoden, die Auflistungen zurückgeben `IEnumerable<T>` damit Sie Linq verwenden können, um Abfragen oder den Inhalt einer Tabelle zu sortieren. Der folgende Code zeigt ein Beispiel für die Verwendung von Linq, alle Einträge herausfiltern, die mit dem Buchstaben "A" beginnen:

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Auswählen eines Objekts mit SQL

Obwohl SQLite.Net objektbasierten Zugriff auf Ihre Daten bereitstellen kann, müssen Sie in einigen Fällen möchten Sie eine komplexere Abfrage als Linq ermöglicht (oder möglicherweise schnellere). Sie können SQL-Befehle mit der Abfrage-Methode verwenden, wie hier gezeigt:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> Wenn SQL-Anweisungen direkt schreiben erstellen Sie eine Abhängigkeit auf den Namen der Tabellen und Spalten, die in der Datenbank aus Ihrer Klassen und ihre Attribute generiert wurden. Wenn Sie diesen Namen im Code ändern müssen Sie daran denken, aktualisieren Sie jede SQL-Anweisung manuell geschrieben.

### <a name="deleting-an-object"></a>Löschen eines Objekts

Der Primärschlüssel ist zum Löschen der Zeile verwendet, wie hier gezeigt:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Sehen Sie sich die `rowcount` , bestätigen, wie viele Zeilen betroffen sind (in diesem Fall gelöscht).

## <a name="using-sqlitenet-with-multiple-threads"></a>Verwenden von SQLite.NET mit mehreren Threads

SQLite unterstützt drei verschiedene threading-Modi: *Single-Thread*, *Multi-Thread*, und *serialisierte*. Wenn Sie die Datenbank aus mehreren Threads ohne jede Einschränkung zugreifen möchten, können Sie konfigurieren, dass SQLite verwendet die **serialisierte** threading-Modus. Es ist wichtig, legen Sie diesen Modus früh in der Anwendung (z. B. am Anfang der `OnCreate` Methode).

Rufen Sie zum Ändern des threading Modus `SqliteConnection.SetConfig`. Zum Beispiel konfiguriert diese Codezeile SQLite für **serialisierte** Modus: 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Die Android-Version des SQLite verfügt über eine Einschränkung, die einige zusätzliche Schritte erforderlich sind. Wenn der Aufruf von `SqliteConnection.SetConfig` erzeugt Sie z. B. eine Ausnahme SQLite `library used incorrectly`, müssen Sie die folgende problemumgehung verwenden:

1.  Verknüpfung mit dem systemeigenen **libsqlite.so** Bibliothek, damit die `sqlite3_shutdown` und `sqlite3_initialize` APIs sind für die app zur Verfügung gestellt:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  Ganz am Anfang des der `OnCreate` -Methode, fügen Sie diesen Code zum Herunterfahren SQLite, konfigurieren Sie ihn für **serialisierte** Modus, und initialisieren SQLite:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Diese problemumgehung funktioniert auch für die `Mono.Data.Sqlite` Bibliothek. Weitere Informationen zu SQLite und Multithreading, finden Sie unter [SQLite und mehrere Threads](https://www.sqlite.org/threadsafe.html). 

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
