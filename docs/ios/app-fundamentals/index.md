---
title: Grundlagen der Xamarin.iOS-Anwendung
description: Dieses enthält Dokumentenlinks zu verschiedenen Anleitungen, die für die Entwicklung von Xamarin.iOS, wie Sicherheit auf Transportebene app, grundlegende Konzepte beschrieben backgrounding, Ereignisse und threading.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/21/2017
ms.openlocfilehash: cdace50d851b2c99f9241b869f248e58d5b93377
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784496"
---
# <a name="xamarinios-application-fundamentals"></a>Grundlagen der Xamarin.iOS-Anwendung

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben Dinge oder Konzepte, mit denen Entwickler Beachten Sie beim Entwickeln von Anwendungen Xamarin.iOS (früher MonoTouch) werden müssen.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[App-Transportsicherheit](~/ios/app-fundamentals/ats.md)

In diesem Artikel werden die Änderungen der Sicherheit, die Transportsicherheit für die App auf eine app für iOS 9 und was dies für Ihre Projekte Xamarin.iOS bedeutet erzwingt vorgestellt, berücksichtigt die ATS Konfigurationsoptionen, und es wird beschrieben, wie opt-Out von ATS, falls erforderlich. Da ATS standardmäßig aktiviert ist, lösen alle unsichere internetverbindungen eine Ausnahme in iOS 9-apps, (es sei denn, Sie sie explizit zugelassen haben).


## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)

Im Hintergrund verarbeitet oder backgrounding versteht man ermöglicht Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund.


## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Dieser Artikel erläutert die wichtigsten iOS-Technologien verwendet, um Rückrufe zu empfangen und Benutzeroberflächen-Steuerelemente mit Daten aufzufüllen. Diese Technologien sind Ereignisse, Protokolle und Delegaten. In diesem Artikel erklärt den Zweck jedes davon wird und jede Verwendung von c#. Es wird veranschaulicht, wie Xamarin.iOS iOS-Steuerelemente verwendet, um bekannte .NET Ereignisse sowie wie Xamarin.iOS Objective-C-Konzepten wie Protokolle und Delegaten unterstützt (Objective-C-Delegaten darf nicht mit verwechselt werden C#-Delegaten). Dieser Artikel enthält auch Beispiele, die zeigen, wie Protokolle sowohl als Grundlage für Objective-C-Delegaten und nicht Delegaten Szenarien verwendet werden.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Threading](~/ios/app-fundamentals/threading.md)

In diesem Artikel wird erläutert, in einer Anwendung Xamarin.iOS threading und etwas dreht den Threadpool .NET, reaktionsfähig Anwendungen und Garbagecollection.&nbsp;

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Working with Images (Arbeiten mit Bildern)](~/ios/app-fundamentals/images-icons/index.md)

In diesem Artikel untersucht, wie Bilder in Xamarin.iOS, Unterstützung Anwendungsbilder (z. B. Symbole, laden Bilder usw.) und Bilder innerhalb von Anwendungen (z. B. Bilder, die auf Steuerelemente angewendet werden) zu verwenden. Es werden auch behandelt, wie Visual Studio für Mac verwenden, um Bilder zu einzubinden sowie die Interaktion mit Bildern aus Code.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Arbeiten mit Eigenschaftenlisten](~/ios/app-fundamentals/index.md)

Dieses Dokument stellt Visual Studio für Mac Computer grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit der Datei "Info.plist" und Entitlements.plist bereit. Veranschaulicht das Festlegen von Symbolen und starten Sie Abbilder für iOS-Anwendung, und veranschaulicht, wobei app-Funktionen (Berechtigungen) aus innerhalb von Visual Studio für Mac.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Working with the File System (Arbeiten mit dem Dateisystem)](~/ios/app-fundamentals/file-system.md)

Die gleichen System.IO-Klassen können Xamarin.iOS arbeiten mit Dateien und Verzeichnissen in iOS, die Sie in jeder .NET-Anwendung verwenden würden. Allerdings wird trotz der vertrauten Klassen und Methoden, ein iOS implementiert einige Einschränkungen auf die Dateien, die erstellt oder zugegriffen werden können und außerdem bietet spezielle Funktionen für bestimmte Verzeichnisse. In diesem Artikel werden diese Einschränkungen und Funktionen dargestellt, und veranschaulicht die Funktionsweise der Zugriff auf Dateien in einem Xamarin.iOS-Anwendung.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Erstellen von iOS-Anwendungen in Code](~/ios/app-fundamentals/ios-code-only.md)

In diesem Artikel untersucht, wie zum Erstellen von iOS-Anwendungen vollständig im Code mithilfe von Visual Studio und Visual Studio für Mac. Es wird gezeigt, wie aus eine leere Projektvorlage aus, um eine Anwendungsbildschirm in einem Controller zu erstellen, durch das Erstellen einer Hierarchie von Ansichten von UIKit gestartet. Klicken Sie dann, es wird erläutert, wie benutzerdefinierte Ansichten erstellen, die geladen werden können, in einem Controller.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Arbeiten mit Standardeinstellungen für Benutzer](~/ios/app-fundamentals/user-defaults.md)

Die `NSUserDefaults` -Klasse bietet eine Möglichkeit für iOS-Apps und Erweiterungen für die programmgesteuerte Interaktion mit dem eine systemweite Standardeinstellung System. Verwenden Sie das System standardmäßig, kann der Benutzer einer Verhalten oder die app formatieren, um ihre Voreinstellungen (basierend auf den Entwurf der app) erfüllen konfigurieren. Geben Sie beispielsweise Folgendes ein, um das Darstellen von Daten in Vs Metrik das englische System oder Auswählen eines bestimmten UI-Designs.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Toucheingabe](~/ios/app-fundamentals/touch/index.md)

Touchscreens auf viele der heutigen Geräte ermöglichen Benutzern Geräten auf eine natürliche und intuitive Weise schnell und effizient interagieren. Diese Aktivität ist nicht nur auf einfache Touch Erkennung beschränkt – es ist möglich, auch Gesten verwenden. Zwei-Finger-Zoom Bewegung ist z. B. ein sehr gängiges Beispiel dieser – durch einen Teil des Bildschirms mit zwei Fingern fest, denen der Benutzer vergrößern oder verkleinern kann pinching an. Dieses Handbuch untersucht Touch- und Gesten in iOS.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Arbeiten mit Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md)

Apple hat mehrere Erweiterungen für Sicherheit und Datenschutz für iOS 10 (und höher), mit deren Hilfe wird den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten. In diesem Artikel befasst sich diese Funktionen in einer app Xamarin.iOS implementieren.

##  <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalisierung](~/ios/app-fundamentals/localization/index.md)

Dieser Leitfaden behandelt das Hinzufügen von Codierungen zu einem Xamarin.iOS-Anwendung zur Unterstützung der Internationalisierung.
