---
title: Erstellen eines benutzerdefinierten ContentProvider
description: Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten Contentprovider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter Contentprovider erstellt und anschließend seine Daten genutzt werden.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: e16aa1b96749047554b4f8e6887791d8ed4ff63b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643691"
---
# <a name="creating-a-custom-contentprovider"></a>Erstellen eines benutzerdefinierten ContentProvider

_Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten Contentprovider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter Contentprovider erstellt und anschließend seine Daten genutzt werden._

## <a name="about-contentproviders"></a>Informationen zu contentproviders

Eine Inhaltsanbieter Klasse muss von `ContentProvider`erben. Er sollte aus einem internen Datenspeicher bestehen, der zum Antworten auf Abfragen verwendet wird, und er sollte URIs und MIME-Typen als Konstanten verfügbar machen, um die Verwendung von Code zu unterstützen, die gültige Datenanforderungen verarbeiten.

### <a name="uri-authority"></a>URI (Authority)

`ContentProviders`der Zugriff erfolgt in Android mithilfe eines URI. Eine Anwendung, die einen `ContentProvider` verfügbar macht, legt die URIs fest, auf die in der Datei " **androidmanifest. XML** " reagiert wird. Wenn die Anwendung installiert ist, werden diese URIs registriert, damit andere Anwendungen darauf zugreifen können.

In Mono für Android sollte die Content Provider-Klasse über ein `[ContentProvider]` -Attribut verfügen, um den URI (oder die URIs) anzugeben, der " **androidmanifest. XML**" hinzugefügt werden soll.


### <a name="mime-type"></a>MIME-Typ

Das typische Format für MIME-Typen besteht aus zwei Teilen. Android `ContentProviders` verwendet diese beiden Zeichen folgen häufig für den ersten Teil des MIME-Typs:

1. `vnd.android.cursor.item`Verwenden Sie die `ContentResolver.CursorItemBaseType` -Konstante im Code, um eine einzelne Zeile darzustellen. &ndash;

1. `vnd.android.cursor.dir`Verwenden Sie für mehrere Zeilen die `ContentResolver.CursorDirBaseType` Konstante im Code. &ndash;

Der zweite Teil des MIME-Typs ist spezifisch für Ihre Anwendung, und es sollte ein Reverse-DNS-Standard mit `vnd.` einem Präfix verwendet werden. Der Beispielcode verwendet `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Metadaten des Datenmodells

Die Verwendung von Anwendungen muss URI-Abfragen erstellen, um auf verschiedene Datentypen zuzugreifen. Der Basis-URI kann erweitert werden, um auf eine bestimmte Datentabelle zu verweisen, und kann auch Parameter zum Filtern der Ergebnisse enthalten. Die Spalten und Klauseln, die mit dem resultierenden Cursor zum Anzeigen von Daten verwendet werden, müssen ebenfalls deklariert werden.

Um sicherzustellen, dass nur gültige URI-Abfragen erstellt werden, ist es üblich, die gültigen Zeichen folgen als Konstante Werte bereitzustellen. Dies vereinfacht den Zugriff auf das `ContentProvider` , da die Werte durch Codevervollständigung auffähiert werden können, und es wird verhindert, dass Tippfehler in den Zeichen folgen angezeigt werden.

Im vorherigen Beispiel hat die `android.provider.ContactsContract` -Klasse die Metadaten für die Contacts-Daten verfügbar gemacht. Für unsere Benutzer `ContentProvider` definierten werden nur die Konstanten für die Klasse selbst verfügbar gemacht.


## <a name="implementation"></a>Implementierung

Zum Erstellen und Verwenden eines benutzerdefinierten `ContentProvider`sind drei Schritte erforderlich:

1. **Erstellen einer Datenbankklasse** &ndash; Implementieren`SQLiteOpenHelper`Sie.

2. **Erstellen Sie `ContentProvider` eine** &ndash; -`ContentProvider` Klasse mit einer Instanz der-Datenbank, Metadaten, die als Konstante Werte verfügbar gemacht werden, und Methoden für den Zugriff auf die Daten.

3. **Greifen Sie über den zugehörigen URI auf zu, indem Sie einen mit Auffüllen, auf den über seinen URI zugegriffen wird. `ContentProvider`**  &ndash; `CursorAdapter` `ContentProvider`

Wie bereits erwähnt, `ContentProviders` kann von anderen Anwendungen verwendet werden, als wenn Sie definiert sind. In diesem Beispiel werden die Daten in der gleichen Anwendung verwendet, denken Sie jedoch daran, dass andere Anwendungen auch darauf zugreifen können, solange Sie den URI und die Informationen über das Schema kennen (in der Regel als Konstante Werte verfügbar).


## <a name="create-a-database"></a>Erstellen einer Datenbank

Die `ContentProvider` meisten Implementierungen basieren auf einer `SQLite` -Datenbank. Der Beispiel Datenbankcode in **simplecontentprovider/vegettabledatabase. cs** erstellt eine sehr einfache Datenbank mit zwei Spalten, wie hier gezeigt:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

Bei der Datenbankimplementierung selbst sind keine besonderen Überlegungen erforderlich, die mit `ContentProvider`einem verfügbar gemacht werden. Wenn Sie jedoch `ContentProvider's` beabsichtigen, die `ListView` Daten an ein Steuerelement zu binden `_id` , muss eine eindeutige ganzzahlige Spalte mit dem Namen Teil der Resultset. Weitere Informationen zur Verwendung des `ListView` -Steuer Elements finden Sie im Dokument " [ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) ".


## <a name="create-the-contentprovider"></a>Erstellen des Contentprovider

Im restlichen Teil dieses Abschnitts finden Sie Schritt-für-Schritt-Anleitungen dazu, wie die Beispiel Klasse **simplecontentprovider/vegettableprovider. cs** erstellt wurde.


### <a name="initialize-the-database"></a>Initialisieren der Datenbank

Der erste Schritt besteht in der unter `ContentProvider` Klasse und dem Hinzufügen der zu verwendenden Datenbank.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

Der Rest des Codes bildet die tatsächliche Implementierung des Inhalts Anbieters, mit der die Daten ermittelt und abgefragt werden können.



## <a name="add-metadata-for-consumers"></a>Hinzufügen von Metadaten für Consumer

Es gibt vier verschiedene Arten von Metadaten, die wir für die `ContentProvider` Klasse verfügbar machen. Nur die Autorität ist erforderlich. der Rest wird gemäß der Konvention ausgeführt.

- **Autorität** Das-Attribut muss der-Klasse hinzugefügt werden, damit es bei Android registriert wird, wenn die Anwendung installiert wird. &ndash; `ContentProvider`

- **URI** &ndash; Die`CONTENT_URI` wird als Konstante verfügbar gemacht, sodass Sie einfach im Code verwendet werden kann. Er sollte der Zertifizierungsstelle entsprechen, aber das Schema und den Basispfad einschließen.

- **MIME-Typen** &ndash; Listen der Ergebnisse und einzelne Ergebnisse werden als unterschiedliche Inhaltstypen behandelt, daher definieren wir zwei MIME-Typen, um Sie darzustellen.

- **Interfakeconsts** &ndash; Geben Sie einen konstanten Wert für jeden Datenspalten Namen an, damit der verarbeitende Code Sie problemlos ermitteln und darauf verweisen kann, ohne typografische Fehler zu riskieren.

Dieser Code zeigt, wie die einzelnen Elemente implementiert werden, indem Sie der Daten Bank Definition aus dem vorherigen Schritt hinzugefügt werden:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```


## <a name="implement-the-uri-parsing-helper"></a>Implementieren des URI--Element-Hilfsprogramms

Da der Verwendungs Code URIs verwendet `ContentProvider`, um Anforderungen an zu senden, müssen diese Anforderungen analysiert werden können, um zu bestimmen, welche Daten zurückgegeben werden müssen. Die `UriMatcher` -Klasse kann bei der Analyse von URIs helfen, sobald Sie mit den URI-Mustern initialisiert wurde `ContentProvider` , die von unterstützt werden.

Der `UriMatcher` im Beispiel wird mit zwei URIs initialisiert:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). Ein Sternchen-Platzhalter (\*"") kann auch verwendet werden, um einen Text Parameter abzugleichen.

Im Code verwenden wir die Konstanten, um auf Metadatenwerte wie die Autorität und den\_Basispfad zu verweisen. Die Rückgabecodes werden in Methoden verwendet, die eine URI-Verarbeitung durchführen, um zu bestimmen, welche Daten zurückgegeben werden sollen.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Dieser Code ist für die `ContentProvider` -Klasse privat. Weitere Informationen finden Sie in [der urimatcher-Dokumentation von Google](xref:Android.Content.UriMatcher) .


## <a name="implement-the-querymethod"></a>Implementieren der querymethod

Die einfachste `ContentProvider` Methode, die implementiert werden `Query` soll, ist die-Methode. Die nachfolgende Implementierung verwendet `UriMatcher` das, um den `uri` -Parameter zu analysieren und die richtige Daten Bank Methode aufzurufen. Wenn die `uri` einen ID-Parameter enthält, wird die ganze Zahl analysiert (mithilfe `LastPathSegment`von) und in der Datenbankabfrage verwendet.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

Die `GetType` -Methode muss ebenfalls überschrieben werden. Diese Methode kann aufgerufen werden, um den Inhaltstyp zu bestimmen, der für einen angegebenen URI zurückgegeben wird.
Dies könnte der verarbeitenden Anwendung mitteilen, wie diese Daten verarbeitet werden sollen.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```


## <a name="implement-the-other-overrides"></a>Implementieren Sie die anderen außer Kraft setzungen.

In unserem einfachen Beispiel ist es nicht möglich, Daten zu bearbeiten oder zu löschen, aber die INSERT-, Update-und Delete-Methoden müssen implementiert werden, um Sie ohne Implementierung hinzuzufügen:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Dies schließt die Grund `ContentProvider` Legende-Implementierung ab. Nachdem die Anwendung installiert wurde, werden die Daten, die Sie verfügbar macht, sowohl innerhalb der Anwendung als auch in jeder anderen Anwendung verfügbar sein, die den URI kennt, um darauf zu verweisen.


## <a name="access-the-contentprovider"></a>Zugreifen auf den Contentprovider

Nachdem der `VegetableProvider` implementiert wurde, erfolgt der Zugriff auf die gleiche Weise wie der Kontakt Anbieter am Anfang dieses Dokuments: Rufen Sie einen Cursor mit dem angegebenen URI ab, und verwenden Sie dann einen Adapter für den Zugriff auf die Daten.


## <a name="bind-a-listview-to-a-contentprovider"></a>Binden einer ListView an einen Contentprovider

Zum Auffüllen eines `ListView` mit Daten verwenden wir den URI, der der ungefilterten Liste von Gemüse entspricht. Im Code wird der Konstante Wert `VegetableProvider.CONTENT_URI`verwendet, der in `com.xamarin.sample.vegetableprovider/vegetables`aufgelöst wird. Unsere `VegetableProvider.Query` Implementierung gibt einen Cursor zurück, der dann an den `ListView`gebunden werden kann.

Der Code in `SimpleContentProvider/HomeScreen.cs` zeigt, wie einfach es ist, Daten aus einer `ContentProvider`anzuzeigen:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

Die resultierende Anwendung sieht wie folgt aus:

[![Screenshot der APP, die Gemüse, Früchte, Blumen Knospen, legumes, Glühbirnen, tubers auflistet](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Abrufen eines einzelnen Elements aus einem Contentprovider

Eine verarbeitende Anwendung möchte möglicherweise auch auf einzelne Daten Zeilen zugreifen. Dies kann durch das Erstellen eines anderen URIs erfolgen, der auf eine bestimmte Zeile verweist (z. b.).

Verwenden `ContentResolver` Sie direkt, um auf ein einzelnes Element zuzugreifen, indem Sie einen URI mit `Id`dem erforderlichen aufbauen.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Die Complete-Methode sieht wie folgt aus:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```


## <a name="related-links"></a>Verwandte Links

- [Simplecontentprovider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
