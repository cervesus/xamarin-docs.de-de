---
title: Multi-Core-Geräte und Xamarin.Android
description: Android kann in mehreren verschiedenen Computerarchitekturen ausgeführt werden. Dieses Dokument beschreibt die verschiedenen CPU-Architekturen, die für eine Xamarin.Android-Anwendung verwendet werden können. In diesem Dokument wird auch erläutert, wie Android-Anwendungen verpackt werden, um verschiedene CPU-Architekturen zu unterstützen. Die Application Binary Interface (ABI) wird vorgestellt, und es wird eine Anleitung bereitgestellt, welche ABIs in einer Xamarin.Android-Anwendung verwendet werden.
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2019
ms.openlocfilehash: f24fdb768cc0c4e12fdc58f6e5386edd0db98527
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753944"
---
# <a name="multi-core-devices--xamarinandroid"></a>Multi-Core-Geräte und Xamarin.Android

_Android kann in mehreren verschiedenen Computerarchitekturen ausgeführt werden. Dieses Dokument beschreibt die verschiedenen CPU-Architekturen, die für eine Xamarin.Android-Anwendung verwendet werden können. In diesem Dokument wird auch erläutert, wie Android-Anwendungen verpackt werden, um verschiedene CPU-Architekturen zu unterstützen. Die Application Binary Interface (ABI) wird vorgestellt, und es wird eine Anleitung bereitgestellt, welche ABIs in einer Xamarin.Android-Anwendung verwendet werden._

## <a name="overview"></a>Übersicht

Android ermöglicht die Erstellung von „Fat Binaries“, einer einzelnen `.apk`-Datei, die Computercode enthält, der mehrere, unterschiedliche CPU-Architekturen unterstützt. Dies wird erreicht, indem jedem Stück Computercode eine *Application Binary Interface* (ABI) zugeordnet wird. Die ABI wird verwendet, um zu steuern, welcher Computercode auf einem bestimmten Hardwaregerät ausgeführt wird. Damit eine Android-Anwendung beispielsweise auf einem x86-Gerät ausgeführt werden kann, ist es erforderlich, dass beim Kompilieren der Anwendung die x86-ABI-Unterstützung mit einbezogen wird.

Insbesondere wird jede Android-Anwendung mindestens eine *Embedded-Application Binary Interface* (EABI) unterstützen. EABIs sind Konventionen, die speziell für eingebettete Softwareprogramme gelten. Eine typische EABI beschreibt z.B. die folgenden Dinge:

- Den CPU-Befehlssatz.

- Die Bytereihenfolge der Speicher- und Ladevorgänge des Arbeitsspeichers zur Laufzeit.

- Das Binärformat von Objektdateien und Programmbibliotheken sowie die Art des Inhalts, der in diesen Dateien und Bibliotheken zulässig ist oder unterstützt wird.

- Die verschiedenen Konventionen, mit denen Daten zwischen Anwendungscode und dem System übergeben werden (z.B. wie Register und/oder der Stapel beim Aufruf von Funktionen verwendet werden, Ausrichtungbeschränkungen usw.).

- Ausrichtungs- und Größenbeschränkungen für Enumerationstypen, Strukturen, Felder und Arrays.

- Die Liste der Funktionssymbole, die Ihrem Computercode zur Laufzeit zur Verfügung stehen, in der Regel aus einem ganz bestimmten, ausgewählten Satz von Bibliotheken.

### <a name="armeabi-and-thread-safety"></a>armeabi und Threadsicherheit

Die Application Binary Interface wird weiter unten im Detail besprochen, aber es ist wichtig, sich daran zu erinnern, dass die von Xamarin.Android verwendete `armeabi`-Runtime *nicht threadsicher* ist. Wenn eine Anwendung mit `armeabi`-Unterstützung auf einem `armeabi-v7a`-Gerät bereitgestellt wird, treten zahlreiche seltsame und unerklärliche Ausnahmen auf.

Aufgrund eines Fehlers in Android 4.0.0.0, 4.0.1, 4.0.2, 4.0.2 und 4.0.3 werden die nativen Bibliotheken aus dem Verzeichnis `armeabi` übernommen, obwohl ein Verzeichnis `armeabi-v7a` vorhanden ist und das Gerät ein `armeabi-v7a`-Gerät ist.

> [!NOTE]
> Xamarin.Android stellt sicher, dass `.so`-Dateien dem APK in der richtigen Reihenfolge hinzugefügt werden. Dieser Fehler sollte für Benutzer von Xamarin.Android kein Problem darstellen.

### <a name="abi-descriptions"></a>ABI-Beschreibungen

Jede von Android unterstützte ABI wird durch einen eindeutigen Namen identifiziert.

#### <a name="armeabi"></a>armeabi

Dies ist der Name einer EABI für ARM-basierte CPUs, die mindestens den ARMv5TE-Befehlssatz unterstützen. Android folgt der Little-Endian-ARM-GNU-/Linux-ABI. Diese ABI unterstützt keine hardwareunterstützten Gleitkommaberechnungen. Alle FP-Vorgänge werden von Softwarehilfsfunktionen ausgeführt, die aus der statischen `libgcc.a`-Bibliothek des Compilers stammen. SMP-Geräte werden von `armeabi` nicht unterstützt.

> [!IMPORTANT]
> Der `armeabi`-Code von Xamarin.Android ist nicht threadsicher und sollte nicht auf `armeabi-v7a`-Geräten mit mehreren CPUs verwendet werden (siehe Beschreibung weiter unten). Das Verwenden von `armeabi`-Code auf einem `armeabi-v7a`-Einzelkerngerät ist sicher.

#### <a name="armeabi-v7a"></a>armeabi-v7a

Dies ist ein weiterer ARM-basierter CPU-Befehlssatz, der die oben beschriebene `armeabi`-EABI erweitert. Die `armeabi-v7a`-EABI unterstützt Hardware-Gleitkommavorgänge und mehrere CPU-Geräte (SMP). Eine Anwendung, die die `armeabi-v7a`-EABI verwendet, kann erhebliche Leistungsverbesserungen gegenüber einer Anwendung erwarten, die `armeabi` verwendet.

> [!NOTE]
> `armeabi-v7a`-Computercode kann auf ARMv5 Geräten nicht ausgeführt werden.

#### <a name="arm64-v8a"></a>arm64 v8a

Dies ist ein 64-Bit-Befehlssatz, der auf der ARMv8-CPU-Architektur basiert. Diese Architektur wird im *Nexus 9* verwendet.
In Xamarin.Android 5.1 wurde Unterstützung für diese Architektur eingeführt (weitere Informationen finden Sie unter [Unterstützung für die 64-Bit-Runtime](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)).

#### <a name="x86"></a>x86

Dies ist der Name einer ABI für CPUs, die den Befehlssatz unterstützen, der üblicherweise *x86* oder *IA-32* genannt wird. Diese ABI entspricht den Anweisungen für den Pentium Pro-Befehlssatz, einschließlich der MMX-, SSE-, SSE-, SSE2- und SSE3-Befehlssätze. Sie enthält keine weiteren optionalen IA-32-Befehlssatzerweiterungen wie:

- Die MOVBE-Anweisung.
- Zusätzliche SSE3-Erweiterung (SSSE3).
- Eine Variante von SSE4.

> [!NOTE]
> Google TV wird vom Android NDK nicht unterstützt, obwohl die Anwendung unter x86 ausgeführt wird.

#### <a name="x86_64"></a>x86_64

Dies ist der Name einer ABI für CPUs, die den 64-Bit-x86-Befehlssatz unterstützen (auch als *x64* oder *AMD64* bezeichnet). In Xamarin.Android 5.1 wurde Unterstützung für diese Architektur eingeführt (weitere Informationen finden Sie unter [Unterstützung für die 64-Bit-Runtime](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)).

#### <a name="apk-file-format"></a>APK-Dateiformat

Das APK-Format (Android Application Package) ist das Dateiformat, das den gesamten Code, die Objekte, Ressourcen und Zertifikate enthält, die für eine Android-Anwendung erforderlich sind. Es handelt sich um eine `.zip`-Datei, die jedoch die Dateinamenerweiterung `.apk` verwendet. Der Inhalt einer `.apk`-Datei (nach der Erweiterung), die von Xamarin.Android erstellt wurde, wird im Screenshot unten gezeigt:

[![Inhalt der APK-Datei](multicore-devices-images/00.png)](multicore-devices-images/00.png#lightbox)

Kurze Beschreibung des Inhalts der `.apk`-Datei:

- **AndroidManifest.xml** &ndash; Dies ist die `AndroidManifest.xml`-Datei im XML-Binärformat.

- **classes.dex** &ndash; Enthält den Anwendungscode, der in das `dex`-Dateiformat kompiliert wurde, das von der Android-Runtime-VM verwendet wird.

- **resources.arsc** &ndash; Diese Datei enthält alle vorkompilierten Ressourcen für die Anwendung.

- **lib** &ndash; Dieses Verzeichnis enthält den kompilierten Code für jede ABI. Es enthält einen Unterordner für jede ABI, die im vorherigen Abschnitt beschrieben wurde. Im obigen Screenshot besitzt die fragliche `.apk`-Datei native Bibliotheken für `armeabi-v7a` und für `x86`.

- **META-INF** &ndash; Dieses Verzeichnis (wenn vorhanden) wird zum Speichern von Signaturinformationen, Paket- und Erweiterungskonfigurationsdaten verwendet.

- **res** &ndash; Dieses Verzeichnis enthält die Ressourcen, die nicht in `resources.arsc` kompiliert wurden.

> [!NOTE]
> Die Datei `libmonodroid.so` ist die native Bibliothek, die von allen Xamarin.Android-Anwendungen verlangt wird.

#### <a name="android-device-abi-support"></a>ABI-Unterstützung für Android-Geräte

Jedes Android-Gerät unterstützt die Ausführung von nativem Code in bis zu zwei ABIs:

- **In der „primären“ ABI** &ndash; Dies entspricht dem Computercode, der im Systemimage verwendet wird.

- **In einer „sekundären“ ABI** &ndash; Dies ist eine optionale ABI, die auch vom Systemimage unterstützt wird.

Beispielsweise verfügt ein typisches ARMv5TE-Gerät nur über eine primäre ABI von `armeabi`, während ein ARMv7-Gerät eine primäre ABI von `armeabi-v7a` und eine sekundäre ABI von `armeabi` angeben würde. Ein typisches x86-Gerät würde nur eine primäre ABI von `x86` angeben.

### <a name="android-native-library-installation"></a>Installation der nativen Android-Bibliothek

Bei der Paketinstallation werden native Bibliotheken aus der `.apk`-Datei in das native Bibliotheksverzeichnis der App extrahiert (in der Regel `/data/data/<package-name>/lib`) und anschließend als `$APP/lib` bezeichnet.

Das Installationsverhalten der nativen Bibliothek von Android unterscheidet sich erheblich zwischen den verschiedenen Android-Versionen.

#### <a name="installing-native-libraries-pre-android-40"></a>Installieren nativer Bibliotheken: Vor Android 4.0

Android vor 4.0 Ice Cream Sandwich extrahiert nur native Bibliotheken aus einer *einzigen ABI* in der `.apk`-Datei. Diese alten Android-Anwendungen werden zunächst versuchen, alle nativen Bibliotheken für die primäre ABI zu extrahieren. Wenn keine solchen Bibliotheken vorhanden sind, extrahiert Android dann alle nativen Bibliotheken für die sekundäre ABI. Es erfolgt kein „Mergevorgang“.

Betrachten Sie zum Beispiel eine Situation, in der eine Anwendung auf einem `armeabi-v7a`-Gerät installiert ist. Die `.apk,`-Datei, die sowohl `armeabi` als auch `armeabi-v7a` unterstützt, verfügt über die folgenden ABI `lib`-Verzeichnisse und darin enthaltenen Dateien:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Nach der Installation enthält das Verzeichnis für die native Bibliothek Folgendes:

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

Das heißt, es wird keine Datei `libone.so` installiert. Dies führt zu Problemen, da `libone.so` nicht vorhanden ist, um von der Anwendung zur Laufzeit geladen zu werden. Dieses Verhalten (obwohl unerwartet) wurde als Fehler protokolliert und als „[bestimmungsgemäße Funktion](http://code.google.com/p/android/issues/detail?id=9089)“ neu klassifiziert.

Wenn das Ziel Android-Versionen vor 4.0 sind, ist es daher erforderlich, *alle* nativen Bibliotheken für *jede* ABI bereitzustellen, die von der Anwendung unterstützt werden, d.h. die `.apk`-Datei sollte Folgendes enthalten:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```

#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>Installieren nativer Bibliotheken: Android 4.0 &ndash; Android 4.0.3

Android 4.0 Ice Cream Sandwich ändert die Extraktionslogik. Es werden alle nativen Bibliotheken aufgezählt, es wird geprüft, ob der Basisname der Datei bereits extrahiert wurde, und wenn beide der folgenden Bedingungen erfüllt sind, wird die Bibliothek extrahiert:

- Sie wurde nicht bereits extrahiert.

- Die ABI der nativen Bibliothek stimmt mit der primären oder sekundären ABI des Ziels überein.

Die Erfüllung dieser Bedingungen erlaubt ein „Mergeverhalten“, d.h. wenn eine `.apk`-Datei mit dem folgenden Inhalt vorhanden ist:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Enthält das Verzeichnis der nativen Bibliothek nach der Installation Folgendes:

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

Leider hängt dieses Verhalten wie im folgenden Dokument beschrieben von der Reihenfolge ab: [Issue 24321: Galaxy Nexus 4.0.2 uses armeabi native code when both armeabi and armeabi-v7a is included in apk (Problem 24321: Galaxy Nexus 4.0.2 verwendet nativen armeabi-Code, wenn armeabi und armeabi-v7a in der APK-Datei enthalten sind)](http://code.google.com/p/android/issues/detail?id=25321).

Die nativen Bibliotheken werden „in der jeweiligen Reihenfolge“ verarbeitet (z.B. wie durch unzip aufgelistet), und die *erste Übereinstimmung* wird extrahiert. Da die `.apk`-Datei die Versionen `armeabi` und `armeabi-v7a` von `libtwo.so` enthält und `armeabi` zuerst aufgelistet wird, wird die `armeabi`-Version extrahiert, *nicht* die `armeabi-v7a`-Version:

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

Auch wenn die `armeabi`- und `armeabi-v7a`-ABIs angegeben werden (wie unten im Abschnitt *Deklarieren unterstützter ABIs* beschrieben), wird Xamarin.Android das folgende Element in der .
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

Folglich wird die Datei `armeabi` `libmonodroid.so` zuerst in der `.apk`-Datei gefunden, und `armeabi` `libmonodroid.so` wird die Datei sein, die extrahiert wird, obwohl `armeabi-v7a` `libmonodroid.so` vorhanden und für das Ziel optimiert ist. Dies kann ebenfalls zu unverständlichen Laufzeitfehlern führen, da `armeabi` nicht SMP-sicher ist.

##### <a name="installing-native-libraries-android-404-and-later"></a>Installieren nativer Bibliotheken: Android 4.0.4 oder höher

Android 4.0.4 ändert die Extraktionslogik: Es werden alle nativen Bibliotheken aufgezählt, der Basisname der Datei wird gelesen, dann wird die primäre ABI-Version (wenn vorhanden) oder die sekundäre ABI (wenn vorhanden) extrahiert. Dies erlaubt ein „Mergeverhalten“, d.h. wenn eine `.apk`-Datei mit dem folgenden Inhalt vorhanden ist:

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

Enthält das Verzeichnis der nativen Bibliothek nach der Installation Folgendes:

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```

### <a name="xamarinandroid-and-abis"></a>Xamarin.Android und ABIs

Xamarin.Android unterstützt die folgenden _64-Bit_-Architekturen:

- `arm64-v8a`
- `x86_64`

> [!NOTE]
> Ab August 2018 sind neue Apps erforderlich, um auf API-Ebene 26 abzielen, und ab August 2019 sind Apps [zum Bereitstellen von 64-Bit-Versionen erforderlich](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html), zusätzlich zu der 32-Bit-Version.

Xamarin.Android unterstützt die folgenden 32-Bit-Architekturen:

- `armeabi` ^
- `armeabi-v7a`
- `x86`

> [!NOTE]
> **^** Ab [Xamarin.Android 9.2](https://docs.microsoft.com/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture) wird `armeabi` nicht mehr unterstützt.

Xamarin.Android bietet zurzeit keine Unterstützung für `mips`.

### <a name="declaring-supported-abis"></a>Deklarieren unterstützter ABIs

Standardmäßig verwendet Xamarin.Android `armeabi-v7a` für **Releasebuilds** und `armeabi-v7a` und `x86` für **Debugbuilds**. Unterstützung für verschiedene ABIs kann über die Projektoptionen für ein Xamarin.Android-Projekt festgelegt werden. In Visual Studio kann dies auf der Seite **Android-Optionen** der **Eigenschaften** des Projekts auf der Registerkarte **Erweitert** festgelegt werden, wie im folgenden Screenshot gezeigt:

![Android-Optionen: Erweiterte Eigenschaften](multicore-devices-images/vs-abi-selections.png)

In Visual Studio für Mac können die unterstützten Architekturen auf der Seite **Android-Build** der **Projektoptionen** auf der Registerkarte **Erweitert** ausgewählt werden, wie im folgenden Screenshot gezeigt:

[![Android-Build: Unterstützte ABIs](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png#lightbox)

Es gibt einige Situationen, in denen es erforderlich sein kann, zusätzliche ABI-Unterstützung zu deklarieren, z.B. unter den folgenden Umständen:

- Bereitstellen der Anwendung auf einem `x86`-Gerät.

- Bereitstellen der Anwendung auf einem `armeabi-v7a`-Gerät, um Threadsicherheit zu gewährleisten.

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wurden die verschiedenen CPU-Architekturen beschrieben, in denen eine Android-Anwendung ausgeführt werden kann. Dabei wurde die Application Binary Interface vorgestellt und gezeigt, wie sie von Android verwendet wird, um unterschiedliche CPU-Architekturen zu unterstützen.
Anschließend wurde erläutert, wie die ABI-Unterstützung in einer Xamarin.Android-Anwendung angegeben werden kann und welche Probleme bei der Verwendung von Xamarin.Android-Anwendungen auf einem `armeabi-v7a`-Gerät auftreten, die nur für `armeabi` bestimmt sind.

## <a name="related-links"></a>Verwandte Links

- [ABI für die ARM-Architektur (PDF-Datei)](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html)
- [Problem 9089: Nexus One: Es werden KEINE nativen Bibliotheken aus armeabi geladen, wenn mindestens eine Bibliothek unter armeabi-v7a vorhanden ist.](http://code.google.com/p/android/issues/detail?id=9089)
- [Issue 24321: Galaxy Nexus 4.0.2 uses armeabi native code when both armeabi and armeabi-v7a is included in apk (Problem 24321: Galaxy Nexus 4.0.2 verwendet nativen armeabi-Code, wenn armeabi und armeabi-v7a in der APK-Datei enthalten sind)](http://code.google.com/p/android/issues/detail?id=25321).
