---
title: Bereitstellen und Testen von Xamarin.iOS-Apps
description: Dieser Artikel enthält Links zu verschiedenen Leitfäden, in denen Themen zum Bereitstellen und Testen einer Xamarin.iOS-Anwendung beschrieben werden. Zum Beispiel die App-Verteilung, IPA-Dateien, Bereitstellung, drahtlose Bereitstellung, TestFlight und das Debuggen.
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 29a1134ebe25f0ce1f25f2c41bf28d4c60f8fa6a
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865782"
---
# <a name="deploying-and-testing-xamarinios-apps"></a>Bereitstellen und Testen von Xamarin.iOS-Apps

Dieser Abschnitt enthält Informationen zum Testen und Verteilen einer Anwendung. In den hier aufgeführten Themen werden z.B. Tools zum Debuggen, die Bereitstellung für Tester sowie das Veröffentlichen einer Anwendung auf Google Play erläutert.

## <a name="app-distributioniosdeploy-testapp-distributionindexmd"></a>[App-Verteilung](~/ios/deploy-test/app-distribution/index.md)

In diesem Artikel wird beschrieben, wie Sie eine Xamarin.iOS-Anwendung mit unterschiedlichen Mitteln für die Verteilung konfigurieren, erstellen und veröffentlichen. Hierbei stehen Ihnen z.B. folgende Möglichkeiten zur Verfügung:

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Interne Verteilung (Enterprise-Verteilung)](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

## <a name="ipa-deploymentiosdeploy-testapp-distributionipa-supportmd"></a>[Bereitstellung mit IPA](~/ios/deploy-test/app-distribution/ipa-support.md)

Ad-hoc- und Enterprise-Bereitstellungen ermöglichen Entwicklern das Erstellen von Paketen, die zu Testzwecken oder an interne Unternehmensbenutzer verteilt werden können. In diesem Leitfaden wird erläutert, wie Sie eine IPA-Datei erstellen, die mit einem iOS-Gerät über iTunes synchronisiert werden kann.

## <a name="provisioningprovisioningindexmd"></a>[Bereitstellung](provisioning/index.md)

Diese Reihe von Handbüchern behandelt Grundlagen zu Signatur und Bereitstellung, beispielsweise die Arbeit mit Eigenschaftenlisten und das Bereitstellen Ihrer App für Anwendungsdienste. 

## <a name="wireless-deploymentwireless-deploymentmd"></a>[Drahtlose Bereitstellung](wireless-deployment.md)

 In Xcode 9 wurde die Option zur Bereitstellung für ein iOS-Gerät oder Apple TV über ein Netzwerk eingeführt, sodass Sie die Geräte nicht mehr per Kabel anschließen müssen, wenn Sie Ihre App bereitstellen und debuggen möchten. Dieses Feature befindet sich derzeit in der Vorschau.

## <a name="testflightiosdeploy-testtestflightmd"></a>[TestFlight](~/ios/deploy-test/testflight.md)

TestFlight ist jetzt im Besitz von Apple und die wichtigste Methode zum Betatesten Ihrer Xamarin.iOS-Apps. Dieser Artikel führt Sie durch alle Schritte des TestFlight-Prozesses, vom Hochladen Ihrer App bis hin zum Arbeiten mit iTunes Connect.

## <a name="debugging-in-xamariniosiosdeploy-testdebugging-in-xamarin-iosmd"></a>[Debuggen in Xamarin.iOS](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Die integrierten Entwicklungsumgebungen Visual Studio und Visual Studio für Mac unterstützen das Debuggen von Xamarin.iOS-Anwendungen sowohl im iOS-Simulator als auch auf iOS-Geräten. In diesem Artikel wird beschrieben, wie Sie den Debugger verwenden und verschiedene Optionen konfigurieren, die von diesem unterstützt werden.

## <a name="touchunitiosdeploy-testtouchunitmd"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

Dieses Dokument beschreibt, wie Sie Komponententests für Ihre Xamarin.iOS-Projekte erstellen.
Komponententests mit Xamarin.iOS werden mithilfe des Touch.Unit-Frameworks durchgeführt, das sowohl einen iOS Test Runner als auch eine geänderte Version des [NUnitLite](http://www.nunitlite.com/)-Frameworks enthält, das eine Reihe vertrauter APIs zum Schreiben von Komponententests bietet.

## <a name="using-instruments-to-detect-native-leaks-using-markheapiosdeploy-testusing-instruments-to-detect-native-leaks-using-markheapmd"></a>[Verwenden von Instruments zum Erkennen von nativen Speicherverlusten mit MarkHeap](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

In diesem Artikel wird beschrieben, wie Sie Instruments für ein iOS-Gerät und eine Xamarin.iOS-Anwendung verwenden. Des Weiteren wird das Profilen von Anwendungen im Simulator erläutert.

## <a name="walkthrough---using-apples-instrument-tooliosdeploy-testwalkthrough-apples-instrumentmd"></a>[Verwenden des Apple-Tools Instruments – Erste Schritte](~/ios/deploy-test/walkthrough-apples-instrument.md)

Dieser Artikel erläutert die Verwendung des Apple-Tools Instruments, um Speicherprobleme in einer iOS-Anwendung zu diagnostizieren, die mit Xamarin erstellt wurde. Es wird gezeigt, wie Instruments gestartet wird, wie Heap-Momentaufnahmen erfasst werden und wie der Anstieg des benötigten Speichers analysiert wird. Ebenfalls wird beschrieben, wie Instruments dafür verwendet wird, die genaue Codezeile zu ermitteln und anzuzeigen, die das Speicherproblem verursacht.

## <a name="linking-on-ioslinkermd"></a>[Verknüpfung unter iOS](linker.md)

Erklärt, wie der Linker arbeitet, um das kleinstmögliche Anwendungspaket sicherzustellen, und wie dessen Einstellungen und Verbrauch angepasst werden.

## <a name="xamarinios-performanceperformancemd"></a>[Xamarin.iOS-Leistung](performance.md)

Es gibt viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.iOS erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.

## <a name="mtouchmtouchmd"></a>[mtouch](mtouch.md)

Gibt Hinweise und Informationen zu „mtouch.exe“, dem Befehlszeilentool, das Ihr Projekt in eine von iOS nutzbare Anwendung kompiliert.

## <a name="ios-build-mechanicsios-build-mechanicsmd"></a>[iOS Build Mechanics (Abläufe beim Erstellen von iOS-Builds)](ios-build-mechanics.md)

In diesem Leitfaden erfahren Sie, wie Sie Ihre Anwendungen zeitlich steuern und wie Sie für alle Buildkonfigurationen Methoden für schnellere Builds verwenden können.
