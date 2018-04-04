---
title: Verwenden von CursorAdapters
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: 20311cc50c87638391d8b078c405bc61baceb373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="using-cursoradapters"></a>Verwenden von CursorAdapters


## <a name="overview"></a>Übersicht

Android bietet Adapterklassen, insbesondere, um Daten aus einer Datenbankabfrage SQLite anzuzeigen:

 **SimpleCursorAdapter** : ähnlich wie auf ein `ArrayAdapter` , da sie ohne Erstellung von Unterklassen von verwendet werden kann. Geben Sie die erforderlichen Parameter (z. B. einen Cursor und Layout-Informationen) einfach im Konstruktor ein, und weisen Sie dann eine `ListView`.

 **CursorAdapter** – eine Basisklasse, die Sie aus, wenn Sie mehr über die Bindung von Daten steuern müssen erben können Werte für Layoutsteuerelemente (z. B. Steuerelemente ausblenden/einblenden oder ihre Eigenschaften ändern).

Cursor-Adapter bieten eine leistungsstarke Möglichkeit, einen Bildlauf durch lange Listen mit Daten, die in SQLite gespeichert sind. Der verwendete Code muss in eine SQL-Abfrage definieren eine `Cursor` Objekt, und klicken Sie dann beschrieben, wie das Erstellen und füllen Sie die Ansichten für jede Zeile.


## <a name="creating-an-sqlite-database"></a>Erstellen eine SQLite-Datenbank

Müssen eine einfache Implementierung der SQLite-Datenbank, um Cursor Adapter demonstrieren. Der Code in **SimpleCursorTableAdapter/VegetableDatabase.cs** enthält den Code und SQL zu erstellen, eine Tabelle mit Daten aufzufüllen.
Die vollständige `VegetableDatabase` Klasse wird hier gezeigt:

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

Die `VegetableDatabase` Klasse instanziiert werden der `OnCreate` Methode der `HomeScreen` Aktivität. Die `SQLiteOpenHelper` Basisklasse verwaltet die Einrichtung der Datenbankdatei und stellt sicher, dass das SQL in seiner `OnCreate` Methode wird nur einmal ausgeführt. Diese Klasse wird verwendet, in den folgenden zwei Beispielen für `SimpleCursorAdapter` und `CursorAdapter`.

Die Cursorabfrage *müssen* haben eine Spalte mit ganzen Zahlen `_id` für die `CursorAdapter` arbeiten. Wenn die zugrunde liegende Tabelle keine Integer-Spalte, die mit dem Namen `_id` verwenden Sie einen Spaltenalias für eine andere eindeutige ganze Zahl in die `RawQuery` , aus der Cursor besteht. Finden Sie in der [Android Docs](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) für Weitere Informationen zu erhalten.


### <a name="creating-the-cursor"></a>Erstellen den Cursor

Die Beispiele verwenden eine `RawQuery` So aktivieren Sie eine SQL-Abfrage in eine `Cursor` Objekt. Die Liste der Spalten, die vom Cursor zurückgegeben wird definiert, die Datenspalten, die für die Anzeige im Cursor Adapter verfügbar sind. Der Code, der die Datenbank im erstellt die **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` Methode ist im folgenden dargestellt:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Jeglicher Code, der Aufrufe `StartManagingCursor` sollten auch aufrufen, `StopManagingCursor`. In den Beispielen `OnCreate` gestartet werden, und `OnDestroy` Schließen des Cursors. Die `OnDestroy` Methode enthält dieser Code:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Sobald eine Anwendung eine SQLite-Datenbank verfügbar hat und ein Cursorobjekt erstellt wurde, wie dargestellt, kann es verwenden, entweder eine `SimpleCursorAdapter` oder eine Unterklasse von `CusorAdapter` anzuzeigenden Zeilen in eine `ListView`.


## <a name="using-simplecursoradapter"></a>Verwenden von SimpleCursorAdapter

`SimpleCursorAdapter` entspricht der `ArrayAdapter`, aber für die Verwendung mit SQLite spezialisiert. Es nicht erfordern Unterklassen – einige einfache Parameter nur festgelegt werden, beim Erstellen des Objekts und weisen Sie ihn einer `ListView`des `Adapter` Eigenschaft.

Die Parameter des Konstruktors SimpleCursorAdapter lauten:

 **Kontext** – einen Verweis auf die enthaltende Aktivität.

 **Layout** – die Ressourcen-ID der Zeilenansicht zu verwenden.

 **ICursor** – einen Cursor, die die SQLite-Abfrage für die anzuzeigenden Daten enthält.

 **Aus** Zeichenfolgenarray – ein Array von Zeichenfolgen, die entsprechend den Namen der Spalten im Cursor.

 **Um** Ganzzahlarray – ein Array von Layout-IDs, die die Steuerelemente in das Zeilenlayout entsprechen. Der Wert der Spalte angegeben wird, der `from` Array wird an die in diesem Array am gleichen Index angegebenen ControlID gebunden werden.

Die `from` und `to` Arrays müssen die gleiche Anzahl von Einträgen haben, da sie eine Zuordnung aus der Datenquelle zu Layout-Steuerelemente in der Ansicht bilden.

Die **SimpleCursorTableAdapter/HomeScreen.cs** Beispiel Code Drähten Einrichten einer `SimpleCursorAdapter` wie folgt:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` ist eine schnelle und einfache Möglichkeit zum Anzeigen von SQLite-Daten in eine `ListView`. Die haupteinschränkung ist, dass es kann nur Spaltenwerte anzuzeigenden Steuerelemente binden, er lässt keine anderen Aspekte des Zeilenlayouts (z. B. Steuerelemente anzeigen/ausblenden oder Ändern von Eigenschaften) ändern.


## <a name="subclassing-cursoradapter"></a>Erstellen von Unterklassen CursorAdapter

Ein `CursorAdapter` Unterklasse hat die gleichen Leistungsvorteile als die `SimpleCursorAdapter` Anzeigen von Daten aus SQLite, sondern auch Sie vollständige Kontrolle über die Erstellung und das Layout der einzelnen Ansichten Zeile durch zur Verfügung stehen. Die `CursorAdapter` Implementierung ist grundlegend von Unterklassen `BaseAdapter` , da es nicht überschreibt `GetView`, `GetItemId`, `Count` oder `this[]` Indexer.

Erhält eine funktionierende SQLite-Datenbank, müssen Sie nur überschreiben, zwei Methoden zum Erstellen einer `CursorAdapter` Unterklasse:

- **BindView** – Wenn Sie eine Sicht, ein update zum Anzeigen der Daten in den angegebenen Cursor.

- **NewView** – wird aufgerufen, wenn die `ListView` erfordert eine neue Ansicht angezeigt. Die `CursorAdapter` übernimmt das recycling von Sichten (im Gegensatz zu den `GetView` Methode auf reguläre Adapter).

Der Adapter Unterklassen in vorherigen Beispielen haben Methoden zum Zurückgeben der Anzahl der Zeilen sowie zum Abrufen des aktuellen Elements – der `CursorAdapter` erfordert keine dieser Methoden aus, da diese Informationen aus dem Cursor selbst abgeleitet werden kann. Durch das Erstellen und Auffüllen der einzelnen Ansichten in diesen beiden Methoden Aufteilen der `CursorAdapter` erzwingt die Sicht erneut zu verwenden. Dies ist im Gegensatz dazu eine reguläre Adapter, ist es möglich ist, ignoriert der `convertView` Parameter von der `BaseAdapter.GetView` Methode.


### <a name="implementing-the-cursoradapter"></a>Implementieren der CursorAdapter

Der Code in **CursorTableAdapter/HomeScreenCursorAdapter.cs** enthält eine `CursorAdapter` Unterklasse. Sie speichert einen Kontext Verweis an den Konstruktor übergeben wird, sodass darauf zugreifen können eine `LayoutInflater` in die `NewView` Methode. Die vollständige Klasse sieht wie folgt:

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


### <a name="assigning-the-cursoradapter"></a>Zuweisen der CursorAdapter

In der `Activity` zeigt, die die `ListView`, erstellen Sie den Cursor und `CursorAdapter` weisen Sie diese der Liste angezeigt.

Der Code, der führt diese Aktion in der **CursorTableAdapter/HomeScreen.cs** `OnCreate` Methode ist im folgenden dargestellt:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

Die `OnDestroy` Methode enthält die `StopManagingCursor` Methodenaufruf, der zuvor beschriebenen.



## <a name="related-links"></a>Verwandte Links

- [SimpleCursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/CursorTableAdapter/)
