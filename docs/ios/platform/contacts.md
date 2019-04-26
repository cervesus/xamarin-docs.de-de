---
title: Kontakte und ContactsUI in Xamarin.iOS
description: Dieser Artikel beschreibt das Arbeiten mit dem neue Kontakte und Kontakte-UI-Frameworks in einer Xamarin.iOS-app. Diese Frameworks ersetzen vorhandenes Adressbuchs und Adresse Book-Benutzeroberfläche, die in früheren Versionen von iOS verwendet.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: e3f1533605d08df58d8d257714dd8135690c0e5d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388982"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Kontakte und ContactsUI in Xamarin.iOS

_Dieser Artikel beschreibt das Arbeiten mit dem neue Kontakte und Kontakte-UI-Frameworks in einer Xamarin.iOS-app. Diese Frameworks ersetzen vorhandenes Adressbuchs und Adresse Book-Benutzeroberfläche, die in früheren Versionen von iOS verwendet._

Apple hat mit der Einführung von iOS 9 sind zwei neue Frameworks vorgestellt, `Contacts` und `ContactsUI`, ersetzen Sie das vorhandene Adressbuch und Adressbuch Benutzeroberflächen-Frameworks von iOS 8 und frühere Versionen verwendet.

Die beiden neuen Frameworks enthalten die folgende Funktionen:

- [**Kontakte** ](#contacts) -bietet Zugriff auf die Daten des Benutzers Kontaktliste.
    Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicher, schreibgeschützten Zugriff optimiert.

- [**ContactsUI** ](#contactsui) -bietet der Xamarin.iOS-UI-Elemente angezeigt werden, bearbeiten, wählen und Kontakte auf iOS-Geräten erstellen.

[![](contacts-images/add01.png "Ein Beispiel für Contact-Tabelle auf einem iOS-Gerät")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Die vorhandene `AddressBook` und `AddressBookUI` Frameworks von iOS 8 verwendet wird (und früher) sind veraltet in iOS 9 und ersetzt werden soll, mit dem neuen `Contacts` und `ContactsUI` Frameworks so schnell wie möglich für alle vorhandenen Xamarin.iOS-app. Neue apps sollten für die neue Frameworks geschrieben werden.




In den folgenden Abschnitten werde wir einen Blick auf diese neue Frameworks und wie Sie diese in einer Xamarin.iOS-app zu implementieren.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Das Framework für Kontakte

Die Contacts-Framework bietet es sich um Xamarin.iOS-Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicher, schreibgeschützten Zugriff optimiert.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Contact-Objekte

Die `CNContact` Klasse bietet sicheren, nur-Lese Threadzugriffs auf die Eigenschaften eines Kontakts, z. B. Name, Adresse oder Telefonnummer. `CNContact` Funktionen, wie z.B. eine `NSDictionary` und enthält mehrere schreibgeschützten Auflistungen von Eigenschaften (z. B. Adressen oder Telefonnummern):

[![](contacts-images/contactobjects.png "Wenden Sie sich an Object-Überblick")](contacts-images/contactobjects.png#lightbox)

Bei jeder Eigenschaft, die mehrere Werte (z. B. e-Mail-Adresse oder Telefonnummer Zahlen) haben kann, werden sie als Bytearray dargestellt werden `NSLabeledValue` Objekte. `NSLabeledValue` ein Thread sicher Tupel bestehend aus einem nur-Lese Satz von Bezeichnungen und Werte, in dem die Bezeichnung der Wert für den Benutzer (z. B. Heimnetzwerk oder e-Mail) definiert. Die Contacts-Framework bietet eine Auswahl von vordefinierten Bezeichnungen (über die `CNLabelKey` und `CNLabelPhoneNumberKey` statische Klassen), die Sie in Ihrer app verwenden können, oder Sie haben die Möglichkeit, benutzerdefinierte Bezeichnungen für Ihre Anforderungen zu definieren.

Verwenden Sie für eine Xamarin.iOS-app, die die Werte eines schon vorhandenen Kontakts anpassen (oder neue erstellen), die `NSMutableContact` Version der Klasse und ihrer untergeordneten Klassen (z. B. `CNMutablePostalAddress`).

Der folgende Code wird z. B. einen neuen Kontakt erstellen und fügen Sie es dem Benutzer die Sammlung von Kontakten hinzu:

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

Wenn dieser Code auf iOS 9-Geräten ausgeführt wird, wird der Benutzer-Auflistung ein neuer Kontakt hinzugefügt werden. Zum Beispiel:

[![](contacts-images/add01.png "Einen neuen Kontakt, der Benutzer der Auflistung hinzugefügt wurde")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Wenden Sie sich an Formatierungen und Lokalisierung

Die Contacts-Framework enthält verschiedene Objekte und Methoden, die Ihnen helfen, formatieren und Inhalt für die Anzeige für den Benutzer zu lokalisieren. Der folgende Code würde beispielsweise einen Namen für die Kontakte und e-Mail-Adresse für die Anzeige ordnungsgemäß formatieren:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Für die Eigenschaft-Bezeichnern, die Sie in Ihrer app Benutzeroberfläche anzeigen lassen, hat das Framework wenden Sie sich an Methoden für diese Zeichenfolgen auch lokalisieren. In diesem Fall basiert dies auf dem aktuellen Gebietsschema des iOS-Geräts, die die app ausgeführt wird. Zum Beispiel:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Abrufen der existierenden Kontakte

Mit einer Instanz von der `CNContactStore` -Klasse, können Sie Kontaktinformationen aus der Datenbank des Benutzers Kontakte abrufen. Die `CNContactStore` enthält alle Methoden zum Abrufen oder Aktualisieren von Kontakten und Gruppen aus der Datenbank erforderlich. Da diese Methoden synchron sind, wird empfohlen, dass Sie diese in einem Hintergrundthread, um zu verhindern, die Blockierung der Benutzeroberfläche ausführen.

Mithilfe von Prädikaten (erstellt aus den `CNContact` Klasse), können Sie die Ergebnisse zurückgegeben, wenn das Abrufen von Kontakten aus der Datenbank filtern. Nur Kontakte abzurufen, die die Zeichenfolge enthalten `Appleseed`, verwenden Sie den folgenden Code:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Generische und zusammengesetzte Prädikate werden durch das Framework Kontakte nicht unterstützt.

Beispielsweise, um das Beschränken des Abruf nur der **"givenName"** und **FamilyName** Eigenschaften des Kontakts, verwenden Sie den folgenden Code:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Zum Durchsuchen der Datenbank und die Ergebnisse zurückzugeben, verwenden Sie abschließend den folgenden Code:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Wenn dieser Code nach dem Beispiel, die wir ausgeführt wurde in erstellt die **Contacts-Objekts** Abschnitt oben zurückgibt "John Appleseed" Wenden Sie sich an, die wir gerade erstellt haben.

### <a name="contact-access-privacy"></a>Wenden Sie sich an den Zugriff von Datenschutz

Da Endbenutzer können gewähren oder Verweigern des Zugriffs auf die Kontaktinformationen auf einer Basis pro Anwendung, beim ersten Sie stellen einen Aufruf der `CNContactStore`, ein Dialogfeld wird gebeten, die Zugriff für Ihre app angezeigt werden.

Der berechtigungsanforderung wird nur einmal angezeigt werden, die app ausgeführt, und nachfolgende wird, erstmalig ausgeführt wird oder Aufrufe von der `CNContactStore` verwenden Sie die Berechtigung, die der Benutzer zu diesem Zeitpunkt ausgewählt werden.

Entwerfen Sie Ihre app, damit den Benutzer Verweigern des Zugriffs auf ihre Kontaktdatenbank ordnungsgemäß verarbeitet.

#### <a name="fetching-partial-contacts"></a>Partielle Kontakte abrufen

Ein _teilweise wenden Sie sich an_ wird ein Kontakt, der nur einige der verfügbaren Eigenschaften aus dem Store wenden Sie sich an, für abgerufen wurden. Wenn Sie versuchen, eine Eigenschaft zuzugreifen, die nicht bereits abgerufen wurde, führt dies zu einer Ausnahme.

Sie können ganz einfach überprüfen, um festzustellen, ob Sie ein bestimmter Kontakt die gewünschte Eigenschaft hat, indem Sie entweder die `IsKeyAvailable` oder `AreKeysAvailable` Methoden der `CNContact` Instanz. Zum Beispiel:

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
> Die `GetUnifiedContact` und `GetUnifiedContacts` Methoden der `CNContactStore` Klasse _nur_ einen partiellen Kontakt beschränkt auf die Eigenschaften von die Fetch-Schlüssel bereitgestellte angefordert zurückgeben.

### <a name="unified-contacts"></a>Einheitliche Kontakte

Ein Benutzer möglicherweise unterschiedliche Quellen von Kontaktinformationen für eine einzelne Person in ihre Kontaktdatenbank (z.B. iCloud, Facebook oder Google Mail). In iOS und OS X-apps können diese Kontaktinformationen automatisch miteinander verknüpft sind und angezeigt, die für den Benutzer als einzelne _Unified wenden Sie sich an_:

[![](contacts-images/unified01.png "Übersicht über die einheitliche Kontakte")](contacts-images/unified01.png#lightbox)

Diese einheitliche wenden Sie sich an ist eine temporäre in-Memory-Sicht der Kontaktinformationen Link, die einen eigenen eindeutigen Bezeichner zugewiesen wird (die, den Kontakt erneut abzurufen, bei Bedarf verwendet werden soll). Standardmäßig wird das Framework Kontakte einen Kontakt Unified, wann immer möglich zurückgegeben.

### <a name="creating-and-updating-contacts"></a>Erstellen und Aktualisieren von Kontakten

Wie in beschrieben der [Objekte wenden Sie sich an](#Contact_Objects) weiter oben, Sie verwenden eine `CNContactStore` und einer Instanz von einer `CNMutableContact` zum Erstellen neuer Kontakte, die Sie dann auf das geschrieben werden wenden Sie sich an Datenbank mithilfe einer `CNSaveRequest`:

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

Ein `CNSaveRequest` kann auch verwendet werden, zum Zwischenspeichern von mehrere Kontakt- und Änderungen in einem Vorgang, und diese Änderungen für batch die `CNContactStore`.

Um eine nicht änderbare wenden Sie sich an, die von einem Abrufvorgang abgerufen zu aktualisieren, müssen Sie zuerst eine änderbare Kopie anfordern, die Sie ändern und speichern Sie dann die Kontaktspeicher. Zum Beispiel:

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

### <a name="contact-change-notifications"></a>Wenden Sie sich an Änderungsbenachrichtigungen

Bei einer Änderung ein Kontakts, der Kontakt-Store stellt ein `CNContactStoreDidChangeNotification` auf die Standard-Mitteilungszentrale. Wenn Sie zwischengespeichert haben oder aktuell im Kontakte angezeigt werden, müssen Sie die Elemente aus der Contact-Store zu aktualisieren (`CNContactStore`).

### <a name="containers-and-groups"></a>Container und Gruppen

Kontakte eines Benutzers können entweder lokal auf dem Gerät des Benutzers oder als Kontakte, die auf dem Gerät synchronisiert werden, von einem oder mehreren Serverkonten (z.B. Facebook oder Google) vorhanden sein. Jeder Pool von Kontakten verfügt über eine eigene _Container_ und ein bestimmter Kontakt kann nur in einem Container vorhanden sein.

[![](contacts-images/containers01.png "Container und Gruppen (Übersicht)")](contacts-images/containers01.png#lightbox)

Einige Container ermöglichen Kontakte in eine oder mehrere angeordnet werden _Gruppen_ oder _Untergruppen_. Dieses Verhalten ist abhängig von der Sicherungsspeicher für einen bestimmten Container. Z. B. kann iCloud weist nur einen Container jedoch haben viele Gruppen (aber keine untergeordneten Gruppen). Microsoft Exchange auf der anderen Seite Gruppen nicht unterstützt. jedoch kann mehrere Container (eine für jeden Exchange-Ordner) haben.

[![](contacts-images/containers02.png "In Container und Gruppen überlappen")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Das Framework ContactsUI

In Situationen, in denen Ihre Anwendung nicht notwendigerweise um eine benutzerdefinierte Benutzeroberfläche anzuzeigen, können Sie das ContactsUI-Framework zur Darstellung der Elemente der Benutzeroberfläche anzeigen, bearbeiten, wählen und Kontakte in Ihrer Xamarin.iOS-app erstellen.

Mithilfe von Apple integrierte Steuerelemente reduzieren Sie nicht nur die Menge des Codes, die Sie erstellen, um Kontakte zu Ihrer Xamarin.iOS-app zu unterstützen, aber Sie stellen eine konsistente Schnittstelle, die Benutzer der app.

### <a name="the-contact-picker-view-controller"></a>Der Kontakt-Auswahl-View-Controller

Der Kontakt-Auswahl-View-Controller (`CNContactPickerViewController`) verwaltet der Kontakt-Auswahl-Standardansicht, die dem Benutzer ermöglicht, wählen Sie einen Kontakt oder eine Kontakt-Eigenschaft der Datenbank des Benutzers wenden Sie sich an. Der Benutzer kann eine oder mehrere Kontakt (basierend auf dessen Nutzung) auswählen, und wenden Sie sich an Auswahl View-Controller fordert nicht für die Berechtigung vor der Anzeige der Auswahl.

Vor dem Aufruf der `CNContactPickerViewController` -Klasse, Sie definieren die Eigenschaften, die der Benutzer auswählen kann, und Definieren von Prädikaten übereinstimmen, zum Steuern der Anzeige und die Auswahl der Eigenschaften, wenden Sie sich an.

Verwenden Sie eine Instanz der Klasse, die von erbt `CNContactPickerDelegate` , auf die Interaktion mit der Auswahl des Benutzers zu reagieren. Zum Beispiel:

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

Damit wird den Benutzer eine e-Mail-Adresse aus den Kontakten in ihre Datenbank auswählen, können Sie den folgenden Code verwenden:

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

### <a name="the-contact-view-controller"></a>Wenden Sie sich an Ansichtscontroller

Der Kontakt-View-Controller (`CNContactViewController`)-Klasse bietet einen Controller aus, um eine Kontakt-Standardansicht für den Endbenutzer zu präsentieren. Wenden Sie sich an der Ansicht können neue, die unbekannt oder die vorhandene Kontakte angezeigt und der Typ muss angegeben werden, bevor die Ansicht angezeigt wird, durch den richtigen statischen Konstruktor aufrufen (`FromNewContact`, `FromUnknownContact`, `FromContact`). Beispiel:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit der wenden Sie sich an und wenden Sie sich an UI-Frameworks in einer Xamarin.iOS-Anwendung geführt. Erstens behandelt es die verschiedenen Typen von Objekten, die Kontakt-Framework bietet, und wie verwenden sie zum Erstellen oder existierenden Kontakte zugreifen. Er überprüft auch das Framework wenden Sie sich an der Benutzeroberfläche zum Auswählen der existierenden Kontakte und zum Anzeigen von Kontaktinformationen.


## <a name="related-links"></a>Verwandte Links

- [QuickContacts-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Neuigkeiten in iOS 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Frameworkverweis Kontakte](https://developer.apple.com/documentation/contacts?language=objc)
- [Frameworkverweis ContactsUI](https://developer.apple.com/documentation/contactsui?language=objc)
