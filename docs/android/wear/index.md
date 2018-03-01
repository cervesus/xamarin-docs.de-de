---
title: Android Wear
description: "Erstellen von apps für Android wearable Geräte aus."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1dad5e859efdf69e7003b45724f718b16faffd62
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="android-wear"></a>Android Wear

## <a name="android-wear"></a>Android Wear

Android Abnutzung ist eine Version von Android, die zur wearable Geräte wie z. B. intelligente Überwachungen vorgesehen ist. Dieser Abschnitt enthält Anweisungen zum Installieren und Konfigurieren von Tools für die Entwicklung von Abnutzung, eine schrittweise Anleitung zum Erstellen Ihres ersten Abnutzung Geräts und eine Liste von Beispielen, die Sie verwenden können, um für die Erstellung Ihres eigenen apps Dach erforderlich.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Erste Schritte](~/android/wear/get-started/index.md)

Android Dach führt, wird beschrieben, wie zum Installieren und konfigurieren Sie den Computer für die Entwicklung von Abnutzung und enthält die Schritte zum Erstellen und Ausführen Ihrer erste Android Dach-app auf einem Emulator oder Abnutzung-Gerät.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Benutzeroberfläche](~/android/wear/user-interface/index.md)

Erläutert Android Abnutzung-spezifische steuert und enthält Links zu Beispielen, die veranschaulichen, wie Sie diese Steuerelemente verwenden.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Plattformfunktionen](~/android/wear/platform/index.md)

Dokumente in diesem Abschnitt werden spezifische Funktionen für Android Dach behandelt. Hier finden Sie ein Thema, das beschreibt, wie eine WatchFace erstellt.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Bildschirmgrößen](~/android/wear/screen-sizes.md)

In der Vorschau anzeigen Sie, und Optimieren Sie Ihre Benutzeroberfläche für die verfügbaren Bildschirmgrößen.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Bereitstellung und Tests](~/android/wear/deploy-test/index.md)

Erläutert, wie Ihre app Android Dach ein Dach Android-Gerät oder auf Android-Emulator für Abnutzung konfiguriert bereitstellen. Er umfasst außerdem das Debuggen, Tipps und Informationen zum Einrichten einer Bluetooth-Verbindung zwischen Ihrem Computer und einem Android-Gerät.


<a name="Samples" />

## <a name="samples"></a>Proben

Sie erhalten eine Anzahl von [Beispiele](https://developer.xamarin.com/samples/android/Android%20Wear/) mit Android Dach (oder direkt [Github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>Beispiel</strong>
      </th>
      <th>
          <strong>Beschreibung</strong>
      </th>
      <th>
          <strong>bildschirmabbildung von</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
Ein einfaches Beispiel der Grundlagen der wearable-Projekte, einschließlich GridViewPager und interaktive Benachrichtigungen.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Ein einfaches Demo des WatchViewStub-Steuerelements, die Form "Bildschirm" erkennt und lädt automatisch die richtige Layout.
Funktionsweise des WatchViewStub in der <b>Resources/layout/main_actvity.xml</b> Layout.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Demo der Abnutzung Benachrichtigung Seiten in Form von Rezept Schritte. Benachrichtigungen werden erstellt, <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
Arbeitserleichterung Beispiel der Interaktion mit "Personal"Assistant aufgerufen Eliza, erstellen eine Konversation mithilfe von programmierten Antworten mithilfe von Abnutzung interaktive Benachrichtigungen.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager implementiert das Muster für 2D Navigation, in denen der Benutzer Kundenkarte vertikal und horizontal zum Navigieren durch Optionen und Inhalt.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> ist eine benutzerdefinierte Überwachung Schaltflächenoberseite Analog-Stil Stunde, Minute und der zweite Hände geraten. In diesem Beispiel wird veranschaulicht, wie zum Erstellen eines Watch-Face-Diensts, der die aktuelle Uhrzeit zeichnet und Handles ambient-Modus und Sichtbarkeit-Ereignissen Change. Es enthält einen broadcast Empfänger, der für zeitzonenänderungen überwacht und aktualisiert automatisch entsprechend die Zeit.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Videos

Sehen Sie sich diese video, Links, die mit Abnutzung Xamarin.Android behandeln unterstützen.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0"/ /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L und vieles mehr</a>
        <br />
Die Android L Developer Preview eingeführt, eine Fülle von neue APIs für Entwickler zu nutzen, einschließlich Material Entwurf, Benachrichtigungen und neue Animationen, um nur einige zu nennen.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# ist in meinen Ohren und Augen: Google Glass und Android Dach</a>
        <br />
Wearable computing scheint etwas aus der Zukunft (oder eine Folge von Inspektor-Gadget), aber viele Personen sind bereits Hinwendung Zukunft heute! C#-Entwickler wissen und haben bereits die Tools und Fertigkeiten, die Leistung der wearable Geräte (Evolve 2014) nutzen zu können.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" / /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Was ist neu in Xamarin.Android</a>
        <br />
        <i>Android L, Android Abnutzung, Android, TV, Android Auto, Material Entwurf und Grafiken; Was Sie als Entwickler Xamarin bedeutet dies? </i> aus weiterentwickelt 2014.</td>
    </tr>
</table>


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
