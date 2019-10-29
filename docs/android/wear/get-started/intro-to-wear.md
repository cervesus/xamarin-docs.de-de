---
title: Einführung in Android Wear
description: Mit der Einführung des Android Wear von Google sind Sie nicht mehr auf Smartphones und Tablets beschränkt, wenn es um die Entwicklung großartiger Android-Apps geht. Xamarin. Android-Unterstützung für Android Wear ermöglicht Ihnen das Ausführen C# von Code auf Ihrem Handgelenk! Diese Einführung bietet eine grundlegende Übersicht über Android Wear, beschreibt die wichtigsten Features und bietet einen Überblick über die Features, die in Android Wear 2,0 verfügbar sind. Darin sind einige der gängigeren Android Wear-Geräte aufgeführt. Außerdem finden Sie hier Links zu grundlegender Google Android Wear-Dokumentationen für weitere Informationen.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3b1d27b1489cb71d4bd1922c2de993567ddf36bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028620"
---
# <a name="introduction-to-android-wear"></a>Einführung in Android Wear

_Mit der Einführung des Android Wear von Google sind Sie nicht mehr auf Smartphones und Tablets beschränkt, wenn es um die Entwicklung großartiger Android-Apps geht. Xamarin. Android-Unterstützung für Android Wear ermöglicht Ihnen das Ausführen C# von Code auf Ihrem Handgelenk! Diese Einführung bietet eine grundlegende Übersicht über Android Wear, beschreibt die wichtigsten Features und bietet einen Überblick über die Features, die in Android Wear 2,0 verfügbar sind. Darin sind einige der gängigeren Android Wear-Geräte aufgeführt. Außerdem finden Sie hier Links zu grundlegender Google Android Wear-Dokumentationen für weitere Informationen._

## <a name="overview"></a>Übersicht

Android Wear kann auf einer Vielzahl von Geräten ausgeführt werden, z. b. in der ersten Generation von Motorola 360, LG G Watch und Samsung Gear Live. Eine zweite Generation, einschließlich der Smartwatch 3 von Apple, wurde ebenfalls mit zusätzlichen Funktionen veröffentlicht, einschließlich integrierter GPS-und Offline-Musikwiedergabe. Für Android Wear 2,0 hat sich Google mit LG für zwei neue Überwachungen zusammengetan: der LG Watch Sport und der LG-Überwachungs Stil.

![Android Wear 2,0-Geräte](intro-to-wear-images/hero-image.png "Beispiel: Android Wear 2,0-Geräte")

Xamarin. Android 5,0 und höher unterstützt Android-Wear über unsere Android 4.4 w (API 20)-Unterstützung und ein nuget-Paket, das zusätzliche Verschleiß spezifische UI-Steuerelemente hinzufügt. Xamarin. Android 5,0 und höher umfasst auch Funktionen zum Verpacken Ihrer Wear-apps. Nuget-Pakete sind auch für Android Wear 2,0 verfügbar, wie weiter unten in diesem Handbuch beschrieben.

## <a name="android-wear-basics"></a>Grundlagen zu Android Wear

Android Wear verfügt über ein Benutzeroberflächen Paradigma, das sich von Android-Apps unterscheidet. Die erste Welle von Wear-Apps wurde entwickelt, um eine begleitende Handheld-App auf irgendeine Weise zu erweitern, aber ab Android Wear 2,0 können Wear-apps eigenständig verwendet werden. Wenn Sie eine Wear-App bereitstellen, wird Sie mit einer begleitenden Hand Held-App verpackt. Da die meisten Wear-apps von einer begleitenden begleitenden App abhängen, benötigen Sie eine Möglichkeit, mit Handheld-apps zu kommunizieren. In den folgenden Abschnitten werden diese Verwendungs Szenarien beschrieben und die wesentlichen Features für Android-Wear erläutert. 

### <a name="usage-scenarios"></a>Verwendungsszenarien

Die erste Version von Android Wear konzentriert sich hauptsächlich auf die Erweiterung aktueller Handheld-Anwendungen mit erweiterten Benachrichtigungen und die Synchronisierung von Daten zwischen der Handheld-APP und der tragbaren-app. Daher ist es relativ einfach, diese Szenarien zu implementieren.

#### <a name="wearable-notifications"></a>Wearable-Benachrichtigungen

Die einfachste Möglichkeit zur Unterstützung von Android Wear besteht darin, die gemeinsame Nutzung von Benachrichtigungen zwischen dem Hand Held und dem tragbaren-Gerät zu nutzen. Mithilfe der Unterstützung der V4-Benachrichtigungs-API und der `WearableExtender`-Klasse (in der [xamarin Android-Unterstützungs Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar) können Sie auf die systemeigenen Features der Plattform tippen, wie z. b. Post fach Karten oder Spracheingaben. Das Beispiel " [recipeer Assistant](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-recipeassistant) " enthält Beispielcode, der veranschaulicht, wie eine Liste von Benachrichtigungen an ein Android Wear-Gerät gesendet wird. 

#### <a name="companion-applications"></a>Begleit Anwendungen

Eine andere Strategie besteht darin, eine komplette Anwendung zu erstellen, die System intern auf dem tragbaren-Gerät ausgeführt wird, und Paare mit einer begleitenden Handheld-app. Ein gutes Beispiel für diesen Ansatz ist die [Quiz](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-quiz) -Beispiel-APP, die veranschaulicht, wie ein Quiz erstellt wird, das auf einem Hand Held Gerät ausgeführt wird, und Quiz Fragen auf dem tragbaren-Gerät stellt. 

### <a name="user-interface"></a>Benutzeroberfläche

Das primäre Navigationsmuster für Wear ist eine vertikale Reihe von Karten. Jeder dieser Karten können Aktionen zugeordnet werden, die in derselben Zeile angeordnet werden. Die `GridViewPager`-Klasse stellt diese Funktionalität bereit. Sie entspricht dem gleichen Adapterkonzept wie `ListView`. In der Regel ordnen Sie die `GridViewPager` einem `FragmentGridPagerAdaptor` (oder `GridPagerAdaptor`) zu, mit dem Sie jede Zeilen-und Spalten Zelle als `Fragment` darstellen können: 

[![Wear-Navigation](intro-to-wear-images/2d-picker-sml.png "Wear-Navigation")](intro-to-wear-images/2d-picker.png#lightbox)

Wear verwendet auch Aktions Schaltflächen, die aus einem großen farbigen Kreis mit kleinen Beschreibungstext darunter stehen (wie oben gezeigt).  Das [GridViewPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager) -Beispiel veranschaulicht die Verwendung von `GridViewPager` und `GridPagerAdapter` in einer Wear-app.

Android Wear 2,0 fügt der Benutzeroberfläche "Wear" eine Navigations-, Aktions-und Inline Aktions Schaltfläche hinzu. Weitere Informationen zu Android Wear 2,0-Benutzeroberflächen Elementen finden Sie im Thema Android- [Anatomie](https://www.google.com/design/spec-wear/system-overview/anatomy.html) . 

### <a name="communications"></a>Kommunikation

Android Wear bietet zwei verschiedene Kommunikations-APIs, um die Kommunikation zwischen tragbaren apps und begleitenden Handheld-apps zu vereinfachen: 

**Data API** -&ndash; diese API ähnelt einem synchronisierten Datenspeicher zwischen dem webaugerät und dem Hand Held Gerät. Android übernimmt die Weitergabe von Änderungen zwischen tragbaren und Hand Held, wenn dies optimal ist. Wenn die tragbaren außerhalb des Bereichs liegt, wird die Synchronisierung für einen späteren Zeitpunkt in die Warteschlange eingereiht. Der Haupteinstiegspunkt für diese API ist `WearableClass.DataApi`. Weitere Informationen zu dieser API finden Sie im Thema Android- [Synchronisierungs Datenelemente](https://developer.android.com/training/wearables/data-layer/data-items.html) . 

**Message-API** -&ndash; diese API ermöglicht es Ihnen, einen Kommunikations Pfad auf niedrigerer Ebene zu verwenden: eine kleine Nutzlast wird unidirektional gesendet, ohne dass eine Synchronisierung zwischen den Handheld-und den tragbaren-apps erfolgt.
Der Haupteinstiegspunkt für diese API ist `WearableClass.MessageApi`.
Weitere Informationen zu dieser API finden Sie im Thema Android [Send and empfansnachrichten](https://developer.android.com/training/wearables/data-layer/messages.html) .

Sie können auch Rückrufe zum Empfangen dieser Nachrichten über jede API-listenerschnittstelle registrieren oder einen Dienst in der APP implementieren, der von `WearableListenerService` abgeleitet ist.
Dieser Dienst wird automatisch von Android Wear instanziiert.
Das [findmyphone](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-findmyphonesample) -Beispiel veranschaulicht, wie ein `WearableListenerService` implementiert wird.

### <a name="deployment"></a>Bereitstellung

Jede tragbaren APP wird mit einer eigenen APK-Datei bereitgestellt, die in das APK der Hauptanwendung eingebettet ist. Diese Verpackung wird automatisch in xamarin. Android 5,0 und höher behandelt, muss aber für Versionen von xamarin. Android vor Version 5,0 manuell ausgeführt werden. 
Beim [Arbeiten mit der Paket](~/android/wear/deploy-test/packaging.md) Erstellung wird die Bereitstellung ausführlicher erläutert. 

## <a name="going-further"></a>Weiterführende Themen 

Die beste Möglichkeit, sich mit Android Wear vertraut zu machen, besteht darin, Ihre erste APP zu erstellen und zu testen. Die folgende Liste enthält eine empfohlene Lesereihenfolge, mit der Sie schnell loslegen können:

1. [Setup & Installation](~/android/wear/get-started/installation.md) enthält ausführliche Anweisungen zum Installieren und Konfigurieren der Entwicklungsumgebung für die Erstellung von xamarin. Android Wear-apps. 

2. Nachdem Sie die erforderlichen Pakete installiert und einen Emulator oder ein Gerät konfiguriert haben, finden Sie unter [Hello, Wear](~/android/wear/get-started/hello-wear.md) eine Schritt-für-Schritt-Anleitung, in der erläutert wird, wie ein kleines Android Wear-Projekt erstellt wird, das Schaltflächen Klicks behandelt und einen Klick-Counter auf dem Wear anzeigt. Schutz. 

3. Das [Bereitstellungs & testet](~/android/wear/deploy-test/index.md) ausführlichere Informationen zum Konfigurieren und Bereitstellen von Emulatoren und Geräten, einschließlich Anweisungen zum Bereitstellen der APP auf einem Wear-Gerät über Bluetooth.

4. [Bei der Arbeit mit Bildschirmgrößen](~/android/wear/screen-sizes.md) wird erläutert, wie Sie die Benutzeroberfläche für die verschiedenen verfügbaren Bildschirmgrößen von Geräten in der Vorschau anzeigen und optimieren. 

5. Beim [Arbeiten mit der Paket](~/android/wear/deploy-test/packaging.md) Erstellung werden die Schritte zum manuellen Verpacken von Wear-Apps für die Verteilung auf Google Play beschrieben.

Nachdem Sie Ihre erste Wear-App erstellt haben, möchten Sie möglicherweise versuchen, ein benutzerdefiniertes Überwachungs Gesicht für Android Wear zu erstellen. 
Das [Erstellen eines Watch-Gesichts](~/android/wear/platform/creating-a-watchface.md) enthält schrittweise Anleitungen und Beispielcode für die Entwicklung eines abzurufenden Digital Watch-Gesichts dienstanweises, gefolgt von mehr Code, der ihn auf eine Uhr des analogen Stils mit zusätzlichen Features erweitert. 

## <a name="android-wear-20"></a>Android Wear 2,0

Android Wear 2,0 führt eine Reihe von neuen Features und Funktionen ein, wie z. b. *Komplikationen*, Kurven Layouts, Navigations-und Aktions-und erweiterte Benachrichtigungen. Außerdem ermöglicht Wear 2,0 das Erstellen eigenständiger apps, die unabhängig von Handheld-apps funktionieren. Die neue Funktion " *Handgelenk Gesten* " ermöglicht eindimensionale Interaktionen mit Ihrer APP. In den folgenden Abschnitten werden diese Features hervorgehoben, und es werden Links bereitgestellt, die Ihnen bei der Verwendung in Ihrer APP helfen.

### <a name="install-wear-20-packages"></a>Installieren von Wear 2,0-Paketen

Um eine Wear 2,0-App mit xamarin. Android zu erstellen, müssen Sie das **xamarin. Android. Wear v 2.0** -Paket zu Ihrem Projekt hinzufügen (Klicken Sie auf die **Registerkarte Durchsuchen**):

[![Xamarin. Android. Wear v 2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "Installieren Sie den nuget-Code für xamarin. Android. Wear v 2.0.")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Dieses nuget-Paket enthält Bindungen für die Android-Unterstützung für Wearable-und Wear-compat-Bibliotheken.

Zusätzlich zu **xamarin. Android. Wear**empfiehlt es sich, die **xamarin. googleplayservices. Wearable** -nuget zu installieren: 

[![Xamarin. googleplayservices. Wearable](intro-to-wear-images/gpsw-nuget-sml.png "Installieren des xamarin. googleplayservices. Wearable-nuget")](intro-to-wear-images/gpsw-nuget.png#lightbox)

### <a name="key-features-of-wear-20"></a>Wichtige Features von Wear 2,0

Android Wear 2,0 ist das größte Update von Android Wear seit dem ersten Start in 2014. In den folgenden Abschnitten werden die wichtigsten Features von Android Wear 2,0 hervorgehoben, und es werden Links bereitgestellt, die Ihnen den Einstieg in die Verwendung dieser neuen Features in Ihrer APP erleichtern. 

#### <a name="complications"></a>Komplikationen

*Komplikationen* sind kleine Watch-Gadgets, die Sie auf einen Blick sehen können, ohne das Gesicht schwenken zu müssen. Komplikationen ähneln dashboardwidgets im Desktop Stil. Sie zeigen Informationen wie z. b. Wetter, Akku Lebensdauer, Kalenderereignisse und Fitness-app-Statistiken an: 

![Beispiel für Komplikationen](intro-to-wear-images/complications.png "Beispiel für Komplikationen")

Weitere Informationen zu Komplikationen finden Sie im Thema Android [Watch Face Komplikationen](https://developer.android.com/wear/preview/features/complications.html) . 

#### <a name="navigation-and-action-drawers"></a>Navigations-und Aktions-Schub Schub 

In Wear 2,0 sind zwei neue Schubkarren enthalten. Der *Navigations*Bereich, der oben auf dem Bildschirm angezeigt wird, ermöglicht Benutzern das Navigieren zwischen App-Ansichten (wie auf der linken Seite dargestellt). Die *Aktions*Leiste, die unten auf dem Bildschirm angezeigt wird (wie auf der rechten Seite gezeigt), ermöglicht Benutzern die Auswahl aus einer Liste von Aktionen. 

![Navigations-und Aktions-Schub Schub](intro-to-wear-images/drawers.png "Navigations-und Aktions-Schub Schub")

Weitere Informationen zu diesen beiden neuen interaktiven Schub Zeugen finden Sie im Thema Android [Wear-Navigation und-Aktionen](https://developer.android.com/wear/preview/features/ui-nav-actions.html) . 

#### <a name="curved-layouts"></a>Kurven Layouts 

Mit "Wear 2,0" werden neue Features zum Anzeigen von Kurven Layouts auf runwear-Geräten eingeführt. Insbesondere die neue `WearableRecyclerView`-Klasse ist für die Anzeige einer Liste vertikaler Elemente in Runden anzeigen optimiert: 

![Beispiel für Kurven Layout](intro-to-wear-images/curved-layout.png "Beispiel für Kurven Layout")

`WearableRecyclerView` erweitert die `RecyclerView`-Klasse zur Unterstützung von gekrümmten Layouts und zirkulären Bildlauf Weitere Informationen finden Sie in der Android [wearablerecyclerview](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) -API-Dokumentation. 

#### <a name="standalone-apps"></a>Eigenständige apps 

Android Wear 2,0-Apps können unabhängig von Handheld-Apps verwendet werden. Dies bedeutet, dass beispielsweise eine Smartwatch weiterhin vollständige Funktionalität bietet, auch wenn das begleitende Gerät ausgeschaltet ist, das sich vom tragbaren-Gerät entfernt befindet. Weitere Informationen zu diesem Feature finden Sie im Thema " [eigenständige](https://developer.android.com/wear/preview/features/standalone-apps.html) Android-Apps".

#### <a name="wrist-gestures"></a>Handgelenk Gesten 

Handgesten ermöglichen es Benutzern, mit Ihrer APP zu interagieren, ohne den Touchscreen zu verwenden, &ndash; Benutzer auf die APP mit einer einzigen Hand reagieren können. Zwei Handgesten werden unterstützt: 

- Handgelenk ausgeflicke
- Handgelenk im Handumdrehen

Weitere Informationen finden Sie im Thema Android- [Hand Handgesten](https://developer.android.com/wear/preview/features/gestures.html) . 

Es gibt noch viele weitere Funktionen der Features 2,0, wie z. b. Inline Aktionen, intelligente Antworten, Remote Eingaben, erweiterte Benachrichtigungen und einen neuen Überbrückungs Modus für Benachrichtigungen. Weitere Informationen zu den neuen Funktionen von Wear 2,0 finden Sie in der [Übersicht](https://developer.android.com/wear/preview/api-overview.html)über die Android-API. 

## <a name="devices"></a>Geräte

Hier sind einige Beispiele für Geräte, auf denen Android Wear ausgeführt werden kann:

- [Motorola 360](https://moto360.motorola.com/)
- [LG G ansehen](https://www.lg.com/us/smart-watches/lg-W100-g-watch)
- [LG G ansehen R](https://www.lg.com/us/smartwatch/g-watch-r)
- [Samsung Zahnrad Live](https://www.samsung.com/global/microsite/gear/gearlive_design.html)
- [Sony Smartwatch 3](https://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
- [ASUS-Überwachung](https://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)

## <a name="further-reading"></a>Weiterführende Themen

Sehen Sie sich die Google-Dokumentation zu Android Wear an:

- [Informationen zu Android Wear](https://www.android.com/wear/)
- [Android Wear-App-Entwurf](https://developer.android.com/design/wear/index.html)
- [Android. Support. Wearable-Bibliothek](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [Android Wear 2,0](https://developer.android.com/wear/preview/index.html)

## <a name="summary"></a>Zusammenfassung

Diese Einführung bietet einen Überblick über Android Wear. Es wurden die grundlegenden Features von Android Wear beschrieben und eine Übersicht über die in Android Wear 2,0 eingeführten Features enthalten. Es wurden Links zu wichtigen Lese Vorlagen bereitgestellt, die Entwicklern den Einstieg in die Entwicklung von xamarin. Android Wear erleichtern, und es wurden Beispiele für einige Android Wear-Geräte aufgelistet, die sich derzeit auf dem Markt befinden.

## <a name="related-links"></a>Verwandte Links

- [Installation und Setup](~/android/wear/get-started/installation.md)
- [Erste Schritte](~/android/wear/get-started/index.md)
