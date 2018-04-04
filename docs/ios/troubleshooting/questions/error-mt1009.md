---
title: 'Fehler MT1009: Die Assembly konnte nicht kopiert werden.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Fehler MT1009: Die Assembly konnte nicht kopiert werden.

> [!IMPORTANT]
> Dieses Problem wurde bei neueren Versionen von Xamarin.iOS behoben. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.

Wie beschrieben in unserer [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), dies ist ein bekanntes Problem in Xamarin.iOS 7.2.6. Dieses Problem ist aufgrund von Dateiberechtigungen, die erhöhte Rechte erforderlich, wenn Xamarin.iOS, und ein anderes Benutzerkonto installiert wird Hauptkonto des Entwicklers.

Zur Umgehung des Problems öffnen Sie die Terminal.app auf der Arbeitsstation Mac, und führen Sie den folgenden Befehl:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

