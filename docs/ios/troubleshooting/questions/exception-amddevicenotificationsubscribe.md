---
title: System.Exception AMDeviceNotificationSubscribe zurückgegeben ...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8ae465c98dee25cd0f1fe635da45f4d399b42ee3
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284483"
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>System.Exception AMDeviceNotificationSubscribe zurückgegeben ...

> [!IMPORTANT]
> Dieses Problem wurde in den letzten Versionen von xamarin gelöst. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.


## <a name="fix"></a>Korrektur

1. Beenden Sie `usbmuxd` den Prozess, sodass er vom System neu gestartet wird:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2. Wenn das Problem nicht behoben wird, starten Sie den Mac neu.

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

Diese Meldung kann beim ersten Start Visual Studio für Mac in einem Fehler Dialogfeld oder in der `mtbserver.log` Datei in der xamarin. IOS-buildhost-app (**xamarin. IOS-buildhost > Anzeigen des buildhostprotokolls**) angezeigt werden.

Beachten Sie, dass dies ein ungewöhnliches Problem ist. Wenn Visual Studio Probleme beim Herstellen einer Verbindung mit dem Mac-buildhost hat, gibt es weitere Fehler, die in der `mtbserver.log` Datei wahrscheinlicher erscheinen.

### <a name="errors-in-systemlog"></a>Fehler in "System. log"

In einigen Fällen werden möglicherweise die folgenden beiden Fehlermeldungen in `/var/log/system.log`wiederholt angezeigt:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Zusätzliche Informationen

Der Grund für den Fehler liegt darin, dass die Systemdienste OS X, die für die Berichterstattung von IOS-Geräte-und simulatorinformationen verantwortlich sind, in seltenen Fällen einen unerwarteten Status aufweisen können. Xamarin kann in diesem Zustand nicht ordnungsgemäß mit den System Diensten interagieren. Beim Neustart des Computers werden die Systemdienste neu gestartet und das Problem gelöst.

Basierend auf den auftretenden Fehlern `system.log` erscheint dieses Problem möglicherweise in Bezug auf Bonjour (`mDNSResponder`). Die Umstellung zwischen verschiedenen WiFi-Netzwerken scheint die Wahrscheinlichkeit zu erhöhen, dass das Problem auftritt.

## <a name="references"></a>Verweise

* [Bug 11789-MonoTouch. MobileDevice. mobiledeviceexception: AMDeviceNotificationSubscribe hat Folgendes zurückgegeben: 0xe8000063 [aufgelöst NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
