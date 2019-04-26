---
title: Verwenden die ListView in Xamarin.Android
description: ListView ist eine wichtige Benutzeroberflächenkomponente von Android-Anwendungen. Sie wird überall aus kurzen Listen von Menüoptionen zu lange Listen mit Kontakten oder Internet-Favoriten verwendet. Es bietet eine einfache Möglichkeit, eine bildlauffähigen Liste mit Zeilen vorhanden, die entweder werden mit einer integrierten Formatvorlage formatiert oder können umfassend angepasst werden.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: a30256722647bbea482970d0c4a751954810d99e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61170799"
---
# <a name="listview"></a>ListView

_ListView ist eine wichtige Benutzeroberflächenkomponente von Android-Anwendungen. Sie wird überall aus kurzen Listen von Menüoptionen zu lange Listen mit Kontakten oder Internet-Favoriten verwendet. Es bietet eine einfache Möglichkeit, eine bildlauffähigen Liste mit Zeilen vorhanden, die entweder werden mit einer integrierten Formatvorlage formatiert oder können umfassend angepasst werden._


## <a name="overview"></a>Übersicht

Listenansichten und Adapter sind in die grundlegenden Bausteine von Android-Anwendungen enthalten. Die `ListView` Klasse bietet eine flexible Möglichkeit zur Darstellung der Daten, ob es sich um eine kurze Menü oder einer langen scrollliste ist. Es bietet nützliche Funktionen wie schnell der Bildlauf, Indizes und einfach- oder Mehrfachauswahl können Sie Mobilgeräte Benutzeroberflächen für Ihre Anwendungen zu erstellen. Eine `ListView`-Instanz erfordert einen *Adapter*, um sie mit in Zeilenansichten enthaltenen Daten zu füllen.

Dieses Handbuch wird erläutert, wie implementieren `ListView` und die verschiedenen `Adapter` Klassen in Xamarin.Android. Außerdem wird veranschaulicht, wie die Darstellung anpassen einer `ListView`, und erläutert die Bedeutung der Zeile erneut zu verwenden, um die arbeitsspeichernutzung zu verringern. Es gibt auch eine Erläuterung der Auswirkungen der Aktivitätslebenszyklus `ListView` und `Adapter` verwenden. Wenn Sie plattformübergreifende Anwendungen mit Xamarin.iOS, arbeiten die `ListView` Steuerelement ist ähnlich strukturiert wie für den iOS- `UITableView` (und die Android `Adapter` ist vergleichbar mit der `UITableViewSource`).

Zuerst ein kurzes Lernprogramm führt die `ListView` anhand eines Beispiels für die basic-Code. Als Nächstes werden Links zu Themen für fortgeschrittenere Benutzer bereitgestellt, um Informationen zur Nutzung von `ListView` in WPF-Anwendungen.


> [!NOTE]
> Die `RecyclerView` Widget ist ein flexibler und besser erweiterte Version des `ListView`. Da `RecyclerView` fungiert als der Nachfolger von `ListView` (und `GridView`), es wird empfohlen, die Sie verwenden `RecyclerView` statt `ListView` für die neue app-Entwicklung. Weitere Informationen finden Sie unter [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).



## <a name="listview-tutorial"></a>ListView-Tutorial

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) is a [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
eine Liste von bildlauffähigen Elementen erstellt. Listenelemente werden automatisch eingefügt, um die Liste mit einem [ `IListAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.IListAdapter/).

In diesem Tutorial erstellen Sie eine Übersicht der Ländernamen, die aus einem Zeichenfolgenarray gelesen werden. Wenn ein Listenelement ausgewählt ist, wird eine Popup-Nachricht die Position des Elements in der Liste angezeigt.


Starten Sie ein neues Projekt namens **HelloListView**.

Erstellen Sie eine XML-Datei mit dem Namen **list_item.xml** und speichern Sie ihn in das **Ressourcen/Layout/** Ordner. Fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Diese Datei definiert das Layout für jedes Element, das in platziert werden, wird die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Open `MainActivity.cs` und ändern Sie die Klasse erweitern [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/) (anstelle von [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)):

```csharp
public class MainActivity : ListActivity
{
```

Fügen Sie den folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
Methode:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

Beachten Sie, dass dies keine Layoutdatei für die Aktivität geladen wird (Dies geschieht in der Regel mit [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))).
Festlegen von stattdessen die [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
Eigenschaft fügt automatisch eine [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
Um von den gesamten Bildschirm einnimmt der [ `ListActivity` ](https://developer.xamarin.com/api/type/Android.App.ListActivity/).
Diese Methode akzeptiert eine [ `ArrayAdapter<T>` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), verwaltet das Array der Listenelemente, die in gespeichert werden die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).
Die [`ArrayAdapter<T>`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/)
Konstruktor akzeptiert die Anwendung [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/), die Layout-Beschreibung für jedes Listenelement (im vorherigen Schritt erstellt), und ein `T[]` oder [`Java.Util.IList<T>`](https://developer.xamarin.com/api/type/Java.Util.IList/)
Array von Objekten zum Einfügen in die [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
(als Nächstes definiert).

Die [`TextFilterEnabled`](https://developer.xamarin.com/api/property/Android.Widget.AbsListView.TextFilterEnabled/)
textfilterung für die Eigenschaft aktiviert die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/), sodass Wenn der Benutzer beginnt, eingeben, die Liste gefiltert werden.

Die [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
Ereignis kann verwendet werden, Handler für Mausklicks zu abonnieren. Wenn ein Element in der [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
ist geklickt haben, wird der Handler aufgerufen und ein [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
Meldung wird angezeigt, mit dem Text aus dem Element geklickt wurde.

Liste Element Entwürfe, die von der Plattform, anstatt Ihre eigenen Layoutdatei für bereitgestellte können die [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).
Beispielsweise verwenden Sie `Android.Resource.Layout.SimpleListItem1` anstelle von `Resource.Layout.list_item`.

Fügen Sie die folgenden `using` Anweisung:

```csharp
using System;
```
Fügen Sie die folgenden Zeichenfolgen-Array als Mitglied der `MainActivity`:

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

Dies ist das Array von Zeichenfolgen, die in gespeichert werden die [ `ListView` ](https://developer.xamarin.com/api/type/Android.Widget.ListView/).

Führen Sie die Anwendung aus. Können die Liste Scrollen oder eingeben, um es zu filtern, und klicken Sie dann ein Element, um eine Meldung angezeigt. Folgendes sollte angezeigt werden:

[![Beispielscreenshot von ListView mit Ländernamen](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

Beachten Sie, dass mit einem hartcodierten Zeichenfolgen-Array nicht die bewährte Methode für den Entwurf. Einer wird in diesem Tutorial der Einfachheit halber verwendet, um zu veranschaulichen die [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
widget. Empfohlen wird, auf ein Zeichenfolgenarray, das von einer externen Ressource, z. B. mit definiert eine `string-array` Ressource in Ihrem Projekt **Resources/Values/Strings.xml** Datei. Zum Beispiel:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
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

Verwenden Sie diese Ressourcenzeichenfolgen für die [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter%601/), ersetzen Sie das ursprüngliche [`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)
die Zeile mit den folgenden:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```
Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Beispielscreenshot von ListView mit kleineren Liste von Namen](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)


## <a name="going-further-with-listview"></a>Weitergehende Möglichkeiten mit ListView

In die übrigen Themen (aus dem Link unten) nutzen, einen umfassenden Überblick über das Arbeiten mit der `ListView` -Klasse und die verschiedenen Typen von Sie mit der sie können Kartentypen. Die Struktur lautet wie folgt:

-   **Visuelle Darstellung** &ndash; Teile der `ListView` -Steuerelement und deren Funktionsweise.

-   **Klassen** &ndash; Übersicht über die Klassen, die zum Anzeigen einer `ListView`.

-   **Anzeigen von Daten in einer ListView** &ndash; wie eine einfache Liste mit Daten angezeigt, die zum Implementieren `ListView's` nützliche Funktionen, wie andere integrierte Zeile Layouts verwendet und wie Adaptern Arbeitsspeicher zu sparen, indem das Wiederverwenden von Zeilenansichten.

-   **Benutzerdefinierte Darstellung** &ndash; ändern die Darstellung der `ListView` mit benutzerdefinierten Layouts, Schriftarten und Farben.

-   **Mithilfe von SQLite** &ndash; Vorgehensweise beim Anzeigen von Daten aus einer SQLite-Datenbank mit einem `CursorAdapter`.

-   **Aktivitätslebenszyklus** &ndash; Überlegungen zum Entwurf, bei der Implementierung `ListView` Aktivitäten, einschließlich, in dem im Lebenszyklus Sie Ihre Daten zu füllen soll, und wann Sie die Ressourcen freizugeben.

Die Diskussion (unterteilt in sechs Komponenten) beginnt mit einer Übersicht über die `ListView` -Klasse selbst vor der Einführung von zunehmend komplex Beispiele verwenden.

-   [ListView-Komponenten und -Funktionen](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
-   [Eine ListView mit Daten auffüllen](~/android/user-interface/layouts/list-view/populating.md)
-   [Anpassen einer ListView-Darstellung](~/android/user-interface/layouts/list-view/customizing-appearance.md)
-   [Verwenden von CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
-   [Verwenden von ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
-   [ListView und der Aktivitätslebenszyklus](~/android/user-interface/layouts/list-view/activity-lifecycle.md)


## <a name="summary"></a>Zusammenfassung

Diese Themensammlung eingeführt `ListView` und einige Beispiele zur Verwendung der integrierten Funktionen von der `ListActivity`. Es wurden benutzerdefinierte Implementierungen der behandelt `ListView` , die für die farbige Layouts zulässig und mithilfe einer SQLite-Datenbank und kurz auf die Relevanz der aktivitätslebenszyklus wurden Ihre `ListView` Implementierung.


## <a name="related-links"></a>Verwandte Links

- [AccessoryViews (Beispiel)](https://developer.xamarin.com/samples/AccessoryViews/)
- [BasicTableAndroid (Beispiel)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (Beispiel)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [BuiltInViews (Beispiel)](https://developer.xamarin.com/samples/BuiltInViews/)
- [CustomRowView (Beispiel)](https://developer.xamarin.com/samples/CustomRowView/)
- [FastScroll (Beispiel)](https://developer.xamarin.com/samples/FastScroll/)
- [SectionIndex (Beispiel)](https://developer.xamarin.com/samples/SectionIndex/)
- [SimpleCursorTableAdapter (sample)](https://developer.xamarin.com/samples/SimpleCursorTableAdapter/)
- [CursorTableAdapter (Beispiel)](https://developer.xamarin.com/samples/CursorTableAdapter/)
- [Aktivität-Lebenszyklus (Lernprogramm)](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Arbeiten mit Tabellen und Zellen (Xamarin.iOS)](~/ios/user-interface/controls/tables/index.md)
- [ListView-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [ListActivity-Klassenreferenz](https://developer.xamarin.com/api/type/Android.App.ListActivity/)
- [BaseAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
- [ArrayAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
- [CursorAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
