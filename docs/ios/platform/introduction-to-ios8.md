---
title: "Einführung in iOS 8"
description: "Apple hat mit iOS 8 eine Fülle von neuen Frameworks und APIs auszeichnen und suchen zu erfreuen Entwickler bereitgestellt. In diesem Handbuch wird diese neuen APIs einführen und finden Sie unter erläutert, wie iOS 8 Entwickler und Benutzer profitieren können."
ms.topic: article
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 8a4fabd5cc63434950f4646336b06676f6eb915b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-ios-8"></a>Einführung in iOS 8

_Apple hat mit iOS 8 eine Fülle von neuen Frameworks und APIs auszeichnen und suchen zu erfreuen Entwickler bereitgestellt. In diesem Handbuch wird diese neuen APIs einführen und finden Sie unter erläutert, wie iOS 8 Entwickler und Benutzer profitieren können._

iOS 7 geändert visuell die gesamten IOS-Benutzeroberfläche aus, welche Benutzer und Entwickler stammen mussten, direkt aus dem ersten iPhone OS erwartet. IOS 8 weiterhin mit dieser durch die Bereitstellung von vielen Frameworks für Entwickler, die Benutzern ermöglicht, fast jeden Aspekt von deren Lebensdauer, die gerade mit ihrer iPhone zu steuern. Z. B. System- und Eignung können analysiert werden mit *HealthKit*, Passcodes sind mit der Verwendung der biometrischen Authentifizierung obsolescent *LocalAuthentication*, *App-Erweiterungen*öffnen Sie den Kommunikationskanal zwischen dem 3. apps von Drittanbietern und *HomeKit* können Sie die Möglichkeit, ein Haus der Zukunft Haus verwandeln. 

War zum Benutzer delighting iOS 7 schwerpunktmäßig iOS 8 delighting Entwickler, die mit einer Vielzahl von diesen RAR neue Tools ab. 

Dieses Handbuch enthält die neuen APIs für Entwickler von Xamarin.iOS.  

Es gibt auch einige APIs, die im iOS 8 veraltet sind die am Ende dieses Dokuments erläutert werden.

## <a name="requirements"></a>Anforderungen

Im folgenden sind erforderlich zum Erstellen von iOS 8-apps in Visual Studio für Mac:

- **Xcode 7 und iOS 8 oder neueren** – Apple neuesten Xcode und iOS-APIs installiert und auf dem Computer des Entwicklers konfiguriert werden müssen.
- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden.
- **iOS 8-Gerät oder den Simulator** – ein iOS-Gerät, das die aktuelle Version des iOS 8 für das Testen.

## <a name="home-and-leisure"></a>Startseite "und" Freizeit

iOS 8 hat beigetragen, dass Apple und das iOS-Gerät ist direkt in das Kernstück von Haus durch die Verwendung von HomeKit und HealthKit einzuschleusen. In diesem Abschnitt untersuchen wir wird wie beide diese neue Frameworks funktionieren und wie sie in Ihrer Anwendung Xamarin.iOS integriert werden können.

## <a name="homekit"></a>HomeKit

Steuern Ihre Geräte mit Ihrem iPhone ist keine neue Anwendung der Technologie; Viele verbunden Home-Produkte können über eine iOS-app gesteuert werden. Jedoch wird HomeKit jetzt dies noch einen Schritt weiter durch ein gemeinsames Protokoll für home Automation Geräte heraufstufen und und eine öffentliche API für bestimmte Hersteller, z. B. iHome, überprüft und Honeywell zur Verfügung. Für den Benutzer dies bedeutet, dass sie fast jeden Aspekt von zuhause nahtlos von steuern können innerhalb einer Anwendung. Dabei ist es unerheblich, damit sie wissen, dass sie eine Glühbirne überprüft Farbton oder eine Schachtelung Alarm verwenden. Benutzer können auch zahlreiche intelligenten home Prozesse zusammen in "Szenen" verketten.

Mit HomeKit können Drittanbieter-apps Siri Zubehör ermitteln und sie ihre persönliche home Konfigurationsdatenbank hinzufügen, bearbeiten und werden auf diese Daten angewendet und kommunizieren mit Zubehör und ihre Dienste zur Ausführung einer Aktion.

### <a name="configuration"></a>Konfiguration

Das folgende Diagramm zeigt die Hierarchie der Konfiguration von HomeKit Zubehör:

![](introduction-to-ios8-images/image1.png "Dieses Diagramm zeigt die Hierarchie der Konfiguration von HomeKit Zubehör")
 
Um mit HomeKit beginnen, müssen Entwickler sicherstellen, dass ihre provisioning-Profil ausgewählten HomeKit-Dienst hat. Apple hat auch Entwickler mit einem HomeKit Simulator-add-in für Xcode bereitgestellt. Diese finden Sie in der [Apple Developer Center](https://developer.apple.com/downloads/index.action)unter `Hardware IO Tools for Xcode`. 

Weitere Informationen finden Sie unter unsere [HomeKit](~/ios/platform/homekit.md) Handbuch.

## <a name="healthkit"></a>HealthKit

HealthKit ist ein Framework, eingeführt in iOS 8, die einen zentralisierte, koordinierten und sicheren Datenspeicher für Integrität bezogene Informationen bereitstellt. Das Betriebssystem wird sichergestellt, dass der Datenschutz und Sicherheit der Zustandsinformationen und mit der Integrität-app ein Dashboard für den Benutzer. Mit der Berechtigung des Benutzers können Anwendungen lesen und Schreiben von einer Vielzahl von Zustandsinformationen.

Weitere Informationen zur Verwendung dieser in Ihrer app Xamarin.iOS finden Sie in der [Einführung in HealthKit](~/ios/platform/healthkit.md) Handbuch.

## <a name="extending-iphone-functionality"></a>Erweitern Sie die iPhone-Funktion
Mit iOS8 sind die Entwickler gegeben wird deutlich mehr Kontrolle darüber, wer ihre app und eine höhere Kapazität für weitere offene Kommunikation zwischen apps von Drittanbietern verwenden kann. Funktionen wie App-Erweiterungen und die Auswahl einer Dokument öffnen eine Welt mit Möglichkeiten für wie Anwendungen in der Apple-Ökosystem verwendet werden können.

### <a name="app-extensions"></a>App-Erweiterungen
App-Erweiterungen, um zu stark vereinfachen durch, bieten eine Möglichkeit für Drittanbieter-apps, die miteinander kommunizieren. Hohe Sicherheitsstandards ausüben und die Integrität der Sandbox-apps einzuhalten, zieht nicht direkt zwischen Anwendungen diese Kommunikation. Stattdessen, es erfolgt durch ein *Erweiterung* in der Mitte aufweist.

Der erste Schritt beim Erstellen einer App-Erweiterung zum Definieren des richtigen Erweiterungspunkts wird – dies ist wichtig, das Verhalten und die Verfügbarkeit der richtigen APIs zu gewährleisten. Um eine App-Erweiterung in Visual Studio für Mac zu erstellen, fügen Sie es für eine vorhandene Anwendung durch Hinzufügen eines neuen Projekts zur Projektmappe aus.

In der **neues Projekt** Dialogfeld navigieren Sie zu **c#** > **iOS** > **einheitliche API**  >   **Erweiterungen**, wie im folgenden Screenshot gezeigt:

![](introduction-to-ios8-images/image2.png "Erstellen einer neuen Erweiterungs")
 
Das Dialogfeld "Neues Projekt" enthält sieben neue Projektvorlagen zum Erstellen von App-Erweiterungen, und es werden unten erläutert. Beachten Sie, dass viele der Erweiterungen mit anderen neuen APIs in iOS, z. B. Auswahl des Dokuments in Zusammenhang stehen:

- **Aktion** – Dies ermöglicht Entwicklern das Erstellen von eindeutigen benutzerdefinierten Aktionsschaltflächen so, dass Benutzer bestimmte Aufgaben ausgeführt
- **Benutzerdefinierte Tastenkombinationen** – Dies ermöglicht Entwicklern Wertebereich ein hinzuzufügende integrierte Apple Tastaturen durch ihre eigenen benutzerdefinierten hinzufügen. Der beliebten Tastatur verwendet Swype diese auf um ihrer Tastatur auf iOS zu bringen.
- **Dokumentieren Sie die Auswahl einer** – ein Dokument Picker-View-Controller die ermöglicht dem Benutzer den Zugriff auf Dateien außerhalb der Anwendung Sandkasten enthält.
- **Dokumentieren Picker-Dateianbieter** – Dies bietet sichere Speicherung für Dateien, die mit der Auswahl einer Dokument.
- **Foto bearbeiten** – diese erweitert den Filtern und Bearbeiten von bereits in der Anwendung Fotos, damit Benutzer mehr Kontrolle und weitere Optionen beim Bearbeiten ihrer Fotos von Apple bereitgestellten Tools.
- **Heute** – Dies ermöglicht Anwendungen die Widgets im Abschnitt "heute" der Mitteilungszentrale angezeigt.

Weitere Informationen zum Verwenden von App-Erweiterungen in Xamarin finden Sie in der [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md) Handbuch.

### <a name="touch-id"></a>Touch ID

Touch ID wurde als Mittel zum Authentifizieren des Benutzers in iOS 7 eingeführt – eine Kennung ähnelt. Es wurde jedoch zum Entsperren des Geräts, den App Store, mithilfe der iTunes und authentifizieren den iCloud-Schlüsselbund nur begrenzte 

Es sind nun zwei Möglichkeiten zur Verwendung von Touch ID als Authentifizierungsmechanismus in iOS 8-Anwendungen, die mithilfe der lokalen Authentifizierung-API. Es ist derzeit nicht möglich, lokale Authentifizierung zu verwenden, um die Remote authentifiziert werden.

Erstens unterstützt er die vorhandenen Dienste für die Schlüsselsammlung durch die Verwendung von neuen Schlüsselbund Zugriffssteuerungslisten (ACLs). Schlüsselbund Daten können mit der erfolgreichen Authentifizierung Benutzer Fingerabdruck entsperrt werden.

Zweitens stellt LocalAuthentication zwei Methoden, um die Anwendung lokal zu authentifizieren. Entwickler sollten verwenden `CanEvaluatePolicy` zu bestimmen, ob das Gerät zur Annahme von Touch ID fähig ist und dann `EvaluatePolicy` Starten des Vorgangs für die Authentifizierung.

Weitere Informationen auf Touch ID und erfahren, wie es in einem Xamarin.iOS-Anwendung integriert, finden Sie in der [Einführung um Touch ID](~/ios/platform/touchid.md) Handbüchern.

### <a name="document-picker"></a>Dokument-Auswahl

Dokument Datumsauswahl arbeitet mit einem Benutzer iCloud-Laufwerk, damit der Benutzer zum Öffnen von Dateien, die in eine andere app erstellt wurden, importieren und bearbeiten und exportieren Sie sie zurück wieder. Dies erstellt eine intuitive Workflow und daher eine bessere navigationsmöglichkeiten, für Benutzer. einen Schritt weiter akzeptiert iCloud synchronisieren – alle Änderungen, die in einer Anwendung wird auch konsistent allen Geräten angewendet werden.

Die Auswahl einer Dokument ausführlicher Informationen und erfahren, wie es in einem Xamarin.iOS-Anwendung integriert, finden Sie in der [Einführung in das Dokument Datumsauswahl](~/ios/platform/document-picker.md) Handbuch.

### <a name="handoff"></a>Übergabe

Übergabe, die Teil der größeren Geschäftskontinuität-Funktion ist, wird einen Schritt in Richtung der Integration von OS X und iOS. Dies schließt plattformübergreifende AirDrop, iPhone Anrufe, SMS, auf dem iPad und Mac, und Verbesserungen in tethering auf Ihrem iPhone aufnehmen.

Übergabe kann mit iOS 8 und Yosemite und erfordert ein iCloud-Konto für alle anderen Geräte angemeldet sein, dass Sie verwenden möchten. Sie muss mit vorinstallierte am Apple-apps, einschließlich Safari, iWork, Karten, Kalender und Kontakte ausgeführt werden.

Weitere Informationen finden Sie unter unsere [Übergabe](~/ios/platform/handoff.md) Handbuch.

## <a name="unified-storyboards"></a>Einheitliche Storyboards
iOS 8 umfasst ein neues einfacher Mechanismus zum Erstellen der Benutzeroberfläche zu verwenden – das einheitliche Storyboard. Mit einem einzelnen Storyboard aller der unterschiedliche Hardware Bildschirmgrößen abdecken können schnelle und reaktionsschnelle Sichten in einem "true" "Entwerfen Sie einmal, viele" Stil erstellt werden.

Vor iOS8, Entwickler verwendet `UIInterfaceOrientation` Hochformat- und querformatausrichtung Modi unterscheiden und `UIInterfaceIdiom` iOS-Geräte unterscheiden. In iOS8 ist es nicht mehr erforderlich, separate Storyboards für iPhone und iPad-Geräte zu erstellen – Ausrichtung und die Geräte werden mit bestimmt *Größenklassen*.

Jedes Gerät wird mit einer Größe-Klasse, in der vertikalen und horizontalen Achse definiert, und es gibt zwei Arten von Größenklassen in iOS 8:

- **Reguläre** -Dies ist für eine große Bildschirmgröße (z. B. einem iPad) oder ein Gadget, die den Eindruck von eine beträchtliche Größe (z. B. ein UIScrollView-Element enthält.
- **Compact** -Dies ist für kleinere Geräte (z. B. das iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.

Wenn die beiden Konzepte zusammen verwendet werden, ist das Ergebnis ein 2 x 2-Raster mit den definiert die verschiedenen möglichen Größen, die in den beiden unterschiedlichen Ausrichtungen verwendet werden können, wie im folgenden Diagramm dargestellt:

![](introduction-to-ios8-images/image3.png "Ein Auszug aus der 2 x 2-Raster, das die verschiedenen möglichen Größen, die verwendet werden, können in beide die verschiedenen Ausrichtungen definiert")
 
Weitere Informationen zu Größenklassen finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Foto Kit
Foto-Kit ist ein neues Framework, das ermöglicht es Anwendungen, die systembilderbibliothek Abfragen und die Erstellung benutzerdefinierter Benutzeroberflächen zum Anzeigen und ändern den Inhalt. Umfasst eine Reihe von Klassen, die Bild und video Bestand sowie Auflistungen von Ressourcen wie z. B. Alben und Ordner darstellen.

Weitere Informationen finden Sie unter unsere [PhotoKit](~/ios/platform/photokit.md) Handbuch.

## <a name="games"></a>Spiele

<a name="scenekit" />

### <a name="scene-kit"></a>Szene Kit

Szene Kit ist eine 3D-Szene-Graph-API, die das Arbeiten mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Szene Kit erfordert erstellen beeindruckende 3D Visualisierungen und gelegentlichen 3D-Spielen Kenntnisse OpenGL keine. Baut auf allgemeine Konzepte der Szene-Diagramm und abstrahiert Szene Kit die Komplexitäten von OpenGL- und OpenGL-ES, die Dies erleichtert die sehr 3D hinzuzufügende Inhalt zu einer Anwendung. Wenn Sie eine OpenGL-Experte sind, hat Szene Kit allerdings bietet umfassende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Funktionen, die 3D Grafiken, z. B. physikalische, ergänzen und mehrere andere Apple Frameworks, z. B. Animation Core, Core-Image und Sprite Kit sehr gut integriert.

Weitere Informationen finden Sie unter unsere [SceneKit](~/ios/platform/gaming/scenekit.md) Dokumentation.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite Kit

Sprite Kit, das Spiel 2D Framework von Apple, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu zählen die Integration in Szene Kit, Shader-Unterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnung Generation und physikalische Erweiterungen. Insbesondere sollten es die neuen Funktionen für physikalische sehr einfach, ein Spiel realistische Effekte hinzufügen.

Weitere Informationen finden Sie unter unsere [SpriteKit](~/ios/platform/gaming/spritekit.md) Dokumentation.

## <a name="other-changes"></a>Weitere Änderungen
Sowie wichtigen Änderungen an der iOS 8, die oben beschrieben werden, verfügt über Apple außerdem viele vorhandene Frameworks aktualisiert. Diese werden unten genauer beschrieben:

- **[Core-Image](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple hat bei der Image-verarbeitungsframework erweitert, indem Sie eine bessere Unterstützung für die Erkennung von rechteckige Bereiche hinzufügen und QR-codes in Bildern. Mike Bluestein untersucht dies in seinem Blog post Anspruch [Image Erkennung in iOS 8](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs
Mit allen Verbesserungen in iOS 8 vorgenommen wurden wurden eine Reihe von APIs als veraltet. Einige davon werden unten genauer beschrieben.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – die verwendeten Methoden und Eigenschaften für die remote-Benachrichtigungen registriert wurden als veraltet. Dies sind RegisterForRemoteNotificationTypes und EnabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – "Traits" und "Größe ersetzt haben, die Methoden und Eigenschaften verwendet, um die Ausrichtung der Schnittstelle beschreiben. Finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) für Weitere Informationen zum verwenden.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  – dies wurde ersetzt durch UISearchController in iOS8.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel, die wir auf einige der neuen Funktionen von Apple iOS 8 gesucht haben.



## <a name="related-links"></a>Verwandte Links

- [UIKitEnhancements (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Einführung in die Auswahl einer Dokument](~/ios/platform/document-picker.md)
- [Einführung in HealthKit](~/ios/platform/healthkit.md)
- [Einführung in die manuelle Kamera-Steuerelemente](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Einführung in die Verwendung von Touch ID](~/ios/platform/touchid.md)
- [Einführung in die einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
