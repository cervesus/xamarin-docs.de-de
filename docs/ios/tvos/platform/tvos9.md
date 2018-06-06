---
title: Einführung in die tvos. außerdem wurden 9
description: In diesem Artikel werden alle neuen und geänderten-APIs und in tvos. außerdem wurden 9 verfügbaren Funktionen für Entwickler Xamarin.tvOS eingeführt.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: c3c278666c5d57d00b4038ae6d3f2d7925e88537
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789023"
---
# <a name="introduction-to-tvos-9"></a>Einführung in die tvos. außerdem wurden 9

_In diesem Artikel werden alle neuen und geänderten-APIs und in tvos. außerdem wurden 9 verfügbaren Funktionen für Entwickler Xamarin.tvOS eingeführt._

Apple hat die 4. Generierung von Apple TV-Hardware, wenn Sie eine überarbeitete, Touch-Enable Remote herausstellen, denen das neue Betriebssystem für tvos. außerdem wurden (basierend auf iOS 9) veröffentlicht.

Zum ersten Mal öffnet tvos. außerdem wurden die Apple TV-Plattform für den Entwickler, und Sie können umfangreiche, faszinierend apps erstellen und über den integrierten Fernseher Apple App Store in einem Prozess, der den Funktionen schreiben und Freigeben von apps für iOS mit iTunes App freigeben Speicher.

Wenn Sie bei der Entwicklung von Xamarin.iOS vertraut sind, sollten Sie den Übergang zu tvos. außerdem wurden relativ einfach suchen. Die meisten APIs und -Funktionen sind identisch, allerdings viele gängige APIs (z. B. WebKit) nicht verfügbar sind. Darüber hinaus arbeiten mit den mit der Remoteinstanz Siri bringt Entwurf, die nicht in Touchscreen basierend iOS-Geräte vorhanden sind.

Dieses Handbuch erhalten eine Einführung in alle neuen und geänderten-APIs und in tvos. außerdem wurden 9 verfügbaren Funktionen für Entwickler Xamarin.tvOS. Weitere Informationen zu tvos. außerdem wurden, finden Sie im Apple [entwickeln für die neue Apple TV](https://developer.apple.com/tvos/) Dokumentation.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Unterstützte und nicht unterstützte Funktionen

tvos. außerdem wurden-apps, die auf den Apple TV ausgeführt haben und Funktionen Folgendes unterstützt:

 - App-Gruppen
 - Hintergrundmodi
 - Schutz von Daten
 - Game Center
 - Gamecontroller
 - iCloud
 - In App-Käufe
 - Keychain-Freigabe

Die folgenden Features und Funktionen werden nicht unterstützt:

 - Apple Pay
 - App-Sandkasten
 - Zugehörige Domänen
 - HealthKit
 - HomeKit
 - Inter-App-Audio
 - Karten
 - Persönliches VPN
 - Pushbenachrichtigungen
 - Wallet
 - Konfiguration für drahtloses Zubehör

Finden Sie in unserer [unterstützt Assemblys](~/ios/tvos/internals/assemblies.md) und [unterstützt Frameworks](~/ios/tvos/internals/frameworks.md) Dokumentation weitere Informationen.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV-Hardware

Die neue Apple TV gelten die folgenden Hardwarespezifikationen:

 - 64-Bit-A8-Prozessor
 - 32GB und 64GB Speicher
 - 2GB RAM
 - 10/100 Mbit/s Ethernet
 - WiFi-802.11a/b/g/n/ac
 - Auflösung von 1080p
 - HDMI
 - USB-C-Anschluss (für Entwickler und nur der Diagnose verwenden)
 - Neue Siri Remote oder Apple TV Remote (basierend auf Region)

### <a name="siri-remote"></a>Siri Remote

Anhand des Bereichs, der angegebenen Apple TV-Remote geschaltet in Konfigurationen mit einem: Siri Remote- oder Apple TV-Remote.

Siri Remote ist in den folgenden Ländern verfügbar:

 - Australien
 - Kanada
 - Frankreich
 - Deutschland
 - Japan
 - Spanien
 - Vereinigtes Königreich
 - USA

Alle anderen Ländern werden der Apple TV-Remote angezeigt, die die Schaltfläche "Siri" mit einer Schaltfläche "Suchen", die von der Standardbildschirm für die Suche mit Texteingabe ersetzt für die Suche wird:

[![](tvos9-images/remote02.png "Siri Remote")](tvos9-images/remote02.png#lightbox)

Weitere Informationen finden Sie unter unsere [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV-Bereitstellung

Genau wie für iOS entwickeln, ist die neue tvos. außerdem wurden die richtigen Bereitstellungsprofil für Entwicklung und Verteilung basierend auf dem die Team-Mitgliedschaft und Signieren von Identitäten, die Sie bereits eingerichtet haben bei Apple erforderlich.

Es ist auch erforderlich, tvos. außerdem wurden Funktionen wie iCloud KVS oder CloudKit Datenspeicher zugreifen, ordnungsgemäße Bereitstellung. Finden Sie in unserer [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) Informationen zur Unterstützung von iCloud in Ihren apps Xamarin.tvOS.

Provisioning Profiles werden erstellt und installiert die gleiche Weise wie das Arbeiten mit Xamarin.iOS-apps. Daher finden Sie in unserer iOS [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Weitere Einzelheiten s. Dokumentation.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV-Apps

Die neue Apple TV-Hardware und tvos. außerdem wurden 9 unterstützt zwei Arten von apps: herkömmliche und Client / Server-apps.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Herkömmliche-Apps

Herkömmliche apps werden aus dem Apple TV-App-Store erworben und direkt auf dem Gerät installiert sind. Spiele, Hilfsprogramme oder Media-apps, die mit dem gleichen Frameworks und Techniken Xamarin.iOS apps entwickelt wurden, können diese apps werden.

Apple TV-apps haben eine maximale Größe von 200MB und eine zusätzliche 2GB von Inhalten mithilfe von On-Demand-Ressourcen herunterladen können. Finden Sie in unserem [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) für Weitere Informationen.

Finden Sie in unserer [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md) mit den Tools und Konzepten erforderlich, um mit Xamarin.tvOS tvos. außerdem wurden-apps entwickeln vertraut.

<a name="Summary" />

### <a name="client-server-apps"></a>Client / Server-Apps

Zusätzlich zu den installierten apps herkömmlichen erleichtert Apple TV Clientserver webbasierte Medien-streaming-apps mithilfe von webtechnologien ("HTTPS", "XML" und "JavaScript") zu erstellen. Sie entwerfen die Benutzeroberfläche mithilfe von Apple TVML Markupsprache und JavaScript verwenden, um die app-Verhalten, die mithilfe von TVMLKit zu definieren.

Weitere Informationen finden Sie in der Apple- [Apple TV Markup Language Reference](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [zu HTTP-Livestreaming](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) und [Authoring HLS-Spezifikation für App TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) Dokumentation.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Benutzer-Schnittstelle Herausforderungen

Im Gegensatz zu iOS und OS X die Apple TV kein Touchscreen oder eine Maus, mit denen den Benutzer direkt auswählen und eine app oder ihr Inhalt interagieren können. Stattdessen diese Benutzer neue Siri Remoteinstanz oder einen Bluetooth-Game-Controller, um einer app-Benutzeroberfläche zu navigieren. Weitere Informationen finden Sie unter unsere [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

Darüber hinaus ist die allgemeine benutzerfreundlichkeit IOS- oder Mac-apps, die einzelnen Benutzeroberflächen werden tendenziell erheblich unterscheidet. Mit dem Apple TV tendenziell Benutzeroberflächen mehr sozialen Natur, in dem mehrere Personen können auf die Interaktion mit einer app und anderen Couch genutzt werden. Um eine erfolgreiche Apple TV-app-Erfahrung (eine neue app oder eine vorhandene Portieren) zu entwerfen, müssen diese Änderungen berücksichtigt werden. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Arbeiten mit einer Fokussierung und Parallax Bilder

Wie bereits erwähnt, werden Benutzern der Anwendung Xamarin.tvOS nicht Interaktion mit der Schnittstelle direkt als mit iOS verwendet werden, in dem sie Bilder auf dem Gerätebildschirm jedoch indirekt über den Raum, der mithilfe von Siri Remote Tippen Sie auf. Um darzustellen, und Behandeln dieser Benutzerinteraktionen, verwendet die Apple TV ein Fokus Basis-Modell. 

Als fokusänderungen sind feine Animationen und Auswirkungen (z. B. die Parallax Auswirkung auf Bilder) verwendet, die Benutzeroberfläche Element eindeutig zu identifizieren, das gerade den Fokus besitzt.

Wenn der Benutzer eine Geste langsame, kreisförmige auf der Remoteinstanz Siri herstellt, wird das Element mit Fokus in Echtzeit in Reaktion auf diese Bewegung sway. Wie die Schlingern auftritt, wird ein beleuchteten Sheen auf seines Abbilds machen die Oberfläche verwendet anscheinend angewendet. Nach einem bestimmten Zeitraum der Inaktivität Blendet alle Inhalte, die Out-of-Focus, und das Element fokussiert wächst auch größere.

Weitere Informationen finden Sie unter unsere [arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Der Startbildschirm

Die Apple TV-Startbildschirm zeigt alle apps, die installiert werden und bietet eine Möglichkeit, die Benutzereinstellungen zugreifen:

[![](tvos9-images/home01.png "Der Startbildschirm")](tvos9-images/home01.png#lightbox)

Der Benutzer navigiert ein Raster mit app-Symbole Fingereingabe auf Siri Remote mit Fokus, wählen Sie eine app, und starten Sie ihn verwenden. Das Symbol "App" wird die Ihres ersten Wahrscheinlichkeit der Ausführung eine hervorragende Eindruck für Ihre potenziellen Benutzer und sollte Ihre app Zweck auf einen Blick vermitteln.

Alle Apps muss eine kleine und eine große Version von dessen Symbol "App" angeben. Des kleinen Symbols wird auf dem Bildschirm Apple TV-Startseite verwendet werden, wenn die app installiert wird. Die große Version wird von dem App Store. "Große Symbole" App sollte imitieren, die das Aussehen und Verhalten von der Version des kleinen Symbols an.

Weitere Informationen finden Sie unter unsere [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Oberes Regal

Wenn der Benutzer die app Xamarin.tvOS auf die Zeile nach oben auf dem Bildschirm Apple TV-Startseite aufgegeben hat, wird ein großes oben Regal Bild angezeigt, wenn es sich bei Ihrer app vom Benutzer ausgewählt wird. Dieses Bild sollte markieren Sie die Funktionen der app, oder geben Sie direkte Links zum jeweiligen Inhalt.

[![](tvos9-images/topshelf01.png "Oberes Regal")](tvos9-images/topshelf01.png#lightbox)

Die oberen Regal Image kann entweder als eine einzelne statische bereitgestellt `.png` oder `.lsr` Dateien, oder es kann dynamisch erstellt werden zur Laufzeit als eine einzelne Zeile mit den Fokus erhalten kann Elemente.

Anstatt ein statisches Bild der Top-Regal können sie eine dynamische Zeile oder den Fokus erhalten kann Elemente oder einen dynamischen Satz von Durchführen eines Bildlaufs Banner enthalten. Sowohl diese dynamische Format können Sie markieren Sie den Inhalt von Ihrer app oder ein Sprung in die am häufigsten verwendeten Funktionen bereitgestellt.

Weitere Informationen finden Sie unter unsere [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation und Apple [TVServices Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) Weitere Informationen zum Hinzufügen von sinnvolles Regal nach oben zu Ihrer app bereitstellen Dynamische oben Regal-Inhalt.






## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Erstellen von apps für tvos. außerdem wurden mit Xamarin (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
