---
title: Databases
description: Dieser Artikel behandelt mit Codierung mit Schlüssel-Wert und Schlüssel-Wert prüfen, um die Datenbindung zwischen SQLite-Datenbanken und die Elemente der Benutzeroberfläche in Xcodes Benutzeroberflächen-Generator zu ermöglichen. Es werden auch mithilfe der SQLite.NET ORM SQLite Datenzugriff behandelt.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 33c1ab7092669bb1dbd4e7bfae628b58a0bf3726
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="databases"></a>Databases

_Dieser Artikel behandelt mit Codierung mit Schlüssel-Wert und Schlüssel-Wert prüfen, um die Datenbindung zwischen SQLite-Datenbanken und die Elemente der Benutzeroberfläche in Xcodes Benutzeroberflächen-Generator zu ermöglichen. Es werden auch mithilfe der SQLite.NET ORM SQLite Datenzugriff behandelt._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleichen SQLite-Datenbanken, die eine Xamarin.iOS oder Xamarin.Android Anwendung zugreifen können.

In diesem Artikel werden zwei Möglichkeiten, SQLite Datenzugriff behandelt:

1. **Direkten Zugriff auf die** – durch direkten Zugriff auf eine SQLite-Datenbank, können wir Daten aus der Datenbank für die Codierung von Schlüssel-Wert- und -Datenbindung mit Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator erstellt. Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf.
2. **SQLite.NET ORM-** – mithilfe der open Source [SQLite.NET](http://www.sqlite.org) Objekt Beziehung Manager (ORM) wir kann erheblich reduzieren den Umfang des Codes zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank erforderlich. Diese Daten können dann zum Auffüllen einer Benutzer-Schnittstelle-Element, z. B. einer Tabellenansicht verwendet werden.

[![Ein Beispiel für die ausgeführte app](databases-images/intro01.png "ein Beispiel für die ausgeführte app")](databases-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel-Wert zu codieren und Datenbindung mit SQLite-Datenbanken in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Da wir Schlüssel-Wert zu codieren und die Datenbindung verwenden, arbeiten Sie durch die [Datenbindung und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) zunächst als Haupt-Techniken und Konzepte behandelt werden, die in dieser Dokumentation und seine Beispielcode verwendet werden die Anwendung.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.

## <a name="direct-sqlite-access"></a>Direkten Zugriff auf die SQLite

SQLite-Daten, die Elemente der Benutzeroberfläche in Xcodes Benutzeroberflächen-Generator gebunden werden soll, wird dringend empfohlen, dass Sie auf die SQLite-Datenbank direkt (im Gegensatz zur Verwendung einer Technik, z. B. ein ORM) zugreifen, da Sie die vollständige Kontrolle über die Möglichkeit haben, wird die Daten geschrieben und gelesen  aus der Datenbank.

Wie in der wir gesehen haben die [Datenbindung und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation, mithilfe von Schlüssel-Wert beim Codieren und Datenbindungsmethoden in Ihrer Anwendung Xamarin.Mac können Sie erheblich verringern, den Umfang des Codes, die Sie schreiben müssen, und Verwalten Sie zum Auffüllen von und Arbeiten mit UI-Elemente. In Kombination mit direktem Zugriff auf eine SQLite-Datenbank kann es auch erheblich reduzieren den Umfang des Codes zum Lesen und Schreiben von Daten in dieser Datenbank erforderlich.

In diesem Artikel werden wir ändern, die Beispiel-app aus dem Datenbindung und die Codierung Schlüssel-Wert-Dokument, eine SQLite-Datenbank als Quelle für die Sicherung für die Bindung verwendet werden.

### <a name="including-sqlite-database-support"></a>Einschließlich der Unterstützung der SQLite-Datenbank

Bevor wir fortfahren können, müssen wir durch Einschließen von Verweisen auf eine Reihe von SQLite-Datenbank unterstützt unsere Anwendung hinzufügen. DLLs-Dateien.

Führen Sie folgende Schritte aus:

1. In der **Lösung Pad**, mit der rechten Maustaste auf die **Verweise** Ordner, und wählen **Verweise bearbeiten**.
2. Wählen Sie sowohl die **Mono.Data.Sqlite** und **"System.Data"** Assemblys: 

    [![Hinzufügen der erforderlichen Verweise](databases-images/reference01.png "die erforderlichen Verweise hinzufügen")](databases-images/reference01-large.png#lightbox)
3. Klicken Sie auf die **OK** Schaltfläche, um die Änderungen zu speichern und die Verweise hinzuzufügen.

### <a name="modifying-the-data-model"></a>Ändern das Datenmodell

Wir Unterstützung für den direkten Zugriff auf eine SQLite-Datenbank auf die Anwendung hinzugefügt haben, müssen wir unsere Data Model-Objekts zum Lesen und Schreiben von Daten aus der Datenbank (ebenso wie bieten, Schlüssel-Wert zu codieren und Datenbindung) ändern. Bei unserem beispielanwendung bearbeiten wir die **PersonModel.cs** Klasse, und stellen sie wie folgt aussehen:

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

Werfen wir einen Blick auf die Änderungen im folgenden ausführlich.

Zunächst haben wir hinzugefügt mehrere using-Anweisungen, die Verwendung der SQLite erforderlich sind, und wir haben eine Variable, um unsere letzten Verbindung mit der SQLite-Datenbank speichern hinzugefügt:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Wir verwenden diese gespeicherte Verbindung, um alle Änderungen automatisch auf den Datensatz mit der Datenbank zu speichern, wenn der Benutzer den Inhalt in der Benutzeroberfläche, über die Datenbindung ändert:

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

Alle Änderungen an der **Namen**, **Beruf** oder **IsManager** Eigenschaften werden an die Datenbank gesendet, wenn die Daten vor dem es gespeichert wurde (z. B. wenn die `_conn` die Variable ist nicht `null`). Als Nächstes sehen wir uns die Methoden, die wir hinzugefügt haben **erstellen**, **Update**, **laden** und **löschen** Personen aus der Datenbank.

#### <a name="create-a-new-record"></a>Erstellen Sie einen neuen Datensatz

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

Wir verwenden eine `SQLiteCommand` auf den neuen Eintrag in der Datenbank zu erstellen. Erhalten wir einen neuen Befehl von der `SQLiteConnection` (conn), dass es durch den Aufruf an die Methode übergeben `CreateCommand`. Anschließend legen wir die SQL-Anweisung in den neuen Eintrag tatsächlich zu schreiben Parameter für die tatsächlichen Werte bereitstellen:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Später wir legen Sie die Werte für den Parameter mit der `Parameters.AddWithValue` Methode für die `SQLiteCommand`. Mit Parametern, stellen wir sicher, dass die Werte (z. B. ein einfaches Anführungszeichen) ordnungsgemäß codiert abrufen, bevor an SQLite gesendet werden. Beispiel:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Schließlich, da eine Person kann werden ein Manager und eine Auflistung von Mitarbeitern unter sich haben, werden wir rekursiv Aufrufen der `Create` -Methode für die Personen zu speichernde an die Datenbank auch:

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

Der folgende Code wurde hinzugefügt, um einen vorhandenen Datensatz in die SQLite-Datenbank zu aktualisieren:

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

Wie **erstellen** oberhalb, erhalten wir einen `SQLiteCommand` aus dem übergebenen in `SQLiteConnection`, und legen Sie unsere SQL, unserem Datensatz (mit Parametern) zu aktualisieren:

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Wir geben Sie die Parameterwerte (Beispiel: `command.Parameters.AddWithValue ("@COL1", ID);`) und wieder, rekursiv aufrufen Update für alle untergeordneten Datensätze:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Laden einen Datensatz

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

Da die Routine rekursiv über ein übergeordnetes Objekt (z. B. eine Manager-Objekt, das ihre Mitarbeiter Objekt geladen) aufgerufen werden, kann, wurde spezieller Code zur Behandlung von öffnen und Schließen der Verbindung mit der Datenbank hinzugefügt:

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

Wie immer legen wir unsere SQL, um den Datensatz abrufen und verwenden Sie Parameter fest:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Schließlich wir einen Datenleser verwenden, um die Abfrage auszuführen und die Datensatzfelder zurück (die wir in der Instanz von Kopieren der `PersonModel` Klasse):

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

Wenn diese Person ein Manager ist, müssen wir auch laden, alle Mitarbeiter (von rekursiven Aufruf von ihren `Load` Methode):

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

Hier bieten wir den SQL aus, um den Datensatz Manager und die Datensätze aller Mitarbeiter, Manager (Verwenden von Parametern) zu löschen:

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Nachdem der Datensatz entfernt wurde, deaktivieren, die aktuelle Instanz von der `PersonModel` Klasse:

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

Mit den Änderungen an unseren Datenmodell direktes Lesen und Schreiben in die Datenbank unterstützen müssen wir keine Verbindung mit der Datenbank, und initialisieren Sie es bei der ersten Ausführung. Fügen Sie den folgenden Code zu unserem **MainWindow.cs** Datei:

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

Werfen Sie genauere Betrachtung der obige Code ein. Zunächst wählen wir einen Speicherort für die neue Datenbank (in diesem Beispiel dem Desktop des Benutzers), suchen, um festzustellen, ob die Datenbank vorhanden ist, und wenn dies nicht der Fall, erstellen Sie ihn:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Als Nächstes erstellen wir die Verbindung mit der Datenbank mithilfe des Pfads, dem wir zuvor erstellt haben:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Erstellen Sie dann die SQL-Tabellen in der Datenbank, die es erfordern:

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

Schließlich verwenden wir unser Datenmodell (`PersonModel`) erstellen Sie eine Reihe von Datensätzen für die Datenbank das erste Mal die Anwendung ausgeführt wird oder wenn die Datenbank nicht vorhanden ist:

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

Wenn die Anwendung startet und öffnet im Hauptfenster, stellen wir eine Verbindung mit der Datenbank, die mit den oben hinzugefügten Code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Laden von gebundenen Daten

Mit allen Komponenten für den Zugriff auf direkt gebundenen Daten aus einer SQLite-Datenbank vorhanden können wir laden die Daten in den verschiedenen Ansichten, die die Anwendung bereitstellt, und wird in unserer GUI automatisch angezeigt werden.

#### <a name="loading-a-single-record"></a>Laden einen einzelnen Datensatz

Zum Laden von ein einzelnen Datensatz dabei ist die ID kennen, wir den folgenden Code verwenden können:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Laden alle Datensätze

Verwenden Sie den folgenden Code, um alle Personen, unabhängig davon zu laden, wenn sie einen Manager sind:

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

Hier verwenden wir eine Überladung des Konstruktors für die `PersonModel` Klasse, um die Person, die in den Arbeitsspeicher geladen:

```csharp
var person = new PersonModel (_conn, childID);
```

Wir sind auch die Klasse gebundenen Daten, um die Person, die Auflistung von Personen hinzufügen Aufrufen `AddPerson (person)`, dadurch wird sichergestellt, dass unsere UI die Änderung erkennt und ihn zeigt:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Laden nur die Datensätze oberster Ebene

Um nur von Managern (z. B. zum Anzeigen von Daten in einer Gliederungsansicht) zu laden, verwenden Sie folgenden Code:

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

Der einzige wirkliche Unterschied in der in SQL-Anweisung (die lädt nur Manager `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), aber funktioniert genauso wie im obigen Abschnitt andernfalls.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Datenbanken und Kombinationsfelder

Die Menüsteuerelemente MacOS (z. B. Kombinationsfeld) zur Verfügung, kann der Dropdown-Liste entweder von einer internen Liste aufgefüllt (die können werden vordefinierte in Benutzeroberflächen-Generator oder über Code aufgefüllt) festgelegt werden oder indem Sie eigene benutzerdefinierte, externe Datenquelle bereitstellen. Finden Sie unter [Menü Steuerelementdaten bereitstellen](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) Weitere Details.

Beispielsweise ein Kombinationsfeld Bearbeiten im Beispiel oben im-Generator-Schnittstelle: einfache Bindung hinzufügen und verfügbar zu machen ihn mit einer Netzsteckdose, mit dem Namen `EmployeeSelector`:

[![Verfügbarmachen von einem Kombinationsfeld im Feld Steckdose](databases-images/combo01.png "verfügbar machen eine Kombinationsfeld Feld an")](databases-images/combo01-large.png#lightbox)

In der **Attribute Inspektor**, überprüfen Sie die **Autocompletes** und **Datenquelle verwendet** Eigenschaften:

![Konfigurieren von Attributen der Kombinationsfeld-Feld](databases-images/combo02.png "Konfigurieren von Attributen der Kombinationsfeld-Feld")

Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.

#### <a name="providing-combobox-data"></a>Combobox-Daten bereitstellt

Als Nächstes fügen Sie eine neue Klasse, um das Projekt mit der Bezeichnung `ComboBoxDataSource` , und stellen sie wie folgt aussehen:

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

In diesem Beispiel erstellen wir ein neues `NSComboBoxDataSource` Kombinationsfeldelemente aus beliebigen Datenquellen SQLite vorweisen können. Zuerst definieren wir die folgenden Eigenschaften:

- `Conn` -Ruft ab oder legt eine Verbindung mit der SQLite-Datenbank fest.
- `TableName` -Ruft ab oder legt den Tabellennamen fest.
- `IDField` -Ruft ab oder legt das Feld, das die eindeutige ID für die angegebene Tabelle bereitstellt. Der Standardwert ist `ID`.
- `DisplayField` -Ruft ab oder legt das Feld, das in der Dropdownliste angezeigt wird.
- `RecordCount` -Ruft die Anzahl der Datensätze in der angegebenen Tabelle.

Wenn wir eine neue Instanz des Objekts erstellen, übergeben wir die Verbindung, Tabellenname, optional das Feld "ID" und das Anzeigen eines Felds:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

Die `GetRecordCount` Methode gibt die Anzahl der Datensätze in die angegebene Tabelle zurück:

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

Es wird jedes Mal aufgerufen der `TableName`, `IDField` oder `DisplayField` Eigenschaftswert geändert wird.

Die `IDForIndex` Methode gibt die eindeutige ID (`IDField`) für den Eintrag am angegebenen Dropdown Liste Elementindex: 

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

Die `ValueForIndex` -Methode gibt den Wert (`DisplayField`) für das Element am angegebenen Dropdown Listenindex:

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

Die `IDForValue` Methode gibt die eindeutige ID (`IDField`) für den gegebenen Wert (`DisplayField`):

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

Die `ItemCount` gibt die vorausberechnete Anzahl von Elementen in der Liste wie berechnet, wenn die `TableName`, `IDField` oder `DisplayField` Eigenschaften geändert werden:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

Die `ObjectValueForItem` Methode stellt der Wert (`DisplayField`) für den Index des angegebenen Dropdown Liste Elements:

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

Beachten Sie, dass die von uns verwendeten die `LIMIT` und `OFFSET` Anweisungen in unseren SQLite-Befehl aus, um einen Datensatz zu beschränken, dass es nicht erforderlich sind.

Die `IndexOfItem` Methodenrückgabe Dropdownliste Elementindex des Werts (`DisplayField`) angegebenen:

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

Wenn der Wert kann nicht gefunden werden, die `NSRange.NotFound` Wert zurückgegeben wird und alle Elemente werden deaktiviert, in der Dropdownliste aus.

Die `CompletedString` Methode gibt den ersten übereinstimmenden Wert (`DisplayField`) für eine teilweise typisierte Eintrag:

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

Um alle Teile zusammenzuführen, bearbeiten Sie die `SubviewSimpleBindingController` , und stellen sie wie folgt aussehen:

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

Die `DataSource` Eigenschaft verfügt über eine Tastenkombination, die `ComboBoxDataSource` (oben erstellten) dem Kombinationsfeld angefügt.

Die `LoadSelectedPerson` Methode lädt die Person, die aus der Datenbank für die angegebene eindeutige ID:

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

In der `AwakeFromNib` methodenüberschreibung, wir fügen zuerst eine Instanz von unsere benutzerdefinierte Kombinationsfeld im-Datenquelle:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Als Nächstes wir Antworten an den Benutzer, der den Textwert des Kombinationsfelds bearbeiten, durch die zugeordnete eindeutige ID zu suchen (`IDField`) der Daten darstellen, und laden die angegebene Person ein, wenn gefunden:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Wir haben auch eine neue Person laden, klickt der Benutzer ein neues Element aus der Dropdownliste aus:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Schließlich Auffüllen wir das Kombinationsfeld und angezeigten Person mit dem ersten Element in der Liste:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Wie oben erwähnt, mithilfe der open Source [SQLite.NET](http://www.sqlite.org) Objekt Beziehung Manager (ORM) wir kann erheblich reduzieren den Umfang des Codes zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank erforderlich. Dies möglicherweise nicht die optimale Route wird beim Binden von Daten aufgrund von mehreren Anforderungen, die Codierung von Schlüssel-Wert und Datenbindung für ein Objekt zu platzieren.

Gemäß der Website SQLite.Net _"SQLite ist eine Softwarebibliothek, die eine eigenständige, serverlose, Konfigurationsfreie, transaktionale SQL-Datenbankmodul implementiert. SQLite ist die am häufigsten bereitgestellten Datenbankmodul in der ganzen Welt. Der Quellcode für SQLite ist in der öffentlichen Domäne."_

In den folgenden Abschnitten zeigen wir, wie SQLite.Net verwenden, um Daten für eine Tabellensicht bereitzustellen.

### <a name="including-the-sqlitenet-nuget"></a>Einschließlich der SQLite.net NuGet

SQLite.NET wird als ein NuGet-Paket angezeigt, die Sie in der Anwendung einschließen. Bevor wir mit SQLite.NET datenbankunterstützung hinzufügen können, müssen wir dieses Paket enthalten.

Führen Sie Folgendes ein, um das Paket hinzufügen:

1. In der **Lösung Pad**, mit der rechten Maustaste die **Pakete** Ordner, und wählen **Pakete hinzufügen...**
2. Geben Sie `SQLite.net` in der **Suchfeld** , und wählen Sie die **Sqlite-Net** Eintrag:

    [![Hinzufügen der SQLite-NuGet-Paket](databases-images/nuget01.png "hinzufügen SQLite NuGet-Pakets")](databases-images/nuget01-large.png#lightbox)
3. Klicken Sie auf die **Paket hinzufügen** Schaltfläche, um den Vorgang abzuschließen.

### <a name="creating-the-data-model"></a>Erstellen das Datenmodell

Wir das Projekt eine neue Klasse hinzu, und rufen Sie in `OccupationModel`. Als Nächstes bearbeiten wir die **OccupationModel.cs** Datei, und stellen sie wie folgt aussehen:

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

Zuerst schließen wir SQLite.NET (`using Sqlite`), klicken Sie dann mehreren Eigenschaften zur Verfügung, von denen jede wird werden in die Datenbank geschrieben wird dieser Datensatz gespeichert wird. Die erste Eigenschaft, die wir stellen als Primärschlüssel und legen Sie sie auf automatische Inkrementierung der wie folgt:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Initialisieren der Datenbank

Mit den Änderungen an unseren Datenmodell direktes Lesen und Schreiben in die Datenbank unterstützen müssen wir keine Verbindung mit der Datenbank, und initialisieren Sie es bei der ersten Ausführung. Fügen wir den folgenden Code hinzu:

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

Zunächst einen Pfad zu der Datenbank (der Desktop des Benutzers in diesem Fall) abzurufen und festzustellen, ob die Datenbank bereits vorhanden ist:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Als Nächstes erstellen wir eine Verbindung mit der Datenbank unter dem Pfad, dem wir zuvor erstellt haben:

```csharp
var conn = new SQLiteConnection (db);
```

Schließlich erstellen Sie die Tabelle und fügen Sie einige Standard-Einträge hinzu:

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

Als ein Verwendungsbeispiel fügen wir eine Tabellensicht auf unserer GUI in Xcodes Benutzeroberflächen-Generator. Wir müssen hier Tabelle über eine Steckdose verfügbar zu machen (`OccupationTable`), damit wir über C#-Code darauf zugreifen können:

[![Verfügbarmachen von einer Tabelle anzeigen Steckdose](databases-images/table01.png "Verfügbarmachen von einer Steckdose der Tabelle anzeigen")](databases-images/table01-large.png#lightbox)

Fügen Sie anschließend die benutzerdefinierten Klassen zum Auffüllen dieser Tabelle mit Daten aus der Datenbank SQLite.NET.

### <a name="creating-the-table-data-source"></a>Erstellen der Datenquelle der Tabelle

Erstellen wir eine benutzerdefinierte Datenquelle zum Bereitstellen von Daten für die Tabelle ein. Fügen Sie zunächst eine neue Klasse namens `TableORMDatasource` , und stellen sie wie folgt aussehen:

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

Wenn wir eine Instanz dieser Klasse zu einem späteren Zeitpunkt erstellen, müssen wir in unserer öffnen SQLite.NET datenbankverbindung übergeben. Die `LoadOccupations` Methode fragt die Datenbank und die gefundenen Datensätze in den Arbeitsspeicher kopiert (mit unserem `OccupationModel` Datenmodell).

### <a name="creating-the-table-delegate"></a>Der Delegat für die Tabelle erstellen

Die endgültige-Klasse, die Sie benötigen ist eine benutzerdefinierte Tabelle-Delegat, der die Informationen anzuzeigen, die wir aus der Datenbank SQLite.NET geladen haben. Wir fügen Sie einen neuen `TableORMDelegate` unsere Projekt, und stellen sie wie folgt aussehen:

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

Hier verwenden wir die Datenquelle `Occupations` Sammlung (die wir aus der Datenbank SQLite.NET geladen) So füllen Sie die Spalten der Tabelle über die `GetViewForItem` Methode überschreiben.

### <a name="populating-the-table"></a>Auffüllen der Tabelle

Alle Teile vorhanden, wir Auffüllen unserer Tabelle, wenn es aus der Datei .xib, durch Überschreiben vergrößert werden der `AwakeFromNib` -Methode und somit zu suchen, wie folgt aus:

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

Zunächst greifen wir unsere SQLite.NET-Datenbank erstellen und sein Befüllen, wenn er nicht bereits vorhanden. Als Nächstes erstellen wir eine neue Instanz der Datenquelle unsere benutzerdefinierte Tabelle unsere datenbankverbindung übergeben, und wir fügen sie der Tabelle. Schließlich erstellen eine neue Instanz der benutzerdefinierten Tabelle Delegaten wir, in unserem Datenquelle übergeben, und fügen Sie es in der Tabelle.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit dem Datenbindung und Schlüssel-Wert-Codierung mit SQLite-Datenbanken in einer Anwendung Xamarin.Mac übernommen. Zuerst sucht er am Verfügbarmachen von einer c#-Klasse für Objective-C mit Schlüssel-Wert-Codierung (KVM), und beobachten von Schlüssel-Wert (KVO). Als Nächstes es wurde gezeigt, wie eine kompatible KVO-Klasse verwenden, und Daten an Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator gebunden wird. Der Artikel behandelt auch mit SQLite-Daten über die SQLite.NET ORM arbeiten und diese Daten in einer Tabelle anzeigen.



## <a name="related-links"></a>Verwandte Links

- [MacDatabase (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Standardsteuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederung-Ansichten](~/mac/user-interface/outline-view.md)
- [Auflistungsansichten](~/mac/user-interface/collection-view.md)
- [Schlüssel-Wert-Codierung Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Einführung in die Programmierung Themen Kakao-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in Kakao Bindungen Verweis](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [MacOS von menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
