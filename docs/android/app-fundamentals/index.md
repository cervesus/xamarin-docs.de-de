---
title: Grundlagen der Xamarin.Android-Anwendung
description: Konzepte der Core-Anwendung
ms.prod: xamarin
ms.assetid: 935B8BFE-23B7-4239-5C87-F4A503B889CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 3ed616340dbb6dd64d85998d84e340ec381f6b54
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864086"
---
# <a name="xamarinandroid-application-fundamentals"></a>Grundlagen der Xamarin.Android-Anwendung

Dieser Abschnitt enthält eine Anleitung für einige der häufiger Aufgaben der Dinge oder Konzepte, denen Entwickler bei der Entwicklung von Android-Anwendungen berücksichtigen müssen.

## <a name="accessibilityandroidapp-fundamentalsaccessibilitymd"></a>[Barrierefreiheit](~/android/app-fundamentals/accessibility.md)

Diese Seite beschreibt, wie die Android Eingabehilfen-APIs zum Erstellen von apps, die gemäß der [Checkliste für die Barrierefreiheit](~/cross-platform/app-fundamentals/accessibility.md).

## <a name="understanding-android-api-levelsandroidapp-fundamentalsandroid-api-levelsmd"></a>[Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md)

Dieser Anleitung wird beschrieben, wie Android-API-Ebenen zum Verwalten von app-Kompatibilität für verschiedene Versionen von Android verwendet, und es wird erläutert, wie Xamarin.Android-projekteinstellungen zum Bereitstellen dieser API-Ebenen in Ihrer app zu konfigurieren. Darüber hinaus dieser Anleitung wird erläutert, wie Common Language Runtime-Code schreiben, der mit unterschiedlichen API behandelt, und es bietet eine Liste der alle Android-API-Ebenen, Versionsnummern (z. B. Android 8.0), Android-Code-Namen (z. B. Oreo) und Build-Versionscodes.



## <a name="resources-in-androidandroidapp-fundamentalsresources-in-androidindexmd"></a>[Ressourcen in Android](~/android/app-fundamentals/resources-in-android/index.md)

Dieser Artikel führt das Konzept der Android-Ressourcen in Xamarin.Android und Dokumente ihrer Verwendung. Es wird beschrieben, wie Ressourcen in Ihrer Android-Anwendung verwenden, um anwendungslokalisierung und mehrere Geräte aus unterschiedlichen Bildschirmgrößen und dichten zu unterstützen.




## <a name="activity-lifecycleandroidapp-fundamentalsactivity-lifecycleindexmd"></a>[Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)

Aktivitäten sind ein wesentlicher Baustein von Android-Anwendungen, und sie können in einer Reihe von anderen Zuständen vorhanden sein. Der aktivitätslebenszyklus beginnt mit der Instanziierung und endet mit Zerstörung und viele Status in der Zwischenzeit enthält. Wenn eine Aktivität den Status ändert, wird die entsprechende Lebenszyklus-Event-Methode aufgerufen, die Aktivität der bevorstehenden statusänderung zu benachrichtigen und zum Ausführen von Code zur Anpassung an diese Änderung ermöglicht. In diesem Artikel werden die Aktivitäten des Lebenszyklus untersucht und erläutert, die Verantwortung, dass eine Aktivität während dieser Zustandsänderungen als Teil einer Anwendung kaum und zuverlässige verfügt.

## <a name="localizationandroidapp-fundamentalslocalizationmd"></a>[Lokalisierung](~/android/app-fundamentals/localization.md)

In diesem Artikel wird erläutert, wie eine xamarin.Android-Anwendung in andere Sprachen zu lokalisieren, indem Sie Übersetzen von Zeichenfolgen und alternative Images bereitstellen.

## <a name="servicesandroidapp-fundamentalsservicesindexmd"></a>[Dienste](~/android/app-fundamentals/services/index.md)

Dieser Artikel behandelt die Android-Dienste, die Android-Komponenten sind, mit die Arbeit im Hintergrund ausgeführt werden können. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind, und zeigt, wie sie implementiert sowohl für die Durchführung von lang andauernden Hintergrundaufgaben sowie, bieten eine Schnittstelle für Remoteprozeduraufrufe.

## <a name="broadcast-receiversandroidapp-fundamentalsbroadcast-receiversmd"></a>[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)

Dieses Handbuch beschrieben, wie zum Erstellen und Verwenden der übertragungsempfänger, ein Android-Komponente, die auf eine systemweite Broadcasts in Xamarin.Android reagiert.



## <a name="permissionsandroidapp-fundamentalspermissionsmd"></a>[Berechtigungen](~/android/app-fundamentals/permissions.md)

Sie können die Tools zur Unterstützung in Visual Studio für Mac oder Visual Studio integriert verwenden, erstellen und Hinzufügen von Berechtigungen auf das Android-Manifest. Dieses Dokument beschreibt die Vorgehensweise beim Hinzufügen von Berechtigungen in Visual Studio und Xamarin Studio.



## <a name="graphics-and-animationandroidapp-fundamentalsgraphics-and-animationmd"></a>[Grafiken und Animationen](~/android/app-fundamentals/graphics-and-animation.md)

Android bietet ein sehr umfangreichen und verschiedenartigen-Framework für die Unterstützung von Direct2D-Grafiken und Animationen. Dieses Dokument führt dieser Frameworks und erläutert, wie benutzerdefinierte Grafiken und Animationen erstellen und zu deren Verwendung in einer Xamarin.Android-Anwendung.


## <a name="cpu-architecturesandroidapp-fundamentalscpu-architecturesmd"></a>[CPU-Architekturen](~/android/app-fundamentals/cpu-architectures.md)

Xamarin.Android unterstützt mehrere CPU-Architekturen, einschließlich 32-Bit und 64-Bit-Geräten. In diesem Artikel wird erläutert, wie Sie eine app eine oder mehrere Android unterstützte CPU-Architekturen abzielen.




## <a name="handling-rotationandroidapp-fundamentalshandling-rotationmd"></a>[Verarbeiten der Drehung](~/android/app-fundamentals/handling-rotation.md)

Dieser Artikel beschreibt, wie Änderungen an Device-Ausrichtung in Xamarin.Android behandelt wird. Es wird beschrieben, wie mit dem Android-Ressourcen-System, um Ressourcen für ein bestimmtes geräteausrichtung auch automatisch zu laden, wie an die programmgesteuerte Behandlung der Ausrichtung Änderungen funktioniert. Klicken Sie dann es werden Verfahren beschrieben Zustand beibehalten, wenn ein Gerät gedreht wird.



## <a name="android-audioandroidapp-fundamentalsandroid-audiomd"></a>[Android Audio](~/android/app-fundamentals/android-audio.md)

Android-Betriebssystems bietet umfangreiche Unterstützung für Multimedia, umfasst, Audio und Video verwendet wird. Dieser Leitfaden konzentriert sich auf Audiodaten in Android und deckt wiedergeben und Aufzeichnen von Audio mithilfe der integrierten Audioplayer und Recorder Klassen als auch die Low-Level-audio-API. Hierin sind auch arbeiten mit Audio-Ereignissen, die von anderen Anwendungen übertragen werden, damit Entwickler kaum Anwendungen erstellen können.




## <a name="notificationsandroidapp-fundamentalsnotificationsindexmd"></a>[Benachrichtigungen](~/android/app-fundamentals/notifications/index.md)

In diesem Abschnitt wird erläutert, wie lokale und remote-Benachrichtigungen in Xamarin.Android implementiert wird. Es wird beschrieben, die von einer Android-benachrichtigungs UI-Elemente und behandelt die API des mit dem Erstellen und Anzeigen einer Benachrichtigung beteiligt. Von remotebenachrichtigungen werden sowohl Google Cloud Messaging und Firebase Cloud Messaging erläutert. Schrittweise exemplarische Vorgehensweisen und Codebeispiele sind enthalten.



## <a name="touchandroidapp-fundamentalstouchindexmd"></a>[Toucheingabe](~/android/app-fundamentals/touch/index.md)

In diesem Abschnitt wird erläutert, dass die Konzepte und die Details der Implementierung von touch-Gesten auf Android. Touch-APIs eingeführt und beschrieben, gefolgt von einer Untersuchung der Bewegung Erkennungen.



## <a name="httpclient-stack-and-ssltlsandroidapp-fundamentalshttp-stackmd"></a>[HttpClient-Stapel und SSL/TLS](~/android/app-fundamentals/http-stack.md)

Dieser Abschnitt erläutert die HttpClient-Stapel und SSL/TLS-Implementierung Selektoren für Android. Diese Einstellungen bestimmen die "HttpClient" und SSL/TLS-Implementierung, die von den Xamarin.Android-apps verwendet werden.


## <a name="writing-responsive-applicationswriting-responsive-appsmd"></a>[Schreiben von-reaktionsschneller Anwendungen](writing-responsive-apps.md)

Dieser Artikel beschreibt, wie Sie threading verwenden, um einer Xamarin.Android-Anwendung reaktionsfähig zu halten, durch das Verschieben von lang andauernden Aufgaben in einem Hintergrundthread.
