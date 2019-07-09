---
title: Verwendung von ADO.NET mit Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 10b5a1696b0416bfda115627f7c7b8c2fbd20fcb
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649582"
---
# <a name="using-adonet-with-android"></a>Verwendung von ADO.NET mit Android

Xamarin verfügt über integrierte Unterstützung für die SQLite-Datenbank, die auf Android verfügbar und kann mithilfe der vertrauten ADO.NET-ähnliche Syntax verfügbar gemacht werden. Mithilfe dieser APIs müssen Sie SQL-Anweisungen zu schreiben, die vom SQLite,, z. B. verarbeitet werden `CREATE TABLE`, `INSERT` und `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Zugriff SQLite über ADO.NET verwenden, Sie hinzufügen müssen, `System.Data` und `Mono.Data.Sqlite` Verweise auf Ihrem Android-Projekt, wie hier gezeigt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Android-Referenzen in Visual Studio](using-adonet-images/image7.png "Android verweist, in Visual Studio") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos) 

![Android-Referenzen in Visual Studio für Mac](using-adonet-images/image5.png "Android-Verweise in Visual Studio für Mac") 

-----


Mit der rechten Maustaste **Verweise > Verweise bearbeiten...**  und klicken Sie, um die erforderlichen Assemblys auszuwählen.

## <a name="about-monodatasqlite"></a>Informationen zu Mono.Data.Sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection` Klasse, um eine leere Datei erstellen und dann zum Instanziieren `SqliteCommand` Objekten, die wir zum Ausführen von SQL-Anweisungen für die Datenbank verwenden können.

**Erstellen eine leere Datenbank** &ndash; rufen Sie die `CreateFile` Methode mit einem gültigen Dateipfad für die (d. h. beschreibbaren). Sie sollten überprüfen, ob die Datei vor dem Aufrufen dieser Methode bereits vorhanden ist, andernfalls eine neue (leere) wird, oberhalb des alten erstellt und die Daten in die alte Datei gehen verloren.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` Die `dbPath` Variablen entsprechend die weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden soll.

**Erstellen einer Datenbankverbindung** &ndash; nach der Erstellung der SQLite-Datenbank-Datei können Sie ein Verbindungsobjekt für den Zugriff auf die Daten erstellen. Die Verbindung wird erstellt, mit einer Verbindungszeichenfolge im Format der `Data Source=file_path`wie hier gezeigt:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Wie bereits erwähnt, sollte eine Verbindung über andere Threads nicht erneut verwendet werden. Erstellen Sie die Verbindung nach Bedarf, und schließen Sie sie an, wenn Sie fertig sind, im Zweifelsfall; aber Achten Sie darauf, dass Sie dieses Thema häufig als erforderlich zu erledigen.

**Erstellen und Ausführen eines Befehls für die Datenbank** &ndash; Sobald wir eine Verbindung haben können wir beliebige SQL-Befehle für sie ausführen. Der folgende Code zeigt eine `CREATE TABLE` -Anweisung ausgeführt wird.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Bei Ausführung von SQL direkt in der Datenbank sollten Sie Vorsichtsmaßnahmen ergreifen, die normalen, nicht, wie z. B. versuchen, eine Tabelle zu erstellen, die bereits vorhanden ist, ungültige Anforderungen zu stellen. Behalten Sie den Überblick darüber der Struktur der Datenbank, damit Sie keine verursachen eine `SqliteException` wie z. B. **SQLite Fehlertabelle [Elemente] ist bereits vorhanden**.

## <a name="basic-data-access"></a>Grundlagen des Datenzugriffs

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht folgendermaßen aus, bei der Ausführung unter Android:

![Beispiel zu Android ADO.NET](using-adonet-images/image8.png "Android ADO.NET-Beispiel")

Der folgende Code veranschaulicht, wie einfache SQLite-Vorgänge ausführen und zeigt die Ergebnisse als Text im Hauptfenster der Anwendung.

Sie müssen diese Namespaces enthalten:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Das folgende Codebeispiel zeigt eine ganze Datenbankinteraktion:

1.  Erstellen der Datenbankdatei
2.  Einfügen von Daten
3.  Abfragen der Daten

Diese Vorgänge werden in der Regel an mehreren Orten im gesamten Code, zum Beispiel erscheinen können Sie die Datenbankdatei und die Tabellen beim ersten der Anwendung Start zu erstellen und die Lese- und Schreibvorgänge in die einzelnen Bildschirme in Ihrer app ausführen. Im folgenden Beispiel haben in eine einzelne Methode für dieses Beispiel gruppiert wurden:

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

Da SQLite beliebige SQL-Befehle für die Daten ausgeführt werden kann, können Sie ausführen, was auch immer `CREATE`, `INSERT`, `UPDATE`, `DELETE`, oder `SELECT` Anweisungen, die Ihnen gefällt. Sie können über die SQL-Befehle, die von SQLite unterstützt werden, auf der SQLite-Website lesen. Die SQL-Anweisungen ausgeführt werden, mithilfe einer der drei Methoden für eine `SqliteCommand` Objekt:

-   **ExecuteNonQuery** &ndash; in der Regel verwendet, für die Tabelle erstellen oder Daten einfügen. Der Rückgabewert bei einigen Vorgängen ist die Anzahl der betroffenen Zeilen, andernfalls -1 ist.

-   **"ExecuteReader"** &ndash; verwendet, wenn eine Sammlung von Zeilen sollen, als zurückgegeben werden eine `SqlDataReader`.

-   **"ExecuteScalar"** &ndash; Ruft einen einzelnen Wert (z. B. ein Aggregat) ab.


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, und `DELETE` Anweisungen werden die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL­Anweisungen gibt-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>"EXECUTEREADER"

Die folgende Methode zeigt ein `WHERE` -Klausel in der `SELECT` Anweisung.
Da der Code eine vollständige SQL-Anweisung erstellen, ist muss es peinlich escape von reservierten Zeichen wie z. B. das Anführungszeichen ('), um Zeichenfolgen.

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

Die `ExecuteReader`-Methode gibt ein `SqliteDataReader`-Objekt zurück. Zusätzlich zu den `Read` -Methode in diesem Beispiel andere nützliche Eigenschaften enthalten:

-   **RowsAffected** &ndash; Anzahl der von der Abfrage betroffenen Zeilen.

-   **HasRows** &ndash; gibt an, ob alle Zeilen zurückgegeben wurden.


### <a name="executescalar"></a>"EXECUTESCALAR"

Verwenden Sie diese für `SELECT` Anweisungen, die einen einzelnen Wert (z. B. ein Aggregat) zurückgeben.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Die `ExecuteScalar` ist der Rückgabetyp der Methode `object` &ndash; sollten Sie das Ergebnis abhängig von der Datenbankabfrage umwandeln. Das Ergebnis ist möglicherweise eine ganze Zahl zwischen einer `COUNT` Abfrage oder eine Zeichenfolge aus einer einzelnen Spalte `SELECT` Abfrage. Beachten Sie, dass dies auf anderen `Execute` Methoden, die ein Reader-Objekt oder die Anzahl der betroffenen Zeilen zurückgegeben werden.



## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
