---
title: Xamarin.Essentials-Konnektivität
description: Die Konnektivität-Klasse können Sie die Änderungen in das Gerät die netzwerkbedingungen überwachen, überprüfen Sie den aktuellen Netzwerkzugriff und wie er derzeit verbunden ist.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 99faa518f44dca51cd1bbe2562a83893c48d2917
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials-Konnektivität

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Konnektivität** Klasse können Sie überwachen, Änderungen in das Gerät die netzwerkbedingungen, überprüfen Sie den aktuellen Netzwerkzugriff, und wie er derzeit verbunden ist.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Konnektivität** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die `AccessNetworkState` -Berechtigung ist erforderlich und muss in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **AssemblyInfo.cs** Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **AndroidManifest.xml** Datei unter dem **Eigenschaften** Ordner und fügen Sie die folgenden innerhalb eines der **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** Suchen der **die erforderlichen Berechtigungen verfügen:** Bereichs- und überprüfen Sie die **der Zugriffsebene "Netzwerk"** Berechtigung. Dadurch wird automatisch aktualisiert die **AndroidManifest.xml** Datei.

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-connectivity"></a>Mit der Konnektivität

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

[Netzwerkzugriff](xref:Xamarin.Essentials.NetworkAccess) fallen in folgenden Kategorien:

* **Internet** – lokale und vorhandenem Internetzugriff.
* **ConstrainedInternet** – Zugriff auf das Internet eingeschränkt. Gibt an die gebundenen-portalkonnektivität lokaler Zugriff auf Web-Portal wird bereitgestellt, wobei ein Zugriff auf das Internet erfordert, dass bestimmte Anmeldeinformationen über ein Portal bereitgestellt werden.
* **Lokale** – lokale nur Netzwerkzugriff.
* **Keine** – keine Konnektivität verfügbar ist.
* **Unbekannte** – Internetverbindung nicht erkannt.

Sie können überprüfen, welche Art von [Verbindungsprofils](xref:Xamarin.Essentials.ConnectionProfile) das Gerät aktiv verwendet wird:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Bei jedem Zugriff auf Änderungen, die Verbindungsprofils oder Netzwerk erhalten Sie ein Ereignis auslösen:

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

Es ist wichtig zu beachten, dass es möglich ist, `Internet` wird von gemeldet `NetworkAccess` vollen Zugriff auf das Web ist jedoch nicht verfügbar. Aufgrund der Funktionsweise von Verbindungen auf jeder Plattform können sie nur sicherstellen, dass eine Verbindung verfügbar ist. Das Gerät kann z. B. mit einem Wi-Fi-Netzwerk verbunden werden, aber der Router vom Internet getrennt ist. In dieser Instanz Internet möglicherweise gemeldet, aber eine aktive Verbindung ist nicht verfügbar.

## <a name="api"></a>API

* [Konnektivität-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/Connectivity)
* [Connectivity-API-Dokumentation](xref:Xamarin.Essentials.Connectivity)
