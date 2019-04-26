---
title: Verwenden des ContentProvider für Kontakte
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 48bb334e7e400d57e7eddc23b0b4ff183a7eba9b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60955947"
---
# <a name="using-the-contacts-contentprovider"></a>Verwenden des ContentProvider für Kontakte

Code, der verwendet Daten, die verfügbar gemacht werden, indem Sie Zugriff auf eine `ContentProvider` nicht erfordern einen Verweis auf die `ContentProvider` überhaupt Klasse. Stattdessen wird ein Uri verwendet, zum Erstellen eines Cursors für die Daten, die verfügbar gemacht werden, indem die `ContentProvider`. Android, die den Uri verwendet, um das System für die Anwendung zu suchen, die verfügbar macht eine `ContentProvider` mit diesem Bezeichner. Der Uri ist eine Zeichenfolge, in der Regel in einem Reverse-DNS-Format, z. B. `com.android.contacts/data`.

Statt, wodurch Entwickler, die diese Zeichenfolge, die Android Denken Sie daran *Kontakte* -Anbieter stellt die Metadaten in die `android.provider.ContactsContract` Klasse. Diese Klasse wird verwendet, um zu bestimmen, den Uri der `ContentProvider` sowie die Namen der Tabellen und Spalten, die abgefragt werden können.

Einige Datentypen erfordern auch speziellen Berechtigungen zum Zugriff auf. Erfordert die integrierte Contacts-Liste die `android.permission.READ_CONTACTS` -Berechtigung für die **"androidmanifest.xml"** Datei.

Es gibt drei Möglichkeiten, einen Cursor aus dem Uri zu erstellen:

1. **ManagedQuery()** &ndash; der bevorzugte Ansatz in Android 2.3 (API Level 10) und früher eine `ManagedQuery` gibt einen Cursor zurück und Aktualisieren der Daten, und Schließen des Cursors, auch automatisch verwaltet. Diese Methode ist in Android 3.0 (API-Ebene 11) veraltet.

1. **ContentResolver.Query()** &ndash; gibt einen nicht verwaltete Cursor, der es bedeutet, dass es aktualisiert und in Code explizit geschlossen werden muss.

1. **CursorLoader(). LoadInBackground()** &ndash; eingeführt in Android 3.0 (API-Ebene 11), `CursorLoader` ist jetzt die bevorzugte Methode zum Verarbeiten einer `ContentProvider` . `CursorLoader` Abfragen einer `ContentResolver` in einem Hintergrundthread, damit die Benutzeroberfläche nicht blockiert ist.
   Diese Klasse kann in früheren Versionen von Android unter Verwendung der v4-Kompatibilität-Bibliothek zugegriffen werden.


Jede dieser Methoden hat es sich um den gleichen grundlegenden Satz von Eingaben:

-  **URI** &ndash; der vollqualifizierte Name des der `ContentProvider` .
-  **Projektion** &ndash; Spezifikation, welche Spalten für den Cursor auswählen.
-  **Auswahl** &ndash; ähnlich wie bei einer SQL `WHERE` Klausel.
-  **SelectionArgs** &ndash; Parameter, in der Auswahl ersetzt werden soll.
-  **SortOrder** &ndash; Spalten, um nach zu sortieren.



## <a name="creating-inputs-for-a-query"></a>Erstellen von Eingaben für eine Abfrage

Die `ContactsProvider` Beispielcode führt eine sehr einfache Abfrage für Anbieter für integrierte Android-Kontakte. Sie müssen nicht wissen, die tatsächlichen Uri oder Spaltennamen – alle erforderlichen Informationen zum Abfragen von Kontakten `ContentProvider` steht als Konstanten, die verfügbar gemacht werden, indem die `ContactsContract` Klasse.

Unabhängig davon, welche Methode zum Abrufen des Cursors verwendet wird, werden diese Objekte als Parameter verwendet, siehe die *ContactsProvider/ContactsAdapter.cs* Datei:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

In diesem Beispiel die `selection`, `selectionArgs` und `sortOrder` ignoriert werden, indem sie auf `null`.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Erstellen einen Cursor aus einem Uri-Inhaltsanbieter

Nachdem die Parameterobjekte erstellt wurden, können sie in einem der folgenden drei Arten verwendet werden:



### <a name="using-a-managed-query"></a>Verwenden einer verwalteten Abfrage

Anwendungen für Android 2.3 (API Level 10) oder früher verwenden diese Methode:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Dieser Cursor werden von Android verwaltet werden, damit Sie nicht benötigen, um es zu schließen.



### <a name="using-contentresolver"></a>Verwenden von ContentResolver

Zugreifen auf `ContentResolver` direkt zum Abrufen eines Cursors für eine `ContentProvider` kann wie folgt durchgeführt:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Dieser Cursor wird nicht verwaltet, damit sie geschlossen werden muss, wenn nicht mehr benötigt.
Stellen Sie sicher, dass der Code schließt einen Cursor, der geöffnet ist, andernfalls tritt ein Fehler auf.

```csharp
cursor.Close();
```

Sie können alternativ Aufrufen `StartManagingCursor()` und `StopManagingCursor()` "den Cursor verwalten". Verwaltete Cursor automatisch deaktiviert und erneut abgefragt werden, wenn Aktivitäten beendet und neu gestartet werden.



### <a name="using-cursorloader"></a>Verwenden von CursorLoader

Erstellte Anwendungen für Android 3.0 (API-Ebene 11) oder höher sollten diese Methode verwenden:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

Die `CursorLoader` wird sichergestellt, dass alle Cursorvorgänge werden in einem Hintergrundthread ausgeführt, und können auf intelligente Weise erneut einen vorhandenen Cursor für Aktivitätsinstanzen eine Aktivität (z. B. aufgrund einer konfigurationsänderung) anstatt neu gestartet wird, die die Daten erneut zu laden.

Frühere Android-Versionen können Sie auch die `CursorLoader` Klasse, indem die [v4-Unterstützungsbibliotheken](https://developer.android.com/tools/support-library/index.html).



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Zum Anzeigen der Cursor-Daten mit einem benutzerdefinierten Adapter

Zur Anzeige des Bilds wenden Sie sich an verwenden wir einen benutzerdefinierten Adapter, damit wir manuell beheben, können die `PhotoId` Verweis auf ein Image-Dateipfad.

Um Daten mit einem benutzerdefinierten Adapter anzuzeigen, die im Beispiel wird eine `CursorLoader` zum Abrufen aller wenden Sie sich an Daten in einer lokalen Sammlung in der **FillContacts** aus **ContactsProvider/ContactsAdapter.cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Implementieren Sie dann die BaseAdapter Methode, die mit der `contactList` Auflistung. Der Adapter wird implementiert, wie es bei jeder anderen Sammlung wäre &ndash; besteht hier keine besondere Behandlung, da die Daten stammen eine `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

Das Bild wird angezeigt (falls vorhanden) und dem Uri der Bilddatei auf dem Gerät. Die Anwendung sieht folgendermaßen aus:

[![Screenshot der app, die Kontakte in einem ListView angezeigt; ein Bild wird links neben einem Eintrag angezeigt.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Verwenden ein ähnliches Codemuster wie, kann Ihre Anwendung eine Vielzahl von Systemdaten, einschließlich des Benutzers, Fotos, Videos und Musik zugreifen.
Einige Datentypen erfordern besondere Berechtigungen des Projekts angefordert werden **"androidmanifest.xml"**.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Zum Anzeigen der Cursor-Daten mit einem SimpleCursorAdapter

Der Cursor kann auch angezeigt werden, mit einem `SimpleCursorAdapter` (auch wenn nur der Name angezeigt wird, nicht das Foto). Dieser Code zeigt, wie eine `ContentProvider` mit `SimpleCursorAdapter` (dieser Code wird nicht angezeigt, in dem Beispiel):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Finden Sie in der [ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) für Weitere Informationen zur Implementierung `SimpleCursorAdapter`.


## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
