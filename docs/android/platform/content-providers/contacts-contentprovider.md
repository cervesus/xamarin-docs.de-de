---
title: Verwenden die Kontakte ContentProvider
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 754b81cec7f1adbe5c7ff1c820260e162e226b15
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765840"
---
# <a name="using-the-contacts-contentprovider"></a>Verwenden die Kontakte ContentProvider

Code, der von verfügbar gemachten Daten zuzugreifen, verwendet eine `ContentProvider` nicht erfordern einen Verweis auf die `ContentProvider` überhaupt Klasse. Ein Uri stattdessen dient zum Erstellen von eines Cursors über die Daten, die verfügbar gemacht werden, indem Sie die `ContentProvider`. Android verwendet den Uri des Systems für die Anwendung zu suchen, die macht eine `ContentProvider` mit dieser Kennung. Der Uri ist eine Zeichenfolge, in der Regel in einem Reverse-DNS-Format, z. B. `com.android.contacts/data`.

Statt Entwickler, die dieser Zeichenfolge übereinstimmt, das Android merken *Kontakte* Anbieter macht die Metadaten in die `android.provider.ContactsContract` Klasse. Diese Klasse wird verwendet, um den Uri des bestimmen die `ContentProvider` sowie die Namen der Tabellen und Spalten, die abgefragt werden können.

Einige Datentypen erfordern auch spezielle Berechtigung zum Zugriff auf. Die Liste "Kontakte" integrierte erfordert die `android.permission.READ_CONTACTS` -Berechtigung für die **AndroidManifest.xml** Datei.

Es gibt drei Möglichkeiten, einen Cursor aus dem Uri zu erstellen:

1. **ManagedQuery()** &ndash; den optimalen Ansatz in Android 2.3 (API-Ebene 10) und früher eine `ManagedQuery` gibt einen Cursor zurück und verwaltet automatisch auch die Daten zu aktualisieren, und Schließen des Cursors. Diese Methode ist in Android 3.0 (API-Ebene 11) veraltet.

1. **ContentResolver.Query()** &ndash; Returns an unmanaged cursor, which means it must be refreshed and closed explicitly in code.

1. **CursorLoader().LoadInBackground()** &ndash; Introduced in Android 3.0 (API Level 11), `CursorLoader` is now the preferred way to consume a `ContentProvider` . `CursorLoader` Abfragen einer `ContentResolver` in einem Hintergrundthread, damit die Benutzeroberfläche nicht blockiert ist.
   Diese Klasse kann in früheren Versionen von Android mithilfe der v4-Kompatibilität-Bibliothek zugegriffen werden.


Jede dieser Methoden hat den gleichen grundlegenden Satz von Eingaben:

-  **URI** &ndash; der vollständig qualifizierte Name des der `ContentProvider` .
-  **Projektion** &ndash; Spezifikation, welche Spalten für den Cursor auswählen.
-  **Auswahl** &ndash; ähnlich wie eine SQL `WHERE` Klausel.
-  **SelectionArgs** &ndash; Parameter in der Auswahl ersetzt werden sollen.
-  **SortOrder** &ndash; Spalten zu sortieren.



## <a name="creating-inputs-for-a-query"></a>Erstellen von Eingaben für eine Abfrage

Die `ContactsProvider` Beispielcode führt eine sehr einfache Abfrage für Android integrierte Kontakte Anbieter. Sie müssen nicht wissen, die tatsächlichen Uri oder Spaltennamen - alle Informationen erforderlich, um die Kontakte abzufragen `ContentProvider` steht als Konstanten, die verfügbar gemacht werden, indem die `ContactsContract` Klasse.

Unabhängig davon, welche Methode verwendet wird, um den Cursor abzurufen, werden diese Objekte als Parameter verwendet, entsprechend der *ContactsProvider/ContactsAdapter.cs* Datei:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

In diesem Beispiel die `selection`, `selectionArgs` und `sortOrder` ignoriert werden, indem sie auf `null`.



## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Erstellen eines Cursors aus einem Uri-Inhaltsanbieter

Nachdem die Parameterobjekte erstellt wurden, können sie in einem der folgenden drei Arten verwendet werden:



### <a name="using-a-managed-query"></a>Verwenden einer verwalteten Abfrage

Anwendungen für Android 2.3 (API-Ebene 10) oder früher sollten diese Methode verwenden:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Dieser Cursor werden von Android verwaltet werden, damit Sie nicht benötigen, um es zu schließen.



### <a name="using-contentresolver"></a>Verwenden von ContentResolver

Zugreifen auf `ContentResolver` direkt zum Abrufen eines Cursors für eine `ContentProvider` kann wie folgt durchgeführt:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Dieser Cursor wird nicht verwaltet, damit sie geschlossen werden muss, wenn nicht mehr erforderlich.
Stellen Sie sicher, dass der Code schließt einen Cursor, der geöffnet ist, andernfalls tritt ein Fehler auf.

```csharp
cursor.Close();
```

Sie können alternativ Aufrufen `StartManagingCursor()` und `StopManagingCursor()` "den Cursor verwalten". Verwaltete Cursor werden automatisch deaktiviert und erneut abgefragt werden, wenn Aktivitäten beendet und neu gestartet werden.



### <a name="using-cursorloader"></a>Verwenden von CursorLoader

Anwendungen für Android 3.0 (API-Ebene 11) erstellt wurden, oder höher sollten diese Methode verwenden:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

Die `CursorLoader` wird sichergestellt, dass alle Cursorvorgänge, die in einem Hintergrundthread fertig sind, und eine intelligente fehlerwiederherstellung erneut können einen vorhandenen Cursor über Aktivitätsinstanzen, wenn eine Aktivität (z. B. aufgrund einer konfigurationsänderung) statt neu gestartet wird, die die Daten erneut laden.

Android Vorgängerversionen können auch die `CursorLoader` Klasse, indem die [v4-Unterstützungsbibliotheken](http://developer.android.com/tools/support-library/index.html).



## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Zum Anzeigen der Cursor-Daten mit einem benutzerdefinierten Adapter

Zur Anzeige des Bilds wenden wir verwenden einen benutzerdefinierten Adapter, damit wir manuell lösen können die `PhotoId` Verweis auf ein Bilddateipfad.

Zum Anzeigen von Daten mit einem benutzerdefinierten Adapter im Beispiel wird eine `CursorLoader` abzurufenden alle wenden Sie sich an Daten in einer lokalen Sammlung in der **FillContacts** Methode **ContactsProvider/ContactsAdapter.cs**:

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

Implementieren Sie die BaseAdapter-Methoden, die mithilfe der `contactList` Auflistung. Der Adapter implementiert wird, ebenso wie wäre es mit einer anderen Sammlung &ndash; besteht hier keine besondere Behandlung, da die Quelle der Daten aus einer `ContentProvider`:

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

Das Bild angezeigt (falls vorhanden) mit dem Uri zur Bilddatei auf dem Gerät. Die Anwendung sieht wie folgt:

[![Screenshot der app, die Kontakte anzeigen, in einer ListView; ein Bild wird auf der linken Seite des ein Eintrag angezeigt.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Mit einem ähnlichen Codemuster, kann die Anwendung eine Vielzahl von Systemdaten, einschließlich des Benutzers Fotos, Videos und Musik zugreifen.
Einige Datentypen erfordern spezielle Berechtigungen, um die Anforderung des Projekts **AndroidManifest.xml**.



## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Zum Anzeigen der Cursor-Daten mit einem SimpleCursorAdapter

Der Cursor kann auch angezeigt werden, mit einem `SimpleCursorAdapter` (wird zwar nur der Name angezeigt, nicht das Foto). Dieser Code zeigt, wie eine `ContentProvider` mit `SimpleCursorAdapter` (in diesem Code wird im Beispiel nicht angezeigt):

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

Finden Sie in der [Listenansichten und Adapter](~/android/user-interface/layouts/list-view/index.md) für Weitere Informationen zum Implementieren von `SimpleCursorAdapter`.


## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
