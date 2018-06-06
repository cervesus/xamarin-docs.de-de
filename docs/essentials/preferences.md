---
title: 'Xamarin.Essentials: Voreinstellungen'
description: Dieses Dokument beschreibt die Voreinstellungen-Klasse in Xamarin.Essentials, die Anwendungseinstellungen in einem Schlüssel-Wert-Speicher speichert. Er erläutert, wie die Klasse und die Arten von Daten, die gespeichert werden können.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e453c04a953e60be2508670723d175bde3dc7c42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782843"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: Voreinstellungen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Voreinstellungen** Klasse zum Speichern von Anwendungseinstellungen in einem Schlüssel-Wert-Speicher unterstützt.

## <a name="using-secure-storage"></a>Verwenden die sichere Speicherung

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Speichern Sie einen Wert für einen bestimmten _Schlüssel_ in den Voreinstellungen:

```csharp
Preferences.Set("my_key", "my_value");
```

Zum Abrufen eines Werts aus Voreinstellungen oder einen Standardwert, sofern nichts anderes festgelegt:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

So entfernen Sie die _Schlüssel_ aus Systemeinstellungen:

```csharp
Preferences.Remove("my_key");
```

So entfernen Sie alle Einstellungen:

```csharp
Preferences.Clear();
```

Neben diesen Methoden akzeptieren jeweils in einem optionalen `sharedName` , die zum Erstellen von Anforderungen entsprechend zusätzlicher Containers verwendet werden kann. Lesen Sie die Einzelheiten zur Plattform Implementierung.

## <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden in unterstützt **Voreinstellungen**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

# <a name="androidtabandroid"></a>[Android](#tab/android)

Alle Daten, gespeichert in [freigegebenen Voreinstellungen](https://developer.android.com/training/data-storage/shared-preferences.html). Wenn kein `sharedName` angegeben ist, die freigegebene Standardeinstellungen verwendet werden, andernfalls der Name zum Abrufen einer **private** freigegebenen Voreinstellungen mit dem angegebenen Namen.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) wird verwendet, um die Werte auf iOS-Geräten zu speichern. Wenn kein `sharedName` angegeben ist die `StandardUserDefaults` sind verwendet, andernfalls der Name dient zum Erstellen eines neuen `NSUserDefaults` mit dem angegebenen Namen verwendet, die für die `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) wird verwendet, um die Werte auf dem Gerät zu speichern. Wenn kein `sharedName` entspricht der `LocalSettings` werden verwendet, andernfalls der Name wird verwendet, um einen neuen Container innerhalb eines erstellen `LocalSettings`.

--------------

## <a name="limitations"></a>Einschränkungen

Wenn Sie eine Zeichenfolge zu speichern, ist dieser API vorgesehen, um kleine Mengen von Text zu speichern.  Leistung ist möglicherweise subpar, wenn Sie versuchen, diese zum Speichern großer Textmengen nutzen.

## <a name="api"></a>API

- [Voreinstellungen-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Voreinstellungen für API-Dokumentation](xref:Xamarin.Essentials.Preferences)
