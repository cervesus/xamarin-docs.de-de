---
title: "Xamarin.Essentials: AppInfo" description: "In diesem Dokument wird die AppInfo-Klasse in Xamarin.Essentials beschrieben, die Informationen zu Ihrer Anwendung bereitstellt. Sie enthält beispielsweise den Namen und die Version der App."
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA author: jamesmontemagno ms.author: jamont ms.date: 01/29/2019 ms.custom: video no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: App-Informationen

Die Klasse **AppInfo** stellt Informationen zu Ihrer Anwendung bereit.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-appinfo"></a>Verwenden von App-Informationen

Fügen Sie Ihrem Projekt einen Xamarin.Essentials-Verweis hinzu:

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
AppInfo.ShowSettingsUI();
```

Diese Seite mit den Einstellungen ermöglicht dem Benutzer, Anwendungsberechtigungen zu ändern und andere plattformspezifische Aufgaben auszuführen.

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="android"></a>[Android](#tab/android)

Für die folgenden Felder werden App-Informationen aus der Datei `AndroidManifest.xml` abgerufen:

- **Build**: `android:versionCode` im `manifest`-Knoten
- **Name**: `android:label` im `application`-Knoten
- **PackageName**: `package` im `manifest`-Knoten
- **VersionString**: `android:versionName` im `application`-Knoten

# <a name="ios"></a>[iOS](#tab/ios)

Für die folgenden Felder werden App-Informationen aus der Datei `Info.plist` abgerufen:

- **Build**: `CFBundleVersion`
- **Name**: wenn festlegt `CFBundleDisplayName`, andernfalls `CFBundleName`
- **PackageName**: `CFBundleIdentifier`
- **VersionString**: `CFBundleShortVersionString`

# <a name="uwp"></a>[UWP](#tab/uwp)

Für die folgenden Felder werden App-Informationen aus der Datei `Package.appxmanifest` abgerufen:

- **Build**: Verwendet den `Build` der `Version` im `Identity`-Knoten
- **Name**: `DisplayName` im `Properties`-Knoten
- **PackageName**: `Name` im `Identity`-Knoten
- **VersionString**: `Version` im `Identity`-Knoten

--------------

## <a name="api"></a>API

- [AppInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo-API-Dokumentation](xref:Xamarin.Essentials.AppInfo)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/App-Information-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
