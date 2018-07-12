---
title: 'Xamarin.Essentials: Einstellungen'
description: Dieses Dokument beschreibt die Einstellungen-Klasse in Xamarin.Essentials, die Anwendungsvoreinstellungen in einem Schlüssel-Wert-Speicher speichert. Er erläutert, wie die Klasse und die Arten von Daten, die gespeichert werden können.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca6d4f1ec60a80b483c79dd75267144e67d80c0b
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831763"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: Einstellungen

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Voreinstellungen** Klasse ermöglicht es Ihnen, Voreinstellungen für Anwendung in einem Schlüssel-Wert-Speicher speichert.

## <a name="using-preferences"></a>Einstellungen

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Speichern Sie einen Wert für eine bestimmte _Schlüssel_ in den Voreinstellungen:

```csharp
Preferences.Set("my_key", "my_value");
```

Zum Abrufen eines Werts aus den Einstellungen oder einen Standardwert, wenn nicht festgelegt:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

So entfernen Sie die _Schlüssel_ aus den Einstellungen:

```csharp
Preferences.Remove("my_key");
```

So entfernen Sie alle Einstellungen:

```csharp
Preferences.Clear();
```

Neben diesen Methoden akzeptieren jeweils in einem optionalen `sharedName` , die zum Erstellen zusätzlicher Container für die Einstellung verwendet werden kann. Lesen Sie die folgenden Besonderheiten der Plattform-Implementierung.

## <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden in unterstützt **Voreinstellungen**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Details zur Implementierung

Werte von `DateTime` befinden sich in einem 64-Bit-Binärdatei (lange ganze Zahl)-Format mit zwei Methoden definiert, durch die `DateTime` Klasse: die [ `ToBinary` ](xref:System.DateTime.ToBinary) Methode dient zum Codieren der `DateTime` Wert und die [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) -Methode decodiert den Wert. Finden Sie unter die Dokumentation zu diesen Methoden für die Anpassungen, die zu decodierte vorgenommen werden, wenn Werte eine `DateTime` wird gespeichert, nicht auf einen Wert (Coordinated Universal Time, UTC).

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="androidtabandroid"></a>[Android](#tab/android)

Alle Daten werden gespeichert, in [freigegebene Einstellungen](https://developer.android.com/training/data-storage/shared-preferences.html). Wenn kein `sharedName` angegeben ist, die standardmäßig freigegebenen-Einstellungen werden verwendet, andernfalls der Name wird zum Abrufen einer **private** freigegebene Einstellungen mit dem angegebenen Namen.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) wird verwendet, um die Werte auf iOS-Geräten zu speichern. Wenn kein `sharedName` angegeben ist die `StandardUserDefaults` werden verwendet, andernfalls der Name dient zum Erstellen eines neuen `NSUserDefaults` mit dem angegebenen Namen, die zum die `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

["Applicationdatacontainer"](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) wird verwendet, um die Werte auf dem Gerät zu speichern. Wenn kein `sharedName` entspricht der `LocalSettings` werden verwendet, andernfalls der Name wird verwendet, um einen neuen Container innerhalb des erstellen `LocalSettings`.

--------------

## <a name="limitations"></a>Einschränkungen

Wenn Sie eine Zeichenfolge zu speichern, dient diese API, um kleine Mengen an Text zu speichern.  Leistung ist möglicherweise subpar, wenn Sie versuchen, die sie verwenden, um große Mengen an Text zu speichern.

## <a name="api"></a>API

- [Einstellungen von Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Einstellungen-API-Dokumentation](xref:Xamarin.Essentials.Preferences)
