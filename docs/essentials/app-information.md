---
title: 'Xamarin.Essentials: App-Informationen'
description: Dieses Dokument beschreibt die AppInfo-Klasse in Xamarin.Essentials, die Informationen zu Ihrer Anwendung bereitstellt. Beispielsweise macht es app-Name und Version.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831506"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: App-Informationen

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **AppInfo** Klasse stellt Informationen zu Ihrer Anwendung bereit.

## <a name="using-appinfo"></a>Mithilfe der AppInfo

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Abrufen von Anwendungsinformationen:

Die folgende Informationen werden über die API verfügbar gemacht:

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

Die **AppInfo** -Klasse kann auch eine Seite mit Einstellungen, die vom Betriebssystem für die Anwendung verwaltet anzeigen:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Diese Seite "Einstellungen" ermöglicht den Benutzer Anwendungsberechtigungen ändern und andere plattformspezifische Aufgaben.

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)
