---
title: Einführung in Android Wear
description: Mit der Einführung Googles Android Wear sind Sie nicht mehr auf nur Smartphones und Tablets beschränkt, wenn es darum geht, hervorragende Android-apps entwickeln. Der Xamarin.Android-Unterstützung für Android Wear ermöglicht es für die Ausführung von C# Code in die Finger! Diese Einführung bietet eine grundlegende Übersicht über Android Wear, beschreibt ihre wichtigsten Funktionen und bietet einen Überblick über die in Android Wear 2.0 verfügbaren Features. Es führt einige der gängigeren Geräte mit Android Wear, und es enthält Links zu essential Google Android Wear-Dokumentation für Weitere Informationen.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: a35cb82f4f6d20e91f45a782c73d3ef811947c3a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61284196"
---
# <a name="introduction-to-android-wear"></a>Einführung in Android Wear

_Mit der Einführung Googles Android Wear sind Sie nicht mehr auf nur Smartphones und Tablets beschränkt, wenn es darum geht, hervorragende Android-apps entwickeln. Der Xamarin.Android-Unterstützung für Android Wear ermöglicht es für die Ausführung von C# Code in die Finger! Diese Einführung bietet eine grundlegende Übersicht über Android Wear, beschreibt ihre wichtigsten Funktionen und bietet einen Überblick über die in Android Wear 2.0 verfügbaren Features. Es führt einige der gängigeren Geräte mit Android Wear, und es enthält Links zu essential Google Android Wear-Dokumentation für Weitere Informationen._


## <a name="overview"></a>Übersicht

Android Wear wird auf einer Vielzahl von Geräten, wie z.B. die erste Generation Motorola 360 LGs G-Überwachung und der Samsung Zahnradsymbol Live ausgeführt. Eine zweite Generation, einschließlich des Sony SmartWatch 3, hat auch mit zusätzlichen Funktionen, einschließlich integrierten GPS und offline Musikwiedergabe veröffentlicht wurde. Für Android Wear 2.0 Google hat mit zusammengetan, LG für zwei neue Überwachungen: die LG Watch Sport und LG Watch-Stil.

![Android Wear-2.0-Geräte](intro-to-wear-images/hero-image.png "Beispiel Android Wear-2.0-Geräte")

Xamarin.Android 5.0 und höher unterstützt Android Wear über unser Android 4.4W (20-API) unterstützen, und ein NuGet-Paket, das zusätzliche fügt Wear-spezifische UI-Steuerelemente. Xamarin.Android 5.0 und höher umfasst auch Funktionen zum Packen Ihrer Wear-apps. NuGet-Pakete stehen auch für Android Wear-2.0 wie weiter unten in diesem Handbuch beschrieben.


## <a name="android-wear-basics"></a>Android Wear-Grundlagen

Android Wear verfügt über ein Paradigma der Benutzer-Schnittstelle, die von der Android-handheld-apps unterscheidet. Die erste Welle von Wear-apps wurden entworfen, um eine begleitende erweitern handheld-app in bestimmter Weise aber ab Android Wear 2.0, Wear-apps verwendet eigenständige werden können. Wenn Sie eine Wear-app bereitstellen, wird es mit handheld Begleit-app verpackt. Da die meisten Wear apps hängen handheld Begleit-app, sie benötigen eine Möglichkeit zur Kommunikation mit handheld-apps. In den folgenden Abschnitten werden diese Nutzungsszenarien und beschreiben die wesentlichen Funktionen für Android Wear. 



### <a name="usage-scenarios"></a>Verwendungsszenarien

Die erste Version von Android Wear lag der Schwerpunkt hauptsächlich auf aktuelle handheld-Anwendungen mit erweiterten Benachrichtigungen erweitern und Synchronisieren von Daten zwischen der handheld-app und der tragbare app. Diese Szenarien sind daher relativ einfach implementieren.


#### <a name="wearable-notifications"></a>Tragbare Benachrichtigungen

Die einfachste Möglichkeit zur Unterstützung von Android Wear, ist der freigegebene Art von Benachrichtigungen zwischen dem Handheldgerät und das tragbare Gerät nutzen. Mithilfe von API-Unterstützung v4 Benachrichtigung und die `WearableExtender` Klasse (verfügbar in der [unterstützt Xamarin Android-Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), können Sie profitieren Sie von der nativen Features der Plattform, z. B. kartengestaltung Posteingang oder voice-Eingabe. Die [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) Beispiel enthält Beispielcode, der zeigt, wie Sie eine Liste der Benachrichtigungen an ein Android Wear-Gerät zu senden. 



#### <a name="companion-applications"></a>Companion-Anwendungen

Eine andere Strategie ist eine vollständige Anwendung zu erstellen, die wird systemeigen unter das tragbare Gerät ausgeführt und mit Begleit-app handheld-Paaren. Ein gutes Beispiel dieses Ansatzes ist die [Quiz](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) Beispiel-app, die veranschaulicht, wie Sie ein Quiz zu erstellen, die auf einem tragbaren Gerät ausgeführt wird, und fordert auf das tragbare Gerät Quizfragen. 



### <a name="user-interface"></a>Benutzeroberfläche

Das primäre navigationsmuster für Verschleiß ist eine Reihe von Karten, die vertikal angeordnet. Jede dieser Karten kann Aktionen zugeordnet, die sich auf derselben Zeile Ebenen auf. Die `GridViewPager` Klasse bietet folgende Funktionen; er entspricht, auf das gleiche Konzept der Adapter als `ListView`. Ordnen Sie Sie in der Regel die `GridViewPager` mit einer `FragmentGridPagerAdaptor` (oder `GridPagerAdaptor`), mit der Sie die einzelnen Zellen in Zeilen- und als darstellen einer `Fragment`: 

[![Wear-Navigation](intro-to-wear-images/2d-picker-sml.png "Wear-Navigation")](intro-to-wear-images/2d-picker.png#lightbox)

Wear auch mithilfe von Aktionsschaltflächen, die aus einem großen bestehen Kreis mit kleinen Beschreibungstext darunter (wie oben dargestellt) gefärbt ist.  Die [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) Beispiel veranschaulicht, wie `GridViewPager` und `GridPagerAdapter` in einer Wear-app.

Android Wear 2.0 werden die Wear-Benutzeroberfläche eine Navigations-Drawer, eine Aktion Drawer und Inline-Aktionsschaltflächen hinzugefügt. Weitere Informationen zu Android Wear-2.0-Benutzeroberflächenelemente, finden Sie unter Android [Aufbau](https://www.google.com/design/spec-wear/system-overview/anatomy.html) Thema. 



### <a name="communications"></a>Kommunikation

Android Wear bietet zwei verschiedene-Kommunikations-APIs, um die Kommunikation zwischen apps für tragbare Geräte und tragbare Begleit-apps zu vereinfachen: 

**Daten-API** &ndash; diese API ist eine synchronisierte Datenspeicher zwischen dem tragbare Gerät und dem Handheldgerät ähnelt. Android übernimmt das Weitergeben von Änderungen zwischen tragbare und handheld, wenn sie dazu eine optimale ist. Wenn das tragbare Gerät außerhalb des gültigen Bereichs befindet, wird die Synchronisierung für einen späteren Zeitpunkt Warteschlange. Der Haupteinstiegspunkt für diese API ist `WearableClass.DataApi`. Weitere Informationen zu dieser API finden Sie im Android [Synchronisierung Datenelemente](https://developer.android.com/training/wearables/data-layer/data-items.html) Thema. 

**Message-API-** &ndash; diese API ermöglicht es Ihnen die Verwendung einen niedrigeren Ebene Kommunikationspfad: eine kleine Nutzlast wird unidirektionale Kommunikation ohne Synchronisierung zwischen den apps Handheld- und tragbare gesendet.
Der Haupteinstiegspunkt für diese API ist `WearableClass.MessageApi`.
Weitere Informationen zu dieser API finden Sie im Android [senden und Empfangen von Nachrichten](https://developer.android.com/training/wearables/data-layer/messages.html) Thema.

Sie können auswählen, zum Registrieren von Rückrufen für den Empfang von Nachrichten über jede der Schnittstellen-API-Listener oder alternativ Implementieren eines Diensts in Ihrer app, die von abgeleitet `WearableListenerService`.
Dieser Dienst wird automatisch vom Android Wear instanziiert werden.
Die [FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) Beispiel veranschaulicht das Implementieren einer `WearableListenerService`.



### <a name="deployment"></a>Bereitstellung

Jede tragbare app wird mit eigenen APK-Datei, die in der hauptanwendung APK eingebettet bereitgestellt. Diese paketerstellung erfolgt automatisch in Xamarin.Android 5.0 oder höher, aber Sie muss manuell durchgeführt werden, für die Versionen von Xamarin.Android vor Version 5.0. 
[Arbeiten mit der Paketerstellung](~/android/wear/deploy-test/packaging.md) Bereitstellung ausführlicher erläutert. 



## <a name="going-further"></a>Weiterführende Themen 

Die beste Möglichkeit, die mit Android Wear vertraut ist zum Erstellen und Testen Ihre erste app. Die folgende Liste enthält eine empfohlene Lesereihenfolge lassen sich schnell an:

1.  [Setup und Installation](~/android/wear/get-started/installation.md) enthält detaillierte Anweisungen zum Installieren und Konfigurieren der Entwicklungsumgebung zum Erstellen von Xamarin.Android Wear-apps. 

2.  Nachdem Sie die erforderlichen Pakete installiert und konfiguriert werden, einem Emulator oder Gerät haben, finden Sie unter [Hallo Wear](~/android/wear/get-started/hello-wear.md) für schrittweise Anweisungen dazu, wie zum Erstellen von eines kleinen Projekts für Android Wear, Handles-Schaltfläche klickt, und zeigt eine Klicken Sie auf den Zähler der Wear-Gerät. 

3.  [Bereitstellung und Tests](~/android/wear/deploy-test/index.md) bietet Ausführlichere Informationen zum Konfigurieren und bereitstellen, Emulatoren und Geräten, einschließlich Anweisungen zum Bereitstellen Ihrer app auf einem Wear-Gerät über Bluetooth.

4.  [Arbeiten mit Bildschirmen](~/android/wear/screen-sizes.md) wird erläutert, wie Sie eine Vorschau anzeigen und die Benutzeroberfläche für die verschiedenen verfügbaren Bildschirmgrößen auf Wear-Geräten zu optimieren. 

5.  [Arbeiten mit der Paketerstellung](~/android/wear/deploy-test/packaging.md) beschreibt die Schritte zum manuellen Packen Wear-apps für die Verteilung auf Google Play.

Nachdem Sie Ihre erste Wear-app erstellt haben, sollten Sie versuchen, erstellen eine benutzerdefinierte watchface für Android Wear. 
[Erstellen eines Zifferblatts](~/android/wear/platform/creating-a-watchface.md) bietet schrittweise Anleitungen und Beispielcode für die Entwicklung von digitalen sehen Sie sich Gesicht-Diensts, gefolgt von weiteren Code, um ein Pendant-Stil watchface mit zusätzlichen Funktionen verbessert, die keinen Fall eine gekürzte. 



## <a name="android-wear-20"></a>Android Wear 2.0

Android Wear 2.0 führt zu einer Vielzahl von neuen Features und Funktionen, z. B. *Komplikationen*, gekrümmt, Layouts, Navigation und Aktion drawern und erweiterten Benachrichtigungen. Darüber hinaus erleichtert Wear 2.0 können Sie eigenständige Anwendungen erstellen, die unabhängig von handheld-apps zu arbeiten. Die neue *Handgelenk Gesten* ermöglicht die einhändige Interaktionen mit Ihrer app. In den folgenden Abschnitten markieren diese Funktionen und angeben, dass Links können Sie mit der Nutzung in Ihrer app beginnen.



### <a name="install-wear-20-packages"></a>Wear-2.0-Pakete installieren

Sie müssen zum Erstellen einer 2.0 Wear-app, die mit Xamarin.Android Hinzufügen der **Xamarin.Android.Wear v2. 0** Paket Ihrem Projekt (klicken Sie auf die **Registerkarte "Durchsuchen"**):

[![Xamarin.Android.Wear v2. 0](intro-to-wear-images/wear-nuget-2.0-sml.png "Installieren von NuGet das Xamarin.Android.Wear-v2. 0")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Dieses NuGet-Paket enthält die Bindungen für tragbare für Android unterstützt sowohl die Wear Compat-Bibliotheken.

Zusätzlich zu **Xamarin.Android.Wear**, es wird empfohlen, die Sie installieren die **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Wichtige Features von Wear 2.0

Android Wear 2.0 ist das größte Update zu Android Wear, seit der ersten Einführung im Jahr 2014 vornehmen. Den folgenden Abschnitten werden die wichtigsten Features von Android Wear 2.0 und Links werden bereitgestellt, um zu ersten Schritten mit diesen neuen Features in Ihrer app. 


#### <a name="complications"></a>Komplikationen

*Komplikationen* sind kleine Watch gesichtserkennungs-Widgets, die Sie auf einen Blick sehen können, ohne dass auf dem Zifferblatt Ihrer Apple Watch Wischen. Komplikationen ähneln Desktop-Stil dashboardwidgets. Sie zeigen Informationen wie z. B. das Wetter, Akkuverbrauch gering zu halten, Termine im Kalender und Fitness-app-Statistiken: 

![Beispiel für schwierigkeiten](intro-to-wear-images/complications.png "Komplikationen-Beispiel")

Weitere Informationen zu Komplikationen, finden Sie in der Android [Watch Gesicht Komplikationen](https://developer.android.com/wear/preview/features/complications.html) Thema. 



#### <a name="navigation-and-action-drawers"></a>Navigation und Aktion Drawern 

Zwei neue drawern sind in Wear 2.0 enthalten. Die *Navigations-Drawer*, die am oberen Rand des Bildschirms angezeigt wird, können Benutzer zwischen den Ansichten der app navigieren (siehe unten links). Die *Aktion Drawer*, die am unteren Rand des Bildschirms (wie rechts gezeigt) angezeigt wird, können Benutzer aus einer Liste von Aktionen auswählen. 

![Navigation und Aktion Drawern](intro-to-wear-images/drawers.png "Navigation und Aktion Drawern")

Weitere Informationen zu diesen zwei neuen interaktive Schubladen, finden Sie unter Android [Wear-Navigation und Aktionen](https://developer.android.com/wear/preview/features/ui-nav-actions.html) Thema. 



#### <a name="curved-layouts"></a>Gekrümmte Layouts 

Wear 2.0 werden neue Features für die Anzeige von gekrümmten Layouts auf round Wear-Geräten eingeführt. Insbesondere das neue `WearableRecyclerView` Klasse ist für die eine Liste der vertikalen Elemente anzeigt, auf die Runde Anzeige optimiert: 

![Beispiel für einen gekrümmt](intro-to-wear-images/curved-layout.png "gekrümmten Layout-Beispiel")

`WearableRecyclerView` Erweitert die `RecyclerView` Klasse gekrümmte Layouts und zirkuläre Bildlauf Gesten unterstützt. Weitere Informationen finden Sie im Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) -API-Dokumentation. 



#### <a name="standalone-apps"></a>Eigenständige Anwendungen 

Android Wear-2.0-apps können unabhängig von handheld-apps arbeiten. Bedeutet dass, kann z. B. eine intelligenten Überwachung weiterhin vollständige Funktionalität zu bieten, selbst wenn das zugehörige handheld-Gerät deaktivieren oder weit entfernt vom tragbare Gerät aktiviert ist. Weitere Informationen zu diesem Feature finden Sie im Android [eigenständige Apps](https://developer.android.com/wear/preview/features/standalone-apps.html) Thema.



#### <a name="wrist-gestures"></a>Finger-Bewegungen 

Finger-Bewegungen können sie Benutzer mit Ihrer app interagieren, ohne den Touchscreen &ndash; Benutzer für die app mit der ein einzelnes Black reagieren können. Zwei-Finger-Bewegungen werden unterstützt: 

-   Flick Handgelenk out
-   Flick Handgelenk in

Weitere Informationen finden Sie im Android [Handgelenk Gesten](https://developer.android.com/wear/preview/features/gestures.html) Thema. 


Es gibt viele weitere Wear 2.0-Features wie z. B. inlineaktionen, smart Reply, remoteeingabe, erweiterten Benachrichtigungen und einen neuen bridging-Modus für Benachrichtigungen. Weitere Informationen zu den neuen Wear-2.0-Features finden Sie im Android [-API – Übersicht](https://developer.android.com/wear/preview/api-overview.html). 



## <a name="devices"></a>Geräte

Hier sind einige Beispiele für die Geräte, die Android Wear ausgeführt werden können:

* [Motorola 360](https://moto360.motorola.com/)
* [LG G ansehen](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G Watch R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung Gear Live](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>Weiterführende Themen

Sehen Sie sich Googles Android Wear-Dokumentation:

* [Informationen zu Android Wear](http://www.android.com/wear/)
* [Android Wear-App-Design](https://developer.android.com/design/wear/index.html)
* [android.support.wearable library ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android Wear 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>Zusammenfassung

In dieser Einführung werden eine Übersicht über Android Wear bereitgestellt. Es erläutert die grundlegenden Funktionen von Android Wear und enthalten eine Übersicht über die Funktionen, die in Android Wear 2.0 eingeführt wurde. Diese angegeben, dass Links zu den wesentlichen lesen, damit Entwickler, die mit Xamarin.Android Wear-Entwicklung beginnen können, und sie Beispiele für die Android Wear-Geräte derzeit auf dem Markt aufgeführt.


## <a name="related-links"></a>Verwandte Links

- [Installation und Einrichtung](~/android/wear/get-started/installation.md)
- [Erste Schritte](~/android/wear/get-started/index.md)
