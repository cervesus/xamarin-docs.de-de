---
title: Welche projekteinstellungen sind erforderlich, damit der Debugger?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.openlocfilehash: 67c4b51a518f5c7dba6ae372dbd9206dd3ef8e9f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
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

