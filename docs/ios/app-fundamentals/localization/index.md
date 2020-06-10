---
title: Lokalisierung in xamarin. IOS
description: In diesem Dokument werden die Funktionen der IOS-Lokalisierung beschrieben und erläutert, wie diese Features in xamarin. IOS-Apps verwendet werden. Es werden Sprache, Gebiets Schema, Zeichen folgen Dateien, Start Bilder und mehr erläutert.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/28/2017
ms.openlocfilehash: c42b41f9b853fba58ef70b8bd2f8ab20a3369647
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569242"
---
# <a name="localization-in-xamarinios"></a>Lokalisierung in xamarin. IOS

_Dieses Dokument behandelt die Lokalisierungs Features des IOS SDK und erläutert, wie Sie mit xamarin darauf zugreifen können._

Anweisungen zum Einschließen von Zeichensätzen/Codepages in Anwendungen, die nicht-Unicode-Daten verarbeiten müssen, finden Sie in den [Internationalisierungs Codierungen](encodings.md) .

## <a name="ios-platform-features"></a>IOS-Platt Form Features

In diesem Abschnitt werden einige der Lokalisierungs Features in ios beschrieben. Fahren Sie mit dem [nächsten Abschnitt](#localization-basics-in-ios) fort, um einen bestimmten Code und Beispiele anzuzeigen.

### <a name="language"></a>Sprache

Benutzer wählen Ihre Sprache in der App " **Einstellungen** " aus. Diese Einstellung wirkt sich auf die sprach Zeichenfolgen und Bilder aus, die vom Betriebssystem und in apps angezeigt werden.

Um die Sprache zu bestimmen, die in einer App verwendet wird, erhalten Sie das erste Element von `NSBundle.MainBundle.PreferredLocalizations` :

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Bei diesem Wert handelt es sich um einen Sprachcode `en` , z. b. für Englisch, für `es` Spanisch, `ja` für Japanisch usw. Der zurückgegebene Wert ist auf eine der von der Anwendung unterstützten Lokationen beschränkt (mithilfe von Fall Back-Regeln, um die beste Entsprechung zu bestimmen).

Der Anwendungscode muss nicht immer nach diesem Wert suchen – xamarin und IOS stellen Funktionen bereit, mit denen die richtige Zeichenfolge oder Ressource für die Sprache des Benutzers automatisch bereitgestellt werden kann. Diese Features werden im weiteren Verlauf dieses Dokuments beschrieben.

> [!NOTE]
> Verwenden `NSLocale.PreferredLanguages` Sie, um die Spracheinstellungen des Benutzers zu bestimmen, unabhängig von den von der APP unterstützten Lokalisierungs Vorfassungen. Die von dieser Methode zurückgegebenen Werte wurden in ios 9 geändert. Weitere Informationen finden [Sie unter Technical Note TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) .

### <a name="locale"></a>Gebietsschema

Benutzer wählen Ihr Gebiets Schema in der App " **Einstellungen** " aus. Diese Einstellung wirkt sich darauf aus, wie Datumsangaben, Uhrzeiten, Zahlen und Währungen formatiert werden.

Dadurch können Benutzer wählen, ob Sie 12-Stunden-oder 24-Stunden-Zeitformate sehen, ob das Dezimaltrennzeichen ein Komma oder ein Punkt ist und wie die Reihenfolge von Tag, Monat und Jahr in der Datumsanzeige liegt.

Mit xamarin können Sie sowohl auf die IOS-Klassen von Apple ( `NSNumberFormatter` ) als auch auf die .NET-Klassen in System. Globalization zugreifen. Entwickler sollten auswerten, welche für Ihre Anforderungen besser geeignet ist, da jeweils verschiedene Features verfügbar sind. Insbesondere wenn Sie in-App-Kaufpreise mit storekit abrufen und anzeigen, sollten Sie die Formatierungs Klassen von Apple für die zurückgegebenen Preisinformationen verwenden.

Das aktuelle Gebiets Schema kann auf zweierlei Weise abgefragt werden:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

Der erste Wert kann vom Betriebssystem zwischengespeichert werden und kann daher nicht immer das aktuell ausgewählte Gebiets Schema des Benutzers widerspiegeln. Verwenden Sie den zweiten Wert, um das aktuell ausgewählte Gebiets Schema abzurufen.

> [!NOTE]
> Mono (die .NET-Laufzeit, auf der xamarin. IOS basiert) und die IOS-APIs von Apple unterstützen keine identischen Sätze von Kombinationen aus Sprache und Region.
> Aus diesem Grund ist es möglich, eine Kombination aus Sprache und Region in der IOS- **Einstellungs** -App auszuwählen, die keinem gültigen Wert in Mono zugeordnet ist. Wenn Sie z. b. die Sprache eines iPhones auf Englisch und die Region auf Spanien festlegen, werden die folgenden APIs zu unterschiedlichen Werten führen:
>
> - `CurrentThead.CurrentCulture`: en-US (Mono-API)
> - `CurrentThread.CurrentUICulture`: en-US (Mono-API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple-API)
>
> Da Mono `CurrentThread.CurrentUICulture` zum Auswählen von Ressourcen und `CurrentThread.CurrentCulture` zum Formatieren von Datums-und Währungswerten verwendet, liefert Mono-basierte Lokalisierung (z. b. mit RESX-Dateien) möglicherweise nicht die erwarteten Ergebnisse für diese Kombinationen von Sprache und Region. In diesen Fällen müssen Sie die Apple-APIs für die Lokalisierung nach Bedarf nutzen.

### <a name="nscurrentlocaledidchangenotification"></a>Nscurrentlocaledidchangenotifizierung

IOS generiert eine, `NSCurrentLocaleDidChangeNotification` Wenn der Benutzer sein Gebiets Schema aktualisiert. Anwendungen können diese Benachrichtigung während der Ausführung überwachen und entsprechende Änderungen an der Benutzeroberfläche vornehmen.

## <a name="localization-basics-in-ios"></a>Grundlagen der Lokalisierung in ios

Die folgenden Funktionen von IOS können problemlos in xamarin genutzt werden, um dem Benutzer lokalisierte Ressourcen für die Anzeige bereitzustellen. Weitere Informationen zur Implementierung dieser Ideen finden Sie im [TaskyL10n-Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) .

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Angeben von standardmäßigen und unterstützten Sprachen in "Info. plist"

In [Technical Q&A QA1828: wie IOS die Sprache für Ihre APP bestimmt](https://developer.apple.com/library/content/qa/qa1828/_index.html), beschreibt Apple, wie IOS eine Sprache für die Verwendung in einer App auswählt. Die folgenden Faktoren wirken sich auf die angezeigte Sprache aus:

- Die bevorzugten Sprachen des Benutzers (in der App " **Einstellungen** " gefunden)
- Die mit der APP (lproj-Ordner) gebündelten Lokalisierungs-
- `CFBundleDevelopmentRegion`(**Info. plist** -Wert, der die Standardsprache für die APP angibt)
- `CFBundleLocalizations`(**Info. plist** -Array, das alle unterstützten Lokationen angibt)

Wie im technischen Q-&A angegeben, `CFBundleDevelopmentRegion` stellt die Standard Region und die Sprache einer APP dar. Wenn die APP die bevorzugten Sprachen eines Benutzers nicht explizit unterstützt, wird die in diesem Feld angegebene Sprache verwendet.

> [!IMPORTANT]
> IOS 11 wendet diesen Sprachauswahl Mechanismus strenger an als frühere Versionen des Betriebssystems. Aus diesem Grund wird für jede IOS 11-APP, die ihre unterstützten Lokationen nicht explizit deklariert – entweder durch Einschließen von lproj-Ordnern oder durch Festlegen eines Werts für –, unter `CFBundleLocalizations` Umständen eine andere Sprache als IOS 10 angezeigt.

Wenn `CFBundleDevelopmentRegion` nicht in der **Info. plist** -Datei angegeben wurde, verwenden die xamarin. IOS-Buildtools aktuell den Standardwert `en_US` . Dies kann sich in einer zukünftigen Version ändern, bedeutet aber, dass die Standardsprache Englisch ist.

Führen Sie die folgenden Schritte aus, um sicherzustellen, dass Ihre APP eine erwartete Sprache auswählt:

- Geben Sie eine Standardsprache an. Öffnen Sie " **Info. plist** ", und verwenden Sie die **Quell** Ansicht, um einen Wert für den Schlüssel festzulegen. `CFBundleDevelopmentRegion` in XML sollte Sie in etwa wie folgt aussehen:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

In diesem Beispiel wird "es" verwendet, um anzugeben, dass, wenn keine der bevorzugten Sprachen eines Benutzers unterstützt wird, der Standardwert Spanisch ist.

- Deklarieren Sie alle unterstützten Lokalisierungs-. Verwenden Sie in der Datei " **Info. plist**" die **Quell** Ansicht, um ein Array für den Schlüssel festzulegen. `CFBundleLocalizations` in XML sollte es in etwa wie folgt aussehen:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

Xamarin. IOS-apps, die mit .net-Mechanismen wie RESX-Dateien lokalisiert wurden, müssen diese **Info. plist** -Werte ebenfalls bereitstellen.

Weitere Informationen zu diesen **Info. plist** -Schlüsseln finden Sie in der Apple-Referenz für die [Informations Eigenschaften Liste](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>GetLocalizedString-Methode

Die- `NSBundle.MainBundle.GetLocalizedString` Methode sucht nach lokalisiertem Text, der in **Strings** -Dateien im Projekt gespeichert wurde. Diese Dateien sind nach Sprache organisiert, in speziell benannten Verzeichnissen mit einem **lproj** -Suffix (Beachten Sie, dass der erste Buchstabe der Erweiterung ein Kleinbuchstabe "L" ist).

#### <a name="strings-file-locations"></a>Speicherorte für. Strings-Dateien

- **Base. lproj** ist das Verzeichnis, das Ressourcen für die Standardsprache enthält.
  Sie befindet sich häufig im Stammverzeichnis des Projekts (kann aber auch im Ordner " **Resources** " abgelegt werden).
- ** &lt; Language &gt; . lproj** -Verzeichnisse werden für jede unterstützte Sprache erstellt, in der Regel im Ordner " **Resources** ".

In jedem Sprachverzeichnis können mehrere verschiedene **Strings** -Dateien vorhanden sein:

- **Lokalisier** bare Zeichen folgen – die Hauptliste lokalisierter Text.
- **Infoplist. Strings** – bestimmte spezifische Schlüssel sind in dieser Datei zulässig, um Dinge wie den Anwendungsnamen zu übersetzen.
- ** \<storyboard-name> . Strings** – optionale Datei, die Übersetzungen für Elemente der Benutzeroberfläche in einem Storyboard enthält.

Die **Buildaktion** für diese Dateien sollte eine **Bundle-Ressource**sein.

#### <a name="strings-file-format"></a>. Strings-Dateiformat

Die Syntax für lokalisierte Zeichen folgen Werte lautet:

```console
/* comment */
"key"="localized-value";
```

Sie sollten die folgenden Zeichen in Zeichen folgen mit Escapezeichen versehen:

- `\"`ATS
- `\\`umgekehrten Schrägstrich
- `\n`Zeilenumbruch

Dies ist ein Beispiel für **es/lokalisierbare. Strings** (d.h. Spanisch) aus dem Beispiel:

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

So lokalisieren Sie ein Image in ios:

1. Sehen Sie sich das Image im Code an, z. b.:

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. Platzieren Sie das standardbilddatei- **Flag. png** in **base. lproj** (dem Verzeichnis der systemeigenen Entwicklungssprache).

3. Platzieren Sie optional lokalisierte Versionen des Bilds in **lproj** -Ordnern für jede Sprache (z. b. **es. lproj**, **Ja. lproj**). Verwenden Sie in jedem Sprachverzeichnis das gleiche Dateiname- **Flag. png** .

Wenn ein Bild nicht für eine bestimmte Sprache vorhanden ist, greift IOS auf den standardmäßigen systemeigenen Sprachordner zurück und lädt das Image von dort.

#### <a name="launch-images"></a>Start Bilder

Verwenden Sie die Standard Benennungs Konventionen für die Start Images (und XIb oder Storyboard für iPhone 6-Modelle), wenn Sie diese in den **lproj** -Verzeichnissen für jede Sprache platzieren.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>App-Name

Wenn Sie eine **infoplist. Strings** -Datei in einem **lproj** -Verzeichnis platzieren, können Sie einige Werte aus der **Info. plist**-Datei der APP außer Kraft setzen, einschließlich des Anwendungs namens:

```console
"CFBundleDisplayName" = "LeónTodo";
```

Weitere Schlüssel zum [Lokalisieren von anwendungsspezifischen](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) Zeichen folgen sind:

- Cfbundlename
- CFBundleShortVersionString
- Nshumanumablecopyright

### <a name="dates-and-times"></a>Datums- und Zeitangaben

Obwohl es möglich ist, die integrierten .net-Datums-und Uhrzeit Funktionen (zusammen mit dem aktuellen `CultureInfo` ) zu verwenden, um Datumsangaben und Uhrzeiten für ein Gebiets Schema zu formatieren, werden dadurch Gebiets Schema spezifische Benutzereinstellungen ignoriert (die separat von der Sprache festgelegt werden können).

Verwenden Sie IOS `NSDateFormatter` , um eine Ausgabe zu erhalten, die der Gebiets Schema Einstellung des Benutzers entspricht. Der folgende Beispielcode veranschaulicht die grundlegenden Formatierungsoptionen für Datum und Uhrzeit:

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

Ergebnisse für Englisch in der USA:

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

Weitere Informationen finden Sie in der Dokumentation zu den Apple [Date Formatters](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) . Beim Testen der Gebiets Schema bezogenen Datums-und Uhrzeit Formatierung überprüfen Sie die Einstellungen der **iPhone-Sprache** und- **Region** .

<a name="rtl"></a>

### <a name="right-to-left-rtl-layout"></a>Layout von rechts nach links (RTL)

IOS bietet eine Reihe von Features zur Unterstützung beim Entwickeln von apps, die auf RTL-Unterstützung aufbauen:

- Verwenden Sie das-Attribut und das-Attribut des automatischen Layouts `leading` für die Ausrichtung des Steuer Elements `trailing` (das von Links und rechts für Englisch entspricht, jedoch für RTL-Sprachen umgekehrt).
  Das- [`UIStackView`](~/ios/user-interface/controls/uistackview.md) Steuerelement ist besonders nützlich zum Anordnen von Steuerelementen, die von RTL unterstützt werden.
- Verwenden Sie `TextAlignment = UITextAlignment.Natural` für die Textausrichtung (die für die meisten Sprachen, aber Recht für RTL belassen wird).
- `UINavigationController`kehrt die Schaltfläche "zurück" automatisch um und kehrt die Richtung um.

Die folgenden Screenshots zeigen das [lokalisierte Tasky-Beispiel](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) in Arabisch und Hebräisch an (Obwohl Englisch in den Feldern eingegeben wurde):

[![](images/rtl-ar-sml.png "Localization in Arabic")](images/rtl-ar.png#lightbox "Arabic")

[![](images/rtl-he-sml.png "Localization in Hebrew")](images/rtl-he.png#lightbox "Hebrew")

IOS kehrt automatisch zurück `UINavigationController` , und die anderen Steuerelemente werden in das `UIStackView` automatische Layout eingefügt oder darauf ausgerichtet.
Der RTL-Text wird mithilfe von **. Strings** -Dateien auf dieselbe Weise wie der Ltr-Text lokalisiert.

<a name="code"></a>

## <a name="localizing-the-ui-in-code"></a>Lokalisieren der Benutzeroberfläche in Code

Im Beispiel [Tasky (lokalisiert in Code)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) wird gezeigt, wie Sie eine Anwendung lokalisieren, bei der die Benutzeroberfläche in Code (anstelle von XIb oder Storyboards) erstellt wird.

### <a name="project-structure"></a>Projektstruktur

![](images/solution-code.png "Resources tree")

### <a name="localizablestrings-file"></a>Lokalisierbare Strings-Datei

Wie oben beschrieben, besteht das **Lokalisierbare Strings** -Dateiformat aus Schlüssel-Wert-Paaren. Der Schlüssel beschreibt die Absicht der Zeichenfolge, und der Wert ist der übersetzte Text, der in der APP verwendet werden soll.

Die Spanisch (**es**)-Lokationen für das Beispiel sind unten dargestellt:

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

### <a name="performing-the-localization"></a>Durchführen der Lokalisierung

Wenn im Anwendungscode der Anzeige Text einer Benutzeroberfläche festgelegt ist (unabhängig davon, ob es sich um einen Bezeichnungs Text oder einen Platzhalter der Eingabe handelt usw.), verwendet der Code die IOS- `GetLocalizedString` Funktion, um die richtige Übersetzung zum Anzeigen abzurufen:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"></a>

## <a name="localizing-storyboard-uis"></a>Lokalisieren von Storyboard-UIs

Das Beispiel [Tasky (lokalisiertes Storyboard)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) zeigt, wie Text in Steuerelementen in einem Storyboard lokalisiert wird.

### <a name="project-structure"></a>Projektstruktur

Das Verzeichnis " **base. lproj** " enthält das Storyboard und sollte auch alle Bilder enthalten, die in der Anwendung verwendet werden.

Die anderen sprach Verzeichnisse enthalten eine **Lokalisierbare Strings** -Datei für alle Zeichen folgen Ressourcen, auf die im Code verwiesen wird, sowie die Datei " **mainstoryboard. Strings** ", die Übersetzungen für Text im Storyboard enthält.

![](images/solution-storyboard.png "Resources tree")

Die sprach Verzeichnisse sollten eine Kopie aller lokalisierten Bilder enthalten, um die in " **base. lproj**" vorhandene Grafik zu überschreiben.

### <a name="object-id--localization-id"></a>Objekt-ID/Lokalisierungs-ID

Wenn Sie Steuerelemente in einem Storyboard erstellen und bearbeiten, wählen Sie jedes Steuerelement aus, und überprüfen Sie die ID für die Lokalisierung:

- In Visual Studio für Mac befindet er sich im **Eigenschaftenpad** und wird als Lokalisierungs- **ID**bezeichnet.
- In Xcode wird Sie als **Objekt-ID**bezeichnet.

Dieser Zeichen folgen Wert hat häufig ein Formular wie z. b. "NF3-h8-XMR", wie im folgenden Screenshot zu sehen:

![](images/xs-designer-localization-id.png "Xcode view of Storyboard localization")

Dieser Wert wird in der **Strings** -Datei verwendet, um jedem Steuerelement automatisch übersetzten Text zuzuweisen.

### <a name="mainstoryboardstrings"></a>Mainstoryboard. Strings

Das Format der Storyboard-Übersetzungsdatei ähnelt der **lokalisierbaren Strings** -Datei, mit der Ausnahme, dass der Schlüssel (der Wert auf der linken Seite) Nichtbenutzer definiert sein kann, sondern ein sehr spezifisches Format aufweisen muss: `ObjectID.property` .

Im Beispiel **mainstoryboard. Strings** unten sehen Sie `UITextField` , dass s über eine `placeholder` Text Eigenschaft verfügen, die lokalisiert `UILabel` werden kann. s weisen eine `text` -Eigenschaft auf, und `UIButton` s-Standardtext wird mithilfe von festgelegt `normalTitle` :

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
> Die Verwendung eines Storyboards mit Größenklassen kann zu Übersetzungen führen, die in der Anwendung nicht angezeigt werden. [Die Xcode-Versions Anmerkungen von Apple](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) geben an, dass ein Storyboard oder XIb nicht ordnungsgemäß lokalisiert wird, wenn drei Dinge zutreffen: es verwendet Größenklassen, die Basis Lokalisierung und das Buildziel werden auf Universal festgelegt, und der Build zielt auf IOS 7,0 ab. Die Korrektur besteht darin, die storyboardzeichenfolgen-Datei in zwei identische Dateien zu duplizieren: **mainstoryboard ~ iPhone. Strings** und **mainstoryboard ~ iPad. Strings**, wie im folgenden Screenshot zu sehen:
>
> ![](images/xs-dup-strings.png "Strings files")

<a name="appstore"></a>

## <a name="app-store-listing"></a>App Store-Auflistung

Nachfolgend finden Sie häufig gestellte Fragen zur [Lokalisierung von App Stores](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) , um Übersetzungen für jedes Land einzugeben, in dem Ihre APP verkauft wird. Beachten Sie die Warnung, dass die Übersetzungen nur angezeigt werden, wenn Ihre APP auch ein lokalisiertes **lproj** -Verzeichnis für die Sprache enthält.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden die Grundlagen der Lokalisierung von IOS-Anwendungen mithilfe der integrierten ressourcenverarbeitungs-und storyboardfeatures behandelt.

Weitere Informationen zu i18n und l10n für IOS-, Android-und plattformübergreifende Apps (einschließlich xamarin. Forms) finden Sie in [diesem plattformübergreifenden Leitfaden](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Verwandte Links

- [Tasky (lokalisiert im Code) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (lokalisiertes Storyboard) (Beispiel)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Leitfaden zur Apple-Lokalisierung](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Übersicht über die plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms-Lokalisierung](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android Localization (Android-Lokalisierung)](~/android/app-fundamentals/localization.md)
