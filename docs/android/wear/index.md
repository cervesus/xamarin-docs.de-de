---
title: Android Wear
description: Erstellen von apps für Android wearable Geräte aus.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/16/2018
ms.openlocfilehash: ec8ed32eba7d8341904f800d5c174235dc25388c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="android-wear"></a>Android Wear

Android Abnutzung ist eine Version von Android, die zur wearable Geräte wie z. B. intelligente Überwachungen vorgesehen ist. Dieser Abschnitt enthält Anweisungen zum Installieren und Konfigurieren von Tools für die Entwicklung von Abnutzung, eine schrittweise Anleitung zum Erstellen Ihres ersten Abnutzung Geräts und eine Liste von Beispielen, die Sie verwenden können, um für die Erstellung Ihres eigenen apps Dach erforderlich.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Erste Schritte](~/android/wear/get-started/index.md)

Android Dach führt, wird beschrieben, wie zum Installieren und konfigurieren Sie den Computer für die Entwicklung von Abnutzung und enthält die Schritte zum Erstellen und Ausführen Ihrer erste Android Dach-app auf einem Emulator oder Abnutzung-Gerät.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Benutzeroberfläche](~/android/wear/user-interface/index.md)

Erläutert Android Abnutzung-spezifische steuert und enthält Links zu Beispielen, die veranschaulichen, wie Sie diese Steuerelemente verwenden.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Plattformfeatures](~/android/wear/platform/index.md)

Dokumente in diesem Abschnitt werden spezifische Funktionen für Android Dach behandelt. Hier finden Sie ein Thema, das beschreibt, wie eine WatchFace erstellt.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Bildschirmgrößen](~/android/wear/screen-sizes.md)

In der Vorschau anzeigen Sie, und Optimieren Sie Ihre Benutzeroberfläche für die verfügbaren Bildschirmgrößen.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Bereitstellung und Testen](~/android/wear/deploy-test/index.md)

Erläutert, wie Ihre app Android Dach ein Dach Android-Gerät oder auf Android-Emulator für Abnutzung konfiguriert bereitstellen. Er umfasst außerdem das Debuggen, Tipps und Informationen zum Einrichten einer Bluetooth-Verbindung zwischen Ihrem Computer und einem Android-Gerät.

##  <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Dach APIs](https://developer.android.com/reference/android/support/wearable)

Android Developer-Website enthält ausführliche Informationen zu wichtigsten Dach APIs wie z. B. [Wearable Aktivität](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [Intents](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [Authentifizierung](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [ Komplikationen](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [Komplikationen Rendern](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [Benachrichtigungen](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [Ansichten](https://developer.android.com/reference/android/support/wearable/view/package-summary.html), und [WatchFace](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Proben

Sie erhalten eine Anzahl von [Beispiele](https://developer.xamarin.com/samples/android/Android%20Wear/) mit Android Dach (oder direkt [Github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Beispiel|Beschreibung|Bildschirmabbildung|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|Ein einfaches Beispiel der Grundlagen der wearable-Projekte, einschließlich GridViewPager und interaktive Benachrichtigungen.|![Screenshot des Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Ein einfaches Demo des WatchViewStub-Steuerelements, die Form "Bildschirm" erkennt und lädt automatisch die richtige Layout.  Funktionsweise des WatchViewStub in der **Resources/layout/main_actvity.xml** Layout.|![Screenshot des WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Demo der Abnutzung Benachrichtigung Seiten in Form von Rezept Schritte. Benachrichtigungen werden in RecipeService.cs erstellt.|![Screenshot des RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|Arbeitserleichterung Beispiel der Interaktion mit "Personal"Assistant aufgerufen Eliza, erstellen eine Konversation mithilfe von programmierten Antworten mithilfe von Abnutzung interaktive Benachrichtigungen.|![Screenshot des ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager implementiert das Muster für 2D Navigation, in denen der Benutzer Kundenkarte vertikal und horizontal zum Navigieren durch Optionen und Inhalt.|![Screenshot des GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace ist eine benutzerdefinierte Überwachung Schaltflächenoberseite Analog-Stil Stunde, Minute und der zweite Hände geraten. In diesem Beispiel wird veranschaulicht, wie zum Erstellen eines Watch-Face-Diensts, der die aktuelle Uhrzeit zeichnet und Handles ambient-Modus und Sichtbarkeit-Ereignissen Change. Es enthält einen broadcast Empfänger, der für zeitzonenänderungen überwacht und aktualisiert automatisch entsprechend die Zeit.|![Screenshot des WatchFace](images/gridviewpager.png)|


##  <a name="videos"></a>Videos

Sehen Sie sich diese video, Links, die mit Abnutzung Xamarin.Android behandeln unterstützen:

|Beschreibung|Bildschirmabbildung|
|--- |--- |
|[Android L und noch viel mehr](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; die Entwicklervorschau für Android L eingeführt, eine Fülle von neue APIs für Entwickler zu nutzen, einschließlich Material Entwurf, Benachrichtigungen und neue Animationen, um nur einige zu nennen.|![Bildschirmabbildung von der Präsentation Video](images/video-android-l.png)|
|[C# ist in meinen Ohren und Augen: Google Glass und Android Dach](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable computing scheint etwas aus der Zukunft (oder eine Folge von Inspektor-Gadget), aber viele Personen sind bereits Hinwendung Zukunft heute! C#-Entwickler wissen und haben bereits die Tools und Fertigkeiten, die Leistung der wearable Geräte (Evolve 2014) nutzen zu können.|![Bildschirmabbildung von der Präsentation Video](images/video-eyes-ears.png)|
|[Neuheiten bei Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android Dach, Android, TV, Android Auto, Material Entwurf und ClipArt; was dies tut Mean Ihnen als Entwickler Xamarin? von Evolve 2014.|![Bildschirmabbildung von der Präsentation Video](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
