---
title: Xamarin.Forms-Barrierefreiheit
description: Xamarin.Forms-Barrierefreiheit
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.custom: video
ms.openlocfilehash: 7ac8b305ae09e005013aea9f83fb4a3e4740f2b2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84129806"
---
# <a name="xamarinforms-accessibility"></a>_Durch die Erstellung einer barrierefreien Anwendung wird sichergestellt, dass die Anwendung von Personen verwendet werden kann, die die Benutzeroberfläche mit unterschiedlichen Bedürfnissen und Erfahrungen nutzen möchten._

Um für eine Xamarin.Forms-Anwendung Barrierefreiheit zu erzielen, müssen Sie sich Gedanken über das Layout und Design der vielen Benutzeroberflächenelemente machen.

Unter [Barrierefreiheit in Xamarin-Apps](~/cross-platform/app-fundamentals/accessibility.md) finden Sie Richtlinien für die Herangehensweise an unterschiedliche Themen, die beachtet werden sollten. Vielen Probleme im Zusammenhang mit der Barrierefreiheit, z. B. große Schriftarten und geeignete Farb- und Kontrasteinstellungen, können schon von Xamarin.Forms-APIs gelöst werden. Die Leitfäden [Eingabehilfen unter Android](~/android/app-fundamentals/accessibility.md) und [Eingabehilfen unter iOS](~/ios/app-fundamentals/accessibility.md) enthalten Informationen über die nativen APIs, die durch Xamarin verfügbar gemacht werden. Der Leitfaden [Verfügbarmachen von grundlegenden Informationen zur Barrierefreiheit](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) erläutert die native Herangehensweise auf dieser Plattform.

Diese APIs werden zur vollständigen Implementierung barrierefreier Anwendungen auf jeder Plattform verwendet. Xamarin.Forms verfügt derzeit nicht über *integrierte* Unterstützung für alle Barrierefreiheits-APIs, die auf jeder zugrunde liegenden Plattform verfügbar sind.

Jedoch wird die Erstellung von Automatisierungseigenschaften auf Benutzeroberflächenelementen unterstützt, um somit Hilfstools für die Sprachausgabe und Navigation zu unterstützen, was einen der wichtigsten Teile der Erstellung von barrierefreien Anwendungen darstellt. Weitere Informationen finden Sie unter [Automatisierungseigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md). Für Xamarin.Forms-Anwendungen kann auch die Aktivierreihenfolge der Steuerelemente angegeben werden, um die Benutzerfreundlichkeit und Barrierefreiheit zu optimieren.

Weitere Informationen finden Sie unter [Barrierefreiheit der Tastatur](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md). Andere Barrierefreiheits-APIs (z. B. [PostNotification on iOS](~/ios/app-fundamentals/accessibility.md)) sind unter Umständen besser für ein [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)-Konzept oder die Implementierung eines [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) geeignet.

Diese werden jedoch nicht in diesem Leitfaden behandelt. Testen der Barrierefreiheit

## <a name="testing-accessibility"></a>Xamarin.Forms-Anwendungen sind in der Regel für mehrere Plattformen konzipiert. Dies bedeutet, dass die Barrierefreiheitsfunktionen gemäß der jeweiligen Plattform getestet werden.

Folgen Sie diesen Links, um zu erfahren, wie Sie die Barrierefreiheit auf der jeweiligen Plattform testen können: [**iOS Testing (Tests unter iOS)** ](~/ios/app-fundamentals/accessibility.md)

- [**Android Testing (Tests unter Android)** ](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)** ](https://msdn.microsoft.com/library/windows/desktop/dn433239)
- Verwandte Links

## <a name="related-links"></a>[Cross-platform Accessibility (Plattformübergreifende Barrierefreiheit)](~/cross-platform/app-fundamentals/accessibility.md)

- [Automatisierungseigenschaften](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Barrierefreiheit der Tastatur](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
- Zugehörige Videos

## <a name="related-video"></a>Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Making-Mobile-Apps-Accessible/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
