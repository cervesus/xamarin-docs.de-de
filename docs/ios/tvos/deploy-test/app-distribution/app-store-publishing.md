---
title: Veröffentlichen im Apple TV App Store
description: In diesem Dokument wird beschrieben, wie Sie eine app in der Apple TV App Store veröffentlichen. Es wird erläutert, wie konfigurieren, bereitstellen, erstellen und Übermitteln einer TvOS-Anwendung mit Xamarin erstellt wurde.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b941bcc8588e7fb0377430cca2829ad72ecbc8c6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61416676"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Veröffentlichen im Apple TV App Store

Verteilen von Anwendungen für alle Apple TV-Geräte in der Reihenfolge, Apple erfordert, dass apps über veröffentlicht werden die *Apple TV App Store*, wodurch die App-Store die eine Anlaufstelle für TvOS-apps. Entwickler vieler verschiedener Arten von apps können auf diese einzelne Fehlerquelle Distribution vom großen Erfolg großgeschrieben. Das Apple TV App Store ist eine sofort einsetzbare Lösung bietet app-Entwicklern sowohl Verteilung als auch Zahlungssysteme.

Der Prozess der Übermittlungsprozess einer Anwendung an das Apple TV App Store umfasst:

1. Erstellen einer *App-ID* und Auswählen von *Berechtigungen*
2. Erstellen eines *Verteilungsbereitstellungsprofils*
3. Verwenden dieses Profil, um Ihre app zu erstellen.
4. Übermitteln der app über *iTunes Connect*.


In diesem Artikel wird beschrieben, alle die erforderlichen Schritte zum Bereitstellen, erstellen und senden Sie eine app für Apple TV App Store-Verteilung.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Vor dem Übermitteln der Anwendung

Nachdem Sie eine app zur Veröffentlichung an das Apple TV App Store eingereicht haben, durchläuft sie einen Überprüfungsprozess von Apple, um sicherzustellen, dass sie die Apple Richtlinien für die Qualität und der Inhalt entspricht. Erfüllt Ihre Anwendung diese Richtlinien nicht, wird sie von Apple abgelehnt. In diesem Fall müssen Sie die von Apple angeführten Nichtübereinstimmungen beheben und die Anwendung erneut übermitteln.
Daher sollten Sie sich mit diesen Richtlinien vertraut machen und Ihre Anwendung bestmöglich anpassen, um Ihre Chancen bei der Überprüfung durch Apple zu erhöhen. Apple Richtlinien finden Sie unter [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html) und [bereiten Sie vor der Übermittlung der App für die neue Apple TV](https://developer.apple.com/tvos/submit/).

Beachten Sie bei der Übermittlung einer App Folgendes:

1. Stellen Sie sicher, dass die app Beschreibung in der app enthaltenen Funktionen übereinstimmt.
2. Stellen Sie sicher, dass die App bei normaler Nutzung nicht abstürzt. Dies schließt die Nutzung auf jedem Apple TV-Gerät, die Sie unterstützen.


Apple verwaltet auch eine Liste mit Tipps für die Übermittlung von Apple TV App Store. Diese Tipps finden Sie unter [Distributing on the App Store](https://developer.apple.com/appstore/resources/submission/tips.html) (Verteilen im App Store).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurieren der Anwendung in iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) ist eine Suite von webbasierten Tools, u. a. Ihre TvOS-apps auf der Apple TV App Store verwalten. Ihre app Xamarin.tvOS ordnungsgemäß eingerichtet werden müssen und in iTunes Connect konfiguriert werden, bevor sie zur Überprüfung an Apple gesendet werden kann und schließlich für den Verkauf oder als kostenlose app in der Apple TV App Store veröffentlicht.

Führen Sie folgende Schritte aus:

1. Gehen Sie in iTunes Connect zum Abschnitt **Vereinbarungen, Steuern und Bankgeschäfte**. Stellen Sie sicher, dass die Vereinbarungen richtig eingerichtet und auf dem neuesten Stand sind, um eine iOS-Anwendung kostenlos oder zum Verkauf freizugeben.
2. Erstellen Sie ein neues **iTunes Connect-Datensatz** für die Anwendung, und geben Sie die **Anzeigenamen** (wie in der Apple TV App Store angezeigt).
3. Wählen Sie den **Verkaufspreis** aus, oder geben Sie an, dass die Anwendung kostenlos ist.
4. Geben Sie eine **App Store-Symbol** (großes Symbol) und Screenshots von Ihrer Anwendung in Aktion, die auf den Apple TV-Geräten unterstützt. Finden Sie in unserer [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Handbuch für die weitere Details.
5. Liefern Sie eine kurze **Beschreibung** der app, einschließlich aller Funktionen und Vorteile für Endbenutzer.
6. Geben Sie **Kategorien**, **Unterkategorien**, und **Schlüsselwörter** , die Benutzer Ihre app in der Apple TV App Store finden können.
7. Geben Sie wie von Apple erfordert **Kontaktinformationen** und **Support-URLs** zu Ihrer Website an.
8. Legen Sie Ihre Anwendungsverzeichnis **Bewertung**, die von jugendschutzeinstellungen auf der Apple TV App Store verwendet wird.
9. Konfigurieren Sie optionale App Store-Technologien, wie z.B. **Game Center** und **In-App-Käufe**.

Weitere Informationen finden Sie unserem [Konfigurieren Ihrer TvOS-App in iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) Dokumentation.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Vorbereiten für die Verteilung im App Store

Um eine app in der Apple TV App Store veröffentlichen zu können, müssen Sie zuerst für die Verteilung erstellen. Dies zahlreiche Schritte erfordert. In den folgenden Abschnitten behandelt alles, was erforderlich, um ein Xamarin.tvOS-app für die Veröffentlichung vorbereiten, sodass diese erstellt werden kann und an das Apple TV App Store zur Überprüfung und Freigabe zu übermitteln.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple bietet eine Auswahl an speziellen Anwendungsdiensten, auch Berechtigungen genannt, die für TvOS-app aktiviert werden können, wenn Sie eine eindeutige ID zu erstellen. Ob Sie benutzerdefinierte Berechtigungen oder nicht verwenden, müssen Sie eine eindeutige ID für Ihre Xamarin.tvOS-app erstellen, bevor er auf die Apple TV App Store veröffentlicht werden kann.

Führen Sie die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus, um eine App-ID zu erstellen und optional Berechtigungen auszuwählen:

1. Wählen Sie **Bereitstellung** > **Entwicklung**.
2. Klicken Sie auf die **+**-Schaltfläche, und stellen Sie einen **Namen** und eine **Bündel-ID** für die neue Anwendung bereit.
3. Scrollen Sie zum unteren Rand des Bildschirms, und wählen Sie einen **Anwendungsdienste** , die Ihre app Xamarin.tvOS erforderlich sein sollen.
4. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Zusätzlich zur Auswahl, und der erforderlichen Anwendungsdienste konfigurieren, wenn Sie Ihre App-ID definieren, müssen Sie auch die App-ID und die Berechtigungen in Ihrem Xamarin.tvOS-Projekt zu konfigurieren, bearbeiten Sie hierfür die `Info.plist` und `Entitlements.plist` Dateien.

Gehen Sie im Visual Studio für Mac:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. In der **TvOS Application Target** Abschnitt, geben Sie einen Namen für Ihre Anwendung aus, und geben Sie die **Bündel-ID** , dass Sie erstellt haben, wenn Sie die App ID. definiert
3. Speichern Sie die Änderungen an der Datei `Info.plist`.
4. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
5. Wählen Sie aus, und konfigurieren Sie die Berechtigungen für Ihre Xamarin.tvOS-app, damit sie mit den Einstellungen übereinstimmen, die, dem Sie oben ausgeführt, wenn Sie die App ID. definiert
6. Speichern Sie die Änderungen an der Datei `Entitlements.plist`.

Eine ausführliche Anleitung finden Sie in der Dokumentation unter [Provisioning for Application Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) (Bereitstellung für Anwendungsdienste). Während dieses Dokuments für iOS geschrieben wurde, werden die gleichen Schritte verwendet, um eine Xamarin.tvOS-app bereitzustellen.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Festlegen der App-Symbole, Startbild und Top Shelf-Bild

Für eine TvOS-app für die Aufnahme in den Apple TV App Store von Apple akzeptiert wird erfordert sie korrekte Symbole, Start- und Top Shelf-Images für alle Apple TV-Geräte, die es ausgeführt wird. Müssen fügen Sie die erforderlichen Image-Ressourcen, die in kompiliert werden, wird eine `Assets.car` Datei, und in Ihre Xamarin.tvOS-app Bundle eingeschlossen werden, bevor sie in iTunes Connect hochgeladen werden.

Ausführliche Anweisungen finden Sie in unserem [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Erstellen und Installieren eines Verteilungsprofils

TvOS verwendet *bereitstellungsprofile* steuern, wie ein bestimmtes anwendungsbuild bereitgestellt werden kann. Hierbei handelt es sich um Dateien mit Informationen zu dem Zertifikat, das zum Signieren einer App verwendet wurde, der *Anwendungs-ID* sowie dem Ort, an dem die App installiert werden kann. Für die Entwicklung und Ad-hoc-Verteilung enthält das Bereitstellungsprofil auch eine Liste der zulässigen Geräte, auf denen die App bereitgestellt werden kann. Für Apple TV App Store-Verteilung, nur Zertifikats- und app-ID-Informationen sind jedoch eingeschlossen werden, da der einzige Mechanismus zur öffentlichen Verteilung über das Apple TV App Store ist.

Führen Sie für die Bereitstellung die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus:

1.  Wählen Sie **Bereitstellung** > **Verteilung** aus.
2.  Klicken Sie auf die **+** Schaltfläche, und wählen Sie den Typ des Verteilungsprofils, das Sie erstellen möchten **Apple TV App Store**.
3.  Wählen Sie aus der Dropdownliste die **App-ID** aus, für die Sie ein Verteilungsprofil erstellen möchten.
4.  Wählen Sie Zertifikat zum Signieren der Anwendung erforderlich sind.
5.  Geben Sie einen **Namen** für das neue **Verteilungsprofil** ein, und generieren Sie das Profil.
6.  Aktualisieren Sie die Liste der verfügbaren Profile in Xcode.
7.  Wählen Sie die Verteilungsbereitstellungsprofils in Visual Studio für die **App Store** _Buildkonfiguration_.

Eine ausführliche Anleitung finden Sie unter [Creating a Distribution Profile](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) (Erstellen eines Verteilungsprofils) und [Selecting a Distribution Profile in a Xamarin.iOS Project](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile) (Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt). In diesem Fall beide Dokumente sind spezifisch für iOS, jedoch das gleiche Verfahren für TvOS-apps verwendet wird.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Festlegen der Buildkonfiguration für die Anwendung

Standardmäßig wird bei der Erstellung einer neuen Xamarin.tvOS-app, _Buildkonfigurationen_ werden automatisch erstellt, für beide **Debuggen** und **Version** Bereitstellung. Vor den endgültigen Build von Ihrer app, die übermittelt werden soll, Apple, es gibt einige Änderungen, die Sie an der Basis vornehmen müssen **Version** Konfiguration.

Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf die **Projektname** in die **Projektmappen-Explorer** und Auswahl **Optionen** für die Bearbeitung zu öffnen.
2. Wenn Sie eine bestimmte Version von TvOS nutzen möchten, wählen sie unter **TvOS-Build** > **iOS SDK-Version**. Lassen Sie diesen Wert auf, für die Vorschauversion von TvOS-Unterstützung, **Standard**.
3. Verknüpfen von verringert, dass die Gesamtgröße der Ihrer app verteilbarer entfernt nicht verwendete Methoden, Eigenschaften, Klassen, usw., und es wird in den meisten Fällen sollte auf den Standardwert des bleiben **nur Link Framework SDK**. In einigen Situationen, z. B. bei Verwendung von bestimmten 3. Bibliotheken von Drittanbietern, Sie u. u. gezwungen, legen Sie diesen Wert auf **nicht verknüpfen** um zu verhindern, dass die erforderliche Element entfernt wird.
4. Um ein Xamarin.tvOS-app senden zu können, müssen Sie, dass der LLVM-Optimierungscompiler eingesetzt werden. Sicherstellen, dass die **LLVM-Optimierungscompiler verwenden** Kontrollkästchen aktiviert ist, unter der **Version** Konfiguration.
5. Apple muss dann auch TvOS-apps Bitcode verwenden. Erneut unter die **Version** -Konfiguration hinzufügen `--bitcode=asmonly` auf die **Weitere Mtouch-Argumente** Feld.
6. Die **Optimierung von PNG-Bilddateien für iOS** Kontrollkästchen sollte überprüft werden, da dadurch zu noch weiter zu verringern die Gesamtgröße Ihrer app können.
7. Das Debuggen sollte *nicht* aktiviert werden, weil es den Build unnötig zu vergrößern wird.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Erstellen und Übermitteln der verteilbaren Anwendung

Mit der Xamarin.tvOS-app ordnungsgemäß konfiguriert wird sind Sie nun bereit für das endgültige verteilungsbuild erstellen, das Sie senden an Apple zur Überprüfung und Freigabe.

#### <a name="build-your-archive"></a>Erstellen des Archivs

1. Wählen Sie in Visual Studio für Mac die Konfiguration **Release | Gerät** aus:

    ![](app-store-publishing-images/buildxs01new.png "Wählen Sie die Release-Konfiguration")
2. Wählen Sie aus dem **Build**-Menü die Option **Zur Veröffentlichung aktivieren**:

    [![](app-store-publishing-images/buildxs02new.png "„Archive for Publishing“ (Für Veröffentlichung archivieren) auswählen")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt:

    [![](app-store-publishing-images/buildxs03new.png "Die Ansicht \"Archive\"")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen Ihrer App

Beim Erstellen Ihrer Anwendung für das Archiv wird automatisch die *Archiv-Ansicht* geöffnet. Darin werden alle archivierten Projekte nach Projektmappe gruppiert angezeigt. Standardmäßig wird in dieser Ansicht nur die aktuelle geöffnete Projektmappe angezeigt. Klicken Sie auf **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen.

Es wird empfohlen, Archive beizubehalten, die bei den Kunden bereitgestellt wurden (App Store- oder Unternehmensbereitstellungen). Dadurch können alle generierten Debuginformationen zu einem späteren Zeitpunkt symbolisiert werden.

Gehen Sie folgendermaßen vor, um Ihre App für die Verteilung zu signieren und vorzubereiten:

1. Wählen Sie die **signieren und verteilen...** wie unten dargestellt:

    [![](app-store-publishing-images/buildxs04new.png ", Wählen Sie TheSign und verteilen...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Dadurch wird der Veröffentlichungs-Assistent geöffnet. Wählen Sie den Verteilungskanal **App Store** aus, um ein Paket zu erstellen, und öffnen Sie den Application Loader:

    [![](app-store-publishing-images/distribute01.png "Wählen Sie den App Store-Verteilungskanal")](app-store-publishing-images/distribute01.png#lightbox)
3. Klicken Sie auf der Seite "Bereitstellungsprofil" Wählen Sie Ihre signierungsidentität sowie das entsprechende Bereitstellungsprofil, oder mit einer anderen Identität erneut signieren:

    [![](app-store-publishing-images/distribute02.png "Wählen Sie die signierungsidentität und das entsprechende Bereitstellungsprofil")](app-store-publishing-images/distribute02.png#lightbox)
4. Überprüfen Sie die Details Ihres Pakets, und klicken Sie zum Speichern des `.ipa`-Pakets auf **Veröffentlichen**:

    [![](app-store-publishing-images/distribute03.png "Überprüfen Sie die Details des Pakets")](app-store-publishing-images/distribute03.png#lightbox)
5. Sobald die `.ipa` gespeichert wurde, kann Ihre App über den Application Loader in iTunes Connect hochgeladen werden:

    [![](app-store-publishing-images/distribute04.png "In iTunes Connect über den Application Loader hochgeladen")](app-store-publishing-images/distribute04.png#lightbox)

Nachdem Ihr Verteilungsbuild erstellt und archiviert wurde, können Sie nun Ihre Anwendung an iTunes Connect übermitteln.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Übermitteln der App an Apple

Nach Abschluss des Verteilungsbuilds können Sie Ihre iOS-Anwendung nun zur Überprüfung und Freigabe im App Store an Apple übermitteln.


Wird archivworkflow in Visual Studio für Mac Application Loader automatisch geöffnet, nachdem Sie gespeichert haben die `.ipa`:

2. Wählen Sie *Ihre App übermitteln* aus, und klicken Sie auf die Schaltfläche *Auswählen*:

    [![](app-store-publishing-images/publishvs01.png "Wählen Sie „Ihre App übermitteln“ aus")](app-store-publishing-images/publishvs01.png#lightbox)

3. Wählen Sie die zuvor erstellte ZIP- oder IPA-Datei aus, und klicken Sie auf die Schaltfläche **OK**.
4. Die Datei wird durch den Application Loader überprüft:

    [![](app-store-publishing-images/publishvs02.png "Der Application Loader-Überprüfungsbildschirm")](app-store-publishing-images/publishvs02.png#lightbox)
5. Klicken Sie auf die Schaltfläche *Weiter*. Die Anwendung wird nun für den App Store überprüft:

    [![](app-store-publishing-images/publishvs03.png "Die Anwendung überprüft wird, für den App Store")](app-store-publishing-images/publishvs03.png#lightbox)
6. Klicken Sie auf die Schaltfläche **Senden**, um die Anwendung zur Überprüfung an Apple zu senden.
7. Sie werden vom Application Loader informiert, sobald die Datei erfolgreich hochgeladen wurde.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>Status in iTunes Connect

Wenn Sie erneut bei iTunes Connect anmelden, und wählen Sie Ihre app aus der Liste der verfügbaren apps, der Status in iTunes Connect sollte nun anzeigen, dass es **warten auf Überprüfung** (möglicherweise vorübergehend lesen **Upload erhalten** während der Verarbeitung):

[![](app-store-publishing-images/image21.png "Der Status in iTunes Connect mit Warten auf Überprüfung")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie beim Übermitteln der Xamarin.tvOS-app an das Apple TV App Store Probleme auftreten, finden Sie unsere [Problembehandlung](~/ios/tvos/troubleshooting.md) Guide. Sie enthält mehrere bekannte Probleme, die auftreten können und wie Sie sie in der Xamarin.tvOS lösen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine schrittweise Anleitung zum Konfigurieren, erstellen und Übermitteln einer app für Apple TV App Store-Veröffentlichung. Zuerst wird die Vorgehensweise zum Erstellen und Installieren eines Verteilungsbereitstellungsprofils erklärt. Als Nächstes wurde erläutert, wie sie mit Visual Studio für Mac zum Erstellen eines. Schließlich wird aufgezeigt, wie Sie iTunes Connect und das Xcode-Archiv-Tool verwenden, um eine Anwendung in der Apple TV App Store zu übermitteln.


## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md)
- [Vorbereiten der Übermittlung der App für die neue Apple TV](https://developer.apple.com/tvos/submit/)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Common App Rejections (Häufige Ablehnungsgründe für Apps)](https://developer.apple.com/app-store/review/rejections/)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
