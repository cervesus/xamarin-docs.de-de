---
title: Einführung in watchOS 3
description: In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in watchos 3 für xamarin-Entwickler zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: B8ABE1E1-8688-4262-BE66-A16813C2D671
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/07/2017
ms.openlocfilehash: 50278eaa6d3518b8de85685c1faf64eabac4531d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292205"
---
# <a name="introduction-to-watchos-3"></a>Einführung in watchOS 3

_In diesem Artikel werden alle neuen und geänderten APIs und Features vorgestellt, die in watchos 3 für xamarin-Entwickler zur Verfügung stehen._

In diesem Dokument werden die folgenden Themen behandelt:

- [Neues in watchos 3](#Whats-New-in-watchOS-3)
  - [Apple Pay Erweiterungen](#Apple-Pay-Enhancements) bietet Unterstützung für in-App-Zahlungen auf dem Apple Watch.
  - [Hintergrundaufgaben](#Background-Tasks) ermöglichen der APP, Ihre Informationen im Hintergrund zu aktualisieren, damit Sie bereit ist, wenn der Benutzer Sie benötigt.
  - Es wurden [Komplikationen](#Complications-Enhancements) für watchos 3 vorgenommen, die neue Features für die apps bereitstellen.
  - [Neu verfügbare Frameworks](#Newly-Available-Frameworks) haben für die watchos-apps verfügbar gemacht.
  - [Proaktives vorschlagen](#Proactive-Suggestions) ermöglicht der APP, den Benutzern proaktiv Informationen anzuzeigen.
  - An watchos 3 wurden mehrere [Verbesserungen hinsichtlich Sicherheit und Datenschutz](#Security-and-Privacy-Enhancements) vorgenommen.
  - Mithilfe von [Momentaufnahmen und Andocken](#Snapshots-and-Dock) erhalten Benutzer einen schnellen Zugriff auf die APP-watchos-apps.
  - [Benutzer Benachrichtigungen](#User-Notifications) enthalten sowohl lokale als auch Remote Benachrichtigungen für den Benutzer.
  - In watchos 3 wurden mehrere [Verbesserungen der konnektivitätsframeworkerweiterungen](#Watch-Connectivity-Framework-Enhancements) vorgenommen.
  - In watchos 3 wurden mehrere [Verbesserungen des watchkit-Frameworks](#WatchKit-Framework-Enhancements) vorgenommen.
  - Dank der [Verbesserungen](#Workout-App-Enhancements) bei der APP-Steigerung können Sie die Apple Watch-Apps für das Training
- In watchos 3 wurden [zusätzliche frameworkänderungen](#Additional-Framework-Changes) vorgenommen.
- Als [veraltet markierte APIs](#Deprecated-APIs) in watchos 3.

<a name="Whats-New-in-watchOS-3" />

## <a name="whats-new-in-watchos-3"></a>Neues in watchos 3

Apple hat mehrere neue APIs und Dienste in watchos 3 zusammen mit vielen Verbesserungen an vorhandenen Features hinzugefügt, darunter:

<a name="Apple-Pay-Enhancements" />

## <a name="apple-pay-enhancements"></a>Apple Pay Erweiterungen

In watchos 3 wurde das passkit-Framework erweitert, um Unterstützung für sichere, in-App-Zahlungen (sowohl physischer waren als auch Dienste) für die apps, die auf dem Apple Watch ausgeführt werden, zu ermöglichen.

Verwenden Sie die neuen Klassen [pkpaymentauthorizationcontroller](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [pkpaymentauthorizationcontrollerdelegat](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate) , um eine Schnittstelle zu präsentieren und darauf zu reagieren, wo der Benutzer Zahlungsanforderungen autorisieren kann.

Weitere Informationen finden Sie im Leitfaden zur [Apple Pay Erweiterungen](~/ios/watchos/platform/apple-pay.md) .

<a name="Background-Tasks" />

## <a name="background-tasks"></a>Hintergrundaufgaben

watchos 3 führt mehrere Hintergrundaufgaben ein, die eine APP zum Aktualisieren Ihrer Informationen verwenden kann, um sicherzustellen, dass Sie die Inhalte der Benutzer benötigt, bevor Sie Sie öffnen.

Die folgenden neuen Hintergrundaufgaben sind verfügbar:

- Aktualisierung der **Hintergrund-App** : die Aufgabe " [wkapplicationrefresh backgroundtask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) " ermöglicht der APP, ihren Status im Hintergrund zu aktualisieren. Dies schließt in der Regel eine andere Aufgabe ein, z. b. das Herunterladen von neuem Inhalt aus dem Internet mithilfe von [nsurlsession](https://developer.apple.com/reference/foundation/nsurlsession)
- **Aktualisieren von Moment** Aufnahmen von Momentaufnahmen: die [wksnapshotrefreshbackgroundtask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) -Aufgabe ermöglicht der APP, den Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme erstellt, die zum Auffüllen des Andock Werts verwendet wird.
- **Konnektivität bei der Hintergrundüberwachung** : die Aufgabe " [wkwatchconnectivityaktubackgroundtask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) " wird für die APP gestartet, wenn Sie Hintergrunddaten von dem gekoppelten iPhone empfängt.
- **URL-URL-Sitzung** : die [wkurlsessionerfrischendes backgroundtask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) -Aufgabe wird für die APP gestartet, wenn eine Hintergrund Übertragung eine Autorisierung erfordert oder abgeschlossen ist (erfolgreich oder fehlerhaft).

Weitere Informationen finden Sie im Leitfaden für [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) .

<a name="Complications-Enhancements" />

## <a name="complications-enhancements"></a>Erweiterungen der Komplikationen

Komplikationen sind kleine visuelle Elemente, die hilfreiche Informationen auf einen Blick bereitstellen. Je nach ausgewähltem Überwachungs Gesicht hat der Benutzer die Möglichkeit, ein Überwachungs Gesicht mit einer oder mehreren Komplikationen anzupassen.

watchos 3 bietet der APP die Möglichkeit, eine oder mehrere Komplikationen für die Watch-APP zu erstellen, sodass der Benutzer auf einen Blick von einem Überwachungs Gesicht aus auf seine Informationen zugreifen kann.

Außerdem bieten Komplikationen die folgenden Vorteile:

- Der Benutzer kann die app schnell starten, indem er direkt von einem Überwachungs Gesicht aus auf die Komplikation tippt.
- Wenn Sie eine der Komplikationen der APP auf der Seite "überwachen" haben, bewirkt das System, dass die app in einem betriebsbereiten Zustand ist, in dem Sie versucht, die APP im Hintergrund zu starten, Sie im Arbeitsspeicher beibehalten und zusätzliche Zeit zum Aktualisieren zu erhalten.
- Komplikationen sind mindestens 50 pushupdates pro Tag garantiert.
- Wenn die APP Komplikationen umfasst, wird Sie im Apple Watch Gesicht Gallery vorgestellt (Weitere Informationen finden Sie unter [Hinzufügen von Komplikationen](https://developer.apple.com/documentation/clockkit/adding_complications_to_the_gallery) von Apple zur Katalog Dokumentation).

In watchos 3 enthält das clockkit-Framework nun einige neue Vorlagen für zusätzliche große Komplikationen, wie z. b. [clkcomplicationtemplateextralargecolumnstext](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargecolumnstext) und [clkcomplicationtemplateextralargeringimage](https://developer.apple.com/reference/clockkit/clkcomplicationtemplateextralargeringimage). Verwenden Sie zum Erstellen von Lokalisier barem Text außerdem neue Methoden der [clktextprovider](https://developer.apple.com/reference/clockkit/clktextprovider) -Klasse.

Weitere Informationen finden Sie in unserem Handbuch zur [schnellen Interaktion mit watchos 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Newly-Available-Frameworks" />

## <a name="newly-available-frameworks"></a>Neu verfügbare Frameworks

watchos 3 umfasst mehrere vorhandene Apple-Frameworks, die zuvor nicht verfügbar waren, z. b.:

- **Scenekit** : Verwenden Sie scenekit, um 3D-Modelle in die Benutzeroberfläche der Watch-App einzubeziehen, einschließlich der meisten Features, die auf anderen Plattformen wie Beleuchtung, Schattierung, Animation, Physik und Partikelsysteme verfügbar sind. räumliche 3D-Audiodaten, benutzerdefinierte Metal-oder OpenGL-Shader, Kern Bild Filter und physisch-basiertes Material werden nicht unterstützt.
- **Spritekit** : Verwenden Sie spritekit zum Rendering und Animieren von Sprites in der Benutzeroberfläche der APP Watch-APP, einschließlich der meisten Features, die auf anderen Plattformen wie Aktionen, Physik, Beleuchtung und Partikelsystemen zur Verfügung stehen. 3D-Audiodaten, Videowiedergabe und Core-Bild Filter werden nicht unterstützt.
- **AVFoundation** -zur Verwaltung und Wiedergabe von Audiodaten.
- **Cloudkit** : Verschieben von Daten zwischen der Watch-APP und icloud-Containern.
- **Kernaudiodatei** : zum Verwalten von Datentypen für die Darstellung von Audiostreams, komplexen Puffern und Uhrzeitwerten.
- **GameKit** : um soziale Spiele zu erstellen.

<a name="Proactive-Suggestions" />

## <a name="proactive-suggestions"></a>Proaktive Vorschläge

watchos 3 ermöglicht der APP, den Benutzern in den angegebenen Kontexten proaktiv Informationen zur Verfügung zu stellen. Zur Unterstützung dieser Funktion enthält [nsuseractivity](https://developer.apple.com/reference/foundation/nsuseractivity) nun die `MapItem` -Eigenschaft, mit der die APP Standortinformationen zur späteren Verwendung durch andere apps bereitstellen kann.

Weitere Informationen finden Sie im Leitfaden [Einführung in proaktives vorschlagen](~/ios/watchos/platform/proactive-suggestions.md) .

<a name="Security-and-Privacy-Enhancements" />

## <a name="security-and-privacy-enhancements"></a>Erweiterungen für Sicherheit und Datenschutz

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in watchos 3 vorgenommen, die dem Entwickler dabei helfen, die Sicherheit Ihrer apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten.

Folglich müssen apps, die auf watchos 3 (oder höher) ausgeführt werden, ihre Absicht, auf bestimmte Features oder Benutzerinformationen zuzugreifen, statisch deklarieren, indem Sie einen oder mehrere Daten `Info.plist` Schutz spezifische Schlüssel in Ihren Dateien eingeben, die dem Benutzer erklären, warum die APP Zugriff erhalten möchte.

Da watchos 3 diese Änderungen mit IOS 10 gemeinsam nutzt, finden Sie weitere Informationen im Leitfaden zu den [Sicherheits-und Datenschutz Erweiterungen](~/ios/app-fundamentals/security-privacy.md) für IOS 10.

<a name="Snapshots-and-Dock" />

## <a name="snapshots-and-dock"></a>Momentaufnahmen und Dock

In watchos 3 hat Apple das Dock hinzugefügt, mit dem Benutzer Ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer die Seiten Schaltfläche auf dem Apple Watch drückt, wird ein Katalog mit angehefteten App-Momentaufnahmen angezeigt. Der Benutzer kann nach links oder rechts schwenken, um die gewünschte APP zu suchen, und dann auf die APP tippen, um die Momentaufnahme durch die Benutzeroberfläche der laufenden app zu ersetzen.

Das System erstellt in regelmäßigen Abständen Momentaufnahmen der App-Benutzeroberfläche und verwendet diese Momentaufnahmen, um die Dokumente aufzufüllen. watchos bietet der APP die Möglichkeit, den Inhalt und die Benutzeroberfläche zu aktualisieren, bevor die Momentaufnahme erstellt wird.

Weitere Informationen finden Sie in unserem Handbuch für [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) und in der [wksnapshotrefreshbackgroundtask-Referenz](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) von Apple.

<a name="User-Notifications" />

## <a name="user-notifications"></a>Benutzer Benachrichtigungen

Das in watchos 3 eingeführte Benutzer Benachrichtigungs Framework unterstützt die Übermittlung von lokalen Benachrichtigungen und Remote Benachrichtigungen an den Apple Watch. Verwenden Sie dieses Framework, um Benachrichtigungen auf der Grundlage bestimmter Bedingungen zu planen, z. b. Uhrzeit oder Standort, und Benachrichtigungen zu empfangen und zu verarbeiten.

Weitere Informationen finden Sie in unserem Handbuch zur [schnellen Interaktion mit watchos 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Watch-Connectivity-Framework-Enhancements" />

## <a name="watch-connectivity-framework-enhancements"></a>Verbesserungen des Konnektivitäts

Die neue `HasContentPending` -Eigenschaft der [wcsession](https://developer.apple.com/reference/watchconnectivity/wcsession) -Klasse gibt an, dass die Sitzung Daten im Hintergrund empfangen hat, die verarbeitet werden müssen. Und die `RemainingComplicationUserInfoTransfers` -Eigenschaft gibt die restlichen Zeiten zurück, in denen die IOS-app Ihre watchos-Komplikation aktualisieren kann.

Weitere Informationen finden Sie im Leitfaden für [Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md) .

<a name="WatchKit-Framework-Enhancements" />

## <a name="watchkit-framework-enhancements"></a>Erweiterungen des watchkit-Frameworks

watchos 3 umfasst mehrere Verbesserungen des watchkit-Frameworks, einschließlich der folgenden:

- Die APP kann den Status der Digital Crown mithilfe der neuen [wkcrownsequencer](https://developer.apple.com/reference/watchkit/wkcrownsequencer) -Klasse abrufen und Updates erhalten, wenn der Benutzer die Krone mithilfe der [wkcrowndelegatklasse](https://developer.apple.com/reference/watchkit/wkcrowndelegate) rotiert.
- Die [wkextension](https://developer.apple.com/reference/watchkit/wkextension) -Klasse enthält nun `ApplicationState` die-Methode und die [wkapplicationstate](https://developer.apple.com/reference/watchkit/wkapplicationstate) -Konstante, mit der die APP den Lauf Zeit Status der APP verfolgen kann. `WKExtension`stellt außerdem zwei neue Methoden bereit, die zum Planen von Hintergrundaufgaben verwendet werden können.
- Der [wkextensiondelegat](https://developer.apple.com/reference/watchkit/wkextensiondelegate) enthält jetzt die `ApplicationWillEnterForeground`neuen `ApplicationDidEnterBackground` Methoden `HandleBackgroundTasks` und, um Änderungen am Zustand der APP zu überwachen und Aktualisierungen der Hintergrundaufgabe zu verarbeiten.
- Eine neue [wkgesturerecognizer](https://developer.apple.com/reference/watchkit/wkgesturerecognizer) -Klasse wurde hinzugefügt, um die folgenden Arten von Gestenerkennung für die Watch-apps bereitzustellen: [Wklongpressgesturerecognizer](https://developer.apple.com/reference/watchkit/wklongpressgesturerecognizer), [wkpangesturerecognizer](https://developer.apple.com/reference/watchkit/wkpangesturerecognizer), [wkswipgesturerecognizer](https://developer.apple.com/reference/watchkit/wkswipegesturerecognizer) und [wktapgesturerecognizer](https://developer.apple.com/reference/watchkit/wktapgesturerecognizer).
- Die neue [wkinterfacehmcamera](https://developer.apple.com/reference/watchkit/wkinterfacehmcamera) -Klasse stellt eine Schnittstelle für alle angeschlossenen homekit-IP-Kameras bereit.
- Die neue [wkinterfaceinlinemovie](https://developer.apple.com/reference/watchkit/wkinterfaceinlinemovie) -Klasse ermöglicht der APP, ein Movie-"Poster" anzuzeigen, das durch den laufenden Film ersetzt wird, wenn der Benutzer darauf tippt.
- Die neue [wkinterfacepaymentbutton](https://developer.apple.com/reference/watchkit/wkinterfacepaymentbutton) -Klasse ermöglicht der APP, auf der Benutzeroberfläche eine Apple Pay Schaltfläche darzustellen, die eine Zahlungs Anforderung initiiert, wenn Sie getippt wird.
- Die neue [wkinterfacescnscene](https://developer.apple.com/reference/watchkit/wkinterfacescnscene) -Klasse stellt eine Schnittstelle zum Anzeigen einer scenekit-Szene auf der Apple Watch dar.
- Die neue [wkinterfaceskscene](https://developer.apple.com/reference/watchkit/wkinterfaceskscene) -Klasse stellt eine Schnittstelle zum Anzeigen einer spritekit-Szene auf dem Apple Watch dar.

Weitere Informationen finden Sie in unserem Handbuch zur [schnellen Interaktion mit watchos 3](~/ios/watchos/platform/quick-interaction-techniques.md) .

<a name="Workout-App-Enhancements" />

## <a name="workout-app-enhancements"></a>Trainings-App-Erweiterungen

Neu bei watchos 3: mit dem Training verbundene Apps können im Hintergrund des Apple Watch ausgeführt werden. Um dieses Feature zu aktivieren (und Zugriff auf healthkit-Daten zu erhalten), muss die `WKBackgroundModes` App den Schlüssel `Info.plist` in die Datei mit `workout-processing`dem Wert einschließen.

Außerdem bietet der Entwickler jetzt die Möglichkeit, die watchos-Trainings-App aus der IOS-App-Version auf dem gekoppelten iPhone zu starten.

Weitere Informationen finden Sie im Handbuch zu den [Verbesserungen der Trainings-App](~/ios/watchos/platform/workout-apps.md) .

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>Weitere frameworkänderungen

Zusätzlich zu den oben aufgeführten wichtigen frameworkänderungen und Ergänzungen hat Apple viele zusätzliche Änderungen am geringfügigen Framework in watchos 3 vorgenommen.

Weitere Informationen finden Sie in unserem Handbuch zu [zusätzlichen Framework-Änderungen](~/ios/watchos/platform/introduction-to-watchos3/additional-framework-changes.md) .

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>Nicht mehr unterstützte APIs

Die folgenden APIs sind in watchos 3 veraltet:

- Die `UILocalNotification` Klasse von UIKit wurde als veraltet markiert und sollte durch das Benutzer Benachrichtigungs Framework ersetzt werden.

Eine umfassende Liste mit veralteten Vorgängen und Änderungen finden Sie in der Dokumentation zu den Apple- [watchos 2,2 zur watchos 3,0 API-Unterschiede](https://developer.apple.com/library/prerelease/content/releasenotes/General/watchOS30APIDiffs/index.html) .


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [Neues in watchos 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
