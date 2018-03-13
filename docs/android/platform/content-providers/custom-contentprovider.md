---
title: Erstellen eine benutzerdefinierte ContentProvider
description: "Im vorherigen Abschnitt veranschaulicht, wie Daten aus der Implementierung einer integrierten ContentProvider genutzt wird. In diesem Abschnitt wird erläutert, wie eine benutzerdefinierte ContentProvider erstellen und dann seine Daten verwenden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: 9fac4a233cecd9332602047bc83830d145b5fb08
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-custom-contentprovider"></a>Erstellen eine benutzerdefinierte ContentProvider

_Im vorherigen Abschnitt veranschaulicht, wie Daten aus der Implementierung einer integrierten ContentProvider genutzt wird. In diesem Abschnitt wird erläutert, wie eine benutzerdefinierte ContentProvider erstellen und dann seine Daten verwenden._

## <a name="about-contentproviders"></a>Informationen zu ContentProviders

Eine Klasse Inhaltsanbieter erben muss `ContentProvider`. Es sollte ein interner Datenspeicher, der verwendet wird, auf Abfragen reagieren bestehen und er sollte verfügbar machen Uris und MIME-Typen als Konstanten zu verwendenden Code gültige Anforderungen für Daten auszuführen.

### <a name="uri-authority"></a>URI (Authority)

`ContentProviders` wird im Android mithilfe eines URIs aus zugegriffen werden. Eine Anwendung, die macht eine `ContentProvider` legt die Uris, die in Reaktion auf ihre **AndroidManifest.xml** Datei. Wenn die Anwendung installiert ist, diese Uris werden registriert, damit andere Anwendungen darauf zugreifen können.

Mono für Android, die Inhalt Anbieterklasse darf haben eine `[ContentProvider]` Attribut, um den Uri (oder Uris) anzugeben, die hinzugefügt werden sollen, um **AndroidManifest.xml**.


### <a name="mime-type"></a>MIME-Typ

Das typische Format für MIME-Typen besteht aus zwei Teilen. Android `ContentProviders` verwenden Sie diese zwei Zeichenfolgen häufig für den ersten Teil der MIME-Typ:

1. `vnd.android.cursor.item` &ndash; Um eine einzelne Zeile darzustellen, verwenden Sie die `ContentResolver.CursorItemBaseType` Konstanten im Code.

1. `vnd.android.cursor.dir` &ndash; Verwenden Sie für mehrere Zeilen der `ContentResolver.CursorDirBaseType` Konstanten im Code.

Der zweite Teil der MIME-Typ bezieht sich auf die Anwendung, und ein Reverse-DNS-Standard mit die zu verwendende eine `vnd.` Präfix. Im Beispielcode wird `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Datenmodellmetadaten

Fungierende Anwendungen müssen Uri-Abfragen für den Zugriff auf verschiedene Arten von Daten zu erstellen. Die base-Uri erweitert werden kann, um auf eine bestimmte Tabelle mit Daten zu verweisen und möglicherweise auch Parameter enthalten, um die Ergebnisse zu filtern. Die Spalten und Klauseln, die mit dem Cursor verwendet, um Daten anzuzeigen, müssen auch deklariert werden.

Um sicherzustellen, dass nur gültige Uri-Abfragen erstellt werden, ist es üblich, die gültigen Zeichenfolgen als Konstante Werte bereitzustellen. Dies erleichtert es für den Zugriff auf die `ContentProvider` da es die Werte über codevervollständigung erkennbar macht, wird verhindert, dass die Zeichenfolgen auf Tippfehler.

Im vorherigen Beispiel die `android.provider.ContactsContract` Klasse verfügbar gemacht, die Metadaten für die Kontakte-Daten. Für unsere benutzerdefinierten `ContentProvider` wird nur die Konstanten für die Klasse selbst zur Verfügung.


## <a name="implementation"></a>Implementierung

Es gibt drei Schritte zum Erstellen und nutzen eine benutzerdefinierte `ContentProvider`:

1. **Erstellen Sie eine Datenbankklasse** &ndash; implementieren `SQLiteOpenHelper`.

2. **Erstellen einer `ContentProvider` Klasse** &ndash; implementieren `ContentProvider` mit einer Instanz des Datenbankmoduls, Metadaten verfügbar gemacht werden, als Konstante Werte und Methoden für den Datenzugriff.

3. **Zugriff der `ContentProvider` über seine Uri** &ndash; Auffüllen einer `CursorAdapter` mithilfe der `ContentProvider`, über die Uri.

Wie zuvor bereits erläutert, `ContentProviders` genutzt werden können, von Anwendungen als der, in dem sie definiert sind. In diesem Beispiel die Daten in der gleichen Anwendung genutzt werden, aber beachten Sie dabei, die andere Anwendungen auch darauf zugreifen können so lange sie wissen, der Uri und die Informationen zum Schema (die in der Regel als Konstante Werte zugänglich ist).


## <a name="create-a-database"></a>Erstellen Sie eine Datenbank

Die meisten `ContentProvider` Implementierungen basieren auf einer `SQLite` Datenbank. Der Beispielcode für die Datenbank in **SimpleContentProvider/VegetableDatabase.cs** eine sehr einfache Datenbank für die zwei Spalten erstellt, wie dargestellt:

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

Die Datenbank selbst-serverimplementierung muss keine Besonderheiten mit verfügbar gemacht werden eine `ContentProvider`, jedoch wenn Sie beabsichtigen, binden die `ContentProvider's` Daten an eine `ListView` Steuerelement wird dann eine eindeutige ganzzahlige Spalte mit dem Namen `_id` muss Teil der Resultset. Finden Sie unter der [Listenansichten und Adapter](~/android/user-interface/layouts/list-view/index.md) Dokument detaillierte Informationen zur Verwendung der `ListView` Steuerelement.


## <a name="create-the-contentprovider"></a>Erstellen Sie die ContentProvider

Im restlichen Teil dieses Abschnitts enthält schrittweise Anweisungen wie die **SimpleContentProvider/VegetableProvider.cs** Beispielklasse erstellt wurde.


### <a name="initialize-the-database"></a>Initialisieren Sie die Datenbank

Der erste Schritt besteht darin Unterklasse `ContentProvider` und fügen Sie die Datenbank, in denen verwendet wird.

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

Der Rest des Codes wird die tatsächliche Inhaltsanbieter Implementierung bilden, die die Daten ermittelt und abgefragt werden können.



## <a name="add-metadata-for-consumers"></a>Hinzufügen von Metadaten für Consumer

Es gibt vier verschiedene Typen von Metadaten, die wir auf verfügbar machen möchten die `ContentProvider` Klasse. Nur die Berechtigung erforderlich ist, werden die restlichen gemäß der Konvention ausgeführt.

- **Autorität für die** &ndash; der `ContentProvider` Attribut *müssen* der Klasse hinzugefügt werden, damit sie mit der Android registriert ist, wenn die Anwendung installiert wird.

- **URI** &ndash; der `CONTENT_URI` als Konstante verfügbar gemacht wird, damit er im Code einfach zu verwenden ist. Es sollte die Autorität entsprechen, aber enthalten das Schema und den Basispfad.

- **MIME-Typen** &ndash; Listen der Ergebnisse und die einzelnen Ergebnisse werden als verschiedene Inhaltstypen, behandelt, damit wir zwei MIME-Typen zur Darstellung sie definieren.

- **InterfaceConsts** &ndash; einen konstanten Wert für jeden Namen der Datenspalte, bieten, sodass problemlos ermitteln und auf sie verweisen, ohne Gefahr eines Datenbankausfalls Rechtschreibfehler kann Code nutzen.

Dieser Code zeigt, wie jedes dieser Elemente implementiert wird, die Datenbankdefinition aus dem vorherigen Schritt hinzufügen:

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


## <a name="implement-the-uri-parsing-helper"></a>Implementieren Sie den URI Helper analysieren

Da Uris verwendenden Code verwendet werden, um Anfragen von stellen eine `ContentProvider`, müssen wir diese Anforderungen bestimmen, welche Daten zurückzugebenden analysieren können. Die `UriMatcher` Klasse kann dazu beitragen, um Uris zu analysieren, nachdem er mit initialisiert wurde der Uri-Muster, die die `ContentProvider` unterstützt.

Die `UriMatcher` im Beispiel wird mit zwei Uris initialisiert werden:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). Ein Sternchen-Platzhalter ("\*") kann auch verwendet werden, um mit einem Textparameter übereinstimmen.

In den Code verwenden wir die Konstanten zum Verweisen auf Metadatenwerten, z. B. die Autorität und BASE\_Pfad. Die Rückgabecodes werden in Methoden verwendet werden, die Uri analysieren, um zu bestimmen, welche Daten zurückgegeben.

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

Dieser Code ist für alle privat der `ContentProvider` Klasse. Verweisen auf [Googles UriMatcher Dokumentation](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) für Weitere Informationen zu erhalten.


## <a name="implement-the-querymethod"></a>Implementieren der QueryMethod

Die einfachste `ContentProvider` Methode für die Implementierung ist die `Query` Methode. Die Implementierung unter Verwendung der `UriMatcher` beim Analysieren der `uri` Parameter, und rufen Sie die richtige Datenbank-Methode. Wenn die `uri` einen ID-Parameter enthält, und klicken Sie dann, die ganze Zahl analysiert werden (mit `LastPathSegment`) und in der Datenbankabfrage verwendet.

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

Die `GetType` Methode muss auch überschrieben werden. Diese Methode kann aufgerufen werden, um den Inhaltstyp zu bestimmen, der für einen bestimmten Uri zurückgegeben werden.
Dies kann der konsumierende Anwendung wie Daten behandelt werden soll.

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


## <a name="implement-the-other-overrides"></a>Implementieren Sie die anderen Außerkraftsetzungen

Einfache Beispiel lässt keine Bearbeitung oder Löschung von Daten, aber die INSERT-, Update- und Delete-Methoden implementiert werden müssen sie daher ohne Implementierung hinzufügen:

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

Somit ist die grundlegende `ContentProvider` Implementierung. Nach der Installation der Anwendung werden die macht Daten verfügbar werden sowohl in der Anwendung, sondern auch für eine andere Anwendung, die den Uri, darauf zu verweisen bekannt.


## <a name="access-the-contentprovider"></a>Zugriff auf die ContentProvider

Einmal die `VegetableProvider` wurde implementiert, Zugriff auf sie erfolgt die gleiche Weise wie die Kontakte-Anbieter am Anfang dieses Dokuments: Abrufen eines Cursors, der mit dem angegebenen Uri, und klicken Sie dann einen Adapter für den Datenzugriff verwenden.


## <a name="bind-a-listview-to-a-contentprovider"></a>Binden Sie eine Listenansicht an eine ContentProvider

Zum Auffüllen einer `ListView` mit Daten, die wir verwenden des Uri, der die ungefilterte Liste von Gemüse entspricht. In den Code verwenden wir den konstanten Wert `VegetableProvider.CONTENT_URI`, das wir wissen löst in `com.xamarin.sample.vegetableprovider/vegetables`. Unsere `VegetableProvider.Query` Implementierung wird einen Cursor, der dann an gebunden werden kann Zurückgeben der `ListView`.

Der Code in `SimpleContentProvider/HomeScreen.cs` wird gezeigt, wie einfach es ist zum Anzeigen von Daten aus einer `ContentProvider`:

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

Die resultierende Anwendung sieht wie folgt:

[![Screenshot der app Gemüse, Früchte, Blüten sowie deren Knospen, Körnerleguminosen, Glühbirnen, Knollen auflisten](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Ein einzelnes Element aus einem ContentProvider abrufen

Eine verbrauchende Anwendung sollten sich auch auf einzelne Zeilen mit Daten, die ausgeführt werden können, durch das Erstellen eines anderen URIs, der auf eine bestimmte Zeile (z. B.) verweist.

Verwendung `ContentResolver` direkt an ein einzelnes Element zuzugreifen, durch das Einrichten von eines Uri mit den erforderlichen `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

Die vollständige Methode sieht wie folgt:

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

- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
