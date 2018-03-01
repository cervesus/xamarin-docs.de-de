---
title: Arbeiten mit App-Gruppen
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 27ce6c48c5bca69605773eb5ef5637201b9ce6c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-app-groups"></a>Arbeiten mit App-Gruppen


Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- Apple Watch [Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- Freigegebene [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Freigegebene [Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Konfigurieren Sie ein App-Gruppe

Mit der freigegebene Speicherort konfiguriert ein [App-Gruppe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), die konfiguriert ist, der **Zertifikate "," Bezeichner "und" Profile** Abschnitt [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Dieser Wert muss auch in jedem Projekt verwiesen werden **Entitlements.plist**.

### <a name="provisioning"></a>Bereitstellung

Die app-Gruppe hat einen Bezeichner zeigen, die in der Regel die Paket-ID ist mit einem `group.` Präfix. Beispielsweise können wir die Paket-ID `com.xamarin.WatchSettings` und die app-Gruppe `group.com.xamarin.WatchSettings`.

[ ![](app-groups-images/app-group-sml.png "Verwenden Sie die Paket-ID com.xamarin.WatchSettings und group.com.xamarin.WatchSettings der app-Gruppe")](app-groups-images/app-group.png)

### <a name="entitlementsplist"></a>Entitlements.plist

Sowie die provisioning-Profil konfigurieren **App-Gruppen aktivieren** in der **Entitlements.plist** und geben Sie die ID, die Sie ausgewählt haben:

[ ![](app-groups-images/entitlements-sml.png "Konfigurieren Sie die Plist, und geben Sie die ID")](app-groups-images/entitlements.png)


### <a name="deployment"></a>Bereitstellung

Stellen Sie sicher, konfigurieren Sie die App-Gruppe, die in Ihrem [Bereitstellung](~/ios/watchos/deploy-test/index.md#app-groups) bereitstellen.


Weitere Informationen finden Sie unter der [App Gruppenverwaltungsfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.


## <a name="related-links"></a>Verwandte Links

- [Apple Freigeben von Daten mit der enthaltenden App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple App-Gruppe doc](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
