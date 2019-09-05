---
title: Grundlagen der xamarin. IOS-Anwendung
description: In diesem Dokument finden Sie Links zu verschiedenen Leitfäden, in denen die Konzepte für die xamarin. IOS-Entwicklung beschrieben werden, wie z. b. die APP-Transportsicherheit, die Sicherung, Ereignisse und Threading
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/21/2017
ms.openlocfilehash: 59257dafc1d92756feb85046df43de7b9da0cc42
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290143"
---
# <a name="xamarinios-application-fundamentals"></a>Grundlagen der xamarin. IOS-Anwendung

Dieser Abschnitt enthält eine Anleitung für einige der gängigeren Aufgaben und Konzepte, die Entwickler bei der Entwicklung von xamarin. IOS-Anwendungen (früher MonoTouch) beachten müssen.

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[Barrierefreiheit](~/ios/app-fundamentals/accessibility.md)

In diesem Dokument werden verschiedene APIs und Tools beschrieben, die verwendet werden können, um Anwendungen zu erstellen, auf die so viele Benutzer wie möglich zugreifen können.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[App-Transportsicherheit](~/ios/app-fundamentals/ats.md)

In diesem Artikel werden die Sicherheitsänderungen vorgestellt, die von der APP-Transport Sicherheit für eine IOS 9-APP erzwungen werden, und das bedeutet, dass für Ihre xamarin. IOS-Projekte die Optionen der ATS-Konfiguration behandelt werden, und es wird erläutert, wie Sie bei Bedarf deaktiviert werden. Da ATS standardmäßig aktiviert ist, wird bei allen nicht sicheren Internetverbindungen eine Ausnahme in ios 9-apps ausgelöst (es sei denn, Sie haben Sie explizit zugelassen).

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)

Hintergrundverarbeitung oder zurück Setzung ist der Prozess, mit dem Anwendungen Aufgaben im Hintergrund ausführen können, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch bietet eine Einführung in die Hintergrundverarbeitung in ios.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Erstellen von IOS-Anwendungen im Code](~/ios/app-fundamentals/ios-code-only.md)

In diesem Artikel wird das Erstellen von IOS-Anwendungen vollständig im Code mithilfe von Visual Studio und Visual Studio für Mac erläutert. Es zeigt, wie Sie von einer leeren Projektvorlage aus starten, um einen Anwendungsbildschirm in einem Controller zu erstellen, indem Sie eine Hierarchie von Sichten von UIKit erstellen. Anschließend wird erläutert, wie Sie benutzerdefinierte Ansichten erstellen, die in einen Controller geladen werden können.

## <a name="exception-marshalingiosplatformexception-marshalingmd"></a>[Ausnahme Marshalling](~/ios/platform/exception-marshaling.md)

Beschreibt, wie Ziel-C und verwaltete Ausnahmen zwischen nativen und verwalteten Frames gemarshallt werden.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)

In diesem Artikel werden die wichtigsten IOS-Technologien vorgestellt, mit denen Rückrufe empfangen und Steuerelemente der Benutzeroberfläche mit Daten aufgefüllt werden können. Diese Technologien sind Ereignisse, Protokolle und Delegaten. in diesem Artikel wird erläutert, worum es sich bei C#den einzelnen Funktionen handelt und wie diese in verwendet werden. Es veranschaulicht, wie xamarin. IOS IOS-Steuerelemente verwendet, um vertraute .net-Ereignisse verfügbar zu machen, und wie xamarin. IOS Unterstützung für Ziel-c-Konzepte wie Protokolle und Delegaten bietet (Ziel- C# c-Delegaten sollten nicht mit Delegaten verwechselt werden). In diesem Artikel finden Sie auch Beispiele, die zeigen, wie Protokolle sowohl als Grundlage für Ziel-C-Delegaten als auch in Szenarios ohne Delegaten verwendet werden.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md)

Xamarin. IOS kann dieselben System.IO-Klassen verwenden, um mit Dateien und Verzeichnissen in IOS zu arbeiten, die Sie in einer beliebigen .NET-Anwendung verwenden würden. Allerdings implementiert IOS trotz der vertrauten Klassen und Methoden einige Einschränkungen für die Dateien, die erstellt werden können oder auf die Sie zugreifen können, und stellt auch spezielle Features für bestimmte Verzeichnisse bereit. In diesem Artikel werden diese Einschränkungen und Features beschrieben, und es wird veranschaulicht, wie der Dateizugriff in einer xamarin. IOS-Anwendung funktioniert.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md)

In diesem Artikel wird die Verwendung von Images in xamarin. IOS, sowohl Anwendungs Unterstützungs Images (z. b. Symbole, Laden von Bildern usw.) als auch Bilder in Anwendungen (z. b. Bilder, die auf Steuerelemente angewendet werden) untersucht. Außerdem wird erläutert, wie Sie mit Visual Studio für Mac Bilder integrieren und mit Bildern aus Code interagieren.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalisierung](~/ios/app-fundamentals/localization/index.md)

Dieser Leitfaden behandelt das Hinzufügen von Codierungen zu einer xamarin. IOS-Anwendung, um die Internationalisierung zu unterstützen.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Arbeiten mit Eigenschaften Listen](~/ios/app-fundamentals/index.md)

In diesem Dokument wird der grafische und erweiterte Eigenschaften Listen-Editor (plist) Visual Studio für Mac für die Verwendung von "Info. plist" und "Berechtigungen. plist" eingeführt. Es veranschaulicht das Festlegen von Symbolen und Start Bildern für eine IOS-Anwendung und veranschaulicht das Angeben von App-Funktionen (Berechtigungen) in Visual Studio für Mac.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Arbeiten mit Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md)

Apple hat mehrere Verbesserungen hinsichtlich Sicherheit und Datenschutz in ios 10 (und höher) vorgenommen, die dem Entwickler dabei helfen, die Sicherheit Ihrer apps zu verbessern und den Datenschutz für den Endbenutzer zu gewährleisten. In diesem Artikel wird die Implementierung dieser Features in einer xamarin. IOS-App behandelt.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Threading](~/ios/app-fundamentals/threading.md)

In diesem Artikel wird das Threading in einer xamarin. IOS-Anwendung erläutert, und es wird ein wenig über den .net-Thread Pool, reaktionsschnelle Anwendungen und Garbage Collection gesprochen.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Toucheingabe](~/ios/app-fundamentals/touch/index.md)

Mit Touchscreens auf vielen der heutigen Geräte können Benutzer auf natürliche und intuitive Weise schnell und effizient mit Geräten interagieren. Diese Interaktion ist nicht nur auf einfache Berührungs Erkennung beschränkt – es ist auch möglich, Gesten zu verwenden. Beispielsweise ist die Stift-zu-Zoom-Geste ein sehr gängiges Beispiel für diese –, indem ein Teil des Bildschirms mit zwei Fingern fixiert wird, die der Benutzer vergrößern oder verkleinern kann. In dieser Anleitung werden toucheingaben und Gesten in ios untersucht.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Arbeiten mit Benutzer Standardwerten](~/ios/app-fundamentals/user-defaults.md)

Mit `NSUserDefaults` der-Klasse können IOS-apps und-Erweiterungen Programm gesteuert mit dem systemweiten Standardsystem interagieren. Mithilfe des Standardsystems kann der Benutzer das Verhalten oder das Formatieren einer APP so konfigurieren, dass die Einstellungen (basierend auf dem Entwurf der APP) erfüllt werden. Beispielsweise zur Darstellung von Daten in Metriken im Vergleich zu kaiserlichen Messungen oder zum Auswählen eines bestimmten UI-Designs.
