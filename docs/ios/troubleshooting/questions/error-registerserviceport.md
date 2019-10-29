---
title: iOS-Designer-Fehler mit RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 07b7e47ac448fdb9a4297fe03e8f4ea16295fddd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031139"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS-Designer-Fehler mit RegisterServicePort

## <a name="sample-error"></a>Beispiel Fehler
> System. AggregateException: ---> System. SystemException ist mindestens ein Fehler aufgetreten: Registerserviceport (com. xamarin. mthosting. 2a0b1, com. Apple. Powermanagement. Control): Zurückgegebener Kernel:-308 (-308): (IPC/MIG) Server ist ausgefallen.

## <a name="explanation"></a>Erklärung
Fehler mit `RegisterServicePort` und ähnlichen Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Schadsoftware auf dem Computer. Sehen Sie sich den [Kommentar zu diesem Fehlerbericht](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) an, um weitere Informationen zu erhalten, sowie den Link zur [Apple-Forumsdiskussion](https://discussions.apple.com/thread/5596008) zum Entfernen einer möglichen Infektion. 

Um die Diagnose des Problems zu unterstützen, öffnen Sie die macOS-Anwendungs **Konsole** , und löschen Sie alle Dateien im Abschnitt **Benutzer Diagnose Berichte** [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy). Starten Sie Visual Studio für Mac, und versuchen Sie, den Designer zu verwenden. Wenn nach dem fehlgeschlagenen Initialisieren des Designers in diesem Abschnitt neue Protokolldateien angezeigt werden, speichern Sie diese, damit Sie analysiert werden können.  

Beachten Sie unbedingt die folgende Datei: 
> /usr/lib/libimckit.dylib

Unabhängig von den oben aufgeführten Ergebnissen, wenn diese Datei vorhanden ist, ist das oben beschriebene Problem mit Spyware und Schadsoftware auf Ihrem Computer vorhanden.  

Der folgende Link enthält die Schritte zum Entfernen dieser Spyware/Malware: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
