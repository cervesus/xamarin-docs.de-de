---
title: Xamarin.Forms-Eingabehilfen
description: Erstellen eine barrierefreie Anwendung stellt sicher, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit einem Bereich von Anforderungen und Erfahrungen, die für eine Methode.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 813100acad03684fa9db5343534aee7ca13400c1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241854"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms-Eingabehilfen

_Erstellen eine barrierefreie Anwendung stellt sicher, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit einem Bereich von Anforderungen und Erfahrungen, die für eine Methode._

Eine Xamarin.Forms-Anwendung zugänglich gemacht bedeutet darum geht, das Layout und den Entwurf der viele Elemente der Benutzeroberfläche. Richtlinien für Punkte zu berücksichtigen, finden Sie unter der [Eingabehilfen Prüfliste](~/cross-platform/app-fundamentals/accessibility.md). Viele Zugriffsprobleme z. B. große Schriftarten und geeignete Einstellungen für Farbe und Kontrast können bereits von Xamarin.Forms APIs adressiert werden.

Die [Android Eingabehilfen](~/android/app-fundamentals/accessibility.md) und [iOS Eingabehilfen](~/ios/app-fundamentals/accessibility.md) Handbücher enthalten Details zu den systemeigenen APIs von Xamarin, verfügbar gemacht werden und die [uwp-Eingabehilfen Guide auf MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) erläutert, die einheitlichen Ansatz für diese Plattform. Diese APIs werden verwendet, um Anwendungen mit Barrierefreiheit vollständig auf jeder Plattform zu implementieren.

Xamarin.Forms verfügt nicht über *integrierte* aller Eingabehilfen-APIs, die für jedes der zugrunde liegenden Plattformen zu unterstützen. Es unterstützt jedoch Eingabehilfen Einstellungswerte auf Elemente der Benutzeroberfläche zur Unterstützung der Bildschirm Reader und Navigation-Unterstützung nach "-Tools, einer der wichtigsten Bestandteile Erstellen von Anwendungen zugegriffen werden kann. Weitere Informationen finden Sie unter [Eingabehilfen Einstellungswerte auf Elemente der Benutzeroberfläche](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Andere Eingabehilfen-APIs (z. B. [PostNotification auf iOS](~/ios/app-fundamentals/accessibility.md)) besser geeignet sein kann eine [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) oder [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) Implementierung. Diese werden in diesem Handbuch nicht behandelt.

## <a name="testing-accessibility"></a>Barrierefreiheit

Xamarin.Forms-Anwendungen in der Regel auf mehrere Plattformen ausgerichtet, d. h. die Barrierefreiheitsfunktionen entsprechend der Plattform zu testen. Führen Sie diesen Links erfahren, wie Sie Zugriff auf jeder Plattform zu testen:

- [**iOS testen**](~/ios/app-fundamentals/accessibility.md)
- [**Android testen**](~/android/app-fundamentals/accessibility.md)
- [**Windows-AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Eingabehilfen](~/cross-platform/app-fundamentals/accessibility.md)
- [Festlegen von Werten für die Barrierefreiheit für Elemente der Benutzeroberfläche](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
