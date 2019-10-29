---
title: Anpassen einer ListView-Darstellung
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 48b23a1dce66f13efd3ad598cd61684e64e2b03c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028904"
---
# <a name="customizing-a-listviews-appearance-with-xamarinandroid"></a>Anpassen der Darstellung eines ListView-Steuermoduls mit xamarin. Android

Das Aussehen einer ListView wird durch das Layout der Zeilen vorgegeben, die angezeigt werden. Um die Darstellung einer `ListView`zu ändern, verwenden Sie ein anderes Zeilen Layout.

## <a name="built-in-row-views"></a>Integrierte Zeilen Sichten

Es gibt zwölf integrierte Sichten, auf die mithilfe von **Android. Resource. Layout**verwiesen werden kann:

- **Testlistitem** &ndash; einzelne Textzeile mit minimaler Formatierung.

- **SimpleListItem1** &ndash; einzelne Textzeile.

- **SimpleListItem2** &ndash; zwei Textzeilen.

- **Simpleselectablelistitem** &ndash; einzelne Textzeile, die die Auswahl einzelner oder mehrerer Elemente unterstützt (auf API-Ebene 11 hinzugefügt).

- **SimpleListItemActivated1** &ndash; ähnlich wie SimpleListItem1, aber die Hintergrundfarbe gibt an, wenn eine Zeile ausgewählt wird (auf API-Ebene 11 hinzugefügt).

- **SimpleListItemActivated2** &ndash; ähnlich wie SimpleListItem2, aber die Hintergrundfarbe gibt an, wenn eine Zeile ausgewählt wird (auf API-Ebene 11 hinzugefügt).

- **Simplelistitemcheck** &ndash; zeigt Häkchen an, um die Auswahl anzugeben.

- **Simplelistitemmultiplechoice** &ndash; zeigt Kontrollkästchen an, um die Auswahl mehrerer Auswahl anzuzeigen.

- **Simplelistitemsinglechoice** &ndash; zeigt Options Felder an, um sich gegenseitig ausschließende Auswahl anzuzeigen.

- **Twolinelistitem** &ndash; zwei Textzeilen.

- **Activitylistitem** &ndash; einzelne Textzeile mit einem Bild.

- **Simpleexpandablelistitem** &ndash; gruppiert Zeilen nach Kategorien, und jede Gruppe kann erweitert oder reduziert werden.

Jeder integrierten Zeilen Ansicht ist ein integrierter Stil zugeordnet. Diese Screenshots zeigen, wie jede Ansicht angezeigt wird:

[![Screenshots von testlistitem, simpleselectablelistitem, SimpleListitem1 und SimpleListItem2](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![Screenshots von SimpleListItemActivated1, SimpleListItemActivated2, simplelistitemcheck und simplelistitemmultiplecheck](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![Screenshots von simplelistitemsinglechoice, twolinelistitem, activitylistitem und simpleexpandablelistitem](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

Die Beispieldatei " **builtinviews/homescreenadapter. cs** " (in der Projekt Mappe " **builtinviews** ") enthält den Code, mit dem die nicht erweiterbaren Listenelement Bildschirme erzeugt werden. Die Sicht wird in der `GetView`-Methode wie folgt festgelegt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

Die Eigenschaften der Ansicht können dann festgelegt werden, indem Sie auf die Standard Steuerelement-IDs `Text1``Text2` und `Icon` unter `Android.Resource.Id` (legen Sie keine Eigenschaften fest, die die Sicht nicht enthält, oder es wird eine Ausnahme ausgelöst):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

Die " **builtinexpandableviews/expandablescreenadapter. cs"-** Beispieldatei (in der Projekt Mappe " **builtinviews** ") enthält den Code zum Erstellen des Bildschirms "simpleexpandablelistitem". Die Gruppenansicht wird in der `GetGroupView`-Methode wie folgt festgelegt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

Die untergeordnete Ansicht wird in der `GetChildView`-Methode wie folgt festgelegt:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

Die Eigenschaften für die Gruppenansicht und die untergeordnete Ansicht können dann festgelegt werden, indem Sie auf die Standard `Text1` und `Text2` Steuerelement Bezeichner verweisen, wie oben gezeigt. Der simpleexpandablelistitem-Bildschirmfoto (siehe oben) enthält ein Beispiel für eine einzeilige Gruppenansicht (SimpleExpandableListItem1) und eine untergeordnete zweizeilige Ansicht (SimpleExpandableListItem2). Alternativ kann die Gruppenansicht für zwei Zeilen (SimpleExpandableListItem2) konfiguriert werden, und die untergeordnete Ansicht kann für eine Zeile (SimpleExpandableListItem1) konfiguriert werden, oder sowohl die Gruppenansicht als auch die untergeordnete Ansicht können die gleiche Anzahl von Zeilen aufweisen. 

## <a name="accessories"></a>Accessories

Für Zeilen können Zubehör rechts neben der Ansicht hinzugefügt werden, um den Auswahl Status anzugeben:

- **Simplelistitemcheck** &ndash; erstellt eine Auswahlliste mit einer Auswahl als Indikator.

- **Simplelistitemsinglechoice** &ndash; eine Liste von Options Feldern erstellt, in der nur eine Auswahl möglich ist.

- **Simplelistitemmultiplechoice** &ndash; erstellt Kontrollkästchen, bei denen mehrere Optionen möglich sind.

Die oben genannten Zubehör sind in der jeweiligen Reihenfolge in den folgenden Bildschirmen dargestellt:

[![Screenshots von simplelistitemcheck, simplelistitemsinglechoice und simplelistitemmultiplechoice mit Zubehör](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Um eines dieser Zubehör anzuzeigen, übergeben Sie die erforderliche layoutressourcenid an den Adapter, und legen Sie dann den Auswahl Status für die erforderlichen Zeilen manuell fest. Diese Codezeile zeigt, wie eine `Adapter` mit einem dieser Layouts erstellt und zugewiesen wird:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

Der `ListView` selbst unterstützt verschiedene Auswahl Modi, unabhängig davon, welches Zubehör angezeigt wird. Um Verwirrung zu vermeiden, verwenden Sie `Single` Auswahlmodus mit `SingleChoice` Zubehör und dem `Checked` oder `Multiple` Modus mit dem `MultipleChoice` Stil. Der Auswahlmodus wird durch die `ChoiceMode`-Eigenschaft des `ListView`gesteuert.

### <a name="handling-api-level"></a>Behandeln von API-Ebenen

In früheren Versionen von xamarin. Android wurden Enumerationen als ganzzahlige Eigenschaften implementiert. Die neueste Version hat ordnungsgemäße .net-Enumerationstypen eingeführt, die es Ihnen erleichtern, die möglichen Optionen zu ermitteln.

Je nachdem, welche API-Ebene Sie als Ziel haben, ist `ChoiceMode` entweder eine ganze Zahl oder eine Enumeration. Die Beispieldatei **accessoryviews/homescreen. cs** weist einen Block auf, der auskommentiert ist, wenn Sie auf die Lebkuchen-API abzielen möchten:

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

### <a name="selecting-items-programmatically"></a>Programm gesteuertes auswählen von Elementen

Manuelles Festlegen, welche Elemente "ausgewählt" sind, wird mit der `SetItemChecked`-Methode ausgeführt (Sie kann mehrmals für Mehrfachauswahl aufgerufen werden):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

Der Code muss auch die einfache Auswahl anders als bei Mehrfachauswahl erkennen. Verwenden Sie die Eigenschaft `CheckedItemPosition` Ganzzahl, um zu bestimmen, welche Zeile im `Single` Modus ausgewählt wurde:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

Um zu ermitteln, welche Zeilen im `Multiple` Modus ausgewählt wurden, müssen Sie die `CheckedItemPositions` `SparseBooleanArray`durchlaufen. Ein Array mit geringer Dichte ist wie ein Wörterbuch, das nur Einträge enthält, bei denen der Wert geändert wurde. Daher müssen Sie das gesamte Array durchlaufen, um nach `true` Werten zu suchen, um zu wissen, was in der Liste ausgewählt wurde, wie im folgenden Code Ausschnitt veranschaulicht. :

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

## <a name="creating-custom-row-layouts"></a>Erstellen von benutzerdefinierten Zeilen Layouts

Die vier integrierten Zeilen Sichten sind sehr einfach. Um komplexere Layouts (z. b. eine Liste von e-Mails, Tweets oder Kontaktinformationen) anzuzeigen, ist eine benutzerdefinierte Ansicht erforderlich. Benutzerdefinierte Ansichten werden im Allgemeinen als axml-Dateien im Verzeichnis " **Resources/Layout** " deklariert und dann mithilfe ihrer Ressourcen-ID von einem benutzerdefinierten Adapter geladen. Die Sicht kann eine beliebige Anzahl von Anzeige Klassen (z. b. Text views, imageviews und andere Steuerelemente) mit benutzerdefinierten Farben, Schriftarten und Layout enthalten.

Dieses Beispiel unterscheidet sich in vielerlei Hinsicht von den vorherigen Beispielen:

- Erbt von `Activity`, nicht `ListActivity`. Sie können Zeilen für beliebige `ListView` anpassen, andere Steuerelemente können jedoch auch in ein `Activity` Layout (z. b. eine Überschrift, Schaltflächen oder andere Elemente der Benutzeroberfläche) eingeschlossen werden. In diesem Beispiel wird eine Überschrift über der `ListView` hinzugefügt.

- Erfordert eine axml-Layoutdatei für den Bildschirm. in den vorherigen Beispielen erfordert die `ListActivity` keine Layoutdatei. Diese axml enthält eine `ListView`-Steuerelement Deklaration.

- Erfordert eine axml-Layoutdatei zum Rendering der einzelnen Zeilen. Diese axml-Datei enthält die Text-und Image-Steuerelemente mit benutzerdefinierten Schriftart-und Farbeinstellungen.

- Verwendet eine optionale benutzerdefinierte Selektor-XML-Datei, um die Darstellung der Zeile bei der Auswahl festzulegen.

- Die `Adapter`-Implementierung gibt ein benutzerdefiniertes Layout aus der `GetView` Überschreibung zurück.

- `ItemClick` müssen anders deklariert werden (ein Ereignishandler wird an `ListView.ItemClick` anstatt an eine über schreibende `OnListItemClick` in `ListActivity`) angefügt.

Diese Änderungen werden im folgenden ausführlich erläutert, beginnend mit dem Erstellen der Aktivitätsansicht und der benutzerdefinierten Zeilen Ansicht und dem anschließenden abdecken der Änderungen am Adapter und der Aktivität, um Sie zu erstellen.

### <a name="adding-a-listview-to-an-activity-layout"></a>Hinzufügen eines ListView zu einem Aktivitäts Layout

Da `HomeScreen` nicht mehr von erbt, `ListActivity` es keine Standardansicht hat, muss eine axml-Layoutdatei für die Ansicht des Homescreen erstellt werden. In diesem Beispiel enthält die Sicht eine Überschrift (unter Verwendung eines `TextView`) und eine `ListView`, um Daten anzuzeigen. Das Layout wird in der Datei " **Resources/Layout/homescreen. axml** " definiert, die hier gezeigt wird:

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

Der Vorteil der Verwendung einer `Activity` mit einem benutzerdefinierten Layout (anstelle eines `ListActivity`) liegt darin, dass es möglich ist, dem Bildschirm zusätzliche Steuerelemente hinzuzufügen, z. b. die Überschrift `TextView` in diesem Beispiel.

### <a name="creating-a-custom-row-layout"></a>Erstellen eines benutzerdefinierten Zeilen Layouts

Eine weitere axml-Layoutdatei ist erforderlich, um das benutzerdefinierte Layout für jede Zeile zu enthalten, die in der Listenansicht angezeigt wird. In diesem Beispiel enthält die Zeile einen grünen Hintergrund, einen braunen Text und ein rechts Bündiges Bild. Das Android XML-Markup zum Deklarieren dieses Layouts wird unter **Ressourcen/Layout/CustomView. axml**beschrieben:

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

Während ein benutzerdefiniertes Zeilen Layout viele verschiedene Steuerelemente enthalten kann, kann die Bild Laufleistung von komplexen Entwürfen und der Verwendung von Bildern beeinflusst werden (insbesondere, wenn Sie über das Netzwerk geladen werden müssen). Weitere Informationen zur Behandlung von Leistungsproblemen beim Scrollen finden Sie im Google-Artikel.

### <a name="referencing-a-custom-row-view"></a>Verweisen auf eine benutzerdefinierte Zeilen Ansicht

Die Implementierung des Beispiels für einen benutzerdefinierten Adapter befindet sich in `HomeScreenAdapter.cs`. Die Schlüsselmethode ist `GetView`, in der das benutzerdefinierte axml mithilfe der Ressourcen-ID `Resource.Layout.CustomView`geladen wird. Anschließend werden Eigenschaften für jedes der Steuerelemente in der Ansicht festgelegt, bevor Sie zurückgegeben werden. Die gesamte Adapter Klasse wird angezeigt:

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

### <a name="referencing-the-custom-listview-in-the-activity"></a>Verweisen auf die benutzerdefinierte ListView in der Aktivität

Da die `HomeScreen`-Klasse nun von `Activity`erbt, wird ein `ListView` Feld in der-Klasse deklariert, das einen Verweis auf das in axml deklarierte Steuerelement enthalten soll:

```csharp
ListView listView;
```

Die Klasse muss dann das benutzerdefinierte Layout axml der Aktivität mithilfe der `SetContentView`-Methode laden. Anschließend kann das `ListView`-Steuerelement im Layout gefunden werden, dann wird der Adapter erstellt und zugewiesen und der Click-Handler zugewiesen. Der Code für die OnCreate-Methode wird hier angezeigt:

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

Schließlich muss der `ItemClick` Handler definiert werden. in diesem Fall wird nur eine `Toast` Meldung angezeigt:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

Der resultierende Bildschirm sieht wie folgt aus:

[![Screenshot der sich ergebenden customrowview](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)

### <a name="customizing-the-row-selector-color"></a>Anpassen der Zeilenauswahl Farbe

Wenn eine Zeile berührt wird, sollte Sie für Benutzer Feedback hervorgehoben werden. Wenn eine benutzerdefinierte Ansicht als Hintergrundfarbe angibt, wie **CustomView. axml** dies tut, überschreibt Sie auch die Auswahl Markierung. Mit dieser Codezeile in " **CustomView. axml** " wird der Hintergrund auf Hellgrün festgelegt, aber es bedeutet auch, dass es keinen visuellen Indikator gibt, wenn die Zeile berührt wird:

```xml
android:background="#FFDAFF7F"
```

Zum erneuten Aktivieren des Hervorhebungs Verhaltens und zum Anpassen der verwendeten Farbe legen Sie stattdessen das Background-Attribut auf einen benutzerdefinierten Selektor fest. Der Selektor deklariert sowohl die Standard Hintergrundfarbe als auch die Hervorhebungs Farbe. Die Datei **Resources/drawable/customselector. XML** enthält die folgende Deklaration:

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

Um auf den benutzerdefinierten Selektor zu verweisen, ändern Sie das Background-Attribut in **CustomView. axml** in:

```xml
android:background="@drawable/CustomSelector"
```

Eine ausgewählte Zeile und die entsprechende `Toast` Nachricht sehen jetzt wie folgt aus:

[Wählen Sie eine ausgewählte Zeile in Orange aus, wobei die Popup Meldung den Namen der ausgewählten Zeile anzeigt.![](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)

### <a name="preventing-flickering-on-custom-layouts"></a>Verhindern von Flimmern bei benutzerdefinierten Layouts

Android versucht, die Leistung von `ListView` Bildlauf zu verbessern, indem Layoutinformationen zwischengespeichert werden Wenn Sie über lange Bild Lauf Listen mit Daten verfügen, sollten Sie auch die `android:cacheColorHint`-Eigenschaft für die `ListView` Deklaration in der axml-Definition der Aktivität (auf denselben Farbwert wie für den Hintergrund des benutzerdefinierten Zeilen Layouts) festlegen. Wenn Sie diesen Hinweis nicht einschließen, kann dies zu einem "Flimmern" führen, wenn der Benutzer durch eine Liste mit benutzerdefinierten Zeilen Hintergrundfarben Scrollen würde.

## <a name="related-links"></a>Verwandte Links

- [Builtinviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [Accessoryviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [Customrowview (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
