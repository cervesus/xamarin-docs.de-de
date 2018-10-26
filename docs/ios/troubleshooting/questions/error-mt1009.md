---
title: 'Fehler MT1009: Die Assembly konnte nicht kopiert werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4a67537cc53aeecf1b86d11dbf041cea79587dd2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105396"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Fehler MT1009: Die Assembly konnte nicht kopiert werden.

> [!IMPORTANT]
> In früheren Versionen von Xamarin.iOS hat dieses Problem behoben wurde. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.

Siehe unsere [– Anmerkungen zu dieser](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), dies ist ein bekanntes Problem in Xamarin.iOS 7.2.6. Dieses Problem ist aufgrund der Dateiberechtigungen, die erhöhte Rechte erforderlich, wenn Xamarin.iOS, und ein anderes Benutzerkonto installiert wird Hauptkonto des Entwicklers.

Zur Umgehung des Problems öffnen Sie das Terminal.app auf der Arbeitsstation Mac, und führen Sie den folgenden Befehl:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

