---
title: Android-Lokalisierung
description: In diesem Dokument werden die Lokalisierungsfeatures von Android SDK und wie Sie sie mit Xamarin zugreifen.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 634131025b322b64e89ece3b4c9d092e6b17a373
ms.sourcegitcommit: d09391c315336d36496880ef465a72b8974f2ac7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579816"
---
# <a name="android-localization"></a>Android-Lokalisierung

_In diesem Dokument werden die Lokalisierungsfeatures von Android SDK und wie Sie sie mit Xamarin zugreifen._

## <a name="android-platform-features"></a>Android-Plattformfeatures

Dieser Abschnitt beschreibt die wichtigsten Lokalisierungsfeatures von Android. Fahren Sie mit der [nächsten Abschnitt](#basics) zu spezifischen Code und Beispiele finden Sie unter.

### <a name="locale"></a>Gebietsschema

Benutzer wählen die Sprache in **Einstellungen > Language & Eingabe**. Diese Option steuert die Anzeigesprache und die regionalen Einstellungen verwendet (z. b. für Datum und die zahlenformatierung).

Das aktuelle Gebietsschema abgefragt werden kann, über des aktuellen Kontexts `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Dieser Wert wird eine Gebietsschema-ID sein, das ein Sprachcode und einem Code des Gebietsschemas, getrennt durch einen Unterstrich enthält. Zu Referenzzwecken hier ist ein [Liste der Gebietsschemas, Java](http://www.oracle.com/technetwork/java/javase/locales-137662.html) und [Android unterstützte Gebietsschemas über StackOverflow](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Gängige Beispiele:

* `en_US` für Englisch (USA)
* `es_ES` für Spanisch (Spanien)
* `ja_JP` für Japanisch (Japan)
* `zh_CN` für Chinesisch (China)
* `zh_TW` für Chinesisch (Taiwan)
* `pt_PT` Portugiesisch (Portugal)
* `pt_BR` Portugiesisch (Brasilien)

### <a name="localechanged"></a>LOCALE_CHANGED

Android generiert `android.intent.action.LOCALE_CHANGED` Wenn der Benutzer die Auswahl der Sprache ändert.

Aktivitäten können sich entscheiden, zum Behandeln dieses durch Festlegen der `android:configChanges` Attribut für die Aktivität wie folgt:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Internationalisierung-Grundlagen in Android

Android-Lokalisierungsstrategie hat die folgenden wichtigen Teile:

- Ressourcenordner lokalisierte Zeichenfolgen, Bilder und andere Ressourcen enthält.

- `GetText` Methode, die zum Abrufen von lokalisierter Zeichenfolgen im Code verwendet wird

- `@string/id` in AXML-Dateien, um lokalisierte Zeichenfolgen automatisch in Layouts zu platzieren.

### <a name="resource-folders"></a>Ressourcenordner

Android-Anwendungen verwalten die meisten Inhalte im Ressourcenordner, z.B.:

* **Layout** -AXML-Layoutdateien enthält.
* **drawable** -Bilder und andere zeichenbaren Ressourcen enthält.
* **Werte** -Zeichenfolgen enthält.
* **unformatierte** -Datendateien enthält.

Die meisten Entwickler sind bereits vertraut sind, mit der Verwendung von **dpi** Suffixe für die **drawable** Verzeichnis mehrere Versionen eines Bilds, sodass Android wählen Sie die richtige Version für jedes Gerät bereitstellen. Derselbe Mechanismus wird verwendet, mehrere sprachübersetzungen zu ermöglichen, indem Sie die Ressourcenverzeichnisse Sprache und Kultur Bezeichner durchzuführen.

![Bildschirmabbildung von Ressourcen/Werte und Ressourcen/drawable-Ordner für mehrere kulturellen Bezeichner](localization-images/resources.png)

> [!NOTE]
> Beim Angeben einer Sprache auf oberster Ebene, wie `es` nur zwei Zeichen erforderlich; jedoch wenn Sie ein vollständiges Gebietsschema angeben, das Format des Verzeichnisses einen Bindestrich und dem Kleinbuchstaben erfordert **r** trennen die beiden Teile, z. B. **pt-rBR** oder **Zh-rCN**. Vergleichen Sie dies mit den Rückgabewert in Code mit einem Unterstrich (z. b. `pt_BR`) angezeigt wird. Beide unterscheiden sich auf den Wert .NET `CultureInfo` Klasse verwendet, der hat eines Bindestrich nur (z. b. `pt-BR`) angezeigt wird. Behalten Sie unter Berücksichtigung dieser Unterschiede bei der Arbeit auf Xamarin-Plattformen.

#### <a name="stringsxml-file-format"></a>Dateiformat von "Strings.xml"

Eine lokalisierte **Werte** Verzeichnis (z. b. **Werte-es** oder **Werte – pt-rBR**) sollte eine Datei namens "enthalten" **von "Strings.xml"** , den übersetzten Text für dieses Gebietsschema enthält.

Jede übersetzbarer Zeichenfolge ist ein XML-Element mit den Ressourcen-ID angegeben wird, als die `name` -Attribut und die übersetzte Zeichenfolge als Wert:

```xml
<string name="app_name">TaskyL10n</string>
```

Entsprechend den üblichen XML-Regeln, mit Escapezeichen versehen werden sollen und die `name` muss eine gültige Android-Ressourcen-ID (keine Leerzeichen oder Bindestriche) sein. Hier ist ein Beispiel für die Standarddatei für die (in englischer Sprache) Zeichenfolgen für das Beispiel aus:

**values/Strings.xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

Das Verzeichnis Spanisch **Werte-es** enthält eine Datei mit dem gleichen Namen (**von "Strings.xml"**), die Übersetzungen enthält:

**values-es/Strings.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![Screenshot der mehrere Werte von Ordnern, jeweils eine Datei von "Strings.xml"](localization-images/values.png)

Mit den Zeichenfolgen Dateien eingerichtet ist können die übersetzten Werte in Layouts und Code verwiesen werden.

### <a name="axml-layout-files"></a>AXML-Layoutdateien

Um lokalisierte Zeichenfolgen in Layout-Dateien zu verweisen, verwenden die `@string/id` Syntax. Diese XML-Ausschnitt des Beispiels zeigt `text` mit festzulegende Eigenschaften lokalisierte Ressourcen-IDs (andere Attribute wurden ausgelassen):

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText-Methode

Verwenden Sie zum Abrufen von übersetzter Zeichenfolgen im Code die `GetText` Methode und übergeben Sie die Ressourcen-ID:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Menge von Zeichenfolgen

Zeichenfolgenressourcen für Android können Sie erstellen auch *Menge Zeichenfolgen* , mit denen Übersetzer unterschiedliche Übersetzungen für unterschiedliche Mengen, wie z. B. angeben:

* "Es gibt 1 Aufgabe nach links."
* "Es gibt 2 Aufgaben immer noch dazu."

(statt einer generischen "Es sind n Aufgabe(n) links").

In der **von "Strings.xml"**

```xml
<plurals name="numberOfTasks">
   <!--
      As a developer, you should always supply "one" and "other"
      strings. Your translators will know which strings are actually
      needed for their language.
    -->
   <item quantity="one">There is %d task left.</item>
   <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

Die Verwendung der vollständigen Zeichenfolge Rendern der `GetQuantityString` Methode und übergeben die Ressourcen-ID und den Wert angezeigt werden sollen (die übergeben werden zweimal). Der zweite Parameter von Android verwendet wird, um zu bestimmen, *die* `quantity` die Zeichenfolge, die verwenden, der dritte Parameter ist der Wert, der tatsächlich in der Zeichenfolge (beide sind erforderlich) ersetzt.

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Gültige `quantity` Switches sind:

* Null
* one
* Zwei
* Einige
* many
* andere

Ausführlicher beschrieben die [Android-Dokumentation](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Wenn eine bestimmte Sprache keine benötigt "spezielle" behandeln, bei denen `quantity` Zeichenfolgen ignoriert werden (z. B. Englisch verwendet nur `one` und `other`; die Angabe einer `zero` Zeichenfolge hat keine Auswirkungen, es wird nicht verwendet werden).

### <a name="images"></a>Bilder

Lokalisierter Bilder gelten dieselben Regeln als Zeichenfolgen-Dateien: alle Images, die auf die verwiesen wird in der Anwendung platziert werden soll, im **drawable** Verzeichnisse, es gibt also ein Fallback.

Gebietsschemaspezifische Images dann platziert werden soll in qualifizierten drawable-Ordner z. B. **drawable-es** oder **drawable-ja** (dpi-Spezifizierer können auch hinzugefügt werden).

In diesem Screenshot vier Bilder gespeichert sind, der **drawable** Directory, aber nur eine **flag.png**, wurde die Kopien in anderen Verzeichnissen lokalisiert.

![Bildschirmabbildung von mehreren drawable-Ordner, jeweils eine oder mehrere lokalisierte PNG-Dateien](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Andere Ressourcentypen

Sie können auch andere Arten von Alternativ sprachspezifische Ressourcen, einschließlich Layouts, Animationen und raw-Dateien bereitstellen. Dies bedeutet, Sie einem bestimmten Bildschirmlayout für eine oder mehrere der Ihre Zielsprache bereitstellen konnte, könnte z. B. Sie ein Layout speziell für Deutsch erstellen, die sehr lange Beschriftungen ermöglicht.

Android 4.2 führte die Unterstützung für [von rechts nach links (RTL) Sprachen](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) setzen Sie die anwendungseinstellung `android:supportsRtl="true"`. Die ressourcenqualifizierer `"ldrtl"` kann in den Namen eines Verzeichnisses, um benutzerdefinierte Layouts enthalten, die entwickelt wurden, für die Anzeige von RNL eingeschlossen werden.

Weitere Informationen zu Resource Directory-Namen und Fallback, finden Sie in der Android-Dokumentation [Bereitstellen alternativer Ressourcen](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>App-name

Der Anwendungsname ist einfach, mit dem Lokalisieren einer `@string/id` im für die `MainLauncher` Aktivität:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Rechts-nach-links (RTL)-Sprachen

Android 4.2 und höher bietet vollständige Unterstützung für RTL-Layouts, ausführlich beschrieben die [Native Unterstützung von RTL Blog](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

Bei Verwendung von Android 4.2 (API-Ebene 17) und höher, Ausrichtung, die Werte angegeben werden können, mit `start` und `end` anstelle von `left` und `right` (z. B. `android:paddingStart`). Es gibt auch neue APIs wie `LayoutDirection`, `TextDirection`, und `TextAlignment` zum Erstellen von Bildschirmen, die sich anpassen für RTL-Reader.

Der folgende Screenshot zeigt die [lokalisierte **Tasky** Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch:

[![Screenshot der app, die arabische Tasky](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

Der nächste Screenshot zeigt die [lokalisierte **Tasky** Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) – Hebräisch:

[![Screenshot der Tasky app – Hebräisch](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

RNL-Text ist mit dem lokalisiert **von "Strings.xml"** Dateien in die gleiche Weise wie LTR-Text.

## <a name="testing"></a>Test

Stellen Sie sicher, dass das Standardgebietsschema gründlich zu testen. Die Anwendung stürzt ab, wenn die Standardressourcen aus irgendeinem Grund nicht geladen werden können, (d. h. sind sie nicht vorhanden).

### <a name="emulator-testing"></a>Emulator testen

Finden Sie in Google [Tests auf einem Android-Emulator](https://developer.android.com/guide/topics/resources/localization.html#testing) Abschnitt Anweisungen zum Festlegen ein Emulators auf ein bestimmtes Gebietsschema, das mit der ADB-Shell.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Gerätetests

Um auf einem Gerät zu testen, ändern Sie die Sprache in der **Einstellungen** app.
**Tipp:** machen Sie sich die Symbole und den Speicherort der Menüelemente, damit Sie die Sprache, die die ursprüngliche Einstellung wiederherstellen können.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Grundlagen der Android-Anwendungen mit der integrierten Ressourcenbehandlung lokalisieren. Finden Sie weitere Informationen zu i18n und L10n für iOS, Android und Cross-Platform (einschließlich Xamarin.Forms)-apps in [in der vorliegenden plattformübergreifende](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>Verwandte Links

- [Tasky (im Code lokalisiert) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Lokalisieren von Ressourcen von Android](http://developer.android.com/guide/topics/resources/localization.html)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS-Lokalisierung](~/ios/app-fundamentals/localization/index.md)
