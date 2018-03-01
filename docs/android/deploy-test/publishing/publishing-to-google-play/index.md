---
title: "Veröffentlichung in Google Play"
ms.topic: article
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 74635b10e97513d6b023cb44ede7745448aa153c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="publishing-to-google-play"></a>Veröffentlichung in Google Play

Obwohl es eine Reihe von App-Märkten für das Vertreiben von Anwendungen gibt, ist Google Play bei Weitem der weltweit größte und am häufigsten besuchte Store für Android-Apps. Google Play bietet eine einzelne Plattform zum Verteilen, für die Ankündigung, den Verkauf und die Analyse der Verkäufe einer Android-Anwendung.

Dieser Abschnitt behandelt Themen, die für Google Play spezifisch sind, z.B. die Registrierung, um ein Herausgeber zu werden, das Sammeln von Assets, um Google Play zu helfen, Ihre Anwendungen zu bewerben und anzukündigen, Richtlinien zur Bewertung Ihrer Anwendung in Google Play und Verwenden von Filtern zum Einschränken der Entwicklung einer Anwendungen für bestimmte Geräte.

<a name="Requirements"  />

## <a name="requirements"></a>Anforderungen

Um eine Anwendung über Google Play zu verteilen, muss ein Entwicklerkonto erstellt werden. Dieses muss nur einmal erstellt werden und kostet einmalig 25,00 USD.

Alle Anwendungen müssen mit einem kryptografischen Schlüssel signiert sein, der nach dem 22. Oktober 2033 abläuft.

Die maximale Größe eines über Google Play veröffentlichten Android-Anwendungspakets beträgt 100 MB. Wenn eine Anwendung diese Größe überschreitet, erlaubt Google Play, dass zusätzliche Objekte über *APK-Erweiterungsdateien* geliefert werden dürfen. Android-Erweiterungsdateien erlauben dem Android-Anwendungspaket zwei zusätzliche Dateien, und jede dieser Datei darf eine Größe von bis zu 2 GB besitzen. Google Play hostet und verteilt diese Dateien kostenlos. Erweiterungsdateien werden in einem anderen Abschnitt erläutert.

Google Play ist nicht global verfügbar. Einige Standorte werden für die Verteilung von Anwendungen möglicherweise nicht unterstützt.


<a name="Becoming_a_Publisher"  />

## <a name="becoming-a-publisher"></a>So werden Sie ein Herausgeber

Um Anwendungen in Google Play zu veröffentlichen, müssen Sie über ein Herausgeberkonto verfügen. Befolgen Sie folgende Schritte, um sich für ein Herausgeberkonto zu registrieren:

1.  Besuchen Sie die [Google Play-Entwicklerkonsole](https://play.google.com/apps/publish).
1.  Geben Sie die grundlegenden Informationen über Ihre Entwickleridentität an.
1.  Lesen und akzeptieren Sie das Developer Distribution Agreement (Vertriebsvereinbarung für Entwickler) für Ihr Gebietsschema.
1.  Zahlen Sie die Registrierungsgebühr von 25,00 USD.
1.  Bestätigen sie die Überprüfung via E-Mail.
1.  Nachdem das Konto erstellt wurde, können Sie Anwendungen mit Google Play veröffentlichen.


Google Play unterstützt nicht alle Länder der Welt. Die neueste Liste der unterstützten Länder finden Sie unter folgenden Links:

1.  [Unterstützte Registrierungsstandorte für Entwickler &amp; Händler](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; Dies ist eine Liste aller Länder, in denen sich Entwickler als Händler registrieren können und bezahlte Anwendungen verkaufen können.

1.  [Unterstützte Bereitstellungsstandorte für Google Play-Nutzer](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; Dies ist eine Liste aller Länder, in denen Anwendungen verteilt werden dürfen.


<a name="Preparing_Promotional_Assets"  />

### <a name="preparing-promotional-assets"></a>Vorbereiten von Werbeobjekten

Um eine Anwendung in Google Play effektiv hochzustufen und zu bewerben, erlaubt Google Entwicklern Werbeobjekte wie Screenshots, Grafiken und Videos zu übermitteln. Google Play verwendet diese Objekte dann, um die Anwendung zu bewerben und höher zu stufen.


<a name="Launcher_Icons"  />

#### <a name="launcher-icons"></a>Startprogrammsymbole

Ein *Startprogrammsymbol* ist eine Grafik, die eine Anwendung darstellt. Jedes Startprogrammsymbol sollte eine 32-Bit-PNG-Datei mit einem Alphakanal für die Transparenz sein. Eine Anwendung sollte über Symbole für alle verallgemeinerte Bildschirmdichten, wie in der Liste unten aufgegliedert, verfügen:

-   **ldpi** (120dpi) &ndash; 36 x 36 px
-   **mdpi** (160dpi) &ndash; 48 x 48 px
-   **hdpi** (240dpi) &ndash; 72 x 72 px
-   **xhdpi** (320dpi) &ndash; 96 x 96 px


Startprogrammsymbole sind das Erste, das Benutzer für Anwendungen in Google Play sehen, seien Sie also besonders sorgfältig, damit Startprogrammsymbole visuell ansprechend und aussagekräftig sind.

Tipps für Startprogrammsymbole:

1.  **Einfach und übersichtlich**&ndash; Startprogrammsymbole sollten einfach gehalten werden und übersichtlich sein. Das heißt, dass Sie den Namen der Anwendung aus dem Symbol ausschließen sollten. Einfachere Symbole sind leichter zu merken und können bei kleinerer Größe einfacher voneinander zu unterscheiden sein.

1.  **Symbole dürfen nicht dünn sein**&ndash; Sehr dünne Symbole heben sich nicht gut vom Hintergrund hervor.

1.  **Verwenden Sie einen Alphakanal**&ndash; Symbole sollten vom Alphakanal Gebrauch machen und keine Bilder im Vollbildformat sein.


<a name="High_Resolution_Application_Icon"  />

#### <a name="high-resolution-application-icons"></a>Hochauflösende Anwendungssymbole

Anwendungen in Google Play erfordern eine hochwertige Version des Anwendungssymbols. Diese wird nur von Google Play verwendet und ersetzt nicht das Startprogrammsymbol der Anwendung. Die Spezifikationen für das Symbol mit hoher Auflösung sind Folgende:

1.  32-Bit-PNG mit Alphakanal
1.  512 x 512 Pixel
1.  Maximale Größe von 1024 KB

Das [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) ist ein hilfreiches Tool zum Erstellen passender Startprogrammsymbole und des hochauflösenden Anwendungssymbols.


<a name="Screen_shots"  />

#### <a name="screen-shots"></a>Screenshots

Google Play erfordert mindestens zwei und maximal acht Screenshots für eine Anwendung. Diese werden auf der Detailseite der Anwendung in Google Play angezeigt.

Die Spezifikationen für Screenshots sind Folgende:

1.  24-Bit-PNG oder JPG ohne Alphakanal
1.  320 x 480 (B x H) oder 480 x 800 (B x H) oder 480 x 854 (B x H). Bilder im Querformat werden zugeschnitten.


<a name="Promotional_Graphic" />

#### <a name="promotional-graphic"></a>Werbegrafik

Die ist ein optionales Bild, das von Google Play verwendet wird:

1.  Es handelt sich um ein 24-Bit-PNG oder JPG mit den Maßen 180 x 120 (B x H) ohne Alphakanal.
1.  Werbegrafik ohne Kunstrahmen.


<a name="Feature_Graphic" />

#### <a name="feature-graphic"></a>Funktionsgrafik

Wird vom Funktionsabschnitt von Google Play verwendet. Dies Grafik kann allein und ohne Anwendungssymbol angezeigt werden.

1.  24-Bit-PNG oder JPG mit den Maßen 1024 x 500 (B x H) ohne Alphakanal und Transparenz.
1.  Alle wichtigen Inhalte sollten sich innerhalb eines Rahmens von 924 x 500 befinden. Pixel außerhalb dieses Rahmens werden möglicherweise für stilistische Zwecke zugeschnitten.
1.  Diese Grafik kann womöglich zentral herunterskaliert werden: Verwenden Sie viel Text, und halten Sie Grafiken einfach.


<a name="Video_Link" />

#### <a name="video-link"></a>Videoverknüpfungen

Hierbei handelt es sich um eine URL zu einem YouTube-Video, das die Anwendung veranschaulichen soll. Das Video sollte 30 Sekunden bis zu 2 Minuten lang sein und die Highlights Ihrer Anwendung darstellen.


<a name="pubgp" />

### <a name="publishing-to-google-play"></a>Veröffentlichung in Google Play

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In Xamarin.Android 7.0 wird ein integrierter Workflow für die Veröffentlichung von Apps in Google Play von Visual Studio eingeführt. Wen Sie eine Version von Xamarin.Android vor Version 7.0 verwenden, müssen Sie Ihr Android-Anwendungspaket manuell über die Google Play Developer-Konsole hochladen. Es muss auch mindestens ein APK bereits hochgeladen sein, bevor Sie den integrierten Workflow verwenden können. Wenn Sie Ihr erstes Android-Anwendungspaket noch nicht hochgeladen haben, müssen Sie dies manuell tun. Weitere Informationen finden Sie unter [Manually Uploading the APK (Manuelles Hochladen des Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

Unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) wurde erklärt, wie ein neues Zertifikat zum Signieren von Android-Apps erstellt wird. Der nächste Schritt besteht aus dem Veröffentlichen einer signierten App in Google Play:

1. Melden Sie sich in Ihrem Google Play Developer-Konto an, um ein neues Projekt zu erstellen, das mit Ihrem Google Play Developer-Konto verknüpft ist.
2. Erstellen Sie einen **OAuth-Client**, der Ihre App authentifiziert.
3. Geben Sie die daraus entstehende Client-ID sowie den geheimen Clientschlüssel in Visual Studio ein.
4. Registrieren Sie Ihr Konto in Visual Studio.
5. Signieren Sie die Anwendung mit Ihrem Zertifikat.
6. Veröffentlichen Sie Ihre signierte App in Google Play.

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) stellte das Dialogfeld **Verteilungskanal** zwei Möglichkeiten für die Verteilung dar: **Ad-hoc** und **Google Play**. Wenn das Dialogfeld **Signierungsidentität** stattdessen angezeigt wird, klicken Sie auf **Zurück**, um zum Dialogfeld **Verteilungskanal** zurückzukehren. Wählen Sie **Google Play**, und klicken Sie auf **Weiter**:

[ ![Dialogfeld „Verteilungskanal“](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png)

Wählen Sie im Dialogfeld **Signierungsidentität** die unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) erstellte Identität aus, und klicken Sie auf **Weiter**:

[ ![Dialogfeld „Signierungsidentität“](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png)

Klicken Sie im Dialogfeld **Google Play-Konten** auf die Schaltfläche **+**, um ein neues Google Play-Konto hinzuzufügen:

[ ![Dialogfeld „Google Play-Konten“](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png)

Im Dialogfeld **Google-API-Zugriff registrieren** müssen Sie die _Client-ID_ sowie den _geheimen Clientschlüssel_ bereitstellen, wodurch die API Zugriff auf Ihr Google Play Developer-Konto erhält:

[ ![Dialogfeld „Google-API-Zugriff registrieren“](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png)

Im nächsten Abschnitt wird erläutert, wie ein neues Google API-Projekt erstellt und die erforderliche _Client-ID_ sowie der _geheime Clientschlüssel_ generiert werden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Visual Studio für Mac umfasst einen integrierten Workflow für die Veröffentlichung von Apps in Google Play. Wenn Sie eine Xamarin Studi-Version vor 5.9 verwenden, müssen Sie Ihre Android-Anwendungspaket über die Google Play-Entwicklerkonsole manuell hochladen. Verwenden Sie anschließend das Dialogfeld **In Google Play veröffentlichen** für weiter APK-Updates. Es muss auch mindestens ein APK bereits hochgeladen sein, bevor Sie **In Google Play veröffentlichen** nutzen können. Wenn Sie Ihr erstes Android-Anwendungspaket noch nicht hochgeladen haben, müssen Sie dies manuell tun. Weitere Informationen zum manuellen Hochladen eines Android-Anwendungspakets finden Sie unter [Manually Uploading the APK (Manuelles Hochladen eines Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

Unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) wurde das Erstellen eines neuen Zertifikats für die Signierung von Android-Apps erläutert. Die folgenden Schritte gliedern auf, wie eine Xamarin.Android-App in Google Play veröffentlicht wird:

1. Melden Sie sich in Ihrem Google Play Developer-Konto an, um ein neues Projekt zu erstellen, das mit Ihrem Google Play Developer-Konto verknüpft ist.
2. Erstellen Sie einen _OAuth-Client_, der Ihre App authentifiziert.
3. Geben Sie die resultierende _Client-ID_ und den _geheimen Clientschlüssel_ in Visual Studio für Mac ein.
4. Registrieren Sie Ihr Konto in Visual Studio für Mac.
5. Melden Sie die Anwendung mit Ihrem Zertifikat an.
6. Veröffentlichen Sie Ihre signierte Anwendung in Google Play.

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) stellte das Dialogfeld **Signieren und verteilen...** zwei Möglichkeiten für die Verteilung dar. Wählen Sie **Google Play**, und klicken Sie auf **Weiter**:

[ ![Dialogfeld zur Auswahl der Android-Verteilung](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png)

Im Dialogfeld **Google Play API-Konto** müssen Sie die _Client-ID_ sowie den _geheimen Clientschlüssel_ bereitstellen, wodurch die API Zugriff auf Ihr Google Play Developer-Konto erhält:

[ ![Dialogfeld Google Play API-Konto](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png)

Im nächsten Abschnitt wird erläutert, wie ein neues Google API-Projekt erstellt und die erforderliche _Client-ID_ sowie der _geheime Clientschlüssel_ generiert werden.

-----


#### <a name="create-a-google-api-project"></a>Erstellen eines Google API-Projekts

Melden Sie sich zunächst bei Ihrem [Google Play Developer-Konto](https://play.google.com/apps/publish) an.
Wenn Sie nicht bereits über ein Google Play Developer-Konto verfügen, gehen Sie unter [Einführung](http://developer.android.com/distribute/googleplay/start.html).
Die Google Play Developer API [Getting Started (Erste Schritte)](https://developers.google.com/android-publisher/getting_started) erklärt auch, wie die Google Play Developer-API verwendet wird. Nachdem Sie sich in der Google Play Developer-Konsole angemeldet haben, klicken Sie auf **Einstellungen**:

[ ![Symbol „Einstellungen“](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png)

Wählen Sie auf der Seite **EINSTELLUNGEN** **API-Zugriff** aus, und klicken Sie auf die Schaltfläche **Neues Projekt erstellen**:

[ ![Schaltfläche „Neues Projekt erstellen“](images/02-create-new-project-sml.png)](images/02-create-new-project.png)

Nach etwa einer Minute wird das neue API-Projekt automatisch generiert und mit Ihrem Google Play Developer Console-Konto verknüpft.

Der nächste Schritt besteht aus der Erstellung eines OAuth-Clients für die App (falls dieser nicht schon erstellt wurde). Wenn Benutzer Zugriff auf Ihre privaten Daten über Ihre App anfordern, wird Ihre OAuth-Client-ID verwendet, um Ihre App zu identifizieren.
Klicken Sie auf **Create OAuth Client** (OAuth-Client erstellen), um einen neuen OAuth-Client zu erstellen.

[ ![Schaltfläche „OAuth-Client erstellen“](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png)

Nach wenigen Sekunden ist eine neue Client-ID generiert. Klicken Sie auf **View in Google Developers Console** (Auf der Google Developers Console anzeigen), um Ihre neue Client-ID in der Entwicklerkonsole von Google anzuzeigen:

[ ![Angezeigte Client-ID](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png)

Die Client-ID wird zusammen mit dem Namen und dem Erstellungsdatum angezeigt. Klicken Sie auf das Symbol **OAuth-Client bearbeiten**, um den geheimen Clientschlüssel für Ihre App anzuzeigen:

[ ![Anzeigen von App-Anmeldeinformationen](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png)

Der Standardname des OAuth-Clients ist *Google Play Android Developer*. Dieser kann in den Namen der Xamarin.Android-App oder einen anderen geeigneten Namen geändert werden. In diesem Beispiel wird der Name des OAuth-Clients in den Namen der App, **MyApp**, geändert:

[ ![Angezeigte Client-ID und Geheimnis](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png)

Klicken Sie auf **Speichern**, um die Änderungen zu speichern. Dadurch kehren Sie zur Seite **Anmeldeinformationen** zurück, auf der Sie die Anmeldeinformationen durch Klicken auf das **JSON herunterladen**-Symbol herunterladen können:

[ ![Symbol „JSON herunterladen“](images/07-download-json-sml.png)](images/07-download-json.png)

Diese JSON-Datei enthält die Client-iD und den geheimen Clientschlüssel, den Sie ausschneiden und in das Dialogfeld **Signieren und verteilen** im nächsten Schritt einfügen können.


#### <a name="register-google-api-access"></a>Google-API-Zugriff registrieren

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Verwenden Sie die Client-ID und den geheimen Clientschlüssel, um das Dialogfeld **Google Play API-Konto** in Visual Studio für Mac abzuschließen. Es ist möglich, das Konto mit einer Beschreibung zu versehen &ndash; dadurch wird ermöglicht, mehr als ein Google Play-Konto zu registrieren und zukünftige Android-Anwendungspakete in unterschiedliche Google Play-Konten hochzuladen. Kopieren Sie die Client-ID und den geheimen Clientschlüssel in dieses Dialogfeld, und klicken Sie auf **Registrieren**:

[ ![Dialogfeld „Google-API-Zugriff registrieren“](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png)

Ein Webbrowser wird geöffnet und fordert Sie auf, sich bei Ihrem Google Play Android Developer-Konto anzumelden (falls Sie nicht bereits angemeldet sind). Nach der Anmeldung wird die folgende Aufforderung im Webbrowser angezeigt.
Klicken Sie auf **Zulassen**, um die App zu autorisieren:

[ ![Dialogfeld zum Autorisieren der App](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png)

#### <a name="publish"></a>Veröffentlichen

Nachdem Sie auf **Zulassen** geklickt haben, meldet der Browser _Empfangener Überprüfungscode. Wird geschlossen..._, und die App wird der Liste der Google Play-Konten in Visual Studio hinzugefügt. Klicken Sie im Dialogfeld **Google Play-Konten** auf **Weiter**:

[ ![Zu den Google Play-Konten hinzugefügtes Konto](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png)

Als Nächstes wird das Dialogfeld **Google Play-Titel** angezeigt. Google Play bietet vier mögliche Versionen zum Hochladen Ihrer App:

* **Alpha**: Wird zum Hochladen einer sehr frühen Version der App zu einer kleinen Liste von Testern verwendet.
* **Beta**: Wird zum Hochladen einer frühen Version der App zu einer größeren Liste von Testern verwendet.
* **Rollout**: Erlaubt, dass ein prozentualer Anteil von Benutzern eine aktualisierte Version der App erhält. Dadurch ist es möglich, den Prozentsatz langsam von etwas 10 % der Benutzer auf 100 % zu erhöhen, während Sie Probleme bereinigen.
* **Production** (Produktion): Wählen Sie diesen Titel aus, wenn die App aus dem Google Play Store vollständig verteilt werden kann.

Wählen Sie aus, welcher Google Play-Track zum Hochladen der App verwendet werden soll, und klicken Sie auf **Hochladen**. Wenn Sie **Rollout** auswählen, stellen Sie sicher, dass sie einen Prozentwert eingeben:

[ ![Auswählen von Alpha, Beta, Rollout oder Production](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png)

Weiter Informationen zu Google Play-Tests und gestaffelten Rollouts finden Sie unter [Alpha-/Betatests einrichten](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Als Nächstes wird ein Dialogfeld angezeigt, in das Sie das Kennwort für das Signaturzertifikat eingeben.
Geben Sie ein Kennwort ein, und klicken Sie auf **OK**:

[ ![Dialogfeld „Signierungskennwort“](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png)

Als Nächstes zeigt der **Archiv-Manager** den Uploadfortschritt an.

[ ![Fortschritt des Android-Anwendungspaketuploads](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png)

Wenn der Upload abgeschlossen ist, wird der Abschlussstatus in der linken unteren Ecke von Visual Studio angezeigt:

[ ![Benachrichtigung für den Abschluss der Veröffentlichung des Projekts](images/vs/11-published-sml.png)](images/vs/11-published.png)


### <a name="troubleshooting"></a>Problembehandlung

Beachten Sie, dass ein Android-Anwendungspaket bereits an den Google Play Store gesendet worden sein muss, bevor **In Google Play veröffentlichen** funktioniert. Wenn ein Android-Anwendungspaket noch nicht hochgeladen wurde, zeigt der Veröffentlichungs-Assistent den folgenden Fehler im **Fehler**-Bereich an:

[ ![Google Play erfordert, dass das erste APK für die App manuell hochgeladen wird](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png)

Wenn dieser Fehler angezeigt wird, laden Sie manuell ein Android-Anwendungspaket (z.B. als Ad-hoc-Build) über die Google Play Developer Console hoch, und verwenden Sie das Dialogfeld **Verteilungskanal** für nachfolgende APK-Updates.  Weitere Informationen finden Sie unter [Manually Uploading the APK (Manuelles Hochladen des Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Der Versionscode des Android-Anwendungspakets muss sich mit jedem Upload ändern. Andernfalls wird folgender Fehler ausgegeben:

[ ![Ein APK mit Versionscode (1) wurde bereits hochgeladen](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png)

Um diesen Fehler zu beheben, erstellen Sie die App mit einer anderen Versionsnummer neu, und übermitteln Sie sie über das Dialogfeld **Verteilungskanal** erneut an Google Play.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Verwenden Sie die Client-ID und den geheimen Clientschlüssel, um das Dialogfeld **Google Play API-Konto** in Visual Studio für Mac abzuschließen. Es ist möglich, das Konto mit einer Beschreibung zu versehen &ndash; dadurch wird ermöglicht, mehr als ein Google Play-Konto zu registrieren und zukünftige Android-Anwendungspakete in unterschiedliche Google Play-Konten hochzuladen. Kopieren Sie die Client-ID und den geheimen Clientschlüssel in dieses Dialogfeld, und klicken Sie auf **Registrieren**:

[ ![Dialogfeld „Zugriff autorisieren“](images/xs/10-register-sml.png)](images/xs/10-register.png)

Wenn die Client-ID und der geheime Clientschlüssel akzeptiert werden wird die Meldung **Registrierung war erfolgreich** angezeigt. Klicken Sie auf **Weiter**:

[ ![Benachrichtigung „Registrierung war erfolgreich.“](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png)

Wählen Sie im Dialogfeld **Google Play-Konto** ein Google-Konto und eine Version zum Hochladen der Anwendung aus:

[ ![Dialogfeld zum Auswählen des Google-Kontos](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png)

Google Play bietet vier mögliche Versionen zum Hochladen Ihrer App:

-   **Alpha**: Wird zum Hochladen einer sehr frühen Version der App zu einer kleinen Liste von Testern verwendet.

-   **Beta**: Wird zum Hochladen einer frühen Version der App zu einer größeren Liste von Testern verwendet.

-   **Rollout**: Erlaubt, dass ein prozentualer Anteil von Benutzern eine aktualisierte Version der App erhält. Dadurch ist es möglich, den Prozentsatz langsam von etwas 10 % der Benutzer auf 100 % zu erhöhen, während Sie Probleme bereinigen.

-   **Production** (Produktion): Wählen Sie diesen Titel aus, wenn die App aus dem Google Play Store vollständig verteilt werden kann.

Weiter Informationen zu Google Play-Tests und gestaffelten Rollouts finden Sie unter [Alpha-/Betatests einrichten](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Wählen Sie als Nächstes eine Signierungsidentität, die zum Signieren der App verwendet wird.
Wählen Sie **Vorhandenen Schlüssel verwenden** aus, um eine vorhandene Signierungsidentität zu verwenden. Lesen Sie alternativ den Leitfaden [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert), um Informationen zum Erstellen eines neuen Schlüssels zu erhalten. Wenn Sie ein Zertifikat zur Signierung der Anwendung ausgewählt haben, klicken Sie auf **Weiter**:

[ ![Dialogfeld „Android-Signierungsidentität“](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png)

Zu diesem Zeitpunkt kann die App in Google Play hochgeladen werden. Das Dialogfeld **In Google Play veröffentlichen** fasst die Informationen über Ihre App zusammen &ndash; klicken Sie auf **Veröffentlichen**, um Ihre App in Google Play zu veröffentlichen:

[ ![Dialogfeld „In Google Play veröffentlichen“](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png)

Beachten Sie, dass ein Android-Anwendungspaket bereits an den Google Play Store gesendet worden sein muss, bevor **In Google Play veröffentlichen** funktioniert. Wenn ein Android-Anwendungspaket nicht hochgeladen wird, kann folgender Fehler auftreten:

> _Google Play erfordert, dass das erste APK für die App manuell hochgeladen wird. Zu diesem Zweck können Sie ein Ad-hoc-APK verwenden._

oder

> _No application was found for the given package name. [404]_ (Für den angegebenen Paketnamen konnte keine Anwendung gefunden werden. [404])

Um diesen Fehler zu beheben, laden Sie manuell ein Android-Anwendungspaket (z.B. als Ad-hoc-Build) über die Google Play Developer Console hoch, und verwenden Sie das Dialogfeld **In Google Play veröffentlichen** für nachfolgende APK-Updates. Weitere Informationen zum manuellen Hochladen eines Android-Anwendungspakets finden Sie unter [Manually Uploading the APK (Manuelles Hochladen eines Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
