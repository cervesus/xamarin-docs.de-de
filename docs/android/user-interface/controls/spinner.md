---
title: Xamarin. Android-Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: ba4a83eb997b879e8a2398f9857e2fd80221f8ef
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029168"
---
# <a name="xamarinandroid-spinner"></a>Xamarin. Android-Spinner

[`Spinner`](xref:Android.Widget.Spinner) ist ein Widget, das eine Dropdown Liste für die Auswahl von Elementen darstellt. In diesem Handbuch wird erläutert, wie Sie eine einfache APP erstellen, die eine Liste mit Auswahlmöglichkeiten in einem Spinner anzeigt, gefolgt von Änderungen, in denen andere Werte angezeigt werden, die mit der ausgewählten Auswahl verknüpft sind.

## <a name="basic-spinner"></a>Einfache Spinner

Im ersten Teil dieses Tutorials erstellen Sie ein einfaches Spinner-Widget, das eine Liste von Planeten anzeigt. Wenn ein Planet ausgewählt ist, zeigt eine Popup Meldung das ausgewählte Element an:

[![Beispiel-Screenshots der hellospinner-App](spinner-images/01-example-screenshots-sml.png)](spinner-images/01-example-screenshots.png#lightbox)

Starten Sie ein neues Projekt mit dem Namen **hellospinner**.

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie den folgenden XML-Code ein:

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

Beachten Sie, dass das `android:text`-Attribut des [`TextView`](xref:Android.Widget.TextView)und das `android:prompt`-Attribut des [`Spinner`](xref:Android.Widget.Spinner)auf dieselbe Zeichen folgen Ressource verweisen. Dieser Text verhält sich als Titel für das Widget. Wenn Sie auf die [`Spinner`](xref:Android.Widget.Spinner)angewendet wird, wird der Titeltext im Auswahl Dialogfeld angezeigt, das bei der Auswahl des Widgets angezeigt wird.

Bearbeiten Sie **Resources/Values/Strings. XML** , und ändern Sie die Datei so, dass Sie wie folgt aussieht:

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

Das zweite `<string>` Element definiert die Titel Zeichenfolge, auf die durch die [`TextView`](xref:Android.Widget.TextView) verwiesen wird, und [`Spinner`](xref:Android.Widget.Spinner) im Layout oben.
Das `<string-array>`-Element definiert die Liste der Zeichen folgen, die im [`Spinner`](xref:Android.Widget.Spinner) -Widget als Liste angezeigt werden.

Öffnen Sie nun **MainActivity.cs** , und fügen Sie die folgende `using`-Anweisung hinzu:

```csharp
using System;
```

Fügen Sie als nächstes den folgenden Code für die [`OnCreate()`](xref:Android.App.Activity.OnCreate*))-Methode ein:

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

Nachdem das `Main.axml` Layout als Inhaltsansicht festgelegt wurde, wird das [`Spinner`](xref:Android.Widget.Spinner) -widget aus dem Layout mit [`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*)aufgezeichnet.
Der [`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
die Methode erstellt dann einen neuen [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter), der jedes Element im Zeichen folgen Array an die anfängliche Darstellung für das [`Spinner`](xref:Android.Widget.Spinner) bindet (auf diese Weise werden die einzelnen Elemente im Spinner angezeigt, wenn Sie ausgewählt werden). Die `Resource.Array.planets_array`-ID verweist auf die oben definierte `string-array`, und die `Android.Resource.Layout.SimpleSpinnerItem`-ID verweist auf ein Layout für die Standard spindarstellung, die von der Plattform definiert wird.
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
wird aufgerufen, um die Darstellung für jedes Element zu definieren, wenn das Widget geöffnet wird. Zum Schluss wird die [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) so festgelegt, dass alle Elemente mit dem [`Spinner`](xref:Android.Widget.Spinner) verknüpft werden, indem die [`Adapter`](xref:Android.Widget.ArrayAdapter) -Eigenschaft festgelegt wird.

Stellen Sie jetzt eine Rückruf Methode bereit, mit der die Anwendung benachrichtigt wird, wenn ein Element aus dem [`Spinner`](xref:Android.Widget.Spinner)ausgewählt wurde. Diese Methode sollte wie folgt aussehen:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Wenn ein Element ausgewählt wird, wird der Absender in eine [`Spinner`](xref:Android.Widget.Spinner) umgewandelt, sodass auf Elemente zugegriffen werden kann. Mithilfe der `Position`-Eigenschaft auf dem `ItemEventArgs`können Sie den Text des ausgewählten Objekts ermitteln und zum Anzeigen eines [`Toast`](xref:Android.Widget.Toast)verwenden.

Führen Sie die Anwendung aus. Es sollte wie folgt aussehen:

[![Screenshot: Beispiel für Spinner mit als Planet ausgewähltem Mars](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Spinner mit Schlüssel-Wert-Paaren

Häufig ist es erforderlich, `Spinner` zum Anzeigen von Schlüsselwerten zu verwenden, die mit einer Art von Daten verknüpft sind, die von Ihrer APP verwendet werden. Da `Spinner` nicht direkt mit Schlüssel-Wert-Paaren funktioniert, müssen Sie das Schlüssel-Wert-Paar separat speichern, das `Spinner` mit Schlüsselwerten auffüllen und dann die Position des ausgewählten Schlüssels im Spinner verwenden, um den zugeordneten Datenwert zu suchen. 

In den folgenden Schritten wird die **hellospinner** -APP so geändert, dass Sie die mittlere Temperatur für den ausgewählten Planet anzeigt:

Fügen Sie die folgende `using`-Anweisung zu **MainActivity.cs**hinzu:

```csharp
using System.Collections.Generic;
```

Fügen Sie der `MainActivity`-Klasse die folgende Instanzvariable hinzu.
Diese Liste enthält Schlüssel/Wert-Paare für die Planeten und deren Durchschnittstemperatur:

```csharp
private List<KeyValuePair<string, string>> planets;
```

Fügen Sie in der `OnCreate`-Methode den folgenden Code hinzu, bevor `adapter` deklariert ist:

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

Mit diesem Code wird ein einfacher Speicher für-und die damit verbundenen Durchschnittstemperaturen erstellt. (In einer realen APP wird eine Datenbank normalerweise zum Speichern von Schlüsseln und deren zugeordneten Daten verwendet.)

Fügen Sie direkt nach dem obigen Code die folgenden Zeilen hinzu, um die Schlüssel zu extrahieren und Sie in einer Liste (in der richtigen Reihenfolge) zu platzieren:

```csharp
List<string> planetNames = new List<string>();
foreach (var item in planets)
    planetNames.Add (item.Key);
```

Übergeben Sie diese Liste an den `ArrayAdapter`-Konstruktor (anstelle der `planets_array` Ressource):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Ändern Sie `spinner_ItemSelected` so, dass die ausgewählte Position verwendet wird, um den Wert (die Temperatur) zu suchen, der dem ausgewählten Planet zugeordnet ist:

```csharp
private void spinner_ItemSelected(object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format("The mean temperature for planet {0} is {1}",
        spinner.GetItemAtPosition(e.Position), planets[e.Position].Value);
    Toast.MakeText(this, toast, ToastLength.Long).Show();
}
```

Führen Sie die Anwendung aus. der Toast sollte wie folgt aussehen:

[![Beispiel für die Auswahl der Temperatur in der Welt](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>Ressourcen

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
