---
title: Verwenden des ContentProvider für Kontakte
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020546"
---
# <a name="using-the-contacts-contentprovider"></a>Verwenden des ContentProvider für Kontakte

Code, der Zugriffsdaten verwendet, die von einem `ContentProvider` verfügbar gemacht werden, erfordert keinen Verweis auf die `ContentProvider` Klasse. Stattdessen wird ein URI verwendet, um einen Cursor für die Daten zu erstellen, die vom `ContentProvider`verfügbar gemacht werden. Android verwendet den URI, um das System nach der Anwendung zu durchsuchen, die eine `ContentProvider` mit diesem Bezeichner verfügbar macht. Der URI ist eine Zeichenfolge, in der Regel in einem Reverse-DNS-Format, z. b. `com.android.contacts/data`.

Anstatt den Entwicklern diese Zeichenfolge zu merken, macht der Android *Contacts* -Anbieter seine Metadaten in der `android.provider.ContactsContract`-Klasse verfügbar. Diese Klasse wird verwendet, um den URI der `ContentProvider` sowie die Namen der Tabellen und Spalten zu bestimmen, die abgefragt werden können.

Einige Datentypen benötigen auch spezielle Berechtigungen für den Zugriff auf. Die Liste der integrierten Kontakte erfordert die `android.permission.READ_CONTACTS`-Berechtigung in der Datei " **androidmanifest. XML** ".

Es gibt drei Möglichkeiten, einen Cursor aus dem URI zu erstellen:

1. **Managedquery ()** &ndash; der bevorzugte Ansatz in Android 2,3 (API-Ebene 10) und früher gibt ein `ManagedQuery` einen Cursor zurück, der die Aktualisierung der Daten und das Schließen des Cursors automatisch verwaltet. Diese Methode ist in Android 3,0 (API-Ebene 11) veraltet.

1. Der **contentresolver. Query ()** -&ndash; gibt einen nicht verwalteten Cursor zurück, d. h., er muss explizit im Code aktualisiert und geschlossen werden.

1. **Cursor Loader (). Loadinbackground ()** &ndash; in Android 3,0 eingeführt (API-Ebene 11), `CursorLoader` ist nun die bevorzugte Methode, um eine `ContentProvider` zu nutzen. `CursorLoader` fragt eine `ContentResolver` in einem Hintergrund Thread ab, sodass die Benutzeroberfläche nicht blockiert wird.
   Auf diese Klasse kann in älteren Versionen von Android mithilfe der V4-Kompatibilitäts Bibliothek zugegriffen werden.

Jede dieser Methoden hat denselben grundlegenden Satz von Eingaben:

- **URI** &ndash; den voll qualifizierten Namen des `ContentProvider`.
- **Projektion** &ndash; Spezifikation der Spalten, die für den Cursor ausgewählt werden sollen.
- **Auswahl** &ndash; ähnlich einer SQL `WHERE`-Klausel.
- **Selectionargs** &ndash; Parameter, die in der Auswahl ersetzt werden sollen.
- **Sortor** &ndash; Spalten, nach denen sortiert werden soll.

## <a name="creating-inputs-for-a-query"></a>Erstellen von Eingaben für eine Abfrage

Der `ContactsProvider` Beispielcode führt eine sehr einfache Abfrage für den integrierten Contacts-Anbieter von Android aus. Sie müssen die tatsächlichen URI-oder Spaltennamen nicht kennen. alle Informationen, die erforderlich sind, um die Kontakte abzufragen `ContentProvider` sind als Konstanten verfügbar, die von der `ContactsContract`-Klasse verfügbar gemacht werden.

Unabhängig davon, welche Methode zum Abrufen des Cursors verwendet wird, werden dieselben Objekte als Parameter verwendet, wie in der Datei *contactsprovider/contactadapter. cs* dargestellt:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

In diesem Beispiel werden die `selection`, `selectionArgs` und `sortOrder` ignoriert, indem Sie auf `null`festgelegt werden.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Erstellen eines Cursors aus einem Inhaltsanbieter-URI

Nachdem die Parameter Objekte erstellt wurden, können Sie auf eine der folgenden drei Arten verwendet werden:

### <a name="using-a-managed-query"></a>Verwenden einer verwalteten Abfrage

Anwendungen, die auf Android 2,3 (API-Ebene 10) oder früher abzielen, sollten diese Methode verwenden:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Dieser Cursor wird von Android verwaltet, sodass Sie ihn nicht schließen müssen.

### <a name="using-contentresolver"></a>Verwenden von contentresolver

Der direkte Zugriff auf `ContentResolver`, um einen Cursor für eine `ContentProvider` zu erhalten, kann wie folgt ausgeführt werden:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Dieser Cursor ist nicht verwaltet und muss daher geschlossen werden, wenn er nicht mehr benötigt wird.
Stellen Sie sicher, dass der Code einen geöffneten Cursor schließt. andernfalls tritt ein Fehler auf.

```csharp
cursor.Close();
```

Sie können auch `StartManagingCursor()` aufzurufen und `StopManagingCursor()`, um den Cursor zu verwalten. Verwaltete Cursor werden automatisch deaktiviert und erneut abgefragt, wenn Aktivitäten beendet und neu gestartet werden.

### <a name="using-cursorloader"></a>Verwenden von Cursor Loader

Anwendungen, die für Android 3,0 (API-Ebene 11) oder höher erstellt wurden, sollten diese Methode verwenden:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

Der `CursorLoader` stellt sicher, dass alle Cursor Vorgänge in einem Hintergrund Thread durchgeführt werden, und kann einen vorhandenen Cursor über Aktivitäts Instanzen hinweg Intelligent wieder verwenden, wenn eine Aktivität neu gestartet wird (z. b. aufgrund einer Konfigurationsänderung), anstatt die Daten erneut zu laden.

Frühere Android-Versionen können auch die `CursorLoader`-Klasse verwenden, indem Sie die [V4-Unterstützungs Bibliotheken](https://developer.android.com/tools/support-library/index.html)verwenden.

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Anzeigen der Cursor Daten mit einem benutzerdefinierten Adapter

Zum Anzeigen des Kontakt Bilds verwenden wir einen benutzerdefinierten Adapter, damit wir den `PhotoId` Verweis auf einen Bilddatei Pfad manuell auflösen können.

Zum Anzeigen von Daten mit einem benutzerdefinierten Adapter verwendet das Beispiel eine `CursorLoader`, um alle Kontaktdaten in einer lokalen Sammlung in der **fillcontacts** -Methode von **contactsprovider/contactadapter. cs**abzurufen:

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

Implementieren Sie dann die baseadapter-Methoden mithilfe der `contactList` Auflistung. Der Adapter wird genauso wie bei jeder anderen Sammlung implementiert &ndash; es gibt hier keine besondere Behandlung, da die Daten aus einer `ContentProvider`stammen:

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

Das Bild wird (sofern vorhanden) mit dem URI für die Bilddatei auf dem Gerät angezeigt. Die Anwendung sieht wie folgt aus:

[![Screenshot der APP, die Kontakte in einer ListView anzeigt; Links von einem Eintrag wird ein Bild angezeigt.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Wenn Sie ein ähnliches Code Muster verwenden, kann Ihre Anwendung auf eine Vielzahl von Systemdaten zugreifen, einschließlich der Fotos, Videos und Musik des Benutzers.
Für einige Datentypen sind spezielle Berechtigungen erforderlich, die in der Datei " **androidmanifest. XML**" des Projekts angefordert werden müssen.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Anzeigen der Cursor Daten mit einem simplecursoradapter

Der Cursor kann auch mit einem `SimpleCursorAdapter` angezeigt werden (es wird jedoch nur der Name angezeigt, nicht das Foto). In diesem Code wird veranschaulicht, wie eine `ContentProvider` mit `SimpleCursorAdapter` verwendet wird (dieser Code wird nicht im Beispiel angezeigt):

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

Weitere Informationen zum Implementieren von `SimpleCursorAdapter`finden Sie in den [ListViews und Adaptern](~/android/user-interface/layouts/list-view/index.md) .

## <a name="related-links"></a>Verwandte Links

- [Contacttadapter-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
