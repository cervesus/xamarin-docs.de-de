---
title: Problembehandlung Xamarin Live Player
description: In diesem Dokument werden bekannte Probleme mit dem Xamarin Live Player und potenziellen Fixes beschrieben. Es werden Verbindungsprobleme, Konfigurationsprobleme und mehr erläutert.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
ms.openlocfilehash: d51241bee5f4ddc06032006071fa8296be37f2fb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73005944"
---
# <a name="troubleshooting-xamarin-live-player"></a>Problembehandlung Xamarin Live Player

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player Vorschau wurde beendet. Die APP ist nicht mehr verfügbar. Die folgenden Anweisungen werden für Kunden bereitgestellt, die die Vorschauversion mit Visual Studio 2017 weiterhin verwenden.

> [!TIP]
> Sie können den [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md) in Visual Studio 2019 oder Visual Studio für Mac verwenden, um die Bildschirm Entwürfe während der Bearbeitung anzuzeigen.

In diesem Artikel werden die Einschränkungen von Live Player und einige häufige Probleme mit den Schritten zum Beheben der Fehler erläutert.

## <a name="limitations-of-xamarin-live-player"></a>Einschränkungen für Xamarin Live Player

### <a name="ide-requirements"></a>IDE-Anforderungen

Die Live Player-Vorschau ist nur in Visual Studio 2017 verfügbar.

### <a name="device-requirements"></a>Geräteanforderungen

Die Xamarin Live Player-App unterstützt die folgenden Android-Geräte:

- Android 4,2 oder höher.
- Arm-v7a, Arm-V8A, ARM64-V8A, x86 oder x86_64-Prozessor.

### <a name="ios-limitations"></a>IOS-Einschränkungen

Live Player ist für IOS nicht verfügbar.

### <a name="xamarinforms-limitations"></a>Xamarin. Forms-Einschränkungen

- Benutzerdefinierte Renderer werden nicht unterstützt.
- Effekte werden nicht unterstützt.
- Benutzerdefinierte Steuerelemente mit benutzerdefinierten bindbaren Eigenschaften werden nicht unterstützt.
- Eingebettete Ressourcen werden nicht unterstützt (d.h. das Einbetten von Bildern oder anderen Ressourcen in einer PCL).
- MVVM-Frameworks von Drittanbietern werden nicht unterstützt (d... Prism, MVVM Cross, MVVM Light usw.).

### <a name="other-project-type-limitations"></a>Einschränkungen für andere Projekttypen

- Live Player ist nicht für Native Android-Projekte gedacht (die Android XML für die Benutzeroberfläche verwenden).

### <a name="miscellaneous-limitations"></a>Sonstige Einschränkungen

- Eingeschränkte Unterstützung für Reflektion (wirkt sich zurzeit auf einige beliebte nugets aus, wie sqlite und JSON.net). Andere nugets können weiterhin unterstützt werden.
- Einige System Klassen können nicht überschrieben werden (z. b. können Sie keine Unterklasse implementieren).
- Einige Platt Form Features, die eine Bereitstellung erfordern, können nicht in der Xamarin Live Player-App funktionieren (Sie wurde jedoch für gängige Vorgänge wie den Zugriff auf Fotogalerie) konfiguriert.
- Benutzerdefinierte Ziele und Buildschritte werden ignoriert. Beispielsweise können Tools wie fody, Refit, autofac und AutoMapper nicht integriert werden.
- F#Projekte werden nicht unterstützt.
- Erweiterte Szenarien mit benutzerdefinierten generischen Klassen und Schnittstellen werden möglicherweise nicht unterstützt.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobiles Gerät stellt keine Verbindung nach dem Scannen des Barcodes her (oder wechselt in den Code)

Tritt auf, wenn sich das Mobile Gerät, auf dem Xamarin Live Player ausgeführt wird, nicht im selben Netzwerk wie der Computer befindet, auf dem die IDE Sehen Sie sich die folgenden Informationen an:

- Vergewissern Sie sich, dass sich sowohl das Gerät als auch der Computer im gleichen WLAN befinden.
  - Wenn der Computer auch mit einem verkabelten Netzwerk verbunden ist, versuchen Sie, die verdrahtete Verbindung wiederherzustellen.
- Das Netzwerk ist möglicherweise eng geschützt (z. b. einige Unternehmensnetzwerke), sodass die von Xamarin Live Player benötigten Ports blockiert werden.
- Schließen Sie die Xamarin Live Player App, und starten Sie Sie neu.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>"Fehler beim Bereitstellen der Meldung in der IDE

**"IOException: Es können keine Daten aus der Transport Verbindung gelesen werden: der Vorgang für einen nicht blockierenden Socket würde blockieren"**

Dieser Fehler tritt häufig auf, wenn sich das Mobile Gerät, das Xamarin Live Player ausgeführt wird, nicht im selben Netzwerk wie der Computer befindet, auf dem Visual Studio ausgeführt wird. Dies geschieht häufig, wenn eine Verbindung mit einem Gerät hergestellt wird, das zuvor erfolgreich gekoppelt wurde.

- Überprüfen Sie, ob sich das Gerät und der Computer im gleichen WLAN befinden.
- Das Netzwerk ist möglicherweise eng geschützt (z. b. einige Unternehmensnetzwerke), sodass die von Xamarin Live Player benötigten Ports blockiert werden. Die folgenden Ports sind für die Xamarin Live Player erforderlich:
  - 37847 – interner Netzwerk Zugriff 
  - 8090 – externer Netzwerk Zugriff

## <a name="manually-configure-device"></a>Manuelles Konfigurieren des Geräts

Wenn Sie über Wi-Fi keine Verbindung mit Ihrem Gerät herstellen können, können Sie versuchen, Ihr Gerät mithilfe der Konfigurationsdatei manuell zu konfigurieren, indem Sie die folgenden Schritte ausführen:

**Schritt 1: Öffnen der Konfigurationsdatei**

Wechseln Sie zum Ordner "Anwendungsdaten":

- Windows: **%UserProfile%\appdata\roaming**
- macOS: **~/Users/$User/.config**

In diesem Ordner finden Sie " **playerdevicelist. XML** ", wenn es nicht vorhanden ist. Sie müssen eines erstellen.

**Schritt 2: IP-Adresse erhalten**

Wechseln Sie in der Xamarin Live Player-App zu info **> Verbindungstest > starten Sie den Verbindungstest**.

Notieren Sie sich die IP-Adresse. Sie benötigen die IP-Adresse, die beim Konfigurieren des Geräts aufgeführt wird.

**Schritt 3: Holen Sie sich den paarcode.**

Klicken Sie erneut auf **paar** oder **paar**, und drücken Sie dann Xamarin Live Player die **EINGABETASTE manuell**. Es wird ein numerischer Code angezeigt, den Sie aktualisieren müssen, um die Konfigurationsdatei zu aktualisieren.

**Schritt 4: Generieren der GUID**

Wechseln Sie zu: https://www.guidgenerator.com/online-guid-generator.aspx, und generieren Sie eine neue GUID, und stellen Sie sicher, dass Großbuchstaben auf on.

**Schritt 5: Konfigurieren des Geräts**

Öffnen Sie die Datei " **playerdevicelist. XML** " in einem Editor wie z. b. Visual Studio oder Visual Studio Code. Sie müssen Ihr Gerät manuell in dieser Datei konfigurieren. Standardmäßig sollte die Datei das folgende leere `Devices` XML-Element enthalten:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Android-Gerät hinzufügen:**

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

**Schließen Sie Visual Studio, und öffnen Sie es erneut.** Ihr Gerät sollte in der Liste angezeigt werden.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Meldung "der Typ oder Namespace wurde nicht gefunden" in der IDE

Überprüfen Sie, ob Sie ein **Startprojekt** ausgewählt haben, das dem Gerätetyp entspricht (z. b. Android) und die Konfiguration mit dem Gerätetyp übereinstimmt (z. b. **Debuggen** für Android).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"Der Konstruktor für den Typ ' interpretedxamarin. Forms. Button ' wurde nicht gefunden"-Nachricht in Player

Einige System Klassen können nicht überschrieben werden, z. b.:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:" Resource. Layout "enthält keine Definition für" Main ".

Dieser Fehler tritt für Android-Projekte mit in axml-Dateien definierten Benutzeroberflächen auf.
Axml-Dateien werden in Xamarin Live Player zurzeit nicht unterstützt.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android-Symbolleiste und Registerkarten werden mithilfe von xamarin. Forms falsch dargestellt.

Xamarin. Forms Android-Projekte müssen "Toolbar. axml" und "Tabbar. axml" für die Namen der relevanten Layoutdateien verwenden. Die Standardvorlage verwendet diese Namen. Das Umbenennen führt zu Renderingproblemen.

## <a name="related-links"></a>Verwandte Links

- [Setup (Einrichtung)](~/tools/live-player/install.md)
