---
title: Xamarin.Essentials Vibrationen
description: Die Vibrationen-Klasse können Sie die starten und beenden die Vibrate-Funktionalität für einen gewünschten Zeitraum.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 53271f3cee06f33cea4fa0bd28d3cff1baf0cd3e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials Vibrationen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Vibrationen** -Klasse können Sie das Starten und beenden die Vibrate-Funktionalität für einen gewünschten Zeitraum.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Vibrationen** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Vibrate-Berechtigung ist erforderlich und muss in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **AssemblyInfo.cs** Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **AndroidManifest.xml** Datei unter dem **Eigenschaften** Ordner und fügen Sie die folgenden innerhalb eines der **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** Suchen der **die erforderlichen Berechtigungen verfügen:** Bereichs- und überprüfen Sie die **VIBRATE** Berechtigung. Dadurch wird automatisch aktualisiert die **AndroidManifest.xml** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-vibration"></a>Vibrationen verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Vibrationen-Funktionalität kann für eine Zeitspanne festlegen oder den Standardwert von 500 Millisekunden angefordert werden.

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

Der Abbruch des Geräts Vibrationen kann angefordert werden, mit der `Cancel` Methode:

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

## <a name="platform-differences"></a>Platform-Unterschiede

| Plattform | Unterschied |
| --- | --- |
| iOS | Immer Urgency für 500 Millisekunden. |
| iOS | Nicht möglich, Vibrationen "Abbrechen". |

## <a name="api"></a>API

- [Vibrationen-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Vibration)
- [Vibrationen API-Dokumentation](xref:Xamarin.Essentials.Vibration)
