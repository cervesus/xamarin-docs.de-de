---
title: Zielframework für Xamarin.Mac
description: Dieser Artikel behandelt die Zielframeworks (Base Class Libraries) für Xamarin.Mac verfügbar, und die Auswirkungen Ihrer Verwendung in Ihrem Projekt Xamarin.Mac.
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: 28d312ae10ce736a1720384fe76714910c3ff8f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792501"
---
# <a name="target-framework-for-xamarinmac"></a>Zielframework für Xamarin.Mac

_Dieser Artikel behandelt die Zielframeworks (Base Class Libraries) für Xamarin.Mac verfügbar, und die Auswirkungen Ihrer Verwendung in Ihrem Projekt Xamarin.Mac._

![Ziel-Framework-Optionen für Xamarin.Mac](target-framework-images/select-target.png "Framework von Optionen für Xamarin.Mac als Ziel")

## <a name="background"></a>Hintergrund

Jeder .NET Programm oder eine Bibliothek hängt davon ab, durch die Basisklassenbibliothek (BCL) bereitgestellte Funktionalität. Diese BCL enthält Assemblys wie Mscorlib, System, System.Net.Http und "System.xml", die die allgemeine Funktionalität, die in allen integrierten bereitstellen.

Im Laufe der Jahre haben gibt es mehrere verschiedene Versionen von diesem BCL, optimiert für verschiedene Anwendungsfälle entwickelt. Die BCL "desktop" enthält einen umfassenderen Satz an Bibliotheken zu schwer für andere Anwendungsfälle während Mobile konzentriert sich auf das sicherstellen, dass die APIs für verknüpfen, sicher sind, die möglicherweise entfernt nicht verwendeten Code, um die Anwendung zu verkleinern.

Keines dieser anderen Ziel-Frameworks, die noch wichtiger Verarbeitungsverhaltens handelt, die alle Assemblys in einem bestimmten Programm *müssen* kompatibel BCL-Assemblys als Ziel. Wenn dies nicht der Fall war, konnte Sie zwei Assemblys, die für verschiedene Versionen von verknüpft haben die **"System.dll"** Einwand über die Signatur eines bestimmten Typs. Eine freigegebene Bibliothek können entweder Ziel [.NET Standard 2](https://blog.xamarin.com/share-code-net-standard-2-0/), also die gleiche Teilmenge von der Zielframeworks oder ein bestimmtes Ziel-Framework.

Drei Zielframework-Optionen sind verfügbar für Xamarin.Mac, jeweils mit verschiedenen vor- und Nachteile:

- **Moderne** (Mobile in ältere Dokumentation genannt) – eine Teilmenge sehr ähnlich, welche Befugnisse Xamarin.iOS, mit hoher Leistung und Größe optimiert. Dieses Ziel-Framework ist Linker sicher, damit diese Projekte können ihre endgültigen Speicherbedarf erheblich reduziert wird, durch das Entfernen von nicht verwendeten Codes.

- **Vollständige** (XM 4.5 in ältere Dokumentation genannt) – eine sehr ähnliche Teilmenge, die "desktop" BCL, mit wenigen kleine entfernen. Als das Zielframework weitgehend net45 (und höher) ist, problemlos beanspruchen viele Nugets, die keine entweder netstandard2 bereitstellen kann, oder bestimmte Xamarin.Mac erstellt. Es ist jedoch aufgrund der Verwendung von "System.Configuration" bei der Verknüpfung nicht kompatibel.

- **Nicht unterstützte** (genannt System in der Dokumentation für ältere) – stattdessen mit einem BCL Xamarin.Mac gebotenen verknüpfen, verwenden das aktuelle System installiert Mono. Hier werden die Computerressourcen Satz von Assemblys, einschließlich einige bekannte problematisch sein (z. B. "System.Drawing"). Diese Option ist nur vorhanden, hat ein "Notfall", und es wird dringend empfohlen, andere Optionen ausgeschöpft, bevor Sie ihn verwenden. Wie der Name schon sagt, wird die Verwendung von Kanälen offizielle Unterstützung nicht unterstützt.

## <a name="setting-the-target-framework"></a>Festlegen des Zielframeworks

Führen Sie folgende Schritte aus, um in den Ziel-Framework-Typ für ein Projekt Xamarin.Mac zu ändern:

1. Öffnen Sie das Xamarin.Mac-Projekt in Visual Studio für Mac.
2. Doppelklicken Sie auf die Projektdatei im **Projektmappen-Explorer**, um die **Projektoptionen** zu öffnen.
3. Aus der **allgemeine** Registerkarte, wählen Sie den Typ des **Zielframework** , die den Anforderungen Ihrer Anwendung am besten entspricht:

  [![Verwenden Sie das Fenster Projektoptionen zur ein Zielframeworks](target-framework-images/select-target-full.png "verwenden Sie das Fenster Projektoptionen zur ein Zielframeworks")](target-framework-images/select-target-full-large.png#lightbox)

4. Klicken Sie auf **OK**, um die Änderungen zu speichern.

Sie sollten **Bereinigen** und dann **neu erstellen** Xamarin.Mac Projekt nach dem Wechsel der Ziel-Framework-Typ.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden kurz behandelt, die verschiedenen Typen von Zielframeworks (Base Class Libraries) verfügbar, eine Anwendung Xamarin.Mac und jede Art von Framework verwendet werden soll.


## <a name="related-links"></a>Verwandte Links

- [iOS und Mac-Codefreigabe](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Portable Klassenbibliotheken](~/cross-platform/app-fundamentals/pcl.md)
- [Assemblys](~/cross-platform/internals/available-assemblies.md)
- [Aktualisieren vorhandener Mac-apps](~/cross-platform/macios/unified/updating-mac-apps.md)
