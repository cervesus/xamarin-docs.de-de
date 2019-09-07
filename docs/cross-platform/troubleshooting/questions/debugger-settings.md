---
title: Welche Projekteinstellungen sind für den Debugger erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 343f8d37d77726d2cdc06a74c44e476af00dde27
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765158"
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
