---
title: 'Xamarin.Essentials: Konnektivität'
description: Die Konnektivität-Klasse in Xamarin.Essentials können Sie die Änderungen in das Gerät die netzwerkbedingungen zu überwachen, überprüfen Sie den aktuellen Netzwerkzugriff haben und wie sie derzeit verbunden ist.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353697"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: Konnektivität

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Konnektivität** Klasse können Sie Änderungen in das Gerät die netzwerkbedingungen, überprüfen Sie den aktuellen Netzwerkzugriff und wie sie derzeit verbunden ist.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Konnektivität** Funktionalität die folgende Plattform Einrichtung erforderlich ist.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die `AccessNetworkState` -Berechtigung ist erforderlich und muss in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **"AssemblyInfo.cs"** -Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **"androidmanifest.xml"** -Datei unter dem **Eigenschaften** Ordner und fügen Sie Folgendes in der die **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** finden Sie die **erforderliche Berechtigungen:** Bereichs- und überprüfen Sie die **Netzwerk Zugriffsstatus** Berechtigung. Dadurch wird automatisch aktualisiert. die **"androidmanifest.xml"** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-connectivity"></a>-Konnektivität

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Überprüfen Sie die aktuellen Netzwerkzugriff:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Netzwerkzugriff](xref:Xamarin.Essentials.NetworkAccess) lässt sich in den folgenden Kategorien aufteilen:

* **Internet** – lokal und vorhandenem Internetzugriff.
* **ConstrainedInternet** – Zugriff auf das Internet beschränkt. Gibt an, captive Portal-Konnektivität, in denen der lokale Zugriff auf ein Webportal bereitgestellt, aber Zugriff auf das Internet erfordert, dass bestimmte Anmeldeinformationen über ein Portal bereitgestellt werden.
* **Lokale** – lokale nur Netzwerkzugriff.
* **Keine** – es ist keine Verbindung zur Verfügung.
* **Unbekannter** – kann nicht über Internetkonnektivität bestimmt.

Sie können überprüfen, welche Art von [Verbindungsprofil](xref:Xamarin.Essentials.ConnectionProfile) das Gerät ist aktiv verwenden:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Wenn der Verbindungsprofil oder im Netzwerk auf Änderungen erhalten Sie ein Ereignis ausgelöst:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Einschränkungen

Es ist wichtig zu beachten, dass es möglich, `Internet` wird von gemeldet `NetworkAccess` vollen Zugriff auf das Web ist jedoch nicht verfügbar. Aufgrund der Funktionsweise der Konnektivität auf jeder Plattform können sie nur sicherstellen, dass eine Verbindung verfügbar ist. Beispielsweise kann das Gerät mit einem Wi-Fi-Netzwerk verbunden, aber der Router vom Internet getrennt wird. In diesem Fall Internet gemeldet werden kann, aber eine aktive Verbindung ist nicht verfügbar.

## <a name="api"></a>API

* [Konnektivität-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Connectivity-API-Dokumentation](xref:Xamarin.Essentials.Connectivity)
