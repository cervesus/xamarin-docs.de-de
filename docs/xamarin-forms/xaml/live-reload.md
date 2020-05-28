---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
robots: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b67594e2675c774512f3bf64f2e91ef10dbff444
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134211"
---
# <a name="xamarin-live-reload-preview"></a>Xamarin Live Neuladen (Vorschau)

> [!NOTE]
> Die Vorschauversion von xamarin Live Neuladen wurde beendet, und wir möchten für alle Ihre Meinung und Ihre Kommentare danken. 
>
> Wenn Sie XAML bearbeiten möchten, während Ihre APP ausgeführt wird, verwenden Sie [XAML Xamarin.Forms Hot Upload for ](~/xamarin-forms/xaml/hot-reload.md).
>

Mit xamarin Live Neuladen können Sie **Änderungen an Ihrem XAML-Code vornehmen und diese wiedergeben, ohne dass eine andere Kompilierung und Bereitstellung erforderlich ist**. Alle Änderungen, die an Ihrem XAML-Code vorgenommen wurden, werden bei der Speicherung erneut bereitgestellt und auf dem Bereitstellungs Ziel reflektiert.

## <a name="requirements"></a>Anforderungen

* [Visual Studio 2017 Version 15,7 oder höher](https://visualstudio.microsoft.com/vs/) mit der Arbeitsauslastung für die **Mobile-Entwicklung mit .net** .
* [ Xamarin.Forms 3.0.0 oder höher](https://www.nuget.org/packages/Xamarin.Forms/).

## <a name="getting-started"></a>Erste Schritte
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Installieren Sie xamarin Live-Neuladen aus dem Visual Studio Marketplace

Xamarin Live-Neuladen wird über das Visual Studio Marketplace verteilt. Um die Erweiterung zu installieren, besuchen Sie die [Seite xamarin Live Upload auf der Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) -Website, und klicken Sie auf **herunterladen**.

Öffnen Sie die vsix-Datei, die heruntergeladen wurde, und klicken Sie auf **Installieren**.

![Visual Studio-Installer xamarin Live Reload-Bestätigung](images/LiveReloadVSIXInstall.png)

Alternativ dazu können Sie im Dialogfeld " **Erweiterungen und Updates** " in Visual Studio auf der Registerkarte " **Online** " danach suchen.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurieren der APP für die Verwendung von Live-Neuladen

Das Hinzufügen von Live Neuladen zu vorhandenen mobilen apps kann in drei Schritten ausgeführt werden:

1. Stellen Sie sicher, dass alle Projekte aktualisiert werden, sodass Sie [ Xamarin.Forms 3.0.0 oder höher](https://www.nuget.org/packages/Xamarin.Forms/) oder höher verwenden.

2. Fügen Sie das **xamarin. livereload** -nuget-Paket hinzu:

    a. **.NET Standard** – installieren Sie den nuget-Code für **xamarin. livereload** in Ihrer .NET Standard 2,0-Bibliothek. Dies muss nicht in ihren Platt Form Projekten installiert werden. Stellen Sie sicher, dass die **Paketquelle** auf **alle**festgelegt ist.
    
    b. Frei **gegebene Projekte** – installieren Sie den nuget-Code für **xamarin. livereload** in alle Platt Form Projekte (z. b. Android, Ios, UWP usw.). Stellen Sie sicher, dass die **Paketquelle** auf **alle**festgelegt ist.

    [![Hinzufügen von xamarin Live Neuladen nuget mit dem nuget-Paket-Manager](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Fügen Sie `LiveReload.Init();` dem Konstruktor in der- `Application` Klasse hinzu, wie im folgenden Code Ausschnitt gezeigt:

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

### <a name="3-start-live-reloading"></a>3. Starten des Live erneuten Ladens

Kompilieren und Bereitstellen der Anwendung. Nachdem die APP bereitgestellt wurde, öffnen Sie eine XAML-Datei, nehmen einige Änderungen vor und speichern die Datei. Die Änderungen werden auf dem Bereitstellungs Ziel erneut bereitgestellt.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Das Live-Neuladen funktioniert mit Änderungen an jeder beliebigen XAML-Datei. Änderungen an c# oder das Hinzufügen/Entfernen von nuget-Paketen erfordern, dass ein neuer Build und eine Bereitstellung wirksam werden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Ist xamarin Live Neuladen auf Visual Studio für Mac verfügbar? 

Nein, Vorschauversion von xamarin Live Reload ist nur für Visual Studio 2017 verfügbar.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Funktioniert dies mit allen Bibliotheken, z. b. Prism? 

Da Ihre APP kompiliert ist, funktioniert Live Neuladen mit allen Bibliotheken, z. b. Prism, und Steuer Bibliotheken von Drittanbietern, wie z. b. Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity und anderen Steuerungs Anbietern.

### <a name="what-changes-does-live-reload-redeploy"></a>Welche Änderungen werden bei der erneuten Bereitstellung von Live Neuladen 

Beim Live Neuladen werden nur Änderungen an XAML oder CSS angewendet. Wenn Sie Änderungen an einer c#-Datei vornehmen, ist eine Neukompilierung erforderlich. 

### <a name="what-platforms-are-supported"></a>Welche Plattformen werden unterstützt? 

Live Neuladen funktioniert auf allen Plattformen, die von unterstützt Xamarin.Forms werden, einschließlich Android, IOS und UWP.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Funktioniert dies für Emulatoren, Simulatoren und physische Geräte? 

Ja, Live-Neuladen funktioniert mit allen gültigen Bereitstellungs Zielen, einschließlich Android-Emulatoren, IOS-Simulatoren und physischen Geräten. Für die Bereitstellung auf einem Gerät müssen sich das Gerät und der Computer im gleichen WLAN befinden.

### <a name="does-this-work-with-corporate-networks"></a>Funktioniert dies mit Unternehmensnetzwerken?

Wenn Sie einen Android-Emulator oder einen IOS-Simulator Debuggen, verwendet Live Neuladen localhost für die Kommunikation. Wenn Sie auf einem Gerät bereitstellen möchten, müssen sich das Gerät und der Computer im gleichen WLAN befinden. In Szenarios, in denen dies nicht möglich ist, können Sie einen [eigenen Live-Neuladen Server konfigurieren](#live-reload-server), mit dem Sie unabhängig von den Einstellungen für die Netzwerk Konnektivität ein Live Ladevorgang durchführen können.

### <a name="does-it-require-debugging-the-app"></a>Muss die APP debuggt werden? 

Nein. Tatsächlich können Sie sogar alle Ihre unterstützten Anwendungs Ziele (Android, IOS und UWP) auf einer beliebigen Anzahl von Geräten oder Simulatoren bzw. Emulatoren starten und alle Updates gleichzeitig durch sehen. 

## <a name="limitations"></a>Einschränkungen

* Das erneute Laden von XAML wird nur unterstützt.
* Der Benutzeroberflächen Zustand kann zwischen der erneuten Bereitstellung nicht beibehalten werden, es sei denn, Sie verwenden MVVM.

## <a name="known-issues"></a>Bekannte Probleme

* Wird nur in Visual Studio unterstützt.
* Die Verknüpfung muss so festgelegt werden, dass **Framework-SDI** **nicht** verknüpft oder verknüpft werden. 
* Beim erneuten Laden von App-weiten Ressourcen (z.b. **app. XAML** oder freigegebene Ressourcen Wörterbücher) wird die APP-Navigation zurückgesetzt. 
* Zum erneuten Laden von contentview muss die enthaltende Seite erneut geladen werden.
* Elemente, die AutomationId enthalten, können einen erneuten Ladefehler verursachen.
* Das Bearbeiten von XAML beim Debuggen von UWP kann zu einem Lauf Zeit Absturz führen Problem Umgehung: Verwenden Sie " **Starten ohne Debugging" (STRG + F5)** anstelle von " **Debuggen starten" (F5)**.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="error-codes"></a>Fehlercodes

* **XLR001**: *das aktuelle Projekt verweist auf die xamarin. livereload-Version des nuget-Pakets "[Version]", aber die xamarin Live-Reaktivierungs Erweiterung erfordert Version "[Version]".*

  Das nuget-Paket und die Visual Studio-Erweiterung müssen genau übereinstimmen, um die schnelle iterierung und Weiterentwicklung der Funktion zum erneuten Laden von Funktionen zu ermöglichen. Aktualisieren Sie das nuget-Paket auf dieselbe Version der Erweiterung, die Sie installiert haben.

* **XLR002**: für *Live Neuladen ist mindestens die Eigenschaft "mqtthostname" erforderlich, wenn die Erstellung über die Befehlszeile durchführen wird. Legen Sie alternativ "enablelivereload" auf "false" fest, um die Funktion zu deaktivieren.*

  Die Eigenschaften, die für das Live Neuladen erforderlich sind, sind nicht verfügbar, wenn Sie über die Befehlszeile (oder in Continuous Integration) einen Buildvorgang durchführen, und müssen daher explizit 

* **XLR003**: *das nuget-Paket für Live Neuladen erfordert die Installation der xamarin Live Upload Visual Studio-Erweiterung.*

  Es wurde versucht, ein Projekt zu erstellen, das auf das nuget-Paket zum Live Neuladen verweist, aber die visuelle Erweiterung ist nicht installiert  

* *Ausnahme beim Laden von Assemblys: System. IO. File otfoundexception: die Assembly "xamarin. Live. Reload, Version = 0.3.27.0, Culture = neutral, PublicKeyToken =" konnte nicht geladen werden.*

  Das Host Projekt sollte anstelle von verwendet werden. `PackageReference``packages.config`

### <a name="app-doesnt-connect"></a>App stellt keine Verbindung her

Wenn die Anwendung erstellt wird, werden die Informationen aus **Tools > Optionen > xamarin > Live Neuladen** (Hostname, Port und Verschlüsselungsschlüssel) in die APP eingebettet, sodass bei Ausführung von `LiveReload.Init()` keine Kopplung oder Konfiguration erforderlich ist, damit die Verbindung erfolgreich hergestellt werden kann.

Abgesehen von den normalen Netzwerkproblemen (Firewall, Gerät in einem anderen Netzwerk) ist der Hauptgrund, warum die APP möglicherweise nicht erfolgreich eine Verbindung mit der IDE herstellt, weil die Konfiguration von der Konfiguration in Visual Studio abweicht. Dies kann bei folgenden Aktionen vorkommen:

* Die APP wurde auf einem anderen Computer kompiliert.
* Die APP wurde in einer anderen Visual Studio-Sitzung kompiliert und bereitgestellt, und die **Automatische Generierung von Verschlüsselungsschlüsseln** ist aktiviert (Standardeinstellung) in Extras **> Optionen > xamarin > Live Neuladen**.
* Die Visual Studio-Einstellungen wurden geändert (d. h. Hostname, Port oder Verschlüsselungsschlüssel), aber die APP wurde nicht erneut erstellt und bereitgestellt.

Diese Fälle werden alle gelöst, indem die APP neu aufgebaut und bereitgestellt wird.

### <a name="uninstalling-preview-1"></a>Vorschauversion 1 wird deinstalliert.

Wenn Sie über eine ältere Vorschauversion verfügen und Probleme beim Deinstallieren auftreten, führen Sie die folgenden Schritte aus:

1. Löschen Sie den Ordner " **c:\Programme (x86) \Microsoft Visual studio\preview\enterprise\common7\ide\extensions\xamarin\livereload** " (Hinweis: Ersetzen Sie "Enterprise" durch die installierte Edition, und "Preview" mit "2017", wenn Sie auf ein stabiles vs installiert haben).
2. Öffnen Sie eine **Developer-Eingabeaufforderung** für Visual Studio, und führen Sie aus `devenv /updateconfiguration` . 

## <a name="tips--tricks"></a>Tipps & Tricks

* Solange sich die Einstellungen für das aktive Neuladen nicht ändern (einschließlich der Verschlüsselungsschlüssel, z. b. Wenn Sie die **Automatische Generierung von Verschlüsselungsschlüsseln**deaktivieren) und Sie auf demselben Computer erstellen, müssen Sie die APP nach der erstmaligen Bereitstellung nicht erstellen und bereitstellen, es sei denn, Sie ändern Code oder Abhängigkeiten. Sie können eine zuvor bereitgestellte App einfach starten und eine Verbindung mit dem zuletzt verwendeten Host herstellen.

* Es gibt keine Beschränkung für die Anzahl der Geräte, die Sie mit derselben Visual Studio-Sitzung verbinden können. Sie können die app in beliebig vielen Geräten/Simulatoren bereitstellen und starten, um zu sehen, wie das Live reladen für alle gleichzeitig funktioniert.

* Beim Neuladen wird nur der Benutzeroberflächen Teil der APP neu geladen. die Seiten werden jedoch *nicht* neu erstellt, weder das Ansichts Modell noch der Bindungs Kontext. Dies bedeutet, dass der *gesamte* App-Zustand immer über Reloads hinweg beibehalten wird, einschließlich der injizierten Abhängigkeiten.

## <a name="live-reload-server"></a>Live neu laden des Servers

In Szenarien, in denen eine Verbindung zwischen der laufenden app und dem Computer (wie durch die Verwendung von `localhost` oder `127.0.0.1` in **Tools > Optionen > xamarin > Live Neuladen**) nicht möglich ist (d. h. Firewalls, unterschiedliche Netzwerke), können Sie stattdessen einen Remote Server konfigurieren, auf den sowohl die IDE als auch die APP angleichen werden.

Live-Neuladen verwendet das standardmäßige [mqtt-Protokoll](https://mqtt.org/) zum Austauschen von Nachrichten und kann daher mit [Servern von Drittanbietern](https://github.com/mqtt/mqtt.github.io/wiki/servers)kommunizieren. Es stehen sogar [öffentliche Server](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (auch als *Broker*bezeichnet) zur Verfügung, die Sie verwenden können. Das Live Neuladen wurde mit `broker.hivemq.com` -und- `iot.eclipse.org` Hostnamen sowie den von [www.cloudmqtt.com](https://www.cloudmqtt.com) und [www.cloudamqp.com](https://www.cloudamqp.com)bereitgestellten Diensten getestet. Sie können auch einen eigenen mqtt-Server in der Cloud bereitstellen, z. b. [hivemq in Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Sie können einen beliebigen Port konfigurieren, es ist jedoch üblich, den Standardport 1883 für Remote Server zu verwenden. Nachrichten nach dem Live erneuten Laden verwenden eine sichere End-to-End-AES-symmetrische Verschlüsselung, sodass es sicher ist, eine Verbindung mit Remote Servern herzustellen. Standardmäßig werden sowohl der Verschlüsselungsschlüssel als auch der Initialisierungs Vektor (IV) für jede Visual Studio-Sitzung neu generiert.

Wahrscheinlich ist es am einfachsten, [den Server mit dem Server auf einer](https://mosquitto.org) leeren Ubuntu-VM in Azure zu installieren:

1. Erstellen einer neuen Ubuntu-Server-VM im Azure-Portal
2. Hinzufügen einer neuen eingehenden Port Regel für 1883 (Standard-mqtt-Port) auf der Registerkarte "Netzwerke"
3. Öffnen Sie die [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash-Modus).
4. Geben `ssh [USERNAME]@[PUBLIC_IP]` Sie den Benutzernamen ein, den Sie in 1 ausgewählt haben, und die öffentliche IP-Adresse auf der Übersichtsseite des virtuellen Computers
5. Ausführen `sudo apt-get install mosquitto` und geben Sie das Kennwort ein, das Sie in 1 ausgewählt haben

Nun können Sie diese IP-Adresse verwenden, um eine Verbindung mit Ihrem eigenen mqtt-Server herzustellen.
