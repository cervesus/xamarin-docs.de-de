---
title: Regeln zur Codeanalyse Xamarin.iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 7cf627f369b666bb54d0f512dc1361d2a685a057
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-analysis-rules"></a>Regeln zur Codeanalyse Xamarin.iOS


## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **Problem:** der Linker ist für den Debugmodus auf Gerät deaktiviert.
- **Fix:** sollten Sie versuchen, Ihren Code mit den Linker an, alle überraschungen vermeiden auszuführen.
Um dies einzurichten, wechseln Sie zum Projekt > iOS-Build > Linker-Verhalten.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **Problem:** App-Builds, die den Test Cloud Agent initialisieren zurückgewiesen von Apple, wenn übermittelt, wie sie private-API verwenden.
- **Fix:** hinzufügen oder korrigieren Sie die erforderlichen #if und im Code definiert.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **Problem:** Debug-Konfiguration, die Entwickler Signaturschlüssel verwendet eine IPA Bedarf nur für die Verteilung, die jetzt die Publishing-Assistent verwendet nicht generieren soll.
- **Fix:** IPA Build in den Projekteigenschaften für die Debugkonfiguration zu deaktivieren.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **Problem:** die unterstützten Architektur für "release | Gerät"wird nicht auf 64-Bit-kompatibel, ARM64 fehlt. Dies ist ein Problem, da es sich bei Apple 32 Bits nur iOS-apps in den App Store nicht akzeptiert wird.
- **Fix:** Double klicken Sie auf Ihrem iOS-Projekt, wechseln Sie zur Erstellung > iOS erstellen und unterstützten Architekturen ändern, sodass er ARM64 verfügt.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **Problem:** nicht mit der float32-Option (--Aot-Options = O = float32) führt zu einer umfangreichen Leistungseinbußen, speziell für Mobiltelefon, wobei Genauigkeit mathematische Funktionen mit doppelter messbar langsamer ist. Beachten Sie, dass .NET mit doppelter Genauigkeit intern, auch für "float", also die Aktivierung dieser Option wirkt sich auf Genauigkeit und möglicherweise die Kompatibilität.
- **Fix:** Double klicken Sie auf Ihrem iOS-Projekt, wechseln Sie zur Erstellung > iOS zu erstellen, und deaktivieren Sie die "Alle 32-Bit-Gleitkommawert-Vorgänge als 64-Bit-Gleitkommawert ausführen".
