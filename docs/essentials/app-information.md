---
title: 'Xamarin.Essentials: App-Informationen'
description: In diesem Dokument wird die Klasse „AppInfo“ in Xamarin.Essentials beschrieben, die Informationen zu Ihrer Anwendung bereitstellt. Sie macht beispielsweise den Namen und die Version der App verfügbar.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 00419fb746609464b49be343938905614c59ab29
ms.sourcegitcommit: 704d4cfd418c17b0e85a20c33a16d2419db0be71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51691762"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: App-Informationen

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **AppInfo** stellt Informationen zu Ihrer Anwendung bereit.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>Verwenden von App-Informationen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Abrufen von App-Informationen:

Die folgenden Informationen werden über die API verfügbar gemacht:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>Anzeigen von Anwendungseinstellungen

Die Klasse **AppInfo** kann auch eine Seite mit den Einstellungen anzeigen, die vom Betriebssystem für die Anwendung verwaltet werden:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Diese Seite mit den Einstellungen ermöglicht dem Benutzer, Anwendungsberechtigungen zu ändern und andere plattformspezifische Aufgaben auszuführen.

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)
