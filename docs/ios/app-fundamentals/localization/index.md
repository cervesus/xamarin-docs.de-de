---
title: Lokalisierung in Xamarin.iOS
description: Dieses Dokument beschreibt die iOS-Lokalisierungsfeatures und wie Sie diese Funktionen in Xamarin.iOS-apps verwenden. Es wird erläutert, Sprache, Gebietsschema, Zeichenfolgen-Dateien, startbilder und mehr.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: 0c52db61689dd640332fb1e02e2260dda08e4686
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115925"
---
# <a name="localization-in-xamarinios"></a>Lokalisierung in Xamarin.iOS

_Dieses Dokument behandelt die Lokalisierungsfeatures von iOS-SDK und wie Sie sie mit Xamarin zugreifen._

Finden Sie in der [Internationalisierungscodierungen](encodings.md) Anleitungen, einschließlich der Zeichen legt/Codepages in Anwendungen, die nicht-Unicode-Daten verarbeiten müssen.

## <a name="ios-platform-features"></a>iOS-Plattformfeatures

Dieser Abschnitt beschreibt einige der Lokalisierungsfeatures in iOS. Fahren Sie mit der [nächsten Abschnitt](#basics) zu spezifischen Code und Beispiele finden Sie unter.

### <a name="language"></a>Sprache

Benutzer wählen die Sprache in der **Einstellungen** app. Diese Einstellung wirkt sich auf die Zeichenfolgen in einer Sprache und Bildern, die vom Betriebssystem und in apps angezeigt. 

Um zu bestimmen, die Sprache, die in einer app verwendet wird, erhalten Sie das erste Element der `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Dieser Wert wird ein Sprachcode sein, z. B. `en` für Englisch, `es` für Spanisch `ja` usw. für Japanisch. Der zurückgegebene Wert ist eines der lokalisierten Versionen unterstützt, die von der Anwendung (mithilfe von fallback-Regeln bestimmen die beste Übereinstimmung) beschränkt.

Anwendungscode muss nicht immer auf diesem Wert – Xamarin überprüft, und beide iOS bieten Funktionen, mit denen Sie die Sprache des Benutzers automatisch die richtige Zeichenfolge oder eine Ressource bereit. Diese Funktionen werden im weiteren Verlauf dieses Dokuments beschrieben.

> [!NOTE]
> Verwendung `NSLocale.PreferredLanguages` spracheinstellungen des Benutzers, unabhängig davon, die von der app unterstützten lokalisierungen fest. Die Rückgabewerte von dieser Methode, die in iOS 9 geändert; finden Sie unter [technischen Hinweis TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Details.

### <a name="locale"></a>Gebietsschema

Benutzer auswählen, deren Gebietsschema in der **Einstellungen** app. Diese Einstellung wirkt sich auf die Möglichkeit, dass Datumsangaben, Uhrzeiten, Zahlen und Währung formatiert sind.

Dadurch können Benutzer wählen, ob ihre Dezimaltrennzeichen ein Komma oder einem Punkt und die Reihenfolge der Tag, Monat und Jahr im Datumsanzeige wird, ob sie 12- oder 24-Stunden-Zeitformate, finden Sie unter.

Mit Xamarin, die Sie haben Zugriff auf beide Apple iOS-Klassen (`NSNumberFormatter`) sowie die .NET-Klassen im System.Globalization. Entwickler sollten ausgewertet werden die besser für ihre Anforderungen geeignet ist, weil es verschiedene Funktionen in jeder verfügbaren gibt. Wenn Sie abrufen und Anzeigen von In-App-Käufe Preise mit StoreKit sollten Sie vor allem Formatierungen von Apple-Klassen für die zurückgegebenen Preisinformationen verwenden.

Das aktuelle Gebietsschema kann durch eine von zwei Arten abgefragt werden:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Der erste Wert kann durch das Betriebssystem zwischengespeichert werden und daher möglicherweise nicht immer widerspiegeln aktuell ausgewählten Gebietsschema des Benutzers. Verwenden Sie den zweiten Wert, um das aktuell ausgewählte Gebietsschema abzurufen.

> [!NOTE]
> Mono (die .NET Runtime, die auf die Xamarin.iOS basiert) und Apple iOS-APIs unterstützen keine identische Kopien von Kombinationen von Sprache/Region.
> Aus diesem Grund ist es möglich, wählen Sie eine Sprache/Region-Kombination in der iOS- **Einstellungen** -app, die nicht in einen gültigen Wert in Mono zugeordnet ist. Festlegen von einem iPhone die Sprache auf Englisch und die Region auf Spanien verursacht z. B. die folgenden APIs, um unterschiedliche Werte zu erhalten:
> 
> - `CurrentThead.CurrentCulture`: En-US (Mono-API)
> - `CurrentThread.CurrentUICulture`: En-US (Mono-API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: En_ES (Apple-API)
>
> Da Mono verwendet `CurrentThread.CurrentUICulture` Ressourcen auswählen und `CurrentThread.CurrentCulture` Mono-basierte Lokalisierung (z. B. mit RESX-Dateien) möglicherweise zum Formatieren von Datumsangaben und Währungen ergibt nicht erwartete Ergebnisse für diese Sprache/Region-Kombinationen. Basieren Sie in diesen Fällen auf Apple-APIs, um nach Bedarf zu lokalisieren.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS generiert eine `NSCurrentLocaleDidChangeNotification` Wenn der Benutzer ihrem Gebietsschema aktualisiert. Für diese Benachrichtigung können Anwendungen überwachen, während sie ausgeführt werden und können entsprechende Änderungen an der UI vornehmen.

## <a name="localization-basics-in-ios"></a>Grundlagen der Lokalisierung in iOS

Die folgenden Funktionen von iOS werden in Xamarin zum Bereitstellen von lokalisierter Ressourcen für die Anzeige für den Benutzer leicht genutzt werden. Finden Sie in der [TaskyL10n Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) zu erfahren, wie Sie diese Ideen zu implementieren.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Angeben der Standardsprache sowie unterstützte Sprachen in "Info.plist"

In [technischen Fragen und Antworten eine QA1828: wie iOS die Sprache für Ihre App ermittelt](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple wird beschrieben, wie iOS eine Sprache für die Verwendung in einer app auswählt. Die folgenden Faktoren auswirken, welche Sprache angezeigt wird:

- Der Benutzer die bevorzugte Sprachen (finden Sie in der **Einstellungen** app)
- Die lokalisierten Versionen zusammen mit der app (.lproj Ordner)
- `CFBundleDevelopmentRegion` (**"Info.plist"** Wert, der die Standardsprache für die app angibt)
- `CFBundleLocalizations` (**"Info.plist"** Array gibt alle unterstützte lokalisierungen)

Wie in der technischen Fragen und Antworten, `CFBundleDevelopmentRegion` einer app Standardregion und Sprache darstellt. Wenn die app explizit eine der bevorzugten Sprachen eines Benutzers nicht unterstützt, wird die nach diesem Feld angegebene Sprache verwendet. 

> [!IMPORTANT]
> iOS 11 wendet diesen Auswahlmechanismus Sprache strenger als in vorherige Versionen des Betriebssystems. Daher jede iOS 11-app, die die unterstützten lokalisierungen – einschließlich .lproj Ordner oder Festlegen eines Werts für nicht explizit deklarieren `CFBundleLocalizations` – zeigt möglicherweise eine andere Sprache in iOS 11 als in iOS 10 aufweisen.

Wenn `CFBundleDevelopmentRegion` wurde nicht angegeben wurde, der **"Info.plist"** -Datei, die Xamarin.iOS-Build-Tools derzeit den Standardwert verwenden `en_US`. Obwohl dies in einer zukünftigen Version ändert sich möglicherweise, bedeutet dies, dass die Standardsprache Englisch ist.

Gehen Sie folgendermaßen vor, um sicherzustellen dass Ihre app eine erwartete Sprache auswählen:

- Geben Sie eine Standardsprache an. Open **"Info.plist"** und verwenden Sie die **Quelle** Ansicht zum Festlegen eines Werts für die `CFBundleDevelopmentRegion` key; im XML-Code, es sieht etwa wie folgt:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Dieses Beispiel verwendet "es", um anzugeben, dass wenn keines Benutzers bevorzugten Sprachen unterstützt werden, standardmäßig auf Spanisch.

- Deklarieren Sie alle unterstützte lokalisierungsanforderungen. In **"Info.plist"**, verwenden Sie die **Quelle** Ansicht, um ein Array für die `CFBundleLocalizations` key; im XML-Code, es sieht etwa wie folgt:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Xamarin.iOS-apps, die mit .NET Mechanismen, wie z. B. resx-Dateien, die diese bereitstellen müssen lokalisiert wurden **"Info.plist"** auch Werte.

Weitere Informationen zu diesen **"Info.plist"** Schlüssel, sehen Sie sich von Apple [Informationen Eigenschaftsverweis Liste Schlüssel](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>GetLocalizedString-Methode

Die `NSBundle.MainBundle.GetLocalizedString` Methode sucht lokalisierter Text, der in gespeichert wurde **.strings** Dateien im Projekt. Diese Dateien sind nach Sprache im speziell benannte Verzeichnisse mit organisiert, dass ein **.lproj** Suffix (Beachten Sie den ersten Buchstaben der Erweiterung ist ein kleines "L").

#### <a name="strings-file-locations"></a>.Strings Dateispeicherorte

- **Base.lproj** ist das Verzeichnis, die Ressourcen für die Standardsprache enthält.
  Häufig befindet sich in das Stammverzeichnis des Projekts (aber auch in platziert werden die **Ressourcen** Ordner).
- **&lt;Sprache&gt;.lproj** Verzeichnisse werden erstellt für jede unterstützte Sprache, in der Regel der **Ressourcen** Ordner.

Es kann eine Reihe von verschiedenen sein **.strings** Dateien in jedem Sprachverzeichnis:

- **Localizable.Strings** – der Hauptliste der lokalisierte Text.
- **InfoPlist.strings** – bestimmte bestimmte Schlüssel sind in dieser Datei zulässig, um beispielsweise den Namen der Anwendung zu übersetzen.
- **< Storyboard-Name > .strings** – optionale Datei, die Übersetzungen für Elemente der Benutzeroberfläche in einem Storyboard enthält.

Die **Buildvorgang** für diese Dateien müssen **Bundle Ressource**.

#### <a name="strings-file-format"></a>.Strings-Dateiformat

Die Syntax für die lokalisierte Zeichenfolgenwerte lautet:

```console
/* comment */
"key"="localized-value";
```

Sie sollten die folgenden Zeichen in Zeichenfolgen mit Escapezeichen versehen:

* `\"` Anführungszeichen
* `\\` umgekehrter Schrägstrich
* `\n` Zeilenumbruch

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

Um ein Bild in iOS zu lokalisieren:

1. Verweisen Sie auf das Bild im Code, z.B.:

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. Legen Sie die Standard-Image-Datei **flag.png** in **Base.lproj** (das Verzeichnis der systemeigene Entwicklung-Sprache).

3. Platzieren Sie optional die lokalisierte Versionen des Bilds in **.lproj** Ordner für jede Sprache (z. b. **es.lproj**, **ja.lproj**). Verwenden Sie den gleiche Dateinamen **flag.png** in jedem Sprachverzeichnis.

Wenn ein Bild nicht für eine bestimmte Sprache vorhanden ist, wird IOS-ein Fallback auf den Standardordner der systemeigenen Sprache und laden das Image von dort aus.

#### <a name="launch-images"></a>Startbilder

Verwenden Sie die standardmäßigen Benennungskonventionen für die startbilder (und die XIB oder Storyboard für iPhone 6-Modelle) bei der Platzierung in der **.lproj** Verzeichnisse für jede Sprache.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>App-name

Platzieren einer **InfoPlist.strings** Datei eine **.lproj** Directory können Sie einige Werte aus der app außer Kraft setzen **"Info.plist"**, einschließlich den Namen der Anwendung:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Andere Schlüssel, mit denen Sie können [Lokalisieren von Zeichenfolgen für anwendungsspezifische](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) sind:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Datums- und Uhrzeitangaben

Obwohl es möglich ist, verwenden Sie die integrierten Funktionen von .NET Datum und Uhrzeit (zusammen mit dem aktuellen `CultureInfo`) zum Formatieren von Datumsangaben und Uhrzeiten für ein Gebietsschema, würde dies gebietsschemaspezifische-benutzereinstellungen (der separat von Sprache festgelegt werden können) ignorieren.

Nutzen Sie das iOS `NSDateFormatter` Ausgabe erzeugt, das Gebietsschema des Benutzers entspricht. Der folgende Beispielcode veranschaulicht die grundlegende Datum und Uhrzeit, die Formatierungsoptionen:

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

Ergebnisse für Englisch in den Vereinigten Staaten:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

Ergebnisse für Spanisch in Spanien:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Finden Sie in der Apple [Datum Formatierer](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) Dokumentation zu informieren. Wenn gebietsschemaabhängige Datums- und uhrzeitformatierungen testen möchten, aktivieren Sie beide **iPhone Sprache** und **Region** Einstellungen.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Layout von rechts-nach-links (RTL)

iOS enthält eine Reihe von Funktionen zum Erstellen von RTL-fähige apps:

- Automatisches Layout verwenden `leading` und `trailing` Attribute für das Steuerelement Ausrichtung keine besondere Vorschrift (der für Englisch links und rechts entspricht, ist jedoch umgekehrt für RTL-Sprachen).
  Die [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Steuerelement ist besonders nützlich für die Anordnung von Steuerelementen RTL-fähig sein.
- Verwendung `TextAlignment = UITextAlignment.Natural` für die textausrichtung (die für die meisten Sprachen jedoch rechts für RTL gelassen werden).
- `UINavigationController` automatisch spiegelt die Schaltfläche "zurück", und kehrt Wischen Richtung.

Die folgenden Screenshots zeigen die [lokalisierte Tasky Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch und Hebräisch (obwohl Englisch in die Felder eingegeben wurde):

[![](images/rtl-ar-sml.png "Lokalisierung in Arabisch")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "Lokalisierung – Hebräisch")](images/rtl-he.png#lightbox "Hebrew")

iOS automatisch umgekehrt wird die `UINavigationController`, und die anderen Steuerelemente befinden sich in `UIStackView` oder ohne Auto-Layout ausgerichtet.
RNL-Text ist mit dem lokalisiert **.strings** Dateien in die gleiche Weise wie LTR-Text.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Lokalisieren von der Benutzeroberfläche in code

Die [Tasky (im Code lokalisiert)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Beispiel zeigt, wie eine Anwendung lokalisieren, in dem die Benutzeroberfläche in Code (statt XIBs oder Storyboards) erstellt.

### <a name="project-structure"></a>Struktur des Projekts

![](images/solution-code.png "Ressourcen-Struktur")

### <a name="localizablestrings-file"></a>Localizable.Strings-Datei

Wie oben beschrieben die **Localizable.strings** Dateiformat bestehen aus Schlüssel-Wert-Paaren. Der Schlüssel beschreibt den Verwendungszweck der Zeichenfolge ein, und der Wert ist der übersetzte Text in der app verwendet werden.

Das für Spanisch (**es**) – lokalisierungen für das Beispiel unten sind:

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

Im Anwendungscode, wo Text von einer Benutzeroberfläche anzeigen (sowohl Text mit einer Bezeichnung, oder die Eingabe-Platzhalter usw.) festgelegt ist der Code verwendet das iOS `GetLocalizedString` Funktion, um die ordnungsgemäße Übersetzung anzuzeigende abzurufen:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Lokalisieren von Storyboard-Benutzeroberflächen

Das Beispiel [Tasky (lokalisierte Storyboard)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) zeigt, wie Text für Steuerelemente in einem Storyboard zu lokalisieren.

### <a name="project-structure"></a>Projektstruktur

Die **Base.lproj** Directory enthält das Storyboard und sollten auch in der Anwendung verwendeten Bilder enthalten.

Die andere Sprachverzeichnisse enthalten eine **Localizable.strings** -Datei für alle Zeichenfolgenressourcen in Code auf die verwiesen wird. als auch ein **MainStoryboard.strings** -Datei mit Übersetzungen von Text in der Storyboard.

![](images/solution-storyboard.png "Ressourcen-Struktur")

Die Sprachverzeichnisse sollte eine Kopie der Bilder, die lokalisiert wurden, um die Grenze, die im außer Kraft setzt, enthalten **Base.lproj**.

### <a name="object-id--localization-id"></a>Objekt-ID / Lokalisierungs-ID

Beim Erstellen und Bearbeiten von Steuerelementen in einem Storyboard, wählen Sie jedes Steuerelement, und überprüfen Sie die ID, für die Lokalisierung verwendet:

- In Visual Studio für Mac befindet sich die **Pad "Eigenschaften"** und heißt **Lokalisierungs-ID**.
- In Xcode, heißt es **Objekt-ID**.

Diesen Zeichenfolgenwert besitzt ein Format z. B. "NF3-h8-XmR" oft, wie im folgenden Screenshot gezeigt:

![](images/xs-designer-localization-id.png "Xcode-Ansicht der Storyboard-Lokalisierung")

Dieser Wert wird verwendet, der **.strings** Datei übersetzten Text jedes Steuerelement automatisch zuweisen.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Das Format der Übersetzungsdatei Storyboard ähnelt der **Localizable.strings** Datei mit dem Unterschied, dass der Schlüssel (der Wert auf der linken Seite), nicht den benutzerdefinierten, jedoch stattdessen muss ein sehr spezifisches Format haben: `ObjectID.property`.

Im Beispiel **Mainstoryboard.strings** unten sehen Sie `UITextField`s haben eine `placeholder` Text-Eigenschaft, die lokalisiert werden kann; `UILabel`s haben eine `text` Eigenschafts- und `UIButton`Standardtext s wird festgelegt, `normalTitle`:

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
> Verwenden ein Storyboard mit Größenklassen führen Übersetzungen, die nicht in der Anwendung angezeigt werden. [Apple Xcode-Anmerkungen zu dieser Version](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) anzugeben, dass ein Storyboard oder eine XIB nicht richtig lokalisiert werden wenn drei Punkte erfüllt sind: Größenklassen verwendet, die Basis Lokalisierung und das Build-Ziel auf Universal festgelegt und der Build ausgerichtet ist, iOS 7.0. Die Korrektur besteht darin, Ihre Storyboarddatei für Zeichenfolgen in zwei identische Dateien zu duplizieren: **MainStoryboard~iphone.strings** und **MainStoryboard~ipad.strings**, wie im folgenden Screenshot gezeigt:
> 
> ![](images/xs-dup-strings.png "Zeichenfolgen-Dateien")

<a name="appstore" />

## <a name="app-store-listing"></a>App-Store-Liste

Apple häufig gestellte Fragen zu folgt [App Store Lokalisierung](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) um Übersetzungen für jedes Land geben Ihre app für den Verkauf ist. Beachten Sie die Warnung, die die Übersetzungen wird nur angezeigt, wenn Ihre app auch einen lokalisierten enthält **.lproj** Verzeichnis für die Sprache.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Grundlagen der Lokalisierung von iOS-Anwendungen, die mithilfe der integrierten Ressourcen behandeln und Storyboard-Funktionen.

Finden Sie weitere Informationen zu i18n und L10n für iOS, Android und plattformübergreifende apps (einschließlich Xamarin.Forms) in [in der vorliegenden plattformübergreifende](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Verwandte Links

- [Tasky (im Code lokalisiert) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalisierte Storyboard) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Lokalisierung Apple-Handbuch](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android-Lokalisierung](~/android/app-fundamentals/localization.md)
