---
title: ListView mit Daten auffüllen
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 83b398faf4fd634b7d492d372524b5fdd00163d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="populating-a-listview-with-data"></a>ListView mit Daten auffüllen


## <a name="overview"></a>Übersicht

Zum Hinzufügen von Zeilen zu einer `ListView` müssen Sie das Layout und die implementieren hinzuzufügen ein `IListAdapter` mit Methoden, die `ListView` Aufrufe an sich selbst zu füllen. Android umfasst integrierte `ListActivity` und `ArrayAdapter` Klassen, die Sie verwenden können, ohne alle benutzerdefinierten Layout-XML oder den Code zu definieren. Die `ListActivity` Klasse erstellt automatisch eine `ListView` offen gelegt eine `ListAdapter` Eigenschaft der Zeilenansichten anzuzeigenden über einen Adapter angeben.

Die integrierten Adapter nehmen eine Ansicht Ressourcen-ID als Parameter, der für jede Zeile verwendet wird. Sie können integrierte Ressourcen, z. B. die `Android.Resource.Layout` Sie müssen also keine Schreiben eigener.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Verwenden von ListActivity und ArrayAdapter&lt;Zeichenfolge&gt;

Im Beispiel **BasicTable/HomeScreen.cs** veranschaulicht, wie diese Klassen zum Anzeigen einer `ListView` in nur wenigen Codezeilen:

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


### <a name="handling-row-clicks"></a>Behandlung von Zeile klickt

In der Regel eine `ListView` auch ermöglicht dem Benutzer, eine Zeile zum Durchführen von Maßnahmen (z. B. Wiedergabe eines Musiktitels Aufrufen eines Kontakts oder einem anderen Bildschirm anzeigen) zu berühren. So reagieren Sie auf Benutzer Fingereingaben es eine muss weitere Methode implementiert, der `ListActivity` &ndash; `OnListItemClick` &ndash; wie folgt:

[![Screenshot des eine SimpleListItem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Nachdem der Benutzer eine Zeile berühren kann und eine `Toast` Warnung wird angezeigt:

[![Screenshot der Toast, die angezeigt wird, wenn eine Zeile verwendet wird](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>Implementieren eine ListAdapter

`ArrayAdapter<string>` eignet sich hervorragend aufgrund seiner Einfachheit, aber sie sehr eingeschränkt. Häufig müssen Sie jedoch eine Auflistung von Geschäftseinheiten, statt nur Zeichenfolgen, die Sie binden möchten.
Z. B. wenn eine Auflistung von Klassen für Mitarbeiter Ihre Daten besteht, empfiehlt klicken Sie dann die Liste, um nur die Namen der einzelnen Mitarbeiter anzuzeigen. Zum Anpassen des Verhaltens von einem `ListView` steuern, welche Daten angezeigt werden, müssen Sie eine Unterklasse von implementieren `BaseAdapter` überschreiben die folgenden vier Elemente:

-   **Anzahl** &ndash; zu dem Steuerelement erkennen, wie viele Zeilen in den Daten.

-   **GetView** &ndash; zurückzugebenden eine Ansicht für jede Zeile mit Daten aufgefüllt.
    Diese Methode verfügt über einen Parameter für die `ListView` in einer vorhandenen, nicht verwendeten Zeile für die erneute Verwendung zu übergeben.

-   **GetItemId** &ndash; eine Zeilen-ID zurückgeben (in der Regel die Zeile Zahl ist, obwohl es long-Wert sein kann, die Ihnen gefällt).

-   **Diese [Int]** Indexer &ndash; um eine bestimmte Zeilennummer zugeordneten Daten zurückzugeben.

Der Beispielcode in **BasicTableAdapter/HomeScreenAdapter.cs** veranschaulicht, wie Sie Unterklasse `BaseAdapter`:

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

Verwenden des benutzerdefinierten Adapters ähnelt einem der integrierten `ArrayAdapter`, und übergeben Sie eine `context` und die `string[]` der Werte angezeigt:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Da in diesem Beispiel wird die gleiche Zeilenlayout verwendet (`SimpleListItem1`) sieht die resultierende Anwendung mit dem vorherigen Beispiel identisch.


### <a name="row-view-re-use"></a>Zeile Sicht erneut verwenden

In diesem Beispiel sind nur sechs Elemente. Seit der Bildschirm acht anpassen kann, benötigt keine Zeile erneut verwenden. Anzeigen von Hunderten oder Tausenden von Zeilen jedoch wäre es ein Abfall des Arbeitsspeichers zum Erstellen von Hunderten oder Tausenden von `View` Objekte beim nur acht passen, auf dem Bildschirm zu einem Zeitpunkt. Um diese Situation zu vermeiden, wenn eine Zeile aus dem Bildschirm ausgeblendet, die eine Sicht in einer Warteschlange zur erneuten Verwendung platziert wird. Wenn der Benutzer einen Bildlauf durchführt, die `ListView` Aufrufe `GetView` zum Anfordern von neuer Ansichten angezeigt &ndash; Wenn verfügbaren eine nicht verwendete Sicht in übergibt die `convertView` Parameter. Wenn dieser Wert null ist, und klicken Sie dann der Code eine neue Instanz der Ansicht erstellen soll, können andernfalls erneut legen Sie die Eigenschaften dieses Objekts und verwenden Sie es erneut.

Die `GetView` Methode sollte dieses Muster folgen, um Zeilenansichten erneut verwenden:

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

Benutzerdefinierte adapterimplementierungen sollten *immer* Wiederverwenden der `convertView` Objekt vor dem Erstellen neuer Ansichten, um sicherzustellen, dass sie nicht ausführen, nicht genügend Arbeitsspeicher. wenn lange Listen anzeigen.

Einige adapterimplementierungen (z. B. die `CursorAdapter`) keine `GetView` -Methode, stattdessen benötigen sie zwei verschiedene Installationsmethoden `NewView` und `BindView` die erzwingen Zeile erneut verwenden, durch die Trennung von Aufgaben der `GetView` in zwei Methoden. Es ist ein `CursorAdapter` Beispiel weiter unten in das Dokument.


## <a name="enabling-fast-scrolling"></a>Aktivieren die schnelle Durchführen eines Bildlaufs

Schnelle Bildlauf hilft dem Benutzer in lange Listen einen Bildlauf durch die Bereitstellung eines zusätzlichen "Handles", das als eine Bildlaufleiste, um einen Teil der Liste der direkten Zugriff auf fungiert. Diese bildschirmabbildung zeigt das schnelle Scroll-Handle an:

[![Screenshot der Fast mit einem Bildlauf Handle Bildlauf](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Verursacht das schnelle Durchführen eines Bildlaufs Handle angezeigt werden, ist so einfach wie das Festlegen der `FastScrollEnabled` Eigenschaft `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>Hinzufügen eines Indexes Abschnitt

Bietet eine Abschnittsindex zusätzliches Feedback für Benutzer, Fast Bildlauf in einer langen Liste werden &ndash; es wird gezeigt, welche sie zum Bildlauf haben Abschnitt. Die dazu führen, dass die Abschnittsindex angezeigt werden, muss die Adapter-Unterklasse implementieren die `ISectionIndexer` Schnittstelle, geben Sie den Text je nach den Zeilen angezeigt werden:

[![Screenshot der H angezeigt wird, klicken Sie oben im Abschnitt beginnt, die mit H](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Implementiert `ISectionIndexer` müssen Sie drei Methoden zu einem Adapter hinzufügen:

-   **GetSections** &ndash; enthält eine vollständige Liste des Abschnitts Index Softwaretitel an, die angezeigt werden können. Diese Methode erfordert ein Array von Java-Objekten, daher muss der Code zum Erstellen einer `Java.Lang.Object[]` aus einer Auflistung .NET. In unserem Beispiel gibt es eine Liste der anfänglichen Zeichen in der Liste als `Java.Lang.String` .

-   **GetPositionForSection** &ndash; gibt die erste Zeilenposition für einen bestimmten Abschnittsindex zurück.

-   **GetSectionForPosition** &ndash; gibt den Abschnittsindex im für eine bestimmte Zeile angezeigt werden soll.


Im Beispiel `SectionIndex/HomeScreenAdapter.cs` Datei implementiert die Methoden und zusätzlicher Code im Konstruktor. Der Konstruktor erstellt die Abschnittsindex durchläuft jede Zeile, und extrahieren das erste Zeichen des Titels (die Elemente müssen dazu bereits sortiert werden.).

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

Ihr Index Abschnittstiteln müssen nicht Ihre tatsächliche Abschnitte 1:1 zugeordnet. Dies ist deshalb die `GetPositionForSection` Methode vorhanden ist.
`GetPositionForSection` haben Sie die Möglichkeit, ordnen Sie alle Indizes sind in Ihrem Indexliste, um alle Abschnitte in der Listenansicht angezeigt werden. Z. B. müssen unter Umständen ein "Z" in Ihrem Index, aber Sie verfügen nicht über die einen im Tabellenabschnitt für alle Buchstaben, damit anstelle von "Z" Zuordnung 26, Zuordnung zu möglicherweise 25 oder 24 oder beliebige Abschnittsindex "Z" zugeordnet.



## <a name="related-links"></a>Verwandte Links

- [BasicTableAndroid (sample)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (Beispiel)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (Beispiel)](https://developer.xamarin.com/samples/FastScroll/)
