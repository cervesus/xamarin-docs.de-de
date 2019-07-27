---
title: Benutzerprofil
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/22/2018
ms.openlocfilehash: bed346b33ac92f6a1c73cdd3b29fb70ba17c5e91
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509673"
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

Ab Android 4 (API-Ebene 14) ist die [contactcontact. profile](xref:Android.Provider.ContactsContract.Profile) -Klasse über den `ContactsContract` Anbieter verfügbar. `ContactsContact.Profile` Bietet Zugriff auf das persönliche Profil für den Besitzer eines Geräts, das Kontaktdaten wie den Namen des Geräte Besitzers und die Telefonnummer enthält.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Lesen und Schreiben von Kontaktdaten müssen Anwendungen die `READ_CONTACTS` Berechtigungen und `WRITE_CONTACTS` anfordern.
Außerdem müssen Anwendungen die `READ_PROFILE` Berechtigungen und `WRITE_PROFILE` anfordern, um das Benutzerprofil zu lesen und zu bearbeiten.

## <a name="updating-profile-data"></a>Aktualisieren von Profildaten

Nachdem diese Berechtigungen festgelegt wurden, kann eine Anwendung normale Android-Techniken verwenden, um mit den Daten des Benutzerprofils zu interagieren. Um z. b. den anzeigen amen des Profils zu aktualisieren, nennen Sie [contentresolver. Update](xref:Android.Content.ContentResolver.Update*) mit einem `Uri` , das über die Eigenschaft [contactcontract. profile. contentrawcontactsuri](xref:Android.Provider.ContactsContract.Profile.ContentRawContactsUri) abgerufen wurde, wie unten dargestellt:

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

Um zum Benutzerprofil zu navigieren, erstellen Sie abschließend eine Absicht mit einer `ActionView` Aktion, `ContactsContract.Profile.ContentUri` und übergeben Sie Sie dann wie `StartActivity` folgt an die-Methode:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);
StartActivity (intent);
```

Wenn Sie den obigen Code ausführen, wird das Benutzerprofil wie im folgenden Screenshot gezeigt angezeigt:

[![Screenshot des Profils, das das Benutzerprofil von John Doe anzeigt](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Das Arbeiten mit dem Benutzerprofil ähnelt dem interagieren mit anderen Daten in Android und bietet ein zusätzliches Maß an Geräte Personalisierung.

## <a name="related-links"></a>Verwandte Links

- [Contactsproviderdemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Einführung in Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4,0-Plattform](https://developer.android.com/sdk/android-4.0.html)
