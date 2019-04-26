---
title: System.Exception AMDeviceNotificationSubscribe zurückgegeben ...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4fb0712366422e8810a2db60d40c3b85d9f4cd82
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421943"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe zurückgegeben ...

> [!IMPORTANT]
> In früheren Versionen von Xamarin hat dieses Problem behoben wurde. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.


## <a name="fix"></a>Korrektur

1.  Beenden der `usbmuxd` verarbeitet werden, damit das System wird es neu starten:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Wenn das Problem auf diese Weise nicht gelöst werden kann, neu starten Sie den Mac.

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

Diese Meldung kann in ein Fehlerdialogfeld angezeigt, wenn Sie Visual Studio für Mac oder in zunächst die `mtbserver.log` -Datei in die Xamarin.iOS-buildhostanwendung (**Xamarin.iOS-Buildhost > Build Host-Protokoll anzeigen**).

Beachten Sie, dass es sich um ein ungewöhnlich, dass Problem handelt. Wenn Visual Studio beim Herstellen einer Verbindung mit dem Mac-buildhost Probleme sollten ist, stehen Ihnen andere Fehler, die eher in angezeigt werden, sind die `mtbserver.log` Datei.

### <a name="errors-in-systemlog"></a>Fehler in system.log

In einigen Fällen die folgenden beiden Fehler Nachrichten möglicherweise auch angezeigt, wiederholt in `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Zusätzliche Informationen

Eine Vermutung in Bezug auf die eigentliche Ursache des Fehlers ist, dass OS X-Systemdienste, die verantwortlich für die iOS-Geräten und Simulatoren Berichtsinformationen in seltenen Fällen können einen unerwarteten Zustand eingeben. Xamarin kann nicht ordnungsgemäß mit der Systemdienste in diesem Zustand interagieren. Neustart des Computers die Systemdienste neu gestartet, und löst das Problem.

Basierend auf den Fehler `system.log` er angezeigt wird, dass dieses Problem mit Bonjour verknüpft werden kann (`mDNSResponder`). Wechsel zwischen verschiedenen WLANs scheint erhöhen die Wahrscheinlichkeit, dass das Problem auf.

## <a name="references"></a>Verweise

*   [Fehler 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe zurückgegeben: 0XE8000063 [AUFGELÖST NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
