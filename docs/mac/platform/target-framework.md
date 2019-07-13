---
title: Ziel-Framework für Xamarin.Mac
description: Dieser Artikel behandelt die Zielframeworks (Base Class Libraries) für die Xamarin.Mac verfügbar, und die Auswirkungen der Verwendung in Ihre Xamarin.Mac-Projekt.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
ms.openlocfilehash: e9e20b4966e9e6cb8a4ce3ad6724cf0ba2565c33
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865865"
---
# <a name="target-framework-for-xamarinmac"></a>Ziel-Framework für Xamarin.Mac

_Dieser Artikel behandelt die Zielframeworks (Base Class Libraries) für die Xamarin.Mac verfügbar, und die Auswirkungen der Verwendung in Ihre Xamarin.Mac-Projekt._

![Ziel-Framework-Optionen für Xamarin.Mac](target-framework-images/select-target.png "als Ziel für Xamarin.Mac Framework-Optionen")

## <a name="background"></a>Hintergrund

Jeder .NET Programm oder die Bibliothek hängt davon ab, durch die Basisklassenbibliothek (BCL) für die Funktionalität. Diese BCL umfasst-Assemblys wie Mscorlib, System, System.Net.Http und "System.xml", die die allgemeine Funktionalität, die in allen Sprachen für .NET integriert bereitstellen.

Im Laufe der Jahre haben gibt es mehrere verschiedene Versionen der dieser BCL, optimiert für verschiedene Anwendungsfälle entwickelt. Die "desktop" BCL umfasst einen umfangreicheren Satz von Bibliotheken zu schwer für andere Anwendungsfälle, während Mobile konzentriert sich auf, um sicherzustellen, dass die APIs für die Verknüpfung, sind möglicherweise, der entfernt nicht verwendeten Code, um die Anwendung zu reduzieren.

Einer von der wichtigeren folgen diese unterschiedlichen Ziel-Frameworks ist, alle Assemblys in einem bestimmten Programm *müssen* kompatiblen BCL-Assemblys als Ziel. Wenn dies nicht der Fall war, können Sie zwei Assemblys, die für verschiedene Versionen von verknüpft haben die **"System.dll"** Einwand über die Signatur eines bestimmten Typs. Eine freigegebene Bibliothek kann entweder Ziel [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), dies ist die gleiche Teilmenge von Ziel-Frameworks oder ein bestimmtes Zielframework.

Drei Zielframework-Optionen stehen zur Verfügung, für die Xamarin.Mac, jeweils mit verschiedenen vor- und Nachteile:

- **Moderne** (Mobile in die ältere Dokumentation genannt) – eine Teilmenge sehr ähnlich, welche Befugnisse Xamarin.iOS, hoher Leistung und Größe optimiert. Zielframework ist Linker sicher ist, sodass diese Projekte die endgültige Speicherbedarf erheblich reduziert, indem nicht verwendeter Code entfernt haben.

- **Vollständige** (XM 4.5 in die ältere Dokumentation genannt) – eine sehr ähnlich Teilmenge an die BCL "desktop" mit ein paar kleinen entfernungen. Das Zielframework beinahe identisch, net45 (und höher) ist, problemlos nutzen viele NuGet-Pakete, die keine entweder netstandard2 bereitstellen oder bestimmte Xamarin.Mac erstellt. Es ist jedoch aufgrund der Verwendung von "System.Configuration" nicht kompatibel mit verknüpfen.

- **Nicht unterstützte** (genannt System in die ältere Dokumentation) – stattdessen mit einer BCL von Xamarin.Mac bereitgestellten verknüpfen, verwenden die aktuelle System installierte Mono. Dadurch wird die vollen Satz von Assemblys, bekannt, dass problematisch sein, einschließlich (z. B. "System.Drawing"). Diese Option ist nur vorhanden, verfügt über "letztes Mittel", und es wird dringend empfohlen, um die anderen Optionen ausgeschöpft, bevor Sie sie verwenden. Wie der Name schon sagt, wird die Verwendung von offiziellen Support-Kanäle nicht unterstützt.

## <a name="setting-the-target-framework"></a>Festlegen des Zielframeworks

Führen Sie folgende Schritte aus, um in den Zielframework-Typ für eine Xamarin.Mac-Projekt zu ändern:

1. Öffnen Sie das Xamarin.Mac-Projekt in Visual Studio für Mac.
2. Doppelklicken Sie auf die Projektdatei im **Projektmappen-Explorer**, um die **Projektoptionen** zu öffnen.
3. Von der **allgemeine** Registerkarte, wählen Sie den Typ der **Zielframework** , die den Anforderungen Ihrer Anwendung passt:

    [![Verwenden das Fenster "Projektoptionen" auf ein Zielframework](target-framework-images/select-target-full.png "mit, dass das Fenster \"Projektoptionen\" Wählen Sie ein Zielframework")](target-framework-images/select-target-full-large.png#lightbox)

4. Klicken Sie auf **OK**, um die Änderungen zu speichern.

Sollten Sie **Bereinigen** und dann **Rebuild** Ihrer Xamarin.Mac-Projekt nach dem Wechsel des Ziel-Framework-Typs.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden kurz behandelt, die verschiedenen Typen von Zielframeworks (Base Class Libraries) verfügbar, um einer Xamarin.Mac-Anwendung, und bei jeder Art von Framework verwendet werden soll.


## <a name="related-links"></a>Verwandte Links

- [iOS und Mac-Codefreigabe](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Assemblys](~/cross-platform/internals/available-assemblies.md)
- [Aktualisieren von vorhandenen Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md)
