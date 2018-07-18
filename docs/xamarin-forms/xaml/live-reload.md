---
title: Live neu laden
description: Finden Sie Änderungen an Ihrer XAML Live-, wiedergegeben werden, ohne dass eine andere kompilieren und bereitstellen.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: 12b677c8cc4a709a865d2eaee3ea44a6babf1b05
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38860666"
---
# <a name="xamarin-live-reload"></a>Xamarin Live neu laden

![Vorschau](~/media/shared/preview.png)

Xamarin Live Reload ermöglicht es Ihnen, **nehmen Sie Änderungen an Ihrer XAML und finden Sie diese sofort live, ohne dass eine andere kompilieren und Bereitstellen von**. Alle Änderungen an Ihre XAML werden erneut bereitgestellt werden zu speichern und wiedergegeben werden, auf dem Zielcomputer bereitstellen.

Da Ihre app bei Verwendung von Live Reload kompiliert wird, funktioniert mit allen Bibliotheken und Drittanbieter-Steuerelementen. Live neu laden funktioniert auf allen Plattformen, die xamarin.Forms unterstützt, einschließlich Android, iOS und UWP, und alle gültigen Bereitstellungsziele, einschließlich Simulatoren, Emulatoren und physische Geräte funktioniert.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live neu laden ist derzeit nur in Visual Studio 2017 verfügbar.

## <a name="requirements"></a>Anforderungen

* [Visual Studio 2017 Version 15.7 oder höher](https://visualstudio.microsoft.com/vs/) oder höher mit der **Mobile Entwicklung mit .NET** arbeitsauslastung.
* [Xamarin.Forms 3.0.0 oder höher](https://www.nuget.org/packages/Xamarin.Forms/) oder höher.

## <a name="getting-started"></a>Erste Schritte
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Installieren Sie Xamarin Live neu laden von Visual Studio Marketplace

Xamarin Live Reload wird über den Visual Studio Marketplace verteilt. Um die Erweiterung zu installieren, besuchen Sie die [Xamarin Live Reload-Seite in Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) -Website und klicken Sie auf **herunterladen**.

Öffnen Sie die VSIX, die heruntergeladen wird, und klicken Sie auf **installieren**.

![Visual Studio-Installer Xamarin Live Reload-Bestätigung](images/LiveReloadVSIXInstall.png)

Alternativ können Sie hierfür im Suchen der **Online** Registerkarte die **Erweiterungen und Updates** Dialogfeld in Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurieren Sie Ihrer app zur Verwendung von Live neu laden

Hinzufügen von Live Reload zu vorhandenen mobile apps kann in drei Schritten erfolgen:

1. Stellen Sie sicher, alle Projekte werden aktualisiert, um verwenden [Xamarin.Forms 3.0.0 oder höher](https://www.nuget.org/packages/Xamarin.Forms/) oder höher.

2. Hinzufügen der **Xamarin.LiveReload** NuGet-Paket:

    a. **.NET standard** – installieren Sie die **Xamarin.LiveReload** NuGet in die .NET Standard 2.0-Bibliothek. Dies muss nicht in Ihren Plattform-Projekten installiert werden. Sicherstellen, dass die **Paketquelle** nastaven NA hodnotu **alle**.
    
    b. **Freigegebene Projekte** – installieren Sie die **Xamarin.LiveReload** NuGet in alle Plattformprojekte (z. B. mit Android, iOS, UWP, usw.). Sicherstellen, dass die **Paketquelle** nastaven NA hodnotu **alle**.

    [![Xamarin Live Reload NuGet mit NuGet-Paket-Manager hinzufügen](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Hinzufügen `LiveReload.Init();` an den Konstruktor in der `Application` Klasse, wie im folgenden Codeausschnitt gezeigt:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Starten Sie live neu laden

Kompilieren Sie und Bereitstellen Sie Ihrer Anwendung. Sobald die app bereitgestellt ist, öffnen Sie eine XAML-Datei, ein nehmen Sie Paar Änderungen vor und speichern Sie die Datei. Ihre Änderungen werden an das Bereitstellungsziel erneut bereitgestellt.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live neu laden, die mit Änderungen auf alle XAML-Dateien funktioniert. Änderungen an den C#- oder hinzufügen bzw. Entfernen von NuGet-Pakete müssen einen neuen Build und bereitstellen, um wirksam werden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs) 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Ist Xamarin Live Reload für Mac in Visual Studio verfügbar? 

Die erste Vorschauversion von Xamarin Live Reload ist nur für Visual Studio 2017 verfügbar. Unterstützung für Visual Studio für Mac ist für eine künftige Version geplant.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Funktioniert dies mit der alle Bibliotheken verwenden, z. B. Prism? 

Da es sich bei Ihrer app kompiliert wird, funktioniert Live Reload mit alle Bibliotheken verwenden, z. B. Prism und Bibliotheken der Drittanbieter-Steuerelement, z. B. Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity und anderen Herstellern Steuerelement aus.

### <a name="what-changes-does-live-reload-redeploy"></a>Welche Änderungen bereitstellen Live Reload erneut? 

Live neu laden gilt nur Änderungen an XAML oder CSS-Code. Wenn Sie eine C#-Datei ändern, wird eine Neukompilierung erforderlich sein. Unterstützung für das erneute Laden C# -Code ist für eine zukünftige Version geplant.

### <a name="what-platforms-are-supported"></a>Welche Plattformen werden unterstützt? 

Live neu laden funktioniert auf allen Plattformen, die von Xamarin.Forms, einschließlich Android, iOS und UWP unterstützt werden.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Funktioniert dies auf Emulatoren und Simulatoren physische Geräte? 

Ja, funktioniert Live Reload mit allen gültigen Bereitstellungsziele, einschließlich Android-Emulatoren, iOS-Simulatoren und physische Geräte. Bereitstellung auf einem Gerät erfordert, dass das Gerät und der Computer im gleichen WLAN-Netzwerk.

### <a name="does-this-work-with-corporate-networks"></a>Funktioniert dies mit Unternehmensnetzwerken?

Wenn Sie auf einem Android-Emulator oder auf iOS-Simulator Debuggen, verwendet Live Reload "localhost" Kommunikation. Wenn Sie auf einem Gerät bereitstellen möchten, müssen die Geräte und Computer im gleichen WLAN-Netzwerk sein. In Szenarien, in denen dies nicht möglich ist, können Sie [konfigurieren Sie Ihren eigenen Server Live Reload](#live-reload-server), dadurch werden Sie zum Live neu laden, unabhängig von Einstellungen für die Netzwerkkonnektivität.

### <a name="does-it-require-debugging-the-app"></a>Ist es erforderlich, die app zu debuggen? 

Nein. In der Tat können sogar starten Sie alle Ihre unterstützten Anwendungsziele (Android, iOS und UWP) auf eine beliebige Anzahl von Geräten und Simulatoren/Emulatoren, und sehen sie alle gleichzeitig zu aktualisieren. 

## <a name="limitations"></a>Einschränkungen

* XAML nur das erneute Laden wird unterstützt.
* Zustand der Benutzeroberfläche möglicherweise zwischen erneut bereitstellen, nicht verwaltet werden, es sei denn, mithilfe von MVVM.

## <a name="known-issues"></a>Bekannte Probleme

* Nur unterstützt in Visual Studio.
* Verknüpfen von muss festgelegt werden, um **nicht verknüpfen** oder **Link nur Framework-SDKs** 
* Erneutes Laden der gesamten app-Ressourcen (z. B. **"App.xaml"** oder freigegebenen Ressourcenverzeichnisse), app-Navigation wird zurückgesetzt. Dies wird in der nächsten Vorabversion behoben werden.
* Bearbeiten von XAML beim Debuggen von UWP einen Absturz zur Laufzeit verursachen. Problemumgehung: Verwenden Sie **Starten ohne Debuggen (STRG + F5)** anstelle von **starten (F5) Debuggen**.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-codes"></a>Fehlercodes

* **XLR001**: *das aktuelle Projekt verweist auf die "Xamarin.LiveReload" NuGet-Paket-Version [VERSION], aber die Xamarin Live Reload-Erweiterung erfordert Version [VERSION].*

  Um eine schnelle Iteration und Weiterentwicklung von Live Reload-Funktion zu ermöglichen, müssen das Nuget-Paket und die Visual Studio-Erweiterung genau übereinstimmen. Aktualisieren Sie Ihr Nuget-Paket, auf die gleiche Version der Erweiterung, die Sie installiert haben.

* **XLR002**: *Live Reload erfordert mindestens die Eigenschaft "MqttHostname" Wenn Sie über die Befehlszeile zu erstellen. Alternativ festlegen Sie 'EnableLiveReload' auf 'False', um das Feature deaktivieren.*

  Die Live Reload erforderlichen Eigenschaften sind nicht verfügbar, beim Erstellen von der Befehlszeile aus (oder in der continuous Integration) und muss daher explizit angegeben werden. 

* **XLR003**: *Live Reload-Nuget-Paket erfordert die Installation von Xamarin Live Reload Visual Studio-Erweiterung.*

  Es wurde versucht, ein Projekt erstellen, die auf das Nuget-Paket für Live neu laden, aber die Erweiterung für Visual ist nicht installiert.  

* *Ausnahme beim Laden von Assemblys: System.IO.FileNotFoundException: Assembly konnte nicht geladen "Xamarin.Live.Reload, Version = 0.3.27.0, Culture = Neutral, PublicKeyToken =".*

  Das Hostprojekt verwenden soll `PackageReference` anstelle von `packages.config`

### <a name="app-doesnt-connect"></a>App verbunden nicht.

Wenn die Anwendung wird erstellt, die Informationen aus **Tools > Optionen > Xamarin > Live Reload** (Host, Port und die Verschlüsselungsoption Schlüssel) werden in der app eingebettet, also bei `LiveReload.Init()` ausgeführt wird, keine Kopplung oder die Konfiguration ist erforderlich für die Verbindung hergestellt werden soll.

Als normale Netzwerkprobleme ("Firewall", "Gerät auf einem anderen Netzwerk") ist die Hauptgründe dafür, dass die app keine erfolgreiche IDE hergestellt, da es sich bei die Konfiguration von in Visual Studio unterscheidet. Mögliche Ursachen:

* App wurde auf einem anderen Computer kompiliert.
* App wurde kompiliert und bereitgestellt, die in einer anderen Visual Studio-Sitzung, und **Automatisches Generieren von Verschlüsselungsschlüsseln** ist aktiviert (Standard) **Tools > Optionen > Xamarin > Live Reload**.
* Visual Studio-Einstellungen geändert wurden (d. h., den Port oder Verschlüsselung-Schlüssel), aber die app wurde nicht erstellt und erneut bereitgestellt werden.

Diese Fälle, die alle gelöst werden, indem erstellen und Bereitstellen der app erneut.

### <a name="uninstalling-preview-1"></a>Deinstallieren von Preview 1

Wenn Sie eine ältere Vorschauversion und Problemen deinstallieren, gehen Sie wie folgt vor:

1. Löschen Sie den Ordner **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (Hinweis: Ersetzen Sie "Enterprise", durch die installierte Edition aus, und "Preview" mit "2017", wenn Sie installiert einen stabilen im Vergleich)
2. Öffnen einer **Developer-Eingabeaufforderung** für die Visual Studio und führen Sie `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Tipps und Tricks

* Solange die Live Reload-Einstellungen nicht ändern (einschließlich der Verschlüsselungsschlüssel verwendet, z. B. Wenn Sie wieder deaktivieren **Automatisches Generieren von Verschlüsselungsschlüsseln**) und Sie aus dem gleichen Computer erstellen, müssen Sie nicht zum Erstellen und Bereitstellen der app nach der Erstkonfiguration Stellen Sie bereit, es sei denn, Sie über Code oder Abhängigkeiten ändern. Sie können nur starten, erneut eine zuvor bereitgestellte app, und es stellt eine Verbindung her mit dem letzten Host verwendet wird.

* Es gibt keine Einschränkung für wie viele Geräte, die Sie mit der gleichen Visual Studio-Sitzung eine Verbindung herstellen können. Sie können die bereitstellen und starten Sie die app in so viele Geräte-Simulatoren, sofern nötig, um live Neuladen arbeiten für alle zur gleichen Zeit finden Sie unter.

* Live neu laden wird nur neu den Benutzeroberflächenteil der app geladen, sondern nur *nicht* Ihre Seiten neu zu erstellen, die weder ersetzt es Ihrem Ansichtsmodell (oder den Bindungskontext). Dies bedeutet, dass die *gesamte* Anwendungszustand Neuladen, einschließlich der eingefügten Abhängigkeiten immer beibehalten.

## <a name="live-reload-server"></a>Live neu laden-Server

In Szenarien, die eine Verbindung aus der ausgeführten app auf Ihrem Computer (durch Verwendung `localhost` oder `127.0.0.1` in **Tools > Optionen > Xamarin > Live Reload**) ist nicht möglich (z. B. Firewalls, verschiedenen Netzwerken) Sie können einen Remoteserver stattdessen konfigurieren, die die IDE und die app Herstellen einer Verbindung mit.

Live neu laden, verwendet die Standarddatei [MQTT-Protokoll](http://mqtt.org/) zum Austauschen von Nachrichten und können daher kommunizieren mit [-Servern von Drittanbietern](https://github.com/mqtt/mqtt.github.io/wiki/servers). Es gibt sogar [öffentlichen Servern](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (auch bekannt als *Broker*) verfügbar, die Sie verwenden können. Live Reload mit getestet wurde `broker.hivemq.com` und `iot.eclipse.org` Hostnamen als auch von bereitgestellten Dienste [www.cloudmqtt.com](https://www.cloudmqtt.com) und [www.cloudamqp.com](https://www.cloudamqp.com). Sie können auch Ihre eigenen MQTT-Server in der Cloud bereitstellen, z. B. [HiveMQ in Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Sie können einen beliebigen Port konfigurieren, aber es ist üblich, den Standardport für die 1883 für Remoteserver verwenden. Live Reload-Nachrichten verwenden die sichere End-to-End-AES symmetrische Verschlüsselung, daher ist es sicher ist, mit dem Remoteserver herstellen. Standardmäßig werden sowohl der Verschlüsselungsschlüssel und den Initialisierungsvektor (IV) für jede Visual Studio-Sitzung neu generiert.

Ist Sie möglicherweise die einfachste Möglichkeit zum Installieren der [Mosquitto](https://mosquitto.org) Server auf eine leere Ubuntu-VM in Azure:

1. Erstellen einer neuen Ubuntu-Server-VM im Azure-Portal
2. Fügen Sie eine neue Regel für eingehenden Port für 1883 (Standardport MQTT) in der Registerkarte "Netzwerke" hinzu.
3. Öffnen der [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash-Modus)
4. Geben Sie `ssh [USERNAME]@[PUBLIC_IP]` mithilfe des Benutzernamens, die Sie in 1 ausgewählt haben) und die öffentliche IP-Adresse in Ihrem VM-Seite "Übersicht" angezeigt
5. Führen Sie `sudo apt-get install mosquitto`, das Kennwort, das Sie ausgewählt, in 1 haben eingegeben)

Jetzt können Sie diese IP-Adresse verwenden, für die Verbindung mit Ihrem eigenen MQTT-Server.
