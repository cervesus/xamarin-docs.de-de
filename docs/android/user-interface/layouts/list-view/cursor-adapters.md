---
title: Verwenden von CursorAdapters
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/25/2017
ms.openlocfilehash: d0b5845036ab2981a4aa06d2a01ed6b13d094bef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028911"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Verwenden von Cursor Adaptern mit xamarin. Android

Android stellt Adapter Klassen speziell zum Anzeigen von Daten aus einer SQLite-Datenbankabfrage bereit:

 **Simplecurrsoradapter** – ähnlich wie eine `ArrayAdapter`, da Sie ohne Unterklassen verwendet werden kann. Geben Sie einfach die erforderlichen Parameter (z. b. einen Cursor und Layoutinformationen) im Konstruktor an, und weisen Sie dann eine `ListView`zu.

 **CursorAdapter** – eine Basisklasse, von der Sie erben können, wenn Sie mehr Kontrolle über die Bindung von Datenwerten an Layoutsteuerelemente benötigen (z. b. das Ausblenden und zeigen von Steuerelementen oder das Ändern ihrer Eigenschaften).

Cursor Adapter bieten eine leistungsstarke Möglichkeit, einen Bildlauf durch lange Listen der in SQLite gespeicherten Daten durchführen zu können. Der verarbeitende Code muss eine SQL-Abfrage in einem `Cursor`-Objekt definieren und dann beschreiben, wie die Ansichten für die einzelnen Zeilen erstellt und aufgefüllt werden.

## <a name="creating-an-sqlite-database"></a>Erstellen einer SQLite-Datenbank

Zum Veranschaulichen der Cursor-Adapter ist eine einfache SQLite-Datenbankimplementierung erforderlich. Der Code in **simplecurrsortableadapter/vegettabledatabase. cs** enthält den Code und SQL, um eine Tabelle zu erstellen und mit Daten zu füllen.
Die gesamte `VegetableDatabase`-Klasse wird hier angezeigt:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
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
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

Die `VegetableDatabase`-Klasse wird in der `OnCreate`-Methode der `HomeScreen`-Aktivität instanziiert. Die `SQLiteOpenHelper` Basisklasse verwaltet die Einrichtung der Datenbankdatei und stellt sicher, dass die SQL-Methode in der `OnCreate`-Methode nur einmal ausgeführt wird. Diese Klasse wird in den folgenden zwei Beispielen für `SimpleCursorAdapter` und `CursorAdapter`verwendet.

Die Cursor Abfrage *muss* über eine ganzzahlige Spalte verfügen, `_id`, damit die `CursorAdapter` funktionieren. Wenn die zugrunde liegende Tabelle keine ganzzahlige Spalte mit dem Namen `_id` hat, verwenden Sie einen Spaltenalias für eine andere eindeutige Ganzzahl in der `RawQuery`, aus der der Cursor besteht. Weitere Informationen finden Sie in der [Android](xref:Android.Widget.CursorAdapter) -Dokumentation.

### <a name="creating-the-cursor"></a>Erstellen des Cursors

In den Beispielen wird eine `RawQuery` verwendet, um eine SQL-Abfrage in ein `Cursor` Objekt umzuwandeln. Die Spaltenliste, die vom Cursor zurückgegeben wird, definiert die Datenspalten, die im CursorAdapter angezeigt werden können. Der Code, der die Datenbank in der **simplecurrsortableadapter/homescreen. cs-`OnCreate`-** Methode erstellt, wird hier angezeigt:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Jeder Code, der `StartManagingCursor` aufruft, sollte auch `StopManagingCursor`aufrufen. In den Beispielen wird `OnCreate` verwendet, um den Cursor zu starten und `OnDestroy` zu schließen. Die `OnDestroy`-Methode enthält folgenden Code:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Sobald eine Anwendung über eine SQLite-Datenbank verfügt und wie gezeigt ein Cursor Objekt erstellt hat, kann Sie entweder eine `SimpleCursorAdapter` oder eine Unterklasse von `CusorAdapter` verwenden, um Zeilen in einem `ListView`anzuzeigen.

## <a name="using-simplecursoradapter"></a>Verwenden von simplecurrsoradapter

`SimpleCursorAdapter` ist wie die `ArrayAdapter`, aber spezialisiert für die Verwendung mit SQLite. Sie erfordert keine Unterklassen – legen Sie einfach einige einfache Parameter fest, wenn Sie das Objekt erstellen, und weisen Sie es dann einer `ListView``Adapter`-Eigenschaft zu.

Die Parameter für den simplecurrsoradapter-Konstruktor lauten:

 **Context** – ein Verweis auf die enthaltende Aktivität.

 **Layout** – die Ressourcen-ID der zu verwendenden Zeilen Ansicht.

 **ICursor** – ein Cursor, der die SQLite-Abfrage für die anzuzeigenden Daten enthält.

 **From** String Array – ein Array von Zeichen folgen, das den Namen der Spalten im Cursor entspricht.

 **Zu** einem ganzzahligen Array – ein Array von Layout-IDs, die den Steuerelementen im Zeilen Layout entsprechen. Der Wert der im `from` Array angegebenen Spalte wird an den ControlID gebunden, der in diesem Array am gleichen Index angegeben ist.

Die `from`-und `to` Arrays müssen die gleiche Anzahl von Einträgen aufweisen, da Sie eine Zuordnung von der Datenquelle zu den Layoutsteuerelementen in der Ansicht bilden.

Der Beispielcode **simplecurrsortableadapter/homescreen. cs** verbindet eine `SimpleCursorAdapter` wie die folgende:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` ist eine schnelle und einfache Möglichkeit, SQLite-Daten in einem `ListView`anzuzeigen. Die Haupt Beschränkung besteht darin, dass Spaltenwerte nur an die Anzeige von Steuerelementen gebunden werden können. es ist nicht möglich, andere Aspekte des Zeilen Layouts zu ändern (z. b. das Anzeigen/Ausblenden von Steuerelementen oder das Ändern von Eigenschaften).

## <a name="subclassing-cursoradapter"></a>Unterklassen-CursorAdapter

Eine `CursorAdapter`-Unterklasse hat die gleichen Leistungsvorteile wie die `SimpleCursorAdapter` zum Anzeigen von Daten aus SQLite, bietet Ihnen aber auch eine umfassende Kontrolle über die Erstellung und das Layout der einzelnen Zeilen Ansichten. Die `CursorAdapter`-Implementierung unterscheidet sich stark von der Unterklassen `BaseAdapter`, da Sie `GetView`, `GetItemId`, `Count` oder `this[]` Indexer nicht außer Kraft setzt.

Wenn Sie eine funktionierende SQLite-Datenbank haben, müssen Sie nur zwei Methoden überschreiben, um eine `CursorAdapter` Unterklasse zu erstellen:

- **BindView** – Hiermit wird eine Ansicht aktualisiert, um die Daten im bereitgestellten Cursor anzuzeigen.

- **NEWVIEW** – wird aufgerufen, wenn das `ListView` eine neue Ansicht zum Anzeigen benötigt. Der `CursorAdapter` kümmert sich um die Wiederverwendung von Sichten (im Gegensatz zur `GetView` Methode auf regulären Adaptern).

Die Adapter Unterklassen in früheren Beispielen verfügen über Methoden zum Zurückgeben der Anzahl von Zeilen und zum Abrufen des aktuellen Elements – das `CursorAdapter` benötigt diese Methoden nicht, da diese Informationen vom Cursor selbst abgeleitet werden können. Wenn Sie die Erstellung und das Auffüllen jeder Ansicht in diese beiden Methoden aufteilen, erzwingt die `CursorAdapter` die erneute Verwendung der Ansicht. Dies steht im Gegensatz zu einem regulären Adapter, bei dem der `convertView`-Parameter der `BaseAdapter.GetView`-Methode ignoriert werden kann.

### <a name="implementing-the-cursoradapter"></a>Implementieren des Cursor Adapters

Der Code in " **currsortableadapter/homeskreencurrsoradapter. cs** " enthält eine `CursorAdapter` Unterklasse. Es speichert einen Kontext Verweis, der an den Konstruktor übergeben wird, sodass er auf eine `LayoutInflater` in der `NewView`-Methode zugreifen kann. Die komplette Klasse sieht wie folgt aus:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```

### <a name="assigning-the-cursoradapter"></a>Zuweisen des Cursor Adapters

Erstellen Sie in der `Activity`, die die `ListView`anzeigt, den Cursor, und weisen Sie ihn `CursorAdapter` dann der Listenansicht zu.

Der Code, der diese Aktion in der `OnCreate` Methode " **Cursor TableAdapter/homescreen. cs** " ausführt, wird hier angezeigt:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

Die `OnDestroy`-Methode enthält den zuvor beschriebenen `StopManagingCursor` Methoden aufzurufen.

## <a name="related-links"></a>Verwandte Links

- [Simplecurrsortableadapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [Cursor TableAdapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
