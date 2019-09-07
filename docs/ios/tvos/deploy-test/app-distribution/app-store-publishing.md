---
title: Veröffentlichen im Apple TV App Store
description: In diesem Dokument wird beschrieben, wie Sie eine APP im Apple TV App Store veröffentlichen. Darin wird erläutert, wie Sie eine mit xamarin erstellten tvos-Anwendung konfigurieren, bereitstellen, erstellen und übermitteln.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 4dface536504b0a79d376ab0979443a5ed19e901
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769248"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Veröffentlichen im Apple TV App Store

Um Anwendungen an alle Apple TV-Geräte zu verteilen, erfordert Apple, dass Apps über den *Apple TV App Store*veröffentlicht werden. Dadurch wird der App Store zu einem einzigen Ort für tvos-apps. Entwickler von vielen Arten von Apps können mit dem enormen Erfolg dieses einzelnen Verteilungs Punkts in Großbuchstaben arbeiten. Der Apple TV App Store ist eine sofort einsetzbare Lösung, die APP-Entwicklern sowohl Verteilungs-als auch Zahlungssysteme bietet.

Der Vorgang zum Übermitteln einer Anwendung an den Apple TV App Store umfasst Folgendes:

1. Erstellen einer *App-ID* und Auswählen von *Berechtigungen*
2. Erstellen eines *Verteilungsbereitstellungsprofils*
3. Verwenden Sie dieses Profil, um Ihre APP zu erstellen.
4. Übermitteln Ihrer APP über *iTunes Connect*.

In diesem Artikel werden alle erforderlichen Schritte zum Bereitstellen, erstellen und Übermitteln einer APP für die Verteilung von Apple TV App Store behandelt.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Vor dem Übermitteln der Anwendung

Nachdem Sie eine APP für die Veröffentlichung im Apple TV App Store eingereicht haben, durchläuft Sie einen Überprüfungsprozess von Apple, um sicherzustellen, dass Sie die Richtlinien von Apple für Qualität und Inhalte erfüllt. Erfüllt Ihre Anwendung diese Richtlinien nicht, wird sie von Apple abgelehnt. In diesem Fall müssen Sie die von Apple angeführten Nichtübereinstimmungen beheben und die Anwendung erneut übermitteln.
Daher sollten Sie sich mit diesen Richtlinien vertraut machen und Ihre Anwendung bestmöglich anpassen, um Ihre Chancen bei der Überprüfung durch Apple zu erhöhen. Die Apple-Richtlinien finden Sie unter [Richtlinien für den App Store-Review](https://developer.apple.com/appstore/resources/approval/guidelines.html) und [Vorbereiten Ihrer APP-Übermittlung für das neue Apple TV](https://developer.apple.com/tvos/submit/).

Beachten Sie bei der Übermittlung einer App Folgendes:

1. Stellen Sie sicher, dass die Beschreibung der APP mit der in der APP enthaltenen Funktionalität übereinstimmt.
2. Stellen Sie sicher, dass die App bei normaler Nutzung nicht abstürzt. Dies schließt die Verwendung auf jedem von Ihnen unterstützten Apple TV-Gerät ein.

Apple unterhält außerdem eine Liste der Tipps für die Übermittlung von Apple TV App Store. Diese Tipps finden Sie unter [Distributing on the App Store](https://developer.apple.com/appstore/resources/submission/tips.html) (Verteilen im App Store).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurieren der Anwendung in iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) ist eine Suite von webbasierten Tools, die unter anderem die Verwaltung Ihrer tvos-apps im Apple TV App Store unterstützen. Ihre xamarin. tvos-app muss in iTunes Connect ordnungsgemäß eingerichtet und konfiguriert werden, bevor Sie zur Überprüfung an Apple übermittelt werden kann, und schließlich für den Verkauf oder als kostenlose App im Apple TV App Store veröffentlicht werden kann.

Führen Sie folgende Schritte aus:

1. Gehen Sie in iTunes Connect zum Abschnitt **Vereinbarungen, Steuern und Bankgeschäfte**. Stellen Sie sicher, dass die Vereinbarungen richtig eingerichtet und auf dem neuesten Stand sind, um eine iOS-Anwendung kostenlos oder zum Verkauf freizugeben.
2. Erstellen Sie einen neuen **iTunes Connect-Datensatz** für die Anwendung, und geben Sie den **anzeigen Amen** an (wie im Apple TV App Store zu sehen).
3. Wählen Sie den **Verkaufspreis** aus, oder geben Sie an, dass die Anwendung kostenlos ist.
4. Geben Sie ein **App Store-Symbol** (großes Symbol) und Screenshots Ihrer Anwendung in Aktion auf den unterstützten Apple TV-Geräten an. Weitere Informationen finden Sie [im Handbuch arbeiten mit Symbolen und Images](~/ios/tvos/app-fundamentals/icons-images.md) .
5. Geben Sie eine klare, prägnante **Beschreibung** der APP einschließlich ihrer Features und Vorteile für den Endbenutzer an.
6. Stellen Sie **Kategorien**, **Unterkategorien**und **Schlüsselwörter** bereit, um dem Benutzer zu helfen, Ihre APP im Apple TV App Store zu finden.
7. Geben Sie wie von Apple erfordert **Kontaktinformationen** und **Support-URLs** zu Ihrer Website an.
8. Legen Sie die **Bewertung**Ihrer Anwendung fest, die von den Steuerelementen für Eltern im Apple TV App Store verwendet wird.
9. Konfigurieren Sie optionale App Store-Technologien, wie z.B. **Game Center** und **In-App-Käufe**.

Weitere Informationen finden Sie [in der Dokumentation configure your tvos app in iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) .

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Vorbereiten für die Verteilung im App Store

Wenn Sie eine APP im Apple TV App Store veröffentlichen möchten, müssen Sie Sie zuerst für die Verteilung erstellen. Dies umfasst viele Schritte. In den folgenden Abschnitten wird erläutert, wie Sie eine xamarin. tvos-App für die Veröffentlichung vorbereiten, damit Sie erstellt und zur Überprüfung und Freigabe an den Apple TV App Store übermittelt werden kann.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple bietet eine Auswahl spezieller Anwendungsdienste (auch als Berechtigungen bezeichnet), die für Ihre tvos-App aktiviert werden können, wenn Sie eine eindeutige ID für Sie erstellen. Unabhängig davon, ob Sie benutzerdefinierte Berechtigungen verwenden oder nicht, müssen Sie dennoch eine eindeutige ID für Ihre xamarin. tvos-app erstellen, bevor Sie im Apple TV App Store veröffentlicht werden kann.

Führen Sie die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus, um eine App-ID zu erstellen und optional Berechtigungen auszuwählen:

1. Wählen Sie **Bereitstellungs** > **Entwicklung**aus.
2. Klicken Sie auf die **+** -Schaltfläche, und stellen Sie einen **Namen** und eine **Bündel-ID** für die neue Anwendung bereit.
3. Scrollen Sie zum unteren Rand des Bildschirms, und wählen Sie alle **App Services** aus, die von der xamarin. tvos-App benötigt werden.
4. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Zusätzlich zur Auswahl und Konfiguration der erforderlichen Anwendungsdienste beim Definieren Ihrer APP-ID müssen Sie auch die APP-ID und die Berechtigungen in Ihrem xamarin. tvos-Projekt konfigurieren, indem Sie `Info.plist` sowohl `Entitlements.plist` die-als auch die-Datei bearbeiten.

Führen Sie die folgenden Schritte in Visual Studio für Mac aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. Geben Sie im Abschnitt **tvos-Anwendungs Ziel** einen Namen für Ihre Anwendung ein, und geben Sie die **Bündel** -ID ein, die Sie beim Definieren der APP-ID erstellt haben.
3. Speichern Sie die Änderungen an der Datei `Info.plist`.
4. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
5. Wählen Sie die erforderlichen Berechtigungen für Ihre xamarin. tvos-App aus, und konfigurieren Sie diese, damit Sie dem Setup entsprechen, das Sie oben beim Definieren der APP-ID ausgeführt haben.
6. Speichern Sie die Änderungen an der Datei `Entitlements.plist`.

Eine ausführliche Anleitung finden Sie in der Dokumentation unter [Provisioning for Application Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) (Bereitstellung für Anwendungsdienste). Während dieses Dokument für IOS geschrieben wurde, werden dieselben Schritte auch zum Bereitstellen einer xamarin. tvos-App verwendet.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Festlegen der APP-Symbole, Start Bild und Top-Regal Bild

Damit eine tvos-APP von Apple für die Einbindung in den Apple TV App Store akzeptiert wird, sind für alle Apple TV-Geräte, auf denen Sie ausgeführt wird, geeignete Symbole, Start-und Top-Regal-Bilder erforderlich. Sie müssen die erforderlichen bildassets hinzufügen, die in eine `Assets.car` Datei kompiliert und in das Paket ihrer xamarin. tvos-app eingefügt werden, bevor Sie in iTunes Connect hochgeladen werden.

Ausführliche Anweisungen finden Sie in der Dokumentation [Arbeiten mit Symbolen und Images](~/ios/tvos/app-fundamentals/icons-images.md) .

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Erstellen und Installieren eines Verteilungsprofils

tvos verwendet *Bereitstellungs profile* , um zu steuern, wie ein bestimmter anwendungsbuild bereitgestellt werden kann. Hierbei handelt es sich um Dateien mit Informationen zu dem Zertifikat, das zum Signieren einer App verwendet wurde, der *Anwendungs-ID* sowie dem Ort, an dem die App installiert werden kann. Für die Entwicklung und Ad-hoc-Verteilung enthält das Bereitstellungsprofil auch eine Liste der zulässigen Geräte, auf denen die App bereitgestellt werden kann. Bei der Verteilung des Apple TV-App-Stores sind jedoch nur Zertifikat-und APP-ID-Informationen enthalten, da der einzige Mechanismus für die öffentliche Verteilung über den Apple TV App Store ist.

Führen Sie für die Bereitstellung die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus:

1. Wählen Sie **Bereitstellung** > **Verteilung** aus.
2. Klicken Sie **+** auf die Schaltfläche, und wählen Sie den Typ des Verteilungs Profils aus, das Sie als **Apple TV App Store**erstellen möchten.
3. Wählen Sie aus der Dropdownliste die **App-ID** aus, für die Sie ein Verteilungsprofil erstellen möchten.
4. Wählen Sie das zum Signieren der Anwendung erforderliche Zertifikat aus.
5. Geben Sie einen **Namen** für das neue **Verteilungsprofil** ein, und generieren Sie das Profil.
6. Aktualisieren Sie die Liste der verfügbaren Profile in Xcode.
7. Wählen Sie das Verteilungs Bereitstellungs Profil in Visual Studio für die **App Store** -Buildkonfiguration aus.

Eine ausführliche Anleitung finden Sie unter [Creating a Distribution Profile](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) (Erstellen eines Verteilungsprofils) und [Selecting a Distribution Profile in a Xamarin.iOS Project](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile) (Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt). Beide Dokumente sind zwar für IOS spezifisch, aber die gleiche Technik wird für tvos-Apps verwendet.

<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Festlegen der Buildkonfiguration für die Anwendung

Wenn Sie eine neue xamarin. tvos-app erstellen, werden standardmäßig _Buildkonfigurationen_ sowohl für die **Debug** -als auch für die **releasebereitstellung** erstellt. Vor der endgültigen Erstellung Ihrer APP, die Sie an Apple übermitteln, müssen Sie einige Änderungen an der Basis **Releasekonfiguration** vornehmen.

Führen Sie folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste auf den **Projektnamen** in den **Projektmappen-Explorer** -und Auswahl **Optionen** , um Sie zur Bearbeitung zu öffnen.
2. Wenn Sie auf eine bestimmte Version von tvos abzielen, wählen Sie diese unter **tvos Build** > **IOS SDK-Version**aus. Legen Sie für die Vorschauversion der tvos-Unterstützung diesen Wert auf **default**fest.
3. Durch die Verknüpfung wird die Gesamtgröße der verteilbaren App der APP reduziert, indem nicht verwendete Methoden, Eigenschaften, Klassen usw. entfernt werden. in den meisten Fällen sollte der Standardwert des **Link Framework SDK nur**überlassen werden. In einigen Situationen, z. b. bei der Verwendung bestimmter Bibliotheken von Drittanbietern, ist es möglicherweise gezwungen, diesen Wert so festzulegen, dass er **nicht verknüpft** wird, damit das benötigte Element nicht entfernt wird.
4. Zum Versenden einer xamarin. tvos-App müssen Sie den llvm-Optimierungs Compiler verwenden. Stellen Sie sicher, dass das Kontrollkästchen **llvm-Optimierungs Compiler verwenden** unter der **Releasekonfiguration** aktiviert ist.
5. Außerdem erforderte Apple, dass tvos-apps Bitcode verwenden. Fügen Sie erneut unter der **Releasekonfiguration** dem Feld `--bitcode=asmonly` **zusätzliches mberührungs-Argument** hinzu.
6. Aktivieren Sie das Kontrollkästchen **PNG-Bilddateien für IOS optimieren** , da dies dazu beiträgt, die erstellbare Größe Ihrer APP weiter zu verringern.
7. Das Debuggen sollte *nicht* aktiviert werden, da dadurch der Build unnötig vergrößert wird.

<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Erstellen und Übermitteln der verteilbaren Anwendung

Wenn Sie Ihre xamarin. tvos-App ordnungsgemäß konfiguriert haben, können Sie nun den endgültigen verteilungsbuild durchführen, den Sie zur Überprüfung und Freigabe an Apple senden.

#### <a name="build-your-archive"></a>Erstellen des Archivs

1. Wählen Sie in Visual Studio für Mac die Konfiguration **Release | Gerät** aus:

    ![](app-store-publishing-images/buildxs01new.png "Auswählen der Releasekonfiguration")
2. Wählen Sie aus dem **Build**-Menü die Option **Zur Veröffentlichung aktivieren**:

    [![](app-store-publishing-images/buildxs02new.png "„Archive for Publishing“ (Für Veröffentlichung archivieren) auswählen")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt:

    [![](app-store-publishing-images/buildxs03new.png "Die Archivansicht")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen Ihrer App

Beim Erstellen Ihrer Anwendung für das Archiv wird automatisch die *Archiv-Ansicht* geöffnet. Darin werden alle archivierten Projekte nach Projektmappe gruppiert angezeigt. Standardmäßig wird in dieser Ansicht nur die aktuelle geöffnete Projektmappe angezeigt. Klicken Sie auf **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen.

Es wird empfohlen, Archive beizubehalten, die bei den Kunden bereitgestellt wurden (App Store- oder Unternehmensbereitstellungen). Dadurch können alle generierten Debuginformationen zu einem späteren Zeitpunkt symbolisiert werden.

Gehen Sie folgendermaßen vor, um Ihre App für die Verteilung zu signieren und vorzubereiten:

1. Wählen Sie das unten dargestellte **Vorzeichen und verteilen...** aus:

    [![](app-store-publishing-images/buildxs04new.png ", Wählen Sie die Option für das Signieren und verteilen...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Dadurch wird der Veröffentlichungs-Assistent geöffnet. Wählen Sie den Verteilungskanal **App Store** aus, um ein Paket zu erstellen, und öffnen Sie den Application Loader:

    [![](app-store-publishing-images/distribute01.png "Wählen Sie den Verteilungs Kanal für App Store aus.")](app-store-publishing-images/distribute01.png#lightbox)
3. Wählen Sie auf dem Bildschirm "Bereitstellungs Profil" Ihre Signierungs Identität und das zugehörige Bereitstellungs Profil aus, oder signieren Sie mit einer anderen Identität erneut:

    [![](app-store-publishing-images/distribute02.png "Signatur Identität und entsprechendes Bereitstellungs Profil auswählen")](app-store-publishing-images/distribute02.png#lightbox)
4. Überprüfen Sie die Details Ihres Pakets, und klicken Sie zum Speichern des `.ipa`-Pakets auf **Veröffentlichen**:

    [![](app-store-publishing-images/distribute03.png "Überprüfen Sie die Details des Pakets.")](app-store-publishing-images/distribute03.png#lightbox)
5. Sobald die `.ipa` gespeichert wurde, kann Ihre App über den Application Loader in iTunes Connect hochgeladen werden:

    [![](app-store-publishing-images/distribute04.png "Hochgeladen über das Anwendungs Lade Modul in iTunes Connect")](app-store-publishing-images/distribute04.png#lightbox)

Nachdem Ihr Verteilungsbuild erstellt und archiviert wurde, können Sie nun Ihre Anwendung an iTunes Connect übermitteln.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Übermitteln der App an Apple

Nach Abschluss des Verteilungsbuilds können Sie Ihre iOS-Anwendung nun zur Überprüfung und Freigabe im App Store an Apple übermitteln.

Der Archivierungs Workflow in Visual Studio für Mac öffnet das Anwendungs Lade Modul automatisch, sobald Sie `.ipa`Folgendes gespeichert haben:

1. Wählen Sie *Ihre App übermitteln* aus, und klicken Sie auf die Schaltfläche *Auswählen*:

    [![](app-store-publishing-images/publishvs01.png "Wählen Sie „Ihre App übermitteln“ aus")](app-store-publishing-images/publishvs01.png#lightbox)

2. Wählen Sie die zuvor erstellte ZIP- oder IPA-Datei aus, und klicken Sie auf die Schaltfläche **OK**.
3. Die Datei wird durch den Application Loader überprüft:

    [![](app-store-publishing-images/publishvs02.png "Der Validierungs Bildschirm des Anwendungs Lade Moduls")](app-store-publishing-images/publishvs02.png#lightbox)
4. Klicken Sie auf die Schaltfläche *Weiter*. Die Anwendung wird nun für den App Store überprüft:

    [![](app-store-publishing-images/publishvs03.png "Die Anwendung, die anhand des App Store überprüft wird.")](app-store-publishing-images/publishvs03.png#lightbox)
5. Klicken Sie auf die Schaltfläche **Senden**, um die Anwendung zur Überprüfung an Apple zu senden.
6. Sie werden vom Application Loader informiert, sobald die Datei erfolgreich hochgeladen wurde.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>Status in iTunes Connect

Wenn Sie sich bei iTunes Connect anmelden und Ihre APP aus der Liste der verfügbaren apps auswählen, sollte der Status in iTunes Connect nun anzeigen, dass er **auf die Überprüfung wartet** (während der Verarbeitung den **Upload** möglicherweise vorübergehend lesen):

[![](app-store-publishing-images/image21.png "Der Status in iTunes Connect, der auf die Überprüfung wartet")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme beim Einreichen Ihrer xamarin. tvos-APP an den Apple TV App Store haben, finden Sie weitere Informationen im Handbuch zur [Problem](~/ios/tvos/troubleshooting.md) Behandlung. Sie enthält mehrere bekannte Probleme, die auftreten können, und wie Sie in xamarin. tvos gelöst werden.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine Schritt-für-Schritt-Anleitung zum Konfigurieren, entwickeln und Übermitteln einer APP für die Apple TV App Store-Veröffentlichung vorgestellt. Zuerst wird die Vorgehensweise zum Erstellen und Installieren eines Verteilungsbereitstellungsprofils erklärt. Im nächsten Schritt wird erläutert, wie Sie mit Visual Studio für Mac einen verteilungsbuild erstellen. Schließlich haben Sie erfahren, wie Sie iTunes Connect und das Xcode Archive-Tool verwenden, um eine Anwendung an den Apple TV App Store zu übermitteln.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md)
- [Vorbereiten der APP-Übermittlung für das neue Apple TV](https://developer.apple.com/tvos/submit/)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Common App Rejections (Häufige Ablehnungsgründe für Apps)](https://developer.apple.com/app-store/review/rejections/)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
