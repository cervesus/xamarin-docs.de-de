---
title: Einführung in iOS 8
description: Mit IOS 8 hat Apple eine Vielzahl von neuen Frameworks und APIs bereitgestellt, mit denen sich Entwickler begeistern und begeistern können. In diesem Handbuch werden diese neuen APIs vorgestellt und erläutert, wie IOS 8 von Entwicklern und Benutzern profitieren kann.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 9353cffd924688058c1495b9258cc7f0e0ce7b82
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78910824"
---
# <a name="introduction-to-ios-8"></a>Einführung in iOS 8

_Mit IOS 8 hat Apple eine Vielzahl von neuen Frameworks und APIs bereitgestellt, mit denen sich Entwickler begeistern und begeistern können. In diesem Handbuch werden diese neuen APIs vorgestellt und erläutert, wie IOS 8 von Entwicklern und Benutzern profitieren kann._

IOS 7 hat die gesamte IOS-Benutzeroberfläche von dem ersten iPhone-Betriebssystem, das von Benutzern und Entwicklern erwartet wurde, visuell geändert. IOS 8 fährt damit fort, indem viele Frameworks für Entwickler bereitgestellt werden, die es Benutzern ermöglichen, fast alle Aspekte ihrer Lebensdauer direkt von Ihrem iPhone aus zu steuern. Beispielsweise können Integrität und Fitness mit *healthkit*analysiert werden, Kennungen sind mit der biometrischen Authentifizierung über *localauthentication*veraltet, *App-Erweiterungen* eröffnen einen Kommunikationskanal zwischen Drittanbieter-apps, und *homekit* ermöglicht die Möglichkeit, Ihr Haus zu einem Zuhause in der Zukunft zu machen. 

Wenn IOS 7 mit einem erfreuen von Benutzern zu tun hat, konzentriert sich IOS 8 auf die Auswahl von Entwicklern, die über eine ganze Reihe von leckeren neuen Tools verfügen. 

In diesem Handbuch werden die neuen APIs für xamarin. IOS-Entwickler vorgestellt.  

Es gibt auch einige APIs, die in ios 8 als veraltet eingestuft wurden, die am Ende dieses Dokuments ausführlich erläutert werden.

## <a name="requirements"></a>Requirements (Anforderungen)

Zum Erstellen von IOS 8-Apps in Visual Studio für Mac sind die folgenden Schritte erforderlich:

- **Xcode 7 und IOS 8 oder** höher – die neuesten Xcode-und IOS-APIs von Apple müssen auf dem Computer des Entwicklers installiert und konfiguriert werden.
- **Visual Studio für Mac** – die neueste Version von Visual Studio für Mac muss auf dem Benutzergerät installiert und konfiguriert werden.
- **IOS 8-Gerät oder-Simulator** – ein IOS-Gerät, auf dem die neueste Version von IOS 8 zum Testen ausgeführt wird.

## <a name="home-and-leisure"></a>Zuhause und Freizeit

IOS 8 unterstützt Sie bei der festen Einrichtung von Apple und dem IOS-Gerät durch die Verwendung von homekit und healthkit direkt in das Herzstück. In diesem Abschnitt wird erläutert, wie diese neuen Frameworks funktionieren und wie Sie in Ihre xamarin. IOS-Anwendung integriert werden können.

## <a name="homekit"></a>HomeKit

Das Steuern ihrer Geräte über Ihr iPhone ist keine neue Technologie Anwendung. viele Connected-Home-Produkte können über eine IOS-App gesteuert werden. Homekit führt diesen Schritt jedoch weiter aus, indem ein gängiges Protokoll für Home Automation-Geräte herauf gestuft wird und eine öffentliche API für bestimmte Hersteller verfügbar ist, z. b. iHome, Philips und Honeywell. Für den Benutzer bedeutet dies, dass Sie fast jeden Aspekt Ihrer Startseite nahtlos von einer Anwendung aus steuern können. Es ist unerheblich, dass Sie eine Philips Hue Lightbulb oder einen Schachtelungs Alarm verwenden. Benutzer können auch zahlreiche Smart Home-Prozesse in "Szenen" verketten.

Mit homekit können Drittanbieter-apps und Siri Zubehör erkennen und ihrer eigenen Konfigurations Datenbank für die privat Datenbank hinzufügen, diese Daten bearbeiten und bearbeiten und mit Zubehör und Ihren Diensten kommunizieren, um eine Aktion auszuführen.

### <a name="configuration"></a>Konfiguration

Das folgende Diagramm zeigt die grundlegende Hierarchie der Konfiguration von homekit-Zubehör:

![](introduction-to-ios8-images/image1.png "This diagram shows the basic hierarchy of the configuration of HomeKit accessories")

Um mit homekit zu beginnen, müssen Entwickler sicherstellen, dass für das Bereitstellungs Profil der homekit-Dienst ausgewählt ist. Apple hat Entwicklern auch ein homekit-Simulator-Add-in für Xcode bereitgestellt. Diese finden Sie im [Apple Developer Center](https://developer.apple.com/downloads/index.action)unter `Hardware IO Tools for Xcode`. 

Weitere Informationen finden Sie in unserem [homekit](~/ios/platform/homekit.md) -Handbuch.

## <a name="healthkit"></a>HealthKit

Healthkit ist ein Framework, das in ios 8 eingeführt wurde und einen zentralisierten, koordinierten und sicheren Datenspeicher für Integritäts bezogene Informationen bietet. Das Betriebssystem stellt den Datenschutz und die Sicherheit von Integritäts Informationen und, mit der Health-APP, als Dashboard für den Benutzer sicher. Mit der Benutzer Berechtigung können Anwendungen eine Vielzahl von Integritäts Informationen lesen und schreiben.

Weitere Informationen zur Verwendung dieser in ihrer xamarin. IOS-App finden Sie im Handbuch [Introduction to healthkit (Einführung in healthkit](~/ios/platform/healthkit.md) ).

## <a name="extending-iphone-functionality"></a>Erweitern der iPhone-Funktionalität
Mit iOS8 erhalten Entwickler wesentlich mehr Kontrolle darüber, wer Ihre APP verwenden kann, und die Funktion für eine besser geöffnete Kommunikation zwischen Drittanbieter-apps zu erhöhen. Features wie App-Erweiterungen und Dokument Auswahl eröffnen eine Vielzahl von Möglichkeiten, wie Anwendungen im Ökosystem von Apple eingesetzt werden können.

### <a name="app-extensions"></a>App-Erweiterungen
App-Erweiterungen, die zu viel mplify sind, sind eine Möglichkeit für Drittanbieter-apps, miteinander zu kommunizieren. Diese Kommunikation erfolgt nicht direkt zwischen Anwendungen, um hohe Sicherheitsstandards zu gewährleisten und die Integrität der Sandbox-apps aufrechtzuerhalten. Stattdessen wird Sie durch eine *Erweiterung* in der Mitte durchgeführt.

Der erste Schritt beim Erstellen einer APP-Erweiterung besteht darin, den richtigen Erweiterungs Punkt zu definieren – Dies ist wichtig, um das Verhalten und die Verfügbarkeit der richtigen APIs sicherzustellen. Um eine APP-Erweiterung in Visual Studio für Mac zu erstellen, fügen Sie Sie einer vorhandenen Anwendung hinzu, indem Sie der Projekt Mappe ein neues Projekt hinzufügen.

Navigieren Sie im Dialogfeld **Neues Projekt** zu **C#**  > **IOS** - > **Unified API** > **Erweiterungen**, wie im folgenden Screenshot veranschaulicht:

![](introduction-to-ios8-images/image2.png "Creating a new extension")

Das Dialogfeld "Neues Projekt" enthält sieben neue Projektvorlagen zum Erstellen von App-Erweiterungen, die im folgenden erläutert werden. Beachten Sie, dass viele der Erweiterungen sich auf andere neue APIs in ios beziehen, wie z. b. die Dokument Auswahl:

- **Aktion** – Hiermit können Entwickler eindeutige benutzerdefinierte Aktions Schaltflächen erstellen, die es Benutzern ermöglichen, bestimmte Aufgaben auszuführen.
- **Benutzerdefinierte Tastatur** – dadurch können Entwickler den Bereich der erstellten Apple-Tastaturen durch Hinzufügen Ihrer eigenen benutzerdefinierten Tastatur hinzufügen. Die beliebte Tastatur, der Swap-Austausch verwendet diese, um Ihre Tastatur auf IOS zu bringen.
- **Dokument** Auswahl – enthält einen Ansichts Controller für die Dokument Auswahl, mit dem Benutzer auf Dateien außerhalb des Sandkastens der Anwendung zugreifen können.
- **Datei Anbieter** für die Dokument Auswahl – hiermit wird ein sicherer Speicher für Dateien mithilfe der Dokument Auswahl bereitstellt.
- **Fotobearbeitung** – Dies erweitert die Filter und Bearbeitungs Tools, die bereits von Apple in der Fotos-Anwendung bereitgestellt wurden, um Benutzern mehr Kontrolle und mehr Optionen beim Bearbeiten Ihrer Fotos zu bieten.
- **Heute** – Hiermit können Anwendungen im Abschnitt "heute" des Benachrichtigungs Centers Widgets anzeigen.

Weitere Informationen zur Verwendung von App-Erweiterungen in xamarin finden Sie im Handbuch [Introduction to App Extensions](~/ios/platform/extensions.md) .

### <a name="touch-id"></a>Touch ID

Die Fingereingabe-ID wurde in ios 7 als Mittel zum Authentifizieren des Benutzers eingeführt – ähnlich wie bei einer Kennung. Dies war jedoch auf das Entsperren des Geräts, die Verwendung des App Store, die Verwendung von iTunes und das Authentifizieren der icloud-Keychain beschränkt. 

Es gibt zwei Möglichkeiten, die Berührungs-ID als Authentifizierungsmechanismus in ios 8-Anwendungen mithilfe der lokalen Authentifizierungs-API zu verwenden. Es ist derzeit nicht möglich, die lokale Authentifizierung für die Remote Authentifizierung zu verwenden.

Erstens werden die vorhandenen Keychain-Dienste durch die Verwendung neuer Keychain-Access Control Listen (ACLs) unterstützt. Keychain-Daten können mit der erfolgreichen Authentifizierung eines Benutzer Fingerabdrucks entsperrt werden.

Zweitens bietet localauthentication zwei Methoden, um Ihre Anwendung lokal zu authentifizieren. Entwickler sollten mithilfe von `CanEvaluatePolicy` ermitteln, ob das Gerät die Berührungs-ID akzeptieren kann, und dann `EvaluatePolicy`, um den Authentifizierungs Vorgang zu starten.

Weitere Informationen über die Fingereingabe-ID und Informationen zur Integration in eine xamarin. IOS-Anwendung finden Sie unter Fingereingabe [-ID und Face ID in xamarin. IOS](~/ios/platform/touch-id-face-id.md) Guides.

### <a name="document-picker"></a>Dokumentauswahl

Die Dokument Auswahl funktioniert mit einem Benutzer-icloud-Laufwerk, um dem Benutzer zu ermöglichen, Dateien zu öffnen, die in einer anderen APP erstellt wurden, Sie zu importieren und zu manipulieren und dann wieder zurück zu exportieren. Dadurch entsteht ein intuitiver Workflow und somit eine viel bessere Benutzer Leistung. die icloud-Synchronisierung führt dies einen Schritt weiter aus – alle Änderungen, die in einer Anwendung vorgenommen werden, werden auch konsistent auf allen ihren Geräten widerspiegeln.

Weitere Informationen zur Dokument Auswahl und zur Integration in eine xamarin. IOS-Anwendung finden Sie in der [Einführung in das Dokument](~/ios/platform/document-picker.md) Auswahl Handbuch.

### <a name="handoff"></a>Handoff

Der Handoff, der Teil des größeren Kontinuitäts Features ist, führt zur Integration von OS X und IOS einen Schritt weiter. Dies umfasst plattformübergreifende airdrop, die Möglichkeit zum Übernehmen von iPhone-anrufen, SMS auf dem iPad und Mac sowie Verbesserungen beim Tethering von Ihrem iPhone.

Die Übergabe funktioniert mit IOS 8 und Yosemite und erfordert, dass ein icloud-Konto bei allen verschiedenen Geräten angemeldet ist, die Sie verwenden möchten. Es sollte mit den meisten vorinstallierten Apple-Apps wie Safari, iWork, Maps, Kalenders und Kontakten funktionieren.

Weitere Informationen finden Sie in unserem [Handzettel](~/ios/platform/handoff.md) Handbuch.

## <a name="unified-storyboards"></a>Einheitliche Storyboards
IOS 8 umfasst einen neuen einfacheren Verwendungs Mechanismus zum Erstellen der Benutzeroberfläche – das vereinheitlichte Storyboard. Mit einem einzelnen Storyboard, das alle unterschiedlichen Hardware Bildschirmgrößen abdeckt, können schnelle und reaktionsschnelle Ansichten in einem echten "Design einmal erstellt werden.

Vor iOS8 nutzten Entwickler `UIInterfaceOrientation`, um zwischen hoch-und Querformat zu unterscheiden, und `UIInterfaceIdiom`, um zwischen IOS-Geräten zu unterscheiden. In iOS8 ist es nicht mehr erforderlich, separate Storyboards für iPhone-und iPad-Geräte zu erstellen – Ausrichtung und Gerät werden durch die Verwendung von *Größenklassen*bestimmt.

Jedes Gerät wird in der vertikalen und der horizontalen Achse durch eine Size-Klasse definiert, und es gibt zwei Typen von Größenklassen in ios 8:

- **Regulär** : Dies ist entweder für eine große Bildschirmgröße (z. b. ein iPad) oder eine Mini Anwendung, die den Eindruck einer großen Größe (z. b. eine uiscrollview) liefert.
- **Compact** : Dies gilt für kleinere Geräte (z. b. das iPhone). Diese Größe berücksichtigt die Ausrichtung des Geräts.

Wenn die beiden Konzepte gleichzeitig verwendet werden, ist das Ergebnis ein 2 x 2-Raster, in dem die verschiedenen möglichen Größen definiert werden, die in den unterschiedlichen Ausrichtungen verwendet werden können, wie in der folgenden Abbildung dargestellt:

![](introduction-to-ios8-images/image3.png "A diagram representing the 2 x 2 grid that defines the different possible sizes that can be used in both the differing orientations")

Weitere Informationen zu Größenklassen finden Sie unter [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md).

## <a name="photo-kit"></a>Fotokit
Photo Kit ist ein neues Framework, das es Anwendungen ermöglicht, die System Image Bibliothek abzufragen und benutzerdefinierte Benutzeroberflächen zu erstellen, um deren Inhalte anzuzeigen und zu ändern. Sie enthält eine Reihe von Klassen, die Bild-und Video Medienobjekte sowie Sammlungen von Assets wie z. b. Alben und Ordner darstellen.

Weitere Informationen finden Sie in unserem Leitfaden für [photokit](~/ios/platform/photokit.md) .

## <a name="games"></a>Spiele

<a name="scenekit" />

### <a name="scene-kit"></a>Szene-Kit

Das Scene Kit ist eine 3D-Szene-Graph-API, die die Arbeit mit 3D-Grafiken vereinfacht. Sie wurde erstmals in OS X 10,8 eingeführt und ist jetzt auf IOS 8. Mit dem Szene-Kit, das immersive 3D-Visualisierungen erstellt, und ungezwungene 3D-Spiele erfordert kein Know-how in OpenGL. Das Scene Kit baut auf gängigen Szenen Diagramm Konzepten auf und abstrahiert die Komplexität von OpenGL und OpenGL, sodass es sehr einfach ist, 3D-Inhalte zu einer Anwendung hinzuzufügen. Wenn Sie jedoch ein OpenGL-Experte sind, bietet das Szene-Kit auch hervorragend Unterstützung für die direkte Bindung mit OpenGL. Außerdem sind zahlreiche Features enthalten, die 3D-Grafiken (z. b. Physik) ergänzen und sehr gut in verschiedene andere Apple-Frameworks integriert sind, z. b. Core Animation, Core Image und Sprite Kit.

Weitere Informationen finden Sie in unserer [scenekit](~/ios/platform/gaming/scenekit.md) -Dokumentation.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite-Kit

Sprite Kit, das 2D-Spiel Framework von Apple, verfügt über einige interessante neue Features in ios 8 und OS X Yosemite. Hierzu gehören die Integration in das Szene-Kit, shaderunterstützung, Beleuchtung, Schatten, Einschränkungen, normale Zuordnungs Generierung und physikalische Erweiterungen. Insbesondere die neuen Physik Features erleichtern das Hinzufügen realistischer Effekte zu einem Spiel.

Weitere Informationen finden Sie in unserer [spritekit](~/ios/platform/gaming/spritekit.md) -Dokumentation.

## <a name="other-changes"></a>Weitere Änderungen
Zusätzlich zu den wichtigsten Änderungen in ios 8, die oben beschrieben wurden, hat Apple zusätzlich viele vorhandene Frameworks aktualisiert. Diese werden im folgenden beschrieben:

- **[Core-Image](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)** – Apple hat sich auf das Abbild Verarbeitungs Framework ausgedehnt, indem es eine bessere Unterstützung für die Erkennung von rechteckigen Bereichen und QR-Codes in Bildern bietet. Mike Bluestein untersucht dies in seinem Blogbeitrag " [Image Erkennung in ios 8](https://blog.xamarin.com/image-detection-in-ios-8/) ".

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs
Mit allen in ios 8 vorgenommenen Verbesserungen sind einige APIs veraltet. Einige davon sind unten aufgeführt.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)** – die Methoden und Eigenschaften, die zum Registrieren von Remote Benachrichtigungen verwendet werden, sind veraltet. Dies sind registerforremotenotificationtypes und enabledremutenotificationtypes.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)** – Merkmale und Größenklassen haben die Methoden und Eigenschaften ersetzt, die zur Beschreibung der Schnittstellen Ausrichtung verwendet werden. Weitere Informationen zur Verwendung dieser Funktionen finden Sie unter [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)** – dies wurde durch uisearchcontroller in iOS8 ersetzt.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel wurden einige der neuen Features behandelt, die von Apple in ios 8 eingeführt wurden.

## <a name="related-links"></a>Verwandte Links

- [Uikitenhancements (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-uikitenhancements)
- [Einführung in App-Erweiterungen](~/ios/platform/extensions.md)
- [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
- [Einführung in die Dokument Auswahl](~/ios/platform/document-picker.md)
- [Einführung in healthkit](~/ios/platform/healthkit.md)
- [Einführung in manuelle Kamera Steuerelemente](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Fingereingabe-ID und Face ID mit xamarin. IOS](~/ios/platform/touch-id-face-id.md)
- [Einführung in vereinheitlichte Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
