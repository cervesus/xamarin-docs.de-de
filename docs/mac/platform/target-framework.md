---
title: Ziel Framework für xamarin. Mac
description: Dieser Artikel behandelt die Ziel-Frameworks (Basisklassen Bibliotheken), die für xamarin. Mac verfügbar sind, und die Auswirkungen der Verwendung in Ihrem xamarin. Mac-Projekt.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 11/10/2017
ms.openlocfilehash: 4ae8834427580c387de7a38a69d711207b04821e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290890"
---
# <a name="target-framework-for-xamarinmac"></a>Ziel Framework für xamarin. Mac

_Dieser Artikel behandelt die Ziel-Frameworks (Basisklassen Bibliotheken), die für xamarin. Mac verfügbar sind, und die Auswirkungen der Verwendung in Ihrem xamarin. Mac-Projekt._

![Ziel Framework-Optionen für xamarin. Mac](target-framework-images/select-target.png "Ziel Framework-Optionen für xamarin. Mac")

## <a name="background"></a>Hintergrund

Jedes .NET-Programm oder jede beliebige Bibliothek ist von der von der Basisklassen Bibliothek (BCL) bereitgestellten Funktionalität abhängig. Diese BCL umfasst Assemblys wie mscorlib, System, System .net. http und System. XML, die die allgemeinen Funktionen bereitstellen, die in alle .NET-Sprachen integriert sind.

Im Laufe der Jahre wurden mehrere verschiedene Versionen dieser BCL entwickelt, die für unterschiedliche Anwendungsfälle optimiert wurden. Die BCL "Desktop" umfasst einen umfassenderen Satz an Bibliotheken, die für andere Anwendungsfälle zu vielleicht zu groß werden können, während sich der Mobile Fokus darauf konzentriert, dass APIs für die Verknüpfung sicher sind

Eine der wichtigsten Auswirkungen dieser unterschiedlichen Ziel-Frameworks ist, dass alle Assemblys in einem bestimmten Programm kompatible BCL-Assemblys als Ziel haben *müssen* . Wenn dies nicht der Fall ist, können Sie über zwei Assemblys verfügen, die mit verschiedenen Versionen der **System. dll** verknüpft sind, um die Signatur eines bestimmten Typs zu überschreiben. Eine freigegebene Bibliothek kann entweder [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), d. h. die gemeinsame Teilmenge der Ziel-Frameworks, oder ein bestimmtes Ziel Framework als Ziel haben.

Für xamarin. Mac stehen drei Ziel Framework-Optionen zur Verfügung, die jeweils unterschiedliche Vorteile und Nachteile bieten:

- **Modern** (in älterer Dokumentation als "Mobile" bezeichnet) – eine sehr ähnliche Teilmenge, die für die Leistung und Größe von xamarin. IOS stark optimiert ist. Dieses Ziel Framework ist Linker sicher. Daher können diese Projekte durch Entfernen von nicht verwendetem Code drastisch reduziert werden.

- **Vollständig** (in der älteren Dokumentation als XM 4,5 bezeichnet) – eine sehr ähnliche Teilmenge auf die "Desktop"-BCL mit einigen kleinen Entfernungen. Da das Ziel Framework fast mit net45 (und höher) identisch ist, kann es problemlos viele nuget verarbeiten, die weder netstandard2 noch bestimmte xamarin. Mac-Builds bereitstellen. Aufgrund der Verwendung von System. Configuration ist Sie jedoch nicht mit dem Verknüpfen kompatibel.

- **Nicht unterstützt** (in älterer Dokumentation genannt) – verwenden Sie das aktuell installierte System Mono, anstatt eine Verknüpfung mit einer von xamarin. Mac bereitgestellten BCL zu verwenden. Dies bietet den vollständigsten Satz von Assemblys, einschließlich einiger bekanntermaßen problematisch (z. b. System. Drawing). Diese Option ist bereits vorhanden und wird dringend empfohlen, andere Optionen zu debugden, bevor Sie Sie verwenden. Wie der Name schon sagt, wird die Verwendung durch offizielle Support Kanäle nicht unterstützt.

## <a name="setting-the-target-framework"></a>Festlegen des Ziel Frameworks

Gehen Sie folgendermaßen vor, um zum Ziel Framework-Typ für ein xamarin. Mac-Projekt zu wechseln:

1. Öffnen Sie das Xamarin.Mac-Projekt in Visual Studio für Mac.
2. Doppelklicken Sie auf die Projektdatei im **Projektmappen-Explorer**, um die **Projektoptionen** zu öffnen.
3. Wählen Sie auf der Registerkarte **Allgemein** den Typ des **Ziel Frameworks** aus, das den Anforderungen Ihrer Anwendung entspricht:

    [![Verwenden des Fensters "Projektoptionen" zum Auswählen eines Ziel Frameworks](target-framework-images/select-target-full.png "Verwenden des Fensters \"Projektoptionen\" zum Auswählen eines Ziel Frameworks")](target-framework-images/select-target-full-large.png#lightbox)

4. Klicken Sie auf **OK**, um die Änderungen zu speichern.

Sie sollten das xamarin. Mac-Projekt nach dem Wechsel des zielframeworktyps **Bereinigen** und anschließend **neu erstellen** .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die verschiedenen Typen von Ziel-Frameworks (Basisklassen Bibliotheken), die für eine xamarin. Mac-Anwendung verfügbar sind, und die einzelnen Frameworktypen, die verwendet werden sollten, kurz behandelt.


## <a name="related-links"></a>Verwandte Links

- [IOS-und Mac-Code Freigabe](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Assemblys](~/cross-platform/internals/available-assemblies.md)
- [Aktualisieren vorhandener Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md)
