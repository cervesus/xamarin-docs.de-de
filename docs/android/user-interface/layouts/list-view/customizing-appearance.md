---
title: Anpassen einer ListView-Darstellung
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: fef81fb5e5d2de79508b43a5612bf56af68d0772
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178434"
---
# <a name="customizing-a-listviews-appearance"></a>Anpassen einer ListView-Darstellung


## <a name="overview"></a>Übersicht

Durch das Layout der angezeigten Zeilen wird die Darstellung einer ListView festgelegt. So ändern Sie die Darstellung einer `ListView`, verwenden Sie eine andere Zeilenlayout.


## <a name="built-in-row-views"></a>Integrierte Zeilenansichten

Es gibt zwölf integrierte Sichten, die mit verwiesen werden können **Android.Resource.Layout**:

- **TestListItem** &ndash; Textzeile mit minimaler Formatierung.

- **SimpleListItem1** &ndash; einzelne Textzeile.

- **SimpleListItem2** &ndash; zwei Textzeilen.

- **SimpleSelectableListItem** &ndash; eine Textzeile, die von einem oder mehreren Elementauswahl (hinzugefügt in API-Ebene 11) unterstützt.

- **SimpleListItemActivated1** &ndash; SimpleListItem1 ähnlich, aber die Farbe des Hintergrunds gibt an, wenn eine Zeile ausgewählt ist (in der API-Ebene 11 hinzugefügt).

- **SimpleListItemActivated2** &ndash; SimpleListItem2 ähnlich, aber die Farbe des Hintergrunds gibt an, wenn eine Zeile ausgewählt ist (in der API-Ebene 11 hinzugefügt).

- **SimpleListItemChecked** &ndash; zeigt Häkchen Auswahl an.

- **SimpleListItemMultipleChoice** &ndash; zeigt Kontrollkästchen Multiple-Choice-Auswahl an.

- **SimpleListItemSingleChoice** &ndash; zeigt Optionsfelder gegenseitig Auswahl an.

- **TwoLineListItem** &ndash; zwei Textzeilen.

- **ActivityListItem** &ndash; eine Textzeile, die mit einem Bild.

- **SimpleExpandableListItem** &ndash; gruppiert Zeilen nach Kategorien, und jede Gruppe erweitert oder reduziert werden können.

Jede Zeilenansicht integrierten verfügt über einen integrierten Stil zugeordnet. Diese Screenshots zeigen, wie jeder Ansicht angezeigt wird:

[![Screenshots der TestListItem, SimpleSelectableListItem, SimpleListitem1 und SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Screenshots der SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked und SimpleListItemMultipleChecked](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Screenshots der SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem und SimpleExpandableListItem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

Die **BuiltInViews/HomeScreenAdapter.cs** -Beispieldatei (in der **BuiltInViews** Lösung) enthält den Code, um die nicht erweiterbaren Liste Element Bildschirme zu erstellen. Die Ansicht festgelegt ist, der `GetView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Die Eigenschaften der Sicht festgelegt werden, durch Verweisen auf die standard-Steuerelement-IDs `Text1`, `Text2` und `Icon` unter `Android.Resource.Id` (Eigenschaften, die die Sicht enthält keine oder eine Ausnahme ausgelöst wird nicht festgelegt):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

Die **BuiltInExpandableViews/ExpandableScreenAdapter.cs** -Beispieldatei (in der **BuiltInViews** Lösung) enthält den Code, um den Bildschirm SimpleExpandableListItem zu erzeugen. Die Ansicht "" festgelegt ist, der `GetGroupView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Die untergeordnete Ansicht festgelegt ist, der `GetChildView` Methode wie folgt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Die Eigenschaften für die Gruppe anzeigen und die untergeordnete Ansicht können dann durch Verweisen auf den Standard festgelegt werden `Text1` und `Text2` Bezeichner zu steuern, wie oben gezeigt. Screenshot SimpleExpandableListItem (siehe oben) enthält ein Beispiel für eine einzige Zeile Gruppenansicht (SimpleExpandableListItem1) und einer zweizeiligen untergeordnete Ansicht (SimpleExpandableListItem2). Alternativ können Sie die Ansicht "" kann für zwei Zeilen (SimpleExpandableListItem2) konfiguriert werden und die untergeordnete Ansicht kann für eine Zeile (SimpleExpandableListItem1), konfiguriert werden oder sowohl Gruppen-als Ansicht und untergeordnete Ansicht haben die gleiche Anzahl von Zeilen. 



## <a name="accessories"></a>Zubehör

Zeilen können Zubehör rechts von der Ansicht, um den Auswahlzustand anzugeben hinzugefügt haben:

- **SimpleListItemChecked** &ndash; erstellt eine Einfachauswahl-Liste mit einem Häkchen als Indikator.

- **SimpleListItemSingleChoice** &ndash; erstellt Funktyp-Schaltfläche enthält, in denen nur eine Auswahl möglich ist.

- **SimpleListItemMultipleChoice** &ndash; erstellt das Kontrollkästchen-Type-Listen, in denen mehrere Auswahlmöglichkeiten möglich sind.

Die oben genannten Zubehör sind in den folgenden Bildschirmen, in der entsprechenden Reihenfolge dargestellt:

[![Screenshots der SimpleListItemChecked SimpleListItemSingleChoice und SimpleListItemMultipleChoice mit Zubehör](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Zum Anzeigen eines dieser Zubehör übergeben die erforderlichen Layout-Ressourcen-ID an den Adapter klicken Sie dann manuell den Auswahlzustand festlegen für die erforderlichen Zeilen. Diese Codezeile veranschaulicht das Erstellen und Zuweisen einer `Adapter` mithilfe eines Layouts:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

Die `ListView` selbst unterstützt die unterschiedlichen Auswahlmodi, unabhängig von die Zugriffsmethode, die angezeigt wird. Um Verwirrung zu vermeiden, verwenden Sie `Single` Auswahlmodus mit `SingleChoice` Zubehör und `Checked` oder `Multiple` Modus mit der `MultipleChoice` Stil. Der Auswahlmodus wird gesteuert, indem die `ChoiceMode` Eigenschaft der `ListView`.


### <a name="handling-api-level"></a>Behandlung von API-Ebene

Enumerationen werden von frühere Versionen von Xamarin.Android als ganzzahligen Eigenschaften implementiert. Die neueste Version hat richtige Enumerationstypen in .NET eingeführt, wodurch sie viel einfacher, um die möglichen Optionen zu ermitteln.

Abhängig von der API-Ebene Sie verwenden möchten, `ChoiceMode` ist entweder eine ganze Zahl oder eine Enumeration. Die Beispieldatei **AccessoryViews/HomeScreen.cs** verfügt über ein Block auskommentiert werden, wenn Sie die Gingerbread-API als Ziel möchten:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>Programmgesteuertes auswählen von Elementen

Manuelles Festlegen der Elemente sind "ausgewählt" erfolgt mit der `SetItemChecked` Methode (er kann mehrmals aufgerufen werden für die Mehrfachauswahl):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Der Code muss auch einzelne Auswahl anders als mehrere Auswahlmöglichkeiten zu erkennen. Um zu bestimmen, welche Zeile ausgewählt wurde `Single` Modus verwenden die `CheckedItemPosition` -Zahl mit Vorzeichen:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Um zu bestimmen, welche Zeilen ausgewählt wurden `Multiple` Modus Sie durchlaufen müssen die `CheckedItemPositions` `SparseBooleanArray`. Ein Array mit geringer Dichte ist wie ein Wörterbuch, der nur die Einträge enthält, wurde der Wert geändert wurde, damit Sie das gesamte Array nach durchlaufen müssen `true` Werte wissen, was in der Liste ausgewählt wurde wie im folgenden Codeausschnitt gezeigt:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>Erstellen von benutzerdefinierten Zeile Layouts

Die vier integrierten Zeilenansichten sind sehr einfach. Zum Anzeigen von komplexere Layouts (z. B. eine Liste von e-Mails, Tweets oder Kontaktinformationen) ist eine benutzerdefinierte Ansicht erforderlich. Benutzerdefinierte Ansichten werden in der Regel als AXML-Dateien deklariert die **Ressourcen/Layout** Verzeichnis, und klicken Sie dann die mit den Ressourcen-Id eines benutzerdefinierten Adapters geladen. Die Sicht kann eine beliebige Anzahl von anzeigen-Klassen (z. B. TextViews, ImageViews und andere Steuerelemente) mit benutzerdefinierten Farben, Schriftarten und Layout enthalten.

In diesem Beispiel unterscheidet sich von den vorherigen Beispielen in eine Reihe von Möglichkeiten:

-  Erbt von `Activity` , nicht `ListActivity` . Sie können die Zeilen für alle anpassen `ListView` , jedoch auch andere Steuerelemente auch aufgenommen werden können, können Sie in einer `Activity` Layout (z. B. eine Überschrift, Schaltflächen oder andere Elemente der Benutzeroberfläche). In diesem Beispiel wird eine Überschrift oben die `ListView` um zu veranschaulichen.

-  Erfordert ein AXML-Layoutdatei für den Bildschirm an. in den vorherigen Beispielen die `ListActivity` eine Layoutdatei ist nicht erforderlich. Diese AXML enthält eine `ListView` Deklaration zu steuern.

-  Erfordert ein AXML-Layoutdatei um jede Zeile zu rendern. Diese AXML-Datei enthält die Text- und Image-Steuerelemente mit benutzerdefinierten Schriftart- und farbeinstellungen.

-  Verwendet eine optionale benutzerdefinierte Auswahl XML-Datei die Darstellung der Zeile festgelegt, wenn es ausgewählt wird.

-  Die `Adapter` Implementierung gibt ein benutzerdefiniertes Layout aus der `GetView` außer Kraft setzen.

-  `ItemClick` muss anders deklariert werden (ein Ereignishandler angefügt ist `ListView.ItemClick` statt einer überschreiben `OnListItemClick` in `ListActivity`).


Diese Änderungen werden unten genauer beschrieben erstellen, die Ansicht der Aktivität und die der benutzerdefinierten Zeilenansicht und deckt die klicken Sie dann die Änderungen an den Adapter und die Aktivität, um die Berichte Rendern ab.


### <a name="adding-a-listview-to-an-activity-layout"></a>Hinzufügen einer ListView auf einer Aktivitätsüberlagerung

Da `HomeScreen` erbt nicht mehr von `ListActivity` keine Standardansicht, damit eine AXML-Layoutdatei für den HomeScreen Ansicht erstellt werden muss. In diesem Beispiel wird die Ansicht eine Überschrift haben (mithilfe einer `TextView`) und ein `ListView` zum Anzeigen von Daten. Das Layout wird definiert, der **Resources/Layout/HomeScreen.axml** Datei, die hier gezeigt wird:

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

Der Vorteil der Verwendung einer `Activity` mit einem benutzerdefinierten Layout (anstelle von einem `ListActivity`) liegt in der Lage, fügen zusätzliche Steuerelemente auf dem Bildschirm z. B. die Überschrift `TextView` in diesem Beispiel.


### <a name="creating-a-custom-row-layout"></a>Erstellen eines benutzerdefinierten Zeile Layouts

Eine andere AXML-Layoutdatei ist erforderlich, das benutzerdefinierte Layout für jede Zeile enthält, die in der Listenansicht angezeigt wird. In diesem Beispiel wird die Zeile eine grünem Hintergrund, braunen Text und Bild rechts ausgerichtete verfügen. Das Android XML-Markup, deklarieren dieses Layout finden Sie im **Resources/Layout/CustomView.axml**:

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

Während einer benutzerdefinierten Zeilenlayout viele verschiedene Steuerelemente enthalten kann, die bildlaufleistung kann durch komplexe Entwürfe beeinflusst werden, und verwenden-images (insbesondere, wenn sie über das Netzwerk geladen werden müssen). Finden Sie im Artikel von Google Informationen auf Bildlauf Leistungsprobleme.


### <a name="referencing-a-custom-row-view"></a>Verweisen auf eine Zeile der benutzerdefinierten Ansicht

Die Implementierung des benutzerdefinierten Adapter Beispiel befindet sich in `HomeScreenAdapter.cs`. Die Schlüsselmethode ist `GetView` geladen, in dem die benutzerdefinierte AXML mithilfe der Ressourcen-ID `Resource.Layout.CustomView`, und legt dann die Eigenschaften für jedes der Steuerelemente in der Ansicht vor dem zurückgeben. Die vollständige Adapterklasse wird angezeigt:

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


### <a name="referencing-the-custom-listview-in-the-activity"></a>Verweisen auf die benutzerdefinierte ListView, in der Aktivität

Da die `HomeScreen` Klasse erbt nun von `Activity`, `ListView` Feld ist deklariert, in der Klasse für einen Verweis auf das Steuerelement in der AXML deklariert:

```csharp
ListView listView;
```

Die Klasse muss dann der Aktivität benutzerdefinierte Layout AXML Laden mithilfe der `SetContentView` Methode. Suchen sie Sie dann die `ListView` -Steuerelement in das Layout wird erstellt und weist den Adapter und weist Sie den Click-Handler. Der Code für die der OnCreate-Methode ist hier dargestellt:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Zum Schluss die `ItemClick` Handler muss definiert werden; in diesem Fall es zeigt nur eine `Toast` Nachricht:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Die angezeigten Bildschirm sieht folgendermaßen aus:

[![Screenshot der resultierenden CustomRowView](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>Die Farbe der Auswahl anpassen

Bei eine Zeile verwendet wird, sollten sie für Benutzerfeedback hervorgehoben werden. Wenn eine benutzerdefinierte Ansicht gibt als Hintergrundfarbe als **CustomView.axml** der Fall ist, außerdem überschreibt er die Markierung der. Diese Codezeile in **CustomView.axml** legt der Hintergrund hellgrün, aber es bedeutet auch, dass es gibt keinen visuellen Indikator, wenn die Zeile verwendet wird:

```xml
android:background="#FFDAFF7F"
```

So aktivieren Sie das Verhalten für die Hervorhebung erneut, und auch die Farbe anpassen, die verwendet wird, legen Sie stattdessen das Background-Attribut auf eine benutzerdefinierte aufweist. Die Auswahl wird sowohl die Standardhintergrundfarbe als auch die Hervorhebungsfarbe deklarieren. Die Datei **Resources/Drawable/CustomSelector.xml** enthält die folgende Deklaration:

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

Um die benutzerdefinierte Auswahl zu verweisen, ändern Sie das Background-Attribut in **CustomView.axml** auf:

```xml
android:background="@drawable/CustomSelector"
```

Eine ausgewählte Zeile und dem entsprechenden `Toast` Nachricht jetzt sieht wie folgt aus:

[![Eine ausgewählte Zeile in Orange mit eingeblendeten Nachricht, die Namen der ausgewählten Zeile anzeigen](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>Verhindern von Flimmern in benutzerdefinierten Layouts

Android versucht wird, zur Verbesserung der Leistung von `ListView` Durchführen eines Bildlaufs durch Zwischenspeichern der Layoutinformationen. Wenn man einen Bildlauf lange Listen mit Daten sollten Sie auch Festlegen der `android:cacheColorHint` Eigenschaft für die `ListView` Deklaration in der Aktivität AXML-Definition (auf denselben Farbwert als Ihre benutzerdefinierten Zeilenlayout Hintergrund). Dieser Hinweis enthalten kann "Flimmern" als der Benutzer einen Bildlauf durch eine Liste mit benutzerdefinierten Zeilenhintergrundfarben führen.



## <a name="related-links"></a>Verwandte Links

- [BuiltInViews (Beispiel)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (Beispiel)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (Beispiel)](https://developer.xamarin.com/samples/CustomRowView/)
