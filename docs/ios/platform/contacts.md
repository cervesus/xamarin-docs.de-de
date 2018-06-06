---
title: Kontakte und ContactsUI in Xamarin.iOS
description: Dieser Artikel umfasst das Arbeiten mit dem neuen Kontakte und Kontakte UI Frameworks in einem Xamarin.iOS-app. Diese Frameworks Ersetzen der vorhandenen Adressbuch und Adresse Book-Benutzeroberfläche, die in früheren Versionen von iOS verwendet.
ms.prod: xamarin
ms.assetid: 7b6fb66a-5e19-4a5a-9ed2-f6b02af099af
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 60b59023e937215bc640aeb4e9858baa0533db14
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786677"
---
# <a name="contacts-and-contactsui-in-xamarinios"></a>Kontakte und ContactsUI in Xamarin.iOS

_Dieser Artikel umfasst das Arbeiten mit dem neuen Kontakte und Kontakte UI Frameworks in einem Xamarin.iOS-app. Diese Frameworks Ersetzen der vorhandenen Adressbuch und Adresse Book-Benutzeroberfläche, die in früheren Versionen von iOS verwendet._

Mit der Einführung von iOS 9, Apple hat zwei neue Frameworks, veröffentlicht `Contacts` und `ContactsUI`, ersetzen die vorhandene Adressbuch und Adressbuch Benutzeroberflächen-Frameworks, die von iOS 8 und früher verwendeten.

Die beiden neuen Frameworks enthalten die folgende Funktionen:

- [**Kontakte** ](#contacts) -ermöglicht den Zugriff auf die Daten des Benutzers Kontaktliste.
    Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicheren, nur-Lese Zugriff optimiert.

- [**ContactsUI** ](#contactsui) -Xamarin.iOS Benutzeroberflächenelemente bietet anzeigen, bearbeiten, wählen und Kontakte auf iOS-Geräte erstellen.

[![](contacts-images/add01.png "Ein Beispiel für Kontakt Blatt auf einem iOS-Gerät")](contacts-images/add01.png#lightbox)

> [!IMPORTANT]
> Die vorhandene `AddressBook` und `AddressBookUI` Frameworks Verwenden von iOS 8 (vor) sind in iOS 9 veraltet und sollte ersetzt werden, mit dem neuen `Contacts` und `ContactsUI` -Frameworks für alle bisherigen Xamarin.iOS app so bald wie möglich. Neue apps sollten mit den neuen Frameworks geschrieben werden.




In den folgenden Abschnitten führen wir einen Blick auf diese neuen Frameworks und wie diese in einer app Xamarin.iOS implementiert.

<a name="contacts" />

## <a name="the-contacts-framework"></a>Das Framework Kontakte

Das Kontakte-Framework bietet Xamarin.iOS Zugriff auf die Kontaktinformationen des Benutzers. Da die meisten apps nur schreibgeschützten Zugriff erfordern, wurde dieses Framework für den Thread sicheren, nur-Lese Zugriff optimiert.

<a name="Contact_Objects" />

### <a name="contact-objects"></a>Contact-Objekte

Die `CNContact` Klasse bietet sicheren, nur-Lese Threadzugriffs auf die Eigenschaften eines Kontakts, z. B. Name, Adresse oder Telefonnummern. `CNContact` Funktionen wie eine `NSDictionary` und enthält mehrere, schreibgeschützten Auflistungen von Eigenschaften (z. B. Adressen oder Telefonnummern):

[![](contacts-images/contactobjects.png "Wenden Sie sich an Objekt (Übersicht)")](contacts-images/contactobjects.png#lightbox)

Für jede Eigenschaft, die mehrere Werte (z. B. e-Mail-Adresse oder Telefonnummer Ziffern) enthalten kann, werden sie als Array von dargestellt werden `NSLabeledValue` Objekte. `NSLabeledValue` ist ein Tupel der Thread-sichere bestehend aus einem nur-Lese Satz von Bezeichnungen und Werte, in dem die Bezeichnung der Wert für den Benutzer (z. B. Heim- oder Arbeit e-Mail) definiert. Die Kontakte-Framework bietet eine Auswahl von vordefinierten Bezeichnungen (über die `CNLabelKey` und `CNLabelPhoneNumberKey` statische Klassen), die Sie in Ihrer app verwenden können oder Sie haben die Möglichkeit, benutzerdefinierte Etiketten für Ihre Anforderungen definieren.

Verwenden Sie für alle Xamarin.iOS app, die die Werte eines vorhandenen Kontakts anpassen (oder neue erstellen) muss der `NSMutableContact` Version der Klasse und ihre untergeordneten Klassen (z. B. `CNMutablePostalAddress`).

Im folgenden Code wird z. B. erstellen ein neues Kontakts und Auflistung der Benutzer, Kontakte hinzuzufügen:

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

Wenn dieser Code auf einem iOS 9-Gerät ausgeführt wird, wird der User-Auflistung ein neuer Kontakt hinzugefügt werden. Zum Beispiel:

[![](contacts-images/add01.png "Ein neuer Kontakt hinzugefügt, der User-Auflistung")](contacts-images/add01.png#lightbox)

### <a name="contact-formatting-and-localization"></a>Wenden Sie sich an Formatierung und Lokalisierung

Das Framework Kontakte enthält verschiedene Objekte und Methoden, die Ihnen dabei helfen, formatieren und Inhalt für die Anzeige für den Benutzer zu lokalisieren. Beispielsweise würde der folgende Code einen Namen für die Kontakte und e-Mail-Adresse für die Anzeige ordnungsgemäß formatieren:

```csharp
Console.WriteLine(CNContactFormatter.GetStringFrom(contact, CNContactFormatterStyle.FullName));
Console.WriteLine(CNPostalAddressFormatter.GetStringFrom(workAddress, CNPostalAddressFormatterStyle.MailingAddress));
```

Wenden Sie sich an Framework verfügt für die Eigenschaft Bezeichnungen, die Sie in Ihrer app-Benutzeroberfläche anzeigen werden Methoden zum Lokalisieren von diese Zeichenfolgen als auch. Erneut, basiert dies auf das aktuelle Gebietsschema des iOS-Gerät, die die app ausgeführt wird. Zum Beispiel:

```csharp
// Localized properties
Console.WriteLine(CNContact.LocalizeProperty(CNContactOptions.Nickname));
Console.WriteLine(CNLabeledValue<NSString>.LocalizeLabel(CNLabelKey.Home));
```

### <a name="fetching-existing-contacts"></a>Existierenden Kontakte abrufen

Mit einer Instanz von der `CNContactStore` -Klasse, können Sie Kontaktinformationen aus der Benutzerdatenbank Kontakte abzurufen. Die `CNContactStore` enthält alle Methoden für das Abrufen oder aktualisieren, Kontakte und Gruppen aus der Datenbank erforderlich sind. Da diese Methoden synchron sind, wird empfohlen, dass Sie sie in einem Hintergrundthread zum Blockieren der Benutzeroberflächenautomatisierungs ausführen.

Mithilfe von Prädikaten (erstellt aus den `CNContact` Klasse), können Sie die Ergebnisse zurückgegeben, wenn das Abrufen von Kontakten aus der Datenbank filtern. Nur Kontakte abzurufen, die die Zeichenfolge enthalten `Appleseed`, verwenden Sie den folgenden Code:

```csharp
// Create predicate to locate requested contact
var predicate = CNContact.GetPredicateForContacts("Appleseed");
```

> [!IMPORTANT]
> Generische und zusammengesetzte Prädikate werden nicht durch das Framework für Kontakte unterstützt.

Um beispielsweise nur das Abrufen von Daten zu beschränken die **"givenName"** und **FamilyName** Eigenschaften des Kontakts, verwenden Sie den folgenden Code:

```csharp
// Define fields to be searched
var fetchKeys = new NSString[] {CNContactKey.GivenName, CNContactKey.FamilyName};
```

Um die Datenbank zu suchen, und die Ergebnisse zurückgeben, verwenden Sie schließlich den folgenden Code:

```csharp
// Grab matching contacts
var store = new CNContactStore();
NSError error;
var contacts = store.GetUnifiedContacts(predicate, fetchKeys, out error);
```

Wenn dieser Code nach dem das Beispiel, die wir ausgeführt wurde in erstellt die **Kontakte Objekt** obigen Abschnitt zurückgibt "John Appleseed" Wenden Sie sich an, die soeben erstellt wurde.

### <a name="contact-access-privacy"></a>Wenden Sie sich an den Zugriff Datenschutz

Da Endbenutzer können gewähren oder Verweigern des Zugriffs auf ihre Kontaktinformationen für eine einzelne Anwendung, erstmalig Sie stellen einen Aufruf der `CNContactStore`, ein Dialogfeld wird angezeigt und darum gebeten, Zugriff auf Ihre app zu ermöglichen.

Die berechtigungsanforderung wird nur einmal angezeigt werden, die app, ausgeführt wurde, und nachfolgende ist, erstmalig ausgeführt wird, oder Aufrufe von der `CNContactStore` verwenden die Berechtigung, die der Benutzer zu diesem Zeitpunkt ausgewählt.

Entwerfen Sie Ihre app, damit es den Benutzer Verweigern des Zugriffs auf ihre Kontaktdatenbank ordnungsgemäß behandelt.

#### <a name="fetching-partial-contacts"></a>Partielle Kontakte abrufen

Ein _teilweise wenden Sie sich an_ wird ein Kontakt, der nur einige der verfügbaren Eigenschaften aus dem Kontakt Store für abgerufen wurden. Wenn Sie versuchen, eine Eigenschaft zuzugreifen, die nicht bereits abgerufen wurde, führt dies zu einer Ausnahme.

Sie können leicht überprüfen, um festzustellen, ob Sie ein bestimmter Kontakt die gewünschte Eigenschaft verfügt, indem Sie entweder die `IsKeyAvailable` oder `AreKeysAvailable` Methoden die `CNContact` Instanz. Zum Beispiel:

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
> Die `GetUnifiedContact` und `GetUnifiedContacts` Methoden die `CNContactStore` Klasse _nur_ einen partiellen Kontakt begrenzt auf die Eigenschaften, die vom bereitgestellten Fetch-Schlüssel angefordert zurückgeben.

### <a name="unified-contacts"></a>Einheitliche Kontakte

Ein Benutzer möglicherweise verschiedene Quellen von Kontaktinformationen für eine einzelne Person in ihrer Datenbank (z. B. iCloud, Facebook oder Google Mail). In iOS und OS X-apps können diese Kontaktinformationen automatisch miteinander verknüpft und angezeigt, die dem Benutzer als ein einzelnes _Unified wenden Sie sich an_:

[![](contacts-images/unified01.png "Übersicht über Unified Kontakte")](contacts-images/unified01.png#lightbox)

Unified Kontakt ist einen temporären, speicherresidenten Überblick über die Link-Kontaktinformationen, die seinen eigenen eindeutigen Bezeichner zugewiesen wird (die, den Kontakt erneut abzurufen, bei Bedarf verwendet werden soll). Standardmäßig wird das Framework Kontakte Unified Kontakt möglichst zurück.

### <a name="creating-and-updating-contacts"></a>Erstellen und Aktualisieren von Kontakten

Wie wir gesehen, in haben der [Kontaktobjekte](#Contact_Objects) Abschnitt oben, verwenden Sie eine `CNContactStore` und einer Instanz von eine `CNMutableContact` zum Erstellen neuer Kontakte, die dann an des Benutzers geschrieben werden wenden Sie sich an Datenbank mithilfe einer `CNSaveRequest`:

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

Ein `CNSaveRequest` kann auch verwendet werden, das Zwischenspeichern von mehreren Kontakt- und Gruppenobjekte Änderungen in einem einzigen Vorgang batch diese Änderungen an der `CNContactStore`.

Um ein nicht änderbarer Kontakt aus einem Abrufvorgang abgerufen zu aktualisieren, müssen Sie zuerst eine änderbare Kopie anfordern, die Sie ändern und in den Kontakt Speicher speichern. Zum Beispiel:

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

Bei einer Änderung ein Kontakts, der Kontakt-Speicher sendet eine `CNContactStoreDidChangeNotification` an Standardeinstellung Benachrichtigung Center. Wenn Sie zwischengespeichert haben oder Kontakte, die aktuell angezeigt werden, müssen Sie diese Objekte aus dem Store Kontakt aktualisieren (`CNContactStore`).

### <a name="containers-and-groups"></a>Container und Gruppen

Kontakte eines Benutzers können entweder lokal auf dem Gerät des Benutzers oder als aus einem oder mehreren Serverkonten (z. B. Facebook oder Google) auf dem Gerät synchronisierte Kontakte vorhanden sein. Jeder Pool von Kontakten verfügt über einen eigenen _Container_ ein bestimmtes Kontakts kann nur in einem Container befinden.

[![](contacts-images/containers01.png "Container und Gruppen (Übersicht)")](contacts-images/containers01.png#lightbox)

Einige Container ermöglichen die Kontakte in eine oder mehrere angeordnet werden _Gruppen_ oder _Untergruppen_. Dieses Verhalten ist abhängig von der Sicherungsspeicher für einen Container. Beispielsweise kann iCloud hat nur ein Container aber viele Gruppen (jedoch keine untergeordneten Gruppen). Microsoft Exchange unterstützt keine Gruppen andererseits, jedoch können mehrere Container (eine für jeden Exchange-Ordner) haben.

[![](contacts-images/containers02.png "Überschneidungen Sie Container und Gruppen")](contacts-images/containers02.png#lightbox)

<a name="contactsui" />

## <a name="the-contactsui-framework"></a>Das Framework ContactsUI

Für Situationen, in denen Ihre Anwendung keine benutzerdefinierte Benutzeroberfläche anzuzeigen muss, können Sie der ContactsUI-Framework verwenden, um Benutzeroberflächenelemente anzeigen, bearbeiten, auswählen und Erstellen von Kontakten in Ihrer app Xamarin.iOS darzustellen.

Mithilfe des Apple integrierte Steuerelemente verringern Sie nicht nur die Menge an Code, die Sie erstellen, um Kontakte in Ihrer app Xamarin.iOS unterstützen, sondern Sie stellen eine konsistente Oberfläche für Benutzer der app.

### <a name="the-contact-picker-view-controller"></a>Die Auswahl einer Kontakt-View-Controller

Wenden Sie sich an Picker-View-Controller (`CNContactPickerViewController`) verwaltet der Kontakt Auswahl einer Standardsicht, die dem Benutzer ermöglicht, wählen Sie einen Kontakt oder ein Kontakt-Eigenschaft aus der Benutzerdatenbank wenden Sie sich an. Der Benutzer kann eine oder mehrere Kontakt (basierend auf ihrer Verwendung) auswählen, und wenden Sie sich an Datumsauswahl View-Controller fordert nicht für die Berechtigung, ehe Sie die Auswahl.

Vor dem Aufrufen der `CNContactPickerViewController` -Klasse, Sie definieren, welche Eigenschaften kann der Benutzer auswählen und Definieren von Prädikaten übereinstimmen, zum Steuern der Anzeige und die Auswahl von Kontakteigenschaften.

Verwenden Sie eine Instanz der Klasse, die von erben `CNContactPickerDelegate` So reagieren Sie auf die Interaktion mit der Auswahl des Benutzers. Zum Beispiel:

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

Damit wird den Benutzer eine e-Mail-Adresse aus den Kontakten in ihre Datenbank auswählen, können Sie den folgenden Code:

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

### <a name="the-contact-view-controller"></a>Die Kontakt-View-Controller

Die Kontakt-View-Controller (`CNContactViewController`)-Klasse stellt einen Controller aus, um eine Standardansicht für wenden Sie sich an dem Endbenutzer präsentieren. Wenden Sie sich an der Ansicht können neue neu "," Unbekannt "oder" vorhandene Kontakte angezeigt und der Typ muss angegeben werden, bevor die Ansicht angezeigt wird, durch den richtigen statischen Konstruktor aufrufen (`FromNewContact`, `FromUnknownContact`, `FromContact`). Beispiel:

```csharp
// Create a new contact view
var view = CNContactViewController.FromContact(contact);

// Display the view
PresentViewController(view, true, null);
```

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit den Kontakt- und wenden Sie sich an UI-Frameworks in einer Anwendung Xamarin.iOS übernommen. Zuerst behandelt sie die verschiedenen Typen von Objekten, die Kontakt-Framework bietet, und wie Sie sie neu erstellen oder existierenden Kontakte zuzugreifen. Außerdem wird die wenden Sie sich an Benutzeroberflächen-Framework, um vorhandene Kontakte auswählen und Anzeigen von Kontaktinformationen untersucht.


## <a name="related-links"></a>Verwandte Links

- [QuickContacts-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/QuickContacts/)
- [Was ist neu in iOS 9](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Frameworkverweis Kontakte](https://developer.apple.com/documentation/contacts?language=objc)
- [ContactsUI Frameworkverweis](https://developer.apple.com/documentation/contactsui?language=objc)
