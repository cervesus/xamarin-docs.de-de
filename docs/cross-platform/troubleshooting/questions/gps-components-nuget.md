---
title: Vereinheitlichen von Google Play-Dienstkomponenten und NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 8a7fd77a3f6460f0edbd76f8a4ccf45b32b3ed87
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284991"
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Vereinheitlichen von Google Play-Dienstkomponenten und NuGet

## <a name="history"></a>Versionsgeschichte

Es wurden mehrere Google Play Services Komponenten und nuget-Pakete verwendet:

- Google Play Services (Froyo)
- Google Play Services (Lebkuchen)
- Google Play Services (ICS)
- Google Play Services (Jellybean)
- Google Play Services (KitKat)

Google gibt eigentlich nur zwei JAR-Dateien für Google Play Services aus:

- `google-play-services-froyo.jar`
- `google-play-services.jar`

Die Diskrepanz ist aufgetreten, da unsere Tools nicht `aapt.exe` richtig feststellen, was die maximale Ressourcen-API-Ebene für eine bestimmte App verwendet werden sollte. Dies bedeutete, dass wir Kompilierungsfehler erhalten haben, wenn wir versuchten, die Google Play Services (KitKat)-Bindung auf einer niedrigeren API-Ebene wie Lebkuchen zu verwenden.

## <a name="unifying-google-play-services"></a>Vereinheitlichung Google Play Services

In neueren Versionen von xamarin. Android haben wir nun `aapt.exe` festzustellen, welche maximale Ressourcen Version verwendet werden soll, sodass dieses Problem für uns nicht mehr auftritt.

Dies bedeutet, dass es keinen wirklichen Grund gibt, separate Pakete für Lebkuchen/ICS/Jellybean/KitKat zu verwenden (es wird jedoch immer noch eine separate Bindung für Froyo benötigt, da es sich um eine andere JAR-Datei handelt).

Um Entwicklern die Arbeit zu erleichtern, haben wir nun die Komponenten und nuget-Pakete in zwei vereinheitlicht:

- Google Play Services (Froyo) (Bindungen `google-play-services-froyo.jar`)
- Google Play Services (Bindungen `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Welches sollte verwendet werden?

In nahezu jedem Fall sollten Google Play Services verwendet werden. Der einzige Grund für die Verwendung des Pakets (Froyo) ist, dass Sie "Froyo" aktiv als Ziel verwenden. Der einzige Grund, aus dem diese separate JAR-Datei von Google besteht, liegt darin, dass Froyo auf einem so kleinen Prozentsatz von Geräten liegt, dass Sie sich selbst nicht mehr unterstützen müssen, sodass diese JAR-Datei eine fixierte, nicht unterstützte Momentaufnahme von Google Play Services ist.

### <a name="note-about-gingerbread"></a>Hinweis zu Lebkuchen

Lebkuchen hat standardmäßig keine fragmentunterstützung, und aus diesem Grund können einige Klassen in der Bindung in einer App zur Laufzeit auf einem Lebkuchen Gerät nicht verwendet werden. Klassen wie `MapFragment` können nicht für Lebkuchen verwendet werden, und stattdessen `SupportMapFragment`sollte Ihre Support Variante verwendet werden. Der Entwickler muss wissen, welche zu verwenden sind. Diese Inkompatibilität wird in der Google Play Services-Dokumentation von Google vermerkt.

### <a name="what-happens-to-the-old-componentsnugets"></a>Was geschieht mit den alten Komponenten/nuget?

Da Sie nicht mehr benötigt werden, haben wir die folgenden Komponenten/nugets deaktiviert/getrennt:

- Google Play Services (Lebkuchen)
- Google Play Services (Jellybean)
- Google Play Services (KitKat)

Die vorhandene _Google Play Services (ICS)_ -Komponente/nuget wurde in _Google Play Services_ umbenannt und wird in Zukunft auf dem neuesten Stand gehalten. Alle Projekte, die auf eines der deaktivierten/Delisted-Pakete verweisen, sollten aktualisiert werden, um dieses zu verwenden.

Die deaktivierten Komponenten sind noch vorhanden und sollten für Projekte wiederherstellbar sein, in denen Sie weiterhin referenziert werden, um eine Unterbrechung zu vermeiden. Auf ähnliche Weise sind auch die Delisted nuget-Pakete vorhanden und können wieder hergestellt werden. Sie werden nicht weiter aktualisiert.
