---
title: Einführung in tvOS 9
description: Dieser Artikel enthält alle neuen und geänderten APIs und in TvOS 9 verfügbaren Features für Entwickler mit Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: c4aea5a35fd5db46c9e7ca245b852c988e82f53e
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830498"
---
# <a name="introduction-to-tvos-9"></a>Einführung in tvOS 9

_Dieser Artikel enthält alle neuen und geänderten APIs und in TvOS 9 verfügbaren Features für Entwickler mit Xamarin.tvOS._

Apple hat die 4. Generation von Apple TV-Hardware, eine neu gestaltete, Touch-Enable Remote featuring, denen das neue TvOS-Betriebssystem (basierend auf iOS 9) veröffentlicht.

Zum ersten Mal öffnet TvOS für Apple TV-Plattform, für den Entwickler, sodass Sie zum Erstellen umfassender, immersiver apps und über die integrierte App-Store von Apple TV in einem Prozess, der den Funktionen schreiben und Freigeben von apps für iOS mithilfe des iTunes App freigeben Store.

Wenn Sie mit der Xamarin.iOS-Entwicklung vertraut sind, sollten Sie den Übergang tvos recht einfach suchen. Die meisten APIs und Features sind identisch, allerdings sind viele allgemeine APIs (z. B. WebKit) nicht verfügbar. Darüber hinaus arbeiten mit der mit dem Remoterepository Siri bringt Entwurf, die nicht in Touchscreen-basierte iOS-Geräte vorhanden sind.

Dieses Handbuch erhalten eine Einführung in alle der neuen und geänderten APIs und in TvOS 9 verfügbaren Features für Entwickler mit Xamarin.tvOS. Weitere Informationen unter TvOS finden Sie unter Apple [entwickeln für die neue Apple TV](https://developer.apple.com/tvos/) Dokumentation.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Unterstützte und nicht unterstützte Funktionen

TvOS-apps für Apple TV haben die folgenden Features und Funktionen unterstützt:

- App-Gruppen
- Hintergrundmodi
- Schutz von Daten
- Game Center
- Gamecontroller
- iCloud
- In-App-Käufe
- Keychain-Freigabe

Die folgenden Features und Funktionen werden nicht unterstützt:

- Apple Pay
- App-Sandbox
- Zugehörige Domänen
- HealthKit
- HomeKit
- Inter-App-Audio
- Karten
- Persönliches VPN
- Pushbenachrichtigungen
- Wallet
- Konfiguration für drahtloses Zubehör

Informieren Sie sich unsere [Assemblys unterstützt](~/ios/tvos/internals/assemblies.md) und [Frameworks unterstützt](~/ios/tvos/internals/frameworks.md) finden Sie Dokumentation.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV-Hardware

Neue Apple TV hat die folgenden Hardwarespezifikationen:

- 64-Bit-A8-Prozessor
- 32GB- oder 64GB Speicher
- 2GB RAM
- 10/100 Mbit/s Ethernet
- WiFi-802.11a/b/g/n/ac
- Auflösung von 1080p
- HDMI
- USB-C-Anschluss (für Entwickler und nur für die Diagnose Verwendung)
- Neue Siri am Remote- oder Apple TV-Remoteserver (basierend auf Region)

### <a name="siri-remote"></a>Siri-Remote

Basierend auf der Region, werden das angegebene Apple TV-Remote in Konfigurationen mit einem gesendet: Siri-Remote- oder Apple TV-Remote.

Siri Remote ist in den folgenden Ländern zurzeit verfügbar:

- Australien
- Kanada
- Frankreich
- Deutschland
- Japan
- Spanien
- Vereinigtes Königreich
- USA

Alle anderen Länder erhalten der Remotecomputer Apple TV, das die Schaltfläche "Siri" mit einer Schaltfläche "Suchen", die Sie dem Standardbildschirm für die Suche mit der Texteingabe ersetzt für die Suche bietet:

[![](tvos9-images/remote02.png "Siri-Remote")](tvos9-images/remote02.png#lightbox)

Weitere Informationen finden Sie unserem [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV-Bereitstellung

Genau wie für iOS entwickeln, ist die neue TvOS das korrekte Bereitstellungsprofil für die Entwicklung und Verteilung auf Basis der Teammitgliedschaft und Signierungsidentitäten, die Sie bereits eingerichtet haben bei Apple erforderlich.

Es ist auch erforderlich, die TvOS-Funktionen wie iCloud KVS oder CloudKit-Datenspeicher zugreifen, ordnungsgemäße Bereitstellung. Informieren Sie sich unsere [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) Informationen zur Unterstützung von iCloud in Ihren apps Xamarin.tvOS.

Bereitstellungsprofile erstellt und installiert die gleiche Weise wie das Arbeiten mit Xamarin.iOS-apps. Daher finden Sie in unserem iOS [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Dokumentation.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV-Apps

Die neue Apple TV-Hardware und TvOS 9 unterstützt zwei Arten von apps: herkömmliche und Client / Server-apps.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Herkömmliche Apps

Herkömmliche apps werden aus dem Apple TV App Store erworben und direkt auf dem Gerät installiert sind. Diese apps möglich, Spiele, Hilfsprogramme oder Media-apps, die mit die gleichen Frameworks und Techniken wie Xamarin.iOS-apps entwickelt werden.

Apple TV-apps haben eine maximale Größe von 200MB und Herunterladen von Inhalten mithilfe von On-Demand-Ressourcen zusätzlich 2GB. Informieren Sie sich unsere [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) für Weitere Informationen.

Finden Sie in unserem [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md) vertraut mit den Tools und Konzepte erforderlich, um mit Xamarin.tvOS TvOS-apps entwickeln.

<a name="Summary" />

### <a name="client-server-apps"></a>Client / Server-Apps

Zusätzlich zu der herkömmlichen installierten apps erleichtert Apple TV zum Erstellen von webbasierten Client / Server-Medien-streaming-apps mithilfe von webtechnologien ("HTTPS", "XML" und "JavaScript"). Sie entwerfen die Benutzeroberfläche von Apple TVML Markupsprache mit und Verwenden von JavaScript zum Definieren eines TVMLKit mithilfe der app-Verhaltens.

Weitere Informationen finden Sie unter Apple [Apple TV-Markupsprachreferenz](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [zu HTTP Live Streaming](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) und [HLS Authoring-Spezifikation für Apple TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) Dokumentation.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Herausforderungen für Benutzer-Schnittstelle

Im Gegensatz zu iOS und OS X Apple TV kein Touchscreen oder eine Maus, die den Benutzer direkt auswählen, und interagieren mit einer app oder den Inhalt zu ermöglichen. Stattdessen diese Benutzer der neuen Remotedebugging-Siri oder ein Spiel Bluetooth-Controller, um der app-Benutzeroberfläche zu navigieren. Weitere Informationen finden Sie unserem [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

Darüber hinaus ist die allgemeine benutzerfreundlichkeit IOS- oder Mac-apps, die einzelnen Benutzeroberflächen werden tendenziell erheblich unterscheiden. Mit Apple TV tendenziell Benutzeroberflächen mehr sozialer Natur, auf dem Sofa, die eine einzelne app und miteinander interagieren können, in denen mehrere Personen sitzen werden. Um eine erfolgreiche Apple TV app-Benutzeroberfläche (entweder eine neue app oder Portieren eine vorhandene) zu entwerfen, müssen diese Änderungen berücksichtigt werden. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Arbeiten mit Fokus und Parallax-Bildern

Wie bereits erwähnt, werden Benutzer Ihrer Xamarin.tvOS-app nicht mit seinem-Schnittstelle direkt als mit iOS interagiert werden, in dem sie Bilder auf den Bildschirm des Geräts, sondern indirekt durch das Zimmer Siri Remote mit Tippen Sie auf. Um darzustellen, und dieser Benutzerinteraktionen verarbeiten, werden von Apple TV ein Fokus basierend-Modell verwendet. 

Als fokusänderungen sind feine Animation und Effekte (z. B. den Parallaxeneffekt auf Images) verwendet, das Element der Benutzeroberfläche eindeutig zu identifizieren, das gerade den Fokus besitzt.

Wenn der Benutzer eine Geste für eine langsamere, runden auf dem Remotecomputer Siri trifft, wird das Element mit Fokus als Reaktion auf diese Bewegung in Echtzeit sway. Wie die Sway auftritt, wird ein beleuchtete Glanz auf das Bild, sodass der Oberfläche angezeigt, die verwendet werden angewendet. Nach einem bestimmten Zeitraum der Inaktivität alle Inhalte für die Out-of-Focus abgeblendet, und das Focused-Element wird immer noch größer.

Weitere Informationen finden Sie unserem [arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Die Startseite

Das Apple TV-Startbildschirm zeigt alle apps, die installiert werden und bietet eine Möglichkeit, die Benutzereinstellungen zugreifen:

[![](tvos9-images/home01.png "Die Startseite")](tvos9-images/home01.png#lightbox)

Der Benutzer navigiert ein Raster mit app-Symbole, die mithilfe von Touchgesten auf dem Siri-Remotecomputer mit Fokus, wählen Sie eine app, und starten Sie sie. Das App-Symbol ist das Ihre erste Ihre chance, einen hervorragenden Eindruck für Ihre potenziellen Benutzer ist, und sprechen Zweck von Ihrer app auf einen Blick.

Jede app muss sowohl eine kleine als auch eine große Version der App-Symbol angeben. Das kleine Symbol wird auf dem Bildschirm Apple TV-Startseite verwendet werden, wenn die app installiert ist. Die große Version wird von der App Store verwendet. Die großen App-Symbol sollte es sich um das Aussehen und Verhalten der Version Minisymbols imitieren.

Weitere Informationen finden Sie unserem [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Oberes Regal

Wenn der Benutzer Ihre Xamarin.tvOS-app auf der obersten Zeile, auf dem Bildschirm Apple TV-Startseite aufgegeben hat, wird eine große Top Shelf Image angezeigt, wenn es sich bei Ihrer app vom Benutzer ausgewählt wird. Dieses Image sollte markieren Sie die Funktionen Ihrer App oder enthalten direkte Links zum jeweiligen Inhalt.

[![](tvos9-images/topshelf01.png "Oberes Regal")](tvos9-images/topshelf01.png#lightbox)

Die Top Shelf Image kann entweder als eine einzelne statische bereitgestellt `.png` oder `.lsr` Dateien, oder es kann dynamisch erstellt zur Laufzeit als eine einzelne Zeile den Fokus erhalten kann Elemente.

Anstatt eine statische Top Shelf Image zu verwenden, kann sie eine dynamische Zeile oder den Fokus erhalten kann Elemente oder einen dynamischen Satz von Durchführen eines Bildlaufs Banner enthalten. Beide dieser dynamische Format können Sie markieren Sie den Inhalt von Ihrer app oder ein Sprung in die am häufigsten verwendeten Funktionen bereitgestellt.

Weitere Informationen finden Sie unsere [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation und Apple [TVServices Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) für Weitere Informationen zum Hinzufügen einer Top Shelf-Erweiterung zu Ihrer app bereitstellen Dynamische Top Shelf-Inhalt.






## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Erstellen von apps für TvOS mit Xamarin (video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
