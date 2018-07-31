---
title: Vereinheitlichen von Google Play Services-Dienstkomponenten und NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 3f5c5f75ae1c7a44537afa59ff4a15d54b1df50b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351482"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Vereinheitlichen von Google Play Services-Dienstkomponenten und NuGet

## <a name="history"></a>Versionsgeschichte

Es werden mehrere Google Play-Dienstkomponenten und NuGet-Pakete verwendet:

-   Google Play-Dienste (Froyo)
-   Google Play-Dienste (Gingerbread)
-   Google Play-Dienste (gemeinsam genutzter Internetverbindung)
-   Google Play-Dienste (JellyBean)
-   Google Play-Dienste (KitKat)

Google tatsächlich nur umfasst zwei JAR-Dateien für Google Play-Dienste:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

Die Abweichung vorhanden waren, da unsere Tools nicht ordnungsgemäß mitteilen würden `aapt.exe` was die maximale Ressource-API-Ebene für eine bestimmte app verwendet werden. Dies bedeutete, dass wir Kompilierungsfehler erhalten, wenn wir versuchen, die Google Play-Dienste (KitKat)-Bindung auf einer niedrigeren API-Ebene wie Gingerbread verwenden.

## <a name="unifying-google-play-services"></a>Vereinheitlichen von Google Play-Dienste

In neueren Versionen von Xamarin.Android können wir jetzt sagen `aapt.exe` welche maximale Version zu verwenden, damit dieses Problem für uns verschwindet,.

Dies bedeutet, dass Sie keinen triftigen Grund haben separate Pakete für Gingerbread/ICS/JellyBean/KitKat (jedoch wir weiterhin benötigen eine separate Bindung für Froyo, da es eine andere JAR-Datei vollständig ist) besteht.

Um die Dinge für Entwickler zu vereinfachen, haben wir jetzt unsere Dienstkomponenten und NuGet unified Pakete in zwei:

-   Google Play-Dienste (Froyo) (bindet `google-play-services-froyo.jar`)
-   Google Play-Dienste (bindet `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Welche sollte verwendet werden?

In fast allen Fällen sollte die Google Play-Dienste verwendet werden. Der einzige Grund für das Paket (Froyo) verwenden, ist, wenn Sie aktiv Froyo verwenden möchten. Ist der einzige Grund, dass diese separate JAR-Datei von Google vorhanden ist, da Froyo mit einem kleinen Prozentsatz der Geräte ist, die sie selbst haben sich entschieden, beenden, daher ist diese JAR-Datei eine fixiert, nicht unterstützte Momentaufnahme von Google Play Services unterstützen.

### <a name="note-about-gingerbread"></a>Hinweis zur Gingerbread

Gingerbread besitzt keine Fragment standardmäßig unterstützt, und aus diesem Grund einige der Klassen in der Bindung nicht verwendet werden in einer app zur Laufzeit auf einem Gerät Gingerbread. Klassen wie `MapFragment` auf Gingerbread nicht funktionsfähig, und stattdessen ihre Support-Variante verwendet werden `SupportMapFragment`. Es liegt an der Entwickler wissen, was Sie verwenden. Diese Inkompatibilität wird von Google in der Dokumentation von Google Play-Dienste aufgeführt.

### <a name="what-happens-to-the-old-componentsnugets"></a>Was geschieht mit den alten Komponenten/NuGet?

Da sie nicht mehr benötigt werden, haben wir die folgenden Komponenten/NuGet-Pakete deaktiviert/Delisted:

-   Google Play-Dienste (Gingerbread)
-   Google Play-Dienste (JellyBean)
-   Google Play-Dienste (KitKat)

Die vorhandene _(Google Play Services, ICS)_ Komponente/Nuget wurde umbenannt in _Google Play Services_ und bleiben auf dem neuesten Stand in Zukunft. Alle Projekte verweisen auf eines der Pakete deaktiviert/Delisted sollte aktualisiert werden, um diese zu verwenden.

Die deaktivierten Komponenten sollte weiterhin vorhanden und für Projekte, denen sie immer noch, verwiesen werden, um zu vermeiden, dass sie wiederherstellbare. Die delisted NuGet-Pakete wird auf ähnliche Weise weiterhin vorhanden, und der wiederhergestellt werden können. Sie werden in Zukunft nicht aktualisiert werden.
