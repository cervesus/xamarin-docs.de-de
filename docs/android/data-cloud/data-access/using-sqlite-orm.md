---
title: Verwenden von SQLite.NET mit Android
description: Die von SQLite.NET PCL-NuGet-Bibliothek stellt einen einfachen Mechanismus für Xamarin.Android-apps bereit.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 6525cb321537a7cefb24feb1e77b532068b098ef
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019667"
---
# <a name="using-sqlitenet-with-android"></a>Verwenden von SQLite.NET mit Android

Die von SQLite.NET-Bibliothek, die Xamarin empfiehlt ist ein sehr einfaches ORM, mit dem Sie ganz einfach speichern und Abrufen von Objekten in der lokalen SQLite-Datenbank auf einem Android-Gerät. ORM steht für die Zuordnung von Objekt relationalen &ndash; eine API, mit dem Sie das Speichern und Abrufen von "Objekte" aus einer Datenbank ohne SQL-Anweisungen schreiben zu müssen.

Um die Bibliothek von SQLite.NET in einer Xamarin-app zu einzuschließen, fügen Sie das folgende NuGet-Paket zu Ihrem Projekt ein:

- **Paketname:** Sqlite-Net-Plc
- **Autor:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![Von SQLite.NET NuGet-Paket](using-sqlite-orm-images/image1a-sml.png "von SQLite.NET NuGet-Paket")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Stehen eine Anzahl von unterschiedlichen SQLite-Paketen zur Verfügung – Achten Sie darauf, dass Sie die richtige Vorlage auswählen (das oberste Suchergebnis in Search möglicherweise nicht).

Nachdem Sie die Bibliothek von SQLite.NET verfügbar haben, führen Sie diese drei Schritte aus, um es verwenden, um den Zugriff auf eine Datenbank aus:

1.  **Hinzufügen einer using Anweisung** &ndash; fügen die folgende Anweisung aus, um die C# Dateien, für den Datenzugriff erforderlich ist:

    ```csharp
    using SQLite;
    ```

2.  **Erstellen einer leeren Datenbank** &ndash; eine datenbankverbindung ein Datenbankverweis übergeben dem Dateipfad des SQLiteConnection-Klassenkonstruktors erstellt werden. Sie müssen nicht überprüfen, ob die Datei bereits vorhanden &ndash; wird es automatisch erstellt werden, falls erforderlich, andernfalls wird die vorhandene Datenbankdatei geöffnet werden. Die `dbPath` Variablen entsprechend die weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden soll:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Speichern von Daten** &ndash; nach der Erstellung eines Objekts SQLiteConnection Datenbankbefehle durch Aufrufen der Methoden, z. B. CreateTable und fügen Sie wie folgt ausgeführt werden:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Abrufen von Daten** &ndash; verwenden Sie zum Abrufen eines Objekts (oder eine Liste von Objekten) die folgende Syntax:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Grundlegende Data Access-Beispiel

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht folgendermaßen aus, wenn unter Android ausgeführt wird. Der Code wird veranschaulicht, wie zum Ausführen von Vorgängen für einfache von SQLite.NET und zeigt die Ergebnisse als Text im Hauptfenster der Anwendung.


**Android**

![Beispiel zu Android von SQLite.NET](using-sqlite-orm-images/image3.png "Android von SQLite.NET-Beispiel")

Das folgende Codebeispiel zeigt eine ganze Datenbankinteraktion mithilfe der von SQLite.NET-Bibliothek, um die auf die zugrunde liegenden Datenbank zu kapseln.
Sie zeigt:

1.  Erstellen der Datenbankdatei

2.  Einfügen von Daten durch Erstellen von Objekten und diese dann speichern

3.  Abfragen der Daten

Sie müssen diese Namespaces enthalten:

```csharp
using SQLite; // from the github SQLite.cs class
```

Der letzte Suchvorgang erfordert, dass Sie SQLite zu Ihrem Projekt hinzugefügt haben. Beachten Sie, dass die SQLite-Datenbank-Tabelle durch Hinzufügen von Attributen zu einer Klasse definiert ist (die `Stock` Klasse) anstelle einer CREATE TABLE-Befehl.

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

Mithilfe der `[Table]` Attribut ohne Angabe ein Tabelle Name-Parameter führt dazu, dass die zugrunde liegenden Datenbanktabelle auf den gleichen Namen wie die Klasse (in diesem Fall "Stock") haben. Den Namen der tatsächlichen Tabelle ist wichtig, wenn Sie SQL-Abfragen direkt in der Datenbank zu schreiben, anstatt die ORM-Methoden für den Datenzugriff verwenden. Auf ähnliche Weise die `[Column("_id")]` Attribut ist optional, und falls nicht vorhanden, eine Spalte in der Tabelle mit dem gleichen Namen wie die Eigenschaft in der Klasse hinzugefügt werden.

## <a name="sqlite-attributes"></a>SQLite-Attribute

Allgemeine Attribute, die Sie anwenden können, auf Ihre Klassen zu steuern, wie sie in der zugrunde liegenden Datenbank gespeichert sind enthalten:

-   **[PrimaryKey]**  &ndash; Dieses Attribut kann auf eine Ganzzahleigenschaft, die zugrunde liegende Tabelle Primärschlüssel erzwingen angewendet werden. Zusammengesetzter Primärschlüssel werden nicht unterstützt.

-   **[AutoIncrement]**  &ndash; Dieses Attribut führt dazu, dass eine Integer-Eigenschaft-Wert, um automatisch inkrementierten für jedes neue Objekt, das in die Datenbank eingefügt werden

-   **[Column(name)]**  &ndash; Angeben des optionalen `name` Parameter überschreibt den Standardwert des zugrunde liegenden der Name der Datenbankspalte (die die Eigenschaft identisch ist).

-   **[Table(name)]**  &ndash; Markiert die Klasse als in einer zugrunde liegenden SQLite-Tabelle gespeichert werden können. Angeben des optionale Name-Parameters überschreibt den Standardwert des zugrunde liegenden den Namen der Datenbanktabelle (die den Namen der Klasse identisch ist).

-   **[MaxLength(value)]**  &ndash; Beschränken Sie die Länge einer Text-Eigenschaft, wenn, dass Sie eine Datenbank einfügen versucht wird. Nutzen von Code sollte überprüfen Sie dies vor dem Einfügen des Objekts, wie dieses Attribut nur 'aktiviert ist' bei Einfüge- oder Updatevorgang versucht wird.

-   **[Ignorieren]**  &ndash; Bewirkt, dass von SQLite.NET, diese Eigenschaft ignoriert werden sollen.
    Dies ist besonders nützlich für Eigenschaften, die einen Typ aufweisen, der nicht in der Datenbank gespeichert werden, oder Eigenschaften, die Auflistungen zu modellieren, die vom SQLite nicht automatisch aufgelöst werden kann.

-   **[Unique]**  &ndash; Wird sichergestellt, dass die Werte in der zugrunde liegenden Datenbank eindeutig sind.


Die meisten dieser Attribute sind optional, SQLite Standardwerte für Tabellen- und Spaltennamen. Sie sollten immer einen Primärschlüssel für die ganze Zahl angeben, so dass die Auswahl und Löschen von Abfragen für Ihre Daten effizient ausgeführt werden können.

## <a name="more-complex-queries"></a>Komplexere Abfragen

Die folgenden Methoden für `SQLiteConnection` können verwendet werden, um andere Datenvorgänge ausführen:

-   **Fügen Sie** &ndash; Fügt ein neues Objekt in der Datenbank.

-   **Erste&lt;T&gt;**  &ndash; versucht, ein Objekt, das über den primären Schlüssel abzurufen.

-   **Tabelle&lt;T&gt;**  &ndash; alle Objekte in der Tabelle zurück.

-   **Löschen Sie** &ndash; Löscht ein Objekt mit dem primären Schlüssel.

-   **Abfrage&lt;T&gt;**  &ndash; führen Sie eine SQL-Abfrage, die eine Anzahl von Zeilen (als Objekte) zurückgibt.

-   **Führen Sie** &ndash; verwenden Sie diese Methode (und nicht `Query`) Wenn Sie erwarten nicht, dass Zeilen aus der SQL (z. B. INSERT, UPDATE und DELETE-Anweisungen) zurück.


### <a name="getting-an-object-by-the-primary-key"></a>Ein Objekt abrufen durch den primären Schlüssel

Von SQLite.Net enthält die Get-Methode, um ein einzelnes Objekt basierend auf den primären Schlüssel abzurufen.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Auswählen eines Objekts mithilfe von Linq

Unterstützung für Methoden, die Auflistungen zurückgeben `IEnumerable<T>` daher können Sie verwenden Linq zum Abfragen oder den Inhalt einer Tabelle zu sortieren. Der folgende Code zeigt ein Beispiel zur Verwendung von Linq, alle Einträge herauszufiltern, die mit dem Buchstaben "A" beginnen:

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Auswählen eines Objekts mithilfe von SQL

Obwohl von SQLite.Net objektbasierten Zugriff auf Ihre Daten bereitstellen können, müssen Sie manchmal möchten Sie eine komplexere Abfrage als Linq ermöglicht (oder möglicherweise schnellere Leistung). Sie können SQL-Befehle mit der Query-Methode verwenden, wie hier gezeigt:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> Beim Schreiben SQL-Anweisungen direkt erstellen Sie eine Abhängigkeit auf den Namen der Tabellen und Spalten in der Datenbank, die aus den Klassen und deren Attribute generiert wurden. Wenn Sie diese Namen in Ihrem Code ändern, müssen Sie alle manuell erstellten SQL-Anweisungen zu aktualisieren.

### <a name="deleting-an-object"></a>Das Löschen eines Objekts

Der primäre Schlüssel dient zum Löschen der Zeile, wie hier gezeigt:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Sehen Sie sich die `rowcount` , bestätigen, wie viele Zeilen betroffen sind (in diesem Fall gelöscht).

## <a name="using-sqlitenet-with-multiple-threads"></a>Verwenden von SQLite.NET mit mehreren Threads

SQLite unterstützt drei unterschiedliche threading-Modi: *Singlethread-*, *mit mehreren Threads*, und *serialisiert*. Wenn Sie die Datenbank von mehreren Threads ohne jegliche Einschränkungen zugreifen möchten, können Sie konfigurieren, dass SQLite verwendet die **serialisiert** threading-Modus. Es ist wichtig, die in diesem Modus einem frühen Zeitpunkt in Ihrer Anwendung einzurichten (z. B. am Anfang der `OnCreate` Methode).

Um den threading-Modus zu ändern, rufen `SqliteConnection.SetConfig`. Diese Codezeile konfiguriert, SQLite für **serialisiert** Modus:

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Die Android-Version von SQLite ist eine Einschränkung, die ein paar mehr Schritte erforderlich sind. Wenn der Aufruf von `SqliteConnection.SetConfig` erzeugt Sie z. B. eine SQLite-Ausnahme `library used incorrectly`, müssen Sie die folgende problemumgehung verwenden:

1.  Link zur systemeigenen **libsqlite.so** Bibliothek, damit die `sqlite3_shutdown` und `sqlite3_initialize` APIs für die app zur Verfügung gestellt:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2.  Ganz am Anfang des der `OnCreate` -Methode, fügen Sie folgenden Code zum Herunterfahren SQLite, konfigurieren Sie diese für **serialisiert** Modus, und initialisieren Sie SQLite:

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Diese problemumgehung funktioniert auch für die `Mono.Data.Sqlite` Bibliothek. Weitere Informationen zu SQLite und Multithreading, finden Sie unter [SQLite und mehreren Threads](https://www.sqlite.org/threadsafe.html).

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
