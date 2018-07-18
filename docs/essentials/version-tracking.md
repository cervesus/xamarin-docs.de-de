---
title: 'Xamarin.Essentials: Die Version nachverfolgung'
description: Die VersionTracking-Klasse in Xamarin.Essentials können Sie die Anwendungen-Version zu überprüfen und Buildnummern zusammen mit solchen zusätzliche Informationen angezeigt, als ob es sich um die erste ist Zeit die Anwendung, die jemals gestartet oder für die aktuelle Version erhalten Sie den vorherigen build Informationen und mehr.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c092d6767045f0af956c5dab74801077dadb51f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815647"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Die Version nachverfolgung

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **VersionTracking** Klasse können Sie die Anwendungen-Version zu überprüfen und Buildnummern zusammen mit solchen zusätzliche Informationen angezeigt, als ob es sich um die erste ist Zeit die Anwendung, die jemals gestartet oder für die aktuelle Version erhalten Sie die vorherige Erstellen Sie die Informationen und mehr.

## <a name="using-version-tracking"></a>Mithilfe der nachverfolgung der Version

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Bei der ersten Verwendung der **VersionTracking** Klasse, die nachverfolgung der aktuellen Version wird gestartet. Rufen Sie `Track` früher nur in Ihrer Anwendung jedes Mal, es geladen wird, um sicherzustellen, dass die Informationen zur aktuellen Version wird nachverfolgt:

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

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

Alle Versionsinformationen wird gespeichert, mit der [Voreinstellungen](preferences.md) -API in Xamarin.Essentials und wird mit einem Dateinamen gespeichert **[YOUR-APP-Paket-ID] .xamarinessentials**.

Deinstallieren der Anwendung führt dazu, dass die _LocalSettings_, und alle Versionen, die zu entfernenden Überwachungsinformationen.

## <a name="api"></a>API

- [Version-nachverfolgung-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Version Arbeitselementverfolgungs-API-Dokumentation](xref:Xamarin.Essentials.VersionTracking)
