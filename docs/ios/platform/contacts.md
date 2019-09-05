---
title: Kontakte und contactsui in xamarin. IOS
description: In diesem Artikel wird das Arbeiten mit den neuen Benutzeroberflächen-Frameworks in einer xamarin. IOS-App behandelt. Diese Frameworks ersetzen das vorhandene Adressbuch und die Adressbuch-Benutzeroberfläche, die in früheren Versionen von IOS verwendet wurde.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 96dbb60b8754223203394745bc86af2297cb5ff3
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278540"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Kontakte und contactsui in xamarin. IOS

_In diesem Artikel wird das Arbeiten mit den neuen Benutzeroberflächen-Frameworks in einer xamarin. IOS-App behandelt. Diese Frameworks ersetzen das vorhandene Adressbuch und die Adressbuch-Benutzeroberfläche, die in früheren Versionen von IOS verwendet wurde._

Mit der Einführung von IOS 9 hat Apple zwei neue Frameworks `Contacts` (und `ContactsUI`) veröffentlicht, die das vorhandene Adressbuch und die Benutzeroberflächen-Frameworks des Adressbuchs ersetzen, die von IOS 8 und früher verwendet werden.

Die beiden neuen Frameworks enthalten die folgenden Funktionen:

- [**Kontakte**](#contacts) : bietet Zugriff auf die Kontaktlisten Daten des Benutzers.
  Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für Thread sicheren, schreibgeschützten Zugriff optimiert.

- [**Contactsui**](#contactsui) : stellt xamarin. IOS-Benutzeroberflächen Elemente zum Anzeigen, bearbeiten, auswählen und Erstellen von Kontakten auf IOS-Geräten bereit.

[![](contacts-images/add01.png "Ein Beispiel für ein Kontaktblatt auf einem IOS-Gerät")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Die vorhandenen `AddressBook` - `AddressBookUI` und-Frameworks, die von IOS 8 (und früher) verwendet werden, sind in ios 9 veraltet und sollten `Contacts` so `ContactsUI` bald wie möglich durch die neuen Frameworks und für jede vorhandene xamarin. IOS-App ersetzt werden. Neue apps sollten für die neuen Frameworks geschrieben werden.




In den folgenden Abschnitten werden diese neuen Frameworks erläutert und erläutert, wie Sie in einer xamarin. IOS-App implementiert werden.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Das Contacts-Framework

Das Contacts-Framework bietet xamarin. IOS Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für Thread sicheren, schreibgeschützten Zugriff optimiert.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Contact-Objekte

Die `CNContact` -Klasse bietet Thread sicheren, schreibgeschützten Zugriff auf die Eigenschaften eines Kontakts, wie z. b. Name, Adresse oder Telefonnummer. `CNContact`Funktionen wie a `NSDictionary` und enthalten mehrere schreibgeschützte Auflistungen von Eigenschaften (z. b. Adressen oder Telefonnummern):

[![](contacts-images/contactobjects.png "Übersicht über das Kontakt Objekt")](contacts-images/contactobjects.png#lightbox)

Für alle Eigenschaften, die mehrere Werte aufweisen können (z. b. e-Mail-Adresse oder Telefonnummern), werden Sie `NSLabeledValue` als Array von-Objekten dargestellt. `NSLabeledValue`ist ein Thread sicheres Tupel, das aus einem schreibgeschützten Satz von Bezeichnungen und Werten besteht, wobei die Bezeichnung den Wert für den Benutzer definiert (z. b. Home oder Work-e-Mail). Das Contacts-Framework bietet eine Auswahl vordefinierter Bezeichnungen (über `CNLabelKey` die `CNLabelPhoneNumberKey` statischen Klassen und), die Sie in Ihrer APP verwenden können, oder Sie haben die Möglichkeit, benutzerdefinierte Bezeichnungen für Ihre Anforderungen zu definieren.

Verwenden Sie für jede xamarin. IOS-APP, die die Werte eines vorhandenen Kontakts anpassen muss (oder neue erstellen), die `NSMutableContact` -Version der-Klasse und deren Unterklassen ( `CNMutablePostalAddress`z. b.).

Der folgende Code erstellt z. b. einen neuen Kontakt und fügt ihn der Auflistung von Kontakten des Benutzers hinzu:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Add email addresses
var homeEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@mac.com"));
var workEmail = new CNLabeledValue<NSString>(CNLabelKey.Work, new NSString("john.appleseed@apple.com"));
contact.EmailAddresses = new CNLabeledValue<NSString>[] { homeEmail, workEmail };

// Add phone numbers
var cellPhone = new CNLabeledValue<CNPhoneNumber>(CNLabelPhoneNumberKey.iPhone, new CNPhoneNumber("713-555-1212"));
var workPhone = new CNLabeledValue<CNPhoneNumber>("Work", new CNPhoneNumber("408-555-1212"));
contact.PhoneNumbers = new CNLabeledValue<CNPhoneNumber>[] { cellPhone, workPhone };

// Add work address
var workAddress = new CNMutablePostalAddress()
{
    Street = "1 Infinite Loop",
    City = "Cupertino",
    State = "CA",
    PostalCode = "95014"
};
contact.PostalAddresses = new CNLabeledValue<CNPostalAddress>[] { new CNLabeledValue<CNPostalAddress>(CNLabelKey.Work, workAddress) };

// Add birthday
var birthday = new NSDateComponents()
{
    Day = 1,
    Month = 4,
    Year = 1984
};
contact.Birthday = birthday;

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

// Attempt to save changes
NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error))
{
    Console.WriteLine("New contact saved");
}
else
{
    Console.WriteLine("Save error: {0}", error);
}
```

Wenn dieser Code auf einem IOS 9-Gerät ausgeführt wird, wird der Benutzer Sammlung ein neuer Kontakt hinzugefügt. Beispiel:

[![](contacts-images/add01.png "Ein neuer Kontakt, der der Sammlung des Benutzers hinzugefügt wurde.")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>An Formatierung und Lokalisierung wenden

Das Contacts-Framework enthält mehrere Objekte und Methoden, die Ihnen helfen können, Inhalte zu formatieren und zu lokalisieren, damit Sie dem Benutzer angezeigt werden. Der folgende Code formatiert z. b. den Namen und die Postanschrift für die Anzeige ordnungsgemäß.

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Für Eigenschafts Bezeichnungen, die Sie in der Benutzeroberfläche Ihrer App anzeigen werden, verfügt das Contact Framework über Methoden zum Lokalisieren dieser Zeichen folgen. Dies basiert auch auf dem aktuellen Gebiets Schema des IOS-Geräts, auf dem die app ausgeführt wird. Beispiel:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Vorhandene Kontakte werden abgerufen.

Wenn Sie eine Instanz der `CNContactStore` -Klasse verwenden, können Sie Kontaktinformationen aus der Contact-Datenbank des Benutzers abrufen. `CNContactStore` Enthält alle Methoden, die für das Abrufen oder Aktualisieren von Kontakten und Gruppen aus der Datenbank erforderlich sind. Da diese Methoden synchron sind, wird empfohlen, Sie in einem Hintergrund Thread auszuführen, um die Blockierung der Benutzeroberfläche zu verhindern.

Mithilfe von Prädikaten (aus der `CNContact` -Klasse erstellt) können Sie die zurückgegebenen Ergebnisse filtern, wenn Sie Kontakte aus der Datenbank abrufen. Zum Abrufen von Kontakten, die die Zeichen `Appleseed`Folge enthalten, verwenden Sie den folgenden Code:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Generische und Verbund Prädikate werden vom Contacts-Framework nicht unterstützt.

Verwenden Sie z. b. den folgenden Code, um den Abruf nur auf die **givenName** -Eigenschaft und die **familyName** -Eigenschaft des Kontakts zu beschränken:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Verwenden Sie abschließend den folgenden Code, um die Datenbank zu durchsuchen und die Ergebnisse zurückzugeben:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Wenn dieser Code nach dem Beispiel ausgeführt wird, das wir im obigen Abschnitt "Contacts"- **Objekt** erstellt haben, würde er den Kontakt "John Appleseed" zurückgeben, den wir soeben erstellt haben.

### <a name="contact-access-privacy"></a>Kontaktdaten Schutz

Da Endbenutzer den Zugriff auf Ihre Kontaktinformationen pro Anwendung gewähren oder verweigern können, wird beim ersten Aufruf `CNContactStore`von ein Dialog angezeigt, in dem Sie aufgefordert werden, den Zugriff für Ihre APP zuzulassen.

Die Berechtigungs Anforderung wird nur einmal angezeigt, wenn die APP zum ersten Mal ausgeführt wird und nachfolgende Ausführungen oder Aufrufe von `CNContactStore` die Berechtigung verwenden, die der Benutzer zu diesem Zeitpunkt ausgewählt hat.

Sie sollten Ihre APP so entwerfen, dass Sie den Benutzer ordnungsgemäß verarbeitet, der den Zugriff auf Ihre Kontaktdatenbank verweigert.

#### <a name="fetching-partial-contacts"></a>Teil Kontakte werden abgerufen.

Bei einem _partiellen Kontakt_ handelt es sich um einen Kontakt, bei dem nur einige der verfügbaren Eigenschaften aus dem Kontakt Speicher für abgerufen wurden. Wenn Sie versuchen, auf eine Eigenschaft zuzugreifen, die nicht zuvor abgerufen wurde, wird eine Ausnahme ausgelöst.

Sie können problemlos überprüfen, ob ein angegebener Kontakt über die gewünschte Eigenschaft verfügt, `IsKeyAvailable` indem `AreKeysAvailable` Sie entweder die `CNContact` -Methode oder die-Methode der-Instanz verwenden. Beispiel:

```csharp
// Does the contact contain the requested key?
if (!contact.IsKeyAvailable(CNContactOption.PostalAddresses)) {
    // No, re-request to pull required info
    var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName, CNContactKey.PostalAddresses};
    var store = new CNContactStore();
    NSError error;
    contact = store.GetUnifiedContact(contact.Identifier, fetchKeys, out error);
}
```

> [!IMPORTANT]
> Die `GetUnifiedContact` - `GetUnifiedContacts` Methode und die `CNContactStore` -Methode der-Klasse geben _nur_ einen partiellen Kontakt zurück, der auf die von den angegebenen Abruf Schlüsseln angeforderten Eigenschaften beschränkt ist

### <a name="unified-contacts"></a>Unified Contacts

Ein Benutzer hat möglicherweise unterschiedliche Quellen mit Kontaktinformationen für eine einzelne Person in der Kontaktdatenbank (z. b. icloud, Facebook oder Google Mail). In IOS-und OS X-apps werden diese Kontaktinformationen automatisch miteinander verknüpft und dem Benutzer als einzelner _einheitlicher Kontakt_angezeigt:

[![](contacts-images/unified01.png "Übersicht über Unified Contacts")](contacts-images/unified01.png#lightbox)

Dieser einheitliche Kontakt ist eine temporäre, in-Memory-Ansicht der Link Kontaktinformationen, die einen eigenen eindeutigen Bezeichner erhalten (der zum erneuten Abrufen des Kontakts bei Bedarf verwendet werden sollte). Standardmäßig gibt das Contacts-Framework nach Möglichkeit einen vereinheitlichten Kontakt zurück.

### <a name="creating-and-updating-contacts"></a>Erstellen und Aktualisieren von Kontakten

Wie wir im obigen Abschnitt " [Contact Objects](#Contact_Objects) " gesehen haben, verwenden `CNContactStore` Sie eine und `CNMutableContact` eine Instanz von, um neue Kontakte zu erstellen, die dann mithilfe von in `CNSaveRequest`die Kontaktdatenbank des Benutzers geschrieben werden:

```csharp
// Create a new Mutable Contact (read/write)
var contact = new CNMutableContact();

// Set standard properties
contact.GivenName = "John";
contact.FamilyName = "Appleseed";

// Save new contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.AddContact(contact, store.DefaultContainerIdentifier);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("New contact saved");
} else {
    Console.WriteLine("Save error: {0}", error);
}
```

Ein `CNSaveRequest` kann auch verwendet werden, um mehrere Kontakt-und Gruppen Änderungen in einem einzelnen Vorgang zwischenzuspeichern und diese `CNContactStore`Änderungen in zu verpacken.

Zum Aktualisieren eines nicht änderbaren Kontakts, der von einem Abruf Vorgang abgerufen wurde, müssen Sie zuerst eine änderbare Kopie anfordern, die Sie dann ändern und im Kontakt Speicher wieder speichern. Beispiel:

```csharp
// Get mutable copy of contact
var mutable = contact.MutableCopy() as CNMutableContact;
var newEmail = new CNLabeledValue<NSString>(CNLabelKey.Home, new NSString("john.appleseed@xamarin.com"));

// Append new email
var emails = new NSObject[mutable.EmailAddresses.Length+1];
mutable.EmailAddresses.CopyTo(emails,0);
emails[mutable.EmailAddresses.Length+1] = newEmail;
mutable.EmailAddresses = emails;

// Update contact
var store = new CNContactStore();
var saveRequest = new CNSaveRequest();
saveRequest.UpdateContact(mutable);

NSError error;
if (store.ExecuteSaveRequest(saveRequest, out error)) {
    Console.WriteLine("Contact updated.");
} else {
    Console.WriteLine("Update error: {0}", error);
}
```

### <a name="contact-change-notifications"></a>Kontakt Änderungs Benachrichtigungen

Wenn ein Kontakt geändert wird, stellt der Kontakt Speicher eine `CNContactStoreDidChangeNotification` im Standard-Benachrichtigungs Center bereit. Wenn Sie alle Kontakte zwischengespeichert oder aktuell angezeigt werden, müssen Sie diese Objekte aus dem Kontakt Speicher (`CNContactStore`) aktualisieren.

### <a name="containers-and-groups"></a>Container und Gruppen

Die Kontakte eines Benutzers können entweder lokal auf dem Gerät des Benutzers oder als Kontakte mit dem Gerät von einem oder mehreren Server Konten (wie Facebook oder Google) vorhanden sein. Jeder Pool von Kontakten verfügt über einen eigenen _Container_ , und ein bestimmter Kontakt darf nur in einem Container vorhanden sein.

[![](contacts-images/containers01.png "Übersicht über Container und Gruppen")](contacts-images/containers01.png#lightbox)

Einige Container ermöglichen das Anordnen von Kontakten in eine oder mehrere _Gruppen_ oder _Untergruppen_. Dieses Verhalten hängt vom Sicherungs Speicher für einen bestimmten Container ab. Icloud hat z. b. nur einen Container, kann jedoch viele Gruppen (aber keine Untergruppen) enthalten. Microsoft Exchange hingegen unterstützt keine Gruppen, Sie können jedoch mehrere Container (einen für jeden Exchange-Ordner) enthalten.

[![](contacts-images/containers02.png "Überlappen innerhalb von Containern und Gruppen")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Das contactsui-Framework

In Situationen, in denen Ihre Anwendung keine benutzerdefinierte Benutzeroberfläche präsentieren muss, können Sie das contactsui-Framework verwenden, um Benutzeroberflächen Elemente zum Anzeigen, bearbeiten, auswählen und Erstellen von Kontakten in ihrer xamarin. IOS-APP zu präsentieren.

Mithilfe der integrierten Steuerelemente von Apple reduzieren Sie nicht nur die Menge an Code, die Sie zum unterstützen von Kontakten in ihrer xamarin. IOS-app erstellen müssen, sondern stellen eine konsistente Schnittstelle für die Benutzer der APP dar.

### <a name="the-contact-picker-view-controller"></a>Der Ansichts Controller der Kontakt Auswahl

Der Ansichts Controller der Kontakt`CNContactPickerViewController`Auswahl () verwaltet die Standardansicht für die Kontakt Auswahl, mit der der Benutzer einen Kontakt oder eine Kontakt Eigenschaft aus der Kontaktdatenbank des Benutzers auswählen kann. Der Benutzer kann einen oder mehrere Kontakte (basierend auf seiner Verwendung) auswählen, und der Ansichts Controller der Kontakt Auswahl fordert vor dem Anzeigen der Auswahl keine Berechtigung zur Eingabe auf.

Vor dem Aufrufen der `CNContactPickerViewController` -Klasse definieren Sie, welche Eigenschaften der Benutzer auswählen und Prädikate definieren kann, um die Anzeige und Auswahl von Kontakt Eigenschaften zu steuern.

Verwenden Sie eine Instanz der-Klasse, die von `CNContactPickerDelegate` erbt, um auf die Interaktion des Benutzers mit der Auswahl zu reagieren. Beispiel:

```csharp
using System;
using System.Linq;
using UIKit;
using Foundation;
using Contacts;
using ContactsUI;

namespace iOS9Contacts
{
    public class ContactPickerDelegate: CNContactPickerDelegate
    {
        #region Constructors
        public ContactPickerDelegate ()
        {
        }

        public ContactPickerDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ContactPickerDidCancel (CNContactPickerViewController picker)
        {
            Console.WriteLine ("User canceled picker");

        }

        public override void DidSelectContact (CNContactPickerViewController picker, CNContact contact)
        {
            Console.WriteLine ("Selected: {0}", contact);
        }

        public override void DidSelectContactProperty (CNContactPickerViewController picker, CNContactProperty contactProperty)
        {
            Console.WriteLine ("Selected Property: {0}", contactProperty);
        }
        #endregion
    }
}
```

Um dem Benutzer die Auswahl einer e-Mail-Adresse aus den Kontakten in der Datenbank zu ermöglichen, können Sie den folgenden Code verwenden:

```csharp
// Create a new picker
var picker = new CNContactPickerViewController();

// Select property to pick
picker.DisplayedPropertyKeys = new NSString[] {CNContactKey.EmailAddresses};
picker.PredicateForEnablingContact = NSPredicate.FromFormat("emailAddresses.@count > 0");
picker.PredicateForSelectionOfContact = NSPredicate.FromFormat("emailAddresses.@count == 1");

// Respond to selection
picker.Delegate = new ContactPickerDelegate();

// Display picker
PresentViewController(picker,true,null);
```

### <a name="the-contact-view-controller"></a>Der Kontakt Ansichts Controller

Die Contact View Controller (`CNContactViewController`)-Klasse stellt einen Controller bereit, um dem Endbenutzer eine Standard Kontaktansicht zu präsentieren. In der Kontaktansicht können neue, unbekannte oder vorhandene Kontakte angezeigt werden, und der Typ muss angegeben werden, bevor die Sicht angezeigt wird, indem der korrekte statische`FromNewContact`Konstruktor `FromContact`(, `FromUnknownContact`,) aufgerufen wird. Beispiel:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie in einer xamarin. IOS-Anwendung mit dem Contact-und Contact-Frameworks arbeiten. Zuerst wurden die unterschiedlichen Typen von Objekten behandelt, die das Kontakt Framework bereitstellt, und es wird erläutert, wie Sie Sie zum Erstellen neuer oder vorhandener Kontakte verwenden können. Außerdem wurde das Framework zum Kontaktieren der Benutzeroberfläche zum Auswählen vorhandener Kontakte und zum Anzeigen von Kontaktinformationen untersucht.


## <a name="related-links"></a>Verwandte Links

- [Beispiel für Kontakte](https://docs.microsoft.com/samples/xamarin/ios-samples/contacts/)
- [Neuerungen in ios 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Framework-Referenz zu Kontakten](https://developer.apple.com/documentation/contacts?language=objc)
- [Contactsui-frameworkverweis](https://developer.apple.com/documentation/contactsui?language=objc)
