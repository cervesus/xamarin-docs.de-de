---
title: 'Xamarin.Essentials: Einheitenkonverter'
description: Die Klasse „UnitConverters“ in Xamarin.Essentials stellt mehrere Einheitenkonverter bereit, die Entwickler mit Xamarin.Essentials nutzen können.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: c07e0c7d9645c22f0d70c75fd7d8dffdec8cde04
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77545033"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials: Einheitenkonverter

Die Klasse **UnitConverters** in Xamarin.Essentials stellt mehrere Einheitenkonverter bereit, die Entwickler mit Xamarin.Essentials nutzen können.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Verwenden von Einheitenkonvertern

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Alle Einheitenkonverter stehen über die statische Klasse `UnitConverters` in Xamarin.Essentials zur Verfügung. Beispielsweise können Sie mühelos Fahrenheit in Celsius konvertieren.

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

Es folgt eine Liste der verfügbaren Konvertierungen:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [Unit Converters source code (Quellcode für den Einheitenkonverter)](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Unit Converters API documentation (Dokumentation der Einheitenkonverter-API)](xref:Xamarin.Essentials.UnitConverters)
