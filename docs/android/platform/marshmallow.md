---
title: Marshmallow-Features
description: Dieser Artikel hilft Ihnen beim Einstieg in die Verwendung von xamarin. Android zum Entwickeln von Apps für Android 6,0 Marshmallow.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 7d5f8d2e80a6583cf49af883db8f2f33e6496e09
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757474"
---
# <a name="marshmallow-features"></a>Marshmallow-Features

_Dieser Artikel hilft Ihnen beim Einstieg in die Verwendung von xamarin. Android zum Entwickeln von Apps für Android 6,0 Marshmallow._

Dieser Artikel bietet einen Überblick über die neuen Features in Android 6,0 Marshmallow, erläutert die Vorbereitung von xamarin. Android für die Entwicklung von Android Marshmallow und bietet Links zu Beispielanwendungen, die veranschaulichen, wie neue Android Marshmallow verwendet werden kann. Funktionen in xamarin. Android-Apps. 

## <a name="overview"></a>Übersicht

[Android 6,0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)ist die nächste wichtige Android-Version nach dem Android Lollipop.
Xamarin. Android unterstützt Android Marshmallow und umfasst Folgendes:

- **API 23/Android 6,0-Bindungen** &ndash; Android 6,0 fügt viele neue APIs für die unten beschriebenen neuen Features hinzu. diese APIs sind für xamarin. Android-Apps verfügbar, wenn Sie auf API-Ebene 23 abzielen. Weitere Informationen zu Android 6,0-APIs finden Sie unter [Android 6,0-APIs](https://developer.android.com/preview/api-overview.html). 

[![Hero-Images von Tablets und Smartphones mit Marshmallow](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Obwohl sich die Marshmallow-Version hauptsächlich auf "Polnisch und Qualität" konzentriert, bietet Sie auch viele neue Features, die für xamarin. Android-Entwickler von Interesse sind. Zu diesen Features zählen: 

- **Lauf Zeit Berechtigungen** &ndash; Diese Erweiterung ermöglicht es Benutzern, Sicherheits Berechtigungen von Fall zu Fall zur Laufzeit zu genehmigen. 

- **Authentifizierungs Verbesserungen** Ab Android Marshmallow können apps nun Fingerabdrucksensoren verwenden, um Benutzer zu authentifizieren, und eine neue Funktion zum Bestätigen von Anmelde Informationen minimiert die Eingabe von Kenn Wörtern. &ndash; 

- **App-Verknüpfung** Mit dieser Funktion entfällt die Notwendigkeit, dass die APP beim Auswählen von Apps automatisch mit Webdomänen verknüpft wird. &ndash; 

- **Direkte Freigabe** Sie können *direkte Freigabe Ziele* definieren, die die Freigabe für Benutzer schnell und intuitiv machen. diese Funktion ermöglicht es utoren, Inhalte für andere apps freizugeben. &ndash; 

- **Sprach Interaktionen** &ndash; Mit dieser neuen API können Sie Funktionen für die Sprachfunktion in Ihrer APP erstellen. 

- **Anzeigemodus 4K** &ndash; In Android Marshmallow kann Ihre APP eine 4K-Anzeige Auflösung auf Hardware anfordern, die Sie unterstützt. 

- **Neue Audiofeatures** &ndash; Ab Marshmallow unterstützt Android nun das Protokoll "MIDI". Außerdem bietet es neue Klassen zum Erstellen digitaler audioerfassungs-und Wiedergabe Objekte und bietet neue API-Hooks zum Zuordnen von Audiodaten und Eingabegeräten. 

- **Neue Video Features** &ndash; Marshmallow bietet eine neue Klasse, die apps hilft, Audiodaten und Videostreams synchron zu Rendering. diese Klasse bietet auch Unterstützung für die dynamische Wiedergabe Rate. 

- **Android for Work** &ndash; Marshmallow umfasst erweiterte Steuerelemente für firmeneigene Geräte mit nur einem Benutzer. Sie unterstützt die automatische Installation und Deinstallation von apps durch den Gerätebesitzer, die automatische Akzeptanz von Systemupdates, die verbesserte Zertifikat Verwaltung, die Nachverfolgung von Daten, die Berechtigungs Verwaltung und Arbeitsstatus Benachrichtigungen. 

- **Unterstützungs Bibliothek für Material Design** Die neue *Entwurfs Unterstützungs Bibliothek* enthält Entwurfs Komponenten und Muster, die es Ihnen erleichtern, das Aussehen und Verhalten von Material Entwürfen in Ihrer APP zu gestalten. &ndash; 

Außerdem wurden viele wichtige Updates der Android-Bibliothek mit Android m veröffentlicht, und diese Updates bieten neue Features für Android m und frühere Versionen von Android.

Außerdem wurden viele wichtige Updates der Android-Bibliothek mit Android Marshmallow veröffentlicht, und diese Updates bieten neue Features für Android Marshmallow und frühere Versionen von Android. Dieser Artikel erläutert die ersten Schritte beim Entwickeln von apps mit Android Marshmallow und bietet einen Überblick über die neuen Funktions Highlights in Android 6,0. 

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Android Marshmallow-Features in xamarin-basierten apps zu verwenden: 

- **Xamarin. Android** &ndash; xamarin. Android 5.1.7.12 oder höher muss entweder mit Visual Studio oder mit Xamarin Studio installiert und konfiguriert werden.

- **Visual Studio für Mac** oder **Visual Studio** &ndash; , wenn Sie Visual Studio für Mac verwenden, ist Version 5.9.7.22 oder höher erforderlich. Wenn Sie Visual Studio verwenden, ist Version 3.11.1537 oder höher der xamarin-Tools für Visual Studio erforderlich. 

- **Android SDK** &ndash; Android SDK 6,0 (API 23) oder höher muss über den Android SDK-Manager installiert werden.

- **Java Developer Kit** Xamarin. Android erfordert [JDK 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 oder höher entwickeln (JDK 1,8 unterstützt auch API-Ebenen vor 24, einschließlich Marshmallow). &ndash; Die 64-Bit-Version von JDK 1,8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder den Formular-Previewer verwenden.

Sie können weiterhin [JDK 1,7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie speziell für API-Ebene 23 oder früher entwickeln. 

## <a name="getting-started"></a>Erste Schritte

Für den Einstieg in die Verwendung von Android Marshmallow mit xamarin. Android müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android Marshmallow-Projekt erstellen können: 

1. Installieren Sie die neuesten xamarin-Updates aus dem **stabilen** Kanal. 

2. Installieren Sie die Android 6,0 Marshmallow-SDK-Pakete und-Tools.

3. Erstellen Sie ein neues xamarin. Android-Projekt, das Android 6,0 Marshmallow (API-Ebene 23) als Ziel hat. 

4. Konfigurieren Sie einen Emulator oder ein Gerät für Android Marshmallow.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="install-xamarin-updates"></a>Installieren von xamarin-Updates

Um xamarin so zu aktualisieren, dass es Unterstützung für Android 6,0 Marshmallow enthält, ändern Sie den Aktualisierungs Kanal in **stabil** , und installieren Sie alle Updates. Weitere Informationen zum Installieren von Updates über den Update Kanal finden Sie unter [Ändern des Update Kanals](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 

### <a name="install-the-android-60-sdk"></a>Android 6,0 SDK installieren

Um ein xamarin. Android-Projekt für Android Marshmallow zu erstellen, müssen Sie zunächst den Android SDK-Manager verwenden, um das Android 6,0 SDK zu installieren:

- Starten Sie den Android SDK-Manager (verwenden Sie in Visual Studio für Mac Extras **> SDK-Manager**; verwenden Sie in Visual Studio Extras **> Android > Android SDK Manager**), und installieren Sie die neueste Android SDK Tools:

    [![Auswählen von Android SDK Tools im Android SDK Manager](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

- Installieren Sie außerdem die neuesten **Android 6,0** SDK-Pakete:

    [![Auswählen von Android 6,0 SDK-Paketen im Android SDK-Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Sie müssen Android SDK Tools Revision 24.3.4 oder höher installieren.
Weitere Informationen zum Verwenden des Android SDK-Managers für die Installation des Android 6,0 SDK finden Sie unter [SDK-Manager](https://developer.android.com/tools/help/sdk-manager.html).

### <a name="start-a-xamarinandroid-project"></a>Starten eines xamarin. Android-Projekts

Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Weitere Informationen zum Erstellen von Android-Projekten. 

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versions Einstellungen für Android 6,0 Marshmallow konfigurieren. Um Ihr Projekt für Marshmallow als Ziel festzustellen, müssen Sie Ihr Projekt für die **API-Ebene 23 (xamarin. Android v 6.0-Unterstützung)** konfigurieren. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

- Gerät: Nexus 5, 6 oder 9.
- Ziel: Android 6,0-API-Ebene 23
- ABI: x86

Beispielsweise ist dieses virtuelle Gerät so konfiguriert, dass ein Nexus 5 emuliert wird:

[![Konfigurieren von AVD mit Nexus 5-Gerät, Android 6,0-Ziel und Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Wenn Sie ein physisches Gerät (z. b. ein Nexus 5, 6 oder 9) verwenden, können Sie ein Vorschaubild von Android Marshmallow installieren. Weitere Informationen zum Aktualisieren Ihres Geräts auf Android Marshmallow finden Sie unter [Hardware System Images](https://developer.android.com/preview/download.html#images).

## <a name="new-features"></a>Neue Funktionen

Viele der in Android Marshmallow eingeführten Änderungen sind darauf ausgerichtet, die Android-Benutzer Funktionen zu verbessern, die Leistung zu erhöhen und Fehler zu beheben. Marshmallow hat jedoch auch einige umfassende Änderungen an den Grundlagen der Android-Plattform eingeführt. In den folgenden Abschnitten werden diese Verbesserungen vorgestellt und Links bereitgestellt, mit denen Sie die neuen Android Marshmallow-Features in Ihrer APP verwenden können. 

### <a name="runtime-permissions"></a>Lauf Zeit Berechtigungen

Das Android-Berechtigungssystem wurde seit Android Lollipop erheblich optimiert und vereinfacht. In Android Marshmallow erteilen Benutzerberechtigungen für eine Groß-/Kleinschreibung zur Laufzeit anstatt zur Installationszeit. Um dieses Feature unter Android Marshmallow und höher zu unterstützen, entwerfen Sie Ihre APP so, dass der Benutzer zur Laufzeit zur Eingabe von Berechtigungen aufgefordert wird (in dem Kontext, in dem die Berechtigungen benötigt werden). Diese Änderung erleichtert es Benutzern, sofort mit der Verwendung Ihrer APP zu beginnen, da Sie den Prozess der Installation und Aktualisierung Ihrer APP optimiert. 

Weitere Informationen zum Implementieren von Lauf Zeit Berechtigungen in xamarin. Android-Apps finden Sie [unter Anfordern von Lauf Zeit Berechtigungen in Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) .
Xamarin bietet auch eine Beispiel-APP, die veranschaulicht, wie Lauf Zeit Berechtigungen in Android Marshmallow (und höher) funktionieren: [Runtimeberechtigungen](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions).

Diese Beispiel-App veranschaulicht Folgendes:

- Überprüfen und Anfordern von Berechtigungen zur Laufzeit.
- Deklarieren von Berechtigungen für Android M-Geräte.

So verwenden Sie diese Beispiel-App:

1. Tippen Sie auf die Schaltflächen **Kamera** oder **Kontakte** , um das Dialogfeld Berechtigungs Anforderung anzuzeigen.
2. Erteilen Sie die Berechtigung zum Anzeigen von Kamera-oder Kontakt Fragmenten.

Weitere Informationen zu den neuen Funktionen für Lauf Zeit Berechtigungen in Android Marshmallow finden Sie unter [Arbeiten mit System Berechtigungen](https://developer.android.com/preview/features/runtime-permissions.html).

### <a name="authentication-enhancements"></a>Authentifizierungs Erweiterungen

Android Marshmallow umfasst zwei Authentifizierungs Erweiterungen, die dazu beitragen, die Notwendigkeit von Kenn Wörtern zu vermeiden:

- **Fingerabdruckauthentifizierung** &ndash; Verwendet einen Fingerabdruck Scan, um Benutzer zu authentifizieren.

- Anmelde Informationen **bestätigen** &ndash; Authentifiziert Benutzer, je nachdem, wie lange das Gerät entsperrt wurde.

Die im folgenden beschriebenen Links und Beispiel-Apps können Ihnen helfen, sich mit diesen neuen Features vertraut zu machen.

#### <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

Auf Geräten, die die Fingerabdruck Überprüfung unterstützen, können Sie `FingerPrintManager` die neue Klasse verwenden, um einen Benutzer zu authentifizieren.
Weitere Informationen zur Funktion Fingerabdruckauthentifizierung in Android Marshmallow finden Sie unter [Fingerabdruckauthentifizierung](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin bietet eine Beispiel-APP, die die Verwendung registrierter Fingerabdrücke zum Authentifizieren eines Benutzers in Ihrer APP veranschaulicht: [Fingerprintdialog](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog).

So verwenden Sie diese Beispiel-App:

1. Tippen Sie auf die Schaltfläche " **kaufen** ", um ein Dialogfeld zur Authentifizierung
2. Überprüfen Sie den registrierten Fingerabdruck, um sich zu authentifizieren.

Beachten Sie, dass für diese Beispiel-App ein Gerät mit einem Fingerabdruckleser erforderlich ist.
Diese APP speichert Ihren Fingerabdruck (oder Ihr Kennwort) nicht.

#### <a name="voice-interactions"></a>Sprach Interaktionen

Das neue sprach Interaktionen-Feature, das in Android Marshmallow eingeführt wurde, ermöglicht Benutzern Ihrer APP die Verwendung Ihrer Stimme zum Bestätigen von Aktionen und zum auswählen aus einer Liste von Optionen. Weitere Informationen zu sprach Interaktionen finden Sie unter [Übersicht über die API für die Sprachinteraktion](https://developers.google.com/voice-actions/interaction/). 

Weitere Informationen (einschließlich Codebeispielen) zur Implementierung von sprach Interaktionen in xamarin. Android-Apps finden [Sie unter Hinzufügen einer Konversation zu Ihrer Android-App mit sprach Interaktionen](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) .
Eine Beispiel-APP ist verfügbar, die die Verwendung der sprachinteraktions-API in einer xamarin. Android-App veranschaulicht: [Sprach Interaktionen](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).

#### <a name="confirm-credential"></a>Anmelde Informationen bestätigen

Mithilfe des neuen Features zum *bestätigen* der Anmelde Informationen von Android Marshmallow können Sie Benutzern die Möglichkeit geben, sich an App-spezifische Kenn Wörter zu erinnern und Sie einzugeben, indem Sie Sie basierend auf der Sperre Ihres Geräts authentifizieren.
Zu diesem Zweck verwenden Sie die neue `SetUserAuthenticationValidityDurationSeconds` Methode `KeyGenerator`von. Verwenden Sie `KeyGuardManager`die `CreateConfirmDeviceCredentialIntent` -Methode, um den Benutzer in Ihrer APP erneut zu authentifizieren. Weitere Informationen zu diesem neuen Feature in Android Marshmallow finden Sie unter [Confirm Credential](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin bietet eine Beispiel-APP, die die Verwendung von Geräte Anmelde Informationen (z. b. PIN, Muster oder Kennwort) in Ihrer APP veranschaulicht: [Confirmcredential](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)

So verwenden Sie diese Beispiel-App:

1. Richten Sie einen sicheren Sperrbildschirm auf Ihrem Gerät ein (**Secure > Security > screenlock**).
2. Tippen Sie auf die Schaltfläche **kaufen** , und bestätigen Sie die Anmelde Informationen für den sicheren Sperr

### <a name="chrome-custom-tabs"></a>Angepasste Chrome-Registerkarten

App-Entwickler haben eine Auswahl, wenn ein Benutzer auf eine URL tippt: die APP kann entweder einen Browser starten oder einen in-App-Browser `WebView`auf der Grundlage eines verwenden. Beide Optionen stellen Probleme &ndash; beim Starten des Browsers ist ein starker Kontextwechsel, der nicht anpassbar ist, während `WebView`e den Zustand nicht mit dem Browser gemeinsam nutzen. Außerdem kann die Verwendung `WebView`von s zu einem zusätzlichen Wartungsaufwand führen. 

Mithilfe von *benutzerdefinierten Chrome-Registerkarten* können Sie Websites problemlos und elegant mit der Leistung von Chrome anzeigen, ohne dass die Benutzer ihre app verlassen müssen. Diese Funktion bietet Ihrer APP mehr Kontrolle über die Weboberfläche des Benutzers. die Umstellung zwischen nativem und Webinhalt verläuft nahtlos, ohne dass auf eine `WebView`zurückgegriffen werden muss. Ihre APP kann sich auch darauf auswirken, wie Chrome aussehen kann, indem Sie Folgendes anpassen: 

- Symbolleisten Farbe

- Animationen eingeben und beenden

- Benutzerdefinierte Aktionen in der Chrome-Symbolleiste und im Überlauf Menü

- Vor dem Start von Chrome und vorab Abruf von Inhalten (für schnelleres Laden)

Wenn Sie dieses Feature in ihrer xamarin. Android-App nutzen möchten, können Sie die [Bibliothek benutzerdefinierte Registerkarten für Android-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)herunterladen und installieren.
Weitere Informationen zu dieser Funktion finden Sie unter [benutzerdefinierte Chrome-Registerkarten](https://developer.chrome.com/multidevice/android/customtabs).

### <a name="material-design-support-library"></a>Unterstützungs Bibliothek für Material Design

Android Lollipop hat den [Material Entwurf](http://www.google.com/design/spec/material-design/introduction.html) als neue Entwurfs Sprache eingeführt, um die Android-Darstellung zu aktualisieren (Weitere Informationen zur Verwendung von Material Design in xamarin. Android-Apps finden Sie im [Material](~/android/user-interface/material-theme.md) Design). Mit Android Marshmallow hat Google die *Bibliothek für Android-Entwurfs Unterstützung* eingeführt, mit der App-Entwickler das Aussehen und Verhalten von Material Entwürfen leichter übernehmen können. Diese Bibliothek enthält die folgenden Komponenten:

- **Coordinatorlayout** Das neue `CoordinatorLayout` Widget ist vergleichbar mit, aber leistungsfähiger als `FrameLayout`ein. &ndash; Sie können als `CoordinatorLayout` Container für untergeordnete Sichten oder als Layout der obersten Ebene verwenden, und es wird ein `layout_anchor` -Attribut bereitgestellt, das zum Verankern von Sichten in Bezug auf andere Ansichten verwendet werden kann.

- Reduzieren von **Symbolleisten** Die neue `CollapsingToolbarLayout` ist eine reduzierende App-Leiste, die `Toolbar`ein Wrapper für ist. &ndash; (Beachten Sie, dass die *App-Leiste* zuvor als *Aktionsleiste*bezeichnet wurde.)

- **Schaltfläche** für Gleit Komma &ndash; Eine Runde Schaltfläche, die die primäre Aktion der-Schnittstelle Ihrer APP angibt.

- Gleit **Komma Bezeichnungen zum Bearbeiten von Text** Verwendet ein neues `TextInputLayout` Widget (das `EditText`umschließt), um eine Gleit Komma Bezeichnung anzuzeigen, wenn ein Benutzer Text eingibt. &ndash;

- **Navigationsansicht** &ndash; Das neue`NavigationView` Widget hilft Ihnen dabei, die Navigationsleiste auf eine Weise zu verwenden, die für Benutzer einfacher zu navigieren ist.

- **Snackbar** &ndash; Das neue`SnackBar` Widget ist ein schlanker Feedback Mechanismus (ähnlich wie ein Toast), der am unteren Bildschirmrand eine kurze Meldung anzeigt, die über allen anderen Elementen auf dem Bildschirm angezeigt wird.

- **Material Registerkarten** &ndash; Das neue`TabLayout` Widget bietet ein horizontales Layout zum Anzeigen von Registerkarten als Methode zum Implementieren der Navigation auf oberster Ebene in Ihrer APP.

Wenn Sie die [Entwurfs Unterstützungs Bibliothek](https://developer.android.com/tools/support-library/features.html#design) in ihrer xamarin. Android-App nutzen möchten, laden Sie das xamarin [xamarin-Unterstützungs Bibliotheks Design](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) -nuget-Paket herunter, und installieren Sie es.

Weitere Details (einschließlich Codebeispiele) zur Verwendung der Material Design-Unterstützungs Bibliothek in xamarin. Android-Apps finden Sie unter Design [Bibliothek für die Android-Unterstützung](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) .
Xamarin bietet eine Beispiel-APP, die die neue Android-Entwurfs Bibliothek in xamarin &ndash; . Android [cheesesquare untersucht](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-cheesesquare).
In diesem Beispiel werden die folgenden Funktionen der Entwurfs Bibliothek veranschaulicht:

- Reduzieren der Symbolleiste
- Schaltfläche für Gleit Komma
- Verankern anzeigen
- NavigationView
- Snackbar

Weitere Informationen zur Entwurfs Bibliothek finden Sie [unter Android Design Support Library](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) im Blog des Android-Entwicklers.

### <a name="additional-library-updates"></a>Weitere Bibliotheks Updates

Zusätzlich zu Android Marshmallow hat Google Verwandte Updates für mehrere Android-Kernbibliotheken angekündigt. Xamarin bietet xamarin. Android-Unterstützung für diese Updates über mehrere Vorschauversion-nuget-Pakete: 

- [Google Play Services](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) Die neueste Version von Google Play Services enthält das neue *App-Einladungen* -Feature, das es Benutzern ermöglicht, Ihre APP für Freunde freizugeben. &ndash; Weitere Informationen zu diesem Feature finden Sie unter [Erweitern der APP-Reichweite mit den App-Einladungen von Google](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

- [Android-Unterstützungs Bibliotheken](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; Diese nugets bieten Features, die nur für Bibliotheks-APIs verfügbar sind, während Sie abwärts kompatible Versionen der Android Framework-APIs bereitstellen. 

- [Android-Wearable-Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; diese nuget umfasst Google Play Services Bindungen. Die neueste Version der tragbaren Library bietet neue Features (einschließlich einfacherer Navigation für benutzerdefinierte Apps) für die Android Wear-Plattform. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Marshmallow vorgestellt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung auf Marshmallow installiert und konfiguriert werden. Außerdem wurde eine Übersicht über die spannendsten neuen Android Marshmallow-Features für die xamarin. Android-Entwicklung bereitgestellt.

## <a name="related-links"></a>Verwandte Links

- [Android 6,0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Übersicht über Features](https://developer.android.com/preview/api-overview.html)
- [Anmerkungen zu dieser Version](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [Runtimeberechtigungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions)
- [Confirmcredential (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)
- [Fingerprintdialog (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
