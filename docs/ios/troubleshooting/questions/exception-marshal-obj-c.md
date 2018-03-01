---
title: "Warum wird meine app für iOS 9 ausgeführt, mit: System.Exception: Fehler beim Objective-C-Objekt Marshallen?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: baba2526eefa1b69d47da47b73ea0bd417ecdc57
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Warum wird meine app für iOS 9 ausgeführt, mit: System.Exception: Fehler beim Objective-C-Objekt Marshallen?

Möglicherweise wird einen Fehler des Formulars angezeigt:

> System.Exception: Fehler bei der Objective-C-Objekt marshallen... Eine Instanz des vorhandene verwaltete wurde für dieses Objekt nicht gefunden werden...

API-Änderungen in iOS 9 erfordern, dass ein Rückruf-Konstruktor verwendet werden, wenn Aufrufen von nicht verwaltetem Code als der zugrunde liegenden API jetzt er davon ausgeht. Verwenden Sie die folgende Zeile, die Rückruf-Konstruktor der Klasse hinzu: 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>Nächste Schritte

Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf. 
