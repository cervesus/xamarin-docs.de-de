---
title: 'Fehler MT1009: Die Assembly konnte nicht kopiert werden.'
ms.topic: article
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 030c54275ef384f4a020abaf79456705e8fac0d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>Fehler MT1009: Die Assembly konnte nicht kopiert werden.

> [!IMPORTANT]
> Dieses Problem wurde bei neueren Versionen von Xamarin.iOS behoben. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.

Wie beschrieben in unserer [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), dies ist ein bekanntes Problem in Xamarin.iOS 7.2.6. Dieses Problem ist aufgrund von Dateiberechtigungen, die erhöhte Rechte erforderlich, wenn Xamarin.iOS, und ein anderes Benutzerkonto installiert wird Hauptkonto des Entwicklers.

Zur Umgehung des Problems öffnen Sie die Terminal.app auf der Arbeitsstation Mac, und führen Sie den folgenden Befehl:

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

