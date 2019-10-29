---
title: Verwenden von ListView in xamarin. Android
description: ListView ist eine wichtige Benutzeroberflächen Komponente von Android-Anwendungen. Sie wird überall von kurzen Listen mit Menü Optionen zu langen Listen von Kontakten oder Internet Favoriten verwendet. Es bietet eine einfache Möglichkeit, eine Bildlauf-Liste mit Zeilen darzustellen, die entweder mit einem integrierten Stil formatiert oder ausgiebig angepasst werden können.
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: f6579e3b70e3788046916db12e201550e7fd5f16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028890"
---
# <a name="xamarinandroid-listview"></a>Xamarin. Android-ListView

_ListView ist eine wichtige Benutzeroberflächen Komponente von Android-Anwendungen. Sie wird überall von kurzen Listen mit Menü Optionen zu langen Listen von Kontakten oder Internet Favoriten verwendet. Es bietet eine einfache Möglichkeit, eine Bildlauf-Liste mit Zeilen darzustellen, die entweder mit einem integrierten Stil formatiert oder ausgiebig angepasst werden können._

## <a name="overview"></a>Übersicht

Listenansichten und Adapter sind in den grundlegendsten Bausteine von Android-Anwendungen enthalten. Die `ListView`-Klasse bietet eine flexible Möglichkeit zum Darstellen von Daten, unabhängig davon, ob es sich um ein kurzes Menü oder eine lange scrollliste handelt. Es bietet Nutzbarkeits Features wie schnelles Scrollen, Indizes und eine einfache oder mehrfache Auswahl, die Sie beim Erstellen von benutzerfreundlichen Benutzeroberflächen für Ihre Anwendungen unterstützen. Eine `ListView`-Instanz erfordert einen *Adapter*, um sie mit in Zeilenansichten enthaltenen Daten zu füllen.

In diesem Handbuch wird erläutert, wie `ListView` und die verschiedenen `Adapter` Klassen in xamarin. Android implementiert werden. Außerdem wird veranschaulicht, wie die Darstellung eines `ListView`angepasst wird. Außerdem wird die Wichtigkeit der erneuten Verwendung von Zeilen erläutert, um den Speicherverbrauch zu verringern. Außerdem wird erläutert, wie sich der Aktivitäts Lebenszyklus auf `ListView` und `Adapter` Verwendung auswirkt. Wenn Sie mit xamarin. IOS an plattformübergreifenden Anwendungen arbeiten, ist das `ListView` Steuerelement strukturell ähnlich wie das IOS-`UITableView` (und das Android-`Adapter` ähnelt dem `UITableViewSource`).

Zuerst führt ein kurzes Tutorial den `ListView` mit einem einfachen Codebeispiel ein. Im nächsten Schritt werden Links zu erweiterten Themen bereitgestellt, die Ihnen bei der Verwendung von `ListView` in realen Apps helfen.

> [!NOTE]
> Das `RecyclerView`-Widget ist eine erweiterte und flexiblere Version von `ListView`. Da `RecyclerView` als Nachfolger für `ListView` (und `GridView`) entwickelt wurde, empfiehlt es sich, `RecyclerView` anstelle von `ListView` für die Entwicklung neuer apps zu verwenden. Weitere Informationen finden Sie unter [recyclerview](~/android/user-interface/layouts/recycler-view/index.md).

## <a name="listview-tutorial"></a>ListView-Tutorial

[`ListView`](xref:Android.Widget.ListView) ist ein [`ViewGroup`](xref:Android.Views.ViewGroup)
Dadurch wird eine Liste von scrollbaren Elementen erstellt. Die Listenelemente werden mithilfe eines [`IListAdapter`](xref:Android.Widget.IListAdapter)automatisch in die Liste eingefügt.

In diesem Tutorial erstellen Sie eine Bild lauffähigen Liste mit Ländernamen, die aus einem Zeichen folgen Array gelesen werden. Wenn ein Listenelement ausgewählt wird, wird in einer Popup Meldung die Position des Elements in der Liste angezeigt.

Starten Sie ein neues Projekt mit dem Namen **hellolistview**.

Erstellen Sie eine XML-Datei mit dem Namen **list_item. XML** , und speichern Sie Sie im **Ressourcen/Layout/** Ordner. Fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

Diese Datei definiert das Layout für jedes Element, das in der [`ListView`](xref:Android.Widget.ListView)platziert wird.

Öffnen Sie `MainActivity.cs`, und ändern Sie die-Klasse, um [`ListActivity`](xref:Android.App.ListActivity) (anstelle von [`Activity`](xref:Android.App.Activity)) zu erweitern:

```csharp
public class MainActivity : ListActivity
{
```

Fügen Sie den folgenden Code für die-Methode [`OnCreate()`](xref:Android.App.Activity.OnCreate*)) ein:

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

Beachten Sie, dass dadurch keine Layoutdatei für die Aktivität geladen wird (was Sie in der Regel mit [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*))).
Legen Sie stattdessen die [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter) fest.
die Eigenschaft fügt automatisch eine [`ListView`](xref:Android.Widget.ListView)
, um den gesamten Bildschirm der [`ListActivity`](xref:Android.App.ListActivity)auszufüllen.
Diese Methode verwendet eine [`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1), die das Array von Listenelementen verwaltet, die in die [`ListView`](xref:Android.Widget.ListView)eingefügt werden.
Der [`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)
der Konstruktor nimmt die Anwendungs [`Context`](xref:Android.Content.Context), die layoutbeschreibung für jedes Listenelement (im vorherigen Schritt erstellt) und eine `T[]` oder [`Java.Util.IList<T>`](xref:Java.Util.IList)
Array von-Objekten, die in den [`ListView`](xref:Android.Widget.ListView) eingefügt werden sollen
(definiert als nächstes).

Der [`TextFilterEnabled`](xref:Android.Widget.AbsListView.TextFilterEnabled)
die-Eigenschaft schaltet die Text Filterung für die [`ListView`](xref:Android.Widget.ListView)ein, sodass die Liste gefiltert wird, wenn der Benutzer mit der Eingabe beginnt.

Der [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
das Ereignis kann zum Abonnieren von Handlern für Klicks verwendet werden. Wenn ein Element in der [`ListView`](xref:Android.Widget.ListView)
wird geklickt, der Handler wird aufgerufen, und ein [`Toast`](xref:Android.Widget.Toast)
die Meldung wird mit dem Text aus dem Element angezeigt, auf das geklickt wurde.

Sie können Listenelement Entwürfe verwenden, die von der Plattform bereitgestellt werden, anstatt eine eigene Layoutdatei für die [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)zu definieren.
Versuchen Sie z. b., anstelle von `Resource.Layout.list_item``Android.Resource.Layout.SimpleListItem1` zu verwenden.

Fügen Sie die folgende `using`-Anweisung hinzu:

```csharp
using System;
```

Fügen Sie als nächstes das folgende Zeichen folgen Array als Member von `MainActivity`hinzu:

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

Dies ist das Array von Zeichen folgen, die in der [`ListView`](xref:Android.Widget.ListView)platziert werden.

Führen Sie die Anwendung aus. Sie können einen Bildlauf in der Liste durchführen oder eingeben, um ihn zu filtern, und dann auf ein Element klicken, um eine Meldung anzuzeigen Folgendes sollte angezeigt werden:

[![Beispiel Bildschirm Abbildung von ListView mit Ländernamen](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

Beachten Sie, dass die Verwendung eines hart codierten Zeichen folgen Arrays nicht die beste Entwurfs Praxis ist. Der Einfachheit halber wird in diesem Tutorial verwendet, um die [`ListView`](xref:Android.Widget.ListView)
-Widget verwenden. Die bessere Vorgehensweise besteht darin, auf ein durch eine externe Ressource definiertes Zeichen folgen Array zu verweisen, z. b. mit einer `string-array` Ressource in der Datei Project **Resources/Values/Strings. XML** . Beispiel:

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

Um diese Ressourcen Zeichenfolgen für die [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter`1)zu verwenden, ersetzen Sie den ursprünglichen [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
Zeile mit folgendem:

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Beispiel Bildschirm Abbildung von ListView mit einer kleineren Liste von Namen](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)

## <a name="going-further-with-listview"></a>Fortfahren mit ListView

Die verbleibenden Themen (links unten) machen einen umfassenden Einblick in die Arbeit mit der `ListView`-Klasse und den verschiedenen Typen von Adapter Typen, die Sie mit der Klasse verwenden können. Die Struktur lautet wie folgt:

- **Visuelle** Darstellung &ndash; Teile des `ListView` Steuer Elements und deren Funktionsweise.

- **Klassen** &ndash; Übersicht über die Klassen, die zum Anzeigen einer `ListView`verwendet werden.

- **Anzeigen von Daten in einem ListView** -&ndash;, wie eine einfache Datenliste angezeigt wird. So implementieren Sie `ListView's` Verwendbarkeits Features Verwenden verschiedener integrierter Zeilen Layouts und wie Adapter Speicher durch wieder verwenden von Zeilen Ansichten speichern.

- **Benutzerdefinierte** Darstellung &ndash; Ändern des Stils der `ListView` mit benutzerdefinierten Layouts, Schriftarten und Farben.

- **Mithilfe von SQLite** &ndash; Vorgehensweise beim Anzeigen von Daten aus einer SQLite-Datenbank mit einem `CursorAdapter`.

- **Aktivitäts Lebenszyklus** &ndash; Entwurfs Überlegungen bei der Implementierung von `ListView` Aktivitäten, einschließlich der Art und Weise, in der Sie Ihre Daten auffüllen sollten und wann Ressourcen freigegeben werden sollen.

Die Erörterung (in sechs Teile unterteilt) beginnt mit einer Übersicht über die `ListView` Klasse selbst, bevor Sie progressivere Beispiele für deren Verwendung einführt.

- [ListView-Komponenten und -Funktionen](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
- [Auffüllen von ListView mit Daten](~/android/user-interface/layouts/list-view/populating.md)
- [Anpassen einer ListView-Darstellung](~/android/user-interface/layouts/list-view/customizing-appearance.md)
- [Verwenden von CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
- [Verwenden von ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
- [ListView und der Aktivitätslebenszyklus](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

## <a name="summary"></a>Zusammenfassung

In diesem Thema wurden `ListView` erläutert und einige Beispiele für die Verwendung der integrierten Funktionen des `ListActivity`bereitgestellt. Es wurden benutzerdefinierte Implementierungen von `ListView` erläutert, die für farbige Layouts und die Verwendung einer SQLite-Datenbank zulässig waren, und es wurde kurz auf die Relevanz des Aktivitäts Lebenszyklus in ihrer `ListView` Implementierung eingegangen.

## <a name="related-links"></a>Verwandte Links

- [Accessoryviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [Basictableandroid (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [Basictableadapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [Builtinviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [Customrowview (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
- [Fastscroll (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
- [Sectionindex (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sectionindex)
- [Simplecurrsortableadapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [Cursor TableAdapter (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
- [Tutorial zum Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Arbeiten mit Tabellen und Zellen (in xamarin. IOS)](~/ios/user-interface/controls/tables/index.md)
- [ListView-Klassen Verweis](xref:Android.Widget.ListView)
- [Listactivity-Klassen Verweis](xref:Android.App.ListActivity)
- [Baseadapter-Klassenreferenz](xref:Android.Widget.BaseAdapter)
- [Arrayadapter-Klassenreferenz](xref:Android.Widget.ArrayAdapter)
- [CursorAdapter-Klassenreferenz](xref:Android.Widget.CursorAdapter)
