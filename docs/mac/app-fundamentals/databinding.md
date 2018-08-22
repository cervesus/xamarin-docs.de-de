---
title: Die Datenbindung und Schlüssel / Wert-Codierung, xamarin.Mac
description: Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert beachtet werden, um die Datenbindung an Benutzeroberflächenelemente in Interface Builder von Xcode zu ermöglichen.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0adb8cda71ca8803c535679da2aecf00f3fa46a5
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251212"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Die Datenbindung und Schlüssel / Wert-Codierung, xamarin.Mac

_Dieser Artikel befasst sich mit der Codierung mit Schlüssel-Wert und Schlüssel-Wert beachtet werden, um die Datenbindung an Benutzeroberflächenelemente in Interface Builder von Xcode zu ermöglichen._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf den gleichen Schlüssel / Wert-Codierung und Datenbindungsverfahren, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ zum Binden von Daten mit UI-Elementen, statt Code zu schreiben.

Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihre Xamarin.Mac-Anwendung, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

[![Ein Beispiel für die ausgeführte app](databinding-images/intro01.png "ein Beispiel für die ausgeführte app")](databinding-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel / Wert-Codierung und der Datenbindung in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Was ist das Schlüssel / Wert-Codierung

Schlüssel / Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf die Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, statt den Zugriff über Instanzvariablen oder Methoden der eigenschaftenzugriffsmethode (`get/set`). Durch die Implementierung von Schlüssel-Wert-Codierung kompatibel Accessors in Ihre Xamarin.Mac-Anwendung, erhalten Sie Zugriff auf andere MacOS (früher als OS X)-Funktionen wie die Schlüssel-Wert beobachten (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und Scriptability.

Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihre Xamarin.Mac-Anwendung, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf. 

Beispielsweise sehen wir uns die folgende Klassendefinition eines kompatiblen KVM-Objekts:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Zunächst wird die `[Register("PersonModel")]` Attribut registriert die Klasse und legt Objective-c Klicken Sie dann die Klasse erben muss `NSObject` (oder eine Unterklasse, die von erbt `NSObject`), dadurch werden mehrere Basis-Methode, mit denen auf die Klasse KVM kompatibel sein. Als Nächstes die `[Export("Name")]` Attribut macht die `Name` Eigenschaft und den Schlüssel-Wert, der später verwendet wird, um Zugriff auf die Eigenschaft über KVM und KVO Techniken definiert. 

Schließlich um die in der Lage sein, beobachtet der Schlüssel-Wert ändert sich der Wert der Eigenschaft, die Zugriffsmethode muss umschließen Änderungen auf den Wert in `WillChangeValue` und `DidChangeValue` Methodenaufrufe (Angeben von dem gleichen Schlüssel wie die `Export` Attribut).  Zum Beispiel:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Dieser Schritt ist _sehr_ wichtig für die Datenbindung in Xcode die eine Interface Builder, (wie wir später in diesem Artikel sehen werden).

Weitere Informationen finden Sie unter Apple [Schlüssel-Wert-Codierung Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Schlüssel und Schlüsselpfade

Ein _Schlüssel_ ist eine Zeichenfolge, die eine bestimmte Eigenschaft eines Objekts identifiziert. Ein Schlüssel in der Regel entspricht der Name einer Accessor-Methode in einer Schlüssel-Wert-Codierung kompatibel Objekt. Schlüssel müssen ASCII-Codierung, verwenden Sie in der Regel mit einem Kleinbuchstaben beginnen und darf kein Leerzeichen enthalten. Angenommen im obigen Beispiel `Name` wäre ein Schlüssel-Wert, der `Name` Eigenschaft der `PersonModel` Klasse. Der Schlüssel und den Namen der Eigenschaft, die sie verfügbar machen müssen übereinstimmen, jedoch in den meisten Fällen sind.

Ein _Schlüsselpfad_ ist eine Zeichenfolge mit Punkt getrennte Schlüssel verwendet, um eine Hierarchie von Objekteigenschaften durchlaufen anzugeben. Die Eigenschaft den ersten Schlüssel in der Sequenz ist relativ zu der Empfänger ein, und jeder nachfolgende Schlüssel relativ zum Wert der vorherigen Eigenschaft ausgewertet wird. Auf die gleiche Weise verwenden Sie Punktnotation, um ein Objekt und seine Eigenschaften in einer C#-Klasse zu durchlaufen.

Wenn Sie erweitert, z. B. die `PersonModel` Klasse hinzugefügt, und wählen Sie `Child` Eigenschaft:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Der Pfad des untergeordneten Standortes Schlüssel wäre `self.Child.Name` oder einfach `Child.Name` (basierend auf wie die Schlüssel-Wert verwendet wurde).

### <a name="getting-values-using-key-value-coding"></a>Abrufen von Werten, die mithilfe von Schlüssel / Wert-Codierung

Die `ValueForKey` -Methode gibt den Wert für den angegebenen Schlüssel (wie eine `NSString`) im Verhältnis zu der Instanz der KVM-Klasse, die die Anforderung zu empfangen. Z. B. wenn `Person` ist eine Instanz der `PersonModel` oben definierten Klasse:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Diese Rückgabe des Werts, der die `Name` -Eigenschaft für diese Instanz von `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Festlegen von Werten, die mithilfe von Schlüssel / Wert-Codierung

Auf ähnliche Weise die `SetValueForKey` legen Sie den Wert für den angegebenen Schlüssel (wie eine `NSString`) im Verhältnis zu der Instanz der KVM-Klasse, die die Anforderung zu empfangen. In diesem Fall also mithilfe einer Instanz von der `PersonModel` Klasse, wie unten dargestellt:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Ändert den Wert des der `Name` Eigenschaft `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Beobachten von wertänderungen

Mithilfe von Schlüssel-Wert prüfen (KVO), Sie können einen Beobachter an einen bestimmten Schlüssel einer KVM-kompatible Klasse anfügen, und benachrichtigt werden, jedes Mal, wenn der Wert für diesen Schlüssel geändert wird (mithilfe von Techniken der KVM oder direkten Zugriff auf die angegebene Eigenschaft in C#-Code). Zum Beispiel:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Nun jederzeit die `Name` Eigenschaft der `Person` Instanz die `PersonModel` Klasse geändert wird, der neue Wert an die Konsole geschrieben wird. 

Weitere Informationen finden Sie unter Apple [Einführung in Schlüssel-Wert beachtet Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Datenbindung

In den folgenden Abschnitten werden angezeigt, wie Sie ein Schlüssel / Wert-Codierung und die Schlüssel-Wert-kompatible Klasse beobachten verwenden können, um Daten an Benutzeroberflächenelemente in Interface Builder von Xcode lesen und Schreiben von Werten, die mithilfe von C#-Code binden. Auf diese Weise trennen Sie Ihre _Datenmodell_ aus den Ansichten, die verwendet werden, um sie anzuzeigen, wodurch die Xamarin.Mac-Anwendung, flexibler und einfacher zu verwalten. Sie verringern erheblich die Menge des Codes, die geschrieben werden.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Definition des Datenmodells

Bevor Sie Daten binden ein Benutzeroberflächenelement in Interface Builder können, benötigen Sie eine kompatible KVM/KVO-Klasse, die in Ihre Xamarin.Mac-Anwendung, die als fungiert definiert die _Datenmodell_ für die Bindung. Das Datenmodell enthält alle Daten, die in der Benutzeroberfläche angezeigt, und empfängt keine Änderungen an den Daten, die der Benutzer in der Benutzeroberfläche vorgenommen werden, beim Ausführen der Anwendung.

Wenn Sie eine Anwendung schreiben, die eine Gruppe von Mitarbeitern verwaltet wurden, können Sie z. B. die folgende Klasse verwenden, um das Datenmodell zu definieren:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
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
    }
}
```

Die meisten Funktionen dieser Klasse behandelt wurden, der [neuerungen von Schlüssel / Wert-Codierung](#What_is_Key-Value_Coding) weiter oben. Allerdings sehen wir uns einige bestimmte Elemente und einige Ergänzungen, die vorgenommen wurden, damit diese Klasse fungiert als Datenmodell für kann **Array-Controllern** und **Struktur-Controller** (die wir später noch Mal mit Daten verwenden Binden Sie **Strukturansichten**, **Gliederungsansichten** und **Auflistungsansichten**).

Zuerst, da ein Mitarbeiter eines Managers sein könnte, wir haben verwendet eine `NSArray` (insbesondere ein `NSMutableArray` damit die Werte geändert werden können) auf die Mitarbeiter zu ermöglichen, die sie verwaltet, die an diese angefügt werden:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Zwei Aspekte zu beachten:

1. Wir verwendeten eine `NSMutableArray` anstatt ein standard C#-Array oder eine Auflistung, da dies eine Anforderung zum Binden von Daten, um AppKit-Steuerelementen, z. B. ist **Tabellenansichten**, **Gliederungsansichten** und **Sammlungen** .
2. Durch umwandeln, damit das Array von Angestellten verfügbar gemacht eine `NSArray` für Daten zu binden und geändert werden die C#-Namen, formatiert `People`, die die Datenbindung wird erwartet, `personModelArray` in Form **{Class_name} Array** (Hinweis: dass das erste Zeichen Kleinbuchstaben vorgenommen wurde).

Als Nächstes müssen wir einige speziell Namen öffentliche Methoden zur Unterstützung hinzufügen **Array-Controllern** und **Struktur-Controller**:

```csharp
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
```

Die Controller aus, um anzufordern, und ändern Sie die Daten, die sie anzeigen können. Wie Sie den verfügbar gemachten `NSArray` höher, Sie verfügen über eine sehr spezifische Namenskonvention gilt (die typische C#-Benennungskonventionen für unterscheidet):

- `addObject:` -Fügt ein Objekt in das Array an.
- `insertObject:in{class_name}ArrayAtIndex:` : Der Speicherort `{class_name}` ist der Name der Klasse. Diese Methode fügt ein Objekt in das Array am angegebenen Index.
- `removeObjectFrom{class_name}ArrayAtIndex:` : Der Speicherort `{class_name}` ist der Name der Klasse. Diese Methode entfernt das Objekt im Array am angegebenen Index.
- `set{class_name}Array:` : Der Speicherort `{class_name}` ist der Name der Klasse. Diese Methode können Sie die vorhandenen Carry durch eine neue zu ersetzen.

Innerhalb dieser Methoden nutzen, haben wir Änderungen an das Array in umschlossen `WillChangeValue` und `DidChangeValue` Nachrichten KVO Kompatibilität.

Schließlich, da die `Icon` Eigenschaft beruht auf den Wert des der `isManager` Eigenschaft, ändert sich in der `isManager` Eigenschaft kann nicht wiedergegeben werden, der `Icon` für Daten gebunden Benutzeroberflächenelemente (während der KVO):

```csharp
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
``` 

Um, die zu korrigieren, verwenden wir den folgenden Code:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Beachten Sie, dass zusätzlich zu seinen eigenen Schlüssel an, die `isManager` Accessor ist auch das Senden der `WillChangeValue` und `DidChangeValue` Nachrichten für die `Icon` Schlüssel, damit sie auch die Änderung angezeigt wird.

Wir verwenden die `PersonModel` Datenmodell im weiteren Verlauf dieses Artikels.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Einfache Datenbindung

Mit unserem Datenmodell definiert sehen wir uns ein einfaches Beispiel für die Datenbindung in Interface Builder von Xcode. Beispielsweise fügen Sie ein Formular für unsere Xamarin.Mac-Anwendung, die verwendet werden kann, so bearbeiten Sie die `PersonModel` , den wir oben definiert. Wir fügen einige Textfelder und ein Kontrollkästchen zum Anzeigen und Bearbeiten der Eigenschaften des Modells.

Zunächst fügen Sie ein neues **Ansichtscontroller** auf unsere **"Main.Storyboard"** in Interface Builder-Datei und nennen Sie die Klasse `SimpleViewController`: 

[![Hinzufügen eines neuen View Controllers](databinding-images/simple01.png "einen neuen View-Controller hinzufügen")](databinding-images/simple01-large.png#lightbox)

Anschließend zurück zu Visual Studio für Mac, bearbeiten Sie die **SimpleViewController.cs** Datei (die das Projekt automatisch hinzugefügt wurde) und Bereitstellen eine Instanz von der `PersonModel` , dass wir unser Formular zum Binden von Daten ist. Fügen Sie den folgenden Code hinzu:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Als Nächstes, wenn die Ansicht geladen wird, erstellen wir eine Instanz von unserem `PersonModel` und füllen Sie sie durch den folgenden Code:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Nun wir das Formular zu erstellen müssen, doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Layout des Formulars, das in etwa wie folgt aussehen:

[![Bearbeiten das Storyboard in Xcode](databinding-images/simple02.png "bearbeiten das Storyboard in Xcode")](databinding-images/simple02-large.png#lightbox)

An Daten binden Sie das Formular, um die `PersonModel` , die wir über verfügbar gemachte der `Person` Schlüssel, gehen Sie folgendermaßen vor:

1. Wählen Sie die **Mitarbeiternamen** Textfeld ein, und wechseln Sie zur der **Bindungsinspektor**.
2. Überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie als Nächstes `self.Person.Name` für die **Schlüsselpfad**: 

    [![Den Schlüsselpfad Eingabe](databinding-images/simple03.png "Schlüsselpfad eingeben")](databinding-images/simple03-large.png#lightbox)
3. Wählen Sie die **"Occupation"** Textfeld ein, und überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie als Nächstes `self.Person.Occupation` für die **Schlüsselpfad**:  

    [![Den Schlüsselpfad Eingabe](databinding-images/simple04.png "Schlüsselpfad eingeben")](databinding-images/simple04-large.png#lightbox)
4. Wählen Sie die **Mitarbeiter ist ein Manager** Kontrollkästchen, und überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie als Nächstes `self.Person.isManager` für die **Schlüsselpfad**:  

    [![Den Schlüsselpfad Eingabe](databinding-images/simple05.png "Schlüsselpfad eingeben")](databinding-images/simple05-large.png#lightbox)
5. Wählen Sie die **Anzahl von Mitarbeitern verwaltet** Textfeld ein, und überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie als Nächstes `self.Person.NumberOfEmployees` für die **Schlüsselpfad**:  

    [![Den Schlüsselpfad Eingabe](databinding-images/simple06.png "Schlüsselpfad eingeben")](databinding-images/simple06-large.png#lightbox)
6. Wenn der Mitarbeiter kein Manager ist, möchten wir die Anzahl von Mitarbeitern verwaltet Beschriftung und Textfeld auszublenden.
7. Wählen Sie die **Anzahl von Mitarbeitern verwaltet** Bezeichnung, erweitern Sie die **Hidden** Pfeil, und Überprüfen der **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie als Nächstes `self.Person.isManager` für die **Schlüsselpfad**:  

    [![Den Schlüsselpfad Eingabe](databinding-images/simple07.png "Schlüsselpfad eingeben")](databinding-images/simple07-large.png#lightbox)
8. Wählen Sie `NSNegateBoolean` aus der **Wert Transformator** Dropdownliste:  

    ![Auswählen der NSNegateBoolean-Key-Transformations](databinding-images/simple08.png "auswählen die NSNegateBoolean-Key-Transformation")
9. Dies weist die Datenbindung an, dass die Bezeichnung ausgeblendet, wenn der Wert des der `isManager` -Eigenschaft ist `false`.
10. Wiederholen Sie die Schritte 7 und 8 für die **Anzahl von Mitarbeitern verwaltet** Textfeld ein.
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Wenn das Ausführen der Anwendung, die Werte aus der `Person` Eigenschaft füllt automatisch das Formular:

[![Zeigt ein Formular automatisch](databinding-images/simple09.png "zeigt ein Formular automatisch")](databinding-images/simple09-large.png#lightbox)

Alle Änderungen, die die Benutzer können auf das Formular werden in zurückgeschrieben werden die `Person` -Eigenschaft in der View-Controller. Z. B. durch Aufheben der Auswahl **Mitarbeiter ist ein Manager** Updates der `Person` Instanz unserer `PersonModel` und die **Anzahl von Mitarbeitern verwaltet** Beschriftung und Textfeld automatisch (per ausgeblendet werden Datenbindung):

[![Ausblenden von der Anzahl von Mitarbeitern für nicht-Manager](databinding-images/simple10.png "Ausblenden von der Anzahl von Mitarbeitern für nicht-Manager")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Tabelle anzeigen, die Datenbindung

Nun, wir die Grundlagen der Datenbindung aus dem Weg haben, sehen wir uns eine komplexere Aufgabe des Daten-Bindung mit einer _Arraycontroller_ und Datenbindung zu einer Tabellenansicht. Weitere Informationen zum Arbeiten mit Tabellenansichten finden Sie unserem [Tabellenansichten](~/mac/user-interface/table-view.md) Dokumentation.

Zunächst fügen Sie ein neues **Ansichtscontroller** auf unsere **"Main.Storyboard"** in Interface Builder-Datei und nennen Sie die Klasse `TableViewController`:

[![Hinzufügen eines neuen View Controllers](databinding-images/table01.png "einen neuen View-Controller hinzufügen")](databinding-images/table01-large.png#lightbox)

Als Nächstes ermöglicht das Bearbeiten der **TableViewController.cs** -Datei (mit unserem Projekt automatisch hinzugefügt wurde) und machen ein Array (`NSArray`) der `PersonModel` Klassen, werden wir unser Formular zum Binden von Daten. Fügen Sie den folgenden Code hinzu:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Wie wir auf die `PersonModel` -Klasse weiter oben in der [Definition des Datenmodells](#Defining_your_Data_Model) Abschnitt haben wir vier speziell benannte öffentliche Methoden verfügbar, damit die Array-Controller und lesen und schreiben Daten in die Auflistung von `PersonModels`.

Als Nächstes müssen wir bei die Ansicht geladen wird, füllen Sie unser Array durch den folgenden Code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Nachdem wir unsere Tabellenansicht erstellen müssen, doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Layout der Tabelle, um in etwa wie folgt aussehen:

[![Anlegen einer neuen Tabellenansicht](databinding-images/table02.png "Anlegen einer neuen Tabellenansicht")](databinding-images/table02-large.png#lightbox)

Wir müssen ein **Arraycontroller** um gebundene Daten mit unserer Tabelle bereitzustellen, gehen Sie folgendermaßen vor:

1. Ziehen Sie ein **Array Controller** aus der **Bibliotheksinspektor** auf die **Schnittstellen-Editor**:  

    ![Auswählen eines Controllers Array aus der Bibliothek](databinding-images/table03.png "ein Controllers Array aus der Bibliothek auswählen")
2. Wählen Sie **Arraycontroller** in die **Schnittstellenhierarchie** und wechseln Sie zu der **Attributinspektor**:  

    [![Auswählen von Attributes Inspector](databinding-images/table04.png "Attributes Inspector auswählen")](databinding-images/table04-large.png#lightbox)
3. Geben Sie `PersonModel` für die **Klassenname**, klicken Sie auf die **Plus** Schaltfläche, und fügen Sie drei Product Keys. Nennen Sie diese `Name`, `Occupation` und `isManager`:  

    ![Hinzufügen der erforderlichen Schlüsselpfade](databinding-images/table05.png "Hinzufügen der erforderlichen Schlüsselpfade")
4. Dadurch wird der wie ein Array von verwalteten und welche Eigenschaften sie sollte verfügbar zu machen (über Schlüssel).
5. Wechseln Sie zu der **Bindungsinspektor** und wählen Sie unter **Content Array** wählen **Binden an** und **Tabellenansichtscontroller**. Geben Sie einen **modellieren Schlüsselpfad** von `self.personModelArray`:  

    ![Eingeben von einem Schlüsselpfad](databinding-images/table06.png "einen Schlüsselpfad eingeben")
6. Dies verbindet die Array-Controller, auf das Array von `PersonModels` , die wir für unsere View-Controller verfügbar gemacht werden.

Nun wir unsere Tabellenansicht mit dem Array-Controller zu binden müssen, führen Sie folgende Schritte aus:

1. Wählen Sie die Tabellenansicht und die **Bindung Inspektor**:  

    [![Auswählen des Bindung Inspektors](databinding-images/table07.png "den Inspektor Bindung auswählen")](databinding-images/table07-large.png#lightbox)
2. Unter den **Tabelleninhalte** Pfeil, wählen **Binden an** und **Arraycontroller**. Geben Sie `arrangedObjects` für die **Controller Schlüssel** Feld:  

    ![Definieren den Controller Schlüssel](databinding-images/table08.png "definieren die Controller-Taste")
3. Wählen Sie die **Tabellenansichtszelle** unter der **Mitarbeiter** Spalte. In der **Bindungsinspektor** unter der **Wert** Pfeil, wählen **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Name` für die **modellieren Schlüsselpfad**:  

    [![Festlegen des Modell-Schlüsselpfads](databinding-images/table09.png "Modell Schlüsselpfad festlegen")](databinding-images/table09-large.png#lightbox)
4. `objectValue` ist die aktuelle `PersonModel` im Array wird vom Netzwerkcontroller verwaltet wird das Array.
5. Wählen Sie die **Tabellenansichtszelle** unter der **"Occupation"** Spalte. In der **Bindungsinspektor** unter der **Wert** Pfeil, wählen **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Occupation` für die **modellieren Schlüsselpfad**:  

    [![Festlegen des Modell-Schlüsselpfads](databinding-images/table10.png "Modell Schlüsselpfad festlegen")](databinding-images/table10-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Wenn wir die Anwendung ausführen, wird in der Tabelle aufgefüllt werden, mit unserem Array von `PersonModels`:

[![Ausführen der Anwendung](databinding-images/table11.png "Ausführen der Anwendung")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Gliederung Anzeigen der Datenbindung

eine Datenbindung für eine Gliederungsansicht ähnelt eine Datenbindung für eine Tabellenansicht. Der Hauptunterschied besteht darin, die wir verwenden eine **Struktur-Controller** statt einer **Arraycontroller** der Gliederungsansicht an die gebundenen Daten anbieten. Weitere Informationen zum Arbeiten mit Gliederungsansichten informieren Sie sich unsere [Gliederungsansichten](~/mac/user-interface/outline-view.md) Dokumentation.

Zunächst fügen Sie ein neues **Ansichtscontroller** auf unsere **"Main.Storyboard"** in Interface Builder-Datei und nennen Sie die Klasse `OutlineViewController`: 

[![Hinzufügen eines neuen View Controllers](databinding-images/outline01.png "einen neuen View-Controller hinzufügen")](databinding-images/outline01-large.png#lightbox)

Als Nächstes ermöglicht das Bearbeiten der **OutlineViewController.cs** -Datei (mit unserem Projekt automatisch hinzugefügt wurde) und machen ein Array (`NSArray`) der `PersonModel` Klassen, werden wir unser Formular zum Binden von Daten. Fügen Sie den folgenden Code hinzu:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Wie wir auf die `PersonModel` -Klasse weiter oben in der [Definition des Datenmodells](#Defining_your_Data_Model) Abschnitt haben wir vier speziell benannte öffentliche Methoden verfügbar, damit die Struktur-Controller und lesen und schreiben Daten in die Auflistung von `PersonModels`.

Als Nächstes müssen wir bei die Ansicht geladen wird, füllen Sie unser Array durch den folgenden Code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Nachdem wir unsere Gliederungsansicht erstellen müssen, doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Layout der Tabelle, um in etwa wie folgt aussehen:

[![Erstellen die Gliederungsansicht](databinding-images/outline02.png "Gliederungsansicht erstellen")](databinding-images/outline02-large.png#lightbox)

Wir müssen ein **Struktur-Controller** um gebundene Daten zu unseren Gliederung bereitzustellen, gehen Sie folgendermaßen vor:

1. Ziehen Sie ein **Struktur-Controller** aus der **Bibliotheksinspektor** auf die **Schnittstellen-Editor**:  

    ![Wählen aus der Bibliothek einen Struktur-Controller](databinding-images/outline03.png "einen Struktur-Controller aus der Bibliothek auswählen")
2. Wählen Sie **Struktur-Controller** in die **Schnittstellenhierarchie** und wechseln Sie zu der **Attributinspektor**:  

    [![Auswählen der Attribute Inspector](databinding-images/outline04.png "Attribute Inspector auswählen")](databinding-images/outline04-large.png#lightbox)
3. Geben Sie `PersonModel` für die **Klassenname**, klicken Sie auf die **Plus** Schaltfläche, und fügen Sie drei Product Keys. Nennen Sie diese `Name`, `Occupation` und `isManager`:  

    ![Hinzufügen der erforderlichen Schlüsselpfade](databinding-images/outline05.png "Hinzufügen der erforderlichen Schlüsselpfade")
4. Dadurch wird die Struktur-Controller wie ein Array von verwalteten und welche Eigenschaften sie sollte verfügbar zu machen (über Schlüssel).
5. Unter den **Struktur-Controller** Geben Sie im Abschnitt `personModelArray` für **untergeordneten**, geben Sie `NumberOfEmployees` unter der **Anzahl** , und geben Sie `isEmployee` unter  **Blattelemente**:  

    ![Festlegen der Struktur-Controller-Schlüsselpfade](databinding-images/outline05.png "den Struktur-Controller-Schlüsselpfade festlegen")
6. Dies weist den Struktur-Controller, wo Sie alle untergeordneten Knoten, wie viele untergeordnete Knoten vorhanden sind und wenn der aktuelle Knoten über untergeordnete Knoten verfügt zu finden.
7. Wechseln Sie zu der **Bindungsinspektor** und wählen Sie unter **Content Array** wählen **Binden an** und **Besitzer der Datei**. Geben Sie einen **modellieren Schlüsselpfad** von `self.personModelArray`:  

    ![Bearbeiten den Schlüsselpfad](databinding-images/outline06.png "Schlüsselpfad bearbeiten")
8. Dies verbindet den Struktur-Controller, auf das Array von `PersonModels` , die wir für unsere View-Controller verfügbar gemacht werden.

Nun wir unsere Gliederungsansicht an den Struktur-Controller zu binden müssen, führen Sie folgende Schritte aus:

1. Auswählen der Gliederungsansicht an und klicken Sie in der **Bindung Inspektor** auswählen:  

    [![Auswählen des Bindung Inspektors](databinding-images/outline07.png "den Inspektor Bindung auswählen")](databinding-images/outline07-large.png#lightbox)
2. Unter den **Inhalt der Gliederung anzeigen** Pfeil, wählen **Binden an** und **Struktur-Controller**. Geben Sie `arrangedObjects` für die **Controller Schlüssel** Feld:  

    ![Festlegen des Controller-Schlüssels](databinding-images/outline08.png "Festlegen des Controller-Schlüssels")
3. Wählen Sie die **Tabellenansichtszelle** unter der **Mitarbeiter** Spalte. In der **Bindungsinspektor** unter der **Wert** Pfeil, wählen **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Name` für die **modellieren Schlüsselpfad**:  

    [![Eingeben des Modell-Schlüsselpfads](databinding-images/outline09.png "Modell Schlüsselpfad eingeben")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` ist die aktuelle `PersonModel` im Array, die von der Struktur-Controller verwaltet.
5. Wählen Sie die **Tabellenansichtszelle** unter der **"Occupation"** Spalte. In der **Bindungsinspektor** unter der **Wert** Pfeil, wählen **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Occupation` für die **modellieren Schlüsselpfad**:  

    [![Eingeben des Modell-Schlüsselpfads](databinding-images/outline10.png "Modell Schlüsselpfad eingeben")](databinding-images/outline10-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Wenn wir die Anwendung ausführen, wird die Gliederung mit unserem Array von aufgefüllt `PersonModels`:

[![Ausführen der Anwendung](databinding-images/outline11.png "Ausführen der Anwendung")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Datenbindung für Auflistung anzeigen

Datenbindung mit einer Auflistungsansicht ist ganz ähnlich wie die Bindung mit einer Tabellenansicht, wie ein Array-Controller verwendet wird, um Daten für die Sammlung bereitstellen. Da die Auflistungsansicht nicht über einen vordefinierten Anzeigeformat verfügt, sind mehr Schritte erforderlich, um Feedback für die Interaktion von Benutzern bereitzustellen und zum Nachverfolgen der Auswahl des Benutzers.

> [!IMPORTANT]
> Aufgrund eines Problems in Xcode 7 und Mac OS 10.11 (und höher) können Auflistungsansichten nicht in einem Storyboard (.storyboard)-Dateien verwendet werden. Daher müssen Sie weiterhin XIB-Dateien zur Definition Ihrer Sichten Auflistung für Ihre Xamarin.Mac-apps verwenden. Informieren Sie sich unsere [Auflistungsansichten](~/mac/user-interface/collection-view.md) Dokumentation zu informieren.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
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
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Debuggen native Abstürze

Einen Fehler gemacht haben Ihre datenbindungen kann dazu führen, eine _Native abstürzen_ in nicht verwaltetem Code und dazu führen, dass Ihre Xamarin.Mac-Anwendung mit vollständig ausfällt eine `SIGABRT` Fehler:

[![Beispiel eines nativen Absturzes-Dialogfelds](databinding-images/debug01.png "Beispiel eines nativen Absturzes-Dialogfelds")](databinding-images/debug01-large.png#lightbox)

Es gibt in der Regel vier wichtigsten Gründe für native Abstürze während der Datenbindung:

1. Ihr Datenmodell erbt nicht von `NSObject` oder eine Unterklasse von `NSObject`.
2. Sie Ihr Eigentum mit Objective-C nicht verfügbar machen die `[Export("key-name")]` Attribut.
3. Sie keine Änderungen an der Accessor, der Wert in umschließen `WillChangeValue` und `DidChangeValue` Methodenaufrufe (Angeben von dem gleichen Schlüssel wie die `Export` Attribut).
4. Sie haben einen falschen oder falsch Schlüssel in der **Bindung Inspektor** in Interface Builder.

### <a name="decoding-a-crash"></a>Decodieren von einem Absturz

Lassen Sie uns dazu führen, dass eines nativen Absturzes in unserer Datenbindung, sodass wir zeigen können, wie Sie suchen und zu beheben. In Interface Builder, ändern wir unsere Bindung der erste Bezeichnung im Beispiel Auflistungsansicht aus `Name` zu `Title`:

[![Bearbeiten den Bindungsschlüssel des](databinding-images/debug02.png "Bindungsschlüssel des bearbeiten")](databinding-images/debug02-large.png#lightbox)

Wir speichern Sie die Änderung, wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisieren und die Anwendung auszuführen. Wenn die Auflistungsansicht angezeigt wird, stürzt die Anwendung wird sofort mit einem `SIGABRT` Fehler (siehe die **Anwendungsausgabe** in Visual Studio für Mac) seit der `PersonModel` macht eine Eigenschaft mit dem Schlüssel `Title`:

[![Beispiel für einen Bindungsfehler](databinding-images/debug03.png "Beispiel für einen Bindungsfehler")](databinding-images/debug03-large.png#lightbox)

Wenn wir einen zu den Fehler in ganz oben Bildlauf die **Anwendungsausgabe** sehen wir die Taste, um das Problem zu lösen:

[![Suchen das Problem in das Fehlerprotokoll](databinding-images/debug04.png "suchen das Problem in das Fehlerprotokoll an")](databinding-images/debug04-large.png#lightbox)

Diese Zeile wird uns mitteilen, die den Schlüssel `Title` ist nicht vorhanden, auf das Objekt, das wir zu binden. Wenn wir ändern die Bindung zurück, in `Name` in Interface Builder, speichern, die Synchronisierung neu zu erstellen und ausführen, wird die Anwendung wie erwartet ohne Probleme ausgeführt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Datenbindung und Schlüssel-Wert zu codieren, in einer Xamarin.Mac-Anwendung verwendet. Zunächst sah im Endeffekt sehr auf das Verfügbarmachen von c#-Klasse mit Objective-C mithilfe von Schlüssel / Wert-Codierung (KVM), und beobachten von Schlüssel-Wert (KVO). Als Nächstes wurde erläutert, wie eine konforme KVO-Klasse verwenden, und bindet es Daten an Benutzeroberflächenelemente in Interface Builder von Xcode. Abschließend wurde erläutert, komplexe Bindung mit **Array-Controllern** und **Struktur-Controller**.


## <a name="related-links"></a>Verwandte Links

- [MacDatabinding Storyboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Standardsteuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Auflistungsansichten](~/mac/user-interface/collection-view.md)
- [Schlüssel-Wert-Codierung Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Einführung in die beobachten von Schlüssel-Wert-Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Einführung in den Themen zur Programmierung Cocoa-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in die Referenz für Cocoa-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [MacOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
