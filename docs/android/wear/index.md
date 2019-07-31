---
title: Android Wear
description: Entwickeln von Apps für Android-tragbaren-Geräte.
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/16/2018
ms.openlocfilehash: 13a9a02984828781cc6402e982a4b5151c077c52
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648328"
---
# <a name="android-wear"></a>Android Wear

Android Wear ist eine Version von Android, die für tragbaren-Geräte wie Smart Watches konzipiert ist. Dieser Abschnitt enthält Anweisungen zum Installieren und Konfigurieren von Tools, die für die Verlaufs Entwicklung erforderlich sind, eine schrittweise exemplarische Vorgehensweise zum Erstellen Ihres ersten Wear-Geräts sowie eine Liste der Beispiele, auf die Sie zum Erstellen eigener Wear-Apps verweisen können.

## <a name="getting-startedandroidwearget-startedindexmd"></a>[Erste Schritte](~/android/wear/get-started/index.md)

Enthält eine Einführung in Android Wear, beschreibt die Installation und Konfiguration Ihres Computers für die Wear-Entwicklung und erläutert die Schritte, die Sie beim Erstellen und Ausführen ihrer ersten Android Wear-App auf einem Emulator oder einem Wear-Gerät unterstützen.

## <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Benutzeroberfläche](~/android/wear/user-interface/index.md)

Beschreibt Wear OS spezifische Steuerelemente und stellt Links zu Beispielen zur Nutzung von Steuerelementen bereit.

## <a name="platform-featuresandroidwearplatformindexmd"></a>[Plattformfeatures](~/android/wear/platform/index.md)

In den Dokumenten in diesem Abschnitt werden die spezifischen Features von Android Wear behandelt. Hier finden Sie ein Thema, in dem die Erstellung eines watchface beschrieben wird.

## <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Bildschirmgrößen](~/android/wear/screen-sizes.md)

Vorschau und Optimierung Ihrer Benutzeroberfläche für die verfügbaren Bildschirmgrößen.

## <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Bereitstellung und Testen](~/android/wear/deploy-test/index.md)

Erläutert, wie Sie Ihre Android Wear-App auf einem Android Wear-Gerät oder im Android-Emulator bereitstellen, der für das Wear konfiguriert ist Außerdem sind Tipps zum Debuggen und Informationen zum Einrichten einer Bluetooth-Verbindung zwischen dem Entwicklungs Computer und einem Android-Gerät enthalten.

## <a name="wear-apishttpsdeveloperandroidcomreferenceandroidsupportwearable"></a>[Wear-APIs](https://developer.android.com/reference/android/support/wearable)

Die Android-Entwickler Website bietet ausführliche Informationen zu Key Wear-APIs, wie z. b. [Wearable Activity](https://developer.android.com/reference/android/support/wearable/activity/package-summary.html), [Intents](https://developer.android.com/reference/com/google/android/wearable/intent/package-summary.html), [Authentifizierung](https://developer.android.com/reference/android/support/wearable/authentication/package-summary.html), [Komplikationen](https://developer.android.com/reference/android/support/wearable/complications/package-summary.html), [Komplikationen Rendering](https://developer.android.com/reference/android/support/wearable/complications/rendering/package-summary.html), [Benachrichtigungen](https://developer.android.com/reference/android/support/wearable/notifications/package-summary.html), [ Sichten](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)und [watchface](https://developer.android.com/reference/android/support/wearable/watchface/package-summary.html).



## <a name="samples"></a>Proben

Eine Reihe von [Beispielen](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Android+wear) finden Sie unter Verwendung von Android Wear (oder direkt zu [GitHub](https://github.com/xamarin/monodroid-samples/tree/master/wear)).

|Beispiel|Beschreibung|Bildschirmabbildung|
|--- |--- |--- |
|[SkeletonWear](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)|Ein einfaches Beispiel für die Grundlagen von webaubaren Projekten, einschließlich GridViewPager und interaktiver Benachrichtigungen.|![Bildschirm Abbildung von "skeletonwear"](images/skeleton.png)|
|[WatchViewStub](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchviewstub)|Eine einfache Demo des watchviewstub-Steuer Elements, das die Bildschirm Form erkennt und automatisch das richtige Layout lädt. Sehen Sie sich an, wie watchviewstub im Layout " **Resources/Layout/main_activity. XML** " funktioniert.|![Screenshot von watchviewstub](images/watchview.png)|
|[RecipeAssistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant)|Demonstration von Nutzungs Benachrichtigungs Seiten in Form von Rezept Schritten. Benachrichtigungen werden in RecipeService.cs erstellt.|![Bildschirm Abbildung von "Receive-Assistant"](images/recipeassist.png)|
|[ElizaChat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-elizachat)|Ein Beispiel für die Interaktion mit einem "persönlichen Assistenten" mit dem Namen "Eliza" und der Verwendung von interaktiven Benachrichtigungen zum Erstellen einer Konversation mithilfe von gespeicherten Antworten.|![Bildschirm Abbildung von elizachat](images/eliza.png)|
|[GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)|GridViewPager implementiert das 2D-Navigationsmuster, bei dem der Benutzer vertikal und dann horizontal zum Navigieren durch Optionen und Inhalte bewegt wird.|![Screenshot von GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)|Watchface ist ein benutzerdefiniertes Überwachungs Gesicht, das Stunden-, Minuten-und zweite Hände im analogen Stil ist. In diesem Beispiel wird veranschaulicht, wie ein Watch-Gesichts Dienst erstellt wird, der die aktuelle Uhrzeit zeichnet und Umgebungs Modus-und Sichtbarkeits Änderungs Ereignisse behandelt. Sie enthält einen Broadcast Empfänger, der auf Zeit Zonen Änderungen lauscht und die Uhrzeit automatisch entsprechend aktualisiert.|![Screenshot von watchface](images/gridviewpager.png)|


## <a name="videos"></a>Videos

Sehen Sie sich diese Video Links an, in denen xamarin. Android mit Wear-Unterstützung erläutert wird:

|Beschreibung|Bildschirmabbildung|
|--- |--- |
|[Android L und vieles mehr](https://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; In der Android L Developer Preview wurden zahlreiche neue APIs vorgestellt, die Entwickler nutzen können, einschließlich Material Design, Benachrichtigungen und neuen Animationen, um nur einige zu nennen.|![Screenshot der Präsentation](images/video-android-l.png)|
|[C#in meiner Augen Ansicht: Google Glass und Android Wear](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; Wearable Computing könnte wie etwas von der Zukunft (oder eine Inspektor-Gadget-Episode) erscheinen, aber viele Leute sind heute bereits in der Zukunft. C#Entwickler kennen dies und verfügen bereits über die Tools und Fähigkeiten, mit denen sich die Leistung von webaugeräten (von weiterentwickelt 2014) zunutze machen lassen.|![Screenshot der Präsentation](images/video-eyes-ears.png)|
|[Neues in xamarin. Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, Android Wear, Android TV, Android Auto, Material Design und Art. Was bedeutet das für Sie als xamarin-Entwickler? von Evolve 2014.|![Screenshot der Präsentation](Images/video-whats-new.png)|


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
