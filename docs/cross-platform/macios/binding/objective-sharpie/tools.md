---
title: Objektive Sharpie Tools und Befehle
description: Dieses Dokument enthält eine Übersicht über die Tools, die in Objective Sharpie und die Befehlszeilenargumente für die Verwendung mit diesen enthalten.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 51a0b81204b743824e24cfed83bd73308fa8d506
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977661"
---
# <a name="objective-sharpie-tools--commands"></a>Objektive Sharpie Tools und Befehle

_Übersicht über die Tools von Ziel Sharpie, und die Befehlszeilenargumente zu verwenden._

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


Sobald der erfolgreiche Objective Sharpie ist [installiert](~/cross-platform/macios/binding/objective-sharpie/get-started.md), öffnen Sie ein Terminal, und informieren Sie sich über die <em>Befehle</em> Ziel Sharpie zu bieten hat:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Objektive Sharpie bietet die folgenden Tools:

|Tool|Beschreibung|
|--- |--- |
|**xcode**|Enthält Informationen zu aktuellen Xcode-Installation sowie die Versionen von iOS und Mac-SDKs, die verfügbar sind. Wir werden diese Informationen später verwenden werden, wenn wir unsere Bindungen generieren.|
|**pod**|Sucht nach, konfiguriert, installiert wird (in einem lokalen Verzeichnis), und Binden von Objective-C- [CocoaPod](https://cocoapods.org/) Bibliotheken, die aus der master-Spec-Repository verfügbar sind. Dieses Tool auswertet, um die Übergabe an die richtige Eingabe automatisch hergeleitet werden die installierten CocoaPod der `bind` der folgenden Tools. Neues in 3.0!|
|**bind**|Analysiert die Header-Dateien (`*.h`) in der Objective-C-Bibliothek, in dem ersten [ApiDefinition.cs und StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) Dateien.|
|**update**|Überprüft, ob neuere Versionen des Ziel-Sharpie und downloads, und startet das Installationsprogramm aus, sofern verfügbar.|
|**verify-docs**|Zeigt detaillierte Informationen zu `[Verify]` Attribute.|
|**docs**|Navigiert zu diesem Dokument in Ihrem Standardwebbrowser.|

Geben Sie den Namen des Tools, um Hilfe für ein bestimmtes Ziel Sharpie-Tool zu erhalten, und die `-help` Option. Z. B. `sharpie xcode -help` gibt die folgende Ausgabe zurück:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Bevor wir den Bindungsprozess beginnen können, müssen wir Informationen über unsere aktuellen installierten SDKs erhalten Sie mithilfe des folgenden Befehls in das Terminal `sharpie xcode -sdks`. Die Ausgabe kann abweichen, je nachdem, welche Versionen von Xcode, die Sie installiert haben. Objektive Sharpie sucht in allen installierten SDKs `Xcode*.app` unter der `/Applications` Verzeichnis:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Von oben sehen wir, dass wir haben die `iphoneos9.1` SDK auf dem Computer installiert und kann `arm64` Architektur unterstützt. Wir werden diesen Wert für alle Beispiele in diesem Abschnitt verwenden. Mit diesen Informationen vorhanden, wir möchten Sie ein Objective-C-Bibliothek-Header-Dateien in den ersten zu analysieren sind `ApiDefinition.cs` und `StructsAndEnums.cs` für das Projekt für die Bindung.
