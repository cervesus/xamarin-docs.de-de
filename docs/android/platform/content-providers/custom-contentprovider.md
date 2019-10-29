---
title: Erstellen eines benutzerdefinierten ContentProvider
description: Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten Contentprovider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter Contentprovider erstellt und anschließend seine Daten genutzt werden.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020523"
---
# <a name="creating-a-custom-contentprovider"></a>Erstellen eines benutzerdefinierten ContentProvider

_Im vorherigen Abschnitt wurde gezeigt, wie Daten aus einer integrierten Contentprovider-Implementierung genutzt werden. In diesem Abschnitt wird erläutert, wie ein benutzerdefinierter Contentprovider erstellt und anschließend seine Daten genutzt werden._

## <a name="about-contentproviders"></a>Informationen zu contentproviders

Eine Inhaltsanbieter Klasse muss von `ContentProvider`erben. Er sollte aus einem internen Datenspeicher bestehen, der zum Antworten auf Abfragen verwendet wird, und er sollte URIs und MIME-Typen als Konstanten verfügbar machen, um die Verwendung von Code zu unterstützen, die gültige Datenanforderungen verarbeiten.

### <a name="uri-authority"></a>URI (Authority)

auf `ContentProviders` wird in Android mithilfe eines URI zugegriffen. Eine Anwendung, die eine `ContentProvider` verfügbar macht, legt die URIs fest, auf die in der Datei " **androidmanifest. XML** " reagiert wird. Wenn die Anwendung installiert ist, werden diese URIs registriert, damit andere Anwendungen darauf zugreifen können.

In Mono für Android sollte die Content Provider-Klasse über ein `[ContentProvider]`-Attribut verfügen, um den URI (oder die URIs) anzugeben, der " **androidmanifest. XML**" hinzugefügt werden soll.

### <a name="mime-type"></a>MIME-Typ

Das typische Format für MIME-Typen besteht aus zwei Teilen. Android `ContentProviders` diese beiden Zeichen folgen in der Regel für den ersten Teil des MIME-Typs verwenden:

1. `vnd.android.cursor.item` &ndash;, um eine einzelne Zeile darzustellen, verwenden Sie die `ContentResolver.CursorItemBaseType` Konstante im Code.

1. `vnd.android.cursor.dir` &ndash; für mehrere Zeilen verwenden, verwenden Sie die `ContentResolver.CursorDirBaseType` Konstante im Code.

Der zweite Teil des MIME-Typs ist spezifisch für Ihre Anwendung, und es sollte ein Reverse-DNS-Standard mit einem `vnd.`-Präfix verwendet werden. Im Beispielcode wird `vnd.com.xamarin.sample.Vegetables`verwendet.

### <a name="data-model-metadata"></a>Metadaten des Datenmodells

Die Verwendung von Anwendungen muss URI-Abfragen erstellen, um auf verschiedene Datentypen zuzugreifen. Der Basis-URI kann erweitert werden, um auf eine bestimmte Datentabelle zu verweisen, und kann auch Parameter zum Filtern der Ergebnisse enthalten. Die Spalten und Klauseln, die mit dem resultierenden Cursor zum Anzeigen von Daten verwendet werden, müssen ebenfalls deklariert werden.

Um sicherzustellen, dass nur gültige URI-Abfragen erstellt werden, ist es üblich, die gültigen Zeichen folgen als Konstante Werte bereitzustellen. Dadurch wird der Zugriff auf die `ContentProvider` vereinfacht, da die Werte durch Codevervollständigung erkennbar sind, und es wird verhindert, dass Tippfehler in den Zeichen folgen sind.

Im vorherigen Beispiel hat die `android.provider.ContactsContract`-Klasse die Metadaten für die Contacts-Daten verfügbar gemacht. Für unsere benutzerdefinierten `ContentProvider` werden die Konstanten nur für die Klasse verfügbar gemacht.

## <a name="implementation"></a>Implementierung

Zum Erstellen und Verwenden eines benutzerdefinierten `ContentProvider`sind drei Schritte erforderlich:

1. **Erstellen Sie eine Datenbankklasse** &ndash; implementieren Sie `SQLiteOpenHelper`.

2. **Erstellen Sie eine `ContentProvider` Klasse** &ndash; implementieren Sie `ContentProvider` mit einer Instanz der Datenbank, Metadaten, die als Konstante Werte verfügbar gemacht werden, und Methoden für den Zugriff auf die Daten.

3. **Greifen Sie über den URI auf die `ContentProvider`** zu, &ndash; eine `CursorAdapter` mithilfe der `ContentProvider`auffüllen, auf die über den zugehörigen URI zugegriffen wird.

Wie bereits erwähnt, können `ContentProviders` von anderen Anwendungen verwendet werden, als wenn Sie definiert sind. In diesem Beispiel werden die Daten in der gleichen Anwendung verwendet, denken Sie jedoch daran, dass andere Anwendungen auch darauf zugreifen können, solange Sie den URI und die Informationen über das Schema kennen (in der Regel als Konstante Werte verfügbar).

## <a name="create-a-database"></a>Erstellen einer Datenbank

Die meisten `ContentProvider` Implementierungen basieren auf einer `SQLite` Datenbank. Der Beispiel Datenbankcode in **simplecontentprovider/vegettabledatabase. cs** erstellt eine sehr einfache Datenbank mit zwei Spalten, wie hier gezeigt:

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

Die Datenbankimplementierung selbst benötigt keine besonderen Überlegungen, damit Sie mit einem `ContentProvider`verfügbar gemacht werden. Wenn Sie jedoch beabsichtigen, die `ContentProvider's` Daten an ein `ListView`-Steuerelement zu binden, muss eine eindeutige ganzzahlige Spalte mit dem Namen `_id` Teil des Resultsets sein. Weitere Informationen zur Verwendung des `ListView`-Steuer Elements finden Sie im Dokument [ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) .

## <a name="create-the-contentprovider"></a>Erstellen des Contentprovider

Im restlichen Teil dieses Abschnitts finden Sie Schritt-für-Schritt-Anleitungen dazu, wie die Beispiel Klasse **simplecontentprovider/vegettableprovider. cs** erstellt wurde.

### <a name="initialize-the-database"></a>Initialisieren der Datenbank

Der erste Schritt besteht in der Unterklasse `ContentProvider` und der hinzufügen der zu verwendenden Datenbank.

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

Es gibt vier verschiedene Arten von Metadaten, die wir für die `ContentProvider`-Klasse verfügbar machen. Nur die Autorität ist erforderlich. der Rest wird gemäß der Konvention ausgeführt.

- **Authority** &ndash; das `ContentProvider` Attribut *muss* der Klasse hinzugefügt werden, damit es bei Android registriert wird, wenn die Anwendung installiert wird.

- **URI** &ndash; die `CONTENT_URI` als Konstante verfügbar gemacht wird, sodass Sie im Code einfach zu verwenden ist. Er sollte der Zertifizierungsstelle entsprechen, aber das Schema und den Basispfad einschließen.

- **MIME-Typen** &ndash; Listen der Ergebnisse und einzelne Ergebnisse werden als unterschiedliche Inhaltstypen behandelt. daher definieren wir zwei MIME-Typen, um Sie darzustellen.

- **Interfakeconsts** &ndash; einen konstanten Wert für jeden Datenspalten Namen bereitstellen, sodass der verarbeitende Code Sie problemlos ermitteln und darauf verweisen kann, ohne typografische Fehler zu riskieren.

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

Da die Nutzung von Code URIs verwendet, um Anforderungen an einen `ContentProvider`zu stellen, müssen wir diese Anforderungen analysieren können, um zu bestimmen, welche Daten zurückgegeben werden müssen. Die `UriMatcher`-Klasse kann dazu beitragen, URIs zu analysieren, sobald Sie mit den URI-Mustern initialisiert wurde, die vom `ContentProvider` unterstützt werden.

Der `UriMatcher` im Beispiel wird mit zwei URIs initialisiert:

1. *"com. xamarin. Sample. vegettableprovider/Gemüse"* &ndash; Anforderung, die vollständige Liste der Gemüse zurückzugeben.

2. *"com. xamarin. Sample. vegettableprovider/Gemüse/\#"* &ndash;, wobei der \# ein Platzhalter für einen numerischen Parameter (der `_id` der Zeile in der Datenbank) ist. Ein Sternchen-Platzhalter ("\*") kann auch verwendet werden, um einen Text Parameter abzugleichen.

Im Code verwenden wir die Konstanten, um auf Metadatenwerte wie die Autorität und den Basis\_Pfad zu verweisen. Die Rückgabecodes werden in Methoden verwendet, die eine URI-Verarbeitung durchführen, um zu bestimmen, welche Daten zurückgegeben werden sollen.

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

Dieser Code ist für die `ContentProvider`-Klasse privat. Weitere Informationen finden Sie in [der urimatcher-Dokumentation von Google](xref:Android.Content.UriMatcher) .

## <a name="implement-the-querymethod"></a>Implementieren der querymethod

Die einfachste `ContentProvider` Methode, die implementiert werden soll, ist die `Query`-Methode. Die nachfolgende Implementierung verwendet die `UriMatcher`, um den `uri`-Parameter zu analysieren und die richtige Daten Bank Methode aufzurufen. Wenn die `uri` einen ID-Parameter enthält, wird die ganze Zahl analysiert (mithilfe `LastPathSegment`) und in der Datenbankabfrage verwendet.

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

Die `GetType`-Methode muss ebenfalls überschrieben werden. Diese Methode kann aufgerufen werden, um den Inhaltstyp zu bestimmen, der für einen angegebenen URI zurückgegeben wird.
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

Dies schließt die grundlegende `ContentProvider`-Implementierung ab. Nachdem die Anwendung installiert wurde, werden die Daten, die Sie verfügbar macht, sowohl innerhalb der Anwendung als auch in jeder anderen Anwendung verfügbar sein, die den URI kennt, um darauf zu verweisen.

## <a name="access-the-contentprovider"></a>Zugreifen auf den Contentprovider

Nachdem der `VegetableProvider` implementiert wurde, erfolgt der Zugriff auf die gleiche Weise wie der Kontakt Anbieter am Anfang dieses Dokuments: Rufen Sie einen Cursor mit dem angegebenen URI ab, und verwenden Sie dann einen Adapter für den Zugriff auf die Daten.

## <a name="bind-a-listview-to-a-contentprovider"></a>Binden einer ListView an einen Contentprovider

Zum Auffüllen eines `ListView` mit Daten verwenden wir den URI, der der ungefilterten Liste von Gemüse entspricht. Im Code verwenden wir den konstanten Wert `VegetableProvider.CONTENT_URI`, der in `com.xamarin.sample.vegetableprovider/vegetables`aufgelöst wird. Unsere `VegetableProvider.Query` Implementierung gibt einen Cursor zurück, der dann an den `ListView`gebunden werden kann.

Der Code in `SimpleContentProvider/HomeScreen.cs` zeigt, wie einfach es ist, Daten aus einem `ContentProvider`anzuzeigen:

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

[![Screenshot der APP, die Gemüse, Früchte, Blumen Knospen, legumes, Glühbirnen, tubers auflistet.](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Abrufen eines einzelnen Elements aus einem Contentprovider

Eine verarbeitende Anwendung möchte möglicherweise auch auf einzelne Daten Zeilen zugreifen. Dies kann durch das Erstellen eines anderen URIs erfolgen, der auf eine bestimmte Zeile verweist (z. b.).

Verwenden Sie `ContentResolver` direkt, um auf ein einzelnes Element zuzugreifen, indem Sie einen URI mit dem erforderlichen `Id`bilden.

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
