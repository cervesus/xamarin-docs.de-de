---
title: Die Veröffentlichung auf dem Apple TV-App Store
description: Dieses Dokument beschreibt, wie eine app auf dem Apple TV-App-Store veröffentlichen. Es wird erläutert, wie konfigurieren, bereitstellen, erstellen und senden eine Anwendung für tvos. außerdem wurden mit Xamarin.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac905caaf0bdefe7f0c5502be0bd63102ca5a813
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789303"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Die Veröffentlichung auf dem Apple TV-App Store

Apple erfordert, dass apps über veröffentlicht werden, verteilen Sie nacheinander alle Geräte von Apple TV-Anwendungen die *Apple TV-App Store*, machen im App Store die Warenkorb praktischen Ort für tvos. außerdem wurden apps. Entwickler von vielen Arten von apps können in Großschreibung den massive Erfolg dieser einzelnen Punkt der Verteilung. Apple TV-App-Store ist eine schlüsselfertige Lösung, app-Entwickler-Verteilung und Zahlungssysteme anbietet.

Der Vorgang der Übermittlung von einer Anwendung auf den Apple TV-App-Store umfasst:

1. Erstellen einer *App-ID* und Auswählen von *Berechtigungen*
2. Erstellen eines *Verteilungsbereitstellungsprofils*
3. Mithilfe dieses Profils beim Erstellen Ihrer app.
4. Übermitteln der app über *iTunes Connect*.


In diesem Artikel wird beschrieben, die Schritte, die zum Bereitstellen, erstellen und senden eine app für Apple TV-App Store-Verteilung erforderlich sind.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Vor dem Übermitteln der Anwendung

Nachdem Sie eine app für die Veröffentlichung auf der Apple TV-App-Store übermitteln, durchläuft der Review-Prozess von Apple, um sicherzustellen, dass sie Apple Richtlinien für die Qualität und Inhalt erfüllt. Erfüllt Ihre Anwendung diese Richtlinien nicht, wird sie von Apple abgelehnt. In diesem Fall müssen Sie die von Apple angeführten Nichtübereinstimmungen beheben und die Anwendung erneut übermitteln.
Daher sollten Sie sich mit diesen Richtlinien vertraut machen und Ihre Anwendung bestmöglich anpassen, um Ihre Chancen bei der Überprüfung durch Apple zu erhöhen. Apple Richtlinien finden Sie unter [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html) und [Vorbereiten Ihrer App Submission für die neue Apple TV](https://developer.apple.com/tvos/submit/).

Beachten Sie bei der Übermittlung einer App Folgendes:

1. Stellen Sie sicher, dass die app-Beschreibung der Funktionen der in der app entspricht.
2. Test, der die app abstürzt nicht bestimmungsgemäßer Verwendung. Dies schließt die Verwendung auf jedem Apple TV-Gerät, die Sie unterstützen.


Apple verwaltet auch eine Liste mit Tipps für Apple TV-App Store-Übermittlung. Diese Tipps finden Sie unter [Distributing on the App Store](https://developer.apple.com/appstore/resources/submission/tips.html) (Verteilen im App Store).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurieren der Anwendung in iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) ist eine Suite von webbasierte Tools, u. a. Ihre apps tvos. außerdem wurden auf dem Apple TV-App Store verwalten. Ihre app Xamarin.tvOS ordnungsgemäß eingerichtet werden müssen und in iTunes Connect konfiguriert werden, bevor er an Apple kann, um zur Überprüfung gesendet werden und schließlich freigegeben werden, für den Verkauf oder als eine kostenlose app im App Store Apple TV.

Führen Sie folgende Schritte aus:

1. Gehen Sie in iTunes Connect zum Abschnitt **Vereinbarungen, Steuern und Bankgeschäfte**. Stellen Sie sicher, dass die Vereinbarungen richtig eingerichtet und auf dem neuesten Stand sind, um eine iOS-Anwendung kostenlos oder zum Verkauf freizugeben.
2. Erstellen Sie ein neues **iTunes Connect Datensatz** für die Anwendung, und geben Sie ihre **Anzeigenamen** (wie in der Apple TV-App-Store dargestellt).
3. Wählen Sie den **Verkaufspreis** aus, oder geben Sie an, dass die Anwendung kostenlos ist.
4. Geben Sie eine **Symbol "App-Store"** ("große Symbole") und Screenshots der Anwendung in Aktion, auf den Apple TV-Geräten unterstützt. Finden Sie in unserer [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Weitere-Handbuch.
5. Geben Sie einen Clear kompakten **Beschreibung** einschließlich der zugehörigen Funktionen der app und für den Endbenutzer profitieren.
6. Geben Sie **Kategorien**, **Unterkategorien**, und **Schlüsselwörter** den Benutzer, die Ihre app im App Store Apple TV suchen helfen.
7. Geben Sie wie von Apple erfordert **Kontaktinformationen** und **Support-URLs** zu Ihrer Website an.
8. Legen Sie der Anwendungsverzeichnis **Bewertung**, der von Jugendschutz im Apple TV-App-Store verwendet wird.
9. Konfigurieren Sie optionale App Store-Technologien, wie z.B. **Game Center** und **In-App-Käufe**.

Weitere Informationen finden Sie unter unsere [Ihrer tvos. außerdem wurden App im iTunes Connect konfigurieren](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) Dokumentation.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Vorbereiten für die Verteilung im App Store

Um eine app auf den Apple TV-App-Store veröffentlichen zu können, müssen Sie zuerst zum Erstellen für die Verteilung, die zahlreiche Schritte umfasst. In den folgenden Abschnitten behandelt notwendigen Vorbereitungen, um eine Xamarin.tvOS-app, damit er erstellt werden kann, und senden Sie es auf den Apple TV-App Store Review und Freigabe vorbereiten.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple bietet eine Auswahl von speziellen Anwendungsdienste, so genannte Ansprüche, die für Ihre app tvos. außerdem wurden aktiviert werden können, wenn Sie eine eindeutige ID erstellen. Ob Sie benutzerdefinierte Berechtigungen oder nicht verwenden, müssen Sie eine eindeutige ID für Ihre app Xamarin.tvOS erstellen, bevor sie auf dem Apple TV-App Store veröffentlicht werden kann.

Führen Sie die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus, um eine App-ID zu erstellen und optional Berechtigungen auszuwählen:

1. Wählen Sie **Bereitstellung** > **Entwicklung**.
2. Klicken Sie auf die **+**-Schaltfläche, und stellen Sie einen **Namen** und eine **Bündel-ID** für die neue Anwendung bereit.
3. Führen Sie einen Bildlauf zum unteren Rand des Bildschirms, und wählen Sie eine **Anwendungsdienste** , die von Ihrer Anwendung Xamarin.tvOS erforderlich.
4. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Neben auswählen und die erforderlichen Dienste für die Anwendung konfigurieren, wenn Sie Ihre App-ID zu definieren, müssen Sie auch so konfigurieren Sie die App-ID und die Berechtigungen bearbeiten sowohl in Ihrem Projekt Xamarin.tvOS der `Info.plist` und `Entitlements.plist` Dateien.

Gehen Sie im Visual Studio für Mac:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. In der **tvos. außerdem wurden Anwendung Ziel** Abschnitt, geben Sie einen Namen für die Anwendung aus, und geben Sie die **Paket-ID** , die Sie erstellt, wenn Sie die App ID. definiert
3. Speichern Sie die Änderungen an der Datei `Info.plist`.
4. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
5. Wählen Sie aus, und konfigurieren Sie die Berechtigungen, die für Sie Xamarin.tvOS app erforderlich ist, damit sie das Setup übereinstimmen, die, dem Sie oben ausgeführt, wenn Sie die App ID. definiert
6. Speichern Sie die Änderungen an der Datei `Entitlements.plist`.

Eine ausführliche Anleitung finden Sie in der Dokumentation unter [Provisioning for Application Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) (Bereitstellung für Anwendungsdienste). Während dieses Dokuments für iOS geschrieben wurde, werden die gleichen Schritte zum Bereitstellen einer app Xamarin.tvOS verwendet.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Festlegen der Apps-Symbole, starten Sie und Oberes Regal Abbild

Damit eine app tvos. außerdem wurden von Apple, die für die Aufnahme in die Apple TV-App-Store akzeptiert werden erfordert sie die richtigen Symbole, starten und Top Regal-Images für alle Apple TV-Geräte, die sie ausgeführt werden sollen. Müssen fügen Sie die erforderlichen Bildanlagen, die in kompiliert werden, wird eine `Assets.car` Datei und in Ihre Xamarin.tvOS-app-Bündel eingeschlossen werden, bevor sie in iTunes Connect hochgeladen werden.

Ausführliche Anleitungen hierzu finden Sie unsere [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Erstellen und Installieren eines Verteilungsprofils

verwendet tvos. außerdem wurden *bereitstellungsprofile* zu steuern, wie ein bestimmte Anwendung Build bereitgestellt werden kann. Hierbei handelt es sich um Dateien mit Informationen zu dem Zertifikat, das zum Signieren einer App verwendet wurde, der *Anwendungs-ID* sowie dem Ort, an dem die App installiert werden kann. Für die Entwicklung und Ad-hoc-Verteilung enthält das Bereitstellungsprofil auch eine Liste der zulässigen Geräte, auf denen die App bereitgestellt werden kann. Für Apple TV-App Store-Verteilung nur Zertifikat und app-ID-Informationen werden jedoch berücksichtigt, da der einzige Mechanismus für die Verteilung öffentlicher über den Apple TV-App Store ist.

Führen Sie für die Bereitstellung die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus:

1.  Wählen Sie **Bereitstellung** > **Verteilung** aus.
2.  Klicken Sie auf die **+** Schaltfläche und wählen Sie den Typ des Profils Verteilung, die Sie erstellen möchten **Apple TV-App Store**.
3.  Wählen Sie aus der Dropdownliste die **App-ID** aus, für die Sie ein Verteilungsprofil erstellen möchten.
4.  Wählen Sie das Zertifikat zum Signieren der Anwendung erforderlich sind.
5.  Geben Sie einen **Namen** für das neue **Verteilungsprofil** ein, und generieren Sie das Profil.
6.  Aktualisiert die Liste der verfügbaren Profile in Xcode.
7.  Wählen Sie die Verteilung Bereitstellungsprofil in Visual Studio für die **App Store** _Buildkonfiguration_.

Eine ausführliche Anleitung finden Sie unter [Creating a Distribution Profile](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) (Erstellen eines Verteilungsprofils) und [Selecting a Distribution Profile in a Xamarin.iOS Project](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile) (Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt). Ebenfalls beide Dokumente sind spezifisch für iOS, jedoch das gleiche Verfahren für tvos. außerdem wurden apps verwendet wird.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Festlegen der Buildkonfiguration für die Anwendung

Standardmäßig wird bei der Erstellung einer neuen Xamarin.tvOS app _Buildkonfigurationen_ werden automatisch erstellt, für beide **Debuggen** und **Version** Bereitstellung. Vor dem Ausführen der letzten Build der Anwendung, die Sie übermittelt werden, werden an Apple, stehen einige Änderungen, die Sie an der Basis vornehmen müssen **Version** Konfiguration.

Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf die **Projektname** in der **Projektmappen-Explorer** und Auswahl **Optionen** für die Bearbeitung zu öffnen.
2. Wenn das Ziel einer bestimmten Version von tvos. außerdem wurden, wählen Sie diese unter **tvos. außerdem wurden Build** > **iOS SDK-Version**. Für die Preview-Version von tvos. außerdem wurden unterstützt werden soll, lassen Sie diesen Wert auf **Standard**.
3. Verknüpfen verringert, dass die Gesamtgröße des Ihrer app verteilbarer Striping nicht verwendeten Methoden, Eigenschaften, Klassen, usw. und in den meisten Fällen sollte der Wert von überlassen werden **Link Framework SDK nur**. In einigen Situationen, z. B. wenn mit einigen spezifischen 3rd Party Bibliotheken Sie u. u. gezwungen, legen Sie diesen Wert auf **nicht verknüpft** zu verhindern, dass die erforderlichen Element entfernt wird.
4. Um eine app Xamarin.tvOS liefern zu können, müssen Sie die LLVM-Optimierungscompiler verwenden. Sicherstellen, dass die **verwenden die LLVM Optimierungscompiler** Kontrollkästchen aktiviert ist, unter der **Version** Konfiguration.
5. Apple erforderlich auch tvos. außerdem wurden apps Bitcode verwenden. Erneut unter der **Release** Konfiguration hinzufügen `--bitcode=asmonly` auf die **zusätzliche Mtouch Argumente** Feld.
6. Die **optimieren PNG-Bilddateien für iOS** Kontrollkästchen sollte aktiviert sein, da dies zu weiteren Verringerung der lieferleistung Größe der app erleichtert wird.
7. Debuggen sollte *nicht* aktiviert werden, wie sie den Build unnötigerweise vergrößern, wird.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Erstellen und Übermitteln der verteilbaren Anwendung

Mit der Xamarin.tvOS-app ordnungsgemäß konfiguriert können Sie jetzt bereit für die endgültige Verteilung-Build, den Sie übermittelt werden, werden an Apple für die Überprüfung und Version.

#### <a name="build-your-archive"></a>Erstellen des Archivs

1. Wählen Sie in Visual Studio für Mac die Konfiguration **Release | Gerät** aus:

    ![](app-store-publishing-images/buildxs01new.png "Wählen Sie die Release-Konfiguration")
2. Wählen Sie aus dem **Build**-Menü die Option **Zur Veröffentlichung aktivieren**:

    [![](app-store-publishing-images/buildxs02new.png "„Archive for Publishing“ (Für Veröffentlichung archivieren) auswählen")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt:

    [![](app-store-publishing-images/buildxs03new.png "Das Archive-Ansicht")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen Ihrer App

Beim Erstellen Ihrer Anwendung für das Archiv wird automatisch die *Archiv-Ansicht* geöffnet. Darin werden alle archivierten Projekte nach Projektmappe gruppiert angezeigt. Standardmäßig wird in dieser Ansicht nur die aktuelle geöffnete Projektmappe angezeigt. Klicken Sie auf **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen.

Es wird empfohlen, Archive beizubehalten, die bei den Kunden bereitgestellt wurden (App Store- oder Unternehmensbereitstellungen). Dadurch können alle generierten Debuginformationen zu einem späteren Zeitpunkt symbolisiert werden.

Gehen Sie folgendermaßen vor, um Ihre App für die Verteilung zu signieren und vorzubereiten:

1. Wählen Sie die **signieren und Verteilen von...** , unten dargestellt:

    [![](app-store-publishing-images/buildxs04new.png ", Wählen Sie TheSign und verteilen...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Dadurch wird der Veröffentlichungs-Assistent geöffnet. Wählen Sie den Verteilungskanal **App Store** aus, um ein Paket zu erstellen, und öffnen Sie den Application Loader:

    [![](app-store-publishing-images/distribute01.png "Wählen Sie den App Store-Verteilung-Kanal")](app-store-publishing-images/distribute01.png#lightbox)
3. Wählen Sie die Signaturidentität sowie das entsprechende Bereitstellungsprofil oder mit einer anderen Identität neu melden Sie an, auf dem Bildschirm Bereitstellungsprofil:

    [![](app-store-publishing-images/distribute02.png "Wählen Sie die Signaturidentität und das entsprechende Bereitstellungsprofil")](app-store-publishing-images/distribute02.png#lightbox)
4. Überprüfen Sie die Details Ihres Pakets, und klicken Sie zum Speichern des `.ipa`-Pakets auf **Veröffentlichen**:

    [![](app-store-publishing-images/distribute03.png "Überprüfen Sie die Details des Pakets")](app-store-publishing-images/distribute03.png#lightbox)
5. Sobald die `.ipa` gespeichert wurde, kann Ihre App über den Application Loader in iTunes Connect hochgeladen werden:

    [![](app-store-publishing-images/distribute04.png "Klicken Sie in iTunes Connect über das Anwendungslademodul hochgeladen")](app-store-publishing-images/distribute04.png#lightbox)

Nachdem Ihr Verteilungsbuild erstellt und archiviert wurde, können Sie nun Ihre Anwendung an iTunes Connect übermitteln.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Übermitteln der App an Apple

Nach Abschluss des Verteilungsbuilds können Sie Ihre iOS-Anwendung nun zur Überprüfung und Freigabe im App Store an Apple übermitteln.


Wird der Archiv-Workflow in Visual Studio für Mac Application Loader automatisch geöffnet, wenn Sie gespeichert haben die `.ipa`:

2. Wählen Sie *Ihre App übermitteln* aus, und klicken Sie auf die Schaltfläche *Auswählen*:

    [![](app-store-publishing-images/publishvs01.png "Wählen Sie „Ihre App übermitteln“ aus")](app-store-publishing-images/publishvs01.png#lightbox)

3. Wählen Sie die zuvor erstellte ZIP- oder IPA-Datei aus, und klicken Sie auf die Schaltfläche **OK**.
4. Die Datei wird durch den Application Loader überprüft:

    [![](app-store-publishing-images/publishvs02.png "Das Anwendungslademodul-Überprüfungsbildschirm")](app-store-publishing-images/publishvs02.png#lightbox)
5. Klicken Sie auf die Schaltfläche *Weiter*. Die Anwendung wird nun für den App Store überprüft:

    [![](app-store-publishing-images/publishvs03.png "Die Anwendung wird gegen den App Store")](app-store-publishing-images/publishvs03.png#lightbox)
6. Klicken Sie auf die Schaltfläche **Senden**, um die Anwendung zur Überprüfung an Apple zu senden.
7. Sie werden vom Application Loader informiert, sobald die Datei erfolgreich hochgeladen wurde.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>Status in iTunes Connect

Wenn Sie melden Sie sich wieder bei iTunes Connect, und wählen Sie Ihre app aus der Liste der verfügbaren apps, der Status in iTunes Connect wird nun angezeigt, dass darauf **warten auf Überprüfung** (möglicherweise vorübergehend lesen **hochladen empfangen** während der Verarbeitung):

[![](app-store-publishing-images/image21.png "Der Status im iTunes Verbinden mit Warten auf Überprüfung")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie beim Übermitteln der app Xamarin.tvOS auf den Apple TV-App-Store Probleme auftreten, finden Sie in unserer [Problembehandlung](~/ios/tvos/troubleshooting.md) Handbuch. Es enthält einige bekannte Probleme, die auftreten können und wie sie in der Xamarin.tvOS gelöst.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel dargestellt eine schrittweise Anleitung zum Konfigurieren, erstellen und senden eine app für Apple TV-App Store-Veröffentlichung. Zuerst wird die Vorgehensweise zum Erstellen und Installieren eines Verteilungsbereitstellungsprofils erklärt. Anschließend durchlaufen sie erläutert, wie Visual Studio für Mac verwenden, um eine Verteilung Build zu erstellen. Schließlich wurde es wie iTunes Connect und das Xcode-Archiv-Tool verwenden, um eine Anwendung auf den Apple TV-App-Store zu übermitteln.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md)
- [Vorbereiten Sie Ihrer App Übermittlung für die neue Apple TV](https://developer.apple.com/tvos/submit/)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Common App Rejections (Häufige Ablehnungsgründe für Apps)](https://developer.apple.com/app-store/review/rejections/)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
