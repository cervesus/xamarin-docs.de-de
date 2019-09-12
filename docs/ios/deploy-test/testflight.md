---
title: Verwenden von TestFlight zum Verteilen von Xamarin.iOS-Apps
description: TestFlight ist jetzt im Besitz von Apple und die wichtigste Methode zum Betatesten Ihrer Xamarin.iOS-Apps. Dieser Artikel führt Sie durch alle Schritte des TestFlight-Prozesses, vom Hochladen Ihrer App bis hin zum Arbeiten mit iTunes Connect.
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 43dce7fe6d2a4a976879b1f583711d767dcacc7c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756210"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>Verwenden von TestFlight zum Verteilen von Xamarin.iOS-Apps

_TestFlight ist jetzt im Besitz von Apple und die wichtigste Methode zum Betatesten Ihrer Xamarin.iOS-Apps. Dieser Artikel führt Sie durch alle Schritte des TestFlight-Prozesses: vom Hochladen Ihrer App bis hin zum Arbeiten mit iTunes Connect._

Das Durchführen von Beta-Tests ist ein wesentlicher Bestandteil des Softwareentwicklungszyklus, und viele plattformübergreifende Anwendungen bieten an, diesen Prozess zu optimieren, wie z.B. [HockeyApp](http://hockeyapp.net/features/), [Applause](http://www.applause.com/mobile-app-testing), und natürlich das Native App Beta-Testing für Android-Apps von Google Play. Dieses Dokument konzentriert sich auf TestFlight von Apple.

TestFlight ist der Beta-Test-Dienst für iOS-Apps von Apple, auf den nur über [iTunes Connect](https://itunesconnect.apple.com/) zugegriffen werden kann. Es ist derzeit für iOS 8.0 Apps und höher verfügbar. TestFlight ermöglicht das Durchführen von Beta-Tests mit internen und externen Benutzern, und aufgrund einer Beta-App-Prüfung für die letztgenannte Aufgabe, wird sichergestellt, dass der endgültige Prüfungsprozess bei der Veröffentlichung im App Store wesentlich vereinfacht wird

Zuvor wurde die Binärdatei in Visual Studio für Mac generiert und zur Verteilung an Tester auf die TestFlightApp-Website hochgeladen. Der neue Prozess umfasst eine ganze Reihe von Verbesserungen, die Ihnen eine hohe Qualität der Apps und gut getestete Apps im App Store bringen. Beispiel:

- Die Beta-App-Prüfung, die für externe Tests benötigt wird, gewährleistet eine höhere Erfolgswahrscheinlichkeit für Ihre endgültige App Store-Prüfung, da beide die Einhaltung der Apple Richtlinien erfordern.
- Vor dem Hochladen muss die App in iTunes Connect registriert werden. Dadurch wird sichergestellt, dass es keine Konflikte zwischen den Bereitstellungsprofilen, Namen und Zertifikaten gibt.
- Die TestFlight-App ist jetzt eine echte iOS-App, sodass sie schneller ausgeführt wird.
- Ist die Beta-Testphase einmal abgeschlossen, ist der Verschiebungsprozess der App zur Prüfung schnell und effizient; ein Klick auf die Schaltfläche genügt.

## <a name="requirements"></a>Requirements (Anforderungen)

Nur Apps mit iOS 8.0 oder höher können über TestFlight getestet werden.

Alle Tester müssen die App auf mindestens einem iOS 8-Gerät testen. Bewährte Methoden geben jedoch vor, dass Ihre App auf allen iOS-Versionen getestet werden soll.

## <a name="provisioning"></a>Bereitstellung

Sie müssen ein *App Store-Verteilungsprofil* mit der neuen Betaberechtigung erstellen, um Ihre Builds mit TestFlight zu testen. Diese Berechtigung ermöglicht das Durchführen von Beta-Tests über TestFlight, und jede **neue** App Store-Verteilung enthält automatisch diese Berechtigung. Sie können die ausführlichen Anweisungen im Handbuch [Creating a Distribution Profile (Erstellen eines Verteilungsprofils)](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) befolgen, um ein neues Profil zu generieren.

Sie können prüfen, ob das Verteilungsprofil die Beta-Berechtigung enthält wenn Sie [Ihren erstellten Xcode überprüfen](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), wie unten gezeigt:

[![](testflight-images/validate-build.png "Die App an Apple übermitteln")](testflight-images/validate-build.png#lightbox)

## <a name="testflight-workflow"></a>TestFlight-Workflow

Der folgende Workflow beschreibt die erforderlichen Schritte zum Starten der Verwendung von TestFlight für das Durchführen von Beta-Tests für Ihre App:

1. Erstellen Sie für neue Apps einen [iTunes Connect-Datensatz](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).
2. [Archivieren und Veröffentlichen](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) Sie Ihre Anwendung in iTunes Connect.
3. Verwalten der Beta-Tests:
    - Metadaten hinzufügen.
    - Interne Benutzer hinzufügen:
      - Maximal 25 Benutzer
    - Externe Benutzer hinzufügen:
      - Maximal 1000 Benutzer.
      - Erfordert eine Beta-Test-Prüfung, die die Einhaltung der Apple-Richtlinien erforderlich macht.
4. Erhalten Sie Feedback von Benutzern, reagieren Sie darauf, und kehren Sie zu Schritt 2 zurück.

## <a name="create-an-itunes-connect-record"></a>Erstellen eines iTunes Connect-Eintrags

1. Melden Sie sich im [iTunes Connect Portal](https://itunesconnect.apple.com/) mithilfe Ihrer Apple-Entwickler-Anmeldeinformationen an.
2. Wählen Sie **Meine Apps** aus:

    [![](testflight-images/my-apps.png "„My Apps“ (Meine Apps) auswählen")](testflight-images/my-apps.png#lightbox)

3. Auf dem **Meine Apps**-Bildschirm, klicken Sie auf die **+** -Schaltfläche in der linken oberen Ecke des Bildschirms, um eine neue App hinzuzufügen. Wenn Sie Mac- und iOS-Entwicklerkonten besitzen, werden Sie aufgefordert, hier den neuen App-Typ auszuwählen.

Es wird Ihnen das Übermittlungsfenster **New iOS App** (Neue iOS-App) angezeigt, in dem genau die gleiche Information wie in der „Info.plist“-Datei Ihrer App enthalten sein muss.

Weitere Informationen zum Erstellen eines neuen iTunes Connect-Datensatzes finden Sie im Handbuch [Erstellen eines iTunes Connect-Datensatzes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

### <a name="completing-the-new-ios-app-submission-form"></a>Ausfüllen des neuen iOS-App-Registrierungsformulars

Das Formular sollte genau die gleichen Informationen in der„Info.plist“-Datei Ihrer App wiedergeben, wie unten gezeigt:

[![](testflight-images/infoplist.png "Die Datei „Info.plist“ der App")](testflight-images/infoplist.png#lightbox)
[![](testflight-images/newiosapp.png "Das Formular in iTunes Connect")](testflight-images/newiosapp.png#lightbox)

- **Name**: Der beschreibende Name, der beim Einrichten des App Bundles verwendet wird. Diese Angabe muss genau mit dem **Anwendungsnamen**-Eintrag in Ihrer `Info.plist` übereinstimmen.
- **Primary Language** (Primäre Sprache): Die Basissprache, die innerhalb der App verwendet wird. Dies ist normalerweise irgendeine von Ihnen gesprochene Sprache.
- **Bundle ID** (Bundle-ID): Ein Dropdownmenü, das alle App-IDs auflistet, die in Ihrem Entwicklerkonto erstellt wurden.
  - **Bundle ID Suffix** (Bundle-ID-Suffix): Wenn Sie eine Wildcard Bundle-ID ausgewählt haben (d.h. endend mit einem *, wie im obigen Beispiel), wird ein zusätzliches Feld angezeigt, in dem Sie zur Eingabe des Bundle-ID-Suffixes aufgefordert werden. Im Beispiel ist die **Bundle ID** (Bundle-ID) `mobi.chkn.*`, das Suffix ist **Seitenansicht**. Zusammen bilden sie den **Bundle Identifier** (die Bundle-ID) in unserer `Info.plist`.
- **Version**: Die Versionsnummer der App, die hochgeladen wird. Dies wird vom Entwickler ausgewählt.
- **SKU**: Die SKU ist eine eindeutige ID für Ihre App, die nicht für Benutzer angezeigt wird. Sie kann als einer Produkt-ID ähnlich betrachtet werden. Im obigen Beispiel habe ich das Datum und eine Versionsnummer für dieses Datum ausgewählt.

## <a name="upload-your-app"></a>Hochladen Ihrer App

Sobald der iTunes Connect-Eintrag erstellt wurde, können Sie neue Builds hochladen. Denken Sie daran, dass Builds die neue Beta-Berechtigung enthalten müssen.

Erstellen Sie zuerst Ihren [endgültigen verteilbaren Code](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) in der IDE, dann [Senden Sie Ihre App an Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) über den Application Loader oder die Archivierungsfunktion in Xcode.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="create-an-archive"></a>Erstellen eines Archivs

 Um eine Binärdatei in Visual Studio für Mac zu erstellen, müssen Sie die _Archiv_-Funktion verwenden. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Archive for Publishing** (Archivieren für die Veröffentlichung) aus, wie unten gezeigt:

 [![](testflight-images/new-archive.png "„Archive for Publishing“ (Für Veröffentlichung archivieren) auswählen")](testflight-images/new-archive.png#lightbox)

 Weitere Informationen finden Sie im Handbuch [Building the Distributable (Erstellen des Verteilbaren Codes)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen Ihrer App

 Beim Erstellen Ihres Archivs wird automatisch die **Archivansicht** geöffnet, in der alle archivierten Projekte, gegliedert nach Projektmappe, angezeigt werden. Wählen Sie zum Signieren Ihrer App, und um diese für die Verteilung vorzubereiten **Anmelden und Verteilen...** , wie unten gezeigt:

[![](testflight-images/archive-view.png "Beim Erstellen eines Archivs öffnet sich automatisch die Ansicht „Archive“")](testflight-images/archive-view.png#lightbox)

 Dadurch wird der Veröffentlichungs-Assistent geöffnet. Wählen Sie den Verteilungskanal **App Store** aus, um ein Paket zu erstellen, und öffnen Sie den Application Loader: Wählen Sie auf der Seite „Bereitstellungsprofil“ Ihre Signierungsidentität sowie das entsprechende Bereitstellungsprofil aus, oder signieren Sie mit einer anderen Identität erneut. Überprüfen Sie die Details Ihres Pakets, und klicken Sie zum Speichern Ihrer `.ipa`auf **Veröffentlichen**.

[![](testflight-images/group.png "Ihr Bereitstellungsprofil und Ihre Signieridentität auswählen, oder mit einer anderen Identität erneut eine Signierung ausführen")](testflight-images/group.png#lightbox)

 Lesen Sie den Abschnitt [Submitting your App to Apple (Übermitteln der App an Apple)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), um weitere Informationen zu diesen Schritten zu erhalten.

### <a name="submitting-your-build"></a>Übermitteln Ihres Builds
 Der Veröffentlichungsassistent öffnet das Application Loader-Programm, um Ihnen zu ermöglichen Ihren Build in iTunes Connect hochzuladen. Wählen Sie die Option **Deliver Your App** (Übermitteln Ihrer App) aus, und laden Sie Ihre oben erstellte `.ipa`-Datei hoch. Der Application Loader überprüft Ihren Build und lädt ihn in iTunes Connect hoch.

 Lesen Sie den Abschnitt [Submitting your App to Apple (Übermitteln der App an Apple)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), um weitere Informationen zu diesen Schritten zu erhalten.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="building-your-final-distributable"></a>Endgültig verteilbaren Code erstellen
 Da das Xamarin-Plug-In für Visual Studio die Archivierung von Xamarin.iOS-Apps zur Veröffentlichung im App Store nicht unterstützt, gibt es zwei Optionen für die Veröffentlichung einer iOS-Anwendung von Visual Studio. Diese lauten wie folgt:

1. Hochladen einer IPA, die über den „Build Adhoc IPA“-Befehl erstellt wurde.
1. Hochladen eines komprimierten `.app`-Bundles.

 Das Handbuch [Building the Distributable (Erstellen des Verteilbaren Codes)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) enthält Anweisungen für beide Optionen.

### <a name="submitting-your-build"></a>Übermitteln Ihres Builds
 Um Ihre App an Apple zu senden, müssen Sie zu Ihrem Buildhost wechseln und das Anwendungsladeprogramm verwenden, das als Teil von Xcode installiert wird. Weitere Informationen zum Zugreifen auf Anwendungsladeprogramm finden Sie im Apple-Handbuch [Access Application Loader (Zugreifen auf das Anwendungsladeprogramm)](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126).

Wählen Sie nach dem Öffnen die Option **Deliver Your App** (App übermitteln) aus, und laden Sie die oben erstellte ZIP- oder `.ipa`-Datei hoch. Der Application Loader überprüft Ihren Build und lädt ihn in iTunes Connect hoch.

 Lesen Sie den Abschnitt [Submitting your App to Apple (Übermitteln der App an Apple)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md), um weitere Informationen zu diesen Schritten zu erhalten.

-----

Das Handbuch [Publishing to the App Store (Veröffentlichung im App Store)](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md) beschreibt alle oben genannten Schritte ausführlicher. Hier finden Sie einen fundierten Überblick über den Einreichungsprozess für den App Store.

Nachdem Sie zum **Meine Apps**-Abschnitt von iTunes Connect zurückgekehrt sind, sollten Sie feststellen, dass die Anwendung erfolgreich hochgeladen wurde. An diesem Punkt können Sie nun einige Beta-Tests durchführen!

## <a name="manage-beta-testing"></a>Verwalten der Beta-Tests

### <a name="add-metadata"></a>Metadaten hinzufügen

Navigieren Sie zum Einstieg in TestFlight zur **Vorabversion**-Registerkarte in Ihrer App. Drei Registerkarten sollten angezeigt werden, mit einer Liste von Builds, Internen Testern und externen Testern, wie unten gezeigt:

[![](testflight-images/app-uploaded.png "Registerkarten „Builds“, „Interne Tester“ und „Externe Tester“")](testflight-images/app-uploaded.png#lightbox)

Um Metadaten zu Ihrer App hinzuzufügen, klicken Sie auf die Buildnummer und anschließend auf TestFlight:

[![](testflight-images/metadata.png "Metadaten hinzufügen")](testflight-images/metadata.png#lightbox)

Unter **Test Information (Testinformationen)** können Sie wichtige Informationen über Ihre App für Tester bereitstellen, wie z.B.:

- Den Testgegenstand
- Eine Beschreibung Ihrer App.
- Eine Marketing-URL: Sie erhalten Informationen zur App, die Sie hinzufügen möchten.
- Eine Datenschutzrichtlinie-URL: Eine URL, die Informationen zu den Datenschutzrichtlinien des Unternehmens bereitstellt.
- Eine Feedback-E-Mail.

Beachten Sie, dass diese Metadaten **nicht** für interne Tester erforderlich sind, aber **für** externe Tester.

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>Beta-Tests aktivieren

Wenn Sie bereit sind mit dem Testen Ihrer App zu beginnen, schalten Sie den **TestFlight Beta-Test** für Ihre Version ein:

[![](testflight-images/turn-on-testing.png "„TestFlight Beta Testing“ (Betatests für TestFlight) aktivieren")](testflight-images/turn-on-testing.png#lightbox)

Jeder Build ist **60 Tage** vom Zeitpunkt, ab dem Sie den TestFlight Beta Switch eingeschaltet haben, aktiv. Sehen Sie die verbleibenden Tage für jeden Build auf der Seite **Test Information** (Testinformationen):

[![](testflight-images/daysleft.png "Die Seite „Testinformationen“")](testflight-images/daysleft.png#lightbox)

Das Testen kann jedoch jederzeit deaktiviert werden.

### <a name="internal-testers"></a>Interne Tester

Interne Tester sind Mitglieder des Entwicklungsteams, denen eine der folgenden Rollen in iTunes Connect zugewiesen wurde:

- **Administrator**: Ein Administrator ist verantwortlich für das Hinzufügen und Verwalten von neuen Benutzern in iTunes Connect.
- **Legal**: Der Team-Agent ist der einzige Administratorbenutzer, dem die Rolle „Legal“ zugewiesen wird. Diese Rolle erlaubt das Signieren von Rechtsverträgen.
- **Technical** (Technisch): Ein technischer Benutzer kann die meisten Eigenschaften im Zusammenhang mit einer App ändern. Z.B. Informationen zur App bearbeiten, eine Binärdatei hochladen und eine App zur Prüfung übermitteln.

Jeder Build kann für maximal 25 Mitglieder freigegeben werden.

Navigieren Sie zum Hinzufügen von Testern zu **Benutzer und Rollen** auf dem iTunes Connect-Hauptbildschirm:

[![](testflight-images/users-and-roles.png "Benutzer und Rollen auf dem iTunes Connect-Hauptbildschirm")](testflight-images/users-and-roles.png#lightbox)

Vorhandene iTunes Connect-Benutzer werden in der Liste angezeigt. Um diese auszuwählen, klicken Sie auf ihren Namen, aktivieren Sie den Switch **Internen Tester**, und klicken Sie auf **Speichern**:

[![](testflight-images/internal-tester.png "Den Switch „Interner Tester“ aktivieren")](testflight-images/internal-tester.png#lightbox)

Um einen Benutzer hinzuzufügen, der nicht auf der Liste ist, wählen Sie die **+** -Schaltfläche neben den *Benutzern* aus, und geben Sie einen Vornamen, Nachnamen und eine E-Mail-Adresse zum Erstellen eines Kontos an. Die Benutzer müssen ihre E-Mail zur Aktivierung des Kontos bestätigen:

[![](testflight-images/add-new-user.png "Einen Benutzer hinzufügen")](testflight-images/add-new-user.png#lightbox)

Wenn Sie zu **Meine Apps > Vorabversion > Interne Tester** zurückkehren, sehen Sie jetzt die Benutzer, die für TestFlight Interne Beta-Tests hinzugefügt wurden:

[![](testflight-images/select-users.png "Eine Liste der Benutzer, die internen Betatests für TestFlight hinzugefügt wurden")](testflight-images/select-users.png#lightbox)

Sie können diese Tester einladen, indem Sie ihren Namen auswählen und auf die **Einladen**-Schaltfläche klicken. Sie erhalten eine E-Mail mit einer Einladung zum Testen Ihrer App.

Sie können den Status ihrer Einladung in der Statusspalte der Internen Tester Seite finden:

[![](testflight-images/status-added.png "Der Status der Einladung")](testflight-images/status-added.png#lightbox)

### <a name="external-testers"></a>Externe Tester

Bevor Sie Externe Tester zum Beta-Test Ihrer App einladen, muss diese eine Beta-App-Prüfung durchlaufen und muss daher mit den [App Store-Review-Richtlinien](https://developer.apple.com/app-store/review/guidelines/) übereinstimmen.

Um die App zur Prüfung zu übermitteln, klicken Sie auf den Text **Submit For Beta App Review (Zur Beta-App-Prüfung übermitteln)** neben dem Build, wie in der folgenden Abbildung gezeigt:

[![](testflight-images/beta-app-review.png "Zur Prüfung der Beta-App übermitteln")](testflight-images/beta-app-review.png#lightbox)

Damit Ihre App die Prüfung besteht, müssen Sie alle erforderlichen Metadaten auf der TestFlight Beta-Informationsseite eingeben.

Sie können jetzt beginnen, Einladungen vorzubereiten und über die Registerkarte „Externe Tester“ bis zu 2000 externe Tester durch Eingabe ihrer E-Mail-Adresse, ihres Vornamens und Nachnamens hinzuzufügen, wie im folgenden Screenshot dargestellt. Die eingegebene E-Mail-Adresse muss nicht ihre Apple-ID sein. Dies ist nur die E-Mail-Adresse, an welche die Einladung geschickt wird.

[![](testflight-images/add-external.png "Tester einladen")](testflight-images/add-external.png#lightbox)

Wenn Sie eine große Anzahl von externen Testern haben, können Sie den **Importdatei**-Link zum Importieren einer `CSV`-Datei verwenden, mit dem folgenden Format pro Zeile:

``` 
first name, last name, email address
```

Sie können auch externe Tester zu verschiedenen Gruppen hinzufügen, um Ihre Tester besser zu organisieren.

Wenn Sie die Details der externen Tester eingegeben haben, klicken Sie auf **Hinzufügen**, und vergewissern Sie sich, dass die Benutzer der Einladung zugestimmt haben:

[![](testflight-images/confirm-consent.png "Bestätigen, dass Sie über die Zustimmung der Benutzer verfügen, diese einzuladen")](testflight-images/confirm-consent.png#lightbox)

Nur nach erfolgreicher Prüfung der Beta-App werden Sie Einladungen an externe Tester senden können. An diesem Punkt wird der Text unter **Extern** auf der Build-Seite in **Einladungen senden** geändert. Klicken Sie auf diese Option, um Einladungen an alle Tester zu senden, die Sie bereits hinzugefügt haben.

Wenn Ihre App abgelehnt wurde, müssen Sie die im **Resolution Center** angezeigten Probleme beheben und die gesamte aktualisierte Binärdatei erneut zur Prüfung übermitteln.

## <a name="as-a-beta-tester"></a>Als ein Beta-Tester

Nachdem Sie Ihre Tester eingeladen haben, erhalten sie eine E-Mail, ähnlich wie im folgenden Screenshot:

[![](testflight-images/tester-email.png "Ein Beispiel einer E-Mail-Einladung")](testflight-images/tester-email.png#lightbox)

Sobald sie auf die **Öffnen in TestFlight**-Schaltfläche klicken, wird Ihre App in der TestFlight-Anwendung geöffnet, oder, wenn sie noch nicht heruntergeladen wurde, verweist sie direkt an den App Store und ermöglicht es ihnen, sie herunterzuladen.

Wird Ihre App in TestFlight geöffnet, zeigt sie Details zum Test an und fordert die Tester auf, Ihre Anwendung auf ihrem iOS 8.0-Gerät (oder höher) zu installieren:

[![](testflight-images/install-app.png "TestFlight zeigt Details dazu an, worauf getestet wird")](testflight-images/install-app.png#lightbox)

Test-Builds werden auf der Startseite des Geräts durch einen orangefarbenen Punkt vor dem Namen der Anwendung angezeigt.

Tester können Ihr Feedback über die TestFlight-App übermitteln, und Sie werden diese Informationen unter der E-Mail-Adresse, die in den Metadaten bereitgestellt wurde erhalten.

### <a name="beta-testing-complete"></a>Beta-Tests abgeschlossen

Sobald die Beta-Tests abgeschlossen sind, können Sie jetzt Ihre App zur App Store-Review von Apple übermitteln. Dieser Prozess ist in iTunes Connect sehr unkompliziert. Klicken Sie auf die Schaltfläche **Zur Prüfung übermitteln**, wie unten gezeigt:

[![](testflight-images/submit-for-review.png "Auf die Schaltfläche „Submit for Review“ (Zur Überprüfung übermitteln) klicken")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie der TestFlight Beta-Test von Apple über iTunes Connect verwendet werden kann. Dieser Artikel behandelte, wie Sie einen neuen Build in iTunes Connect hochladen, und wie Sie interne und externe Beta-Tester einladen unsere App zu verwenden.

## <a name="related-links"></a>Verwandte Links

- [Erstellen eines iTunes Connect-Datensatzes](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Bereitstellen einer App für die Verteilung über den App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Verwenden von Apple TestFlight Beta](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
