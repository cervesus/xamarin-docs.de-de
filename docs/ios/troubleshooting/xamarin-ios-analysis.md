---
title: Regeln für Xamarin.iOS-Analyse
description: Dieses Dokument beschreibt eine Reihe von Regeln für die Codeanalyse, mit denen überprüft Xamarin.iOS-projekteinstellungen, um zu ermitteln, wenn mehr/better-optimized Einstellungen verfügbar sind.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/06/2018
ms.openlocfilehash: 8a4990ce7b2bcacbd4b97b214458531b3d94122e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61416382"
---
# <a name="xamarinios-analysis-rules"></a>Regeln für Xamarin.iOS-Analyse

Xamarin.iOS-Analyse ist ein Satz von Regeln, mit die überprüft die projekteinstellungen, mit denen Sie bestimmen, ob optimierte Einstellungen besser/mehr verfügbar sind.

Führen Sie so oft wie möglich ist, finden mögliche Verbesserungen am Anfang, und speichern Sie die Entwicklungszeit Analyseregeln.

Führen Sie die Regeln an, in Visual Studio für Mac Menü wählen **Projekt > Codeanalyse ausführen**.

> [!NOTE]
> Xamarin.iOS-Analyse kann nur auf die ausgewählte Konfiguration ausgeführt. Es wird dringend empfohlen Ausführen des Tools zum Debuggen **und** Releasekonfigurationen.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: DisabledLinkerRule

- **Problem:** Der Linker ist für den Debugmodus auf Gerät deaktiviert.
- **Fix:** Sie sollten zum Ausführen des Codes mit dem Linker, um überraschungen zu vermeiden.
Informationen zum Einrichten, zu Projekt wechseln > iOS-Build > Linker anders verhält.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: TestCloudAgentReleaseRule

- **Problem:** App-Builds, die der Test Cloud Agent initialisiert werden von Apple, die bei der Übermittlung, zurückgewiesen, wie private-API zu verwenden.
- **Fix:** Hinzufügen oder korrigieren Sie die erforderlichen #if und im Code definiert.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: IPADebugBuildsRule

- **Problem:** Debug-Konfiguration, die Entwickler Signaturschlüssel verwendet sollte keine IPA-Datei generieren, Bedarf nur für die Verteilung, die jetzt den Veröffentlichen-Assistenten verwendet.
- **Fix:** Deaktivieren Sie die Erstellung der IPA-Datei in den Projektoptionen für die Debugkonfiguration.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **Problem:** Der unterstützte Architektur für die "release | Gerät"wird nicht auf 64-Bit-kompatibel, ARM64 fehlt. Dies ist ein Problem, da es sich bei Apple 32 Bits nur iOS-apps in den App Store nicht akzeptiert.
- **Fix:** Doppelklicken Sie auf Ihrem iOS-Projekt, wechseln Sie zum Erstellen > iOS zu erstellen und ändern Sie die unterstützten Architekturen, es gelten somit ARM64.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **Problem:** Nicht mit der float32-Option (--Aot-Options = O = float32) führt zu einer Supportdateien Leistungseinbußen, insbesondere für Mobile, in denen double Precision mathematische merklich langsamer ist. Beachten Sie, dass .NET mit doppelten Genauigkeit intern, auch wenn "float", also durch Aktivieren dieser Option wirkt sich auf Genauigkeit und möglicherweise Kompatibilität.
- **Fix:** Doppelklicken Sie auf Ihrem iOS-Projekt, wechseln Sie zum Erstellen > iOS zu erstellen, und deaktivieren Sie die "Alle 32-Bit-Gleitkommaoperationen als 64-Bit-Gleitkommaoperationen ausführen".

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: HttpClientAvoidManaged

- **Problem:** Es wird empfohlen, den Handler für native "HttpClient" anstelle des verwalteten zur Verbesserung der Leistung kleiner Größe der ausführbaren Datei, und um einfach den neueren Standards zu unterstützen.
- **Fix:** Doppelklicken Sie auf Ihrem iOS-Projekt, wechseln Sie zum Erstellen > iOS erstellen und ändern Sie die "HttpClient"-Implementierung von NSUrlSession (iOS 7 und höher) oder CFNetwork Version vor iOS 7 unterstützen.
