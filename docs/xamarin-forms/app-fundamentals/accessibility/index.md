---
title: Xamarin.Forms-Barrierefreiheit
description: Erstellen eine barrierefreie Anwendung stellt sicher, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit einer Reihe von Anforderungen und Erfahrungen Ansatz.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: ac0ffbdce6b0c55e8ad9d774d80e3d9b8bf84089
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116445"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms-Barrierefreiheit

_Erstellen eine barrierefreie Anwendung stellt sicher, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit einer Reihe von Anforderungen und Erfahrungen Ansatz._

Eine Xamarin.Forms-Anwendung für jedermann bedeutet Gedanken um das Layout und Entwurf von viele Elemente der Benutzeroberfläche. Richtlinien dazu, Probleme zu berücksichtigen, finden Sie unter den [Checkliste für die Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md). Viele Zugriffsprobleme, wie Groß und geeignete Einstellungen für Farbe und Kontrast können von Xamarin.Forms-APIs bereits behoben werden.

Die [Android Barrierefreiheit](~/android/app-fundamentals/accessibility.md) und [iOS Barrierefreiheit](~/ios/app-fundamentals/accessibility.md) Handbücher enthalten die Details der nativen APIs von Xamarin, und die [UWP Barrierefreiheit Guide auf MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) wird erläutert, die einheitlichen Ansatz für diese Plattform. Diese APIs werden verwendet, um Anwendungen mit Barrierefreiheit auf jeder Plattform vollständig zu implementieren.

Xamarin.Forms verfügt derzeit nicht über *integrierte* Unterstützung für alle Eingabehilfen-APIs, die jeweils zugrundeliegenden Plattformen verfügbar. Es unterstützt jedoch Einstellungseigenschaften für die Automatisierung auf die Elemente der Benutzeroberfläche zur Unterstützung der Bildschirm-Reader und Navigation Hilfstools, dies ist einer der wichtigsten Teile des Erstellen von Anwendungen mit Barrierefreiheit. Weitere Informationen finden Sie unter [Automatisierungseigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md).

Xamarin.Forms-Anwendungen können auch die Aktivierreihenfolge von Steuerelementen, die angegeben haben. Weitere Informationen finden Sie unter [Navigation per Tastatur](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md).

Andere Eingabehilfen-APIs (wie z. B. [PostNotification unter iOS](~/ios/app-fundamentals/accessibility.md)) möglicherweise besser eignet sich für eine [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) oder [Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) Implementierung. Diese werden in diesem Handbuch nicht behandelt.

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

Xamarin.Forms-Anwendungen in der Regel auf mehrere Plattformen ausgerichtet, was bedeutet die Barrierefreiheitsfunktionen entsprechend der Plattform zu testen. Führen Sie diesen Links, um zu erfahren, wie Sie Zugriff auf jeder Plattform zu testen:

- [**iOS testen**](~/ios/app-fundamentals/accessibility.md)
- [**Testen von Android**](~/android/app-fundamentals/accessibility.md)
- [**Windows-AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>Verwandte Links

- [Cross-Platform-Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md)
- [Automation-Eigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Tastaturzugriff](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
