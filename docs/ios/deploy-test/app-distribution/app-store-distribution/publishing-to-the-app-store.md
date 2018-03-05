---
title: "Veröffentlichen im App Store"
description: "In diesem Artikel wird beschrieben, wie Sie eine Xamarin.iOS-Anwendung für die Verteilung über den App Store konfigurieren, erstellen und veröffentlichen. In dieser Anleitung wird erklärt, wie Sie eine Anwendung für die Verteilung vorbereiten, wie Sie sie mithilfe der Tools von Apple zur Überprüfung übermitteln und wie Sie Ihre Anwendung schließlich im App Store veröffentlichen."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 84db17ede0019e1134b65edaca85ef2401fb3bc0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-the-app-store"></a>Veröffentlichen im App Store

_In diesem Artikel wird beschrieben, wie Sie eine Xamarin.iOS-Anwendung für die Verteilung über den App Store konfigurieren, erstellen und veröffentlichen. In dieser Anleitung wird erklärt, wie Sie eine Anwendung auf die Verteilung vorbereiten, wie Sie sie mithilfe der Tools von Apple zur Überprüfung übermitteln und wie Sie Ihre Anwendung schließlich im App Store veröffentlichen._

Apple erfordert die Veröffentlichung von Apps über den *App Store*, damit sie an alle iOS-Geräte verteilt werden können. Dadurch wird der App Store zur zentralen Anlaufstelle für iOS-Anwendungen. Entwickler vieler verschiedener Arten von Anwendungen profitieren vom großen Erfolg dieser zentralen Verteilungsstelle mit über 500.000 verfügbaren Anwendungen. Der App Store bietet App-Entwicklern als sofort einsetzbare Lösung sowohl Verteilung als auch Zahlungssysteme.

Der Übermittlungsprozess einer Anwendung an den App Store umfasst die folgenden Schritte:

1. Erstellen einer **App-ID** und Auswählen von **Berechtigungen**
2. Erstellen eines **Verteilungsbereitstellungsprofils**
3. Erstellen der Anwendung mit diesem Profil
4. Übermitteln der Anwendung mit **iTunes Connect**


In diesem Artikel werden alle Schritte beschrieben, die zur Bereitstellung, Erstellung und Übermittlung einer Anwendung für die Verteilung im App Store erforderlich sind.

## <a name="before-you-submit-an-application"></a>Vor dem Übermitteln der Anwendung

Nach der Übermittlung einer App zur Veröffentlichung an den App Store durchläuft sie einen Überprüfungsprozess von Apple, mit dem sichergestellt werden soll, dass die Qualitäts- und Inhaltsrichtlinien von Apple erfüllt werden. Erfüllt Ihre Anwendung diese Richtlinien nicht, wird sie von Apple abgelehnt. In diesem Fall müssen Sie die von Apple angeführten Nichtübereinstimmungen beheben und die Anwendung erneut übermitteln.
Daher sollten Sie sich mit diesen Richtlinien vertraut machen und Ihre Anwendung bestmöglich anpassen, um Ihre Chancen bei der Überprüfung durch Apple zu erhöhen. Die Apple-Richtlinien finden Sie unter: [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html).

Beachten Sie bei der Übermittlung einer App Folgendes:

1. Stellen Sie sicher, dass die Beschreibung der Anwendung mit den enthaltenen Funktionen übereinstimmt.
2. Überprüfen Sie, dass die Anwendung bei normaler Nutzung nicht abstürzt. Dies schließt die Nutzung auf jedem iOS-Gerät, das Sie unterstützen, mit ein.


Apple verwaltet auch eine Liste mit Tipps für die Übermittlung an den App Store. Diese Tipps finden Sie unter [Distributing on the App Store](https://developer.apple.com/appstore/resources/submission/tips.html) (Verteilen im App Store).

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurieren der Anwendung in iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) ist eine Suite von webbasierten Tools, mit denen Sie u.a. Ihre iOS-Anwendungen im App Store verwalten können. Ihre Xamarin.iOS-Anwendung muss ordnungsgemäß in iTunes Connect eingerichtet und konfiguriert werden, bevor sie zur Überprüfung an Apple gesendet und schließlich für den Verkauf oder als kostenlose App im App Store freigegeben werden kann.

Führen Sie folgende Schritte aus:

1. Gehen Sie in iTunes Connect zum Abschnitt **Vereinbarungen, Steuern und Bankgeschäfte**. Stellen Sie sicher, dass die Vereinbarungen richtig eingerichtet und auf dem neuesten Stand sind, um eine iOS-Anwendung kostenlos oder zum Verkauf freizugeben.
2. Erstellen Sie einen neuen **iTunes Connect-Datensatz** für die Anwendung, und geben Sie den **Anzeigenamen** an (wie er im App Store angezeigt wird).
3. Wählen Sie den **Verkaufspreis** aus, oder geben Sie an, dass die Anwendung kostenlos ist.
5. Liefern Sie eine kurze und klare **Beschreibung** der Anwendung, einschließlich aller Funktionen und Vorteile für den Endbenutzer.
6. Stellen Sie **Kategorien**, **Unterkategorien** und **Schlüsselwörter** bereit, damit die Benutzer Ihre App im App Store gut finden können.
7. Geben Sie wie von Apple erfordert **Kontaktinformationen** und **Support-URLs** zu Ihrer Website an.
8. Legen Sie die **Altersfreigabe** Ihrer Anwendung fest, die für die Jugendschutzeinstellungen im App Store eine Rolle spielt.
9. Konfigurieren Sie optionale App Store-Technologien, wie z.B. **Game Center** und **In-App-Käufe**.

Weitere Informationen finden Sie in der Dokumentation unter [Configuring an App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) (Konfigurieren einer App in iTunes Connect).

## <a name="preparing-for-app-store-distribution"></a>Vorbereiten für die Verteilung im App Store

Bevor Sie eine Anwendung im App Store veröffentlichen können, müssen Sie sie zuerst für die Verteilung erstellen. Dies erfordert zahlreiche Schritte. In den folgenden Abschnitten werden alle notwendigen Schritte zur Vorbereitung einer Xamarin.iOS-Anwendung für die Veröffentlichung beschrieben. Anschließend kann die Anwendung erstellt und zur Überprüfung und Freigabe an den App Store übermittelt werden.

### <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple bietet eine Auswahl von speziellen Anwendungsdiensten, auch Berechtigungen genannt, die für Ihre iOS-Anwendung beim Erstellen einer eindeutigen ID aktiviert werden können. Unabhängig davon, ob Sie benutzerdefinierte Berechtigungen verwenden oder nicht: Bevor Ihre Xamarin.iOS-Anwendung im App Store veröffentlicht werden kann, müssen Sie in jedem Fall eine eindeutige ID für sie erstellen.

Führen Sie die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus, um eine App-ID zu erstellen und optional Berechtigungen auszuwählen:

1. Wählen Sie im Abschnitt **Zertifikate, Bezeichner und Profile** die Option **Bezeichner** > **App-ID** aus.
2. Klicken Sie auf die **+**-Schaltfläche, und stellen Sie einen **Namen** und eine **Bündel-ID** für die neue Anwendung bereit.
3. Scrollen Sie zum unteren Ende des Bildschirms, und wählen Sie die **App Services** aus, die für Ihre Xamarin.iOS-Anwendung erforderlich sein sollen.
4. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Beim Definieren Ihrer App-ID müssen Sie zusätzlich zur Auswahl und Konfiguration der erforderlichen Anwendungsdienste auch die App-ID und die Berechtigungen in Ihrem Xamarin.iOS-Projekt konfigurieren. Bearbeiten Sie hierfür die Dateien `Info.plist` und `Entitlements.plist`.

Führen Sie die folgenden Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. Füllen Sie im Abschnitt **Ziel für iOS-Anwendung** den Namen für Ihre Anwendung aus, und geben Sie die **Bündel-ID** ein, die Sie beim Definieren der App-ID erstellt haben.
3. Speichern Sie die Änderungen an der Datei `Info.plist`.
4. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
5. Die Berechtigungen für Ihre Xamarin.iOS-Anwendung müssen so ausgewählt und konfiguriert werden, dass sie mit den Einstellungen übereinstimmen, die Sie beim Definieren der App-ID festgelegt haben.
6. Speichern Sie die Änderungen an der Datei `Entitlements.plist`.

Eine ausführliche Anleitung finden Sie in der Dokumentation unter [Provisioning for Application Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) (Bereitstellung für Anwendungsdienste).

### <a name="setting-the-store-icons"></a>Festlegen der Store-Symbole

App Store-Symbole sollten nun über einen Ressourcenkatalog bereitgestellt werden. Suchen Sie zunächst das **AppIcon**-Bild, das in der Datei **Assets.xcassets** Ihres Projekts festgelegt ist, um ein App Store-Symbol hinzuzufügen.

Das erforderliche Symbole im Ressourcenkatalog sollte **App Store** heißen und eine Größe von **1024 x 1024** aufweisen. Laut Apple kann das App Store-Symbol im Ressourcenkatalog nicht transparent sein oder einen Alphakanal enthalten.

Weitere Informationen zum Festlegen des Store-Symbols finden Sie im Handbuch [App Store Icon (App Store-Symbol)](~/ios/app-fundamentals/images-icons/app-store-icon.md).

### <a name="setting-the-apps-icons-and-launch-screens"></a>Festlegen der App-Symbole und Startbildschirme

Damit eine iOS-Anwendung von Apple für die Aufnahme in den App Store akzeptiert wird, benötigen Sie korrekte Symbole und Startbildschirme für alle iOS-Geräte, auf denen sie ausgeführt wird. App-Symbole werden über ein **AppIcon**-Bild, das in der Datei **Assets.xcassets** festgelegt ist, in einem Ressourcenkatalog zu Ihren Projekten hinzugefügt. Startbildschirme werden durch ein Storyboard hinzugefügt.

Detaillierte Anweisungen zum Erstellen von App-Symbolen und Startbildschirmen finden Sie in den Handbüchern zu [Anwendungssymbolen](~/ios/app-fundamentals/images-icons/app-icons.md) und [Startbildschirmen](~/ios/app-fundamentals/images-icons/launch-screens.md).

### <a name="creating-and-installing-a-distribution-profile"></a>Erstellen und Installieren eines Verteilungsprofils

iOS verwendet *Bereitstellungsprofile*, um zu steuern, wie ein bestimmtes Anwendungsbuild bereitgestellt werden kann. Hierbei handelt es sich um Dateien mit Informationen zu dem Zertifikat, das zum Signieren einer App verwendet wurde, der *Anwendungs-ID* sowie dem Ort, an dem die App installiert werden kann. Für die Entwicklung und Ad-hoc-Verteilung enthält das Bereitstellungsprofil auch eine Liste der zulässigen Geräte, auf denen die App bereitgestellt werden kann. Für die Verteilung im App Store enthält das Profil jedoch nur Zertifikats- und App-ID-Informationen, da eine öffentliche Verteilung nur über den App Store möglich ist.

Führen Sie für die Bereitstellung die folgenden Schritte mithilfe des webbasierten iOS-Bereitstellungsportals von Apple aus:

1.  Wählen Sie **Bereitstellung** > **Verteilung** aus.
2.  Klicken Sie auf die **+**-Schaltfläche, und wählen Sie als Typ des Verteilungsprofils, das Sie erstellen möchten, **App Store** aus.
3.  Wählen Sie aus der Dropdownliste die **App-ID** aus, für die Sie ein Verteilungsprofil erstellen möchten.
4.  Wählen Sie ein gültiges (Verteilungs-)Produktionszertifikat aus, um die Anwendung zu signieren.
5.  Geben Sie einen **Namen** für das neue **Verteilungsprofil** ein, und generieren Sie das Profil.
6.  Öffnen Sie Mac in Xcode, und navigieren Sie zu **Einstellungen > [Wählen Sie Ihre Apple-ID aus] > Details anzeigen...**. Laden Sie alle verfügbaren Profile in Xcode auf den Mac herunter.
7.  Wechseln Sie zurück zur IDE, und wählen Sie aus den Optionen für die **iOS-Bundle-Signierung** das Verteilungsbereitstellungsprofil für die korrekte _Buildkonfiguration_ aus (entweder **App Store** oder **Freigabe**).

Eine ausführliche Anleitung finden Sie unter [Creating a Distribution Profile](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) (Erstellen eines Verteilungsprofils) und [Selecting a Distribution Profile in a Xamarin.iOS Project](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile) (Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt).

## <a name="setting-the-build-configuration-for-your-application"></a>Festlegen der Buildkonfiguration für die Anwendung

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Führen Sie folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Lösungspad** auf den **Projektnamen**. Wählen Sie **Optionen** aus, um diese für die Bearbeitung zu öffnen.
2. Klicken Sie in der Dropdownliste **Konfiguration** auf die Optionen **iOS-Build** und **Release > iPhone**:

    ![](publishing-to-the-app-store-images/configurevs01.png "Wählen Sie „AppStore“ in der Dropdownliste „Konfiguration“ aus")

3. Wenn Sie eine bestimmte iOS-Version als Zielversion angeben möchten, können Sie diese in der Dropdownliste **SDK-Version** auswählen. Andernfalls behalten Sie den Standardwert bei.
4. Verknüpfungen verringern die Gesamtgröße der Anwendung, indem nicht verwendete Methoden, Eigenschaften, Klassen usw. entfernt werden. In den meisten Fällen sollte die Standardeinstellung **Nur SDK-Assemblys verknüpfen** beibehalten werden. In einigen Situationen, wie z.B. bei der Verwendung von bestimmten Drittanbieterbibliotheken, müssen Sie diesen Wert möglicherweise auf **Nicht verknüpfen** festlegen, um zu verhindern, dass erforderliche Elemente entfernt werden. Weitere Informationen finden Sie in der Anleitung [iOS Build Mechanics](~/ios/deploy-test/ios-build-mechanics.md) (iOS-Buildmechanik).
5. Aktivieren Sie das Kontrollkästchen **PNG-Bilddateien für iOS optimieren**, um die Gesamtgröße Ihrer Anwendung noch weiter zu verringern.
6. Das Debuggen sollte _nicht_ aktiviert sein, um das Build nicht unnötig zu vergrößern.
8. Für iOS 11 müssen Sie eine Gerätearchitektur auswählen, die **ARM64** unterstützt. Weitere Informationen zu Builds auf 64-Bit-iOS-Geräten finden Sie im Abschnitt **Enabling 64 Bit Builds of Xamarin.iOS Apps** (64-Bit-Builds in Xamarin.iOS-Apps aktiveren) der Dokumentation [32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64.md) (Überlegungen zu 32-/64-Bit-Plattformen).
9. Optional können Sie auch den **LLVM**-Compiler für kleineren und schnelleren Code verwenden. Die Kompilierung dauert in diesem Fall jedoch länger.
10. Entsprechend der Anforderungen Ihrer Anwendung möchten Sie möglicherweise auch den Typ der für die **Internationalisierung** verwendeten und eingerichteten **Garbage Collection** anpassen.
11. Speichern Sie die Änderungen an der Buildkonfiguration.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Standardmäßig werden beim Erstellen einer neuen Xamarin.iOS-Anwendung in Visual Studio automatisch die _Buildkonfigurationen_ für die **Ad-hoc**- und die **App Store**-Bereitstellung erstellt. Vor dem Erstellen des endgültigen Builds Ihrer Anwendung, das an Apple übermittelt werden soll, müssen Sie einige Änderungen an der Basiskonfiguration vornehmen.

Führen Sie folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den **Projektnamen**. Wählen Sie **Eigenschaften** aus, um diese für die Bearbeitung zu öffnen.
2. Klicken Sie im Dropdownmenü **Konfiguration** auf **iOS Build** und **AppStore** (alternativ **Release**, falls „AppStore“ nicht verfügbar ist):

    ![](publishing-to-the-app-store-images/configurevs01.png "Wählen Sie „AppStore“ in der Dropdownliste „Konfiguration“ aus")

3. Wenn Sie eine bestimmte iOS-Version als Zielversion angeben möchten, können Sie diese in der Dropdownliste **SDK-Version** auswählen. Andernfalls behalten Sie den Standardwert bei.
4. Verknüpfungen verringern die Gesamtgröße der Anwendung, indem nicht verwendete Methoden, Eigenschaften, Klassen usw. entfernt werden. In den meisten Fällen sollte die Standardeinstellung **Nur SDK-Assemblys verknüpfen** beibehalten werden. In einigen Situationen, wie z.B. bei der Verwendung von bestimmten Drittanbieterbibliotheken, müssen Sie diesen Wert möglicherweise auf **Nicht verknüpfen** festlegen, um zu verhindern, dass erforderliche Elemente entfernt werden. Weitere Informationen finden Sie in der Anleitung [iOS Build Mechanics](~/ios/deploy-test/ios-build-mechanics.md) (iOS-Buildmechanik).
5. Aktivieren Sie das Kontrollkästchen **PNG-Bilddateien für iOS optimieren**, um die Gesamtgröße Ihrer Anwendung noch weiter zu verringern.
6. Das Debuggen sollte _nicht_ aktiviert sein, um das Build nicht unnötig zu vergrößern.
7. Klicken Sie auf die Registerkarte **Erweitert**:

    ![](publishing-to-the-app-store-images/configurevs02.png "Die Registerkarte „Erweitert“")

8. Wenn die Zielgeräte Ihrer Xamarin.iOS-Anwendung iOS 8- und 64-Bit-iOS-Geräte sind, müssen Sie eine der Gerätearchitekturen auswählen, die **ARM64** unterstützen. Weitere Informationen zu Builds auf 64-Bit-iOS-Geräten finden Sie im Abschnitt **Enabling 64 Bit Builds of Xamarin.iOS Apps** (64-Bit-Builds in Xamarin.iOS-Apps aktiveren) der Dokumentation [32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64.md) (Überlegungen zu 32-/64-Bit-Plattformen).
9. Optional können Sie auch den **LLVM**-Compiler für kleineren und schnelleren Code verwenden. Die Kompilierung dauert in diesem Fall jedoch länger.
10. Entsprechend der Anforderungen Ihrer Anwendung möchten Sie möglicherweise auch den Typ der für die **Internationalisierung** verwendeten und eingerichteten **Garbage Collection** anpassen.
11. Speichern Sie die Änderungen an der Buildkonfiguration.

-----

## <a name="building-and-submitting-the-distributable"></a>Erstellen und Übermitteln der verteilbaren Anwendung

Nachdem Ihre Xamarin.iOS-Anwendung ordnungsgemäß konfiguriert wurde, können Sie nun das endgültige Verteilungsbuild erstellen, das an Apple zur Überprüfung und Freigabe übermittelt wird.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

### <a name="build-your-archive"></a>Erstellen des Archivs

1. Wählen Sie in Visual Studio für Mac die Konfiguration **Release | Gerät** aus:

    ![](publishing-to-the-app-store-images/buildxs01new.png "Wählen Sie „Release | Gerätekonfiguration“")
1. Wählen Sie aus dem **Build**-Menü die Option **Zur Veröffentlichung aktivieren**:

    ![](publishing-to-the-app-store-images/buildxs02new.png "Wählen Sie „Für Veröffentlichung aktivieren“ aus")

1. Sobald das Archiv erstellt wurde, wird die **Archivansicht** angezeigt:

    ![](publishing-to-the-app-store-images/buildxs03new.png "Die Ansicht „Archive“ wird angezeigt")


> [!NOTE]
> Hinweis: Während der frühere _App Store_ und ältere _Ad-hoc_-Konfigurationen mittlerweile aus allen Visual Studio für Mac-Vorlagenprojekten entfernt wurden, können diese Konfigurationen möglicherweise noch in älteren Projekten enthalten sein. In diesem Fall können Sie die Konfiguration **App Store | Gerät** wie in Schritt 1 oben gezeigt weiterhin verwenden.

### <a name="sign-and-distribute-your-app"></a>Signieren und Verteilen der App

 Beim Erstellen Ihrer Anwendung für das Archiv wird automatisch die **Archiv-Ansicht** geöffnet. Darin werden alle archivierten Projekte nach Projektmappe gruppiert angezeigt. Standardmäßig wird in dieser Ansicht nur die aktuelle geöffnete Projektmappe angezeigt. Klicken Sie auf **Alle Archive anzeigen**, um alle Projektmappen mit Archiven anzuzeigen.

 Es wird empfohlen, Archive beizubehalten, die bei den Kunden bereitgestellt wurden (App Store- oder Unternehmensbereitstellungen). Dadurch können alle generierten Debuginformationen zu einem späteren Zeitpunkt symbolisiert werden.

 Gehen Sie folgendermaßen vor, um Ihre App für die Verteilung zu signieren und vorzubereiten:


1. Wählen Sie wie unten dargestellt die Option **Signieren und verteilen...**  aus:

    ![](publishing-to-the-app-store-images/buildxs04new.png "Wählen Sie „Signieren und verteilen...“ aus")

1. Dadurch wird der Veröffentlichungs-Assistent geöffnet. Wählen Sie den Verteilungskanal **App Store** aus, um ein Paket zu erstellen, und öffnen Sie den Application Loader:

    ![](publishing-to-the-app-store-images/distribute01.png "Öffnen Sie das Anwendungsladeprogramm")

1. Wählen Sie auf der Seite „Bereitstellungsprofil“ Ihre Signierungsidentität sowie das entsprechende Bereitstellungsprofil aus, oder signieren Sie mit einer anderen Identität erneut:

    ![](publishing-to-the-app-store-images/distribute02.png "Wählen Sie die Signieridentität und das entsprechende Bereitstellungsprofil aus")

1. Überprüfen Sie die Details Ihres Pakets, und klicken Sie zum Speichern des `.ipa`-Pakets auf **Veröffentlichen**:

    ![](publishing-to-the-app-store-images/distribute03.png "Überprüfen Sie die Paketdetails")

1. Sobald die `.ipa` gespeichert wurde, kann Ihre App über den Application Loader in iTunes Connect hochgeladen werden:

    ![](publishing-to-the-app-store-images/distribute04.png "Der Bildschirm „Veröffentlichung erfolgreich“")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Das Xamarin-Plug-In für Visual Studio unterstützt derzeit nicht den Archivierungsworkflow für die Veröffentlichung von iOS-Anwendungen im App Store. Daher müssen Sie eine mit dem Befehl **Erstellen einer Ad-hoc-IPA** erstellte IPA-Datei wie unten beschrieben hochladen.


## <a name="build-an-ipa"></a>Erstellen einer IPA-Datei

 In diesem Abschnitt wird beschrieben, wie Sie eine IPA-Datei erstellen. Die Vorgehensweise ähnelt dem Workflow bei der Verwendung der Ad-hoc- oder Unternehmensverteilung. Allerdings wird sie mit dem oben beschriebenen App Store-Bereitstellungsprofil signiert.

 Führen Sie folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Xamarin.iOS-Projektnamen. Klicken Sie auf **Eigenschaften**, um diese für die Bearbeitung zu öffnen:

    ![](publishing-to-the-app-store-images/imagevs01.png "Wählen Sie „Eigenschaften“ aus")
1. Wählen Sie **iOS-Bundle-Signierung** aus, und ändern Sie das Bereitstellungsprofil in ein App Store-Bereitstellungsprofil:

    ![](publishing-to-the-app-store-images/ipa01.png "Wählen Sie „iOS-Bündelsignierung“ aus, und ändern Sie das Bereitstellungsprofil in ein App Store-Bereitstellungsprofil")
1. Wählen Sie **iOS-IPA-Optionen** aus, und wählen Sie anschließend aus der Dropdownliste **Konfiguration** die Option **Ad-hoc** aus (wenn „Ad-hoc“ nicht angezeigt wird, wählen Sie stattdessen **Freigabe** aus):

    ![](publishing-to-the-app-store-images/imagevs02.png "Wählen Sie „Ad-Hoc“ in der Dropdownliste „Konfiguration“ aus")

1. Sie können für die IPA-Datei optional einen **Paketnamen** angeben. Wird kein Paketname angegeben, erhält die Datei den gleichen Namen wie das Xamarin.iOS-Projekt.
1. Speichern Sie die Änderungen an den Projekteigenschaften.
1. Wählen Sie in Visual Studio für Mac aus der Dropdownliste **Buildkonfiguration** die Option **Ad-hoc** aus:

    ![](publishing-to-the-app-store-images/imagevs05.png "Wählen Sie „Ad-Hoc“ in der Dropdownliste „Buildkonfiguration“ aus")
1. Erstellen Sie das Projekt, um das IPA-Paket zu erstellen.
1. Die IPA-Datei wird im Ordner `Bin` > _iOS-Gerät_ > `Ad Hoc` erstellt:

    ![](publishing-to-the-app-store-images/imagevs06.png "Die im Datei-Explorer angezeigte IPA-Datei")

-----


## <a name="customizing-the-ipa-location"></a>Anpassen des IPA-Speicherorts

Eine neue **MSBuild**-Eigenschaft `IpaPackageDir` wurde hinzugefügt, um das Anpassen des Ausgabespeicherorts der `.ipa`-Datei zu vereinfachen. Wenn für einen benutzerdefinierten Speicherort `IpaPackageDir` festgelegt ist, wird die `.ipa`-Datei an diesem Speicherort anstatt im Standardunterverzeichnis mit Zeitstempel abgelegt. Dies kann beim Erstellen von automatisierten Builds nützlich sein, die für eine korrekte Ausführung einen bestimmten Verzeichnispfad benötigen, wie beispielsweise für fortlaufende Integrationsbuilds (CI, Continuous Integration).

Die neue Eigenschaft kann auf unterschiedliche Weise verwendet werden:

Für eine Ausgabe der `.ipa`-Datei in das frühere Standardverzeichnis (wie in Xamarin.iOS 9.6 und früher) können Sie die `IpaPackageDir`-Eigenschaft z.B. mit einer der folgenden Methoden auf `$(OutputPath)` festlegen. Beide Methoden sind mit allen vereinheitlichten Xamarin.iOS-API-Builds, einschließlich IDE-Builds, sowie Befehlszeilenbuilds, die `xbuild`, `msbuild` oder `mdtool` verwenden, kompatibel:

  - Die erste Möglichkeit besteht darin, die `IpaPackageDir`-Eigenschaft in einem `<PropertyGroup>`-Element in einer **MSBuild**-Datei festzulegen. Sie könnten z.B. die folgende `<PropertyGroup>` am Ende der iOS-App-Projektdatei `.csproj` hinzufügen (unmittelbar vor dem `</Project>`-Endtag):

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - Eine bessere Möglichkeit ist das Hinzufügen eines `<IpaPackageDir>`-Elements am Ende der vorhandenen `<PropertyGroup>`, die der beim Erstellen der `.ipa`-Datei verwendeten Konfiguration entspricht. Der Vorteil dieser Möglichkeit ist, dass das Projekt hier für die Kompatibilität mit einer zukünftigen Einstellung auf der Projekteigenschaftenseite der iOS-IPA-Optionen vorbereitet wird. Wenn Sie derzeit die Konfiguration `Release|iPhone` zum Erstellen der `.ipa`-Datei verwenden, könnte die vollständige aktualisierte Eigenschaftengruppe etwa folgendermaßen aussehen:

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
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
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
      </PropertyGroup>
      ```
Eine alternative Methode für `msbuild`- oder `xbuild`-Befehlszeilenbuilds ist das Hinzufügen eines `/p:`-Befehlszeilenarguments, um die `IpaPackageDir`-Eigenschaft festzulegen. Beachten Sie, dass `msbuild` in diesem Fall `$()`-Ausdrücke nicht erweitert, die in der Befehlszeile übergeben werden. Daher kann die `$(OutputPath)`-Syntax nicht verwendet werden. Geben Sie stattdessen einen vollständigen Pfadnamen an. Der Mono-Befehl `xbuild` erweitert zwar `$()`-Ausdrücke, doch sollten Sie trotzdem lieber einen vollständigen Pfadnamen verwenden, da `xbuild` letztendlich in zukünftigen Versionen gegenüber der [plattformübergreifenden `msbuild`-Version](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) veraltet sein wird. Ein vollständiges Beispiel mit dieser Methode könnte unter Windows etwa folgendermaßen aussehen:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Unter Mac könnte es folgendermaßen aussehen:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Nachdem Ihr Verteilungsbuild erstellt und archiviert wurde, können Sie nun Ihre Anwendung an iTunes Connect übermitteln.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatisches Zurückkopieren von App Bundles nach Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Übermitteln der App an Apple

> [!NOTE]
> Hinweis: Der Überprüfungsprozess für iOS-Anwendungen wurde vor Kurzem von Apple geändert. Apps, die `iTunesMetadata.plist` in der IPA-Datei enthalten, werden nun möglicherweise abgelehnt. Tritt bei Ihnen der Fehler `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"` auf, hilft Ihnen die [hier](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) beschriebene Problemumgehung bei der Lösung des Problems.

Nach Abschluss des Verteilungsbuilds können Sie Ihre iOS-Anwendung nun zur Überprüfung und Freigabe im App Store an Apple übermitteln.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 Führen Sie folgende Schritte aus:

1. Starten Sie **Xcode**.
2. Wählen Sie im Menü **Fenster** die Option **Organisator** aus.
3. Klicken Sie auf die Registerkarte **Archive**, und wählen Sie das zuvor erstellte Archiv aus:

    ![](publishing-to-the-app-store-images/publishxs01.png "Wählen Sie das zu übermittelnde Archiv aus")
4. Klicken Sie auf die Schaltfläche **Überprüfen...**.
5. Wählen Sie das Konto aus, das überprüft werden soll, und klicken Sie auf die Schaltfläche **Auswählen**:

    ![](publishing-to-the-app-store-images/publishxs02.png "Wählen Sie das Konto aus, das überprüft werden soll")
6. Klicken Sie auf die Schaltfläche **Überprüfen**:

    ![](publishing-to-the-app-store-images/publishxs03.png "Das Dialogfeld „Dateizusammenfassung“")
7. Falls Probleme mit dem Bündel aufgetreten sind, werden diese nun angezeigt.
8. Beheben Sie eventuelle Probleme, und erstellen Sie das Archiv in Visual Studio für Mac erneut.
9. Klicken Sie auf die Schaltfläche **Senden...**.
10. Wählen Sie das Konto aus, das überprüft werden soll, und klicken Sie auf die Schaltfläche **Auswählen**:

    ![](publishing-to-the-app-store-images/publishxs04.png "Wählen Sie das Konto aus, das überprüft werden soll")
11. Klicken Sie auf die Schaltfläche **Senden**:

    ![](publishing-to-the-app-store-images/publishxs05.png "Das Dialogfeld „Dateizusammenfassung“")
12. Sie werden von Xcode informiert, wenn die Datei zu iTunes Connect hochgeladen wurde.


Application Loader wird automatisch durch den Archivworkflow in Visual Studio für Mac geöffnet, sobald Sie die _IPA_-Datei gespeichert haben:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Das Übermitteln Ihrer Anwendung zur Überprüfung an Apple erfolgt über die Application Loader-App. Führen Sie die folgenden Schritte auf dem Mac-Buildhost aus. [Hier](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg) können Sie eine Kopie von Application Loader herunterladen.

-----

1. Wählen Sie *Ihre App übermitteln* aus, und klicken Sie auf die Schaltfläche *Auswählen*:

    [ ![](publishing-to-the-app-store-images/publishvs01.png "Wählen Sie „Ihre App übermitteln“ aus")](publishing-to-the-app-store-images/publishvs01.png)

2. Wählen Sie die zuvor erstellte ZIP- oder IPA-Datei aus, und klicken Sie auf die Schaltfläche **OK**.

3. Die Datei wird durch den Application Loader überprüft:

    [ ![](publishing-to-the-app-store-images/publishvs02.png "Der Validierungsbildschirm")](publishing-to-the-app-store-images/publishvs02.png)
4. Klicken Sie auf die Schaltfläche *Weiter*. Die Anwendung wird nun für den App Store überprüft:

    [ ![](publishing-to-the-app-store-images/publishvs03.png "Überprüfen für den App Store")](publishing-to-the-app-store-images/publishvs03.png)
5. Klicken Sie auf die Schaltfläche **Senden**, um die Anwendung zur Überprüfung an Apple zu senden.
6. Sie werden vom Application Loader informiert, sobald die Datei erfolgreich hochgeladen wurde.

## <a name="itunes-connect-status"></a>Status in iTunes Connect

Wenn Sie sich nun erneut bei iTunes Connect anmelden und Ihre Anwendung aus der Liste der verfügbaren Apps auswählen, wird der Status in iTunes Connect mit **Warten auf Überprüfung** angezeigt (während der Verarbeitung wird möglicherweise vorübergehend **Upload erhalten** angezeigt):

[ ![](publishing-to-the-app-store-images/image21.png "Der Status in iTunes Connect sollte nun „Auf Überprüfung wird gewartet“ lauten")](publishing-to-the-app-store-images/image21.png)

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine ausführliche Anleitung zur Konfiguration, Erstellung und Übermittlung einer Anwendung für die Veröffentlichung im App Store. Zuerst wird die Vorgehensweise zum Erstellen und Installieren eines Verteilungsbereitstellungsprofils erklärt. Anschließend wurde erläutert, wie Visual Studio und Visual Studio für Mac zum Erstellen eines Verteilungsbuilds verwendet werden. Zum Schluss wird aufgezeigt, wie iTunes Connect und das Tool zur Übermittlung der Anwendung an den App Store verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Working with Images (Arbeiten mit Bildern)](~/ios/app-fundamentals/images-icons/index.md)
- [Anleitung für den iOS-App-Entwicklungsworkflow: Verteilen von Anwendungen](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Common App Rejections (Häufige Ablehnungsgründe für Apps)](https://developer.apple.com/app-store/review/rejections/)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
