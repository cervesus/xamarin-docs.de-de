---
title: Android Wear
description: Erstellen von apps für Android tragbare Geräte aus.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: fca72291dd726d4f2a6635d26390baa103ee0d2d
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864907"
---
# <a name="android-wear"></a>Android Wear

Android Wear ist eine Version von Android, die für tragbare Geräte wie intelligente Überwachungen entwickelt wurde. Dieser Abschnitt enthält Anweisungen zum Installieren und Konfigurieren von Tools, die für eine schrittweise Anleitung zum Erstellen Ihrer ersten Wear-Gerät und eine Liste mit Beispielen, die Sie verweisen können, um für die Erstellung einer eigenen apps Wear Wear-Entwicklung erforderlich.

## <a name="getting-startedandroidwearget-startedindexmd"></a>[Erste Schritte](~/android/wear/get-started/index.md)

Führt Android Wear, beschreibt, wie zum Installieren und konfigurieren Ihren Computer für Wear-Entwicklung und enthält die Schritte zum Erstellen und Ausführen Ihrer erste Android Wear-app auf einem Emulator oder Wear-Gerät.

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Benutzeroberfläche](~/android/wear/user-interface/index.md)

Beschreibt Wear OS spezifische Steuerelemente und stellt Links zu Beispielen zur Nutzung von Steuerelementen bereit.

## <a name="platform-featuresandroidwearplatformindexmd"></a>[Plattformfeatures](~/android/wear/platform/index.md)

Dokumente in diesem Abschnitt werden die Funktionen, die speziell für Android Wear behandelt. Hier finden Sie ein Thema, die beschreibt, wie Sie eine WatchFace zu erstellen.

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Bildschirmgrößen](~/android/wear/screen-sizes.md)

Vorschau, und Optimieren Sie Ihre Benutzeroberfläche für die verfügbaren Bildschirmgrößen.

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Bereitstellung und Testen](~/android/wear/deploy-test/index.md)

Erläutert, wie Ihre Android Wear-app auf einem Android Wear-Gerät oder für Android-Emulator für Wear konfiguriert bereitstellen. Es umfasst auch das Debuggen von Tipps und Informationen zum Einrichten einer Bluetooth-Verbindung zwischen Ihrem Entwicklungscomputer und ein Android-Gerät.

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Wear-APIs](https://developer.android.com/reference/android/support/wearable)

Der Android-Entwicklerwebsite enthält ausführliche Informationen zu wichtige Wear-APIs wie z. B. [tragbare Aktivität](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [Intents](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [Authentifizierung](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Komplikationen](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [Komplikationen, der Rendering](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [Benachrichtigungen](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [Ansichten](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), und [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Proben

Sie finden eine Anzahl von [Beispiele](https://developer.xamarin.com/samples/android/Android%20Wear/) mit Android Wear (oder wechseln Sie direkt zu [Github](https://github.com/xamarin/monodroid-samples/tree/master/wear)).

|Beispiel|Beschreibung|Bildschirmabbildung|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/monodroid/wear/SkeletonWear/)|Ein einfaches Beispiel über die Grundlagen der tragbare Projekte einschließlich GridViewPager und interaktive Benachrichtigungen.|![Screenshot der Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/monodroid/wear/WatchViewStub/)|Um eine einfache Demo des WatchViewStub-Steuerelements, die Form "Bildschirm" erkennt und lädt automatisch das richtige Layout. Funktionsweise WatchViewStub in die **Resources/layout/main_activity.xml** Layout.|![Screenshot der WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/)|Demonstration von Wear-Notification-Seiten, in Form von Rezept Schritte. Benachrichtigungen werden in RecipeService.cs erstellt.|![Screenshot der RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/monodroid/wear/ElizaChat/)|Spaß Beispiel für die Interaktion mit einem "persönlichen Assistenten" wird aufgerufen, Eliza, erstellen eine Konversation mit der erstellten Antworten mithilfe Wear interaktive Benachrichtigungen.|![Screenshot der ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)|GridViewPager implementiert das Direct2D-navigationsmuster, in denen der Benutzer mit einer vertikal wischbewegung und horizontal Navigieren durch Optionen und Inhalt.|![Screenshot der GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace ist eine benutzerdefinierte watchface mit Analog-Stil Stunde, Minute und Sekunde Hände. In diesem Beispiel wird veranschaulicht, wie zum Erstellen eines Gesichts-Diensts überwachen, die die aktuelle Uhrzeit zeichnet und Ereignisse zur Veränderungen bei Handles ambient-Modus und Sichtbarkeit. Es enthält einen broadcast Receiver, der Lauscht auf Änderungen der Zeitzone und Zeitpunkt automatisch entsprechend aktualisiert.|![Screenshot der WatchFace](images/gridviewpager.png)|


## <a name="videos"></a>Videos

Sehen Sie sich diese Videos, Links, in denen erörtert Xamarin.Android mit Wear-Unterstützung:

|Beschreibung|Bildschirmabbildung|
|--- |--- |
|[Android L und vieles mehr](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; die Entwicklervorschau für Android L eingeführt, eine Vielzahl der neuen APIs für Entwickler zu nutzen, einschließlich Material Design, Benachrichtigungen und neue Animationen, um nur einige zu nennen.|![Bildschirmabbildung von Video der Präsentation](images/video-android-l.png)|
|[C#ist in meinem Ohren und Augen: Google Glass und Android Wear](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; tragbare computing mag etwas aus der Zukunft (oder eine Folge von Inspektor-Minianwendung), aber viele Benutzer sehen die Zukunft noch heute! C#Entwickler wissen und bereits über die Tools und Fähigkeiten nutzen, die Leistungsfähigkeit von tragbare Geräte (von Evolve 2014) verfügen.|![Bildschirmabbildung von Video der Präsentation](images/video-eyes-ears.png)|
|[Was ist neu in Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android Wear, Android TV, Android Auto, Material Design und Grafiken; diese Meinung nach als Xamarin-Entwickler? von Evolve 2014.|![Bildschirmabbildung von Video der Präsentation](Images/video-whats-new.png)|


<!--

March 18
https://blog.xamarin.com/android-wear/

August 14
https://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
https://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
