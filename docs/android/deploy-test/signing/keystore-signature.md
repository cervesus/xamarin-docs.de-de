---
title: Ermitteln Ihrer Keystoresignatur
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 46b43e6689f751c4fac1e8668234fce7f953521e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764637"
---
# <a name="finding-your-keystores-signature"></a>Ermitteln Ihrer Keystoresignatur

Die MD5- oder SHA1-Signatur einer Xamarin.Android-Anwendung hängt von der **KEYSTORE**-Datei ab, die Sie zum Signieren des APK verwendet haben. Normalerweise unterscheiden sich die **KEYSTORE**-Dateien, die in einem Debug- und in einem Releasebuild verwendet werden.

## <a name="for-debug--non-custom-signed-builds"></a>Debug-/Nicht benutzerdefinierte signierte Builds

Xamarin.Android signiert alle Debugbuilds mit derselben **debug.keystore**-Datei. Diese Datei wird generiert, wenn Xamarin.Android zum ersten Mal installiert wird. Die unten aufgeführten Schritte beschreiben genauer, wie Sie die MD5- oder SHA1-Signatur der Standard-Xamarin.Android-Datei **debug.keystore** finden.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Machen Sie die Xamarin-Datei **debug.keystore** ausfindig, die zum Signieren der App verwendet wir. Standardmäßig befindet sich der Keystore, der zum Signieren von Debugversionen einer Xamarin.Android-Anwendung verwendet wird, an folgendem Speicherort:

**C:\\Benutzer\\*BENUTZERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

Durch das Ausführen des Befehls `keytool.exe` über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise an folgendem Speicherort:

**C:\\Programme (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

Fügen Sie das Verzeichnis, das **keytool.exe** enthält, der Umgebungsvariablen `PATH` hinzu.
Öffnen Sie eine **Eingabeaufforderung**, und führen Sie `keytool.exe` mit dem folgenden Befehl aus:

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

Bei der Ausführung sollte **keytool.exe** den folgenden Text ausgeben. Die Bezeichnungen **MD5:** und **SHA1:** bestimmen die jeweilige Signatur:

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Machen Sie die Xamarin-Datei **debug.keystore** ausfindig, die zum Signieren der App verwendet wir. Standardmäßig befindet sich der Keystore, der zum Signieren von Debugversionen einer Xamarin.Android-Anwendung verwendet wird, an folgendem Speicherort:

**~/.local/share/Xamarin/Mono for Android/debug.keystore**


Durch das Ausführen des Befehls **keytool** über das JDK erhalten Sie Informationen zu einem Keystore. Dieses Tool befindet sich normalerweise an folgendem Speicherort:

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

Fügen Sie das Verzeichnis, das **keytool** enthält, der Umgebungsvariablen **PATH** hinzu.
Öffnen Sie einen **Terminal**, und führen Sie **keytool** mit dem folgenden Befehl aus:

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

Bei der Ausführung sollte **keytool** den folgenden Text ausgeben. Die Bezeichnungen **MD5:** und **SHA1:** bestimmen die jeweilige Signatur:

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>Release-/Benutzerdefinierte signierte Builds

Der Vorgang für Releasebuilds, die mit einer benutzerdefinierten **KEYSTORE**-Datei signiert werden, ist der gleiche wie oben. Dabei ersetzt die **KEYSTORE**-Releasedatei die Datei **debug.keystore**, die von Xamarin.Android verwendet wird. Für das Keystorekennwort und den Aliasnamen von der Erstellung der KEYSTORE-Releasedatei können Sie Ihre eigenen Werte eingeben.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Wenn Sie den Visual Studio-Assistenten **Verteilen** verwenden, um eine Xamarin.Android-Anwendung zu signieren, befindet sich der entstandene Keystore an folgendem Speicherort:

**C:\\Benutzer\\*BENUTZERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\alias\\alias.keystore**

Wenn Sie beispielsweise die Schritte in [Erstellen eines neuen Zertifikats](~/android/deploy-test/signing/index.md#newcertvs) durchgeführt haben, um einen neuen signierten Schlüssel zu erstellen, befindet sich der erstellte Keystore an folgendem Speicherort:

**C:\\Benutzer\\*BENUTZERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\chimp\\chimp.keystore**

Weitere Informationen zum Signieren einer Xamarin.Android-Anwendung finden Sie unter [Signing the Android Application Package (Signieren des Android-Anwendungspakets)](~/android/deploy-test/signing/index.md).


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Wenn der Visual Studio für Mac-Assistent **Signieren und Verteilen...** zum Signieren Ihrer App verwendet wird, befindet sich der entstandene Keystore an folgendem Speicherort:

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

Wenn Sie beispielsweise die Schritte in [Erstellen eines neuen Zertifikats](~/android/deploy-test/signing/index.md#newcertxs) durchgeführt haben, um einen neuen signierten Schlüssel zu erstellen, befindet sich der erstellte Keystore an folgendem Speicherort:

**~/Bibliothek/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Weitere Informationen zum Signieren einer Xamarin.Android-Anwendung finden Sie unter [Signing the Android Application Package (Signieren des Android-Anwendungspakets)](~/android/deploy-test/signing/index.md).


-----
