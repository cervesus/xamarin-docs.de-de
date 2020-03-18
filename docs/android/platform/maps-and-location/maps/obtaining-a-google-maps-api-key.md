---
title: Abrufen eines API-Schlüssels für Google Maps
description: Hier erfahren Sie, wie Sie einen Google Maps-API-Schlüssel abrufen, um Ihrer App Kartenfunktionalität hinzuzufügen.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "75488971"
---
# <a name="obtaining-a-google-maps-api-key"></a>Abrufen eines API-Schlüssels für Google Maps

Zur Verwendung der Google Maps-Funktionalität in Android müssen Sie sich bei Google für einen Maps-API-Schlüssel registrieren. Bis zur Registrierung wird in Ihren Anwendungen anstelle einer Karte lediglich ein leeres Raster angezeigt. Sie müssen einen Google Maps Android API v2-Schlüssel abrufen – Schlüssel aus der älteren Google Maps Android API v1-Version funktionieren nicht.

Der Abruf eines Maps API v2-Schlüssels umfasst die folgenden Schritte:

1. Rufen Sie den SHA-1-Fingerabdruck des Keystores ab, der zum Signieren der Anwendung verwendet wird.
2. Erstellen Sie ein Projekt in der Google APIs-Konsole.
3. Rufen Sie den API-Schlüssel ab.

## <a name="obtaining-your-signing-key-fingerprint"></a>Abrufen des Fingerabdrucks Ihres Signaturschlüssels

Um einen Maps-API-Schlüssel von Google anzufordern, müssen Sie den SHA-1-Fingerabdruck des Keystores kennen, der zum Signieren der Anwendung verwendet wird.
Typischerweise bedeutet dies, dass Sie den SHA-1-Fingerabdruck für den Debug-Keystore sowie anschließend den SHA-1-Fingerabdruck für den Keystore ermitteln müssen, der zum Signieren Ihrer Anwendung für die Veröffentlichung verwendet wird.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Standardmäßig befindet sich der Keystore, der zum Signieren von Debugversionen einer Xamarin.Android-Anwendung verwendet wird, an folgendem Speicherort:

**C:\\Benutzer\\[BENUTZERNAME]\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise im Java-Verzeichnis „bin“:

**C:\\Programme\\Android\\jdk\\microsoft_dist_openjdk_[VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Standardmäßig befindet sich der Keystore, der zum Signieren von Debugversionen einer Xamarin.Android-Anwendung verwendet wird, an folgendem Speicherort:

**/Benutzer/[BENUTZERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise im Java-Verzeichnis „bin“:

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----

Führen Sie „keytool“ mit dem folgenden Befehl (unter Verwendung der oben gezeigten Dateipfade) aus:

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore-Beispiel

Verwenden Sie für den standardmäßigen Debugschlüssel (der automatisch für das Debuggen erstellt wird) diesen Befehl:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>Produktionsschlüssel

Für die Bereitstellung einer App in Google Play muss diese [mit einem privaten Schlüssel signiert werden](~/android/deploy-test/signing/index.md).
`keytool` muss mit den Informationen des privaten Schlüssels ausgeführt werden, und der resultierende SHA-1-Fingerabdruck wird verwendet, um einen Google Maps-API-Produktionsschlüssel zu erstellen. Denken Sie vor der Bereitstellung daran, die Datei **AndroidManifest.xml** mit dem richtigen Google Maps-API-Schlüssel zu aktualisieren.

### <a name="keytool-output"></a>Ausgabe von „keytool“

Die Ausgabe im Konsolenfenster sollte der folgenden ähneln:

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

Sie verwenden den SHA-1-Fingerabdruck (nach **SHA1** aufgeführt) später in diesem Leitfaden.

## <a name="creating-an-api-project"></a>Erstellen eines API-Projekts

Nachdem Sie den SHA-1-Fingerabdruck des Signatur-Keystores abgerufen haben, müssen Sie in der Google APIs-Konsole ein neues Projekt erstellen (oder den Google Maps Android API v2-Dienst zu einem vorhandenen Projekt hinzuzufügen).

1. Navigieren Sie in einem Browser zu [Google Developer Console – APIs & Dienste](https://console.developers.google.com/apis/dashboard/), und klicken Sie auf **Projekt auswählen**. Klicken Sie auf einen Projektnamen, oder erstellen Sie ein neues Projekt, indem Sie auf **NEUES PROJEKT** klicken:

   [![Schaltfläche NEUES PROJEKT in der Google Developer Console](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Wenn Sie ein neues Projekt erstellt haben, geben Sie den Projektnamen im angezeigten Dialogfeld **Neues Projekt** ein. In diesem Dialogfeld wird eine eindeutige Projekt-ID generiert, die auf dem Namen Ihres Projekts basiert. Klicken Sie als Nächstes auf die Schaltfläche **Erstellen**, wie in diesem Beispiel gezeigt:

   [![Neues Projekt namens „XamarinMapsDemo“](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Nach etwa einer Minute wird das Projekt erstellt, und Sie gelangen zur Seite **Dashboard** für das Projekt. Klicken Sie auf dieser Seite auf **APIS UND DIENSTE AKTIVIEREN**:

   [![Klicken auf die Google Maps-API für Android im Bibliotheksabschnitt](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. Klicken Sie auf der Seite **API-Bibliothek** auf **Maps SDK for Android**. Klicken Sie auf der nächsten Seite auf **AKTIVIEREN**, um den Dienst für dieses Projekt zu aktivieren:

   [![Klicken auf die Schaltfläche AKTIVIEREN im Dashboardabschnitt](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

Zu diesem Zeitpunkt wurde das API-Projekt erstellt, und die Google Maps Android API v2 wurde hinzugefügt. Sie können diese API jedoch erst dann in Ihrem Projekt verwenden, wenn Sie Anmeldeberechtigungen dafür erstellt haben. Im nächsten Abschnitt wird erläutert, wie Sie einen API-Schlüssel erstellen und eine Xamarin.Android-Anwendung in die Whitelist aufnehmen, damit sie zur Verwendung dieses Schlüssels berechtigt ist.

## <a name="obtaining-the-api-key"></a>Abrufen des API-Schlüssels

Nachdem das API-Projekt in der **Google Developer Console** erstellt wurde, muss ein Android-API-Schlüssel erstellt werden. Xamarin.Android-Anwendungen benötigen einen API-Schlüssel, um Zugriff auf Android Map API v2 zu erhalten.

1. Wechseln Sie auf der (nach dem Klicken auf **AKTIVIEREN** im vorherigen Schritt) angezeigten Seite **Maps SDK for Android** zur Registerkarte **Anmeldedaten**, und klicken Sie auf die Schaltfläche **Anmeldedaten erstellen**:

   [![Meldung zu Maps SDK for Android-Anmeldedaten](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Klicken Sie auf **API-Schlüssel**:

   [![Dialogfeld zum Hinzufügen von Anmeldedaten zu Ihrem Projekt](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Nach dem Klicken auf diese Schaltfläche wird der API-Schlüssel erstellt. Als Nächstes muss dieser Schlüssel eingeschränkt werden, damit nur Ihre App APIs mit diesem Schlüssel aufrufen kann. Klicken Sie auf **SCHLÜSSEL EINSCHRÄNKEN**:

   [![Klicken auf „Schlüssel einschränken“ auf der Seite für die Anmeldedaten](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Ändern Sie im Feld **Name** den Wert von **API-Schlüssel 1** in einen Namen, der die Verwendung des Schlüssels angibt (in diesem Beispiel wird **XamarinMapsDemoKey** verwendet). Aktivieren Sie anschließend das Optionsfeld **Android-Apps**:

   [![Aktivierung von „Android-Apps“ auf der Seite mit Anmeldedaten](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Um den SHA-1-Fingerabdruck hinzuzufügen, klicken Sie auf **Paketnamen und Fingerabdruck hinzufügen**:

   [![Klicken auf „Paketnamen und Fingerabdruck hinzufügen“](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Geben Sie den Paketnamen für Ihre App ein, und geben Sie den SHA-1-Fingerabdruck ein (den Sie wie zuvor besprochen über `keytool` abgerufen haben). Im folgenden Beispiel wird der Paketname für `XamarinMapsDemo` eingegeben, gefolgt vom SHA-1-Zertifikatfingerabdruck, der aus **debug.keystore** abgerufen wurde:

   [![Eingegebener Paketname: „com.xamarin.docs.android.map“](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Beachten Sie Folgendes: Damit Ihr APK auf Google Maps zugreifen kann, müssen Sie SHA-1-Fingerabdrücke und Paketnamen für jeden Keystore (Debug und Release) angeben, den Sie zum Signieren Ihrer APK verwenden. Wenn Sie beispielsweise einen Computer für das Debuggen und einen anderen Computer zum Generieren der Release-APK verwenden, müssen Sie den Fingerabdruck des SHA-1-Zertifikats aus dem Debug-Keystore des ersten Computers und den Fingerabdruck des SHA-1-Zertifikats aus dem Release-Keystore des zweiten Computers angeben. Klicken Sie auf **Paketnamen und Fingerabdruck hinzufügen**, um wie in diesem Beispiel einen weiteren Paketnamen und Fingerabdruck hinzuzufügen:

   [![Erstellen eines weiteren SHA-1-Zertifikats durch Hinzufügen eines weiteren Fingerabdrucks](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Klicken Sie auf **Save** (Speichern), um die Änderungen zu speichern. Es wird erneut die Liste Ihrer API-Schlüssel angezeigt. Wenn Sie zuvor weitere API-Schlüssel erstellt haben, werden diese hier ebenfalls aufgeführt. Im vorliegenden Beispiel wird nur ein API-Schlüssel aufgelistet (der in den vorherigen Schritten erstellt wurde):

   [![API-Liste mit Anzeige von „XamarinMapsDemoKey“](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Verbinden des Projekts mit einem Abrechnungskonto

Ab dem 11. Juni 2018 funktioniert der API-Schlüssel nur, wenn das Projekt mit einem Konto für die Abrechnung verbunden ist (auch wenn der Dienst für mobile Anwendungen weiterhin kostenlos ist).

1. Klicken Sie auf das Hamburger-Menü, und wählen Sie die Seite **Abrechnung** aus:

   [![Auswahl des Abschnitts „Abrechnung“ im Hamburger-Menü](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Verknüpfen Sie das Projekt mit einem Abrechnungskonto, indem Sie im angezeigten Popupfenster auf **Rechnungskonto verknüpfen** und dann auf **RECHNUNGSKONTO ERSTELLEN** klicken (wenn Sie über kein Konto verfügen, werden Sie aufgefordert, ein neues Konto zu erstellen):

   [![Verknüpfen eines Projekts mit einem Rechnungskonto](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Hinzufügen des Schlüssels zu Ihrem Projekt

Abschließend wird der API-Schlüssel zur Datei **AndroidManifest.XML** Ihrer Xamarin.Android-App hinzugefügt. Im folgenden Beispiel muss `YOUR_API_KEY` durch den API-Schlüssel ersetzt werden, der in den vorherigen Schritten erstellt wurde:

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

- [Google APIs-Konsole](https://code.google.com/apis/console/)
- [Google Maps-API-Schlüssel](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
