---
title: Backerden in xamarin. IOS
description: Hintergrundverarbeitung oder zurück Setzung ist der Prozess, mit dem Anwendungen Aufgaben im Hintergrund ausführen können, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch bietet eine Einführung in die Hintergrundverarbeitung in ios.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 161fda52002e8bb757db23c9b2a20a6befd132f5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289340"
---
# <a name="backgrounding-in-xamarinios"></a>Backerden in xamarin. IOS

_Hintergrundverarbeitung oder zurück Setzung ist der Prozess, mit dem Anwendungen Aufgaben im Hintergrund ausführen können, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch bietet eine Einführung in die Hintergrundverarbeitung in ios._

Die Rückstellung in mobilen Anwendungen unterscheidet sich grundlegend von dem herkömmlichen Konzept des Multitasking auf dem Desktop. Desktop Computer verfügen über eine Vielzahl von Ressourcen, die für eine Anwendung verfügbar sind, einschließlich Bildschirmimmobilien, Stromversorgung und Arbeitsspeicher. Anwendungen können nebeneinander ausgeführt werden und bleiben leistungsfähig und verwendbar. Auf einem mobilen Gerät sind Ressourcen weitaus eingeschränkter. Es ist schwierig, mehr als eine Anwendung auf einem kleinen Bildschirm anzuzeigen, und das Ausführen mehrerer Anwendungen mit voller Geschwindigkeit würde den Akku entladen. Die hintergrundstellung ist eine Konstante Gefährdung zwischen den Anwendungen, die für die Ausführung der Hintergrundaufgaben erforderlich sind, die für eine gute Ausführung erforderlich sind, und die Reaktions Sicherheit der Vordergrund Anwendung und des Geräts. Sowohl IOS als auch Android verfügen über Vorkehrungen für die Rückstellung, aber Sie werden auf unterschiedliche Weise behandelt.

In IOS wird die hintergrundstellung als Anwendungs Zustand erkannt, und apps werden je nach Verhalten der APP und des Benutzers in den und aus dem Hintergrund Zustand verschoben. IOS bietet auch mehrere Optionen für die Verknüpfung einer App zur Ausführung im Hintergrund, einschließlich der Zeit, die das Betriebssystem zum Durchführen einer wichtigen Aufgabe auffordert, als eine Art von bekannter Hintergrund erforderliche Anwendung und zum Aktualisieren des Inhalts einer Anwendung unter festgelegt ist. Abstände.

In diesem Handbuch und den dazugehörigen exemplarischen Vorgehensweisen erfahren Sie, wie Sie Anwendungsaufgaben im Hintergrund ausführen. Wir werden wichtige Konzepte und bewährte Methoden behandeln und dann die Erstellung einer realen APP schrittweise durchlaufen, die Standort Aktualisierungen im Hintergrund empfängt.

## <a name="contents"></a>Inhalt

1. [Einführung in die Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1. [Demo zum Anwendungslebenszyklus](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1. [Techniken für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1. [Exemplarische Vorgehensweisen: Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1. [Leitfaden für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung haben wir die verschiedenen Methoden zum Durchführen der Hintergrundverarbeitung in ios eingeführt. Wir haben den IOS-Anwendungs Status behandelt und untersucht, welche Rollen Rückstellung im Lebenszyklus der IOS-Anwendung spielt. Außerdem haben wir gelernt, wie Sie einzelne Aufgaben oder gesamte Anwendungen registrieren können, um Sie im Hintergrund in IOS zu betreiben. Schließlich haben wir unser Verständnis von backerden unter IOS verstärkt, indem wir Anwendungen entwickeln, die Aktualisierungen im Hintergrund ausführen.



## <a name="related-links"></a>Verwandte Links

- [Backerden unter Android](~/android/app-fundamentals/services/index.md)
- [Lifecycledemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
- [Speicherort (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/location)
- [Einfache Hintergrund Übertragung (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
- [IOS-Hintergrund Ausführung](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
