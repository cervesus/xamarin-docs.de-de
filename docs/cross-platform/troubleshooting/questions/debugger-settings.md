---
title: Welche Projekteinstellungen sind für den Debugger erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: e7a383f899fab0400104493fa89b125788d610aa
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831339"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Welche Projekteinstellungen sind für den Debugger erforderlich?

Damit kann der Debugger funktioniert wie erwartet (Haltepunkte, Anzeige Debugprotokollen usw.) muss Entwickler Instrumentation und Debuggen Informationen anzeigen aktiviert sein.

Führen Sie diese Schritte aus, um die umgebungseinstellungen zu überprüfen:

## <a name="visual-studio"></a>Visual Studio
1. Öffnen Sie die Optionen für das Projekt
2. Wechseln Sie zu **erstellen > Advanced...** Legen Sie die Informationen zum Debuggen auf **vollständige**
3. Einstellungen für jede Plattform:
   - Wechseln Sie zu **Android-Optionen > Debugoptionen**. Tick der **Entwicklerinstrumentierung aktivieren** Feld.
   - Wechseln Sie zu **iOS-Debuggen > Debugging und Instrumentierung**. Tick der **Debuggen aktivieren** Feld.

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac
1. Öffnen Sie die Optionen für das Projekt
2. Wechseln Sie zu **erstellen > Compiler > Allgemeine Optionen**. Legen Sie die Informationen zum Debuggen auf **vollständige**
3. Einstellungen für jede Plattform:
    - Wechseln Sie zu **erstellen > Android-Build > Debugoptionen**. Tick der **Entwicklerinstrumentierung aktivieren** Feld.
    - Wechseln Sie zu **erstellen > iOS-Debugging**. Tick der **Debuggen aktivieren** Feld.

