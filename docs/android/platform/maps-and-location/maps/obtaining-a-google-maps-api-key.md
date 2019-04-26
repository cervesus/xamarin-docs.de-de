---
title: Abrufen eines API-Schlüssels für Google Maps
description: Abrufen eines Google Maps-API-Schlüssels für das Hinzufügen von Zuordnungen Funktionen zu Ihrer app.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: bfeb9d8fa2a0b5a9b18ab8266500586e2e3b6c68
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155362"
---
# <a name="obtaining-a-google-maps-api-key"></a>Abrufen eines API-Schlüssels für Google Maps

Um die Funktionalität von Google Maps in Android zu verwenden, müssen Sie für einen Karten-API-Schlüssel mit Google registrieren. Bis Sie dies tun, sehen Sie nur ein leeres Raster anstelle einer Karte in Ihren Anwendungen. Sie benötigen einen Google Maps-Android-API-Schlüssel v2 - Schlüssel aus der älteren Version 1 Google Maps Android-API-Schlüssel funktioniert nicht.

Einen Maps-API v2-Schlüssel abrufen, umfasst die folgenden Schritte aus:

1.  Rufen Sie den SHA-1-Fingerabdruck des Keystores, die zum Signieren der Anwendung verwendet wird.
2.  Erstellen Sie ein Projekt, in der Google APIs Console.
3.  Abrufen des API-Schlüssels.


## <a name="obtaining-your-signing-key-fingerprint"></a>Abrufen von Ihr Signaturzertifikat-Fingerabdruck

Um einen Schlüssel für die Maps-API von Google anzufordern, müssen Sie den SHA-1-Fingerabdruck des Keystores kennen, die zum Signieren der Anwendung verwendet wird.
In der Regel bedeutet dies müssen Sie den SHA-1-Fingerabdruck für das Debug-Keystores zu bestimmen, und klicken Sie dann den SHA-1-Fingerabdruck für den Keystore aus, der zum Signieren der Anwendung für die Veröffentlichung verwendet wird.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Finden in der Standardeinstellung des Keystores, der zum Signieren von Debugversionen eine xamarin.Android-Anwendung, die Anwendung verwendet wird am folgenden Speicherort:

**"C:"\\Benutzer\\[USERNAME]\\AppData\\lokalen\\Xamarin\\Mono für Android\\debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich in der Regel im Verzeichnis "bin" Java:

**"C:"\\Programmdateien (x86)\\Java\\Jdk [VERSION]\\Bin\\keytool.exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Finden in der Standardeinstellung des Keystores, der zum Signieren von Debugversionen eine xamarin.Android-Anwendung, die Anwendung verwendet wird am folgenden Speicherort:

**/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich in der Regel im Verzeichnis "bin" Java:

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


Führen Sie mithilfe des folgenden Befehls (unter Verwendung der oben gezeigten Dateipfade) Keytool:

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore-Beispiel

Für den Standard-Debug-Schlüssel (die Sie für das Debuggen wird automatisch erstellt), verwenden Sie diesen Befehl:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>Produktionsschlüsseln

Wenn Sie eine app in Google Play bereitstellen zu können, muss es [mit einem privaten Schlüssel signiert](~/android/deploy-test/signing/index.md).
Die `keytool` müssen für die Ausführung mit der Details zum privaten Schlüssel und den resultierenden SHA-1-Fingerabdruck verwendet, um einem produktionsschlüssel von Google Maps-API zu erstellen. Denken Sie daran, aktualisieren Sie die **"androidmanifest.xml"** Datei mit dem richtigen Google Maps-API-Schlüssel vor der Bereitstellung.

### <a name="keytool-output"></a>Keytool-Ausgabe

Sie sollte etwa wie die folgende Ausgabe im Konsolenfenster angezeigt:

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

Verwenden Sie den SHA-1-Fingerabdruck (nach der aufgelisteten **SHA1**) weiter unten in diesem Handbuch.

## <a name="creating-an-api-project"></a>Erstellen eines API-Projekts

Nachdem Sie den SHA-1-Fingerabdruck des Keystores Signieren abgerufen haben, ist es erforderlich, erstellen ein neues Projekt in der Google APIs Console (oder den v2-Dienst von Google Maps-Android-API zu einem vorhandenen Projekt hinzufügen).

1. In einem Browser, navigieren Sie zu der [-API von Google Entwickler-Konsole & Services-Dashboard](https://console.developers.google.com/apis/dashboard/) , und klicken Sie auf **wählen Sie ein Projekt**. Klicken Sie auf einen Projektnamen ein, oder erstellen Sie eine neue, indem Sie auf **neues Projekt**:

   [![Google Developer Console-Projekt zum Erstellen-Schaltfläche](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Wenn Sie ein neues Projekt erstellt haben, geben Sie den Namen des Projekts in der **neues Projekt** Dialogfeld, das angezeigt wird. Dieses Dialogfeld wird eine eindeutige Projekt-ID produzieren, die auf den Projektnamen Ihres basiert. Klicken Sie anschließend die **erstellen** Schaltfläche wie im folgenden Beispiel gezeigt:

   [![Neues Projekt heißt XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Das Projekt wird nach einer Minute erstellt, und Sie gelangen auf die **Dashboard** Seite des Projekts. Klicken Sie dort auf **-APIS und Dienste aktivieren**:

   [![Klicken Sie auf Google Maps Android-API in der Bibliothek-Abschnitt](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. Von der **-API-Bibliothek** auf **Maps-SDK für Android**. Klicken Sie auf der nächsten Seite auf **aktivieren** um den Dienst für dieses Projekt zu aktivieren:

   [![Klicken Sie auf die Schaltfläche "aktivieren", in dem Abschnitt "Dashboard"](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

An diesem Punkt das API-Projekt erstellt wurde, und Google Maps-Android-API v2 hinzugefügt wurde. Allerdings können nicht Sie diese API in Ihrem Projekt verwenden, bis Sie die Anmeldeinformationen zu erstellen. Im nächste Abschnitt wird erläutert, wie Sie eine API-Schlüssel und -Liste einer Xamarin.Android-Anwendung erstellen, damit er autorisiert ist, um diesen Schlüssel.

## <a name="obtaining-the-api-key"></a>Abrufen der API-Schlüssel

Nach der **Google Developer Console** -API-Projekt wurde erstellt, es ist notwendig, einen Android-API-Schlüssel zu erstellen. Xamarin.Android-Anwendungen benötigen einen API-Schlüssel sie Zugriff auf Android-Map-API v2 gewährt werden.

1. In der **Maps-SDK für Android** Seite, die angezeigt wird (nach dem Klicken auf **aktivieren** im vorherigen Schritt), wechseln Sie zu der **Anmeldeinformationen** Registerkarte, und klicken Sie auf die **erstellen Anmeldeinformationen** Schaltfläche:

   [![Maps SDK für Android-Anmeldeinformationen-Nachricht](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Klicken Sie auf **API-Schlüssel**:

   [![Hinzufügen von Anmeldeinformationen auf Ihrem Dialogfeld "Projekt"](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Nachdem Sie diese Schaltfläche geklickt wird, wird der API-Schlüssel generiert. Als Nächstes ist es notwendig, diesen Schlüssel so einzuschränken, dass nur Ihre app APIs mit diesem Schlüssel aufrufen kann. Klicken Sie auf **RESTRICT Schlüssel**:

   [![Klicken Sie auf der Seite Anmeldeinformationen Schlüssel einschränken auf](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Ändern der **Namen** Feld **-API-Schlüssel 1** in einen Namen, mit denen Sie denken Sie daran, wofür der Schlüssel verwendet wird (**XamarinMapsDemoKey** wird in diesem Beispiel verwendet). Klicken Sie anschließend die **Android-apps** Optionsfeld:

   [![Android-apps auf der Seite Anmeldeinformationen auswählen](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Klicken Sie zum Hinzufügen des SHA-1-Fingerabdrucks **+ hinzufügen Paketnamen und Fingerabdruck**:

   [![Klicken Sie auf Add Package Name und Fingerabdruck](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Geben Sie den Namen Ihrer app-Paket, und geben Sie den SHA1-Fingerabdruck des Zertifikats (über abgerufen `keytool` wie weiter oben in diesem Handbuch beschrieben). Im folgenden Beispiel ist der name des Pakets für `XamarinMapsDemo` eingegeben ist, gefolgt von der SHA-1-Zertifikatfingerabdruck abgerufenes **debug.keystore**:

   [![Name des eingegebenen lautet com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Beachten Sie, dass in der Reihenfolge für das APK Zugriff auf Google Maps, müssen Sie SHA1-Fingerabdrücke gehören und Packen Namen für jede Keystore (Debug und Release), die Sie verwenden, um das APK zu signieren. Z. B. Wenn Sie einen Computer für Debug- und einem anderen Computer zum Generieren des Release-APK verwenden, sollten Sie den SHA-1-Fingerabdruck des Zertifikats aus dem debugkeystore des ersten Computers und der SHA-1-Fingerabdruck des Zertifikats aus dem Keystore Version der einschließen der zweite Computer. Klicken Sie auf **+ hinzufügen Paketnamen und Fingerabdruck** einen anderen Fingerabdruck und die Paket-Namen hinzufügen, wie im folgenden Beispiel gezeigt:

   [![Hinzufügen von einem anderen Fingerabdruck erstellt ein weiteres SHA-1-Zertifikat](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Klicken Sie auf **Save** (Speichern), um die Änderungen zu speichern. Als Nächstes werden Sie zur Liste der Ihre API-Schlüssel zurückgegeben. Wenn Sie eine andere API-Schlüssel, die Sie zuvor erstellt haben verfügen, werden diese auch hier aufgeführt werden. In diesem Beispiel wird nur ein API-Schlüssel (in den vorherigen Schritten erstellt) aufgeführt:

   [![XamarinMapsDemoKey wird in der Liste der API-Schlüssel angezeigt.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Verbinden Sie das Projekt zu einem gebührenpflichtigen Konto

Seit Juni 11 2018 funktioniert der API-Schlüssel nicht, wenn das Projekt nicht zu einem gebührenpflichtigen Konto verbunden ist (selbst wenn der Dienst weiterhin kostenlos für mobile apps ist).

1. Klicken Sie auf die Hamburger-Menü-Schaltfläche, und wählen Sie die **Abrechnung** Seite:

   [![Die Abrechnung Hamburger-Menü-Abschnitt auswählen](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Verknüpfen Sie das Projekt durch Klicken auf ein Rechnungskonto **verknüpfen Sie ein Rechnungskonto** gefolgt von **RECHNUNGSKONTO erstellen** angezeigten Popupfenster (Wenn Sie kein Konto haben, Sie gelangen um eine neue erstellen):

   [![Link-Projekt Rechnungskonto](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Den Schlüssel hinzufügen zu Ihrem Projekt

Fügen Sie abschließend diesen API-Schlüssel, um die **"androidmanifest.xml"** Datei Ihrer Xamarin.Android-App. Im folgenden Beispiel `YOUR_API_KEY` mit den API-Schlüssel in den vorherigen Schritten generiert ersetzt werden soll:

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

- [Google API-Konsole](https://code.google.com/apis/console/)
- [API-Schlüssels für Google Maps](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
