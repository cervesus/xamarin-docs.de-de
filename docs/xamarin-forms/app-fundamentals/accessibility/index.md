---
title: Xamarin.Forms-Barrierefreiheit
description: Durch die Erstellung einer barrierefreien Anwendung wird sichergestellt, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit unterschiedlichen Bedürfnissen und Erfahrungen nutzen möchten.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: 20ea72e588e2e3b1d575bd593446bf6724d73d8c
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971060"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms-Barrierefreiheit

_Durch die Erstellung einer barrierefreien Anwendung wird sichergestellt, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit unterschiedlichen Bedürfnissen und Erfahrungen nutzen möchten._

Eine Xamarin.Forms-Anwendung barrierefrei zu machen, bedeutet, sich Gedanken über das Layout und Design der vielen Benutzeroberflächenelemente zu machen. Unter [Barrierefreiheit in Xamarin-Apps](~/cross-platform/app-fundamentals/accessibility.md) finden Sie Richtlinien für die Herangehensweise an unterschiedliche Themen, die beachtet werden sollten. Vielen Probleme im Zusammenhang mit der Barrierefreiheit, z. B. große Schriftarten und geeignete Farb- und Kontrasteinstellungen, können schon von Xamarin.Forms-APIs gelöst werden.

Die Leitfäden [Eingabehilfen unter Android](~/android/app-fundamentals/accessibility.md) und [Eingabehilfen unter iOS](~/ios/app-fundamentals/accessibility.md) enthalten Informationen über die nativen APIs, die durch Xamarin verfügbar gemacht werden. Der Leitfaden [Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) erläutert die native Herangehensweise auf dieser Plattform. Diese APIs werden zur vollständigen Implementierung barrierefreier Anwendungen auf jeder Plattform verwendet.

Xamarin.Forms verfügt derzeit nicht über *integrierte* Unterstützung für alle Barrierefreiheits-APIs, die auf jeder zugrunde liegenden Plattform verfügbar sind. Jedoch wird die Erstellung von Automatisierungseigenschaften auf Benutzeroberflächenelementen unterstützt, um somit Hilfstools für die Sprachausgabe und Navigation zu unterstützen, was einen der wichtigsten Teile der Erstellung von barrierefreien Anwendungen darstellt. Weitere Informationen finden Sie unter [Automatisierungseigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md).

Für Xamarin.Forms-Anwendungen kann ebenso die Aktivierreihenfolge der Steuerelemente angegeben sein, um die Nutzbarkeit und Barrierefreiheit zu optimieren. Weitere Informationen finden Sie unter [Barrierefreiheit der Tastatur](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md).

Andere Barrierefreiheits-APIs (z. B. [PostNotification on iOS](~/ios/app-fundamentals/accessibility.md)) sind unter Umständen besser für ein [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)-Konzept oder die Implementierung eines [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) geeignet. Diese werden jedoch nicht in diesem Leitfaden behandelt.

## <a name="testing-accessibility"></a>Testen der Barrierefreiheit

Xamarin.Forms-Anwendungen sind in der Regel für mehrere Plattformen konzipiert, das bedeutet, dass die Barrierefreiheitsfunktionen entsprechend der Plattform getestet werden. Folgen Sie diesen Links, um zu erfahren, wie Sie die Barrierefreiheit auf der jeweiligen Plattform testen können:

- [**iOS Testing (Tests unter iOS)**](~/ios/app-fundamentals/accessibility.md)
- [**Android Testing (Tests unter Android)**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>Verwandte Links

- [Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)
- [Automatisierungseigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Barrierefreiheit der Tastatur](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
