---
title: 'Xamarin.Essentials: Sicherer Speicher'
description: Dieses Dokument beschreibt die SecureStorage-Klasse in Xamarin.Essentials, die Ihnen hilft, einfache Schlüssel/Wert-Paare sicher zu speichern. Es wird erläutert, wie die Klasse, Plattformeigenschaften-Implementierung und Einschränkungen zu verwenden.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: fae5f5f0f15d80e2f3bdce26b8beb5f6fae2f81f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830452"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Sicherer Speicher

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **SecureStorage** Klasse hilft, einfache Schlüssel/Wert-Paare sicher zu speichern.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **SecureStorage** Funktionalität, die die folgende plattformspezifischen-Einrichtung ist erforderlich:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aktivieren Sie bei der Entwicklung auf iOS-Simulator die **Keychain** Berechtigung, und fügen Sie eine Zugriffsgruppe für die Bündel-ID der Anwendung hinzu.

Öffnen der **"Entitlements.plist"** im iOS-Projekt, und suchen die **Keychain** Berechtigung und aktivieren Sie ihn. Dadurch wird automatisch die Bezeichner von der Anwendung als Gruppe hinzugefügt.

In den Projekteigenschaften unter **iOS Bundle-Signierung** legen Sie die **benutzerdefinierte Berechtigungen** zu **"Entitlements.plist"**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-secure-storage"></a>Mit dem sicheren Speicher

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Speichern Sie einen Wert für einen bestimmten _Schlüssel_ an einem sicheren Speicherort:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Zum Abrufen eines Werts aus dem sicheren Speicher:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

> [!NOTE]
> Es ist kein Wert, der den angeforderten Schlüssel zugeordnete `GetAsync` zurück `null`.

Um einen bestimmten Schlüssel zu entfernen, rufen Sie ein:

```csharp
SecureStorage.Remove("oauth_token");
```

Um alle Schlüssel zu entfernen, rufen Sie ein:

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die [Android-KeyStore](https://developer.android.com/training/articles/keystore.html) dient zum Speichern des Cipher-Schlüssels verwendet, um den Wert zu verschlüsseln, bevor es gespeichert wird eine [freigegebene Einstellungen](https://developer.android.com/training/data-storage/shared-preferences.html) mit einem Dateinamen **[YOUR-APP-Paket-ID] .xamarinessentials** .  Der Schlüssel in der freigegebenen Einstellungsdatei verwendet eine _MD5-Hash_ des Schlüssels übergeben die `SecureStorage` -APIs.

## <a name="api-level-23-and-higher"></a>API-Ebene 23 oder höher

Auf neueren API-Ebenen einer **AES** Schlüssel aus der Android-KeyStore abgerufen und verwendet eine **AES/GCM/NoPadding** Verschlüsselungsverfahren, die den Wert zu verschlüsseln, bevor sie in der freigegebenen Einstellungsdatei gespeichert ist.

## <a name="api-level-22-and-lower"></a>API-Ebene 22 und niedriger

Für ältere API-Ebenen, nur die Android-KeyStore unterstützt das Speichern von **RSA** Schlüssel, der verwendet wird ein **RSA/ECB/PKCS1Padding** Verschlüsselung zum Verschlüsseln einer **AES** Schlüssel (nach dem Zufallsprinzip zur Laufzeit generiert wird) und in der freigegebenen Einstellungsdatei unter dem Schlüssel gespeichert _SecureStorageKey_, wenn eine nicht bereits generiert wurde.

Alle verschlüsselten Werte werden entfernt werden, wenn die app vom Gerät deinstalliert wird.

# <a name="iostabios"></a>[iOS](#tab/ios)

[KeyChain](https://developer.xamarin.com/api/type/Security.SecKeyChain/) wird verwendet, um die Werte auf iOS-Geräten sicher zu speichern.  Die `SecRecord` verwendet zum Speichern des Werts hat eine `Service` Wert festgelegt wird, um **[YOUR-APP-Bündel-ID] .xamarinessentials**.

In einigen Fällen KeyChain-Daten mit iCloud synchronisiert werden, und Deinstallieren der Anwendung möglicherweise nicht die sicheren Werte von iCloud und anderen Geräten des Benutzers entfernen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) wird verwendet, um verschlüsselter Werte sicher auf UWP-Geräte.

Verschlüsselter Werte werden im gespeichert `ApplicationData.Current.LocalSettings`, innerhalb eines Containers mit dem Namen **[YOUR-APP-ID] .xamarinessentials**.

Deinstallieren der Anwendung führt dazu, dass die _LocalSettings_, sowie alle verschlüsselten Werte auch entfernt werden soll.

-----

## <a name="limitations"></a>Einschränkungen

Diese API ist vorgesehen, um kleine Mengen an Text zu speichern.  Leistung ist möglicherweise langsam, wenn Sie versuchen, die sie verwenden, um große Mengen an Text zu speichern.

## <a name="api"></a>API

- [SecureStorage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage-API-Dokumentation](xref:Xamarin.Essentials.SecureStorage)
