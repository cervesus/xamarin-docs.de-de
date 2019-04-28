---
title: Benutzerprofil
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/22/2018
ms.openlocfilehash: 0613411e5436a0ea8ed08bf4af52dae84a9a701c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307958"
---
# <a name="user-profile"></a>Benutzerprofil

Android unterstützt Aufzählen von Kontakten mit dem [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) Anbieter seit API-Ebene-5. Beispielsweise Kontakte auflisten ist so einfach wie die Verwendung der [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) -Klasse wie im folgenden Codebeispiel dargestellt:

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

Ab Android 4 (API-Ebene 14), die [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) Klasse steht über den `ContactsContract` Anbieter. Die `ContactsContact.Profile` ermöglicht den Zugriff auf das persönliche Profil für den Besitzer eines Geräts, einschließlich Kontaktdaten wie z. B. Name und Phone-Anzahl der Eigentümer des Geräts.


## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Lesen und Schreiben von Daten, die Anwendungen anfordern müssen die `READ_CONTACTS` und `WRITE_CONTACTS` Berechtigungen bzw.
Darüber hinaus zum Lesen und das Benutzerprofil bearbeiten, Anwendungen müssen anfordern der `READ_PROFILE` und `WRITE_PROFILE` Berechtigungen.


## <a name="updating-profile-data"></a>Aktualisieren von Profildaten

Nachdem Sie diese Berechtigungen festgelegt wurden, kann eine Anwendung Techniken für normale Android verwenden, für die Interaktion mit Daten für das Benutzerprofil. Rufen Sie zum Aktualisieren des Profilnamens der Anzeige beispielsweise [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) mit einer `Uri` abrufen, der über die [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) -Eigenschaft, siehe unten:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Lesen von Profildaten

Ausgeben einer Abfrage, die [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) Lesevorgänge zurück, die Profildaten. Der folgende Code wird z. B. Anzeigename für das Benutzerprofil lesen:

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

## <a name="navigating-to-the-user-profile"></a>Navigieren auf das Benutzerprofil

Navigieren Sie zu das Benutzerprofil, erstellen Sie abschließend einen Intent mit einer `ActionView` Aktion und ein `ContactsContract.Profile.ContentUri` übergeben Sie sie an der `StartActivity` Methode wie folgt:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

Wenn den obigen Code ausgeführt wird, wird das Benutzerprofil angezeigt, wie im folgenden Screenshot dargestellt:

[![Screenshot des Profils, die das Profil der Benutzer John Doe anzeigen](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Arbeiten mit dem Benutzerprofil ähnelt der Interaktion mit anderen Daten in Android, und bietet ein höheres Maß an Personalisierung des Geräts.



## <a name="related-links"></a>Verwandte Links

- [ContactsProviderDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](https://developer.android.com/sdk/android-4.0.html)
