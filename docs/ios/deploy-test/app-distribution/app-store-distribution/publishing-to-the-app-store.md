---
title: Veröffentlichen von Xamarin.iOS-Apps im App Store
description: In diesem Artikel wird beschrieben, wie Sie eine Xamarin.iOS-Anwendung für die Verteilung im App Store konfigurieren, erstellen und veröffentlichen.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 822f2ae57241cd51f9e9c4eb2b63c75d30867d83
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "79190332"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>Veröffentlichen von Xamarin.iOS-Apps im App Store

Ein App-Entwickler muss eine App gemeinsam mit Screenshots, einer Beschreibung, Symbolen und anderen Informationen zunächst übermitteln, bevor er sie im [App Store](https://www.apple.com/ios/app-store/) veröffentlichen kann. Nachdem die Veröffentlichung der App genehmigt wurde, wird sie im App Store verfügbar gemacht. Dort können Benutzer die App erwerben und direkt auf ihrem iOS-Gerät installieren.

In diesem Leitfaden werden die Schritte zur Vorbereitung einer App für die Veröffentlichung im App Store und zum Übermitteln der App an Apple zur Überprüfung beschrieben. Folgende Aspekte werden besprochen:

> [!div class="checklist"]
>
> - Befolgen der Richtlinien für die Überprüfung im App Store
> - Einrichten einer App-ID und Berechtigungen
> - Bereitstellen eines Symbols für den App Store und von App-Symbolen
> - Einrichten eines App Store-Bereitstellungsprofils
> - Aktualisieren der Buildkonfiguration **Release**
> - Konfigurieren Ihrer App in iTunes Connect
> - Erstellen Ihrer App und Übermittlung an Apple

> [!IMPORTANT]
> Apple [hat mitgeteilt](https://developer.apple.com/ios/submit/), dass ab März 2019 alle Apps und Updates, die an den App Store gesendet werden, mit dem iOS 12.1 SDK oder höher, das in Xcode 10.1 oder höher enthalten ist, erstellt worden sein müssen.
> Apps müssen ebenso die Bildschirmgrößen des iPhone XS und des iPad Pro in 12,9" unterstützen.

## <a name="app-store-guidelines"></a>App Store-Richtlinien

Stellen Sie sicher, dass Ihre App die von Apple vorgegebenen Standards ([App Store Review Guidelines (Richtlinien für die Überprüfung im App Store)](https://developer.apple.com/appstore/resources/approval/guidelines.html)) erfüllt, bevor Sie eine App für die Veröffentlichung im App Store übermitteln.
Wenn Sie eine App an den App Store senden, prüft Apple diese App, um sicherzustellen, dass sie die Anforderungen erfüllt. Sollte dies nicht der Fall sein, wird die App von Apple abgelehnt. Dann müssen Sie auf die angegebenen Probleme reagieren und die App erneut einreichen.
Deshalb wird empfohlen, die Richtlinien so früh wie möglich im Entwicklungsprozess zu lesen.

Beachten Sie bei der Übermittlung einer App Folgendes:

1. Achten Sie darauf, dass die Beschreibung der App zu deren Funktionen passt.
2. Stellen Sie sicher, dass die App bei normaler Nutzung nicht abstürzt. Dies schließt die Nutzung auf jedem iOS-Gerät, das sie unterstützt, mit ein.

Sehen Sie sich außerdem [Ressourcen bezüglich des App Store](https://developer.apple.com/app-store/resources/) an, die von Apple bereitstellt werden.

## <a name="set-up-an-app-id-and-entitlements"></a>Einrichten einer App-ID und Berechtigungen

Jede iOS-App hat eine eindeutige App-ID, die wiederum verknüpfte Anwendungsdienste aufweist, die sogenannten *Berechtigungen*. Durch Berechtigungen können Apps unterschiedliche Vorgänge durchführen, z.B. das Erhalten von Pushbenachrichtigungen, der Zugriff auf iOS-Features wie HealthKit usw.

Öffnen Sie das [Apple Developer-Portal](https://developer.apple.com/account/), und führen Sie die folgenden Schritt durch, um eine App-ID zu erstellen und erforderliche Berechtigungen auszuwählen:

1. Wählen Sie im Abschnitt **Certificates, IDs & Profiles** (Zertifikate, Bezeichner und Profile) die Option **Bezeichner > App-IDs** aus.
2. Klicken Sie auf die **+** -Schaltfläche, und stellen Sie einen **Namen** und eine **Bündel-ID** für die neue Anwendung bereit.
3. Scrollen Sie zum unteren Ende des Bildschirms, und wählen Sie die **App Services** aus, die für Ihre Xamarin.iOS-Anwendung erforderlich sein sollen. App-Dienste werden im Leitfaden [Arbeiten mit Funktionen in Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md) weiter beschrieben.
4. Klicken Sie auf die Schaltfläche **Continue** (Weiter), und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Wenn Sie Ihre App-ID definieren, müssen Sie nicht nur die erforderlichen Anwendungsdienste auswählen und konfigurieren sondern auch die App-ID und Berechtigungen in Ihrem Xamarin.iOS-Projekt. Bearbeiten Sie dafür die Dateien **Info.plist** und **Entitlements.plist**. Weitere Informationen finden Sie im Leitfaden [Arbeiten mit Berechtigungen in Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md). Dort wird das Erstellen der Datei **Entitlements.plist** und die Bedeutung der verschiedenen enthaltenen Berechtigungseinstellungen beschrieben.

## <a name="include-an-app-store-icon"></a>Bereitstellen eines App Store-Symbols

Wenn Sie eine App an Apple übermitteln, achten Sie darauf, dass sie einen Ressourcenkatalog mit einem App Store-Symbol enthält. Sehen Sie sich den Leitfaden [App Store icons in Xamarin.iOS (App Store-Symbole in Xamarin.iOS)](~/ios/app-fundamentals/images-icons/app-store-icon.md) an, in dem dieser Vorgang näher erläutert wird.

## <a name="set-the-apps-icons-and-launch-screens"></a>Festlegen der App-Symbole und Startbildschirme

Eine App wird nur dann von Apple im App Store veröffentlicht, wenn sie entsprechende Symbole und Startbildschirme für alle iOS-Geräte aufweist, auf denen sie ausgeführt werden kann. Weitere Informationen zum Einrichten von App-Symbolen und Startbildschirmen finden Sie in den folgenden Leitfäden:

- [Anwendungssymbole in Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Startbildschirme für Xamarin.iOS-Apps](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>Erstellen und Bereitstellen eines App Store-Bereitstellungsprofils

iOS verwendet *Bereitstellungsprofile*, um zu steuern, wie ein bestimmtes Anwendungsbuild bereitgestellt werden kann. Hierbei handelt es sich um Dateien mit Informationen zu dem Zertifikat, das zum Signieren einer App verwendet wurde, der App-ID sowie dem Ort, an dem die App installiert werden kann. Für die Entwicklung und Ad-hoc-Verteilung enthält das Bereitstellungsprofil auch eine Liste der zulässigen Geräte, auf denen die App bereitgestellt werden kann. Für die Verteilung im App Store enthält das Profil jedoch nur Zertifikats- und App-ID-Informationen, da eine öffentliche Verteilung nur über den App Store möglich ist.

Führen Sie die folgenden Schritte durch, um ein App Store-Bereitstellungsprofil zu erstellen und zu installieren:

1. Melden Sie sich beim [Apple Developer-Portal](https://developer.apple.com/account/) an.
2. Navigieren Sie unter **Certificates, IDs & Profiles** (Zertifikate, Bezeichner und Profile) zu **Bereitstellungsprofile > Verteilung**.
3. Klicken Sie auf **+** , wählen Sie **App Store** aus, und klicken Sie anschließend auf **Continue** (Weiter).
4. Wählen Sie die **App-ID** Ihrer App aus der Liste aus, und klicken Sie auf **Weiter**.
5. Wählen Sie ein Signaturzertifikat aus, und klicken Sie auf **Weiter**.
6. Geben Sie einen **Profilnamen** ein, und klicken Sie auf **Weiter**, um das Profil zu erstellen.
7. Verwenden Sie die Tools der [Apple-Kontoverwaltung](~/cross-platform/macios/apple-account-management.md) von Xamarin, um das gerade erstellte Bereitstellungsprofil auf Ihren Mac herunterzuladen. Wenn Sie auf einem Mac arbeiten, können Sie das Bereitstellungsprofil auch direkt über das Apple Developer-Portal herunterladen und es durch einen Doppelklick installieren.

Eine ausführliche Anleitung finden Sie unter [Manuelle Bereitstellung für Xamarin.iOS](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) und [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="update-the-release-build-configuration"></a>Aktualisieren der Buildkonfiguration „Release“

Neue Xamarin.iOS-Projekte richten automatisch die _Buildkonfigurationen_ **Debuggen** und **Release** ein. Führen Sie die folgenden Schritte durch, um einen **Releasebuild** zu konfigurieren:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie **Info.plist** über den **Lösungspad**. Klicken Sie auf **Manuelle Bereitstellung**. Speichern und schließen Sie die Datei.
2. Klicken Sie mit der rechten Maustaste auf den **Projektnamen** im **Lösungspad**, wählen Sie **Optionen** aus, und navigieren Sie dann zur Registerkarte **iOS-Build**.
3. Legen Sie **Konfiguration** auf **Release** und **Plattform** auf **iPhone** fest.
4. Wählen Sie ein SDK aus der **SDK-Versionsliste** aus, um den Build mit einem spezifischen iOS-SDK durchzuführen. Behalten Sie andernfalls den **Standardwert** bei.
5. Durch Verknüpfungen können Sie die Gesamtgröße Ihrer Anwendung senken, da nicht verwendeter Code ausgeschlossen wird. In den meisten Fällen sollte das **Linkerverhalten** auf den Standardwert **Nur Framework-SDKs verknüpfen** festgelegt werden. Wenn Sie die Option **Keine Verknüpfung** verwenden, lehnt Apple möglicherweise die App aufgrund des Vorhandenseins nicht öffentlicher iOS-APIs in Xamarin.iOS ab, die mit der Option **Nur Framework SDKs verknüpfen** verknüpft sind. Die Option **Alle verknüpfen** sollten Sie mit Bedacht wählen, da damit Code aus allen Assemblys im Projekt, einschließlich Drittanbieterbibliotheken, entfernt wird. Es kann auch Code entfernt werden, der von der Drittanbieterbibliothek nur über Reflexion verwendet werden kann, die vom Linker nicht erkannt wird. Dieser führt statische Codeanalyse aus, um zu bestimmen, welcher Bibliothekscode verwendet wird. Verwenden Sie die Option **Alle verknüpfen** umsichtig, da Sie möglicherweise einige Klassen und/oder Methoden usw. manuell beibehalten müssen, um Laufzeitfehler durch fehlenden Code zu vermeiden. Weitere Informationen finden Sie im Leitfaden zum [Verknüpfen von Xamarin.iOS-Apps](~/ios/deploy-test/linker.md).
6. Aktivieren Sie **PNG-Bilder optimieren**, um die Größe Ihrer Anwendung weiter zu senken.
7. Das Debuggen sollte _nicht_ aktiviert sein, um den Build nicht unnötig zu vergrößern.
8. Wählen Sie für iOS 11 eine Gerätearchitektur aus, die **ARM64** unterstützt. Weitere Informationen zu Builds auf 64-Bit-iOS-Geräten finden Sie im Abschnitt **Enabling 64 Bit Builds of Xamarin.iOS Apps (64-Bit-Builds in Xamarin.iOS-Apps aktiveren)** der Dokumentation [32/64 bit Platform Considerations (Überlegungen zu 32-/64-Bit-Plattformen)](~/cross-platform/macios/32-and-64/index.md).
9. Mit dem **LLVM**-Compiler können Sie kürzeren und schnelleren Code erstellen. Diese Option führt jedoch zu längeren Kompilierzeiten.
10. Entsprechend der Anforderungen Ihrer Anwendung können Sie auch den Typ der für die **Internationalisierung** verwendeten und eingerichteten **Garbage Collection** anpassen.

    Wenn Sie die oben beschriebenen Optionen festgelegt haben, sollten Ihre Buildeinstellungen in etwa wie folgt aussehen:

    ![iOS-Buildeinstellungen](publishing-to-the-app-store-images/build-m157.png "iOS-Buildeinstellungen")

    Lesen Sie außerdem den Leitfaden [Abläufe beim Erstellen von iOS-Builds](~/ios/deploy-test/ios-build-mechanics.md), in dem Buildeinstellungen ausführlicher beschrieben werden.

11. Navigieren Sie zur Registerkarte **iOS-Bundlesignierung**. Wenn die dort angezeigten Optionen nicht bearbeitet werden können, stellen Sie sicher, dass **Manuelle Bereitstellung** in der Datei **Info.plist** ausgewählt ist.
12. Stellen Sie sicher, dass **Konfiguration** auf **Release** und **Plattform** auf **iPhone** festgelegt ist.
13. Legen Sie **Signierungsidentität** auf **Verteilung (Automatisch)** fest.
14. Wählen Sie für **Bereitstellungsprofil** das [oben erstellte App Store-Bereitstellungsprofil](#create-and-install-an-app-store-provisioning-profile) aus.

    Die Bundlesignierungsoptionen Ihres Projekts sollten jetzt in etwa wie folgt aussehen:

    ![iOS-Bundlesignierung](publishing-to-the-app-store-images/bundleSigning-m157.png "iOS-Bundlesignierung")

15. Klicken Sie auf **OK**, um die Änderungen an den Projekteigenschaften zu speichern.

# <a name="visual-studio-2019"></a>[Visual Studio 2019](#tab/windows)

1. Stellen Sie sicher, dass Visual Studio 2019 [mit einem Mac-Buildhost gekoppelt wurde](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den **Projektnamen**, und wählen Sie **Eigenschaften**.
3. Navigieren Sie zur Registerkarte **iOS-Build**, und legen Sie **Konfiguration** auf **Release** und **Plattform** auf **iPhone** fest.
4. Wählen Sie ein SDK aus der **SDK-Versionsliste** aus, um den Build mit einem spezifischen iOS-SDK durchzuführen. Behalten Sie andernfalls den **Standardwert** bei.
5. Durch Verknüpfungen können Sie die Gesamtgröße Ihrer Anwendung senken, da nicht verwendeter Code ausgeschlossen wird. In den meisten Fällen sollte das **Linkerverhalten** auf den Standardwert **Nur Framework-SDKs verknüpfen** festgelegt werden. Wenn Sie die Option **Keine Verknüpfung** verwenden, lehnt Apple möglicherweise die App aufgrund des Vorhandenseins nicht öffentlicher iOS-APIs in Xamarin.iOS ab, die mit der Option **Nur Framework SDKs verknüpfen** verknüpft sind. Die Option **Alle verknüpfen** sollten Sie mit Bedacht wählen, da damit Code aus allen Assemblys im Projekt, einschließlich Drittanbieterbibliotheken, entfernt wird. Es kann auch Code entfernt werden, der von der Drittanbieterbibliothek nur über Reflexion verwendet werden kann, die vom Linker nicht erkennt wird. Dieser führt statische Codeanalyse aus, um zu bestimmen, welcher Bibliothekscode verwendet wird. Verwenden Sie die Option **Alle verknüpfen** umsichtig, da Sie möglicherweise einige Klassen und/oder Methoden usw. manuell beibehalten müssen, um Laufzeitfehler durch fehlenden Code zu vermeiden. Weitere Informationen finden Sie im Leitfaden zum [Verknüpfen von Xamarin.iOS-Apps](~/ios/deploy-test/linker.md).
6. Aktivieren Sie **PNG-Bilder optimieren**, um die Größe Ihrer Anwendung weiter zu senken.
7. Das Debuggen sollte nicht aktiviert sein, um den Build nicht unnötig zu vergrößern.
8. Wählen Sie für iOS 11 eine Gerätearchitektur aus, die **ARM64** unterstützt. Weitere Informationen zu Builds auf 64-Bit-iOS-Geräten finden Sie im Abschnitt **Enabling 64 Bit Builds of Xamarin.iOS Apps (64-Bit-Builds in Xamarin.iOS-Apps aktiveren)** der Dokumentation [32/64 bit Platform Considerations (Überlegungen zu 32-/64-Bit-Plattformen)](~/cross-platform/macios/32-and-64/index.md).
9. Mit dem **LLVM**-Compiler können Sie kürzeren und schnelleren Code erstellen. Diese Option führt jedoch zu längeren Kompilierzeiten.
10. Entsprechend der Anforderungen Ihrer Anwendung können Sie auch den Typ der für die **Internationalisierung** verwendeten und eingerichteten **Garbage Collection** anpassen.

    Wenn Sie die oben beschriebenen Optionen festgelegt haben, sollten Ihre Buildeinstellungen in etwa wie folgt aussehen:

    ![iOS-Buildeinstellungen](publishing-to-the-app-store-images/build-w157.png "iOS-Buildeinstellungen")

    Lesen Sie außerdem den Leitfaden [Abläufe beim Erstellen von iOS-Builds](~/ios/deploy-test/ios-build-mechanics.md), in dem Buildeinstellungen ausführlicher beschrieben werden.

11. Navigieren Sie zur Registerkarte **iOS-Bundlesignierung**. Stellen Sie sicher, dass **Konfiguration** auf **Release** und **Plattform** auf **iPhone** festgelegt ist und dass **Manuelle Bereitstellung** aktiviert wurde.
12. Legen Sie **Signierungsidentität** auf **Verteilung (Automatisch)** fest.
13. Wählen Sie für **Bereitstellungsprofil** das [oben erstellte App Store-Bereitstellungsprofil](#create-and-install-an-app-store-provisioning-profile) aus.

    Die Bundlesignierungsoptionen Ihres Projekts sollten jetzt in etwa wie folgt aussehen:

    ![Einstellungen der iOS-Bundlesignierung](publishing-to-the-app-store-images/bundleSigning-w157.png "Einstellungen der iOS-Bundlesignierung")

14. Speichern Sie die Buildkonfiguration, und schließen Sie sie.

# <a name="visual-studio-2017"></a>[Visual Studio 2017](#tab/win-vs2017)

1. Stellen Sie sicher, dass Visual Studio 2017 [mit einem Mac-Buildhost gekoppelt wurde](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den **Projektnamen**, und wählen Sie **Eigenschaften**.
3. Navigieren Sie zur Registerkarte **iOS-Build**, und legen Sie **Konfiguration** auf **Release** und **Plattform** auf **iPhone** fest.
4. Wählen Sie ein SDK aus der **SDK-Versionsliste** aus, um den Build mit einem spezifischen iOS-SDK durchzuführen. Behalten Sie andernfalls den **Standardwert** bei.
5. Durch Verknüpfungen können Sie die Gesamtgröße Ihrer Anwendung senken, da nicht verwendeter Code ausgeschlossen wird. In den meisten Fällen sollte das **Linkerverhalten** auf den Standardwert **Nur Framework-SDKs verknüpfen** festgelegt werden. Wenn Sie die Option **Keine Verknüpfung** verwenden, lehnt Apple möglicherweise die App aufgrund des Vorhandenseins nicht öffentlicher iOS-APIs in Xamarin.iOS ab, die mit der Option **Nur Framework SDKs verknüpfen** verknüpft sind. Die Option **Alle verknüpfen** sollten Sie mit Bedacht wählen, da damit Code aus allen Assemblys im Projekt, einschließlich Drittanbieterbibliotheken, entfernt wird. Es kann auch Code entfernt werden, der von der Drittanbieterbibliothek nur über Reflexion verwendet werden kann, die vom Linker nicht erkennt wird. Dieser führt statische Codeanalyse aus, um zu bestimmen, welcher Bibliothekscode verwendet wird. Verwenden Sie die Option **Alle verknüpfen** umsichtig, da Sie möglicherweise einige Klassen und/oder Methoden usw. manuell beibehalten müssen, um Laufzeitfehler durch fehlenden Code zu vermeiden. Weitere Informationen finden Sie im Leitfaden zum [Verknüpfen von Xamarin.iOS-Apps](~/ios/deploy-test/linker.md).
6. Aktivieren Sie **PNG-Bilder optimieren**, um die Größe Ihrer Anwendung weiter zu senken.
7. Das Debuggen sollte nicht aktiviert sein, um den Build nicht unnötig zu vergrößern.
8. Wählen Sie für iOS 11 eine Gerätearchitektur aus, die **ARM64** unterstützt. Weitere Informationen zu Builds auf 64-Bit-iOS-Geräten finden Sie im Abschnitt **Enabling 64 Bit Builds of Xamarin.iOS Apps (64-Bit-Builds in Xamarin.iOS-Apps aktiveren)** der Dokumentation [32/64 bit Platform Considerations (Überlegungen zu 32-/64-Bit-Plattformen)](~/cross-platform/macios/32-and-64/index.md).
9. Mit dem **LLVM**-Compiler können Sie kürzeren und schnelleren Code erstellen. Diese Option führt jedoch zu längeren Kompilierzeiten.
10. Entsprechend der Anforderungen Ihrer Anwendung können Sie auch den Typ der für die **Internationalisierung** verwendeten und eingerichteten **Garbage Collection** anpassen.

    Wenn Sie die oben beschriebenen Optionen festgelegt haben, sollten Ihre Buildeinstellungen in etwa wie folgt aussehen:

    ![iOS-Buildeinstellungen](publishing-to-the-app-store-images/build-w157.png "iOS-Buildeinstellungen")

    Lesen Sie außerdem den Leitfaden [Abläufe beim Erstellen von iOS-Builds](~/ios/deploy-test/ios-build-mechanics.md), in dem Buildeinstellungen ausführlicher beschrieben werden.

11. Navigieren Sie zur Registerkarte **iOS-Bundlesignierung**. Stellen Sie sicher, dass **Konfiguration** auf **Release** und **Plattform** auf **iPhone** festgelegt ist und dass **Manuelle Bereitstellung** aktiviert wurde.
12. Legen Sie **Signierungsidentität** auf **Verteilung (Automatisch)** fest.
13. Wählen Sie für **Bereitstellungsprofil** das [oben erstellte App Store-Bereitstellungsprofil](#create-and-install-an-app-store-provisioning-profile) aus.

    Die Bundlesignierungsoptionen Ihres Projekts sollten jetzt in etwa wie folgt aussehen:

    ![Einstellungen der iOS-Bundlesignierungg](publishing-to-the-app-store-images/bundleSigning-w157.png "Einstellungen der iOS-Bundlesignierung")

14. Navigieren Sie zur Registerkarte **iOS IPA-Optionen**.
15. Stellen Sie sicher, dass **Konfiguration** auf **Release** und **Plattform** auf **iPhone** festgelegt ist.
16. Aktivieren Sie das Kontrollkästchen **iTunes Package Archive (IPA) erstellen**. Durch diese Einstellung erzeugt jeder **Releasebuild** (da dies die ausgewählte Konfiguration ist) eine IPA-Datei. Diese Datei kann an Apple zur Veröffentlichung im App Store übermittelt werden.

    > [!NOTE]
    > **iTunes-Metadaten** und **iTunes-Grafiken** sind für die Veröffentlichung im App Store nicht erforderlich. Weitere Informationen finden Sie unter [Die Datei „iTunesMetadata.plist“ in Xamarin.iOS-Apps](~/ios/deploy-test/app-distribution/itunesmetadata.md) und [iTunes Artwork (iTunes-Grafiken)](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork).

17. Geben Sie einen Namen im Feld **Paketname** ein, um einen Namen einer IPA-Datei anzugeben, der vom Namen des Xamarin.iOS-Projekts abweicht.

    ![Einstellungen der iOS-Bundlesignierung](publishing-to-the-app-store-images/ipaOptions-w157.png "Einstellungen der iOS-Bundlesignierung")

18. Speichern Sie die Buildkonfiguration, und schließen Sie sie.

-----

## <a name="configure-your-app-in-itunes-connect"></a>Konfigurieren Ihrer App in iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com) ist eine Suite von webbasierten Tools, mit denen Sie Ihre iOS-Anwendungen im App Store verwalten können. Ihre Xamarin.iOS-Anwendung muss ordnungsgemäß in iTunes Connect konfiguriert werden, bevor sie zur Überprüfung an Apple gesendet und im App Store freigegeben werden kann.

Im Leitfaden [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) erfahren Sie, wie Sie dies durchführen können.

## <a name="build-and-submit-your-app"></a>Erstellen und Übermitteln Ihrer App

Sobald Sie alle erforderlichen Buildeinstellungen vorgenommen haben und iTunes Connect auf Ihre Übermittlung wartet, können Sie Ihre App erstellen und an Apple übermitteln.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Wählen Sie in Visual Studio für Mac die Buildkonfiguration **Release** und ein Gerät (keinen Simulator) aus, für das Sie den Build durchführen möchten.

    ![Buildkonfiguration und Plattformauswahl](publishing-to-the-app-store-images/chooseConfig-m157.png "Buildkonfiguration und Plattformauswahl")

2. Wählen Sie aus dem **Build**-Menü die Option **Archive for Publishing** (Für Veröffentlichung archivieren):
3. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt. Klicken Sie auf **Signieren und verteilen...** , um den Veröffentlichungs-Assistenten zu öffnen.

    ![Screenshot: Schaltfläche „Signieren und verteilen“ in der Archivansicht](publishing-to-the-app-store-images/archives-mac.png "Screenshot: Schaltfläche „Signieren und verteilen“ in der Archivansicht")

    > [!NOTE]
    > Standardmäßig zeigt die Ansicht **Archive** nur die Archive für die geöffnete Projektmappe an. Aktivieren Sie das Kontrollkästchen **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen. Es empfiehlt sich, alle Archive beizubehalten, damit die darin enthaltenen Debuginformationen verwendet werden können, um Absturzberichte bei Bedarf durch Symbole zu ersetzen.

4. Wählen Sie als Verteilungskanal den **App Store** aus. Klicken Sie auf **Weiter**.

5. Wählen Sie **Upload** als Ziel aus. Klicken Sie auf **Weiter**.

6. Wählen Sie im Fenster **Bereitstellungsprofil** Ihrer Signierungsidentität, App und Ihr Bereitstellungsprofil aus. Klicken Sie auf **Weiter**.

    ![Screenshot: Seite des Assistenten des Bereitstellungsprofils mit einer gültigen Signierungsidentität, einer App und einer Auswahl des Bereitstellungsprofils](publishing-to-the-app-store-images/provProfileSelect-mac.png "Screenshot: Seite des Assistenten des Bereitstellungsprofils mit einer gültigen Signierungsidentität, einer App und einem ausgewählten Bereitstellungsprofil")

7. Wählen Sie im Fenster **App Store Connect information** (Informationen zu App Store Connect) im Menü einen Apple ID-Benutzernamen aus, und geben Sie [ein App-spezifisches Kennwort](https://support.apple.com/ht204397) ein. Klicken Sie auf **Weiter**.

    ![Screenshot: Seite des Assistenten der App Store Connect-Informationen mit einem ausgewählten Apple-ID-Benutzernamen](publishing-to-the-app-store-images/connectInfo-mac.png "Screenshot: Seite des Assistenten der App Store Connect-Informationen mit einem ausgewählten Apple-ID-Benutzernamen")

8. Überprüfen Sie die Details Ihres Pakets, und klicken Sie auf **Veröffentlichen**. Nachdem Sie einen Speicherort für die IPA-Datei ausgewählt haben, wird Ihre App vom Assistenten in App Store Connect hochgeladen.

    > [!NOTE]
    > Es ist möglich, dass Apple Apps ablehnt, bei denen **iTunesMetadata.plist** in der IPA-Datei enthalten ist, wodurch es zu einem Fehler wie dem folgenden kommen kann:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > In [diesem Post auf Xamarin Forums](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) wird beschrieben, wie Sie diesen Fehler umgehen können.

# <a name="visual-studio-2019"></a>[Visual Studio 2019](#tab/windows)

> [!NOTE]
> Das Veröffentlichen im App Store wird in Visual Studio 2019 Version 16.3 und höher unterstützt.

1. Stellen Sie sicher, dass Visual Studio 2019 [mit einem Mac-Buildhost gekoppelt wurde](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Wählen Sie im Dropdownmenü **Projektmappenkonfigurationen** **Release** und aus dem Dropdownmenü **Projektmappenplattformen** **iPhone** aus.

    ![Screenshot: Visual Studio-Symbolleiste mit der Lösungskonfiguration für die Freigabe, der Lösungsplattform für das iPhone und dem Ziel für das Gerät](publishing-to-the-app-store-images/chooseConfig-w157.png "Screenshot: Visual Studio-Symbolleiste mit der Lösungskonfiguration für die Freigabe, der Lösungsplattform für das iPhone und dem Ziel für das Gerät")

3. Wählen Sie im Menü **Erstellen** die Option **Archiv...** aus. Dadurch wird der **Archiv-Manager** geöffnet und mit dem Erstellen eines Archivs begonnen.

4. Sobald das Archiv erstellt wurde, klicken Sie auf **Verteilen...** , um den Assistenten zur Veröffentlichung zu öffnen.

    ![Screenshot: Schaltfläche „Verteilen“ in der Archiv-Manager-Ansicht](publishing-to-the-app-store-images/archives-win.png "Screenshot: Schaltfläche „Verteilen“ in der Archiv-Manager-Ansicht")

5. Wählen Sie als Verteilungskanal den **App Store** aus.

6. Wählen Sie Ihre Signierungsidentität und das Bereitstellungsprofil aus. Klicken Sie auf **Upload to Store** (In Store hochladen).

    ![Screenshot: Veröffentlichungs-Assistent mit einer gültigen Signierungsidentität und einer Auswahl des Bereitstellungsprofils](publishing-to-the-app-store-images/provProfileSelect-win.png "Screenshot: Veröffentlichungs-Assistent mit einer gültigen Signierungsidentität und einer Auswahl des Bereitstellungsprofils")

7. Geben Sie Ihre Apple-ID und [ein für die App spezifisches Kennwort](https://support.apple.com/ht204397) ein. Klicken Sie auf **OK**, um mit dem Hochladen Ihrer App in App Store Connect zu beginnen.

    ![Screenshot: Popupfenster zur Eingabe Ihrer Apple-ID und Ihres App-spezifischen Kennworts](publishing-to-the-app-store-images/connectInfo-win.png "Screenshot: Popupfenster zur Eingabe Ihrer Apple-ID und Ihres App-spezifischen Kennworts")

# <a name="visual-studio-2017"></a>[Visual Studio 2017](#tab/win-vs2017)

> [!NOTE]
> Visual Studio 2017 unterstützt den Workflow für die vollständige Veröffentlichung in Visual Studio für Mac und Visual Studio 2019 nicht.
>
> Die Schritt unten gelten für Xcode 10.
>
> Sie können nach wie vor die folgenden Schritte ausführen, um eine .IPA-Datei zu erstellen. Verwenden Sie für die Bereitstellung im App Store mithilfe von Xcode 11 (was für die iOS 13-Unterstützung erforderlich ist) [Visual Studio für Mac](?tabs=macos#build-and-submit-your-app).

1. Stellen Sie sicher, dass Visual Studio 2017 [mit einem Mac-Buildhost gekoppelt wurde](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Wählen Sie im Dropdownmenü **Projektmappenkonfigurationen** in Visual Studio 2017 **Release** und aus dem Dropdownmenü **Projektmappenplattformen** **iPhone** aus.

    ![Buildkonfiguration und Plattformauswahl](publishing-to-the-app-store-images/chooseConfig-w157.png "Buildkonfiguration und Plattformauswahl")

3. Erstellen Sie das Projekt. Dadurch wird eine IPA-Datei erstellt.

    > [!NOTE]
    > Im Abschnitt [Aktualisieren der Buildkonfiguration „Release“](#update-the-release-build-configuration) in diesem Artikel haben Sie die Buildeinstellungen der App konfiguriert, sodass eine IPA-Datei für jeden **Releasebuild** erstellt wird.

4. Klicken Sie mit der rechten Maustaste auf den Namen des Xamarin.iOS-Projekts im Visual Studio 2019- oder im Visual Studio 2017-**Projektmappen-Explorer**, und wählen Sie **Ordner in Datei-Explorer öffnen** aus, um die IPA-Datei auf einem Windows-Computer zu finden. Navigieren Sie dann im gerade geöffneten Windows-**Datei-Explorer** zum Unterverzeichnis **bin/iPhone/Release**. Wenn Sie [den Ausgabespeicherort der IPA-Datei nicht angepasst haben](#customize-the-ipa-location), sollte sich die Datei in diesem Verzeichnis befinden.
5. Klicken Sie mit der rechten Maustaste auf den Namen des Xamarin.iOS-Projekts im Visual Studio 2019-**Projektmappen-Explorer** (unter Windows), und wählen Sie **IPA-Datei auf Buildserver anzeigen** aus, um stattdessen die IPA-Datei auf dem Mac-Buildhost anzuzeigen. Dadurch wird ein **Finder**-Fenster auf dem Mac-Buildhost mit ausgewählter IPA-Datei geöffnet.

    > [!TIP]
    >
    > Die folgenden Schritte sind nur gültig, wenn Sie Xcode 10 verwenden und für iOS 12 und früher entwickeln.
    >
    > Wenn Sie mit Xcode 11 (iOS 13) eine Bereitstellung im App Store durchführen möchten, sollten Sie [Visual Studio für Mac](?tabs=macos#build-and-submit-your-app) verwenden, um Ihre App zu erstellen und hochzuladen. Das **Anwendungsladeprogramm** ist für Xcode 11 nicht verfügbar.

6. Öffnen Sie **Application Loader** auf dem Mac-Buildhost. Wählen Sie in Xcode **Xcode > Open Developer Tool (Entwicklertool öffnen) > Application Loader** aus.

    > [!NOTE]
    > Weitere Informationen zu diesem Tool finden Sie in der [Apple-Dokumentation zu Application Loader](https://help.apple.com/itc/apploader/#/apdS673accdb).

7. Melden Sie sich bei Application Loader an (Sie müssen ein [App-spezifisches Kennwort](https://support.apple.com/ht204397) für Ihre Apple-ID erstellen).
8. Wählen Sie **Ihre App übermitteln** aus, und klicken Sie auf die Schaltfläche **Auswählen**:

    ![Auswählen von „Ihre App übermitteln“](publishing-to-the-app-store-images/publishvs01.png "Auswählen von „Ihre App übermitteln“")

9. Wählen Sie die zuvor erstellte IPA-Datei aus, und klicken Sie auf **OK**.
10. Die Datei wird durch den Application Loader überprüft:

    ![Der Validierungsbildschirm](publishing-to-the-app-store-images/publishvs02.png "Der Validierungsbildschirm")

11. Klicken Sie auf die Schaltfläche **Weiter**. Die Anwendung wird nun für den App Store überprüft:

    ![Überprüfen für den App Store](publishing-to-the-app-store-images/publishvs03.png "Überprüfen für den App Store")

12. Klicken Sie auf die Schaltfläche **Senden**, um die Anwendung zur Überprüfung an Apple zu senden.
13. Sie werden vom Application Loader informiert, sobald die Datei erfolgreich hochgeladen wurde.

    > [!NOTE]
    > Es ist möglich, dass Apple Apps ablehnt, bei denen **iTunesMetadata.plist** in der IPA-Datei enthalten ist, wodurch es zu einem Fehler wie dem folgenden kommen kann:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > In [diesem Post auf Xamarin Forums](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) wird beschrieben, wie Sie diesen Fehler umgehen können.

-----

## <a name="itunes-connect-status"></a>Status in iTunes Connect

Melden Sie sich bei iTunes Connect an, und wählen Sie Ihre App aus, um den Status Ihrer Übermittlung einzusehen. Der Status sollte zunächst **Waiting for Review** (Überprüfung ausstehend) lauten, vorrübergehend kann er aber auch **Upload Received** (Upload eingegangen) lauten, wenn die Übermittlung verarbeitet wird.

![Warten auf Überprüfung](publishing-to-the-app-store-images/image21.png "Warten auf Überprüfung")

## <a name="tips-and-tricks"></a>Tipps und Tricks

### <a name="customize-the-ipa-location"></a>Anpassen des IPA-Speicherorts

Die **MSBuild**-Eigenschaft `IpaPackageDir` ermöglicht das Anpassen des Ausgabespeicherorts der IPA-Datei. Wenn für einen benutzerdefinierten Speicherort `IpaPackageDir` festgelegt ist, wird die IPA-Datei an diesem Speicherort anstatt im Standardunterverzeichnis mit Zeitstempel abgelegt. Dies kann beim Erstellen von automatisierten Builds nützlich sein, die für eine korrekte Ausführung einen bestimmten Verzeichnispfad benötigen, wie beispielsweise für fortlaufende Integrationsbuilds (CI, Continuous Integration).

Die neue Eigenschaft kann auf unterschiedliche Weise verwendet werden. Für eine Ausgabe der IPA-Datei in das frühere Standardverzeichnis (wie in Xamarin.iOS 9.6 und früher) können Sie die `IpaPackageDir`-Eigenschaft z.B. mit einer der folgenden Methoden auf `$(OutputPath)` festlegen. Beide Methoden sind mit allen Unified API-Xamarin.iOS-Builds kompatibel. Hierzu zählen sowohl IDE-Builds als auch Befehlszeilenbuilds, die **msbuild** oder **mdtool** verwenden:

- Die erste Möglichkeit besteht darin, die `IpaPackageDir`-Eigenschaft in einem `<PropertyGroup>`-Element in einer **MSBuild**-Datei festzulegen. Sie könnten z.B. die folgende `<PropertyGroup>` am Ende der CSPROJ-Datei des iOS-App-Projekts hinzufügen (unmittelbar vor dem `</Project>`-Endtag):

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Eine bessere Möglichkeit ist das Hinzufügen eines `<IpaPackageDir>`-Elements am Ende der vorhandenen `<PropertyGroup>`, die der beim Erstellen der IPA-Datei verwendeten Konfiguration entspricht. Der Vorteil dieser Möglichkeit ist, dass das Projekt hier für die Kompatibilität mit einer zukünftigen Einstellung auf der Projekteigenschaftenseite der iOS-IPA-Optionen vorbereitet wird. Wenn Sie derzeit die Konfiguration `Release|iPhone` zum Erstellen der IPA-Datei verwenden, könnte die vollständige aktualisierte Eigenschaftengruppe etwa folgendermaßen aussehen:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
       <Optimize>true</Optimize>
       <OutputPath>bin\iPhone\Release</OutputPath>
       <ErrorReport>prompt</ErrorReport>
       <WarningLevel>4</WarningLevel>
       <ConsolePause>false</ConsolePause>
       <CodesignKey>iPhone Developer</CodesignKey>
       <MtouchUseSGen>true</MtouchUseSGen>
       <MtouchUseRefCounting>true</MtouchUseRefCounting>
       <MtouchFloat32>true</MtouchFloat32>
       <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
       <MtouchLink>SdkOnly</MtouchLink>
       <MtouchArch>ARMv7, ARM64</MtouchArch>
       <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
       <MtouchTlsProvider>Default</MtouchTlsProvider>
       <BuildIpa>true</BuildIpa>
       <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

Eine alternative Methode für **msbuild**-Befehlszeilenbuilds ist das Hinzufügen eines `/p:`-Befehlszeilenarguments, um die `IpaPackageDir`-Eigenschaft festzulegen. Beachten Sie, dass **msbuild** in diesem Fall `$()`-Ausdrücke nicht erweitert, die in der Befehlszeile übergeben werden. Daher kann die `$(OutputPath)`-Syntax nicht verwendet werden. Geben Sie stattdessen einen vollständigen Pfadnamen an.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Unter Mac könnte es folgendermaßen aussehen:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Nachdem Ihr Verteilungsbuild erstellt und archiviert wurde, können Sie nun Ihre Anwendung an iTunes Connect übermitteln.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird die Konfiguration, das Erstellen und das Übermitteln einer iOS-App zum Veröffentlichen im App Store beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Apple Developer-Portal (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [App Store Review Guidelines (Richtlinien für die Überprüfung im App Store) (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Common App Rejections (Häufige Ablehnungsgründe für Apps) (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Arbeiten mit Funktionen in Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Arbeiten mit Berechtigungen in Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md)
- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Anwendungssymbole in Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Startbildschirme für Xamarin.iOS-Apps](~/ios/app-fundamentals/images-icons/launch-screens.md)
