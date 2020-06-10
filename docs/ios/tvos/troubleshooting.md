---
title: Problembehandlung bei mit xamarin erstellten tvos-apps
description: Dieser Artikel bietet verschiedene Tipps zur Problembehandlung bei der Entwicklung einer mit xamarin erstellten tvos-app. Es beschreibt bekannte Probleme und bestimmte Fehler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 124E4953-4DFA-42B0-BCFC-3227508FE4A6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c73be27ed82a643b01528ccba3887f59beeceb53
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574053"
---
# <a name="troubleshooting-tvos-apps-built-with-xamarin"></a>Problembehandlung bei mit xamarin erstellten tvos-apps

_In diesem Artikel werden Probleme behandelt, die bei der Arbeit mit der tvos-Unterstützung von xamarin auftreten können._

<a name="Known-Issues"></a>

## <a name="known-issues"></a>Bekannte Probleme

Die aktuelle Version der tvos-Unterstützung von xamarin hat die folgenden bekannten Probleme:

- **Mono-Framework** – Mono 4,3 Cryptography. ProtectedData kann Daten aus Mono 4,2 nicht entschlüsseln. Daher wird die Wiederherstellung von nuget-Paketen mit dem Fehler nicht ausgeführt, `Data unprotection failed` Wenn eine geschützte nuget-Quelle konfiguriert wird.
  - Problem **Umgehung** – in Visual Studio für Mac müssen Sie alle nuget-Paketquellen, die die Kenn Wort Authentifizierung verwenden, zurück fügen, bevor Sie versuchen, die Pakete wiederherzustellen.
- **Visual Studio für Mac w/F # Add-in** – Fehler beim Erstellen einer F #-Android-Vorlage unter Windows. Dies sollte weiterhin ordnungsgemäß auf dem Mac funktionieren.
- **Xamarin. Mac** – Wenn Sie das vereinheitlichte xamarin. Mac-Vorlagen Projekt ausführen und das Ziel Framework auf festgelegt `Unsupported` ist, wird das Popup `Could not connect to the debugger` möglicherweise angezeigt.
  - **Mögliche Problem Umgehung** – Herabstufen der Mono-Framework-Version, die in unserem stabilen Kanal verfügbar ist.
- **Xamarin Visual Studio & xamarin. IOS** – bei der Bereitstellung von watchkit-Anwendungen in Visual Studio wird der Fehler `The file ‘bin\iPhoneSimulator\Debug\WatchKitApp1WatchKitApp.app\WatchKitApp1WatchKitApp’ does not exist` möglicherweise angezeigt.

Melden Sie alle Fehler, die Sie auf [GitHub](https://github.com/xamarin/xamarin-macios/issues/new)finden.

## <a name="troubleshooting"></a>Problembehandlung

In den folgenden Abschnitten werden einige bekannte Probleme aufgelistet, die bei der Verwendung von tvos 9 mit xamarin. tvos auftreten können, sowie die Lösung für diese Probleme:

### <a name="invalid-executable---the-executable-does-not-contain-bitcode"></a>Ungültige ausführbare Datei-die ausführbare Datei enthält keinen Bitcode.

Wenn Sie versuchen, eine xamarin. tvos-APP an den Apple TV App Store zu übermitteln, erhalten Sie möglicherweise eine Fehlermeldung im Format _"Ungültige ausführbare Datei-die ausführbare Datei enthält keinen Bitcode"_.

Gehen Sie folgendermaßen vor, um dieses Problem zu beheben:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf die xamarin. tvos-Projektdatei im **Projektmappen-Explorer** , und wählen Sie **Optionen**aus.
2. Wählen Sie **tvos Build** aus, und stellen Sie sicher, dass Sie die **Releasekonfiguration** : 

    [![](troubleshooting-images/ts01.png "Select tvOS Build options")](troubleshooting-images/ts01.png#lightbox)
3. Fügen Sie `--bitcode=asmonly` dem Feld **zusätzliche mberührungs-Argumente** hinzu, und klicken Sie auf die Schaltfläche **OK** .
4. Erstellen Sie die app in der **Releasekonfiguration** neu.

### <a name="verifying-that-your-tvos-app-contains-bitcode"></a>Überprüfen, ob Ihre tvos-App Bitcode enthält

Öffnen Sie die Terminal-app, und geben Sie Folgendes ein, um zu überprüfen, ob Ihr xamarin. tvos-App-Build Bitcode enthält:

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

`addr`und `size` unterscheiden sich, aber andere Felder müssen identisch sein.

Sie müssen sicherstellen, dass alle von Ihnen verwendeten statischen ( `.a` ) Drittanbieter-Bibliotheken für tvos-Bibliotheken (nicht für IOS-Bibliotheken) erstellt wurden und dass Sie auch Bitcode Informationen enthalten.

Für apps oder Bibliotheken, die gültigen Bitcode enthalten, `size` ist größer als 1. Es gibt einige Situationen, in denen eine Bibliothek über den Bitcode-Marker verfügen kann, aber keinen gültigen Bitcode enthalten kann. Beispiel:

**Ungültiger Bitcode**

```csharp
 $ otool -arch arm64 libLibrary.a | grep __bitcode -A 3
   sect name __bitcode
   segname __LLVM
      add 0x0000000000000670
      size 0x0000000000000001
```

**Gültiger Bitcode** 

```csharp
$ otool -l -arch arm64 libDownloadableAgent-tvos.a |grep __bitcode -A 3
   sectname __bitcode
   segname __LLVM
      addr 0x000000000001d2d0
      size 0x0000000000045440
```

Beachten Sie, dass der Unterschied `size` zwischen den beiden Bibliotheken im aufgeführten Beispiel oben ausgeführt wird. Die Bibliothek muss von einem Xcode-Archivierungs Build mit aktiviertem Bitcode (Xcode-Einstellung `ENABLE_BITCODE` ) als Lösung für dieses Größen Problem generiert werden.

### <a name="apps-that-only-contain-the-arm64-slice-must-also-have-arm64-in-the-list-of-uirequireddevicecapabilities-in-infoplist"></a>Apps, die nur den arm64 Slice enthalten, müssen in der Liste der uirequirements ddevicecapabiliin "Info. plist" auch "arm64" enthalten.

Wenn Sie eine APP an den Apple TV App Store zur Veröffentlichung übermitteln, erhalten Sie möglicherweise eine Fehlermeldung in der Form:

_"Apps, die nur den arm64 Slice enthalten, müssen in der Liste der uirequirements ddevicecapabiliin" Info. plist "auch" arm64 "enthalten._

Wenn dies auftritt, bearbeiten `Info.plist` Sie die Datei, und stellen Sie sicher, dass Sie über die folgenden Schlüssel verfügt:

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
  <string>arm64</string>
</array>
```

Kompilieren Sie Ihre APP erneut, und senden Sie Sie erneut an iTunes Connect.

### <a name="task-mtouch-execution----failed"></a>Task "mtouchscreen-Ausführung--Fehler"

Wenn Sie eine Drittanbieter Bibliothek verwenden (z. b. monogame) und die Releasekompilierung mit einer langen Reihe von Fehlermeldungen beendet `Task "MTouch" execution -- FAILED` wurde, versuchen Sie, `-gcc_flags="-framework OpenAL"` Ihre **zusätzlichen Berührungs Argumente**hinzuzufügen:

[![](troubleshooting-images/mtouch01.png "Task MTouch execution")](troubleshooting-images/mtouch01.png#lightbox)

Sie sollten auch `--bitcode=asmonly` in die **zusätzlichen Berührungs Argumente**einschließen, lassen Sie die **Linkeroptionen auf alle verknüpfen** und eine saubere Kompilierung ausführen.

### <a name="itms-90471-error-the-large-icon-is-missing"></a>ITMS-90471-Fehler. Das große Symbol fehlt.

Wenn Sie eine Meldung im Format "iTMS-90471 Error" erhalten. Das große Symbol fehlt "" beim Versuch, eine xamarin. tvos-App für die Veröffentlichung an den Apple TV App Store zu übermitteln, überprüfen Sie Folgendes:

1. Stellen Sie sicher, dass Sie die großen Symbol Objekte in `Assets.car` die Datei eingefügt haben, die Sie mithilfe der [App](~/ios/tvos/app-fundamentals/icons-images.md#App-Icons) -Symbol Dokumentation erstellt haben.
2. Stellen Sie sicher, dass Sie die `Assets.car` Datei aus der Dokumentation zum [Arbeiten mit Symbolen und Bildern](~/ios/tvos/app-fundamentals/icons-images.md) in das endgültige Anwendungspaket eingefügt haben.

### <a name="invalid-bundle--an-app-that-supports-game-controllers-must-also-support-the-apple-tv-remote"></a>Ungültiges Bündel – eine APP, die Spiele Controller unterstützt, muss auch das Apple TV Remote unterstützen.

oder 

### <a name="invalid-bundle--apple-tv-apps-with-the-gamecontroller-framework-must-include-the-gcsupportedgamecontrollers-key-in-the-apps-infoplist"></a>Ungültiges Bündel – Apple TV-apps mit dem Gamecontroller-Framework müssen den gcsupportedgamecontrollers-Schlüssel in der "Info. plist" der App enthalten.

Spiele Controller können verwendet werden, um das Spiel zu verbessern und ein Gefühl für das Eintauchen in ein Spiel zu bieten. Sie können auch verwendet werden, um die standardmäßige Apple TV-Schnittstelle zu steuern, sodass der Benutzer nicht zwischen Remote und Controller wechseln muss.

Wenn Sie eine xamarin. tvos-App mit Game Controller-Unterstützung für den Apple TV App Store übermitteln und eine Fehlermeldung mit dem folgenden Code erhalten:

_Wir haben ein oder mehrere Probleme mit Ihrer aktuellen Übermittlung für "App-Name" erkannt. Ihre Übermittlung war erfolgreich, aber Sie möchten möglicherweise die folgenden Probleme bei der nächsten Übermittlung beheben:_

_Ungültiges Bündel – eine APP, die Spiele Controller unterstützt, muss auch das Apple TV-Remote unterstützen._

oder 

_Ungültiges Bündel – Apple TV-apps mit dem Gamecontroller-Framework müssen den gcsupportedgamecontrollers-Schlüssel in der "Info. plist" der App enthalten._

Die Lösung besteht darin, die Unterstützung für die Siri `GCMicroGamepad` -Remote Datei () zur Datei Ihrer APP hinzuzufügen `Info.plist` . Das Micro Game Controller-Profil wurde von Apple hinzugefügt, um das Siri-Remote Ziel zu erreichen. Fügen Sie z. b. die folgenden Schlüssel ein:

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
> Bluetooth-Spiele Controller sind ein optionaler Kauf, den Endbenutzer möglicherweise treffen, Ihre APP kann den Benutzer nicht zwingen, eine solche zu erwerben. Wenn Ihre APP Game Controller unterstützt, muss Sie auch die Siri-Remote Unterstützung unterstützen, damit das Spiel von allen Apple TV-Benutzern verwendbar ist.

Weitere Informationen finden Sie im Abschnitt [Arbeiten mit Spiel Controllern](~/ios/tvos/platform/remote-bluetooth.md#Working-with-Game-Controllers) in der Dokumentation zu den [Siri-Remote-und Bluetooth-Controllern](~/ios/tvos/platform/remote-bluetooth.md) .

### <a name="incompatible-target-framework-netportable-versionv45-profileprofile78"></a>Inkompatibles Ziel Framework:. Netportable, Version = v 4.5, Profile = Profile78

Wenn Sie versuchen, eine portable Klassenbibliothek (PCL) in ein xamarin. tvos-Projekt einzufügen, erhalten Sie möglicherweise eine Nachricht in, um Folgendes zu erstellen:

_Inkompatibles Ziel Framework:. Netportable, Version = v 4.5, Profile = Profile78_

Um dieses Problem zu beheben, fügen Sie eine XML-Datei `Xamarin.TVOS.xml` mit dem Namen und folgendem Inhalt hinzu:

```xml
<Framework Identifier="Xamarin.TVOS" MinimumVersion="1.0" Profile="*" DisplayName="Xamarin.TVOS"/>
```

Zum folgenden Pfad:

```csharp
/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/Profile/Profile259/SupportedFrameworks/

```

Beachten Sie, dass die Profil Nummer im Pfad der Profil Nummer der PCL entsprechen muss.

Wenn diese Datei vorhanden ist, sollten Sie in der Lage sein, die PCL-Datei erfolgreich dem xamarin. tvos-Projekt hinzuzufügen.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
