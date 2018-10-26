---
title: Grundlagen der Xamarin.iOS-Anwendung
description: Dieses Dokument enthält Links zu verschiedenen Leitfäden, die Xamarin.iOS-Entwicklung, wie etwa app Transport Security, grundlegende Konzepte beschreiben hintergrundverarbeitung, Ereignisse und threading.
ms.prod: xamarin
ms.assetid: 608403AE-B09F-4D9C-8F59-F9DE9F0B1CF1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/21/2017
ms.openlocfilehash: a40227454b597578ff1c1c247b326e523c23493b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110471"
---
# <a name="xamarinios-application-fundamentals"></a>Grundlagen der Xamarin.iOS-Anwendung

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben der Dinge oder Konzepte, mit denen Entwickler beim Entwickeln von Anwendungen für Xamarin.iOS (früher MonoTouch) berücksichtigen müssen.

## <a name="accessibilityiosapp-fundamentalsaccessibilitymd"></a>[Barrierefreiheit](~/ios/app-fundamentals/accessibility.md)

Dieses Dokument beschreibt die verschiedenen APIs und Tools, die zum Erstellen von Anwendungen, die für so viele Benutzer wie möglich verwendet werden können.

## <a name="app-transport-securityiosapp-fundamentalsatsmd"></a>[App-Transportsicherheit](~/ios/app-fundamentals/ats.md)

In diesem Artikel werden die Änderungen der Sicherheit, die die App-Transportsicherheit für eine app für iOS 9 und was dies für Ihre Xamarin.iOS-Projekte bedeutet erzwingt vorgestellt, die ATS-Konfigurationsoptionen berücksichtigt, und es wird beschrieben, wie Sie ATS und nicht bei Bedarf. Da ATS standardmäßig aktiviert ist, lösen alle unsicheren internetverbindungen (es sei denn, Sie es explizit zugelassen haben) eine Ausnahme in iOS 9-apps.

## <a name="backgroundingiosapp-fundamentalsbackgroundingindexmd"></a>[Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)

Hintergrund zu verarbeiten oder hintergrundverarbeitung ist der Prozess dadurch, dass Anwendungen, die Aufgaben im Hintergrund, während eine andere Anwendung im Vordergrund ausgeführt wird. Dieses Handbuch dient als Einführung in iOS-Verarbeitung im Hintergrund.

## <a name="creating-ios-applications-in-codeiosapp-fundamentalsios-code-onlymd"></a>[Erstellen von iOS-Anwendungen in code](~/ios/app-fundamentals/ios-code-only.md)

In diesem Artikel wird untersucht, wie zum Erstellen von iOS-Anwendungen vollständig in Code mithilfe von Visual Studio und Visual Studio für Mac. Es veranschaulicht, wie über eine leere Projektvorlage aus, um einem Anwendungsbildschirm in einem Controller zu erstellen, durch das Erstellen einer Hierarchie von Ansichten aus UIKit starten. Klicken Sie dann, es wird erläutert, wie benutzerdefinierte Ansichten erstellen, die geladen werden können in einem Controller.

## <a name="events-protocols-and-delegatesiosapp-fundamentalsdelegates-protocols-and-eventsmd"></a>[Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)

Dieser Artikel enthält die wichtigsten iOS-Technologien verwendet, um Rückrufe empfangen und Steuerelemente der Benutzeroberfläche mit Daten aufgefüllt. Diese Technologien sind Ereignisse, Protokolle und Delegaten. In diesem Artikel wird erläutert, was jede davon ist, und jede Verwendung von c#. Es wird veranschaulicht, wie Xamarin.iOS iOS-Steuerelemente verwendet, um vertraut .NET ebenfalls auf Ereignisse verfügbar zu machen, wie Sie Xamarin.iOS Objective-C-Konzepte, z. B. Protokolle und Delegaten unterstützt (Objective-C-Delegaten sollten nicht verwechselt werden mit C#-Delegaten). Dieser Artikel enthält auch Beispiele, die zeigen, wie Protokolle sowohl als Grundlage für Objective-C-Delegaten und in Szenarien mit nicht-Delegaten verwendet werden.

## <a name="working-with-the-file-systemiosapp-fundamentalsfile-systemmd"></a>[Arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md)

Xamarin.iOS können die gleichen System.IO-Klassen verwenden, arbeiten mit Dateien und Verzeichnissen in iOS, die Sie in einer beliebigen .NET-Anwendung verwenden zu können. Allerdings wird trotz der vertrauten Klassen und Methoden, ein iOS implementiert einige Einschränkungen auf die Dateien, die erstellt oder auf die zugegriffen werden können und auch bietet spezielle Funktionen für bestimmte Verzeichnisse. In diesem Artikel werden diese Einschränkungen und Funktionen beschrieben und veranschaulicht die Funktionsweise von den Zugriff auf Dateien in einer Xamarin.iOS-Anwendung.

## <a name="working-with-imagesiosapp-fundamentalsimages-iconsindexmd"></a>[Arbeiten mit Bildern](~/ios/app-fundamentals/images-icons/index.md)

In diesem Artikel wird untersucht, wie von Bildern in Xamarin.iOS, sowohl Anwendung unterstützen (z. B. Symbole, laden, Bilder usw.) auch Images in Anwendungen (z. B. Bilder, die auf Steuerelemente angewendet werden). Dazu gehört auch, wie Sie Visual Studio für Mac verwenden, um Abbilder zu integrieren sowie wie für die Interaktion mit Images aus dem Code.

## <a name="localizationiosapp-fundamentalslocalizationindexmd"></a>[Lokalisierung](~/ios/app-fundamentals/localization/index.md)

Dieser Leitfaden behandelt das Hinzufügen von Codierungen zu einer Xamarin.iOS-Anwendung zur Unterstützung der Internationalisierung.

## <a name="working-with-property-listsiosapp-fundamentalsindexmd"></a>[Arbeiten mit Eigenschaftenlisten](~/ios/app-fundamentals/index.md)

In diesem Dokument werden die Visual Studio für Mac grafischen und der erweiterten Eigenschaft Eigenschaftenliste (plist)-Editor für die Arbeit mit "Info.plist" und "Entitlements.plist". Festlegen von Symbolen dargestellt und starten Sie images für iOS-Anwendung und veranschaulicht, wobei app-Funktionen (Berechtigungen) aus Visual Studio für Mac.

## <a name="working-with-security-and-privacyiosapp-fundamentalssecurity-privacymd"></a>[Arbeiten mit Sicherheit und Datenschutz](~/ios/app-fundamentals/security-privacy.md)

Apple hat verschiedene Verbesserungen sowohl Sicherheit und Datenschutz in iOS 10 (und höher) vorgenommen, die den Entwickler verbessern Sie die Sicherheit ihrer Apps und des Endbenutzers Datenschutz gewährleisten können. Dieser Artikel behandelt diese Features in einer Xamarin.iOS-app implementieren.

## <a name="threadingiosapp-fundamentalsthreadingmd"></a>[Threading](~/ios/app-fundamentals/threading.md)

In diesem Artikel wird das Threading behandeln, in einer Xamarin.iOS-Anwendung und befasst sich mit ein wenig .NET Threadpool, reaktionsfähiger Anwendungen und Garbagecollection.

## <a name="touchiosapp-fundamentalstouchindexmd"></a>[Toucheingabe](~/ios/app-fundamentals/touch/index.md)

Touchscreens auf viele der heutigen Geräte können Benutzer schnell und effizient mit Geräten auf eine natürliche und intuitive Weise zu interagieren. Diese Interaktion ist nicht beschränkt, nur um einfache Touch-Erkennung – Es ist möglich, Gesten und zu verwenden. Die Pinch-Finger-Zoom-Bewegung ist beispielsweise ein sehr gängiges Beispiel für dieses Feature von möglichen einen Teil des Bildschirms mit zwei Fingern, die, denen der Benutzer vergrößern oder verkleinern kann. Dieses Handbuch untersucht Berührung und Gesten in iOS.

## <a name="working-with-user-defaultsiosapp-fundamentalsuser-defaultsmd"></a>[Arbeiten mit Standardeinstellungen für Benutzer](~/ios/app-fundamentals/user-defaults.md)

Die `NSUserDefaults` -Klasse bietet eine Möglichkeit für iOS-Apps und -Erweiterungen für die programmgesteuerte Interaktion mit dem standardmäßig eine systemweite-System. Verwenden Sie das System standardmäßig, kann der Benutzer der app-Verhalten oder die Formatierung, um ihre Einstellungen (basierend auf den Entwurf der app) zu erfüllen konfigurieren. Geben Sie beispielsweise Folgendes ein, um Daten im Metrik-Vs-Imperial Messungen darstellen oder von bestimmten UI Design auswählen.
