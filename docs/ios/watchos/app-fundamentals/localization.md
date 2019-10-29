---
title: Arbeiten mit der watchos-Lokalisierung in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit xamarin erstellten watchos-apps lokalisieren. Dabei werden Watch-apps, Überwachungs Erweiterungen, Zeichen folgen im Code, storyboardtext, Tests und mehr erläutert.
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 82b739697705ac4c90390a36405d755a5f523159
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028407"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Arbeiten mit der watchos-Lokalisierung in xamarin

_Anpassen Ihrer watchos-Apps für mehrere Sprachen_

![](localization-images/both-languages-sml.png "Apple Watch displaying localized content")

watchos-apps werden mithilfe der standardmäßigen IOS-Methoden lokalisiert:

- Verwenden der **Lokalisierungs-ID** für Storyboard-Elemente
- **. Strings** -Dateien, die dem Storyboard zugeordnet sind, und
- **Lokalisierbare Strings** -Dateien für Text, der im Code verwendet wird.

Die Standard-Storyboards und-Ressourcen befinden sich in einem **Basis** Verzeichnis, und sprachspezifische Übersetzungen und andere Ressourcen werden in **lproj** -Verzeichnissen gespeichert.
das Betriebssystem IOS und Watch verwendet automatisch die Sprachauswahl des Benutzers, um die richtigen Zeichen folgen und Ressourcen zu laden.

Da eine Apple Watch-App zwei Teile hat: Überwachen von App-und Watch-Erweiterungen, sind lokalisierte Zeichen folgen Ressourcen an zwei Stellen erforderlich, je nachdem, wie Sie verwendet werden.

Der lokalisierte Text und die Ressourcen *unterscheiden* sich in der Watch-APP und der Watch-Erweiterung.

## <a name="watch-app"></a>Watch-App

Die Watch-app enthält das Storyboard, das die Benutzeroberfläche der APP beschreibt. Alle Steuerelemente (z. b. `Label` und `Image`), die die Lokalisierung unterstützen, haben eine **Lokalisierungs-ID**

Jedes sprachspezifische **. lproj** -Verzeichnis sollte **. Strings** -Dateien mit den Übersetzungen für jedes Element (mit der **Lokalisierungs-ID**) sowie Bilder enthalten, auf die das Storyboard verweist.

## <a name="watch-extension"></a>Erweiterung überwachen

In der Watch-Erweiterung wird der app-Code ausgeführt. Jeder Text, der dem Benutzer aus dem Code angezeigt wird, muss in der Erweiterung und nicht in der Watch-App lokalisiert werden.

Die Erweiterung sollte auch sprachspezifische **lproj** -Verzeichnisse enthalten, aber die **Strings** -Dateien erfordern nur Übersetzungen für Text, der in Ihrem Code verwendet wird.

## <a name="globalizing-the-watch-solution"></a>Globalisieren der Überwachungslösung

Die Globalisierung ist der Prozess, bei dem eine Anwendung lokalisierbar ist.
Für Watch-apps bedeutet dies, dass das Storyboard mit unterschiedlichen Textlängen entworfen wird. Dadurch wird sichergestellt, dass jedes Bildschirmlayout entsprechend dem angezeigten Text ordnungsgemäß angepasst wird. Außerdem müssen Sie sicherstellen, dass alle Zeichen folgen, auf die im Überwachungs Erweiterungs Code verwiesen wird, mit der `LocalizedString`-Methode übersetzt werden können.

### <a name="watch-app"></a>Watch-App

Standardmäßig ist die Watch-APP nicht für die Lokalisierung konfiguriert. Sie müssen die Standard-storyboarddatei verschieben und einige andere Verzeichnisse für Ihre Übersetzungen erstellen:

1. Erstellen Sie das Basisverzeichnis " **. lproj** ", und verschieben Sie das **Interface. Storyboard** in dieses Verzeichnis.

2. Erstellen Sie **\<Sprache >. lproj** -Verzeichnisse für jede Sprache, die Sie unterstützen möchten.

3. Die **lproj** -Verzeichnisse sollten eine **Interface. Strings** -Textdatei enthalten (der Dateiname sollte dem Namen des storboards entsprechen). Optional können Sie alle Bilder platzieren, die in diesen Verzeichnissen lokalisiert werden müssen.

Das Überwachungs-App-Projekt sieht wie folgt aus, nachdem diese Änderungen vorgenommen wurden (es wurden nur englische und spanische Sprachdateien hinzugefügt):

  ![](localization-images/watchapp-solution.png "The watch app project with English and Spanish language files")

#### <a name="storyboard-text"></a>Storyboardtext

Wenn Sie das Storyboard bearbeiten, wählen Sie jedes Element aus, und beachten Sie die **Lokalisierungs-ID** , die im **eigenschaftenpad** angezeigt wird:

  [![](localization-images/storyboard-sml.png "The Localization ID that appears in the Properties pad")](localization-images/storyboard.png#lightbox)

Erstellen Sie im Ordner **base. lproj** die Schlüssel-Wert-Paare wie unten dargestellt, wobei der Schlüssel durch die **Lokalisierungs-ID** und einen Eigenschaftsnamen im Steuerelement gebildet wird, verknüpft durch einen Punkt (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

Beachten Sie in diesem Beispiel, dass eine **Lokalisierungs-ID** eine einfache Zahlen Zeichenfolge sein kann (z. b. "0", "1" usw.) oder eine komplexere Zeichenfolge (z. b. "AGC-El-HGC"). `Label` Steuerelemente über eine `Text`-Eigenschaft verfügen und `Button`s über eine `Title`-Eigenschaft verfügen, die sich in der Art und Weise der Festlegung ihrer lokalisierten Werte widerspiegelt, sollten Sie den Namen der Kleinbuchstaben verwenden, wie im obigen Beispiel gezeigt.

Wenn das Storyboard auf der Überwachung gerendert wird, werden die korrekten Werte automatisch extrahiert und entsprechend der vom Benutzer ausgewählten Sprache angezeigt.

#### <a name="storyboard-images"></a>Storyboard-Bilder

Die Beispiellösung umfasst auch ein **gradient@2x.png** Image in jedem Sprachordner. Dieses Bild kann sich für jede Sprache unterscheiden (z. b. Möglicherweise ist ein eingebetteter Text vorhanden, der übersetzt werden muss oder lokalisierte iconographie verwendet werden muss.

Legen Sie einfach die **Image** -Eigenschaft des Bilds im Storyboard fest, und das richtige Bild wird auf der Überwachung entsprechend der vom Benutzer ausgewählten Sprache gerendert.

![](localization-images/storyboard-image.png "Set the images Image property in the storyboard")

Hinweis: da alle Apple Watch Retina-anzeigen haben, ist nur die **@2x** Version des Bilds erforderlich. Sie müssen **@2x** im Storyboard nicht angeben.

### <a name="watch-extension"></a>Erweiterung überwachen

Die Watch-Erweiterung erfordert eine ähnliche Verzeichnisstruktur zur Unterstützung der Lokalisierung, aber es ist kein Storyboard vorhanden. Die lokalisierten Zeichen folgen in der Erweiterung sind nur die C# , auf die durch Code verwiesen wird.

![](localization-images/watchextension-solution.png "The watch extension directory structure to support localization")

#### <a name="strings-in-code"></a>Zeichen folgen im Code

Die **Lokalisierbare Strings** -Datei hat eine etwas andere Struktur, als wenn Sie einem Storyboard zugeordnet wäre. In diesem Fall können wir eine beliebige "Key"-Zeichenfolge auswählen. Apple empfiehlt die Verwendung eines Schlüssels, der den tatsächlichen Text widerspiegelt, der in der Standardsprache angezeigt wird:

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

Die `NSBundle.MainBundle.LocalizedString`-Methode wird verwendet, um Zeichen folgen in Ihre übersetzten Entsprechungen aufzulösen, wie im folgenden Code dargestellt.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>Bilder im Code

Bilder, die durch Code aufgefüllt werden, können auf zwei Arten festgelegt werden.

1. Sie können ein `Image` Steuerelement ändern, indem Sie dessen Wert auf den Zeichen folgen Namen eines Images festlegen, das bereits in der Watch-app vorhanden ist, z. b.

    ```csharp
    displayImage.SetImage("gradient"); // image in Watch App (as shown above)
    ```

2. Sie können ein Bild von der Erweiterung in die Überwachung mithilfe `FromBundle` verschieben, und die APP wählt automatisch das richtige Bild für die Sprachauswahl des Benutzers aus. In der Beispiellösung wird ein Bild **language@2x.png** in jedem Sprachordner angezeigt, und es wird auf `DetailController` mit folgendem Code angezeigt:

    ```csharp
    using (var image = UIImage.FromBundle ("language")) {
        displayImage.SetImage (image);
    }
    ```

    Beachten Sie, dass Sie die **@2x** nicht angeben müssen, wenn Sie auf den Dateinamen des Bilds verweisen.

Die zweite Methode ist auch anwendbar, wenn Sie ein Abbild von einem Remote Server herunterladen, um es auf der Uhr zu Rendering. in diesem Fall sollten Sie jedoch sicherstellen, dass das Abbild, das Sie herunterladen, gemäß den Einstellungen des Benutzers ordnungsgemäß lokalisiert wird.

## <a name="localization"></a>Lokalisierung

Nachdem Sie die Lösung konfiguriert haben, müssen die Konvertierungsprogramme Ihre **Strings** -Dateien und-Images für jede Sprache verarbeiten, die Sie unterstützen möchten.

Sie können beliebig viele **lproj** -Verzeichnisse erstellen (eine für jede unterstützte Sprache). Sie werden mithilfe von Sprachcodes benannt, wie z. b. **en**, **es**, **de**, **Ja**, **pt-br**usw. (für Englisch, Spanisch, Deutsch, Japanisch und Portugiesisch (Brasilianisch)).

Das angefügte Beispiel verwendet (computergenerierte) Übersetzungen, um zu veranschaulichen, wie eine watchos-App lokalisiert wird.

### <a name="watch-app"></a>Watch-App

Diese Werte werden verwendet, um die Benutzeroberfläche zu übersetzen, die im Storyboard der Watch-App definiert ist. Der *Schlüssel* Wert ist eine Kombination aus der **Lokalisierungs-ID** jedes Storyboard-Steuer Elements und der Eigenschaft, die übersetzt wird.

Es wird empfohlen, der Datei Kommentare hinzuzufügen, die den ursprünglichen Text enthalten, damit Konvertierer wissen, was die Übersetzung sein sollte.

#### <a name="eslprojinterfacestrings"></a>es. lproj/Interface. Strings

Die (maschinell übersetzt) spanischsprachigen Zeichen folgen für das Storyboard sind unten dargestellt. Es ist hilfreich, jeder Zeile Kommentare hinzuzufügen, da es schwierig ist, zu wissen, auf welche **Lokalisierungs-ID** verwiesen wird:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>Erweiterung überwachen

Diese Werte werden im Code verwendet, um Informationen zu übersetzen, bevor Sie dem Benutzer angezeigt werden. Der *Schlüssel* wird vom Entwickler beim Schreiben von Code ausgewählt und enthält in der Regel die tatsächlich zu über setzende Zeichenfolge.

#### <a name="eslprojlocalizablestrings-file"></a>es. lproj/lokalisierbare. Strings-Datei

Die (maschinell übersetzt) spansish Language Strings:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>Test

Die Methode zum Ändern der Spracheinstellungen unterscheidet sich zwischen dem Simulator und physischen Geräten.

### <a name="simulator"></a>Simulator

Wählen Sie im Simulator die zu testende Sprache mithilfe der IOS- **Einstellungs** -App aus (das graue Zahnrad Symbol auf dem Startbildschirm des Simulators).

  ![](localization-images/sim-settings-sml.png "The iOS Settings app Localization settings")

### <a name="watch-device"></a>Gerät überwachen

Wenn Sie mit einer Überwachung testen, ändern Sie die Sprache der Uhr in der **Apple Watch** -App auf dem gekoppelten iPhone.

  ![](localization-images/phone-settings-sml.png "Change the watch's language in the Apple Watch app on the paired iPhone")

## <a name="related-links"></a>Verwandte Links

- [Watchlocalization (Beispiel)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchLocalization/)
