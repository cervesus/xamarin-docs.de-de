---
title: iOS-Designer-Fehler mit RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: b76de0f0dc8f1fd2124eac9cfe03b872c71095b1
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940954"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS-Designer-Fehler mit RegisterServicePort

## <a name="sample-error"></a>Beispiel Fehler
> System. AggregateException: mindestens ein Fehler ist aufgetreten---> System. SystemException: registerserviceport (com. xamarin. mthosting. 2a0b1, com. Apple. Powermanagement. Control): zurückgegebener Kernel:-308 (-308): (IPC/MIG) Server ist ausgefallen.

## <a name="explanation"></a>Erklärung
Fehler mit `RegisterServicePort` und ähnlichen Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Schadsoftware auf dem Computer. Sehen Sie sich den [Kommentar zu diesem Fehlerbericht](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) an, um weitere Informationen zu erhalten, sowie den Link zur [Apple-Forumsdiskussion](https://discussions.apple.com/thread/5596008) zum Entfernen einer möglichen Infektion. 

Um die Diagnose des Problems zu unterstützen, öffnen Sie die macOS-Anwendungs **Konsole** , und löschen Sie alle Dateien im Abschnitt **Benutzer Diagnose Berichte** [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy). Starten Sie Visual Studio für Mac, und versuchen Sie, den Designer zu verwenden. Wenn nach dem fehlgeschlagenen Initialisieren des Designers in diesem Abschnitt neue Protokolldateien angezeigt werden, speichern Sie diese, damit Sie analysiert werden können.  

Beachten Sie unbedingt die folgende Datei: 
> /usr/lib/libimckit.dylib

Unabhängig von den oben aufgeführten Ergebnissen, wenn diese Datei vorhanden ist, ist das oben beschriebene Problem mit Spyware und Schadsoftware auf Ihrem Computer vorhanden.  

Der folgende Link enthält die Schritte zum Entfernen dieser Spyware/Malware: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
