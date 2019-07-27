---
title: Xamarin. Android-Spinner
ms.prod: xamarin
ms.assetid: 004089E9-7C1D-2285-765A-B69143091F2A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2c7f0de2347e614b8c24de32bf3f88362a212a94
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510407"
---
# <a name="xamarinandroid-spinner"></a>Xamarin. Android-Spinner

[`Spinner`](xref:Android.Widget.Spinner)ein Widget, das eine Dropdown Liste für die Auswahl von Elementen darstellt. In diesem Handbuch wird erläutert, wie Sie eine einfache APP erstellen, die eine Liste mit Auswahlmöglichkeiten in einem Spinner anzeigt, gefolgt von Änderungen, in denen andere Werte angezeigt werden, die mit der ausgewählten Auswahl verknüpft sind.

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

Beachten Sie, [`TextView`](xref:Android.Widget.TextView)dass `android:text` das-Attribut [`Spinner`](xref:Android.Widget.Spinner)und `android:prompt` das-Attribut jeweils auf dieselbe Zeichen folgen Ressource verweisen. Dieser Text verhält sich als Titel für das Widget. Wenn Sie auf das [`Spinner`](xref:Android.Widget.Spinner)-Feld angewendet wird, wird der Titeltext im Auswahl Dialogfeld angezeigt, das bei der Auswahl des Widgets angezeigt wird.

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

Das zweite `<string>` Element definiert die Titel Zeichenfolge, [`TextView`](xref:Android.Widget.TextView) auf [`Spinner`](xref:Android.Widget.Spinner) die von und im Layout oben verwiesen wird.
Das `<string-array>` -Element definiert die Liste der Zeichen folgen, die als Liste [`Spinner`](xref:Android.Widget.Spinner) im Widget angezeigt werden.

Öffnen Sie jetzt **MainActivity.cs** , und fügen `using` Sie die folgende-Anweisung hinzu:

```csharp
using System;
```

Fügen Sie als nächstes den folgenden Code für [`OnCreate()`](xref:Android.App.Activity.OnCreate*)die Methode) ein:

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

Nachdem das `Main.axml` Layout als Inhaltsansicht festgelegt wurde, wird [`Spinner`](xref:Android.Widget.Spinner) das Widget aus dem Layout mit [`FindViewById<>(int)`](xref:Android.App.Activity.FindViewById*)aufgezeichnet.
Die[`CreateFromResource()`](xref:Android.Widget.ArrayAdapter.CreateFromResource*)
die Methode erstellt dann ein [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)neues-Objekt, das jedes Element im Zeichen folgen Array an die anfängliche Darstellung [`Spinner`](xref:Android.Widget.Spinner) für das bindet (bei Auswahl dieser Option werden die einzelnen Elemente im Spinner angezeigt). Die `Resource.Array.planets_array` ID verweist auf `string-array` das oben definierte, `Android.Resource.Layout.SimpleSpinnerItem` und die ID verweist auf ein Layout für die Standard spindarstellung, die von der Plattform definiert wird.
[`SetDropDownViewResource`](xref:Android.Widget.ArrayAdapter.SetDropDownViewResource*)
wird aufgerufen, um die Darstellung für jedes Element zu definieren, wenn das Widget geöffnet wird. Schließlich wird die [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) -Eigenschaft so festgelegt, dass alle Elemente mit [`Spinner`](xref:Android.Widget.Spinner) dem verknüpft werden [`Adapter`](xref:Android.Widget.ArrayAdapter) , indem die-Eigenschaft festgelegt wird.

Stellen Sie jetzt eine Rückruf Methode bereit, mit der die Anwendung benachrichtigt wird, wenn ein Element aus [`Spinner`](xref:Android.Widget.Spinner)der ausgewählt wurde. Diese Methode sollte wie folgt aussehen:

```csharp
private void spinner_ItemSelected (object sender, AdapterView.ItemSelectedEventArgs e)
{
    Spinner spinner = (Spinner)sender;
    string toast = string.Format ("The planet is {0}", spinner.GetItemAtPosition (e.Position));
    Toast.MakeText (this, toast, ToastLength.Long).Show ();
}
```

Wenn ein Element ausgewählt wird, wird der Absender in eine [`Spinner`](xref:Android.Widget.Spinner) umgewandelt, sodass auf Elemente zugegriffen werden kann. Mithilfe der `Position` -Eigenschaft `ItemEventArgs`im können Sie den Text des ausgewählten Objekts ermitteln und zum Anzeigen eines [`Toast`](xref:Android.Widget.Toast)verwenden.

Führen Sie die Anwendung aus. Es sollte wie folgt aussehen:

[![Screenshot: Beispiel für Spinner mit als Planet ausgewähltem Mars](spinner-images/02-basic-example-sml.png)](spinner-images/02-basic-example.png#lightbox)

## <a name="spinner-using-keyvalue-pairs"></a>Spinner mit Schlüssel-Wert-Paaren

Häufig ist es erforderlich, zum `Spinner` Anzeigen von Schlüsselwerten zu verwenden, die mit einer Art von Daten verknüpft sind, die von Ihrer APP verwendet werden. Da `Spinner` nicht direkt mit Schlüssel-Wert-Paaren funktioniert, müssen Sie das Schlüssel-Wert-Paar separat speichern, das `Spinner` mit Schlüsselwerten auffüllen und dann die Position des ausgewählten Schlüssels im Spinner verwenden, um den zugeordneten Datenwert zu suchen. 

In den folgenden Schritten wird die **hellospinner** -APP so geändert, dass Sie die mittlere Temperatur für den ausgewählten Planet anzeigt:

Fügen Sie die `using` folgende-Anweisung zu **MainActivity.cs**hinzu:

```csharp
using System.Collections.Generic;
```

Fügen Sie der- `MainActivity` Klasse die folgende Instanzvariable hinzu.
Diese Liste enthält Schlüssel/Wert-Paare für die Planeten und deren Durchschnittstemperatur:

```csharp
private List<KeyValuePair<string, string>> planets;
```

Fügen Sie `OnCreate` in der-Methode den folgenden Code `adapter` hinzu, bevor deklariert wird:

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

Übergeben Sie diese Liste an `ArrayAdapter` den Konstruktor (anstelle der `planets_array` Ressource):

```csharp
var adapter = new ArrayAdapter<string>(this,
    Android.Resource.Layout.SimpleSpinnerItem, planetNames);
```

Ändern `spinner_ItemSelected` , sodass die ausgewählte Position verwendet wird, um den Wert (die Temperatur) zu suchen, der dem ausgewählten Planet zugeordnet ist:

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

[![Beispiel für eine Planet-Auswahl, die Temperatur anzeigt](spinner-images/03-keyvalue-example-sml.png)](spinner-images/03-keyvalue-example.png#lightbox)

## <a name="resources"></a>Ressourcen

- [`Resource.Layout`](xref:Android.Resource.Layout)
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`Spinner`](xref:Android.Widget.Spinner)

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
