---
title: Datenbindung und Schlüssel-Wert-Codierung in xamarin. Mac
description: In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierung und Schlüssel-Wert-Beobachtung erläutert, um die Datenbindung an Benutzeroberflächen Elemente in der Interface Builder von Xcode zuzulassen.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 110aaf8d324a13a6ea2e4c5a354adbc74fd8df96
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567579"
---
# <a name="data-binding-and-key-value-coding-in-xamarinmac"></a>Datenbindung und Schlüssel-Wert-Codierung in xamarin. Mac

_In diesem Artikel wird die Verwendung von Schlüssel-Wert-Codierung und Schlüssel-Wert-Beobachtung erläutert, um die Datenbindung an Benutzeroberflächen Elemente in der Interface Builder von Xcode zuzulassen._

## <a name="overview"></a>Übersicht

Wenn Sie mit c# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Daten an Benutzeroberflächen Elemente zu binden, anstatt Code zu schreiben.

Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.

[![Ein Beispiel für die laufende App](databinding-images/intro01.png "Ein Beispiel für die laufende App")](databinding-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Schlüssel-Wert-Codierung und Datenbindung in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , durcharbeiten, da er wichtige Konzepte und Techniken behandelt, die wir in diesem Artikel verwenden werden.

Lesen Sie ggf. den Abschnitt verfügbar machen von [c#-Klassen/-Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) ". darin werden die `Register` Attribute und erläutert, die zum Verknüpfen `Export` ihrer c#-Klassen mit Ziel-c-Objekten und Benutzeroberflächen Elementen verwendet werden.

<a name="What_is_Key-Value_Coding"></a>

## <a name="what-is-key-value-coding"></a>Was ist Schlüssel-Wert-Codierung?

Key-Value Coding (KVC) ist ein Mechanismus für den indirekten Zugriff auf die Eigenschaften eines Objekts, indem Schlüssel (speziell formatierte Zeichen folgen) verwendet werden, um Eigenschaften zu identifizieren, anstatt über Instanzvariablen oder Zugriffsmethoden () auf Sie zuzugreifen `get/set` . Durch Implementieren von Schlüssel-Wert-Codierungs kompatiblen Accessoren in ihrer xamarin. Mac-Anwendung erhalten Sie Zugriff auf andere macOS-Features (ehemals OS X), wie z. b. die Schlüssel-Wert-Beobachtung (KVO), die Datenbindung, die Kern Daten, Cocoa-Bindungen und die scriptbarkeit.

Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.

Betrachten wir beispielsweise die folgende Klassendefinition eines KVC-kompatiblen Objekts:

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

Zuerst registriert das `[Register("PersonModel")]` Attribut die Klasse und macht Sie für "Ziel-C" verfügbar. Anschließend muss die-Klasse von `NSObject` (oder einer Unterklasse, die von erbt `NSObject` ) erben, sodass mehrere Basis Methoden hinzugefügt werden, die der-Klasse die KVC-Konformität ermöglichen. Als nächstes macht das `[Export("Name")]` -Attribut die `Name` -Eigenschaft verfügbar und definiert den Schlüsselwert, der später verwendet wird, um über KVC-und KVO-Techniken auf die-Eigenschaft zuzugreifen.

Schließlich muss der-Accessor Änderungen an seinem Wert in-und-Methoden aufrufen umschließen, um den Schlüsselwert zu ändern, der für den Wert der Eigenschaft `WillChangeValue` `DidChangeValue` erforderlich ist `Export` .  Beispiel:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Dieser Schritt ist für die Datenbindung in der Interface Builder von Xcode _sehr_ wichtig (wie weiter unten in diesem Artikel zu sehen ist).

Weitere Informationen finden Sie im [Programmier Handbuch für Schlüssel-Wert-Codierungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)von Apple.

### <a name="keys-and-key-paths"></a>Schlüssel und Schlüssel Pfade

Ein _Schlüssel_ ist eine Zeichenfolge, die eine bestimmte Eigenschaft eines Objekts angibt. In der Regel entspricht ein Schlüssel dem Namen einer Accessormethode in einem Schlüssel-Wert-Codierungs kompatiblen Objekt. Schlüssel müssen ASCII-Codierung verwenden, in der Regel mit einem Kleinbuchstaben beginnen und dürfen keine Leerzeichen enthalten. Im obigen Beispiel `Name` wäre also ein Schlüsselwert der-Eigenschaft der- `Name` `PersonModel` Klasse. Der Schlüssel und der Name der Eigenschaft, die Sie verfügbar machen, müssen nicht identisch sein. in den meisten Fällen handelt es sich jedoch um.

Ein _Schlüssel Pfad_ ist eine Zeichenfolge mit durch Punkte getrennten Schlüsseln, die verwendet werden, um eine Hierarchie von zu durchsuchenden Objekteigenschaften anzugeben. Die-Eigenschaft des ersten Schlüssels in der Sequenz ist relativ zum Empfänger, und jeder nachfolgende Schlüssel wird relativ zum Wert der vorherigen Eigenschaft ausgewertet. Auf dieselbe Weise verwenden Sie die Punkt Notation, um ein Objekt und seine Eigenschaften in einer c#-Klasse zu durchlaufen.

Wenn Sie z. b. die `PersonModel` -Klasse erweitert und die-Eigenschaft hinzugefügt haben `Child` :

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

Der Schlüssel Pfad zum Namen des untergeordneten Elements ist `self.Child.Name` oder einfach `Child.Name` (basierend auf der Verwendung des Schlüssel Werts).

### <a name="getting-values-using-key-value-coding"></a>Erhalten von Werten mithilfe von Schlüssel-Wert-Codierung

Die- `ValueForKey` Methode gibt den Wert für den angegebenen Schlüssel (als `NSString` ) relativ zur Instanz der KVC-Klasse zurück, die die Anforderung empfängt. Wenn z. b. `Person` eine Instanz der `PersonModel` oben definierten-Klasse ist:

```csharp
// Read value
var name = Person.ValueForKey (new NSString("Name"));
```

Dies würde den Wert der- `Name` Eigenschaft für diese Instanz von zurückgeben `PersonModel` .

### <a name="setting-values-using-key-value-coding"></a>Festlegen von Werten mithilfe von Schlüssel-Wert-Codierung

Entsprechend wird der `SetValueForKey` Wert für den angegebenen Schlüssel (als `NSString` ) in Relation zur Instanz der KVC-Klasse, die die Anforderung empfängt, festgelegt. Verwenden Sie eine Instanz der- `PersonModel` Klasse, wie unten gezeigt:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Würde den Wert der- `Name` Eigenschaft in ändern `Jane Doe` .

<a name="Observing_Value_Changes"></a>

### <a name="observing-value-changes"></a>Beobachten von Wertänderungen

Mithilfe von Key-Value-Beobachtungen (KVO) können Sie einen Beobachter an einen bestimmten Schlüssel einer KVC-kompatiblen Klasse anfügen und jederzeit benachrichtigt werden, wenn der Wert für diesen Schlüssel geändert wird (entweder mithilfe von KVC-Techniken oder direkt Zugriff auf die angegebene Eigenschaft in c#-Code). Beispiel:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Wenn nun die- `Name` Eigenschaft der `Person` Instanz der- `PersonModel` Klasse geändert wird, wird der neue Wert in die Konsole geschrieben.

Weitere Informationen finden Sie unter Apple [Introduction to Key-Value beobachtender Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Datenbindung

In den folgenden Abschnitten wird gezeigt, wie Sie eine Schlüssel-Wert-Codierung und eine Schlüsselwert beobachende kompatible Klasse verwenden können, um Daten an Benutzeroberflächen Elemente in der Interface Builder von Xcode zu binden, anstatt Werte mit c#-Code zu lesen und zu schreiben. Auf diese Weise trennen Sie das _Datenmodell_ von den Sichten, die zum Anzeigen verwendet werden, sodass die xamarin. Mac-Anwendung flexibler und einfacher zu verwalten ist. Außerdem verringern Sie die Menge des zu schreibenden Codes erheblich.

<a name="Defining_your_Data_Model"></a>

### <a name="defining-your-data-model"></a>Definieren des Datenmodells

Bevor Sie ein Benutzeroberflächen Element in Interface Builder binden können, müssen Sie in ihrer xamarin. Mac-Anwendung eine KVC/KVO-kompatible Klasse definieren, die als _Datenmodell_ für die Bindung fungiert. Das Datenmodell stellt alle Daten bereit, die auf der Benutzeroberfläche angezeigt werden, und empfängt Änderungen an den Daten, die der Benutzer während der Ausführung der Anwendung in der Benutzeroberfläche vornimmt.

Wenn Sie z. b. eine Anwendung schreiben, die eine Gruppe von Mitarbeitern verwaltet, können Sie die folgende Klasse verwenden, um das Datenmodell zu definieren:

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

Die meisten Funktionen dieser Klasse wurden im obigen Abschnitt [Was sind Schlüssel-Wert-Codierung](#What_is_Key-Value_Coding) beschrieben. Betrachten wir jedoch einige spezifische Elemente und einige Ergänzungen, die dazu geführt haben, dass diese Klasse als Datenmodell für **Array Controller** und Struktur **Controller** fungieren soll (das wir später für die Datenbindung von Struktur **Ansichten**, Gliederungs **Ansichten** und Auflistungs **Ansichten verwenden**werden).

Da ein Mitarbeiter möglicherweise ein Manager ist, haben wir einen verwendet `NSArray` (insbesondere eine, `NSMutableArray` damit die Werte geändert werden können), damit die Mitarbeiter, denen Sie zugeordnet sind, an Sie angefügt werden können:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Hier sind zwei Punkte zu beachten:

1. Wir haben `NSMutableArray` anstelle eines standardmäßigen c#-Arrays oder einer Standard Auflistung verwendet, da dies eine Voraussetzung für die Datenbindung an AppKit-Steuerelemente wie **Collections** **Tabellen Sichten**, Gliederungs **Sichten** und Auflistungen ist.
2. Wir haben das Array von Mitarbeitern zur Verfügung gestellt, indem wir es zu `NSArray` Daten Bindungs Zwecken in eine umwandeln und den c#-formatierten Namen, `People` , zu einem von der Datenbindung erwarteten Wert `personModelArray` in der Form **{class_name} Array** geändert haben (Beachten Sie, dass das erste Zeichen in Kleinbuchstaben geschrieben wurde).

Als nächstes müssen wir einige besondere öffentliche Methoden hinzufügen, um **Array Controller** und Struktur **Controller**zu unterstützen:

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

Diese ermöglichen es den Controllern, die angezeigten Daten anzufordern und zu ändern. Wie das `NSArray` oben gezeigte haben diese eine sehr spezielle Benennungs Konvention (die sich von den typischen c#-Benennungs Konventionen unterscheidet):

- `addObject:`-Fügt dem Array ein Objekt hinzu.
- `insertObject:in{class_name}ArrayAtIndex:`: Wobei `{class_name}` der Name der Klasse ist. Diese Methode fügt ein Objekt an einem angegebenen Index in das Array ein.
- `removeObjectFrom{class_name}ArrayAtIndex:`: Wobei `{class_name}` der Name der Klasse ist. Diese Methode entfernt das-Objekt im-Array an einem angegebenen Index.
- `set{class_name}Array:`: Wobei `{class_name}` der Name der Klasse ist. Diese Methode ermöglicht es Ihnen, die vorhandene Durchführung durch eine neue zu ersetzen.

Innerhalb dieser Methoden haben wir Änderungen am Array in `WillChangeValue` und `DidChangeValue` Nachrichten für die KVO-Konformität umschließen.

Da die- `Icon` Eigenschaft auf den Wert der-Eigenschaft basiert `isManager` , werden Änderungen an der-Eigenschaft `isManager` möglicherweise nicht in `Icon` für Daten gebundene Benutzeroberflächen Elemente (während der KVO) berücksichtigt:

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

Um dies zu korrigieren, verwenden wir den folgenden Code:

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

Beachten Sie, dass die-Zugriffsmethode zusätzlich zu Ihrem eigenen Schlüssel `isManager` auch die `WillChangeValue` -und- `DidChangeValue` Meldungen für den Schlüssel sendet, `Icon` sodass auch die Änderung angezeigt wird.

Wir verwenden das `PersonModel` Datenmodell im restlichen Verlauf dieses Artikels.

<a name="Simple_Data_Binding"></a>

### <a name="simple-data-binding"></a>Einfache Datenbindung

Wenn wir unser Datenmodell definiert haben, sehen wir uns ein einfaches Beispiel für die Datenbindung in Xcode Interface Builder an. Fügen Sie beispielsweise der xamarin. Mac-Anwendung ein Formular hinzu, das zum Bearbeiten der verwendet werden kann `PersonModel` , die wir zuvor definiert haben. Wir fügen einige Text Felder und ein Kontrollkästchen hinzu, um die Eigenschaften des Modells anzuzeigen und zu bearbeiten.

Fügen Sie zunächst der Datei " **Main. Storyboard** " in Interface Builder einen neuen **Ansichts Controller** hinzu, und benennen Sie die Klasse `SimpleViewController` :

[![Hinzufügen eines neuen Ansichts Controllers](databinding-images/simple01.png "Hinzufügen eines neuen Ansichts Controllers")](databinding-images/simple01-large.png#lightbox)

Kehren Sie als nächstes zu Visual Studio für Mac zurück, bearbeiten Sie die Datei **SimpleViewController.cs** (die dem Projekt automatisch hinzugefügt wurde), und machen Sie eine Instanz von verfügbar, `PersonModel` an die wir Daten binden. Fügen Sie den folgenden Code hinzu:

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

Nachdem die Ansicht geladen wurde, erstellen wir eine Instanz von `PersonModel` und füllen Sie mit dem folgenden Code:

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

Nun müssen wir das Formular erstellen und auf die Datei " **Main. Storyboard** " doppelklicken, um es für die Bearbeitung in Interface Builder zu öffnen. Layout Sie das Formular so, dass es in etwa wie folgt aussieht:

[![Bearbeiten des Storyboards in Xcode](databinding-images/simple02.png "Bearbeiten des Storyboards in Xcode")](databinding-images/simple02-large.png#lightbox)

Gehen Sie folgendermaßen vor, um das Formular an den zu binden, den `PersonModel` wir über den Schlüssel verfügbar gemacht `Person` haben:

1. Wählen Sie das Textfeld **Mitarbeiter Name** aus, und wechseln Sie zum **Bindungs Inspektor**.
2. Aktivieren Sie das Kontrollkästchen **binden an,** und wählen Sie in der Dropdown Liste **Simple View Controller** aus. Geben Sie als nächstes `self.Person.Name` den **Schlüssel Pfad**ein:

    [![Eingeben des Schlüssel Pfads](databinding-images/simple03.png "Eingeben des Schlüssel Pfads")](databinding-images/simple03-large.png#lightbox)
3. Wählen Sie das Feld **Berufs** Text aus, und aktivieren Sie das Feld **binden an,** und wählen Sie in der Dropdown Liste **Simple View Controller** aus. Geben Sie als nächstes `self.Person.Occupation` den **Schlüssel Pfad**ein:

    [![Eingeben des Schlüssel Pfads](databinding-images/simple04.png "Eingeben des Schlüssel Pfads")](databinding-images/simple04-large.png#lightbox)
4. Aktivieren Sie das Kontrollkästchen **Mitarbeiter ist ein Manager** , aktivieren Sie das Kontrollkästchen **binden an,** und wählen Sie in der Dropdown Liste **Simple View Controller** aus. Geben Sie als nächstes `self.Person.isManager` den **Schlüssel Pfad**ein:

    [![Eingeben des Schlüssel Pfads](databinding-images/simple05.png "Eingeben des Schlüssel Pfads")](databinding-images/simple05-large.png#lightbox)
5. Wählen Sie die **Anzahl der Mitarbeiter verwalteten** Textfeld aus, aktivieren Sie das Feld **binden an,** und wählen Sie in der Dropdown Liste **Simple View Controller** aus. Geben Sie als nächstes `self.Person.NumberOfEmployees` den **Schlüssel Pfad**ein:

    [![Eingeben des Schlüssel Pfads](databinding-images/simple06.png "Eingeben des Schlüssel Pfads")](databinding-images/simple06-large.png#lightbox)
6. Wenn der Mitarbeiter kein Manager ist, möchten wir die Anzahl von Mitarbeitern, die verwaltete Bezeichnung und das Textfeld haben, ausblenden.
7. Wählen Sie die Bezeichnung **Anzahl der verwalteten Mitarbeiter** aus, erweitern Sie den ausgeblendeten Pfeil, und aktivieren **Sie das Kontroll** Kästchen **binden an** , **und wählen Sie** in der Dropdown Liste die Option Geben Sie als nächstes `self.Person.isManager` den **Schlüssel Pfad**ein:

    [![Eingeben des Schlüssel Pfads](databinding-images/simple07.png "Eingeben des Schlüssel Pfads")](databinding-images/simple07-large.png#lightbox)
8. Wählen Sie `NSNegateBoolean` aus der Dropdown Liste **Wert Transformator** Folgendes aus:

    ![Auswählen der nsnegateboolean-Schlüssel Transformation](databinding-images/simple08.png "Auswählen der nsnegateboolean-Schlüssel Transformation")
9. Dadurch wird der Datenbindung mitgeteilt, dass die Bezeichnung ausgeblendet wird, wenn der Wert der- `isManager` Eigenschaft ist `false` .
10. Wiederholen Sie die Schritte 7 und 8 für das Feld **Anzahl der Mitarbeiter verwalteten** Text.
11. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Wenn Sie die Anwendung ausführen, füllen die Werte aus der- `Person` Eigenschaft automatisch das folgende Formular auf:

[![Ein automatisch aufgefülltes Formular wird angezeigt.](databinding-images/simple09.png "Ein automatisch aufgefülltes Formular wird angezeigt.")](databinding-images/simple09-large.png#lightbox)

Alle Änderungen, die die Benutzer an dem Formular vornehmen, werden in der- `Person` Eigenschaft des Ansichts Controllers zurückgeschrieben. Wenn Sie z. b. die Auswahl von **Employee** aufheben, aktualisiert die `Person` Instanz von, `PersonModel` und die **Anzahl von Mitarbeitern verwaltete** Bezeichnung und Textfeld werden automatisch ausgeblendet (über die Datenbindung):

[![Ausblenden der Anzahl von Mitarbeitern für nicht-Manager](databinding-images/simple10.png "Ausblenden der Anzahl von Mitarbeitern für nicht-Manager")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding"></a>

### <a name="table-view-data-binding"></a>Tabellen Ansichts Datenbindung

Nachdem wir nun über die Grundlagen der Datenbindung verfügen, sehen wir uns eine komplexere Daten Bindungs Aufgabe an, indem wir einen _Array Controller_ und eine Datenbindung an eine Tabellenansicht verwenden. Weitere Informationen zum Arbeiten mit Tabellen Sichten finden Sie in der Dokumentation der [Tabellen Sichten](~/mac/user-interface/table-view.md) .

Fügen Sie zunächst der Datei " **Main. Storyboard** " in Interface Builder einen neuen **Ansichts Controller** hinzu, und benennen Sie die Klasse `TableViewController` :

[![Hinzufügen eines neuen Ansichts Controllers](databinding-images/table01.png "Hinzufügen eines neuen Ansichts Controllers")](databinding-images/table01-large.png#lightbox)

Als nächstes bearbeiten wir die Datei " **TableViewController.cs** " (die dem Projekt automatisch hinzugefügt wurde) und machen ein Array ( `NSArray` ) von Klassen verfügbar, `PersonModel` an die wir Daten binden. Fügen Sie den folgenden Code hinzu:

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

Genau wie bei der oben aufgeführten `PersonModel` Klasse im Abschnitt [Definieren des Datenmodells](#Defining_your_Data_Model) haben wir vier speziell benannte öffentliche Methoden zur Verfügung gestellt, sodass der Array Controller und Daten aus unserer Auflistung von lesen und schreiben `PersonModels` .

Nachdem die Ansicht geladen wurde, müssen wir das Array mit dem folgenden Code Auffüllen:

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

Nun müssen wir die Tabellenansicht erstellen, auf die Datei " **Main. Storyboard** " doppelklicken, um Sie für die Bearbeitung in Interface Builder zu öffnen. Das Layout der Tabelle sieht in etwa wie folgt aus:

[![Festlegen einer neuen Tabellenansicht](databinding-images/table02.png "Festlegen einer neuen Tabellenansicht")](databinding-images/table02-large.png#lightbox)

Wir müssen einen **Array Controller** hinzufügen, um gebundene Daten für die Tabelle bereitzustellen. gehen Sie dazu wie folgt vor:

1. Ziehen Sie einen **Array Controller** aus der **Bibliothek Inspector** auf den **Schnittstellen-Editor**:

    ![Auswählen eines Array Controllers aus der Bibliothek](databinding-images/table03.png "Auswählen eines Array Controllers aus der Bibliothek")
2. Wählen Sie in der **Schnittstellen Hierarchie** **Array Controller** aus, und wechseln Sie zum **Attribut Inspektor**:

    [![Auswählen des Attribut Inspektors](databinding-images/table04.png "Auswählen des Attribut Inspektors")](databinding-images/table04-large.png#lightbox)
3. Geben Sie `PersonModel` als **Klassennamen**ein, klicken Sie auf die Schaltfläche **plus** , und fügen Sie drei Schlüssel hinzu. Benennen Sie Sie wie folgt `Name` `Occupation` `isManager` :

    ![Hinzufügen der erforderlichen Schlüssel Pfade](databinding-images/table05.png "Hinzufügen der erforderlichen Schlüssel Pfade")
4. Dadurch wird dem Array Controller mitgeteilt, worum es sich bei der Verwaltung eines Arrays von handelt und welche Eigenschaften es (über Schlüssel) verfügbar machen soll.
5. Wechseln Sie zum **Bindungs Inspektor** , und wählen Sie unter **Inhalts Array** die Option **binden an** und **Tabellen Ansichts Controller**aus. Geben Sie einen **Modell Schlüssel Pfad** für Folgendes ein `self.personModelArray` :

    ![Eingeben eines Schlüssel Pfads](databinding-images/table06.png "Eingeben eines Schlüssel Pfads")
6. Dadurch wird der Array Controller mit dem Array von verknüpft `PersonModels` , das wir auf dem Ansichts Controller verfügbar gemacht haben.

Nun muss die Tabellenansicht an den Array Controller gebunden werden. gehen Sie wie folgt vor:

1. Wählen Sie die Tabellenansicht und den **Bindungs Inspektor**aus:

    [![Auswählen des Bindungs Inspektors](databinding-images/table07.png "Auswählen des Bindungs Inspektors")](databinding-images/table07-large.png#lightbox)
2. Wählen Sie unter der **Liste Tabelleninhalt** die Option **binden an** und **Array Controller**aus. Geben Sie `arrangedObjects` für das Feld **Controller Schlüssel** Folgendes ein:

    ![Definieren des Controller Schlüssels](databinding-images/table08.png "Definieren des Controller Schlüssels")
3. Wählen Sie die **Tabellen Ansichts Zelle** unter der Spalte **Employee** aus. Wählen Sie im **Bindungs Inspektor** unter dem **Wert** -Turndown die Option **binden an** und **Tabellenzellen Ansicht**aus. Geben Sie als `objectValue.Name` **Modell Schlüssel Pfad**ein:

    [![Der Modell Schlüssel Pfad wird festgelegt.](databinding-images/table09.png "Der Modell Schlüssel Pfad wird festgelegt.")](databinding-images/table09-large.png#lightbox)
4. `objectValue`der aktuelle `PersonModel` in dem Array, das vom Array Controller verwaltet wird.
5. Wählen Sie die **Tabellen Ansichts Zelle** unter der Spalte **Beruf** aus. Wählen Sie im **Bindungs Inspektor** unter dem **Wert** -Turndown die Option **binden an** und **Tabellenzellen Ansicht**aus. Geben Sie als `objectValue.Occupation` **Modell Schlüssel Pfad**ein:

    [![Der Modell Schlüssel Pfad wird festgelegt.](databinding-images/table10.png "Der Modell Schlüssel Pfad wird festgelegt.")](databinding-images/table10-large.png#lightbox)
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Wenn die Anwendung ausgeführt wird, wird die Tabelle mit dem folgenden Array aufgefüllt `PersonModels` :

[![Ausführen der Anwendung](databinding-images/table11.png "Ausführen der Anwendung")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding"></a>

### <a name="outline-view-data-binding"></a>Datenbindung Gliederungsansicht

die Datenbindung für eine Gliederungs Ansicht ähnelt der Bindung an eine Tabellen Sicht. Der Hauptunterschied besteht darin, dass wir anstelle eines **Array Controllers** einen Struktur **Controller** verwenden, um die gebundenen Daten für die Gliederungs Ansicht bereitzustellen. Weitere Informationen zum Arbeiten mit Gliederungs Ansichten finden Sie in der Dokumentation zu den Gliederungs [Ansichten](~/mac/user-interface/outline-view.md) .

Fügen Sie zunächst der Datei " **Main. Storyboard** " in Interface Builder einen neuen **Ansichts Controller** hinzu, und benennen Sie die Klasse `OutlineViewController` :

[![Hinzufügen eines neuen Ansichts Controllers](databinding-images/outline01.png "Hinzufügen eines neuen Ansichts Controllers")](databinding-images/outline01-large.png#lightbox)

Als nächstes bearbeiten wir die Datei " **OutlineViewController.cs** " (die dem Projekt automatisch hinzugefügt wurde) und machen ein Array ( `NSArray` ) von Klassen verfügbar, `PersonModel` an die wir Daten binden. Fügen Sie den folgenden Code hinzu:

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

Genau wie bei der oben aufgeführten `PersonModel` Klasse im Abschnitt [Definieren des Datenmodells](#Defining_your_Data_Model) haben wir vier speziell benannte öffentliche Methoden zur Verfügung gestellt, sodass der Struktur Controller und Daten aus unserer Auflistung von gelesen und geschrieben werden `PersonModels` .

Nachdem die Ansicht geladen wurde, müssen wir das Array mit dem folgenden Code Auffüllen:

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

Nun müssen wir unsere Gliederungs Ansicht erstellen, auf die Datei " **Main. Storyboard** " doppelklicken, um Sie für die Bearbeitung in Interface Builder zu öffnen. Das Layout der Tabelle sieht in etwa wie folgt aus:

[![Erstellen der Gliederungs Ansicht](databinding-images/outline02.png "Erstellen der Gliederungs Ansicht")](databinding-images/outline02-large.png#lightbox)

Wir müssen einen Struktur **Controller** hinzufügen, um dem Umriss gebundene Daten bereitzustellen. gehen Sie dazu wie folgt vor:

1. Ziehen Sie einen Struktur **Controller** aus der **Bibliothek Inspector** auf den **Schnittstellen-Editor**:

    ![Auswählen eines Struktur Controllers aus der Bibliothek](databinding-images/outline03.png "Auswählen eines Struktur Controllers aus der Bibliothek")
2. Wählen Sie in der **Schnittstellen Hierarchie** Struktur **Controller** aus, und wechseln Sie zum **Attribut Inspektor**:

    [![Auswählen des Attribut Inspektors](databinding-images/outline04.png "Auswählen des Attribut Inspektors")](databinding-images/outline04-large.png#lightbox)
3. Geben Sie `PersonModel` als **Klassennamen**ein, klicken Sie auf die Schaltfläche **plus** , und fügen Sie drei Schlüssel hinzu. Benennen Sie Sie wie folgt `Name` `Occupation` `isManager` :

    ![Hinzufügen der erforderlichen Schlüssel Pfade](databinding-images/outline05.png "Hinzufügen der erforderlichen Schlüssel Pfade")
4. Dadurch wird dem Struktur Controller mitgeteilt, worum es sich bei der Verwaltung eines Arrays von handelt und welche Eigenschaften (über Schlüssel) verfügbar gemacht werden sollen.
5. Geben Sie im Abschnitt Struktur **Controller** für untergeordnete Elemente ein `personModelArray` , geben **Children**Sie `NumberOfEmployees` unter der **Anzahl** ein, und geben Sie `isEmployee` unter **Blatt**ein:

    ![Festlegen der Struktur Controller-Schlüssel Pfade](databinding-images/outline05.png "Festlegen der Struktur Controller-Schlüssel Pfade")
6. Dadurch wird dem Struktur Controller mitgeteilt, wo alle untergeordneten Knoten zu finden sind, wie viele untergeordnete Knoten vorhanden sind und ob der aktuelle Knoten über untergeordnete Knoten verfügt.
7. Wechseln Sie zum **Bindungs Inspektor** , und wählen Sie unter **Inhalts Array** die Option **binden an** und **den Besitzer der Datei**aus. Geben Sie einen **Modell Schlüssel Pfad** für Folgendes ein `self.personModelArray` :

    ![Bearbeiten des Schlüssel Pfads](databinding-images/outline06.png "Bearbeiten des Schlüssel Pfads")
8. Dadurch wird der Struktur Controller mit dem Array von verknüpft `PersonModels` , das wir auf dem Ansichts Controller verfügbar gemacht haben.

Nun müssen wir unsere Gliederungs Ansicht an den Struktur Controller binden, um Folgendes durchzuführen:

1. Wählen Sie die **Gliederungs** Ansicht und in der Bindungs Prüfung Folgendes aus:

    [![Auswählen des Bindungs Inspektors](databinding-images/outline07.png "Auswählen des Bindungs Inspektors")](databinding-images/outline07-large.png#lightbox)
2. Wählen Sie unter der **Ansicht Inhalt der Umriss Ansicht** die Option **binden an** und Struktur **Controller**aus. Geben Sie `arrangedObjects` für das Feld **Controller Schlüssel** Folgendes ein:

    ![Festlegen des Controller Schlüssels](databinding-images/outline08.png "Festlegen des Controller Schlüssels")
3. Wählen Sie die **Tabellen Ansichts Zelle** unter der Spalte **Employee** aus. Wählen Sie im **Bindungs Inspektor** unter dem **Wert** -Turndown die Option **binden an** und **Tabellenzellen Ansicht**aus. Geben Sie als `objectValue.Name` **Modell Schlüssel Pfad**ein:

    [![Eingeben des Modell Schlüssel Pfads](databinding-images/outline09.png "Eingeben des Modell Schlüssel Pfads")](databinding-images/outline09-large.png#lightbox)
4. `objectValue`der aktuelle `PersonModel` in dem Array, das vom Struktur Controller verwaltet wird.
5. Wählen Sie die **Tabellen Ansichts Zelle** unter der Spalte **Beruf** aus. Wählen Sie im **Bindungs Inspektor** unter dem **Wert** -Turndown die Option **binden an** und **Tabellenzellen Ansicht**aus. Geben Sie als `objectValue.Occupation` **Modell Schlüssel Pfad**ein:

    [![Eingeben des Modell Schlüssel Pfads](databinding-images/outline10.png "Eingeben des Modell Schlüssel Pfads")](databinding-images/outline10-large.png#lightbox)
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Wenn die Anwendung ausgeführt wird, wird der Umriss mit dem folgenden Array aufgefüllt `PersonModels` :

[![Ausführen der Anwendung](databinding-images/outline11.png "Ausführen der Anwendung")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Datenbindung für Sammlungsansicht

Die Datenbindung mit einer Auflistungs Ansicht ähnelt der Bindung mit einer Tabellen Sicht, da ein Array Controller zum Bereitstellen von Daten für die Auflistung verwendet wird. Da die Sammlungsansicht nicht über ein voreingestelltes Anzeige Format verfügt, ist mehr Arbeit erforderlich, um Feedback zur Benutzerinteraktion bereitzustellen und die Benutzer Auswahl zu verfolgen.

> [!IMPORTANT]
> Aufgrund eines Problems in Xcode 7 und macOS 10,11 (und höher) können Sammlungs Sichten nicht innerhalb von Storyboard-Dateien (. Storyboard) verwendet werden. Daher müssen Sie weiterhin XIb-Dateien verwenden, um Ihre Sammlungs Ansichten für Ihre xamarin. Mac-apps zu definieren. Weitere Informationen finden Sie in der Dokumentation zu den [Sammlungs Ansichten](~/mac/user-interface/collection-view.md) .

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

1. **Collection View Item** -  That manages a single instance of an item in the collection.
2. **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

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

## <a name="debugging-native-crashes"></a>Debugging nativer Abstürze

Wenn Sie einen Fehler in den Daten Bindungen erzeugen, kann dies zu einem _nativen Absturz_ in nicht verwaltetem Code führen, und ihre xamarin. Mac-Anwendung kann mit einem Fehler vollständig ausfallen `SIGABRT` :

[![Beispiel für einen nativen Absturz (Dialogfeld)](databinding-images/debug01.png "Beispiel für einen nativen Absturz (Dialogfeld)")](databinding-images/debug01-large.png#lightbox)

Es gibt in der Regel vier Hauptgründe für Native Abstürze während der Datenbindung:

1. Das Datenmodell erbt nicht von `NSObject` oder einer Unterklasse von `NSObject` .
2. Sie haben die Eigenschaft nicht mit dem-Attribut für "Ziel-C" verfügbar gemacht `[Export("key-name")]` .
3. Sie haben Änderungen am Wert des Accessors in `WillChangeValue` -und-Methoden aufrufen nicht umschlossen (und dabei `DidChangeValue` denselben Schlüssel wie das- `Export` Attribut angeben).
4. Der **Bindungs Inspektor** in Interface Builder weist einen falschen oder falsch formatierten Schlüssel auf.

### <a name="decoding-a-crash"></a>Decodieren eines Absturzes

Wir verursachen einen systemeigenen Absturz in unserer Datenbindung, damit wir zeigen können, wie Sie ihn finden und beheben können. Ändern Sie in Interface Builder die Bindung der ersten Bezeichnung im Beispiel der Sammlungsansicht von in `Name` `Title` :

[![Bearbeiten des Bindungs Schlüssels](databinding-images/debug02.png "Bearbeiten des Bindungs Schlüssels")](databinding-images/debug02-large.png#lightbox)

Speichern Sie die Änderung, wechseln Sie zurück zu Visual Studio für Mac, um die Synchronisierung mit Xcode durchzuführen, und führen Sie die Anwendung aus. Wenn die Auflistungs Ansicht angezeigt wird, stürzt die Anwendung vorübergehend mit einem `SIGABRT` Fehler ab (wie in der **Anwendungs Ausgabe** in Visual Studio für Mac gezeigt), da der `PersonModel` keine Eigenschaft mit dem Schlüssel verfügbar macht `Title` :

[![Beispiel für einen Bindungs Fehler](databinding-images/debug03.png "Beispiel für einen Bindungs Fehler")](databinding-images/debug03-large.png#lightbox)

Wenn wir einen Bildlauf zum Anfang des Fehlers in der **Anwendungs Ausgabe** ausführen, sehen wir den Schlüssel zur Behebung des Problems:

[![Suchen des Problems im Fehlerprotokoll](databinding-images/debug04.png "Suchen des Problems im Fehlerprotokoll")](databinding-images/debug04-large.png#lightbox)

Diese Zeile teilt uns mit, dass der Schlüssel `Title` für das Objekt, an das wir binden, nicht vorhanden ist. Wenn wir die Bindung `Name` in Interface Builder ändern, speichern, synchronisieren, neu erstellen und ausführen, wird die Anwendung erwartungsgemäß ohne Probleme ausgeführt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Datenbindung und Schlüssel-Wert-Codierung in einer xamarin. Mac-Anwendung ausführlich erläutert. Zuerst wurde das verfügbar machen einer c#-Klasse für "Ziel-C" mithilfe von Key-Value Coding (KVC) und Key-Value-Beobachtungen (KVO) untersucht. Im nächsten Schritt wurde gezeigt, wie eine KVO-kompatible Klasse verwendet und Daten an Benutzeroberflächen Elemente in der Interface Builder von Xcode gebunden werden. Zum Schluss zeigte er eine komplexe Datenbindung mithilfe von **Array Controllern** und Struktur **Controllern**.

## <a name="related-links"></a>Verwandte Links

- [Macdatabinding-Storyboard (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-storyboard)
- [Macdatabinding-xisb (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabinding-xibs)
- [Hello, Mac (Hallo Mac)](~/mac/get-started/hello-mac.md)
- [Standard Steuerelemente](~/mac/user-interface/standard-controls.md)
- [Tabellen Sichten](~/mac/user-interface/table-view.md)
- [Gliederungs Ansichten](~/mac/user-interface/outline-view.md)
- [Auflistungsansichten](~/mac/user-interface/collection-view.md)
- [Programmier Handbuch für Schlüssel-Wert-Codierung](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Einführung in den Programmier Leit Faden für Schlüssel-Wert-Beobachtungen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Programmierthemen "Einführung in Cocoa-Bindungen"](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Einführung in Cocoa-Bindungs Referenz](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [Nscollectionview](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
