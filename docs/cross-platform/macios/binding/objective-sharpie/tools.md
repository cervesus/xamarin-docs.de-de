---
title: Tools und Befehle
description: Übersicht über die Tabellenanalysetools mit Ziel Sharpie und die Befehlszeilenargumente zu verwenden.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 8a307739134fe3b76692fbef5c1dc028af01017d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="tools--commands"></a>Tools und Befehle

_Übersicht über die Tabellenanalysetools mit Ziel Sharpie und die Befehlszeilenargumente zu verwenden._

<style type="text/css"> .Terminal Blau {Color: rgb(10,96,254);} .terminal Grün {Color: rgb(12,156,26);} .terminal Magenta {Farbe: rgb(152,12,103);} </style>


Sobald Ziel Sharpie erfolgreich ist [installiert](~/cross-platform/macios/binding/objective-sharpie/get-started.md), öffnen Sie ein Terminal und machen Sie sich mit den <em>Befehle</em> Ziel Sharpie bieten hat:

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

Objektive Sharpie stellt die folgenden Tools:

|Tool|Beschreibung|
|--- |--- |
|**xcode**|Informationen zum aktuellen Xcode-Installation sowie die Versionen von iOS und Mac-SDKs, die verfügbar sind. Wir werden diese Informationen später verwenden werden, wenn wir unsere Bindungen generieren.|
|**pod**|Sucht nach, konfiguriert, installiert (in einem lokalen Verzeichnis), und bindet Objective-C [CocoaPod](https://cocoapods.org/) Bibliotheken, die aus der master-Spec-Repository verfügbar sind. Dieses Tool wertet die installierten CocoaPod zur Übergabe an die richtige Eingabe automatisch Ableiten der `bind` Tool unten. In 3.0 neu.|
|**bind**|Analysiert die Headerdateien (`*.h`) in der Bibliothek für Objective-C in der ersten [ApiDefinition.cs und StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) Dateien.|
|**update**|Überprüft, ob neuere Versionen des Ziel-Sharpie und heruntergeladen und das Installationsprogramm wird gestartet, wenn ein solcher verfügbar ist.|
|**verify-docs**|Zeigt detaillierte Informationen zu `[Verify]` Attribute.|
|**docs**|Navigiert zu diesem Dokument in Ihrem Standard-Webbrowser.|

Um Hilfe für ein bestimmtes Ziel Sharpie-Tool zu erhalten, geben Sie den Namen des Tools und die `-help` Option. Beispielsweise `sharpie xcode -help` gibt die folgende Ausgabe:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Bevor wir den Bindungsprozess beginnen können, müssen wir erhalten Informationen zu unseren aktuellen installierten SDKs durch Eingabe des folgenden Befehls in die Terminal `sharpie xcode -sdks`. Die Ausgabe möglicherweise unterscheiden sich je nachdem, welche Versionen von Xcode Sie installiert haben. Objektive Sharpie sucht in einem installierten SDKs `Xcode*.app` unter der `/Applications` Verzeichnis:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Aus den oben genannten können wir sehen, dass wir haben die `iphoneos9.1` SDK auf unseren Machine installiert und kann `arm64` Architektur unterstützt. Dieser Wert wird für alle Beispiele in diesem Abschnitt verwendet werden. Diese Informationen sind wir bereit, zum Analysieren der Headerdateien ein Objective-C-Bibliothek in der ersten `ApiDefinition.cs` und `StructsAndEnums.cs` für das Projekt für die Bindung.

