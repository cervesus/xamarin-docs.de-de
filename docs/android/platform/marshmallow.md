---
title: Features von Marshmallow
description: Dieser Artikel unterstützt Sie beim Einstieg in die Verwendung von Xamarin.Android zum Entwickeln von Apps für Android 6.0 Marshmallow.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: fb1ba92be9527d490b3d34bd4c0e454b0a750837
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019987"
---
# <a name="marshmallow-features"></a>Features von Marshmallow

_Dieser Artikel unterstützt Sie beim Einstieg in die Verwendung von Xamarin.Android zum Entwickeln von Apps für Android 6.0 Marshmallow._

Dieser Artikel bietet einen Überblick über die neuen Features in Android 6.0 Marshmallow, erläutert, wie Sie Xamarin.Android für die Entwicklung für Android Marshmallow vorbereiten, und stellt Links zu Beispielanwendungen bereit, die die Verwendung der neuen Android Marshmallow-Features in Xamarin.Android-Apps veranschaulichen. 

## <a name="overview"></a>Übersicht

[Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html) ist das nächste Android-Hauptrelease nach Android Lollipop.
Xamarin.Android unterstützt Android Marshmallow und bietet Folgendes:

- **Bindungen auf API-Ebene 23/Android 6.0-Bindungen** &ndash; Android 6.0 fügt eine Vielzahl neuer APIs für die unten beschriebenen neuen Features hinzu. Diese APIs sind für Xamarin.Android-Apps verfügbar, wenn Sie für API-Ebene 23 entwickeln. Weitere Informationen zu Android 6.0-APIs finden Sie unter [Android 6.0-APIs](https://developer.android.com/preview/api-overview.html). 

[![Herobild von Tablets und Smartphones, auf denen Marshmallow ausgeführt wird](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Das Marshmallow-Release dient hauptsächlich zur Optimierung und Qualitätsverbesserung, bietet aber auch viele neue Features, die für Xamarin.Android-Entwickler interessant sind. Zu diesen Features zählen: 

- **Laufzeitberechtigungen** &ndash; diese Erweiterung ermöglicht es Benutzern, Sicherheitsberechtigungen von Fall zu Fall zur Laufzeit zu genehmigen. 

- **Verbesserungen bei der Authentifizierung** &ndash; ab Android Marshmallow können Apps Fingerabdrucksensoren zum Authentifizieren von Benutzern verwenden, und ein neues Feature zum *Bestätigen von Anmeldeinformationen* minimiert die Notwendigkeit der Eingabe von Kennwörtern. 

- **App-Verknüpfungen** &ndash; dieses Feature verknüpft Web-Apps automatisch mit Webdomänen, sodass die **App-Auswahl** nicht mehr benötigt wird. 

- **Direkte Freigabe** &ndash; Sie können *Ziele für eine direkte Freigabe* definieren. Mit diesem Feature können Benutzer Inhalte schnell und intuitiv für andere Apps freigeben. 

- **Interaktionen per Stimme** &ndash; mit dieser neuen API können Sie Sprachfunktionen für Unterhaltungen in Ihre App integrieren. 

- **4K-Anzeigemodus** &ndash; unter Android Marshmallow kann Ihre App auf Hardwarekomponenten, die dies unterstützen, eine Anzeigeauflösung von 4K anfordern. 

- **Neue Audiofeatures** &ndash; ab Marshmallow unterstützt Android das MIDI-Protokoll. Das Release bietet auch neue Klassen zum Erstellen von Objekten für die Erfassung und Wiedergabe digitaler Audiodaten sowie neue API-Hooks zum Zuordnen von Audiodaten und Eingabegeräten. 

- **Neue Videofeatures** &ndash; Marshmallow bietet eine neue Klasse zum synchronisierten Rendern von Audio- und Videostreams. Diese Klasse bietet auch Unterstützung für eine dynamische Wiedergaberate. 

- **Android for Work** &ndash; Marshmallow enthält erweiterte Steuerelemente für unternehmenseigene Einzelbenutzergeräte. Das Release unterstützt die automatische Installation und Deinstallation von Apps durch den Gerätebesitzer, die automatische Anwendung von Systemupdates, verbesserte Zertifikatverwaltung, Nachverfolgung der Datennutzung, Berechtigungsverwaltung und Benachrichtigungen zum Arbeitsstatus. 

- **Unterstützungsbibliothek für Material Design** &ndash; Die neue *Unterstützungsbibliothek für Entwürfe* bietet Entwurfskomponenten und -muster, sodass Sie Ihre App einfacher mit dem Verhalten und Aussehen von Material Design entwickeln können. 

Darüber hinaus wurden mit Marshmallow viele wichtige Bibliotheksupdates für Android veröffentlicht, die neue Features sowohl für Marshmallow als auch für frühere Android-Versionen bereitstellen.

Darüber hinaus wurden mit Marshmallow viele wichtige Bibliotheksupdates für Android veröffentlicht, die neue Features sowohl für Marshmallow als auch für frühere Android-Versionen bereitstellen. In diesem Artikel wird erläutert, wie Sie mit Android 6.0 Marshmallow Apps entwickeln, und Sie erhalten einen Überblick über die wichtigsten neuen Features in diesem Release. 

## <a name="requirements"></a>Anforderungen

Für die Verwendung der neuen Features von Android Marshmallow in Xamarin-basierten Apps gelten die folgenden Voraussetzungen: 

- **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 oder höher muss entweder mit Visual Studio oder mit Xamarin Studio installiert und konfiguriert sein.

- **Visual Studio für Mac** oder **Visual Studio** &ndash; wenn Sie Visual Studio für Mac verwenden, ist Version 5.9.7.22 oder höher erforderlich. Wenn Sie Visual Studio verwenden, ist Version 3.11.1537 oder höher der Xamarin-Tools für Visual Studio erforderlich. 

- **Android SDK** &ndash; Android SDK 6.0 (API 23) oder höher muss über den Android-SDK-Manager installiert werden.

- **Java Developer Kit** &ndash; Xamarin.Android erfordert [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) oder höher, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 1.8 unterstützt auch niedrigere API-Ebenen als 24, Marshmallow eingeschlossen). Die 64-Bit-Version von JDK 1.8 wird benötigt, wenn Sie benutzerdefinierte Steuerelemente oder die Forms-Vorschau verwenden.

Sie können weiterhin [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie speziell für die API-Ebene 23 oder früher entwickeln. 

## <a name="getting-started"></a>Erste Schritte

Um mit der Nutzung von Android Marshmallow mit Xamarin.Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android Marshmallow-Projekt erstellen können: 

1. Installieren Sie die neuesten Xamarin-Updates aus dem **Stable Channel**. 

2. Installieren Sie die SDK-Pakete und Tools für Android 6.0 Marshmallow.

3. Erstellen Sie ein neues Xamarin.Android-Projekt für die Zielplattform Android 6.0 Marshmallow (API-Ebene 23). 

4. Konfigurieren Sie einen Emulator oder ein Gerät für Android Marshmallow.

Diese Schritte werden in den folgenden Abschnitten im Einzelnen erläutert:

### <a name="install-xamarin-updates"></a>Installieren von Xamarin-Updates

Um Xamarin für die Unterstützung von Android 6.0 Marshmallow zu aktualisieren, ändern Sie den Updatechannel zu **Stable**, und installieren Sie alle Updates. Weitere Informationen zum Installieren von Updates über den Updatechannel finden Sie unter [Change the Updates Channel](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel) (Ändern des Updatechannels). 

### <a name="install-the-android-60-sdk"></a>Installieren des Android 6.0 SDK

Wenn Sie ein Xamarin.Android-Projekt für Android Marshmallow erstellen möchten, müssen Sie zunächst den Android-SDK-Manager verwenden, um das SDK für Android 6.0 zu installieren:

- Starten Sie den Android-SDK-Manager (in Visual Studio für Mac über **Extras > SDK-Manager öffnen**; in Visual Studio über **Extras > Android > Android-SDK-Manager**), und installieren Sie die neuesten Android SDK Tools:

    [![Auswählen der Android SDK Tools im Android-SDK-Manager](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

- Installieren Sie außerdem die neuesten **Android 6.0** SDK-Pakete:

    [![Auswählen der Android 6.0 SDK-Pakete im Android-SDK-Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Sie müssen Version 24.3.4 oder höher der Android SDK Tools installieren.
Weitere Informationen zur Verwendung des Android-SDK-Managers zum Installieren des Android 6.0 SDK finden Sie im Artikel zum [SDK-Manager](https://developer.android.com/tools/help/sdk-manager.html).

### <a name="start-a-xamarinandroid-project"></a>Starten eines Xamarin.Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android (Hallo, Android)](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Android-Projekten. 

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versionseinstellungen für Android 6.0 Marshmallow konfigurieren. Um Marshmallow als Ziel für Ihr Projekt zu verwenden, müssen Sie Ihr Projekt für die **API-Ebene 23 (Unterstützung für Xamarin.Android v.6.0)** konfigurieren. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie einen Emulator verwenden, starten Sie den Android Virtual Device Manager (AVD Manager), und erstellen Sie unter Verwendung der folgenden Einstellungen ein neues Gerät:

- Gerät: Nexus 5, 6 oder 9.
- Ziel: Android 6.0 – API-Ebene 23
- ABI: x86

Dieses virtuelle Gerät ist beispielsweise für die Emulation eines Nexus 5 konfiguriert:

[![Konfigurieren eines AVD für ein Nexus 5-Gerät, Android 6.0 als Ziel und Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Wenn Sie ein physisches Gerät wie z. B. ein Nexus 5, 6 oder 9 verwenden, können Sie ein Vorschauimage von Android Marshmallow installieren. Weitere Informationen zum Aktualisieren von Geräten auf Android Marshmallow finden Sie unter [Hardware System Images](https://developer.android.com/preview/download.html#images).

## <a name="new-features"></a>Neue Funktionen

Viele der in Android Marshmallow eingeführten Änderungen drehen sich um die Verbesserung des Benutzererlebnisses für Android-Benutzer, die Erhöhung der Leistung und die Behebung von Fehlern. Marshmallow führt jedoch auch einige wesentliche Änderungen an den Grundlagen der Android-Plattform ein. Diese Verbesserungen werden in den folgenden Abschnitten näher beleuchtet. Zudem erhalten Sie Links mit weiteren Informationen für den Einstieg in die Nutzung der neuen Android Marshmallow-Features in Ihrer App. 

### <a name="runtime-permissions"></a>Laufzeitberechtigungen

Das Berechtigungssystem von Android wurde seit Android Lollipop erheblich optimiert und vereinfacht. In Android Marshmallow gewähren Benutzer Berechtigungen von Fall zu Fall zur Laufzeit, nicht mehr zum Zeitpunkt der Installation. Um dieses Feature unter Android Marshmallow und höher zu unterstützen, müssen Sie Ihre App so entwerfen, dass der Benutzer zur Laufzeit zur Eingabe von Berechtigungen aufgefordert wird (im Kontext der Anwendung, für die die Berechtigungen benötigt werden). Diese Änderung optimiert den Installations- und Upgradeprozess Ihrer App und sorgt dafür, dass Benutzer Ihre App sofort verwenden können. 

Weitere Details sowie Codebeispiele zur Implementierung von Laufzeitberechtigungen in Xamarin.Android-Apps finden Sie unter [Requesting Runtime Permissions in Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) (Anfordern von Laufzeitberechtigungen in Android Marshmallow).
Xamarin bietet auch eine Beispiel-App, die veranschaulicht, wie Laufzeitberechtigungen in Android Marshmallow (und höher) funktionieren: [RuntimePermissions](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions).

Diese Beispiel-App demonstriert Folgendes:

- Überprüfen und Anfordern von Berechtigungen zur Laufzeit
- Deklarieren von Berechtigungen für Android Marshmallow-Geräte

So verwenden Sie diese Beispiel-App:

1. Tippen Sie auf die Schaltfläche **Kamera** oder **Kontakte**, um ein Dialogfeld zum Anfordern von Berechtigungen anzuzeigen.
2. Gewähren Sie Berechtigungen zum Anzeigen von Kamera- oder Kontaktfragmenten.

Weitere Informationen zu den neuen Features für Laufzeitberechtigungen in Android Marshmallow finden Sie unter [Working with System Permissions](https://developer.android.com/preview/features/runtime-permissions.html) (Arbeiten mit Systemberechtigungen).

### <a name="authentication-enhancements"></a>Verbesserungen bei der Authentifizierung

Android Marshmallow enthält zwei Verbesserungen bei der Authentifizierung, die die Notwendigkeit der Eingabe von Kennwörtern eliminieren:

- **Fingerabdruckauthentifizierung** &ndash; verwendet einen Fingerabdruckscan zum Authentifizieren von Benutzern.

- **Bestätigen von Anmeldeinformationen** &ndash; authentifiziert Benutzer basierend auf Informationen dazu, wie lange ein Gerät entsperrt ist.

Lernen Sie diese neuen Features anhand der Links und Beispiel-Apps in den nächsten Abschnitten kennen.

#### <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

Auf Geräten, die Hardware für Fingerabdruckscans unterstützen, können Sie die neue `FingerPrintManager`-Klasse zum Authentifizieren von Benutzern verwenden.
Weitere Informationen zum Feature der Fingerabdruckauthentifizierung in Android Marshmallow finden Sie unter [Fingerprint Authentication](https://developer.android.com/preview/api-overview.html#fingerprint-authentication) (Fingerabdruckauthentifizierung).

Xamarin stellt eine Beispiel-App bereit, mit der die Verwendung von registrierten Fingerabdrücken zum Authentifizieren eines Benutzers in Ihrer App veranschaulicht wird: [FingerprintDialog](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog).

So verwenden Sie diese Beispiel-App:

1. Tippen Sie auf die Schaltfläche **Kaufen**, um ein Dialogfeld zur Authentifizierung per Fingerabdruck zu öffnen.
2. Scannen Sie Ihren registrierten Fingerabdruck, um sich zu authentifizieren.

Beachten Sie, dass diese Beispiel-App ein Gerät mit Fingerabdruckleser erfordert.
Die App speichert Ihren Fingerabdruck (oder Ihr Kennwort) nicht.

#### <a name="voice-interactions"></a>Interaktionen per Stimme

Das in Android Marshmallow neu eingeführte Feature für Interaktionen per Stimme ermöglicht es Benutzern Ihrer App, mithilfe ihrer Stimme Aktionen zu bestätigen und Optionen aus einer Liste auszuwählen. Weitere Informationen zu Interaktionen per Stimme finden Sie unter [Overview of the Voice Interaction API](https://developers.google.com/voice-actions/interaction/) (Übersicht über die API für Interaktionen per Stimme). 

Informationen sowie Codebeispiele zum Implementieren von solchen Interaktionen in Xamarin.Android-Apps finden Sie unter [Add a Conversation to your Android App with Voice Interactions](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) (Hinzufügen einer Unterhaltung zu Ihrer Android-App mit Interaktionen per Stimme).
Es steht eine Beispiel-App zur Verfügung, die die Verwendung der API für Interaktionen per Stimme in einer Xamarin.Android-App veranschaulicht: [Voice Interactions](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).

#### <a name="confirm-credential"></a>Bestätigen von Anmeldeinformationen

Das neue Android Marshmallow-Feature zum *Bestätigen von Anmeldeinformationen* authentifiziert Benutzer basierend auf Informationen dazu, wie lange ein Gerät entsperrt ist. So müssen Benutzer sich nicht mehr Kennwörter für jede einzelne App merken und diese eingeben.
Verwenden Sie zum Implementieren dieses Features die neue `SetUserAuthenticationValidityDurationSeconds`-Methode von `KeyGenerator`. Verwenden Sie die `CreateConfirmDeviceCredentialIntent`-Methode von `KeyGuardManager`, um einen Benutzer aus der App heraus erneut zu authentifizieren. Weitere Informationen zu diesem neuen Feature in Android Marshmallow finden Sie unter [Confirm Credential](https://developer.android.com/preview/api-overview.html#confirm-credential) (Bestätigen von Anmeldeinformationen).

Xamarin stellt eine Beispiel-App bereit, die die Verwendung von Geräteanmeldeinformationen (z. B. PIN, Muster oder Kennwort) in Ihrer App veranschaulicht: [ConfirmCredential](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)

So verwenden Sie diese Beispiel-App:

1. Richten Sie auf Ihrem Gerät einen sicheren Sperrbildschirm ein (**Sicher > Sicherheit > Bildschirmsperre**).
2. Tippen Sie auf die Schaltfläche **Kaufen**, und bestätigen Sie die Anmeldeinformationen für den sicheren Sperrbildschirm.

### <a name="chrome-custom-tabs"></a>Benutzerdefinierte Chrome-Registerkarten

App-Entwickler haben zwei Optionen, wenn ein Benutzer auf eine URL tippt: Die App kann entweder einen Browser starten oder einen `WebView`-basierten App-internen Browser verwenden. Beide Optionen bergen gewisse Herausforderungen &ndash; das Starten eines Browsers ist ein schwerwiegender Kontextwechsel, der sich nicht anpassen lässt. `WebView`s dagegen geben den Status nicht für den Browser frei. Die Verwendung von `WebView`s kann zudem zusätzlichen Verwaltungsaufwand bedeuten. 

Mit *benutzerdefinierten Chrome-Registerkarten* können Sie Websites einfach und elegant mit der Funktionalität von Chrome anzeigen, ohne dass Ihre Benutzer Ihre App verlassen müssen. Mit diesem Feature kann Ihre App die Weboberfläche der Benutzer besser steuern, und es sorgt für einen nahtlosen Übergang zwischen nativen und Webinhalten, ohne dass Sie auf einen `WebView` zurückgreifen müssen. Ihre App kann außerdem das Verhalten und Aussehen von Chrome beeinflussen, indem Folgendes angepasst wird: 

- Farbe der Symbolleiste

- Animationen für das Starten und Beenden

- Benutzerdefinierte Aktionen in der Symbolleiste und dem Überlaufmenü von Chrome

- Vorabstart und Vorababruf von Inhalten in Chrome (zum schnelleren Laden)

Um dieses Feature in Ihrer Xamarin.Android-App zu nutzen, laden Sie die [Android Support Custom Tabs Library](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/) (Android-Bibliothek zur Unterstützung von benutzerdefinierten Registerkarten) herunter und installieren sie.
Weitere Informationen zu diesem Feature finden Sie unter [Chrome Custom Tabs](https://developer.chrome.com/multidevice/android/customtabs) (Benutzerdefinierte Chrome-Registerkarten).

### <a name="material-design-support-library"></a>Unterstützungsbibliothek für Material Design

In Android Lollipop wurde [Material Design](https://www.google.com/design/spec/material-design/introduction.html) als neue Entwurfssprache eingeführt, um die Android-Benutzeroberfläche zu aktualisieren (Informationen zur Verwendung von Material Design in Xamarin.Android-Apps finden Sie unter [Material Theme](~/android/user-interface/material-theme.md)). Mit Android Marshmallow hat Google die *Android Design Support Library* (Unterstützungsbibliothek für Entwürfe für Android) eingeführt, um es App-Entwicklern zu erleichtern, das Verhalten und Aussehen von Material Design zu übernehmen. Diese Bibliothek schließt folgende Komponenten ein:

- **CoordinatorLayout** &ndash; das neue `CoordinatorLayout`-Widget ähnelt einem `FrameLayout`, ist aber leistungsfähiger. Sie können `CoordinatorLayout` als Container für untergeordnete Ansichten oder als Layout auf oberster Ebene verwenden. Das Widget bietet auch ein `layout_anchor`-Attribut, mit dem sich Ansichten in Relation zu anderen Ansichten verankern lassen.

- **Reduzierbare Symbolleisten** &ndash; das neue `CollapsingToolbarLayout` ist eine reduzierbare App-Leiste, die einen Wrapper für `Toolbar` darstellt. (Beachten Sie, dass die *App-Leiste* früher als *Aktionsleiste* bezeichnet wurde.)

- **Abdockbare Aktionsschaltfläche** &ndash; eine runde Schaltfläche, die die primäre Aktion auf der Benutzeroberfläche Ihrer App bezeichnet.

- **Abdockbare Bezeichnungen zum Bearbeiten von Text** &ndash; verwendet ein neues `TextInputLayout`-Widget (das `EditText` umschließt), um eine abdockbare Bezeichnung anzuzeigen, wenn bei der Eingabe von Text durch den Benutzer ein Hinweis ausgeblendet ist.

- **Navigationsansicht** &ndash; das neue `NavigationView`-Widget hilft Ihnen dabei, den Navigationsdrawer auf eine Weise einzusetzen, die Ihren Benutzern die Navigation erleichtert.

- **Snackbar** &ndash; das neue `SnackBar`-Widget ist ein schlanker Feedbackmechanismus (ähnlich einem Popup), der am unteren Seitenrand über allen anderen Bildschirmelementen eine kurze Meldung anzeigt.

- **Registerkarten im Material-Stil** &ndash; das neue `TabLayout`-Widget stellt ein horizontales Layout für die Anzeige von Registerkarten bereit, mit dem Sie die Navigation auf oberster Ebene in Ihrer App implementieren können.

Um die [Unterstützungsbibliothek für Entwürfe](https://developer.android.com/tools/support-library/features.html#design) in Ihrer Xamarin.Android-App zu nutzen, laden Sie das NuGet-Paket [Xamarin Support Library Design](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) herunter und installieren es.

Weitere Informationen sowie Codebeispiele zur Verwendung der Material Design-Unterstützungsbibliothek in Xamarin.Android-Apps finden Sie unter [Beautiful Material Design with the Android Support Design Library](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) (Ansprechendes Material Design mit der Unterstützungsbibliothek für Entwürfe für Android).
Xamarin stellt eine Beispiel-App zur Demonstration der neuen Android-Entwurfsbibliothek in Xamarin.Android bereit &ndash; [Cheesesquare](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-cheesesquare).
Dieses Beispiel veranschaulicht die folgenden Funktionen der Entwurfsbibliothek:

- Reduzierbare Symbolleiste
- Abdockbare Aktionsschaltfläche
- Verankerung von Ansichten
- NavigationView
- Snackbar

Weitere Informationen zur Entwurfsbibliothek finden Sie im Android Developer-Blog unter [Android Design Support Library](https://android-developers.googleblog.com/2015/05/android-design-support-library.html) (Unterstützungsbibliothek für Entwürfe für Android).

### <a name="additional-library-updates"></a>Zusätzliche Bibliotheksupdates

Zusätzlich zu Android Marshmallow hat Google zugehörige Updates für verschiedene Android-Kernbibliotheken angekündigt. Xamarin bietet Xamarin.Android-Unterstützung für diese Updates über verschiedene NuGet-Pakete in der Vorschauversion: 

- [Google Play Services](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; die neueste Version von Google Play Services enthält das neue Feature für *App-Einladungen* (App Invites), über das Benutzer ihre App für Freunde freigeben können. Weitere Informationen zu diesem Feature finden Sie unter [Expand Your App's Reach with Google's App Invites](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/) (Erweitern der Reichweite Ihrer App mit App-Einladungen von Google). 

- [Android Support Libraries](https://www.nuget.org/packages?q=xamarin+support+library) (Android-Unterstützungsbibliotheken) &ndash; diese NuGet-Pakete bieten Features, die nur für Bibliotheks-APIs verfügbar sind, und stellen gleichzeitig abwärtskompatible Versionen der Android-Framework-APIs bereit. 

- [Android Wearable Library](https://www.nuget.org/packages/Xamarin.Android.Wear) (Android-Bibliothek für Wearables) &ndash; dieses NuGet-Paket enthält Bindungen für Google Play Services. Die neueste Version der Bibliothek für Wearables bietet neue Features (beispielsweise eine einfachere Navigation für benutzerdefinierte Apps) für die Android Wear-Plattform. 

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Marshmallow vorgestellt und erläutert, wie Sie die neuesten Tools und Pakete für die Xamarin.Android-Entwicklung für Marshmallow installieren und konfigurieren. Der Artikel bot auch einen Überblick über die spannendsten neuen Android Marshmallow-Features für die Xamarin.Android-Entwicklung.

## <a name="related-links"></a>Verwandte Links

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Herunterladen des Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Übersicht über die Funktionen](https://developer.android.com/preview/api-overview.html)
- [Anmerkungen zu dieser Version](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [RuntimePermissions (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions)
- [ConfirmCredential (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)
- [FingerprintDialog (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
