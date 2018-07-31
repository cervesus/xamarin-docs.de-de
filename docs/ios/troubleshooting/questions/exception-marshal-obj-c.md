---
title: 'Warum schlägt Meine iOS 9-app fehl mit: System.Exception: Fehler beim Marshallen des Objective-C-Objekts?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 93666fcb39f0cd717c14eb07e6407801e9f0642e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350598"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Warum schlägt Meine iOS 9-app fehl mit: System.Exception: Fehler beim Marshallen des Objective-C-Objekts?

Derartige Fehler möglicherweise angezeigt:

> System.Exception: Fehler beim Marshallen des Objective-C-Objekts... Eine vorhandene verwaltete Instanz wurde für dieses Objekt nicht gefunden...

API-Änderungen in iOS 9 erfordern, dass ein Rückruf-Konstruktor verwendet werden, beim Aufrufen von nicht verwalteten Codes, als die zugrunde liegende API jetzt erwartet. Verwenden Sie die folgende Zeile, um den Rückruf-Konstruktor der Klasse hinzufügen: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf. 
