---
title: 'Xamarin.Essentials: Einheitenkonverter'
description: Die Klasse „UnitConverters“ in Xamarin.Essentials stellt mehrere Einheitenkonverter bereit, die Entwickler mit Xamarin.Essentials nutzen können.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: 866842cbed9f97dc957e3631c037fa8d27d20076
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149775"
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

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
