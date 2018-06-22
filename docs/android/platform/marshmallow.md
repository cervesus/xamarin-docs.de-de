---
title: Marshmallow-Funktionen
description: Dieser Artikel hilft Ihnen Einstieg in Xamarin.Android zum Entwickeln von apps für Android 6.0 Marshmallow verwenden.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d2150e18a377d61a2e79fabfc845f57cfab8a5c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774947"
---
# <a name="marshmallow-features"></a>Marshmallow-Funktionen

_Dieser Artikel hilft Ihnen Einstieg in Xamarin.Android zum Entwickeln von apps für Android 6.0 Marshmallow verwenden._

In diesem Artikel bietet einen Überblick über die neuen Funktionen in Android 6.0 Marshmallow wird erläutert, wie Android Marshmallow Entwicklung Xamarin.Android Vorbereitung und enthält Links zu Beispielanwendungen, die veranschaulichen, wie Nutzen des neuen Android Marshmallow Funktionen in Xamarin.Android apps. 


## <a name="overview"></a>Übersicht

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), wird die nächste größere Android Version nach Android Lollipop.
Xamarin.Android unterstützt Android Marshmallow und umfasst:

-   **API-23/Android 6.0 Bindungen** &ndash; Android 6.0 fügt viele neue APIs für die neuen Funktionen, die unten beschriebenen; diese APIs stehen Xamarin.Android apps beim Abzielen auf API-Ebene 23. Weitere Informationen zur Android 6.0-APIs finden Sie unter [Android 6.0-APIs](http://developer.android.com/preview/api-overview.html). 

[![Hero Images-Tablets und-Telefone Marshmallow ausführen](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Obwohl die Marshmallow-Version auf "Polnisch und Qualität" hauptsächlich ausgelegt ist, bietet es außerdem viele neue Funktionen von Interesse sind für Entwickler Xamarin.Android an. Zu diesen Funktionen gehören: 

-   **Common Language Runtime Berechtigungen** &ndash; diese Erweiterung ermöglicht es Benutzern, die zur Laufzeit Sicherheitsberechtigungen auf Basis von Fall zu genehmigen. 

-   **Verbesserungen der Authentifizierung** &ndash; ab Android Marshmallow, apps können jetzt verwenden Fingerabdruck Sensoren zum Authentifizieren von Benutzern und ein neues *bestätigen Anmeldeinformationen* Funktion verringert die Notwendigkeit zum eingeben Dazu zählen Kennwörter. 

-   **Verknüpfen der App** &ndash; dieses Feature erleichtert die Notwendigkeit der vermeiden der **App Auswahl** diagnosepopup durch die Zuordnung automatisch mit Webdomänen apps. 

-   **Direkte Freigabe** &ndash; können *direkte Freigabe Ziele* , die Stellen Freigabe-schnell und intuitiv für Benutzer; diese Funktion ermöglicht Uers Freigeben von Inhalten mit anderen apps. 

-   **Voice-Interaktionen** &ndash; mit dieser neuen API können Sie natürlichsprachliches Voice-Features in Ihrer app zu erstellen. 

-   **4 K Anzeigemodus** &ndash; In Android Marshmallow Ihrer app kann Anfordern einer 4-KB-Auflösung auf Hardware, die es unterstützt. 

-   **Neue Funktionen von Audio** &ndash; ab Marshmallow unterstützt Android jetzt das MIDI-Protokoll. Sie bietet außerdem neue Klassen zum Erstellen digital audio Erfassung und Wiedergabe Objekte und neue API-Hooks für das Zuordnen von Audio- und die Eingabespalten Geräte bietet. 

-   **Neue Videofunktionen** &ndash; Marshmallow bietet eine neue Klasse, die der Identifikation von apps Audio- und Videofunktionen Datenströme Rendern synchron, wie diese Klasse bietet auch Unterstützung für dynamische Wiedergaberate. 

-   **Android for Work** &ndash; Marshmallow umfasst erweiterte Steuerelemente für unternehmenseigene, Einzelbenutzer-Geräte. Es unterstützt die Installation und Deinstallation von apps, die vom Besitzer Geräts, automatisches akzeptieren der Systemupdates, verbesserte zertifikatverwaltung Daten Nutzung nachverfolgen, Verwaltung von Berechtigungen und Arbeit Status Benachrichtigungen. 

-   **Material Entwurf Unterstützungsbibliothek** &ndash; neuen *Design-Unterstützungsbibliothek* bietet Entwurfskomponenten und Muster, die leichter für Material Entwurf Aussehen und Verhalten in Ihrer app zu erstellen. 

Darüber hinaus wurden viele Core-Bibliothek für Android-Updates mit Android M veröffentlicht, und diese Updates bieten neue Funktionen für Android M und früheren Versionen von Android.

Darüber hinaus wurden viele Core-Bibliothek für Android-Updates mit Android Marshmallow veröffentlicht, und diese Updates bieten neue Funktionen für Android Marshmallow und früheren Versionen von Android. In diesem Artikel wird erläutert, wie für den Einstieg in das Erstellen von apps mit Android Marshmallow, und es bietet eine Übersicht über die neue Funktion in Android 6.0 hervorgehoben. 

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die neuen Funktionen für Android Marshmallow in Xamarin-basierten apps zu verwenden: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 or later must be installed and configured with either Visual Studio or Xamarin Studio.

-   **Visual Studio für Mac** oder **Visual Studio** &ndash; , wenn Sie mit Visual Studio für Mac, Version 5.9.7.22 oder höher erforderlich ist. Bei Verwendung von Visual Studio, Version 3.11.1537 oder später von der Xamarin-Tools für Visual Studio erforderlich ist. 

-   **Android SDK** &ndash; Android SDK 6.0 (API 23) or later must be installed via the Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin.Android requires [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) or later if you are developing for API level 24 or greater (JDK 1.8 also supports API levels earlier than 24, including Marshmallow). Die 64-Bit-Version des JDK 1.8 ist erforderlich, wenn Sie benutzerdefinierte Steuerelemente oder das Vorschauprogramm Forms verwenden.

Sie können weiterhin [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Domänenmodus speziell für API-Ebene 23 entwickeln oder eine frühere Version. 


## <a name="getting-started"></a>Erste Schritte

Zum Einstieg Android Marshmallow mit Xamarin.Android verwenden, müssen Sie herunterladen und installieren die neuesten Tools und SDK-Paketen, vor dem Erstellen eines Projekts für Android Marshmallow: 

1.  Installieren Sie die neuesten Xamarin-Updates von der **stabile** Kanal. 

2.  Installieren Sie die Pakete für Android 6.0 Marshmallow SDK und Tools.

3.  Erstellen Sie ein neues Xamarin.Android-Projekt, dessen Ziel Android 6.0 Marshmallow (API-Level-23). 

4.  Konfigurieren Sie einen Emulator oder Gerät für Android Marshmallow an.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:


### <a name="install-xamarin-updates"></a>Installieren Sie Xamarin-Updates

Um Xamarin aktualisieren, damit sie die Unterstützung für Android 6.0 Marshmallow enthält, ändern Sie den Update-Kanal an **stabile** und installieren Sie alle Updates. Weitere Informationen zum Installieren von Updates aus dem Kanal Updates finden Sie unter [ändern Sie den Kanal Updates](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/). 


### <a name="install-the-android-60-sdk"></a>Installieren Sie das SDK für Android 6.0

Zum Erstellen eines Projekts Xamarin.Android für Android Marshmallow müssen Sie zuerst den Android SDK-Manager verwenden, das Android 6.0 SDK installieren:

-   Starten Sie den Android SDK-Manager (in Visual Studio für Mac verwenden **Tools > SDK-Manager**; in Visual Studio verwenden **Tools > Android > Android SDK Manager**), und installieren Sie die neueste Android SDK-Tools:

    [![Android SDK-Tools in den Android SDK-Manager auswählen.](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Zudem installieren Sie das neueste **Android 6.0** SDK-Paketen:

    [![Auswählen von Android 6.0 SDK-Paketen im Android SDK Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Müssen Sie den Android SDK-Tools Revision 24.3.4 installieren oder höher.
Weitere Informationen zum Verwenden von Android SDK Manager Android 6.0 SDK installieren, finden Sie unter [SDK-Manager](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Starten eines Projekts Xamarin.Android

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Android-Projekte. 

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 6.0 MarshMallow konfigurieren. Um das Projekt für Marshmallow abzielen möchten, müssen Sie das Projekt für die konfigurieren **API-Ebene 23 (Xamarin.Android 6.0 Support)**. Weitere Informationen zum Konfigurieren von Android-API-Ebene Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie einen Emulator verwenden, starten Sie den Android AVD-Manager, und erstellen Sie ein neues Gerät mit den folgenden Einstellungen:

-   Gerät: Nexus 5, 6 oder 9.
-   Ziel: Android 6.0 - API-Ebene 23
-   ABI: x86

Beispielsweise wird dieses virtuelle Gerät zum Emulieren einer Nexus-5 konfiguriert:

[![Konfigurieren ein AVD mit Nexus-5-Geräte, Android 6.0-Ziel und Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Wenn Sie ein physisches Gerät, z. B. eine 5 Nexus verwenden, können Sie eine Vorschau des Android Marshmallow bei 6 oder 9, installieren. Weitere Informationen zum Aktualisieren Ihres Geräts auf Android Marshmallow finden Sie unter [Hardware von Betriebssystemabbildern](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Neue Funktionen

Viele der Änderungen in Android Marshmallow Mittelpunkt stehen verbessern die Android-benutzerfreundlichkeit, Leistung zu erhöhen, und Beheben von Fehlern. Marshmallow wurde jedoch auch einige umfangreiche Änderungen in die Grundlagen der Android-Plattform. In den folgenden Abschnitten markieren Sie diese Verbesserungen und enthalten Links, um Ihnen den Einstieg in die neuen Funktionen für Android Marshmallow in Ihrer app verwenden können. 



### <a name="runtime-permissions"></a>Common Language Runtime-Berechtigungen

Die berechtigungssystems Android wurde deutlich optimiert und seit Android Lollipop vereinfacht. Erteilen Sie Benutzern in Android Marshmallow, dass die Berechtigungen für eine von Fall zu Fall zur Laufzeit statt am Mal zu installieren. Um dieses Feature für Android Marshmallow oder höher unterstützen, Entwerfen Sie Ihre app vom Benutzer Berechtigungen zur Laufzeit (in den Kontext, in dem die Berechtigungen erforderlich sind). Diese Änderung erleichtert es Benutzern, Ihre App beginnen sofort verwendet werden, da es das Installieren und aktualisieren Ihre app vereinfacht. 

Finden Sie unter [Anfordern von Common Language Runtime-Berechtigungen in Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) Weitere Details (einschließlich Codebeispielen) zum Implementieren von Common Language Runtime-Berechtigungen in Xamarin.Android-apps.
Xamarin bietet auch eine Beispiel-app, die veranschaulicht, wie die Common Language Runtime-Berechtigungen in Android Marshmallow (und höher) funktionieren: [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Diese Beispiel-app veranschaulicht Folgendes:

-   Gewusst wie: Überprüfen und Berechtigungen anfordern, zur Laufzeit.
-   Wie Berechtigungen für Android M-Geräte zu deklarieren.

So verwenden Sie diese Beispiel-app:

1.  Tippen Sie auf die **Kamera** oder **Kontakte** Schaltflächen für die Anzeige einer Berechtigungen anfordern, Dialogfeld.
2.  Erteilen Sie die Berechtigung zum Anzeigen der Kamera oder Kontakte Fragmente.

Weitere Informationen zu den neuen Funktionen der Laufzeit Berechtigungen in Android Marshmallow finden Sie unter [arbeiten mit Systemadministratorberechtigungen](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Authentifizierung-Erweiterungen

Android Marshmallow umfasst zwei Authentifizierung-Erweiterungen, mit deren Hilfe Kennwörter erforderlich:

-   **Authentifizierung Fingerabdruck** &ndash; limitieren Fingerabdruck verwendet, um Benutzer zu authentifizieren.

-   **Bestätigen Sie die Anmeldeinformationen** &ndash; authentifiziert Benutzer auf Grundlage, wie lange das Gerät entsperrt wurde.

Die Links und Beispiel-apps, die als Nächstes beschriebenen können Sie bekannte mit den neuen Features.


#### <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

Auf Geräten, die Fingerabdruck Scannen Hardware unterstützen, können Sie die neue `FingerPrintManager` Klasse, um einen Benutzer zu authentifizieren.
Weitere Informationen zu dem fingerabdruckfeature für die Authentifizierung in Android Marshmallow, finden Sie unter [Fingerabdruckauthentifizierung](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin bietet eine Beispiel-app, das veranschaulicht, wie registrierte Fingerabdrücke zu verwenden, um einen Benutzer in Ihrer app zu authentifizieren: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

So verwenden Sie diese Beispiel-app:

1.  Tippen Sie auf die **Kauf** Schaltfläche, um ein Fingerabdruck Authentifizierung-Dialogfeld zu öffnen.
2.  Scannen Sie Ihre registrierten Fingerabdruck zu authentifizieren.

Beachten Sie, dass diese Beispiel-app auf einem Gerät mit einem Fingerabdruckleser erforderlich ist.
Diese app nicht Ihren Fingerabdruck (oder Ihr Kennwort) gespeichert.



#### <a name="voice-interactions"></a>Voice-Interaktionen

Eingeführt in Android Marshmallow die neue Stimme Interaktionen-Funktion kann Benutzer Ihrer App verwenden Sie ihre Stimme, um Aktionen zu bestätigen, und wählen Sie aus einer Liste von Optionen. Weitere Informationen zu Interaktionen Stimme, finden Sie unter [Überblick über die Stimme Interaktion API](https://developers.google.com/voice-actions/interaction/). 

Finden Sie unter [hinzufügen eine Konversation auf Ihrem Android-App mit Aktivitäten, die Stimme](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) für weitere Details (einschließlich Codebeispielen) zum Implementieren von Aktivitäten, die Stimme in Xamarin.Android-apps.
Eine Beispiel-app ist verfügbar, die zeigt, wie die Sprach-Interaktion-API in einer app Xamarin.Android: [Stimme Interaktionen](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Bestätigen von Anmeldeinformationen

Mit dem neuen *bestätigen Anmeldeinformationen* Funktion von Android Marshmallow können Sie Benutzer zu merken, und geben anwendungsspezifischen Kennwörter durch Authentifizierung abhängig, wie lange das Gerät entsperrt wurde freigeben.
Zu diesem Zweck verwenden Sie die neue `SetUserAuthenticationValidityDurationSeconds` Methode der `KeyGenerator`. Verwenden der `KeyGuardManager`des `CreateConfirmDeviceCredentialIntent` Methode, um die Benutzer von innerhalb der app erneut zu authentifizieren. Weitere Informationen zu dieser neuen Funktion in Android Marshmallow, finden Sie unter [bestätigen Anmeldeinformationen](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin bietet eine Beispiel-app, das veranschaulicht, wie Sie die Anmeldeinformationen des Geräts (z. B. PIN, Muster und Kennwort) in Ihrer app zu verwenden: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

So verwenden Sie diese Beispiel-app:

1.  Einrichten einer sicheren Sperrbildschirm auf Ihrem Gerät (**Secure > Sicherheit > Screenlock**).
2.  Tippen Sie auf die **Kauf** Schaltfläche aus, und bestätigen Sie die Anmeldeinformationen des Schloss Bildschirm.



### <a name="chrome-custom-tabs"></a>Benutzerdefinierte Registerkarten Chrome

App-Entwickler stoßen eine Auswahl aus, wenn ein Benutzer eine URL tippt: die app kann entweder einen Browser starten oder einen in-app-Browser auf der Grundlage einer `WebView`. Beide Optionen eine besondere Herausforderung darstellen &ndash; ein hoher Kontextwechsel, die nicht anpassbar ist, beim Starten des Browsers wird `WebView`s freigeben Status nicht mit dem Browser. Weiterhin können Sie mit der `WebView`s zusätzlichen Wartungsaufwand hinzufügen kann. 

*Benutzerdefinierte Registerkarten Chrom* ermöglicht es Ihnen, einfache und elegante Websites mit der Leistungsfähigkeit von Chrome anzuzeigen, ohne die Benutzer Ihrer app zu lassen. Diese Funktion kann Ihre app mehr Kontrolle über den Benutzer webumgebung; Übergänge zwischen systemeigenen zu vereinfachen und Webinhalt nahtlosere ohne verwenden eine `WebView`. Ihre app kann auch beeinflussen, wie Chrome sucht und idealer durch das Anpassen der folgenden: 

-   Symbolleistenfarbe

-   Geben Sie ein, und Beenden von Animationen

-   Benutzerdefinierte Aktionen im Menü Chrome Symbolleiste und einen Überlauf

-   Chrome vor Beginn und den Inhalt vorab abzurufen (für schnelles Laden)

Um diese Funktion in Ihrer app Xamarin.Android nutzen zu können, herunterladen und Installieren der [Bibliothek für Android unterstützt benutzerdefinierte Registerkarten](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Weitere Informationen zu diesem Feature finden Sie unter [Chrome benutzerdefinierte Registerkarten](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Material Design-Unterstützungsbibliothek

Android Lollipop eingeführt [Material Entwurf](http://www.google.com/design/spec/material-design/introduction.html) als eine neue Sprache für Entwurf, aktualisieren Sie die Android-Umgebung (finden Sie unter [Design "Material"](~/android/user-interface/material-theme.md) Informationen zur Verwendung von Material Entwurf in Xamarin.Android-apps). Mit Android Marshmallow Google eingeführt der *Android Entwurf Unterstützungsbibliothek* app erleichtern Entwicklern das Material übernehmen Aussehen und Verhalten entwerfen. Diese Bibliothek enthält die folgenden Komponenten:

-   **CoordinatorLayout** &ndash; neuen `CoordinatorLayout` Widget ist ähnlich, aber leistungsstärker als eine `FrameLayout`. Können Sie `CoordinatorLayout` als Container für untergeordnete Ansichten oder als ein Layout auf der obersten Ebene, und es bietet eine `layout_anchor` -Attribut, das zum Anker Ansichten relativ zu anderen Ansichten verwendet werden kann.

-   **Reduzieren von Symbolleisten** &ndash; neuen `CollapsingToolbarLayout` ist eine reduzierendes app-Leiste, die einen Wrapper für `Toolbar`. (Beachten Sie, dass die *app-Leiste* ist früher als "" bezeichnet wurde die *Aktionsleiste*.)

-   **Gleitkommawert Aktionsschaltfläche** &ndash; einer runden Schaltfläche, die die primäre Aktion für die app-Schnittstelle kennzeichnet.

-   **Gleitkommawert Bezeichnungen für die Bearbeitung von Text** &ndash; verwendet einen neuen `TextInputLayout` Widget (welche umschließt `EditText`) eine unverankerte Bezeichnung zu verzeichnen, wenn ein Hinweis ausgeblendet wird, wenn ein Benutzer Text eingibt.

-   **Navigationsansicht** &ndash; neuen `NavigationView` Widgets können Sie die öffnen Sie die Navigation auf eine Weise zu verwenden, einfacher zum Navigieren ist.

-   **Snackbar** &ndash; neuen `SnackBar` Widget ist ein lightweight Feedbackmechanismus (ähnlich wie ein Toast), die zeigt eine kurze Fehlermeldung am unteren Bildschirmrand oberhalb aller anderen Elemente auf dem Bildschirm angezeigt werden.

-   **Material Registerkarten** &ndash; neuen `TabLayout` Widget enthält ein horizontales Layout zur Anzeige von Registerkarten als Möglichkeit zum Implementieren der Navigation auf oberster Ebene in Ihrer app.

Nutzen der [Design-Unterstützungsbibliothek](http://developer.android.com/tools/support-library/features.html#design) in Ihrer app Xamarin.Android herunterladen und installieren Sie die Xamarin [Xamarin-Unterstützung Bibliotheksentwurf](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet-Paket.

Finden Sie unter [Material Eleganz mit Android Design-Unterstützungsbibliothek](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) für Weitere Informationen (einschließlich Codebeispielen) zur Verwendung der Material Design-Unterstützungsbibliothek in Xamarin.Android-apps.
Xamarin bietet eine Beispiel-app, die die neue Android Design-Bibliothek auf Xamarin.Android demos &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Dieses Beispiel veranschaulicht die folgenden Funktionen von der Design-Bibliothek:


-   Reduzieren die Symbolleiste
-   Unverankerte Aktionsschaltfläche
-   View-Verankerung
-   NavigationView
-   Snackbar

Weitere Informationen zu der Design-Bibliothek, finden Sie unter [Android Entwurf Unterstützungsbibliothek](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) in der Android-Entwickler-Blog.


### <a name="additional-library-updates"></a>Zusätzliche Bibliothek Updates

Zusätzlich zu den Android Marshmallow hat Google verwandte Updates für verschiedene Android Kernbibliotheken angekündigt. Xamarin bietet Xamarin.Android Unterstützung für diese Updates über mehrere Vorschauversion NuGet-Pakete: 

-   [Google Play-Dienste](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; die neueste Version von Google Play-Dienste enthält die neue *App lädt* -Funktion, die Benutzer ihrer app für Freunde freigeben kann. Weitere Informationen zu diesem Feature finden Sie unter [der App erweitern die Reichweite mit Google App lädt](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Android-Unterstützungsbibliotheken](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; diese NuGets bieten Funktionen, die nur für Bibliotheks-APIs verfügbar sind und gleichzeitig die abwärtskompatible Versionen von Android-Framework-APIs bieten. 

-   [Bibliothek für Android Wearable](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; dieses NuGet umfasst Google Play-Dienste-Bindungen. Die neueste Version der wearable-Bibliothek enthält neue Funktionen (einschließlich einfachere Navigation für benutzerdefinierte apps) für die die Dach Android-Plattform. 


## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Marshmallow eingeführt, und es wird erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android auf Marshmallow. Außerdem wird eine Übersicht über die meisten neuen Android Marshmallow Funktionen für Xamarin.Android Entwicklung bereitgestellt.


## <a name="related-links"></a>Verwandte Links

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Abrufen der Android-SDK](https://developer.android.com/sdk/index.html#Other)
- [Übersicht über die Funktionen](https://developer.android.com/preview/api-overview.html)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (Beispiel)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
