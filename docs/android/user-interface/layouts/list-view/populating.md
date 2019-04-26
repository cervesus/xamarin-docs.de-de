---
title: Auffüllen von ListView mit Daten
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 57c69223a01074ed15714026b7e9ec4e995808e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61171181"
---
# <a name="populating-a-listview-with-data"></a>Auffüllen von ListView mit Daten


## <a name="overview"></a>Übersicht

Zum Hinzufügen von Zeilen, eine `ListView` müssen Sie es hinzufügen, um das Layout und implementieren eine `IListAdapter` mit Methoden, die die `ListView` Aufrufe auffüllen. Android umfasst `ListActivity` und `ArrayAdapter` Klassen, die Sie verwenden können, ohne die Definition jeder benutzerdefinierten Layout-XML oder Code. Die `ListActivity` Klasse erstellt automatisch eine `ListView` und verfügbar macht eine `ListAdapter` Eigenschaft, um die Zeilenansichten enthalten, die über einen Adapter anzeigen bereitzustellen.

Die integrierten Adapter werden eine Ansicht-Ressourcen-ID als ein Parameter, der für jede Zeile verwendet wird. Können Sie integrierte Ressourcen, z. B. in `Android.Resource.Layout` daher Sie nicht zum Schreiben eigener müssen.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Mithilfe von ListActivity und ArrayAdapter&lt;Zeichenfolge&gt;

Im Beispiel **BasicTable/HomeScreen.cs** wird veranschaulicht, wie diese Klassen verwenden, um die Anzeige einer `ListView` in nur wenigen Codezeilen:

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>Behandeln von Zeile klickt

In der Regel eine `ListView` kann auch der Benutzer Tippen Sie auf eine Zeile, um eine Aktion (z. B. Wiedergabe eines Musiktitels, Aufruf eines Kontakts oder einem anderen Bildschirm anzeigen) auszuführen. Reagieren auf Benutzer Workflows es eine muss weitere Methode implementiert, der `ListActivity` &ndash; `OnListItemClick` &ndash; wie folgt aus:

[![Screenshot des eine SimpleListItem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Nachdem der Benutzer eine Zeile aus ansprechen kann und eine `Toast` Warnung wird angezeigt:

[![Screenshot des Toasts, die angezeigt wird, wenn eine Zeile verwendet wird](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Implementieren einer ListAdapter

`ArrayAdapter<string>` eignet sich hervorragend aufgrund seiner Einfachheit, aber sie ist sehr begrenzt. Sie haben jedoch oft eine Auflistung von Entitäten, anstatt lediglich Zeichenfolgen, die Sie binden möchten.
Beispielsweise wenn Ihre Daten aus einer Auflistung von Klassen für Mitarbeiter, besteht, dann können Sie die Liste nur zeigen Sie die Namen der einzelnen Mitarbeiter an. Zum Anpassen des Verhaltens von einem `ListView` steuern, welche Daten angezeigt werden, müssen Sie eine Unterklasse von implementieren `BaseAdapter` überschreiben die folgenden vier Elemente:

-   **Anzahl** &ndash; des Steuerelements zu informieren, wie viele Zeilen in den Daten sind.

-   **GetView** &ndash; , eine Ansicht für jede Zeile zurückzugeben, die mit Daten aufgefüllt.
    Diese Methode hat einen Parameter für die `ListView` in einer vorhandenen, nicht verwendeten Zeile für die erneute Verwendung zu übergeben.

-   **GetItemId** &ndash; eine Zeilen-ID zurückgegeben (in der Regel die Zeile Zahl ist, obwohl es long-Wert sein kann, das Ihnen gefällt).

-   **Diese [Int]** Indexer &ndash; , die eine bestimmte Zeilennummer zugeordneten Daten zurück.

Der Beispielcode in **BasicTableAdapter/HomeScreenAdapter.cs** wird veranschaulicht, wie Sie die Unterklasse `BaseAdapter`:

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>Verwenden eines benutzerdefinierten Adapters

Mithilfe des benutzerdefinierten Adapters ähnelt einem der integrierten `ArrayAdapter`, und übergeben Sie einen `context` und `string[]` von Werten angezeigt:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Da in diesem Beispiel wird das Zeilenlayout der gleichen verwendet (`SimpleListItem1`) die Anwendung wird mit dem vorherigen Beispiel identisch aussehen.


### <a name="row-view-re-use"></a>Anzeigen von Zeile erneut verwenden

In diesem Beispiel sind nur sechs Elemente. Da der Bildschirm acht passen, erforderlich, keine Zeile erneut verwenden. Wenn Sie Hunderte oder Tausende von Zeilen anzeigen, jedoch wäre es eine arbeitsspeicherverschwendung, Hunderte oder Tausende von erstellen `View` Objekte beim nur acht passt, auf dem Bildschirm zu einem Zeitpunkt. Um diese Situation zu vermeiden, wenn eine Zeile vom Bildschirm verschwindet, die in einer Warteschlange für die Wiederverwendung platziert wird. Wenn der Benutzer einen Bildlauf durchführt, die `ListView` Aufrufe `GetView` , neue anzuzeigenden Ansichten anzufordern &ndash; wenn verfügbar in eine nicht verwendete Ansicht übergibt die `convertView` Parameter. Wenn dieser Wert null ist, und klicken Sie dann Ihren Code eine neue Ansichtsinstanz erstellen soll, können andernfalls erneut Festlegen der Eigenschaften dieses Objekts und verwenden Sie es erneut.

Die `GetView` Methode sollten dieses Muster zum Wiederverwenden von Zeilenansichten befolgen:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

Benutzerdefinierter Adapter-Implementierungen sollten *immer* Wiederverwenden der `convertView` Objekt vor dem Erstellen neuer Ansichten, um sicherzustellen, dass sie nicht ausführen, nicht genügend Arbeitsspeicher wenn lange Listen angezeigt.

Einige adapterimplementierungen (wie z. B. die `CursorAdapter`) kein `GetView` -Methode, stattdessen müssen sie zwei verschiedene Installationsmethoden `NewView` und `BindView` der erzwingen Zeile erneut zu verwenden, durch die Trennung der Zuständigkeiten der `GetView` in zwei -Methoden. Es gibt eine `CursorAdapter` Beispiel weiter unten in das Dokument.


## <a name="enabling-fast-scrolling"></a>Aktivieren der schnellen Scrollen

Schnelle Bildlauf der hilft Benutzern dabei, lange Listen einen Bildlauf durch die Bereitstellung eines zusätzlichen "Handles", das als eine Bildlaufleiste angezeigt, die einen Teil der Liste der direkten Zugriff auf fungiert. Dieser Screenshot zeigt das Handle des schnellen scrollen:

[![Screenshot der Fast elementscrolling mit einem Bildlauf-handle](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Verursacht das schnelle Bildlauf Handle angezeigt werden, ist so einfach wie das Festlegen der `FastScrollEnabled` Eigenschaft `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Hinzufügen eines Abschnitt-Indexes

Ein Abschnittsindex enthält zusätzliches Feedback für Benutzer schnell elementscrolling in einer langen Liste &ndash; welche "Section" sie haben auf gescrollt werden. Die dazu führen, dass die Abschnittsindex angezeigt werden, muss die Adapter-Unterklasse implementieren die `ISectionIndexer` Schnittstelle, geben Sie den Text Index je nach den Zeilen angezeigt werden:

[![Screenshot der H angezeigt wird, klicken Sie oben im Abschnitt beginnt, die mit H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Zum Implementieren `ISectionIndexer` müssen Sie drei Methoden, die einem Adapter hinzufügen:

-   **GetSections** &ndash; enthält eine vollständige Liste des Abschnitts Index Softwaretitel an, die angezeigt werden können. Diese Methode erfordert ein Array von Java-Objekte aus, daher muss der Code zum Erstellen einer `Java.Lang.Object[]` aus einer Auflistung von .NET. In diesem Beispiel wird eine Liste der dem ersten Zeichen in der Liste als `Java.Lang.String` .

-   **GetPositionForSection** &ndash; gibt die erste Zeilenposition für einen Index angegebenen Abschnitt.

-   **GetSectionForPosition** &ndash; gibt den Abschnittsindex im für eine bestimmte Zeile angezeigt werden soll.


Im Beispiel `SectionIndex/HomeScreenAdapter.cs` Datei implementiert die Methoden und zusätzlichen Code im Konstruktor. Der Konstruktor erstellt die Abschnittsindex durch jede Zeile durchlaufen, und extrahieren das erste Zeichen des Titels (die Elemente müssen bereits damit dies funktioniert sortiert werden).

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

Mit den Datenstrukturen erstellt die `ISectionIndexer` Methoden sind sehr einfach:

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

Ihr Index Abschnittstitel müssen nicht Ihre tatsächliche Abschnitte 1:1 zugeordnet. Deshalb ist die `GetPositionForSection` Methode vorhanden ist.
`GetPositionForSection` bietet Ihnen die Möglichkeit, ordnen alle Indizes werden in der Indexliste ", die alle Abschnitte in der Liste angezeigt werden. Z. B. ein "Z" in Ihrem Index enthalten, aber möglicherweise verfügen Sie keinen im Tabellenabschnitt für alle Buchstaben, damit anstelle von "Z" Zuordnung 26, Zuordnung zu möglicherweise 25 oder 24 oder beliebige Abschnittsindex "Z" zugeordnet.



## <a name="related-links"></a>Verwandte Links

- [BasicTableAndroid (Beispiel)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (Beispiel)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (Beispiel)](https://developer.xamarin.com/samples/FastScroll/)
