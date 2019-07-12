---
title: 'Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 3dbb4d9132d5d94e4533704730e95002b5aec0be
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832270"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)

Derartige Fehler möglicherweise angezeigt:

> System.Exception: Fehler beim Marshallen des Objective-C-Objekts... Eine vorhandene verwaltete Instanz wurde für dieses Objekt nicht gefunden...

API-Änderungen in iOS 9 erfordern, dass ein Rückruf-Konstruktor verwendet werden, beim Aufrufen von nicht verwalteten Codes, als die zugrunde liegende API jetzt erwartet. Verwenden Sie die folgende Zeile, um den Rückruf-Konstruktor der Klasse hinzufügen: 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf. 
