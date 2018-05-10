---
title: Problembehandlung
description: Bekannte Probleme mit der Xamarin-Live-Player und wie sie diesen Fehler zu beheben.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 147ce43d3fe764f71f27dce46b699142dfb99872
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="troubleshooting"></a>Problembehandlung

![Vorschaufunktion](~/media/shared/preview.png)

Diesem Artikel erläutert einige der häufigsten Probleme sowie Schritte, um diese zu beheben.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobiler Geräte wird nicht nach Scan Barcode (oder Code eingeben) eine Verbindung herstellen.

Tritt auf, wenn das mobile Gerät mit Xamarin-Live-Player im selben Netzwerk wie der Computer mit der IDE nicht ist. Sehen Sie sich Folgendes:

- Vergewissern Sie sich, dass das Gerät und der Computer im selben Wi-Fi-Netzwerk sind.
  - Wenn der Computer auch mit einem verkabelten Netzwerk verbunden ist, versuchen Sie die drahtgebundene Verbindung trennen.
- Das Netzwerk möglicherweise eng gesichert werden (z. B. Unternehmensnetzwerken), den von Xamarin Live Player erforderlichen Ports blockieren.
- Schließen Sie die Xamarin-Live-Player-Anwendung, und starten Sie ihn neu.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>"Fehler beim Versuch, das Bereitstellen von" Nachricht in der IDE

**"IOException: kann nicht zum Lesen von Daten von der übertragungsverbindung: Vorgang auf Socket nicht blockierend würde blockieren."**

Dieser Fehler wird häufig aufgetreten ist, wenn das mobile Gerät mit Xamarin Live Player nicht im selben Netzwerk wie der Visual Studio-Computer ist; Dies erfolgt häufig auf, wenn auf einem Gerät eine Verbindung herstellen, die zuvor erfolgreich zugewiesen wurde.

* Überprüfen Sie, dass das Gerät und der Computer im selben Wi-Fi-Netzwerk sind.
* Das Netzwerk möglicherweise eng gesichert werden (z. B. Unternehmensnetzwerken), den von Xamarin Live Player erforderlichen Ports blockieren. Die folgenden Ports sind für die Xamarin-Live-Player erforderlich:
  * 37847 – internen Netzwerkzugriff 
  * 8090 – externe Netzwerkzugriff

## <a name="manually-configure-device"></a>Konfigurieren Sie Gerät manuell.

Wenn Sie nicht auf Ihr Gerät über WLAN verbinden können können Sie versuchen, Ihr Gerät über die XML-Konfigurationsdatei mit den folgenden Schritten manuell zu konfigurieren:

**Schritt 1: Öffnen der Konfigurationsdatei**

Rufen Sie Anwendungsordner für Daten:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

In diesem Ordner finden Sie **PlayerDeviceList.xml** , wenn er nicht vorhanden ist, müssen zu erstellen.

**Schritt 2: Abrufen der IP-Adresse**

Wechseln Sie in der Live-Player Xamarin-app zu **zu > Verbindungstest > Verbindungstest starten**.

Beachten Sie die IP-Adresse, benötigen Sie die IP-Adresse aufgelistet, wenn Sie Ihr Gerät konfigurieren.

**Schritt 3: Rufen Sie Kopplung Code ab**

Innerhalb der Xamarin-Live-Player Tap **Paar** oder **Paar erneut**, drücken Sie dann die **manuell eingeben**. Ein numerischer Code wird angezeigt, die müssen Sie die Konfigurationsdatei aktualisieren.

**Schritt 4: GUID generiert.**

Wechseln Sie zu: https://www.guidgenerator.com/online-guid-generator.aspx und generieren Sie eine neue Guid und sicherzustellen, dass Großbuchstaben auf.


**Schritt 5: Konfigurieren von Gerät**

Öffnen Sie die **PlayerDeviceList.xml** in einem Editor wie z. B. Visual Studio oder Visual Studio Code einrichten. Sie müssen Ihr Gerät in diese Datei manuell konfigurieren. Standardmäßig sollte die Datei den folgenden leeren enthalten `Devices` XML-Element:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Fügen Sie die iOS-Gerät:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```


**Hinzufügen von Android-Gerät:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Schließen Sie und öffnen Sie Visual Studio erneut.** Ihr Gerät sollte in der Liste angezeigt.


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>"Typ oder Namespace wurde nicht gefunden" Nachricht in der IDE

Überprüfung, die Sie ausgewählt haben eine **Startprojekt** , die den Typ Ihres Geräts (iOS oder Android) entspricht und dass die Konfiguration dieser Gerätetyp (z. b. übereinstimmt **Debuggen | iPhone-Simulator** für iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"Konstruktor für Typ"InterpretedXamarin.Forms.Button"nicht gefunden" Nachricht im Player

Einige Systemklassen können, z. B. nicht überschrieben werden:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:"Resource.Layout"enthält keine Definition für"Main""

Dieser Fehler tritt für Android-Projekte mit Benutzeroberflächen in AXML-Dateien definiert.
AXML Dateien sind derzeit nicht im Xamarin-Live-Player unterstützt.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android Symbolleiste und Registerkarten falsch mit Xamarin.Forms gerendert werden

Xamarin.Forms Android-Projekte müssen "Toolbar.axml" und "Tabbar.axml" für den Namen der relevanten Layoutdateien verwenden. Die Standardvorlage verwendet diese Namen. Probleme beim Rendern wird durch das Umbenennen sie verursacht werden.


Melden Sie alle weiteren Probleme auf [Bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/tools/live-player/limitations.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
