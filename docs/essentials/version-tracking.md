---
title: "Xamarin.Essentials: Versionsnachverfolgung'' description: "Mit der „VersionTracking“-Klasse in Xamarin.Essentials können Sie die Anwendungsversion und Buildnummern überprüfen und weitere Informationen anzeigen. So können Sie beispielsweise feststellen, ob es sich um den erstmaligen Start der Anwendung oder der aktuellen Version handelt oder die Nummer des vorherigen Builds abrufen."
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3 author: jamesmontemagno ms.author: jamont ms.date: 05/28/2019 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Versionsnachverfolgung

Mit der Klasse **VersionTracking** können Sie die Anwendungsversion und Buildnummern überprüfen und weitere Informationen anzeigen. So können Sie z. B. feststellen, ob es sich um den allerersten Start der Anwendung oder der aktuellen Version handelt, die Nummer des vorherigen Builds abrufen usw.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-version-tracking"></a>Verwenden der Versionsnachverfolgung

Fügen Sie in Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Bei der ersten Verwendung der Klasse **VersionTracking** wird die Nachverfolgung der aktuellen Version gestartet. Sie müssen früh und nur in Ihrer Anwendung bei jedem Laden `Track` aufrufen, um sicherzustellen, dass die Informationen zur aktuellen Version nachverfolgt werden:

```csharp
VersionTracking.Track();
```

Nach dem ersten Aufrufen von `Track` können Versionsinformationen gelesen werden:

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

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

Alle Versionsinformationen werden mithilfe der [Preferences](preferences.md)-API in Xamarin.Essentials mit einem Dateinamen in Form von **[IHRE-APP-PAKET-ID].xamarinessentials.versiontracking** gespeichert und unterliegen der gleichen Datenpersistenz, wie sie in der Dokumentation zu den [Einstellungen](preferences.md#persistence) beschrieben ist.

## <a name="api"></a>API

- [VersionTracking-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [VersionTracking-API-Dokumentation](xref:Xamarin.Essentials.VersionTracking)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Version-Tracking-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
