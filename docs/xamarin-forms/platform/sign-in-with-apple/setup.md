---
Title: "Setup Steps-Sign in with Apple for Xamarin.Forms " Description: "Sign in with Apple Setup unterscheidet sich von den verschiedenen Plattformen, auf die Ihre Mobile Anwendung abzielt."
ms. Prod: xamarin ms. assetid: 8F 712802-395b-469b-b5be-c927ad1a8391 ms. Technology: xamarin-Forms Author: davidortinau ms. Author: daortin ms. Date: 09/10/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="setup-sign-in-with-apple-for-xamarinforms"></a>Setup bei Apple für anmeldenXamarin.Forms

In dieser Anleitung werden die Schritte beschrieben, die zum Einrichten Ihrer plattformübergreifenden Anwendungen erforderlich sind, um die erweiterte Anmeldung mit Apple zu nutzen. Während das Apple-Setup direkt im Apple-Entwickler Portal ausgeführt wird, sind zusätzliche Schritte erforderlich, um eine sichere Beziehung zwischen Android und Apple herzustellen. 

## <a name="apple-developer-setup"></a>Apple-Entwickler-Setup

Bevor Sie die Anmeldung mit Apple in Ihren Anwendungen verwenden können, müssen Sie einige Setup Schritte im Abschnitt [Zertifikate, Bezeichner & profile](https://developer.apple.com/account/resources/) des Apple-Entwickler Portals durchführen.

### <a name="apple-sign-in-domain"></a>Apple-Anmelde Domäne

Registrieren Sie Ihren Domänen Namen, und überprüfen Sie ihn mit Apple im Abschnitt [Weitere](https://developer.apple.com/account/resources/services/list) im Abschnitt *Zertifikate, Bezeichner & profile* .

![Weitere Abschnitte](sign-in-images/readme-signin-domain-configure.png)

Fügen Sie Ihre Domäne hinzu, und klicken Sie auf **registrieren**.

![Domänen Formular registrieren](sign-in-images/readme-signin-domain-more.png)

> [!NOTE]
> Wenn eine Fehlermeldung angezeigt wird, dass Ihre Domäne nicht SPF-kompatibel ist, müssen Sie Ihrer Domäne einen SPF DNS TXT-Datensatz hinzufügen und darauf warten, dass Sie verteilt wird, bevor Sie fortfahren: die SPF txt könnte etwa wie folgt aussehen:`v=spf1 a a:myapp.com -all`

Als nächstes müssen Sie den Besitz der Domäne überprüfen, indem Sie auf **herunterladen** klicken, um die `apple-developer-domain-association.txt` Datei abzurufen, und Sie dann in den `.well-known` Ordner der Website Ihrer Domäne hochladen.

Nachdem die `.well-known/apple-developer-domain-association.txt` Datei hochgeladen und erreichbar ist, können Sie auf " **überprüfen** " klicken, damit Apple Ihren Domänen Besitz überprüft.

> [!NOTE]
> Apple überprüft den Besitz mit `https://` . Stellen Sie sicher, dass Sie über SSL-Setup verfügen und die Datei über eine sichere URL zugänglich ist.

Dieser Vorgang wird erfolgreich abgeschlossen, bevor Sie fortfahren.

## <a name="setup-your-app-id"></a>Einrichten der APP-ID

Erstellen Sie [im Abschnitt](https://developer.apple.com/account/resources/identifiers/list) Bezeichner einen neuen Bezeichner, und wählen Sie **App-IDs**aus. Wenn Sie bereits über eine APP-ID verfügen, wählen Sie stattdessen aus, Sie zu bearbeiten.

![Erstellen Sie eine neue APP-ID.](sign-in-images/readme-appid-create.png)

Aktivieren Sie die **Anmeldung bei Apple**. Wahrscheinlich möchten Sie auch die Option **als primäre APP-ID aktivieren** verwenden.

![Aktivieren der Anmeldung mit Apple](sign-in-images/readme-appid-signin.png)

Speichern Sie Ihre APP-ID-Änderungen.

## <a name="create-a-service-id"></a>Erstellen einer Dienst-ID

Erstellen Sie im Abschnitt [Identifiers](https://developer.apple.com/account/resources/identifiers/list/serviceId) einen neuen Bezeichner, und wählen Sie **Dienst-IDs**aus.

![Erstellen einer neuen Dienst-ID](sign-in-images/readme-serviceid-create.png)

Vergeben Sie eine Beschreibung und einen Bezeichner für Ihre Dienst-ID.  Dabei handelt es sich um den Bezeichner `ServerId` .  Stellen Sie sicher, dass Sie die **Anmeldung mit Apple**aktivieren.

Bevor Sie fortfahren, klicken Sie neben der von Ihnen aktivierten Option _bei Apple anmelden_ auf **Konfigurieren** .

Stellen Sie im Konfigurations Panel sicher, dass die richtige **primäre APP-ID** ausgewählt ist.

Wählen Sie als nächstes die **Webdomäne** aus, die Sie zuvor konfiguriert haben.

Fügen Sie abschließend mindestens eine **Rückgabe-URL**hinzu.  Alle, die `redirect_uri` Sie später verwenden, müssen hier genauso registriert werden, wie Sie Sie verwenden.  Stellen Sie sicher, dass Sie `http://` oder `https://` in die URL einschließen, wenn Sie Sie eingeben.

> [!NOTE]
> Zu Testzwecken können Sie nicht `127.0.0.1` oder verwenden `localhost` , aber Sie können auch andere Domänen verwenden, z `local.test` . b..  Wenn Sie sich dafür entscheiden, können Sie die Datei Ihres Computers bearbeiten, `hosts` um diese fiktive Domäne in Ihre lokale IP-Adresse aufzulösen.

![Konfigurieren Ihrer Apple-Anmeldung](sign-in-images/readme-serviceid-configure.png)

Speichern Sie die Änderungen, wenn Sie fertig sind.

## <a name="create-a-key-for-your-services-id"></a>Erstellen eines Schlüssels für Ihre Dienst-ID

Erstellen Sie im Abschnitt [Schlüssel](https://developer.apple.com/account/resources/authkeys/list) einen neuen **Schlüssel**.

Geben Sie einen Namen für Ihren Schlüssel ein, und aktivieren Sie die **Anmeldung mit Apple**.

![Neuen Schlüssel erstellen](sign-in-images/readme-key-create.png)

Klicken Sie neben _Anmelden bei Apple_auf **Konfigurieren** .

Stellen **Sie sicher**, dass die richtige **primäre APP-ID** ausgewählt ist, und klicken Sie auf

Klicken Sie auf **weiter** und dann auf **registrieren** , um den neuen Schlüssel zu erstellen.

Als nächstes haben Sie nur die Möglichkeit, den soeben generierten Schlüssel herunterzuladen.  Klicken Sie auf **Download**.

![Download Schlüssel](sign-in-images/readme-key-download.png)

Notieren Sie sich auch die **Schlüssel-ID** in diesem Schritt. Diese wird für `KeyId` später verwendet.

Sie haben eine `.p8` Schlüsseldatei heruntergeladen.  Sie können diese Datei in Notepad oder vscode öffnen, um den Text Inhalt anzuzeigen.  Sie sollten in etwa wie folgt aussehen:

```
-----BEGIN PRIVATE KEY-----
MIGTAgEAMBMGBasGSM49AgGFCCqGSM49AwEHBHkwdwIBAQQg3MX8n6VnQ2WzgEy0
Skoz9uOvatLMKTUIPyPCAejzzUCgCgYIKoZIzj0DAQehRANCAARZ0DoM6QPqpJxP
JKSlWz0AohFhYre10EXPkjrih4jTm+b0AeG2BGuoIWd18i8FimGDgK6IzHHPsEqj
DHF5Svq0
-----END PRIVATE KEY-----
```

Benennen Sie diesen Schlüssel, `P8FileContents` und bewahren Sie ihn an einem sicheren Ort auf. Sie werden Sie verwenden, wenn Sie diesen Dienst in Ihre Mobile Anwendung integrieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Schritte beschrieben, die zum Einrichten der Anmeldung bei Apple zur Verwendung in Ihren Anwendungen erforderlich sind Xamarin.Forms .

## <a name="related-links"></a>Verwandte Links

- [Anmelden mit Apple-Richtlinien](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
  