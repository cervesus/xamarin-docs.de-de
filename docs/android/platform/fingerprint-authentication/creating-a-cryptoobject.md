---
title: Erstellen eine CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-cryptoobject"></a>Erstellen eine CryptoObject

Die Integrität der Ergebnisse Authentifizierung Fingerabdruck ist wichtig, eine Anwendung &ndash; ist wie die Anwendung die Identität des Benutzers kennt. Es ist theoretisch möglich, dass Drittanbieter-Malware abfangen und Manipulieren von der Fingerabdruck Scanner zurückgegebenen Ergebnisse. In diesem Abschnitt wird ein Verfahren zum Beibehalten von der Gültigkeit der Ergebnisse Fingerabdruck erläutert. 

Die `FingerprintManager.CryptoObject` ist ein Wrapper für die Java-Kryptografie-API und dient der `FingerprintManager` , die Integrität der Authentifizierungsanforderung schützen. In der Regel eine `Javax.Crypto.Cipher` Objekt ist ein Mechanismus für die Verschlüsselung von den Ergebnissen des Scanners Fingerabdruck. Die `Cipher` Objekt selbst verwendet einen Schlüssel, die durch die Anwendung mit der Android-Schlüsselspeicher-APIs erstellt wird.

Um zu verstehen, wie diese Klassen, die alle zusammenarbeiten, sehen wir uns zunächst an den folgenden Code die veranschaulicht, wie Sie erstellen eine `CryptoObject`, und klicken Sie dann im Detail erläutert:

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

Der Beispielcode erstellt eine neue `Cipher` für jede `CryptoObject`, mit einem Schlüssel, die von der Anwendung erstellt wurde. Der Schlüssel wird durch identifiziert die `KEY_NAME` Variable, die am Anfang festgelegt wurde die `CryptoObjectHelper` Klasse. Die Methode `GetKey` versuchen wird, und den Schlüssel mithilfe der Android Keystore-APIs abrufen. Wenn der Schlüssel nicht vorhanden ist, klicken Sie dann die Methode `CreateKey` erstellt einen neuen Schlüssel für die Anwendung.

Die Chiffre mit einem Aufruf von instanziiert wird `Cipher.GetInstance`unter Berücksichtigung einer _Transformation_ (ein Zeichenfolgenwert, der dem Verschlüsselungsverfahren teilt mit, wie zum Verschlüsseln und Entschlüsseln von Daten). Der Aufruf von `Cipher.Init` wird die Initialisierung der Verschlüsselung durch Bereitstellen eines Schlüssels aus der Anwendung abgeschlossen. 

Es ist wichtig, beachten Sie, dass es gibt einige Situationen, in denen Android den Schlüssel für ungültig erklären kann: 

* Ein neuer Fingerabdruck wurde mit dem Gerät registriert wurde.
* Es sind keine Fingerabdrücke mit dem Gerät registriert.
* Der Benutzer hat die Bildschirmsperre deaktiviert.
* Der Benutzer hat die Bildschirmsperre (die Art der Screenlock oder das verwendete PIN/Muster) geändert.

In diesem Fall `Cipher.Init` löst eine [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html). Der oben angegebene Beispielcode wird diese Ausnahme abfangen, löschen Sie den Schlüssel und klicken Sie dann ein neues erstellen.

Im nächste Abschnitt wird erläutert, wie die Schlüssel erstellen und speichern Sie sie auf dem Gerät.

## <a name="creating-a-secret-key"></a>Erstellen einen geheimen Schlüssel

Die `CryptoObjectHelper` Klasse verwendet die Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) einen Schlüssel erstellen und speichern Sie sie auf dem Gerät. Die `KeyGenerator` Klasse kann den Schlüssel erstellen, benötigt jedoch einige Metadaten über den Typ des Schlüssels zu erstellen. Diese Informationen werden bereitgestellt, von einer Instanz von der [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) Klasse. 

Ein `KeyGenerator` instanziiert wird, mithilfe der `GetInstance` Factorymethode. Im Beispielcode wird die [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) als Verschlüsselungsalgorithmus. AES wird und die Daten in Blöcke mit einer festen Größe zusammensetzen aller die Blöcke zu verschlüsseln.

Als Nächstes wird ein `KeyGenParameterSpec` erstellt, wobei die `KeyGenParameterSpec.Builder`. Die `KeyGenParameterSpec.Builder` dient als Wrapper für die folgenden Informationen über den Schlüssel, der erstellt werden soll:

* Der Name des Schlüssels.
* Der Schlüssel muss für die Verschlüsselung und Entschlüsselung gültig sein.
* Im Beispielcode die `BLOCK_MODE` festgelegt ist, um _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), was bedeutet, dass jeder Block Pseudozufallsstrom mit dem vorherigen Block (Erstellen von Abhängigkeiten zwischen jeder Block) ist. 
* Die `CryptoObjectHelper` verwendet [ _Public Key Cryptography Standard #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) zum Generieren von Bytes, die aufgefüllt wird, die Blöcke, um sicherzustellen, dass sie alle gleich groß sind .
* `SetUserAuthenticationRequired(true)` bedeutet, dass die Benutzerauthentifizierung erforderlich ist, bevor der Schlüssel verwendet werden kann.

Einmal die `KeyGenParameterSpec` wird erstellt, es dient zum Initialisieren der `KeyGenerator`, generieren Sie einen Schlüssel zu lagern auf dem Gerät. 

## <a name="using-the-cryptoobjecthelper"></a>Verwenden die CryptoObjectHelper

Nun, dass ein Großteil der Logik für das Erstellen von Beispielcode gekapselt werden verfügt über eine `CryptoWrapper` in der `CryptoObjectHelper` Klasse, die wir der Code vom Anfang dieses Handbuchs und Verwenden der `CryptoObjectHelper` das Verschlüsselungsverfahren zu erstellen und starten einen Fingerabdruck-Scanner: 

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

Nun, wir gesehen haben, zum Erstellen einer `CryptoObject`, zu finden Sie unter wechseln, können wie die `FingerprintManager.AuthenticationCallbacks` werden verwendet, um die Ergebnisse des Fingerabdruck Scanner-Diensts in einer Android-Anwendung übertragen.



## <a name="related-links"></a>Verwandte Links

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
