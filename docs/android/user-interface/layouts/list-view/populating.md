---
title: Auffüllen einer xamarin. Android-ListView mit Daten
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: fda021eb90feba1fed2352ef7f771f5583b00920
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028845"
---
# <a name="populating-a-xamarinandroid-listview-with-data"></a>Auffüllen einer xamarin. Android-ListView mit Daten

Wenn Sie einer `ListView` Zeilen hinzufügen möchten, müssen Sie Sie dem Layout hinzufügen und eine `IListAdapter` mit Methoden implementieren, die vom `ListView` aufgerufen werden, um sich selbst aufzufüllen. Android enthält integrierte `ListActivity` und `ArrayAdapter` Klassen, die Sie verwenden können, ohne benutzerdefinierte LayoutXml oder Code zu definieren. Die `ListActivity`-Klasse erstellt automatisch eine `ListView` und macht eine `ListAdapter`-Eigenschaft verfügbar, die die über einen Adapter anzuzeigenden Zeilen Ansichten bereitstellt.

Die integrierten Adapter übernehmen eine Ansichts Ressourcen-ID als Parameter, der für jede Zeile verwendet wird. Sie können integrierte Ressourcen wie z. b. in `Android.Resource.Layout` verwenden, sodass Sie keine eigenen schreiben müssen.

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Verwenden von listactivity und arrayadapter&lt;Zeichenfolge&gt;

Das Beispiel **basictable/homescreen. cs** veranschaulicht, wie diese Klassen verwendet werden, um eine `ListView` nur in wenigen Codezeilen anzuzeigen:

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

### <a name="handling-row-clicks"></a>Behandeln von Zeilen Klicks

In der Regel kann eine `ListView` auch eine Zeile berühren, um Aktionen auszuführen (z. b. das Abspielen eines Titels oder das Aufrufen eines Kontakts oder das Anzeigen eines anderen Bildschirms). Um auf die Benutzer zu reagieren, muss im `ListActivity` &ndash; eine weitere Methode implementiert werden, die wie folgt `OnListItemClick` &ndash;:

[![Screenshot eines simplelistitem](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

Nun kann der Benutzer eine Zeile berühren, und es wird eine `Toast` Warnung angezeigt:

[![Screenshot von Toast, der angezeigt wird, wenn eine Zeile berührt wird](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)

## <a name="implementing-a-listadapter"></a>Implementieren eines listadapter

`ArrayAdapter<string>` ist aufgrund seiner Einfachheit hervorragend, aber es ist äußerst eingeschränkt. Häufig verfügen Sie jedoch über eine Auflistung von Geschäfts Entitäten und nicht nur über Zeichen folgen, die Sie binden möchten.
Wenn die Daten z. b. aus einer Sammlung von Mitarbeiter Klassen bestehen, können Sie in der Liste nur die Namen der einzelnen Mitarbeiter anzeigen. Zum Anpassen des Verhaltens einer `ListView`, um zu steuern, welche Daten angezeigt werden, müssen Sie eine Unterklasse von implementieren `BaseAdapter` die die folgenden vier Elemente überschreiben:

- **Count** &ndash;, um dem Steuerelement mitzuteilen, wie viele Zeilen in den Daten vorliegen.

- **GetView** &ndash;, um eine Ansicht für jede Zeile zurückzugeben, die mit Daten aufgefüllt wird.
    Diese Methode verfügt über einen Parameter für die `ListView`, um eine vorhandene, nicht verwendete Zeile zur erneuten Verwendung zu übergeben.

- **GetItemID** &ndash; einen Zeilen Bezeichner zurückgeben (in der Regel die Zeilennummer, obwohl es sich um einen beliebigen langen Wert handeln kann, den Sie wünschen).

- **dieser [int]** -Indexer &ndash;, um die einer bestimmten Zeilennummer zugeordneten Daten zurückzugeben.

Der Beispielcode in **basictableadapter/homeskreenadapter. cs** veranschaulicht, wie `BaseAdapter`Unterklassen:

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

Die Verwendung des benutzerdefinierten Adapters ähnelt dem integrierten `ArrayAdapter`, wobei ein `context` und die `string[]` der anzuzeigenden Werte übergeben werden:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

Da in diesem Beispiel das gleiche Zeilen Layout (`SimpleListItem1`) verwendet wird, sieht die resultierende Anwendung mit dem vorherigen Beispiel identisch aus.

### <a name="row-view-re-use"></a>Erneute Verwendung der Zeilen Ansicht

In diesem Beispiel sind nur sechs Elemente vorhanden. Da der Bildschirm acht entsprechen kann, ist keine erneute Verwendung von Zeilen erforderlich. Beim Anzeigen von Hunderten oder Tausenden von Zeilen wäre es jedoch ein Speicher Verschwendung, Hunderte oder Tausende von `View` Objekten zu erstellen, wenn jeweils nur acht auf den Bildschirm passen. Um diese Situation zu vermeiden, wird die Ansicht, wenn eine Zeile auf dem Bildschirm ausgeblendet wird, zur erneuten Verwendung in eine Warteschlange eingefügt. Wenn der Benutzer einen Bildlauf ausführt, ruft der `ListView` `GetView` auf, um neue Ansichten anzufordern, die angezeigt werden &ndash; falls verfügbar, übergibt er eine nicht verwendete Ansicht im `convertView`-Parameter. Wenn dieser Wert NULL ist, sollte der Code eine neue Ansichts Instanz erstellen, andernfalls können Sie die Eigenschaften des Objekts neu festlegen und wieder verwenden.

Die `GetView`-Methode sollte diesem Muster folgen, um Zeilen Sichten erneut zu verwenden:

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

Benutzerdefinierte Adapter Implementierungen sollten das `convertView` Objekt *immer* wieder verwenden, bevor neue Sichten erstellt werden, um sicherzustellen, dass Sie nicht über genügend Arbeitsspeicher verfügen, wenn lange Listen angezeigt werden.

Einige Adapter Implementierungen (z. b. die `CursorAdapter`) verfügen über keine `GetView`-Methode, sondern erfordern zwei verschiedene Methoden `NewView` und `BindView`, die die erneute Verwendung von Zeilen erzwingen, indem die Zuständigkeiten von `GetView` in zwei Methoden aufgeteilt werden. Ein `CursorAdapter` Beispiel finden Sie später im Dokument.

## <a name="enabling-fast-scrolling"></a>Aktivieren von schnellem Scrollen

Das schnelle Scrollen unterstützt den Benutzer beim Durchführen eines Bildlaufs durch lange Listen, indem ein zusätzliches "handle" bereitgestellt wird, das als Bild Lauf Leiste fungiert, um direkt auf einen Teil der Liste zuzugreifen. Dieser Screenshot zeigt das fast-scrollhandle:

[![Screenshot mit einem scrollhandle](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

Das schnelle scrollhandle wird so einfach wie das Festlegen der `FastScrollEnabled`-Eigenschaft auf `true`angezeigt:

```csharp
ListView.FastScrollEnabled = true;
```

### <a name="adding-a-section-index"></a>Hinzufügen eines Abschnitts Indexes

Ein Abschnitts Index bietet Benutzern zusätzliches Feedback, wenn Sie schnell durch eine lange Liste scrollen &ndash; in dem angezeigt wird, zu welchem Abschnitt Sie einen Bildlauf durchgeführt haben. Damit der Abschnitts Index angezeigt wird, muss die Adapter Unterklasse die `ISectionIndexer`-Schnittstelle implementieren, um den Indextext abhängig von den angezeigten Zeilen bereitzustellen:

[![Screenshot von h, der oberhalb des Abschnitts erscheint, der mit H beginnt](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

Zum Implementieren von `ISectionIndexer` müssen Sie drei Methoden zu einem Adapter hinzufügen:

- **Getabschnitts** &ndash; bietet die komplette Liste der Abschnitts Index Titel, die angezeigt werden können. Diese Methode erfordert ein Array von Java-Objekten, damit der Code eine `Java.Lang.Object[]` aus einer .net-Auflistung erstellen muss. In unserem Beispiel wird eine Liste der anfänglichen Zeichen in der Liste als `Java.Lang.String` zurückgegeben.

- **Getpositionforsection** &ndash; gibt die erste Zeilen Position für einen angegebenen Abschnitts Index zurück.

- **Getsectionforposition** &ndash; gibt den Abschnitts Index zurück, der für eine bestimmte Zeile angezeigt werden soll.

Das Beispiel `SectionIndex/HomeScreenAdapter.cs` Datei implementiert diese Methoden und zusätzlichen Code im Konstruktor. Der-Konstruktor erstellt den Abschnitts Index, indem er jede Zeile durchläuft und das erste Zeichen des Titels extrahiert (die Elemente müssen bereits sortiert sein, damit dies funktioniert).

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

Wenn die Datenstrukturen erstellt wurden, sind die `ISectionIndexer` Methoden sehr einfach:

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

Ihre Abschnitts Index Titel müssen nicht 1:1 ihren eigentlichen Abschnitten zuordnen. Aus diesem Grund ist die `GetPositionForSection`-Methode vorhanden.
`GetPositionForSection` bietet Ihnen die Möglichkeit, alle Indizes, die in der Indexliste enthalten sind, den Abschnitten zuzuordnen, die in der Listenansicht aufgeführt sind. Angenommen, Sie haben ein "z" in Ihrem Index, Sie verfügen jedoch möglicherweise nicht über einen Tabellen Abschnitt für jeden Buchstaben. anstatt "z" zu 26 zuzuordnen, kann er beispielsweise 25 oder 24 oder den Abschnitts Index "z" zuordnen.

## <a name="related-links"></a>Verwandte Links

- [Basictableandroid (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [Basictableadapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [Fastscroll (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
