---
title: Verwenden von ADO.net mit xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie den ADO.net als Methode für den Zugriff auf sqlite in einer xamarin. IOS-Anwendung verwenden. Hier werden Assemblyverweise, Mono. Data. sqlite und das basicdataaccess-Beispiel erläutert.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d5830fcc4eab2feb5002253a519d72099d6bcdde
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929986"
---
# <a name="using-adonet-with-xamarinios"></a>Verwenden von ADO.net mit xamarin. IOS

Xamarin verfügt über integrierte Unterstützung für die SQLite-Datenbank, die unter IOS verfügbar ist und mit vertrauter ADO.NET-Syntax verfügbar gemacht wird. Die Verwendung dieser APIs erfordert das Schreiben von SQL-Anweisungen, die von SQLite verarbeitet werden, z `CREATE TABLE` `INSERT` . b.-und- `SELECT` Anweisungen.

## <a name="assembly-references"></a>Assemblyverweise

Wenn Sie Access SQLite über ADO.NET verwenden möchten, müssen Sie `System.Data` und `Mono.Data.Sqlite` Verweise auf Ihr IOS-Projekt hinzufügen, wie hier gezeigt (Beispiele in Visual Studio für Mac und Visual Studio):

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

 ![Assemblyverweise in Visual Studio für Mac](using-adonet-images/image4.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  ![Assemblyverweise in Visual Studio](using-adonet-images/image6.png)

-----

Klicken Sie mit der rechten Maustaste auf **Verweise > Verweise bearbeiten...** , und wählen Sie dann die erforderlichen Assemblys.

## <a name="about-monodatasqlite"></a>Informationen zu Mono. Data. sqlite

Wir verwenden die `Mono.Data.Sqlite.SqliteConnection` -Klasse zum Erstellen einer leeren Datenbankdatei und zum Instanziieren von `SqliteCommand` Objekten, die zum Ausführen von SQL-Anweisungen für die Datenbank verwendet werden können.

1. **Erstellen einer leeren Datenbank** : Ruft die- `CreateFile` Methode mit einem gültigen (IE. beschreibbaren) Dateipfad auf. Sie sollten überprüfen, ob die Datei bereits vorhanden ist, bevor Sie diese Methode aufrufen. andernfalls wird eine neue (leere) Datenbank oberhalb der alten erstellt, und die Daten in der alten Datei gehen verloren:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > Die `dbPath` Variable sollte gemäß den zuvor in diesem Dokument beschriebenen Regeln bestimmt werden.

2. **Erstellen einer Datenbankverbindung** : Nachdem die SQLite-Datenbankdatei erstellt wurde, können Sie ein Verbindungs Objekt erstellen, um auf die Daten zuzugreifen. Die Verbindung wird mit einer Verbindungs Zeichenfolge erstellt, die das Format `Data Source=file_path` hat, wie hier gezeigt:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Wie bereits erwähnt, sollte eine Verbindung nie in verschiedenen Threads wieder verwendet werden. Erstellen Sie im Zweifelsfall die Verbindung nach Bedarf, und schließen Sie Sie, wenn Sie fertig sind. Beachten Sie jedoch, dass dies häufiger als erforderlich ist.

3. **Erstellen und Ausführen eines Daten Bank Befehls** : Sobald wir eine Verbindung haben, können wir beliebige SQL-Befehle für ihn ausführen. Der folgende Code zeigt eine CREATE TABLE Anweisung, die ausgeführt wird.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Beim direkten Ausführen von SQL für die Datenbank sollten Sie die normalen Vorsichtsmaßnahmen treffen, um ungültige Anforderungen zu stellen, z. b. den Versuch, eine Tabelle zu erstellen, die bereits vorhanden ist. Behalten Sie die Nachverfolgung der Struktur Ihrer Datenbank bei, sodass keine sqliteexception ausgelöst wird, z. b. "SQLite-Fehler Tabelle [Items] bereits vorhanden".

## <a name="basic-data-access"></a>Grundlegender Datenzugriff

Der *DataAccess_Basic* Beispielcode für dieses Dokument sieht wie folgt aus, wenn er unter IOS ausgeführt wird:

 ![IOS ADO.NET Beispiel](using-adonet-images/image9.png)

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

Da SQLite das Ausführen beliebiger SQL-Befehle für die Daten ermöglicht, können Sie beliebige Anweisungen zum Erstellen, einfügen, aktualisieren, löschen oder auswählen ausführen. Informationen zu den SQL-Befehlen, die von SQLite unterstützt werden, finden Sie auf der SQLite-Website. Die SQL-Anweisungen werden mit einer von drei Methoden auf einem sqlitecommand-Objekt ausgeführt:

- **ExecuteNonQuery** – wird in der Regel zur Tabellenerstellung oder zum Einfügen von Daten verwendet. Der Rückgabewert für einige Vorgänge ist die Anzahl der betroffenen Zeilen, andernfalls "-1".
- **ExecuteReader** – wird verwendet, wenn eine Auflistung von Zeilen als zurückgegeben werden soll `SqlDataReader` .
- **ExecuteScalar** – Ruft einen einzelnen Wert (z. b. ein Aggregat) ab.

### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT-, Update-und DELETE-Anweisungen geben die Anzahl der betroffenen Zeilen zurück. Alle anderen SQL-Anweisungen geben-1 zurück.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Die folgende Methode zeigt eine WHERE-Klausel in der SELECT-Anweisung an. Da der Code eine komplette SQL-Anweisung erstellen soll, muss er darauf achten, dass reservierte Zeichen wie das Anführungszeichen (') um Zeichen folgen entfernt werden.

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

Die ExecuteReader-Methode gibt ein sqlitedatareader-Objekt zurück. Neben der im Beispiel gezeigten Read-Methode umfassen andere nützliche Eigenschaften:

- **Rowsafffiziert** – Anzahl der von der Abfrage betroffenen Zeilen.
- **HasRows** – gibt an, ob Zeilen zurückgegeben wurden.

### <a name="executescalar"></a>EXECUTESCALAR

Verwenden Sie diese Option für SELECT-Anweisungen, die einen einzelnen Wert zurückgeben (z. b. ein Aggregat).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

Der `ExecuteScalar` Rückgabetyp der Methode ist `object` – Sie sollten das Ergebnis in Abhängigkeit von der Datenbankabfrage umwandeln. Das Ergebnis kann eine ganze Zahl aus einer count-Abfrage oder eine Zeichenfolge aus einer einzelnen Spalte SELECT-Abfrage sein. Beachten Sie, dass sich dies von anderen Execute-Methoden unterscheidet, die ein Reader-Objekt oder eine Anzahl der betroffenen Zeilen zurückgeben.

## <a name="microsoftdatasqlite"></a>Microsoft.Data.Sqlite

Es gibt eine andere Bibliothek `Microsoft.Data.Sqlite` , die [von nuget installiert](https://www.nuget.org/packages/Microsoft.Data.Sqlite)werden kann, die funktionell äquivalent zu ist `Mono.Data.Sqlite` und die gleichen Abfrage Typen zulässt.

Es gibt einen [Vergleich zwischen den beiden Bibliotheken](https://docs.microsoft.com/dotnet/standard/data/sqlite/compare) und einigen [xamarin-spezifischen Details](https://docs.microsoft.com/dotnet/standard/data/sqlite/xamarin). Besonders wichtig für xamarin. IOS-apps müssen Sie einen Initialisierungs aufzurufen einschließen:

```csharp
// required for Xamarin.iOS
SQLitePCL.Batteries_V2.Init();
```

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [IOS-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
