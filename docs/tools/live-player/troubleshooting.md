---
title: Problembehandlung bei Xamarin Live Player
description: Dieses Dokument beschreibt bekannte Probleme mit dem Xamarin Live Player und mögliche Korrekturen. Es wird erläutert, Verbindungsprobleme, Konfigurationsprobleme und mehr.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: lobrien
ms.author: laobri
ms.date: 06/13/2019
ms.openlocfilehash: bf0186b55b14d9797397b98390f4d825d669d0f4
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157686"
---
# <a name="troubleshooting-xamarin-live-player"></a>Problembehandlung bei Xamarin Live Player

![Feature der Vorschauversion](~/media/shared/preview.png)

> [!WARNING]
> Die Xamarin Live Player-Vorschauversion wurde beendet. Die app ist nicht mehr verfügbar. Die folgenden Anweisungen sind für Kunden, die Sie weiterhin die Vorschau mit Visual Studio 2017 verwenden bereitgestellt...

> [!TIP]
> Sie können die [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer/index.md) in 2019 für Visual Studio oder Visual Studio für Mac Ihre Entwürfe Bildschirm anzuzeigen, wie Sie sie bearbeiten.

Dieser Artikel beschreibt die Einschränkungen von Live Player und einige der häufigsten Probleme mit den Schritten zu beheben.

## <a name="limitations-of-xamarin-live-player"></a>Einschränkungen von Xamarin Live Player

### <a name="ide-requirements"></a>IDE-Anforderungen

Der Live Player-Vorschau ist nur in Visual Studio 2017 verfügbar.

### <a name="device-requirements"></a>Anforderungen für Geräte

Die Xamarin Live Player-app unterstützt folgende Android-Geräte:

- Android 4.2 oder höher.
- ARM-v7a, ARM-v8a "," ARM64-v8a, x 86- oder x86_64-Prozessor.

### <a name="ios-limitations"></a>iOS-Einschränkungen

Live Player ist nicht verfügbar für iOS.

### <a name="xamarinforms-limitations"></a>Xamarin.Forms-Einschränkungen

- Benutzerdefinierte Renderer werden nicht unterstützt.
- Effekte werden nicht unterstützt.
- Benutzerdefinierte Steuerelemente mit benutzerdefinierten bindbare Eigenschaften werden nicht unterstützt.
- Eingebettete Ressourcen werden nicht unterstützt (d. h. Einbetten von Bildern oder anderen Ressourcen in einer PCL).
- MVVM-Frameworks von Drittanbietern werden (d. h. nicht unterstützt. Prism, Mvvm Cross, Mvvm Light usw.).

### <a name="other-project-type-limitations"></a>Weitere Einschränkungen der Projekt-Typ

- Live Player dient nicht für native Android-Projekte (die Android XML für die Benutzeroberfläche zu verwenden).

### <a name="miscellaneous-limitations"></a>Verschiedene Einschränkungen

- Eingeschränkte Unterstützung für die Reflektion (derzeit einige beliebte NuGet-Pakete, z. B. SQLite und Json.NET betrifft). Andere NuGet-Pakete werden möglicherweise immer noch unterstützt.
- Einige Systemklassen können nicht überschrieben werden (z. B. eine Unterklasse kann nicht implementiert werden).
- Einige Features der Plattform, für die Bereitstellung erforderlich können nicht funktionieren, in die Xamarin Live Player-app (jedoch sie bei allgemeinen Vorgängen, z. B. Photo Gallery-Zugriff konfiguriert wurde).
- Benutzerdefinierte Ziele und Schritte werden ignoriert. Beispielsweise können keine Tools wie Fody, einpassen, AutoFac und AutoMapper integriert werden.
- F#Projekte werden nicht unterstützt.
- Erweiterte Szenarien mit benutzerdefinierten generische Klassen und Schnittstellen werden möglicherweise nicht unterstützt.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobiler Geräte wird nicht nach Scan Barcode (oder Eingeben von Code) eine Verbindung herstellen.

Tritt auf, wenn das mobile Gerät, das Ausführen von Xamarin Live Player nicht im gleichen Netzwerk wie der Computer mit der IDE ist. Sehen Sie sich Folgendes:

- Vergewissern Sie sich, dass das Gerät und die Computer im gleichen WLAN-Netzwerk sind.
  - Wenn der Computer auch mit einem verkabelten Netzwerk verbunden ist, versuchen Sie die verdrahtete Verbindung trennen.
- Das Netzwerk kann umfassend geschützt werden (z. B. Unternehmensnetzwerken), die von Xamarin Live Player benötigte Ports blockiert.
- Schließen Sie die Xamarin Live Player-app, und starten Sie ihn neu.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>"Fehler beim Bereitstellen von"-Nachricht in der IDE

**"IOException: kann nicht zum Lesen von Daten aus der transportverbindung: Vorgang auf Socket nicht blockierend würde blockieren."**

Dieser Fehler häufig auftritt, wenn das mobile Gerät, das Ausführen von Xamarin Live Player nicht im gleichen Netzwerk wie der Computer mit Visual Studio ist; Dies geschieht häufig, wenn auf einem Gerät eine Verbindung herstellen, die zuvor erfolgreich zugeordnet wurde.

* Überprüfen Sie, dass das Gerät und die Computer im gleichen WLAN-Netzwerk sind.
* Das Netzwerk kann umfassend geschützt werden (z. B. Unternehmensnetzwerken), die von Xamarin Live Player benötigte Ports blockiert. Die folgenden Ports sind für den Xamarin Live Player erforderlich:
  * 37847 – internen Netzwerkzugriff 
  * 8090 – externe Netzwerkzugriff

## <a name="manually-configure-device"></a>Konfigurieren Sie Gerät manuell.

Wenn Sie nicht auf Ihr Gerät über WLAN verbinden können können Sie versuchen, Ihr Gerät über die XML-Konfigurationsdatei mit den folgenden Schritten manuell zu konfigurieren:

**Schritt 1: Konfigurationsdatei öffnen**

Navigieren Sie zum Anwendungsordner für Daten:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

In diesem Ordner finden Sie **PlayerDeviceList.xml** , wenn er nicht vorhanden ist, müssen zum Erstellen.

**Schritt 2: IP-Adresse abrufen**

Wechseln Sie in der Xamarin Live Player-app zu **über > Verbindungstest > Verbindungstest starten**.

Notieren Sie sich die IP-Adresse, benötigen Sie die IP-Adresse aufgelistet, wenn Sie Ihr Gerät zu konfigurieren.

**Schritt 3: Abrufen von Kopplungscode**

In den Xamarin Live Player-Tap **Paar** oder **Paar erneut**, drücken Sie dann die **manuell eingeben**. Ein numerischer Code wird angezeigt werden die müssen Sie die Konfigurationsdatei aktualisieren.

**Schritt 4: GUID zu generieren**

Wechseln Sie zu: https://www.guidgenerator.com/online-guid-generator.aspx und generiert eine neue Guid und sicherzustellen, dass Großbuchstaben auf.

**Schritt 5: Gerät konfigurieren**

Öffnen Sie die **PlayerDeviceList.xml** in einem Editor wie z. B. Visual Studio oder Visual Studio Code einrichten. Sie müssen Ihr Gerät in dieser Datei manuell zu konfigurieren. Standardmäßig sollte die Datei den folgenden leeren enthalten `Devices` XML-Element:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
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

**Schließen Sie und öffnen Sie Visual Studio erneut.** Ihr Gerät sollte in der Liste angezeigt werden.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Nachricht "der Typ oder Namespace wurde nicht gefunden", in der IDE

Überprüfen Sie, die Sie ausgewählt haben eine **Startprojekt** , entspricht den Gerätetyp (z. b. Android) und dass Sie mit der Konfiguration übereinstimmt, Gerätetyp (z. b. **Debuggen von** für Android).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"Konstruktor für Typ 'InterpretedXamarin.Forms.Button' wurde nicht gefunden"-Nachricht im Player

Einige "System"-Klassen nicht möglich, z. B. überschrieben werden:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: "Resource.Layout" enthält keine Definition für 'Main' "

Dieser Fehler tritt auf, für Android-Projekte mit Benutzeroberflächen in AXML-Dateien definiert.
AXML-Dateien werden in Xamarin Live Player derzeit nicht unterstützt.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android-Symbolleiste und Registerkarten rendern nicht ordnungsgemäß mit Xamarin.Forms

Xamarin.Forms-Android-Projekte müssen "Toolbar.axml" und "Tabbar.axml" für die Namen der relevanten Layoutdateien verwenden. Die Standardvorlage verwendet diesen Namen. Umbenennen wird Renderingprobleme verursachen.

## <a name="related-links"></a>Verwandte Links

- [Setup (Einrichtung)](~/tools/live-player/install.md)
