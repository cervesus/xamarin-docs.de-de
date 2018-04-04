---
title: Mithilfe von ADO.NET
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: a2f7a7a0c282284d7a45fb81c134d300aef5afba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-adonet"></a>Mithilfe von ADO.NET

Xamarin bietet eine integrierte Unterstützung für die SQLite-Datenbank, die unter Android verfügbar und kann mithilfe der vertrauten ADO.NET-ähnliche Syntax verfügbar gemacht werden. Verwenden diese APIs benötigen Sie SQL-Anweisungen zu schreiben, die von SQLite,, z. B. verarbeitet werden `CREATE TABLE`, `INSERT` und `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Zugriff SQLite über ADO.NET verwenden, müssen Sie hinzufügen `System.Data` und `Mono.Data.Sqlite` Verweise auf Ihrem Android-Projekt ein, wie hier gezeigt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Android-Verweise in Visual Studio](using-adonet-images/image7.png "Android verweist, in Visual Studio") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac) 

![Android-Verweise in Visual Studio für Mac](using-adonet-images/image5.png "Android Verweise in Visual Studio für Mac") 

-----


Mit der rechten Maustaste **Verweise > Verweise bearbeiten...**  und dann klicken Sie hier, um die erforderlichen Assemblys auszuwählen.

## <a name="about-monodatasqlite"></a>Informationen zu Mono.Data.Sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection` Klasse, um eine leere Datei zu erstellen und dann instanziiert `SqliteCommand` -Objekten, die wir verwenden können, um SQL-Anweisungen für die Datenbank auszuführen.

**Erstellen eine leere Datenbank** &ndash; rufen die `CreateFile` Methode durch einen gültigen (ie. beschreibbaren) Dateipfad. Sie sollten überprüfen, ob die Datei bereits vorhanden ist, bevor diese Methode aufgerufen, andernfalls eine neue (leere) Datenbank werden, über den oberen Rand die alten Pläne erstellt wird und die Daten in die alte Datei geht dabei verloren.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` Die `dbPath` Variable entsprechend der weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden soll.

**Erstellen von Datenbankverbindungen** &ndash; nach dem Erstellen die SQLite-Datei können Sie ein Verbindungsobjekt für den Zugriff auf die Daten erstellen. Die Verbindung mit einer Verbindungszeichenfolge im Format von erstellt `Data Source=file_path`, wie hier gezeigt:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Wie bereits erwähnt, sollte eine Verbindung über verschiedene Threads nie erneut verwendet werden. Erstellen Sie die Verbindung nach Bedarf, und schließen Sie es, wenn Sie fertig sind, im Zweifelsfall; jedoch sollten diese mehr als erforderlich zu häufig ausführen.

**Erstellen und Ausführen eines Befehls Datenbank** &ndash; Sobald wir eine Verbindung haben können beliebige SQL-Befehle für ihn ausführen. Der folgende Code zeigt eine `CREATE TABLE` ausgeführte Anweisung.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Beim Ausführen von SQL direkt gegen die Datenbank sollten Sie Vorsichtsmaßnahmen ergreifen, den normalen, nicht stellen ungültige Anforderungen, z. B. versuchen, eine Tabelle zu erstellen, die bereits vorhanden ist. Nachverfolgen von der Struktur Ihrer Datenbank, damit Sie die keine verursachen eine `SqliteException` wie z. B. **SQLite Error-Tabelle [Elemente] bereits**.

## <a name="basic-data-access"></a>Lernprogramm zu Data Access

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht wie folgt, wenn auf Android ausgeführt wird:

![Beispiel zu Android ADO.NET](using-adonet-images/image8.png "Android ADO.NET-Beispiel")

Im folgenden Code wird veranschaulicht, wie einfache SQLite-Operationen und zeigt die Ergebnisse als Text im Hauptfenster der Anwendung.

Sie müssen diese Namespaces enthalten:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Das folgende Codebeispiel zeigt eine gesamte Datenbank-Aktivität:

1.  Erstellen die Datenbankdatei
2.  Einfügen von Daten
3.  Abfragen der Daten

Diese Vorgänge in der Regel scheint an mehreren Orten im gesamten Code, z. B. möglicherweise erstellen Sie die Datenbankdatei und die Tabellen beim ersten der Anwendung Start und führen Datenlese- und-Schreibvorgänge in einzelnen Bildschirme in Ihrer app. Im folgenden Beispiel haben in einer einzelnen Methode für dieses Beispiel gruppiert wurden:

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

Da SQLite beliebige SQL-Befehle in die Daten ausgeführt werden kann, können Sie nach Belieben ausführen `CREATE`, `INSERT`, `UPDATE`, `DELETE`, oder `SELECT` Anweisungen, die Ihnen gefällt. Sie können über die SQL-Befehle an der Sqlite-Website von SQLite unterstützt lesen. Die SQL-Anweisungen ausgeführt werden, mithilfe einer der drei Methoden auf eine `SqliteCommand` Objekt:

-   **ExecuteNonQuery** &ndash; in der Regel verwendet, für die Tabelle erstellen oder Daten einfügen. Der Rückgabewert für bestimmte Vorgänge wird die Anzahl der betroffenen Zeilen, andernfalls -1.

-   **ExecuteReader** &ndash; verwendet, wenn eine Auflistung von Zeilen sollen, als zurückgegeben werden eine `SqlDataReader`.

-   **ExecuteScalar** &ndash; Ruft einen einzelnen Wert (z. B. ein Aggregat) ab.


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, und `DELETE` Anweisungen gibt die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL-Anweisungen gibt-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Die folgende Methode zeigt eine `WHERE` -Klausel in der `SELECT` Anweisung.
Da der Code eine vollständige SQL-Anweisung erstellen, ist muss es achten reservierte Zeichen wie z. B. das Anführungszeichen ('), um die Zeichenfolgen mit Escapezeichen.

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

Die `ExecuteReader`-Methode gibt ein `SqliteDataReader`-Objekt zurück. Zusätzlich zu den `Read` Methode, die im Beispiel gezeigten weiteren nützlichen Eigenschaften enthalten:

-   **RowsAffected** &ndash; Anzahl der von der Abfrage betroffenen Zeilen.

-   **HasRows** &ndash; gibt an, ob alle Zeilen zurückgegeben wurden.


### <a name="executescalar"></a>EXECUTESCALAR

Verwenden Sie diese für `SELECT` Anweisungen, die einen einzelnen Wert (z. B. ein Aggregat) zurückgeben.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Die `ExecuteScalar` ist der Rückgabetyp der Methode `object` &ndash; sollten Sie das Ergebnis abhängig von der Datenbankabfrage umwandeln. Das Ergebnis ist möglicherweise eine ganze Zahl zwischen einer `COUNT` Abfrage oder eine Zeichenfolge aus einer einzelnen Spalte `SELECT` Abfrage. Beachten Sie, dass sich dies auf andere unterscheidet `Execute` Methoden, die ein Readerobjekt oder die Anzahl der betroffenen Zeilen zurückgeben.



## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android Daten Rezepte](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
