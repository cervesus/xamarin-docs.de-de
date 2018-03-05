---
title: iOS Lokalisierung
description: Dieses Dokument behandelt die Lokalisierungsfunktionen des iOS-SDK und Zugriff darauf mit Xamarin.
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS Lokalisierung

_Dieses Dokument behandelt die Lokalisierungsfunktionen des iOS-SDK und Zugriff darauf mit Xamarin._

Finden Sie in der [Internationalisierung Codierungen](encodings.md) Anweisungen einschließlich Zeichen legt/Codepages in Anwendungen, die nicht-Unicode-Daten verarbeiten müssen.

## <a name="ios-platform-features"></a>iOS-Plattform-Funktionen

In diesem Abschnitt werden einige der Lokalisierungsfunktionen in iOS beschrieben. Fahren Sie mit der [nächsten Abschnitt](#basics) zu bestimmten Code und anhand von Beispielen finden Sie unter.

### <a name="language"></a>Sprache

Benutzer wählen Sie ihre Sprache die **Einstellungen** app. Diese Einstellung wirkt sich auf die sprachenzeichenfolgen und die Bilder, die vom Betriebssystem als auch Anwendungen, die erkennen, spracheinstellungen angezeigt.

Dies ist, in denen Benutzer entscheidet, ob Englisch, Spanisch, Japanisch, Französisch oder anderen Sprache angezeigt, die in ihren apps angezeigt werden sollen.

Die tatsächliche Liste der unterstützten Sprachen im iOS 7 ist: Englisch (USA), Englisch (Großbritannien), Chinesisch (vereinfacht), Chinesisch (traditionell), Französisch, Deutsch, Italienisch, Japanisch, Koreanisch, Spanisch, Arabisch, Katalanisch, Kroatisch, Tschechisch, Dänisch, Niederländisch, Finnisch, Griechisch, Hebräisch, Ungarisch, Indonesisch, Malaiisch, Norwegisch, Polnisch, Portugiesisch, Portugiesisch (Brasilien), Rumänisch, Russisch, Slowakisch, Schwedisch, Thai, Türkisch, Ukrainisch, Vietnamesisch Englisch (Australien), Spanisch (Mexiko).

Die aktuelle Sprache abgefragt werden kann, durch den Zugriff auf das erste Element des der `PreferredLanguages` Array:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Dieser Wert wird ein Sprachcode sein, z. B. `en` für Englisch, `es` für Spanisch `ja` usw. für Japanisch. _Der zurückgegebene Wert ist beschränkt auf einen der lokalisierten Versionen von der Anwendung (mithilfe von fallback-Regeln bestimmt die beste Übereinstimmung) unterstützt._

Anwendungscode muss nicht immer für diesen Wert - Xamarin überprüfen, und beide iOS bieten Funktionen, mit deren Hilfe die Sprache des Benutzers automatisch die richtige Zeichenfolge oder eine Ressource bereit. Diese Funktionen werden im weiteren Verlauf dieses Dokuments beschrieben.

> [!NOTE]
> **Hinweis:** vor iOS 9 empfohlene Code war `var lang = NSLocale.PreferredLanguages [0];`.
>
> Durch diesen Code in iOS 9 - geändert zurückgegebenen Ergebnisse finden Sie unter [technischen Hinweis TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) für Weitere Informationen.
>
> Sie können weiterhin verwenden `NSLocale.PreferredLanguages [0]` zur Bestimmung der tatsächlichen Werte, die vom Benutzer ausgewählten (unabhängig von den lokalisierten Versionen Ihrer app unterstützt werden).

### <a name="locale"></a>Gebietsschema

Benutzer auswählen, deren Gebietsschema in die **Einstellungen** app. Diese Einstellung wirkt sich auf die Möglichkeit, dass Datumsangaben, Uhrzeiten, Zahlen und Währungen formatiert werden.

Dadurch können Benutzer wählen, ob sie 12 Stunden oder 24-Stunden-Zeitformate, finden Sie, ob ihre dezimale Trennzeichen ein Komma oder einen Punkt und die Reihenfolge der Tag, Monat und Jahr in Datumsanzeige wird.

Mit Xamarin haben Sie Zugriff auf beide Apple iOS-Klassen (`NSNumberFormatter`) sowie .NET Klassen im System.Globalization. Entwickler sollten Auswerten der besser für ihre Bedürfnisse geeignet ist, wie verschiedene Funktionen in jeder verfügbaren vorhanden sind. Insbesondere wenn Sie das Abrufen und Anzeigen von In-App-Käufe Preise mit StoreKit sollten Sie definitiv Apple Formatierung Klassen für die Preisinformationen zurückgegeben verwenden.

Das aktuelle Gebietsschema kann entweder durch abgefragt werden:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Der erste Wert kann vom Betriebssystem zwischengespeichert werden und daher möglicherweise nicht immer wider aktuell ausgewählten Gebietsschema des Benutzers. Verwenden Sie den zweiten Wert, um das aktuell ausgewählte Gebietsschema abzurufen.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS generiert eine `NSCurrentLocaleDidChangeNotification` Wenn der Benutzer ihre Gebietsschema aktualisiert. Anwendungen können für diese Benachrichtigung lauschen, während sie ausgeführt werden, und nehmen Sie entsprechende Änderungen an der Benutzeroberfläche.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Grundlagen der Lokalisierung in iOS

E/as die folgenden Funktionen sind in Xamarin zum Bereitstellen von lokalisierter Ressourcen für die Anzeige für den Benutzer leicht genutzt. Finden Sie in der [TaskyL10n Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) zu erfahren, wie dieser Faktoren zu implementieren.

### <a name="infoplist"></a>Info.plist

Bevor Sie beginnen, konfigurieren die **"Info.plist"** Datei mit den folgenden Schlüsseln:

- `CFBundleDevelopmentRegion` -die Standardsprache für die Anwendung (in der Regel die Sprache, die von den Entwicklern gesprochen wird, und in Storyboards und Zeichenfolge und Bildressourcen verwendet). Im folgenden Beispiel wird **En** (für Englisch) angegeben ist.
- `CFBundleLocalizations` -ein Array von anderen lokalisierten Versionen unterstützt, die von der Anwendung auch mit Language-Codes wie **vorangestellten** (Spanisch) und **pt-PT** (Portugiesisch Portugal gesprochen).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString-Methode

Die `NSBundle.MainBundle.LocalizedString` Methode sucht lokalisierten Text, der im gespeichert wurde **.strings** Dateien im Projekt. Diese Dateien sind nach Sprache, in einer speziell benannten Verzeichnisse mit organisiert ein **.lproj** Suffix.

#### <a name="strings-file-locations"></a>.Strings-Dateispeicherorte

- **Base.lproj** ist das Verzeichnis, das Ressourcen für die Standardsprache enthält.
  Häufig befindet sich im Projektstammverzeichnis (aber auch platziert werden kann die **Ressourcen** Ordner).
- **<language>.lproj** Verzeichnisse werden erstellt für jede unterstützte Sprache in der Regel der **Ressourcen** Ordner.

Kann eine Anzahl von verschiedenen **.strings** Dateien in jeder Sprachenverzeichnis:

- **Localizable.Strings** -der Hauptliste der lokalisierten Text.
- **InfoPlist.strings** – bestimmte in dieser Datei sind bestimmte Schlüssel zulässig, z. B. den Anwendungsnamen zu übersetzen.
- **< Storyboard-Name > .strings** -optionale Datei, die Übersetzungen für Elemente der Benutzeroberfläche in einem Storyboard enthält.

Die **Buildvorgang** für diese Dateien sollte **Bundle Ressource**.

#### <a name="strings-file-format"></a>.Strings-Dateiformat

Die Syntax für lokalisierte Zeichenfolgenwerte lautet:

```csharp
/* comment */
"key"="localized-value";
```

Sie sollten die folgenden Zeichen in Zeichenfolgen mit Escapezeichen versehen:

* `\"`  Anführungszeichen
* `\\`  umgekehrter Schrägstrich
* `\n`  Zeilenumbruch

Dies ist ein Beispiel **es/Localizable.strings** (d. h. Spanisch) Datei aus dem Beispiel:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Bilder

So lokalisieren Sie ein Bild im iOS:

1. Verweisen Sie auf das Bild im Code, z. B. ein:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Platzieren Sie die Bild-Standarddatei **flag.png** in **Base.lproj** (native Language Verzeichnis Entwicklung).

3. Platzieren Sie optional die lokalisierte Versionen des Bilds in **.lproj** Ordner für jede Sprache (z. b. **es.lproj**, **ja.lproj**). Verwenden Sie den gleichen Dateinamen **flag.png** in jedem Sprachenverzeichnis.

Wenn ein Bild nicht für eine bestimmte Sprache vorhanden ist, wird IOS-ausweichen auf den Standardordner der systemeigenen Sprache und lädt das Bild von dort aus.


#### <a name="launch-images"></a>Starten von Bildern

Verwenden Sie die standardmäßigen Benennungskonventionen für die Start-Bilder (und die XIB oder Storyboard für iPhone 6-Modelle) beim Platzieren sie in der **.lproj** Verzeichnisse für jede Sprache.

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>App-name

Platzieren einer **InfoPlist.strings** in der Datei ein **.lproj** Directory können Sie einige Werte aus der app zu überschreiben **"Info.plist"**, einschließlich des Namens der Anwendung:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

Andere Schlüssel, die Sie verwenden können, [Lokalisieren von Zeichenfolgen anwendungsspezifische](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) sind:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>Lokalisierung systemeigene Entwicklung Region

Die Standardzeichenfolgen (befindet sich in der **Base.lproj** Ordner) wird als gegeben angenommen der "Alternative" Sprache sein. Dies bedeutet, dass wenn eine Übersetzung im Code angefordert wird und sich nicht für befindet eine der aktuellen Sprache die **Base.lproj** Ordner durchsucht werden für die Standardzeichenfolge verwendet (wenn keine Übereinstimmung gefunden wird, die Übersetzung-Bezeichnerzeichenfolge folgt selbst ist wird angezeigt).

Entwickler können beim Fallback werden durch Festlegen des Plist an eine andere Sprache wählen `CFBundleDevelopmentRegionKey`. Der Wert sollte auf den Sprachcode für die systemeigene Entwicklungssprache festgelegt werden. Diese bildschirmabbildung zeigt, dass eine Plist in Xamarin Studio mit der systemeigenen Entwicklung Region auf Spanisch (es) festgelegt:

![](images/cfbundledevelopmentregion.png "InfoPList.strings-Eigenschaft")

Wenn die Standardsprache verwendet, in die Storyboards und während des gesamten Codes nicht Englisch ist, legen Sie diesen Wert entsprechend die systemeigene Sprache, die in der gesamten app-Code verwendet wird.

### <a name="dates-and-times"></a>Datums- und Uhrzeitangaben

Obwohl es möglich ist, verwenden Sie die integrierte .NET Datums- und Uhrzeitfunktionen (zusammen mit den aktuellen `CultureInfo`) zum Formatieren von Datumsangaben und Uhrzeiten für ein Gebietsschema würde diese gebietsschemaspezifische-benutzereinstellungen (die separat von der Sprache festgelegt werden können) ignorieren.

Verwenden Sie die iOS `NSDateFormatter` Ausgabe zu erzeugen, das Gebietsschema des Benutzers entspricht. Der folgende Beispielcode veranschaulicht die grundlegende Datum und Uhrzeit, die Formatierungsoptionen fest:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Die Ergebnisse für Englisch in den Vereinigten Staaten:

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Die Ergebnisse für Spanisch in Spanien:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Finden Sie in der Apple [Datum Formatierungsprogramme](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Dokumentation weitere Informationen. Überprüfen Sie beim Testen von gebietsschemabezogenen Datums- und uhrzeitformatierungen sowohl **iPhone Sprache** und **Region** Einstellungen.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Layout von rechts nach links (RTL)

iOS bietet eine Reihe von Funktionen, die beim Erstellen von RTL-fähige apps unterstützen:

* Verwendung **Auto-Layout** `leading` und `trailing` Attribute für das Steuerelement Ausrichtung keine besondere Vorschrift (entspricht *linken* und *rechten* für Englisch, z. B. wird jedoch umgekehrt für RTL-Sprachen).
  Die [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Steuerelement ist besonders nützlich für das Layout der Steuerelemente auf RTL-fähig sein.
* Verwendung `TextAlignment = UITextAlignment.Natural` für textausrichtung (der ist *linken* für die meisten Sprachen, aber *rechten* für RTL).
* `UINavigationController` automatisch spiegelt die Schaltfläche "zurück", und kehrt Wischen Richtung.

Die folgenden Screenshots zeigen die [lokalisierte **Tasky** Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch und Hebräisch (obwohl Englisch in die Felder eingegeben wurde):

[ ![](images/rtl-ar-sml.png "Lokalisierung in Arabisch")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "Lokalisierung – Hebräisch")](images/rtl-he.png "Hebrew")

iOS automatisch kehrt die `UINavigationController`, und die anderen Steuerelemente befinden sich in `UIStackView` oder Auto-Layout ausgerichtet.
RTL-Text ist mit dem lokalisiert **.strings** Dateien auf die gleiche Weise wie LTR Text.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalisieren von der Benutzeroberfläche im Code

Die [Tasky (im Code lokalisiert)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Beispiel zeigt, wie eine Anwendung lokalisieren, an dem die Benutzeroberfläche in Code (statt XIBs oder Storyboards) erstellt.

### <a name="project-structure"></a>Projektstruktur



![](images/solution-code.png "Ressourcen-Struktur")

### <a name="localizablestrings-file"></a>Localizable.Strings-Datei

Wie oben beschrieben die **Localizable.strings** Dateiformat besteht aus Schlüssel-Wert-Paare, in dem der Schlüssel ist eine vom Benutzer ausgewählte Zeichenfolge, die angibt

Die Spanisch (**vorangestellten**) lokalisierten Versionen des Beispiels sind unten aufgeführt:

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Die Lokalisierung ausführen

Im Anwendungscode, wo eine Benutzeroberfläche Anzeigetext festgelegt ist (ob es eine Bezeichnung, oder eine Eingabe Platzhalter usw. ist) der Code verwendet die iOS `LocalizedString` Funktion, um die ordnungsgemäße Übersetzung anzuzeigenden abzurufen:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalisieren von Storyboard-Benutzeroberflächen

Im Beispiel [Tasky (lokalisierte Storyboard)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) zeigt Informationen zum Lokalisieren von Text in Steuerelemente in einem Storyboard.

### <a name="project-structure"></a>Projektstruktur

Die **Base.lproj** Verzeichnis enthält das Storyboard und sollte auch keine Bilder, die in der Anwendung verwendeten enthalten.

Der anderen Sprache Verzeichnisse enthalten eine **Localizable.strings** -Datei für alle Zeichenfolgenressourcen im Code verwiesen sowie ein **MainStoryboard.strings** -Datei, die Übersetzungen für Text in der Storyboard.

![](images/solution-storyboard.png "Ressourcen-Struktur")

Die Sprache Verzeichnisse dürfen eine Kopie des keine Bilder, die lokalisiert wurden, um dem überschreiben, **Base.lproj**.

### <a name="object-id--localization-id"></a>Objekt-ID / Lokalisierungs-ID

Beim Erstellen und Bearbeiten von Steuerelementen in einem Storyboard, wählen Sie jedes Steuerelement, und überprüfen Sie die ID, für die Lokalisierung verwendet:

* in Visual Studio für Mac, enthält es befindet sich in den Eigenschaften aufgefüllt und aufgerufen **Lokalisierungs-ID**.
* in Xcode er heißt **Objekt-ID**.

Es ist ein Zeichenfolgenwert, der häufig ein Formular wie weist **"NF3 h8 XmR"**:

![](images/xs-designer-localization-id.png "Xcode-Ansicht des Storyboard-Lokalisierung")

Dieser Wert wird verwendet, der **.strings** Datei übersetzten Text jedes Steuerelement automatisch zugewiesen.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Das Format der Übersetzungsdatei Storyboard ähnelt der **Localizable.strings** Datei, mit der Ausnahme, dass der Schlüssel (der Wert auf der linken Seite) nicht mit benutzerdefinierten jedoch stattdessen muss ein sehr spezifisches Format aufweisen: `ObjectID.property`.

Im Beispiel **Mainstoryboard.strings** unten sehen Sie `UITextField`s haben eine `placeholder` Text-Eigenschaft, die lokalisiert werden kann; `UILabel`s haben eine `text` Eigenschafts- und `UIButton`s Standardtext festgelegt ist, mit `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **Verwenden ein Storyboard mit Größenklassen** kann dazu führen, die Übersetzungen, die nicht angezeigt werden. Dies ist möglicherweise im Zusammenhang mit [dieses Problem](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) Apple Dokumentation in den Bereich: Lokalisierung ein Storyboard oder XIB nicht ordnungsgemäß lokalisiert wird, wenn alle der folgenden drei Bedingungen erfüllt sind: die Storyboard oder XIB Größenklassen verwendet. Die Basis Lokalisierung und das Build-Ziel werden auf Universal festgelegt. Der Build-Ziele-iOS 7.0.
Die Korrektur besteht darin, Ihre Storyboarddatei von Zeichenfolgen in zwei identische Dateien doppelte: **MainStoryboard~iphone.strings** und **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "Zeichenfolgen-Dateien")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>Store-App-Angebot

Apple häufig gestellte Fragen auf folgt [App Store Lokalisierung](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) Übersetzungen für jedes Land eingeben Ihrer app für den Verkauf ist. Beachten Sie die Warnung aus, die die Übersetzungen wird nur angezeigt, wenn Ihre app auch einen lokalisierten enthält **.lproj** Verzeichnis für die Sprache.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Grundlagen zum Lokalisieren von iOS-Anwendungen, die die Verwendung der integrierten Ressource "" Verarbeitung "und" Storyboard-Funktionen.

Finden Sie weitere Informationen zum i18n und L10n für iOS, Android und plattformübergreifende apps (einschließlich Xamarin.Forms) in [dieses Handbuch plattformübergreifende](~/cross-platform/app-fundamentals/localization.md).


## <a name="related-links"></a>Verwandte Links

- [Tasky (im Code lokalisiert) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalisierte Storyboard) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple-Lokalisierungsleitfaden](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Plattformübergreifende Lokalisierung (Übersicht)](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms Lokalisierung](~/xamarin-forms/app-fundamentals/localization.md)
- [Android Lokalisierung](~/android/app-fundamentals/localization.md)
