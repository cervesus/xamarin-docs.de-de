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
ms.openlocfilehash: cc7643d89b4a45b6f6fb87bb027edb1c339a20ec
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="ios-localization"></a>iOS Lokalisierung

_Dieses Dokument behandelt die Lokalisierungsfunktionen des iOS-SDK und Zugriff darauf mit Xamarin._

Finden Sie in der [Internationalisierung Codierungen](encodings.md) Anweisungen einschließlich Zeichen legt/Codepages in Anwendungen, die nicht-Unicode-Daten verarbeiten müssen.

## <a name="ios-platform-features"></a>iOS-Plattform-Funktionen

In diesem Abschnitt werden einige der Lokalisierungsfunktionen in iOS beschrieben. Fahren Sie mit der [nächsten Abschnitt](#basics) zu bestimmten Code und anhand von Beispielen finden Sie unter.

### <a name="language"></a>Sprache

Benutzer wählen Sie ihre Sprache die **Einstellungen** app. Diese Einstellung wirkt sich auf die Language-Zeichenfolgen und Bilder, die vom Betriebssystem und apps angezeigt. 

Um zu bestimmen, die Sprache, die in einer app verwendet wird, erhalten Sie das erste Element des `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Dieser Wert wird ein Sprachcode sein, z. B. `en` für Englisch, `es` für Spanisch `ja` usw. für Japanisch. Der zurückgegebene Wert ist beschränkt auf einen der lokalisierten Versionen von der Anwendung (mithilfe von fallback-Regeln bestimmt die beste Übereinstimmung) unterstützt.

Anwendungscode muss nicht immer für diesen Wert – Xamarin überprüfen, und beide iOS bieten Funktionen, mit deren Hilfe die Sprache des Benutzers automatisch die richtige Zeichenfolge oder eine Ressource bereit. Diese Funktionen werden im weiteren Verlauf dieses Dokuments beschrieben.

> [!NOTE]
> Verwendung `NSLocale.PreferredLanguages` bevorzugten Sprache des Benutzers unabhängig davon die lokalisierten Versionen unterstützt, die von der app zu bestimmen. Die Rückgabewerte dieser Methode geändert in iOS 9; finden Sie unter [technischen Hinweis TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Details.

### <a name="locale"></a>Gebietsschema

Benutzer auswählen, deren Gebietsschema in die **Einstellungen** app. Diese Einstellung wirkt sich auf die Möglichkeit, dass Datumsangaben, Uhrzeiten, Zahlen und Währungen formatiert werden.

Dadurch können Benutzer wählen, ob sie 12- oder 24-Stunden-Zeitformate, finden Sie, ob ihre dezimale Trennzeichen ein Komma oder einen Punkt und die Reihenfolge der Tag, Monat und Jahr in Datumsanzeige wird.

Mit Xamarin haben Sie Zugriff auf beide Apple iOS-Klassen (`NSNumberFormatter`) sowie .NET Klassen im System.Globalization. Entwickler sollten Auswerten der besser für ihre Bedürfnisse geeignet ist, wie verschiedene Funktionen in jeder verfügbaren vorhanden sind. Wenn Sie das Abrufen und Anzeigen von In-App-Käufe Preise StoreKit mit sollten Sie insbesondere Apple Formatierung Klassen für die Preisinformationen zurückgegeben verwenden.

Das aktuelle Gebietsschema kann von zwei Arten abgefragt werden:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Der erste Wert kann vom Betriebssystem zwischengespeichert werden und daher möglicherweise nicht immer wider aktuell ausgewählten Gebietsschema des Benutzers. Verwenden Sie den zweiten Wert, um das aktuell ausgewählte Gebietsschema abzurufen.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS generiert eine `NSCurrentLocaleDidChangeNotification` Wenn der Benutzer ihre Gebietsschema aktualisiert. Anwendungen können für diese Benachrichtigung lauschen, während sie ausgeführt werden und können entsprechende Änderungen an der UI vornehmen.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>Grundlagen der Lokalisierung in iOS

E/as die folgenden Funktionen sind in Xamarin zum Bereitstellen von lokalisierter Ressourcen für die Anzeige für den Benutzer leicht genutzt. Finden Sie in der [TaskyL10n Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) zu erfahren, wie dieser Faktoren zu implementieren.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Angeben der Standardsprache sowie unterstützte Sprachen in der Datei "Info.plist"

In [technische Q & A QA1828: wie iOS bestimmt, auf die Sprache für Ihre App](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple wird beschrieben, wie iOS eine Sprache für die Verwendung in einer app auswählt. Die folgenden Faktoren beeinflussen, welche Sprache angezeigt wird:

- Der Benutzer den bevorzugten Sprachen (finden der **Einstellungen** app)
- Die lokalisierten Versionen gebündelt mit der app (.lproj Ordner)
- `CFBundleDevelopmentRegion` (**"Info.plist"** Wert, der die Standardsprache für die app angibt)
- `CFBundleLocalizations` (**"Info.plist"** Array gibt alle unterstützte lokalisierten Versionen)

Wie in der technischen Fragen und Antworten angegeben `CFBundleDevelopmentRegion` eine app standardmäßig Region und Sprache darstellt. Wenn die Anwendung explizit eines Benutzers bevorzugten Sprachen unterstützt, wird die nach diesem Feld angegebene Sprache verwendet. 

> [!IMPORTANT]
> iOS 11 gilt diese Sprache Auswahlmechanismus strenger als in vorherigen Versionen des Betriebssystems. Deswegen jeder iOS 11-app, die die unterstützten lokalisierten Versionen – entweder durch einschließlich .lproj Ordner oder einen Wert für nicht explizit deklarieren `CFBundleLocalizations` – möglicherweise eine andere Sprache in iOS 11 angezeigt, als in iOS 10 aufweisen.

Wenn `CFBundleDevelopmentRegion` wurde nicht angegeben wurde, der **"Info.plist"** Datei, die Xamarin.iOS Buildtools derzeit den Standardwert verwenden `en_US`. Während dies in einer zukünftigen Version möglicherweise geändert werden, bedeutet dies, dass die Standardsprache Englisch ist.

Um sicherzustellen, dass Ihre app eine erwartete Sprache auswählt, gehen Sie folgendermaßen vor:

- Geben Sie eine Standardsprache an. Open **"Info.plist"** und Verwenden der **Quelle** Ansicht, um einen Wert für die `CFBundleDevelopmentRegion` Schlüssel; in XML Aussehen ähnlich der folgenden:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

In diesem Beispiel anhand "des Typs es" angeben, wenn keines Benutzers bevorzugten Sprachen unterstützt werden, standardmäßig auf Spanisch.

- Alle unterstützte lokalisierten Versionen zu deklarieren. In **"Info.plist"**, verwenden Sie die **Quelle** Ansicht, um ein Array für die `CFBundleLocalizations` Schlüssel; in XML Aussehen ähnlich der folgenden:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Xamarin.iOS-apps, die .NET Mechanismen lokalisiert wurden, z. B. resx-Dateien, die diese bereitstellen müssen **"Info.plist"** außerdem Funktionsrückgabewerte.

Weitere Informationen zu diesen **"Info.plist"** Schlüssel, sehen Sie sich die Apple [Informationen Eigenschaftsverweis Liste Schlüssel](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="localizedstring-method"></a>LocalizedString-Methode

Die `NSBundle.MainBundle.LocalizedString` Methode sucht lokalisierten Text, der im gespeichert wurde **.strings** Dateien im Projekt. Diese Dateien sind nach Sprache, in einer speziell benannten Verzeichnisse mit organisiert ein **.lproj** Suffix.

#### <a name="strings-file-locations"></a>.Strings-Dateispeicherorte

- **Base.lproj** ist das Verzeichnis, das Ressourcen für die Standardsprache enthält.
  Häufig befindet sich im Projektstammverzeichnis (aber auch platziert werden kann die **Ressourcen** Ordner).
- **<language>.lproj** Verzeichnisse werden erstellt für jede unterstützte Sprache in der Regel der **Ressourcen** Ordner.

Kann eine Anzahl von verschiedenen **.strings** Dateien in jeder Sprachenverzeichnis:

- **Localizable.Strings** – der Hauptliste der lokalisierten Text.
- **InfoPlist.strings** – bestimmte bestimmte Schlüssel sind in dieser Datei zulässig, z. B. den Anwendungsnamen zu übersetzen.
- **< Storyboard-Name > .strings** – optionale Datei, die Übersetzungen für Elemente der Benutzeroberfläche in einem Storyboard enthält.

Die **Buildvorgang** für diese Dateien sollte **Bundle Ressource**.

#### <a name="strings-file-format"></a>.Strings-Dateiformat

Die Syntax für lokalisierte Zeichenfolgenwerte lautet:

```console
/* comment */
"key"="localized-value";
```

Sie sollten die folgenden Zeichen in Zeichenfolgen mit Escapezeichen versehen:

* `\"`  Anführungszeichen
* `\\`  umgekehrter Schrägstrich
* `\n`  Zeilenumbruch

Dies ist ein Beispiel **es/Localizable.strings** (d. h. Spanisch) Datei aus dem Beispiel:

```console
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

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>App-name

Platzieren einer **InfoPlist.strings** in der Datei ein **.lproj** Directory können Sie einige Werte aus der app zu überschreiben **"Info.plist"**, einschließlich des Namens der Anwendung:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Andere Schlüssel, die Sie verwenden können, [Lokalisieren von Zeichenfolgen anwendungsspezifische](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) sind:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

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

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Die Ergebnisse für Spanisch in Spanien:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Finden Sie in der Apple [Datum Formatierungsprogramme](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Dokumentation weitere Informationen. Überprüfen Sie beim Testen von gebietsschemabezogenen Datums- und uhrzeitformatierungen sowohl **iPhone Sprache** und **Region** Einstellungen.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Layout von rechts nach links (RTL)

iOS bietet eine Reihe von Funktionen, die beim Erstellen von RTL-fähige apps unterstützen:

* Automatisches Layout verwenden `leading` und `trailing` Attribute für das Steuerelement Ausrichtung keine besondere Vorschrift (der für Englisch links und rechts entspricht, jedoch wird umgekehrt für RTL-Sprachen).
  Die [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Steuerelement ist besonders nützlich für das Layout der Steuerelemente auf RTL-fähig sein.
* Verwendung `TextAlignment = UITextAlignment.Natural` für textausrichtung (die für die meisten Sprachen jedoch Recht für RTL gelassen werden).
* `UINavigationController` automatisch spiegelt die Schaltfläche "zurück", und kehrt Wischen Richtung.

Die folgenden Screenshots zeigen die [lokalisierte Tasky Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch und Hebräisch (obwohl Englisch in die Felder eingegeben wurde):

[![](images/rtl-ar-sml.png "Lokalisierung in Arabisch")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalisierung – Hebräisch")](images/rtl-he.png#lightbox "Hebrew")

iOS automatisch kehrt die `UINavigationController`, und die anderen Steuerelemente befinden sich in `UIStackView` oder Auto-Layout ausgerichtet.
RTL-Text ist mit dem lokalisiert **.strings** Dateien auf die gleiche Weise wie LTR Text.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalisieren von der Benutzeroberfläche im Code

Die [Tasky (im Code lokalisiert)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Beispiel zeigt, wie eine Anwendung lokalisieren, an dem die Benutzeroberfläche in Code (statt XIBs oder Storyboards) erstellt.

### <a name="project-structure"></a>Projektstruktur

![](images/solution-code.png "Ressourcen-Struktur")

### <a name="localizablestrings-file"></a>Localizable.Strings-Datei

Wie oben beschrieben die **Localizable.strings** Dateiformat besteht aus Schlüssel-Wert-Paaren. Der Schlüssel beschreibt den Zweck der Zeichenfolge ein, und der Wert ist der übersetzte Text, in der app verwendet werden.

Die Spanisch (**vorangestellten**) lokalisierten Versionen des Beispiels sind unten aufgeführt:

```console
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

* In Visual Studio für Mac, befindet Sie sich der **Eigenschaften Pad** und heißt **Lokalisierungs-ID**.
* in Xcode er heißt **Objekt-ID**.

Dieser Zeichenfolgenwert hat ein Format, z. B. "NF3-h8-XmR" häufig, wie im folgenden Screenshot gezeigt:

![](images/xs-designer-localization-id.png "Xcode-Ansicht des Storyboard-Lokalisierung")

Dieser Wert wird verwendet, der **.strings** Datei übersetzten Text jedes Steuerelement automatisch zugewiesen.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Das Format der Übersetzungsdatei Storyboard ähnelt der **Localizable.strings** Datei, mit der Ausnahme, dass der Schlüssel (der Wert auf der linken Seite) nicht mit benutzerdefinierten jedoch stattdessen muss ein sehr spezifisches Format aufweisen: `ObjectID.property`.

Im Beispiel **Mainstoryboard.strings** unten sehen Sie `UITextField`s haben eine `placeholder` Text-Eigenschaft, die lokalisiert werden kann; `UILabel`s haben eine `text` Eigenschafts- und `UIButton`s Standardtext festgelegt ist, mit `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> Verwenden ein Storyboard mit Größenklassen kann Übersetzungen führen, die nicht in der Anwendung angezeigt werden. [Apple Xcode-Anmerkungen zu dieser Version](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) anzugeben, dass ein Storyboard oder XIB nicht richtig lokalisiert werden wenn drei Punkte erfüllt sind: Größenklassen verwendet, die Basis Lokalisierung und das Build-Ziel so Universal festgelegt sind und der Build-Ziel iOS 7.0. Die Korrektur besteht darin, Ihre Storyboarddatei von Zeichenfolgen in zwei identische Dateien doppelte: **MainStoryboard~iphone.strings** und **MainStoryboard~ipad.strings**, wie im folgenden Screenshot gezeigt:
> 
> ![](images/xs-dup-strings.png "Zeichenfolgen-Dateien")

<a name="appstore" />

## <a name="app-store-listing"></a>Store-App-Angebot

Apple häufig gestellte Fragen auf folgt [App Store Lokalisierung](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) Übersetzungen für jedes Land eingeben Ihrer app für den Verkauf ist. Beachten Sie die Warnung aus, die die Übersetzungen wird nur angezeigt, wenn Ihre app auch einen lokalisierten enthält **.lproj** Verzeichnis für die Sprache.

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
