---
title: Einführung in watchOS 5
description: Dieses Dokument bietet einen Überblick über watchos und beschreibt den Anwendungslebenszyklus, Benutzeroberflächen Typen, Bildschirmgrößen, Einschränkungen und vieles mehr.
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 1b71ff60ea0e23ce9d631286aec624a84f163ce5
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937499"
---
# <a name="introduction-to-watchos"></a>Einführung in watchOS 5

> [!NOTE]
> Unter [Einführung in watchos 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) finden Sie eine Übersicht über die neuesten Features.

## <a name="about-watchos"></a>Informationen zu watchos

Eine watchos-App-Lösung umfasst drei Projekte:

- **Erweiterung ansehen** – ein Projekt, das den Code für die Watch-app enthält.
- **Watch-App** – enthält das Storyboard und die Ressourcen der Benutzeroberfläche.
- über **geordnete IOS-App** – diese APP ist eine normale iPhone-App. Die Watch-APP und-Erweiterung werden in der iPhone-App für die Bereitstellung der Benutzerüberwachung gebündelt.

In watchos 1-apps wird der Code in der Erweiterung auf dem iPhone ausgeführt – das Apple Watch ist effektiv eine externe Anzeige. watchos 2-und 3-apps werden vollständig auf dem Apple Watch ausgeführt. Dieser Unterschied wird in der folgenden Abbildung dargestellt:

[![Der Unterschied zwischen watchos 1 und watchos 2 (und höher) ist in diesem Diagramm dargestellt.](intro-to-watchos-images/arch-sml.png)](intro-to-watchos-images/arch.png#lightbox)

Unabhängig davon, welche watchos-Version als Ziel verwendet wird, wird in Visual Studio für Mac Lösungspad eine vollständige Lösung in etwa wie folgt aussehen:

[![Der Lösungspad](intro-to-watchos-images/projectstructure-sml.png)](intro-to-watchos-images/projectstructure.png#lightbox)

Die über *geordnete App* in einer watchos-Lösung ist eine reguläre IOS-app. Dies ist das einzige Projekt in der Projekt Mappe, das **auf dem Telefon**sichtbar ist. Anwendungsfälle für diese APP enthalten Lernprogramme, administrative Bildschirme und Filter der mittleren Ebene, Zwischenspeicherung usw. Es ist jedoch möglich, dass der Benutzer die Watch-App/-Erweiterung installieren und ausführen kann, ohne die übergeordnete APP **jemals** öffnen zu müssen. Wenn Sie also die übergeordnete App zur einmaligen Initialisierung oder Verwaltung ausführen müssen, müssen Sie Ihre Watch-App/-Erweiterung programmieren, um dem Benutzer mitzuteilen, dass dies der Fall ist.

Obwohl die übergeordnete APP die Watch-APP und-Erweiterung bietet, werden Sie in unterschiedlichen Sand Fächern ausgeführt.

Bei watchos 1 können Sie Daten über eine freigegebene App-Gruppe oder über die statische Funktion freigeben `WKInterfaceController.OpenParentApplication` , die die `UIApplicationDelegate.HandleWatchKitExtensionRequest` Methode in den übergeordneten apps auslöst `AppDelegate` (Weitere Informationen finden Sie unter [Arbeiten mit der übergeordneten App](~/ios/watchos/app-fundamentals/parent-app.md)).

Bei watchos 2 oder höher wird das Watch Connectivity-Framework verwendet, um mithilfe der-Klasse mit der übergeordneten APP zu kommunizieren `WCSession` .

## <a name="application-lifecycle"></a>Anwendungslebenszyklus

In der Watch-Erweiterung wird eine Unterklasse der- `WKInterfaceController` Klasse für jede Storyboard-Szene erstellt.

Diese `WKInterfaceController` Klassen sind analog zu den `UIViewController` Objekten in der IOS-Programmierung, haben aber nicht die gleiche Zugriffsebene für die Ansicht.
Beispielsweise können Sie die Benutzeroberfläche nicht dynamisch hinzufügen oder umstrukturieren.
Sie können jedoch Steuerelemente ausblenden und offenlegen und mit einigen Steuerelementen deren Größe, Transparenz und Darstellungs Optionen ändern.

Der Lebenszyklus eines- `WKInterfaceController` Objekts umfasst die folgenden Aufrufe:

- [Wach](xref:WatchKit.WKInterfaceController.Awake*) : Sie sollten den größten Teil ihrer Initialisierung in dieser Methode ausführen.
- [Willaktivierung](xref:WatchKit.WKInterfaceController.WillActivate) : wird aufgerufen, kurz bevor die Watch-App für den Benutzer angezeigt wird. Verwenden Sie diese Methode, um die letzte Moment Initialisierung auszuführen, Animationen zu starten usw.
- An diesem Punkt wird die Watch-App angezeigt, und die Erweiterung beginnt damit, auf Benutzereingaben zu reagieren und die Anzeige der Watch-App gemäß ihrer Anwendungslogik zu aktualisieren.
- [Diddeaktivieren](xref:WatchKit.WKInterfaceController.DidDeactivate) Nachdem die Watch-APP vom Benutzer verworfen wurde, wird diese Methode aufgerufen. Nachdem diese Methode zurückgegeben wurde, können die Benutzeroberflächen-Steuerelemente erst geändert werden, wenn das nächste Mal `WillActivate` aufgerufen wird. Diese Methode wird auch aufgerufen, wenn die Verbindung mit dem iPhone getrennt ist.
- Nachdem die Erweiterung deaktiviert wurde, kann Ihr Programm nicht mehr auf Sie zugreifen. Ausstehende asynchrone Funktionen werden **nicht** aufgerufen. Für Watch Kit-Erweiterungen werden möglicherweise keine Hintergrund Verarbeitungsmodi verwendet. Wenn das Programm vom Benutzer erneut aktiviert wird, die APP jedoch nicht vom Betriebssystem beendet wurde, ist die erste Methode, die aufgerufen wird `WillActivate` .

![Übersicht über Anwendungslebenszyklus](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png)

## <a name="types-of-user-interface"></a>Typen der Benutzeroberfläche

Es gibt drei Arten der Interaktion, die der Benutzer mit ihrer Watch-APP haben kann.
Alle werden mit benutzerdefinierten Unterklassen von programmiert `WKInterfaceController` , sodass die zuvor erörterte Lebenszyklus Sequenz universell angewendet wird (Benachrichtigungen werden mit Unterklassen von programmiert `WKUserNotificationController` , bei denen es sich um eine Unterklasse von handelt `WKInterfaceController` ):

### <a name="normal-interaction"></a>Normale Interaktion

Die Mehrzahl der Watch-App-/Erweiterungs Interaktionen erfolgt mit Unterklassen von `WKInterfaceController` , die Sie schreiben, um Szenen in der " **Interface. Storyboard**" ihrer Watch-APP zu entsprechen. Dies wird in den Artikeln " [Installation](~/ios/watchos/get-started/installation.md) " und " [Getting Started](~/ios/watchos/get-started/index.md) " ausführlich beschrieben.
Die folgende Abbildung zeigt einen Teil des Storyboards für das [Watch Kit-Katalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) Beispiel. Für jede hier angezeigte Szene gibt es ein entsprechendes benutzerdefiniertes `WKInterfaceController` ( `LabelDetailController` ,, `ButtonDetailController` `SwitchDetailController` usw.) im Erweiterungsprojekt.

![Beispiele für normale Interaktion](intro-to-watchos-images/scenes.png)

### <a name="notifications"></a>Benachrichtigungen

[Benachrichtigungen](~/ios/watchos/platform/notifications.md) sind ein wichtiger Anwendungsfall für die Apple Watch. Lokale Benachrichtigungen und Remote Benachrichtigungen werden unterstützt. Die Interaktion mit Benachrichtigungen erfolgt in zwei Phasen, die kurz und lang aussehen.

Kurze suchen werden kurz angezeigt und zeigen das Symbol für die Watch-APP, den Namen und den Titel an (wie in angegeben `WKInterfaceController.SetTitle` ).

Mit Long Look werden ein vom System bereitgestellter **Schärpe** -Bereich und eine Schaltfläche zum verwerfen mit Ihrem benutzerdefinierten storyboardbasierten Inhalt kombiniert.

`WKUserNotificationInterfaceController`wird `WKInterfaceController` mit den Methoden `DidReceiveLocalNotification` und erweitert `DidReceiveRemoteNotification` .
Überschreiben Sie diese Methoden, um auf Benachrichtigungs Ereignisse zu reagieren.

Weitere Informationen zum Design der Benutzeroberfläche für die Benutzeroberfläche finden Sie in den [Apple Watch Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![Beispiel Benachrichtigungen](intro-to-watchos-images/notifications.png)

## <a name="screen-sizes"></a>Bildschirmgrößen

Die Apple Watch hat zwei Gesichts Größen: 38mm und 42mm, beides mit einem 5:4-Anzeige Verhältnis und eine Retina-Anzeige. Die verwendbaren Größen sind:

- 38mm: 136 x 170 logische Pixel (272 x 340 physische Pixel)
- 42 mm: 156 x 195 logische Pixel (312 x 390 physische Pixel).

Verwenden `WKInterfaceDevice.ScreenBounds` Sie, um zu ermitteln, auf welcher Anzeige Ihre Watch-app ausgeführt wird.

Im Allgemeinen ist es einfacher, Ihren Text-und Layoutentwurf mit der eingeschränkten 38mm-Anzeige zu entwickeln und dann zentral hochzuskalieren.
Wenn Sie mit der größeren Umgebung beginnen, kann das horizontale Herunterskalieren zu hässlichen Überlappungen oder zum Abschneiden von Text führen.

Weitere Informationen finden Sie [unter Arbeiten mit Bildschirmgrößen](~/ios/watchos/app-fundamentals/screen-sizes.md).

## <a name="limitations-of-watchos"></a>Einschränkungen von watchos

Es gibt einige Einschränkungen von watchos, die bei der Entwicklung von watchos-apps zu beachten sind:

- Apple Watch Geräten begrenzten Speicherplatz aufweisen, bevor Sie große Dateien herunterladen (z. b. Audiodateien oder Filmdateien).

- Viele watchos-Steuerelemente verfügen über analoge [Elemente](~/ios/watchos/user-interface/index.md) in UIKit, sind jedoch unterschiedliche Klassen ( `WKInterfaceButton` anstelle `UIButton` `WKInterfaceSwitch` von, for `UISwitch` usw.) und verfügen im Vergleich zu ihren UIKit-Entsprechungen über einen begrenzten Satz von Methoden. Außerdem verfügt watchos über einige Steuerelemente, wie z `WKInterfaceDate` . b. (zum Anzeigen eines Datums und einer Uhrzeit), die nicht von UIKit verwendet werden.

  - Es ist nicht möglich, Benachrichtigungen nur an die Überwachung oder das iPhone weiterzuleiten (welche Art von Kontrolle hat der Benutzer über das Routing nicht angekündigt).

Einige andere bekannte Einschränkungen/häufig gestellte Fragen:

- Apple lässt keine benutzerdefinierten Überwachungs Gesichter von Drittanbietern zu.

- Die APIs, die es ermöglichen, iTunes auf dem verbundenen Telefon zu steuern, sind privat.

## <a name="further-reading"></a>Weitere Informationen

Sehen Sie sich die Dokumentation von Apple an:

- [Entwickeln für Watch Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

- [Programmier Handbuch für Watch Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

- [Apple Watch Richtlinien für die Benutzeroberfläche](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)

## <a name="related-links"></a>Verwandte Links

- [watchos 3-Katalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [watchos 1-Katalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Setup und Installation](~/ios/watchos/get-started/installation.md)
- [Video zur ersten Watch-App](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Leitfaden zum entwickeln für Watch Kit von Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Watchkit-Tipps von Apple](https://developer.apple.com/watchkit/tips/)
- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
