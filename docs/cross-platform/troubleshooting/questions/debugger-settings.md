---
title: Welche Projekteinstellungen sind für den Debugger erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 9a18c97ba227615ae42529424b5c22b5e144f5e5
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526702"
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

