---
title: Verwenden von CursorAdapters
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: fbdd0f2ea000f0cf46178c615e7526bf7f210a41
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187170"
---
# <a name="using-cursoradapters"></a>Verwenden von CursorAdapters


## <a name="overview"></a>Übersicht

Android bietet Adapterklassen speziell zum Anzeigen von Daten aus einer Abfrage der SQLite-Datenbank:

 **SimpleCursorAdapter** : ähnlich wie auf eine `ArrayAdapter` , da sie ohne Unterklassen verwendet werden kann. Geben Sie einfach die erforderlichen Parameter (z. B. einen Cursor und Layout) im Konstruktor, und weisen Sie einem `ListView`.

 **CursorAdapter** – eine Basisklasse, die Sie aus, wenn Sie mehr über die Bindung von Daten steuern müssen erben können Werte für Layoutsteuerelemente (z. B. Steuerelemente anzeigen/ausblenden oder Änderung ihrer Eigenschaften).

Cursor-Adapter bieten eine leistungsfähige Möglichkeit, einen Bildlauf durch lange Listen mit Daten durchführen, die in SQLite gespeichert sind. Der verwendete Code muss in eine SQL-Abfrage definieren eine `Cursor` Objekt aus, und klicken Sie dann beschrieben, wie Sie erstellen und füllen Sie die Ansichten für jede Zeile.


## <a name="creating-an-sqlite-database"></a>Erstellen einer SQLite-Datenbank

Müssen eine einfache Implementierung der SQLite-Datenbank, um Cursor-Adapter demonstrieren. Der Code in **SimpleCursorTableAdapter/VegetableDatabase.cs** enthält den Code und SQL zum Erstellen einer Tabelle und füllen Sie es mit Daten.
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

Die `VegetableDatabase` in Klasse instanziiert die `OnCreate` Methode der `HomeScreen` Aktivität. Die `SQLiteOpenHelper` Basisklasse verwaltet das Setup der Datenbank ein, und stellt sicher, dass die SQL-Code in die `OnCreate` Methode nur einmal ausgeführt wird. Diese Klasse wird verwendet, in den folgenden zwei Beispielen für `SimpleCursorAdapter` und `CursorAdapter`.

Die Cursorabfrage *müssen* haben Sie eine Spalte mit ganzen Zahlen `_id` für die `CursorAdapter` funktioniert. Wenn die zugrunde liegende Tabelle keine Spalte mit ganzen Zahlen mit dem Namen `_id` verwenden Sie dann einen Spaltenalias für eine andere eindeutige ganze Zahl in die `RawQuery` bildet, die den Cursor. Finden Sie in der [Android-Dokumentation](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/) für Weitere Informationen zu erhalten.


### <a name="creating-the-cursor"></a>Erstellen den Cursor

Die Beispiele verwenden eine `RawQuery` zum Aktivieren einer SQL-Abfrage in eine `Cursor` Objekt. Die Liste der Spalten, die vom Cursor zurückgegeben wird, definiert die Datenspalten, die für die Anzeige in der Cursor-Adapter verfügbar sind. Der Code, die Datenbank in erstellt, der **SimpleCursorTableAdapter/HomeScreen.cs** `OnCreate` Methode wird hier gezeigt:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Jeglicher Code, der Aufrufe `StartManagingCursor` aufgerufen werden soll `StopManagingCursor`. In den Beispielen `OnCreate` gestartet wurde, und `OnDestroy` Schließen des Cursors. Die `OnDestroy` Methode enthält dieser Code:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

Sobald eine Anwendung verfügt über eine SQLite-Datenbank verfügbar und ein Cursorobjekt erstellt wurde, wie gezeigt, können sie nutzen, entweder eine `SimpleCursorAdapter` oder eine Unterklasse von `CusorAdapter` anzuzeigenden Zeilen in einer `ListView`.


## <a name="using-simplecursoradapter"></a>Verwenden von SimpleCursorAdapter

`SimpleCursorAdapter` Wie wird die `ArrayAdapter`, jedoch spezielle für die Verwendung mit SQLite. Es nicht erfordern Unterklassen – legen Sie einfach einige einfache Parameter beim Erstellen des Objekts und weisen Sie ihn ein `ListView`des `Adapter` Eigenschaft.

Die Parameter für den Konstruktor SimpleCursorAdapter sind:

 **Kontext** – einen Verweis auf die enthaltende Aktivität.

 **Layout** – die Ressourcen-ID der Zeile an.

 **ICursor** – einen Cursor, die die SQLite-Abfrage für die anzuzeigenden Daten enthält.

 **Von** Zeichenfolgenarray – ein Array von Zeichenfolgen, die entsprechend den Namen der Spalten im Cursor.

 **Um** Ganzzahlarray – ein Array von Layout-IDs, die die Steuerelemente in das Zeilenlayout entsprechen. Der Wert der Spalte angegeben wird, der `from` Array wird an die in diesem Array, an den gleichen Index angegebenen ControlID gebunden werden.

Die `from` und `to` Arrays müssen die gleiche Anzahl von Einträgen, da sie eine Zuordnung aus der Datenquelle an die Layoutsteuerelemente in der Ansicht bilden.

Die **SimpleCursorTableAdapter/HomeScreen.cs** sample Code verbindet sich ein `SimpleCursorAdapter` wie folgt aus:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter` ist eine schnelle und einfache Möglichkeit zum Anzeigen von SQLite-Daten in einem `ListView`. Die wesentliche Einschränkung besteht, kann nur bindet, bindet es Spaltenwerte Steuerelemente angezeigt, die es erlaubt Ihnen nicht so ändern Sie andere Aspekte des Zeilenlayouts (z. B. Steuerelemente anzeigen/ausblenden oder Ändern von Eigenschaften).


## <a name="subclassing-cursoradapter"></a>Erstellen von Unterklassen für CursorAdapter

Ein `CursorAdapter` Unterklasse verfügt über die gleichen Leistungsvorteile als die `SimpleCursorAdapter` für das Anzeigen von Daten von SQLite, sondern auch Sie vollständige Kontrolle über die Erstellung und das Layout der jede Zeile anzeigen kann. Die `CursorAdapter` Implementierung unterscheidet sich sehr von Unterklassen `BaseAdapter` , da es nicht überschreibt `GetView`, `GetItemId`, `Count` oder `this[]` Indexer.

Erhalten eine funktionierende SQLite-Datenbank, Sie brauchen nur noch Überschreiben zweier Methoden zum Erstellen einer `CursorAdapter` Unterklasse:

- **BindView** – Wenn eine Ansicht, zum Anzeigen der Daten in den angegebenen Cursor aktualisieren.

- **NewView** : wird aufgerufen, wenn die `ListView` erfordert eine neue Ansicht angezeigt. Die `CursorAdapter` übernimmt das Wiederverwenden von Ansichten (im Gegensatz zu den `GetView` Methode für reguläre Adapter).

Die Adapter Unterklassen in vorherigen Beispielen haben Methoden zum Zurückgeben der Anzahl von Zeilen und zum Abrufen des aktuellen Elements – der `CursorAdapter` erfordert keine dieser Methoden aus, da diese Informationen aus der Cursor selbst abgeleitet werden kann. Durch aufteilen, das Erstellen und Auffüllen von jeder Ansicht in diesen beiden Methoden, die `CursorAdapter` erzwingt die Sicht erneut zu verwenden. Dies ist im Gegensatz dazu an einer regulären Adapter, ist es möglich, ignorieren die `convertView` Parameter, der die `BaseAdapter.GetView` Methode.


### <a name="implementing-the-cursoradapter"></a>Implementieren der CursorAdapter

Der Code in **CursorTableAdapter/HomeScreenCursorAdapter.cs** enthält eine `CursorAdapter` Unterklasse. Sie speichert einen Kontext-Verweis an den Konstruktor übergeben wird, sodass er zugreifen kann eine `LayoutInflater` in die `NewView` Methode. Die vollständige Klasse sieht folgendermaßen aus:

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

In der `Activity` zeigt, die die `ListView`, erstellen Sie den Cursor und `CursorAdapter` weisen es dann der Liste angezeigt.

Der Code, der diese Aktion in führt die **CursorTableAdapter/HomeScreen.cs** `OnCreate` Methode wird hier gezeigt:

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

- [SimpleCursorTableAdapter (sample)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/CursorTableAdapter/)
