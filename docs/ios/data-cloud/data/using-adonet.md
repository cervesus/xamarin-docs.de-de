---
title: Verwendung von ADO.NET mit Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie das ADO.NET als eine Methode zu verwenden, um die SQLite in einer Xamarin.iOS-Anwendung zugreifen. Es wird erläutert, Assemblyverweise, Mono.Data.Sqlite und das BasicDataAccess-Beispiel.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 9314e1b69df4a5965dfd045d0b4ca3e44f1b9de6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122295"
---
# <a name="using-adonet-with-xamarinios"></a>Verwendung von ADO.NET mit Xamarin.iOS

Xamarin verfügt über integrierte Unterstützung für die SQLite-Datenbank, die unter iOS, verfügbar gemacht werden, mithilfe der vertrauten ADO.NET-ähnliche Syntax verfügbar ist. Mithilfe dieser APIs müssen Sie SQL-Anweisungen zu schreiben, die vom SQLite,, z. B. verarbeitet werden `CREATE TABLE`, `INSERT` und `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Zugriff SQLite über ADO.NET verwenden, Sie hinzufügen müssen, `System.Data` und `Mono.Data.Sqlite` Verweise auf Ihrem iOS-Projekt, wie hier gezeigt (für die Beispiele in Visual Studio für Mac und Visual Studio):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 ![](using-adonet-images/image4.png "Assembly-Verweise in Visual Studio für Mac")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  ![](using-adonet-images/image6.png "Assembly-Verweise in Visual Studio")

-----

Mit der rechten Maustaste **Verweise > Verweise bearbeiten...**  und klicken Sie, um die erforderlichen Assemblys auszuwählen.

## <a name="about-monodatasqlite"></a>Informationen zu Mono.Data.Sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection` Klasse, um eine leere Datei erstellen und dann zum Instanziieren `SqliteCommand` Objekten, die wir zum Ausführen von SQL-Anweisungen für die Datenbank verwenden können.


1. **Erstellen eine leere Datenbank** -Aufrufen die `CreateFile` Methode mit einer gültigen (ie. beschreibbaren) Dateipfad. Sie sollten überprüfen, ob die Datei vor dem Aufrufen dieser Methode bereits vorhanden ist, andernfalls eine neue (leere) wird, oberhalb des alten erstellt und die Daten in die alte Datei gehen verloren:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > Die `dbPath` Variablen entsprechend die weiter oben in diesem Dokument beschriebenen Regeln bestimmt werden soll.

2. **Erstellen einer Datenbankverbindung** – nach der Erstellung der SQLite-Datenbank-Datei können Sie ein Verbindungsobjekt für den Zugriff auf die Daten erstellen. Die Verbindung wird erstellt, mit einer Verbindungszeichenfolge im Format der `Data Source=file_path`wie hier gezeigt:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Wie bereits erwähnt, sollte eine Verbindung über andere Threads nicht erneut verwendet werden. Erstellen Sie die Verbindung nach Bedarf, und schließen Sie sie an, wenn Sie fertig sind, im Zweifelsfall; aber Achten Sie darauf, dass Sie dieses Thema häufig als erforderlich zu erledigen.
    
3. **Erstellen und Ausführen eines Befehls für die Datenbank** – Sobald wir haben es sich um eine Verbindung mit dem wir können beliebige SQL-Befehle für sie ausführen. Der folgende Code zeigt eine CREATE TABLE-Anweisung ausgeführt wird.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Bei Ausführung von SQL direkt in der Datenbank sollten Sie Vorsichtsmaßnahmen ergreifen, die normalen, nicht, wie z. B. versuchen, eine Tabelle zu erstellen, die bereits vorhanden ist, ungültige Anforderungen zu stellen. Verwalten von der Struktur der Datenbank, damit Sie kein SqliteException verursachen, wie z. B. "SQLite Fehlertabelle [Elemente] ist bereits vorhanden".

## <a name="basic-data-access"></a>Grundlagen des Datenzugriffs

Die *DataAccess_Basic* Beispielcode für dieses Dokument sieht folgendermaßen aus, bei der Ausführung unter iOS:

 ![](using-adonet-images/image9.png "iOS-ADO.NET-Beispiel")

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

Da SQLite beliebige SQL-Befehle für die Daten ausgeführt werden kann, können Sie ausführen, was zu erstellen, einfügen, aktualisieren, löschen oder SELECT-Anweisungen, die Ihnen gefällt. Sie können über die SQL-Befehle, die von SQLite unterstützt werden, auf der Sqlite-Website lesen. Die SQL-Anweisungen werden mithilfe von einer der drei Methoden für ein Objekt SqliteCommand ausgeführt:

-  **ExecuteNonQuery** : in der Regel werden verwendet, für die Tabelle erstellen oder Daten einfügen. Der Rückgabewert bei einigen Vorgängen ist die Anzahl der betroffenen Zeilen, andernfalls -1 ist.
-  **"ExecuteReader"** – verwendet, wenn eine Sammlung von Zeilen sollen, als zurückgegeben werden eine `SqlDataReader` .
-  **"ExecuteScalar"** – Ruft einen einzelnen Wert (z. B. ein Aggregat) ab.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT-, Update- und DELETE-Anweisungen gibt die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL­Anweisungen gibt-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>"EXECUTEREADER"

Das folgende Beispiel zeigt einer WHERE-Klausel in der SELECT-Anweisung. Da der Code eine vollständige SQL-Anweisung erstellen, ist muss es peinlich escape von reservierten Zeichen wie z. B. das Anführungszeichen ('), um Zeichenfolgen.

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

ExecuteReader-Methode gibt ein SqliteDataReader-Objekt zurück. Neben der Read-Methode, die im Beispiel gezeigt enthalten andere nützliche Eigenschaften:

-  **RowsAffected** – Anzahl der von der Abfrage betroffenen Zeilen.
-  **HasRows** : Legt fest, ob alle Zeilen zurückgegeben wurden.


### <a name="executescalar"></a>"EXECUTESCALAR"

Verwenden Sie diese für SELECT-Anweisungen, die einen einzelnen Wert (z. B. ein Aggregat) zurückgeben.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Die `ExecuteScalar` ist der Rückgabetyp der Methode `object` – sollten Sie das Ergebnis abhängig von der Datenbankabfrage umwandeln. Das Ergebnis kann es sich um eine ganze Zahl zwischen einem COUNT-Abfrage oder eine Zeichenfolge mit einer einzelnen Spalte SELECT-Abfrage sein. Beachten Sie, dass es sich von anderen Execute-Methoden handelt, die ein Reader-Objekt oder die Anzahl der betroffenen Zeilen zurückgeben.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS-Daten-Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
