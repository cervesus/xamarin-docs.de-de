---
title: Problembehandlung für TvOS-apps mit Xamarin
description: Dieser Artikel enthält verschiedene Tipps zur Problembehandlung bei der Entwicklung einer TvOS-App mit Xamarin erstellt wurde. Bekanntes Problem und Fehlermeldungen beschrieben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 6830df267aa0b9c4f12fbd53520206ea94fc8a38
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831898"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Problembehandlung für TvOS-apps mit Xamarin

_In diesem Artikel behandelt wissen auftretende Probleme bei der Arbeit mit Xamarin TvOS-Unterstützung._

<a name="Known-Issues" />

## <a name="known-issues"></a>Bekannte Probleme

Die aktuelle Version von Xamarin TvOS-Unterstützung hat die folgenden bekannten Probleme:

- **Mono-Framework** – Mono 4.3 Cryptography.ProtectedData zum Entschlüsseln von Daten von Mono 4.2 fehlschlägt. Daher fehl, NuGet-Pakete mit dem Fehler wiederherstellen `Data unprotection failed` Wenn eine geschützte NuGet-Quelle konfiguriert ist.
    - **Problemumgehung** – In Visual Studio für Mac müssen Sie wieder hinzufügen alle NuGet-Pakete, Datenquellen, das Kennwort-Authentifizierung verwenden, bevor Sie versuchen, erneut, um die Pakete wiederherzustellen.
- **Visual Studio für Mac mit F# -Add-in** – Fehler beim Erstellen einer F# Android-Vorlage für Windows. Dies sollte immer noch auf Mac ordnungsgemäß
- **Xamarin.Mac** – Wenn das Zielframework das Xamarin.Mac unified-Vorlagenprojekt mit, dass festgelegt `Unsupported`, das Popup `Could not connect to the debugger` kann angezeigt werden.
    - **Mögliche Lösung** – ein Downgrade die Mono-Framework-Version, die in unserer stabilen Kanal verfügbar.
- **Xamarin für Visual Studio und Xamarin.iOS** – bei der Bereitstellung von WatchKit-Anwendungen in Visual Studio der Fehler `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` kann angezeigt werden.

Bericht, die alle Fehler, Sie finden auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new).

## <a name="troubleshooting"></a>Problembehandlung

Den folgenden Abschnitten werden einige bekannte Probleme, die Verwendung von TvOS 9 mit Xamarin.tvOS und die Lösung dieser Probleme auftreten können:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Ungültige ausführbare Datei: die ausführbare Datei Bitcode enthält nicht

Wenn beim Versuch, eine Xamarin.tvOS-app an das Apple TV App Store übermitteln, erhalten Sie möglicherweise eine Fehlermeldung in der Form _"Ungültige ausführbare Datei: die ausführbare Datei enthält keine Bitcode"_ .

Um dieses Problem zu beheben, führen Sie folgende Schritte aus:

1. Visual Studio für Mac, mit der Maustaste auf Ihre Xamarin.tvOS-Projektdatei in die **Projektmappen-Explorer** , und wählen Sie **Optionen**.
2. Wählen Sie **TvOS-Build** und stellen Sie sicher, dass Sie sich auf befinden die **Version** Konfiguration: 

    [![](troubleshooting-images/ts01.png "Wählen Sie die TvOS-Buildoptionen")](troubleshooting-images/ts01.png#lightbox)
3. Hinzufügen `--bitcode=asmonly` auf die **Weitere Mtouch-Argumente** ein, und klicken Sie auf die **OK** Schaltfläche.
4. Neu erstellen Ihrer app in der **Version** Konfiguration.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Überprüfen, die Ihrer TvOS-App enthält Bitcode

Um sicherzustellen, dass Ihre Xamarin.tvOS-App-Builds Bitcode enthält, öffnen Sie die Terminal-app aus, und geben Sie Folgendes ein:

```csharp
otool -l /path/to/your/tv.app/tv
```

Suchen Sie in der Ausgabe wird für Folgendes:

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

`addr` und `size` unterscheiden sich jedoch die anderen Felder sollten identisch sein.

Sie müssen sicherstellen, dass statische Dritter (`.a`) Bibliotheken, die Sie verwenden für TvOS-Bibliotheken (nicht-iOS-Bibliotheken) erstellt wurden und sie gibt außerdem Aufschluss Bitcode.

Für apps oder Bibliotheken, die gültige Bitcode enthalten die `size` ist größer als 1 sein. Es gibt einige Situationen, in denen eine Bibliothek kann die Markierung Bitcode, noch über keine gültige Bitcode enthalten. Beispiel:

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

Beachten Sie den Unterschied in `size` zwischen den beiden Bibliotheken im aufgeführten Beispiel ausgeführt wird, oben. Die Bibliothek aus einem Xcode-Archiv-Build generiert werden muss, mit Bitcode aktiviert (Xcode-Einstellung `ENABLE_BITCODE`) als Lösung für dieses Problem Größe.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Apps, die nur die arm64-Segment enthalten müssen auch "arm64" in der Liste der "uirequireddevicecapabilities" in "Info.plist" verfügen.

Wenn Sie eine app an das Apple TV App Store für die Veröffentlichung zu übermitteln, erhalten Sie möglicherweise einen Fehler in der Form:

_"Apps, die nur die arm64-Segment enthalten auch"arm64"in der Liste der" uirequireddevicecapabilities "in" Info.plist "müssen"_

In diesem Fall Bearbeiten Ihrer `Info.plist` Datei, und stellen Sie sicher, dass sie die folgenden Schlüssel hat:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
    <string>arm64</string>
</array>
```

Kompilieren Sie Ihre app für die Veröffentlichung neu, und erneut in iTunes Connect übermitteln.

### <a name="task-mtouch-execution----failed"></a>"MTouch" – Fehler bei der Taskausführung

Wenn Sie eine 3rd Party-Bibliothek (z. B. MonoGame) verwenden und Ihre Version Kompilierung bei der eine lange Reihe von Fehlermeldungen, die mit `Task "MTouch" execution -- FAILED`, versuchen Sie es hinzufügen `-gcc_flags="-framework OpenAL"` auf Ihre **zusätzliche Touch Argumente**:

[![](troubleshooting-images/mtouch01.png "MTouch-aufgabenausführung")](troubleshooting-images/mtouch01.png#lightbox)

Sie sollten auch einschließen, `--bitcode=asmonly` in die **zusätzliche Touch-Argumente**, Ihre Optionen des Linkers festgelegt haben **alle verknüpfen** und führen Sie eine einwandfreie Kompilierung.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471-Fehler. Große Symbol ist nicht vorhanden

Wenn Sie eine Nachricht in der Form "ITMS-90471. Fehlermeldung Große Symbol ist nicht vorhanden"beim Versuch, eine Xamarin.tvOS-app an das Apple TV App Store für Version, Senden überprüfen Sie bitte Sie Folgendes:

1. Stellen Sie sicher, dass Sie die Objekte "große Symbole" im haben Ihre `Assets.car` -Datei, die Sie erstellt haben, mit der [-App-Symbole](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) Dokumentation.
2. Stellen Sie sicher, dass Sie enthalten die `Assets.car` -Datei aus der [arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) Dokumentation in Ihrer endgültigen Anwendungspaket.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Ungültiges Paket: muss eine app, die Gamecontroller unterstützt auch Remotecomputer Apple TV unterstützen

oder 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Ungültiges Paket – Apple TV-apps mit GameController Framework muss in der app "Info.plist" den GCSupportedGameControllers Schlüssel enthalten.

Gamecontroller können verwendet werden, verbessern Spiele, und geben Sie einen Eindruck davon zu Entwicklungsthemen in einem Spiel. Sie können auch verwendet werden, um die Standardschnittstelle für Apple TV zu steuern, sodass der Benutzer nicht zum Wechseln zwischen dem Remotecomputer und der Controller.

Wenn eine Xamarin.tvOS-app mit Gamecontroller-Unterstützung im Apple TV App Store übermittelt werden, und Sie eine Fehlermeldung in Form von erhalten:


_Wir haben eine oder mehrere Probleme mit Ihrer aktuellen Bereitstellung für "Anwendungsname" erkannt. Die Übermittlung war erfolgreich, aber möglicherweise möchten Sie Ihre nächste Bereitstellung die folgenden Probleme zu beheben:_

_Ungültiges Paket: muss eine app, die Gamecontroller unterstützt auch Remotecomputer Apple TV unterstützen._

oder 

_Ungültiges Paket – Apple TV-apps mit GameController Framework muss den GCSupportedGameControllers-Schlüssel in der app "Info.plist" enthalten._

Die Lösung besteht darin, Hinzufügen der Unterstützung für die Siri Remote (`GCMicroGamepad`) zu Ihrer app `Info.plist` Datei. Das Micro Game Controller-Profil wurde von Apple Siri Remote Ziel hinzugefügt. Beispielsweise enthalten Sie die folgenden Schlüssel:

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
> Bluetooth-Gamecontroller sind eine optionale erwerben, die Endbenutzer machen können, Ihre app kann nicht zwingen den Benutzer, eine erwerben. Wenn Ihre app Gamecontroller unterstützt muss auch die Siri Remote unterstützt werden, damit das Spiel von allen Benutzern von Apple TV verwendbar ist.

Weitere Informationen finden Sie unsere [arbeiten mit Gamecontroller](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) Teil unserer [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Inkompatibles Zielframework:. NetPortable, Version = 4.5 "," Profile = Profile78

Beim Versuch, eine Portable Klassenbibliothek (PCL) in ein Xamarin.tvOS-Projekt enthalten kann eine Nachricht werden bilden:

_Inkompatibles Zielframework:. NetPortable, Version = 4.5 "," Profile = Profile78_

Um dieses Problem zu beheben, fügen Sie eine XML-Datei mit dem Namen `Xamarin.TVOS.xml` mit folgendem Inhalt:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

In den folgenden Pfad:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Beachten Sie, dass die Anzahl der Profile im Pfad die Profil-Anzahl der PCL entsprechen muss.

Mit dieser Datei vorhanden ist sollten Sie die PCL-Datei erfolgreich der Xamarin.tvOS-Projekt hinzufügen können.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
