---
title: Automatische Vervollständigung
ms.prod: xamarin
ms.assetid: D4C8CA49-8369-35B7-798D-B147FDC24185
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/31/2018
ms.openlocfilehash: cf2221380e5ddbd8278cc2d387c6eb185d990c1a
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671897"
---
# <a name="auto-complete"></a>Automatische Vervollständigung

`AutoCompleteTextView` ist ein bearbeitbarer Text anzeigen-Element, das vervollständigungsvorschläge automatisch zeigt während der Eingabe des Benutzers an. Die Liste der Vorschläge wird in einem Dropdown-Menü angezeigt, in dem der Benutzer ein Element hinzu, ersetzen Sie den Inhalt des Bearbeitungsfelds mit auswählen kann.

![Beispiel für die automatische Vervollständigung](images/auto-complete.png)

## <a name="overview"></a>Übersicht

Verwenden Sie zum Erstellen eines Text-Eintrag-Widgets, die automatische Vervollständigung Vorschläge unterbreitet der [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget. Vorschläge werden aus einer Auflistung von Zeichenfolgen, die das Widget über zugeordnete empfangen ein [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/).

In diesem Tutorial erstellen Sie eine [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
Widgets, die Vorschläge für einen Ländernamen.

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

Die [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) ist eine Bezeichnung, die eingeführt werden die [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget.


## <a name="tutorial"></a>Lernprogramm

Starten Sie ein neues Projekt namens *HelloAutoComplete*.

Erstellen Sie eine XML-Datei mit dem Namen `list_item.xml` und speichern Sie ihn in das **Ressourcen/Layout** Ordner. Legen Sie die Build-Aktion für diese Datei, um `AndroidResource`. Bearbeiten Sie die Datei wie folgt aussehen:

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

Diese Datei definiert eine einfache [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) wird, wird für jedes Element, das in der Liste mit Vorschlägen angezeigt wird.

Open **Resources/Layout/Main.axml** und fügen Sie Folgendes:

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

Open **"mainactivity.cs"** und fügen Sie den folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/(Android.OS.Bundle))
Methode:

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

Nach der Ansicht "Inhalt", um festgelegt ist die `main.xml` Layout, das [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
Widget wird erfasst, aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Ein neues [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) ist dann für die Bindung initialisiert die `list_item.xml` Layout für jedes Listenelement in der `COUNTRIES` Array von Zeichenfolgen (im nächsten Schritt definiert). Zum Schluss `SetAdapter()` wird aufgerufen, um das Zuordnen der [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/) mit der [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
Widget, damit das Zeichenfolgenarray Auffüllen die Liste der Vorschläge.

In der `MainActivity` -Klasse erstellt wird, die Zeichenfolgen-Array:

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

Dies ist die Liste mit Vorschlägen, die in einer Dropdown-Liste bereitgestellt wird, wenn der Benutzer, in eingibt der [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget.

Führen Sie die Anwendung aus. Wenn Sie eingeben, sehen Sie etwa wie folgt:

[![Automatische Vervollständigung beispielscreenshot Auflisten von Namen mit "ca"](auto-complete-images/helloautocomplete.png)](auto-complete-images/helloautocomplete.png#lightbox)



## <a name="more-information"></a>Weitere Informationen

Beachten Sie, dass eine hartcodierte Zeichenfolgen-Array nicht empfohlener Entwurf Methode handelt, da Ihr Anwendungscode, auf das Verhalten, die nicht damit zufrieden konzentrieren sollten. Anwendungsinhalte, z. B. Zeichenfolgen sollte extern ausgelagert werden, aus der Code zum Vereinfachen von Änderungen an den Inhalt und die Lokalisierung des Inhalts zu ermöglichen. Die hartcodierte Zeichenfolgen werden in diesem Tutorial verwendet, nur für einfache und den Schwerpunkt auf der [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)
widget. Stattdessen sollte Ihre Anwendung solche Zeichenfolgen-Arrays in eine XML-Datei deklarieren. Dies erreichen Sie mit einem `<string-array>` Ressource in Ihrem Projekt `res/values/strings.xml` Datei. Zum Beispiel:

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

Verwenden Sie diese Ressourcenzeichenfolgen für die [ `ArrayAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/), ersetzen Sie das ursprüngliche [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
mit den folgenden Konstruktor-Zeile:

```csharp
string[] countries = Resources.GetStringArray (Resource.array.countries_array);
var adapter = new ArrayAdapter<String> (this, Resource.layout.list_item, countries);
```


### <a name="references"></a>Verweise

-   [AutoCompleteTextView Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/autocomplete_text_view/add_an_autocomplete_text_input) &ndash; Xamarin.Android-Beispielprojekt für das `AutoCompleteTextView`.
-   [`ArrayAdapter`](https://developer.xamarin.com/api/type/Android.Widget.ArrayAdapter/)
-   [`AutoCompleteTextView`](https://developer.xamarin.com/api/type/Android.Widget.AutoCompleteTextView/)

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die* 
 [ *Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/) *. Dieses Tutorial basiert auf der* 
 [ *Android automatische vollständige Tutorial*](https://developer.android.com/resources/tutorials/views/hello-autocomplete.html)
 *.*
