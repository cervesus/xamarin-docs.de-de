---
title: Arbeiten mit WatchOS-App-Gruppen in Xamarin
description: Dieses Dokument beschreibt die app-Gruppen und deren Verwendung in einer WatchOS-Anwendung. Es wird erläutert, wie eine app-Gruppe, die Bereitstellung von Anforderungen und "Entitlements.plist" Überlegungen zur Bereitstellung zu konfigurieren.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 78f6c03f73f0e4d8a74f826dd7bc25bbe325d545
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61411703"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Arbeiten mit WatchOS-App-Gruppen in Xamarin


Durch eine App-Gruppe können unterschiedliche Anwendungen (oder eine Anwendung und ihre Erweiterungen) auf einen freigegebenen Dateispeicherort zugreifen. App-Gruppen können für folgende Daten verwendet werden:

- Apple Watch [Einstellungen](~/ios/watchos/app-fundamentals/settings.md).
- Freigegebene [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Freigegebene [Dateien](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Konfigurieren Sie eine App-Gruppe

Der freigegebene Speicherort erfolgt über eine [App-Gruppe](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), die konfiguriert wird, der **Zertifikate, Bezeichner & Profile** im Abschnitt [iOS Developer Center](https://developer.apple.com/devcenter/ios/). Dieser Wert muss außerdem in jedem Projekt auf die verwiesen werden **"Entitlements.plist"**.

### <a name="provisioning"></a>Bereitstellung

Die app-Gruppe müssen Bezeichner, der in der Regel Ihre Paket-ID ist mit einem `group.` Präfix. Wir könnten z. B. Verwenden der Paket-ID `com.xamarin.WatchSettings` und die app-Gruppe `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Verwenden Sie die Bündel-ID com.xamarin.WatchSettings und group.com.xamarin.WatchSettings der app-Gruppe")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

Und konfigurieren das Bereitstellungsprofil **App-Gruppen aktivieren** in die **"Entitlements.plist"** , und geben Sie die ID, die Sie ausgewählt haben:

[![](app-groups-images/entitlements-sml.png "Konfigurieren Sie die plist-Datei, und geben Sie die ID")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Bereitstellung

Stellen Sie sicher, Sie konfigurieren, dass die App-Gruppe, die in Ihrem [Bereitstellung](~/ios/watchos/deploy-test/index.md#App_Groups) bereitstellen.


Weitere Informationen finden Sie unter den [App-Gruppen-Funktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.


## <a name="related-links"></a>Verwandte Links

- [Apple Freigeben von Daten mit Ihrer App mit](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Apple App-Gruppen-doc](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
