---
title: Application Fundamentals (Anwendungsgrundlagen)
description: "Kernkonzepte für die Anwendung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4abb8c823c62bc62fd2e6f717cc1b5bde9057e4e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Application Fundamentals (Anwendungsgrundlagen)

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben Dinge oder Konzepte, denen Entwickler bei der Entwicklung von Android-Anwendungen bewusst sein müssen.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Barrierefreiheit](~/android/app-fundamentals/accessibility.md)

Diese Seite beschreibt, wie Android Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Eingabehilfen Prüfliste](~/cross-platform/app-fundamentals/accessibility.md).

##  <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md)

Dieses Handbuch beschreibt, wie Android-API-Ebenen verwendet, um app-Kompatibilität in den verschiedenen Versionen von Android zu verwalten, und es wird erläutert, wie Xamarin.Android projekteinstellungen zum Bereitstellen dieser API-Ebenen in Ihrer app zu konfigurieren. Darüber hinaus dieser Anleitung wird erläutert, wie Common Language Runtime-Code zu schreiben, der mit verschiedenen API-Ebenen behandelt, und es enthält eine Liste der Android-API-Ebenen, Versionsnummern (z. B. Android 8.0), Android Code Namen (z. B. Oreo) und Build-Version-Codes.



##  <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Ressourcen in Android](~/android/app-fundamentals/resources-in-android/index.md)

Dieser Artikel führt das Konzept von Android-Ressourcen in Xamarin.Android und Dokumente zu ihrer Verwendung. Sie erfahren, wie auf Ressourcen in der Android-Anwendung verwenden, um die anwendungslokalisierung und mehrere Geräte, die unter unterschiedlichen Bildschirmgrößen und Dichte zu unterstützen.




##  <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Aktivitätenlebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)

Aktivitäten sind ein wichtiger Baustein von Android-Anwendungen und sie können in einer Reihe von unterschiedlichen Zuständen annehmen. Der Aktivitätenlebenszyklus beginnt mit Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Bei einer statusänderung von eine Aktivität wird die entsprechende Lifecycle Ereignismethode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code Anpassung an diese Änderung ermöglicht. In diesem Artikel untersucht den Lebenszyklus von Aktivitäten und erläutert, die dafür verantwortlich, dass eine Aktivität bei jedem dieser Zustand Änderungen als Teil einer Anwendung gut konzipierte, zuverlässige verfügt.

##  <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Lokalisierung](~/android/app-fundamentals/localization.md)

In diesem Artikel erläutert, wie eine Xamarin.Android durch Übersetzen von Zeichenfolgen und Bereitstellen von alternativen Bilder in mehrere Sprachen lokalisieren.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Dienste](~/android/app-fundamentals/services/index.md)

Dieser Artikel behandelt die Android-Dienste, die Android-Komponenten sind, mit die Arbeit im Hintergrund ausgeführt werden können. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind und gezeigt, wie deren Implementierung sowohl für lang andauernde Hintergrundaufgaben sowie auf eine Schnittstelle für Remoteprozeduraufrufe bereitzustellen.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Empfänger senden](~/android/app-fundamentals/broadcast-receivers.md)

Diese Anleitung enthält Informationen zum Erstellen und Verwenden von broadcast Empfänger eine Android-Komponente, die auf eine systemweite-Broadcasts in Xamarin.Android reagiert.



##  <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Berechtigungen](~/android/app-fundamentals/permissions.md)

Sie können die toolunterstützung in Visual Studio für Mac oder Visual Studio integriert verwenden, erstellen und Berechtigungen auf das Android-Manifest hinzufügen. Dieses Dokument beschreibt die Schritte zum Hinzufügen von Berechtigungen in Visual Studio und Xamarin Studio.



##  <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafiken und Animationen](~/android/app-fundamentals/graphics-and-animation.md)

Android bietet ein sehr umfangreiches und verschiedene-Framework zur Unterstützung von 2D Grafiken und Animationen. Dieses Dokument führt diese Frameworks und erläutert, wie benutzerdefinierte Grafiken und Animationen erstellen und in einer Anwendung Xamarin.Android zu verwenden.


##  <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU-Architektur](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich der 32-Bit und 64-Bit-Geräten. Dieser Artikel beschreibt, wie Sie eine app an eine oder mehrere Android unterstützt CPU-Architekturen abzielen.




##  <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Behandlung von Drehung](~/android/app-fundamentals/handling-rotation.md)

Dieser Artikel beschreibt, wie Gerät Ausrichtung Änderungen in Xamarin.Android behandelt. Sie erfahren, wie Arbeiten Sie mit dem Ressourcensystem Android, um Ressourcen für ein bestimmtes geräteausrichtung ebenfalls automatisch zu laden, wie an der Ausrichtung programmgesteuert zu verarbeiten Änderungen. Es beschreibt dann Techniken für die Zustandsverwaltung, wenn ein Gerät gedreht wird.



##  <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android Audio](~/android/app-fundamentals/android-audio.md)

Android-Betriebssysteme bietet umfangreiche Unterstützung für Multimedia, Audio und Video umfasst. Dieses Handbuch konzentriert sich auf Audio in Android und wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen sowie die Low-Level audio-API behandelt. Außerdem werden behandelt, arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen, damit Entwickler gut konzipierte Anwendungen erstellen können.




##  <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Benachrichtigungen](~/android/app-fundamentals/notifications/index.md)

In diesem Abschnitt wird erläutert, wie lokale und remote-Benachrichtigungen in Xamarin.Android implementiert wird. Es beschreibt die verschiedenen Benutzeroberflächenelemente der Android-Benachrichtigung und behandelt die API des durch das Erstellen und Anzeigen einer Benachrichtigung beteiligt. Für remote Benachrichtigungen werden sowohl Google Cloud Messaging und Firebase Cloud Messaging erläutert. Schrittweise exemplarische Vorgehensweisen sowie Codebeispiele sind enthalten.



##  <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Tippen Sie auf](~/android/app-fundamentals/touch/index.md)

In diesem Abschnitt erläutert die Konzepte und Details zur Implementierung fingereingabegesten auf Android-Geräten. Touch-APIs sind eingeführt und gefolgt von einer Untersuchung des Geste Prüfer erläutert.



##  <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS](~/android/app-fundamentals/http-stack.md)

Dieser Abschnitt erklärt die Selektoren Stapel für "HttpClient" und SSL/TLS-Implementierung für Android. Diese Einstellungen bestimmen die "HttpClient" und SSL/TLS-Implementierung, die von Ihrer Xamarin.Android-apps verwendet wird.


##  <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Schreiben von Clientanwendungen reaktionsfähig](writing-responsive-apps.md)

In diesem Artikel erläutert, wie eine Anwendung Xamarin.Android reaktionsfähig umgezogen lang andauernden Aufgaben in einem Hintergrundthread verwenden threading.