---
title: Anpassen der Darstellung einer ListView
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1bf481e4999365f4afc52cb9dda83c6e627950e1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-listviews-appearance"></a>Anpassen der Darstellung einer ListView


## <a name="overview"></a>Übersicht

Die Darstellung einer ListView wird durch das Layout der angezeigten Zeilen vorgegeben. So ändern Sie die Darstellung einer `ListView`, verwenden Sie eine andere Zeilenlayout.


## <a name="built-in-row-views"></a>Integrierte Zeilenansichten

Es gibt zwölf integrierte Sichten, die mit verwiesen werden können **Android.Resource.Layout**:

- **TestListItem** &ndash; einzelne Textzeile mit minimaler Formatierung.

- **SimpleListItem1** &ndash; einzelne Textzeile.

- **SimpleListItem2** &ndash; beiden Textzeilen.

- **SimpleSelectableListItem** &ndash; einzelne Textzeile, die einem oder mehreren Elementauswahl (eingeführt in API-Ebene 11) unterstützt.

- **SimpleListItemActivated1** &ndash; SimpleListItem1 ähnlich, aber die Farbe des Hintergrunds gibt an, wenn eine Zeile ausgewählt ist (eingeführt in API-Ebene 11).

- **SimpleListItemActivated2** &ndash; SimpleListItem2 ähnlich, aber die Farbe des Hintergrunds gibt an, wenn eine Zeile ausgewählt ist (eingeführt in API-Ebene 11).

- **SimpleListItemChecked** &ndash; zeigt Häkchen Auswahl an.

- **SimpleListItemMultipleChoice** &ndash; Displays check boxes to indicate multiple-choice selection.

- **SimpleListItemSingleChoice** &ndash; zeigt Optionsfelder gegenseitig Auswahl an.

- **TwoLineListItem** &ndash; beiden Textzeilen.

- **ActivityListItem** &ndash; einzelne Textzeile mit einem Bild.

- **SimpleExpandableListItem** &ndash; Zeilengruppen nach Kategorien und jede Gruppe erweitert oder reduziert werden können.

Jede Zeilenansicht integrierten verfügt über einen integrierten Stil zugeordnet. Diese Screenshots zeigen, wie jede Ansicht angezeigt wird:

[![Screenshots TestListItem, SimpleSelectableListItem SimpleListitem1 und SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Screenshots SimpleListItemActivated1, SimpleListItemActivated2 SimpleListItemChecked und SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Screenshots SimpleListItemSingleChoice, TwoLineListItem ActivityListItem und SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

Die **BuiltInViews/HomeScreenAdapter.cs** -Beispieldatei (in der **BuiltInViews** Lösung) enthält den Code, um die nicht erweiterbare Liste Element Bildschirme zu erstellen. Die Ansicht wird festgelegt, der `GetView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Eigenschaften der Sicht können dann durch Verweisen auf die standard-Steuerelement-IDs festgelegt werden `Text1`, `Text2` und `Icon` unter `Android.Resource.Id` (Eigenschaften, die die Sicht enthält keine oder eine Ausnahme ausgelöst, nicht festgelegt):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

Die **BuiltInExpandableViews/ExpandableScreenAdapter.cs** -Beispieldatei (in der **BuiltInViews** Lösung) enthält den Code, um den Bildschirm SimpleExpandableListItem zu erzeugen. Die Ansicht "" festgelegt ist, der `GetGroupView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Die untergeordnete Ansicht wird festgelegt, der `GetChildView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Dann zum Festlegen der Eigenschaften für die Ansicht "" und die untergeordnete Ansicht verweisen auf den Standard `Text1` und `Text2` Bezeichner zu steuern, wie oben gezeigt. Der SimpleExpandableListItem Screenshot (siehe oben) enthält ein Beispiel einer einzeiligen Gruppe (SimpleExpandableListItem1) und einer zweizeiligen untergeordnete Ansicht (SimpleExpandableListItem2). Klicken Sie alternativ die Ansicht "" kann für zwei Zeilen (SimpleExpandableListItem2) konfiguriert werden und für eine Zeile (SimpleExpandableListItem1), kann die untergeordnete Ansicht konfiguriert werden oder beide Gruppe Ansicht und untergeordnete Ansicht kann die gleiche Anzahl von Zeilen haben. 



## <a name="accessories"></a>Zubehör

Zeilen können Zubehör rechts der Ansicht zur Angabe des Auswahlstatus hinzugefügt haben:

- **SimpleListItemChecked** &ndash; erstellt eine Liste von mit Einfachauswahl mit einem Häkchen als Indikator.

- **SimpleListItemSingleChoice** &ndash; erstellt Radio Schaltflächentyp Listen, in denen nur eine Auswahl möglich ist.

- **SimpleListItemMultipleChoice** &ndash; erstellt Checkbox-Type-Listen, in denen mehrere Optionen möglich sind.

Die zuvor erwähnten Zubehör sind in den folgenden Bildschirmen, in der entsprechenden Reihenfolge dargestellt:

[![Screenshots der SimpleListItemChecked SimpleListItemSingleChoice und SimpleListItemMultipleChoice mit Zubehör](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Zum Anzeigen eines diese Zubehör übergeben Festlegen der erforderlichen Layout-Ressourcen-ID an den Adapter klicken Sie dann manuell den Auswahlzustand für die erforderlichen Zeilen. Diese Codezeile veranschaulicht das Erstellen und Zuweisen einer `Adapter` mit einer der für diese Layouts:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

Die `ListView` selbst unterstützt verschiedene Modi, unabhängig von den Accessor, der angezeigt wird. Um Verwirrung zu vermeiden, verwenden Sie `Single` Auswahlmodus mit `Checked` und `SingleChoice` Zubehör und `Multiple` Modus mit der `MultipleChoice` Stil. Der Auswahlmodus wird gesteuert, indem die `ChoiceMode` Eigenschaft von der `ListView`.


### <a name="handling-api-level"></a>Behandeln von API-Ebene

Frühere Versionen von Xamarin.Android Enumerationen, die als ganzzahligen Eigenschaften implementiert wird. Die neueste Version wurde richtige .NET Enumerationstypen eingeführt, wodurch sie viel einfacher zu ermitteln, die möglichen Optionen.

Abhängig von der API-Ebene anvisierten, `ChoiceMode` ist eine ganze Zahl oder eine Enumeration. Die Beispieldatei **AccessoryViews/HomeScreen.cs** hat ein Block auskommentiert gegebenenfalls die Gingerbread-API als Ziel:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>Programmgesteuertes auswählen von Elementen

Manuelles Festlegen der Elemente sind "aktiviert" erfolgt mit der `SetItemChecked` Methode (es kann mehrfach aufgerufen werden für die Mehrfachauswahl):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Der Code muss auch einzelne Auswahl anders als Mehrfachauswahl zu erkennen. Um zu bestimmen, welche Zeile ausgewählt wurde `Single` Modus verwenden die `CheckedItemPosition` Ganzzahleigenschaft:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Um zu bestimmen, welche Zeilen ausgewählt wurden `Multiple` Modus Sie so durchlaufen Sie müssen die `CheckedItemPositions` `SparseBooleanArray`. Ein Array mit geringer Dichte ist wie ein Wörterbuch, das nur enthält Einträge, wurde der Wert geändert wurde, damit Sie das gesamte Array sucht durchlaufen müssen `true` Werte zu wissen, was in der Liste ausgewählt wurde, wie im folgenden Codeausschnitt dargestellt:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>Erstellen von benutzerdefinierten Zeile Layouts

Die vier integrierte Zeilenansichten sind sehr einfach. Zum Anzeigen von komplexeren Layouts (z. B. eine Liste mit e-Mails oder Tweets oder Kontaktinformationen) ist eine benutzerdefinierte Ansicht erforderlich. Benutzerdefinierte Ansichten werden im Allgemeinen als AXML Dateien deklariert die **Ressourcen/Layout** Verzeichnis, und klicken Sie dann die mit ihren Ressourcen-Id von einem benutzerdefinierten Adapter geladen. Die Ansicht kann eine beliebige Anzahl von Anzeige-Klassen (z. B. TextViews, ImageViews und anderen Steuerelementen) mit benutzerdefinierten Farben, Schriftarten und Layout enthalten.

Dieses Beispiel unterscheidet sich von den vorherigen Beispielen auf vielfältige Weise:

-  Erbt von `Activity` , nicht `ListActivity` . Sie können Zeilen für alle anpassen `ListView` , jedoch auch andere Steuerelemente auch aufgenommen werden können, können Sie in einer `Activity` Layout (z. B. eine Überschrift, Schaltflächen oder andere Elemente der Benutzeroberfläche). In diesem Beispiel wird eine Überschrift oben die `ListView` veranschaulichen.

-  Erfordert eine AXML Layout-Datei für den Bildschirm an. in den vorherigen Beispielen die `ListActivity` eine Layoutdatei ist nicht erforderlich. Diese AXML enthält eine `ListView` Deklaration zu steuern.

-  Erfordert eine AXML Layout-Datei, um jede Zeile zu rendern. Diese AXML-Datei enthält die Text- und Image-Steuerelemente mit benutzerdefinierten Schriftart- und farbeinstellungen an.

-  Verwendet eine optionale benutzerdefinierte Auswahl XML-Datei die Darstellung der Zeile festgelegt, wenn diese Option ausgewählt ist.

-  Die `Adapter` Implementierung gibt ein benutzerdefiniertes Layout aus der `GetView` außer Kraft setzen.

-  `ItemClick` anders deklariert werden muss (an ein Ereignishandler angefügt ist `ListView.ItemClick` anstelle einer überschreiben `OnListItemClick` in `ListActivity`).


Diese Änderungen werden im folgenden beschrieben, beginnend mit der Aktivität und die benutzerdefinierte Zeilenansicht erstellen, und klicken Sie dann die Änderungen an den Adapter und die Aktivität, um die Berichte Rendern abdecken.


### <a name="adding-a-listview-to-an-activity-layout"></a>Ein Aktivitätslayout für die hinzugefügt eine Listenansicht

Da `HomeScreen` erbt nicht mehr von `ListActivity` keine Standardansicht, daher muss eine Layout-AXML-Datei für die Startseite Ansicht erstellt werden. In diesem Beispiel wird die Sicht auf eine Spaltenüberschrift haben (mit einem `TextView`) und ein `ListView` zum Anzeigen von Daten. Das Layout wird definiert, der **Resources/Layout/HomeScreen.axml** Datei, die hier gezeigt wird:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

Der Vorteil der Verwendung einer `Activity` über ein benutzerdefiniertes Layout (anstelle von einer `ListActivity`) liegt in der Möglichkeit, zusätzliche Steuerelemente auf dem Bildschirm z. B. die Überschrift hinzufügen `TextView` in diesem Beispiel.


### <a name="creating-a-custom-row-layout"></a>Erstellen eines benutzerdefinierten Zeile Layouts

Eine andere AXML Layoutdatei ist erforderlich, die benutzerdefinierte Layout für jede Zeile enthält, die in der Listenansicht angezeigt wird. In diesem Beispiel müssen die Zeile einen grünen Hintergrund, braunen Text und Bild rechts ausgerichtet. Das Android-XML-Markup zum Rendern dieses Layout deklarieren beschreiben **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

Während eine benutzerdefinierte Zeilenlayout viele verschiedene Steuerelemente enthalten kann, Durchführen eines Bildlaufs Leistung kann von komplexen Entwürfe betroffen sein, und mit Bildern (vor allem wenn sie über das Netzwerk geladen werden). Finden Sie Google Artikel Weitere Informationen zur Rückgabe von fortlaufenden Leistungsprobleme.


### <a name="referencing-a-custom-row-view"></a>Verweisen auf eine benutzerdefinierte Zeilenansicht

Die Implementierung des Beispiels für benutzerdefinierte Adapter ist `HomeScreenAdapter.cs`. Ist die Hauptmethode `GetView` , in denen es lädt die benutzerdefinierte AXML mithilfe der Ressourcen-ID `Resource.Layout.CustomView`, und legt dann die Eigenschaften für jedes der Steuerelemente in der Ansicht vor dem zurückgeben. Die vollständige Adapterklasse wird angezeigt:

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>Verweisen auf benutzerdefinierte ListView in der Aktivität

Da die `HomeScreen` Klasse erbt jetzt von `Activity`ein `ListView` Feld ist deklariert, in der Klasse, um einen Verweis auf das Steuerelement in der AXML deklariert enthalten:

```csharp
ListView listView;
```

Die Klasse muss dann die Aktivität benutzerdefinierte Layout AXML Laden mithilfe der `SetContentView` Methode. Sie finden können die `ListView` Steuerelement im Layout anschließend erstellt und weist den Adapter und weist Sie den Click-Handler. Der Code für die Methode OnCreate wird hier gezeigt:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Schließlich die `ItemClick` Ereignishandler muss definiert werden; zeigt in diesem Fall nur eine `Toast` Nachricht:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Der resultierende Bildschirm sieht wie folgt:

[![Screenshot des resultierenden CustomRowView](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>Die Farbe der Zeile Auswahl anpassen

Wenn eine Zeile verwendet wird, sollte es für Benutzerfeedback markiert sein. Wenn eine benutzerdefinierte Ansicht gibt als Hintergrundfarbe als **CustomView.axml** der Fall ist, wird auch die auswahlmarkierung überschrieben. Diese Codezeile in **CustomView.axml** legt der Hintergrund hellgrün, aber es bedeutet auch, dass es gibt keinen visuellen Indikator, wenn die Zeile verwendet wird:

```xml
android:background="#FFDAFF7F"
```

Um das Verhalten für die Hervorhebung wieder aktivieren und auch die Farbe Anpassung, die verwendet wird, legen Sie stattdessen das Background-Attribut auf eine benutzerdefinierte Auswahl. Die Auswahl wird sowohl die Standardhintergrundfarbe als auch die Hervorhebungsfarbe deklariert werden. Die Datei **Resources/Drawable/CustomSelector.xml** enthält die folgende Deklaration:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

Um die benutzerdefinierte Auswahl verweisen möchten, ändern Sie das Background-Attribut im **CustomView.axml** an:

```xml
android:background="@drawable/CustomSelector"
```

Eine ausgewählte Zeile und die entsprechende `Toast` Nachricht sieht wie folgt:

[![Eine ausgewählte Zeile in Orange ändert, Toast-Nachricht, die Namen der ausgewählten Zeile anzeigen](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>Verhindern von Flackerns in benutzerdefinierten Layouts

Android versucht wird, um die Leistung zu verbessern `ListView` Durchführen eines Bildlaufs durch Zwischenspeichern Layoutinformationen. Bildlauf lange Listen mit Daten haben sollten Sie auch Festlegen der der `android:cacheColorHint` Eigenschaft auf die `ListView` Deklaration in der die Aktivität AXML Definition (mit denselben Farbwert als Ihre benutzerdefinierten Zeilenlayout Hintergrund). Dieser Hinweis enthalten, kann dies zu "Flimmern" als der Benutzer einen Bildlauf durch eine Liste mit benutzerdefinierten Zeilenhintergrundfarben kommen.



## <a name="related-links"></a>Verwandte Links

- [BuiltInViews (Beispiel)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (Beispiel)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (Beispiel)](https://developer.xamarin.com/samples/CustomRowView/)
