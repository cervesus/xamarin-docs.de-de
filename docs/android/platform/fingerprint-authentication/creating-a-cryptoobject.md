---
title: Erstellen eines cryptoobject
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7328792e0d921beb09389d9a0400ea97766b2246
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756445"
---
# <a name="creating-a-cryptoobject"></a>Erstellen eines cryptoobject

Die Integrität der Ergebnisse der Fingerabdruckauthentifizierung ist wichtig für eine &ndash; Anwendung, dass die Anwendung die Identität des Benutzers kennt. Es ist theoretisch möglich, dass Drittanbieter-Schadsoftware die vom Fingerabdruckscanner zurückgegebenen Ergebnisse abfängt und manipulieren. In diesem Abschnitt wird eine Methode zum Beibehalten der Gültigkeit der Fingerabdruck Ergebnisse erläutert. 

Der `FingerprintManager.CryptoObject` ist ein Wrapper um die Java-kryptografieapis und wird `FingerprintManager` vom verwendet, um die Integrität der Authentifizierungsanforderung zu schützen. In der Regel `Javax.Crypto.Cipher` stellt ein-Objekt den Mechanismus zum Verschlüsseln der Ergebnisse des Fingerabdruckscanners dar. Das `Cipher` Objekt selbst verwendet einen Schlüssel, der von der Anwendung mithilfe der Android Keystore-APIs erstellt wird.

Um zu verstehen, wie diese Klassen zusammenarbeiten, sehen wir uns zunächst den folgenden Code an, der veranschaulicht, wie `CryptoObject`ein erstellt wird, und dann ausführlicher erläutert wird:

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

Der Beispielcode erstellt ein neues `Cipher` für jede `CryptoObject`mithilfe eines Schlüssels, der von der Anwendung erstellt wurde. Der Schlüssel wird durch die `KEY_NAME` Variable identifiziert, die am Anfang `CryptoObjectHelper` der Klasse festgelegt wurde. Die Methode `GetKey` versucht, den Schlüssel mithilfe der Android-Keystore-APIs abzurufen. Wenn der Schlüssel nicht vorhanden ist, erstellt die- `CreateKey` Methode einen neuen Schlüssel für die Anwendung.

Die Chiffre wird mit einem `Cipher.GetInstance`-Befehl instanziiert, wobei eine _Transformation_ (ein Zeichen folgen Wert, der die Chiffre angibt, wie Daten verschlüsselt und entschlüsselt werden) wird. Durch den-Vorgang wirddieInitialisierungderVerschlüsselungdurchBereitstelleneinesSchlüsselsausderAnwendungbeendet.`Cipher.Init` 

Beachten Sie, dass es einige Situationen gibt, in denen Android den Schlüssel ungültig machen kann: 

- Ein neuer Fingerabdruck wurde bei dem Gerät registriert.
- Es sind keine Fingerabdrücke für das Gerät registriert.
- Der Benutzer hat die Bildschirmsperre deaktiviert.
- Der Benutzer hat die Bildschirmsperre geändert (der Typ der Bildschirmsperre oder des verwendeten Pin/Musters).

In diesem Fall `Cipher.Init` wird von eine [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)ausgelöst. Im obigen Beispielcode wird diese Ausnahme abgefangen, der Schlüssel gelöscht und anschließend ein neuer erstellt.

Im nächsten Abschnitt wird erläutert, wie der Schlüssel erstellt und auf dem Gerät gespeichert wird.

## <a name="creating-a-secret-key"></a>Erstellen eines geheimen Schlüssels

Die `CryptoObjectHelper` -Klasse verwendet Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) , um einen Schlüssel zu erstellen und auf dem Gerät zu speichern. Die `KeyGenerator` Klasse kann den Schlüssel erstellen, benötigt jedoch einige Metadaten zum Typ des zu erstellenden Schlüssels. Diese Informationen werden von einer Instanz der [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) -Klasse bereitgestellt. 

Eine `KeyGenerator` wird mithilfe der `GetInstance` Factory-Methode instanziiert. Im Beispielcode wird der [_Advanced Encryption Standard_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) als Verschlüsselungsalgorithmus verwendet. AES teilt die Daten in Blöcke mit fester Größe auf und verschlüsselt jeden dieser Blöcke.

Als nächstes wird `KeyGenParameterSpec` eine mit dem `KeyGenParameterSpec.Builder`erstellt. `KeyGenParameterSpec.Builder` Umschließt die folgenden Informationen über den Schlüssel, der erstellt werden soll:

- Der Name des Schlüssels.
- Der Schlüssel muss sowohl für die Verschlüsselung als auch für die Entschlüsselung gültig sein.
- Im Beispielcode wird der `BLOCK_MODE` auf _Chiffre Block Verkettung_ (`KeyProperties.BlockModeCbc`) festgelegt, was bedeutet, dass jeder Block mit dem vorherigen Block XoReD ist (das Erstellen von Abhängigkeiten zwischen den einzelnen Blöcken). 
- Der `CryptoObjectHelper` verwendet die Verschlüsselungs [_Standard-#7 (Public Key Cryptography Standard_](https://tools.ietf.org/html/rfc2315) ) (_PKCS7_), um die Blöcke zu generieren, die die Blöcke auffüllen, um sicherzustellen, dass Sie die gleiche Größe haben.
- `SetUserAuthenticationRequired(true)`bedeutet, dass eine Benutzerauthentifizierung erforderlich ist, bevor der Schlüssel verwendet werden kann.

Nachdem der `KeyGenParameterSpec` erstellt wurde, wird er verwendet, um den `KeyGenerator`zu initialisieren, der einen Schlüssel generiert und ihn sicher auf dem Gerät speichert. 

## <a name="using-the-cryptoobjecthelper"></a>Verwenden von cryptoobjecthelper

Nachdem der Beispielcode einen Großteil der Logik zum Erstellen eines `CryptoWrapper` in die `CryptoObjectHelper` Klasse gekapselt hat, sehen wir uns den Code vom Anfang dieses Handbuchs an und verwenden den `CryptoObjectHelper` , um die Verschlüsselung zu erstellen und einen Fingerabdruckscanner zu starten: 

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

Nachdem Sie nun gesehen haben, wie ein `CryptoObject`erstellt wird, können Sie mit fortfahren, um zu erfahren, wie die `FingerprintManager.AuthenticationCallbacks` Ergebnisse des Fingerabdruck-Überprüfungs Dienstanbieter in eine Android-Anwendung übertragen werden.

## <a name="related-links"></a>Verwandte Links

- [Verschlüsselungs](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [KAK](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315-PCKS-#7](https://tools.ietf.org/html/rfc2315)
