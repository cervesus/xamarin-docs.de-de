---
title: <legacyBold>System.Data</legacyBold>
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b141dfac49e2cfa2dc80b7c0e4ca3a93968590a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="systemdata"></a><legacyBold>System.Data</legacyBold>

Xamarin.iOS 8.10 bietet Unterstützung für ["System.Data"](https://developer.xamarin.com/api/namespace/System.Data/), einschließlich der `Mono.Data.Sqlite.dll` ADO.NET-Anbieter. Die Unterstützung umfasst das Hinzufügen der folgenden [Assemblys](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>Beispiel

Das folgende Programm erstellt eine Datenbank in `Documents/mydb.db3`, und wenn die Datenbank zuvor nicht vorhanden ist, wird mit Beispieldaten gefüllt. Die Datenbank dann abgefragt, ohne dass die Ausgabe geschrieben `stderr`.

### <a name="add-references"></a>Fügen Sie Verweise hinzu

Zunächst mit der rechten Maustaste auf die **Verweise** Knoten, und wählen Sie **Verweise bearbeiten...**  wählen Sie dann `System.Data` und `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Hinzufügen von neuen verweisen")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Beispielcode

Der folgende Code zeigt ein einfaches Beispiel für eine Tabelle erstellen und Einfügen von Zeilen, die eingebettete SQL-Befehle verwenden:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Wie im Codebeispiel oben erwähnt, ist es nicht üblich, Zeichenfolgen in SQL-Befehlen einzubetten, da er den Code anfällig für wird [SQL Injection](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Verwenden die Befehlsparameter

Der folgende Code zeigt, wie Befehlsparameter eingegebenen Text problemlos einfügen in die Datenbank (selbst wenn der Text SQL-Sonderzeichen wie z. B. einzelne Apostroph enthält) mit:

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>Fehlende Funktionalität

Beide **"System.Data"** und **Mono.Data.Sqlite** fehlen einige Funktionen.

<a name="System.Data" />

### <a name="systemdata"></a><legacyBold>System.Data</legacyBold>

Funktionalität fehlt in der **"System.Data.dll"** besteht aus:

-  Alles andere erfordern [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (z. B.  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  XML-Konfigurationsdatei unterstützen (z. B.  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (hängt mit Unterstützung für XML-Config-Dateien)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  Die `System.EnterpriseServices.dll` Abhängigkeit wurde *entfernt* aus `System.Data.dll` , wodurch das Entfernen der [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) Methode.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

In der Zwischenzeit können **Mono.Data.Sqlite.dll** ist keine quellcodeänderungen, aber stattdessen möglicherweise Host mit der Anzahl von *Runtime* seit Probleme `Mono.Data.Sqlite.dll` SQLite 3.5 bindet. iOS 8, wird in der Zwischenzeit mit SQLite 3.8.5 geliefert. Einige Dinge haben genügt es, dass zwischen den beiden Versionen geändert.

Ältere Version von iOS, die mit den folgenden Versionen von SQLite ausgeliefert werden:

- **iOS 7** -Version 3.7.13.
- **iOS 6** -Version 3.7.13.
- **Ios5** -Version 3.7.7.
- **iOS 4** -Version 3.6.22.

Am häufigsten auftretenden Probleme angezeigt, in Bezug auf Schemas Datenbankabfragen, z. B. zur Laufzeit die Spalten z. B. für eine bestimmte Tabelle vorhanden bestimmen `Mono.Data.Sqlite.SqliteConnection.GetSchema` (überschreiben [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) und `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` () Überschreiben von [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). Kurz gesagt, es scheint, dass etwas mit [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) wahrscheinlich nicht ordnungsgemäß ausgeführt wird.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung

Die Datenbindung wird zu diesem Zeitpunkt nicht unterstützt.

