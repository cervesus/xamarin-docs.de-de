---
title: Arbeiten mit WatchOS Lokalisierung in Xamarin
description: Dieses Dokument beschreibt das Lokalisieren von WatchOS-apps mit Xamarin erstellt wurde. Watch-apps, sehen Sie sich Erweiterungen, erläutert Zeichenfolgen im Code storyboard-Text, Tests und mehr.
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 1362767bf9a80af1eac37d316bd99a6ab364063f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413999"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Arbeiten mit WatchOS Lokalisierung in Xamarin

_Anpassen Ihrer WatchOS-apps für mehrere Sprachen_

![](localization-images/both-languages-sml.png "Apple Watch Anzeigen lokalisierter")

WatchOS-apps sind mit den standard-iOS-Methoden lokalisiert:

- Mithilfe von **Lokalisierungs-ID** Storyboard-Elemente
- **.Strings** Storyboards zugeordneten Dateien und
- **Localizable.Strings** Dateien nach Text im Code verwendet.

Die Standard-Storyboards und Ressourcen befinden sich in einem **Base** Verzeichnis und sprachspezifische Übersetzungen und andere Ressourcen befinden sich **.lproj** Verzeichnisse.
IOS- und sehen Sie sich OS verwendet Sprachauswahl des Benutzers automatisch die richtigen Zeichenfolgen und Ressourcen zu laden.

Da eine Apple Watch-app besteht aus zwei Teilen – Watch-App "und" Watch-Erweiterung - lokalisierte Zeichenfolge, die Ressourcen erforderlich sind, an zwei Stellen, je nachdem, wie sie verwendet werden.

Der lokalisierte Text und die Ressourcen werden *verschiedene* in der Watch-app und der Watch-Erweiterung.

## <a name="watch-app"></a>Watch-App

Die Watch-app enthält das Storyboard, das der app-Benutzeroberfläche beschreibt. Alle Steuerelemente (z. B. `Label` und `Image`) unterstützen der Lokalisierung haben eine **Lokalisierungs-ID**.

Jede sprachspezifische **.lproj** Verzeichnis sollte enthalten **.strings** Dateien mit den Übersetzungen für jedes Element (mithilfe der **Lokalisierungs-ID**), sowie Bilder Das Storyboard verwiesen.

## <a name="watch-extension"></a>Watch-Erweiterung

Die Watch-Erweiterung ist, in dem Ihr app-Code ausgeführt wird. Jeder Text, der dem Benutzer, aus Code angezeigt wird muss in der Erweiterung und nicht in der Watch-app lokalisiert werden soll.

Die Erweiterung sollte außerdem enthalten die sprachspezifischen **.lproj** Verzeichnisse, aber die **.strings** Dateien erfordern nur Übersetzungen für Text, der in Ihrem Code verwendet wird.

## <a name="globalizing-the-watch-solution"></a>Globalisieren von der Lösung ansehen

Globalisierung ist der Prozess der lokalisierbaren für eine Anwendung herstellen.
Für die Watch-apps, die Dies bedeutet, entwerfen das Storyboard mit verschiedene Textlängen Denken Sie daran, passt jeden Bildschirmlayout sicherstellen entsprechend je nachdem welcher Text angezeigt wird. Sie müssen auch sicherstellen, alle Zeichenfolgen, die auf die verwiesen wird im Code-Erweiterung sehen Sie sich mit übersetzt werden können die `LocalizedString` Methode.

### <a name="watch-app"></a>Watch-App

Standardmäßig ist die Watch-app für die Lokalisierung nicht konfiguriert. Sie müssen die standardmäßige Storyboard-Datei zu verschieben, und erstellen einige andere Verzeichnisse für Ihre Übersetzungen:

1. Erstellen Sie **Base.lproj** Directory und verschieben Sie die **Interface.storyboard** hinein.

2. Erstellen Sie **<language>.lproj** Verzeichnisse für jede Sprache, die Sie unterstützen möchten.

3. Die **.lproj** Verzeichnisse enthalten sollte ein **Interface.strings** Textdatei (der Dateiname sollte das Storboard Namen entsprechen). Sie können optional alle Bilder ablegen, für die Lokalisierung in diesen Verzeichnissen erforderlich.

Das Watch-app-Projekt sieht folgendermaßen aus, nachdem diese Änderungen wurden (nur Englisch und Spanisch Sprache Dateien hinzugefügt wurden):

  ![](localization-images/watchapp-solution.png "Das Watch-app-Projekt mit Englisch und Spanisch Language-Dateien")

#### <a name="storyboard-text"></a>Storyboard-Text

Wenn Sie das Storyboard bearbeiten, wählen Sie jedes Element und beachten Sie, dass die **Lokalisierungs-ID** , angezeigt wird, der **Eigenschaften** Pad:

  [![](localization-images/storyboard-sml.png "Die Lokalisierungs-ID, die im Bereich \"Eigenschaften\" angezeigt wird.")](localization-images/storyboard.png#lightbox)

In der **Base.lproj** Ordner Schlüssel-Wert-Paaren zu erstellen, wie unten, wo der Schlüssel gebildet wird dargestellt, die **Lokalisierungs-ID** und ein Eigenschaftennamen für das Steuerelement, getrennt durch einen Punkt (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Beachten Sie, dass in diesem Beispiel, das eine **Lokalisierungs-ID** kann eine einfache Anzahl Zeichenfolge (z. b. "0", "1", usw.) oder eine komplexere Zeichenfolge (z. B. "AgC – eL-Hgc"). `Label` Steuerelemente verfügen über eine `Text` Eigenschaft und `Button`s haben eine `Title` -Eigenschaft, die auf die Weise dargestellt wird, die lokalisierte Werte festgelegt sind: Achten Sie darauf, die den Kleinbuchstaben Eigenschaftennamen verwenden, wie im obigen Beispiel gezeigt.

Wenn das Storyboard in der Apple Watch gerendert wird, wird die richtigen Werte automatisch extrahiert und entsprechend der vom Benutzer ausgewählten Sprache angezeigt.

#### <a name="storyboard-images"></a>Storyboard-Images

Die beispiellösung umfasst außerdem eine **gradient@2x.png** Bild im jeweiligen Sprachordner. Dieses Image kann für jede Sprache (z. b. abweichen Sie können Text, der benötigt wird, übersetzen eingebettet haben, oder verwenden lokalisierte ikonographie).

Legen Sie einfach des Bilds des **Image** -Eigenschaft in das Storyboard und das richtige Bild wird auf der Apple Watch entsprechend der Sprache, die vom Benutzer ausgewählten gerendert werden.

![](localization-images/storyboard-image.png "Die Bilder Bildeigenschaft im Storyboard festlegen")

Hinweis: alle Apple Überwachungen Schreibberechtigung Retina zeigt nur die **@2x** Version des Abbilds ist erforderlich. Sie müssen nicht angeben **@2x** in das Storyboard.

### <a name="watch-extension"></a>Watch-Erweiterung

Die Watch-Erweiterung ist eine ähnliche Verzeichnisstruktur zur Unterstützung der Lokalisierung, jedoch kein Storyboard ist erforderlich. Die lokalisierten Zeichenfolgen in der Erweiterung werden nur auf die verwiesen wird durch C# Code.

![](localization-images/watchextension-solution.png "Die Verzeichnisstruktur des Watch-Erweiterung zur Unterstützung der Lokalisierung")

#### <a name="strings-in-code"></a>Zeichenfolgen im Code

Die **Localizable.strings** Datei besitzt eine etwas andere Struktur als ein Storyboard zugeordnet wurde. In diesem Fall kann eine beliebige Zeichenfolge "Key" ausgewählt werden; Apple Empfehlung besteht darin, einen Schlüssel zu verwenden, der den tatsächlichen Text darstellt, der in der Standardsprache angezeigt wird:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

Die `NSBundle.MainBundle.LocalizedString` Methode dient zum Auflösen von Zeichenfolgen in ihre Entsprechungen mit übersetzten Werten, wie im folgenden Code gezeigt.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Bilder in Code

Images, die vom Code aufgefüllt werden, können auf zwei Arten festgelegt werden.

1. Sie können ändern, eine `Image` Steuerelement durch Festlegen des Werts auf den Namen der Bilddatei, bereits vorhanden ist in der Watch-App, z.B.

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. Sie können ein Bild von der Erweiterung wechseln, sehen Sie sich mit `FromBundle` die app wählt automatisch das richtige Bild für die ausgewählte Sprache des Benutzers. In der Beispielprojektmappe ist ein Bild **language@2x.png** in der jeweiligen Sprache Ordner, und es wird auf `DetailController` mit dem folgenden Code:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  Beachten Sie, die Sie nicht benötigen, geben Sie die **@2x** beim Verweisen auf den Dateinamen des Bilds.

Die zweite Methode ist auch gilt, wenn Sie ein Image von einem Remoteserver Rendern auf der Apple Watch herunterladen. sollte in diesem Fall Sie jedoch sicherstellen, dass das Bild, die, das Sie herunterladen, ordnungsgemäß gemäß den Einstellungen des Benutzers lokalisiert wird.

## <a name="localization"></a>Lokalisierung

Nachdem Sie Ihre Lösung konfiguriert haben, müssen Übersetzer zu verarbeiten Ihrer **.strings** Dateien und Bilder für jede Sprache, die Sie unterstützen möchten.

Sie können so viele erstellen **.lproj** Verzeichnisse als Sie benötigen (eine für jede unterstützte Sprache). Sie werden bezeichnet, mit Sprachcodes, z. B. **En**, **es**, **de**, **Japan**, **pt-BR**, usw. (für Englisch Spanisch, Deutsch, Japanisch und Portugiesisch (Brasilien) bzw.).

Das angefügte Beispiel verwendet die Übersetzungen (computergenerierte) veranschaulicht, wie Sie eine WatchOS-app zu lokalisieren.

### <a name="watch-app"></a>Watch-App

Diese Werte werden verwendet, um die Benutzeroberfläche, die in der Watch-app-Storyboard definiert zu übersetzen. Die *Schlüssel* Wert ist eine Kombination jedes Storyboard-Steuerelements **Lokalisierungs-ID** und die Eigenschaft, die übersetzt wird.

Es wird empfohlen, zum Hinzufügen von Kommentaren, die den ursprünglichen Text in die Datei enthält, damit Übersetzer wissen, was die Übersetzung werden soll.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

Die Zeichenfolgen (maschinell übersetzter) Spanischer Sprache, für das Storyboard werden unten angezeigt. Es ist hilfreich, Kommentare zu jeder Zeile hinzufügen, da es schwierig ist, wissen, was die **Lokalisierungs-ID** andernfalls verweist:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Watch-Erweiterung

Diese Werte werden im Code zum Übersetzen von Informationen, bevor Sie dem Benutzer angezeigt wird. Die *Schlüssel* vom Entwickler ausgewählt ist, während sie Code schreiben, und in der Regel enthält die tatsächliche Zeichenfolge übersetzt werden.

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings-Datei

Die (maschinell übersetzter) Spansish sprachenzeichenfolgen:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Test

Die Methode zum Ändern der spracheinstellungen unterscheidet sich zwischen den Simulator und physische Geräte.

### <a name="simulator"></a>Simulator

Wählen Sie auf dem Simulator, die Sprache zu testen, mit dem iOS- **Einstellungen** app (in der Simulator-Startseite das Symbol "grauen Gears").

  ![](localization-images/sim-settings-sml.png "Der iOS-Einstellungen für app-Lokalisierung")

### <a name="watch-device"></a>Watch-Geräte

Beim Testen mit einem Überwachungselement, ändern Sie die Überwachung der Sprache in der **Apple Watch** app auf dem gekoppelten iPhone.

  ![](localization-images/phone-settings-sml.png "Ändern Sie die Überwachung der Sprache in der Apple Watch-app auf dem gekoppelten iPhone")



## <a name="related-links"></a>Verwandte Links

- [WatchLocalization (Beispiel)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchLocalization/)
