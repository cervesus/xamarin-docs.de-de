---
title: Xamarin. IOS-Analyse Regeln
description: In diesem Dokument wird ein Satz von Analyse Regeln beschrieben, mit denen xamarin. IOS-Projekteinstellungen überprüft werden, um zu bestimmen, ob mehr/besser optimierte Einstellungen verfügbar sind.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2018
ms.openlocfilehash: c0f40ea6fc7d429867f90d3d3c1b49dacb63acb5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030865"
---
# <a name="xamarinios-analysis-rules"></a>Xamarin. IOS-Analyse Regeln

Bei der xamarin. IOS-Analyse handelt es sich um einen Satz von Regeln, mit denen Sie Ihre Projekteinstellungen überprüfen, um festzustellen, ob bessere/optimierte Einstellungen verfügbar sind.

Führen Sie die Analyse Regeln so oft wie möglich aus, um mögliche Verbesserungen frühzeitig zu finden und Entwicklungszeit zu sparen.

Um die Regeln auszuführen, klicken Sie im Menü Visual Studio für Mac auf **Projekt > Code Analyse ausführen**.

> [!NOTE]
> Die xamarin. IOS-Analyse wird nur für die aktuell ausgewählte Konfiguration ausgeführt. Es wird dringend empfohlen, das Tool für Debug **-und** Releasekonfigurationen ausführen.

<a name="XIA0001" />

## <a name="xia0001-disabledlinkerrule"></a>XIA0001: disabledlinkerrule

- **Problem:** Der Linker ist auf dem Gerät für den Debugmodus deaktiviert.
- **Behebung:** Sie sollten versuchen, Ihren Code mit dem Linker auszuführen, um Überraschungen zu vermeiden.
Um dies einzurichten, wechseln Sie zu Projekt > IOS-Build > Linker-Verhalten.

<a name="XIA0002" />

## <a name="xia0002-testcloudagentreleaserule"></a>XIA0002: testcloudagentreleaserule

- **Problem:** App-Builds, die den Test Cloud-Agent initialisieren, werden von Apple bei der Übermittlung abgelehnt, da Sie die private API verwenden.
- **Behebung:** Fügen Sie die erforderliche #if hinzu, und definieren Sie Sie im Code.

<a name="XIA0003" />

## <a name="xia0003-ipadebugbuildsrule"></a>XIA0003: iteibugbuildsrule

- **Problem:** Die Debugkonfiguration, die Entwickler Signatur Schlüssel verwendet, sollte keine IPA-Datei generieren, da Sie nur für die Verteilung erforderlich ist, die jetzt den Veröffentlichungs-Assistenten verwendet.
- **Behebung:** Deaktivieren Sie IPA Build in den Projektoptionen für die Debugkonfiguration.

<a name="XIA0004" />

## <a name="xia0004-missing64bitsupportrule"></a>XIA0004: Missing64BitSupportRule

- **Problem:** Die unterstützte Architektur für "Release | das Gerät "ist nicht 64 Bit kompatibel, ARM64 fehlt. Dies ist ein Problem, da Apple nur IOS-apps mit 32 Bits im AppStore akzeptiert.
- **Behebung:** Doppelklicken Sie auf Ihr IOS-Projekt, navigieren Sie zu Build > IOS-Build, und ändern Sie die unterstützten Architekturen, damit Sie über ARM64 verfügen.

<a name="XIA0005" />

## <a name="xia0005-float32rule"></a>XIA0005: Float32Rule

- **Problem:** Wenn die Option float32 (--AOT-Options =-O = float32) nicht verwendet wird, führt dies zu einem hohen Leistungs Aufwand, insbesondere bei mobilen Geräten, bei denen die mathematische Genauigkeit mit doppelter Genauigkeit measurisch langsamer ist. Beachten Sie, dass .net die doppelte Genauigkeit intern verwendet, sogar für float. das Aktivieren dieser Option wirkt sich daher auf die Genauigkeit und möglicherweise auf Kompatibilität aus.
- **Behebung:** Doppelklicken Sie auf Ihr IOS-Projekt, navigieren Sie zu Build > IOS-Build, und deaktivieren Sie die Option "alle 32-Bit-Float-Vorgänge als 64 Bit Float" ausführen.

<a name="XIA0006" />

## <a name="xia0006-httpclientavoidmanaged"></a>XIA0006: httpclientvermeidmanaged

- **Problem:** Wir empfehlen die Verwendung des systemeigenen httpclient-Handlers anstelle des verwalteten httpclient-Handlers für eine bessere Leistung, eine geringere ausführbare Größe und eine einfache Unterstützung neuer Standards.
- **Behebung:** Doppelklicken Sie auf Ihr IOS-Projekt, navigieren Sie zu Build > IOS-Build, und ändern Sie die HttpClient-Implementierung in nsurlsession (IOS 7 +) oder CFNetwork, um die Version vor IOS 7 zu unterstützen.

<a name="XIA0007" />

## <a name="xia0007-usellvmrule"></a>XIA0007: "".

- **Problem:** Für Release | iPhone-Konfiguration empfiehlt es sich, den llvm-Compiler zu aktivieren, der Code generiert, der auf Kosten der Buildzeit schneller ausgeführt werden kann.
- **Behebung:** Doppelklicken Sie auf Ihr IOS-Projekt, navigieren Sie zu Build > IOS-Build, und aktivieren Sie für Release | iPhone die Option llvm-Optimierungs Compiler.
