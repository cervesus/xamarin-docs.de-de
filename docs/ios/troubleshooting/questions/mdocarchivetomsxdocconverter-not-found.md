---
title: MDocArchiveToMsxDocConverter.exe rvername nicht gefunden. BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 0746174857f66843ef9a09429b6286f2efca90d6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119760"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe rvername nicht gefunden. BaseCommand.OnRequest

> [!IMPORTANT]
> In früheren Versionen von Xamarin hat dieses Problem behoben wurde. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.


## <a name="error-message"></a>Fehlermeldung

Dieser Fehler kann auftreten, der *Mac-Serverprotokoll* in Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Es gibt 2 separate Probleme in dieser Meldung:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Dieser Fehler ist harmlos, aber es ist ebenfalls irreführend. Es [entfernt](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) in einer zukünftigen Version.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Dieser Fehler ist das eigentliche Problem. Leider aufgrund von einem [Einschränkung](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) dieser stapelüberwachung der Ausnahme ist *unvollständige*. Wenn Sie eine unvollständige stapelüberwachung wie folgt in die Mac-Serverprotokoll feststellen, können Sie überprüfen die `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` Datei auf dem Mac-buildhost dem kompletten stapelverlauf gefunden.
