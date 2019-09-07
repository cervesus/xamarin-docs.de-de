---
title: Grundlagen der xamarin. Android-Anwendung
description: Grundlegende Anwendungskonzepte
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: af7ba83b9026a91028f4ffa9894d564d5ff13eb8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755304"
---
# <a name="xamarinandroid-application-fundamentals"></a>Grundlagen der xamarin. Android-Anwendung

Dieser Abschnitt enthält eine Anleitung für einige der gängigeren Aufgaben oder Konzepte, die Entwickler bei der Entwicklung von Android-Anwendungen kennen müssen.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Barrierefreiheit](~/android/app-fundamentals/accessibility.md)

Auf dieser Seite wird beschrieben, wie Sie die Android-Barrierefreiheits-APIs zum Erstellen von apps entsprechend der [Eincheck Checkliste](~/cross-platform/app-fundamentals/accessibility.md)verwenden.

## <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Informationen zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md)

In diesem Leitfaden wird beschrieben, wie Android API-Ebenen zum Verwalten der APP-Kompatibilität in verschiedenen Versionen von Android verwendet. Außerdem wird erläutert, wie Sie xamarin. Android-Projekteinstellungen für die Bereitstellung dieser API-Ebenen in ihrer App konfigurieren. Außerdem wird in diesem Handbuch erläutert, wie Sie Lauf Zeit Code schreiben, der mit verschiedenen API-Ebenen umgeht, und eine Referenzliste aller Android-API-Ebenen, Versionsnummern (z. b. Android 8,0), Android-Code Namen (z. b. Oreo) und buildversionscodes bereitstellen.

## <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Ressourcen in Android](~/android/app-fundamentals/resources-in-android/index.md)

In diesem Artikel wird das Konzept der Android-Ressourcen in xamarin. Android vorgestellt und erläutert, wie Sie verwendet werden. Es wird erläutert, wie Ressourcen in Ihrer Android-Anwendung zur Unterstützung der Lokalisierung von Anwendungen und mehrerer Geräte mit unterschiedlichen Bildschirmgrößen und dichten verwendet werden.

## <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)

Aktivitäten sind ein grundlegender Baustein von Android-Anwendungen und können in einer Reihe unterschiedlicher Zustände vorhanden sein. Der Aktivitäts Lebenszyklus beginnt mit der Instanziierung und endet mit der Zerstörung und umfasst viele Zustände dazwischen. Wenn sich der Zustand einer Aktivität ändert, wird die entsprechende Lifecycle-Ereignismethode aufgerufen, die die Aktivität der bevorstehenden Zustandsänderung benachrichtigt und die Ausführung von Code für die Anpassung an diese Änderung zulässt. In diesem Artikel wird der Lebenszyklus von Aktivitäten untersucht, und es wird erläutert, wie sich eine Aktivität bei den einzelnen Zustandsänderungen als Teil einer gut verhaltenen, zuverlässigen Anwendung verhält.

## <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Lokalisierung](~/android/app-fundamentals/localization.md)

In diesem Artikel wird erläutert, wie Sie xamarin. Android in andere Sprachen lokalisieren, indem Sie Zeichen folgen übersetzen und alternative Bilder bereitstellen.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Dienste](~/android/app-fundamentals/services/index.md)

Dieser Artikel behandelt Android-Dienste, bei denen es sich um Android-Komponenten handelt, die das erledigen von Aufgaben im Hintergrund ermöglichen. Es werden die verschiedenen Szenarios erläutert, für die Dienste geeignet sind, und es wird gezeigt, wie Sie sowohl zum Ausführen von Hintergrundaufgaben mit langer Laufzeit als auch zur Bereitstellung einer Schnittstelle für Remote Prozedur Aufrufe implementiert werden.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)

In diesem Leitfaden wird beschrieben, wie Broadcast Empfänger, eine Android-Komponente, die auf systemweite Übertragungen antwortet, in xamarin. Android erstellt und verwendet werden.

## <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Berechtigungen](~/android/app-fundamentals/permissions.md)

Zum Erstellen und Hinzufügen von Berechtigungen für das Android-Manifest können Sie die Unterstützung von Tools verwenden, die in Visual Studio für Mac oder Visual Studio integriert ist. In diesem Dokument wird beschrieben, wie Sie Berechtigungen in Visual Studio hinzufügen und Xamarin Studio.

## <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafiken und Animationen](~/android/app-fundamentals/graphics-and-animation.md)

Android bietet ein sehr umfangreiches und vielfältiges Framework für die Unterstützung von 2D-Grafiken und-Animationen. In diesem Dokument werden diese Frameworks vorgestellt und erläutert, wie benutzerdefinierte Grafiken und Animationen erstellt und in einer xamarin. Android-Anwendung verwendet werden.

## <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU-Architekturen](~/android/app-fundamentals/cpu-architectures.md)

Xamarin. Android unterstützt verschiedene CPU-Architekturen, einschließlich 32-Bit-und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine APP für eine oder mehrere mit Android unterstützte CPU-Architekturen ausrichten.

## <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Verarbeiten der Drehung](~/android/app-fundamentals/handling-rotation.md)

In diesem Artikel wird beschrieben, wie Änderungen an der Geräte Orientierung in xamarin. Android behandelt werden. Es wird erläutert, wie Sie mit dem Android-Ressourcensystem zum automatischen Laden von Ressourcen für eine bestimmte Geräte Orientierung und zum programmgesteuerten behandeln von Richtungsänderungen arbeiten. Anschließend werden Techniken zum Verwalten des Zustands beim Drehen eines Geräts beschrieben.

## <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android Audio](~/android/app-fundamentals/android-audio.md)

Das Android-Betriebssystem bietet umfassende Unterstützung für Multimedia und umfasst sowohl Audioinformationen als auch Videos. Dieser Leitfaden konzentriert sich auf das Audiomaterial in Android und deckt das Abspielen und Aufzeichnen von Audiodaten mithilfe der integrierten Audioplayer-und Recorder-Klassen sowie der Low-Level-audioapi ab. Außerdem wird die Arbeit mit Audioereignissen behandelt, die von anderen Anwendungen gesendet werden, damit Entwickler gut verhaltene Anwendungen entwickeln können.

## <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Benachrichtigungen](~/android/app-fundamentals/notifications/index.md)

In diesem Abschnitt wird erläutert, wie lokale Benachrichtigungen und Remote Benachrichtigungen in xamarin. Android implementiert werden. Darin werden die verschiedenen Benutzeroberflächen Elemente einer Android-Benachrichtigung beschrieben. Außerdem wird erläutert, welche API zum Erstellen und Anzeigen einer Benachrichtigung beteiligt ist. Für Remote Benachrichtigungen werden sowohl Google Cloud Messaging als auch Firebase Cloud Messaging erläutert. Ausführliche Exemplarische Vorgehensweisen und Codebeispiele sind enthalten.

## <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Toucheingabe](~/android/app-fundamentals/touch/index.md)

In diesem Abschnitt werden die Konzepte und Details zur Implementierung von Touchgesten unter Android erläutert. Touchapis werden eingeführt und erläutert, gefolgt von einer Untersuchung von Gesten erkenungen.

## <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS](~/android/app-fundamentals/http-stack.md)

In diesem Abschnitt werden die HttpClient-Stapel-und SSL/TLS-Implementierungs Auswahl für Android erläutert. Diese Einstellungen bestimmen die HttpClient-und SSL/TLS-Implementierung, die von xamarin. Android-Apps verwendet wird.

## <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Schreiben von reaktionsfähigen Anwendungen](writing-responsive-apps.md)

In diesem Artikel wird erläutert, wie Sie mithilfe von Threading eine xamarin. Android-Anwendung auf reaktionsfähig halten, indem Sie Aufgaben mit langer Ausführungszeit in einen Hintergrund Thread verschieben.
