---
title: Verwenden von SQLite.NET mit Xamarin.iOS
description: Die SQLite.NET PCL NuGet-Bibliothek bietet es sich um einen einfachen Zugriffsmechanismus für Xamarin.iOS apps. Dieses Dokument enthält eine Übersicht über diese Bibliothek verwenden.
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/18/2018
ms.openlocfilehash: 2a96a7c3f9bf02110bc5e2b21e26e71fe9d84d83
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784984"
---
# <a name="using-sqlitenet-with-xamarinios"></a>Verwenden von SQLite.NET mit Xamarin.iOS

Die SQLite.NET-Bibliothek, die empfiehlt Xamarin ist eine grundlegende ORM, mit dem Sie das Speichern und Abrufen von Objekten in der lokalen SQLite-Datenbank auf einem iOS-Gerät.
Verwendung von ORM steht für Objekt Relational Mapping – eine API, mit dem Sie das Speichern und Abrufen von "Objekte" aus einer Datenbank ohne das Schreiben von SQL-Anweisungen.

<a name="Usage"/>

## <a name="usage"></a>Verwendung

Zum Einschließen der SQLite.NET-Bibliothek in einer Xamarin-app fügen Sie die folgenden NuGet-Paket zum Projekt hinzu:

- **Paketname:** Sqlite-Net-Pcl
- **Autor:** Frank A. Krueger
- **ID:** Sqlite-Net-Pcl
- **URL:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet-Paket](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet-Paket")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Stehen eine Reihe von verschiedenen SQLite-Paketen zur Verfügung – Achten Sie darauf, dass Sie die richtige auszuwählen (es möglicherweise nicht die obersten führen zu suchen).

Nachdem Sie die Bibliothek SQLite.NET verfügbar haben, führen Sie diese drei Schritte, um es verwenden, um eine Datenbank zuzugreifen:

1. **Fügen Sie eine using Anweisung** -fügen Sie die folgende Anweisung, um die C#-Dateien, für den Datenzugriff erforderlich ist:

    ```csharp
    using SQLite;
    ```

1. **Erstellen Sie eine leere Datenbank** -ein Datenbankverweis übergeben dem Dateipfad des Klassenkonstruktors SQLiteConnection erstellt werden. Sie müssen nicht überprüfen, ob die Datei bereits vorhanden ist – er automatisch erstellt werden wird, wenn erforderlich ist, andernfalls die vorhandene Datenbankdatei geöffnet wird.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    Die Variable DbPath sollte entsprechend der weiter oben in diesem Dokument beschriebenen Regeln ermittelt werden.

1. **Speichern von Daten** – Nachdem Sie, ein SQLiteConnection-Serverobjekt, Datenbank erstellt haben-Befehle ausgeführt werden, durch Aufrufen der Methoden, z. B. CreateTable und fügen Sie wie folgt aufrufen:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **Abrufen von Daten** – Informationen zum Abrufen eines Objekts (oder eine Liste von Objekten), verwenden Sie die folgende Syntax:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Lernprogramm zu Data Access-Beispiel

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht wie folgt, wenn auf iOS ausgeführt wird. Der Code wird veranschaulicht, wie einfache SQLite.NET Vorgänge ausführen und zeigt die Ergebnisse als Text im Hauptfenster der Anwendung.

**iOS**

 [![iOS SQLite.NET-Beispiel](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

Das folgende Codebeispiel zeigt eine ganze Datenbankinteraktion mithilfe der SQLite.NET-Bibliothek, um die auf die zugrunde liegenden Datenbank zu kapseln. Sie zeigt:

1.  Erstellen die Datenbankdatei
1.  Einfügen von Daten durch Objekte erstellen und diese dann speichern
1.  Abfragen der Daten

Sie müssen diese Namespaces enthalten:

```csharp
using SQLite; // from the github SQLite.cs class
```

Dies erfordert, dass Sie Ihr Projekt, als markierte SQLite hinzugefügt haben [hier](#Usage). Beachten Sie, dass die SQLite-Datenbanktabelle durch Hinzufügen von Attributen zu einer Klasse definiert ist (die `Stock` Klasse) anstatt einer CREATE TABLE-Befehl.

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

-  **[PrimaryKey]**  – Dieses Attribut kann auf eine Ganzzahleigenschaft, die als Primärschlüssel der zugrunde liegenden Tabelle erzwingen angewendet werden. Zusammengesetzte Primärschlüssel werden nicht unterstützt.
-  **[AutoIncrement]**  – Dieses Attribut führt dazu, dass eine Ganzzahleigenschaft Wert automatisch inkrementierten für jedes neue Objekt in die Datenbank eingefügt werden.
-  **[Column(name)]**  – Die optionale Angabe `name` Parameter wird den Standardwert der zugrunde liegende Datenbankspalte Name (identisch mit der Eigenschaft wird) außer Kraft setzen.
-  **[Table(name)]**  – Markiert die Klasse als nicht in einer zugrunde liegenden SQLite-Tabelle gespeichert werden können. Angeben der optionale Name-Parameter wird den Standardwert der zugrunde liegenden Datenbanktabelle namens überschrieben (identisch mit der Klassenname ist).
-  **[MaxLength(value)]**  – Die Länge des eine Texteigenschaft zu beschränken, wenn, eine Datenbank einfügen versucht wird. Nutzung von Code sollte überprüfen Sie dies vor dem Einfügen des Objekts, wie dieses Attribut nur 'aktiviert ist' eine Datenbank INSERT- oder Updatevorgang wird ausgeführt.
-  **[Ignorieren]**  – Bewirkt, dass SQLite.NET, diese Eigenschaft ignoriert werden sollen. Dies ist besonders nützlich für Eigenschaften, die einen Typ aufweisen, der in der Datenbank gespeichert werden kann oder modellauflistungen bereit, die nicht automatisch aufgelöst werden können, SQLite sein.
-  **[Unique]**  – Stellt sicher, dass die Werte in der zugrunde liegenden Datenbankspalte eindeutig sind.


Die meisten dieser Attribute sind optional, SQLite Standardwerte für Tabellen- und Spaltennamen. Sie sollten immer einen ganze Zahl Primärschlüssel angeben, sodass Auswahl und Löschen von Abfragen für Ihre Daten effizient ausgeführt werden können.

## <a name="more-complex-queries"></a>Komplexere Abfragen

Die folgenden Methoden für `SQLiteConnection` genutzt werden, andere Data-Vorgänge auszuführen:

-  **Fügen Sie** – Fügt ein neues Objekt mit der Datenbank.
-  **Abrufen<T>**  – versucht, ein Objekt mit dem primären Schlüssel abzurufen.
-  **Tabelle<T>**  – alle Objekte in der Tabelle zurück.
-  **Löschen Sie** – Löscht ein Objekt mit dem primären Schlüssel.
-  **Abfrage<T>**  -Ausführen eine SQL-Abfrage, die eine Anzahl von Zeilen (als Objekte) zurückgibt.
-  **Führen Sie** – verwenden Sie diese Methode (und nicht `Query` ) Wenn Sie Zeilen aus der SQL (z. B. INSERT, UPDATE und DELETE-Anweisungen) erwarten nicht.


### <a name="getting-an-object-by-the-primary-key"></a>Ein Objekt abrufen durch den primären Schlüssel

SQLite.Net stellt die Get-Methode, um ein einzelnes Objekt basierend auf den primären Schlüssel abzurufen.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Ein Objekt, das mithilfe von Linq auswählen

Methoden, die Auflistungen zurückgeben unterstützen IEnumerable<T> damit Sie Linq verwenden können, um Abfragen oder den Inhalt einer Tabelle zu sortieren. Der folgende Code zeigt ein Beispiel für die Verwendung von Linq, alle Einträge herausfiltern, die mit dem Buchstaben "A" beginnen:

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

> [!IMPORTANT]
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

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
