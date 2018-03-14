---
title: "Optionen für die Codefreigabe"
description: "Dieses Dokument vergleicht die verschiedenen Methoden zum Freigeben von Code für die plattformübergreifende Projekte: freigegebene Projekte, Portable Klassenbibliotheken und .NET Standard, einschließlich der Vorteile und Nachteile."
ms.topic: article
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e7289d92043bdbe9e4ec55776835530f8ccec526
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="sharing-code-options"></a>Optionen für die Codefreigabe

_Dieses Dokument vergleicht die verschiedenen Methoden zum Freigeben von Code für die plattformübergreifende Projekte: freigegebene Projekte, Portable Klassenbibliotheken und .NET Standard, einschließlich der Vorteile und Nachteile._

Es gibt drei alternative Methoden zur Freigabe von Code zwischen plattformübergreifende Anwendungen:

-   [**Freigegebener Projekte** ](#Shared_Projects) – verwenden Sie freigegebene Asset-Projekttyp, um den Quellcode zu organisieren und mit dem Compiler-Direktiven #if nach Bedarf plattformspezifische Anforderungen verwalten.
-   [**Portable Klassenbibliotheken** ](#Portable_Class_Libraries) – erstellen Sie die Anwendung eine Portable Klassenbibliothek (PCL) die Plattformen, die Sie unterstützen möchten, und Schnittstellen verwenden, um plattformspezifische Funktionalität bereitzustellen.
-   [**Standardbibliotheken .NET** ](#Net_Standard) – .NET Standardprojekten funktionieren auf ähnliche Weise zu PCLs, erfordern die Verwendung von Schnittstellen Clientplattform-spezifische Funktionen einfügen.

Das Ziel einer Codefreigabe Strategie ist zur Unterstützung von der Architektur, die in diesem Diagramm gezeigt, in eine einzige Codebasis mehrere Plattformen verwendet werden kann.

 ![](code-sharing-images/conceptualarchitecture.png "Freigegebene Code-Anwendungsarchitektur")

In diesem Artikel werden die drei Methoden zur Auswahl den richtigen Projekttyp für Ihre Anwendungen verglichen.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Gemeinsam genutzte Projekte

Die einfachste Vorgehensweise zum Freigeben von Codedateien ist die Verwendung einer [freigegebenes Projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Diese bildschirmabbildung zeigt eine Projektmappendatei mit drei Anwendungsprojekte (für Android, iOS und Windows Phone), mit einem **Shared** Projekt, das allgemeine C#-Quellcodedateien enthält:

 ![](code-sharing-images/sharedsolution.png "Freigegebene-Projektmappe")

Im folgenden Diagramm wird Architekturkonzept angezeigt, wobei jedes Projekt für alle freigegebenen Quelldateien enthält:

 ![](code-sharing-images/sharedassetproject.png "Freigegebene Projekt-Diagramm")


### <a name="example"></a>Beispiel

Eine plattformübergreifende-Anwendung, die iOS, Android und Windows Phone unterstützt, müsste ein Anwendungsprojekt für jede Plattform. Der allgemeine Code befindet sich im Projekt gemeinsam genutzt.

Die folgenden Ordner und Projekte (Projektnamen ausgewählt für Ausdruckskraft, Ihre Projekte nicht diese Benennungskonvention befolgen verfügen), würde eine Beispiel-Projektmappe enthalten:

-   **Shared** – freigegebenes Projekt mit dem Code, die alle Projekte gemeinsam.
-   **AppAndroid** – Xamarin.Android-Anwendungsprojekt.
-   **AppiOS** – Xamarin.iOS-Anwendungsprojekt.
-   **AppWinPhone** – Windows Phone-Anwendungsprojekt.


Auf diese Weise werden die drei Anwendungsprojekte den gleichen Quellcode (C#-Dateien im freigegebenen) freigeben. Alle Änderungen an den gemeinsam verwendeten Code werden für alle drei Projekte gemeinsam genutzt werden.


### <a name="benefits"></a>Vorteile

-  Ermöglicht es Ihnen, Code in mehreren Projekten gemeinsam nutzen.
-  Freigegebene Code kann verzweigt werden basierend auf der Plattform, die mithilfe von Compiler-Direktiven (z. b. mit `#if __ANDROID__` , wie im Abschnitt der [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument).
-  Anwendungsprojekte zählen plattformspezifischen Verweise, die der gemeinsam verwendeten Code genutzt werden kann (z. B. mit `Community.CsharpSqlite.WP7` im Tasky Beispiel für Windows Phone).



### <a name="disadvantages"></a>Nachteile

-  Im Gegensatz zu den meisten anderen Projekttypen weisen muss ein freigegebenes Projekt keine ""-Ausgabeassembly. Während der Kompilierung werden die Dateien als Teil des verweisenden Projekts behandelt und in dieser Assembly kompiliert. Wenn Sie den Code als Assembly freigeben möchten dann sind portablen Klassenbibliotheken "oder" Standard ".NET eine bessere Lösung.
-  Refactorings, die Code in "inactive" Compilerdirektiven Auswirkungen auf wird den Code nicht aktualisiert werden.


 <a name="Shared_Remarks" />

### <a name="remarks"></a>Hinweise

Eine geeignete Lösung für Anwendungsentwickler, die Schreiben von Code, der nur zur Freigabe in der Anwendung (und nicht an andere Entwickler verteilen) vorgesehen ist.

 <a name="Portable_Class_Libraries" />


## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken


Portable Klassenbibliotheken sind [hier ausführlich erörtert](~/cross-platform/app-fundamentals/pcl.md).

 ![](code-sharing-images/portableclasslibrary.png "Portable Library-Klassendiagramm")


### <a name="benefits"></a>Vorteile

-  Ermöglicht es Ihnen, Code in mehreren Projekten gemeinsam nutzen.
-  Umgestaltungsvorgänge aktualisieren Sie immer alle betroffenen Verweise.


### <a name="disadvantages"></a>Nachteile

-  Compilerdirektiven kann nicht verwendet werden.
-  Nur eine Teilmenge von .NET Framework verwendet, wird durch das ausgewählte Profil bestimmt (finden Sie unter der [Einführung in die PCL](~/cross-platform/app-fundamentals/pcl.md) für Weitere Informationen).


### <a name="remarks"></a>Hinweise

Eine geeignete Lösung, wenn Sie die resultierende Assembly mit anderen Entwicklern gemeinsam nutzen möchten.



<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET Standardbibliotheken

.NET ist [hier ausführlich erörtert](~/cross-platform/app-fundamentals/net-standard.md).

![](code-sharing-images/netstandard.png ".NET standard-Diagramm")

### <a name="benefits"></a>Vorteile

-  Ermöglicht es Ihnen, Code in mehreren Projekten gemeinsam nutzen.
-  Umgestaltungsvorgänge aktualisieren Sie immer alle betroffenen Verweise.
-  Eine größere Oberfläche von der .NET Basisklassenbibliothek (BCL) ist als PCL Profile verfügbar.

### <a name="disadvantages"></a>Nachteile

 -  Compilerdirektiven kann nicht verwendet werden.

### <a name="remarks"></a>Hinweise

.NET standard gleicht PCL, sondern mit einem einfacher Modell für die Plattform-Unterstützung und eine größere Anzahl von Klassen aus der BCL.



## <a name="summary"></a>Zusammenfassung

Die gewählte Strategie für die Codefreigabe wird von den Plattformen Ihrer Zielgruppe gesteuert. Wählen Sie eine Methode, die am besten für Ihr Projekt aus.

PCL "oder" Standard ".NET sind eine gute Wahl zum Erstellen von teilbar Codebibliotheken (insbesondere für NuGet veröffentlichen). Gemeinsam genutzte Projekte sind optimal für Anwendungsentwickler planen, viele Plaform-spezifische Funktionen in ihren Cross-Platform-apps zu verwenden.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Cross Platform Anwendungen (Hauptspeicherorte für Dokumente)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Fallstudie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky Beispiel (Github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Tasky Beispiel mithilfe von PCL (Github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
