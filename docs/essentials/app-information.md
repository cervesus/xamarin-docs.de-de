---
title: 'Xamarin.Essentials: App-Informationen'
description: Dieses Dokument beschreibt die AppInfo-Klasse in Xamarin.Essentials, die Informationen zu Ihrer Anwendung bereitstellt. Beispielsweise macht es der app-Name und Version.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782104"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: App-Informationen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **AppInfo** Klasse enthält Informationen zu Ihrer Anwendung.

## <a name="using-appinfo"></a>Mithilfe der AppInfo

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

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

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)
