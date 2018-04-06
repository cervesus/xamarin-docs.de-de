---
title: Arbeiten mit Lokalisierung
description: Anpassung Ihrer WatchOS-apps für mehrere Sprachen
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c765005491f55a1bdcadb1bc5aea97f693dc4570
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-localization"></a>Arbeiten mit Lokalisierung

_Anpassung Ihrer WatchOS-apps für mehrere Sprachen_

![](localization-images/both-languages-sml.png "Apple Watch Anzeigen lokalisierter")

WatchOS apps sind lokalisiert mithilfe der standardmäßigen iOS-Methoden:

- Mit **Lokalisierungs-ID** auf Storyboardelemente
- **.Strings** das Storyboard zugeordneten Dateien und
- **Localizable.Strings** Dateien für den Text im Code verwendet.

Die Standardeinstellung Storyboards und die Ressourcen befinden sich im ein **Base** -Verzeichnis, und sprachspezifische Übersetzungen und andere Ressourcen werden in gespeicherten **.lproj** Verzeichnisse.
iOS und OS überwachen verwendet Sprachauswahl des Benutzers automatisch die richtigen Zeichenfolgen und die Ressourcen zu laden.

Da eine Apple Watch-app besteht aus zwei Teilen - Watch-App und Watch-Erweiterung - lokalisierte Zeichenfolge, die Ressourcen erforderlich sind, an zwei Orten, je nachdem, wie sie verwendet werden.

Der lokalisierte Text und die Ressourcen werden *unterschiedliche* Watch-app und der Watch-Erweiterung.

## <a name="watch-app"></a>Watch-App

Die Watch-app enthält das Storyboard, das Benutzeroberfläche der Anwendung beschreibt. Alle Steuerelemente (z. B. `Label` und `Image`), dass das unterstützen der Lokalisierung haben eine **Lokalisierungs-ID**.

Jede sprachspezifischen **.lproj** Verzeichnis dürfen **.strings** Dateien mit den Übersetzungen für jedes Element (mithilfe der **Lokalisierungs-ID**), sowie Bilder verweist auf das Storyboard.

## <a name="watch-extension"></a>Watch-Erweiterung

Die Watch-Erweiterung ist, in dem der App_Code ausgeführt wird. Jeglichem Text, der von Code für den Benutzer angezeigt wird, muss in der Erweiterung und nicht in die Watch-app lokalisiert werden soll.

Die Erweiterung sollte auch enthalten die sprachspezifischen **.lproj** Verzeichnisse, aber die **.strings** Dateien erfordern nur Übersetzungen für Text, der in Ihrem Code verwendet wird.

## <a name="globalizing-the-watch-solution"></a>Globalisieren von der Lösung überwachen

Globalisierung versteht man eine Anwendung lokalisierbare gemacht.
Watch-apps, die Dies bedeutet, dass das Storyboard mit verschiedenen Textlängen Bedenken entwerfen, Winterzeit umgestellt sicherstellen jedes Bildschirmlayouts entsprechend je nach Text angezeigt wird. Sie müssen auch sicherstellen, beliebige Zeichenfolgen, die im Code Watch-Erweiterung können übersetzt werden, mithilfe der `LocalizedString` Methode.

### <a name="watch-app"></a>Watch-App

Standardmäßig ist die Watch-app für die Lokalisierung nicht konfiguriert. Müssen Sie die standardmäßige Storyboard-Datei verschieben und einige andere Verzeichnisse für die Übersetzung zu erstellen:

1. Erstellen Sie **Base.lproj** Verzeichnis und verschieben Sie die **Interface.storyboard** hinein.

2. Erstellen Sie **<language>.lproj** Verzeichnisse für jede Sprache, die Sie unterstützen möchten.

3. Die **.lproj** Verzeichnisse enthalten sollte ein **Interface.strings** Textdatei (der Dateiname sollte die Storboard Name übereinstimmen). Optional können Sie keine Bilder einfügen, die Lokalisierung in diese Verzeichnisse erfordern.

Das Watch-app-Projekt sieht wie folgt, nachdem diese Änderungen (nur Englisch und Spanisch Sprache Dateien hinzugefügt wurden) vorgenommen wurden:

  ![](localization-images/watchapp-solution.png "Das Watch-app-Projekt mit Englisch und Spanisch Sprachdateien")

#### <a name="storyboard-text"></a>Storyboard-Text

Wenn Sie das Storyboard bearbeiten, wählen Sie jedes Element, und beachten Sie, dass die **Lokalisierungs-ID** , angezeigt wird, der **Eigenschaften** aufgefüllt:

  [![](localization-images/storyboard-sml.png "Die Lokalisierungs-ID, die in das Auffüllzeichen Eigenschaften wird angezeigt.")](localization-images/storyboard.png#lightbox)

In der **Base.lproj** Ordner Schlüssel-/ Wertpaare erstellen, wie unten, wo der Schlüssel gebildet wird die **Lokalisierungs-ID** und ein Eigenschaftsnamen auf dem Steuerelement hinzugefügt wird, durch einen Punkt (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Beachten Sie, dass in diesem Beispiel, das eine **Lokalisierungs-ID** kann eine einfache Zeichenfolge (z. b. "0", "1", usw.) oder eine komplexere Zeichenfolge (z. B. "AgC-eL-Hgc"). `Label` Steuerelemente verfügen über eine `Text` Eigenschaft und `Button`s haben eine `Title` -Eigenschaft, die bei der Datenerfassung widergespiegelt wird ihre lokalisierte Werte festgelegt sind - werden Sie sicher, dass der Eigenschaftenname Kleinbuchstaben verwenden, wie im obigen Beispiel gezeigt.

Wenn das Storyboard in der Apple Watch gerendert wird, wird die richtigen Werte automatisch extrahiert und entsprechend der vom Benutzer ausgewählten Sprache angezeigt.

#### <a name="storyboard-images"></a>Storyboard-Bilder

Die beispiellösung umfasst außerdem eine **gradient@2x.png** Bild im jeweiligen Sprachordner. Dieses Image kann für jede Sprache (z. b. unterscheiden Sie haben Text, die benötigt wird, übersetzen eingebettet oder Verwendung lokalisiert Iconography).

Legen Sie des Bilds einfach **Image** Eigenschaft in das Storyboard und das richtige Bild für die Überwachung gemäß der vom Benutzer ausgewählten Sprache gerendert wird.

![](localization-images/storyboard-image.png "Legen Sie die Bilder Bildeigenschaft in das storyboard")

Hinweis: alle Apple Überwachungen Schreibberechtigung Retina zeigt nur die **@2x** Version des Abbilds ist erforderlich. Sie müssen nicht angeben **@2x** in das Storyboard.

### <a name="watch-extension"></a>Watch-Erweiterung

Die Watch-Erweiterung ist eine ähnliche Verzeichnisstruktur zum unterstützen der Lokalisierung, jedoch keine Storyboard ist erforderlich. Die lokalisierten Zeichenfolgen in der Erweiterung werden nur C#-Code verweist.

![](localization-images/watchextension-solution.png "Die Verzeichnisstruktur des Watch-Erweiterung zum unterstützen der Lokalisierung")

#### <a name="strings-in-code"></a>Zeichenfolgen im Code

Die **Localizable.strings** Datei hat eine etwas andere Struktur als ein Storyboard zugeordnet wurde. In diesem Fall können wir eine beliebige Zeichenfolge "Schlüssel" auswählen. Apple Empfehlung ist die Verwendung ein Schlüssels, das den tatsächlichen Text angibt, der in der standardmäßigen Sprache angezeigt werden soll:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

Die `NSBundle.MainBundle.LocalizedString` Methode wird zum Auflösen von Zeichenfolgen in ihre übersetzten Entsprechungen verwendet, wie im folgenden Code gezeigt.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Bilder in Code

Bilder, die vom Code aufgefüllt werden, können auf zwei Arten festgelegt werden.

1. Sie können ändern, ein `Image` Steuerelement, indem Sie deren Festlegung auf den Zeichenfolgennamen eines Bilds, das bereits vorhanden ist in die Watch-App z. B.

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Verschieben Sie ein Bild von der Erweiterung, mit der Überwachung `FromBundle` und die app wird automatisch das richtige Bild für die ausgewählte Sprache des Benutzers ausgewählt. In der Beispielprojektmappe ist ein Bild **language@2x.png** in der jeweiligen Sprache Ordner, und es wird auf `DetailController` mit dem folgenden Code:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Beachten Sie, die Sie nicht benötigen, geben Sie die **@2x** beim Verweisen auf den Dateinamen des Bilds.

Die zweite Methode ist auch anwendbar, wenn Sie ein Image von einem Remoteserver zum Rendern auf der Apple Watch herunterzuladen. jedoch müssen in diesem Fall Sie sicherstellen, dass das Bild, das Sie herunterladen gemäß den benutzereinstellungen richtig lokalisiert ist.

## <a name="localization"></a>Lokalisierung

Nachdem Sie Ihre Lösung konfiguriert haben, Übersetzer müssen verarbeiten Ihre **.strings** Dateien und Images für jede Sprache, die Sie unterstützen möchten.

Sie können beliebig viele erstellen **.lproj** Verzeichnisse als Sie benötigen (eine für jede unterstützte Sprache). Sie heißen mit Sprachcodes, wie **En**, **vorangestellten**, **de**, **Japan**, **pt-BR**, usw. (für Englisch aus. Spanisch, Deutsch und Japanisch, Portugiesisch (Brasilien) bzw.).

Das angefügte-Beispiel verwendet die Übersetzungen (computergenerierte) veranschaulicht, wie Sie eine app WatchOS zu lokalisieren.

### <a name="watch-app"></a>Watch-App

Diese Werte werden verwendet, in die Watch-app Storyboard definierte Benutzeroberfläche zu übersetzen. Die *Schlüssel* Wert ist eine Kombination jedes Storyboard-Steuerelements **Lokalisierungs-ID** und die Eigenschaft übersetzt wird.

Es wird empfohlen, zum Hinzufügen von Kommentaren, die den ursprünglichen Text in die Datei enthält, damit Konvertierer wissen, was die Übersetzung werden soll.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Für das Storyboard Zeichenfolgen Spanisch (maschinell übersetzt) sind unten dargestellt. Es ist hilfreich, jede Zeile Kommentare hinzufügen, da es schwierig zu wissen, was die **Lokalisierungs-ID** verweist auf die andernfalls:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Watch-Erweiterung

Diese Werte werden im Code verwendet, zum Übersetzen von Informationen, bevor Sie dem Benutzer angezeigt wird. Die *Schlüssel* vom Entwickler ausgewählt ist, während sie Code schreiben, und in der Regel enthält die tatsächliche Zeichenfolge übersetzt werden.

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings-Datei

Die Language-Zeichenfolgen (maschinell übersetzt) Spansish:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Test

Die Methode zum Ändern der bevorzugten Sprache unterscheidet sich zwischen den Simulator und physischen Geräten.

### <a name="simulator"></a>Simulator

Wählen Sie im Simulator die Sprache zu testen, verwenden die iOS **Einstellungen** app (das graue Gänge-Symbol in der Simulator-Startbildschirm).

  ![](localization-images/sim-settings-sml.png "IOS-app Lokalisierung-Einstellungen")

### <a name="watch-device"></a>Überwachen-Gerät

Beim Testen mit einer Apple Watch ändern Sie die Sprache für die Überwachung in der **Apple Watch** app auf dem iPhone gepaart.

  ![](localization-images/phone-settings-sml.png "Ändern Sie die Überwachung Sprache in der Apple Watch-app auf dem iPhone paarweise zugeordneten")



## <a name="related-links"></a>Verwandte Links

- [WatchLocalization (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
