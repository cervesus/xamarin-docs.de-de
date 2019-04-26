---
title: Einführung in iOS 8
description: Apple hat mit iOS 8 eine Fülle von neuen Frameworks und begeistern, und begeistern Sie Entwickler-APIs bereitgestellt. In diesem Handbuch wir diese neuen APIs eingeführt, und wie die iOS 8 Entwickler und Benutzer unterstützen kann.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 9299322eb20561444262c2b2ba87191d2bddcde4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61317635"
---
# <a name="introduction-to-ios-8"></a>Einführung in iOS 8

_Apple hat mit iOS 8 eine Fülle von neuen Frameworks und begeistern, und begeistern Sie Entwickler-APIs bereitgestellt. In diesem Handbuch wir diese neuen APIs eingeführt, und wie die iOS 8 Entwickler und Benutzer unterstützen kann._

iOS 7 geändert visuell die gesamte iOS-Benutzeroberfläche aus, welche Benutzer und Entwickler zu erwarten, direkt aus der ersten iPhone-Betriebssystem ausgestattet gewesen. IOS 8 weiterhin mit diesem durch die Bereitstellung von vielen Frameworks für Entwickler, der Benutzern ermöglicht, fast jeden Aspekt ihrer Lebensdauer direkt über die iPhone zu steuern. Z. B. Integrität und Fitness analysiert werden können mit *HealthKit*, Kennungen sind veraltet, biometrische Authentifizierung mit *LocalAuthentication*, *App-Erweiterungen*Öffnen eines Kommunikationskanals zwischen 3rd Party-apps und *HomeKit* können Sie die Möglichkeit, Ihr Haus in ein Zuhause für die Zukunft zu verwandeln. 

War iOS 7 zu erfreuen Benutzer, konzentriert sich die iOS 8 für Entwickler schätzen, die mit einer Vielzahl dieser tasty neuen Tools. 

Dieser Leitfaden beschreibt die neuen APIs für Xamarin.iOS-Entwickler.  

Es gibt auch einige APIs, die in iOS 8 veraltet sind die am Ende dieses Dokuments beschrieben werden.

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die iOS 8-apps in Visual Studio für Mac zu erstellen:

- **Xcode 7 und iOS 8 oder neueren** – die neuesten Xcode und Apple iOS-APIs müssen installiert und konfiguriert werden, auf dem Computer des Entwicklers.
- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac installiert und auf dem Benutzergerät konfiguriert werden soll.
- **iOS 8-Gerät oder Simulator** – ein iOS-Gerät mit der neuesten Version von iOS 8 für das Testen.

## <a name="home-and-leisure"></a>Heim- und Freizeit

iOS 8 hat dazu beigetragen, weshalb Apple und das iOS-Gerät direkt in den Kern Ihres Zuhauses, durch die Verwendung von HomeKit und HealthKit einzuschleusen. In diesem Abschnitt betrachten wir beide diese neue Frameworks funktionieren und wie sie in Ihrer Xamarin.iOS-Anwendung integriert werden können.

## <a name="homekit"></a>HomeKit

Steuern Ihre Geräte auf Ihrem iPhone ist keine neue Anwendung der Technologie; Viele verbunden – Startseite Produkte können über eine iOS-app gesteuert werden. Jedoch geht HomeKit jetzt noch einen Schritt weiter durch die Förderung von eines gemeinsamen Protokolls für heimautomatisierungsgeräte und eine öffentliche API verfügbar zu machen. bestimmte Herstellern, z. B. iHome, Philips und Honeywell. Für den Benutzer bedeutet dies, dass sie fast jeden Aspekt ihrer Startseite nahtlos aus steuern können innerhalb einer Anwendung. Dabei ist es unerheblich, feststellen, dass sie eine Glühbirne Philips Hue oder ein Alarm für die Schachtelung verwenden. Benutzer können auch zahlreiche intelligente home-Prozesse in "Szenen" verketten.

Mit HomeKit können Drittanbieter-apps und Siri Zubehör ermitteln ihrer persönlichen Startseite Konfigurationsdatenbank hinzufügen, bearbeiten und werden auf diese Daten angewendet, und kommunizieren mit der Verpackung für Zubehör und ihre Dienste zur Ausführung einer Aktion.

### <a name="configuration"></a>Konfiguration

Das folgende Diagramm zeigt die Hierarchie der Konfiguration des HomeKit-Zubehör:

![](introduction-to-ios8-images/image1.png "Dieses Diagramm zeigt die Hierarchie der Konfiguration des HomeKit-Zubehör")
 
Um den ersten Schritten mit HomeKit müssen Entwickler sicherstellen, dass ihre Bereitstellungsprofil den HomeKit-Dienst ausgewählt verfügt. Apple bereitgestellt für Xcode auch Entwicklern ein HomeKit-Simulator-add-in. Diese finden Sie in der [Apple Developer Center](https://developer.apple.com/downloads/index.action)unter `Hardware IO Tools for Xcode`. 

Weitere Informationen finden Sie unserem [HomeKit](~/ios/platform/homekit.md) Guide.

## <a name="healthkit"></a>HealthKit

HealthKit ist ein Framework, eingeführt in iOS 8, die einen zentralisierten, koordinierten und sicheren Datenspeicher für integritätsbezogene Informationen bereitstellt. Das Betriebssystem wird sichergestellt, Datenschutz und zur Sicherheit von Gesundheitsinformationen und, mit der Health-app, ein Dashboard für den Benutzer. Mit der Berechtigung des Benutzers können Anwendungen lesen und schreiben eine Vielzahl von Gesundheitsinformationen.

Weitere Informationen zur Verwendung in Ihrer Xamarin.iOS-app finden Sie in der [Einführung in HealthKit](~/ios/platform/healthkit.md) Guide.

## <a name="extending-iphone-functionality"></a>Erweitern Sie die iPhone-Funktion
Klicken Sie mit IOS 8 sind Entwickler übergeben wird deutlich mehr Kontrolle darüber, wer ihre app und verbesserte Funktionen für die weitere open Kommunikation zwischen apps von Drittanbietern verwenden kann. Öffnen Sie Funktionen wie App-Erweiterungen und Dokumentauswahl zahlreiche Möglichkeiten, wie Anwendungen im Apple Ökosystem verwendet werden können.

### <a name="app-extensions"></a>App-Erweiterungen
App-Erweiterungen, um oversimplify, sind eine Möglichkeit, apps von Drittanbietern, um miteinander zu kommunizieren. Um die hohen Sicherheitsstandards verwalten und die Integrität der Sandbox-apps einzuhalten, geschieht nicht diese Kommunikation direkt zwischen Anwendungen. Stattdessen, es erfolgt durch ein *Erweiterung* in der Mitte.

Der erste Schritt beim Erstellen einer App-Erweiterung ist, um den richtigen Erweiterungspunkt zu definieren – dies ist wichtig, sicherzustellen, das Verhalten und die Verfügbarkeit der richtigen-APIs. Um eine App-Erweiterung in Visual Studio für Mac zu erstellen, fügen Sie sie zu einer vorhandenen Anwendung hinzu, durch das Hinzufügen eines neuen Projekts Ihrer Projektmappe verwenden.

In der **neues Projekt** Dialogfeld navigieren Sie zu **C#**  >  **iOS** > **Unified API**  >  **Erweiterungen**, wie im folgenden Screenshot dargestellt:

![](introduction-to-ios8-images/image2.png "Erstellen einer neuen Erweiterungs")
 
Das Dialogfeld "Neues Projekt" bietet sieben neue Projektvorlagen zum Erstellen von App-Erweiterungen, und werden weiter unten erläutert. Beachten Sie, dass viele der Erweiterungen für andere neue APIs in iOS, z. B. Dokumentauswahl beziehen:

- **Aktion** – Dies ermöglicht Entwicklern das Erstellen von eindeutigen benutzerdefinierten Aktionsschaltflächen ermöglicht Benutzern, bestimmte Aufgaben ausgeführt
- **Benutzerdefinierte Tastatur** – Dadurch können Entwickler auf den Bereich der hinzuzufügenden integrierte Apple Tastaturen durch ihre eigenen benutzerdefinierten hinzufügen. Der beliebten Tastatur verwendet Swype diese auf um die Tastatur auf iOS zu bringen.
- **Dokumentieren Sie die Auswahl** – enthält einen Ansichtscontroller Dokument Auswahl dem Benutzer Zugriff auf Dateien außerhalb der Anwendung Sandbox ermöglicht.
- **Dokumentieren Sie die Auswahl Dateianbieter** – Dies ermöglicht die sichere Speicherung für Dateien, die über die Auswahl des Dokuments.
- **Foto bearbeiten** – diese erweitert, die den angegebenen Filtern und Bearbeiten von bereits in der Fotos-Anwendung, um Benutzern mehr Kontrolle und weitere Optionen ermöglichen, wenn Sie ihre Fotos bearbeiten von Apple bereitgestellten Tools.
- **Heute** – Dies ermöglicht es Anwendungen, Widgets im Abschnitt "heute" der Mitteilungszentrale angezeigt.

Weitere Informationen zur Verwendung von App-Erweiterungen in Xamarin finden Sie in der [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md) Guide.

### <a name="touch-id"></a>Touch-ID

Touch ID wurde in iOS 7 eingeführt, als Mittel zum Authentifizieren des Benutzers – ähnlich einer Kennung. Es war jedoch beschränkt auf die entsperrung des Geräts, mithilfe der App-Store, mithilfe der iTunes und iCloud-Schlüsselbund nur Authentifizierung 

Es gibt jetzt zwei Möglichkeiten, Touch ID als Authentifizierungsmechanismus in iOS 8-Anwendungen, die mit der lokalen Authentifizierungs-API verwenden. Es ist derzeit nicht möglich, die lokale Authentifizierung verwenden, um die Remote authentifiziert.

Erstens unterstützt er die vorhandenen Keychain-Dienste durch die Verwendung von neuen Keychain Zugriffssteuerungslisten (ACLs). Keychain-Daten können mit der erfolgreichen Authentifizierung eines Fingerabdrucks Benutzer entsperrt werden.

Zweitens LocalAuthentication zwei stellt Methoden bereit, um die Anwendung lokal zu authentifizieren. Entwickler sollten verwenden `CanEvaluatePolicy` zu bestimmen, ob das Gerät Touch ID akzeptieren kann, und klicken Sie dann `EvaluatePolicy` , starten Sie den Authentifizierungsvorgang.

Weitere Informationen für die Touch ID und erfahren, wie sie in einer Xamarin.iOS-Anwendung integriert, finden Sie in der [Einführung zum TouchID](~/ios/platform/touchid.md) Anleitungen.

### <a name="document-picker"></a>Dokumentauswahl

Dokument-Auswahl funktioniert mit einem Benutzer iCloud-Laufwerk, damit der Benutzer zum Öffnen von Dateien, die in eine andere app erstellt wurden, importieren und diese bearbeiten und Exportieren zurück. Dies erstellt eines intuitiven Workflows und daher eine viel bessere benutzererfahrung, für Benutzer. iCloud-Synchronisierung geht noch einen Schritt weiter – alle in einer Anwendung vorgenommenen Änderungen auch werden spiegeln konsistent auf allen Geräten.

Die Dokumentauswahl ausführlicher Informationen und erfahren, wie Sie es in einer Xamarin.iOS-Anwendung zu integrieren, finden Sie in der [Einführung in die Dokumentauswahl](~/ios/platform/document-picker.md) Guide.

### <a name="handoff"></a>Handoff

Übergabe, die Teil der größeren Geschäftskontinuität-Funktion ist, hat es sich um einen weiteren Schritt in Richtung Integration von OS X und iOS. Dies umfasst plattformübergreifende AirDrop, die Fähigkeit, iPhone Anrufe, SMS, auf die iPad- und Mac und Verbesserungen bei der Bindung auf Ihrem iPhone zu werden.

Übergabe arbeitet mit iOS 8 und Yosemite und erfordert ein iCloud-Konto für alle anderen Geräte angemeldet sein, dass Sie verwenden möchten. Sie sollten mit vorinstallierten am Apple-apps, einschließlich der Safari, iWork, Karten, Kalender und Kontakte zusammenarbeiten.

Weitere Informationen finden Sie unserem [Handoff](~/ios/platform/handoff.md) Guide.

## <a name="unified-storyboards"></a>Einheitliche Storyboards
iOS 8 umfasst ein neues einfacher Mechanismus zum Erstellen der Benutzeroberfläche zu verwenden – das einheitliche Storyboard. Mit einem einzelnen Storyboard um sämtliche Bildschirmgrößen der unterschiedliche Hardware abzudecken können schnelle und reaktionsfähige Ansichten in einem "true" "einmal entwerfen, viele" Stil erstellt werden.

Vor IOS 8 wurde für Entwickler verwendet `UIInterfaceOrientation` , zwischen Hoch- und Querformat zu unterscheiden und `UIInterfaceIdiom` zur Unterscheidung zwischen iOS-Geräte. In iOS8 ist es nicht mehr erforderlich, separate Storyboards für iPhone und iPad-Geräte zu erstellen, Ausrichtung und Gerät werden mithilfe von bestimmt *Größenklassen*.

Jedes Gerät wird definiert, indem eine Größenklasse, in der vertikalen und der horizontalen Achse, und es gibt zwei Arten von Größenklassen IOS 8:

- **Reguläre** – Dies ist für eine große Bildschirmgröße (z. B. einem iPad) oder eine minianwendung, die eine beträchtliche Größe (z. B. ein UIScrollView-Element den Eindruck erhalten
- **Compact** – Dies ist für kleinere Geräte (z. B. das iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.

Wenn die beiden Konzepte zusammen verwendet werden, ist das Ergebnis ein 2 x 2-Raster, das definiert die verschiedenen möglichen Größen, die in die beiden unterschiedlichen Ausrichtungen verwendet werden können, wie im folgenden Diagramm dargestellt:

![](introduction-to-ios8-images/image3.png "Ein Auszug aus 2 x 2-Raster, das sowohl die unterschiedlichen Ausrichtungen definiert die verschiedenen möglichen Größen, die verwendet werden können")
 
Weitere Informationen zu Größenklassen, finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Foto-Kit
Foto-Kit ist ein neues Framework, das ermöglicht es Anwendungen, die systembilderbibliothek Abfragen und die Erstellung benutzerdefinierter Benutzeroberflächen zum Anzeigen und ändern seinen Inhalt. Sie enthält eine Reihe von Klassen, die Images und Videoassets sowie Sammlungen von Ressourcen wie z. B. Alben und Ordner darstellen.

Weitere Informationen finden Sie unserem [PhotoKit](~/ios/platform/photokit.md) Guide.

## <a name="games"></a>Spiele

<a name="scenekit" />

### <a name="scene-kit"></a>Szene Kit

Szene Kit ist eine 3D-Szene-Graph-API, die die Arbeit mit 3D-Grafiken vereinfacht. Es wurde erstmals in OS X 10.8 und ist jetzt für iOS 8 stammen. Mit Szene Kit ist die immersive 3D Visualisierungen und casual 3D-Spiele erstellen keine Kenntnisse OpenGL erforderlich. Aufbauend auf allgemeine Konzepte der Szene Graph, abstrahiert Szene Kit die Komplexität der OpenGL und OpenGL-ES, können Sie es sehr einfach, Hinzufügen von 3D Inhalt zu einer Anwendung. Wenn Sie ein OpenGL-Experte sind, hat jedoch Szene Kit hervorragende Unterstützung für die Bindung in den direkt mit OpenGL ebenfalls. Außerdem umfasst zahlreiche Features, die 3D-Grafiken, z. B. Physik ergänzen und mehrere andere Apple-Frameworks, z. B. Core Animation, Core-Image und Spritekit sehr gut integriert.

Weitere Informationen finden Sie unserem [SceneKit](~/ios/platform/gaming/scenekit.md) Dokumentation.

<a name="spritekit" />

### <a name="sprite-kit"></a>Spritekit

Spritekit, dem 2D-spieleframework bei Apple an, verfügt über einige interessante neue Features in iOS 8 und OS X Yosemite. Dazu zählen die Integration mit Szene Kit, Unterstützung für Shader, Beleuchtung, Schatten, Einschränkungen, Normalmap-Generierung und physikalische Enhancements. Insbesondere vereinfachen die neuen Funktionen der Physik, realistische Effekte zu einem Spiel hinzufügen.

Weitere Informationen finden Sie unserem [SpriteKit](~/ios/platform/gaming/spritekit.md) Dokumentation.

## <a name="other-changes"></a>Weitere Änderungen
Sowie die wichtigsten Änderungen in iOS 8, die oben beschrieben sind, hat Apple darüber Frameworks für viele vorhandene aktualisiert. Diese werden nachfolgend ausführlich erläutert:

- **[Core-Image](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple hat bei der Image-verarbeitungsframework erweitert werden, indem Sie eine bessere Unterstützung für die Erkennung von rechteckige Bereiche hinzufügen, und QR-codes in Bildern. Mike Bluestein untersucht, dies in seinem Blog post berechtigt [Bilderkennung IOS 8](https://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs
Alle Verbesserungen in iOS 8 vorgenommen wurden wurden eine Reihe von APIs als veraltet. Einige davon werden nachfolgend beschrieben.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – die Methoden und Eigenschaften, die bei der Registrierung von remotebenachrichtigungen wurden als veraltet. Dies sind RegisterForRemoteNotificationTypes und EnabledRemoteNotificationTypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – "traits" und Größenklassen wurden ersetzt, die Methoden und Eigenschaften verwendet, um anfangsausrichtung der Schnittstelle beschreiben. Finden Sie in der [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) für Weitere Informationen dazu, wie diese verwendet.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  – Dadurch wurde ersetzt durch UISearchController in iOS8.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel haben Sie einige der neuen Funktionen von Apple iOS 8 eingeführtes.



## <a name="related-links"></a>Verwandte Links

- [UIKitEnhancements (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [Einführung in die App-Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Einführung in die Dokumentauswahl](~/ios/platform/document-picker.md)
- [Einführung in HealthKit](~/ios/platform/healthkit.md)
- [Einführung in die Steuerelemente der manuellen Kamera](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Einführung in die Touch ID](~/ios/platform/touchid.md)
- [Einführung in die einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
