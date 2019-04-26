---
title: Erstellen eines benutzerdefinierten ContentProvider
description: Im vorherigen Abschnitt wurde veranschaulicht, wie Daten aus einer integrierten ContentProvider Implementierung nutzen. In diesem Abschnitt wird erläutert, wie zum Erstellen eines benutzerdefinierten ContentProvider, und klicken Sie dann die Daten nutzen.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: da8aacac1f282fefb6b8d0e84cae168cf3a7148b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953362"
---
# <a name="creating-a-custom-contentprovider"></a>Erstellen eines benutzerdefinierten ContentProvider

_Im vorherigen Abschnitt wurde veranschaulicht, wie Daten aus einer integrierten ContentProvider Implementierung nutzen. In diesem Abschnitt wird erläutert, wie zum Erstellen eines benutzerdefinierten ContentProvider, und klicken Sie dann die Daten nutzen._

## <a name="about-contentproviders"></a>Informationen zu ContentProviders

Eine Content Provider-Klasse erben muss `ContentProvider`. Es sollte ein interner Datenspeicher, der verwendet wird, auf Abfragen Antworten bestehen, und es sollten Uris und MIME-Typen verfügbar, wie Konstanten, um zu verwendenden Code gültige Daten anfordern.

### <a name="uri-authority"></a>URI (Autorität)

`ContentProviders` wird unter Android mithilfe eines Uri zugegriffen werden. Eine Anwendung, die verfügbar macht eine `ContentProvider` legt die Uris, die es auf die in reagiert der **"androidmanifest.xml"** Datei. Wenn die Anwendung installiert ist, diese Uris werden registriert, damit andere Anwendungen darauf zugreifen können.

In Mono für Android, die Content Provider-Klasse müssen einen `[ContentProvider]` Attribut, um den Uri (oder die Uris) anzugeben, die hinzugefügt werden sollen, um **"androidmanifest.xml"**.


### <a name="mime-type"></a>MIME-Typ

Das typische Format für die MIME-Typen besteht aus zwei Teilen. Android `ContentProviders` verwenden Sie diese beiden Zeichenfolgen häufig für den ersten Teil der MIME-Typ:

1. `vnd.android.cursor.item` &ndash; Um eine einzelne Zeile darzustellen, verwenden die `ContentResolver.CursorItemBaseType` Konstanten im Code.

1. `vnd.android.cursor.dir` &ndash; Verwenden Sie für mehrere Zeilen der `ContentResolver.CursorDirBaseType` Konstanten im Code.

Der zweite Teil der MIME-Typ bezieht sich auf Ihre Anwendung, und verwenden, sollte ein Reverse-DNS-Standard mit einer `vnd.` Präfix. Der Beispielcode verwendet `vnd.com.xamarin.sample.Vegetables`.


### <a name="data-model-metadata"></a>Datenmodellmetadaten

Verbrauchende Anwendungen müssen zum Erstellen von Uri-Abfragen für den Zugriff auf verschiedene Arten von Daten. Der base-Uri zum Verweisen auf eine bestimmte Tabelle der Daten erweitert werden kann und kann auch Parameter zum Filtern der Ergebnisse enthalten. Die Spalten und Klauseln, die mit dem Cursor verwendet, um Daten anzuzeigen, müssen auch deklariert werden.

Um sicherzustellen, dass nur gültige Uri-Abfragen erstellt werden, ist es üblich, die gültige Zeichenfolgen als Konstante Werte bereitstellen. Dies erleichtert es für den Zugriff auf die `ContentProvider` da die Werte über codevervollständigung ermittelt wird und verhindert, Tippfehler in den Zeichenfolgen dass.

Im vorherigen Beispiel die `android.provider.ContactsContract` Klasse verfügbar gemacht, die Metadaten für die Kontakte-Daten. Für unsere benutzerdefinierte `ContentProvider` wird nur die Konstanten für die Klasse selbst verfügbar.


## <a name="implementation"></a>Implementierung

Die folgenden drei Schritte zum Erstellen und nutzen eine benutzerdefinierte `ContentProvider`:

1. **Erstellen Sie eine Datenbankklasse** &ndash; implementieren `SQLiteOpenHelper`.

2. **Erstellen Sie eine `ContentProvider` Klasse** &ndash; implementieren `ContentProvider` mit einer Instanz der Datenbank an, die für Metadaten als Konstante Werte und Methoden für den Datenzugriff verfügbar gemacht.

3. **Zugriff die `ContentProvider` über seinen Uri** &ndash; Auffüllen einer `CursorAdapter` mithilfe der `ContentProvider`, der Zugriff erfolgt über einen Uri.

Wie bereits erwähnt, `ContentProviders` genutzt werden können, von Anwendungen, in dem sie definiert sind. In diesem Beispiel die Daten in der gleichen Anwendung verwendet werden, aber denken Sie daran, die andere Anwendungen auch darauf zugreifen können, solange sie wissen, den Uri und die Informationen zum Schema (die in der Regel als Konstante Werte bereitgestellt wird).


## <a name="create-a-database"></a>Erstellen einer Datenbank

Die meisten `ContentProvider` Implementierungen werden auf Grundlage einer `SQLite` Datenbank. Der Beispielcode für die Datenbank in **SimpleContentProvider/VegetableDatabase.cs** eine sehr einfache Datenbank zwei Spalten erstellt, wie dargestellt:

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

Die datenbankimplementierung selbst benötigt keine speziellen Überlegungen mit verfügbar gemacht werden eine `ContentProvider`jedoch wenn Sie beabsichtigen, binden die `ContentProvider's` Daten an eine `ListView` Steuerung wird dann eine eindeutige ganzzahlige Spalte mit dem Namen `_id` muss Mitglied der das Resultset. Finden Sie unter den [ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) Dokument Weitere Informationen zur Verwendung der `ListView` Steuerelement.


## <a name="create-the-contentprovider"></a>Erstellen Sie die ContentProvider

Im restlichen Teil dieses Abschnitts bietet eine ausführliche Anleitung, wie sich die **SimpleContentProvider/VegetableProvider.cs** Beispielklasse erstellt wurde.


### <a name="initialize-the-database"></a>Initialisieren der Datenbank

Der erste Schritt ist das Erstellen einer Unterklasse `ContentProvider` und fügen Sie die Datenbank, in denen verwendet wird.

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

Der restliche Code bildet die tatsächlichen Inhaltsanbieter-Implementierung, die die Daten ermittelt und abgefragt werden können.



## <a name="add-metadata-for-consumers"></a>Hinzufügen von Metadaten für Consumer

Es gibt vier verschiedene Typen von Metadaten, die wir möchten über zugänglich machen die `ContentProvider` Klasse. Nur die Autorität für die erforderlich ist, werden die übrigen gemäß der Konvention ausgeführt.

- **Autorität für die** &ndash; der `ContentProvider` Attribut *müssen* der Klasse hinzugefügt werden, damit er mit dem Android registriert ist, wenn die Anwendung installiert ist.

- **URI** &ndash; der `CONTENT_URI` als Konstante verfügbar gemacht wird, damit es im Code einfach zu verwenden ist. Die Autorität entsprechen sollte, sondern enthalten die Schemas und der Basispfad.

- **MIME-Typen** &ndash; Listen der Ergebnisse und die einzelnen Ergebnisse werden als unterschiedliche Inhaltstypen behandelt, sodass wir zwei MIME-Typen zur Darstellung von ihnen definieren.

- **InterfaceConsts** &ndash; bieten Sie einen konstanten Wert für jeden Namen der Datenspalte, sodass leicht ermitteln und auf sie verweisen, ohne zu riskieren Rechtschreibfehler kann Code verwenden.

Dieser Code zeigt, wie jedes dieser Elemente implementiert wird, die Datenbankdefinition aus dem vorherigen Schritt hinzugefügt:

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


## <a name="implement-the-uri-parsing-helper"></a>Implementieren Sie die URI-Analyse Hilfsprogramm

Da Uris Nutzen von Code verwendet werden, um die Anforderungen von einem `ContentProvider`, müssen wir analysieren diese Anforderungen, um zu bestimmen, welche Daten zurückgegeben werden. Die `UriMatcher` Klasse kann dazu beitragen, um Uris zu analysieren, sobald er mit initialisiert wurde der Uri-Muster, die die `ContentProvider` unterstützt.

Die `UriMatcher` im Beispiel wird mit zwei Uris initialisiert werden:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where the \# is a placeholder for a numeric parameter (the `_id` of the row in the database). Ein Sternchen-Platzhalter ("\*") kann auch verwendet werden, um mit einem Textparameter übereinstimmen.

Im Code verwenden wir die Konstanten zum Verweisen auf die Metadatenwerte wie die Autorität und dem BASE\_Pfad. Die Rückgabecodes werden in Methoden verwendet werden, die die Uri-Analyse, um zu bestimmen, welche Daten zurückgegeben.

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

Dieser Code ist für alle privat die `ContentProvider` Klasse. Finden Sie unter [Google UriMatcher Dokumentation](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/) für Weitere Informationen zu erhalten.


## <a name="implement-the-querymethod"></a>Implementieren der QueryMethod

Die einfachste `ContentProvider` zu implementierende Methode ist die `Query` Methode. Die Implementierung unten verwendet die `UriMatcher` beim Analysieren der `uri` Parameter und rufen Sie die richtige Datenbank-Methode. Wenn die `uri` enthält einen ID-Parameter, und klicken Sie dann die ganze Zahl, die Sie analysiert wird (mithilfe von `LastPathSegment`) und in der Datenbankabfrage verwendet.

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
Dadurch kann der verarbeitenden Anwendung wie diese Daten verarbeitet angewiesen.

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

Unser einfache Beispiel lässt keine Bearbeitung oder Löschung von Daten, aber die INSERT-, Update- und Delete-Methoden implementiert werden müssen sie also ohne Implementierung hinzufügen:

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

Damit ist die grundlegende abgeschlossen `ContentProvider` Implementierung. Nachdem die Anwendung installiert wurde, die Daten, die sie verfügbar macht werden sowohl in der Anwendung, sondern auch für andere Anwendungen, die den Uri, darauf zu verweisen bekannt sind.


## <a name="access-the-contentprovider"></a>Zugriff auf die ContentProvider

Sobald die `VegetableProvider` wurde implementiert, den Zugriff auf sie erfolgt genauso wie der Anbieter "Kontakte" am Anfang dieses Dokuments: Abrufen eines Cursors, der mit dem angegebenen Uri, und klicken Sie dann einen Adapter für den Datenzugriff verwenden.


## <a name="bind-a-listview-to-a-contentprovider"></a>Binden Sie eine ListView an ContentProvider

Zum Auffüllen einer `ListView` mit Daten, die wir verwenden des Uri, der die ungefilterte Liste der Gemüse entspricht. Im Code verwenden wir den konstanten Wert `VegetableProvider.CONTENT_URI`, dem wir wissen, löst in `com.xamarin.sample.vegetableprovider/vegetables`. Unsere `VegetableProvider.Query` Implementierung gibt einen Cursor, der dann an gebunden werden, kann die `ListView`.

Der Code in `SimpleContentProvider/HomeScreen.cs` zeigt, wie einfach es ist zum Anzeigen von Daten aus einer `ContentProvider`:

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

Die Anwendung sieht folgendermaßen aus:

[![Screenshot der app-Angebot, Gemüse, Früchte, sowie deren Blume Knospen, Körnerleguminosen, Glühbirnen, Knollen](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)



## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Abrufen eines einzelnen Elements aus ContentProvider

Eine verarbeitende Anwendung sollten sich auch auf einzelne Zeilen mit Daten, die ausgeführt werden kann, erstellen Sie einen anderen Uri an, die auf eine bestimmte Zeile (z. B.) verweist.

Verwendung `ContentResolver` direkt auf ein einzelnes Element, durch das Einrichten einer Uri mit den erforderlichen `Id`.

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

- [SimpleContentProvider (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
