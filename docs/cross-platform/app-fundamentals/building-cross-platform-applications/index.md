---
title: Erstellen von plattformübergreifenden Anwendungen
description: In diesem Abschnitt wird in Zusammenfassung und sechs Teilen erläutert, wie Sie Anwendungen mithilfe der xamarin-Entwicklungsplattform Erstellen – von der Funktionsweise von xamarin bis zum Entwerfen mobiler apps und anschließendes testen und Bereitstellen in den verschiedenen App Stores.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
author: conceptdev
ms.author: crdun
ms.date: 01/28/2016
ms.openlocfilehash: 2c630e58ff5a1221ec9e95099df4781041ecf2b4
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70288058"
---
# <a name="building-cross-platform-applications"></a>Erstellen von plattformübergreifenden Anwendungen

Es gibt zwei Optionen für die gemeinsame Nutzung von Code zwischen plattformübergreifenden mobilen Anwendungen: Freigegebene assetprojekte und Portable Klassenbibliotheken. Diese Optionen werden [hier beschrieben](~/cross-platform/app-fundamentals/code-sharing.md). Weitere Informationen zu [portablen Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md) und frei [gegebenen Projekten](~/cross-platform/app-fundamentals/shared-projects.md) sind ebenfalls verfügbar.

<a name="Sections" />

 [Übersicht](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Teil 1 – Grundlegendes zur xamarin Mobile-Plattform](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Teil 2 – Architektur](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Teil 3 – Einrichten einer plattformübergreifenden xamarin-Lösung](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Teil 4 – Umgang mit mehreren Plattformen](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Teil 5 – praktische Strategien für die Code Freigabe](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Teil 6 – Testen und App Store-Genehmigungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />

## <a name="case-studies"></a>Fallstudien

Die in diesem Dokument beschriebenen Prinzipien werden in der Praxis in der Beispielanwendung *Tasky*und [vorgefertigten Anwendungen](https://xamarin.com/prebuilt) wie [xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm)beschrieben.

 <a name="Tasky" />

### <a name="tasky"></a>Tasky

Tasky ist eine einfache Aufgabenlisten Anwendung für IOS, Android und Windows phone.
Es veranschaulicht die Grundlagen der Erstellung einer plattformübergreifenden Anwendung mit xamarin und der Verwendung einer lokalen SQLite-Datenbank.

 Tasky Liste [ ![Liste](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Lesen Sie die [Tasky-Fallstudie](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).

## <a name="summary"></a>Zusammenfassung

In diesem Abschnitt werden die Anwendungs Entwicklungs Tools von xamarin vorgestellt und erläutert, wie Sie Anwendungen erstellen, die mehrere mobile Plattformen als Ziel haben.

Es behandelt eine geschichtete Architektur, die den Code für die Wiederverwendung über mehrere Plattformen hinweg strukturiert und verschiedene Software Muster beschreibt, die in dieser Architektur verwendet werden können.

Beispiele hierfür sind Allgemeine Anwendungsfunktionen (z. b. Datei-und Netzwerk Vorgänge) und die Art und Weise, wie Sie plattformübergreifend erstellt werden können.

Schließlich wird das Testen kurz erläutert, und es werden Verweise auf eine Fallstudie bereitstellt, die diese Prinzipien in Aktion einfügt.

## <a name="related-links"></a>Verwandte Links

- [Freigeben von Code Optionen](~/cross-platform/app-fundamentals/code-sharing.md)
- [Fallstudie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky-Beispiel-app (GitHub)](https://docs.microsoft.com/samples/xamarin/mobile-samples/taskyportable/)
- [Entwicklung mobiler xamarin-Anwendungen: Platt Form C# übergreifende und xamarin. Forms-Grundlagen (Amazon)](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/)
- [Mobile-Entwicklung C# mit Greg Shackles (o. o.)](http://shop.oreilly.com/product/0636920024002.do)
- [Professionelle plattformübergreifende Mobile Entwicklung in C# von Scott Olson, John Hunter, Ben Horgen, Kenny-Besucher (Wrox)](http://www.wrox.com/WileyCDA/WroxTitle/Professional-Cross-Platform-Mobile-Development-in-C-.productCd-1118157702.html)
