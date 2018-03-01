---
title: iOS-Designer-Fehler mit RegisterServicePort
ms.topic: article
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8042ea895de3a623279c9d5f3a5309b990489773
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS-Designer-Fehler mit RegisterServicePort

## <a name="sample-error"></a>Beispiel-Fehler
> System.AggregateException: Eine oder mehrere Fehler---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): Kernel zurückgegeben:-308 (-308): (Ipc/Mig)-Server wurde unerwartet beendet

## <a name="explanation"></a>Erklärung
Fehler mit `RegisterServicePort` und ähnliche Fehlermeldungen wie oben sind häufig ein Problem mit Spyware/Malware auf dem Computer. Erwägen Sie die [Kommentar für dieses Fehlerbericht](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) zusammen mit den Link, um weitere Informationen die [Apple-Forumsdiskussion](https://discussions.apple.com/thread/5596008) auf eine mögliche Infektion zu entfernen. 

Um Unterstützung bei der Diagnose des Problems, öffnen Sie die Anwendung MacOS **Konsole** und löschen Sie jede Datei innerhalb der **diagnostische Benutzerberichten** Abschnitt [http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy). Anschließend starten Sie Visual Studio für Mac, und versuchen Sie, den Designer zu verwenden. Wenn neuen Protokolldateien in diesem Abschnitt angezeigt, nachdem der Designer nicht initialisiert wurde, speichern Sie diese für uns zu analysieren.  

Bitte beachten Sie, dass die wichtigste zu suchende diese Datei ist ein: 
> /usr/lib/libimckit.dylib

Wenn diese Datei vorhanden ist, ist das zuvor erwähnten Spyware/Malware Problem unabhängig von der oben erwähnten Ergebnisse auf dem Computer vorhanden.  

Im folgende Link umfasst die Schritte zum Entfernen dieser Spyware/Malware: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

