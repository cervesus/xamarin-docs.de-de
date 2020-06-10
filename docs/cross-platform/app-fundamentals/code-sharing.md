---
title: Übersicht über gemeinsame Code
description: 'In diesem Dokument werden die verschiedenen Methoden der Freigabe von Code zwischen plattformübergreifenden Projekten verglichen: freigegebene Projekte, Portable Klassenbibliotheken und .NET Standard, einschließlich der vor-und Nachteile.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: davidortinau
ms.author: daortin
ms.date: 08/06/2018
ms.openlocfilehash: 5edfd8216892eb28a2b1ad14d3ccee1668b21a43
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571219"
---
# <a name="sharing-code-overview"></a>Übersicht über gemeinsame Code

_In diesem Dokument werden die verschiedenen Methoden der Freigabe von Code zwischen plattformübergreifenden Projekten verglichen: .NET Standard, freigegebene Projekte und Portable Klassenbibliotheken, einschließlich der vor-und Nachteile._

Es gibt drei Methoden für die gemeinsame Nutzung von Code zwischen plattformübergreifenden Anwendungen:

- [**.NET Standard Bibliotheken**](#Net_Standard) – .NET Standard Projekte können Code implementieren, der auf mehreren Plattformen freigegeben werden kann, und auf eine große Anzahl von .NET-APIs (abhängig von der Version) zugreifen können. .NET Standard 1,0-1,6 implementieren progressiver größere APIs, während .NET Standard 2,0 die beste Abdeckung der .net-BCL bietet (einschließlich der .NET-APIs, die in xamarin-apps verfügbar sind).
- Frei [**gegebene Projekte**](#Shared_Projects) – verwenden Sie den Projekttyp "freigegeben", um den Quellcode zu organisieren, und verwenden Sie zum `#if` Verwalten von plattformspezifischen Anforderungen Compilerdirektiven.
- [**Portable Klassenbibliotheken**](#Portable_Class_Libraries) (veraltet) – Portable Klassenbibliotheken (Portable Class Libraries, pcls) können auf mehrere Plattformen mit einer gemeinsamen API-Oberfläche abzielen und mithilfe von Schnittstellen plattformspezifische Funktionen bereitstellen. Pcls sind in den neuesten Versionen von Visual Studio veraltet &ndash; . verwenden Sie stattdessen .NET Standard.

Das Ziel einer Strategie für die Code Freigabe besteht darin, die in diesem Diagramm gezeigte Architektur zu unterstützen, in der eine einzelne Codebasis von mehreren Plattformen verwendet werden kann.

 ![Architektur von freigegebenen Code Anwendungen](code-sharing-images/conceptualarchitecture.png "Architektur von freigegebenen Code Anwendungen")

In diesem Artikel werden die verfügbaren Methoden verglichen, damit Sie den richtigen Projekttyp für Ihre Anwendungen auswählen können.

<a name="Net_Standard"></a>

## <a name="net-standard-libraries"></a>.NET Standard-Bibliotheken

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) Bibliotheken stellen einen klar definierten Satz von Basisklassen Bibliotheken bereit, auf die in unterschiedlichen Projekttypen verwiesen werden kann, einschließlich plattformübergreifender Projekte wie xamarin. Android und xamarin. IOS. .NET Standard 2,0 wird empfohlen, um eine maximale Kompatibilität mit vorhandenem .NET Framework Code zu untersuchen.

![.NET Standard Diagramm](code-sharing-images/netstandard.png ".NET Standard Diagramm")

### <a name="benefits"></a>Vorteile

- Ermöglicht die gemeinsame Nutzung von Code in mehreren Projekten.
- Beim Refactoring von Vorgängen werden immer alle betroffenen Verweise aktualisiert.
- Ein größerer Oberflächen Bereich der .net-Basisklassen Bibliothek (Base Class Library, BCL) ist verfügbar als PCL-Profile. Insbesondere hat .NET Standard 2,0 fast dieselbe API-Oberfläche wie die .NET Framework und wird für neue apps und für das Portieren vorhandener pcls empfohlen.

### <a name="disadvantages"></a>Nachteile

- Compilerdirektiven können nicht wie verwendet werden `#if __IOS__` .

### <a name="remarks"></a>Hinweise

.NET Standard [ähnelt PCL](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), aber mit einem einfacheren Modell für Platt Form Unterstützung und einer größeren Anzahl von Klassen aus der BCL.

<a name="Shared_Projects"></a>

## <a name="shared-projects"></a>Freigegebene Projekte

Frei [gegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) enthalten Code Dateien und Assets, die in jedem Projekt enthalten sind, das auf Sie verweist. Freigabe Projekte führen nicht selbst zu einer kompilierten Ausgabe.

Dieser Screenshot zeigt eine Projektmappendatei mit drei Anwendungsprojekten (für Android, IOS und Windows) mit einem frei **gegebenen** Projekt, das allgemeine c#-Quell Code Dateien enthält:

![Projekt Mappe freigegeben](code-sharing-images/sharedsolution.png "Projekt Mappe freigegeben")

Die konzeptionelle Architektur ist im folgenden Diagramm dargestellt, wobei jedes Projekt alle freigegebenen Quelldateien enthält:

![Diagramm des freigegebenen Projekts](code-sharing-images/sharedassetproject.png "Diagramm des freigegebenen Projekts")

### <a name="example"></a>Beispiel

Eine plattformübergreifende Anwendung, die IOS, Android und Windows unterstützt, erfordert ein Anwendungsprojekt für jede Plattform. Der gemeinsame Code befindet sich im freigegebenen Projekt.

Eine Beispiellösung würde die folgenden Ordner und Projekte enthalten (Projektnamen wurden zur Ausdruckskraft ausgewählt, Ihre Projekte müssen diese Benennungs Richtlinien nicht einhalten):

- **Shared** – frei gegebenes Projekt, das den Code enthält, der allen Projekten gemeinsam ist.
- **Appandroid** – xamarin. Android-Anwendungsprojekt.
- **Appios** – xamarin. IOS-Anwendungsprojekt.
- **Appwindows** – Windows-Anwendungsprojekt.

Auf diese Weise verwenden die drei Anwendungsprojekte denselben Quellcode (die c#-Dateien in "Shared"). Alle Änderungen am freigegebenen Code werden in allen drei Projekten gemeinsam genutzt.

### <a name="benefits"></a>Vorteile

- Ermöglicht die gemeinsame Nutzung von Code in mehreren Projekten.
- Frei gegebener Code kann basierend auf der Plattform mithilfe von Compilerdirektiven verzweigt werden (z. b. Verwenden von `#if __ANDROID__` , wie im Dokument zum Entwickeln von Platt [Form übergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) erläutert.
- Anwendungsprojekte können plattformspezifische Verweise enthalten, die vom freigegebenen Code verwendet werden können (z `Community.CsharpSqlite.WP7` . b. die Verwendung von im Tasky-Beispiel für Windows Phone).

### <a name="disadvantages"></a>Nachteile

- Refacktoren, die Code in inaktiven Compilerdirektiven beeinflussen, aktualisieren den Code in diesen Anweisungen nicht.
- Im Gegensatz zu den meisten anderen Projekttypen hat ein frei gegebenes Projekt keine "Output"-Assembly. Während der Kompilierung werden die Dateien als Teil des verweisenden Projekts behandelt und in diese Assembly kompiliert. Wenn Sie Ihren Code als Assembly freigeben möchten, sind .NET Standard oder portablen Klassenbibliotheken eine bessere Lösung.

<a name="Shared_Remarks"></a>

### <a name="remarks"></a>Hinweise

Eine gute Lösung für Anwendungsentwickler, die Code schreiben, der nur für die Freigabe in Ihrer APP vorgesehen ist (und nicht an andere Entwickler verteilt werden soll).

<a name="Portable_Class_Libraries"></a>

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

> [!TIP]
> .NET Standard 2,0-Bibliotheken werden von portablen Klassenbibliotheken empfohlen.

Portable Klassenbibliotheken werden [hier ausführlich erläutert](~/cross-platform/app-fundamentals/pcl.md).

![Diagramm der portablen Klassenbibliothek](code-sharing-images/portableclasslibrary.png "Diagramm der portablen Klassenbibliothek")

### <a name="benefits"></a>Vorteile

- Ermöglicht die gemeinsame Nutzung von Code in mehreren Projekten.
- Beim Refactoring von Vorgängen werden immer alle betroffenen Verweise aktualisiert.

### <a name="disadvantages"></a>Nachteile

- In den neuesten Versionen von Visual Studio als veraltet markiert, werden stattdessen .NET Standard Bibliotheken empfohlen. Beachten Sie diese [Erläuterung der Unterschiede](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) zwischen PCL und .NET Standard.
- Compileranweisungen können nicht verwendet werden.
- Es kann nur eine Teilmenge von .NET Framework verwendet werden, die durch das ausgewählte Profil bestimmt wird (Weitere Informationen finden [Sie in der Einführung in die PCL](~/cross-platform/app-fundamentals/pcl.md) ).

### <a name="remarks"></a>Hinweise

Die PCL-Vorlage wird in den neuesten Versionen von Visual Studio als veraltet eingestuft.

## <a name="summary"></a>Zusammenfassung

Die gewählte Code Freigabe Strategie wird von den Plattformen gesteuert, auf die Sie abzielen. Wählen Sie eine Methode aus, die am besten für Ihr Projekt geeignet ist.

.NET Standard ist die beste Wahl zum Entwickeln von freigegeben-Codebibliotheken (vor allem das Veröffentlichen auf nuget). Freigegebene Projekte funktionieren gut für Anwendungsentwickler, die eine Vielzahl plattformspezifischer Funktionen in ihren plattformübergreifenden apps planen.

Obwohl PCL-Projekte weiterhin in Visual Studio unterstützt werden, wird .NET Standard für neue Projekte empfohlen.

## <a name="related-links"></a>Verwandte Links

- [Entwickeln von plattformübergreifenden Anwendungen (Hauptdokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Fallstudie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky-Beispiel (GitHub)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Tasky-Beispiel mit PCL (GitHub)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
