---
title: Einführung in tvOS 9
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in tvos 9 für xamarin. tvos-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
ms.openlocfilehash: 34f332eb712f479f9f9565a3894212e3cdd5aaf6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030537"
---
# <a name="introduction-to-tvos-9"></a>Einführung in tvOS 9

_In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in tvos 9 für xamarin. tvos-Entwickler zur Verfügung stehen._

Apple hat die 4. Generation der Apple TV-Hardware mit einer neu gestalteten, berührungsfähigen Remote Version veröffentlicht, auf der das neue tvos-Betriebssystem (basierend auf IOS 9) ausgeführt wird.

Zum ersten Mal öffnet tvos die Apple TV-Plattform für den Entwickler, sodass Sie umfassende und immersive Apps erstellen und Sie über den integrierten App Store von Apple TV veröffentlichen können. Dies ähnelt der Vorgehensweise beim Schreiben und Veröffentlichen von Apps für IOS mithilfe der iTunes-App. Speicher.

Wenn Sie mit der xamarin. IOS-Entwicklung vertraut sind, sollten Sie den Übergang zu tvos recht einfach finden. Die meisten APIs und Features sind identisch, aber viele gängige APIs sind nicht verfügbar (z. b. WebKit). Außerdem sind bei der Arbeit mit der Siri-Remote einige Entwurfs Herausforderungen zu meistern, die auf Touchscreen-basierten IOS-Geräten nicht vorhanden sind.

Dieses Handbuch bietet eine Einführung in alle neuen und geänderten APIs und Features, die in tvos 9 für xamarin. tvos-Entwickler zur Verfügung stehen. Weitere Informationen zu tvos finden Sie unter Apple- [Entwicklung für die neue Apple TV](https://developer.apple.com/tvos/) -Dokumentation.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>Unterstützte und nicht unterstützte Funktionen

tvos-apps, die auf dem Apple TV ausgeführt werden, verfügen über die folgenden unterstützten Funktionen und Features:

- App-Gruppen
- Hintergrundmodi
- Schutz von Daten
- Game Center
- Spiele Controller
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

Weitere Informationen finden Sie in unseren [unterstützten](~/ios/tvos/internals/assemblies.md) Assemblys und [unterstützten Frameworks](~/ios/tvos/internals/frameworks.md) .

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV-Hardware

Das neue Apple TV verfügt über die folgenden Hardware Spezifikationen:

- 64-Bit-A8-Prozessor
- 32 GB oder 64 GB Speicher
- 2 GB RAM
- Ethernet mit 10/100 Mbit/s
- WiFi 802.11 a/b/g/n/AC
- 1080p-Auflösung
- HDMI
- USB-C-Port (nur für Entwickler und Diagnose)
- New Siri Remote oder Apple TV Remote (basierend auf der Region)

### <a name="siri-remote"></a>Siri (Remote)

Basierend auf der Region wird der angegebene Apple TV-Remote Computer in einer der folgenden Konfigurationen angezeigt: SIRI Remote oder Apple TV Remote.

Der Siri-Remote Computer ist zurzeit in den folgenden Ländern verfügbar:

- Australien
- Kanada
- Frankreich
- Deutschland
- Japan
- Spanien
- Vereinigtes Königreich
- USA

Alle anderen Länder erhalten den Apple TV-Remote Computer, der die Siri-Schaltfläche durch eine Such Schaltfläche ersetzt, die den Standard Suchbildschirm mit Texteingaben für die Suche öffnet:

[![](tvos9-images/remote02.png "Siri Remote")](tvos9-images/remote02.png#lightbox)

Weitere Informationen finden Sie in unserer Dokumentation zu den [Remote-und Bluetooth-Controllern von Siri](~/ios/tvos/platform/remote-bluetooth.md) .

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV-Bereitstellung

Ebenso wie bei der Entwicklung für IOS benötigen die neuen tvos das geeignete Bereitstellungs Profil für die Entwicklung und Verteilung, basierend auf der Team Mitgliedschaft und den Signierungs Identitäten, die Sie bereits mit Apple erstellt haben.

Die richtige Bereitstellung ist auch erforderlich, um auf tvos-Funktionen wie icloud-KVS oder cloudkit-Datenspeicher zuzugreifen. Weitere Informationen zur Unterstützung von icloud in ihren xamarin. tvos-apps finden Sie in unseren [Ressourcen und Datenspeicher](~/ios/tvos/app-fundamentals/resources-data-storage.md) .

Bereitstellungs Profile werden auf die gleiche Weise wie die Arbeit mit xamarin. IOS-Apps erstellt und installiert. Weitere Informationen finden Sie in der Dokumentation zur IOS- [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) .

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV-apps

Die neue Apple TV-Hardware und tvos 9 unterstützen zwei Arten von apps: herkömmliche apps und Client/Server-apps.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>Herkömmliche apps

Herkömmliche apps werden aus dem Apple TV App Store gekauft und direkt auf dem Gerät installiert. Diese Apps können Spiele, Hilfsprogramme oder Media-apps sein, die mit den gleichen Frameworks und Techniken wie xamarin. IOS-apps entwickelt werden.

Apple TV-apps haben eine maximale Größe von 200 MB und können zusätzliche 2 GB Inhalt mithilfe von bedarfsgesteuerten Ressourcen herunterladen. Weitere Informationen finden Sie in unseren [Ressourcen und der Datenspeicherung](~/ios/tvos/app-fundamentals/resources-data-storage.md) .

Sehen Sie sich unsere [Hello, tvos Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md) an, um sich mit den Tools und Konzepten vertraut zu machen, die zum Entwickeln von tvos-apps mit xamarin. tvos erforderlich sind

<a name="Summary" />

### <a name="client-server-apps"></a>Client-Server-apps

Zusätzlich zu den installierten herkömmlichen apps erleichtert Apple TV das Erstellen webbasierter Streaming-Apps für Client-Server-Medien mithilfe von Webtechnologien (HTTPS, XML und JavaScript). Sie entwerfen die Benutzeroberfläche mit der tvml-Markup Sprache von Apple und verwenden JavaScript, um das Verhalten der App mithilfe von tvmlkit zu definieren.

Weitere Informationen finden Sie in der Apple [TV Markup Language Reference](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [tvjs Framework Reference](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [tvmlkit Framework Reference](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), Informationen [zu http Live Streaming](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) und [HLS Authoring Specification für Apple TV](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) . Dokumentation.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>Herausforderungen der Benutzeroberfläche

Anders als bei IOS oder OS X verfügt das Apple TV nicht über einen Touchscreen oder eine Maus, mit dem der Benutzer eine APP oder den dazugehörigen Inhalt direkt auswählen und damit interagieren kann. Stattdessen verwenden Sie den neuen Siri-Remote-oder Bluetooth-Spiele Controller, um die Benutzeroberfläche einer APP zu navigieren. Weitere Informationen finden Sie in unserer Dokumentation zu den [Remote-und Bluetooth-Controllern von Siri](~/ios/tvos/platform/remote-bluetooth.md) .

Darüber hinaus unterscheidet sich die Gesamt Benutzererfahrung stark von den IOS-oder Mac-apps, die in der Regel eine einzelne Benutzererfahrung bieten. Mit dem Apple TV ist die Benutzererfahrung tendenziell eher sozialer Natur, wenn mehrere Personen auf dem Couch sitzen, die mit einer einzelnen App interagieren. Zum Entwerfen einer erfolgreichen Apple TV-app (entweder eine neue APP oder das Portieren einer bereits vorhandenen) müssen diese Änderungen berücksichtigt werden. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>Arbeiten mit Fokus-und Parametern

Wie bereits erwähnt, interagieren Benutzer Ihrer xamarin. tvos-APP nicht direkt mit ihrer Schnittstelle wie IOS, wo Sie auf Bilder auf dem Bildschirm des Geräts tippen, aber indirekt über den Raum mithilfe von Siri Remote. Zum präsentieren und Verarbeiten dieser Benutzerinteraktion verwendet das Apple TV ein Fokus basiertes Modell. 

Als Fokus Änderungen werden die Benutzeroberflächen Elemente, die derzeit den Fokus haben, durch feine Animationen und Effekte (z. b. die Auswirkungen auf Bilder) eindeutig identifiziert.

Wenn der Benutzer eine langsame, zirkuläre Bewegung für die Siri-Remote Bewegung ausführt, wandelt sich das fokussierte Element in Reaktion auf diese Bewegung in Echtzeit durch. Wenn der Einfluss auftritt, wird ein beleuchteter Glanz auf das Bild angewendet, sodass die Oberfläche scheint. Nach einer bestimmten Menge an Inaktivität werden alle nicht Fokus bezogenen Inhalts-und das fokussierte Element noch größer.

Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Arbeiten mit Symbolen und Images](~/ios/tvos/app-fundamentals/icons-images.md) .

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>Der Startbildschirm

Der Apple TV-Startbildschirm zeigt alle installierten apps an und bietet eine Möglichkeit, auf die Benutzereinstellungen zuzugreifen:

[![](tvos9-images/home01.png "The Home Screen")](tvos9-images/home01.png#lightbox)

Der Benutzer navigiert ein Raster von App-Symbolen mithilfe von Touchgesten auf der Siri-Remote. dabei wird der Fokus verwendet, um eine APP auszuwählen und zu starten. Das App-Symbol ist die erste Möglichkeit, Ihren potenziellen Benutzern einen guten Eindruck zu verschaffen und den Zweck Ihrer APP auf einen Blick zu vermitteln.

Jede APP muss sowohl eine kleine als auch eine große Version des App-Symbols bereitstellen. Das kleine Symbol wird bei der Installation der APP auf dem Apple TV-Startbildschirm verwendet. Die umfangreiche Version wird vom App Store verwendet. Das Symbol für große apps sollte das Aussehen und Gefühl der kleinen Symbol Version imitieren.

Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit Symbolen und Images](~/ios/tvos/app-fundamentals/icons-images.md) .

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>Der obere Regal

Wenn der Benutzer die xamarin. tvos-app in der obersten Zeile des Apple TV-Startbildschirms abgelegt hat, wird ein großes hoch Regal Bild angezeigt, wenn die APP vom Benutzer ausgewählt wird. Dieses Bild sollte die Features Ihrer APP hervorheben oder direkte Links zu den Inhalten bereitstellen.

[![](tvos9-images/topshelf01.png "The Top Shelf")](tvos9-images/topshelf01.png#lightbox)

Das Top-Regal Bild kann entweder als einzelner statischer `.png` oder `.lsr` Datei bereitgestellt werden, oder es kann zur Laufzeit dynamisch als einzelne Zeile von Fokus baren Elementen erstellt werden.

Anstatt ein statisches Top-Regal Bild anzuzeigen, kann es eine dynamische Zeile oder Fokussier Bare Elemente oder einen dynamischen Satz von scrollbanner enthalten. Beide dynamischen Stile ermöglichen es Ihnen, den von der APP bereitgestellten Inhalt hervorzuheben oder zu den am häufigsten verwendeten Features zu springen.

Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit Symbolen und Images](~/ios/tvos/app-fundamentals/icons-images.md) und in der [tvservices-frameworkreferenz](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) von Apple, um weitere Informationen zum Hinzufügen einer Top-Regal-Erweiterung zu Ihrer APP zu erhalten, um dynamische Top-Shelf-Inhalte bereitzustellen

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Entwickeln von Apps für tvos mit xamarin (Video)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
