---
title: Informationen zur App-Xamarin.Essentials
description: Die AppInfo-Klasse stellt Informationen zu Ihrer Anwendung bereit.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5b235fdeb666c3f8f2c1b52a9691c931724ce2b8
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-app-information"></a>Informationen zur App-Xamarin.Essentials

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

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)
