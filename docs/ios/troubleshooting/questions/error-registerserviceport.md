---
title: iOS-Designer-Fehler mit RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 9edcc822b170c3463908b9f5fb1db8b798346e3e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350705"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS-Designer-Fehler mit RegisterServicePort

## <a name="sample-error"></a>Beispielfehler
> System.AggregateException: Mindestens ein Fehler---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): Kernel zurückgegeben:-308 (-308): (Ipc/Mig)-Server wurde unerwartet beendet

## <a name="explanation"></a>Erklärung
Fehler bei `RegisterServicePort` und ähnliche Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Schadsoftware auf dem Computer. Erwägen Sie die [Kommentar auf dieser Fehlerbericht](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) zusammen mit den Link, um weitere Informationen die [Apple Forumsdiskussion](https://discussions.apple.com/thread/5596008) auf eine mögliche Infektion entfernen. 

Hilft bei der Diagnose des Problems, öffnen Sie die MacOS-Anwendung **Konsole** und löschen Sie jede Datei innerhalb der **Benutzer Diagnoseberichte** Abschnitt [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Klicken Sie dann starten Sie Visual Studio für Mac, und versuchen Sie es den Designer verwendet. Wenn neuen Protokolldateien in diesem Abschnitt angezeigt, nachdem der Designer konnte nicht initialisiert wurde, speichern Sie diese für uns zu analysieren.  

Beachten Sie, dass das wichtigste zu prüfen, um diese Datei ist: 
> /usr/lib/libimckit.dylib

Wenn diese Datei vorhanden ist, ist das oben genannte Spyware/Malware-Problem unabhängig von den oben aufgeführten Ergebnissen auf dem Computer vorhanden.  

Im folgende Link verfügt über die Schritte zum Entfernen dieser Spyware/Malware: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

