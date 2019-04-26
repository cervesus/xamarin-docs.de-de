---
title: Hintergrundverarbeitung in Xamarin.iOS
description: Hintergrund zu verarbeiten oder hintergrundverarbeitung ist der Prozess dadurch, dass Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: a4f5112b6e77ab6e00453c19c766d1e905df1144
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946656"
---
# <a name="backgrounding-in-xamarinios"></a>Hintergrundverarbeitung in Xamarin.iOS

_Hintergrund zu verarbeiten oder hintergrundverarbeitung ist der Prozess dadurch, dass Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund._

Hintergrundverarbeitung in mobilen Anwendungen unterscheidet sich grundlegend, als das herkömmliche Konzept des Multitaskings auf dem Desktop. Desktop-Computer haben eine Vielzahl von Ressourcen, die für eine Anwendung, einschließlich der Platz auf dem Bildschirm, Leistung und Arbeitsspeicher verfügbar. Anwendungen sind, können Sie zum Ausführen von Seite-an-Seite, und bleiben leistungsstarke und verwendet werden. Auf einem mobilen Gerät sind die Ressourcen sehr viel stärker eingeschränkt. Es ist schwierig, die mehr als eine Anwendung auf einem kleinen Bildschirm angezeigt, und mit mehreren Anwendungen in vollem Tempo, würde den Akku abzuleiten. Hintergrundverarbeitung ist ein konstanter Kompromiss zwischen ermöglicht Anwendungen die Ressourcen zum Ausführen von Hintergrundaufgaben, die sie benötigen, um gut zu funktionieren, und halten die badgewert Anwendung und das Gerät reagiert. IOS und Android haben Bereitstellungen für die hintergrundverarbeitung, aber sie sehr unterschiedlich behandeln.

In iOS hintergrundverarbeitung wird als ein Anwendungszustand erkannt, und apps in den hintergrundzustand entsprechend dem Verhalten der app und der Benutzer verschoben werden. iOS bietet mehrere Optionen zum Verknüpfen einer app im Hintergrund, einschließlich fordert das Betriebssystem für die Zeit zum Abschließen der einer wichtigen Aufgabe, die als einer Art von bekannten Hintergrund-Bedarf-Anwendung, ausführen und Aktualisieren von Inhalt von einer Anwendung an Intervalle.

In diesem Handbuch und die zugehörigen Exemplarische Vorgehensweisen werden wir erfahren, wie Sie Aufgaben für die Anwendung im Hintergrund auszuführen. Wir behandeln die wichtigsten Konzepte und bewährte Methoden und durchlaufen Sie anschließend beim Erstellen einer realen app, die Speicherort-Updates im Hintergrund erhält.

## <a name="contents"></a>Inhalt

1.  [Einführung in die Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Demo zum Anwendungslebenszyklus](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Techniken für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Exemplarische Vorgehensweisen: Hintergrundverarbeitung in iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Leitfaden für die iOS-Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch werden die verschiedenen Methoden zum Ausführen der hintergrundverarbeitung in iOS vorgestellt. Wir iOS-Anwendung-Zustände behandelt und die Rolle, die hintergrundverarbeitung spielt in der iOS-Lebenszyklus der Anwendung überprüft. Darüber hinaus haben Sie erfahren, wie wir einzelne Vorgänge oder ganze Anwendungen für die Ausführung im Hintergrund in iOS registriert werden konnte. Zum Schluss gestärkt wir unser Verständnis von der hintergrundverarbeitung in iOS durch Erstellen von Anwendungen, die Updates im Hintergrund ausführen.



## <a name="related-links"></a>Verwandte Links

- [Hintergrundverarbeitung unter Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (Beispiel)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Speicherort (Beispiel)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Einfache Hintergrundübertragung (Beispiel)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS-Ausführung im Hintergrund](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
