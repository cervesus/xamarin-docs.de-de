---
title: In Xamarin.iOS backgrounding
description: Im Hintergrund verarbeitet oder backgrounding versteht man ermöglicht Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b22f3ef3276129f7f46c23cc1d06666f151f5ac4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783541"
---
# <a name="backgrounding-in-xamarinios"></a>In Xamarin.iOS backgrounding

_Im Hintergrund verarbeitet oder backgrounding versteht man ermöglicht Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund._

In mobilen Anwendungen backgrounding unterscheidet sich grundlegend als herkömmlichen Konzepts für Multitasking auf dem Desktop. Desktopcomputern bieten eine Vielzahl von Ressourcen, die für eine Anwendung, einschließlich Bildschirmfläche, Leistung und Arbeitsspeicher verfügbar. Anwendungen sind genutzt werden kann, und führen Sie die Seite-an-Seite und leistungsfähiger bleiben können. Auf einem mobilen Gerät sind Ressourcen weitaus beschränkt. Ist es schwierig, die mehr als eine Anwendung auf einem kleinen Bildschirm anzeigen und mit mehreren Anwendungen bei maximaler Geschwindigkeit würde den Akku abzuleiten. Backgrounding ist ein konstanter Kompromiss zwischen ermöglicht Anwendungen die Ressourcen, die Hintergrundaufgaben ausführen, die sie benötigen, um die Leistung erzielen und reaktionsfähig zu halten die foregrounded Anwendung und dem Gerät. IOS und Android haben Bereitstellungen für backgrounding, aber sie sehr unterschiedlich behandeln.

In iOS backgrounding als Anwendungsstatus erkannt wird, und apps werden in und aus den Hintergrund Status abhängig vom Verhalten der app und der Benutzer verschoben. iOS bietet zudem mehrere Optionen für eine app Verkabelung für die Ausführung im Hintergrund, einschließlich der Aufforderung das Betriebssystem für die Zeit zum Abschluss einer wichtigen Aufgabe, die als Typ des bekannten Hintergrund-Bedarf-Anwendung ausgeführt und Aktualisieren von Inhalt von einer Anwendung an Intervalle.

In diesem Handbuch und zugehörigen exemplarischen Vorgehensweisen werden wir erfahren, wie Aufgaben im Hintergrund auszuführen. Wir werden wichtige Konzepte und bewährte Methoden behandelt und durchlaufen Sie anschließend eine realen Anwendung erstellt, die im Hintergrund Speicherort Updates empfängt.

## <a name="contents"></a>Inhalt

1.  [Einführung in die Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Demo zum Anwendungslebenszyklus](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniken für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Exemplarische Vorgehensweisen: Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Leitfaden für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch werden die verschiedenen Methoden zum Ausführen der Verarbeitung im Hintergrund in iOS vorgestellt. Wir abgedeckt iOS-Anwendung Status und die Rolle spielt in der iOS-Anwendungslebenszyklus backgrounding untersucht. Darüber hinaus haben wir gelernt, wie wir einzelne Tasks oder gesamte Anwendungen für den Betrieb im Hintergrund in iOS registriert werden konnte. Schließlich gestärkt wir unser Verständnis von backgrounding auf iOS durch Erstellen von Anwendungen, die Updates im Hintergrund ausführen.



## <a name="related-links"></a>Verwandte Links

- [Backgrounding unter Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (Beispiel)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Speicherort (Beispiel)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Einfache Hintergrundübertragung (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS-Ausführung im Hintergrund](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
