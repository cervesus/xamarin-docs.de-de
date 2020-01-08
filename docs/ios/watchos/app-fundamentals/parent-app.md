---
title: Arbeiten mit dem WatchOS übergeordnete Anwendung in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit einer WatchOS übergeordnete Anwendung in Xamarin. Es werden watchos-App-Erweiterungen, IOS-apps, frei gegebener Speicher und mehr erläutert.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 3e11b163d16be9711bf09102e3ab8604d98299d7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487762"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Arbeiten mit dem WatchOS übergeordnete Anwendung in Xamarin

Es gibt verschiedene Möglichkeiten für die Kommunikation zwischen der Watch-app und dem iOS-app, der sie mit gebündelt wird:

- Watch-Apps können auf dem iPhone Code auf der übergeordneten APP [Ausführen](#run-code) .

- Sehen Sie sich Erweiterungen können [freigeben einen Speicherort](#shared-storage) mit der übergeordneten iPhone-app.

- Übergeben Sie mithilfe von Übergabe Daten aus einer Benachrichtigung an die Watch-APP, und senden Sie den Benutzer an einen bestimmten Schnittstellen Controller in der app.

Die übergeordnete-App wird manchmal auch als der Container-App bezeichnet.

## <a name="run-code"></a>Ausführen von Code

Diese beiden Beispiele veranschaulichen, wie `WCSession` verwendet wird, um Code auszuführen und Nachrichten zwischen einer Watch-APP und dem gekoppelten iPhone zu senden:

- [Konnektivität überwachen](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [Simplewatchconnectivity](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>Freigegebener Speicher

Wenn Sie konfigurieren eine [app-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) und iOS 8-Erweiterungen (einschließlich der Watch-Erweiterungen) Daten mit der übergeordneten Anwendung gemeinsam nutzen können.

### <a name="nsuserdefaults"></a>NSUserDefaults

Der folgende Code kann in der Watch-app-Erweiterung und der übergeordneten iPhone-app geschrieben werden, sodass sie einen gemeinsamen Satz von verweisen können `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Dateien

Die iOS-app "und" Überwachen-Erweiterung kann auch Dateien mit einem gemeinsamen Dateipfad freigeben.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Hinweis: Wenn der Pfad ist `null` überprüfen Sie dann die [app-Gruppenkonfiguration](~/ios/watchos/app-fundamentals/app-groups.md) sicherstellen, dass die bereitstellungsprofile richtig konfiguriert wurden und auf dem Entwicklungscomputer heruntergeladen und installiert wurden.

Weitere Informationen finden Sie unter den [App-Gruppen-Funktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) Dokumentation.

## <a name="related-links"></a>Verwandte Themen

- [Apple WKInterfaceController Verweis](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple Freigeben von Daten mit Ihrer App mit](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
