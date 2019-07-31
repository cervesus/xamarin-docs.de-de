---
title: Datenbanken in xamarin. Mac
description: In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierungen und Schlüssel-Wert-Beobachtungen behandelt, um die Datenbindung zwischen SQLite-Datenbanken und Benutzeroberflächen Elementen in der Interface Builder von Xcode Außerdem wird die Verwendung des sqlite.net ORM zum Bereitstellen des Zugriffs auf SQLite-Daten behandelt.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: febfa23ecb2f1536631b3009d6ddc614fa355f01
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656502"
---
# <a name="databases-in-xamarinmac"></a>Datenbanken in xamarin. Mac

_In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierungen und Schlüssel-Wert-Beobachtungen behandelt, um die Datenbindung zwischen SQLite-Datenbanken und Benutzeroberflächen Elementen in der Interface Builder von Xcode Außerdem wird die Verwendung des sqlite.net ORM zum Bereitstellen des Zugriffs auf SQLite-Daten behandelt._

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen SQLite-Datenbanken, auf die eine xamarin. IOS-oder xamarin. Android-Anwendung zugreifen kann.

In diesem Artikel werden zwei Möglichkeiten für den Zugriff auf SQLite-Daten behandelt:

1. **Direkter Zugriff** : indem Sie direkt auf eine SQLite-Datenbank zugreifen, können wir Daten aus der Datenbank für Schlüssel-Wert-Codierung und Datenbindung mit Benutzeroberflächen Elementen verwenden, die in der Interface Builder von Xcode erstellt wurden. Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.
2. **Sqlite.net ORM** : mit dem Open-Source- [sqlite.net](http://www.sqlite.org) Object Relationship Manager (ORM) können wir die Menge an Code, der zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank benötigt wird, erheblich reduzieren. Diese Daten können dann verwendet werden, um ein Benutzeroberflächen Element (z. b. eine Tabellenansicht) aufzufüllen.

[![Ein Beispiel für die laufende App](databases-images/intro01.png "Ein Beispiel für die laufende App")](databases-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel-Wert-Codierung und Datenbindung mit SQLite-Datenbanken in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Da die Schlüssel-Wert-Codierung und Datenbindung verwendet werden, arbeiten Sie zunächst mit der [Datenbindung und der Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) , da grundlegende Techniken und Konzepte behandelt werden, die in dieser Dokumentation und der zugehörigen Beispielanwendung verwendet werden.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden das-Attribut `Register` und `Export` das-Attribut erläutert, mit C# denen die Klassen an gesendet werden. Ziel-C-Objekte und UI-Elemente.

## <a name="direct-sqlite-access"></a>Direkter SQLite-Zugriff

Für SQLite-Daten, die an Benutzeroberflächen Elemente in der Interface Builder von Xcode gebunden werden sollen, wird dringend empfohlen, direkt auf die SQLite-Datenbank zuzugreifen (im Gegensatz zur Verwendung eines Verfahrens, wie z. b. ein ORM), da Sie die vollständige Kontrolle über die Art und Weise haben, in der die Daten geschrieben und gelesen werden.  aus der Datenbank.

Wie wir in der Dokumentation für die [Datenbindung und die Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) mithilfe von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung gesehen haben, können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um zu füllen und Arbeiten Sie mit Benutzeroberflächen Elementen. In Kombination mit direktem Zugriff auf eine SQLite-Datenbank kann die Menge an Code, der zum Lesen und Schreiben von Daten in diese Datenbank erforderlich ist, erheblich reduziert werden.

In diesem Artikel wird die Beispiel-App aus dem Daten Bindungs-und Schlüssel-Wert-Codierungs Dokument so geändert, dass eine SQLite-Datenbank als Unterstützungs Quelle für die Bindung verwendet wird.

### <a name="including-sqlite-database-support"></a>Einschließlich SQLite-Datenbankunterstützung

Bevor wir fortfahren können, müssen wir der Anwendung SQLite-Datenbankunterstützung hinzufügen, indem wir Verweise auf einige von einbeziehen. DLLs-Dateien.

Führen Sie folgende Schritte aus:

1. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf den Ordner **Verweise** , und wählen Sie **Verweise bearbeiten**aus.
2. Wählen Sie die Assemblys **Mono. Data. sqlite** und **System. Data** aus: 

    [![Hinzufügen der erforderlichen Verweise](databases-images/reference01.png "Hinzufügen der erforderlichen Verweise")](databases-images/reference01-large.png#lightbox)
3. Klicken Sie auf die Schaltfläche **OK** , um die Änderungen zu speichern und die Verweise hinzuzufügen.

### <a name="modifying-the-data-model"></a>Ändern des Datenmodells

Nachdem wir nun Unterstützung für den direkten Zugriff auf eine SQLite-Datenbank für die Anwendung hinzugefügt haben, müssen wir das Datenmodell Objekt ändern, um Daten aus der Datenbank zu lesen und zu schreiben (und die Schlüssel-Wert-Codierung und Datenbindung bereitzustellen). Im Fall der Beispielanwendung bearbeiten wir die **PersonModel.cs** -Klasse und führen Sie wie folgt aus:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Werfen wir einen Blick auf die unten aufgeführten Änderungen.

Zunächst haben wir mehrere using-Anweisungen hinzugefügt, die für die Verwendung von SQLite erforderlich sind, und wir haben eine Variable zum Speichern der letzten Verbindung mit der SQLite-Datenbank hinzugefügt:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Diese gespeicherte Verbindung wird verwendet, um automatisch Änderungen am Datensatz in der Datenbank zu speichern, wenn der Benutzer den Inhalt in der Benutzeroberfläche über die Datenbindung ändert:

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Alle an den Eigenschaften **Name**, **Beruf** oder **IsManager** vorgenommenen Änderungen werden an die Datenbank gesendet, wenn die Daten zuvor gespeichert wurden (z. b. Wenn `_conn` die Variable nicht `null`ist). Als nächstes sehen wir uns die Methoden an, die wir hinzugefügt haben, um Personen aus der Datenbank zu **Erstellen**, zu **Aktualisieren**, zu **Laden** und zu **Löschen** .

#### <a name="create-a-new-record"></a>Neuen Datensatz erstellen

Der folgende Code wurde hinzugefügt, um einen neuen Datensatz in der SQLite-Datenbank zu erstellen:

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Wir verwenden einen `SQLiteCommand` , um den neuen Datensatz in der Datenbank zu erstellen. Wir erhalten einen neuen Befehl von `SQLiteConnection` (conn), den wir durch Aufrufen `CreateCommand`von an die-Methode übermittelt haben. Als nächstes legen wir die SQL-Anweisung so fest, dass Sie den neuen Datensatz tatsächlich schreibt und Parameter für die tatsächlichen Werte bereitstellt:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Später legen wir die Werte für die Parameter mit der `Parameters.AddWithValue` -Methode für `SQLiteCommand`die fest. Mithilfe von Parametern stellen wir sicher, dass Werte (z. b. ein einzelnes Anführungszeichen) ordnungsgemäß codiert werden, bevor Sie an SQLite gesendet werden. Beispiel:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Da eine Person ein Manager sein kann und über eine Sammlung von Mitarbeitern verfügt, wird die `Create` -Methode für diese Personen rekursiv aufgerufen, um Sie in der Datenbank zu speichern:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Aktualisieren eines Datensatzes

Der folgende Code wurde hinzugefügt, um einen vorhandenen Datensatz in der SQLite-Datenbank zu aktualisieren:

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Wie **oben** beschrieben erhalten wir eine `SQLiteCommand` `SQLiteConnection`aus dem weiter gegebenen und legen "SQL" fest, um unseren Datensatz zu aktualisieren (Bereitstellen von Parametern):

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Wir geben die Parameterwerte ein (Beispiel: `command.Parameters.AddWithValue ("@COL1", ID);`) und wieder rekursiv Update für alle untergeordneten Datensätze aufrufen:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Ein Datensatz wird geladen.

Der folgende Code wurde hinzugefügt, um einen vorhandenen Datensatz aus der SQLite-Datenbank zu laden:

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Da die Routine von einem übergeordneten Objekt rekursiv aufgerufen werden kann (z. b. ein Manager-Objekt, das das Objekt "Employees" lädt), wurde spezieller Code hinzugefügt, um das Öffnen und Schließen der Verbindung zur Datenbank zu verarbeiten:

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Wie immer legen wir die SQL-Einstellung so fest, dass die Datensatz-und use-Parameter abgerufen werden:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Zum Schluss verwenden wir einen Daten Reader, um die Abfrage auszuführen und die Daten Satz Felder zurückzugeben, die wir in die Instanz `PersonModel` der Klasse kopieren:

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Wenn es sich bei dieser Person um einen Vorgesetzten handelt, müssen wir auch alle Ihre Mitarbeiter laden (durch rekursiver Aufruf `Load` der Methode):

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Löschen eines Datensatzes

Der folgende Code wurde hinzugefügt, um einen vorhandenen Datensatz aus der SQLite-Datenbank zu löschen:

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

Hier stellen wir die SQL-Daten bereit, um sowohl den Manager-Datensatz als auch die Datensätze aller Mitarbeiter unter diesem Manager (mithilfe von Parametern) zu löschen:

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Nachdem der Datensatz entfernt wurde, löschen wir die aktuelle Instanz der `PersonModel` -Klasse:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Initialisieren der Datenbank

Mit den Änderungen an unserem Datenmodell zur Unterstützung von Lese-und Schreibvorgängen in der Datenbank müssen wir eine Verbindung mit der Datenbank herstellen und Sie bei der ersten Durchführung initialisieren. Fügen Sie der Datei **MainWindow.cs** den folgenden Code hinzu:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Sehen wir uns den obigen Code genauer an. Zunächst wählen wir einen Speicherort für die neue Datenbank aus (in diesem Beispiel der Desktop des Benutzers), überprüfen, ob die Datenbank vorhanden ist, und wenn dies nicht der Fall ist, erstellen Sie Folgendes:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Als Nächstes stellen wir die Verbindung mit der Datenbank mithilfe des zuvor erstellten Pfads her:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Anschließend erstellen wir alle SQL-Tabellen in der Datenbank, die wir benötigen:

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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
```

Schließlich wird unser Datenmodell (`PersonModel`) verwendet, um einen Standardsatz von Datensätzen für die Datenbank zu erstellen, wenn die Anwendung zum ersten Mal ausgeführt wird oder wenn die Datenbank nicht vorhanden ist:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Wenn die Anwendung gestartet und das Hauptfenster geöffnet wird, stellen wir mithilfe des oben hinzugefügten Codes eine Verbindung mit der Datenbank her:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Gebundene Daten werden geladen.

Wenn alle Komponenten für den direkten Zugriff auf gebundene Daten aus einer SQLite-Datenbank vorhanden sind, können wir die Daten in den verschiedenen Ansichten laden, die von der Anwendung bereitstellt werden, und Sie werden automatisch in der Benutzeroberfläche angezeigt.

#### <a name="loading-a-single-record"></a>Laden eines einzelnen Datensatzes

Zum Laden eines einzelnen Datensatzes, bei dem die ID bekannt ist, können Sie den folgenden Code verwenden:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Alle Datensätze werden geladen

Um alle Personen zu laden, unabhängig davon, ob Sie ein Manager sind oder nicht, verwenden Sie den folgenden Code:

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

Hier verwenden wir eine Überladung des Konstruktors für die `PersonModel` -Klasse, um die Person in den Arbeitsspeicher zu laden:

```csharp
var person = new PersonModel (_conn, childID);
```

Wir rufen auch die Daten gebundene Klasse auf, um die Person unserer Sammlung von Personen `AddPerson (person)`hinzuzufügen. Dadurch wird sichergestellt, dass die Benutzeroberfläche die Änderung erkennt und anzeigt:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Nur Datensätze oberster Ebene werden geladen

Wenn Sie nur Manager laden möchten (z. b., um Daten in einer Gliederungs Ansicht anzuzeigen), verwenden wir den folgenden Code:

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

Der einzige wirkliche Unterschied in der in SQL-Anweisung (die nur `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`Manager lädt), funktioniert jedoch genauso wie der oben beschriebene Abschnitt.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Datenbanken und Kombinations Felder

Die Menü Steuerelemente, die für macOS (z. b. das Kombinations Feld) verfügbar sind, können so festgelegt werden, dass die Dropdown Liste entweder aus einer internen Liste (die in Interface Builder vordefiniert oder über Code aufgefüllt werden kann) ausgefüllt wird, oder indem Sie eine eigene benutzerdefinierte, externe Datenquelle bereitstellt. Weitere Informationen finden Sie unter [Bereitstellen von Menü Steuerungsdaten](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) .

Bearbeiten Sie z. b. das Beispiel für eine einfache Bindung in Interface Builder, fügen Sie ein Kombinations Feld hinzu, und machen `EmployeeSelector`Sie es mithilfe eines Outlets namens verfügbar.

[Verfügbar machen ![eines Kombinations Feld-Outlets] Verfügbar machen (databases-images/combo01.png "eines Kombinations Feld-Outlets")](databases-images/combo01-large.png#lightbox)

Überprüfen Sie im **Attribut Inspektor**die Eigenschaften **automatisch abgeschlossen** und **verwendet Datenquelle** :

![Konfigurieren der Kombinations Feld Attribute](databases-images/combo02.png "Konfigurieren der Kombinations Feld Attribute")

Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.

#### <a name="providing-combobox-data"></a>Bereitstellen von ComboBox-Daten

Fügen Sie als nächstes dem Projekt eine neue Klasse mit `ComboBoxDataSource` dem Namen hinzu, und machen Sie Sie wie folgt:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

In diesem Beispiel erstellen wir eine neue `NSComboBoxDataSource` , die Kombinations Feld-Elemente aus einer beliebigen SQLite-Datenquelle darstellen kann. Zuerst definieren wir die folgenden Eigenschaften:

- `Conn`: Ruft eine Verbindung mit der SQLite-Datenbank ab oder legt Sie fest.
- `TableName`: Ruft den Tabellennamen ab oder legt ihn fest.
- `IDField`: Ruft das Feld ab, das die eindeutige ID für die angegebene Tabelle bereitstellt, oder legt dieses fest. Der Standardwert ist `ID`.
- `DisplayField`: Ruft das Feld ab, das in der Dropdown Liste angezeigt wird, oder legt dieses fest.
- `RecordCount`-Ruft die Anzahl der Datensätze in der angegebenen Tabelle ab.

Wenn wir eine neue Instanz des Objekts erstellen, übergeben wir die Verbindung, den Tabellennamen, optional das ID-Feld und das Anzeige Feld:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

Die `GetRecordCount` -Methode gibt die Anzahl der Datensätze in der angegebenen Tabelle zurück:

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

Sie wird immer dann aufgerufen, `TableName` `IDField` wenn der `DisplayField` -Wert oder der-Eigenschafts Wert geändert wird.

Die `IDForIndex` -Methode gibt die eindeutige ID`IDField`() für den Datensatz am angegebenen Dropdown Listenelement-Index zurück: 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Die `ValueForIndex` -Methode gibt den Wert`DisplayField`() für das Element am angegebenen Dropdown-Listen Index zurück:

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Die `IDForValue` -Methode gibt die eindeutige ID`IDField`() für den angegebenen Wert`DisplayField`zurück ():

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

Der `ItemCount` gibt die Voraus berechnete Anzahl von Elementen in der Liste zurück, die `TableName`berechnet werden `DisplayField` , wenn die Eigenschaften, `IDField` oder geändert werden:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

Die `ObjectValueForItem` -Methode stellt den Wert`DisplayField`() für den angegebenen Dropdown-Listenelement Index bereit:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Beachten Sie, dass die `LIMIT` -und-Anweisungen in unserem SQLite- `OFFSET` Befehl verwendet werden, um den einzelnen Datensatz einzuschränken, den wir benötigen.

Die `IndexOfItem` Methode gibt den Dropdown-Element Index des Werts`DisplayField`() zurück, der angegeben ist:

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Wenn der Wert nicht gefunden werden kann, `NSRange.NotFound` wird der Wert zurückgegeben, und alle Elemente werden in der Dropdown Liste deaktiviert.

Die `CompletedString` -Methode gibt den ersten übereinstimmenden Wert (`DisplayField`) für einen teilweise typisierten Eintrag zurück:

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Anzeigen von Daten und reagieren auf Ereignisse

Wenn Sie alle Teile zusammenbringen möchten, bearbeiten Sie `SubviewSimpleBindingController` das, und führen Sie es wie folgt aus:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

Die `DataSource` -Eigenschaft stellt eine Verknüpfung `ComboBoxDataSource` zum (oben erstellten) dar, das an das Kombinations Feld angefügt ist.

Die `LoadSelectedPerson` -Methode lädt die Person für die angegebene eindeutige ID aus der Datenbank:

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

In der `AwakeFromNib` Methoden Überschreibung fügen wir zuerst eine Instanz der benutzerdefinierten Kombinations Feld-Datenquelle an:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Als nächstes Antworten wir auf den Benutzer, der den Textwert des Kombinations Felds bearbeitet, indem Sie die zugeordnete`IDField`eindeutige ID () der Daten ermitteln, die die jeweilige Person darstellen und laden, sofern Sie gefunden wird:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Außerdem wird eine neue Person geladen, wenn der Benutzer ein neues Element aus der Dropdown Liste auswählt:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Zum Schluss wird das Kombinations Feld und die angezeigte Person automatisch mit dem ersten Element in der Liste aufgefüllt:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.net ORM

Wie oben bereits erwähnt, können wir mithilfe des Open Source [sqlite.net](http://www.sqlite.org) Object Relationship Manager (ORM) die Menge des Codes, der zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank benötigt wird, erheblich reduzieren. Dies ist möglicherweise nicht die beste Route, die beim Binden von Daten aufgrund verschiedener Anforderungen erforderlich ist, bei denen die Schlüssel-Wert-Codierung und die Datenbindung für ein Objekt erfolgen.

Gemäß der SQLite.NET-Website _ist "SQLite" eine Software Bibliothek, die ein eigenständiges, transaktionales SQL-Datenbankmodul (Server Loses, null) Konfigurationsmodul implementiert. SQLite ist die am weitesten verbreitete Datenbank-Engine auf der ganzen Welt. Der Quellcode für SQLite befindet sich in der öffentlichen Domäne. "_

In den folgenden Abschnitten wird gezeigt, wie sqlite.NET verwendet wird, um Daten für eine Tabellenansicht bereitzustellen.

### <a name="including-the-sqlitenet-nuget"></a>Einschließlich sqlite.net nuget

SQLite.net wird als nuget-Paket dargestellt, das Sie in Ihre Anwendung einschließen. Bevor wir die Datenbankunterstützung mithilfe von SQLite.NET hinzufügen können, müssen wir dieses Paket einschließen.

Gehen Sie folgendermaßen vor, um das Paket hinzuzufügen:

1. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf den Ordner **Pakete** , und wählen Sie **Pakete hinzufügen... aus.**
2. Geben `SQLite.net` Sie im **Suchfeld** ein, und wählen Sie den Eintrag **SQLite-net** aus:

    [![Hinzufügen des SQLite-nuget-Pakets](databases-images/nuget01.png "Hinzufügen des SQLite-nuget-Pakets")](databases-images/nuget01-large.png#lightbox)
3. Klicken Sie zum Fertigstellen auf die Schaltfläche **Paket hinzufügen**

### <a name="creating-the-data-model"></a>Erstellen des Datenmodells

Fügen Sie dem Projekt eine neue Klasse hinzu, und nennen Sie `OccupationModel`Sie in. Als nächstes bearbeiten wir die Datei **OccupationModel.cs** und legen Sie wie folgt an:

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

Zuerst fügen wir sqlite.net (`using Sqlite`) ein. dann werden mehrere Eigenschaften verfügbar gemacht, die beim Speichern dieses Datensatzes in die Datenbank geschrieben werden. Die erste Eigenschaft, die wir als Primärschlüssel festlegen, und legen die automatische Inkrement wie folgt fest:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Initialisieren der Datenbank

Mit den Änderungen an unserem Datenmodell zur Unterstützung von Lese-und Schreibvorgängen in der Datenbank müssen wir eine Verbindung mit der Datenbank herstellen und Sie bei der ersten Durchführung initialisieren. Fügen Sie den folgenden Code hinzu:

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

Zuerst wird ein Pfad zur Datenbank (in diesem Fall der Desktop des Benutzers) angezeigt, und es wird angezeigt, ob die Datenbank bereits vorhanden ist:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Als Nächstes stellen wir eine Verbindung mit der Datenbank an dem Pfad her, den wir oben erstellt haben:

```csharp
var conn = new SQLiteConnection (db);
```

Schließlich wird die Tabelle erstellt und einige Standarddaten Sätze hinzugefügt:

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Hinzufügen einer Tabellenansicht

Als Beispiel für die Verwendung fügen wir eine Tabellenansicht zu unserer Benutzeroberfläche in den Schnittstellen Generator von Xcode hinzu. Wir machen diese Tabellenansicht über ein Outlet (`OccupationTable`) verfügbar, damit wir über C# Code darauf zugreifen können:

[Verfügbar machen ![eines Outlets der Tabellenansicht] Verfügbar machen (databases-images/table01.png "eines Outlets der Tabellenansicht")](databases-images/table01-large.png#lightbox)

Als Nächstes fügen wir die benutzerdefinierten Klassen hinzu, um diese Tabelle mit Daten aus der SQLite.net-Datenbank aufzufüllen.

### <a name="creating-the-table-data-source"></a>Erstellen der Tabellendaten Quelle

Erstellen wir eine benutzerdefinierte Datenquelle, um Daten für die Tabelle bereitzustellen. Fügen Sie zunächst eine neue Klasse mit `TableORMDatasource` dem Namen hinzu, und machen Sie Sie wie folgt:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Wenn wir später eine Instanz dieser Klasse erstellen, übergeben wir unsere Open sqlite.net-Datenbankverbindung. Die `LoadOccupations` -Methode fragt die Datenbank ab und kopiert die gefundenen Datensätze in den `OccupationModel` Arbeitsspeicher (mit unserem Datenmodell).

### <a name="creating-the-table-delegate"></a>Erstellen des Tabellen Delegaten

Die endgültige Klasse, die wir benötigen, ist ein benutzerdefinierter Tabellen Delegat zum Anzeigen der Informationen, die aus der SQLite.net-Datenbank geladen wurden. Fügen Sie dem Projekt ein `TableORMDelegate` neues hinzu, und machen Sie es wie folgt:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Hier verwenden wir die `Occupations` Sammlung der Datenquelle (die aus der SQLite.net-Datenbank geladen wurde), um die Spalten der Tabelle über die `GetViewForItem` Außerkraftsetzungs Methode auszufüllen.

### <a name="populating-the-table"></a>Auffüllen der Tabelle

Wenn alle Teile vorhanden sind, füllen wir die Tabelle aus, wenn Sie aus der XIb-Datei überschrieben wird, indem Sie die `AwakeFromNib` -Methode überschreiben und wie folgt aussehen:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

Zuerst erhalten wir Zugriff auf unsere sqlite.net-Datenbank, die Sie erstellen und Auffüllen, wenn Sie nicht bereits vorhanden ist. Als Nächstes erstellen wir eine neue Instanz der benutzerdefinierten Tabellendaten Quelle, übergeben die Datenbankverbindung und fügen diese an die Tabelle an. Schließlich erstellen wir eine neue Instanz des benutzerdefinierten Tabellen Delegaten, übergeben die Datenquelle und fügen Sie an die Tabelle an.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit der Datenbindung und der Schlüssel-Wert-Codierung mit SQLite-Datenbanken in einer xamarin. Mac-Anwendung ausführlich erläutert. Zuerst wurde das verfügbar machen einer C# Klasse für "Ziel-C" mithilfe von Key-Value Coding (KVC) und Key-Value-Beobachtungen (KVO) untersucht. Im nächsten Schritt wurde gezeigt, wie eine KVO-kompatible Klasse verwendet und Daten an Benutzeroberflächen Elemente in der Interface Builder von Xcode gebunden werden. Der Artikel behandelt auch das Arbeiten mit SQLite-Daten über den sqlite.net ORM und das Anzeigen dieser Daten in einer Tabellenansicht.



## <a name="related-links"></a>Verwandte Links

- [Macdatabase (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabase)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Standard Steuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellen Sichten](~/mac/user-interface/table-view.md)
- [Gliederungs Ansichten](~/mac/user-interface/outline-view.md)
- [Sammlungs Ansichten](~/mac/user-interface/collection-view.md)
- [Programmier Handbuch für Schlüssel-Wert-Codierung](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Programmierthemen "Einführung in Cocoa-Bindungen"](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in Cocoa-Bindungs Referenz](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
