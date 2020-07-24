---
title: Arbeiten mit watchos-App-Gruppen in xamarin
description: In diesem Dokument werden App-Gruppen und deren Verwendung in einer watchos-Anwendung beschrieben. Darin wird erläutert, wie Sie eine APP-Gruppe, Bereitstellungs Anforderungen, Berechtigungen. plist-Überlegungen und die Bereitstellung konfigurieren.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: ee3897f14c460149e840fcea8b3fb533beeab935
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931624"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Arbeiten mit watchos-App-Gruppen in xamarin

Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- Apple Watch [Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- Freigegebene [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Freigegebene [Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Konfigurieren einer APP-Gruppe

Der freigegebene Speicherort wird mit einer [App-Gruppe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)konfiguriert, die im Abschnitt Zertifikate, Bezeichner **& profile** im [IOS dev Center](https://developer.apple.com/devcenter/ios/)konfiguriert ist. Auf diesen Wert muss in den **Berechtigungen. plist**jedes Projekts verwiesen werden.

### <a name="provisioning"></a>Bereitstellung

Die APP-Gruppe verfügt über einen Bezeichner, der in der Regel die Bündel-ID mit einem `group.` Präfix ist. Beispielsweise könnten wir die Bündel `com.xamarin.WatchSettings` -ID und die APP-Gruppe verwenden `group.com.xamarin.WatchSettings` .

[![Verwenden Sie die Bündel-ID com. xamarin. watchsettings und die APP-Gruppen Gruppe. com. xamarin. watchsettings.](app-groups-images/app-group-sml.png)](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Aktivieren Sie auch das Bereitstellungs Profil, **Aktivieren Sie App-Gruppen** in der Liste " **Berechtigungen. plist** ", und geben Sie die ID ein, die Sie ausgewählt haben:

[![Plist konfigurieren und die ID eingeben](app-groups-images/entitlements-sml.png)](app-groups-images/entitlements.png#lightbox)

### <a name="deployment"></a>Bereitstellung

Stellen Sie sicher, dass Sie die APP-Gruppe in der [Bereitstellungs Bereitstellung](~/ios/watchos/deploy-test/index.md#App_Groups) richtig konfigurieren.

Weitere Informationen finden Sie in der Dokumentation zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="related-links"></a>Verwandte Links

- [Apple-Freigabe von Daten für Ihre enthaltende App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [App-Gruppen Dokument von Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
