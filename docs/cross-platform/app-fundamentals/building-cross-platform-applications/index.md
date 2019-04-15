---
title: Erstellen von plattformübergreifenden Anwendungen
description: Dieser Abschnitt beschreibt eine Zusammenfassung plus sechs Komponenten, wie Anwendungen, die mit der Xamarin-Entwicklung-Plattform – vom Verständnis der Funktionsweise von Xamarin zum Entwerfen von mobilen apps, und klicken Sie dann Tests und Bereitstellung für die verschiedenen app Stores erstellen.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: 683400e24844308769f0562552641216d45e7d11
ms.sourcegitcommit: 91a4fcb715506e18e8070bc89bf2cb14d079ad32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2019
ms.locfileid: "59574792"
---
# <a name="building-cross-platform-applications"></a>Erstellen von plattformübergreifenden Anwendungen

Es gibt zwei Optionen für die Verwendung gemeinsamen Codes in plattformübergreifenden mobilen Anwendungen aus: Freigegebene Asset-Projekte und Portable Klassenbibliotheken. Diese Optionen sind [hier besprochenen](~/cross-platform/app-fundamentals/code-sharing.md); Weitere Informationen zu [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md) und [freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) ist ebenfalls verfügbar.

<a name="Sections" />

 [Übersicht](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Teil 1 – Grundlegendes zu den mobilen Xamarin-Plattform](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Teil 2 – Architektur](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Teil 3 – Einrichten einer plattformübergreifenden Xamarin-Lösung](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Teil 4 – Umgang mit mehreren Plattformen](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Teil 5 – praktische Strategien für die Codefreigabe](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Teil 6 – Testen und App Store-Genehmigungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>Fallstudien

In diesem Dokument beschriebenen Prinzipien werden eingefügt, in der Praxis in der beispielanwendung *Tasky*, als auch [vorgefertigten Anwendungen](https://xamarin.com/prebuilt) wie [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky ist eine einfache aufgabenlistenanwendung für iOS, Android und Windows Phone.
Es veranschaulicht die Grundlagen der Erstellung einer plattformübergreifenden Anwendung mit Xamarin und verwendet eine lokale SQLite-Datenbank.

 [![tasky list](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [![tasky list](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Lesen der [Tasky Fallstudie](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt werden die Xamarin Tools für die Anwendungsentwicklung eingeführt und wird erläutert, wie Sie Anwendungen erstellen, die mehrere mobile Plattformen abzielen.

Er umfasst einer geschichteten Architektur dieser Strukturen-Code für die erneute Verwendung auf mehreren Plattformen und beschreibt verschiedene Softwaremuster, die in dieser Architektur verwendet werden können.

Beispiele es werden vorgestellt, der allgemeine Funktionen der Anwendung (z. B. Datei- und Netzwerk-Vorgänge) und wie diese plattformübergreifende so erstellt werden können.

Schließlich kurz erläutert die Tests, und enthält Verweise auf eine Fallstudie, die diese Prinzipien in Aktion zu versetzt.

## <a name="related-links"></a>Verwandte Links

- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
- [Fallstudie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky Beispiel-App (Github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Xamarin-Mobile-Anwendungsentwicklung: Plattformübergreifende C# und Xamarin.Forms-Grundlagen (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Mobile Entwicklung mit C# von Greg Fußketten (O' Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Professionelle, plattformübergreifende Mobile Entwicklung in C# Scott Olson, John Hunter, Ben Horgen, Kenny Goers (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
