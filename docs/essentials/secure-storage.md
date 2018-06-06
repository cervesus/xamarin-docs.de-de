---
title: 'Xamarin.Essentials: Sicherer Speicher'
description: Dieses Dokument beschreibt die Klasse SecureStorage in Xamarin.Essentials, was hilft, lagern Sie einfache Schlüssel/Wert-Paaren. Es wird erläutert, wie die Klasse, die Plattform implementierungsspezifika und die Einschränkungen zu verwenden.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: d9fd5b5fd0d4dc29f4d2531521370618f97e3846
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783157"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: Sicherer Speicher

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **SecureStorage** Klasse hilft Lagern Sie einfache Schlüssel/Wert-Paaren.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **SecureStorage** Funktionalität, die folgenden plattformspezifischen Setup erforderlich ist:

# <a name="androidtabandroid"></a>[Android](#tab/android)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="iostabios"></a>[iOS](#tab/ios)

Aktivieren Sie bei der Entwicklung für iOS-Simulator die **Schlüsselbund** Rechtsansprüche und hinzufügen eine Zugriffsgruppe für die Paket-ID der Anwendung.

Öffnen der **Entitlements.plist** im iOS-Projekt, und suchen die **Schlüsselbund** Rechtsansprüche und aktivieren Sie ihn. Dadurch wird die Anwendungs-ID automatisch als eine Gruppe hinzugefügt.

In den Projekteigenschaften unter **iOS Bundle Signing** legen Sie die **benutzerdefinierte Ansprüche** auf **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

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

- [SecureStorage-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API-Dokumentation](xref:Xamarin.Essentials.SecureStorage)
