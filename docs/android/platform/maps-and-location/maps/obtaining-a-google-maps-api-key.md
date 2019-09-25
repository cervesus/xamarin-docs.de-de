---
title: Abrufen eines API-Schlüssels für Google Maps
description: Abrufen eines Google Maps-API-Schlüssels zum Hinzufügen von Maps-Funktionen zu Ihrer APP.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: 3868b2a35894cdcd7a11c626268307338744ecb4
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250050"
---
# <a name="obtaining-a-google-maps-api-key"></a>Abrufen eines API-Schlüssels für Google Maps

Um die Google Maps-Funktionalität in Android verwenden zu können, müssen Sie sich für einen Maps-API-Schlüssel mit Google registrieren. Bis zu diesem Zweck wird in Ihren Anwendungen nur ein leeres Raster anstelle einer Zuordnung angezeigt. Sie müssen eine Google Maps Android-API v2-Schlüssel Schlüssel aus dem älteren Google Maps Android-API-Schlüssel v1 erhalten.

Das Abrufen eines Maps-API v2-Schlüssels umfasst die folgenden Schritte:

1. Rufen Sie den SHA-1-Fingerabdruck des Keystores ab, der zum Signieren der Anwendung verwendet wird.
2. Erstellen Sie ein Projekt in der Google APIs-Konsole.
3. Abrufen des API-Schlüssels.

## <a name="obtaining-your-signing-key-fingerprint"></a>Abrufen des Fingerabdrucks des Signatur Schlüssels

Um einen Maps-API-Schlüssel von Google anzufordern, müssen Sie den SHA-1-Fingerabdruck des Keystores kennen, der zum Signieren der Anwendung verwendet wird.
In der Regel bedeutet dies, dass Sie den SHA-1-Fingerabdruck für den Debug-keystore und dann den SHA-1-Fingerabdruck für den Keystore bestimmen müssen, der zum Signieren Ihrer Anwendung für die Veröffentlichung verwendet wird.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Der Keystore, der zum Signieren der Debugversionen einer xamarin. Android-Anwendung verwendet wird, befindet sich standardmäßig unter folgendem Speicherort:

**C:\\Benutzer\\[Benutzername\\] APPDATA\\\\local xamarin\\Mono for\\Android Debug. KeyStore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise im Java-Verzeichnis "bin":

**C:\\Programme (x86)\\Java\\jdk [Version]\\bin\\keytool. exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Der Keystore, der zum Signieren der Debugversionen einer xamarin. Android-Anwendung verwendet wird, befindet sich standardmäßig unter folgendem Speicherort:

**/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

Durch das Ausführen des Befehls `keytool` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise im Java-Verzeichnis "bin":

**/System/Library/Java/JavaVirtualMachines/[Version]. JDK/Content/Home/bin/keytool**

-----

Führen Sie keytool mit dem folgenden Befehl aus (mithilfe der oben gezeigten Dateipfade):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug. KeyStore-Beispiel

Verwenden Sie den folgenden Befehl für den standardmäßigen debugschlüssel (der automatisch für das Debuggen erstellt wird):

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>Produktions Schlüssel

Wenn Sie eine APP für die Google Play bereitstellen, muss Sie [mit einem privaten Schlüssel signiert](~/android/deploy-test/signing/index.md)werden.
`keytool` Muss mit den Details des privaten Schlüssels und dem resultierenden SHA-1-Fingerabdruck ausgeführt werden, der zum Erstellen eines Google Maps-API-Schlüssels für die Produktion verwendet wird. Denken Sie daran, die Datei " **androidmanifest. XML** " vor der Bereitstellung mit dem richtigen Google Maps-API-Schlüssel zu aktualisieren

### <a name="keytool-output"></a>Keytool-Ausgabe

In Ihrem Konsolenfenster sollte etwas wie die folgende Ausgabe angezeigt werden:

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

Sie verwenden den SHA-1-Fingerabdruck (nach **SHA1**) weiter unten in diesem Handbuch.

## <a name="creating-an-api-project"></a>Erstellen eines API-Projekts

Nachdem Sie den SHA-1-Fingerabdruck des Signatur-Keystores abgerufen haben, müssen Sie ein neues Projekt in der Google-APIs-Konsole erstellen (oder den Google Maps Android API v2-Dienst einem vorhandenen Projekt hinzufügen).

1. Navigieren Sie in einem Browser zur [Google Developers Console-API & Services-Dashboard](https://console.developers.google.com/apis/dashboard/) , und klicken Sie auf **Projekt auswählen**. Klicken Sie auf einen Projektnamen, oder erstellen Sie einen neuen, indem Sie auf " **Neues Projekt**" klicken:

   [![Schaltfläche zum Erstellen eines Projekts in Google Developer Console](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Wenn Sie ein neues Projekt erstellt haben, geben Sie den Projektnamen im Dialogfeld **Neues Projekt** ein, das angezeigt wird. In diesem Dialogfeld wird eine eindeutige Projekt-ID erstellt, die auf Ihrem Projektnamen basiert. Klicken Sie anschließend auf die Schaltfläche **Erstellen** , wie im folgenden Beispiel gezeigt:

   [![Neues Projekt mit dem Namen "xamarinmapsdemo"](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Nach einer Minute oder so wird das Projekt erstellt, und Sie gelangen zur **Dashboardseite** des Projekts. Klicken Sie dort auf **APIs und Dienste aktivieren**:

   [![Klicken auf Google Maps Android-API im Abschnitt "Library"](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. Klicken Sie auf der Seite **API-Bibliothek** auf **Maps SDK für Android**. Klicken Sie auf der nächsten Seite auf aktivieren, um den Dienst für dieses Projekt zu **aktivieren** :

   [![Klicken auf die Schaltfläche "aktivieren" im dashboardabschnitt](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

An diesem Punkt wurde das API-Projekt erstellt, und Google Maps Android API v2 wurde hinzugefügt. Allerdings können Sie diese API nicht in Ihrem Projekt verwenden, bis Sie dafür Anmelde Informationen erstellen. Im nächsten Abschnitt wird erläutert, wie ein API-Schlüssel erstellt wird und wie eine xamarin. Android-Anwendung in eine Whitelist aufgenommen wird, damit Sie für die Verwendung dieses Schlüssels autorisiert ist.

## <a name="obtaining-the-api-key"></a>Abrufen des API-Schlüssels

Nachdem das **Google Developer Console** -API-Projekt erstellt wurde, ist es erforderlich, einen Android-API-Schlüssel zu erstellen. Xamarin. Android-Anwendungen müssen über einen API-Schlüssel verfügen, bevor Ihnen der Zugriff auf Android Map API v2 gewährt wird.

1. Wechseln Sie auf der Seite **Maps SDK für Android** , die angezeigt wird (nachdem Sie im vorherigen Schritt auf **aktivieren** geklickt haben), zur Registerkarte **Anmelde** Informationen, und klicken Sie auf die Schaltfläche **Anmelde Informationen erstellen** :

   [![Meldung zum Maps SDK für Android-Anmelde Informationen](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Klicken Sie auf **API-Schlüssel**:

   [![Hinzufügen von Anmelde Informationen zum Projekt Dialogfeld](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. Nachdem Sie auf diese Schaltfläche geklickt haben, wird der API-Schlüssel generiert. Als nächstes muss dieser Schlüssel eingeschränkt werden, sodass nur Ihre APP APIs mit diesem Schlüssel abrufen kann. Klicken Sie auf **Schlüssel einschränken**:

   [![Klicken auf Schlüssel einschränken auf der Seite "Anmelde Informationen"](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Ändern Sie das Feld **Name** von **API-Schlüssel 1** in einen Namen, mit dem Sie merken können, wofür der Schlüssel verwendet wird (in diesem Beispiel wird**xamarinmapsdemokey** verwendet). Klicken Sie anschließend auf das Optionsfeld **Android-Apps** :

   [![Auswählen von Android-Apps auf der Seite "Anmelde Informationen"](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Um den SHA-1-Fingerabdruck hinzuzufügen, klicken Sie auf **+ Paketnamen und Fingerabdruck hinzufügen**:

   [![Klicken auf Paketnamen und Fingerabdruck hinzufügen](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Geben Sie den Paketnamen Ihrer APP ein, und geben Sie den SHA-1- `keytool` Zertifikat Fingerabdruck ein (wie weiter oben in diesem Handbuch erläutert). Im folgenden Beispiel wird der Paketname für `XamarinMapsDemo` eingegeben, gefolgt vom SHA-1-Zertifikat Fingerabdruck, der von **Debug. KeyStore**abgerufen wurde:

   [![Der eingegebene Paketname lautet "com. xamarin. docs. Android. map".](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Beachten Sie, dass in der Reihenfolge für das APK Zugriff auf Google Maps, müssen Sie SHA1-Fingerabdrücke gehören und Packen Namen für jede Keystore (Debug und Release), die Sie verwenden, um das APK zu signieren. Z. B. Wenn Sie einen Computer für Debug- und einem anderen Computer zum Generieren des Release-APK verwenden, sollten Sie den SHA-1-Fingerabdruck des Zertifikats aus dem debugkeystore des ersten Computers und der SHA-1-Fingerabdruck des Zertifikats aus dem Keystore Version der einschließen der zweite Computer. Klicken Sie auf **+ Paketnamen und Fingerabdruck hinzufügen** , um einen weiteren Fingerabdruck und Paketnamen hinzuzufügen, wie im folgenden Beispiel gezeigt

   [![Hinzufügen eines weiteren Fingerabdrucks](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Klicken Sie auf **Save** (Speichern), um die Änderungen zu speichern. Als nächstes werden Sie zur Liste ihrer API-Schlüssel zurückgegeben. Wenn Sie bereits über andere API-Schlüssel verfügen, die Sie zuvor erstellt haben, werden diese ebenfalls aufgelistet. In diesem Beispiel wird nur ein API-Schlüssel (erstellt in den vorherigen Schritten) aufgelistet:

   [![Xamarinmapsdemokey wird in der Liste mit den API-Schlüsseln angezeigt.](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Verbinden des Projekts mit einem abrechnungsfähigen Konto

Ab dem 11. Juni 2018 funktioniert der API-Schlüssel nicht, wenn das Projekt nicht mit einem abrechnungsfähigen Konto verbunden ist (selbst wenn der Dienst für Mobile Apps noch kostenlos ist).

1. Klicken Sie auf die Schaltfläche Hamburger, und wählen Sie die Seite **Abrechnung** aus:

   [![Auswählen des Abschnitts "Hamburger-Menü Abrechnung"](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Verknüpfen Sie das Projekt mit einem Abrechnungskonto, indem Sie auf mit **einem Abrechnungskonto verknüpfen** und dann auf dem angezeigten Popup ein **Abrechnungskonto erstellen** (wenn Sie über kein Konto verfügen, werden Sie aufgefordert, ein neues Konto zu erstellen):

   [![Projekt mit Abrechnungskonto verknüpfen](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Hinzufügen des Schlüssels zum Projekt

Fügen Sie schließlich den API-Schlüssel der Datei " **androidmanifest. XML** " ihrer xamarin. Android-App hinzu. Im folgenden Beispiel muss durch `YOUR_API_KEY` den API-Schlüssel ersetzt werden, der in den vorherigen Schritten generiert wurde:

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

- [Google-APIs-Konsole](https://code.google.com/apis/console/)
- [Der Google Maps-API-Schlüssel](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
