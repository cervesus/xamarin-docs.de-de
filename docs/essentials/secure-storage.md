---
title: Xamarin.Essentials sichere Speicherung
description: Die Klasse SecureStorage hilft Lagern Sie einfache Schlüssel/Wert-Paaren.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
ms.technology: xamarin-crossplatform
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 23b38721d1437edba611dc6575593b83e2d4a548
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials sichere Speicherung

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **SecureStorage** Klasse hilft Lagern Sie einfache Schlüssel/Wert-Paaren.

## <a name="using-secure-storage"></a>Verwenden die sichere Speicherung

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Speichern Sie einen Wert für einen bestimmten _Schlüssel_ im sicheren Speicher:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Zum Abrufen eines Werts aus dem sicheren Speicher:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die [Android KeyStore](https://developer.android.com/training/articles/keystore.html) dient zum Speichern des verschlüsselte Schlüssels verwendet, um den Wert zu verschlüsseln, bevor er in gespeichert ist ein [freigegebenen Voreinstellungen](https://developer.android.com/training/data-storage/shared-preferences.html) mit einem Dateinamen **.xamarinessentials [YOUR-APP-Paket-ID]** .  Der in der Datei gemeinsam genutzte Einstellungen verwendete Schlüssel ist ein _MD5-Hash_ des Schlüssels an übergeben der `SecureStorage` -APIs.

## <a name="api-level-23-and-higher"></a>API-Ebene 23 und höher

Auf neueren API-Ebenen ein **AES** Schlüssel aus der Android-Schlüsselspeicher abgerufen und verwendet ein **AES/GCM/NoPadding** Cipher, um den Wert zu verschlüsseln, bevor er in der gemeinsam genutzte Einstellungen-Datei gespeichert wird.

## <a name="api-level-22-and-lower"></a>API-Ebene 22 und niedriger

Auf älteren API-Ebenen des Android KeyStore nur unterstützt das Speichern von **RSA** Schlüssel, der verwendet wird ein **RSA/ECB/PKCS1Padding** Cipher zum Verschlüsseln einer **AES** Schlüssel (nach dem Zufallsprinzip zur Laufzeit generierten) und in der Datei gemeinsam genutzte Einstellungen unter dem Schlüssel gespeichert _SecureStorageKey_, sofern noch nicht generiert wurde.

Alle verschlüsselten Werte werden entfernt, wenn die app vom Gerät deinstalliert wird.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Schlüsselbund](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) dient zum sicheren Speichern von Werten auf iOS-Geräten.  Die `SecRecord` verwendet zum Speichern des Werts hat eine `Service` Wert festgelegt wird, um **[YOUR-APP-Bündel-ID] .xamarinessentials**.

In einigen Fällen Schlüsselsammlung mit iCloud synchronisiert ist, und Deinstallieren der Anwendung möglicherweise nicht die sichere Werte von iCloud und anderen Geräten des Benutzers entfernen.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) wird verwendet, um Encryped Werte sicher auf uwp-Geräten.

Encryped Werte werden im gespeichert `ApplicationData.Current.LocalSettings`, innerhalb eines Containers mit einem Namen eines **[YOUR-APP-ID] .xamarinessentials**.

Deinstallieren der Anwendung führt dazu, dass die _LocalSettings_, sowie alle verschlüsselten Werte auch entfernt werden soll.

-----

## <a name="limitations"></a>Einschränkungen

Diese API ist vorgesehen, um kleine Mengen von Text zu speichern.  Leistung ist möglicherweise langsam, wenn Sie versuchen, diese zum Speichern großer Textmengen nutzen.

## <a name="api"></a>API

- [SecureStorage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [SecureStorage API-Dokumentation](xref:Xamarin.Essentials.SecureStorage)
