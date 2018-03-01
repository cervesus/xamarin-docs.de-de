---
title: Welche projekteinstellungen sind erforderlich, damit der Debugger?
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 6097dc8dfdff8807137ef68be86a08e4c9e23988
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Welche projekteinstellungen sind erforderlich, damit der Debugger?

In der Reihenfolge für den Debugger so funktionieren wie erwartet (Haltepunkte, Anzeige Debugprotokolle usw.) muss angezeigten Informationen Instrumentation und Debug Developer aktiviert sein.

Führen Sie diese Schritte aus, um die umgebungseinstellungen zu überprüfen:

## <a name="visual-studio"></a>Visual Studio
1. Öffnen Sie die Projektoptionen
2. Wechseln Sie zu **erstellen > erweiterte...** Festlegen von Debuginformationen zu **vollständige**
3. Einstellungen für jede Plattform:
   - Wechseln Sie zu **Android Options > Debugoptionen**. Tick der **aktivieren Entwickler Instrumentation** Feld.
   - Wechseln Sie zu **iOS-Build > Optionen-Debuggen**. Tick der **Debuggen aktivieren** Feld.

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac
1. Öffnen Sie die Projektoptionen
2. Wechseln Sie zu **erstellen > Compiler > Allgemeine Optionen**. Festlegen von Debuginformationen zu **vollständige**
3. Einstellungen für jede Plattform:
  - Wechseln Sie zu **erstellen > Android Build > Debugoptionen**. Tick der **aktivieren Entwickler Instrumentation** Feld.
  - Wechseln Sie zu **erstellen > iOS Debug**. Tick der **Debuggen aktivieren** Feld.

