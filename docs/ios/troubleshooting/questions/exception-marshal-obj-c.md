---
title: 'Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 1bb67eaa884e523e96ef1015daaa6b959ea1512d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031096"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>Warum schlägt meine iOS 9-App wie folgt fehl: System.Exception: Failed to marshal the Objective-C object (Das Objective-C-Objekt konnte nicht gemarshallt werden.)

Möglicherweise wird folgende Fehlermeldung angezeigt:

> System.Exception: Das Ziel-C-Objekt konnte nicht gemarshallt werden... Es wurde keine vorhandene verwaltete Instanz für dieses Objekt gefunden...

API-Änderungen in ios 9 erfordern, dass ein Rückruf Konstruktor verwendet wird, wenn nicht verwalteter Code aufgerufen wird, da die zugrunde liegende API dies nun erwartet. Verwenden Sie die folgende Zeile, um der-Klasse den Rückruf Konstruktor hinzuzufügen: 

`public foo (IntPtr handle) : base (handle)` 

### <a name="next-steps"></a>Nächste Schritte

Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. . 
