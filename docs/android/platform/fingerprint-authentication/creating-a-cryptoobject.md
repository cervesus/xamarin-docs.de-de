---
title: Erstellen eine CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1305a8a1f39d34b5e91e478a769750911afb2b3e
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669637"
---
# <a name="creating-a-cryptoobject"></a>Erstellen eine CryptoObject

Die Integrität der das Ergebnis der Authentifizierung per Fingerabdruck ist wichtig, eine Anwendung &ndash; ist wie die Anwendung für die Identität des Benutzers kennt. Es ist theoretisch möglich, dass Drittanbieter-Malware abfangen und Manipulieren von des fingerabdruckscanners zurückgegebenen Ergebnisse. Dieser Abschnitt wird erläutert, dass eine Methode, um die Gültigkeit der Fingerabdruck Ergebnisse beibehalten. 

Die `FingerprintManager.CryptoObject` ist ein Wrapper um die Kryptografie-APIs von Java und dient der `FingerprintManager` zum Schutz der Integrität der Authentifizierungsanforderung. In der Regel eine `Javax.Crypto.Cipher` Objekt ist der Mechanismus für die Ergebnisse der Überprüfung per Fingerabdruck zu verschlüsseln. Die `Cipher` Objekt selbst wird verwenden Sie einen Schlüssel, die von der Anwendung, die mit der Android-Keystore-APIs erstellt wird.

Um zu verstehen, wie diese Klassen, die alle zusammenarbeiten, sehen wir uns zunächst an den folgenden Code, das zeigt, wie zum Erstellen einer `CryptoObject`, und klicken Sie dann im Detail erläutert:

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

Der Beispielcode erstellt eine neue `Cipher` für jede `CryptoObject`, mit einem Schlüssel, die von der Anwendung erstellt wurde. Der Schlüssel wird durch identifiziert die `KEY_NAME` Variable, die am Anfang des festgelegt wurde die `CryptoObjectHelper` Klasse. Die Methode `GetKey` wird, versuchen Sie es, und den Schlüssel mithilfe der Android-Keystore-APIs abrufen. Wenn der Schlüssel ist nicht vorhanden, klicken Sie dann die Methode `CreateKey` erstellt einen neuen Schlüssel für die Anwendung.

Die Chiffre mit einem Aufruf von instanziiert `Cipher.GetInstance`, wobei eine _Transformation_ (ein Zeichenfolgenwert, der dem Verschlüsselungsverfahren erfahren, wie Sie Daten verschlüsseln und entschlüsseln). Der Aufruf von `Cipher.Init` wird schließen Sie die Initialisierung der Verschlüsselung, durch die Angabe eines Schlüssels aus der Anwendung. 

Es ist wichtig zu wissen, dass es gibt einige Situationen, in denen Android den Schlüssel für ungültig erklären kann: 

* Ein neuer Fingerabdruck wurde mit dem Gerät registriert.
* Es sind keine Fingerabdrücke mit dem Gerät registriert.
* Der Benutzer hat die Bildschirmsperre deaktiviert.
* Der Benutzer wurde der Bildschirmsperre (in den Typ der Screenlock oder die PIN/Pattern verwendet) geändert.

In diesem Fall `Cipher.Init` löst eine [ `KeyPermanentlyInvalidatedException` ](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Der obige Beispielcode wird diese Ausnahme abfangen, löschen Sie den Schlüssel und klicken Sie dann eine neue erstellen.

Im nächste Abschnitt wird beschrieben, wie die Schlüssel erstellen und speichern Sie sie auf dem Gerät.

## <a name="creating-a-secret-key"></a>Erstellen eines geheimen Schlüssels

Die `CryptoObjectHelper` Klasse verwendet die Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) um einen Schlüssel erstellen und auf dem Gerät zu speichern. Die `KeyGenerator` Klasse kann den Schlüssel erstellen, muss jedoch einige Metadaten über den Typ des zu erstellenden Schlüssels. Diese Informationen werden bereitgestellt, von einer Instanz der [ `KeyGenParameterSpec` ](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) Klasse. 

Ein `KeyGenerator` instanziiert wird, mit der `GetInstance` Factorymethode. Der Beispielcode verwendet die [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) als Verschlüsselungsalgorithmus. AES wird aufteilen die Daten in Blöcke mit einer festen Größe, und jeder dieser Blöcke zu verschlüsseln.

Als Nächstes wird ein `KeyGenParameterSpec` wurde mit der `KeyGenParameterSpec.Builder`. Die `KeyGenParameterSpec.Builder` dient als Wrapper für die folgenden Informationen über den Schlüssel, der erstellt werden soll:

* Der Name des Schlüssels.
* Der Schlüssel muss für die Verschlüsselung und Entschlüsselung gültig sein.
* Im Beispielcode die `BLOCK_MODE` nastaven NA hodnotu _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), was bedeutet, dass jeder Block Pseudozufallsstrom mit dem vorherigen Block (Erstellen von Abhängigkeiten zwischen jedem Block) ist. 
* Die `CryptoObjectHelper` verwendet [ _Public Key Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) Bytes zu generieren, die aufgefüllt wird, stellen Sie sicher, dass sie alle gleich groß sind die Blöcke .
* `SetUserAuthenticationRequired(true)` bedeutet, dass die Benutzerauthentifizierung erforderlich ist, bevor der Schlüssel verwendet werden kann.

Nach der `KeyGenParameterSpec` wird erstellt, es dient zum Initialisieren der `KeyGenerator`, der einen Schlüssel generieren und speichern Sie ihn sicher auf dem Gerät. 

## <a name="using-the-cryptoobjecthelper"></a>Verwenden die CryptoObjectHelper

Nun, dass ein Großteil der Logik für das Erstellen von Beispielcode gekapselt werden verfügt über eine `CryptoWrapper` in die `CryptoObjectHelper` Klasse, die wir noch einmal den Code am Anfang dieser Anleitung und Verwenden der `CryptoObjectHelper` erstellen das Verschlüsselungsverfahren und starten eine fingerabdruckscanners: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Nun, wir gesehen haben, wie erstellen Sie eine `CryptoObject`, ermöglicht, fahren Sie mit finden Sie unter wie die `FingerprintManager.AuthenticationCallbacks` werden verwendet, um die Ergebnisse der Fingerprint-Überprüfungsdienst an eine Android-Anwendung zu übertragen.



## <a name="related-links"></a>Verwandte Links

- [Verschlüsselungsstärke](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
