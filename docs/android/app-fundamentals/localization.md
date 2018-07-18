---
title: Android Lokalisierung
description: Dieses Dokument führt die Lokalisierungsfunktionen von Android SDK und Zugriff darauf mit Xamarin.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 076cadd16c3953ee4e06193190b59035ad57f2c1
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798604"
---
# <a name="android-localization"></a>Android Lokalisierung

_Dieses Dokument führt die Lokalisierungsfunktionen von Android SDK und Zugriff darauf mit Xamarin._

## <a name="android-platform-features"></a>Android-Plattformfeatures

Dieser Abschnitt beschreibt die wichtigsten Lokalisierungsfunktionen von Android. Fahren Sie mit der [nächsten Abschnitt](#basics) zu bestimmten Code und anhand von Beispielen finden Sie unter.

### <a name="locale"></a>Gebietsschema

Benutzer wählen Sie ihre Sprache **Einstellungen > Language & Eingabe**. Diese Auswahl steuert die Anzeigesprache und die regionalen Einstellungen verwendet (z. b. für Datum und Zahlenformate).

Das aktuelle Gebietsschema abgefragt werden kann, über des aktuellen Kontexts `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Dieser Wert wird ein Gebietsschemabezeichner sein, der ein Sprachcode und eines Gebietsschemacodes, getrennt durch einen Unterstrich enthält. Zu Referenzzwecken im folgenden ist ein [Liste der Java-Gebietsschemas](http://www.oracle.com/technetwork/java/javase/locales-137662.html) und [Android unterstützt Gebietsschemas über StackOverflow](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Gängige Beispiele:

* `en_US` für Englisch (USA Statees)
* `es_ES` für Spanisch (Spanien)
* `ja_JP` für Japanisch (Japan)
* `zh_CN` für Chinesisch (China)
* `zh_TW` für Chinesisch (Taiwan)
* `pt_PT` für Portugiesisch (Portugal)
* `pt_BR` für Portugiesisch (Brasilien)

### <a name="localechanged"></a>LOCALE_CHANGED

Android generiert `android.intent.action.LOCALE_CHANGED` Wenn der Benutzer ihre Sprachauswahl ändert.

Aktivitäten können optional auch angeben, zum Behandeln dieses durch Festlegen der `android:configChanges` -Attribut der Aktivität wie folgt:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Internationalisierung-Grundlagen in Android

Android Lokalisierungsstrategie besteht aus den folgenden Schlüssel Teilen:

- Ressourcenordner des lokalisierten Zeichenfolgen, Bilder und andere Ressourcen enthalten.

- `GetText` Methode, die zum Abrufen von lokalisierter Zeichenfolgen im Code verwendet wird

- `@string/id` in AXML-Dateien, um lokalisierte Zeichenfolgen automatisch in Layouts zu platzieren.

### <a name="resource-folders"></a>Ressourcenordner des

Verwalten von Android-Anwendungen die meisten Inhalt im Ressourcenordner, z. B.:

* **Layout** -AXML-Layout-Dateien enthält.
* **zeichenbaren** -Bilder und andere zeichenbaren Ressourcen enthält.
* **Werte** -Zeichenfolgen enthält.
* **unformatierte** -Datendateien enthält.

Die meisten Entwickler sind bereits vertraut sind, mit der Verwendung von **dpi** Suffixen auf die **zeichenbaren** Verzeichnis mehrere Versionen eines Bilds, lassen die richtige Version für jedes Gerät wählen Sie Android bereitstellen. Derselbe Mechanismus wird verwendet, um mehrere sprachübersetzungen Breitzeichenformat Ressourcenverzeichnisse mit Sprache und Kultur Bezeichnern bereitzustellen.

![Bildschirmabbildung von Ressourcen und Ausgaben möglich und Ressourcen-Werte für mehrere kulturellen Bezeichner](localization-images/resources.png)

> [!NOTE]
> Beim Angeben einer anderen Sprache wie der obersten Ebene `es` nur zwei Zeichen erforderlich sind, aber wenn Sie ein vollständige Gebietsschema angeben, das Format des Verzeichnisses einen Bindestrich und Kleinbuchstaben erfordert **r** trennen Sie die beiden Teile, z. B. **pt rBR** oder **Zh-rCN**. Vergleichen Sie dies mit den Rückgabewert in Code mit einem Unterstrich (z. b. `pt_BR`) angezeigt wird. Beide Fälle unterscheiden sich auf den Wert .NET `CultureInfo` -Klasse verwendet, verfügt über einen Bindestrich nur (z. b. `pt-BR`) angezeigt wird. Behalten Sie unter Berücksichtigung dieser Unterschiede beim Xamarin plattformübergreifend zu arbeiten.

#### <a name="stringsxml-file-format"></a>Strings.XML-Dateiformat

Eine lokalisierte **Werte** Verzeichnis (z. b. **Werte-es** oder **Werte-pt-rBR**) dürfen eine Datei namens **Strings.xml** , den übersetzten Text für dieses Gebietsschema enthält.

Jede übersetzbare Zeichenfolge ist ein XML-Element mit den Ressourcen-ID angegeben wird, als die `name` Attribut und die übersetzte Zeichenfolge als Wert:

```xml
<string name="app_name">TaskyL10n</string>
```

Sie müssen gemäß dem üblichen XML-Regeln mit Escapezeichen und die `name` muss eine gültige Android-Ressourcen-ID (keine Leerzeichen oder Bindestriche) sein. Hier ist ein Beispiel für die Standarddatei für die Zeichenfolgen (Englisch) für das Beispiel aus:

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

Das Verzeichnis Spanisch **Werte-es** enthält eine Datei mit dem gleichen Namen (**Strings.xml**), die Übersetzungen enthält:

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

![Screenshot der mehrere Werte Ordnern, jeweils eine Datei Strings.xml](localization-images/values.png)

Mit der Zeichenfolgen-Dateien-Installation können die übersetzten Werte in Layouts und Code verwiesen werden.

### <a name="axml-layout-files"></a>AXML Layout-Dateien

Um lokalisierte Zeichenfolgen in Layout-Dateien zu verweisen, verwenden die `@string/id` Syntax. Diese XML-Ausschnitt des Beispiels zeigt `text` Eigenschaftensatz mit lokalisierten Ressourcen-IDs (einige andere Attribute haben ausgelassen):

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

Android Zeichenfolgenressourcen ermöglichen die Erstellung *Menge Zeichenfolgen* Übersetzer zu andere Übersetzungen für unterschiedliche Mengen bereitstellen, z. B. ermöglichen:

* "Es ist links 1-Vorgang."
* "Es sind 2 Aufgaben weiterhin hierzu."

(statt eine generische "stehen links n Ausgabedialogfeld").

In der **Strings.xml**

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

Um die Verwendung der vollständigen Zeichenfolge zu Rendern der `GetQuantityString` -Methode auf und übergibt die Ressourcen-ID und der Wert angezeigt werden sollen (die übergeben wird zweimal). Der zweite Parameter wird von Android verwendet, um zu bestimmen, *der* `quantity` Zeichenfolge verwenden, der dritte Parameter ist der Wert, der tatsächlich in der Zeichenfolge (beide sind erforderlich) eingesetzt.

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Gültige `quantity` -Switches sind:

* Null
* one
* zwei
* Einige
* many
* andere

Sie sind im ausführlicher beschrieben die [Android Docs](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Wenn eine bestimmte Sprache nicht erforderlich ist "spezielles" behandeln, bei denen `quantity` Zeichenfolgen ignoriert werden (z. B. Englisch verwendet nur `one` und `other`; die Angabe einer `zero` Zeichenfolge hat keine Auswirkungen, es wird nicht verwendet werden).

### <a name="images"></a>Bilder

Lokalisierte Bilder, gelten dieselben Regeln wie Zeichenfolgen-Dateien: aller Images verwiesen wird, in der Anwendung sollte **zeichenbaren** Verzeichnisse, daher ein Fallback besteht.

Gebietsschemaspezifische Bilder sollte dann wie im qualifizierten zeichenbaren Ordner platziert **zeichenbaren vorangestellten** oder **zeichenbaren Japan** (dpi-Spezifizierer können auch hinzugefügt werden).

In diesem Screenshot vier Bilder gespeichert sind, der **zeichenbaren** Directory, aber nur einer **flag.png**, Kopien in anderen Verzeichnissen lokalisiert wurde.

![Screenshot des mehrere zeichenbaren Ordner, jeweils eine oder mehrere lokalisierte PNG-Dateien](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Andere Typen

Sie können auch andere Arten von Alternativ sprachspezifische Ressourcen, einschließlich Layouts, Animationen und raw-Dateien angeben. Das bedeutet, dass Sie eine bestimmte bildschirminstanz Layout für ein oder mehrere Ihrer Zielsprachen bereitstellen konnte, konnte Sie z. B. ein Layout speziell für Deutsch erstellen, die sehr lange Beschriftungen zulässt.

Führte die Unterstützung für Android 4.2 [von rechts nach links (RTL) Sprachen](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) , wenn Sie festlegen, dass die anwendungseinstellung `android:supportsRtl="true"`. Der ressourcenqualifizierer `"ldrtl"` in einen Namen Direcory benutzerdefinierte Layouts enthalten, die entwickelt wurden, für die Anzeige von RNL eingeschlossen werden können.

Weitere Informationen zur Benennung von Ressource Verzeichnis und Fallback finden Sie in Android Dokumente für [Bereitstellen alternativer Ressourcen](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>App-name

Der Anwendungsname ist einfach zu lokalisieren mithilfe einer `@string/id` in für die `MainLauncher` Aktivität:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Sprachen von rechts nach links (RTL)

Android 4.2 und höher bietet vollständige Unterstützung für RTL-Layouts, ausführlich beschrieben unter der [Blog Native RTL unterstützen](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

Bei Verwendung von Android 4.2 (API-Ebene 17) und höher, Ausrichtung keine besondere Vorschrift Werte angegeben werden können, mit `start` und `end` anstelle von `left` und `right` (z. B. `android:paddingStart`). Es gibt auch neue APIs wie `LayoutDirection`, `TextDirection`, und `TextAlignment` zur Erstellung der Bildschirme, die Anpassung für RTL-Reader.

Der folgende Screenshot zeigt die [lokalisierte **Tasky** Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch:

[![Screenshot der Tasky-app in Arabisch](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

Der nächste Screenshot zeigt die [lokalisierte **Tasky** Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Hebräisch:

[![Screenshot des Tasky app – Hebräisch](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

RTL-Text ist mit dem lokalisiert **Strings.xml** Dateien auf die gleiche Weise wie LTR Text.

## <a name="testing"></a>Test

Stellen Sie sicher, dass das Standardgebietsschema gründlich testen. Ihre Anwendung stürzt auf, wenn die Standardressourcen aus irgendeinem Grund (d. h. sind sie nicht vorhanden) geladen werden können.

### <a name="emulator-testing"></a>Emulator testen

Finden Sie in Google [Tests auf einem Android-Emulator](https://developer.android.com/guide/topics/resources/localization.html#testing) Abschnitt Anweisungen zum Festlegen ein Emulators auf ein bestimmtes Gebietsschema, das mit der ADB-Shell.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Test des Geräts

Um auf einem Gerät zu testen, ändern Sie die Sprache in der **Einstellungen** app.
**Tipp:** machen Sie sich die Symbole und den Speicherort der Menüelemente, damit Sie die Sprache der ursprünglichen Einstellung wiederherstellen können.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Grundlagen zum Lokalisieren von Android-Anwendungen die integrierten Ressource ""-Behandlung verwenden. Weitere Informationen zum i18n und L10n erfahren Sie für iOS, Android und plattformübergreifende (einschließlich Xamarin.Forms)-apps in [dieses Handbuch plattformübergreifende](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>Verwandte Links

- [Tasky (im Code lokalisiert) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android Lokalisieren von Ressourcen](http://developer.android.com/guide/topics/resources/localization.html)
- [Plattformübergreifende Lokalisierung (Übersicht)](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms Lokalisierung](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS Lokalisierung](~/ios/app-fundamentals/localization/index.md)
