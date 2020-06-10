---
title: Arbeiten mit der übergeordneten watchos-Anwendung in xamarin
description: In diesem Dokument wird beschrieben, wie Sie eine übergeordnete watchos-Anwendung in xamarin arbeiten. Es werden watchos-App-Erweiterungen, IOS-apps, frei gegebener Speicher und mehr erläutert.
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 49f2bdf63c286464073308cd1f17239692aa2395
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567332"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Arbeiten mit der übergeordneten watchos-Anwendung in xamarin

Es gibt verschiedene Möglichkeiten für die Kommunikation zwischen der Watch-APP und der IOS-APP, mit der Sie gebündelt ist:

- Watch-Apps können auf dem iPhone Code auf der übergeordneten APP [Ausführen](#run-code) .

- Überwachungs Erweiterungen können [einen Speicherort](#shared-storage) für die übergeordnete iPhone-App freigeben.

- Übergeben Sie mithilfe von Übergabe Daten aus einer Benachrichtigung an die Watch-APP, und senden Sie den Benutzer an einen bestimmten Schnittstellen Controller in der app.

Die übergeordnete APP wird manchmal auch als Container-App bezeichnet.

## <a name="run-code"></a>Ausführen von Code

Diese beiden Beispiele veranschaulichen die Verwendung von `WCSession` zum Ausführen von Code und Senden von Nachrichten zwischen einer Watch-APP und dem gekoppelten iPhone:

- [Konnektivität überwachen](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [Simplewatchconnectivity](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>Freigegebener Speicher

Wenn Sie eine [App-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) konfigurieren, können IOS 8-Erweiterungen (einschließlich der Überwachungs Erweiterungen) Daten für die übergeordnete App freigeben.

### <a name="nsuserdefaults"></a>NSUserDefaults

Der folgende Code kann in der Watch-App-Erweiterung und der übergeordneten iPhone-App geschrieben werden, sodass Sie auf einen gemeinsamen Satz von verweisen können `NSUserDefaults` :

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

<a name="files"></a>

### <a name="files"></a>Files

Die IOS-APP und die Watch-Erweiterung können auch Dateien mit einem gemeinsamen Dateipfad freigeben.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Hinweis: Wenn der Pfad lautet `null` , überprüfen Sie die [Konfiguration der APP-Gruppe](~/ios/watchos/app-fundamentals/app-groups.md) , um sicherzustellen, dass die Bereitstellungs profile ordnungsgemäß konfiguriert wurden und auf dem Entwicklungs Computer heruntergeladen bzw. installiert wurden.

Weitere Informationen finden Sie in der Dokumentation zu [App-Gruppenfunktionen](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) .

## <a name="related-links"></a>Verwandte Links

- [Wkinterfakecontroller-Referenz für Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple-Freigabe von Daten für Ihre enthaltende App](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
