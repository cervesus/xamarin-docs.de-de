---
title: Mithilfe von ADO.NET
ms.topic: article
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 45d7b3e844501f8aa97d75f2fc8af27a961290b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="using-adonet"></a>Mithilfe von ADO.NET

Xamarin bietet eine integrierte Unterstützung für die SQLite-Datenbank, die bei iOS kann mit vertrauten ADO.NET-ähnliche Syntax verfügbar gemacht wurden, verfügbar ist. Verwenden diese APIs benötigen Sie SQL-Anweisungen zu schreiben, die von SQLite,, z. B. verarbeitet werden `CREATE TABLE`, `INSERT` und `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Zugriff SQLite über ADO.NET verwenden, müssen Sie hinzufügen `System.Data` und `Mono.Data.Sqlite` Verweise auf Ihrem iOS-Projekt, wie hier gezeigt (für in Visual Studio für Mac und Visual Studio-Beispiele):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Assemblyverweise in Visual Studio für Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Assemblyverweise in Visual Studio")

-----

Mit der rechten Maustaste **Verweise > Verweise bearbeiten...**  und dann klicken Sie hier, um die erforderlichen Assemblys auszuwählen.

## <a name="about-monodatasqlite"></a>Informationen zu Mono.Data.Sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection` Klasse, um eine leere Datei zu erstellen und dann instanziiert `SqliteCommand` -Objekten, die wir verwenden können, um SQL-Anweisungen für die Datenbank auszuführen.


1. **Erstellen eine leere Datenbank** -rufen Sie die `CreateFile` Methode durch einen gültigen (ie. beschreibbaren) Dateipfad. Sie sollten überprüfen, ob die Datei bereits vorhanden ist, bevor diese Methode aufgerufen, andernfalls eine neue (leere) Datenbank werden, über den oberen Rand die alten Pläne erstellt wird und die Daten in die alte Datei verloren:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
> **Hinweis:** DbPath Variablen entsprechend der weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden soll.

2. **Erstellen von Datenbankverbindungen** – nach dem Erstellen die SQLite-Datei können Sie ein Verbindungsobjekt für den Zugriff auf die Daten erstellen. Die Verbindung mit einer Verbindungszeichenfolge im Format von erstellt `Data Source=file_path`, wie hier gezeigt:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Wie bereits erwähnt, sollte eine Verbindung über verschiedene Threads nie erneut verwendet werden. Erstellen Sie die Verbindung nach Bedarf, und schließen Sie es, wenn Sie fertig sind, im Zweifelsfall; jedoch sollten diese mehr als erforderlich zu häufig ausführen.
    
3. **Erstellen und Ausführen eines Befehls Datenbank** – Sobald wir haben eine Verbindung mit dem beliebige SQL-Befehle für sie ausführen können. Der folgende Code zeigt eine CREATE TABLE-Anweisung ausgeführt wird.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Beim Ausführen von SQL direkt gegen die Datenbank sollten Sie Vorsichtsmaßnahmen ergreifen, den normalen, nicht stellen ungültige Anforderungen, z. B. versuchen, eine Tabelle zu erstellen, die bereits vorhanden ist. Nachverfolgen von der Struktur Ihrer Datenbank, damit Sie kein SqliteException verursachen, wie z. B. "SQLite Error-Tabelle [Elemente] ist bereits vorhanden".

## <a name="basic-data-access"></a>Lernprogramm zu Data Access

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht wie folgt bei Ausführung auf iOS:

 ![](using-adonet-images/image9.png "iOS ADO.NET-Beispiel")

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

SQLite beliebige SQL-Befehle in die Daten ausgeführt werden kann, können Sie ausgeführt werden, da nach Belieben erstellen, einfügen, aktualisieren, löschen oder SELECT-Anweisungen, die Ihnen gefallen. Sie können über die SQL-Befehle an der Sqlite-Website von SQLite unterstützt lesen. Die SQL-Anweisungen ausgeführt werden, mithilfe einer der drei Methoden für ein Objekt SqliteCommand:

-  **ExecuteNonQuery** – in der Regel verwendet, für die Tabelle erstellen oder Daten einfügen. Der Rückgabewert für bestimmte Vorgänge wird die Anzahl der betroffenen Zeilen, andernfalls -1.
-  **ExecuteReader** – verwendet, wenn eine Auflistung von Zeilen sollen, als zurückgegeben werden eine `SqlDataReader` .
-  **ExecuteScalar** – Ruft einen einzelnen Wert (z. B. ein Aggregat) ab.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT-, Update- und DELETE-Anweisungen werden die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL-Anweisungen gibt-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Die folgende Methode zeigt eine WHERE-Klausel in der SELECT-Anweisung. Da der Code eine vollständige SQL-Anweisung erstellen, ist muss es achten reservierte Zeichen wie z. B. das Anführungszeichen ('), um die Zeichenfolgen mit Escapezeichen.

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

Die ExecuteReader-Methode gibt ein SqliteDataReader-Objekt. Neben der Read-Methode, die im Beispiel gezeigt enthalten weiteren nützlichen Eigenschaften auf:

-  **RowsAffected** – Anzahl der von der Abfrage betroffenen Zeilen.
-  **HasRows** – ob alle Zeilen zurückgegeben wurden.


### <a name="executescalar"></a>EXECUTESCALAR

Verwenden Sie diese für SELECT-Anweisungen, die einen einzelnen Wert (z. B. ein Aggregat) zurückgeben.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Die `ExecuteScalar` ist der Rückgabetyp der Methode `object` – sollten Sie das Ergebnis abhängig von der Datenbankabfrage umwandeln. Das Ergebnis konnte eine ganze Zahl zwischen COUNT-Abfrage oder eine Zeichenfolge aus einer einzelnen Spalte SELECT-Abfrage sein. Beachten Sie, dass sich dies auf andere Execute-Methoden unterscheidet, die ein Readerobjekt oder die Anzahl der betroffenen Zeilen zurückgeben.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Daten Rezepte](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
