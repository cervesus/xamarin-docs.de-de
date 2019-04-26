---
title: Datenbanken in Xamarin.Mac
description: Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert beachtet werden, um die Datenbindung zwischen SQLite-Datenbanken und Elementen der Benutzeroberfläche in Interface Builder von Xcode zu ermöglichen. Hierin sind auch mit, dass die von SQLite.NET ORM bieten Zugriff auf SQLite-Daten.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 465b4a34d54dbee92461669b16c3b8a13188bbde
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378342"
---
# <a name="databases-in-xamarinmac"></a>Datenbanken in Xamarin.Mac

_Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert beachtet werden, um die Datenbindung zwischen SQLite-Datenbanken und Elementen der Benutzeroberfläche in Interface Builder von Xcode zu ermöglichen. Hierin sind auch mit, dass die von SQLite.NET ORM bieten Zugriff auf SQLite-Daten._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleiche SQLite-Datenbanken, die eine Xamarin.iOS oder Xamarin.Android-Anwendung zugreifen kann.

In diesem Artikel werden wir zwei Möglichkeiten, SQLite für den Datenzugriff zu folgenden Themen werden:

1. **Direkter Zugriff auf** – durch direkten Zugriff auf eine SQLite-Datenbank, können wir Daten aus der Datenbank für die Codierung von Schlüssel-Wert und die Datenbindung mit UI-Elementen, die in Interface Builder von Xcode erstellt. Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihre Xamarin.Mac-Anwendung, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf.
2. **Von SQLite.NET ORM** – mithilfe der open Source [von SQLite.NET](http://www.sqlite.org) Objekt Beziehung Manager (ORM), wir können die erforderliche Menge an Code zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank erheblich reduzieren. Diese Daten können dann zum Auffüllen eines Benutzeroberflächenelements wie z. B. eine Tabellenansicht verwendet werden.

[![Ein Beispiel für die ausgeführte app](databases-images/intro01.png "ein Beispiel für die ausgeführte app")](databases-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel / Wert-Codierung und Datenbindung mit SQLite-Datenbanken in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Da wir Schlüssel / Wert-Codierung und die Datenbindung verwenden, wenden Sie sich über die [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) zunächst als core-Methoden und Konzepte werden besprochen werden, die in dieser Dokumentation und der Beispielcode verwendet werden die Anwendung.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.

## <a name="direct-sqlite-access"></a>Direkter Zugriff auf die SQLite

Für die SQLite-Daten, die an UI-Elemente in Interface Builder von Xcode gebunden werden soll, wird dringend empfohlen, dass Sie auf die SQLite-Datenbank direkt (und nicht über eine Methode wie z. B. ein ORM) zugreifen, da Sie vollständige Kontrolle über die Möglichkeit haben die Daten geschrieben und gelesen wird  aus der Datenbank.

Wie in der wir gesehen haben die [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation die, mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihre Xamarin.Mac-Anwendung können Sie erheblich verringern, die Menge des Codes, die Sie schreiben müssen, und Behalten Sie zum Auffüllen und zum Arbeiten mit UI-Elemente. In Kombination mit direktem Zugriff auf eine SQLite-Datenbank können sie erheblich reduzieren den Umfang des Codes zum Lesen und Schreiben von Daten in der Datenbank erforderlich.

In diesem Artikel werden wir die Beispiel-app von Datenbindung und Schlüssel-Wert-Codierung-Dokument mit einer SQLite-Datenbank als die Quelle der Sicherung für die Bindung ändern können.

### <a name="including-sqlite-database-support"></a>Einschließlich Unterstützung für SQLite-Datenbank

Bevor wir fortfahren können, müssen wir unsere Anwendung Unterstützung für SQLite-Datenbanken hinzufügen, indem die einschließen von Verweisen auf eine Reihe von. Dateien, DLLs.

Führen Sie folgende Schritte aus:

1. In der **Lösungspad**, mit der rechten Maustaste auf die **Verweise** Ordner, und wählen **Verweise bearbeiten**.
2. Wählen Sie die Optionen der **Mono.Data.Sqlite** und **"System.Data"** Assemblys: 

    [![Hinzufügen der erforderlichen Verweise](databases-images/reference01.png "die erforderlichen Verweise hinzufügen")](databases-images/reference01-large.png#lightbox)
3. Klicken Sie auf die **OK** Schaltfläche, um die Änderungen zu speichern und die Verweise hinzuzufügen.

### <a name="modifying-the-data-model"></a>Ändern das Datenmodell

Nun, wir Unterstützung für den direkten Zugriff auf eine SQLite-Datenbank, um die Anwendung hinzugefügt haben, müssen wir unsere Data Model-Objekts zum Lesen und Schreiben von Daten aus der Datenbank (und geben Sie Schlüssel / Wert-Codierung und die Datenbindung) ändern. Im Fall von unserer beispielanwendung bearbeiten wir die **PersonModel.cs** Klasse, und stellen sie wie folgt aussehen:

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

Werfen wir einen Blick auf die Änderungen im folgenden ausführlicher beschrieben.

Wir haben zunächst hinzugefügt mehrere using-Anweisungen, die zum Verwenden von SQLite erforderlich sind, und wir haben eine Variable zum Speichern von unserer letzten Verbindungs mit der SQLite-Datenbank hinzugefügt:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Verwenden wir diese gespeicherte Verbindung, um alle Änderungen automatisch auf den Datensatz in der Datenbank zu speichern, wenn der Benutzer den Inhalt in der Benutzeroberfläche, über die Datenbindung ändert:

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

Alle Änderungen an der **Namen**, **"Occupation"** oder **IsManager** Eigenschaften werden an die Datenbank gesendet, wenn die Daten vor dem es gespeichert wurde (z. B. wenn die `_conn` Variable ist nicht `null`). Als Nächstes sehen wir uns die Methoden, die wir hinzugefügt haben **erstellen**, **Update**, **Load** und **löschen** Personen aus der Datenbank.

#### <a name="create-a-new-record"></a>Erstellen eines neuen Datensatzes

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

Wir verwenden eine `SQLiteCommand` des neuen Eintrags in der Datenbank zu erstellen. Wir erhalten einen neuen Befehl von der `SQLiteConnection` (conn), dass wir die Methode durch den Aufruf übergebene `CreateCommand`. Anschließend legen wir die SQL-Anweisung, um tatsächlich den neuen Datensatz, schreiben und Parameter für die tatsächlichen Werte:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Später legen wir die Werte für die Parameter dabei mit der `Parameters.AddWithValue` Methode für die `SQLiteCommand`. Mit Parametern, stellen wir sicher, dass Werte (z. B. ein einfaches Anführungszeichen) vor dem Senden zu SQLite ordnungsgemäß codiert zu erhalten. Beispiel:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Schließlich, da ein Benutzer kann ein Manager und eine von Mitarbeitern, die unter diesen Sammlung, werden wir rekursiv Aufrufen der `Create` Methode für diesen Personen in der Datenbank zu speichern:

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

Wie **erstellen** höher erhalten wir eine `SQLiteCommand` aus dem übergebenen in `SQLiteConnection`, und legen Sie unser SQL, unserem Datensatz (mit dem Parameter) zu aktualisieren:

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Wir geben Sie die Parameterwerte (Beispiel: `command.Parameters.AddWithValue ("@COL1", ID);`) und erneut rekursiv aufrufen, Updates für keines der untergeordneten Datensätze:

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

Da die Routine rekursiv von einem übergeordneten Objekt (z. B. eine Manager-Objekt, das Laden von ihren Mitarbeitern-Objekt) aufgerufen werden, kann, wurde spezieller Code hinzugefügt, um öffnen und schließen die Verbindung mit der Datenbank zu verarbeiten:

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

Wie immer legen wir unsere SQL rufen Sie den Datensatz und Parameter verwenden:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Schließlich verwenden wir ein Datenleser zum Ausführen der Abfrage und die Datensatzfelder zurück (die wir in die Instanz der Kopieren der `PersonModel` Klasse):

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

Wenn diese Person ein Manager ist, müssen wir auch alle ihre Mitarbeiter zu laden (in diesem Fall durch rekursiv aufrufen ihre `Load` Methode):

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

Hier bieten wir die SQL aus, um sowohl die Datensätze von Mitarbeitern unter diesem Manager (mit Parameter) als auch der Manager-Datensatz zu löschen:

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Nachdem der Datensatz entfernt wurde, deaktivieren Sie die aktuelle Instanz von der `PersonModel` Klasse:

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

Mit den Änderungen, unserem Datenmodell vorhanden sein, um lesen und Schreiben in die Datenbank zu unterstützen müssen wir eine Verbindung mit der Datenbank öffnen, und initialisieren Sie sie bei der ersten Ausführung. Fügen Sie den folgenden Code, um unsere **MainWindow.cs** Datei:

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

Werfen Sie einen Blick den oben dargestellten Code ein. Zunächst wählen wir einen Speicherort für die neue Datenbank (in diesem Beispiel den Desktop des Benutzers), suchen, um festzustellen, ob die Datenbank vorhanden ist, und wenn dies nicht der Fall, erstellen Sie ihn:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Als Nächstes erstellen wir das Herstellen einer Verbindung mit der Datenbank unter Verwendung des Pfads, die, dem wir zuvor erstellt haben:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Anschließend erstellen wir die SQL-Tabellen in der Datenbank, die wir benötigen:

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

Schließlich verwenden wir unser Datenmodell (`PersonModel`) erstellen Sie eine Reihe von Datensätzen für die die erste Datenbankzeit die Anwendung ausgeführt wird oder wenn die Datenbank nicht vorhanden ist:

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

Wenn die Anwendung gestartet wird, und es wird das Hauptfenster geöffnet, stellen wir eine Verbindung mit der Datenbank, die mit dem Code, dem wir oben hinzugefügt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Laden von gebundenen Daten

Mit allen Komponenten für den direkten Zugriff auf gebundene Daten aus einer SQLite-Datenbank vorhanden können wir laden die Daten in die verschiedenen Ansichten, die unsere Anwendung bereitgestellt und wird automatisch auf unserer Benutzeroberfläche angezeigt.

#### <a name="loading-a-single-record"></a>Laden einen einzelnen Datensatz

Zum Laden ein einzelnes Datensatzes dabei ist die ID kennen, verwenden wir den folgenden Code:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Laden alle Datensätze

Um alle Personen, unabhängig davon, laden sie ein Manager sind, verwenden Sie den folgenden Code:

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

Hier verwenden wir eine Überladung des Konstruktors für den `PersonModel` Klasse, um die Person, die in den Arbeitsspeicher geladen:

```csharp
var person = new PersonModel (_conn, childID);
```

Wir sind auch die datengebundene-Klasse, um die Person, die die Auflistung von Personen hinzufügen aufruft `AddPerson (person)`, dadurch wird sichergestellt, dass unserer Benutzeroberfläche erkennt die Änderung wird angezeigt:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Laden nur die Datensätze auf oberster Ebene

Um nur von Managern (z. B. zum Anzeigen von Daten in einer Gliederungsansicht) zu laden, verwenden wir den folgenden Code:

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

Der einzige wirkliche Unterschied in der in SQL-Anweisung (die nur Manager lädt `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), aber funktioniert genauso wie im obigen Abschnitt andernfalls.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Datenbanken und comboboxes

MacOS (z. B. das Kombinationsfeld) verfügbaren Steuerelemente im Menü der Dropdown-Liste entweder aus einer internen Liste aufgefüllt (die können vordefinierte in Interface Builder oder über Code aufgefüllt) festgelegt werden können oder durch Ihre eigenen benutzerdefinierten, externe Datenquelle bereitstellen. Finden Sie unter [Menü Steuerelementdaten bereitstellen](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) Weitere Details.

Als Beispiel fügen Sie im Beispiel oben im-Generator-Schnittstelle: einfache Datenbindung bearbeiten ein Kombinationsfeld, und machen Sie es mithilfe eines outlets mit dem Namen `EmployeeSelector`:

[![Verfügbarmachen von einem Kombinationsfeld Feld Outlet](databases-images/combo01.png "Outlet ein Kombinationsfeld-Feld verfügbar zu machen")](databases-images/combo01-large.png#lightbox)

In der **Attributes Inspector**, überprüfen Sie die **vervollständigt** und **Datenquelle verwendet** Eigenschaften:

![Konfigurieren die Attribute des Kombinationsfelds Feld](databases-images/combo02.png "Combo Box Attribute konfigurieren")

Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.

#### <a name="providing-combobox-data"></a>Bereitstellen von Daten für "ComboBox"

Als Nächstes fügen Sie eine neue Klasse mit dem Namen `ComboBoxDataSource` und legen Sie ihn wie folgt aussehen:

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

In diesem Beispiel erstellen wir ein neues `NSComboBoxDataSource` Kombinationsfeldelemente aus einer beliebigen Datenquelle SQLite vorweisen kann. Zuerst definieren wir die folgenden Eigenschaften:

- `Conn` -Ruft ab oder legt eine Verbindung mit der SQLite-Datenbank fest.
- `TableName` -Ruft ab oder legt den Namen der Tabelle.
- `IDField` -Ruft ab oder legt das Feld, der die eindeutige ID für die angegebene Tabelle fest. Der Standardwert ist `ID`.
- `DisplayField` -Ruft ab oder legt das Feld, das in der Dropdownliste angezeigt wird.
- `RecordCount` -Ruft die Anzahl der Datensätze in der angegebenen Tabelle.

Wenn wir eine neue Instanz des Objekts erstellen, übergeben wir die Verbindung, Tabellenname, optional das ID-Feld und das Anzeigen eines Felds:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

Die `GetRecordCount` Methode gibt die Anzahl der Datensätze in der angegebenen Tabelle zurück:

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

Es heißt, jedes Mal die `TableName`, `IDField` oder `DisplayField` Eigenschaften geändert wird.

Die `IDForIndex` Methode gibt die eindeutige ID (`IDField`) für den Datensatz an den Elementindex der angegebenen Dropdown-Liste: 

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

Die `ValueForIndex` -Methode gibt den Wert (`DisplayField`) für das Element am Index angegebenen Dropdown-Liste:

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

Die `ObjectValueForItem` Methode stellt den Wert (`DisplayField`) für den Elementindex der angegebenen Dropdown-Liste:

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

Beachten Sie, die wir verwenden die `LIMIT` und `OFFSET` Anweisungen in unserem SQLite-Befehl, um, die einen Datensatz zu beschränken, dass wir erforderlich sind.

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

Wenn der Wert kann nicht gefunden werden, die `NSRange.NotFound` Wert wird zurückgegeben, und alle Elemente in der Dropdownliste deaktiviert werden.

Die `CompletedString` Methode gibt den ersten übereinstimmenden Wert (`DisplayField`) für einen teilweise typisierte Eintrag:

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

Um alle Teile zusammenzuführen, bearbeiten Sie die `SubviewSimpleBindingController` und legen Sie ihn wie folgt aussehen:

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

Die `DataSource` Eigenschaft verfügt über eine Tastenkombination, die `ComboBoxDataSource` (oben erstellt) auf das Kombinationsfeld angefügt.

Die `LoadSelectedPerson` Methode lädt die Person, die aus der Datenbank für den angegebenen eindeutigen Bezeichner:

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

In der `AwakeFromNib` methodenüberschreibung, zunächst fügen wir eine Instanz von der Datenquelle der benutzerdefinierten Kombinationsfeld im:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Als Nächstes, wir Antworten an den Benutzer, der den Textwert des Kombinationsfelds bearbeitet, durch die zugeordnete eindeutige ID zu suchen (`IDField`) der Daten darstellen, und laden die angegebene Person ein, wenn gefunden:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Wir laden Sie eine neue Person, wenn der Benutzer ein neues Element in der Dropdownliste auswählt:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Zum Schluss Auffüllen wir das Kombinationsfeld und angezeigten Person mit dem ersten Element in der Liste:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Wie oben angegeben, mit der open Source [von SQLite.NET](http://www.sqlite.org) Objekt Beziehung Manager (ORM), wir können die erforderliche Menge an Code zum Lesen und Schreiben von Daten aus einer SQLite-Datenbank erheblich reduzieren. Dies möglicherweise nicht die beste Route an, die beim Binden von Daten aufgrund von mehreren Anforderungen, die Schlüssel / Wert-Codierung und der Datenbindung für ein Objekt zu platzieren.

Gemäß der Website von SQLite.Net _"SQLite ist eine Softwarebibliothek, die eine eigenständige, serverlose, ohne Konfigurationsaufwand, transaktionale SQL-Datenbank-Engine implementiert. SQLite ist die am häufigsten bereitgestellten Datenbank-Engine in der ganzen Welt. Der Quellcode für SQLite ist in der öffentlichen Domäne."_

In den folgenden Abschnitten zeigen wir, wie Sie von SQLite.Net verwenden, um Daten für eine Tabellenansicht bereitzustellen.

### <a name="including-the-sqlitenet-nuget"></a>Einschließlich des NuGet-Pakets von SQLite.net

Von SQLite.NET wird als NuGet-Paket angezeigt, die Sie in der Anwendung einschließen. Vor dem Verwenden von SQLite.NET datenbankunterstützung hinzufügen können, müssen wir diesem Paket enthalten.

Gehen Sie zum Hinzufügen des Pakets:

1. In der **Lösungspad**, mit der rechten Maustaste die **Pakete** Ordner, und wählen **Pakete hinzufügen...**
2. Geben Sie `SQLite.net` in die **Suchfeld** , und wählen Sie die **Sqlite-Net-** Eintrag:

    [![Hinzufügen des SQLite-NuGet-Pakets](databases-images/nuget01.png "des SQLite-NuGet-Pakets hinzufügen")](databases-images/nuget01-large.png#lightbox)
3. Klicken Sie auf die **Paket hinzufügen** Schaltfläche, um den Vorgang abzuschließen.

### <a name="creating-the-data-model"></a>Erstellen des Datenmodells

Wir fügen Sie dem Projekt eine neue Klasse und rufen Sie in `OccupationModel`. Als Nächstes bearbeiten wir die **OccupationModel.cs** Datei, und stellen sie wie folgt aussehen:

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

Zunächst schließen wir von SQLite.NET (`using Sqlite`), klicken Sie dann mehrere Eigenschaften verfügbar, von denen jede geschrieben in die Datenbank wird dieser Datensatz gespeichert wird. Die erste Eigenschaft, die wir als Primärschlüssel vornehmen und es wie folgt auf die automatische Erhöhung festgelegt:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Initialisieren der Datenbank

Mit den Änderungen, unserem Datenmodell vorhanden sein, um lesen und Schreiben in die Datenbank zu unterstützen müssen wir eine Verbindung mit der Datenbank öffnen, und initialisieren Sie sie bei der ersten Ausführung. Fügen Sie den folgenden Code ein:

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

Zunächst erhalten einen Pfad zu der Datenbank (dem Desktop des Benutzers in diesem Fall) und festzustellen, ob die Datenbank bereits vorhanden ist:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Als Nächstes erstellen wir eine Verbindung mit der Datenbank unter dem Pfad, die, dem wir zuvor erstellt haben:

```csharp
var conn = new SQLiteConnection (db);
```

Schließlich erstellen wir in der Tabelle und fügen Sie einige Standard-Einträge hinzu:

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

Als Beispiel fügen wir eine Tabellenansicht zu unserer Benutzeroberfläche in Interface Builder von Xcode. Wir werden in dieser Ansicht der Tabelle über eine Steckdose verfügbar zu machen (`OccupationTable`), damit wir sie über die c#-Code zugreifen können:

[![Verfügbarmachen von einer Tabelle anzeigen Outlet](databases-images/table01.png "verfügbar machen eine Tabelle Ansicht an")](databases-images/table01-large.png#lightbox)

Als Nächstes fügen wir die benutzerdefinierten Klassen, um diese Tabelle mit Daten aus der Datenbank von SQLite.NET aufzufüllen.

### <a name="creating-the-table-data-source"></a>Erstellen der Tabelle-Datenquelle

Wir erstellen eine benutzerdefinierte Datenquelle zum Bereitstellen von Daten für die Tabelle ein. Fügen Sie zunächst eine neue Klasse namens `TableORMDatasource` und legen Sie ihn wie folgt aussehen:

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

Wenn wir eine Instanz dieser Klasse zu einem späteren Zeitpunkt erstellen, werden wir in unserem open von SQLite.NET datenbankverbindung übergeben. Die `LoadOccupations` Methode fragt die Datenbank und die gefundenen Datensätze in den Arbeitsspeicher kopiert (mit unserer `OccupationModel` Datenmodell).

### <a name="creating-the-table-delegate"></a>Der Delegat für die Tabelle erstellen

Die endgültige Klasse benötigten ist es sich um einen benutzerdefinierten Delegaten der Tabelle zum Anzeigen der Informationen, die wir aus der Datenbank von SQLite.NET geladen haben. Fügen Sie ein neues `TableORMDelegate` auf das Projekt und legen Sie ihn wie folgt aussehen:

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

Hier verwenden wir die Datenquelle `Occupations` Sammlung (die wir aus der Datenbank von SQLite.NET geladen), geben Sie in den Spalten der Tabelle über die `GetViewForItem` Methode außer Kraft setzen.

### <a name="populating-the-table"></a>Auffüllen der Tabelle

Alle Komponenten beisammen, lassen Sie uns Auffüllen der Tabelle, wenn sie aus der XIB-Datei, durch Überschreiben vergrößert werden der `AwakeFromNib` -Methode und somit zu sehen, wie im folgenden Beispiel:

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

Zunächst greifen wir unsere Datenbank von SQLite.NET erstellen und befüllen, wenn er nicht bereits vorhanden. Als Nächstes erstellen wir eine neue Instanz der benutzerdefinierten Tabelle-Datenquelle, übergeben Sie die datenbankverbindung und wir sie an der Tabelle anfügen. Schließlich erstellen eine neue Instanz der unserer benutzerdefinierten Delegaten für die Tabelle wir, übergeben in der Datenquelle, und fügen Sie es an der Tabelle.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Datenbindung und Schlüssel / Wert-Codierung, mit SQLite-Datenbanken in einer Xamarin.Mac-Anwendung verwendet. Zunächst sah im Endeffekt sehr auf das Verfügbarmachen von c#-Klasse mit Objective-C mithilfe von Schlüssel / Wert-Codierung (KVM), und beobachten von Schlüssel-Wert (KVO). Als Nächstes wurde erläutert, wie eine konforme KVO-Klasse verwenden, und bindet es Daten an Benutzeroberflächenelemente in Interface Builder von Xcode. Der Artikel behandelt auch arbeiten mit SQLite-Daten über die von SQLite.NET ORM und Anzeigen von Daten in einer Tabellenansicht.



## <a name="related-links"></a>Verwandte Links

- [MacDatabase (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Die Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Standardsteuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Auflistungsansichten](~/mac/user-interface/collection-view.md)
- [Schlüssel-Wert-Codierung Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Einführung in den Themen zur Programmierung Cocoa-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in die Referenz für Cocoa-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
