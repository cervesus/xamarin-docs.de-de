---
title: Spinner
ms.topic: article
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 44775853a29a384216af308a607cfddd18c9c192
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="spinner"></a>Spinner

[`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) ist ein Widget, in dem eine Dropdownliste zum Auswählen von Elementen dargestellt. Dieses Handbuch erläutert, wie eine einfache Anwendung erstellen, die zeigt eine Liste mit Auswahlmöglichkeiten in ein Drehfeld, gefolgt von Änderungen, die andere Werte, die verknüpft sind, mit der Auswahl angezeigt wird.

## <a name="basic-spinner"></a>Grundlegende "Spinner"

Im ersten Teil dieses Lernprogramms erstellen Sie eine einfache "Spinner" Widget, das eine Liste der Planeten anzeigt. Wenn ein Planet aktiviert ist, zeigt eine Toast-Nachricht das ausgewählte Element aus:

[![Beispiel Screenshots HelloSpinner-app](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png)

Starten Sie ein neues Projekt mit dem Namen **HelloSpinner**.

Open **Resources/Layout/Main.axml** , und fügen Sie die folgenden XML-Code:

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

Beachten Sie, dass die [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/)des `android:text` Attribut und der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/)des `android:prompt` Attribut beide verweisen auf die gleiche Zeichenfolgenressource. Dieser Text verhält sich wie einen Titel für das Widget. Bei Anwendung auf die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/), der Titeltext erscheinen im Dialogfeld zum auswählen, die beim Auswählen des Widgets angezeigt wird.

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

Die zweite `<string>` -Element definiert die Titelzeichenfolge verweist die [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) und [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) im obigen Layout.
Die `<string-array>` Element definiert die Liste von Zeichenfolgen, die als der Liste angezeigt wird der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) Widget.

Öffnen Sie jetzt **MainActivity.cs** und fügen Sie die folgenden `using` Anweisung:

```csharp
using System;
```

Als Nächstes fügen Sie den folgenden Code für die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle)) Methode:

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

Nach der `Main.axml` Layout festgelegt ist, als die Inhaltsansicht der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) Widget wird erfasst, aus dem Layout mit [ `FindViewById<>(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Die [ `CreateFromResource()` ](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.CreateFromResource/p/Android.Content.Context/System.Int32/System.Int32/) Methode erstellt dann ein neues [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), die jedes Element im Zeichenfolgenarray bindet, auf die erste Darstellung für die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) (Dies ist das wie jedes Element in der "Spinner", wenn ausgewählt angezeigt wird). Die `Resource.Array.planets_array` ID Verweise der `string-array` oben definiert und die `Android.Resource.Layout.SimpleSpinnerItem` ID verweist auf ein Layout für die standardmäßige Spinner-Darstellung, von der Plattform definiert.
[`SetDropDownViewResource`](https://developer.xamarin.com/api/member/Android.Widget.ArrayAdapter.SetDropDownViewResource/p/System.Int32/) wird aufgerufen, um die Darstellung für jedes Element zu definieren, wenn das Widget geöffnet wird. Schließlich die [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) festgelegt ist, alle Elemente mit Zuordnen der [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) durch Festlegen der [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter) Eigenschaft.

Geben Sie nun eine Rückrufmethode, die die Anwendung notifys, wenn ein Element aus ausgewählt wurden die [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/). Hier wird diese Methode sollte folgendermaßen aussehen:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Wenn ein Element ausgewählt ist, der Absender ist eine Umwandlung in einen [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) , damit Elemente zugegriffen werden kann. Mithilfe der `Position` Eigenschaft auf der `ItemEventArgs`, können Sie ermitteln, die den Text des ausgewählten Objekts und verwenden, um die Anzeige einer [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/).

Führen Sie die Anwendung ein; Es sollte wie folgt aussehen:

[![Bildschirmabbildung von Beispiel "Spinner" bei Mars-Funktion als Planeten ausgewählt](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png)

## <a name="spinner-using-keyvalue-pairs"></a>Mithilfe von Schlüssel-/Wertpaaren "Spinner"

Es ist oft notwendig, verwenden Sie `Spinner` um Schlüsselwerte anzuzeigen, die eine Art von Daten, die von Ihrer app genutzten zugeordnet sind. Da `Spinner` funktioniert nicht direkt mit Schlüssel/Wert-Paare, müssen Sie separat speichern, das Schlüssel/Wert-Paar, Auffüllen der `Spinner` mit Schlüsselwerten, klicken Sie dann mit der die Position des Schlüssels in der "Spinner" Nachschlagen des Werts der zugehörigen Daten. 

In den folgenden Schritten die **HelloSpinner** app wurde geändert, um die mittlere Temperatur für den ausgewählten Planeten anzuzeigen:

Fügen Sie die folgenden `using` Anweisung **MainActivity.cs**:

```csharp
using System.Collections.Generic;
```

Die folgenden Instanzvariable zum Hinzufügen der `MainActivity` Klasse.
Diese Liste wird Schlüssel/Wert-Paare für die Planeten und deren Mittelwert Temperaturen enthalten:

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

Dieser Code erstellt einen einfachen Store für Planeten und ihre zugeordneten Mean Temperaturen an. (In einer realen-app, eine Datenbank dient normalerweise zum Speichern von Schlüsseln und deren zugeordneten Daten.)

Fügen Sie unmittelbar nach der obige Code die folgenden Zeilen den Schlüssel zu extrahieren, und fügen sie in einer Liste (in entsprechender Reihenfolge):

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

Ändern Sie `spinner_ItemSelected` , damit die ausgewählte Position verwendet wird, um der ausgewählten Planet zugeordnete Wert (die Temperatur) zu suchen:

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Führen Sie die Anwendung ein; der Toast sollte wie folgt aussehen:

[![Beispiel einer Planeten Auswahl anzeigen Temperatur](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png)
   
  
<a name="Resources" />

## <a name="resources"></a>Ressourcen

-   [`Resource.Layout`](https://developer.xamarin.com/api/type/Android.Resource+Layout/) 
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) 
-   [`Spinner`](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
