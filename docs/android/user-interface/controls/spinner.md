---
title: Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 90b4755cdb4b8248c2b731d070d720076d4dda40
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102996"
---
# <a name="spinner"></a>Spinner

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) ist ein Widget, das eine Dropdown-Liste für das Auswählen von Elementen enthält. Dieses Handbuch wird erläutert, wie eine einfache Anwendung erstellen, in dem eine Liste von Optionen, in ein Drehfeld angezeigt, gefolgt von Änderungen, die andere Werte, mit der Auswahl anzuzeigen.

## <a name="basic-spinner"></a>Grundlegende Spinner

Im ersten Teil dieses Tutorials erstellen Sie ein einfaches Spinner-Widget aus, die eine Liste von Planeten anzeigt. Wenn ein Planet ausgewählt ist, zeigt eine Popup-Nachricht das ausgewählte Element:

[![Beispielscreenshots HelloSpinner-App](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Starten Sie ein neues Projekt namens **HelloSpinner**.

Open **Resources/Layout/Main.axml** und fügen Sie den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="10dip"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dip"
        android:text="@string/planet_prompt"
    />
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:prompt="@string/planet_prompt"
    />
</LinearLayout>
```

Beachten Sie, dass die [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)des `android:text` Attribut und die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)des `android:prompt` Attribut sowohl auf die gleiche Zeichenfolgenressource verweisen. Dieser Text verhält sich wie einen Titel für das Widget. Bei Anwendung auf die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), der Text wird angezeigt, in dem Dialogfeld "Auswahl", die nach dem Auswählen des Widgets angezeigt wird.

Bearbeiten Sie **Resources/Values/Strings.xml** und ändern Sie die Datei wie folgt aussehen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloSpinner</string>
  <string name="planet_prompt">Choose a planet</string>
  <string-array name="planets_array">
    <item>Mercury</item>
    <item>Venus</item>
    <item>Earth</item>
    <item>Mars</item>
    <item>Jupiter</item>
    <item>Saturn</item>
    <item>Uranus</item>
    <item>Neptune</item>
  </string-array>
</resources>
```

Die zweite `<string>` -Element definiert die Title-Zeichenfolge, die auf die [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) und [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) im oben genannten Layout.
Die `<string-array>` Element definiert die Liste von Zeichenfolgen, die als der Liste angezeigt wird der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) Widget.

Öffnen Sie nun **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisung:

```csharp
using System;
```

Als Nächstes fügen Sie folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
Methode:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    Spinner spinner = FindViewById<Spinner> (Resource.Id.spinner);

    spinner.ItemSelected += new EventHandler<AdapterView.ItemSelectedEventArgs> (spinner_ItemSelected);
    var adapter = ArrayAdapter.CreateFromResource (
            this, Resource.Array.planets_array, Android.Resource.Layout.SimpleSpinnerItem);

    adapter.SetDropDownViewResource (Android.Resource.Layout.SimpleSpinnerDropDownItem);
    spinner.Adapter = adapter;
}
```

Nach der `Main.axml` Layout festgelegt ist, als der Inhaltsansicht der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) Widget wird erfasst, aus dem Layout mit [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Die [`CreateFromResource()`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/)
-Methode erstellt dann ein neues [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), die jedes Element in der Zeichenfolge bindet, auf die ursprüngliche Darstellung für die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (Dies ist wie jedes Element in das Drehfeld, wenn ausgewählt angezeigt wird) . Die `Resource.Array.planets_array` ID-verweisen die `string-array` oben definierten und `Android.Resource.Layout.SimpleSpinnerItem` ID verweist auf ein Layout für die standard-Spinner-Darstellung, die von der Plattform definiert.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/)
wird aufgerufen, um die Darstellung für die einzelnen Elemente zu definieren, wenn das Widget geöffnet wird. Zum Schluss die [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) festgelegt ist, um sämtliche Elemente mit zuzuordnen der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) durch Festlegen der [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) Eigenschaft.

Geben Sie nun eine Rückrufmethode, die die Anwendung notifys, bei der Auswahl eines Elements aus der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). Hier ist diese Methode sollte folgendermaßen aussehen:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Wenn ein Element ausgewählt ist, der Absender der Umwandlung in einen [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) , damit Elemente zugegriffen werden kann. Mit der `Position` Eigenschaft für die `ItemEventArgs`, Sie können herausfinden, den Text des ausgewählten Objekts und verwenden sie zum Anzeigen einer [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Führen Sie die Anwendung; Es sollte wie folgt aussehen:

[![Beispielhafter Screenshot des Spinner bei Mars-Funktion wie der Planet ausgewählt](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Drehfeld, die mithilfe von Schlüssel/Wert-Paaren

Es ist häufig erforderlich, verwenden Sie `Spinner` Schlüsselwerte angezeigt, die eine Art von Daten, die von Ihrer app verwendeten zugeordnet sind. Da `Spinner` funktioniert nicht direkt mit Schlüssel-/Wertpaaren, müssen Sie separat Speichern von Schlüssel/Wert-Paar, Auffüllen der `Spinner` mit Schlüsselwerten, verwenden Sie Sie dann die Position des Schlüssels in das Drehfeld, um den zugeordneten Wert zu suchen. 

In den folgenden Schritten wird die **HelloSpinner** app wird geändert, um die Mean-Temperatur für den ausgewählten Planeten anzuzeigen:

Fügen Sie die folgenden `using` Anweisung **"mainactivity.cs"**:

```csharp
using System.Collections.Generic;
```

Fügen Sie die folgende Instanzvariable, um die `MainActivity` Klasse.
Diese Liste enthält die Schlüssel/Wert-Paare für die Planeten und die durchschnittlichen Temperaturen:

```csharp
private List<KeyValuePair<string, string>> planets;
```

In der `OnCreate` -Methode, fügen Sie den folgenden Code vor dem `adapter` deklariert wird:

```csharp
planets = new List<KeyValuePair<string, string>>
{
    new KeyValuePair<string, string>("Mercury", "167 degrees C"),
    new KeyValuePair<string, string>("Venus", "464 degrees C"),
    new KeyValuePair<string, string>("Earth", "15 degrees C"),
    new KeyValuePair<string, string>("Mars", "-65 degrees C"),
    new KeyValuePair<string, string>("Jupiter" , "-110 degrees C"),
    new KeyValuePair<string, string>("Saturn", "-140 degrees C"),
    new KeyValuePair<string, string>("Uranus", "-195 degrees C"),
    new KeyValuePair<string, string>("Neptune", "-200 degrees C")
};
```

Dieser Code erstellt einen einfachen Store Planeten und ihre zugeordneten Mean Temperaturen an. (In einer realen app wird eine Datenbank in der Regel verwendet zum Speichern von Schlüsseln und die zugehörigen Daten.)

Unmittelbar nach dem obigen Code fügen Sie die folgenden Zeilen ein, um die Schlüssel zu extrahieren, und platzieren Sie diese in einer Liste (in Reihenfolge) hinzu:

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Übergeben Sie diese Liste, um die `ArrayAdapter` Konstruktor (statt der `planets_array` Ressource):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Ändern Sie `spinner_ItemSelected` , damit die ausgewählte Position verwendet wird, um den ausgewählten weltweit zugeordneten Wert (die Temperatur) zu suchen:

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Führen Sie die Anwendung; der Toast sollte wie folgt aussehen:

[![Beispiel einer weltweit-Auswahl, die Anzeige von Temperatur](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)
   
  

## <a name="resources"></a>Ressourcen

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
