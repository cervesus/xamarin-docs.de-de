---
title: Abrufen einer Google-API-Schlüssel zugeordnet ist
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/20/2018
ms.openlocfilehash: 5a162ba15bbb6d6dcdf7debe01fb12b3b08a843e
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309789"
---
# <a name="obtaining-a-google-maps-api-key"></a>Abrufen einer Google-API-Schlüssel zugeordnet ist

Um die Funktionalität von Google Maps in Android verwenden zu können, müssen Sie für einen Maps-API-Schlüssel mit Google registrieren. Bis Sie dies tun, sehen Sie nur ein leeres Raster statt es als Karte in Ihren Anwendungen. Sie benötigen einen Google Maps Android-API v2-Schlüssel - Schlüssel von der älteren Google Maps Android-API-Schlüssel v1 funktioniert nicht.

Ein Maps-API v2 Schlüssel umfasst die folgenden Schritte aus:

1.  Rufen Sie die SHA-1-Fingerabdruck eines Schlüsselspeicher, der zum Signieren der Anwendung verwendet wird.
2.  Erstellen Sie ein Projekt in der Google APIs Console.
3.  Abrufen des API-Schlüssels an.


## <a name="obtaining-your-signing-key-fingerprint"></a>Ihr Schlüssel signieren Fingerabdruck abrufen

Um ein Maps-API-Schlüssel von Google anfordern, müssen Sie den SHA-1-Fingerabdruck des Schlüsselspeichers kennen, die zum Signieren der Anwendung verwendet wird.
Dies bedeutet normalerweise, müssen Sie den SHA-1-Fingerabdruck für das Debug-Schlüsselspeicher zu bestimmen, und klicken Sie dann den SHA-1-Fingerabdruck für den Schlüsselspeicher, der zum Signieren der Anwendung für Version verwendet wird.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Standardmäßig Schlüsselspeicher, der verwendet wird, zum Signieren von Debugversionen von einem Xamarin.Android Anwendung werden finden Sie unter folgendem Speicherort:

**"C:"\\Benutzer\\[USERNAME]\\AppData\\lokale\\Xamarin\\für Android Mono /\\debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich in der Regel im Verzeichnis "bin" Java:

**"C:"\\Programmdateien (x86)\\Java\\Jdk [VERSION]\\"bin"\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Standardmäßig Schlüsselspeicher, der verwendet wird, zum Signieren von Debugversionen von einem Xamarin.Android Anwendung werden finden Sie unter folgendem Speicherort:

**/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich in der Regel im Verzeichnis "bin" Java:

**/System/Library/Java/JavaVirtualMachines/[Version].JDK/Contents/Home/bin/keytool**

-----


Führen Sie Keytool, die mit dem folgenden Befehl (mit der oben gezeigten Dateipfade):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore-Beispiel

Für den standardmäßigen Debug-Schlüssel (die automatisch für Sie zum Debuggen erstellt wird), verwenden Sie diesen Befehl:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Produktions-Schlüssel

Wenn eine app auf Google Play bereitstellen zu können, muss er [mit einem privaten Schlüssel signiert](~/android/deploy-test/signing/index.md).
Die `keytool` müssen für die Ausführung mit dem privaten Schlüssel Details und den resultierenden SHA-1-Fingerabdruck verwendet, um einen Produktion Google Maps-API-Schlüssel zu erstellen. Denken Sie daran, aktualisieren Sie die **AndroidManifest.xml** Datei mit dem richtigen Google Maps-API-Schlüssel vor der Bereitstellung.

### <a name="keytool-output"></a>Keytool-Ausgabe

Sie sollte etwa wie die folgende Ausgabe in Ihrem Konsolenfenster angezeigt werden:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Verwenden Sie den Fingerabdruck des SHA-1 (aufgelistet nach **SHA1**) weiter unten in diesem Handbuch.

## <a name="creating-an-api-project"></a>Erstellen eines API-Projekts

Nachdem Sie die SHA-1-Fingerabdruck eines Signaturzertifikat Keystore abgerufen haben, ist es notwendig, erstellen ein neues Projekt in der Google APIs Console (bzw. den Google Maps Android-API v2-Dienst zu einem vorhandenen Projekt hinzufügen).

1. In einem Browser, navigieren Sie zu der [Google Entwickler Konsole API & Services-Dashboard](https://console.developers.google.com/apis/dashboard/) , und klicken Sie auf **wählen Sie ein Projekt**. Klicken Sie auf einen Projektnamen ein, oder ein neues erstellen, indem Sie auf **neues Projekt**:

   [![Schaltfläche für Google Developer Console-Projekt zum Erstellen](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Wenn Sie ein neues Projekt erstellt haben, geben Sie den Namen des Projekts in der **neues Projekt** Dialogfeld, das angezeigt wird. Dieses Dialogfeld wird eine eindeutige ID produzieren, die auf den Projektnamen basiert. Klicken Sie anschließend auf die **erstellen** Schaltfläche wie im folgenden Beispiel gezeigt:

   [![Neues Projekt wird mit dem Namen XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Das Projekt wird nach einer Minute erstellt, und Sie gelangen auf die **Dashboard** Seite des Projekts. Klicken Sie dort auf **aktivieren APIS und-Dienste,**:

   [![Klicken Sie auf die Google Maps Android-API in der Bibliothek-Abschnitt](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. Aus der **API-Bibliothek** auf **Maps-SDK für Android**. Klicken Sie auf der nächsten Seite auf **aktivieren** um den Dienst für dieses Projekt zu aktivieren:

   [![Klicken Sie auf die Schaltfläche "aktivieren" im Abschnitt Dashboard](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

An diesem Punkt der API-Projekt erstellt wurde, und Google Maps Android-API v2 hinzugefügt wurde. Allerdings können Sie erst verwenden, diese API in Ihrem Projekt Anmeldeinformationen dafür zu erstellen. Im nächste Abschnitt erläutert, wie ein API-Schlüssel und die weiße Liste eine Xamarin.Android-Anwendung erstellen, damit Sie diesen Schlüssel autorisiert ist.

## <a name="obtaining-the-api-key"></a>Abrufen von API-Schlüssel

Nach der **Google-Entwicklerkonsole** -API-Projekt wurde erstellt, es ist notwendig, einen Android-API-Schlüssel zu erstellen. Xamarin.Android Anwendungen benötigen einen API-Schlüssel auf, bevor ihnen der Zugriff auf Android-Map-API v2 erteilt werden.

1. In der **Maps-SDK für Android** Seite, die angezeigt wird (nach dem Klicken auf **aktivieren** im vorherigen Schritt), wechseln Sie zu der **Anmeldeinformationen** Registerkarte, und klicken Sie auf die **erstellen Anmeldeinformationen** Schaltfläche:

   [![SDK für Android Anmeldeinformationen Nachricht zuordnet](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Klicken Sie auf **API-Schlüssel**:

   [![Hinzufügen von Anmeldeinformationen auf Ihr Dialogfeld "Projekt"](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Nachdem diese Schaltfläche geklickt wird, wird die API-Schlüssel generiert. Als Nächstes ist es erforderlich, diesen Schlüssel beschränken, damit nur die app APIs mit diesem Schlüssel aufrufen kann. Klicken Sie auf **beschränken Schlüssel**:

   [![Klicken Sie auf der Seite "Anmeldeinformationen" Schlüssel beschränken auf](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Ändern der **Namen** Feld **API-Schlüssel 1** in einen Namen, die Informationen bereitstellen, was für der Schlüssel verwendet wird (**XamarinMapsDemoKey** wird in diesem Beispiel verwendet). Klicken Sie anschließend auf die **Android-apps** Optionsfeld:

   [![Android-apps auf der Seite "Anmeldeinformationen" auswählen](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Klicken Sie zum Hinzufügen des SHA1-Fingerabdruck **+ hinzufügen Paketnamen und Fingerabdruck**:

   [![Klicken Sie auf Hinzufügen-Paketnamen und Fingerabdruck](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Geben Sie Ihre app ein, und geben Sie den Zertifikatfingerabdruck SHA-1 (über abgerufen `keytool` wie weiter oben in diesem Handbuch erläutert). Im folgenden Beispiel der name des Pakets für `XamarinMapsDemo` eingegeben wird, gefolgt von den SHA-1-Zertifikat-Fingerabdruck abgerufenes **debug.keystore**:

   [![Paketname eingegeben wird com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Beachten Sie, dass in der Reihenfolge für die APK Zugriff auf Google Maps, müssen Sie SHA-1-Fingerabdrücke gehören und Namen für jede Keystore (Debug und Release), mit denen Sie signieren Ihre APK-Paket. Z. B. Wenn Sie einen Computer für die Debug- und einem anderen Computer zum Generieren der Release-APK verwenden, sollten Sie den Fingerabdruck des SHA-1-Zertifikat von der Debug-Schlüsselspeicher des ersten Computers und den Fingerabdruck des SHA-1-Zertifikat aus der Version Schlüsselspeicher des einschließen der zweite Computer. Klicken Sie auf **+ hinzufügen Paketnamen und Fingerabdruck** So fügen Sie einen anderen Fingerabdruck und die Paket-Namen hinzu, wie im folgenden Beispiel gezeigt:

   [![Hinzufügen von einem anderen Fingerabdruck erstellt ein anderes SHA-1-Zertifikat](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Klicken Sie auf **Save** (Speichern), um die Änderungen zu speichern. Als Nächstes werden Sie der Liste Ihrer API-Schlüssel zurückgegeben. Wenn Sie andere API-Schlüssel, die Sie zuvor erstellt haben verfügen, werden sie auch hier aufgeführt. In diesem Beispiel ist nur ein API-Schlüssel (in den vorherigen Schritten erstellten) aufgeführt:

   [![XamarinMapsDemoKey wird in der Liste der API-Schlüssel angezeigt.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Verbinden Sie das Projekt mit einem abrechenbare-Konto

Ab Juni, 11 2018, funktioniert API-Schlüssel nicht, wenn das Projekt nicht mit einem abrechenbare-Konto verbunden ist, (selbst wenn der Dienst für mobile apps weiterhin kostenlos ist).

1. Klicken Sie auf die Schaltfläche Hamburger-Menü, und wählen Sie die **Abrechnung** Seite:

   [![Die Abrechnung Hamburger Menüabschnitt auswählen](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Verknüpfen Sie das Projekt mit einer Abrechnungskonto durch Klicken auf **verknüpfen Sie ein Abrechnungskonto** gefolgt von **ABRECHNUNGSKONTO erstellen** angezeigten Popupfenster (Wenn Sie kein Konto verfügen, Sie werden werden geführt, um eine neue zu erstellen):

   [![Link-Projekt, um Abrechnungskonto](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Den Schlüssel hinzufügen zum Projekt

Fügen Sie schließlich diese API-Schlüssel, der **AndroidManifest.XML** Datei Ihrer App Xamarin.Android. Im folgenden Beispiel `YOUR_API_KEY` mit der API-Schlüssel generiert, die in den vorherigen Schritten ersetzt werden soll:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>Verwandte Links

- [Die Konsole der Google-APIs](https://code.google.com/apis/console/)
- [Die Google Maps-API-Schlüssel](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
