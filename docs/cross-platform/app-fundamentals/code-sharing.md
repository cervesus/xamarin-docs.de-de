---
title: Übersicht über das Freigeben von Code
description: 'In diesem Dokument werden die verschiedenen Methoden des Verwendung gemeinsamen Codes in plattformübergreifenden Projekten verglichen: freigegebene Projekte, Portable Class Libraries und .NET Standard, einschließlich der Vorteile und Nachteile.'
ms.prod: xamarin
ms.assetid: B73675D2-09A3-14C1-E41E-20352B819B53
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 82a73619e4c0507e8857cc91d88ababa870013de
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270471"
---
# <a name="sharing-code-overview"></a>Übersicht über das Freigeben von code

_In diesem Dokument werden die verschiedenen Methoden des Verwendung gemeinsamen Codes in plattformübergreifenden Projekten verglichen: .NET Standard, Projekte freigegeben und Portable Class Libraries, einschließlich der Vorteile und Nachteile._

Es gibt drei Methoden zur Freigabe von Code zwischen plattformübergreifenden Anwendungen:

- [**.NET standard-Bibliotheken** ](#Net_Standard) : .NET Standard-Projekte können implementieren den Code auf mehreren Plattformen gemeinsam verwendet werden, und können auf eine große Anzahl von .NET-APIs (abhängig von der Version) zugreifen. .NET Standard 1.0-1.6 implementieren zunehmend größer werdenden Sätze von APIs, während .NET Standard 2.0 die beste Abdeckung der bereitstellt.
- [**Freigegebene Projekte** ](#Shared_Projects) – verwenden Sie den Typ des freigegebenen Projekts, um Ihren Quellcode zu organisieren, und verwenden `#if` Compilerdirektiven erforderlich, um plattformspezifischen Anforderungen verwalten.
- [**Portable Klassenbibliotheken** ](#Portable_Class_Libraries) (veraltet) – Portable Class Libraries (PCLs) können mehrere Plattformen mit einer gemeinsamen API-Oberfläche, und verwenden Sie Schnittstellen, um plattformspezifische Funktionalität bereitzustellen. PCLs sind in den neuesten Versionen von Visual Studio veraltet &ndash; verwenden Sie stattdessen .NET Standard.

Das Ziel einer Strategie für die gemeinsame Verwendung von Code werden die in diesem Diagramm dargestellte Architektur unterstützen, in eine einzigen Codebasis von mehreren Plattformen genutzt werden kann.

 ![Freigegebene Code Anwendungsarchitektur](code-sharing-images/conceptualarchitecture.png "freigegebene Code-Anwendungsarchitektur")

In diesem Artikel werden die Methoden zur Auswahl den richtigen Projekttyp für Ihre Anwendungen verglichen.

<a name="Net_Standard" />

## <a name="net-standard-libraries"></a>.NET standard-Bibliotheken

[.NET standard](~/cross-platform/app-fundamentals/net-standard.md) Bibliotheken bieten eine gut definierten Satzes von den Basisklassenbibliotheken, die in anderen Projekttypen zur Verfügung, einschließlich plattformübergreifende Projekte wie Xamarin.Android und Xamarin.iOS verwiesen werden kann. .NET standard 2.0 wird für maximale Kompatibilität mit vorhandenem .NET Framework-Code empfohlen.

![.NET standard-Diagramm](code-sharing-images/netstandard.png ".NET Standard-Diagramm")

### <a name="benefits"></a>Vorteile

- Können Sie Code in mehreren Projekten gemeinsam nutzen.
- Umgestaltungsvorgänge aktualisieren Sie immer alle betroffenen Verweise.
- Eine größere Oberfläche von der .NET Basisklassenbibliothek (BCL) ist als PCL-Profile verfügbar. Insbesondere .NET Standard 2.0 verfügt über fast die gleichen API-Oberfläche als .NET Framework und wird für neue apps und Portieren vorhandene PCLs empfohlen.

### <a name="disadvantages"></a>Nachteile

- Können keine Compilerdirektiven wie `#if __IOS__`.

### <a name="remarks"></a>Hinweise

.NET standard ist [PCL ähnelt](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries), aber mit ein einfacheres Modell für die Plattform-Unterstützung und eine größere Anzahl von Klassen aus der BCL.

<a name="Shared_Projects" />

## <a name="shared-projects"></a>Freigegebene Projekte

[Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md) enthalten Dateien mit Code und Ressourcen, die in jedem Projekt enthalten sind, die sie verweist. Freigabe Projekte erzeugen nicht kompilierte Ausgabe selbst.

Dieser Screenshot zeigt eine Projektmappendatei mit drei Anwendungsprojekte (für Android, iOS und Windows), mit einem **Shared** Projekt, das allgemeine C#-Quellcodedateien enthält:

![Freigegebene projektlösung](code-sharing-images/sharedsolution.png "projektlösung freigegeben")

Die konzeptionelle Architektur ist in der folgenden Abbildung dargestellt, wobei jedes Projekt die freigegebene Quelldateien enthält:

![Diagramm der Shared-Projekt](code-sharing-images/sharedassetproject.png "Shared-Projekt-Diagramm")

### <a name="example"></a>Beispiel

Eine plattformübergreifende-Anwendung, die iOS-, Android- und Windows unterstützt würde ein Anwendungsprojekt für jede Plattform erfordern. Der allgemeine Code befindet sich im freigegebenen Projekt.

Eine Beispiel für eine Projektmappe würde enthalten, die folgenden Ordner und Projekte (Projektnamen ausgewählt worden sein für Ausdruckskraft, müssen Ihre Projekte nicht diesen Benennungsrichtlinien folgen):

- **Freigegebene** – freigegebenes Projekt, die den Code für alle Projekte.
- **AppAndroid** – Xamarin.Android-Anwendung-Projekt.
- **AppiOS** – Xamarin.iOS-Anwendungsprojekts.
- **AppWindows** – Windows-Anwendungsprojekt.

Auf diese Weise werden die drei Projekte den gleiche Quellcode (c#-Dateien in freigegebenen) freigeben. Alle Änderungen an der freigegebene Code werden alle drei Projekte gemeinsam genutzt werden.

### <a name="benefits"></a>Vorteile

- Können Sie Code in mehreren Projekten gemeinsam nutzen.
- Freigegebener Code kann gebrancht werden basierend auf der Plattform, die mithilfe von compileranweisungen (z. b. Mithilfe von `#if __ANDROID__` , wie dies in der [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument).
- Webanwendungsprojekte zählen plattformspezifischen Referenzen, die der freigegebene Code nutzen können (z. B. `Community.CsharpSqlite.WP7` im Tasky Beispiel für Windows Phone).

### <a name="disadvantages"></a>Nachteile

- Refactorings, mit denen Code innerhalb von 'inactive' Compilerdirektiven Auswirkungen auf werden den Code in diese Anweisungen nicht aktualisiert werden.
- Im Gegensatz zu den meisten anderen Projekttypen ist ein freigegebenes Projekt keine Assembly "Output". Während der Kompilierung werden die Dateien als Teil des verweisenden Projekts behandelt und in die Assembly kompiliert. Wenn Sie den Code als Assembly freigeben möchten sind .NET Standard oder Portable Class Libraries eine bessere Lösung.

<a name="Shared_Remarks" />

### <a name="remarks"></a>Hinweise

Eine gute Lösung für Anwendungsentwickler, die Schreiben von Code, der nur für Freigabe in der app (und nicht für die Verteilung an andere Entwickler) vorgesehen ist.

<a name="Portable_Class_Libraries" />

## <a name="portable-class-libraries"></a>Portable Klassenbibliotheken

> [!TIP]
> .NET standard 2.0-Bibliotheken sind Portable Class Libraries vorgezogen.

Portable Klassenbibliotheken sind [hier ausführlich erörtert](~/cross-platform/app-fundamentals/pcl.md).

![Portable Bibliothek Klassendiagramm](code-sharing-images/portableclasslibrary.png "Portable Class Library-Diagramm")

### <a name="benefits"></a>Vorteile

- Können Sie Code in mehreren Projekten gemeinsam nutzen.
- Umgestaltungsvorgänge aktualisieren Sie immer alle betroffenen Verweise.

### <a name="disadvantages"></a>Nachteile

- Als veraltet markiert, in den neuesten Versionen von Visual Studio, werden .NET Standard-Bibliotheken stattdessen empfohlen. Verwenden Sie dieses [Erläuterung der Unterschiede](https://docs.microsoft.com/dotnet/standard/net-standard#comparison-to-portable-class-libraries) zwischen PCL und .NET Standard.
- Compiler-Direktiven kann nicht verwendet werden.
- Nur eine Teilmenge von .NET Framework zur Verfügung zu verwenden, durch das ausgewählte Profil bestimmt (finden Sie unter den [Einführung in die PCL](~/cross-platform/app-fundamentals/pcl.md) Informationen).

### <a name="remarks"></a>Hinweise

PCL-Vorlage wird in den neuesten Versionen von Visual Studio als veraltet betrachtet.

## <a name="summary"></a>Zusammenfassung

Durch die Plattform, die Sie verwenden möchten, wird die gewählte Strategie für die Codefreigabe gesteuert werden. Wählen Sie eine Methode, die am besten für Ihr Projekt.

.NET standard ist die beste Wahl für das Erstellen von freigegebenen Codebibliotheken (insbesondere auf NuGet veröffentlichen). Freigegebene Projekte eignen sich gut für Anwendungsentwickler, die viele plattformspezifische Funktionalität in ihre plattformübergreifenden apps verwenden möchten.

Während PCL-Projekte in Visual Studio werden unterstützt weiterhin, wird .NET Standard für neue Projekte empfohlen.

## <a name="related-links"></a>Verwandte Links

- [Building Cross Platform Applications (wichtigsten Dokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Freigegebene Projekte](~/cross-platform/app-fundamentals/shared-projects.md)
- [.NET-Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Fallstudie: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky Beispiel (Github)](https://github.com/xamarin/mobile-samples/tree/master/Tasky)
- [Tasky Beispiel mit PCL (Github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
