---
title: "\"System.Data\", in Xamarin.iOS"
description: Dieses Dokument beschreibt, wie "System.Data" und Mono.Data.Sqlite.dll auf SQLite-Daten in einer Xamarin.iOS-Anwendung zugreifen.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: 4e9b782cf266a96f30c79eaf139ef88332e02dca
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119834"
---
# <a name="systemdata-in-xamarinios"></a>"System.Data", in Xamarin.iOS

Xamarin.iOS 8.10 bietet Unterstützung für ["System.Data"](xref:System.Data), einschließlich der `Mono.Data.Sqlite.dll` ADO NET-Anbieter. Unterstützung umfasst das Hinzufügen der folgenden [Assemblys](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Beispiel

Das folgende Programm erstellt eine Datenbank in `Documents/mydb.db3`, und wenn die Datenbank zuvor noch nicht mit Beispieldaten gefüllt ist. Die Datenbank wird dann abgefragt, mit der Ausgabe geschrieben, um `stderr`.

### <a name="add-references"></a>Fügen Sie Verweise hinzu

Zunächst mit der rechten Maustaste auf die **Verweise** Knoten, und wählen Sie **Verweise bearbeiten...**  wählen Sie dann `System.Data` und `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Hinzufügen von neuen verweisen")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Beispielcode

Der folgende Code zeigt ein einfaches Beispiel für eine Tabelle zu erstellen und Einfügen von Zeilen, die über eingebettete SQL-Befehle:

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
> Wie im obigen Codebeispiel erwähnt, ist es nicht üblich, Zeichenfolgen in SQL-Befehlen eingebettet werden, weil Ihr Code anfällig ist [SQL-Einschleusung](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Über Parameter des Befehls

Der folgende Code zeigt, wie Sie die Befehlsparameter zu verwenden, um Benutzer eingegebener Text sicher in die Datenbank eingefügt werden soll, (selbst wenn der Text SQL-Sonderzeichen wie z. B. einzelne-Apostroph):

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

## <a name="missing-functionality"></a>Fehlende Funktionen

Beide **"System.Data"** und **Mono.Data.Sqlite** fehlen einige Funktionen.

<a name="System.Data" />

### <a name="systemdata"></a><legacyBold>System.Data</legacyBold>

Funktionen, die fehlen **"System.Data.dll"** besteht aus:

-  Alles, was erfordern [System.CodeDom](xref:System.CodeDom) (z.B.)  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  XML-Konfiguration Datei unterstützen (z.B.)  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (hängt mit Unterstützung für XML-Config-Dateien)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  Die `System.EnterpriseServices.dll` Abhängigkeit wurde *entfernt* aus `System.Data.dll` , wodurch das Entfernen der [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) Methode.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

In der Zwischenzeit **Mono.Data.Sqlite.dll** ist keine Änderungen am Quellcode, aber stattdessen möglicherweise Host mit der Anzahl von *Runtime* seit Probleme `Mono.Data.Sqlite.dll` SQLite 3.5 bindet. iOS 8 umfasst in der Zwischenzeit SQLite 3.8.5. Naja, haben einige Dinge zwischen den beiden Versionen geändert werden.

Im Lieferumfang von älteren iOS-Version sind der folgenden Versionen von SQLite:

- **iOS 7** -Version 3.7.13.
- **iOS 6** -Version 3.7.13.
- **iOS 5** -Version 3.7.7.
- **iOS 4** -Version 3.6.22.

Am häufigsten auftretenden Probleme angezeigt, in Bezug auf Schemas Abfragen, z. B. zur Laufzeit die Spalten einer Tabelle, wie z. B. vorhanden bestimmen `Mono.Data.Sqlite.SqliteConnection.GetSchema` (überschreiben [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) und `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (und damit überschreiben [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable). Kurz gesagt: Es scheint, dass alles, was mit [DataTable](xref:System.Data.DataTable) funktioniert wahrscheinlich nicht.

<a name="Data_Binding" />

## <a name="data-binding"></a>Datenbindung

Die Datenbindung wird zu diesem Zeitpunkt nicht unterstützt.

