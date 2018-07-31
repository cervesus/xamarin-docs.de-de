---
title: Welche projekteinstellungen sind für den Debugger erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 646ef7f708be2de6a851ace25d69a7c2f0b18a83
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350806"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>Welche projekteinstellungen sind für den Debugger erforderlich?

Damit kann der Debugger funktioniert wie erwartet (Haltepunkte, Anzeige Debugprotokollen usw.) muss Entwickler Instrumentation und Debuggen Informationen anzeigen aktiviert sein.

Führen Sie diese Schritte aus, um die umgebungseinstellungen zu überprüfen:

## <a name="visual-studio"></a>Visual Studio
1. Öffnen Sie die Optionen für das Projekt
2. Wechseln Sie zu **erstellen > Advanced...** Legen Sie die Informationen zum Debuggen auf **vollständige**
3. Einstellungen für jede Plattform:
   - Wechseln Sie zu **Android-Optionen > Debugoptionen**. Tick der **Entwicklerinstrumentierung aktivieren** Feld.
   - Wechseln Sie zu **iOS-Build > Optionen-Debuggen**. Tick der **Debuggen aktivieren** Feld.

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac
1. Öffnen Sie die Optionen für das Projekt
2. Wechseln Sie zu **erstellen > Compiler > Allgemeine Optionen**. Legen Sie die Informationen zum Debuggen auf **vollständige**
3. Einstellungen für jede Plattform:
  - Wechseln Sie zu **erstellen > Android-Build > Debugoptionen**. Tick der **Entwicklerinstrumentierung aktivieren** Feld.
  - Wechseln Sie zu **erstellen > iOS-Debugging**. Tick der **Debuggen aktivieren** Feld.

