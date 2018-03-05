---
title: Problembehandlung
description: "In diesem Artikel deckt bekannten Problemen, die beim Arbeiten mit Unterstützung für tvos. außerdem wurden die Xamarin auftreten können."
ms.topic: article
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 278b9e782073a26dc04bac9418613ea4c09db445
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Problembehandlung

_In diesem Artikel deckt bekannten Problemen, die beim Arbeiten mit Unterstützung für tvos. außerdem wurden die Xamarin auftreten können._

<a name="Known-Issues" />

## <a name="known-issues"></a>Bekannte Probleme

Die aktuelle Version der Xamarin tvos. außerdem wurden Unterstützung hat die folgenden bekannten Probleme:

- **Mono-Framework** – Mono 4.3 Cryptography.ProtectedData zum Entschlüsseln von Daten aus Mono 4.2 fehlschlägt. Deshalb fehl NuGet-Pakete, mit dem Fehler wiederherstellen `Data unprotection failed` Wenn eine geschützte NuGet-Quelle konfiguriert ist.
    - **Problemumgehung** – In Visual Studio für Mac, müssen Sie wieder hinzufügen NuGet-Paket Datenquellen dieses Kennwort-Authentifizierung verwenden, bevor Sie erneut versuchen, stellen Sie die Pakete wieder her.
- **Visual Studio für Mac mit F#-Add-Ins** – Fehler beim Erstellen einer Vorlage für f# Android unter Windows. Dies sollte weiterhin ordnungsgemäß auf einem Mac-Funktion
- **Xamarin.Mac** – Wenn das Zielframework einheitliche Vorlagenprojekts Xamarin.Mac mit, dass festgelegt `Unsupported`, das Popup `Could not connect to the debugger` angezeigt werden kann.
    - **Mögliche Lösung** – ein Downgrade die Mono-Framework-Version in unseren stabil Kanal verfügbar.
- **Xamarin für Visual Studio & Xamarin.iOS** – beim Bereitstellen von WatchKit Anwendungen in Visual Studio, die Fehler `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` angezeigt werden kann.

Alle Fehler Sie Bericht finden Sie auf [Bugzilla](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

## <a name="troubleshooting"></a>Problembehandlung

Den folgenden Abschnitten werden einige bekannte Probleme, die auftreten können, wenn tvos. außerdem wurden 9 mit Xamarin.tvOS und die Lösung dieser Probleme verwenden:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Ungültige ausführbare - die ausführbare Datei Bitcode enthält nicht

Wenn beim Versuch, eine app Xamarin.tvOS auf den Apple TV-App-Store zu übermitteln, erhalten Sie möglicherweise folgende Fehlermeldung in der Form _"Ungültige ausführbare - die ausführbare Datei enthält keinen Bitcode"_.

Um dieses Problem zu beheben, führen Sie folgende Schritte aus:

1. Visual Studio für Mac, mit der Maustaste auf die Projektdatei Xamarin.tvOS in der **Projektmappen-Explorer** , und wählen Sie **Optionen**.
2. Wählen Sie **tvos. außerdem wurden Build** und stellen Sie sicher, dass Sie auf die **Version** Konfiguration: 

    [ ![](troubleshooting-images/ts01.png "Wählen Sie tvos. außerdem wurden Buildoptionen")](troubleshooting-images/ts01.png)
3. Hinzufügen `--bitcode=asmonly` auf die **zusätzliche Mtouch Argumente** Feld, und klicken Sie auf die **OK** Schaltfläche.
4. Neu erstellen Ihrer app in der **Version** Konfiguration.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Überprüfen, ob Ihre tvos. außerdem wurden App enthält Bitcode

Um sicherzustellen, dass Ihre Xamarin.tvOS App-Build Bitcode enthält, öffnen Sie die Terminal-app, und geben Sie Folgendes ein:

```csharp
otool -l /path/to/your/tv.app/tv
```

Suchen Sie in der Ausgabe nach folgendem:

```csharp
Section
  sectname __bundle
   segname __LLVM
      addr 0x0000000100001000
      size 0x000000000000124f
    offset 4096
     align 2^0 (1)
    reloff 0
    nreloc 0
     flags 0x00000000
 reserved1 0
 reserved2 0
```

`addr` und `size` weicht jedoch andere Felder sollten identisch sein.

Sie müssen sicherstellen, dass statische Dritter (`.a`) Bibliotheken, die Sie verwenden, die für tvos. außerdem wurden-Bibliotheken (nicht iOS-Bibliotheken) erstellt wurden, und sie gibt außerdem Aufschluss Bitcode.

Für apps oder Bibliotheken, die gültige Bitcode enthalten die `size` ist größer als 1 sein. Es gibt einige Situationen, in dem eine Bibliothek haben die Markierung Bitcode kann trotzdem enthalten keine gültigen Bitcode. Zum Beispiel:

**Ungültige Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Gültige Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Beachten Sie den Unterschied im `size` zwischen beiden Bibliotheken im aufgeführten Beispiel wird über ausgeführt. Die Bibliothek muss über einen Xcode Archiv Build generiert werden, mit Bitcode aktiviert (Xcode Einstellung `ENABLE_BITCODE`) als Lösung für dieses Problem Größe.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Apps, die nur das Segment arm64 enthalten müssen auch "arm64" in der Liste der UIRequiredDeviceCapabilities in der Datei "Info.plist" haben.

Wenn Sie eine app auf den Apple TV-App Store für Veröffentlichung übermitteln zu können, erhalten Sie möglicherweise einen Fehler in der Form:

_"Apps, die nur das Segment arm64 enthalten auch"arm64"in der Liste der in der Datei" Info.plist "UIRequiredDeviceCapabilities benötigen"_

In diesem Fall Bearbeiten Ihrer `Info.plist` Datei, und stellen Sie sicher, dass sie die folgenden Schlüssel hat:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

Kompilieren Sie Ihre app für Release und iTunes Connect erneut übermitteln.

### <a name="task-mtouch-execution----failed"></a>Task "MTouch" execution -- FAILED

Wenn Sie eine 3rd Party-Bibliothek (z. B. MonoGame) verwenden und die Kompilierung Version bei der eine lange Reihe von Fehlermeldungen Endziffern `Task "MTouch" execution -- FAILED`, versuchen Sie es hinzufügen `-gcc_flags="-framework OpenAL"` auf Ihre **zusätzliche Touch Argumente**:

[ ![](troubleshooting-images/mtouch01.png "Ausführung der Aufgabe MTouch")](troubleshooting-images/mtouch01.png)

Sollten Sie auch einschließen `--bitcode=asmonly` in der **zusätzliche Touch Argumente**, haben die Linkeroptionen so eingestellt **Link alle** , und führen Sie eine saubere Kompilierung.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS 90471-Fehler. Die "große Symbole" fehlt.

Wenn Sie eine Nachricht in der Form "ITMS 90471. Fehlermeldung Die "große Symbole" ist nicht vorhanden"beim Versuch, eine Xamarin.tvOS app auf den Apple TV-App-Store Version senden überprüfen Sie bitte Sie Folgendes:

1. Stellen Sie sicher, dass Sie die "große Symbole" Medienobjekte in aufgenommen haben Ihre `Assets.car` -Datei, die Sie erstellt haben, mit der [App-Symbole](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) Dokumentation.
2. Stellen Sie sicher, dass Sie enthalten die `Assets.car` -Datei von der [arbeiten mit Symbolen und Bilder](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation in Ihrem endgültigen Anwendungspaket.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Ungültiges Bündel – muss eine app, die Gamecontroller unterstützt auch die Apple TV remote unterstützen

oder 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Ungültiges Bündel – Apple TV-apps mit GameController Framework muss der GCSupportedGameControllers-Schlüssel in der app-Datei "Info.plist" enthalten.

Gamecontroller können verwendet werden, um Spielverlauf verbessern, und geben Sie einen Eindruck von stärker in ein Spiel. Sie können auch verwendet werden, um Apple TV-Standardschnittstelle zu steuern, sodass der Benutzer zum Wechseln zwischen dem Remotecomputer und der Controller enthält.

Wenn Sie eine app Xamarin.tvOS Gamecontroller-Support, wenn die Apple TV-App-Store übermittelt, und Sie eine Fehlermeldung in Form von erhalten:


_Wir haben eine oder mehrere Probleme mit der die letzte Übermittlung für "Anwendungsname" ermittelt. Die Bereitstellung war erfolgreich, aber vielleicht möchten Sie die folgenden Probleme in Ihrer Übermittlung weiter zu beheben:_

_Ungültiges Bündel – muss für eine app, die Gamecontroller unterstützt auch die Apple TV remote unterstützen._

oder 

_Ungültiges Bündel – Apple TV-apps mit GameController Framework muss der GCSupportedGameControllers-Schlüssel in der app-Datei "Info.plist" enthalten._

Die Lösung besteht darin, Hinzufügen der Unterstützung für den Remoteserver Siri (`GCMicroGamepad`) auf Ihre app `Info.plist` Datei. Das Profil Micro Gamecontroller wurde von Apple Siri Remote Ziel hinzugefügt. Beispielsweise enthalten Sie die folgenden Schlüssel:

```xml
<key>GCSupportedGameControllers</key>  
  <array>  
    <dict>  
      <key>ProfileName</key>  
      <string>ExtendedGamepad</string>  
    </dict>  
    <dict>  
      <key>ProfileName</key>  
      <string>MicroGamepad</string>  
    </dict>  
  </array>  
<key>GCSupportsControllerUserInteraction</key>  
<true/>
```

> [!IMPORTANT]
> **Hinweis:** Ihrer app kann nicht den Benutzer einen erwerben erzwingen, dass Bluetooth-Gamecontroller sind eine optionale Kauf, der Endbenutzer vorgenommen werden können. Wenn Ihre app Gamecontroller unterstützt, muss es auch Siri Remote unterstützen, damit das Spiel von allen Benutzern von Apple TV verwendbar ist.

Weitere Informationen finden Sie unter unsere [arbeiten mit Gamecontroller](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) Teil unserer [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Inkompatible Zielframework:. NetPortable, Version = v4. 5, Profile = Profile78

Beim Versuch, eine Portable Klassenbibliothek (PCL) in ein Projekt Xamarin.tvOS enthalten Sie eine Meldung erhalten möglicherweise zu bilden:

_Inkompatible Zielframework:. NetPortable, Version = v4. 5, Profile = Profile78_

Um dieses Problem zu beheben, fügen Sie eine XML-Datei mit dem Namen ` Xamarin.TVOS.xml` mit folgendem Inhalt:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

In den folgenden Pfad:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Beachten Sie, dass die Anzahl der Profile im Pfad die Profil-Anzahl der PCL entsprechen muss.

Mit dieser Datei vorhanden ist sollten Sie erfolgreich die PCL-Datei dem Projekt Xamarin.tvOS hinzufügen können.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
