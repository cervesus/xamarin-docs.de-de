---
title: Welche Projekteinstellungen sind für den Debugger erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 856c04d129058e8cbac30dcdf619e8b2b5a66cb6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014267"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Welche Projekteinstellungen sind für den Debugger erforderlich?

Damit der Debugger erwartungsgemäß funktioniert (Haltepunkte erreichen, Debugprotokolle anzeigen usw.), müssen die Entwickler Instrumentation und die Anzeige der Debuginformationen aktiviert werden.

Führen Sie die folgenden Schritte aus, um die Umgebungseinstellungen zu überprüfen:

## <a name="visual-studio"></a>Visual Studio

1. Öffnen der Projektoptionen
2. Gehe zu **Build > erweitert..** . Debuginformationen auf " **Full** " festlegen
3. Einstellungen für die einzelnen Plattformen:
   - Wechseln Sie zu **Android-Optionen > Debugoptionen**. Aktivieren Sie das Kontrollkästchen **Entwickler Instrumentation aktivieren** .
   - Wechseln Sie zu **IOS debug > Debugging & Instrumentation**. Aktivieren Sie das Kontrollkästchen **Debuggen aktivieren** .

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Öffnen der Projektoptionen
2. Wechseln Sie zu **Build > Compiler > Allgemeine Optionen**. Debuginformationen auf " **Full** " festlegen
3. Einstellungen für die einzelnen Plattformen:
    - Wechseln Sie zu **Build > Android Build > Debugoptionen**. Aktivieren Sie das Kontrollkästchen **Entwickler Instrumentation aktivieren** .
    - Wechseln Sie zu **Build > IOS Debuggen**. Aktivieren Sie das Kontrollkästchen **Debuggen aktivieren** .
