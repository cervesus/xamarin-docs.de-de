---
title: „MDocArchiveToMsxDocConverter.exe“ nicht gefunden. rver.BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 3d21dfdbf6c9be00fe6851bb288268faccd74308
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030971"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>„MDocArchiveToMsxDocConverter.exe“ nicht gefunden. rver.BaseCommand.OnRequest

> [!IMPORTANT]
> Dieses Problem wurde in den letzten Versionen von xamarin gelöst. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

## <a name="error-message"></a>Fehlermeldung

Dieser Fehler wird möglicherweise im *Mac-Server Protokoll* in Visual Studio angezeigt:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

In dieser Meldung gibt es zwei separate Probleme:

1. `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Dieser Fehler ist harmlos, aber auch irreführend. Sie [wird](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) in einer zukünftigen Version entfernt.

2. `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Dieser Fehler ist das wirkliche Problem. Leider ist diese Ausnahme Stapel Überwachung aufgrund einer [Einschränkung](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) *unvollständig*. Wenn eine unvollständige Stapel Überwachung wie diese im Mac-Server Protokoll festgestellt wird, können Sie die `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` Datei auf dem Mac-buildhost überprüfen, um die vollständige Stapel Überwachung zu suchen.
