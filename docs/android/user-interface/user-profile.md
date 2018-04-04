---
title: Benutzerprofil
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/22/2018
ms.openlocfilehash: 1eaae86ab9eacf007eca792d96e889db6f367922
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="user-profile"></a>Benutzerprofil

Aufzählen von Kontakten mit Android unterstützt die [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) Anbieter seit API-Ebene 5. Auflisten von Kontakten beispielsweise so einfach wie mit ist der [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) -Klasse wie im folgenden Codebeispiel dargestellt:

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

Android 4 (API-Ebene 14), ab der [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) Klasse kann über die `ContactsContract` Anbieter. Die `ContactsContact.Profile` bietet Zugriff auf das persönliche Profil für den Besitzer eines Geräts, die Kontaktdaten, z. B. Name und Phone Besitzer des Geräts umfasst.


## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Lesen und Schreiben von Daten, Anwendungen anfordern müssen die `READ_CONTACTS` und `WRITE_CONTACTS` Berechtigungen bzw.
Darüber hinaus zum Lesen und bearbeiten das Benutzerprofil, Anwendungen müssen anfordern der `READ_PROFILE` und `WRITE_PROFILE` Berechtigungen.


## <a name="updating-profile-data"></a>Aktualisieren von Profildaten

Sobald diese Berechtigungen festgelegt wurden, kann eine Anwendung normale Android Techniken verwenden, für die Interaktion mit Daten für das Benutzerprofil. Z. B. um Anzeigenamen für das Profil zu aktualisieren, rufen [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) mit einer `Uri` abgerufen, die über die [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) Eigenschaft, die wie gezeigt Im folgenden:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Lesen von Profildaten

Ausgeben einer Abfrage, die [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) Lesevorgänge zurück, die Profildaten. Im folgende Code wird z. B. Anzeigenamen für das Benutzerprofil lesen:

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

## <a name="navigating-to-the-user-profile"></a>Navigieren in das Benutzerprofil

Um das Benutzerprofil zu navigieren, erstellen Sie abschließend Priorität mit einer `ActionView` Aktion und ein `ContactsContract.Profile.ContentUri` übergeben Sie sie an der `StartActivity` Methode wie folgt:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Wenn den obigen Code ausgeführt wird, wird das Benutzerprofil angezeigt, wie im folgenden Screenshot gezeigt:

[![Screenshot des Profils, das Benutzerprofil John Doe anzeigen](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Arbeiten mit dem Benutzerprofil ist etwa dem interagieren mit anderen Daten in Android und bietet ein höheres Maß an Gerät Personalisierung.



## <a name="related-links"></a>Verwandte Links

- [ContactsProviderDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
