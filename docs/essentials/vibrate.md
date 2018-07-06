---
title: 'Xamarin.Essentials: Ihr Telefon Vibrieren'
description: Dieses Dokument beschreibt die Vibration-Klasse in Xamarin.Essentials, was Ihnen das Starten und beenden die Vibrieren-Funktionalität für eine gewünschte Zeitspanne.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca21f43631c261cd384f9049f30f0fa29e2ca44e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855168"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Ihr Telefon Vibrieren

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Vibration** Klasse können Sie das Starten und beenden die Vibrieren-Funktionalität für eine gewünschte Zeitspanne.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Vibration** Funktionalität die folgende Plattform Einrichtung erforderlich ist.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Vibrieren-Berechtigung ist erforderlich und muss in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **"AssemblyInfo.cs"** -Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **"androidmanifest.xml"** -Datei unter dem **Eigenschaften** Ordner und fügen Sie Folgendes in der die **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** finden Sie die **erforderliche Berechtigungen:** Bereichs- und überprüfen Sie die **VIBRIEREN** Berechtigung. Dadurch wird automatisch aktualisiert. die **"androidmanifest.xml"** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-vibration"></a>Verwenden Ihr Telefon Vibrieren

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Vibration-Funktionalität kann für einen festgelegten Zeitraum oder den Standardwert von 500 Millisekunden angefordert werden.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Abbruch des Geräts Vibration kann angefordert werden, mit der `Cancel` Methode:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="androidtabandroid"></a>[Android](#tab/android)

Keine Plattformunterschiede.

# <a name="iostabios"></a>[iOS](#tab/ios)

* Vibriert nur, wenn Geräte auf "Vibrieren auf Ring" festgelegt ist.
* Immer vibriert für 500 Millisekunden.
* Nicht möglich, Vibration abzubrechen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="api"></a>API

- [Ihr Telefon Vibrieren-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Ihr Telefon Vibrieren-API-Dokumentation](xref:Xamarin.Essentials.Vibration)
