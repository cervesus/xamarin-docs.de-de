---
title: Android-Lokalisierung
description: Dieses Dokument enthält eine Einführung in die Lokalisierungs Features der Android SDK und wie Sie mit xamarin darauf zugreifen können.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: c76eb8c6553768af68687ab6a45447a16775940e
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119140"
---
# <a name="android-localization"></a>Android-Lokalisierung

_Dieses Dokument enthält eine Einführung in die Lokalisierungs Features der Android SDK und wie Sie mit xamarin darauf zugreifen können._

## <a name="android-platform-features"></a>Android-Plattformfeatures

In diesem Abschnitt werden die wichtigsten Lokalisierungs Features von Android beschrieben. Fahren Sie mit dem [nächsten Abschnitt](#basics) fort, um einen bestimmten Code und Beispiele anzuzeigen.

### <a name="locale"></a>Gebietsschema

Benutzer wählen Ihre Sprache in den **Einstellungen > sprach & Eingabe**aus. Diese Auswahl steuert sowohl die angezeigte Sprache als auch die verwendeten regionalen Einstellungen (z. b. für Datums-und Zahlen Formatierung).

Das aktuelle Gebiets Schema kann über den aktuellen Kontext `Resources`abgefragt werden:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Dieser Wert ist ein Gebiets Schema Bezeichner, der sowohl einen Sprachcode als auch einen Gebiets Schema Code enthält, der durch einen Unterstrich getrennt ist. Im folgenden finden Sie eine [Liste der Java](https://www.oracle.com/technetwork/java/javase/locales-137662.html) -Gebiets Schemas und von [Android unterstützten Gebiets Schemas über StackOverflow](https://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Gängige Beispiele:

- `en_US`für Englisch (USA)
- `es_ES`für Spanisch (Spanien)
- `ja_JP`für Japanisch (Japan)
- `zh_CN`für Chinesisch (China)
- `zh_TW`für Chinesisch (Taiwan)
- `pt_PT`für Portugiesisch (Portugal)
- `pt_BR`für Portugiesisch (Brasilien)

### <a name="locale_changed"></a>LOCALE_CHANGED

Android generiert `android.intent.action.LOCALE_CHANGED` , wenn der Benutzer seine Sprachauswahl ändert.

Aktivitäten können dies durch Festlegen des `android:configChanges` -Attributs auf die-Aktivität, wie folgt, festlegen:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Grundlagen der Internationalisierung in Android

Die Lokalisierungsstrategie von Android besteht aus den folgenden Hauptkomponenten:

- Ressourcen Ordner, die lokalisierte Zeichen folgen, Bilder und andere Ressourcen enthalten.

- `GetText`-Methode, die verwendet wird, um lokalisierte Zeichen folgen im Code abzurufen.

- `@string/id`in axml-Dateien, um lokalisierte Zeichen folgen automatisch in Layouts zu platzieren.

### <a name="resource-folders"></a>Ressourcen Ordner

Android-Anwendungen verwalten die meisten Inhalte in Ressourcen Ordnern, wie z. b.:

- **Layout** : enthält axml-Layoutdateien.
- **drawable** -enthält Bilder und andere drawable-Ressourcen.
- **Values** -enthält Zeichen folgen.
- **RAW** -enthält Datendateien.

Die meisten Entwickler sind bereits mit der Verwendung von **dpi** -Suffixen im **drawable** -Verzeichnis vertraut, um mehrere Versionen eines Images bereitzustellen, sodass Android die richtige Version für jedes Gerät auswählen kann. Der gleiche Mechanismus wird zum Bereitstellen mehrerer Sprachübersetzungen verwendet, indem Ressourcen Verzeichnisse mit Sprach-und Kultur bezeichlern angehängt werden.

![Screenshot der Ordner "Resources/drawable" und "Resources/values" für mehrere Kultur Bezeichner](localization-images/resources.png)

> [!NOTE]
> Wenn eine Sprache `es` der obersten Ebene angegeben wird, sind nur zwei Zeichen erforderlich. Wenn Sie jedoch ein vollständiges Gebiets Schema angeben, benötigt das Verzeichnisnamen Format einen Bindestrich und einen Kleinbuchstaben, um die beiden Teile voneinander zu trennen, z. **b. pt-RBR** oder  **ZH-RCN**. Vergleichen Sie dies mit dem im Code zurückgegebenen Wert, der einen Unterstrich aufweist (z. b. `pt_BR`) angezeigt wird. Beide unterscheiden sich von dem Wert, der `CultureInfo` von der .NET-Klasse verwendet wird, der nur einen Bindestrich aufweist (z. b. `pt-BR`) angezeigt wird. Beachten Sie diese Unterschiede, wenn Sie über xamarin-Plattformen arbeiten.

#### <a name="stringsxml-file-format"></a>Strings. XML-Dateiformat

Ein lokalisiertes **Werte** Verzeichnis (z. b. **Values-es** oder **Values-PT-RBR**) sollte eine Datei mit dem Namen **Strings. XML** enthalten, die den übersetzten Text für dieses Gebiets Schema enthält.

Jede übersetzbare Zeichenfolge ist ein XML-Element mit der Ressourcen- `name` ID, die als-Attribut angegeben ist, und der übersetzten Zeichenfolge als Wert:

```xml
<string name="app_name">TaskyL10n</string>
```

Sie müssen gemäß den `name` normalen XML-Regeln eine Escapesequenz durcharbeiten, und muss eine gültige Android-Ressourcen-ID (keine Leerzeichen oder Bindestriche) sein. Im folgenden finden Sie ein Beispiel für die Standard Zeichenfolgen-Datei (Englisch) für das Beispiel:

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

Die spanischen Verzeichnis **Werte-es** enthält eine Datei mit dem gleichen Namen (**Strings. XML**), die die Übersetzungen enthält:

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

![Screenshot mehrerer Werte Ordner, die jeweils eine Strings. XML-Datei enthalten](localization-images/values.png)

Bei der Einrichtung der Zeichen folgen Dateien können sowohl in Layouts als auch im Code auf die übersetzten Werte verwiesen werden.

### <a name="axml-layout-files"></a>Axml-Layoutdateien

Verwenden Sie die `@string/id` Syntax, um auf lokalisierte Zeichen folgen in Layoutdateien zu verweisen. Dieser XML-Code Ausschnitt aus dem Beispiel `text` zeigt die Eigenschaften, die mit lokalisierten Ressourcen-IDs festgelegt werden (einige andere Attribute wurden ausgelassen):

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

Um übersetzte Zeichen folgen im Code abzurufen, `GetText` verwenden Sie die-Methode, und übergeben Sie die Ressourcen-ID:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Mengen Zeichenfolgen

Mit Android-Zeichen folgen Ressourcen können Sie auch *Mengen* Zeichenfolgen erstellen, mit denen Konvertierungen verschiedene Übersetzungen für unterschiedliche Mengen bereitstellen können, z.b.:

- "Es ist 1 Aufgabe übrig."
- "Es sind noch zwei Aufgaben zu erledigen."

(anstelle eines generischen "Es sind n Aufgaben vorhanden").

In der Datei " **Strings. XML** "

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

Verwenden Sie zum Rendering der gesamten Zeichen `GetQuantityString` Folge die-Methode, und übergeben Sie dabei die Ressourcen-ID und den anzuzeigenden Wert (der zweimal übergeben wird). Der zweite Parameter wird von Android verwendet, um zu bestimmen, *welche* `quantity` Zeichenfolge verwendet werden soll. der dritte Parameter ist der Wert, der tatsächlich in die Zeichenfolge ersetzt wird (beides ist erforderlich).

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Gültige `quantity` Switches sind:

- Null
- one
- Zweikampf
- nur
- many
- andere

Diese werden in der [Android](https://developer.android.com/guide/topics/resources/string-resource.html#Plurals)-Dokumentation ausführlicher beschrieben. Wenn eine bestimmte Sprache keine ' besondere ' Behandlung erfordert, werden diese `quantity` Zeichen folgen ignoriert (z. b. wird nur in `one` englischer `other`Sprache verwendet, `zero` und die Angabe einer Zeichenfolge hat keine Auswirkung, Sie wird nicht verwendet).

### <a name="images"></a>Bilder

Lokalisierte Images folgen denselben Regeln wie Zeichen folgen Dateien: alle Bilder, auf die in der Anwendung verwiesen wird, sollten in **drawable** -Verzeichnissen abgelegt werden, sodass ein Fallback vorliegt.

Gebiets Schema spezifische Images sollten dann in qualifizierte drawable-Ordner platziert werden, z. b. **drawable-es** oder **drawable-ja** (dpi-Spezifizierer können auch hinzugefügt werden).

In diesem Screenshot werden vier Bilder im **drawable** -Verzeichnis gespeichert, aber nur eine, " **Flag. png**", hat lokalisierte Kopien in anderen Verzeichnissen.

![Screenshot mehrerer drawable-Ordner, die jeweils eine oder mehrere lokalisierte PNG-Dateien enthalten](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Andere Ressourcentypen

Sie können auch andere Arten alternativer, sprachspezifischer Ressourcen bereitstellen, einschließlich Layouts, Animationen und Rohdatendateien. Dies bedeutet, dass Sie ein bestimmtes Bildschirmlayout für eine oder mehrere Zielsprachen bereitstellen können, z. b. Wenn Sie ein Layout speziell für Deutsch erstellen, das sehr lange Text Bezeichnungen zulässt.

Android 4,2 hat Unterstützung für [Sprachen von rechts nach links (RTL)](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) eingeführt, wenn Sie `android:supportsRtl="true"`die Anwendungs Einstellung festlegen. Der Ressourcen Qualifizierer `"ldrtl"` kann in einen Verzeichnisnamen eingefügt werden, um benutzerdefinierte Layouts zu enthalten, die für die RTL-Anzeige entworfen wurden.

Weitere Informationen zur Benennung und zum Fallback von Ressourcen Verzeichnissen finden Sie in der Android-Dokumentation zum [Bereitstellen alternativer Ressourcen](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>App-Name

Der Anwendungsname kann leicht lokalisiert werden, indem ein `@string/id` in für die `MainLauncher` -Aktivität verwendet wird:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Rechts-nach-links (RTL)-Sprachen

Android 4,2 und neuere Version bieten vollständige Unterstützung für RTL-Layouts, die im systemeigenen [Support Blog von RTL](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html)ausführlich beschrieben werden.

Bei Verwendung von Android 4,2 (API-Ebene 17) und neuer können Ausrichtungs Werte mit `start` und `end` anstelle von `left` und `right` angegeben werden ( `android:paddingStart`z. b.). Es gibt auch neue APIs `LayoutDirection`, z. b., `TextDirection`und `TextAlignment` , um Bildschirme zu erstellen, die sich für RTL-Leser anpassen.

Der folgende Screenshot zeigt das [lokalisierte **Tasky** -Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) auf Arabisch:

[![Screenshot der Tasky-App auf Arabisch](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

Der nächste Screenshot zeigt das [lokalisierte **Tasky** -Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Hebräisch:

[![Screenshot der Tasky-app in Hebräisch](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

Der RTL-Text wird mithilfe von **Strings. XML** -Dateien auf dieselbe Weise wie Ltr-Text lokalisiert.

## <a name="testing"></a>Test

Stellen Sie sicher, dass Sie das Standard Gebiets Schema gründlich testen. Die Anwendung stürzt ab, wenn die Standard Ressourcen aus irgendeinem Grund nicht geladen werden können (d. h., Sie fehlen).

### <a name="emulator-testing"></a>Emulator-Tests

Anweisungen zum Festlegen eines Emulators auf ein bestimmtes Gebiets Schema mithilfe der ADB-Shell finden Sie im Abschnitt [Testen von Google in einem Android-Emulator](https://developer.android.com/guide/topics/resources/localization.html#testing) Abschnitt.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Gerätetests

Um auf einem Gerät zu testen, ändern Sie die Sprache in der App " **Einstellungen** ".

> [!TIP]
> Notieren Sie sich die Symbole und den Speicherort der Menü Elemente, damit Sie die Sprache auf die ursprüngliche Einstellung zurücksetzen können.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden die Grundlagen der Lokalisierung von Android-Anwendungen mithilfe der integrierten Ressourcen Behandlung behandelt. Weitere Informationen zu i18n und l10n für IOS-, Android-und plattformübergreifende Apps (einschließlich xamarin. Forms) finden Sie in [diesem plattformübergreifenden Leitfaden](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>Verwandte Links

- [Tasky (lokalisiert im Code) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android-Lokalisierung mit Ressourcen](https://developer.android.com/guide/topics/resources/localization.html)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin. Forms-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS Localization (iOS-Lokalisierung)](~/ios/app-fundamentals/localization/index.md)
