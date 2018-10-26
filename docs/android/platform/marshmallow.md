---
title: Marshmallow-Funktionen
description: Dieser Artikel unterstützt Sie erste Schritte mit in mithilfe von Xamarin.Android zum Entwickeln von apps für Android 6.0 Marshmallow.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 0393b9a994c1fd62f51cff01a88aa73f71019d53
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113455"
---
# <a name="marshmallow-features"></a>Marshmallow-Funktionen

_Dieser Artikel unterstützt Sie erste Schritte mit in mithilfe von Xamarin.Android zum Entwickeln von apps für Android 6.0 Marshmallow._

In diesem Artikel bietet einen Überblick über die neuen Features in Android 6.0 Marshmallow wird erläutert, wie Xamarin.Android für die Entwicklung von Android Marshmallow vorbereiten und enthält Links zu Beispielanwendungen, die veranschaulichen, wie Sie der neuen Android Marshmallow zu verwenden. Features in Xamarin.Android-apps. 


## <a name="overview"></a>Übersicht

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), ist die nächste wichtige Android Version nach Android Lollipop.
Xamarin.Android unterstützt Android Marshmallow und umfasst:

-   **API-23/Android 6.0 Bindungen** &ndash; Android 6.0 fügt viele neue APIs für die neuen Features, die unten beschriebenen; diese APIs sind für Xamarin.Android-apps verfügbar, wenn Sie die API-Ebene 23 abzielen. Weitere Informationen zu Android 6.0-APIs, finden Sie unter [Android 6.0-APIs](http://developer.android.com/preview/api-overview.html). 

[![Hero-Abbilder von Tablets und Smartphones Marshmallow ausführen](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Obwohl die Marshmallow Version hauptsächlich auf "polnischer und Qualität" gerichtet ist, bietet sie auch viele neue Funktionen für Xamarin.Android-Entwickler an. Zu diesen Features zählen: 

-   **Laufzeitberechtigungen** &ndash; diese Verbesserung ermöglicht Benutzern das Genehmigen von Sicherheitsberechtigungen für von Fall zu Fall zur Laufzeit. 

-   **Verbesserungen der Authentifizierung** &ndash; seit Android Marshmallow, apps können nun Fingerabdruck Sensoren Authentifizierung von Benutzern und ein neues *Anmeldeinformationen bestätigen* Funktion verringert die Notwendigkeit für die Eingabe Kennwörter. 

-   **App verknüpfen** &ndash; dieses Feature unterstützt wird, um die Notwendigkeit beseitigen der **App-Auswahl** angezeigt werden, indem apps Webdomänen automatisch zugeordnet. 

-   **Leiten Sie die Freigabe** &ndash; können Sie definieren *direkten Freigabe Ziele* , die Stellen Freigabe schnell und intuitiv für Benutzer; diese Funktion ermöglicht VMs zur Freigabe von Inhalten für andere apps. 

-   **Voice-Interaktionen** &ndash; dieser neuen API können Sie conversational Voice-Funktionen in Ihrer app zu erstellen. 

-   **4 K-Anzeigemodus** &ndash; In Android Marshmallow Ihrer app kann anfordern, eine 4-KB-Bildschirmauflösung auf Hardware, die es unterstützt. 

-   **Neue Features für Audio** &ndash; ab Marshmallow unterstützt Android jetzt das MIDI-Protokoll. Es bietet außerdem neue Klassen zur Erstellung von digital-audio-Erfassung und Wiedergabe-Objekte, und bietet neue API-Hooks für das Zuordnen von Audio- und Eingabe-Geräte. 

-   **Neue Videofunktionen** &ndash; Marshmallow, bietet eine neue Klasse, die apps unterstützt Rendern Audio- und Videodatenströme synchron, wie diese Klasse bietet auch Unterstützung für dynamische Playback-Geschwindigkeit. 

-   **Android for Work** &ndash; Marshmallow enthält erweiterte Steuerelemente für unternehmenseigene Einzelbenutzer-Geräte. Es unterstützt die automatische Installation und Deinstallation von apps von der Eigentümer des Geräts, automatisches akzeptieren der Systemupdates, verbesserte zertifikatsverwaltung, Daten Nutzung nachverfolgen, Verwaltung von Berechtigungen und Benachrichtigungen für die Arbeit. 

-   **Material Design-Unterstützungsbibliothek** &ndash; neuen *Design-Unterstützungsbibliothek* enthält, Designkomponenten und Muster, die Ihnen die Erstellung von Material Design aussehen und Verhalten in Ihrer app erleichtert. 

Darüber hinaus wurden viele Core-Bibliothek für Android-Updates mit einem Android M veröffentlicht, und diese Updates stellt neue Features für Android M und früheren Versionen von Android bereit.

Darüber hinaus wurden viele Core-Bibliothek für Android-Updates mit Android Marshmallow veröffentlicht, und diese Updates stellt neue Features für Android Marshmallow und früheren Versionen von Android bereit. In diesem Artikel wird erläutert, wie für den Einstieg in die Erstellung von apps mit Android Marshmallow, und es bietet eine Übersicht über das neue Feature in Android 6.0 werden hervorgehoben. 

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Features für Android Marshmallow in Xamarin-basierte apps verwenden: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 or later must be installed and configured with either Visual Studio or Xamarin Studio.

-   **Visual Studio für Mac** oder **Visual Studio** &ndash; , wenn Sie Visual Studio für Mac, Version 5.9.7.22 oder höher erforderlich ist. Wenn Sie Visual Studio, Version 3.11.1537 verwenden oder höher des ist die Xamarin-Tools für Visual Studio erforderlich. 

-   **Android SDK** &ndash; Android SDK-6.0 (API 23) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin.Android erfordert [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für API-Ebene 24 entwickeln oder größer (JDK 1.8 unterstützt auch API-Ebenen älter als 24, einschließlich Marshmallow). Die 64-Bit-Version des JDK 1.8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

Sie können weiterhin verwenden [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Wenn Sie die Entwicklung von speziell für die API-Ebene 23 oder früher sind. 


## <a name="getting-started"></a>Erste Schritte

Zum Einstieg anhand von Android Marshmallow mit Xamarin.Android, müssen Sie herunterladen und die neuesten Tools und SDK-Pakete installieren, bevor Sie ein Android Marshmallow-Projekt erstellen können: 

1.  Installieren Sie die neuesten Updates von Xamarin von der **stabile** Kanal. 

2.  Android 6.0 Marshmallow SDK-Pakete und Tools zu installieren.

3.  Erstellen eines neuen Xamarin.Android-Projekts, das als Ziel Android 6.0 Marshmallow (API-Ebene 23). 

4.  Konfigurieren Sie einen Emulator oder Gerät für Android Marshmallow an.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:


### <a name="install-xamarin-updates"></a>Installieren von Xamarin-Updates

Um Xamarin zu aktualisieren, Unterstützung für Android 6.0 Marshmallow enthalten, Ändern des updatekanals zu **stabile** und installieren Sie alle Updates. Weitere Informationen zum Installieren von Updates aus dem updatekanal finden Sie unter [ändern Sie den Kanal Updates](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 


### <a name="install-the-android-60-sdk"></a>Installieren Sie das SDK für Android 6.0

Um ein Xamarin.Android-Projekt für Android Marshmallow zu erstellen, müssen Sie zuerst den Android SDK Manager verwenden, zum Installieren des SDK für Android 6.0:

-   Starten Sie den Android SDK Manager (in Visual Studio für Mac verwenden **Extras > SDK-Manager**; in Visual Studio verwenden **Tools > Android > Android SDK-Manager**) herunter, und Installieren der neuesten Android SDK Tools:

    [![Android SDK-Tools im Android SDK-Manager auswählen.](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Außerdem installieren Sie das neueste **Android 6.0** SDK-Pakete:

    [![Auswählen von Android 6.0-SDK-Pakete im Android SDK-Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Müssen Sie den Android SDK Tools-Revision 24.3.4 installieren oder höher.
Weitere Informationen zur Verwendung von Android SDK-Manager zum Installieren von Android 6.0 SDK finden Sie unter [SDK-Manager](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Ein Xamarin.Android-Projekt starten

Erstellen eines neuen Xamarin.Android-Projekts an. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) um weitere Informationen zum Erstellen von Android-Projekte. 

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 6.0 MarshMallow konfigurieren. Um Ihr Projekt für Marshmallow Ziel festzulegen, müssen Sie Ihr Projekt für das Konfigurieren **API-Ebene 23 (Xamarin.Android 6.0 Unterstützung)**. Weitere Informationen zum Konfigurieren von Android-API-Ebene Level, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager aus, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

-   Gerät: Nexus 5, 6 oder 9.
-   Ziel: Android 6.0 - API-Ebene 23
-   ABI: x86

Beispielsweise wird der dieses virtuelle Gerät zum Emulieren von Nexus 5 konfiguriert:

[![Konfigurieren ein AVD mit Nexus 5-Geräte, Android 6.0-Ziel und Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Wenn Sie ein physisches Gerät, z. B. einem Nexus 5 verwenden, können Sie ein Vorschaubild der Android Marshmallow bei 6 oder 9, installieren. Weitere Informationen zum Aktualisieren von Ihrem Geräts auf Android Marshmallow finden Sie unter [Hardware-Systemimages](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Neue Funktionen

Viele der Änderungen in Android Marshmallow konzentrieren uns auf die Verbesserung der benutzerfreundlichkeit für Android, Leistung zu erhöhen, und Beheben von Fehlern. Marshmallow führte jedoch auch einige umfassende Änderungen in die Grundlagen der Android-Plattform. In den folgenden Abschnitten markieren diese Verbesserungen und enthalten Links, um Ihnen den Einstieg in die neuen Features für Android Marshmallow in Ihrer app verwenden können. 



### <a name="runtime-permissions"></a>Laufzeitberechtigungen

Das Android-Berechtigungen-System wurde wesentlich optimiert und vereinfacht, da Android Lollipop. In Android Marshmallow erteilen Sie Benutzern, dass die Berechtigungen für von Fall zu Fall zur Laufzeit statt zur installieren. Um dieses Feature für Android Marshmallow oder höher unterstützen, Entwerfen Sie Ihre app vom Benutzer Berechtigungen zur Laufzeit (in Kontext, in dem die Berechtigungen erforderlich sind). Diese Änderung erleichtert es für Benutzer beim Einstieg in Ihre app unmittelbar, da es das Installieren und aktualisieren Ihre app vereinfacht. 

Finden Sie unter [Anfordern von Laufzeitberechtigungen in Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) für weitere Details (einschließlich Codebeispiele) zum Implementieren von Laufzeitberechtigungen in Xamarin.Android-apps.
Xamarin bietet auch eine Beispiel-app, die zeigt, wie die Common Language Runtime-Berechtigungen auf Android Marshmallow (und höher) funktionieren: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Diese Beispiel-app veranschaulicht Folgendes:

-   Informationen zum Überprüfen und Anfordern von Berechtigungen zur Laufzeit.
-   So deklarieren Sie die Berechtigungen für die Geräte mit Android M.

So verwenden Sie diese Beispiel-app

1.  Tippen Sie auf die **Kamera** oder **Kontakte** Schaltflächen für die Anzeige einer Berechtigungen anfordern, Dialogfeld.
2.  Erteilen Sie die Berechtigung zum Anzeigen der Kamera oder Kontakte Fragmente.

Weitere Informationen zu den neuen Features der Common Language Runtime-Berechtigungen in Android Marshmallow finden Sie unter [arbeiten mit einer Systemberechtigungen](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Verbesserungen der Authentifizierung

Android Marshmallow enthält zwei Verbesserungen der Authentifizierung, die dabei helfen, die keine Kennwörter mehr erforderlich:

-   **Authentifizierung-Fingerabdruck** &ndash; eine Überprüfung per Fingerabdruck zum Authentifizieren von Benutzern verwendet.

-   **Bestätigen Sie die Anmeldeinformationen** &ndash; authentifiziert Benutzer anhand, wie lange das Gerät entsperrt wurde.

Die Links und als Nächstes beschriebenen Beispiel-apps können Sie vertraute mit diesen neuen Features.


#### <a name="fingerprint-authentication"></a>Authentifizierung per Fingerabdruck

Auf Geräten, die mittels fingerabdruckscan Hardware unterstützen, können Sie die neue `FingerPrintManager` Klasse, um einen Benutzer zu authentifizieren.
Weitere Informationen zu dem fingerabdruckfeature für die Authentifizierung in Android Marshmallow, finden Sie unter [Authentifizierung per Fingerabdruck](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin bietet eine Beispiel-app, das veranschaulicht, wie Sie registrierte Fingerabdrücke verwenden, um einen Benutzer in Ihrer app authentifizieren: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

So verwenden Sie diese Beispiel-app

1.  Tippen Sie auf die **Kauf** , um einen Fingerabdruck-Dialogfeld "Authentifizierung" zu öffnen.
2.  Scannen Sie Ihre registrierten Fingerabdruck zu authentifizieren.

Beachten Sie, dass diese Beispiel-app auf einem Gerät mit einem Fingerabdruckleser erforderlich ist.
Diese app speichert Ihr Fingerabdruck (oder Ihr Kennwort) nicht.



#### <a name="voice-interactions"></a>Interaktionen per Sprache

Die neue Interaktionen per Sprache-Funktion, die in Android Marshmallow eingeführte ermöglicht Ihrer App verwenden Sie ihre Stimme, um Aktionen zu bestätigen, und wählen Sie aus einer Liste von Optionen. Weitere Informationen zu Interaktionen per Sprache, finden Sie unter [Überblick über die Sprach-API Interaktion](https://developers.google.com/voice-actions/interaction/). 

Finden Sie unter [fügen Sie eine Konversation zu Ihrer Android-App mit Interaktionen per Sprache](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) für weitere Details (einschließlich Codebeispiele) zum Implementieren von Interaktionen per Sprache in Xamarin.Android-apps.
Eine Beispiel-app ist verfügbar, die veranschaulicht, wie der Voice-Interaktion-API in einer Xamarin.Android-app: [Sprachinteraktionen](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Überprüfen der Anmeldeinformationen

Mit dem neuen *Anmeldeinformationen bestätigen* Funktion von Android Marshmallow können Benutzer zu merken, und geben app-spezifische Kennwörter durch authentifizieren sie nach, wie lange das Gerät entsperrt wurde freigegeben.
Zu diesem Zweck verwenden Sie die neue `SetUserAuthenticationValidityDurationSeconds` Methode der `KeyGenerator`. Verwenden der `KeyGuardManager`des `CreateConfirmDeviceCredentialIntent` Methode, um den Benutzer in Ihrer app erneut zu authentifizieren. Weitere Informationen zu diesem neuen Feature in Android Marshmallow, finden Sie unter [Anmeldeinformationen bestätigen](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin bietet eine Beispiel-app, das veranschaulicht, wie Sie die Anmeldeinformationen des Geräts (z. B. PIN, Muster und Kennwort) in Ihrer app zu verwenden: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

So verwenden Sie diese Beispiel-app

1.  Einrichten einer sicheren Sperrbildschirm auf Ihrem Gerät (**Secure > Sicherheit > Screenlock**).
2.  Tippen Sie auf die **Kauf** und bestätigen Sie die Anmeldeinformationen des Schloss Bildschirm.



### <a name="chrome-custom-tabs"></a>Benutzerdefinierte Registerkarten Chrome

App-Entwickler stehen vor eine Wahl, wenn ein Benutzer eine URL tippt: die app startet einen Browser kann oder verwenden Sie einen in-app-Browser basierend auf einer `WebView`. Beide Optionen eine besondere Herausforderung darstellen &ndash; starten den Browser ist ein hoher Kontextwechsel, die angepasst werden kann, während nicht `WebView`s Status nicht mit dem Browser freigeben. Darüber hinaus verwenden `WebView`s kann zusätzliche Wartungsaufwand hinzufügen. 

*Benutzerdefinierte Registerkarten in Chrome* ermöglicht es Ihnen, einfach und elegant Websites mit der Leistungsfähigkeit von Chrome anzuzeigen, ohne dass Ihre Benutzer Ihre app zu verlassen. Diese Funktion ermöglicht Ihrer app mehr Kontrolle über die Weboberfläche des Benutzers; Übergänge zwischen nativer zu vereinfachen und Webinhalte nahtloser ohne selbst eine `WebView`. Ihre app kann auch beeinflussen, wie Chrome- Erscheinungsbild durch Folgendes anpassen: 

-   Symbolleistenfarbe

-   Geben Sie ein, und Beenden von Animationen

-   Benutzerdefinierte Aktionen in der Chrome-Symbolleiste "und" Überlauf im

-   Chrome vor dem Start und Inhalte vorab abzurufen (zum schnelleren Laden)

Zur Nutzung dieses Features in Xamarin.Android-app herunterladen und Installieren der [Unterstützungsbibliothek "Android" (benutzerdefinierte Registerkarten](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Weitere Informationen zu diesem Feature finden Sie unter [Chrome benutzerdefinierten Registerkarten](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Material Design-Unterstützungsbibliothek

Android Lollipop eingeführt [Material Design](http://www.google.com/design/spec/material-design/introduction.html) als eine neue Design-Sprache, um die Android-Umgebung zu aktualisieren (finden Sie unter [Material Design](~/android/user-interface/material-theme.md) Informationen zur Verwendung von Material Design in Xamarin.Android-apps). Google Android Marshmallow eingeführt der *Android Design-Unterstützungsbibliothek* um zu erleichtern für app-Entwickler Material übernehmen entwerfen Aussehen und Verhalten. Diese Bibliothek enthält die folgenden Komponenten:

-   **CoordinatorLayout** &ndash; neuen `CoordinatorLayout` Widget entspricht, jedoch leistungsfähiger als ein `FrameLayout`. Können Sie `CoordinatorLayout` als Container für untergeordnete Ansichten oder als ein Layout der obersten Ebene aus, und es bietet eine `layout_anchor` -Attribut, das zum Anker Ansichten relativ zu anderen Ansichten verwendet werden kann.

-   **Reduzieren von Symbolleisten** &ndash; neuen `CollapsingToolbarLayout` ist eine Reduzierung appleiste, die einen Wrapper für `Toolbar`. (Beachten Sie, dass die *app-Leiste* ist, was früher als bezeichnet wurde die *Aktionsleiste*.)

-   **Unverankerte interaktive Schaltfläche** &ndash; einer runden Schaltfläche, die die primäre Aktion in Ihrer app-Schnittstelle kennzeichnet.

-   **Unverankerte Bezeichnungen für den Text bearbeiten** &ndash; verwendet ein neues `TextInputLayout` Widget (Wrapper einschließt `EditText`) eine unverankerte Bezeichnung angezeigt, wenn ein Hinweis ausgeblendet wird, wenn ein Benutzer Text eingibt.

-   **Navigationsansicht** &ndash; neuen `NavigationView` Widget unterstützt Sie der Navigations-Drawer auf eine Weise, die für Benutzer leichter, navigieren zu verwenden.

-   **Snackbar** &ndash; neuen `SnackBar` Widget ist eine einfache Feedbackmechanismus (ähnlich wie ein Popup), die eine kurze Fehlermeldung am unteren Rand des Bildschirms angezeigt werden über allen anderen Elementen auf dem Bildschirm anzeigt.

-   **Material Registerkarten** &ndash; neuen `TabLayout` Widget enthält ein horizontales Layout für das Anzeigen von Registerkarten als Möglichkeit zur Navigation auf oberster Ebene in Ihrer app zu implementieren.

Nutzen der [Design-Unterstützungsbibliothek](http://developer.android.com/tools/support-library/features.html#design) in Ihrer Xamarin.Android-app herunterladen und installieren Sie den Xamarin [Bibliotheksentwurf für Xamarin-Unterstützung](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet-Paket.

Finden Sie unter [ansprechende Material Design mit der Android-Unterstützung Entwurf Bibliothek](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) für weitere Details (einschließlich Codebeispiele) zur Verwendung der Material Design-Support-Bibliothek in Xamarin.Android-apps.
Xamarin bietet eine Beispiel-app, die die neue Android-Design-Bibliothek für Xamarin.Android demos &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Dieses Beispiel veranschaulicht die folgenden Funktionen von der Design-Bibliothek:


-   Reduzieren die Symbolleiste
-   Unverankerte interaktive Schaltfläche
-   Ansicht-Verankerung
-   NavigationView
-   Snackbar

Weitere Informationen über die Design-Bibliothek finden Sie unter [Android Design-Unterstützungsbibliothek](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) in der Android-Entwickler-Blog.


### <a name="additional-library-updates"></a>Weitere Library-Updates

Zusätzlich zum Android Marshmallow hat Google die entsprechenden Updates für verschiedene Android-Kernbibliotheken angekündigt. Xamarin bietet Xamarin.Android-Unterstützung für diese Updates über mehrere Preview-Release-NuGet-Pakete: 

-   [Google Play Services](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; die neueste Version von Google Play Services umfasst die neue *App Invites* -Funktion, die Benutzer ihre app für Freunde freigeben kann. Weitere Informationen zu diesem Feature finden Sie unter [Ihrer Anwendung erweitern die Reichweite mit Google App Invites](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Android-Unterstützungsbibliotheken](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; diese NuGet-Pakete bieten Funktionen, die nur für das Bibliotheks-APIs verfügbar und bietet gleichzeitig Abwärtskompatibilität-Versionen von Android-Framework-APIs sind. 

-   [Bibliothek für Android tragbare](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; dieses NuGet-Paket enthält den Google Play Services-Bindungen. Die neueste Version der tragbare Bibliothek führt neue Features, die (einschließlich einfachere Navigation für benutzerdefinierte apps) in der Android Wear-Plattform. 


## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Marshmallow eingeführt und wurde erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android auf Marshmallow. Außerdem wird eine Übersicht über die interessantesten neuen Features für Android Marshmallow bereitgestellt, für die Entwicklung von Xamarin.Android.


## <a name="related-links"></a>Verwandte Links

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Abrufen des Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Übersicht über Features](https://developer.android.com/preview/api-overview.html)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
