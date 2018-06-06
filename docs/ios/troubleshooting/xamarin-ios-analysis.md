---
title: Regeln zur Codeanalyse Xamarin.iOS
description: Dieses Dokument beschreibt einen Satz von Regeln für die Codeanalyse, mit denen überprüft Xamarin.iOS-projekteinstellungen, um zu ermitteln, wenn mehr/better-optimized Einstellungen verfügbar sind.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 25d2936f70981ed239626193ba6e4993f1378108
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788897"
---
# <a name="xamarinios-analysis-rules"></a>Regeln zur Codeanalyse Xamarin.iOS

Xamarin.iOS Analyse ist ein Satz von Regeln, die überprüfen Sie das Projekt, mit denen Sie bestimmen, ob die Einstellungen für optimierte besser/mehr verfügbar sind.

Führen Sie die Regeln für die Codeanalyse so oft wie möglich bei mögliche Verbesserungen frühzeitig zu suchen, und speichern die Entwicklungszeit.

Wählen Sie zum Ausführen der Regeln in Visual Studio für Mac Menü **Projekt > Codeanalyse ausführen**.

> [!NOTE]
> Xamarin.iOS Analyse wird nur für die derzeit ausgewählte Konfiguration ausgeführt. Es wird dringend empfohlen Ausführen des Tools für das Debuggen **und** Releasekonfigurationen.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **Problem:** der Linker ist für den Debugmodus auf Gerät deaktiviert.
- **Fix:** sollten Sie versuchen, Ihren Code mit den Linker an, alle überraschungen vermeiden auszuführen.
Um dies einzurichten, wechseln Sie zum Projekt > iOS-Build > Linker-Verhalten.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **Problem:** App-Builds, die den Test Cloud Agent initialisieren zurückgewiesen von Apple, wenn übermittelt, wie sie private-API verwenden.
- **Fix:** hinzufügen oder korrigieren Sie die erforderlichen #if und im Code definiert.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **Problem:** Debug-Konfiguration, die Entwickler Signaturschlüssel verwendet eine IPA Bedarf nur für die Verteilung, die jetzt die Publishing-Assistent verwendet nicht generieren soll.
- **Fix:** IPA Build in den Projekteigenschaften für die Debugkonfiguration zu deaktivieren.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **Problem:** die unterstützten Architektur für "release | Gerät"wird nicht auf 64-Bit-kompatibel, ARM64 fehlt. Dies ist ein Problem, da es sich bei Apple 32 Bits nur iOS-apps in den App Store nicht akzeptiert wird.
- **Fix:** Double klicken Sie auf Ihrem iOS-Projekt, wechseln Sie zur Erstellung > iOS erstellen und unterstützten Architekturen ändern, sodass er ARM64 verfügt.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **Problem:** nicht mit der float32-Option (--Aot-Options = O = float32) führt zu einer umfangreichen Leistungseinbußen, insbesondere für Mobile, in denen Genauigkeit mathematische Funktionen mit doppelter messbar langsamer wird. Beachten Sie, dass .NET mit doppelter Genauigkeit intern, auch für "float", also die Aktivierung dieser Option wirkt sich auf Genauigkeit und möglicherweise die Kompatibilität.
- **Fix:** Double klicken Sie auf Ihrem iOS-Projekt, wechseln Sie zur Erstellung > iOS zu erstellen, und deaktivieren Sie die "Alle 32-Bit-Gleitkommawert-Vorgänge als 64-Bit-Gleitkommawert ausführen".

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **Problem:** es wird empfohlen, das verwaltete Objekt für eine bessere Leistung, kleinere Größe ausführbarer, anstelle des systemeigenen Handlers für "HttpClient" und eine neuere Standards problemlos zu unterstützen.
- **Fix:** Double klicken Sie auf Ihrem iOS-Projekt, wechseln Sie zur Erstellung > iOS erstellen und ändern Sie die Implementierung für "HttpClient" in NSUrlSession (iOS 7 und höher) oder CFNetwork Version vor iOS 7 unterstützt.
