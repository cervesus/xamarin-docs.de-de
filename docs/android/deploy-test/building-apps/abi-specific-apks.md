---
title: Erstellen ABI-spezifischer Android-Anwendungspakete (APKs)
description: In diesem Dokument wird erörtert, wie ein APK erstellt wird, das mithilfe von Xamarin.Android auf eine einzelne ABI abzielt.
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 0520439b89458b7f73a025cd8d6b2cf8fc41dac0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940635"
---
# <a name="building-abi-specific-apks"></a>Erstellen ABI-spezifischer Android-Anwendungspakete (APKs)

_In diesem Dokument wird erörtert, wie ein APK erstellt wird, das mithilfe von Xamarin.Android auf eine einzelne ABI abzielt._

## <a name="overview"></a>Übersicht

In bestimmten Situationen kann es für eine Anwendung vorteilhaft sein, mehrere APKs zu haben. Jedes APK ist mit demselben Keystore signiert und hat denselben Paketnamen, wird aber für ein bestimmtes Gerät oder eine bestimmte Android-Konfiguration kompiliert. Dieser Ansatz wird nicht empfohlen, denn es ist viel einfacher, über ein APK zu verfügen, das mehrere Geräte und Konfigurationen unterstützt. Es gibt allerdings Situationen, in denen das Erstellen mehrerer APKs nützlich sein kann, wie z. B.:

- **Reduzieren der Größe des APK**: Google Play erzwingt für APK-Dateien eine Größenbeschränkung von 100 MB. Das Erstellen gerätespezifischer APKs kann die Größe des APK reduzieren, da Sie nur eine Teilmenge von Objekten und Ressourcen für die Anwendung bereitstellen müssen.

- **Unterstützung verschiedener CPU-Architekturen**: Wenn Ihre Anwendung über gemeinsam genutzte Bibliotheken für bestimmte CPUs verfügt, können Sie nur die gemeinsam genutzten Bibliotheken für die jeweilige CPU verteilen.

Mehrere APKs können die Verteilung erschweren – ein Problem, das von Google Play angegangen wird. Google Play stellt sicher, dass das richtige APK an ein Gerät übermittelt wird, und zwar basierend auf dem Versionscode der Anwendung und anderen Metadaten, die in der Datei **AndroidManifest.XML** enthalten sind. Spezifische Details und Einschränkungen, wie Google Play mehrere APKs für eine Anwendung unterstützt, finden Sie in der [Google- Dokumentation zur Unterstützung mehrerer APKs](https://developer.android.com/google/play/publishing/multiple-apks.html).

In dieser Anleitung wird beschrieben, wie für das Erstellen mehrerer APKs für eine Xamarin.Android-Anwendung ein Skript erstellt wird, wobei jedes APK auf eine bestimmte ABI zugeschnitten ist. Die folgenden Themen werden behandelt:

1. Erstellen eines eindeutigen *Versionscodes* für das APK
1. Erstellen einer temporären Version der Datei **AndroidManifest.XML**, die für dieses APK verwendet wird
1. Erstellen der Anwendung mithilfe der Datei **AndroidManifest.XML** aus dem vorherigen Schritt
1. Vorbereiten des APK auf die Freigabe, indem es signiert und Zipalign darauf angewendet wird

Am Ende dieser Anleitung finden Sie eine exemplarische Vorgehensweise, die demonstriert, wie Sie für diese Schritte mit [Rake](https://martinfowler.com/articles/rake.html) ein Skript erstellen.

### <a name="creating-the-version-code-for-the-apk"></a>Erstellen des Versionscodes für das APK

Google empfiehlt einen bestimmten Algorithmus für den siebenstelligen Versionscode (siehe Abschnitt zum *Verwenden eines Versionscodeschemas* im Dokument zur [Unterstützung mehrerer APKs](https://developer.android.com/google/play/publishing/multiple-apks.html)).
Durch die Erweiterung dieses Versionscodeschemas auf acht Ziffern ist es möglich, einige ABI-Informationen in den Versionscode aufzunehmen, die sicherstellen, dass Google Play das richtige APK an ein Gerät verteilt. Die folgende Liste erläutert dieses Versionscodeformat mit acht Ziffern (Indizierung von links nach rechts):

- **Index 0** (im folgenden Diagramm rot) &ndash; Eine ganze Zahl für die ABI:
  - 1 &ndash; `armeabi`
  - 2 &ndash; `armeabi-v7a`
  - 6 &ndash; `x86`

- **Index 1-2** (im folgenden Diagramm orange) &ndash; Die API-Mindestebene, die von der Anwendung unterstützt wird.

- **Index 3-4** (im folgenden Diagramm blau) &ndash; Die unterstützten Bildschirmgrößen:
  - 1 &ndash; klein
  - 2 &ndash; normal
  - 3 &ndash; groß
  - 4 &ndash; sehr groß

- **Index 5-7** (im folgenden Diagramm grün) &ndash; Eine eindeutige Zahl für den Versionscode. 
    Diese wird vom Entwickler festgelegt. Sie muss für jede öffentliche Freigabe der Anwendung erhöht werden.

Das folgende Diagramm veranschaulicht die Position jedes Code, der in der obigen Liste beschrieben wird:

[![Diagramm des achtstelligen Versionscodeformats mit farblicher Kennzeichnung](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)

Google Play stellt sicher, dass auf Grundlage der `versionCode`- und APK-Konfiguration das richtige APK an das Gerät übermittelt wird. Das APK mit dem höchsten Versionscode wird an das Gerät übermittelt. Beispielsweise kann eine Anwendung drei APKs mit den folgenden Versionscodes aufweisen:

- 11413456: Die ABI ist `armeabi`; für die API-Ebene 14; kleine bis große Bildschirme; mit der Versionsnummer 456.
- 21423456: Die ABI ist `armeabi-v7a`; für die API-Ebene 14; normale &amp; große Bildschirme; mit der Versionsnummer 456.
- 61423456: Die ABI ist `x86`; für die API-Ebene 14; normale &amp; große Bildschirme; mit der Versionsnummer 456.

Um mit diesem Beispiel fortzufahren, stellen Sie sich vor, dass ein Fehler behoben wurde, der spezifisch für `armeabi-v7a` war. Die Anwendungsversion erhöht sich auf 457, und ein neues APK wird erstellt, wobei die `android:versionCode` auf 21423457 festgelegt wird. Die Versionscodes für die Versionen `armeabi` und `x86` bleiben hingegen unverändert.

Stellen Sie sich nun vor, dass die x86-Version einige Updates oder Fehlerkorrekturen erhält, die auf eine neuere API (API-Ebene 19) abzielen, sodass dies die Version 500 der Anwendung darstellt. Der neue `versionCode` ändert sich in 61923500, während „armeabi/armeabi-v7a“ unverändert bleiben. Zu diesem Zeitpunkt lauten die Versionscodes wie folgt:

- 11413456: Die ABI ist `armeabi`; für die API-Ebene 14; kleine bis große Bildschirme; mit der Versionsbezeichnung 456.
- 21423457: Die ABI ist `armeabi-v7a`; für die API-Ebene 14; normale &amp; große Bildschirme; mit der Versionsbezeichnung 457.
- 61923500: Die ABI ist `x86`; für die API-Ebene 19; normale &amp; große Bildschirme; mit der Versionsbezeichnung 500.

Die manuelle Pflege dieser Versionscodes kann einen erheblichen Aufwand für den Entwickler bedeuten. Der Prozess der Berechnung des ordnungsgemäßen `android:versionCode` und der anschließenden Erstellung der APKs sollte automatisiert werden.
Ein Beispiel dafür wird in der exemplarischen Vorgehensweise am Ende dieses Dokuments erläutert.

### <a name="create-a-temporary-androidmanifestxml"></a>Erstellen der temporären Datei „AndroidManifest.XML“

Obwohl nicht unbedingt notwendig, kann die Erstellung der temporären Datei **AndroidManifest.XML** für jede ABI dazu beitragen, Probleme zu vermeiden, die durch das Entweichen von Informationen aus einem APK in ein anderes entstehen können. Beispielsweise ist es entscheidend, die das Attribut `android:versionCode` für jedes APK eindeutig ist.

Wie dies geschieht, hängt vom jeweiligen Skripterstellungssystem ab. Aber es umfasst üblicherweise das Erstellen einer Kopie des Android-Manifests, das während der Entwicklung verwendet wird, dessen Modifizierung und die anschließende Verwendung des modifizierten Manifests während des Buildprozesses.

### <a name="compiling-the-apk"></a>Kompilieren des APK

Die Erstellung des APK pro ABI erfolgt am besten mit `xbuild` oder `msbuild`, wie in der folgenden Beispielbefehlszeile gezeigt:

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

In der folgenden Liste werden die einzelnen Befehlszeilenparameter erläutert:

- `/t:Package` &ndash; Erstellt ein Android APK, das mithilfe des Debug-Keystores signiert ist.

- `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; Diese ist die ABI, auf die abgezielt wird. Entweder `armeabi`, `armeabi-v7a` oder `x86`.

- `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; Dies ist das Verzeichnis, in dem die Zwischendateien gespeichert werden, die als Teil des Builds erstellt werden. Bei Bedarf erstellt Xamarin.Android ein Verzeichnis, das nach der ABI benannt ist, wie z. B. `obj.armeabi-v7a`. Es wird empfohlen, für jede ABI einen Ordner zu verwenden, um Probleme zu vermeiden, die dazu führen, dass Dateien von einem Build zum anderen „entweichen“. Beachten Sie, dass dieser Wert mit einem Verzeichnistrennzeichen (bei OS X `/`) abgeschlossen wird.

- `/p:AndroidManifest` &ndash; Diese Eigenschaft gibt den Pfad zur Datei **AndroidManifest.XML** an, die während des Builds verwendet wird.

- `/p:OutputPath=bin.<TARGET_ABI>` &ndash; Dies ist das Verzeichnis, in dem das endgültige APK gespeichert wird. Xamarin.Android erstellt ein Verzeichnis, das nach der ABI benannt ist, wie z. B. `bin.armeabi-v7a`.

- `/p:Configuration=Release` &ndash; Erstellen Sie einen Releasebuild des APK. Debugbuilds werden möglicherweise nicht auf Google Play hochgeladen.

- `<CS_PROJ FILE>` &ndash; Dies ist der Pfad zur Datei `.csproj` für das Xamarin.Android-Projekt.

### <a name="sign-and-zipalign-the-apk"></a>Signieren des und Anwenden von Zipalign auf das APK

Das APK muss signiert werden, bevor es über Google Play verbreitet werden kann. Dies kann mit der Anwendung `jarsigner` erfolgen, die Bestandteil des Java Developer's Kit ist. Der folgende Befehlszeile demonstriert, wie `jarsigner` verwendet wird:

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

Auf alle Xamarin.Android-Anwendungen muss Zipalign angewendet werden, bevor sie auf einem Gerät ausgeführt werden können. Dies ist das Format der zu verwendenden Befehlszeile:

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```

## <a name="automating-apk-creation-with-rake"></a>Automatisieren der APK-Erstellung mit Rake

Das Beispielprojekt [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) ist ein einfaches Android-Projekt, das veranschaulicht, wie eine ABI-spezifische Versionsnummer berechnet und drei separate APKs für jede der folgenden ABIs erstellt werden können:

- armeabi
- armeabi-v7a
- x86

[rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) im Beispielprojekt führt jeden der Schritte aus, die in den vorhergehenden Abschnitten beschrieben wurden:

1. [Erstellen eines Android-Versionscodes](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30) für das APK

1. [Schreiben des Android-Versionscodes](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55) in eine benutzerdefinierte **AndroidManifest.XML**-Datei für dieses APK

1. [Kompilieren eines Releasebuilds](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63) des Xamarin.Android-Projekts, das ausschließlich auf die ABI abzielt, und Verwenden der Datei **AndroidManifest.XML**, die im vorherigen Schritt erstellt wurde.

1. [Signieren des APK ](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66) mit einem Produktions-Keystore.

1. Anwenden von [Zipalign](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67) auf das APK

Um alle APKs für die Anwendung zu erstellen, führen Sie den Rake-Task `build` an der Befehlszeile aus:

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Sobald der Rake-Task abgeschlossen ist, gibt es drei `bin`-Ordner mit der Datei `xamarin.helloworld.apk`. Der nächste Screenshot zeigt alle diese Ordner samt Inhalt:

[![Speicherorte plattformspezifischer Ordner, die „xamarin.helloworld.apk“ enthalten](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)

> [!NOTE]
> Der in dieser Anleitung beschriebene Buildprozess kann in eins der vielen verschiedenen Buildsysteme implementiert werden. Obwohl wir kein vorgefertigtes Beispiel haben, sollte es auch mit [PowerShell](https://technet.microsoft.com/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) oder [Fake](https://fsharp.github.io/FAKE/) möglich sein.

## <a name="summary"></a>Zusammenfassung

Diese Anleitung enthält Vorschläge zur Erstellung von Android APKs, die auf eine bestimmte ABI abzielen. Außerdem wurde ein mögliches Schema für die Erstellung von `android:versionCodes` erörtert, das die CPU-Architektur ermittelt, für die das APK vorgesehen ist. Die exemplarische Vorgehensweise enthielt ein Beispielprojekt, dessen Builderstellung per Skript mithilfe von Rake erfolgte.

## <a name="related-links"></a>Verwandte Links

- [OneABIPerAPK (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/oneabiperapk)
- [Veröffentlichen einer Anwendung](~/android/deploy-test/publishing/index.md)
- [Unterstützung mehrerer APKs für Google Play](https://developer.android.com/google/play/publishing/multiple-apks.html)
