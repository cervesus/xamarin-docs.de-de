---
title: IPA-Datei hat 0 Byte
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 052df9676e4e6e105b5345d6120b884a3dead11f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031014"
---
# <a name="ipa-file-is-0-bytes"></a>IPA-Datei hat 0 Byte

> [!IMPORTANT]
> Dieses Problem wurde in den letzten Versionen von xamarin gelöst. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.

In früheren Versionen von xamarin gab es einige bekannte Probleme, die dazu führen konnten, dass die IPA-Datei unter Windows 0 Bytes groß war. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Korrigiert in xamarin für Visual Studio 3.11.584 

- [Fehler 24416: die Ad-hoc-Konfiguration wird von der Befehlszeile aus nicht in Windows kopiert.](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Fehler 24417-durch Ändern von "Projekteigenschaften-> IOS IPA-Optionen-> Paketname" wird verhindert, dass IPA zurück in Windows kopiert wird](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [Fehler 29822-[XVS. IOS 3,11] durch Festlegen der "Build"-Nummer, die sich von der Versionsnummer unterscheidet, wird IPA nicht in Windows kopiert.](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Korrigiert in xamarin für Visual Studio 4.1.0.496

- [Fehler 27989: IPA-Datei auf Buildserver anzeigen schlägt fehl, wenn der Assemblyname nicht mit dem Projektnamen identisch ist.](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
