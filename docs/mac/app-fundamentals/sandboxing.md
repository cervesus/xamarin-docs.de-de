---
title: Sandkasten einer Xamarin.Mac-app
description: Dieser Artikel behandelt die Sandbox einer Xamarin.Mac-Anwendung zur Freigabe zu den App Store. Hierin sind alle Elemente, die in einer Sandbox, z. B. Containerverzeichnisse, Berechtigungen, Benutzer bestimmte Berechtigungen, die Trennung von Berechtigungen und Erzwingung von Kernel aufgenommen.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6bf2f63e944e178d80f76fe363ef24410ff052ce
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61236922"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Sandkasten einer Xamarin.Mac-app

_Dieser Artikel behandelt die Sandbox einer Xamarin.Mac-Anwendung zur Freigabe zu den App Store. Hierin sind alle Elemente, die in einer Sandbox, z. B. Containerverzeichnisse, Berechtigungen, Benutzer bestimmte Berechtigungen, die Trennung von Berechtigungen und Erzwingung von Kernel aufgenommen._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung müssen Sie auch die Möglichkeit, Sandbox eine Anwendung, wie Sie bei der Arbeit mit Objective-C oder Swift.

[![Ein Beispiel für die ausgeführte app](sandboxing-images/intro01.png "ein Beispiel für die ausgeführte app")](sandboxing-images/intro01-large.png#lightbox)

In diesem Artikel behandeln wir die Grundlagen der Arbeit mit Sandkasten in einer Xamarin.Mac-Anwendung und alle Elemente, die in der Sandbox eingehen: Containerverzeichnisse, Berechtigungen, Benutzer bestimmte Berechtigungen, die Trennung von Berechtigungen und Erzwingung von Kernel. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.

## <a name="about-the-app-sandbox"></a>Informationen zu den App-Sandbox

Die App-Sandbox bietet starken Schutz vor Beschädigungen, die durch eine bösartige Anwendung, die auf einem Mac ausgeführt wird, indem der Zugriff eingeschränkt, die eine Anwendung auf Systemressourcen verursacht werden kann.

Eine nicht-Sandbox-Anwendung kann die vollständigen Berechtigungen des Benutzers, der die app ausgeführt wird und Zugriff auf oder nichts, die der Benutzer kann. Wenn die app enthält Sicherheitslücken (oder ein beliebiges Framework, die es verwendet wird), kann potenziell ein Hacker diese Schwächen ausnutzen und verwenden Sie die app Kontrolle über den Mac, der es ausgeführt wird.

Durch das Beschränken des Zugriffs auf Ressourcen auf einer Basis pro Anwendung, bietet eine Sandbox-app eine Verteidigungslinie gegen Diebstahl, Beschädigung oder böswilliger Absicht Teil einer Anwendung, die auf dem Computer des Benutzers ausgeführt.

Die App-Sandbox ist eine Access-steuerelementtechnologie MacOS (auf der Kernelebene erzwungen), die eine zwei-Strategie bietet integriert:

1. Die App-Sandbox kann der Entwickler beschreiben _wie_ mit dem Betriebssystem und auf diese Weise wird die Anwendung interagieren, wird sie nur die Zugriffsrechte, die zur Erledigung der Aufgabe erforderlich "und" nicht mehr gewährt.
2. Die App-Sandbox kann es sich um die Benutzer nahtlos gewähren weiteren Zugriff auf das System über das Öffnen und speichern die Dialogfelder Drag & drop-Vorgänge und andere, häufige Benutzerinteraktionen.

### <a name="preparing-to-implement-the-app-sandbox"></a>Vorbereiten der Implementierung der App-Sandbox von

Die Elemente der App-Sandbox, die in diesem Artikel ausführlich erläutert werden, lauten wie folgt:

- Containerverzeichnisse
- Berechtigungen
- Benutzer bestimmte Berechtigungen
- Trennung von Berechtigungen
- Kernel-Erzwingung

Nachdem Sie diese Details verstehen, müssen Sie einen Plan für den Umstieg auf die App-Sandbox in Ihre Xamarin.Mac-Anwendung erstellen können.

Zunächst müssen Sie bestimmen, ob Ihre Anwendung ein guter Kandidat für Sandkasten ist (die meisten Anwendungen sind). Als Nächstes müssen Sie zum Beheben von Inkompatibilitäten API und zu bestimmen, welche Elemente der Sandbox-App benötigen Sie. Sehen Sie sich schließlich zur Verwendung der Trennung von Berechtigungen für die Sie Defense-Stufe der Anwendung maximieren.

Wenn die App-Sandbox zu übernehmen, werden einige Speicherorte im Dateisystem, die Ihre Anwendung verwendet unterscheiden. Insbesondere müssen Ihre Anwendung ein Container-Verzeichnis, die für die Unterstützung von Anwendungsdateien, Datenbanken, Caches und anderen Dateien, die nicht Benutzerdokumente sind verwendet werden. Bieten Unterstützung, um diese Arten von Dateien von ihren alten Speicherorten in den Container zu migrieren, MacOS und Xcode.

## <a name="sandboxing-quick-start"></a>Sandkasten-Schnellstart

In diesem Abschnitt erstellen wir eine einfache Xamarin.Mac-app, die eine Webansicht verwendet (Dies ist eine Netzwerkverbindung, die unter Sandbox beschränkt ist, es sei denn, Sie ausdrücklich angefordert erfordert) als Beispiel für erste Schritte mit der Sandbox-App.

Wir verifizieren, dass die Anwendung tatsächlich Sandbox und erfahren, wie Sie behandeln und Beheben von häufigen App-Sandbox-Fehlern.

### <a name="creating-the-xamarinmac-project"></a>Erstellen das Xamarin.Mac-Projekt

Wir gehen Sie zum Erstellen von unserem Beispielprojekt:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf die **neue Projektmappe...** einen Fehler.
2. Von der **neues Projekt** wählen Sie im Dialogfeld **Mac** > **App** > **Cocoa-App**: 

    [![Erstellen einer neuen Cocoa-App](sandboxing-images/sample01.png "Erstellen einer neuen Cocoa-App")](sandboxing-images/sample01-large.png#lightbox)
3. Klicken Sie auf die **Weiter** Geben Sie eine Schaltfläche `MacSandbox` für das Projekt und klicken Sie auf die **erstellen** Schaltfläche: 

    [![Geben Sie den app-Namen](sandboxing-images/sample02.png "der app-Name eingeben")](sandboxing-images/sample02-large.png#lightbox)
4. In der **Lösungspad**, doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie zur Bearbeitung in Xcode geöffnet: 

    [![Bearbeiten die Haupt-Storyboard](sandboxing-images/sample03.png "bearbeiten die Haupt-Storyboard")](sandboxing-images/sample03-large.png#lightbox)
5. Ziehen Sie eine **Webansicht** in das Fenster, die Größe der Inhaltsbereich und legen sie zum Vergrößern und Verkleinern mit dem Fenster: 

    [![Hinzufügen einer Webansicht](sandboxing-images/sample04.png "Hinzufügen einer Webansicht")](sandboxing-images/sample04-large.png#lightbox)
6. Erstellen ein outlets für die Webansicht namens `webView`: 

    [![Erstellen eine neue Outlet](sandboxing-images/sample05.png "erstellen einen neuen outlets")](sandboxing-images/sample05-large.png#lightbox)
7. Zurück zu Visual Studio für Mac, und doppelklicken Sie auf die **ViewController.cs** Datei die **Lösungspad** um ihn zur Bearbeitung zu öffnen.
8. Fügen Sie die folgenden using-Anweisung: `using WebKit;`
9. Stellen Sie die `ViewDidLoad` Methode sehen wie folgt: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Speichern Sie die Änderungen.

Führen Sie die Anwendung aus, und stellen Sie sicher, dass die Apple-Website wie folgt im Fenster angezeigt wird:

[![Zeigt eine Beispiel-app ausführen](sandboxing-images/sample06.png "zeigt eine Beispiel-app ausführen")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Signieren und Bereitstellen der app

Bevor wir die App-Sandbox aktivieren können, müssen wir zuerst bereitstellen und Signieren die Xamarin.Mac-Anwendung.

Lassen Sie die folgenden Schritte ausführen:

1. Melden Sie sich im Apple Developer Portal: 

    [![Anmelden bei Apple-Entwicklerportal](sandboxing-images/sign01.png "Anmeldung bei Apple-Entwicklerportal")](sandboxing-images/sign01-large.png#lightbox)
2. Wählen Sie **Zertifikate, Bezeichner & Profile**: 

    [![Auswählen von „Zertifikate, Bezeichner und Profile“](sandboxing-images/sign02.png "Selecting Certificates, Identifiers & Profiles")](sandboxing-images/sign02-large.png#lightbox)
3. Klicken Sie unter **Mac-Apps**Option **Bezeichner**: 

    [![Auswählen von Bezeichnern](sandboxing-images/sign03.png "Bezeichner auswählen")](sandboxing-images/sign03-large.png#lightbox)
4. Erstellen Sie eine neue ID für die Anwendung: 

    [![Erstellen einer neuen App-ID](sandboxing-images/sign04.png "erstellen eine neue App-ID")](sandboxing-images/sign04-large.png#lightbox)
5. Klicken Sie unter **Bereitstellungsprofile**Option **Entwicklung**: 

    [![Wählen die Entwicklung](sandboxing-images/sign05.png "Entwicklung auswählen")](sandboxing-images/sign05-large.png#lightbox)
6. Ein neues Profil erstellen, und wählen Sie **Mac-App-Entwicklung**: 

    [![Erstellen eines neuen Profils](sandboxing-images/sign06.png "Erstellen eines neuen Profils")](sandboxing-images/sign06-large.png#lightbox)
7. Wählen Sie die App-ID, die wir zuvor erstellt haben: 

    [![Auswählen der App-ID](sandboxing-images/sign07.png "Auswählen der App-ID")](sandboxing-images/sign07-large.png#lightbox)
8. Wählen Sie die Entwickler für dieses Profil aus: 

    [![Hinzufügen von Entwicklern](sandboxing-images/sign08.png "hinzufügen-Entwickler")](sandboxing-images/sign08-large.png#lightbox)
9. Wählen Sie die Computer für dieses Profil: 

    [![Auswählen der zulässigen Computer](sandboxing-images/sign09.png "Auswählen der zulässigen Computer")](sandboxing-images/sign09-large.png#lightbox)
10. Benennen Sie dem Profil: 

    [![Benennen des Profils mit einem Namen](sandboxing-images/sign10.png "vergeben eines Namens für dem Profil")](sandboxing-images/sign10-large.png#lightbox)
11. Klicken Sie auf die **Fertig** Schaltfläche.

> [!IMPORTANT]
> In einigen Fällen müssen Sie das neue Bereitstellungsprofil direkt aus dem Apple Entwicklerportal herunterladen, und doppelklicken darauf installieren. Sie sollten auch beenden und starten Sie Visual Studio für Mac neu, bevor er auf das neue Profil zugreifen kann.

Als Nächstes müssen wir die neue App-ID und das Profil auf dem Entwicklungscomputer zu laden. Lassen Sie uns wie folgt vor:

1. Starten Sie Xcode, und wählen Sie **Voreinstellungen** aus der **Xcode** Menü: 

    ![Bearbeiten von Konten in Xcode](sandboxing-images/sign11.png "Bearbeiten von Konten in Xcode")
2. Klicken Sie auf die **Details anzeigen...**  Schaltfläche: 

    ![Klicken Sie auf die Schaltfläche "Details anzeigen"](sandboxing-images/sign12.png "auf die Schaltfläche \"Details anzeigen\"")
3. Klicken Sie auf die **aktualisieren** Schaltfläche (in der unteren linken Ecke).
4. Klicken Sie auf die **Fertig** Schaltfläche.

Als Nächstes müssen wir die neue App-ID und das Bereitstellungsprofil in unserem Xamarin.Mac-Projekt auswählen. Lassen Sie uns wie folgt vor:

1. In der **Lösungspad**, doppelklicken Sie auf die **"Info.plist"** Datei, die sie für die Bearbeitung zu öffnen.
2. Sicherstellen, dass die **Bündel-ID** entspricht unserer App-ID, die wir oben erstellt haben (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Bearbeiten die Bündel-ID](sandboxing-images/sign13.png "bearbeiten die Bündel-ID")](sandboxing-images/sign13-large.png#lightbox)
3. Doppelklicken Sie dann auf die **"Entitlements.plist"** Datei, und stellen Sie sicher unsere **iCloud Schlüssel-Wert-Store** und **iCloud-Container** alle übereinstimmen, dass unsere App-ID, die wir oben erstellt haben (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Bearbeiten der Datei "Entitlements.plist"](sandboxing-images/sign17.png "Bearbeiten der Datei \"Entitlements.plist\"")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen.
4. In der **Lösungspad**, doppelklicken Sie auf die Projektdatei, um die Optionen für die Bearbeitung zu öffnen:  

    ![Editign der Projektmappe Optionen](sandboxing-images/sign14.png "Editign der Projektmappe-Optionen")
5. Wählen Sie **Mac-Signierung**, überprüfen Sie anschließend **anwendungsbündel signieren** und **Installer-Paket signieren**. Klicken Sie unter **Bereitstellungsprofil**, wählen Sie den wir oben erstellt haben: 

    ![Das Bereitstellungsprofil festlegen](sandboxing-images/sign15.png "Bereitstellungsprofil festlegen")
6. Klicken Sie auf die **Fertig** Schaltfläche.

> [!IMPORTANT]
> Sie müssen möglicherweise beenden und starten Sie Visual Studio für Mac, um sie zu erhalten, damit erkennt die neue App-ID und das Bereitstellungsprofil, die von Xcode installiert wurde.

#### <a name="troubleshooting-provisioning-issues"></a>Problembehandlung bei der Bereitstellung

An diesem Punkt sollten Sie versuchen, führen Sie die Anwendung, und stellen Sie sicher, dass alles, was signiert ist und ordnungsgemäß bereitgestellt wird. Wenn die app weiterhin wie zuvor ausgeführt wird, ist alles, was gut. Ein Fehler auftritt erhalten Sie möglicherweise ein Dialogfeld wie folgt:

[![Ein Beispiel, das Problem bereitstellungsdialogfeld](sandboxing-images/sign16.png "ein Beispiel, das Problem bereitstellungsdialogfeld")](sandboxing-images/sign16-large.png#lightbox)

Hier sind die häufigsten Ursachen für die Bereitstellung und Probleme:

- Die App-Bündel-ID stimmt nicht mit der App-ID des ausgewählten Profils überein.
- Der Entwickler-ID stimmt nicht mit der Entwickler-ID des ausgewählten Profils überein.
- Die UUID des Mac-Computers auf getestet wird, nicht als Teil des ausgewählten Profils registriert ist.

Beheben Sie bei einem Problem das Problem im Apple Developer Portal, aktualisieren Sie die Profile in Xcode aus, und führen Sie einen bereinigten Build in Visual Studio für Mac.

### <a name="enable-the-app-sandbox"></a>Aktivieren Sie die App-Sandbox

Sie aktivieren die App-Sandbox ein Kontrollkästchen in Ihren Projekten Optionen auswählen. Führen Sie folgende Schritte aus:

1. In der **Lösungspad**, doppelklicken Sie auf die **"Entitlements.plist"** Datei, die sie für die Bearbeitung zu öffnen.
2. Aktivieren Sie beide **aktivieren Sie Berechtigungen** und **aktivieren Sie die Anwendungs-Sandbox**: 

    [![Bearbeiten von Berechtigungen, und aktivieren Sandkasten](sandboxing-images/sign17.png "Berechtigungen bearbeiten, und aktivieren die Sandbox")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen.

An diesem Punkt haben Sie die App-Sandbox aktiviert, aber Sie haben nicht die erforderlichen Netzwerkzugriff für die Webansicht angegeben. Wenn Sie die Anwendung jetzt ausführen, erhalten Sie ein leeres Fenster:

[![Mit der Web-Zugriff blockiert](sandboxing-images/sample08.png "mit der Web-Zugriff blockiert")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Überprüfen, dass die app als sandkastenlösung bereitgestellt wird

Abgesehen von der Ressource blockiert gibt es drei Arten mitteilen, ob es sich bei eine Xamarin.Mac-Anwendung wurde erfolgreich Sandbox:

1. Überprüfen Sie im Finder zeigen, den Inhalt der `~/Library/Containers/` Ordner: Wenn die app Sandbox, fallen in einen Ordner mit dem Namen wie Ihre app Bündel-ID (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Öffnen die app Bundle](sandboxing-images/sample09.png "öffnen die app Bundle")](sandboxing-images/sample09-large.png#lightbox)
2. Das System erkennt die app als Sandbox im Aktivitätsmonitor überwachen:
    - Starten des Aktivitätsmonitors (unter `/Applications/Utilities`). 
    - Wählen Sie **Ansicht** > **Spalten** und sicherstellen, dass die **Sandbox** Menüelement aktiviert ist.
    - Sicherstellen, dass die Sandbox-Spalte liest `Yes` für Ihre Anwendung: 

    [![Überprüfen die app im Aktivitätsmonitor](sandboxing-images/sample10.png "überprüfen die app im Aktivitätsmonitor")](sandboxing-images/sample10-large.png#lightbox)
3. Überprüfen Sie, dass die app, die binäre als sandkastenlösung bereitgestellt wird:
    - Starten Sie die Terminal-app.
    - Navigieren Sie zu den Anwendungen `bin` Verzeichnis.
    - Dieser Befehl: `codesign -dvvv --entitlements :- executable_path` (wobei `executable_path` ist der Pfad zu Ihrer Anwendung): 

    [![Überprüfen die app in der Befehlszeile](sandboxing-images/sample11.png "überprüfen die app in der Befehlszeile")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Debuggen einer Sandbox-app

Der Debugger eine Verbindung herstellt, Xamarin.Mac-Apps über TCP, das bedeutet, dass standardmäßig beim Sandkasten, aktivieren sie für die app eine Verbindung herstellen kann, sodass, wenn Sie versuchen, die app ausführen, ohne die richtigen Berechtigungen, die aktiviert, Sie einen Fehler erhalten *"kann nicht für die Verbindung der Debugger"*. 

[![Festlegen der erforderlichen Optionen](sandboxing-images/debug01.png "Festlegen von Optionen")](sandboxing-images/debug01-large.png#lightbox)

Die **zulassen ausgehender Netzwerkverbindungen (Client)** Berechtigung ist erforderlich, damit der Debugger, aktivieren diese ermöglicht das Debuggen. Da Sie ohne Debuggen können nicht, wir haben aktualisiert die `CompileEntitlements` Ziel für `msbuild` die Berechtigungen für jede app, die als sandkastenlösung bereitgestellt, für das Debuggen wird nur builds automatisch diese Berechtigung hinzuzufügen. Releasebuilds sollten die Berechtigungen, die in der Berechtigungsdatei enthalten, die unverändert angegeben verwenden.

### <a name="resolving-an-app-sandbox-violation"></a>Auflösen von eine Verletzung der App-Sandbox

Eine Verletzung der App-Sandbox tritt auf, wenn eine Sandbox Xamarin.Mac-Anwendung hat versucht, eine Ressource zuzugreifen, die nicht explizit zugelassen werden. Beispielsweise unsere Webansicht ohne mehr verwendet werden kann, um die Apple-Website anzuzeigen.

Die häufigste Ursache Verstöße gegen die Sandbox-App tritt ein, wenn die Berechtigung Einstellungen in Visual Studio für Mac nicht die Anforderungen der Anwendung übereinstimmen. Zurück zu unserem Beispiel: die fehlenden Netzwerkverbindung erneut Berechtigungen, die Arbeit der Webansicht verhindern.

#### <a name="discovering-app-sandbox-violations"></a>Ermittlung von App-Sandbox-Verstöße

Wenn Sie vermuten, eine Verletzung der Sandbox-App in Ihre Xamarin.Mac-Anwendung ausgeführt wird, ist die schnellste Möglichkeit zum Ermitteln des Problems mithilfe der **Konsole** app.

Führen Sie folgende Schritte aus:

1. Kompilieren Sie die betreffende Anwendung, und führen Sie es in Visual Studio für Mac.
2. Öffnen der **Konsole** Anwendung (über `/Applications/Utilties/`).
3. Wählen Sie **alle Nachrichten** in der Randleiste, und geben Sie `sandbox` in der Suche: 

    [![Ein Beispiel für einen Sandkasten-Problem in der Konsole](sandboxing-images/resolve01.png "ein Beispiel für einen Sandkasten-Problem in der Konsole")](sandboxing-images/resolve01-large.png#lightbox)

Für die oben genannten Beispiel-app, sehen Sie, dass der Kernel blockiert die `network-outbound` Datenverkehr aufgrund der App-Sandbox, da wir dieses Recht nicht angefordert haben.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Beheben von Verstößen mit Berechtigungen für die App-Sandbox

Nun, wir gesehen haben, wie Sie App-Sandbox-Verstöße zu finden, sehen wir uns an, wie sie behoben werden können, indem Sie der Anwendung Berechtigungen anpassen.

Führen Sie folgende Schritte aus:

1. In der **Lösungspad**, doppelklicken Sie auf die **"Entitlements.plist"** Datei, die sie für die Bearbeitung zu öffnen.
2. Unter den **Berechtigungen** aktivieren Sie im Abschnitt der **zulassen ausgehender Netzwerkverbindungen (Client)** Kontrollkästchen: 

    [![Bearbeiten der Berechtigungen](sandboxing-images/sign17.png "Bearbeiten der Berechtigungen")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen an die Anwendung ein.

Wenn wir den oben beschriebenen, für die Beispiel-app Aktionen, klicken Sie dann zu erstellen und ausführen, wird die Webinhalte wie erwartet nun angezeigt.

## <a name="the-app-sandbox-in-depth"></a>Die detaillierte App-Sandbox

Die Mechanismen der Zugriffssteuerung bereitgestellt, die von der Sandbox-App sind einige und einfach zu verstehen. Allerdings ist die Möglichkeit, dass die App-Sandbox von jeder app übernommen werden, wird eindeutig und basierend auf den Anforderungen der app.

Erhält Ihr Möglichstes, um die verhindern, dass Ihre Xamarin.Mac-Anwendung, die von böswilligem Code ausgenutzt wird, gibt es ggf. nur eine einzelne Schwachstelle in der app (oder eine der Bibliotheken oder Frameworks sie beansprucht) Kontrolle über die app Interaktionen mit der System.

Die App-Sandbox soll zu verhindern, dass die Übernahme (oder beschränken den Schaden, den es führen kann), sodass Sie an der Anwendung vorgesehene Interaktionen mit dem System. Das System gewährt nur Zugriff auf die Ressource, die die Anwendung, die zur Erledigung der Aufgabe erforderlich ist und nicht mehr.

Beim Entwerfen der App-Sandbox, sind Sie für den schlimmsten Fall entwerfen. Wenn die Anwendung durch bösartigen Code beeinträchtigt wird, ist es für den Zugriff auf nur die Dateien und Ressourcen in der app-Sandbox beschränkt.

### <a name="entitlements-and-system-resource-access"></a>Berechtigungen und Zugriff auf die System-Ressourcen

Wie wir oben gesehen haben, wird eine Xamarin.Mac-Anwendung, die nicht mit Sandbox wurde gewährt, die Vollzugriff und den Zugriff des Benutzers, der die app ausgeführt wird. Gefährdung durch bösartigen Code kann eine nicht geschützte app als Agent für kritischere Verhalten ein, mit der eine Vielzahl reichen für Schäden verursacht potenzielle fungieren.

Aktivieren Sie die App-Sandbox, Sie entfernen alles bis auf einen minimalen Satz von Berechtigungen, die Sie dann erneut mit einer Xamarin.Mac-app-Berechtigungen müssen nur einzelne aktivieren. 

Sie ändern die Ressourcen der Anwendung App-Sandbox durch Bearbeiten der **"Entitlements.plist"** Datei überprüfen oder die Rechte erforderlich sind, aus den Dropdownfeldern Editoren auswählen:

[![Bearbeiten der Berechtigungen](sandboxing-images/sign17.png "Bearbeiten der Berechtigungen")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Containerverzeichnisse und Dateisystemzugriff

Bei Ihrer Xamarin.Mac-Anwendung über den App-Sandbox übernimmt, hat er Zugriff auf den folgenden Speicherorten:

- **Das Containerverzeichnis App** -bei der ersten Ausführung erstellt das Betriebssystem eine besondere _Containerverzeichnis_ , aller zugehörigen Ressourcen wechseln, dass nur er zugreifen kann. Die app hat vollständige Lese-/Schreibzugriff auf dieses Verzeichnis.
- **Containerverzeichnisse für App-Gruppe** -Ihrer-app Zugriff auf eine oder mehrere gewährt werden kann _Gruppencontainer_ , die in der gleichen Gruppe für apps genutzt werden.
- **Dateien mit benutzerdefinierten** -erhält die Anwendung automatisch den Zugriff auf Dateien, die explizit geöffnet oder gezogen und auf die Anwendung vom Benutzer gelöscht.
- **Verwandte Elemente** -mit den entsprechenden Berechtigungen, die Anwendung haben Zugriff auf eine Datei mit demselben Namen, aber einer anderen Erweiterung. Z. B. ein Dokument, das als gespeichert wurde eine `.txt` Datei und ein `.pdf`.
- **Temporären Verzeichnisse, Befehlszeilentool Verzeichnisse und Welt lesbaren Ortsangaben** -Ihrer-app hat unterschiedlichem Ausmaß des Zugriffs auf Dateien in anderen klar definierten Speicherorten wie vom System angegeben.

#### <a name="the-app-container-directory"></a>Die Container-app-Verzeichnisses

App-Container-Verzeichnis einer Xamarin.Mac-Anwendung weist folgende Merkmale auf:

- Es ist in einem ausgeblendeten Speicherort im Basisverzeichnis des Benutzers (in der Regel `~Library/Containers`) und zugegriffen werden kann, mit der `NSHomeDirectory` Funktion (siehe unten) in Ihrer Anwendung. Da es in das Stammverzeichnis ist, erhält jeder Benutzer ihre eigenen Container für die app.
- Die app verfügt über Lese-/Schreibzugriff auf das Container-Verzeichnis und alle seine Unterverzeichnisse und Dateien darin uneingeschränkt.
- Die meisten Pfad zur Ermittlung des MacOS-APIs sind relativ zu der app-Container. Beispielsweise der Container hat eine eigene **Bibliothek** (Zugriff erfolgt über `NSLibraryDirectory`), **Anwendungsunterstützung** und **Voreinstellungen** Unterverzeichnisse.
- MacOS erstellt und erzwingt die Verbindung zwischen und -app und den Container über das Signieren von Code. Wird auch eine andere app versucht, mithilfe die app zu fälschen seine **Bündel-ID**, es ist nicht möglich, Zugriff auf den Container aufgrund Signieren von Code.
- Der Container ist nicht für vom Benutzer generierte Dateien. Stattdessen ist es für Dateien, die Ihre Anwendung z. B. Datenbanken, Caches oder andere bestimmten Arten von Daten verwendet.
- Für _Shoebox_ Typen (z. B. Apple Foto-app), geht der Benutzer den Inhalt in den Container.

> [!IMPORTANT]
> Leider Xamarin.Mac verfügt nicht über 100 % API-Abdeckung noch (im Gegensatz zu Xamarin.iOS), daher die `NSHomeDirectory` -API-wurde nicht in der aktuellen Version von Xamarin.Mac zugeordnet.

Als vorübergehende problemumgehung können Sie den folgenden Code:

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Das Containerverzeichnis der app-Gruppe

Beginnend mit dem Mac, MacOS 10.7.5 (und höher), eine Anwendung kann mithilfe der `com.apple.security.application-groups` Berechtigung für einen gemeinsamen Container zuzugreifen, die für alle Anwendungen in der Gruppe ist. Sie können diesen freigegebenen Container für Nichtbenutzercode zugängliche Inhalte wie Datenbank oder andere Arten von unterstützenden Dateien (z. B. Caches) verwenden.

(Wenn sie Mitglied einer Gruppe sind) werden automatisch die Gruppencontainer, die in jeder app-Sandbox-Container hinzugefügt und an gespeichert sind `~/Library/Group Containers/<application-group-id>`. Die Gruppen-ID _müssen_ z. B. mit Ihrer Entwicklung-Team-ID und einem Punkt beginnen:

```plist
<team-id>.com.company.<group-name>
```

Weitere Informationen finden Sie unter Apple [Hinzufügen einer Anwendung auf eine Anwendungsgruppe](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) in [Referenz für Berechtigungsschlüssel](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Powerbox und den Dateinamen Systemzugriff außerhalb des app-Containers

Eine Sandbox Xamarin.Mac-Anwendung kann auf folgende Weise auf Speicherorte im Dateisystem außerhalb ihres Containers zugreifen:

- Klicken Sie auf die bestimmte Richtung des Benutzers (über Open und Dialogfelder Speichern oder andere Methoden, wie Sie Drag & Drop).
- Durch Verwenden von Berechtigungen für bestimmte Speicherorte im Dateisystem (z. B. `/bin` oder `/usr/lib`).
* Wenn ist der Speicherort im Dateisystem in bestimmten Verzeichnissen, die World (z. B. Freigabe) lesbar sind.

_Powerbox_ ist MacOS sicherheitstechnologie, die Interaktion mit dem Benutzer, die Rechte für den Dateizugriff einer Sandbox Xamarin.Mac-app zu erweitern. Powerbox verfügt über keine API, jedoch wird transparent aktiviert, wenn die app Ruft eine `NSOpenPanel` oder `NSSavePanel`. Powerbox Zugriff erfolgt über die Berechtigungen, die Sie für die Xamarin.Mac-Anwendung festlegen.

Wenn eine Sandbox-app zeigt eine offene oder Dialogfeld "Speichern", wird das Fenster wird durch Powerbox (und nicht von AppKit) angezeigt und hat daher Zugriff auf eine beliebige Datei oder das Verzeichnis, das der Benutzer Zugriff hat.

Wenn ein Benutzer das Öffnen einer Datei oder eines Verzeichnisses auswählt oder Dialogfeld "Speichern" (oder zieht entweder auf das Symbol der App), wird die app Sandbox von Powerbox den entsprechenden Pfad hinzugefügt.

Darüber hinaus ermöglicht das System automatisch eine Sandbox-app Folgendes:

- Verbindung mit einer System-Eingabe-Methode.
- Rufen Sie Dienste, die ausgewählt werden, von dem Benutzer über die **Services** im Menü (nur für Dienste gekennzeichnet _für Sandbox-apps sicher_ vom Service Provider).
- Geöffnete Dateien auswählen, von dem Benutzer über die **öffnen aktuelle** Menü.
- Verwenden Sie kopieren und Einfügen zwischen anderen Anwendungen ein.
- Lesen Sie Dateien von den folgenden Speicherorten für die Welt lesbar:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Lesen und Schreiben von Dateien in den Verzeichnissen, die von erstellten `NSTemporaryDirectory`.

Standardmäßig bleiben die Dateien geöffnet oder gespeichert, die von einer Sandbox Xamarin.Mac-app zugegriffen werden kann, bis die app beendet wird (es sei denn, die Datei noch geöffnet war, wenn die app beendet wird). Geöffnete Dateien werden automatisch mit der app-Sandbox über den MacOS-Feature "Resume" das nächste Mal wiederhergestellt werden, die, das die app gestartet wird.

Um Persistenz auf Dateien außerhalb einer Xamarin.Mac-App-Container bereitzustellen, verwenden Sie Sicherheit bezogenen Lesezeichen (siehe unten).

#### <a name="related-items"></a>Verwandte Elemente

Der App-Sandbox ermöglicht eine app auf verwandte Elemente, die die gleiche Dateinamen, aber verschiedene Erweiterungen aufweisen. Die Funktion besteht aus zwei Teilen: (a) eine Liste der entsprechenden Erweiterungen in der app `Info.plst` Datei, b) um die Sandbox mitzuteilen, was die app mit diesen Dateien durchführen.

Es gibt zwei Szenarien, wo dies sinnvoll ist:

1. Die app muss sich um eine andere Version der Datei (mit der neuen Erweiterung) speichern zu können. Exportieren Sie beispielsweise eine `.txt` -Datei in eine `.pdf` Datei. Um diese Situation zu behandeln, müssen Sie verwenden eine `NSFileCoordinator` für den Dateizugriff. Sie rufen die `WillMove(fromURL, toURL)` Methode zuerst Verschieben der Datei in die neue Erweiterung, und rufen dann `ItemMoved(fromURL, toURL)`.
2. Die app muss eine Hauptdatei mit einer Erweiterung und mehrere unterstützende Dateien mit verschiedenen Erweiterungen zu öffnen. Zum Beispiel einen Film und eine Datei mit Untertiteln. Verwenden einer `NSFilePresenter` für den Zugriff auf die sekundäre Datei. Geben Sie die Hauptdatei, die `PrimaryPresentedItemURL` -Eigenschaft und die sekundäre Datei in die `PresentedItemURL` Eigenschaft. Wenn die Haupt-Datei geöffnet wird, rufen Sie die `AddFilePresenter` -Methode der der `NSFileCoordinator` Klasse, um die sekundäre Datei registrieren.

In beiden Szenarien, die app die **"Info.plist"** Datei muss deklarieren die Dokumenttypen, die die app öffnen können. Für einen beliebigen Dateityp hinzufügen der `NSIsRelatedItemType` (mit einem Wert von `YES`), dessen Eintrag in der `CFBundleDocumentTypes` Array.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Öffnen und Speichern von Dialogfeld-Verhalten mit Sandbox-apps

Die folgenden Grenzwerte gelten für sind die `NSOpenPanel` und `NSSavePanel` , wenn sie in einer Sandbox Xamarin.Mac-app aufrufen:

- Sie können nicht programmgesteuert aufrufen, die **OK** Schaltfläche.
- Sie können die Auswahl des Benutzers in nicht programmgesteuert ändern eine `NSOpenSavePanelDelegate`.

Darüber hinaus sind die folgenden Änderungen für die Vererbung vorhanden:

- **Nicht-Sandbox-App** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Im Bereich Sicherheit einer Lesezeichen und permanenten Zugriff auf

Wie bereits erwähnt, kann eine Sandbox Xamarin.Mac-Anwendung eine Datei oder Ressource außerhalb ihres Containers über direkte Benutzerinteraktion (gemäß PowerBox) zugreifen. Jedoch Zugriff auf diese Ressourcen nicht automatisch über app-Start oder das System neu gestartet wird beibehalten.

Mithilfe von _Security-Scoped Lesezeichen_, eine Sandbox Xamarin.Mac-Anwendung der Benutzerabsicht beibehalten und verwalten Sie den Zugriff auf die angegebene frei, nachdem eine app neu starten kann.

#### <a name="security-scoped-bookmark-types"></a>Sicherheit im Bereich einer Textmarke-Typen

Bei der Arbeit mit Security-Scoped Lesezeichen und permanenten Zugriff auf Ressourcen stehen zwei sistine Anwendungsfälle:

- **Ein Lesezeichen App-Scoped stellt ständigen Zugriff auf eine vom Benutzer angegebene Datei oder einen Ordner bereit.** 

    Angenommen, im Sicherheitssandkasten ausgeführte Xamarin.Mac-Anwendung verwenden, um ein externes Dokument zur Bearbeitung öffnen kann (mithilfe einer `NSOpenPanel`), die app kann, damit es in Zukunft wieder dieselbe Datei zugreifen kann ein App-Scoped Lesezeichen zu erstellen.
- **Ein Lesezeichen Document-Scoped bietet es sich um eine bestimmte permanenten Zugriff auf Dokumente in einer untergeordneten Datei.** 

Beispielsweise hat eine bearbeitende-video-app, die eine Projektdatei erstellt den Zugriff auf einzelne Bilder, Videoclips und Audiodateien, die später zu einer einzelnen Films zusammengefasst werden können.

Wenn der Benutzer eine Ressourcendatei in das Projekt importiert (über eine `NSOpenPanel`), die app erstellt ein Lesezeichen Document-Scoped für das Element, das im Projekt gespeichert ist, damit die Datei immer für die app zugänglich ist.

Ein Lesezeichen Document-Scoped kann von jeder Anwendung gelöst werden, die die Lesezeichen-Daten und das Dokument öffnen können. Dadurch wird Portabilität, damit der Benutzer die Projektdateien an einem anderen Benutzer senden und, die alle Lesezeichen ebenfalls für sie funktionieren mit unterstützt.

> [!IMPORTANT]
> Ein Lesezeichen Document-Scoped können _nur_ zeigen Sie auf eine einzelne Datei und kein Ordner, und diese Datei kann nicht an einem Speicherort, der vom System verwendet werden (z. B. `/private` oder `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Verwenden von Lesezeichen im Bereich einer Sicherheit

Mit beiden Security-Scoped Lesezeichen, müssen Sie die folgenden Schritte ausführen:

1. **Legen Sie die entsprechenden Berechtigungen in der Xamarin.Mac-app, die Security-Scoped Lesezeichen verwenden, muss** -For App-Scoped Lesezeichen, festgelegt werden, die `com.apple.security.files.bookmarks.app-scope` Berechtigung-Taste, um `true`. Legen Sie für Document-Scoped Lesezeichen, das `com.apple.security.files.bookmarks.document-scope` Berechtigung-Taste, um `true`.
2. **Erstellen eines Lesezeichens Security-Scoped** -Sie erreichen dies für Dateien oder Ordnern, die der Benutzer den Zugriff auf bereitgestellt wurde (über `NSOpenPanel` z. B.), dass die app ständigen Zugriff auf benötigen. Verwenden der `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` Methode der `NSUrl` Klasse, um das Lesezeichen zu erstellen.
3. **Beheben Sie das Lesezeichen Security-Scoped** : Wenn die app Zugriff auf die Ressource erneut aus (z. B. nach dem Neustart muss) muss das Lesezeichen an eine URL im Bereich einer Sicherheit zu beheben. Verwenden der `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` Methode der `NSUrl` Klasse, um das Lesezeichen zu beheben.
4. **Das System, das Sie aus der URL Security-Scoped Datei zugreifen möchten explizit darüber zu benachrichtigen** – dieser Schritt muss ausgeführt werden, sofort nach dem Abrufen der oben genannten Security-Scoped-URL oder wenn Sie später wieder Zugriff auf die Ressource nach müssen Cacheseiten Ihren Zugriffsberechtigungen. Rufen Sie die `StartAccessingSecurityScopedResource ()` Methode der `NSUrl` Klasse, um den Zugriff auf eine URL Security-Scoped zu starten.
5. **Explizit darüber zu benachrichtigen des Systems, das Sie haben Zugriff auf die Datei aus der URL Security-Scoped** -so bald wie möglich, sollten Sie das System informieren, wenn die app Zugriff auf die Datei nicht mehr benötigt, (z. B., wenn der Benutzer es schließt). Rufen Sie die `StopAccessingSecurityScopedResource ()` Methode der `NSUrl` Klasse, um den Zugriff auf eine URL Security-Scoped zu beenden.

Nachdem Sie Zugriff auf eine Ressource aufgegeben haben, müssen Sie Schritt 4 erneut aus, um den Zugriff wiederherzustellen. Wenn die Xamarin.Mac-app neu gestartet wird, müssen Sie zurück zu Schritt 3 und eine erneute Auflösung für das Lesezeichen.

> [!IMPORTANT]
> Fehler beim Zugriff auf Security-Scoped URL-Ressourcen freizugeben, bewirkt, eine Xamarin.Mac-app, um Kernel zu Ressourcenverlusten kommen. Daher wird die app nicht mehr in der Lage Speicherorte im Dateisystem an ihren Container hinzufügen, bis er neu gestartet wird.

### <a name="the-app-sandbox-and-code-signing"></a>Die App-Sandbox und das Signieren von code

Nachdem Sie die App-Sandbox zu aktivieren und aktivieren die spezifischen Anforderungen für die Xamarin.Mac-app (über die Berechtigungen), müssen Sie code für registrieren Sie das Projekt für den Sandkasten wirksam werden. Führen Sie die Code signieren, da die Berechtigungen für die Anwendungs-Sandbox mit der app-Signatur verknüpft sind. 

MacOS erzwingt eine Verknüpfung zwischen der app-Container und seine Codesignatur, auf diese Weise, die keine andere Anwendung diesen Container zugreifen kann, auch wenn sie die Bündel-ID-apps-spoofing ist Dieser Mechanismus funktioniert wie folgt aus:

1. Wenn das System die app Container erstellt wird, wird ein _Zugriffssteuerungsliste_ (ACL) auf diesen Container. Der erste Eintrag in der Liste enthält der app _Anforderung festgelegten_ (DR), die beschreibt, wie künftige Versionen der app erkannt werden kann (Wenn sie aktualisiert wurde).
2. Jedes Mal eine Anwendung mit der gleichen Paket-ID gestartet wird, überprüft das System, dass der app-Code-Signatur die festgelegten Anforderungen in einem der Einträge in die ACL des Containers entspricht. Wenn das System nicht über eine Übereinstimmung findet, verhindert es das Starten die app.

Code Signing funktioniert folgendermaßen:

1. Erhalten Sie vor dem Erstellen der Xamarin.Mac-Projekt, ein Entwicklungszertifikat, eines Verteilungszertifikats und ein Entwickler-ID-Zertifikat aus dem Apple-Entwicklerportal.
2. Bei den Mac App Store die Xamarin.Mac-app verteilt wird, wird es mit einer Apple-Codesignatur signiert.

Beim Testen und Debuggen, müssen Sie eine Version von Xamarin.Mac-Anwendung verwenden, dass Sie angemeldet (die dient zum Erstellen des App-Containers). Später, wenn Sie möchten, um zu testen, oder installieren Sie die Version aus dem Apple App Store, es wird mit der Apple-Signatur signiert werden und kann nicht gestartet wird (da es die gleiche Codesignatur wie der ursprüngliche App-Container vorhanden ist). In diesem Fall wird einen Absturzbericht ähnlich der folgenden angezeigt:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Um dieses Problem zu beheben, müssen Sie zum Anpassen des ACL-Eintrags, um auf die Apple signierte Version der app zu verweisen.

Weitere Informationen zum Erstellen und Herunterladen der Bereitstellungsprofile für Sandkasten erforderlich sind, finden Sie unter den [Codesignaturen und Bereitstellen der App](#Signing_and_Provisioning_the_App) weiter oben.

#### <a name="adjusting-the-acl-entry"></a>Anpassen des ACL-Eintrags

Damit können die Apple signierte Version der Xamarin.Mac-app ausführen, führen Sie folgende Schritte aus:

1. Öffnen Sie die Terminal-app (in `/Applications/Utilities`).
2. Öffnen Sie ein Finder auf die Apple signierte Version der Xamarin.Mac-app.
3. Typ `asctl container acl add -file ` in die Terminal-Fenster.
4. Ziehen Sie die Xamarin.Mac-app-Symbol aus dem Finder-Fenster, und legen Sie sie in der Terminal-Fenster.
5. Der Befehl im Terminal wird der vollständige Pfad zur Datei hinzugefügt werden.
6. Drücken Sie **EINGABETASTE** zum Ausführen des Befehls.

Die ACL des Containers enthält jetzt die designierten Code-Anforderungen für beide Versionen der Xamarin.Mac-app aus, und MacOS ermöglicht jetzt entweder-Version ausgeführt.

#### <a name="display-a-list-of-acl-code-requirements"></a>Zeigt eine Liste der Anforderungen der ACL-code

Sie können eine Liste der Code in der ACL des Containers anzeigen, indem Sie wie folgt vorgehen:

1. Öffnen Sie die Terminal-app (in `/Applications/Utilities`).
2. Geben Sie `asctl container acl list -bundle <container-name>` ein.
3. Drücken Sie **EINGABETASTE** zum Ausführen des Befehls.

Die `<container-name>` ist in der Regel die Bündel-ID für die Xamarin.Mac-Anwendung.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Entwerfen einer Xamarin.Mac-app für die App-Sandbox

Es ist ein gängiger Workflow, der beim Entwerfen einer Xamarin.Mac-app für die App-Sandbox befolgt werden sollten. Dies bedeutet, dass die Einzelheiten der Implementierung von Sandkasten in einer app ausgeführt werden soll für die angegebene app Funktionalität eindeutig sein.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Sechs Schritte für den Umstieg auf die App-Sandbox

Entwerfen einer Xamarin.Mac-app für die App-Sandbox in der Regel umfasst die folgenden Schritte aus:

1. Bestimmt, ob die app für die Sandbox geeignet ist.
2. Entwerfen einer Strategie für Entwicklung und Verteilung.
3. Beheben von Inkompatibilitäten API.
4. Wenden Sie die erforderlichen Berechtigungen der App-Sandbox auf dem Xamarin.Mac-Projekt ein.
5. Fügen Sie die Trennung von Berechtigungen mit XPC hinzu.
6. Implementieren einer Strategie zur Migration.

> [!IMPORTANT]
> Müssen Sie nicht nur Sandbox die zentrale ausführbare Datei, in der Sie app-Bündel, aber auch alle enthaltenen Helper-app oder ein Tool in der Paketdatei. Dies ist erforderlich für jede app, die von den Mac App Store verteilt und, wenn möglich, sollte für eine andere Form der Verteilung der app vorgenommen werden.

Eine Liste aller ausführbaren Binärdateien in einer Xamarin.Mac-app-Bündel Geben Sie den folgenden Befehl im Terminal aus:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Wo `[Your-App-Bundle]` ist der Name und Pfad zum Paket von der Anwendung.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Bestimmen Sie, ob es sich bei eine Xamarin.Mac-app für die Sandbox geeignet ist.

Die meisten Xamarin.Mac-apps sind vollständig kompatibel mit der App-Sandbox und somit auch für die Sandbox geeignet ist. Wenn die app ein Verhalten erforderlich, die die App-Sandbox nicht zugelassen wird ist, sollten Sie einen alternativen Ansatz.

Wenn Ihre app eine der folgenden Verhaltensweisen erforderlich ist, ist es nicht kompatibel mit der Sandbox-App:

- **Autorisierungsdienste** -mit der App-Sandbox, Sie können nicht mit den Funktionen, die in beschriebenen arbeiten [Autorisierung Services C#-Referenz](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Eingabehilfen-APIs** -Sandbox Unterstützung wie z.B. Sprachausgabe oder apps, die Steuern von anderen Anwendungen ist nicht möglich.
- **Senden von Apple-Ereignissen an beliebige Apps** – Wenn die app erforderlich sind, Apple-Ereignisse an eine unbekannte, beliebige app senden, ist nicht möglich Sandbox. Eine bekannte Liste mit den aufgerufenen-apps die app kann weiterhin Sandbox sein, und die Berechtigungen müssen die Liste der aufgerufenen einschließen.
- **Wörterbücher für Benutzer Informationen zu anderen Aufgaben in Benachrichtigungen verteilten zu senden** -mit der App-Sandbox, können nicht einbezogen werden eine `userInfo` Wörterbuch beim Veröffentlichen von Beiträgen ein `NSDistributedNotificationCenter` -Objekt für andere Aufgaben messaging.
- **Laden Sie Erweiterungen für Kernel** -das Laden von Erweiterungen der Kernel ist nicht zulässig, von der Sandbox-App.
- **Simulieren von Eingabe Benutzer öffnen und speichern Sie Dialogfelder** – programmgesteuert bearbeiten öffnen oder speichern Dialoge zu simulieren, oder Ändern von Benutzereingaben ist nicht von der App-Sandbox zulässig.
- **Zugreifen auf oder Voreinstellungen mit anderen Apps** -ändern anderer Apps von der App-Sandbox unzulässig ist.
- **Konfigurieren von Netzwerkeinstellungen** -Netzwerkeinstellungen bearbeiten ist nicht zulässig, von der Sandbox-App.
- **Beenden andere Apps** -Sandbox-App verhindert, dass mit `NSRunningApplication` andere apps beendet.

### <a name="resolving-api-incompatibilities"></a>Beheben von Inkompatibilitäten der API

Beim Entwerfen einer Xamarin.Mac-app für die App-Sandbox können Inkompatibilitäten für die Verwendung von einigen MacOS APIs auftreten.

Hier sind einige allgemeine Probleme und Dinge, die Sie ausführen können, um diese zu beheben:

- **Sie öffnen, speichern und Nachverfolgen von Dokumenten** – Wenn Sie Dokumente, die als eine beliebige Technologie verwenden verwalten `NSDocument`, sollten Sie stattdessen diese Dank der integrierten Unterstützung für die App-Sandbox. `NSDocument` automatisch mit PowerBox funktioniert und bietet Unterstützung für Dokumente in Ihrer Sandbox beibehalten, wenn der Benutzer sie im Finder verschoben.
- **Behalten Sie den Zugriff auf Systemressourcen Datei** : Wenn das Xamarin.Mac-app hängt von ständigen Zugriff auf Ressourcen außerhalb ihres Containers, Security bezogenen Lesezeichen verwenden, um weiter Zugriff zu.
- **Erstellen Sie ein Element für die Anmeldung für eine App** -mit der App-Sandbox, Sie können keine erstellen Sie eine Anmeldung mit `LSSharedFileList` noch können Sie ändern den Status der starten-Dienste, die mit `LSRegisterURL`. Verwenden der `SMLoginItemSetEnabled` funktioniert wie beschrieben in Äpfel [Hinzufügen von Anmeldung-Elementen, die mit dem Service Management-Framework](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) Dokumentation.
- **Zugreifen auf Benutzerdaten** – Wenn Sie POSIX-Funktionen, z. B. verwenden `getpwuid` Verzeichnisdienste Basisverzeichnis des Benutzers abrufen, sollten Sie mit Cocoa oder Core Foundation Symbole wie `NSHomeDirectory`.
- **Zugreifen auf die Einstellungen von anderen Apps** : Da die App-Sandbox Pfad Auffinden von APIs in der app-Container und leitet Ändern von Einstellungen erfolgt innerhalb des Containers und den Zugriff auf eine andere apps Einstellungen in nicht zulässig. 
- **Mithilfe von eingebettetem HTML5-Video in Webansichten** – Wenn die Xamarin.Mac-app WebKit verwendet, um eingebettete HTML5-Videos wiederzugeben, müssen Sie auch eine Verknüpfung die app mit dem AV-Foundation-Framework. Die App-Sandbox verhindert CoreMedia Play in diesen Videos, die andernfalls.

### <a name="applying-required-app-sandbox-entitlements"></a>Anwenden der erforderliche App-Sandbox-Berechtigungen

Sie müssen die Berechtigungen für eine Xamarin.Mac-Anwendung bearbeiten, die Sie verwenden möchten, führen in der Sandbox-App, und Überprüfen der **Anwendungs-Sandbox aktivieren** Kontrollkästchen.

Basierend auf der Funktionalität der app, müssen Sie zur Aktivierung von anderen Berechtigungen den Zugriff auf Funktionen des Betriebssystems oder Ressourcen. App-Sandkasten funktioniert am besten, wenn Sie die Berechtigungen minimieren, die Sie anfordern, die für die Mindestanforderungen zur Ausführung Ihrer Anwendung hierzu einfach eine zufällige Berechtigungen aktivieren Sie.

Um zu bestimmen, welche Berechtigungen eine Xamarin.Mac-app erfordert, führen Sie folgende Schritte aus:

1. Aktivieren Sie die App-Sandbox und führen Sie die Xamarin.Mac-app.
2. Führen Sie die Features der App.
3. Öffnen Sie die Konsolen-app (verfügbar in `/Applications/Utilities`) und suchen Sie nach `sandboxd` Verstöße in den **alle Nachrichten** Protokoll.
4. Für jede `sandboxd` Verletzung, beheben Sie das Problem entweder über den app-Container anstelle von anderen Speicherorten im Dateisystem oder Anwenden von App-Sandbox-Berechtigungen den Zugriff auf eingeschränkte Funktionen des Betriebssystems zu aktivieren.
5. Führen Sie erneut aus, und Testen Sie alle Features der Xamarin.Mac-app erneut.
6. Wiederholen Sie diesen Schritt, bis alle `sandboxd` Verstöße behoben wurden.

### <a name="add-privilege-separation-using-xpc"></a>Fügen Sie Trennung von Berechtigungen mit XPC hinzu

Beim Entwickeln einer Xamarin.Mac-app für die App-Sandbox betrachten Sie in den Bedingungen der Berechtigungen und den Zugriff der app-Verhalten Sie sich dann mit hohem Risiko Vorgängen in ihre eigenen Dienste XPC trennen.

Weitere Informationen finden Sie auf der Apple [erstellen XPC Services](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) und [Daemons und Services Programming Guide](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Implementieren einer Strategie zur migration

Wenn Sie eine neue, Sandbox Version einer Xamarin.Mac-Anwendung, die nicht zuvor Sandbox freigeben, müssen Sie sicherstellen, dass aktuelle Benutzer einen nahtlosen Upgradepfad. 

 Informationen darüber, wie Sie ein Manifest der Container-Migration zu implementieren, finden Sie in Apple [Migrieren einer App in einer Sandbox](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel hat einem detaillierten Einblick in Sandkasten in einer Xamarin.Mac-Anwendung geführt. Zunächst haben wir einfach Xamarin.Mac-app um die Grundlagen der Sandbox-App anzuzeigen. Als Nächstes wurde gezeigt, wie Sandkasten-Verletzungen zu beheben. Anschließend haben wir eine detaillierte sehen Sie sich die App-Sandbox und schließlich erläutert, eine Xamarin.Mac-app für die Sandbox-App entwerfen.



## <a name="related-links"></a>Verwandte Links

- [Veröffentlichen im App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Informationen zu App-Sandbox](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Anwendungs-Sandbox-Probleme](https://developer.apple.com/library/content/qa/qa1773/_index.html)
