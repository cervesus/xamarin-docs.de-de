---
title: 'Fehler MT1009: Die Assembly konnte nicht kopiert werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 8973093d851cf2c38fe57bada0e9d01e9664b15b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293005"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Fehler MT1009: Die Assembly konnte nicht kopiert werden.

> [!IMPORTANT]
> Dieses Problem wurde in den letzten Versionen von xamarin. IOS gelöst. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

Wie in den Anmerkungen zu dieser [Version](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_7/xamarin.ios_7.2/index.md)beschrieben, handelt es sich hierbei um ein bekanntes Problem in xamarin. IOS 7.2.6. Dieses Problem ist darauf zurückzuführen, dass Dateiberechtigungen höhere Berechtigungen erfordern, wenn xamarin. IOS mit einem anderen Benutzerkonto als das Hauptkonto des Entwicklers installiert wird.

Um das Problem zu umgehen, öffnen Sie die Terminal. app auf der Mac-Arbeitsstation, und führen Sie den folgenden Befehl aus:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`
