---
title: Xamarin.Essentials Versionsüberwachung
description: Die VersionTracking-Klasse können Sie die Version der Anwendung zu überprüfen und Buildnummern zusammen mit z. B. zusätzliche Informationen anzeigen, als wäre sie das erste Mal von der Anwendung, die jemals gestartet oder für die aktuelle Version zu erhalten, die vorherige Buildinformationen und vieles mehr.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ec9d62589ddfb270d5c8a5321b3bc733fc597e4b
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials Versionsüberwachung

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **VersionTracking** -Klasse können Sie die Version der Anwendung zu überprüfen und Buildnummern zusammen mit z. B. zusätzliche Informationen anzeigen, als wäre sie das erste Mal von der Anwendung nie gestartet oder für die aktuelle Version erhalten Sie die vorherigen Geben Sie Buildinformationen zu und mehr.

## <a name="using-version-tracking"></a>Mithilfe der Version nachverfolgung

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Zum ersten Mal Sie verwenden die **VersionTracking** Klasse Nachverfolgen von der aktuellen Version wird gestartet. Rufen Sie `Track` frühen nur in der Anwendung jedes Mal geladen wird, um sicherzustellen, dass die aktuelle Versionsinformationen nachverfolgt:

```csharp
VersionTracking.Track();
```

Nach der Erstkonfiguration `Track` heißt Versionsinformationen gelesen werden kann:

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

Alle Versionsinformationen wird gespeichert, mit der [Voreinstellungen](preferences.md) -API in Xamarin.Essentials und wird mit einem Dateinamen gespeichert **[YOUR-APP-Paket-ID] .xamarinessentials**.

Deinstallieren der Anwendung führt dazu, dass die _LocalSettings_, und alle Version Nachverfolgungsinformationen entfernt werden soll.

## <a name="api"></a>API

- [Version Tracking-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/VersionTracking)
- [Version Arbeitselementverfolgungs-API-Dokumentation](xref:Xamarin.Essentials.VersionTracking)
