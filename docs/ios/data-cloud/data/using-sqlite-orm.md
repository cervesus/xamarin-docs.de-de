---
title: Verwenden von SQLite.net mit xamarin. IOS
description: Die nuget-Bibliothek sqlite.net PCL bietet einen einfachen Datenzugriffsmechanismus für xamarin. IOS-apps. Dieses Dokument enthält eine Übersicht über die Verwendung dieser Bibliothek.
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/18/2018
ms.openlocfilehash: 0416aed671c9675e7c2c990eb59a7a4d8cab7888
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199610"
---
# <a name="using-sqlitenet-with-xamarinios"></a>Verwenden von SQLite.net mit xamarin. IOS

Die von xamarin Empfohlene sqlite.NET-Bibliothek ist ein grundlegender ORM, mit dem Sie Objekte in der lokalen SQLite-Datenbank auf einem IOS-Gerät speichern und abrufen können.
ORM steht für relationale Objekt Zuordnung – eine API, mit der Sie "Objekte" aus einer Datenbank speichern und abrufen können, ohne SQL-Anweisungen zu schreiben.

<a name="Usage"/>

## <a name="usage"></a>Verwendung

Fügen Sie dem Projekt das folgende nuget-Paket hinzu, um die SQLite.NET-Bibliothek in eine xamarin-App einzubeziehen:

- **Paketname:** SQLite-net-PCL
- **Autor:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **URL:** [nuget.org/Packages/SQLite-net-PCL](https://www.nuget.org/packages/sqlite-net-pcl/)

[![Sqlite.net nuget-Paket](using-sqlite-orm-images/image1a-sml.png "Sqlite.net nuget-Paket")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Es gibt eine Reihe von unterschiedlichen SQLite-Paketen – achten Sie darauf, dass Sie die richtige Option auswählen (Dies ist möglicherweise nicht das beste Ergebnis bei der Suche).

Sobald die SQLite.NET-Bibliothek verfügbar ist, führen Sie die folgenden drei Schritte aus, um Sie für den Zugriff auf eine Datenbank zu verwenden:

1. **Fügen Sie eine using-Anweisung hinzu** : Fügen Sie den C# Dateien, in denen der Datenzugriff erforderlich ist, folgende Anweisung hinzu:

    ```csharp
    using SQLite;
    ```

1. **Erstellen einer leeren Datenbank** : ein Daten Bank Verweis kann erstellt werden, indem Sie den Dateipfad des sqliteconnection-Klassenkonstruktors übergeben. Sie müssen nicht überprüfen, ob die Datei bereits vorhanden ist – Sie wird bei Bedarf automatisch erstellt. andernfalls wird die vorhandene Datenbankdatei geöffnet.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    Die DBPath-Variable sollte gemäß den zuvor in diesem Dokument beschriebenen Regeln bestimmt werden.

1. **Speichern von Daten** : Nachdem Sie ein sqliteconnection-Objekt erstellt haben, werden Daten Bank Befehle durch Aufrufen der zugehörigen Methoden ausgeführt, wie z. b. CreateTable und INSERT wie folgt:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **Daten abrufen** : Verwenden Sie die folgende Syntax, um ein Objekt (oder eine Liste von Objekten) abzurufen:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Beispiel für den grundlegenden Datenzugriff

Der *DataAccess_Basic* -Beispielcode für dieses Dokument sieht wie folgt aus, wenn er unter IOS ausgeführt wird. Im Code wird veranschaulicht, wie einfache sqlite.net-Vorgänge durchgeführt und die Ergebnisse in als Text im Hauptfenster der Anwendung angezeigt werden.

**iOS**

 [![IOS sqlite.NET Beispiel](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

Das folgende Codebeispiel zeigt eine gesamte Daten Bank Interaktion mithilfe der SQLite.NET-Bibliothek, um den zugrunde liegenden Datenbankzugriff zu kapseln. Folgendes wird angezeigt:

1. Erstellen der Datenbankdatei
1. Einfügen von Daten durch Erstellen von Objekten und anschließendes Speichern
1. Abfragen der Daten

Sie müssen diese Namespaces einschließen:

```csharp
using SQLite; // from the github SQLite.cs class
```

Dies erfordert, dass Sie SQLite zu Ihrem Projekt hinzugefügt haben, wie [hier](#Usage)hervorgehoben. Beachten Sie, dass die SQLite-Datenbanktabelle durch Hinzufügen von Attributen zu `Stock` einer Klasse (der-Klasse) anstelle eines CREATE TABLE Befehls definiert wird.

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

Die Verwendung `[Table]` des-Attributs ohne Angabe eines Table Name-Parameters bewirkt, dass die zugrunde liegende Datenbanktabelle denselben Namen wie die Klasse hat (in diesem Fall "Stock"). Der tatsächliche Tabellenname ist wichtig, wenn Sie SQL-Abfragen direkt für die Datenbank schreiben, anstatt die ORM-Datenzugriffs Methoden zu verwenden. Auf ähnliche `[Column("_id")]` Weise ist das-Attribut optional, und wenn kein Wert vorhanden ist, wird der Tabelle eine Spalte mit demselben Namen wie die-Eigenschaft in der-Klasse hinzugefügt.

## <a name="sqlite-attributes"></a>SQLite-Attribute

Allgemeine Attribute, die Sie auf die Klassen anwenden können, um zu steuern, wie Sie in der zugrunde liegenden Datenbank gespeichert werden, umfassen Folgendes:

- **[PrimaryKey]** – dieses Attribut kann auf eine ganzzahlige Eigenschaft angewendet werden, um den Primärschlüssel der zugrunde liegenden Tabelle zu erzwingen. Zusammengesetzte Primärschlüssel werden nicht unterstützt.
- **[AUTOINCREMENT]** – dieses Attribut bewirkt, dass der Wert einer ganzzahligen Eigenschaft für jedes neue Objekt, das in die Datenbank eingefügt wird, automatisch erhöht wird.
- **[Column (Name)]** – bei Angabe des `name` optionalen Parameters wird der Standardwert für den Namen der zugrunde liegenden Daten Bank Spalte (identisch mit der-Eigenschaft) überschrieben.
- **[Tabelle (Name)]** – markiert die Klasse als in einer zugrunde liegenden SQLite-Tabelle gespeichert werden kann. Wenn Sie den optionalen Name-Parameter angeben, wird der Standardwert des Namens der zugrunde liegenden Datenbanktabelle (der mit dem Klassennamen identisch ist) überschrieben.
- **[MaxLength (Wert)]** – schränkt die Länge einer Text Eigenschaft ein, wenn versucht wird, eine Daten Bank Einfügung auszuführen. Durch die Verwendung von Code sollte dies vor dem Einfügen des Objekts überprüft werden, da dieses Attribut nur beim Versuch, einen Daten Bank Einfüge-oder Aktualisierungs Vorgang auszuführen, "überprüft" ist
- **[Ignore]** – bewirkt, dass SQLite.net diese Eigenschaft ignoriert. Dies ist besonders nützlich für Eigenschaften, die einen Typ haben, der nicht in der Datenbank gespeichert werden kann, oder für Eigenschaften, die Sammlungen modellieren, die nicht automatisch aufgelöst werden können, SQLite.
- **[Eindeutig]** – stellt sicher, dass die Werte in der zugrunde liegenden Daten Bank Spalte eindeutig sind.


Die meisten dieser Attribute sind optional, SQLite verwendet Standardwerte für Tabellen-und Spaltennamen. Sie sollten immer einen ganzzahligen Primärschlüssel angeben, damit Auswahl-und Lösch Abfragen für Ihre Daten effizient ausgeführt werden können.

## <a name="more-complex-queries"></a>Komplexere Abfragen

Die folgenden Methoden für `SQLiteConnection` können verwendet werden, um andere Daten Vorgänge auszuführen:

- **Insert** – fügt der Datenbank ein neues Objekt hinzu.
- **Get\<T >** – versucht, ein Objekt mithilfe des Primärschlüssels abzurufen.
- **Tabelle\<T >** – gibt alle Objekte in der Tabelle zurück.
- **Delete** – löscht ein Objekt mithilfe seines Primärschlüssels.
- **Query\<T >** : führt eine SQL-Abfrage aus, die eine Reihe von Zeilen (als-Objekte) zurückgibt.
- **Execute** – verwenden Sie diese Methode (und `Query` nicht), wenn Sie keine Zeilen von der SQL-Anweisung (z. b. Insert-, Update-und DELETE-Anweisungen) erwarten.


### <a name="getting-an-object-by-the-primary-key"></a>Ein Objekt wird durch den Primärschlüssel erhalten.

SQLite.NET stellt die Get-Methode zum Abrufen eines einzelnen Objekts basierend auf seinem Primärschlüssel bereit.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Auswählen eines Objekts mithilfe von LINQ

Methoden, die Auflistungen zurückgeben,\<unterstützen IEnumerable T > sodass Sie LINQ verwenden können, um den Inhalt einer Tabelle abzufragen oder zu sortieren. Der folgende Code zeigt ein Beispiel für die Verwendung von LINQ zum Filtern aller Einträge, die mit dem Buchstaben "A" beginnen:

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>Auswählen eines Objekts mithilfe von SQL

Obwohl sqlite.NET den objektbasierten Zugriff auf Ihre Daten bereitstellen kann, müssen Sie möglicherweise eine komplexere Abfrage ausführen, als LINQ zulässt (oder Sie benötigen möglicherweise eine schnellere Leistung). Sie können SQL-Befehle mit der Query-Methode verwenden, wie hier gezeigt:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> Wenn Sie SQL-Anweisungen direkt schreiben, erstellen Sie eine Abhängigkeit von den Namen der Tabellen und Spalten in der Datenbank, die von den Klassen und deren Attributen generiert wurden. Wenn Sie diese Namen im Code ändern, müssen Sie daran denken, manuell geschriebene SQL-Anweisungen zu aktualisieren.

### <a name="deleting-an-object"></a>Löschen eines Objekts

Der Primärschlüssel wird zum Löschen der Zeile verwendet, wie hier gezeigt:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Sie können das `rowcount` überprüfen, um zu bestätigen, wie viele Zeilen betroffen sind (in diesem Fall gelöscht).

## <a name="using-sqlitenet-with-multiple-threads"></a>Verwenden von SQLite.net mit mehreren Threads

SQLite unterstützt drei verschiedene Threading Modi: *Single Thread*, *Multithread*und *serialisiert*. Wenn Sie von mehreren Threads ohne Einschränkungen auf die Datenbank zugreifen möchten, können Sie SQLite so konfigurieren, dass der **serialisierte** Threading Modus verwendet wird. Es ist wichtig, diesen Modus frühzeitig in der Anwendung festzulegen (z. b. am Anfang `OnCreate` der Methode).

Um den Threading Modus zu ändern, `SqliteConnection.SetConfig` wird aufgerufen, der `Mono.Data.Sqlite` sich im-Namespace befindet. Diese Codezeile konfiguriert beispielsweise sqlite für den **serialisierten** Modus:

```csharp
using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
