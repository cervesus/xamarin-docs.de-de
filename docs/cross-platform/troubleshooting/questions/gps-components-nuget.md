---
title: Vereinheitlichung der Google Play Services, Komponenten und NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.openlocfilehash: cfd417f4fc01b07b4334259c45472eb24b73abd8
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919871"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Vereinheitlichung der Google Play Services, Komponenten und NuGet

### <a name="history"></a>Versionsgeschichte

Gibt es mehrere Google-Dienstekomponenten wiedergeben und NuGet-Pakete werden verwendet:

-   Google Play-Dienste (Froyo)
-   Google Play-Dienste (Gingerbread)
-   Google Play-Dienste (ICS)
-   Google Play-Dienste (JellyBean)
-   Google Play-Dienste (KitKat)

Google tatsächlich nur umfasst zwei JAR-Dateien für Google Play-Dienste:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Die Abweichung vorhanden waren, da unsere Tools ordnungsgemäß erkennen nicht `aapt.exe` was die maximale Ressource API-Ebene für eine bestimmte app verwendet wurde. Dies bedeutet, dass wir Kompilierungsfehler erhalten, wenn es wurde versucht, mit der Bindung von Google Play-Dienste (KitKat) auf einer niedrigeren Ebene der API wie Gingerbread.

### <a name="unifying-google-play-services"></a>Vereinheitlichung der Google Play-Dienste

In neueren Versionen von Xamarin.Android, teilen wir jetzt `aapt.exe` zu verwenden, damit dieses Problem für uns verschwindet, welche maximale-Version.

Ist dies bedeutet, dass es keinen Grund, separate Paketen für Gingerbread/ICS/JellyBean/KitKat (jedoch wir noch benötigen eine separate Bindung für Froyo, da es eine andere JAR-Datei vollständig ist).

Um für Entwickler vereinfachen, wir haben jetzt vereinheitlichte unsere Komponenten und NuGet-Pakete in zwei:

-   Google Play-Dienste (Froyo) (bindet `google-play-services-froyo.jar`)
-   Google Play-Dienste (bindet `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Welche sollte verwendet werden?

In nahezu jedem Fall sollte die Google Play-Dienste verwendet werden. Der einzige Grund für das Paket (Froyo) zu verwenden ist, wenn Sie aktiv Froyo abzielen. Der einzige Grund, dass diese separaten JAR-Datei von Google vorhanden ist, da Froyo auf einen kleinen Prozentsatz der Geräte ist, möchten sie selbst beenden unterstützen, damit diese JAR-Datei eine fixierte, nicht unterstützte Momentaufnahme von Google Play-Dienste ist.

### <a name="note-about-gingerbread"></a>Hinweis zur Gingerbread

Gingerbread keine Fragment standardmäßig unterstützt, und aus diesem Grund einige der Klassen in der Bindung werden nicht in einer app zur Laufzeit auf einem Gerät Gingerbread verwendet werden kann. Klassen wie `MapFragment` Gingerbread nicht funktionsfähig, und ihre Unterstützung Variante sollte stattdessen verwendet werden `SupportMapFragment`. Es liegt an der Entwickler wissen, welche verwenden. Diese Inkompatibilität wird von Google in der Dokumentation für ihre Google Play-Dienste aufgeführt.

### <a name="what-happens-to-the-old-componentsnugets"></a>Was geschieht mit den alten Komponenten/NuGet?

Da sie nicht mehr benötigt werden, haben wir die folgenden Komponenten/NuGets deaktiviert/Delisted:

-   Google Play-Dienste (Gingerbread)
-   Google Play-Dienste (JellyBean)
-   Google Play-Dienste (KitKat)

Die vorhandene _Google wiedergeben Dienste (ICS)_ Komponente/Nuget wurde umbenannt in _Google Play-Dienste_ und wird auf dem neuesten Stand zukünftig beibehalten. Alle Projekte, verweisen Sie auf eines der Pakete deaktiviert/Delisted sollten aktualisiert werden, um diese zu verwenden.

Deaktivierten Komponenten sind noch vorhanden und müssen wiederherstellbaren für Projekte, denen diese weiterhin im, verwiesen wird, um zu vermeiden, unterbrochen werden. Auf ähnliche Weise sind die delisted NuGet-Pakete noch vorhanden und können wiederhergestellt werden. Sie werden zukünftig nicht aktualisiert werden.
