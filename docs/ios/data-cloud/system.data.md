---
title: System. Data in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie mithilfe von "System. Data" und "Mono. Data. sqlite. dll" auf SQLite-Daten in einer xamarin. IOS-Anwendung zugreifen.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 11/25/2015
ms.openlocfilehash: 44d2e468efeacea919af2d243588d0da6d72945d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766537"
---
# <a name="systemdata-in-xamarinios"></a>System. Data in xamarin. IOS

Xamarin. IOS 8,10 bietet Unterstützung für [System. Data](xref:System.Data), einschließlich `Mono.Data.Sqlite.dll` des ADO.NET-Anbieters. Die Unterstützung umfasst das Hinzufügen [der folgenden](~/cross-platform/internals/available-assemblies.md)Assemblys:

- `System.Data.dll`
- `System.Data.Service.Client.dll`
- `System.Transactions.dll`
- `Mono.Data.Tds.dll`
- `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Beispiel

Das folgende Programm erstellt eine Datenbank in `Documents/mydb.db3`, und wenn die Datenbank nicht bereits vorhanden ist, wird Sie mit Beispiel Daten aufgefüllt. Die Datenbank wird dann abgefragt, und die Ausgabe wird in `stderr`geschrieben.

### <a name="add-references"></a>Verweise hinzufügen

Klicken Sie zunächst mit der rechten Maustaste auf den Knoten **Verweise** , und wählen Sie **Verweise bearbeiten...** aus, `System.Data` und `Mono.Data.Sqlite`wählen Sie dann aus:

[![](system.data-images/edit-references-sml.png "Neue Verweise werden hinzugefügt")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Beispielcode

Der folgende Code zeigt ein einfaches Beispiel für das Erstellen einer Tabelle und das Einfügen von Zeilen mithilfe von eingebetteten SQL-Befehlen:

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
> Wie im obigen Codebeispiel erwähnt, empfiehlt es sich, Zeichen folgen in SQL-Befehle einzubetten, da der Code für die [SQL-Injektion](https://en.wikipedia.org/wiki/SQL_injection)anfällig ist.

### <a name="using-command-parameters"></a>Verwenden von CommandParameter-Eigenschaften

Der folgende Code zeigt, wie Befehlsparameter verwendet werden, um den von Benutzern eingegebenen textsicher in die Datenbank einzufügen (auch wenn der Text spezielle SQL-Zeichen wie Single-Apostroph enthält):

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

In " **System. Data** " und " **Mono. Data. sqlite** " fehlen einige Funktionen.

<a name="System.Data" />

### <a name="systemdata"></a><legacyBold>System.Data</legacyBold>

Die in " **System. Data. dll** " fehlenden Funktionen bestehen aus folgendem:

- Alles, was [System. CodeDom](xref:System.CodeDom) erfordert (z. b.  [System. Data. TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
- Unterstützung der XML-Konfigurationsdatei (z. b.  [System. Data. Common. dbproviderconfigurationhandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
- [System. Data. Common. dbproviderfactorys](xref:System.Data.Common.DbProviderFactories) (abhängig von der Unterstützung der XML-Konfigurationsdatei)
- [System.Data.OleDb](xref:System.Data.OleDb)
- [System.Data.Odbc](xref:System.Data.Odbc)
- Die `System.EnterpriseServices.dll` Abhängigkeit wurde aus `System.Data.dll` entfernt, was zum Entfernen der Methode [SqlConnection. endlistdistributedtransaction (ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) führte.

<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono. Data. sqlite

In der Zwischenzeit hat **Mono. Data. sqlite. dll** keine Quell Codeänderungen, sondern möglicherweise als Host für eine Reihe von Lauf `Mono.Data.Sqlite.dll` Zeitproblemen, da SQLite 3,5 bindet. IOS 8 wird in der Zwischenzeit mit SQLite 3.8.5 ausgeliefert. Es genügt, dass sich einige Dinge zwischen den beiden Versionen geändert haben.

Die ältere Version von IOS wird mit den folgenden Versionen von SQLite ausgeliefert:

- **IOS 7** -Version 3.7.13.
- **IOS 6** -Version 3.7.13.
- **IOS 5** -Version 3.7.7.
- **IOS 4** -Version 3.6.22.

Die häufigsten Probleme stehen im Zusammenhang mit Datenbankschema Abfragen, z. b. der Bestimmung der Laufzeit, welche Spalten in einer bestimmten Tabelle vorhanden `Mono.Data.Sqlite.SqliteConnection.GetSchema` sind, z. b. (über `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` Schreiben von [DbConnection. GetSchema](xref:System.Data.Common.DbConnection.GetSchema) und ( [Überschreiben von DbDataReader. getschemsierbar](xref:System.Data.Common.DbDataReader.GetSchemaTable). Kurz gesagt: Es ist unwahrscheinlich, dass alles, was die [Daten](xref:System.Data.DataTable) Tabelle verwendet, funktioniert.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung

Die Datenbindung wird zurzeit nicht unterstützt.
