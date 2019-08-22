---
title: Manuelles Signieren des APK
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d20ec990253ff86e7b426baad8da5a919a91ef6c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525020"
---
# <a name="manually-signing-the-apk"></a>Manuelles Signieren des APK


Nachdem die Anwendung für die Veröffentlichung erstellt wurde, muss das APK vor der Verteilung signiert werden, damit es auf einem Android-Gerät ausgeführt werden kann. Dieser Prozess wird in der Regel von der IDE ausgeführt. Es gibt jedoch einige Situationen, in denen es erforderlich ist, das APK manuell in der Befehlszeile zu signieren. Der Signaturvorgang eines APK besteht aus den folgenden Schritten:

1. **Erstellen eines privaten Schlüssels** &ndash; Dieser Schritt muss nur einmal durchgeführt werden. Ein privater Schlüssel ist für das digitale Signieren des APKs erforderlich.
    Nachdem der private Schlüssel vorbereitet wurde, kann dieser Schritt für alle weiteren Releasebuilds übersprungen werden.

2. **Ausrichten der Anwendung mit Zipalign** &ndash; *Zipalign* ist der Optimierungsvorgang einer Anwendung. Dadurch kann Android zur Laufzeit effizienter mit dem APK interagieren. Xamarin.Android führt zur Laufzeit eine Prüfung der Anwendung durch und verhindert das Ausführen dieser, wenn APK nicht mit Zipalign ausgerichtet wurde.

3. **Signieren des APK** &ndash; In diesem Schritt verwenden Sie das Dienstprogramm **apksigner** aus dem Android SDK und signieren das APK mit dem privaten Schlüssel, den Sie im vorherigen Schritt erstellt haben. Anwendungen, die mit älteren Versionen des Android SDK-Buildtools vor v24.0.3 entwickelt wurden, verwenden die **jarsigner**-App aus dem JDK. Beide Tools werden im Folgenden ausführlicher erläutert. 

Die Reihenfolge der Schritte ist wichtig und hängt davon ab, welches Tool zum Signieren des APK verwendet wird. Bei der Verwendung von **apksigner** ist es wichtig, die Anwendung zuerst mit **Zipalign** auszurichten und sie dann mit **Apksigner** zu signieren.  Sollte es nötig sein, **jarsigner** zum Signieren des APK zu verwenden, ist es wichtig, zunächst das APK zu signieren und dann **Zipalign** auszuführen. 



## <a name="prerequisites"></a>Erforderliche Komponenten

Dieser Leitfaden konzentriert sich auf die Verwendung von **apksigner** aus den Android SDK-Buildtools, v24.0.3 oder höher. Es wird davon ausgegangen, dass bereits ein APK erstellt wurde.

Anwendungen, die mit einer älteren Version der Android SDK-Buildtools erstellt werden, müssen wie unten in [Signieren des APK mit jarsigner](#Sign_the_APK_with_jarsigner) beschrieben **jarsigner** verwenden.



## <a name="create-a-private-keystore"></a>Erstellen eines privaten Keystores

Ein *Keystore* ist eine Datenbank mit Sicherheitszertifikaten, die mit dem Programm [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) aus dem Java SDK erstellt wird. Ein Keystore ist für die Veröffentlichung einer Xamarin.Android-Anwendung unerlässlich, da Android keine Anwendungen ausführt, die nicht digital signiert wurden.

Während der Entwicklung verwendet Xamarin.Android einen Debugkeystore zum Signieren der Anwendung. Dadurch kann die Anwendung direkt im Emulator oder auf den Geräten bereitgestellt werden, die für debugfähige Anwendungen konfiguriert wurden.
Dieser Keystore kann allerdings nicht zur Verteilung von Anwendungen verwendet werden.

Aus diesem Grund muss ein privater Keystore erstellt und zum Signieren von Anwendungen verwendet werden. Dieser Schritt sollte nur einmal ausgeführt werden, da derselbe Schlüssel zum Veröffentlichen von Updates verwendet wird und anschließend zum Signieren anderer Anwendungen verwendet werden kann.

Der Schutz dieses Keystores ist wesentlich. Wenn er verloren geht, ist es nicht mehr möglich, Updates der Anwendung mit Google Play zu veröffentlichen.
Wenn Sie einen Keystore verloren haben, besteht die einzige Möglichkeit darin, einen neuen Keystore zu erstellen, das APK mit dem neuen Schlüssel erneut zu signieren und anschließend eine neue Anwendung einzureichen. Dann muss die alte Anwendung aus Google Play entfernt werden. Wenn der neue Keystore kompromittiert oder öffentlich verteilt wird, ist es ebenfalls möglich, dass inoffizielle Versionen einer Anwendung oder Versionen mit Schadcode verteilt werden.



### <a name="create-a-new-keystore"></a>Erstellen eines neuen Keystores

Zum Erstellen eines neuen Keystores benötigen Sie das Befehlszeilentool [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) aus dem Java SDK. Der folgende Ausschnitt ist ein Beispiel für den Gebrauch von **keytool** (ersetzen Sie `<my-filename>` durch den Dateinamen des Keystores und `<key-name>` durch den Namen des Schlüssels innerhalb des Keystores):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

**keytool** fragt zunächst nach dem Kennwort für den Keystore. Anschließend fragt es nach Informationen zur Erstellung des Schlüssels. Der folgende Ausschnitt ist ein Beispiel für das Erstellen eines neuen Schlüssels mit dem Namen `publishingdoc`, der in der Datei `xample.keystore` gespeichert wird:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

Um die Schlüssel aufzulisten, die in einem Keystore gespeichert sind, verwenden Sie das **Keytool** mit der Option &ndash; `list`:

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>Ausrichten des APK

Vor dem Signieren des APK mit **apksigner** ist es wichtig, zunächst die Datei mit dem Tool **Zipalign** aus dem Android SDK zu optimieren. **Zipalign** strukturiert die Ressourcen in ein APK mit 4-Byte-Blöcken. Diese Anordnung ermöglicht es Android, Ressourcen schnell aus dem APK zu laden, die Leistung der Anwendung zu steigern und möglicherweise die Arbeitsspeichernutzung zu verringern. Xamarin.Android stellt mit einer Überprüfung zur Laufzeit fest, ob das APK mit Zipalign ausgerichtet wurde. Ist dies nicht der Fall, wird die Anwendung nicht ausgeführt.

Der folgende Befehl verwendet das signierte APK und erzeugt einen signiertes, mit Zipalign ausgerichtetes APK namens **helloworld.apk**, das verteilt werden kann.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>Signieren des APKs

Nach der Ausrichtung des APK mit Zipalign ist es notwendig, es mit einem Keystore zu signieren. Dies erfolgt mit dem Tool **apksigner** aus dem Verzeichnis **build-tools** der Version des SDK-Buildtools.  Wenn beispielsweise die Android SDK-Buildtools v25.0.3 installiert sind, finden Sie **apksigner** im Verzeichnis:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

Im folgenden Codeausschnitt wird vorausgesetzt, dass **apksigner** für die `PATH`-Umgebungsvariable zugänglich ist. Es signiert ein APK mithilfe des Aliasschlüssels `publishingdoc`, der in der Datei **xample.keystore** enthalten ist:

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

Wenn dieser Befehl ausgeführt wird, wird **apksigner** ggf. nach dem Kennwort des Keystores fragen.

Weitere Informationen zur Verwendung von **apksigner** finden Sie in der [Google-Dokumentation](https://developer.android.com/studio/command-line/apksigner.html).

> [!NOTE]
> Laut [Problem 62696222](https://issuetracker.google.com/issues/62696222) im Google Issue Tracker „fehlt“ **apksigner** im Android SDK. Um dieses Problem zu umgehen, müssen Sie die Android SDK-Buildtools v25.0.3 installieren und diese Version von **apksigner** verwenden.  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>Signieren des APK mit jarsigner

> [!WARNING]
> Dieser Abschnitt gilt nur, wenn das APK mit dem Dienstprogramm **jarsigner** signiert werden muss. Entwicklern wird empfohlen, **apksigner** zum Signieren des APK zu verwenden.

Dabei wird die APK-Datei mithilfe des Befehls **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** aus dem Java SDK signiert.  Das **jarsigner**-Tool wird vom Java SDK bereitgestellt. 

Der folgende Codeausschnit zeigt, wie ein APK mit **jarsigner** signiert wird, und den Schlüssel `publishingdoc`, der in einer Keystore-Datei namens **xample.keystore** enthalten ist:

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> Bei der Verwendung von **jarsigner** ist es wichtig, das APK _zuerst_ zu signieren und anschließend **Zipalign** zu verwenden.  



## <a name="related-links"></a>Verwandte Links

- [Anwendungssignierung](https://source.android.com/security/apksigning/)
- [Signieren von Java-JAR](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Build Tools 26.0.0 - where did apksigner go? (Buildtools 26.0.0 – Wo ist apksigner?)](https://issuetracker.google.com/issues/62696222)
