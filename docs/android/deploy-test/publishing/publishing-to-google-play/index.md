---
title: Veröffentlichung in Google Play
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 04e83fc68218216fe36cce67e43b83e8ad8feaa5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "80070908"
---
# <a name="publishing-to-google-play"></a>Veröffentlichung in Google Play

Obwohl es eine Reihe von App-Märkten für das Vertreiben von Anwendungen gibt, ist Google Play bei Weitem der weltweit größte und am häufigsten besuchte Store für Android-Apps. Google Play bietet eine einzelne Plattform zum Verteilen, für die Ankündigung, den Verkauf und die Analyse der Verkäufe einer Android-Anwendung.

Dieser Abschnitt behandelt Themen, die für Google Play spezifisch sind, z.B. die Registrierung, um ein Herausgeber zu werden, das Sammeln von Assets, um Google Play zu helfen, Ihre Anwendungen zu bewerben und anzukündigen, Richtlinien zur Bewertung Ihrer Anwendung in Google Play und Verwenden von Filtern zum Einschränken der Entwicklung einer Anwendungen für bestimmte Geräte.

## <a name="requirements"></a>Anforderungen

Um eine Anwendung über Google Play zu verteilen, muss ein Entwicklerkonto erstellt werden. Dieses muss nur einmal erstellt werden und kostet einmalig 25,00 USD.

Alle Anwendungen müssen mit einem kryptografischen Schlüssel signiert sein, der nach dem 22. Oktober 2033 abläuft.

Die maximale Größe eines über Google Play veröffentlichten Android-Anwendungspakets beträgt 100 MB. Wenn eine Anwendung diese Größe überschreitet, erlaubt Google Play, dass zusätzliche Objekte über *APK-Erweiterungsdateien* geliefert werden dürfen. Android-Erweiterungsdateien erlauben dem Android-Anwendungspaket zwei zusätzliche Dateien, und jede dieser Datei darf eine Größe von bis zu 2 GB besitzen. Google Play hostet und verteilt diese Dateien kostenlos. Erweiterungsdateien werden in einem anderen Abschnitt erläutert.

Google Play ist nicht global verfügbar. Einige Standorte werden für die Verteilung von Anwendungen möglicherweise nicht unterstützt.

## <a name="becoming-a-publisher"></a>So werden Sie ein Herausgeber

Um Anwendungen in Google Play zu veröffentlichen, müssen Sie über ein Herausgeberkonto verfügen. Befolgen Sie folgende Schritte, um sich für ein Herausgeberkonto zu registrieren:

1. Besuchen Sie die [Google Play-Entwicklerkonsole](https://play.google.com/apps/publish).
1. Geben Sie die grundlegenden Informationen über Ihre Entwickleridentität an.
1. Lesen und akzeptieren Sie das Developer Distribution Agreement (Vertriebsvereinbarung für Entwickler) für Ihr Gebietsschema.
1. Zahlen Sie die Registrierungsgebühr von 25,00 USD.
1. Bestätigen sie die Überprüfung via E-Mail.
1. Nachdem das Konto erstellt wurde, können Sie Anwendungen mit Google Play veröffentlichen.

Google Play unterstützt nicht alle Länder der Welt. Die neueste Liste der unterstützten Länder finden Sie unter den folgenden Links:

1. [Unterstützte Registrierungsstandorte für Entwickler &amp; Händler](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) &ndash; Dies ist eine Liste aller Länder/Regionen, in denen sich Entwickler als Händler registrieren können und bezahlte Anwendungen verkaufen können.

1. [Unterstützte Bereitstellungsstandorte für Google Play-Nutzer](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) &ndash; Dies ist eine Liste aller Länder/Regionen, in denen Anwendungen verteilt werden dürfen.

### <a name="preparing-promotional-assets"></a>Vorbereiten von Werbeobjekten

Um eine Anwendung in Google Play effektiv hochzustufen und zu bewerben, erlaubt Google Entwicklern Werbeobjekte wie Screenshots, Grafiken und Videos zu übermitteln. Google Play verwendet diese Objekte dann, um die Anwendung zu bewerben und höher zu stufen.

#### <a name="launcher-icons"></a>Startprogrammsymbole

Ein *Startprogrammsymbol* ist eine Grafik, die eine Anwendung darstellt. Jedes Startprogrammsymbol sollte eine 32-Bit-PNG-Datei mit einem Alphakanal für die Transparenz sein. Eine Anwendung sollte über Symbole für alle verallgemeinerte Bildschirmdichten, wie in der Liste unten aufgegliedert, verfügen:

- **ldpi** (120dpi) &ndash; 36 x 36 px
- **mdpi** (160dpi) &ndash; 48 x 48 px
- **hdpi** (240dpi) &ndash; 72 x 72 px
- **xhdpi** (320dpi) &ndash; 96 x 96 px

Startprogrammsymbole sind das Erste, das Benutzer für Anwendungen in Google Play sehen, seien Sie also besonders sorgfältig, damit Startprogrammsymbole visuell ansprechend und aussagekräftig sind.

Tipps für Startprogrammsymbole:

1. **Einfach und übersichtlich**&ndash; Startprogrammsymbole sollten einfach gehalten werden und übersichtlich sein. Das heißt, dass Sie den Namen der Anwendung aus dem Symbol ausschließen sollten. Einfachere Symbole sind leichter zu merken und können bei kleinerer Größe einfacher voneinander zu unterscheiden sein.

1. **Symbole dürfen nicht dünn sein**&ndash; Sehr dünne Symbole heben sich nicht gut von allen Hintergründen ab.

1. **Verwenden Sie einen Alphakanal**&ndash; Symbole sollten vom Alphakanal Gebrauch machen und keine Bilder im Vollbildformat sein.

#### <a name="high-resolution-application-icons"></a>Hochauflösende Anwendungssymbole

Anwendungen in Google Play erfordern eine hochwertige Version des Anwendungssymbols. Diese wird nur von Google Play verwendet und ersetzt nicht das Startprogrammsymbol der Anwendung. Die Spezifikationen für das Symbol mit hoher Auflösung sind Folgende:

1. 32-Bit-PNG mit Alphakanal
1. 512 x 512 Pixel
1. Maximale Größe von 1024 KB

Das [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) ist ein hilfreiches Tool zum Erstellen passender Startprogrammsymbole und des hochauflösenden Anwendungssymbols.

#### <a name="screenshots"></a>Screenshots

Für Google Play müssen zwischen zwei und acht Screenshots pro Anwendung vorhanden sein. Diese werden auf der Detailseite der Anwendung in Google Play angezeigt.

Es gelten die folgenden Spezifikationen für Screenshots:

1. 24-Bit-PNG oder JPG ohne Alphakanal
1. 320 x 480 (B x H) oder 480 x 800 (B x H) oder 480 x 854 (B x H). Bilder im Querformat werden zugeschnitten.

#### <a name="promotional-graphic"></a>Werbegrafik

Die ist ein optionales Bild, das von Google Play verwendet wird:

1. Es handelt sich um ein 24-Bit-PNG oder JPG mit den Maßen 180 x 120 (B x H) ohne Alphakanal.
1. Werbegrafik ohne Kunstrahmen.

#### <a name="feature-graphic"></a>Funktionsgrafik

Wird vom Funktionsabschnitt von Google Play verwendet. Dies Grafik kann allein und ohne Anwendungssymbol angezeigt werden.

1. 24-Bit-PNG oder JPG mit den Maßen 1024 x 500 (B x H) ohne Alphakanal und Transparenz.
1. Alle wichtigen Inhalte sollten sich innerhalb eines Rahmens von 924 x 500 befinden. Pixel außerhalb dieses Rahmens werden möglicherweise für stilistische Zwecke zugeschnitten.
1. Diese Grafik kann womöglich zentral herunterskaliert werden: Verwenden Sie viel Text, und halten Sie Grafiken einfach.

#### <a name="video-link"></a>Videoverknüpfungen

Hierbei handelt es sich um eine URL zu einem YouTube-Video, das die Anwendung veranschaulichen soll. Das Video sollte 30 Sekunden bis zu 2 Minuten lang sein und die Highlights Ihrer Anwendung darstellen.

### <a name="publishing-to-google-play"></a>Veröffentlichung in Google Play

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

In Xamarin.Android 7.0 wird ein integrierter Workflow für die Veröffentlichung von Apps in Google Play von Visual Studio eingeführt. Wen Sie eine Version von Xamarin.Android vor Version 7.0 verwenden, müssen Sie Ihr Android-Anwendungspaket manuell über die Google Play Developer-Konsole hochladen. Es muss auch mindestens ein APK bereits hochgeladen sein, bevor Sie den integrierten Workflow verwenden können. Wenn Sie Ihr erstes Android-Anwendungspaket noch nicht hochgeladen haben, müssen Sie dies manuell tun. Weitere Informationen finden Sie unter [Manually Uploading the APK (Manuelles Hochladen des Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

Unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) wurde erklärt, wie ein neues Zertifikat zum Signieren von Android-Apps erstellt wird. Der nächste Schritt besteht aus dem Veröffentlichen einer signierten App in Google Play:

1. Melden Sie sich in Ihrem Google Play Developer-Konto an, um ein neues Projekt zu erstellen, das mit Ihrem Google Play Developer-Konto verknüpft ist.
2. Erstellen Sie einen **OAuth-Client**, der Ihre App authentifiziert.
3. Geben Sie die daraus entstehende Client-ID sowie den geheimen Clientschlüssel in Visual Studio ein.
4. Registrieren Sie Ihr Konto in Visual Studio.
5. Signieren Sie die Anwendung mit Ihrem Zertifikat.
6. Veröffentlichen Sie Ihre signierte App in Google Play.

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) standen im Dialogfeld **Verteilungskanal** zwei Möglichkeiten für die Verteilung zur Auswahl: **Ad-hoc** und **Google Play**. Wenn das Dialogfeld **Signierungsidentität** stattdessen angezeigt wird, klicken Sie auf **Zurück**, um zum Dialogfeld **Verteilungskanal** zurückzukehren. **Google Play** auswählen:

[![Dialogfeld „Verteilungskanal“](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

Wählen Sie im Dialogfeld **Signierungsidentität** die unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) erstellte Identität aus, und klicken Sie auf **Weiter**:

[![Dialogfeld „Signierungsidentität“](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

Klicken Sie im Dialogfeld **Google Play-Konten** auf die Schaltfläche **+** , um ein neues Google Play-Konto hinzuzufügen:

[![Dialogfeld „Google Play-Konten“](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

Im Dialogfeld **Google-API-Zugriff registrieren** müssen Sie die _Client-ID_ sowie den _geheimen Clientschlüssel_ bereitstellen, wodurch die API Zugriff auf Ihr Google Play Developer-Konto erhält:

[![Dialogfeld „Google-API-Zugriff registrieren“](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

Im nächsten Abschnitt wird erläutert, wie ein neues Google API-Projekt erstellt und die erforderliche _Client-ID_ sowie der _geheime Clientschlüssel_ generiert werden.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac umfasst einen integrierten Workflow für die Veröffentlichung von Apps in Google Play.

Unter [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert) wurde das Erstellen eines neuen Zertifikats für die Signierung von Android-Apps erläutert. Die folgenden Schritte gliedern auf, wie eine Xamarin.Android-App in Google Play veröffentlicht wird:

1. Melden Sie sich in Ihrem Google Play Developer-Konto an, um ein neues Projekt zu erstellen, das mit Ihrem Google Play Developer-Konto verknüpft ist.
2. Erstellen Sie einen _OAuth-Client_, der Ihre App authentifiziert.
3. Geben Sie die resultierende _Client-ID_ und den _geheimen Clientschlüssel_ in Visual Studio für Mac ein.
4. Registrieren Sie Ihr Konto in Visual Studio für Mac.
5. Melden Sie die Anwendung mit Ihrem Zertifikat an.
6. Veröffentlichen Sie Ihre signierte Anwendung in Google Play.

Unter [Zur Veröffentlichung aktivieren](~/android/deploy-test/release-prep/index.md#archive) stellte das Dialogfeld **Signieren und verteilen...** zwei Möglichkeiten für die Verteilung dar. Wählen Sie **Google Play**, und klicken Sie auf **Weiter**:

[![Dialogfeld zur Auswahl der Android-Verteilung](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

Im Dialogfeld **Google Play API-Konto** müssen Sie die _Client-ID_ sowie den _geheimen Clientschlüssel_ bereitstellen, wodurch die API Zugriff auf Ihr Google Play Developer-Konto erhält:

[![Dialogfeld Google Play API-Konto](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

Im nächsten Abschnitt wird erläutert, wie ein neues Google API-Projekt erstellt und die erforderliche _Client-ID_ sowie der _geheime Clientschlüssel_ generiert werden.

-----

#### <a name="create-a-google-api-project"></a>Erstellen eines Google API-Projekts

Melden Sie sich zunächst bei Ihrem [Google Play Developer-Konto](https://play.google.com/apps/publish) an.
Wenn Sie nicht bereits über ein Google Play Developer-Konto verfügen, gehen Sie unter [Einführung](https://developer.android.com/distribute/googleplay/start.html).
Die Google Play Developer API [Getting Started (Erste Schritte)](https://developers.google.com/android-publisher/getting_started) erklärt auch, wie die Google Play Developer-API verwendet wird. Nachdem Sie sich bei der Google Play Developer Console angemeldet haben, klicken Sie auf **Anwendung erstellen**:

[![Schaltfläche „Neues Projekt erstellen“](images/01-create-new-project-sml.png)](images/01-create-new-project.png#lightbox)

Nachdem das neue Projekt erstellt wurde, wird es mit Ihrem Google Play Developer Console-Konto verknüpft.

Der nächste Schritt besteht aus der Erstellung eines OAuth-Clients für die App (falls dieser nicht schon erstellt wurde). Wenn Benutzer Zugriff auf Ihre privaten Daten über Ihre App anfordern, wird Ihre OAuth-Client-ID verwendet, um Ihre App zu identifizieren.

Navigieren Sie zur Seite **Einstellungen**.

[![Symbol „Einstellungen“](images/02-google-play-developer-console-sml.png)](images/02-google-play-developer-console.png#lightbox)

Klicken Sie auf der Seite **Einstellungen** erst auf **API-Zugriff** und dann auf **CREATE OAUTH CLIENT** (OAuth-Client erstellen), um einen neuen OAuth-Client zu erstellen:

[![Schaltfläche „OAuth-Client erstellen“](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

Nach wenigen Sekunden ist eine neue Client-ID generiert. Klicken Sie auf **View in Google Developers Console** (Auf der Google Developers Console anzeigen), um Ihre neue Client-ID in der Entwicklerkonsole von Google anzuzeigen:

[![Angezeigte Client-ID](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

Die Client-ID wird zusammen mit dem Namen und dem Erstellungsdatum angezeigt. Klicken Sie auf das Symbol **OAuth-Client bearbeiten**, um den geheimen Clientschlüssel für Ihre App anzuzeigen:

[![Anzeigen von App-Anmeldeinformationen](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

Der Standardname des OAuth-Clients ist *Google Play Android Developer*. Dieser kann in den Namen der Xamarin.Android-App oder einen anderen geeigneten Namen geändert werden. In diesem Beispiel wird der Name des OAuth-Clients in den Namen der App, **MyApp**, geändert:

[![Angezeigte Client-ID und Geheimnis](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

Klicken Sie zum Speichern der Änderungen auf **Speichern**. Dadurch kehren Sie zur Seite **Anmeldeinformationen** zurück, auf der Sie die Anmeldeinformationen durch Klicken auf das Symbol für die Option **JSON herunterladen** herunterladen können:

[![Symbol „JSON herunterladen“](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

Diese JSON-Datei enthält die Client-iD und den geheimen Clientschlüssel, den Sie ausschneiden und in das Dialogfeld **Signieren und verteilen** im nächsten Schritt einfügen können.

#### <a name="register-google-api-access"></a>Google-API-Zugriff registrieren

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Verwenden Sie die Client-ID und den geheimen Clientschlüssel, um das Dialogfeld **Google Play API-Konto** in Visual Studio für Mac abzuschließen. Es ist möglich, das Konto mit einer Beschreibung zu versehen &ndash; dadurch wird ermöglicht, mehr als ein Google Play-Konto zu registrieren und zukünftige Android-Anwendungspakete in unterschiedliche Google Play-Konten hochzuladen. Kopieren Sie die Client-ID und den geheimen Clientschlüssel in dieses Dialogfeld, und klicken Sie auf **Registrieren**:

[![Dialogfeld „Google-API-Zugriff registrieren“](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Ein Webbrowser wird geöffnet und fordert Sie auf, sich bei Ihrem Google Play Android Developer-Konto anzumelden (falls Sie nicht bereits angemeldet sind). Nach der Anmeldung wird die folgende Aufforderung im Webbrowser angezeigt.
Klicken Sie auf **Zulassen**, um die App zu autorisieren:

[![Dialogfeld zum Autorisieren der App](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>Veröffentlichen

Nachdem Sie auf **Zulassen** geklickt haben, meldet der Browser _Empfangener Überprüfungscode. Wird geschlossen..._ , und die App wird der Liste der Google Play-Konten in Visual Studio hinzugefügt. Klicken Sie im Dialogfeld **Google Play-Konten** auf **Weiter**:

[![Zu den Google Play-Konten hinzugefügtes Konto](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

Als Nächstes wird das Dialogfeld **Google Play-Titel** angezeigt. Google Play bietet vier Tracks zum Hochladen Ihrer App:

- **Intern:** Dieser Track wird für die schnelle Verteilung Ihrer App für interne Tests und Qualitätssicherungsüberprüfungen verwendet.
- **Alpha:** Dieser Track wird zum Hochladen einer frühen Version der App für wenige Tester verwendet.
- **Beta**: Dieser Track wird zum Hochladen einer frühen Version der App für mehrere Tester verwendet.
- **Produktion:** Dieser Track wird für die vollständige Verteilung im Google Play Store verwendet.
- **Benutzerdefiniert:** Dieser Track wird zum Testen von Vorabversionen von Apps mit bestimmten Benutzern verwendet, indem eine Liste mit den E-Mail-Adressen von Testern erstellt wird.

Wählen Sie aus, welcher Google Play-Track zum Hochladen der App verwendet werden soll, und klicken Sie auf **Hochladen**.

[![Zu den Google Play-Konten hinzugefügtes Konto](images/vs/08-google-play-track-sml.png)](images/vs/07-account-added.png#lightbox)

Weitere Informationen zu Tests für Google Play finden Sie unter [Offenen, geschlossenen oder internen Test einrichten](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Als Nächstes wird ein Dialogfeld angezeigt, in das Sie das Kennwort für das Signaturzertifikat eingeben.
Geben Sie ein Kennwort ein, und klicken Sie auf **OK**:

[![Dialogfeld „Signierungskennwort“](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

Als Nächstes zeigt der **Archiv-Manager** den Uploadfortschritt an.

[![Fortschritt des Android-Anwendungspaketuploads](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

Wenn der Upload abgeschlossen ist, wird der Abschlussstatus in der linken unteren Ecke von Visual Studio angezeigt:

[![Benachrichtigung für den Abschluss der Veröffentlichung des Projekts](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)

### <a name="troubleshooting"></a>Problembehandlung

Wenn bei der Auswahl eines Tracks für Google Play Ihr benutzerdefinierter Track nicht angezeigt wird, überprüfen Sie, ob Sie in der Google Play Developer Console ein Release für diesen Track erstellt haben. Anweisungen zum Erstellen eines Release finden Sie unter [Releases vorbereiten und einführen](https://support.google.com/googleplay/android-developer/answer/7159011?hl=en).

Beachten Sie, dass ein Android-Anwendungspaket bereits an den Google Play Store gesendet worden sein muss, bevor **In Google Play veröffentlichen** funktioniert. Wenn ein Android-Anwendungspaket noch nicht hochgeladen wurde, zeigt der Veröffentlichungs-Assistent den folgenden Fehler im **Fehler**-Bereich an:

[![Google Play erfordert, dass das erste APK für die App manuell hochgeladen wird](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

Wenn dieser Fehler angezeigt wird, laden Sie manuell ein Android-Anwendungspaket (z. B. als Ad-hoc-Build) über die Google Play Developer Console hoch, und verwenden Sie das Dialogfeld **Verteilungskanal** für nachfolgende APK-Updates.  Weitere Informationen finden Sie unter [Manually Uploading the APK (Manuelles Hochladen des Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md). Der Versionscode des Android-Anwendungspakets muss sich mit jedem Upload ändern. Andernfalls wird folgender Fehler ausgegeben:

[![Ein APK mit Versionscode (1) wurde bereits hochgeladen](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

Um diesen Fehler zu beheben, erstellen Sie die App mit einer anderen Versionsnummer neu, und übermitteln Sie sie über das Dialogfeld **Verteilungskanal** erneut an Google Play.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Verwenden Sie die Client-ID und den geheimen Clientschlüssel, um das Dialogfeld **Google Play API-Konto** in Visual Studio für Mac abzuschließen. Es ist möglich, das Konto mit einer Beschreibung zu versehen &ndash; dadurch wird ermöglicht, mehr als ein Google Play-Konto zu registrieren und zukünftige Android-Anwendungspakete in unterschiedliche Google Play-Konten hochzuladen. Kopieren Sie die Client-ID und den geheimen Clientschlüssel in dieses Dialogfeld, und klicken Sie auf **Registrieren**:

[![Dialogfeld „Zugriff autorisieren“](images/xs/03-register-sml.png)](images/xs/03-register.png#lightbox)

Wenn die Client-ID und der geheime Clientschlüssel akzeptiert werden wird die Meldung **Registrierung war erfolgreich** angezeigt. Klicken Sie auf **Weiter**:

[![Benachrichtigung „Registrierung war erfolgreich.“](images/xs/04-registration-successful-sml.png)](images/xs/04-registration-successful.png#lightbox)

Wählen Sie im Dialogfeld **Google Play-Konto** ein Google-Konto und eine Version zum Hochladen der Anwendung aus:

[![Dialogfeld zum Auswählen des Google-Kontos](images/xs/05-choose-google-account-sml.png)](images/xs/05-choose-google-account.png#lightbox)

Google Play bietet vier Tracks zum Hochladen Ihrer App:

- **Intern:** Dieser Track wird für die schnelle Verteilung Ihrer App für interne Tests und Qualitätssicherungsüberprüfungen verwendet.
- **Alpha:** Dieser Track wird zum Hochladen einer frühen Version der App für wenige Tester verwendet.
- **Beta**: Dieser Track wird zum Hochladen einer frühen Version der App für mehrere Tester verwendet.
- **Produktion:** Dieser Track wird für die vollständige Verteilung im Google Play Store verwendet.
- **Benutzerdefiniert:** Dieser Track wird zum Testen von Vorabversionen von Apps mit bestimmten Benutzern verwendet, indem eine Liste mit den E-Mail-Adressen von Testern erstellt wird.

Weitere Informationen zu Google Play-Tests finden Sie unter [Offenen, geschlossenen oder internen Test einrichten](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en).

Wählen Sie als Nächstes eine Signierungsidentität, die zum Signieren der App verwendet wird.
Wählen Sie **Vorhandenen Schlüssel verwenden** aus, um eine vorhandene Signierungsidentität zu verwenden. Lesen Sie alternativ den Leitfaden [Creating a New Certificate (Erstellen eines neuen Zertifikats)](~/android/deploy-test/signing/index.md#newcert), um Informationen zum Erstellen eines neuen Schlüssels zu erhalten. Wenn Sie ein Zertifikat zur Signierung der Anwendung ausgewählt haben, klicken Sie auf **Weiter**:

[![Dialogfeld „Android-Signierungsidentität“](images/xs/06-android-signing-identity-sml.png)](images/xs/06-android-signing-identity.png#lightbox)

Zu diesem Zeitpunkt kann die App in Google Play hochgeladen werden. Das Dialogfeld **In Google Play veröffentlichen** fasst die Informationen über Ihre App zusammen &ndash; klicken Sie auf **Veröffentlichen**, um Ihre App in Google Play zu veröffentlichen:

[![Dialogfeld „In Google Play veröffentlichen“](images/xs/07-publish-to-google-play-sml.png)](images/xs/07-publish-to-google-play.png#lightbox)

### <a name="troubleshooting"></a>Problembehandlung

Wenn bei der Auswahl eines Tracks für Google Play zum Hochladen Ihrer App Ihr benutzerdefinierter Track nicht angezeigt wird, überprüfen Sie, ob Sie in der Google Play Developer Console ein Release für diesen Track erstellt haben. Anweisungen zum Erstellen eines Release finden Sie unter [Releases vorbereiten und einführen](https://support.google.com/googleplay/android-developer/answer/7159011?hl=en).

Beachten Sie, dass ein Android-Anwendungspaket bereits an den Google Play Store gesendet worden sein muss, bevor **In Google Play veröffentlichen** funktioniert. Wenn ein Android-Anwendungspaket nicht hochgeladen wird, kann folgender Fehler auftreten:

> _Google Play erfordert, dass das erste APK für die App manuell hochgeladen wird. Zu diesem Zweck können Sie ein Ad-hoc-APK verwenden._

oder

> _No application was found for the given package name. [404]_ (Für den angegebenen Paketnamen konnte keine Anwendung gefunden werden. [404])

Sie können diesen Fehler beheben, indem Sie manuell ein Android-Anwendungspaket (z. B. als Ad-hoc-Build) über die Google Play Developer Console hochladen und das Dialogfeld **In Google Play veröffentlichen** für nachfolgende APK-Updates verwenden. Weitere Informationen zum manuellen Hochladen eines Android-Anwendungspakets finden Sie unter [Manually Uploading the APK (Manuelles Hochladen eines Android-Anwendungspakets)](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md).

-----
