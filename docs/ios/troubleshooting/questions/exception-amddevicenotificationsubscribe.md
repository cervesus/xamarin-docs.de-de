---
title: System.Exception AMDeviceNotificationSubscribe zurückgegebene...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 257e0c14de3c23825b6abe6601c25438db81c58e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776481"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe zurückgegebene...

> [!IMPORTANT]
> Dieses Problem wurde in den neuesten Versionen von Xamarin behoben. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.


## <a name="fix"></a>Problemlösung

1.  Beenden der `usbmuxd` verarbeiten, damit es Neustart des Systems:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Wenn, die das Problem weiterhin besteht, starten Sie neu dem Mac

## <a name="error-message"></a>Fehlermeldung

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

Diese Meldung kann in ein Fehlerdialogfeld angezeigt, wenn Sie Visual Studio für Mac oder in zuerst starten der `mtbserver.log` Datei in der app Xamarin.iOS-Buildhost (**Xamarin.iOS-Buildhost > Erstellen Sie Host-Protokoll anzeigen**).

Beachten Sie, dass dies ungewöhnlich, dass ein Problem aufgetreten ist. Wenn Visual Studio Probleme beim Herstellen einer Verbindung mit dem Mac-Build-Host aufgetreten sind, sind andere Fehler, die wahrscheinlich in angezeigt werden die `mtbserver.log` Datei.

### <a name="errors-in-systemlog"></a>Fehler in system.log

In einigen Fällen die folgenden beiden Fehler Nachrichten möglicherweise auch angezeigt wiederholt `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Zusätzliche Informationen

Eine Schätzung auf die Ursache des Fehlers ist, dass die OS X-Systemdienste verantwortlich für die Berichterstattung von iOS-Gerät und der Simulator-Informationen in seltenen Fällen können einen nicht erwarteten Status eingeben. Xamarin kann nicht ordnungsgemäß mit Systemdienste in diesem Zustand interagieren. Neustart des Computers Neustart Systemdienste und löst das Problem.

Basierend auf den Fehler aus `system.log` er angezeigt wird, dass dieses Problem zu Bonjour verknüpft sein kann (`mDNSResponder`). Wechseln zwischen verschiedenen WiFi-Netzwerke scheint erhöhen die Wahrscheinlichkeit des Erreichens des Problems.

## <a name="references"></a>Verweise

*   [Bug 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe returned: 0xe8000063 [RESOLVED NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
