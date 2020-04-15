---
title: Verwenden des ContentProvider für Kontakte
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020546"
---
# <a name="using-the-contacts-contentprovider"></a>Verwenden des ContentProvider für Kontakte

Code, der auf Daten zugreift, die über einen `ContentProvider` verfügbar gemacht werden, benötigt keinerlei Verweis auf die `ContentProvider`-Klasse. Stattdessen wird mit einem URI ein Cursor für die über `ContentProvider` verfügbar gemachten Daten erstellt. Android verwendet den URI, um das System nach einer Anwendung zu durchsuchen, die einen `ContentProvider` mit diesem Bezeichner verfügbar macht. Der URI ist eine Zeichenfolge, typischerweise im Reverse-DNS-Format: `com.android.contacts/data`.

Die Entwickler müssen sich diese Zeichenfolge nicht merken, weil der Android-Anbieter *Contacts* seine Metadaten in der Klasse `android.provider.ContactsContract` verfügbar macht. Mithilfe dieser Klasse können der URI von `ContentProvider` und die Namen der Tabellen und Spalten bestimmt werden, die abgefragt werden können.

Einige Datentypen erfordern für den Zugriff besondere Berechtigungen. Die integrierte Kontaktliste erfordert die `android.permission.READ_CONTACTS`-Berechtigung in der Datei **AndroidManifest.xml**.

Es gibt drei Möglichkeiten, einen Cursor aus dem URI zu erstellen:

1. **ManagedQuery()** &ndash; Der bevorzugte Ansatz in Android 2.3 (API-Ebene 10) und früheren Versionen. Ein `ManagedQuery` gibt einen Cursor zurück und sorgt darüber hinaus für die automatische Aktualisierung der Daten und für das Schließen des Cursors. Diese Methode wurde in Android 3.0 (API-Ebene 11) als veraltet eingestuft.

1. **ContentResolver.Query()** &ndash; Hiermit wird ein nicht verwalteter Cursor zurückgegeben. Dies bedeutet, dass die Aktualisierung und das Schließen des Cursors explizit im Code erfolgen müssen.

1. **CursorLoader().LoadInBackground()** &ndash; Eingeführt in Android 3.0 (API-Ebene 11). `CursorLoader` ist aktuell die bevorzugte Vorgehensweise zur Nutzung von `ContentProvider`. `CursorLoader` ruft einen `ContentResolver` in einem Hintergrundthread ab, damit die Benutzeroberfläche nicht blockiert wird.
   Auf diese Klasse kann in älteren Versionen von Android mit der v4-Kompatibilitätsbibliothek zugegriffen werden.

Jede dieser Methoden umfasst dieselben Basiseingaben:

- **Uri** &ndash; der vollqualifizierte Name des `ContentProvider`
- **Projection** &ndash; gibt an, welche Spalten für den Cursor ausgewählt werden sollen
- **Selection** &ndash; ähnelt einer SQL-`WHERE`-Klausel
- **SelectionArgs** &ndash; Parameter, die in der Auswahl ersetzt werden sollen
- **SortOrder** &ndash; Spalten, nach denen sortiert werden soll

## <a name="creating-inputs-for-a-query"></a>Erstellen von Eingaben für eine Abfrage

Der `ContactsProvider`-Beispielcode führt eine sehr einfache Abfrage des integrierten Android-Anbieters „Contacts“ aus. Sie müssen den tatsächlichen URI oder die Spaltennamen nicht kennen – alle benötigten Informationen zum Abfragen des `ContentProvider` „Contacts“ werden als Konstanten durch die `ContactsContract`-Klasse verfügbar gemacht.

Unabhängig von der gewählten Methode zum Abrufen des Cursors werden dieselben Objekte als Parameter verwendet, wie in der Datei *ContactsProvider/ContactsAdapter.cs* gezeigt:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

Für dieses Beispiel werden `selection`, `selectionArgs` und `sortOrder` ignoriert, indem sie auf `null` festgelegt werden.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Erstellen eines Cursors aus einem Inhaltsanbieter-URI

Sobald die Parameterobjekte erstellt wurden, können sie auf eine der folgenden drei Arten verwendet werden:

### <a name="using-a-managed-query"></a>Verwenden einer verwalteten Abfrage

Anwendungen, die auf Android 2.3 (API-Ebene 10) oder früher abzielen, sollten diese Methode verwenden:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Dieser Cursor wird von Android verwaltet, sodass Sie ihn nicht schließen müssen.

### <a name="using-contentresolver"></a>Verwenden von ContentResolver

Ein direkter Zugriff auf `ContentResolver` zum Abrufen eines Cursors für einen `ContentProvider` kann folgendermaßen erreicht werden:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Dieser Cursor ist nicht verwaltet, deshalb müssen Sie ihn schließen, wenn er nicht länger benötigt wird.
Stellen Sie sicher, dass der Code einen offenen Cursor schließt, da es andernfalls zu einem Fehler kommt.

```csharp
cursor.Close();
```

Alternativ können Sie `StartManagingCursor()` und `StopManagingCursor()` zum „Verwalten“ des Cursors aufrufen. Verwaltete Cursor werden automatisch deaktiviert und erneut abgefragt, wenn Aktivitäten beendet und neu gestartet werden.

### <a name="using-cursorloader"></a>Verwenden von CursorLoader

Anwendungen, die auf Android 3.0 (API-Ebene 11) oder höher abzielen, sollten diese Methode verwenden:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

`CursorLoader` stellt sicher, dass alle Cursorvorgänge in einem Hintergrundthread ausgeführt werden, und kann einen vorhandenen Cursor intelligent über Aktivitätsinstanzen hinweg wiederverwenden, wenn eine Aktivität neu gestartet wird (z. B. aufgrund einer Konfigurationsänderung), statt die Daten erneut zu laden.

Frühere Android-Versionen können durch Verwendung der [v4-Unterstützungsbibliotheken](https://developer.android.com/tools/support-library/index.html) auch die `CursorLoader`-Klasse nutzen.

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Anzeige der Cursordaten mit einem benutzerdefinierten Adapter

Zur Anzeige des Bilds für einen Kontakt werden wir einen benutzerdefinierten Adapter verwenden, sodass wir den `PhotoId`-Verweis manuell in einen Bilddateipfad auflösen können.

Um Daten mit einem benutzerdefinierten Adapter anzuzeigen, verwendet das Beispiel einen `CursorLoader`, um alle Kontaktdaten in eine lokale Sammlung in der Methode **FillContacts** von **ContactsProvider/ContactsAdapter.cs** abzurufen:

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

Anschließend werden die BaseAdapter-Methoden unter Verwendung der `contactList`-Sammlung implementiert. Der Adapter wird genauso wie bei jeder anderen Sammlung implementiert &ndash; es erfolgt hier keine besondere Verarbeitung, weil die Daten aus einem `ContentProvider` stammen:

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

Das Bild wird (sofern vorhanden) unter Verwendung des URI zur Bilddatei auf dem Gerät angezeigt. Die Anwendung sieht wie folgt aus:

[![Screenshot der App mit Anzeige von Kontakten in einer ListView. Das Bild wird jeweils links vom Eintrag angezeigt.](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Unter Verwendung eines ähnlichen Codemusters kann Ihre Anwendung auf eine Vielzahl von Systemdaten zugreifen, darunter z. B. auf Fotos, Videos und Musik des Benutzers.
Für einige Daten müssen in der Datei **AndroidManifest.xml** des Projekts besondere Berechtigungen angefordert werden.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Anzeige der Cursordaten mit einem SimpleCursorAdapter

Der Cursor könnte auch mit einem `SimpleCursorAdapter` angezeigt werden (wenngleich nur der Name angezeigt wird, nicht das Foto). Der folgende Code veranschaulicht, wie ein `ContentProvider` mit `SimpleCursorAdapter` verwendet wird (dieser Code ist nicht im Beispiel enthalten):

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

Weitere Informationen zur Implementierung von `SimpleCursorAdapter` finden Sie im Artikel zu [ListView und Adaptern](~/android/user-interface/layouts/list-view/index.md).

## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
