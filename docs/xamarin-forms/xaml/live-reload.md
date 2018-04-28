---
title: Live neu laden
description: Finden Sie Änderungen an XAML-Code live, wiedergegeben werden, ohne eine andere kompilieren und bereitstellen.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 04/23/2018
ms.openlocfilehash: 11e876207285689b230bb2fada3a4c836443e360
ms.sourcegitcommit: a69439ad4c9fd0abe759143687d3b23582573d90
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="xamarin-live-reload"></a>Xamarin Live neu laden

![Vorschau](~/media/shared/preview.png)

Xamarin Live Reload ermöglicht es Ihnen, **nehmen Sie Änderungen an XAML-Code und finden Sie unter diese live, ohne dass eine andere kompilieren und Bereitstellen von**. Alle Änderungen an XAML-Code werden neu bereitgestellt werden auf Speichern, und Ihr Bereitstellungsziel berücksichtigt.

Da es sich bei Ihrer app kompiliert wird, bei Verwendung von Live erneut laden, funktioniert sie mit allen Bibliotheken und Drittanbieter-Steuerelemente. Live-Reload funktioniert auf allen Plattformen, die xamarin.Forms unterstützt, einschließlich Android, iOS und universelle Windows-Plattform, und funktioniert auf alle gültigen Bereitstellungsziele, einschließlich Simulatoren, Emulatoren und Geräten.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live-Reload ist derzeit nur in Visual Studio 2017 verfügbar.

## <a name="requirements"></a>Anforderungen

* [Visual Studio 2017 15.7 Preview 4](https://www.visualstudio.com/vs/preview/) oder höher mit der **Mobile Entwicklung mit .NET** arbeitsauslastung.
* [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) oder höher.

## <a name="getting-started"></a>Erste Schritte
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Installieren Sie Xamarin Live zum erneuten Laden von Visual Studio Marketplace.

Live-Laden von Xamarin wird über die Visual Studio Marketplace verteilt. Um die Erweiterung installieren möchten, besuchen Sie die [Xamarin Live zum erneuten Laden der Seite in Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) Website und klicken Sie auf **herunterladen**.

Öffnen Sie die VSIX, die heruntergeladen wird, und klicken Sie auf **installieren**.

![Installer für Visual Studio Xamarin Live Reload-Bestätigung](images/LiveReloadVSIXInstall.png)

> Alternativ können Sie dafür in Suchen der **Online** Registerkarte der **Erweiterungen und Updates** Dialogfeld innerhalb von Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurieren Sie Ihre app zur Verwendung von Live neu laden.

Hinzufügen von Live zum erneuten Laden zu vorhandenen mobilen apps kann in drei Schritten ausgeführt werden:

1. Stellen Sie sicher, alle Projekte werden aktualisiert, um verwenden [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) oder höher.
2. Installieren der **Xamarin.LiveReload** NuGet in Ihre .NET Standard 2.0-Bibliothek. Dies muss nicht in Ihren plattformprojekten installiert werden. Sicherstellen, dass die **Paketquelle** festgelegt ist, um **alle**.

![Xamarin Live Reload NuGet mit NuGet-Paket-Manager hinzufügen](images/addlivereloadnuget.png)

3. Hinzufügen `LiveReload.Init();` an den Konstruktor in der `Application` Klasse, wie im folgenden Codeausschnitt gezeigt:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Starten Sie die live neu laden.

Kompilieren Sie und Bereitstellen Sie Ihrer Anwendung. Sobald die app bereitgestellt ist, öffnen Sie eine XAML-Datei, nehmen Sie einige Änderungen vor und speichern Sie die Datei. Die Änderungen werden an das Bereitstellungsziel entweder erneut bereitgestellt.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live-Reload funktioniert mit Änderungen an alle XAML-Datei. Änderungen an der C#- oder das Hinzufügen/Entfernen von NuGet-Pakete erfordert einen neuen Build und bereitstellen, um wirksam wird.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen (FAQs) 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Steht Live Laden von Xamarin für Visual Studio für Mac? 

Die anfängliche Preview-Version von Xamarin Live zum erneuten Laden ist nur für Visual Studio-2017 verfügbar. Unterstützung für Visual Studio für Mac ist für eine zukünftige Version geplant.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Werden dies für alle Bibliotheken, z. B. Prism verwendet? 

Da Ihrer app kompiliert wird, funktioniert Live zum erneuten Laden mit alle Bibliotheken, z. B. Prism und -Bibliotheken der Drittanbieter-Steuerelement, z. B. Telerik, Infragistics tätig, Syncfusion, ArcGIS, GrapeCity und anderen Herstellern Steuerelement aus.

### <a name="what-changes-does-live-reload-redeploy"></a>Welche Änderungen bereitstellen Live zum erneuten Laden erneut? 

Live-Reload gilt nur Änderungen an XAML. Wenn Sie eine C#-Datei ändern, wird eine Neukompilierung erforderlich sein. Unterstützung für Neuladen von c# ist für eine zukünftige Version geplant.

### <a name="what-platforms-are-supported"></a>Welche Plattformen unterstützt werden? 

Live Reload funktioniert auf jeder Plattform, indem Sie Xamarin.Forms, einschließlich Android, iOS und universelle Windows-Plattform unterstützt.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Funktioniert dies für Emulatoren und Simulatoren physische Geräte? 

Ja, funktioniert Live erneut laden, mit allen gültigen Bereitstellungsziele verwenden, einschließlich Android-Emulatoren, iOS-Simulatoren und physische Geräte. Bereitstellung auf einem Gerät erfordert, dass die Geräte und Computer im selben Wi-Fi-Netzwerk.

### <a name="does-this-work-with-corporate-networks"></a>Ist dies mit Unternehmensnetzwerken funktionsfähig?

Wenn Sie eine Android-Emulator oder iOS-Simulator Debuggen, wird "localhost" Live zum erneuten Laden zur Kommunikation verwendet. Wenn Sie auf einem Gerät bereitstellen möchten, müssen die Geräte und Computer im selben Wi-Fi-Netzwerk sein. In Szenarien, in denen dies nicht möglich ist, können Sie [konfigurieren Sie einen eigenen Server Live zum erneuten Laden](#live-reload-server), dadurch werden Sie zum erneuten Laden Live, unabhängig von den Einstellungen für die Netzwerkkonnektivität.

### <a name="does-it-require-debugging-the-app"></a>Ist es erforderlich, das Debuggen der Anwendung? 

Nein. Tatsächlich können Sie sogar starten Sie alle Ihre Anwendung unterstützten Ziele (Android, iOS und universelle Windows-Plattform) auf eine beliebige Anzahl der Geräte oder Simulatoren-Emulatoren und werden alle auf einmal aktualisieren. 

## <a name="limitations"></a>Einschränkungen

* Nur XAML-Neuladen wird unterstützt.
* Nur unterstützt in Visual Studio.
* Funktioniert nur mit .NET Standardbibliotheken.
* CSS-Stylesheets werden nicht unterstützt.
* Benutzeroberflächenzustand möglicherweise zwischen erneut bereitstellen, es sei denn, die mit MVVM nicht beibehalten.

## <a name="live-reload-server"></a>Live-Server neu laden

In Szenarien, die eine Verbindung aus der ausgeführten app auf Ihrem Computer (mit angegebene `localhost` oder `127.0.0.1` in **Tools > Optionen > Xamarin > Live zum erneuten Laden**) ist nicht möglich (z. B. Firewalls, unterschiedlichen Netzwerken), Sie können einen Remoteserver stattdessen konfigurieren, wird die IDE und die app zu verbinden.

Live zum erneuten Laden verwendet den Standard [MQTT Protokoll](http://mqtt.org/) zu tauschen Nachrichten aus, und kann daher mit kommunizieren [Servern von Drittanbietern](https://github.com/mqtt/mqtt.github.io/wiki/servers). Es gibt auch [öffentlichen Servern](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (auch bekannt als *Brokern*) verfügbar, die Sie verwenden können. Live zum erneuten Laden wurde mit getestet `broker.hivemq.com` und `iot.eclipse.org` Hostnamen als auch die Dienste von [www.cloudmqtt.com](https://www.cloudmqtt.com) und [www.cloudamqp.com](https://www.cloudamqp.com). Sie können auch einen eigenen MQTT-Server in der Cloud bereitstellen, wie z. B. [HiveMQ in Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud) oder [Kaninchen MQ auf AWS](http://www.rabbitmq.com/ec2.html). 

Sie können einen beliebigen Port konfigurieren, aber es ist üblich, den 1883-Standardport für Remoteserver verwenden. Live Reload-Nachrichten verwenden starke End-to-End-AES symmetrische Verschlüsselung, sodass für die Verbindung zu Remoteservern werden kann. Standardmäßig werden die Verschlüsselungsschlüssel und den Initialisierungsvektor (IV) auf jedes Visual Studio-Sitzung neu generiert.

## <a name="troubleshooting"></a>Problembehandlung

Als die Anwendung erstellt wird, wird die Informationen aus **Tools > Optionen > Xamarin > Reload Live** (Name, Port und Verschlüsselung Hostschlüssel) werden daher in der app eingebettet, die bei `LiveReload.Init();` ausgeführt wird, keine Kopplung oder Konfiguration ist erforderlich für die Verbindung erfolgreich hergestellt werden kann.

Als normale Netzwerkprobleme (Firewall, Gerät in einem anderen Netzwerk) ist die Hauptgründe dafür, dass die app keine IDE Verbindung hergestellt werden kann, da es sich bei die Konfiguration von in Visual Studio unterscheidet. Dies kann vorkommen, wenn werden:

* App wurde auf einem anderen Computer kompiliert.
* App kompiliert und in einer anderen Visual Studio-Sitzung bereitgestellt wurde und **Automatisches Generieren von Verschlüsselungsschlüsseln** ist aktiviert (Standard) **Tools > Optionen > Xamarin > Reload Live**.
* Visual Studio-Einstellungen wurden (d. h. Hostname, Port oder die Encryption-Schlüssel) geändert, aber die app wurde nicht erstellt und erneut bereitgestellt.

Diesen Fällen werden alle Ausgabeklasse erstellen und Bereitstellen der app erneut aus.

## <a name="tips--tricks"></a>Tipps und Tricks

* Solange die Live-Reload-Einstellungen nicht ändern (einschließlich der Verschlüsselungsschlüssel, z. B. Wenn Sie deaktivieren **Automatisches Generieren von Verschlüsselungsschlüsseln**) und Sie aus dem gleichen Computer erstellen, müssen Sie nicht zum Erstellen und Bereitstellen der app nach der Erstkonfiguration Stellen Sie bereit, es sei denn, Sie Code oder Abhängigkeiten ändern. Sie können nur starten, erneut eine zuvor bereitgestellte app und wird eine Verbindung mit dieser bis zum letzten Host verwendet.

* Es gibt keine Einschränkung auf wie viele Geräte, die Sie mit der gleichen Visual Studio-Sitzung verbinden können. Sie können bereitstellen und starten Sie die app in beliebig viele Geräte-Simulatoren nach Bedarf, das erneute Laden live arbeiten auf allen von ihnen gleichzeitig anzuzeigen.

* Live Reload Laden nur den Benutzeroberflächenteil der app, aber dies der Fall ist *nicht* Ihren Seiten neu zu erstellen, keins ist es ersetzen, Ihre Ansichtsmodell (oder Bindungskontext). Dies bedeutet, dass die *gesamte* app-Status wird in den Neuladen, einschließlich der eingefügten Abhängigkeiten immer beibehalten.
