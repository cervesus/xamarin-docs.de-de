---
title: Einführung in Android Abnutzung
description: Mit der Einführung von Google Android Dach sind Sie nicht mehr auf nur-Telefone und Tablets beschränkt, .NET-Sprachen, wenn es um hervorragende Android-apps entwickeln. Der Xamarin.Android-Unterstützung für Android Dach ermöglicht Sie C#-Code auf Ihrem Handgelenk ausführen! Diese Einführung bietet eine grundlegende Übersicht über Android Dach, beschreibt ihre wichtigsten Funktionen und bietet eine Übersicht über die Features, die in Android Dach 2.0 verfügbar sind. Es werden einige der gängigeren Dach Android-Geräte aufgeführt, und bietet Links zu den wesentlichen Google Android Dach Dokumentation weitere Informationen.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 0ab166bb71c23d456cb70d35a2794717110642fd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772093"
---
# <a name="introduction-to-android-wear"></a>Einführung in Android Abnutzung

_Mit der Einführung von Google Android Dach sind Sie nicht mehr auf nur-Telefone und Tablets beschränkt, .NET-Sprachen, wenn es um hervorragende Android-apps entwickeln. Der Xamarin.Android-Unterstützung für Android Dach ermöglicht Sie C#-Code auf Ihrem Handgelenk ausführen! Diese Einführung bietet eine grundlegende Übersicht über Android Dach, beschreibt ihre wichtigsten Funktionen und bietet eine Übersicht über die Features, die in Android Dach 2.0 verfügbar sind. Es werden einige der gängigeren Dach Android-Geräte aufgeführt, und bietet Links zu den wesentlichen Google Android Dach Dokumentation weitere Informationen._


## <a name="overview"></a>Übersicht

Android Abnutzung wird auf einer Vielzahl von Geräten, einschließlich der ersten Generation Motorola 360, LGs G überwachen und die Samsung "Zahnrad"-Symbols Live ausgeführt. Eine zweite Generation, einschließlich des Sony SmartWatch 3, wurde auch mit zusätzlichen Funktionen, einschließlich integrierte GPS und offline Musikwiedergabe freigegeben. Für Android Dach 2.0, Google wurde zusammen mit LG für zwei neue Überwachungen: die LG Überwachungsfenster Sport und den LG Watch-Stil.

![Dach 2.0-Android-Geräte](intro-to-wear-images/hero-image.png "Beispiel Android Dach 2.0-Geräte")

Xamarin.Android 5.0 und höher unterstützt Android Dach über unsere Android 4.4W (API-20) unterstützen, und ein NuGet-Paket, das zusätzliche fügt Abnutzung-spezifischen Benutzeroberflächen-Steuerelemente. Xamarin.Android 5.0 und höher enthält auch Funktionen für Ihre apps Abnutzung verpacken. NuGet-Pakete stehen auch für Android Dach 2.0 wie weiter unten in diesem Handbuch beschrieben.


## <a name="android-wear-basics"></a>Android Abnutzung-Grundlagen

Android Abnutzung hat ein Benutzer Schnittstelle Paradigma, die von der Android-handheld-apps unterscheidet. Die erste Wave Abnutzung Apps wurden entwickelt, um eine begleitende erweitern handheld-app in bestimmter Weise aber beginnend mit Android Abnutzung 2.0, Abnutzung apps können eigenständig verwendet werden. Wenn Sie eine app Abnutzung bereitstellen, wird es mit einer Begleit-handheld-app verpackt. Da die meisten Dach apps hängen von einer handheld Begleit-app, wenn sie benötigen in irgendeiner Weise mit handheld-apps kommunizieren. In den folgenden Abschnitten beschreiben diese Szenarien für die Verwendung und die wesentlichen Android Dach Features vorgestellt. 



### <a name="usage-scenarios"></a>Verwendungsszenarien

Die erste Version von Android Dach ging es in erster Linie in aktuellen Handheld-Applikationen mit erweiterten Benachrichtigungen erweitern und Synchronisieren von Daten zwischen der handheld-app und der wearable app. Diese Szenarien sind daher relativ einfach zu implementieren.


#### <a name="wearable-notifications"></a>Wearable Benachrichtigungen

Die einfachste Möglichkeit zur Unterstützung von Android Dach ist auf den freigegebenen Charakter Benachrichtigungen zwischen dem Gerät und dem wearable Gerät nutzen. Mithilfe der Support-v4-benachrichtigungs API und die `WearableExtender` Klasse (verfügbar in der [unterstützt Xamarin Android-Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), können Sie in die systemeigenen Funktionen von der Plattform, z. B. Posteingang Stil Karten tippen oder voice-Eingabe. Die [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) Beispiel enthält Beispielcode, der veranschaulicht, wie eine Liste der Benachrichtigungen an einem Dach Android-Gerät gesendet. 



#### <a name="companion-applications"></a>Begleit-Anwendungen

Eine andere Strategie besteht darin, eine vollständige Anwendung zu erstellen, die ausgeführt wird, auf dem Gerät wearable systemintern und mit einer Begleit-app handheld-Paaren. Ein gutes Beispiel dieses Ansatzes ist die [Quiz](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) Beispiel-app, die veranschaulicht, wie Sie ein Quiz erstellen, die auf einem Handheldgerät ausgeführt wird und Quiz Fragen auf dem Gerät wearable. 



### <a name="user-interface"></a>Benutzeroberfläche

Das Muster primäre Navigation für Abnutzung ist eine Reihe von Karten, die vertikal angeordnet. Jede dieser Karten kann Aktionen zugeordnet, die, in der gleichen Zeile angeordnet sind. Die `GridViewPager` Klasse bietet folgende Funktionen; dasselbe Konzept Adapter wie folgt `ListView`. Ordnen Sie Sie in der Regel die `GridViewPager` mit einer `FragmentGridPagerAdaptor` (oder `GridPagerAdaptor`), mit der Sie die einzelnen Zeilen- und Zellen als darstellen einer `Fragment`: 

[![Navigation Dach](intro-to-wear-images/2d-picker-sml.png "Dach Navigation")](intro-to-wear-images/2d-picker.png#lightbox)

Dach auch macht, verwenden von Aktionsschaltflächen, die aus einer großen bestehen Kreis mit kleinen Beschreibungstext darunter (wie oben dargestellt gefärbt).  Die [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) Beispiel veranschaulicht, wie `GridViewPager` und `GridPagerAdapter` in einer app Abnutzung.

Android Dach 2.0 werden die Benutzeroberfläche Abnutzung eine Navigation zu einer Aktion zu und Inline-Aktionsschaltflächen hinzugefügt. Weitere Informationen über Benutzeroberflächenelemente Android Dach 2.0 finden Sie unter den Android [Aufbau](https://www.google.com/design/spec-wear/system-overview/anatomy.html) Thema. 



### <a name="communications"></a>Kommunikation

Android Abnutzung bietet zwei verschiedene Kommunikation APIs für die Kommunikation zwischen wearable und begleitenden handheld-apps zu erleichtern: 

**Daten-API** &ndash; diese API ist eine synchronisierte Datenspeicher zwischen dem wearable Gerät und dem Handheldgerät ähnelt. Android übernimmt das Weitergeben von Änderungen zwischen wearable und handheld, wenn er dazu optimal ist. Wenn die tragbar außerhalb des gültigen Bereichs ist, wird die Synchronisierung für einen späteren Zeitpunkt Warteschlange. Der Haupteinstiegspunkt für diese API ist `WearableClass.DataApi`. Weitere Informationen zu dieser API finden Sie unter den Android [Synchronisierung Datenelemente](https://developer.android.com/training/wearables/data-layer/data-items.html) Thema. 

**Message-API-** &ndash; diese API ermöglicht Ihnen die Verwendung einen niedrigeren Ebene Kommunikationspfad: eine kleine Nutzlast unidirektionale Kommunikation ohne Synchronisierung zwischen apps Hand- und wearable gesendet.
Der Haupteinstiegspunkt für diese API ist `WearableClass.MessageApi`.
Weitere Informationen zu dieser API finden Sie unter den Android [senden und Empfangen von Nachrichten](https://developer.android.com/training/wearables/data-layer/messages.html) Thema.

Sie können auswählen, um Rückrufe für den Empfang von Nachrichten über jede der API-Listenerschnittstellen registrieren oder alternativ Implementieren eines Diensts in Ihrer app, die abgeleitet `WearableListenerService`.
Dieser Dienst wird automatisch von Android Dach instanziiert werden.
Die [FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) Beispiel veranschaulicht das Implementieren einer `WearableListenerService`.



### <a name="deployment"></a>Bereitstellung

Jede wearable app wird mit einer eigenen APK-Datei eingebettet, die innerhalb der hauptanwendung APK bereitgestellt. Dieses Paket erfolgt automatisch bei Xamarin.Android 5.0 und höher, aber Sie müssen manuell für Xamarin.Android-Versionen vor Version 5.0 ausgeführt werden. 
[Arbeiten mit Verpackung](~/android/wear/deploy-test/packaging.md) Bereitstellung im Detail erläutert. 



## <a name="going-further"></a>Weiterführende Themen 

Die beste Möglichkeit, die mit Android Dach vertraut ist, erstellen und Testen Ihrer erste app. Die folgende Liste enthält eine empfohlene Lesefolge damit Sie schnell zu beschleunigen zugreifen können:

1.  [Einrichtung und Installation](~/android/wear/get-started/installation.md) enthält detaillierte Anweisungen zum Installieren und Konfigurieren der Entwicklungsumgebung zum Erstellen von Xamarin.Android Abnutzung apps. 

2.  Nachdem Sie die erforderlichen Pakete installiert und einem Emulator oder Gerät konfiguriert haben, finden Sie unter [Hallo, Abnutzung](~/android/wear/get-started/hello-wear.md) für-Schritt-Anweisungen, die erläutern, wie ein kleines Dach Android-Projekt die Handles-Schaltfläche erstellt klickt und zeigt eine Klicken Sie auf dem Gerät Abnutzung Leistungsindikator aus. 

3.  [Bereitstellung und Tests](~/android/wear/deploy-test/index.md) bietet Ausführlichere Informationen zum Konfigurieren und bereitstellen, Emulatoren und Geräte, einschließlich Anweisungen zum Bereitstellen Ihrer app auf einem Gerät Abnutzung über Bluetooth.

4.  [Arbeiten mit Bildschirmgrößen](~/android/wear/screen-sizes.md) wird erläutert, wie in der Vorschau anzeigen und Optimieren Sie Ihre Benutzeroberfläche für die verschiedenen verfügbaren Bildschirmgrößen auf Abnutzung Geräten. 

5.  [Arbeiten mit Verpackung](~/android/wear/deploy-test/packaging.md) beschreibt die Schritte zum Verpacken manuell Abnutzung-apps für die Verteilung auf Google Play.

Nachdem Sie Ihre erste Abnutzung app erstellt haben, empfiehlt es sich, eine benutzerdefinierte Überwachung Gesicht für Android Dach TestBuild. 
[Erstellen eine Zifferblatt Watch](~/android/wear/platform/creating-a-watchface.md) bietet schrittweise Anweisungen sowie einen Beispielcode für die Entwicklung einer entfernten digitale Überwachungsfenster Gesicht Dienst, gefolgt von weiteren Code, der es eine analoge-Stil überwachen-Oberfläche mit zusätzlichen Funktionen verbessert. 



## <a name="android-wear-20"></a>Android Wear 2.0

Android Dach 2.0 führt zu einer Vielzahl von neuen Features und Funktionen, wie z. B. *Komplikationen*, gekrümmte Layouts, Navigation und Aktion Fächer und erweiterten Benachrichtigungen. Darüber hinaus vereinfacht Dach 2.0 für die Sie zum Erstellen von eigenständigen apps, die unabhängig von handheld-apps funktionieren. Die neue *Handgelenks-Gesten* ermöglicht von Interaktionen mit der app. In den folgenden Abschnitten veranschaulichen die diese Funktionen und Links helfen Ihnen beim Einstieg, in deren Verwendung in Ihrer app bieten.



### <a name="install-wear-20-packages"></a>Install Dach 2.0 Pakete

Sie müssen zum Erstellen einer app Dach 2.0 mit Xamarin.Android Hinzufügen der **Xamarin.Android.Wear v2. 0** Paket Ihrem Projekt (klicken Sie auf die **Registerkarte Durchsuchen**):

[![Xamarin.Android.Wear v2. 0](intro-to-wear-images/wear-nuget-2.0-sml.png "der Xamarin.Android.Wear-NuGet v2. 0 installieren")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Dieses NuGet-Paket enthält die Bindungen für Android-Unterstützung Wearable sowohl die Dach Compat-Bibliotheken.

Zusätzlich zu **Xamarin.Android.Wear**, es wird empfohlen, die Sie installieren die **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Install the Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Hauptfunktionen von Abnutzung 2.0

Android Dach 2.0 ist die größte Update für Android Dach seit der ersten Einführung in 2014. In den folgenden Abschnitten markieren Sie die Hauptfunktionen von Android Dach 2.0, und Links werden bereitgestellt, um zu ersten Schritten mit diesen neuen Features in Ihrer app. 


#### <a name="complications"></a>Bereichsregeln

*Komplikationen* sind kleine Überwachungsfenster Gesicht-Widgets, mit denen Sie auf einen Blick sehen können, ohne dass auf dem Zifferblatt der Uhr Streifen. Komplikationen ähneln dashboardwidgets Desktop-Stil. Sie zeigen Informationen wie z. B. Weather, Akkulaufzeit, Kalenderereignisse und die Eignung app-Statistiken: 

![Beispiel für Komplikationen](intro-to-wear-images/complications.png "Komplikationen-Beispiel")

Weitere Informationen zu Komplikationen finden Sie in der Android [überwachen Gesicht Komplikationen](https://developer.android.com/wear/preview/features/complications.html) Thema. 



#### <a name="navigation-and-action-drawers"></a>Navigation und Aktion Fächer 

Zwei neue Fächer sind in Dach 2.0 enthalten. Die *Navigation zu*, das am oberen Rand der Seite angezeigt wird ermöglicht es Benutzern, Navigieren zwischen app-Ansichten (wie auf der linken Seite unten gezeigt). Die *Aktion zu*, die am unteren Bildschirmrand (wie auf der rechten Seite dargestellt), wird angezeigt, ermöglicht es Benutzern, die aus einer Liste von Aktionen auswählen. 

![Navigation und Aktion Fächer](intro-to-wear-images/drawers.png "Navigations- und Aktion Fächer")

Weitere Informationen zu diesen zwei neue interaktive Fächer, finden Sie unter den Android [Dach Navigations- und Aktionen](https://developer.android.com/wear/preview/features/ui-nav-actions.html) Thema. 



#### <a name="curved-layouts"></a>Gekrümmte Layouts 

Abnutzung 2.0 verfügt über neue Funktionen für die Anzeige von gekrümmten Layouts auf round Abnutzung-Geräten. Insbesondere die neuen `WearableRecyclerView` Klasse ist optimiert für Anzeigen einer Liste von vertikalen Elementen auf round zeigt: 

![Beispiel für einen gekrümmt](intro-to-wear-images/curved-layout.png "Layout gekrümmt, Beispiel")

`WearableRecyclerView` Erweitert die `RecyclerView` Klasse gekrümmte Layouts und zirkuläre Durchführen eines Bildlaufs Gesten unterstützt. Weitere Informationen finden Sie in der Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) API-Dokumentation. 



#### <a name="standalone-apps"></a>Eigenständige Apps 

Android Dach 2.0-apps können unabhängig von handheld-apps arbeiten. Bedeutet dass, kann z. B. eine intelligente Überwachung weiterhin volle Funktionalität bieten, selbst wenn dem Handheldgerät Companion deaktiviert oder weit vom wearable Gerät aktiviert ist. Weitere Informationen zu diesem Feature finden Sie unter den Android [eigenständige Apps](https://developer.android.com/wear/preview/features/standalone-apps.html) Thema.



#### <a name="wrist-gestures"></a>Handgelenks-Gesten 

Handgelenks-Gesten können Benutzer mit der app interagieren, ohne mit dem Touchscreen &ndash; Benutzer auf die app mit einer einzelnen Hand reagieren können. Zwei Handgelenks-Gesten werden unterstützt: 

-   Bewegung Handgelenks-out
-   Bewegung Handgelenks-in

Weitere Informationen finden Sie in der Android [Handgelenks-Gesten](https://developer.android.com/wear/preview/features/gestures.html) Thema. 


Es gibt viele weitere Dach 2.0-Funktionen, z. B. Inline-Aktionen, intelligente Antwort remote Eingabe, erweiterten Benachrichtigungen und einen neuen bridging-Modus für Benachrichtigungen. Weitere Informationen zu den neuen Dach 2.0-Features finden Sie unter den Android [Übersicht über die API](https://developer.android.com/wear/preview/api-overview.html). 



## <a name="devices"></a>Geräte

Hier sind einige Beispiele für die Geräte, auf denen Android Dach ausgeführt werden können:

* [Motorola 360](https://moto360.motorola.com/)
* [LG G Watch](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [LG G Watch R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung Gear Live](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>Weiterführende Themen

Sehen Sie sich Googles Android Dach Dokumentation:

* [Informationen zu Android Abnutzung](http://www.android.com/wear/)
* [Abnutzung Android-App-Design](https://developer.android.com/design/wear/index.html)
* [Android.Support.wearable-Bibliothek ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android Wear 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>Zusammenfassung

Diese Einführung eines Überblick über die Android Dach. Er erläutert die grundlegenden Funktionen von Android Dach und enthalten eine Übersicht über die Funktionen in Android Dach 2.0 eingeführt. Es Links zu den wesentlichen lesen, die Entwicklern Xamarin.Android Abnutzung Entwicklung Einstieg erleichtern bereitgestellt und sie Beispiele für einige der derzeit auf dem Markt Geräte Android Dach aufgeführt.


## <a name="related-links"></a>Verwandte Links

- [Installation und Einrichtung](~/android/wear/get-started/installation.md)
- [Erste Schritte](~/android/wear/get-started/index.md)
