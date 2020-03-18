---
title: Erstellen eines benutzerdefinierten ContentProvider
description: Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten ContentProvider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter ContentProvider erstellt und anschließend seine Daten genutzt werden.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020523"
---
# <a name="creating-a-custom-contentprovider"></a>Erstellen eines benutzerdefinierten ContentProvider

_Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten ContentProvider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter ContentProvider erstellt und anschließend seine Daten genutzt werden._

## <a name="about-contentproviders"></a>Informationen zu ContentProvidern

Eine Inhaltsanbieterklasse muss von `ContentProvider` erben. Sie sollte aus einem internen Datenspeicher bestehen, der zum Antworten auf Abfragen verwendet wird, und sie sollte URIs und MIME-Typen als Konstanten verfügbar machen, um den verwendenden Code bei gültigen Datenanforderungen zu unterstützen.

### <a name="uri-authority"></a>URI (Authority)

Der Zugriff auf `ContentProviders` erfolgt in Android mithilfe eines URI. Eine Anwendung, die einen `ContentProvider` verfügbar macht, legt die URIs, auf die sie reagiert, in der Datei **AndroidManifest.XML** fest. Wenn die Anwendung installiert ist, werden diese URIs registriert, damit andere Anwendungen darauf zugreifen können.

In Mono für Android sollte die Inhaltsanbieterklasse über ein `[ContentProvider]`-Attribut verfügen, um den URI (oder die URIs) anzugeben, die **AndroidManifest.xml** hinzugefügt werden sollen.

### <a name="mime-type"></a>MIME-Typ

Das typische Format für MIME-Typen besteht aus zwei Teilen. Android-`ContentProviders` verwenden diese beiden Zeichenfolgen in der Regel für den ersten Teil des MIME-Typs:

1. `vnd.android.cursor.item` &ndash; um eine einzelne Zeile darzustellen, verwenden Sie die `ContentResolver.CursorItemBaseType`-Konstante im Code.

1. `vnd.android.cursor.dir` &ndash; um mehrere Zeilen darzustellen, verwenden Sie die `ContentResolver.CursorDirBaseType`-Konstante im Code.

Der zweite Teil des MIME-Typs ist spezifisch für Ihre Anwendung und sollte einen Reverse-DNS-Standard mit einem `vnd.`-Präfix verwenden. Im Beispielcode wird `vnd.com.xamarin.sample.Vegetables` verwendet.

### <a name="data-model-metadata"></a>Datenmodellmetadaten

Verwendende Anwendungen müssen URI-Abfragen erstellen, um auf verschiedene Datentypen zuzugreifen. Der Basis-URI kann erweitert werden, um auf eine bestimmte Datentabelle zu verweisen, und kann auch Parameter zum Filtern der Ergebnisse enthalten. Die Spalten und Klauseln, die mit dem resultierenden Cursor zum Anzeigen von Daten verwendet werden, müssen ebenfalls deklariert werden.

Um sicherzustellen, dass nur gültige URI-Abfragen erstellt werden, ist es üblich, die gültigen Zeichenfolgen als konstante Werte bereitzustellen. Dadurch wird der Zugriff auf den `ContentProvider` vereinfacht, da die Werte durch Codevervollständigung erkennbar sind, und es werden Tippfehler in den Zeichenfolgen verhindert.

Im vorherigen Beispiel hat die `android.provider.ContactsContract`-Klasse die Metadaten für die Contacts-Daten verfügbar gemacht. Für unseren benutzerdefinierten `ContentProvider` werden wir die Konstanten einfach in der Klasse selbst verfügbar machen.

## <a name="implementation"></a>Implementierung

Zum Erstellen und Verwenden eines benutzerdefinierten `ContentProvider` sind drei Schritte erforderlich:

1. **Erstellen einer Datenbankklasse** &ndash; implementieren Sie `SQLiteOpenHelper`.

2. **Erstellen einer `ContentProvider`-Klasse** &ndash; implementieren Sie `ContentProvider` mit einer Instanz der Datenbank, als konstante Werte verfügbar gemachten Metadaten und Methoden für den Zugriff auf die Daten.

3. **Zugreifen auf den `ContentProvider` über seinen URI** &ndash; füllen Sie einen `CursorAdapter` mit dem `ContentProvider`, auf den über den zugehörigen URI zugegriffen wird.

Wie bereits erwähnt, können `ContentProviders` von Anwendungen verwendet werden, die nicht mit der identisch sind, in der sie definiert wurden. In diesem Beispiel werden die Daten in der gleichen Anwendung verwendet, aber denken Sie daran, dass andere Anwendungen auch darauf zugreifen können, solange sie den URI und die Informationen über das Schema kennen (in der Regel als konstante Werte verfügbar).

## <a name="create-a-database"></a>Erstellen einer Datenbank

Die meisten `ContentProvider`-Implementierungen basieren auf einer `SQLite`-Datenbank. Der Beispieldatenbankcode in **SimpleContentProvider/VegetableDatabase.cs** erstellt, wie hier gezeigt, eine sehr einfache, zweispaltige Datenbank:

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

Die Datenbankimplementierung selbst kann ohne besondere Überlegungen mit einem `ContentProvider` verfügbar gemacht werden. Wenn Sie jedoch beabsichtigen, die `ContentProvider's`-Daten an ein `ListView`-Steuerelement zu binden, muss eine eindeutige Ganzzahlenspalte mit dem Namen `_id` Teil des Resultsets sein. Weitere Informationen zur Verwendung des `ListView`-Steuerelements finden Sie im Dokument [ListViews and Adapters](~/android/user-interface/layouts/list-view/index.md) (ListViews und Adapter).

## <a name="create-the-contentprovider"></a>Erstellen des ContentProvider

Im restlichen Teil dieses Abschnitts finden Sie Schritt-für-Schritt-Anleitungen dazu, wie die **SimpleContentProvider/VegetableProvider.cs**-Beispielklasse erstellt wurde.

### <a name="initialize-the-database"></a>Initialisieren der Datenbank

Im ersten Schritt erstellen Sie die Unterklasse `ContentProvider` und fügen die Datenbank hinzu, die sie verwendet.

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

Der Rest des Codes bildet die tatsächliche Implementierung des Inhaltsanbieters, mit der die Daten ermittelt und abgefragt werden können.

## <a name="add-metadata-for-consumers"></a>Hinzufügen von Metadaten für Consumer

Es gibt vier verschiedene Typen von Metadaten, die wir in der `ContentProvider`-Klasse verfügbar machen. Nur „Authority“ ist erforderlich, der Rest wird gemäß der Konvention ausgeführt.

- **Authority** &ndash; das `ContentProvider`-Attribut *muss* der Klasse hinzugefügt werden, damit sie bei Android registriert wird, wenn die Anwendung installiert wird.

- **URI** &ndash; der `CONTENT_URI` wird als Konstante verfügbar gemacht, sodass er im Code einfach zu verwenden ist. Er sollte „Authority“ entsprechen, aber schließen Sie Schema und Basispfad ein.

- **MIME-Typen** &ndash; Listen von Ergebnissen und einzelne Ergebnisse werden als unterschiedliche Inhaltstypen behandelt, daher definieren wir zwei MIME-Typen, um sie darzustellen.

- **InterfaceConsts** &ndash; stellen einen konstanten Wert für jeden Datenspaltennamen bereit, sodass der verarbeitende Code sie problemlos ermitteln und darauf verweisen kann, ohne typografische Fehler zu riskieren.

Dieser Code zeigt, wie die einzelnen Elemente implementiert werden, indem Sie der Datenbank die Definition aus dem vorherigen Schritt hinzufügen:

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

## <a name="implement-the-uri-parsing-helper"></a>Implementieren des URI-Analysehilfsprogramms

Da der verwendende Code mit URIs Anforderungen an einen `ContentProvider` stellt, müssen wir diese Anforderungen analysieren können, um zu bestimmen, welche Daten zurückgegeben werden müssen. Die `UriMatcher`-Klasse kann dazu beitragen, URIs zu analysieren, sobald sie mit den URI-Mustern initialisiert ist, die vom `ContentProvider` unterstützt werden.

Der `UriMatcher` im Beispiel wird mit zwei URIs initialisiert:

1. *„com.xamarin.sample.VegetableProvider/vegetables“* &ndash; Anforderung, um die vollständige Liste der Gemüse zurückzugeben.

2. *„com.xamarin.sample.VegetableProvider/vegetables/\#“* &ndash; wobei \# ein Platzhalter für einen numerischen Parameter (`_id` der Zeile in der Datenbank) ist. Ein Sternchenplatzhalter („\*“) kann auch für einen Textparameter verwendet werden.

Im Code verwenden wir die Konstanten, um auf Metadatenwerte wie AUTHORITY und BASE\_PATH zu verweisen. Die Rückgabecodes werden in Methoden verwendet, die eine URI-Verarbeitung durchführen, um zu bestimmen, welche Daten zurückgegeben werden sollen.

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

Dieser Code ist privat für die `ContentProvider`-Klasse. Weitere Informationen finden Sie in der [Google-Dokumentation zu UriMatcher](xref:Android.Content.UriMatcher).

## <a name="implement-the-querymethod"></a>Implementieren von QueryMethod

Die einfachste `ContentProvider`-Methode, die implementiert werden soll, ist die `Query`-Methode. Die nachfolgende Implementierung verwendet `UriMatcher`, um den `uri`-Parameter zu analysieren und die richtige Datenbankmethode aufzurufen. Wenn `uri` einen ID-Parameter enthält, wird die ganze Zahl analysiert (mit `LastPathSegment`) und in der Datenbankabfrage verwendet.

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

Die `GetType`-Methode muss auch überschrieben werden. Diese Methode kann aufgerufen werden, um den Inhaltstyp zu bestimmen, der für einen bestimmten URI zurückgegeben wird.
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

## <a name="implement-the-other-overrides"></a>Implementieren der sonstigen Überschreibungen

In unserem einfachen Beispiel ist es nicht möglich, Daten zu bearbeiten oder zu löschen, aber da die Insert-, Update- und Delete-Methoden implementiert werden müssen, fügen Sie sie ohne Implementierung hinzu:

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

Dies schließt die grundlegende `ContentProvider`-Implementierung ab. Nachdem die Anwendung installiert wurde, sind die Daten, die sie verfügbar macht, sowohl innerhalb der Anwendung als auch in jeder anderen Anwendung verfügbar, die den URI kennt, um darauf zu verweisen.

## <a name="access-the-contentprovider"></a>Zugreifen auf den ContentProvider

Nachdem der `VegetableProvider` implementiert wurde, erfolgt der Zugriff darauf in gleicher Weise wie auf den Contacts-Anbieter am Anfang dieses Dokuments: Abrufen eines Cursors mit dem angegebenen URI und dann Verwenden eines Adapters für den Zugriff auf die Daten.

## <a name="bind-a-listview-to-a-contentprovider"></a>Binden einer ListView an einen ContentProvider

Um eine `ListView` mit Daten aufzufüllen, verwenden wir den URI, der der ungefilterten Gemüseliste entspricht. Im Code verwenden wir den konstanten Wert `VegetableProvider.CONTENT_URI`, der bekanntermaßen in `com.xamarin.sample.vegetableprovider/vegetables` aufgelöst wird. Unsere `VegetableProvider.Query`-Implementierung gibt einen Cursor zurück, der dann an die `ListView` gebunden werden kann.

Der Code in `SimpleContentProvider/HomeScreen.cs` zeigt, wie einfach es ist, Daten aus einem `ContentProvider` anzuzeigen:

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

Die endgültige Anwendung sieht wie folgt aus:

[![Screenshot der App, die Gemüse, Früchte, Blumenknospen, Hülsenfrüchte, Zwiebeln, Knollen auflistet](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Abrufen eines einzelnen Elements aus einem ContentProvider

Eine verwendende Anwendung möchte möglicherweise auch auf einzelne Datenzeilen zugreifen. Dies kann (z. B.) durch das Erstellen eines anderen URIs erfolgen, der auf eine bestimmte Zeile verweist.

Verwenden Sie `ContentResolver` direkt, um auf ein einzelnes Element zuzugreifen, indem Sie einen URI mit der erforderlichen `Id` erstellen.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Die vollständige Methode sieht folgendermaßen aus:

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

- [SimpleContentProvider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
