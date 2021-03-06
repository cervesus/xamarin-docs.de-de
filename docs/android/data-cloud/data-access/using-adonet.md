---
title: Verwenden von ADO.net mit Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: 6592bd6d5cf7b78918fa2d020be723d662625e06
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023757"
---
# <a name="using-adonet-with-android"></a>Verwenden von ADO.net mit Android

Xamarin verfügt über integrierte Unterstützung für die SQLite-Datenbank, die unter Android verfügbar ist und mit vertrauter ADO.NET-Syntax verfügbar gemacht werden kann. Die Verwendung dieser APIs erfordert das Schreiben von SQL-Anweisungen, die von SQLite verarbeitet werden, z. b. `CREATE TABLE`, `INSERT` und `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Wenn Sie Access SQLite über ADO.NET verwenden möchten, müssen Sie Ihrem Android-Projekt `System.Data` und `Mono.Data.Sqlite` Verweise hinzufügen, wie hier gezeigt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Android-Verweise in Visual Studio](using-adonet-images/image7.png "Android-Verweise in Visual Studio") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos) 

![Android-Verweise in Visual Studio für Mac](using-adonet-images/image5.png "Android-Verweise in Visual Studio für Mac") 

-----

Klicken Sie mit der rechten Maustaste auf **Verweise > Verweise bearbeiten...** , und wählen Sie dann die erforderlichen Assemblys.

## <a name="about-monodatasqlite"></a>Informationen zu Mono. Data. sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection`-Klasse, um eine leere Datenbankdatei zu erstellen und dann `SqliteCommand` Objekte zu instanziieren, die wir zum Ausführen von SQL-Anweisungen für die Datenbank verwenden können.

Beim **Erstellen einer leeren Datenbank** &ndash; die `CreateFile`-Methode mit einem gültigen (beschreibbaren) Dateipfad aufgerufen werden. Sie sollten überprüfen, ob die Datei bereits vorhanden ist, bevor Sie diese Methode aufrufen. andernfalls wird eine neue (leere) Datenbank oberhalb der alten erstellt, und die Daten in der alten Datei gehen verloren.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` die `dbPath` Variable gemäß den zuvor in diesem Dokument beschriebenen Regeln bestimmt werden soll.

Erstellen **einer Datenbankverbindung** &ndash; nachdem die SQLite-Datenbankdatei erstellt wurde, können Sie ein Verbindungs Objekt erstellen, um auf die Daten zuzugreifen. Die Verbindung wird mit einer Verbindungs Zeichenfolge erstellt, die das Format `Data Source=file_path`annimmt, wie hier gezeigt:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Wie bereits erwähnt, sollte eine Verbindung nie in verschiedenen Threads wieder verwendet werden. Erstellen Sie im Zweifelsfall die Verbindung nach Bedarf, und schließen Sie Sie, wenn Sie fertig sind. Beachten Sie jedoch, dass dies häufiger als erforderlich ist.

Wenn Sie **einen Datenbankbefehl erstellen und ausführen** &ndash; nachdem wir eine Verbindung hergestellt haben, können Sie beliebige SQL-Befehle für ihn ausführen. Der folgende Code zeigt eine `CREATE TABLE` Anweisung, die ausgeführt wird.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Beim direkten Ausführen von SQL für die Datenbank sollten Sie die normalen Vorsichtsmaßnahmen treffen, um ungültige Anforderungen zu stellen, z. b. den Versuch, eine Tabelle zu erstellen, die bereits vorhanden ist. Behalten Sie die Datenbankstruktur bei, sodass keine `SqliteException` wie z. b. **SQLite-Fehler Tabelle [Items] bereits vorhanden**ist.

## <a name="basic-data-access"></a>Grundlegender Datenzugriff

Der *DataAccess_Basic* -Beispielcode für dieses Dokument sieht wie folgt aus, wenn Sie unter Android ausgeführt werden:

![Android ADO.NET-Beispiel](using-adonet-images/image8.png "Android ADO.NET-Beispiel")

Der folgende Code veranschaulicht, wie einfache SQLite-Vorgänge durchgeführt und die Ergebnisse in als Text im Hauptfenster der Anwendung angezeigt werden.

Sie müssen diese Namespaces einschließen:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Das folgende Codebeispiel zeigt eine gesamte Daten Bank Interaktion:

1. Erstellen der Datenbankdatei
2. Einfügen von Daten
3. Abfragen der Daten

Diese Vorgänge werden in der Regel an mehreren Stellen im gesamten Code angezeigt. Sie können z. b. die Datenbankdatei und die Tabellen erstellen, wenn Ihre Anwendung zum ersten Mal gestartet wird, und Lese-und Schreibvorgänge für Daten in den einzelnen Bildschirmen der Im folgenden Beispiel wurden in einer einzigen Methode für dieses Beispiel gruppiert:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Komplexere Abfragen

Da SQLite das Ausführen beliebiger SQL-Befehle für die Daten ermöglicht, können Sie beliebige `CREATE`, `INSERT`, `UPDATE`, `DELETE`oder `SELECT` Anweisungen ausführen. Informationen zu den SQL-Befehlen, die von SQLite unterstützt werden, finden Sie auf der SQLite-Website. Die SQL-Anweisungen werden mit einer von drei Methoden für ein `SqliteCommand` Objekt ausgeführt:

- **ExecuteNonQuery** &ndash; in der Regel zur Tabellenerstellung oder zum Einfügen von Daten verwendet. Der Rückgabewert für einige Vorgänge ist die Anzahl der betroffenen Zeilen, andernfalls "-1".

- **ExecuteReader** &ndash; verwendet, wenn eine Auflistung von Zeilen als `SqlDataReader`zurückgegeben werden soll.

- **ExecuteScalar** &ndash; Ruft einen einzelnen Wert (z. b. ein Aggregat) ab.

### <a name="executenonquery"></a>EXECUTENONQUERY

die Anweisungen `INSERT`, `UPDATE`und `DELETE` geben die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL-Anweisungen geben-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Die folgende Methode zeigt eine `WHERE`-Klausel in der `SELECT`-Anweisung.
Da der Code eine komplette SQL-Anweisung erstellen soll, muss er darauf achten, dass reservierte Zeichen wie das Anführungszeichen (') um Zeichen folgen entfernt werden.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

Die `ExecuteReader`-Methode gibt ein `SqliteDataReader`-Objekt zurück. Zusätzlich zur `Read`-Methode, die im Beispiel gezeigt wird, umfassen weitere nützliche Eigenschaften:

- Zeilen **&ndash; Anzahl** der Zeilen, die von der Abfrage betroffen sind.

- **HasRows** &ndash;, ob Zeilen zurückgegeben wurden.

### <a name="executescalar"></a>EXECUTESCALAR

Verwenden Sie dies für `SELECT`-Anweisungen, die einen einzelnen Wert zurückgeben (z. b. ein Aggregat).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Der Rückgabetyp der `ExecuteScalar` Methode ist `object` &ndash; Sie sollten das Ergebnis abhängig von der Datenbankabfrage umwandeln. Das Ergebnis kann eine ganze Zahl aus einer `COUNT` Abfrage oder eine Zeichenfolge aus einer einzelnen Spalte `SELECT` Abfrage sein. Beachten Sie, dass dies sich von anderen `Execute` Methoden unterscheidet, die ein Reader-Objekt oder eine Anzahl der betroffenen Zeilen zurückgeben.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
