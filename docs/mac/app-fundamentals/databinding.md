---
title: Datenbindung und Schlüssel-Wert-Codierung in Xamarin.Mac
description: Dieser Artikel behandelt mit Codierung mit Schlüssel-Wert und Schlüssel-Wert prüfen, um Daten Anbindung an Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator zu ermöglichen.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 88567e47f488a94fcf7334584a678c9689b83306
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792137"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Datenbindung und Schlüssel-Wert-Codierung in Xamarin.Mac

_Dieser Artikel behandelt mit Codierung mit Schlüssel-Wert und Schlüssel-Wert prüfen, um Daten Anbindung an Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator zu ermöglichen._

## <a name="overview"></a>Überblick

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf den gleichen Schlüssel-Wert-Codierung und die Daten Bindung Techniken, die ein Entwickler arbeiten in *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zum Binden von Daten mit Benutzeroberflächenelemente keinen Code schreiben.

Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

[![Ein Beispiel für die ausgeführte app](databinding-images/intro01.png "ein Beispiel für die ausgeführte app")](databinding-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel-Wert zu codieren und die Datenbindung in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Was ist die Codierung von Schlüssel-Wert

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, anstatt über Nachrichteninstanzvariablen auf Sie zuzugreifen oder Methoden des Eigenschaftenaccessors (`get/set`). Durch die Implementierung von Schlüssel-Wert-Codierung kompatibel Accessors in Ihrer Anwendung Xamarin.Mac, haben Sie Zugriff auf andere MacOS (ehemals OS X)-Funktionen, z. B. Schlüssel-Wert beachten (KVO), die Datenbindung, Core, Kakao Bindungen und Scriptability.

Mithilfe von Schlüssel-Wert-Codierung und Datenbindungsmethoden in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf. 

Zum Beispiel betrachten wir die folgende Klassendefinition eines kompatiblen KVM-Objekts:

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

Zunächst wird der `[Register("PersonModel")]` Attribut registriert die Klasse und verfügbar gemacht wird, Objective-C. Anschließend muss die Klasse erben `NSObject` (oder eine Unterklasse, die von erben `NSObject`), dies fügt mehrere Basis-Methode, die auf die Klasse zu ermöglichen, KVM-kompatibel sein. Als Nächstes wird die `[Export("Name")]` Attribut macht die `Name` Eigenschaft und definiert den Schlüssel-Wert, die später zum Zugriff auf die Eigenschaft über KVM und KVO Techniken verwendet werden. 

Schließlich, um in der Lage sein, beachtet der Schlüssel-Wert ändert sich in den Wert der Eigenschaft, die Zugriffsmethode muss umschließen Änderungen auf den Wert in `WillChangeValue` und `DidChangeValue` Methodenaufrufe (Angeben von dem gleichen Schlüssel wie die `Export` Attribut).  Zum Beispiel:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Dieser Schritt ist _sehr_ wichtig für die Datenbindung in Xcode des eine Schnittstelle-Generator, (Siehe wir später in diesem Artikel wird).

Weitere Informationen finden Sie in der Apple- [Schlüssel-Wert-Codierung Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Schlüssel und Schlüsselpfade

Ein _Schlüssel_ ist eine Zeichenfolge, die eine bestimmte Eigenschaft eines Objekts identifiziert. Ein Schlüssel in der Regel entspricht der Name einer Accessor-Methode in einem Schlüssel-Wert-Codierung kompatibel Objekt. Schlüssel müssen ASCII-Codierung verwenden, in der Regel mit einem Kleinbuchstaben beginnen und darf keine Leerzeichen enthalten. Angenommen, die oben aufgeführten Beispiel `Name` wäre ein Schlüssel-Wert, der `Name` Eigenschaft von der `PersonModel` Klasse. Der Schlüssel und den Namen der Eigenschaft, die sie verfügbar machen müssen übereinstimmen, jedoch in den meisten Fällen stehen.

Ein _Schlüsselpfad_ ist eine Zeichenfolge der Punkt getrennte Schlüssel verwendet, um eine Hierarchie von Objekteigenschaften durchlaufen anzugeben. Die Eigenschaft den ersten Schlüssel in der Sequenz ist, relativ zum Empfänger, und jeder nachfolgende Schlüssel relativ zum Wert der vorherigen Eigenschaft ausgewertet wird. Auf die gleiche Weise verwenden Sie Punktnotation, um ein Objekt und dessen Eigenschaften in einer C#-Klasse zu durchlaufen.

Wenn Sie erweitert beispielsweise die `PersonModel` Klasse hinzugefügt, und wählen Sie `Child` Eigenschaft:

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

Der Schlüsselpfad, der Name der untergeordneten wäre `self.Child.Name` oder einfach `Child.Name` (basierend auf wie die Schlüssel-Wert verwendet wurde).

### <a name="getting-values-using-key-value-coding"></a>Abrufen von Werten mithilfe von Schlüssel-Wert-Codierung

Die `ValueForKey` -Methode gibt den Wert für den angegebenen Schlüssel (als eine `NSString`), relativ zu der Instanz die KVM-Klasse, die die Anforderung zu empfangen. Z. B. wenn `Person` ist eine Instanz der `PersonModel` oben definierte Klasse:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Dies würde der Rückgabewert von der `Name` Eigenschaft für diese Instanz von `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Festlegen von Werten, die mithilfe von Schlüssel-Wert-Codierung

Auf ähnliche Weise die `SetValueForKey` legen Sie den Wert für den angegebenen Schlüssel (als eine `NSString`), relativ zu der Instanz die KVM-Klasse, die die Anforderung zu empfangen. Daher erneut mit einer Instanz vom die `PersonModel` Klasse, wie unten dargestellt:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Würde die Änderung des Werts der `Name` Eigenschaft `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Beobachten von wertänderungen

Beobachten von Schlüssel-Wert (KVO) verwenden, Sie können eine "Beobachter" auf einen bestimmten Schlüssel einer kompatiblen KVM-Klasse zuordnen und jedes Mal, wenn der Wert für diesen Schlüssel geändert wird (mit den KVM-Verfahren oder direkten Zugriff auf die angegebene Eigenschaft in C#-Code) benachrichtigt werden. Zum Beispiel:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Jetzt jederzeit die `Name` Eigenschaft von der `Person` Instanz von der `PersonModel` Klasse geändert wird, der neue Wert in die Konsole geschrieben wird. 

Weitere Informationen finden Sie in der Apple- [Einführung in die Schlüssel-Wert beobachten Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Datenbindung

In den folgenden Abschnitten erfahren, wie Sie eine Codierung von Schlüssel-Wert und Schlüssel-Wert-kompatible Klasse beobachten zum Binden von Daten an Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator, anstatt zu lesen und Schreiben von Werten, die mit C#-Code verwenden können. Trennen Sie auf diese Weise Ihre _Datenmodell_ aus den Ansichten, die verwendet werden, um sie anzuzeigen, sodass Sie Xamarin.Mac flexibler und einfacher zu verwalten. Sie verringert erheblich den Umfang des Codes, die geschrieben werden.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Definieren Ihr Datenmodell

Bevor Sie Daten zu binden ein Benutzeroberflächenautomatisierungs-Elements in der Benutzeroberflächen-Generator können, benötigen Sie eine kompatible KVM/KVO-Klasse, die in Ihrer Anwendung Xamarin.Mac fungieren als definiert die _Datenmodell_ für die Bindung. Das Datenmodell enthält alle Daten, die in der Benutzeroberfläche angezeigt und alle Änderungen an den Daten, mit der der Benutzer in der Benutzeroberfläche während der Ausführung der Anwendung, empfängt.

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

Wurden die meisten Funktionen dieser Klasse in behandelt die [was Codierung von Schlüssel-Wert ist](#What_is_Key-Value_Coding) obigen Abschnitt. Allerdings sehen wir uns einige bestimmte Elemente und einigen Zusätzen, die vorgenommen wurden, können diese Klasse fungiert als Datenmodell für **Array Controller** und **Struktur-Controller** (die wir später an Daten verwenden Binden **Strukturansichten**, **Gliederung Ansichten** und **Auflistungsansichten**).

Zuerst, da ein Mitarbeiter ein Manager sein kann, wir haben verwendet eine `NSArray` (insbesondere eine `NSMutableArray` die Werte nicht geändert werden können) um die Mitarbeiter zu ermöglichen, die sie an diese angefügt werden verwaltet:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Zwei Dinge zu beachten:

1. Wir verwendet eine `NSMutableArray` anstatt ein standard C#-Arrays oder einer Auflistung, da dies eine Anforderung zum Binden von Daten an Steuerelemente AppKit, z. B. ist **Tabellensichten**, **Gliederung Ansichten** und **Sammlungen** .
2. Das Array der Mitarbeiter durch Umwandlung damit verfügbar gemacht eine `NSArray` für Daten zu binden und geändert werden die C#-Name, formatiert `People`, hinzu, die die Datenbindung erwartet dass, `personModelArray` in Form **{Class_name} Array** (Beachten Sie dass das erste Zeichen Kleinbuchstaben vorgenommen wurde).

Als Nächstes müssen wir einige speziell Namen öffentliche Methoden zur Unterstützung hinzufügen **Array Controller** und **Struktur-Controller**:

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

Die Controller anfordern, und ändern Sie die Daten, die sie anzeigen können. Wie die verfügbar gemachten `NSArray` oben beschrieben, verfügen über eine sehr spezifische Benennungskonvention (die von den normalen C#-Benennungskonventionen zu unterscheidet):

- `addObject:` -Fügt ein Objekt in das Array an.
- `insertObject:in{class_name}ArrayAtIndex:` -Wo `{class_name}` ist der Name der Klasse. Diese Methode fügt ein Objekt im Array am angegebenen Index.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Wo `{class_name}` ist der Name der Klasse. Diese Methode entfernt das Objekt im Array am angegebenen Index.
- `set{class_name}Array:` -Wo `{class_name}` ist der Name der Klasse. Diese Methode können Sie vorhandene Carry durch einen neuen zu ersetzen.

Innerhalb dieser Methoden haben wir umschlossen, Änderungen an das Array im `WillChangeValue` und `DidChangeValue` Nachrichten KVO-Kompatibilität.

Schließlich, da die `Icon` Eigenschaft basiert auf dem Wert des der `isManager` -Eigenschaft, Änderungen an der `isManager` Eigenschaft möglicherweise nicht wiedergegeben werden, der `Icon` für Daten gebunden Benutzeroberflächenelemente (während KVO):

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

Um, die zu beheben, verwenden Sie folgenden Code:

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

Beachten Sie, dass zusätzlich zu seiner eigenen Schlüssel der `isManager` Accessor auch sendet die `WillChangeValue` und `DidChangeValue` Nachrichten für die `Icon` Schlüssel, damit es als auch die Änderung angezeigt wird.

Wir verwenden die `PersonModel` Datenmodell im weiteren Verlauf dieses Artikels.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Einfache Datenbindung

Mit unseren Datenmodell definiert sehen wir uns ein einfaches Beispiel für die Datenbindung in Xcodes Benutzeroberflächen-Generator. Fügen z. B. ein Formular für unsere Xamarin.Mac-Anwendung, die verwendet werden kann, so bearbeiten Sie die `PersonModel` , die wir zuvor definierten. Wir werden einige Textfelder und ein Kontrollkästchen zum Anzeigen und bearbeiten Eigenschaften von unserem Modell hinzufügen.

Zunächst sehen wir fügen Sie einen neuen **View Controller** auf unserer **Main.storyboard** -Datei im Benutzeroberflächen-Generator, und benennen Sie die Klasse `SimpleViewController`: 

[![Hinzufügen eines neuen Sicht Controllers](databinding-images/simple01.png "Hinzufügen eines neuen Ansicht-Controllers")](databinding-images/simple01-large.png#lightbox)

Anschließend zurück zu Visual Studio für Mac, bearbeiten Sie die **SimpleViewController.cs** Datei (das Projekt automatisch hinzugefügt wurde) und Verfügbarmachen von einer Instanz von der `PersonModel` , dass wir unser Formular zum Binden von Daten ist. Fügen Sie den folgenden Code hinzu:

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

Als Nächstes beim Laden der Ansicht erstellen wir eine Instanz von unseren `PersonModel` und füllen sie mit dem folgenden Code:

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

Nachdem wir unser Formular erstellen müssen, doppelklicken Sie auf die **Main.storyboard** Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Layout das Formular an in etwa wie folgt aussehen:

[![Bearbeiten das Storyboard in Xcode](databinding-images/simple02.png "bearbeiten das Storyboard in Xcode")](databinding-images/simple02-large.png#lightbox)

An Daten binden Sie das Formular, um die `PersonModel` , die wir über verfügbar gemachte der `Person` Schlüssel, gehen Sie folgendermaßen vor:

1. Wählen Sie die **Mitarbeiternamen** Textfeld und wechseln Sie in der **Bindungen Inspektor**.
2. Überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie anschließend `self.Person.Name` für die **Schlüsselpfad**: 

    [![Der Schlüsselpfad eingeben](databinding-images/simple03.png "Schlüsselpfad eingeben")](databinding-images/simple03-large.png#lightbox)
3. Wählen Sie die **Beruf** Textfeld, und Überprüfen der **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie anschließend `self.Person.Occupation` für die **Schlüsselpfad**:  

    [![Der Schlüsselpfad eingeben](databinding-images/simple04.png "Schlüsselpfad eingeben")](databinding-images/simple04-large.png#lightbox)
4. Wählen Sie die **Mitarbeiter ist ein Manager** Kontrollkästchen, und überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie anschließend `self.Person.isManager` für die **Schlüsselpfad**:  

    [![Der Schlüsselpfad eingeben](databinding-images/simple05.png "Schlüsselpfad eingeben")](databinding-images/simple05-large.png#lightbox)
5. Wählen Sie die **Anzahl von Mitarbeitern verwalteten** Textfeld, und Überprüfen der **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie anschließend `self.Person.NumberOfEmployees` für die **Schlüsselpfad**:  

    [![Der Schlüsselpfad eingeben](databinding-images/simple06.png "Schlüsselpfad eingeben")](databinding-images/simple06-large.png#lightbox)
6. Wenn der Mitarbeiter, die kein Manager ist, möchten wir die Anzahl der Mitarbeiter verwaltet Bezeichnung und Text-Feld ausblenden.
7. Wählen Sie die **Anzahl von Mitarbeitern verwaltet** Bezeichnung, erweitern Sie die **Hidden** Turndown und überprüfen Sie die **Binden an** und wählen Sie **einfache View-Controller** aus der Dropdownliste aus. Geben Sie anschließend `self.Person.isManager` für die **Schlüsselpfad**:  

    [![Der Schlüsselpfad eingeben](databinding-images/simple07.png "Schlüsselpfad eingeben")](databinding-images/simple07-large.png#lightbox)
8. Wählen Sie `NSNegateBoolean` aus der **Wert Transformer** Dropdownliste:  

    ![Auswählen der NSNegateBoolean-Key-Transformations](databinding-images/simple08.png "NSNegateBoolean-Key-Transformation auswählen")
9. Dies weist die Datenbindung an, dass die Bezeichnung ausgeblendet werden, wenn der Wert von der `isManager` Eigenschaft ist `false`.
10. Wiederholen Sie die Schritte 7 und 8 für die **Anzahl von Mitarbeitern verwalteten** Textfeld.
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Wenn das Ausführen der Anwendung, die Werte aus den `Person` Auffüllen-Eigenschaft automatisch unser Formular:

[![Zeigt ein automatisch ausgefülltes Formular](databinding-images/simple09.png "ein automatisch ausgefülltes Formular anzeigen")](databinding-images/simple09-large.png#lightbox)

Alle Änderungen, die der Benutzer stellt dem Formular werden zurückgeschrieben werden die `Person` Eigenschaft in die View-Controller. Z. B. durch Aufheben der Auswahl **Mitarbeiter ist ein Manager** Updates der `Person` Instanz von unserer `PersonModel` und die **Anzahl von Mitarbeitern verwalteten** Bezeichnung und Textfeld werden automatisch (über ausgeblendet die Datenbindung):

[![Ausblenden von der Anzahl von Mitarbeitern für nicht-Abteilungsleiter](databinding-images/simple10.png "zum Ausblenden von der Anzahl von Mitarbeitern für nicht-Abteilungsleiter")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Tabelle anzeigen, die Datenbindung

Nun mit den Grundlagen der aus dem Weg zum Binden von Daten, betrachten wir eine komplexere Daten Bindung Aufgabe mithilfe einer _Array Controller_ und Binden von Daten an eine Tabelle anzeigen. Weitere Informationen zum Arbeiten mit Tabellensichten finden Sie unter unsere [Tabellensichten](~/mac/user-interface/table-view.md) Dokumentation.

Zunächst sehen wir fügen Sie einen neuen **View Controller** auf unserer **Main.storyboard** -Datei im Benutzeroberflächen-Generator, und benennen Sie die Klasse `TableViewController`:

[![Hinzufügen eines neuen Sicht Controllers](databinding-images/table01.png "Hinzufügen eines neuen Ansicht-Controllers")](databinding-images/table01-large.png#lightbox)

Als Nächstes ermöglicht das Bearbeiten der **TableViewController.cs** Datei (, unsere Projekt automatisch hinzugefügt wurde), und machen Sie ein Array (`NSArray`) der `PersonModel` Klassen, dass wir unser Formular zum Binden von Daten ist. Fügen Sie den folgenden Code hinzu:

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

Wie haben wir auf die `PersonModel` -Klasse oben in der [definieren Ihr Datenmodell](#Defining_your_Data_Model) Abschnitt haben vier speziell benannte öffentliche Methoden verfügbar gemacht, damit die Array-Controller Lese- und Schreibzugriff Daten aus der Auflistung von `PersonModels`.

Als Nächstes müssen wir beim Laden der Sicht Auffüllen unserer Array mit dem folgenden Code:

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

Nachdem wir unsere Tabellenansicht erstellen müssen, doppelklicken Sie auf die **Main.storyboard** Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Layout der Tabelle, die in etwa wie folgt aussehen:

[![Layouts mit einer neuen Tabellenansicht](databinding-images/table02.png "Layouts einer neuen Tabellenansicht")](databinding-images/table02-large.png#lightbox)

Wir müssen eine **Array Controller** gebundene Daten mit unserer Tabelle bereitstellen möchten, gehen Sie folgendermaßen vor:

1. Ziehen Sie ein **Array Controller** aus der **Bibliothek Inspektor** auf die **Schnittstelle Editor**:  

    ![Auswählen eines Controllers Array aus der Bibliothek](databinding-images/table03.png "Auswählen eines Controllers Array aus der Bibliothek")
2. Wählen Sie **Array Controller** in der **Schnittstellenhierarchie** und wechseln Sie zu der **Attribut Inspektor**:  

    [![Auswählen des Attribute Inspektors](databinding-images/table04.png "auswählen des Attribute-Inspektors")](databinding-images/table04-large.png#lightbox)
3. Geben Sie `PersonModel` für die **Klassenname**, klicken Sie auf die **Plus** Schaltfläche, und fügen Sie drei Schlüssel hinzu. Nennen Sie diese `Name`, `Occupation` und `isManager`:  

    ![Hinzufügen der erforderlichen Schlüsselpfade](databinding-images/table05.png "erforderlichen Schlüsselpfade hinzufügen")
4. Das weist den Array-Controller was er ein Array von, verwaltet und welche Eigenschaften sie sollte verfügbar machen (über Schlüssel).
5. Wechseln Sie zu der **Bindungen Inspektor** und wählen Sie unter **Inhalt Array** wählen **Binden an** und **Tabelle-View-Controller**. Geben Sie einen **modellieren Schlüsselpfad** von `self.personModelArray`:  

    ![Eingeben einen Schlüsselpfad](databinding-images/table06.png "einen Schlüsselpfad eingeben")
6. Dies bindet den Array-Controller auf das Array von `PersonModels` , die wir für unsere View-Controller verfügbar gemacht werden.

Nachdem wir unsere Tabellenansicht auf den Array-Controller binden müssen, führen Sie folgende Schritte aus:

1. Wählen Sie die Tabellenansicht und die **binden Inspektor**:  

    [![Auswählen des Bindung Inspektors](databinding-images/table07.png "den Inspektor Bindung auswählen")](databinding-images/table07-large.png#lightbox)
2. Unter den **Tabelleninhalte** Turndown select **Binden an** und **Array Controller**. Geben Sie `arrangedObjects` für die **Controller Schlüssel** Feld:  

    ![Definition des Controller-Schlüssels](databinding-images/table08.png "Definition des Controller-Schlüssels")
3. Wählen Sie die **Ansicht Tabellenzelle** unter der **Mitarbeiter** Spalte. In der **Bindungen Inspektor** unter der **Wert** Turndown select **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Name` für die **modellieren Schlüsselpfad**:  

    [![Festlegen der Modell-Schlüsselpfad](databinding-images/table09.png "Modell Schlüsselpfad festlegen")](databinding-images/table09-large.png#lightbox)
4. `objectValue` der aktuelle `PersonModel` in das Array durch den Array-Controller verwaltet wird.
5. Wählen Sie die **Ansicht Tabellenzelle** unter der **Beruf** Spalte. In der **Bindungen Inspektor** unter der **Wert** Turndown select **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Occupation` für die **modellieren Schlüsselpfad**:  

    [![Festlegen der Modell-Schlüsselpfad](databinding-images/table10.png "Modell Schlüsselpfad festlegen")](databinding-images/table10-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Wenn wir die Anwendung ausführen, wird die Tabelle mit dem Array der aufgefüllt `PersonModels`:

[![Ausführen der Anwendung](databinding-images/table11.png "Ausführen der Anwendung")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Gliederung anzeigen-Datenbindung

Datenbindung anhand einer Gliederungsansicht ist vergleichbar mit der Bindung für eine Sicht, die Tabelle. Der Hauptunterschied besteht darin, dass wir verwenden eine **Struktur-Controller** anstelle von einer **Array Controller** um die gebundenen Daten für die Gliederungsansicht bereitzustellen. Weitere Informationen zum Arbeiten mit Gliederung Ansichten finden Sie unter unsere [Gliederung Ansichten](~/mac/user-interface/outline-view.md) Dokumentation.

Zunächst sehen wir fügen Sie einen neuen **View Controller** auf unserer **Main.storyboard** -Datei im Benutzeroberflächen-Generator, und benennen Sie die Klasse `OutlineViewController`: 

[![Hinzufügen eines neuen Sicht Controllers](databinding-images/outline01.png "Hinzufügen eines neuen Ansicht-Controllers")](databinding-images/outline01-large.png#lightbox)

Als Nächstes ermöglicht das Bearbeiten der **OutlineViewController.cs** Datei (, unsere Projekt automatisch hinzugefügt wurde), und machen Sie ein Array (`NSArray`) der `PersonModel` Klassen, dass wir unser Formular zum Binden von Daten ist. Fügen Sie den folgenden Code hinzu:

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

Wie haben wir auf die `PersonModel` -Klasse oben in der [definieren Ihr Datenmodell](#Defining_your_Data_Model) Abschnitt haben vier speziell benannte öffentliche Methoden verfügbar gemacht, damit die Struktur-Controller, Lese- und Schreibzugriff Daten aus der Auflistung von `PersonModels`.

Als Nächstes müssen wir beim Laden der Sicht Auffüllen unserer Array mit dem folgenden Code:

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

Nachdem wir unsere Gliederungsansicht erstellen müssen, doppelklicken Sie auf die **Main.storyboard** Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Layout der Tabelle, die in etwa wie folgt aussehen:

[![Erstellen die Gliederungsansicht](databinding-images/outline02.png "die Gliederungsansicht erstellen")](databinding-images/outline02-large.png#lightbox)

Wir müssen einen **Struktur-Controller** zum Bereitstellen von gebundener Daten in unserer Gliederung, gehen Sie folgendermaßen vor:

1. Ziehen Sie ein **Struktur-Controller** aus der **Bibliothek Inspektor** auf die **Schnittstelle Editor**:  

    ![Auswählen eines Struktur-Controllers aus der Bibliothek](databinding-images/outline03.png "einen Struktur-Controller aus der Bibliothek auswählen")
2. Wählen Sie **Struktur-Controller** in der **Schnittstellenhierarchie** und wechseln Sie zu der **Attribut Inspektor**:  

    [![Auswählen des Attribut-Inspektors](databinding-images/outline04.png "den Inspektor Attribut auswählen")](databinding-images/outline04-large.png#lightbox)
3. Geben Sie `PersonModel` für die **Klassenname**, klicken Sie auf die **Plus** Schaltfläche, und fügen Sie drei Schlüssel hinzu. Nennen Sie diese `Name`, `Occupation` und `isManager`:  

    ![Hinzufügen der erforderlichen Schlüsselpfade](databinding-images/outline05.png "erforderlichen Schlüsselpfade hinzufügen")
4. Das weist den Struktur-Controller was er ein Array von, verwaltet und welche Eigenschaften sie sollte verfügbar machen (über Schlüssel).
5. Unter den **Struktur-Controller** Geben Sie im Abschnitt `personModelArray` für **Kinder**, geben Sie `NumberOfEmployees` unter der **Anzahl** , und geben Sie `isEmployee` unter  **Endknoten**:  

    ![Festlegen der Struktur-Controller Schlüsselpfade](databinding-images/outline05.png "den Struktur-Controller Schlüsselpfade festlegen")
6. Das weist den Struktur-Controller Verfügbarkeit alle untergeordneten Knoten, wie viele untergeordnete Knoten vorhanden sind und wenn der aktuelle Knoten über untergeordnete Knoten verfügt.
7. Wechseln Sie zu der **Bindungen Inspektor** und wählen Sie unter **Inhalt Array** wählen **Binden an** und **des Dateibesitzer**. Geben Sie einen **modellieren Schlüsselpfad** von `self.personModelArray`:  

    ![Bearbeiten die Schlüsselpfad](databinding-images/outline06.png "Schlüsselpfad bearbeiten")
8. Dies bindet den Struktur-Controller auf das Array von `PersonModels` , die wir für unsere View-Controller verfügbar gemacht werden.

Nachdem wir unsere Gliederungsansicht an den Struktur-Controller binden möchten, führen Sie folgende Schritte aus:

1. Wählen Sie die Gliederungsansicht und klicken Sie in der **binden Inspektor** auswählen:  

    [![Auswählen des Bindung Inspektors](databinding-images/outline07.png "den Inspektor Bindung auswählen")](databinding-images/outline07-large.png#lightbox)
2. Klicken Sie unter der **Gliederung ' Inhalt anzeigen '** Turndown select **Binden an** und **Struktur-Controller**. Geben Sie `arrangedObjects` für die **Controller Schlüssel** Feld:  

    ![Festlegen des Controllers an](databinding-images/outline08.png "Festlegen des Controllers an")
3. Wählen Sie die **Ansicht Tabellenzelle** unter der **Mitarbeiter** Spalte. In der **Bindungen Inspektor** unter der **Wert** Turndown select **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Name` für die **modellieren Schlüsselpfad**:  

    [![Eingeben der Modell-Schlüsselpfad](databinding-images/outline09.png "Modell Schlüsselpfad eingeben")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` der aktuelle `PersonModel` im Array von der Struktur-Controller verwaltet werden.
5. Wählen Sie die **Ansicht Tabellenzelle** unter der **Beruf** Spalte. In der **Bindungen Inspektor** unter der **Wert** Turndown select **Binden an** und **Zelle Tabellenansicht**. Geben Sie `objectValue.Occupation` für die **modellieren Schlüsselpfad**:  

    [![Eingeben der Modell-Schlüsselpfad](databinding-images/outline10.png "Modell Schlüsselpfad eingeben")](databinding-images/outline10-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Wenn wir die Anwendung ausführen, wird die Gliederung mit unserer Array von aufgefüllt `PersonModels`:

[![Ausführen der Anwendung](databinding-images/outline11.png "Ausführen der Anwendung")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Auflistung Ansicht-Datenbindung

Datenbindung mit einer Auflistungsansicht ist sehr ähnlich wie mit einer Tabellenansicht binden, wie ein Array-Controller verwendet wird, um Daten für die Sammlung bereitstellen. Da die Auflistungsansicht einen voreingestellten Anzeigeformat besitzt, sind mehr Schritte erforderlich, zum Bereitstellen von Feedback der Benutzer-Interaktion und zum Nachverfolgen der Auswahl des Benutzers.

> [!IMPORTANT]
> Aufgrund eines Problems in Xcode 7 und MacOS 10.11 (und höher) sind die Auflistungsansichten kann nicht in eine Storyboard (.storyboard)-Dateien verwendet werden. Daher müssen Sie weiterhin .xib Dateien verwenden, um Ihre Auflistungsansichten für Ihre apps Xamarin.Mac zu definieren. Wenden Sie sich unsere [Auflistungsansichten](~/mac/user-interface/collection-view.md) Dokumentation weitere Informationen.

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

## <a name="debugging-native-crashes"></a>Debuggen von systemeigenen Abstürze

Einen Fehler in Ihrem datenbindungen treffen, kann dazu führen, ein _Native stürzt ab_ in nicht verwaltetem Code und dazu führen, dass Ihre Anwendung Xamarin.Mac zu vollständig mit Fehlern eine `SIGABRT` Fehler:

[![Beispiel eines Dialogfelds native Absturz](databinding-images/debug01.png "Beispiel für einen systemeigenen Absturz (Dialogfeld)")](databinding-images/debug01-large.png#lightbox)

Es gibt in der Regel vier Hauptursachen für systemeigene Abstürze während der Datenbindung:

1. Ihr Datenmodell erbt nicht von `NSObject` oder eine Unterklasse von `NSObject`.
2. Sie nicht Objective-C mithilfe Ihrer Eigenschaft verfügbar machen die `[Export("key-name")]` Attribut.
3. Sie nicht umbrochen, Änderungen an der Accessor-Wert in `WillChangeValue` und `DidChangeValue` Methodenaufrufe (Angeben von dem gleichen Schlüssel wie die `Export` Attribut).
4. Sie haben einen falschen oder falsch geschriebenes Schlüssel in der **binden Inspektor** in Benutzeroberflächen-Generator.

### <a name="decoding-a-crash"></a>Decodieren eines Absturzes

Wir verursacht einen systemeigenen Absturz in unserer Datenbindung, damit wir zeigen können, wie Suchen und zu beheben. Ändern Sie im Benutzeroberflächen-Generator, wir unsere Bindung der ersten Bezeichnung in der Auflistungsansicht-Beispiel aus `Name` auf `Title`:

[![Bearbeiten den Bindungsschlüssel des](databinding-images/debug02.png "Bindungsschlüssel des bearbeiten")](databinding-images/debug02-large.png#lightbox)

Sehen wir die Änderung speichern, wechseln Sie zu Visual Studio für Mac mit Xcode synchronisieren, und führen Sie die Anwendung zurück. Wenn die Auflistungsansicht angezeigt wird, die Anwendung stürzt vorübergehend mit einem `SIGABRT` Fehler (entsprechend der **Anwendungsausgabe** in Visual Studio für Mac) seit der `PersonModel` macht eine Eigenschaft mit dem Schlüssel `Title`:

[![Beispiel für einen Bindungsfehler](databinding-images/debug03.png "Beispiel für einen Bindungsfehler")](databinding-images/debug03-large.png#lightbox)

Wenn wir einen zu den Anfang des Fehlers in Bildlauf der **Anwendungsausgabe** sehen wir die Taste, um das Problem zu lösen:

[![Suchen das Problem im Fehlerprotokoll](databinding-images/debug04.png "suchen das Problem in das Fehlerprotokoll an")](databinding-images/debug04-large.png#lightbox)

Diese Zeile ist uns mitteilen, die den Schlüssel `Title` ist nicht vorhanden, auf das Objekt, das wir zu binden. Wenn wir ändern die Bindung zurück, in `Name` Schnittstelle-Generator speichern, Synchronisierung, neu erstellen und ausführen, wird die Anwendung erwartungsgemäß ohne Probleme ausgeführt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit dem Datenbindung und Schlüssel-Wert in einer Anwendung Xamarin.Mac Codierung übernommen. Zuerst sucht er am Verfügbarmachen von einer c#-Klasse für Objective-C mit Schlüssel-Wert-Codierung (KVM), und beobachten von Schlüssel-Wert (KVO). Als Nächstes es wurde gezeigt, wie eine kompatible KVO-Klasse verwenden, und Daten an Benutzeroberflächenelemente in Xcodes Benutzeroberflächen-Generator gebunden wird. Schließlich bleiben komplexe Daten mithilfe von Bindung **Array Controller** und **Struktur-Controller**.


## <a name="related-links"></a>Verwandte Links

- [MacDatabinding Storyboard (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (Beispiel)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Standardsteuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederung-Ansichten](~/mac/user-interface/outline-view.md)
- [Auflistungsansichten](~/mac/user-interface/collection-view.md)
- [Schlüssel-Wert-Codierung Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Einführung in Programmierhandbuch beobachten von Schlüssel-Wert](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Einführung in die Programmierung Themen Kakao-Bindungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in Kakao Bindungen Verweis](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [MacOS von menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
