---
title: 'Xamarin.Essentials: Konnektivität'
description: Mit der Connectivity-Klasse in Xamarin.Essentials können Sie Änderungen der Netzwerkbedingungen des Geräts überwachen, den aktuellen Netzwerkzugriff überprüfen und die aktuelle Verbindung bestimmen.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: bc0cd206881356a92128c758f0225f98f88c2814
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675405"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: Konnektivität

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Mit der **Connectivity**-Klasse können Sie Änderungen der Netzwerkbedingungen des Geräts überwachen, den aktuellen Netzwerkzugriff überprüfen und die aktuelle Verbindung bestimmen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Der Zugriff auf die **Connectivity**-Funktionalität erfordert das folgende plattformspezifische Setup.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Berechtigung `AccessNetworkState` ist obligatorisch und muss im Android-Projekt konfiguriert werden. Das Hinzufügen erfolgt folgendermaßen:

Öffnen Sie die Datei **AssemblyInfo.cs** im Ordner **Eigenschaften** und fügen Sie Folgendes hinzu:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

Alternativ können Sie das Android-Manifest aktualisieren:

Öffnen Sie die Datei **AndroidManifest.xml** im Ordner **Eigenschaften**, und fügen Sie Folgendes im Knoten **Manifest** hinzu.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Überprüfen Sie unter **Android-Manifest** im Bereich **Erforderliche Berechtigungen:** die Berechtigung **Zugriff auf Netzwerkstatus**. Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

# <a name="iostabios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Es ist kein zusätzliches Setup erforderlich.

-----

## <a name="using-connectivity"></a>Verwenden der Konnektivität

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Überprüfen Sie den aktuellen Netzwerkzugriff:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Netzwerkzugriff](xref:Xamarin.Essentials.NetworkAccess) lässt sich in die folgenden Kategorien unterteilen:

* **Internet**: Lokaler und Internetzugriff.
* **ConstrainedInternet**: Eingeschränkter Internetzugriff. Beschreibt die Konnektivität über ein Erfassungsportal: Es wird lokaler Zugriff auf ein Webportal bereitgestellt, doch der Zugriff auf das Internet erfordert, dass bestimmte Anmeldeinformationen über ein Portal angegeben werden.
* **Lokal**: Nur lokaler Netzwerkzugriff.
* **Kein**:Keine Verbindung verfügbar.
* **Unbekannt**: Internetkonnektivität kann nicht bestimmt werden.

Sie können überprüfen, welche Art von [Verbindungsprofil](xref:Xamarin.Essentials.ConnectionProfile) das Gerät aktiv verwendet:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Wenn sich das Verbindungsprofil oder der Netzwerkzugriff ändern, wird ein Ereignis ausgelöst, das Sie empfangen können:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(object sender, ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Einschränkungen

Beachten Sie, dass möglicherweise `Internet` von `NetworkAccess` gemeldet wird, jedoch kein Vollzugriff auf das Web zur Verfügung steht. Aufgrund der unterschiedlichen Funktionsweise von Konnektivität auf den Plattformen kann sie nur garantieren, dass eine Verbindung verfügbar ist. So kann das Gerät beispielsweise mit WLAN verbunden sein, doch der Router ist vom Internet getrennt. In diesem Fall wird ggf. Internet gemeldet, obwohl keine aktive Verbindung verfügbar ist.

## <a name="api"></a>API

* [Connectivity-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Connectivity-API-Dokumentation](xref:Xamarin.Essentials.Connectivity)
