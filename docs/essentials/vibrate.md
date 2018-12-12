---
title: 'Xamarin.Essentials: Vibrieren'
description: In diesem Dokument wird die Klasse „Vibration“ in Xamarin.Essentials beschrieben, mit der Sie die Vibrationsfunktion für eine gewünschte Zeitspanne starten und anhalten können.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d9bf7a1e5e0d15f1fdc909745cd439115b6f8463
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52898935"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Vibrieren

Mit der Klasse **Vibration** können Sie die Vibrationsfunktion für eine gewünschte Zeitspanne starten und anhalten.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **Vibrationsfunktion** ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Berechtigung „Vibrate“ (Vibrieren) ist obligatorisch und muss im Android-Projekt konfiguriert werden. Das Hinzufügen erfolgt folgendermaßen:

Öffnen Sie die Datei **AssemblyInfo.cs** im Ordner **Eigenschaften** und fügen Sie Folgendes hinzu:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

Alternativ können Sie das Android-Manifest aktualisieren:

Öffnen Sie die Datei **AndroidManifest.xml** im Ordner **Eigenschaften**, und fügen Sie Folgendes im Knoten **Manifest** hinzu.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Suchen Sie unter **Android-Manifest** den Bereich **Erforderliche Berechtigungen:**, und aktivieren Sie die Berechtigung **VIBRATE** (Vibrieren). Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

# <a name="iostabios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="using-vibration"></a>Verwenden der Vibrationsfunktion

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Vibrationsfunktion kann für eine bestimmte Zeitspanne oder den Standardzeitraum von 500 Millisekunden angefordert werden.

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

Der Abbruch der Gerätevibration kann mit der Methode `Cancel` angefordert werden:

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

* Vibriert nur, wenn das Gerät auf „Beim Klingeln vibrieren“ eingestellt ist.
* Vibriert immer für 500 Millisekunden.
* Es ist nicht möglich, die Vibration abzubrechen.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Keine Plattformunterschiede.

-----

## <a name="api"></a>API

- [Vibration-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Vibration-API-Dokumentation](xref:Xamarin.Essentials.Vibration)
