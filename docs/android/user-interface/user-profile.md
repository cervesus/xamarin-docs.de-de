---
title: Benutzerprofil
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/22/2018
ms.openlocfilehash: 252a104118b0419f33abdf7f522ad8fc358e3f76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028713"
---
# <a name="user-profile"></a>Benutzerprofil

Android hat das Auflisten von Kontakten mit dem [contacgscontract](xref:Android.Provider.ContactsContract) -Anbieter seit API-Ebene 5 unterstützt. Beispielsweise ist das Auflisten von Kontakten so einfach wie die Verwendung der [contactcontracts. Contacts](xref:Android.Provider.ContactsContract.Contacts) -Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Ab Android 4 (API-Ebene 14) ist die [contactcontact. profile](xref:Android.Provider.ContactsContract.Profile) -Klasse über den `ContactsContract` Anbieter verfügbar. Der `ContactsContact.Profile` bietet Zugriff auf das persönliche Profil für den Besitzer eines Geräts, das Kontaktdaten wie den Namen und die Telefonnummer des Geräte Besitzers enthält.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Lesen und Schreiben von Kontaktdaten müssen Anwendungen die Berechtigungen für den `READ_CONTACTS` und `WRITE_CONTACTS` anfordern.
Außerdem müssen Anwendungen die Berechtigungen `READ_PROFILE` und `WRITE_PROFILE` anfordern, um das Benutzerprofil zu lesen und zu bearbeiten.

## <a name="updating-profile-data"></a>Aktualisieren von Profildaten

Nachdem diese Berechtigungen festgelegt wurden, kann eine Anwendung normale Android-Techniken verwenden, um mit den Daten des Benutzerprofils zu interagieren. Um z. b. den anzeigen amen des Profils zu aktualisieren, können Sie [contentresolver. Update](xref:Android.Content.ContentResolver.Update*) mit einem `Uri` aufrufen, das über die Eigenschaft [contactcontract. profile. contentrawcontactsuri](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri) abgerufen wurde, wie unten dargestellt:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Lesen von Profildaten

Durch das Ausgeben einer Abfrage an [contactcontact. profile. contenturi](xref:Android.Provider.ContactsContract.Profile.ContentUri) werden die Profildaten gelesen. Der folgende Code liest z. b. den anzeigen amen des Benutzerprofils:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Navigieren zum Benutzerprofil

Um zum Benutzerprofil zu navigieren, erstellen Sie zum Schluss eine Absicht mit einer `ActionView` Aktion und eine `ContactsContract.Profile.ContentUri` und übergeben Sie Sie dann wie folgt an die `StartActivity`-Methode:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

Wenn Sie den obigen Code ausführen, wird das Benutzerprofil wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot des Profils, das das Benutzerprofil von John Doe anzeigt](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Das Arbeiten mit dem Benutzerprofil ähnelt dem interagieren mit anderen Daten in Android und bietet ein zusätzliches Maß an Geräte Personalisierung.

## <a name="related-links"></a>Verwandte Links

- [Contactsproviderdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/contactsproviderdemo)
- [Einführung in Ice Cream Sandwich](https://www.android.com/about/ice-cream-sandwich/)
- [Android 4,0-Plattform](https://developer.android.com/sdk/android-4.0.html)
