---
title: "Xamarin.Essentials: SecureStorage" description: "In diesem Dokument wird die SecureStorage-Klasse in Xamarin.Essentials beschrieben, mit der einfache Schlüssel-Wert-Paare sicher gespeichert werden können. Hier erfahren Sie das Wichtigste über die Verwendung der Klasse, die Besonderheiten der Plattformimplementierung und geltende Einschränkungen."
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64 author: jamesmontemagno ms.author: jamont ms.date: 04/02/2019 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Sicherer Speicher

Mit der Klasse **SecureStorage** können Sie einfache Schlüssel/Wertpaare sicher speichern.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Der Zugriff auf die **SecureStorage**-Funktionalität erfordert das folgende plattformspezifische Setup:

# <a name="android"></a>[Android](#tab/android)

> [!TIP]
> Die [Automatische Sicherung für Apps](https://developer.android.com/guide/topics/data/autobackup) ist eine Funktion von Android 6.0 (API-Ebene 23) und höher, die die App-Daten von Benutzern sichert (freigegebene Einstellungen, Dateien im internen App-Speicher und andere spezifische Dateien). Die Daten werden wiederhergestellt, wenn eine App neu installiert oder auf einem neuen Gerät installiert wird. Dies kann sich auf die `SecureStorage`-Klasse auswirken, die Freigabeeinstellungen verwendet, die gesichert werden und nicht beim Wiederherstellen entschlüsselt werden können. Xamarin.Essentials verarbeitet diesen Fall automatisch durch Entfernen des Schlüssels, um das Zurücksetzen zu ermöglichen. Sie können die automatische Sicherung jedoch auch manuell deaktivieren.

### <a name="enable-or-disable-backup"></a>Aktivieren und Deaktivieren der Sicherung
Sie können die automatische Sicherung für Ihre gesamte Anwendung deaktivieren, indem Sie die Einstellung `android:allowBackup` in der Datei `AndroidManifest.xml` auf FALSE festlegen. Dieser Ansatz wird nur empfohlen, wenn Sie Daten auf eine andere Weise wiederherstellen möchten.

```xml
<manifest ... >
    ...
    <application android:allowBackup="false" ... >
        ...
    </application>
</manifest>
```

### <a name="selective-backup"></a>Selektive Sicherung
Die automatische Sicherung lässt sich konfigurieren, um bestimmte Inhalte davon auszuschließen. Sie können ein benutzerdefiniertes Regelset erstellen, um `SecureStore`-Elemente von der Sicherung auszuschließen.

1. Legen Sie das `android:fullBackupContent`-Attribut in Ihrer **AndroidManifest.xml** fest:

    ```xml
    <application ...
        android:fullBackupContent="@xml/auto_backup_rules">
    </application>
    ```

2. Erstellen Sie eine neue XML-Datei mit dem Namen **auto_backup_rules.xml** mit dem Buildvorgang **AndroidResource** im Verzeichnis **Resources/xml**. Legen Sie dann den folgenden Inhalt fest, der alle freigegebenen Einstellungen mit Ausnahme von `SecureStorage` enthält:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <full-backup-content>
        <include domain="sharedpref" path="."/>
        <exclude domain="sharedpref" path="${applicationId}.xamarinessentials.xml"/>
    </full-backup-content>
    ```

# <a name="ios"></a>[iOS](#tab/ios)

Aktivieren Sie beim Entwickeln mit dem **iOS-Simulator** die Berechtigung **Keychain**, und fügen Sie eine Keychain-Zugriffsgruppe für die Bündel-ID der Anwendung hinzu.

Öffnen Sie die Datei **Entitlements.plist** im iOS-Projekt, suchen Sie die Berechtigung **Keychain**, und aktivieren Sie sie. Dadurch wird der Bezeichner der Anwendung automatisch als Gruppe hinzugefügt.

Legen Sie in den Projekteigenschaften unter **iOS-Bündelsignierung** **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** fest.

> [!TIP]
> Beim Bereitstellen auf iOS-Geräten ist diese Berechtigung nicht erforderlich und sollte entfernt werden.

# <a name="uwp"></a>[UWP](#tab/uwp)

Es ist kein zusätzliches Setup erforderlich.

-----

## <a name="using-secure-storage"></a>Verwenden des sicheren Speichers

Fügen Sie Ihrem Projekt einen Xamarin.Essentials-Verweis hinzu:

```csharp
using Xamarin.Essentials;
```

So speichern Sie einen Wert für einen bestimmten _Schlüssel_ im sicheren Speicher:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

So rufen Sie einen Wert aus dem sicheren Speicher ab:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> Wenn dem angeforderten Schlüssel kein Wert zugeordnet ist, gibt `GetAsync``null` zurück.

So entfernen Sie einen bestimmten Schlüssel:

```csharp
SecureStorage.Remove("oauth_token");
```

Rufen Sie Folgendes auf, um alle Schlüssel zu entfernen:

```csharp
SecureStorage.RemoveAll();
```

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Der [Android-KeyStore](https://developer.android.com/training/articles/keystore.html) wird verwendet, um den Chiffrierschlüssel zu speichern, mit dem der Wert verschlüsselt wurde, bevor er in einer [freigegebenen Einstellung](https://developer.android.com/training/data-storage/shared-preferences.html) mit einem Dateinamen von **[YOUR-APP-PACKAGE-ID].xamarinessentials** gespeichert wird.  Der in der freigegebenen Einstellungsdatei verwendete Schlüssel (kein kryptografischer Schlüssel, sondern der _Schlüssel_ für den _Wert_) ist ein _MD5-Hash_ des Schlüssels, der an die `SecureStorage`-APIs übergeben wird.

**API-Ebene 23 und höher**

Auf neueren API-Ebenen wird ein **AES**-Schlüssel im Android-KeyStore abgerufen und mit einer **AES/GCM/NoPadding**-Verschlüsselung verwendet, um den Wert zu verschlüsseln, bevor er in der freigegebenen Einstellungsdatei gespeichert wird.

**API-Ebene 22 und niedriger**

Auf älteren API-Ebenen unterstützt der Android-KeyStore nur das Speichern von **RSA**-Schlüsseln, die mit einer **RSA/ECB/PKCS1Padding**-Verschlüsselung verwendet werden, um einen **AES**-Schlüssel zu verschlüsseln (zufällig zur Laufzeit erzeugt), und in der freigegebenen Einstellungsdatei unter dem Schlüssel _SecureStorageKey_ gespeichert werden, wenn noch keiner generiert wurde.

**SecureStorage** verwendet die [Einstellungen](preferences.md)-API und berücksichtigt die in der Dokumentation [Einstellungen](preferences.md#persistence) beschriebene Datenpersistenz. Wenn ein Gerät von API-Ebene 22 oder niedriger auf API-Ebene 23 oder höher aktualisiert wird, wird diese Art der Verschlüsselung weiterhin verwendet, es sei denn, die App wird deinstalliert oder **RemoveAll** aufgerufen.

# <a name="ios"></a>[iOS](#tab/ios)

[KeyChain](xref:Security.SecKeyChain) wird verwendet, um Werte sicher auf iOS-Geräten zu speichern.  Das zum Speichern des Werts verwendete `SecRecord`-Element hat einen `Service`-Wert, der auf **[YOUR-APP-BUNDLE-ID].xamarinessentials** festgelegt ist.

In einigen Fällen werden KeyChain-Daten mit iCloud synchronisiert, und bei der Deinstallation der Anwendung werden die sicheren Werte möglicherweise nicht aus iCloud und anderen Geräten des Benutzers entfernt.

# <a name="uwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) wird zur sicheren Verschlüsselung von Werten auf UWP-Geräten verwendet.

Verschlüsselte Werte werden in `ApplicationData.Current.LocalSettings` gespeichert, in einem Container mit dem Namen **[YOUR-APP-ID].xamarinessentials**.

**SecureStorage** verwendet die [Einstellungen](preferences.md)-API und berücksichtigt die in der Dokumentation [Einstellungen](preferences.md#persistence) beschriebene Datenpersistenz. Es verwendet außerdem `LocalSettings`, das die Einschränkung aufweist, dass der Name jeder einzelnen Einstellung höchstens 255 Zeichen lang sein darf. Jede Einstellung kann bis zu 8 KB groß sein, und jede zusammengesetzte Einstellung kann bis zu 64 KB groß sein.

-----

## <a name="limitations"></a>Einschränkungen

Diese API wurde zum Speichern kleiner Textmengen konzipiert.  Die Leistung ist ggf. langsam, wenn Sie versuchen, damit große Textmengen zu speichern.

## <a name="api"></a>API

- [SecureStorage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage-API-Dokumentation](xref:Xamarin.Essentials.SecureStorage)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Secure-Storage-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
