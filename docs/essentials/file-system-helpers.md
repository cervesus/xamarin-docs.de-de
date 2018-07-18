---
title: 'Xamarin.Essentials: Datei System-Hilfsprogramme'
description: Die dateisystemklasse in Xamarin.Essentials enthält eine Reihe von Hilfsfunktionen zum Suchen, die Anwendung den Cache und die Datenverzeichnisse und öffnen Dateien in das app-Paket.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815617"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Datei System-Hilfsprogramme

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **FileSystem** Klasse enthält eine Reihe von Hilfsfunktionen zum Suchen von der Anwendung Cache und Datenverzeichnisse und Öffnen von Dateien in das app-Paket.

## <a name="using-file-system-helpers"></a>Verwenden von Datei-System-Hilfsprogrammen

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Verzeichnis zum Speichern der Anwendung abzurufende **Zwischenspeichern von Daten**. Zwischenspeichern von Daten können für alle Daten verwendet werden, die länger als temporäre Daten beibehalten werden muss, jedoch sollte sich nicht auf die Daten, die erforderlich ist, um ordnungsgemäß zu arbeiten.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Zum Abrufen von der obersten Ebene im Verzeichnis der Anwendung für alle Dateien, die Benutzerdatendateien nicht sind. Diese Dateien werden mit dem Betriebssystem synchronisieren Framework gesichert. Finden Sie unter folgenden Implementierung-Plattformeigenschaften.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Zum Öffnen einer Datei, die in das Anwendungspaket im Paket enthalten ist:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – gibt die [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) des aktuellen Kontexts.
- **AppDataDirectory** – gibt die [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) den aktuellen Kontext und sind mit gesichert [automatische Sicherung](https://developer.android.com/guide/topics/data/autobackup.html) ab, auf die API-23 und höher.

Fügen Sie eine beliebige Datei in die **Assets** Ordner im Android Projekt, und markieren Sie die Buildaktion als **AndroidAsset** , sie mit `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – gibt die [Library/Caches](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) Verzeichnis.
- **AppDataDirectory** – gibt die [Bibliothek](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) Verzeichnis, das von iTunes- und iCloud werden gesichert.

Fügen Sie eine beliebige Datei in die **Ressourcen** Ordner in der iOS-Projekt, und markieren Sie die Buildaktion als **BundledResource** , sie mit `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – gibt die [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) Directory...
- **AppDataDirectory** – gibt die [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) Verzeichnis, das in der Cloud wurden gesichert.

Fügen Sie eine beliebige Datei in das Stammverzeichnis in der UWP-Projekt hinzu, und markieren Sie die Buildaktion als **Content** , sie mit `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Quellcode für Datei-System-Hilfsprogramme](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Datei-System-API-Dokumentation](xref:Xamarin.Essentials.FileSystem)
