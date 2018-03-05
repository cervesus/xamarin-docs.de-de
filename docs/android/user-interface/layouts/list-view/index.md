---
title: ListView
description: "ListView ist eine wichtige Benutzeroberflächenkomponente von Android-Anwendungen. Es wird überall Short Listen von Menüoptionen zu lange Listen mit Kontakte oder Favoriten verwendet. Es bietet eine einfache Möglichkeit zum Präsentieren von einer bildlauffähigen Liste mit Zeilen, die kann entweder mit einer integrierten Formatvorlage formatiert oder umfassend angepasst werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 70a7abb186c102fb803c0ab6fa38c7b2d8222292
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="listview"></a>ListView

_ListView ist eine wichtige Benutzeroberflächenkomponente von Android-Anwendungen. Es wird überall Short Listen von Menüoptionen zu lange Listen mit Kontakte oder Favoriten verwendet. Es bietet eine einfache Möglichkeit zum Präsentieren von einer bildlauffähigen Liste mit Zeilen, die kann entweder mit einer integrierten Formatvorlage formatiert oder umfassend angepasst werden._

<a name="overview" />

## <a name="overview"></a>Übersicht

Listenansichten und Adapter werden in die grundlegenden Bausteine von Android-Anwendungen enthalten. Die `ListView` Klasse bietet eine flexible Möglichkeit zur Darstellung der Daten aus, ob es sich um eine kurze Menüs oder einer langen bildlauffähigen Liste ist. Es bietet nützliche Funktionen wie z. B. schnelles Durchführen eines Bildlaufs, Indizes und einfach- oder Mehrfachauswahl können Sie die mobilgerätefreundliche Benutzeroberflächen für Ihre Anwendungen zu erstellen. Eine `ListView`-Instanz erfordert einen *Adapter*, um sie mit in Zeilenansichten enthaltenen Daten zu füllen.

Diese Anleitung wird erläutert, wie implementieren `ListView` und die verschiedenen `Adapter` Klassen in Xamarin.Android. Außerdem wird veranschaulicht, wie die Darstellung anpassen einer `ListView`, und es wird erläutert, die Wichtigkeit der Zeile erneut zu verwenden, um die Auslastung von Speicher zu reduzieren. Es gibt auch einige Erläuterung der Auswirkungen der Aktivitätenlebenszyklus `ListView` und `Adapter` verwenden. Wenn Sie plattformübergreifende Anwendungen mit Xamarin.iOS, arbeiten die `ListView` Steuerelement ist für den iOS-strukturell ähnlicher `UITableView` (und das Android `Adapter` ähnelt der `UITableViewSource`).

Zuerst wird ein kurzes Lernprogramm führt die `ListView` mit einem Beispiel für basic-Code. Als Nächstes werden Verknüpfungen sowie erweiterte Themen bereitgestellt, zur Verwendung von `ListView` in realen apps.


> [!NOTE]
> **Hinweis**: die `RecyclerView` Widget ist eine erweiterte und flexible Version von `ListView`. Da `RecyclerView` soll der Nachfolger von `ListView` (und `GridView`), wir empfehlen die Verwendung `RecyclerView` statt `ListView` für die neue app-Entwicklung. Weitere Informationen finden Sie unter [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


<a name="tutorial" />

## <a name="listview-tutorial"></a>ListView-Lernprogramm

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) ist eine [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , der eine Liste von bildlauffähigen Elementen erstellt. Die Listenelemente werden automatisch eingefügt, um die Liste mit einem [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/).

In diesem Lernprogramm erstellen Sie eine Übersicht der Ländernamen, die aus einem Array von Zeichenfolgen gelesen werden. Wenn ein Listenelement ausgewählt ist, wird eine Meldung Toast die Position des Elements in der Liste angezeigt.


Starten Sie ein neues Projekt mit dem Namen **HelloListView**.

Erstellen Sie eine XML-Datei mit dem Namen **list_item.xml** und speichern Sie sie in der **Ressourcen/Layout/** Ordner. Fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Diese Datei definiert das Layout für jedes Element, die in platziert werden die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Öffnen der `HelloListView.cs` und machen die Klasse erweitern [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (anstelle von [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class HelloListView : ListActivity
{
```

Fügen Sie den folgenden Code für die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) Methode:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, ItemEventArgs args) {
        // When clicked, show a toast with the TextView text
        Toast.MakeText (Application, ((TextView)args.View).Text, ToastLength.Short).Show ();
    };
}
```

Beachten Sie, dass dies eine Layoutdatei für die Aktivität nicht geladen wird (Dies geschieht in der Regel mit [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
Stattdessen Festlegen der [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) Eigenschaft fügt automatisch eine [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) füllen Sie den gesamten Bildschirm der der [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/).
Diese Methode nimmt ein [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), verwaltet das Array von Elementen, die in platziert werden in der die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
Die [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/) Konstruktor akzeptiert die Anwendung [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), die Layout Beschreibung für jedes Listenelement (im vorherigen Schritt erstellt), und ein `T[]` oder [ `Java.Util.IList<T>` ](https://developer.xamarin.com/api/type/Java.Util.IList/) Array von Objekten zum Einfügen in die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) (definiert als Nächstes).

Die [ `TextFilterEnabled` ](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/) aktiviert Text Filterung für die Eigenschaft der [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), sodass, wenn der Benutzer beginnt, eingeben, wird die Liste gefiltert.

Die [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) -Ereignis kann zum Abonnieren der Handler für Klicks verwendet werden. Wenn ein Element in der [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) ist geklickt haben, wird der Handler aufgerufen und ein [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) Meldung angezeigt wird, verwenden den Text aus dem Element geklickt wurde.

Liste Element Entwürfe, die von der Plattform, anstatt eine eigene Layoutdatei für bereitgestellt können die [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).
Angenommen, versuchen Sie es mit `Android.Resource.Layout.SimpleListItem1` anstelle von `Resource.Layout.list_item`.

Nach der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) -Methode, fügen Sie das Zeichenfolgenarray hinzu:

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

Dies ist das Array von Zeichenfolgen, die in eingefügt werden, wird die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Führen Sie die Anwendung aus. Sie können einen Bildlauf die Liste aus, oder geben Sie zum Filtern, und klicken Sie auf ein Element, um eine Meldung angezeigt. Folgendes sollte angezeigt werden:

[ ![Beispiel-Screenshot des ListView mit Ländernamen](images/helloviews6.png)](images/helloviews6.png)

Beachten Sie, dass ein fest programmiertes Zeichenfolgenarray die bewährte Methode für den Entwurf ist es nicht verwenden. Einer wird in diesem Lernprogramm der Einfachheit halber verwendet, um zu veranschaulichen die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/) Widget. Die bessere Methode besteht darin, ein String-Array, das von einer externen Ressource, z. B. mit definiert verweisen eine `string-array` Ressourcen in Ihrem Projekt **Resources/Values/Strings.xml** Datei. Zum Beispiel:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="countries_array">
        <item>Bahrain</item>
        <item>Bangladesh</item>
        <item>Barbados</item>
        <item>Belarus</item>
        <item>Belgium</item>
        <item>Belize</item>
        <item>Benin</item>
    </string-array>
</resources>
```

Verwenden Sie diese Ressourcenzeichenfolgen für die [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), ersetzen Sie das ursprüngliche [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/) Zeile mit den folgenden:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

<a name="going_further" />

## <a name="going-further-with-listview"></a>Fortfahren mit ListView

(Aus dem Link unten) in die übrigen Themen ansehen umfassende arbeiten mit der `ListView` Klasse und die verschiedenen Typen von Adapter-Typen, die mit ihm können. Die Struktur lautet wie folgt:

-   **Visuelle Darstellung** &ndash; Bestandteile der `ListView` Steuerelement und deren Funktionsweise.

-   **Klassen** &ndash; Überblick über die Klassen, die zur Anzeige verwendet eine `ListView`.

-   **Anzeigen von Daten in einem ListView** &ndash; wie eine einfache Liste mit Daten angezeigt, wie Sie implementieren `ListView's` nützliche Funktionen wie Layouts für verschiedene integrierte Zeile; verwenden und wie Adaptern Speicher speichern, indem Sie erneut Zeilenansichten.

-   **Benutzerdefinierte Darstellung** &ndash; ändern den Stil der `ListView` mit benutzerdefinierten Layouts, Schriftarten und Farben.

-   **Mit SQLite** &ndash; Vorgehensweise beim Anzeigen von Daten aus einer SQLite-Datenbank mit einem `CursorAdapter`.

-   **Aktivitätenlebenszyklus** &ndash; Überlegungen zum Entwurf, bei der Implementierung `ListView` Aktivitäten, einschließlich, in dem im Lebenszyklus Sie Ihre Daten zu füllen soll, und wann Ressourcen freizugeben.

Die Diskussion (unterteilt sechs) beginnt mit einem Überblick über die `ListView` selbst vor der Einführung von progressiv komplexere Beispiele dafür, wie mit dieser Klasse.

-   [ListView-Komponenten und -Funktionen](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [ListView mit Daten auffüllen](~/android/user-interface/layouts/list-view/populating.md)
-   [Anpassen einer ListView-Darstellung](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [Verwenden von CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [Verwenden von ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView und der Aktivitätslebenszyklus](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

Diese Themensammlung eingeführt `ListView` und einige Beispiele zur Verwendung der integrierten Funktionen von der `ListActivity`. Sie benutzerdefinierte Implementierungen der besprochen `ListView` , die für die farbige Layouts zulässig und unter Verwendung einer SQLite-Datenbank und kurz auf die Relevanz der Aktivität Lebenszyklus berührt werden auf Ihre `ListView` Implementierung.


## <a name="related-links"></a>Verwandte Links

- [AccessoryViews (Beispiel)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (sample)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (Beispiel)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (Beispiel)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (Beispiel)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (Beispiel)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (Beispiel)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [Aktivität-Lebenszyklus (Lernprogramm)](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Arbeiten mit Tabellen und Zellen (im Xamarin.iOS)](~/ios/user-interface/controls/tables/index.md)
- [ListView-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity-Klassenreferenz](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
