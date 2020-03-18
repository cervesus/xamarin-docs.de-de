---
title: Erstellen eines CryptoObject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "75487775"
---
# <a name="creating-a-cryptoobject"></a>Erstellen eines CryptoObject

Die Integrität der Ergebnisse der Fingerabdruck-Authentifizierung ist für eine Anwendung wichtig &ndash; die Anwendung erkennt so die Identität des Benutzers. Es ist theoretisch möglich, dass Schadsoftware von Drittanbietern die vom Fingerabdruckscanner zurückgegebenen Ergebnisse abfängt und manipuliert. In diesem Abschnitt wird eine Methode zum Beibehalten der Gültigkeit der Fingerabdruckergebnisse erläutert. 

`FingerprintManager.CryptoObject` ist ein Wrapper um die Java-Kryptografie-APIs und wird von `FingerprintManager` verwendet, um die Integrität der Authentifizierungsanforderung zu schützen. In der Regel stellt ein `Javax.Crypto.Cipher`-Objekt den Mechanismus für die Verschlüsselung der Ergebnisse des Fingerabdruckscanners dar. Das `Cipher`-Objekt selbst verwendet einen Schlüssel, der von der Anwendung mithilfe der Android-Keystore-APIs erstellt wird.

Um zu verstehen, wie diese Klassen zusammenarbeiten, sehen Sie sich zunächst den folgenden Code an, der veranschaulicht, wie Sie ein `CryptoObject` erstellen. Dieser Vorgang wird anschließend ausführlicher erläutert:

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
            cipher.Init(CipherMode.EncryptMode, key);
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

Der Beispielcode erstellt ein neues `Cipher`-Element für jedes `CryptoObject` mithilfe eines Schlüssels, der von der Anwendung erstellt wurde. Der Schlüssel wird durch die `KEY_NAME`-Variable identifiziert, die am Anfang der `CryptoObjectHelper`-Klasse festgelegt wurde. Die Methode `GetKey` versucht, den Schlüssel mithilfe der Android-Keystore-APIs abzurufen. Wenn der Schlüssel nicht vorhanden ist, erstellt die Methode `CreateKey` einen neuen Schlüssel für die Anwendung.

Die Verschlüsselung wird mit einem Aufruf von `Cipher.GetInstance` instanziiert und nimmt eine _Transformation_ an (ein Zeichenfolgenwert, der die Verschlüsselung informiert, wie Daten verschlüsselt und entschlüsselt werden). Der Aufruf von `Cipher.Init` schließt die Initialisierung der Verschlüsselung ab, indem ein Schlüssel aus der Anwendung bereitgestellt wird. 

Beachten Sie, dass es einige Situationen gibt, in denen Android den Schlüssel für ungültig erklären kann: 

- Ein neuer Fingerabdruck wurde für das Gerät registriert.
- Es sind keine Fingerabdrücke für das Gerät registriert.
- Der Benutzer hat die Bildschirmsperre deaktiviert.
- Der Benutzer hat die Bildschirmsperre geändert (den Typ der Bildschirmsperre oder die verwendete PIN bzw. das Muster).

In diesem Fall löst `Cipher.Init` eine [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html) aus. Im obigen Beispielcode wird diese Ausnahme abgefangen, der Schlüssel gelöscht und anschließend ein neuer Schlüssel erstellt.

Im nächsten Abschnitt wird erläutert, wie der Schlüssel erstellt und auf dem Gerät gespeichert wird.

## <a name="creating-a-secret-key"></a>Erstellen eines geheimen Schlüssels

Die `CryptoObjectHelper`-Klasse verwendet den Android-[`KeyGenerator`](xref:Javax.Crypto.KeyGenerator), um einen Schlüssel zu erstellen und auf dem Gerät zu speichern. Die `KeyGenerator`-Klasse kann den Schlüssel erstellen, benötigt jedoch einige Metadaten zum Typ des zu erstellenden Schlüssels. Diese Informationen werden von einer Instanz der [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)-Klasse bereitgestellt. 

Ein `KeyGenerator` wird mit der `GetInstance`-Factorymethode instanziiert. Im Beispielcode wird [_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) als Verschlüsselungsalgorithmus verwendet. AES teilt die Daten in Blöcke mit fester Größe auf und verschlüsselt jeden dieser Blöcke.

Danach wird eine `KeyGenParameterSpec` mit `KeyGenParameterSpec.Builder` erstellt. `KeyGenParameterSpec.Builder` schließt die folgenden Informationen zum Schlüssel, der erstellt werden soll, in einen Wrapper ein:

- Der Name des Schlüssels.
- Der Schlüssel muss sowohl für Verschlüsselung als auch für Entschlüsselung gültig sein.
- Im Beispielcode wird der `BLOCK_MODE` auf _Cipher Block Chaining (Blockchiffreverkettung)_ (`KeyProperties.BlockModeCbc`) festgelegt, was bedeutet, dass jeder Block mit dem vorherigen Block über XOR verkettet wird (und so Abhängigkeiten zwischen allen Blöcken erstellt werden). 
- `CryptoObjectHelper` verwendet [_Public Key Cryptography Standard Nr. 7_](https://tools.ietf.org/html/rfc2315) (_PKCS7_), um die Bytes zu generieren, die die Blöcke auffüllen, um sicherzustellen, dass sie alle die gleiche Größe aufweisen.
- `SetUserAuthenticationRequired(true)` bedeutet, dass Benutzerauthentifizierung erforderlich ist, bevor der Schlüssel verwendet werden kann.

Nachdem die `KeyGenParameterSpec` erstellt wurde, wird sie verwendet, um `KeyGenerator` zu initialisieren, der einen Schlüssel generiert und diesen sicher auf dem Gerät speichert. 

## <a name="using-the-cryptoobjecthelper"></a>Verwenden von CryptoObjectHelper

Da der Beispielcode nun einen Großteil der Logik zum Erstellen eines `CryptoWrapper` in der `CryptoObjectHelper`-Klasse gekapselt hat, sehen wir uns den Code vom Anfang dieses Leitfadens an und verwenden `CryptoObjectHelper`, um die Verschlüsselung zu erstellen und einen Fingerabdruckscanner zu starten: 

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

Nachdem Sie nun erfahren haben, wie Sie ein `CryptoObject`erstellen, können Sie sich mit der Verwendung der `FingerprintManager.AuthenticationCallbacks` für das Übertragen der Ergebnisse des Fingerabdruck-Scannerdiensts auf eine Android-Anwendung befassen.

## <a name="related-links"></a>Verwandte Links

- [Verschlüsselung](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315: PCKS #7](https://tools.ietf.org/html/rfc2315)
