---
title: Auto Vervollständigen für xamarin. Android
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: 575235569351d0856c7fbffbf38a981ede1a35ce
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762441"
---
# <a name="auto-complete-for-xamarinandroid"></a>Auto Vervollständigen für xamarin. Android

`AutoCompleteTextView`ein bearbeitbares Text Ansichts Element, das die Vervollständigungs Vorschläge automatisch anzeigt, während der Benutzer die Eingabe durchläuft. Die Liste der Vorschläge wird in einem Dropdown Menü angezeigt, in dem der Benutzer ein Element auswählen kann, um den Inhalt des Bearbeitungs Felds durch zu ersetzen.

![Beispiel für Auto Vervollständigen](images/auto-complete.png)

## <a name="overview"></a>Übersicht

Verwenden Sie zum Erstellen eines Text Eintrags Widgets, das Empfehlungen für automatische Vervollständigung bereitstellt, das[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Program. Vorschläge werden aus einer Auflistung von Zeichen folgen, die dem Widget zugeordnet sind [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter), über einen empfangen.

In diesem Tutorial erstellen Sie eine[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
ein Widget, das Vorschläge für einen Ländernamen bereitstellt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

[`TextView`](xref:Android.Widget.TextView) Ist eine Bezeichnung, die die[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Program.

## <a name="tutorial"></a>Lernprogramm

Starten Sie ein neues Projekt mit dem Namen *helloautocomplete*.

Erstellen Sie eine XML- `list_item.xml` Datei mit dem Namen, und speichern Sie Sie im Ordner " **Resources/Layout** ". Legen Sie die Buildaktion dieser Datei `AndroidResource`auf fest. Bearbeiten Sie die Datei so, dass Sie wie folgt aussieht:

```xml
<?xml version="1.0" encoding="utf-8"?>

<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp"
    android:textColor="#000">
</TextView> 
```

Diese Datei definiert eine einfache [`TextView`](xref:Android.Widget.TextView) , die für jedes Element verwendet wird, das in der Liste der Vorschläge angezeigt wird.

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="5dp">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Country" />
    <AutoCompleteTextView android:id="@+id/autocomplete_country"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"/>
</LinearLayout>
```

Öffnen Sie **MainActivity.cs** , und fügen Sie den folgenden Code für das[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "Main" layout resource
    SetContentView (Resource.Layout.Main);

    AutoCompleteTextView textView = FindViewById<AutoCompleteTextView> (Resource.Id.autocomplete_country);
    var adapter = new ArrayAdapter<String> (this, Resource.Layout.list_item, COUNTRIES);

    textView.Adapter = adapter;
}
```

Nachdem die Inhaltsansicht auf das `main.xml` Layout festgelegt wurde, wird das[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
das Widget wird aus dem Layout mit [`FindViewById`](xref:Android.App.Activity.FindViewById*)aufgezeichnet. Anschließend wird [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) ein neuer initialisiert, um das `list_item.xml` Layout an `COUNTRIES` jedes Listenelement im Zeichen folgen Array (im nächsten Schritt definiert) zu binden. Schließlich wird aufgerufen, um die [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter) mit dem `SetAdapter()`[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Widget, damit das Zeichen folgen Array die Liste der Vorschläge auffüllt.

Fügen Sie `MainActivity` in der-Klasse das Zeichen folgen Array hinzu:

```csharp
static string[] COUNTRIES = new string[] {
  "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra",
  "Angola", "Anguilla", "Antarctica", "Antigua and Barbuda", "Argentina",
  "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium",
  "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia",
  "Bosnia and Herzegovina", "Botswana", "Bouvet Island", "Brazil", "British Indian Ocean Territory",
  "British Virgin Islands", "Brunei", "Bulgaria", "Burkina Faso", "Burundi",
  "Cote d'Ivoire", "Cambodia", "Cameroon", "Canada", "Cape Verde",
  "Cayman Islands", "Central African Republic", "Chad", "Chile", "China",
  "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo",
  "Cook Islands", "Costa Rica", "Croatia", "Cuba", "Cyprus", "Czech Republic",
  "Democratic Republic of the Congo", "Denmark", "Djibouti", "Dominica", "Dominican Republic",
  "East Timor", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Ethiopia", "Faeroe Islands", "Falkland Islands", "Fiji", "Finland",
  "Former Yugoslav Republic of Macedonia", "France", "French Guiana", "French Polynesia",
  "French Southern Territories", "Gabon", "Georgia", "Germany", "Ghana", "Gibraltar",
  "Greece", "Greenland", "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau",
  "Guyana", "Haiti", "Heard Island and McDonald Islands", "Honduras", "Hong Kong", "Hungary",
  "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", "Israel", "Italy", "Jamaica",
  "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", "Kuwait", "Kyrgyzstan", "Laos",
  "Latvia", "Lebanon", "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg",
  "Macau", "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands",
  "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia",
  "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
  "Nicaragua", "Niger", "Nigeria", "Niue", "Norfolk Island", "North Korea", "Northern Marianas",
  "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay", "Peru",
  "Philippines", "Pitcairn Islands", "Poland", "Portugal", "Puerto Rico", "Qatar",
  "Reunion", "Romania", "Russia", "Rwanda", "Sqo Tome and Principe", "Saint Helena",
  "Saint Kitts and Nevis", "Saint Lucia", "Saint Pierre and Miquelon",
  "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Saudi Arabia", "Senegal",
  "Seychelles", "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands",
  "Somalia", "South Africa", "South Georgia and the South Sandwich Islands", "South Korea",
  "Spain", "Sri Lanka", "Sudan", "Suriname", "Svalbard and Jan Mayen", "Swaziland", "Sweden",
  "Switzerland", "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "The Bahamas",
  "The Gambia", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey",
  "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", "Virgin Islands", "Uganda",
  "Ukraine", "United Arab Emirates", "United Kingdom",
  "United States", "United States Minor Outlying Islands", "Uruguay", "Uzbekistan",
  "Vanuatu", "Vatican City", "Venezuela", "Vietnam", "Wallis and Futuna", "Western Sahara",
  "Yemen", "Yugoslavia", "Zambia", "Zimbabwe"
};
```

Dies ist die Liste der Vorschläge, die in einer Dropdown Liste bereitgestellt werden, wenn der Benutzer in den[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Program.

Führen Sie die Anwendung aus. Wenn Sie eingeben, sollte etwa Folgendes angezeigt werden:

[![Beispiel für eine automatische Vervollständigung von Screenshots, die "ca" enthalten](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)

## <a name="more-information"></a>Weitere Informationen

Beachten Sie, dass die Verwendung eines hart codierten Zeichen folgen Arrays keine empfohlene Entwurfs Praxis ist, da sich Ihr Anwendungscode auf das Verhalten, nicht auf den Inhalt konzentrieren sollte. Anwendungs Inhalt, z. b. Zeichen folgen, sollte vom Code extern ausgelagert werden, um Änderungen am Inhalt zu vereinfachen und die Lokalisierung der Inhalte zu vereinfachen. Die hart codierten Zeichen folgen werden in diesem Tutorial lediglich verwendet, um Sie einfach zu gestalten und sich auf das[`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)
Program. Stattdessen sollte die Anwendung solche Zeichen folgen Arrays in einer XML-Datei deklarieren. Dies kann mit einer `<string-array>` Ressource in ihrer Projekt `res/values/strings.xml` Datei erfolgen. Beispiel:

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

Um diese Ressourcen Zeichenfolgen für [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)die zu verwenden, ersetzen Sie den ursprünglichen[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
konstruktorzeile mit folgendem:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```

### <a name="references"></a>Verweise

- [Autocompletetextview-Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) &ndash; Xamarin. Android-Beispiel Projekt für das`AutoCompleteTextView`
- [`ArrayAdapter`](xref:Android.Widget.ArrayAdapter)
- [`AutoCompleteTextView`](xref:Android.Widget.AutoCompleteTextView)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden. Dieses Tutorial basiert auf dem [Android Auto Complete Tutorial *](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)._
