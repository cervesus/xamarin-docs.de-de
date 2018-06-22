---
title: Verwenden einer Sandbox eine Xamarin.Mac-app
description: Dieser Artikel behandelt Sandkasten eine Xamarin.Mac-Anwendung für die Veröffentlichung im App Store. Es umfasst alle Elemente, die in den Sandkasten, z. B. Containerverzeichnisse, Berechtigungen, Berechtigungen für Benutzer bestimmt, Recht Trennung und Kernel-Erzwingung wechseln.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a02d7639975de092b05f31bacedd6bde4c9392f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787908"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Verwenden einer Sandbox eine Xamarin.Mac-app

_Dieser Artikel behandelt Sandkasten eine Xamarin.Mac-Anwendung für die Veröffentlichung im App Store. Es umfasst alle Elemente, die in den Sandkasten, z. B. Containerverzeichnisse, Berechtigungen, Berechtigungen für Benutzer bestimmt, Recht Trennung und Kernel-Erzwingung wechseln._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie die gleiche Möglichkeit für das Sandboxing einer Anwendung, wie Sie beim Arbeiten mit Objective-C oder Swift.

[![Ein Beispiel für die ausgeführte app](sandboxing-images/intro01.png "ein Beispiel für die ausgeführte app")](sandboxing-images/intro01-large.png#lightbox)

In diesem Artikel wird beschrieben, die Grundlagen der Arbeit mit Sandkasten in einer Anwendung Xamarin.Mac und alle Elemente, die in den Sandkasten wechseln: Containerverzeichnisse, Berechtigungen, Berechtigungen für Benutzer bestimmt, Recht Trennung und Kernel-Erzwingung. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.

## <a name="about-the-app-sandbox"></a>Informationen zu den App-Sandkasten

Der Sandkasten-App bietet starke Verteidigung gegen Beschädigung, die von einer böswilligen Anwendung auf einem Mac ausgeführt wird, indem der Zugriff eingeschränkt, die eine Anwendung auf Systemressourcen verursacht werden kann.

Eine nicht-Sandbox-Anwendung kann verfügt über vollständigen Berechtigungen des Benutzers, der die app ausgeführt wird und Zugriff auf oder nichts, die der Benutzer kann. Wenn die app enthält Sicherheit Löcher (oder einem Framework, die verwendet wird), kann ein Hacker potenziell ausnutzen von Schwachstellen und verwenden Sie die app die Steuerung der Mac, der es ausgeführt wird.

Durch das Beschränken des Zugriffs auf Ressourcen für eine einzelne Anwendung, stellt eine Sandbox-app eine Verteidigungslinie gegen den Diebstahl, beschädigt oder böswilligen Absichten seitens einer Anwendung, die auf dem Computer des Benutzers ausgeführt.

Der App-Sandkasten ist eine Access-Steuerelement-Technologie integriert MacOS (auf der Kernelebene erzwungen), die eine Strategie verfolgt bereitstellt:

1. Der Sandbox-App kann der Entwickler beschreiben _wie_ eine Anwendung mit dem Betriebssystem und auf diese Weise interagiert, wird er nur die Zugriffsrechte, die sind erforderlich, damit die Aufgabe zu erledigen und nicht mehr gewährt.
2. Der Sandkasten-App ermöglicht dem Benutzer nahtlos gewähren weiteren Zugriff auf das System über das Öffnen und speichern die Dialogfelder, ziehen und Ablegen von Vorgängen und anderen, gemeinsamen Benutzerinteraktionen.

### <a name="preparing-to-implement-the-app-sandbox"></a>Vorbereiten der Implementierung der App-Sandkasten

Die Elemente der Sandbox-App, die im Artikel detailliert erläutert werden, lauten wie folgt:

- Containerverzeichnisse
- Berechtigungen
- Berechtigungen für Benutzer bestimmt
- Trennung von Berechtigungen
- Kernel-Erzwingung

Nachdem Sie diese Details verstanden haben, müssen Sie einen Plan für die Übernahme der App-Sandkasten in Ihrer Anwendung Xamarin.Mac erstellen sein.

Zunächst müssen Sie bestimmen, ob Ihre Anwendung ein guter Kandidat für Sandkasten darstellt (die meisten Anwendungen sind). Als Nächstes müssen Sie zum Auflösen von Inkompatibilitäten API und zu bestimmen, welche Elemente der Sandbox-App benötigen. Untersuchen Sie schließlich mit Trennung von Berechtigungen zum Schutz der Anwendungsebene zu maximieren.

Bei der App-Sandkasten einführen, werden einige Dateisystemspeicherorte, die von der Anwendung verwendeten abweichen. Insbesondere müssen die Anwendung ein Container-Verzeichnis, das verwendet wird, für die Anwendung unterstützenden Dateien, Datenbanken, Caches und alle anderen Dateien, die nicht Benutzerdokumente sind. Geben Sie MacOS und Xcode-Support, um diese Arten von Dateien aus ihren legacy Speicherorten auf den Container zu migrieren.

## <a name="sandboxing-quick-start"></a>Verwenden einer Sandbox-Schnellstart

In diesem Abschnitt erstellen wir eine einfache Xamarin.Mac-app, die eine Webansicht verwendet (wofür eine Netzwerkverbindung, die unter Sandkasten beschränkt ist, sofern nicht ausdrücklich angefordert) als ein Beispiel für erste Schritte mit der Sandbox-App.

Wir müssen überprüfen, ob die Anwendung tatsächlich Sandbox und erfahren Sie, wie Suchen und beheben häufige App Sandkasten auftreten.

### <a name="creating-the-xamarinmac-project"></a>Erstellen des Projekts Xamarin.Mac

Führen Sie Folgendes ein, um unsere Beispielprojekt erstellen wir:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf die **neue Projektmappe...** klicken.
2. Aus der **neues Projekt** wählen Sie im Dialogfeld **Mac** > **App** > **Kakao App**: 

    [![Erstellen einer neuen Kakao App](sandboxing-images/sample01.png "beim Erstellen einer neuen Kakao-App")](sandboxing-images/sample01-large.png#lightbox)
3. Klicken Sie auf die **Weiter** Schaltfläche, geben Sie `MacSandbox` für den Projektnamen und klicken Sie auf die **erstellen** Schaltfläche: 

    [![Eingeben des app-namens](sandboxing-images/sample02.png "eingeben des app-namens")](sandboxing-images/sample02-large.png#lightbox)
4. In der **Lösung Pad**, doppelklicken Sie auf die **Main.storyboard** Datei, die sie zur Bearbeitung in Xcode geöffnet: 

    [![Bearbeiten die Haupt-Storyboard](sandboxing-images/sample03.png "Haupt-Storyboard bearbeiten")](sandboxing-images/sample03-large.png#lightbox)
5. Ziehen Sie eine **Webansicht** in das Fenster zu skalieren, um das Füllen des Inhaltsbereichs, und legen Sie ihn zu vergrößern und Verkleinern von mit dem Fenster: 

    [![Hinzufügen einer Webansicht](sandboxing-images/sample04.png "eine Webansicht hinzufügen")](sandboxing-images/sample04-large.png#lightbox)
6. Erstellen Sie eine Steckdose für die Webansicht aufgerufen `webView`: 

    [![Erstellen einer neuen Steckdose](sandboxing-images/sample05.png "Erstellen einer neuen Steckdose")](sandboxing-images/sample05-large.png#lightbox)
7. Zurück zu Visual Studio für Mac, und doppelklicken Sie auf die **ViewController.cs** in der Datei die **Lösung Pad** um ihn zur Bearbeitung zu öffnen.
8. Fügen Sie die folgende Anweisung: `using WebKit;`
9. Stellen Sie die `ViewDidLoad` Methode aussehen wie folgt: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Speichern Sie die Änderungen.

Führen Sie die Anwendung, und stellen Sie sicher, dass der Apple-Website wie folgt im Fenster angezeigt wird:

[![Zeigt eine Beispiel-app ausführen](sandboxing-images/sample06.png "zeigt eine Beispiel-app ausführen")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Signieren und Bereitstellen der app

Bevor wir die App-Sandkasten aktivieren können, müssen wir zuerst bereitstellen und signieren Sie unsere Xamarin.Mac-Anwendung.

Können Sie folgende Aktionen ausführen:

1. Melden Sie sich das Apple-Entwicklerportal: 

    [![Anmelden bei der Apple-Entwicklerportal](sandboxing-images/sign01.png "Anmeldung bei der Apple-Entwicklerportal")](sandboxing-images/sign01-large.png#lightbox)
2. Wählen Sie **Zertifikate "," Bezeichner "und" Profile**: 

    [![Auswählen von „Zertifikate, Bezeichner und Profile“](sandboxing-images/sign02.png "Selecting Certificates, Identifiers & Profiles")](sandboxing-images/sign02-large.png#lightbox)
3. Klicken Sie unter **Mac Apps**Option **Bezeichner**: 

    [![Auswählen von Bezeichnern](sandboxing-images/sign03.png "Bezeichner auswählen")](sandboxing-images/sign03-large.png#lightbox)
4. Erstellen Sie eine neue ID für die Anwendung: 

    [![Erstellen einer neuen App-ID](sandboxing-images/sign04.png "Erstellen einer neuen App-ID")](sandboxing-images/sign04-large.png#lightbox)
5. Klicken Sie unter **Provisioning Profile**Option **Entwicklung**: 

    [![Auswählen der Entwicklung](sandboxing-images/sign05.png "Entwicklung auswählen")](sandboxing-images/sign05-large.png#lightbox)
6. Erstellen Sie ein neues Profil, und wählen Sie **Mac App-Entwicklung**: 

    [![Erstellen eines neuen Profils](sandboxing-images/sign06.png "Erstellen eines neuen Profils")](sandboxing-images/sign06-large.png#lightbox)
7. Wählen Sie die App-ID, die wir zuvor erstellt haben: 

    [![Auswählen der App-ID](sandboxing-images/sign07.png "App-ID auswählen")](sandboxing-images/sign07-large.png#lightbox)
8. Wählen Sie die Entwickler für dieses Profil aus: 

    [![Hinzufügen von Entwicklern](sandboxing-images/sign08.png "hinzufügen-Entwickler")](sandboxing-images/sign08-large.png#lightbox)
9. Wählen Sie die Computer für dieses Profil: 

    [![Auswahl der zulässigen Computer](sandboxing-images/sign09.png "zulässigen Computer auswählen")](sandboxing-images/sign09-large.png#lightbox)
10. Benennen Sie dem Profil: 

    [![Vergabe eines Namens für dem Profil](sandboxing-images/sign10.png "Vergabe eines Namens für dem Profil")](sandboxing-images/sign10-large.png#lightbox)
11. Klicken Sie auf die **Fertig** Schaltfläche.

> [!IMPORTANT]
> In einigen Fällen müssen Sie möglicherweise neue Bereitstellungsprofil direkt von der Apple-Entwicklerportal herunterladen, und doppelklicken darauf installieren. Sie müssen möglicherweise auch beenden und starten Sie Visual Studio für Mac, bevor er auf das neue Profil zugreifen kann.

Als Nächstes müssen wir die neuen App-ID und das Profil auf dem Entwicklungscomputer zu laden. Führen Sie wir Folgendes:

1. Starten Sie Xcode, und wählen Sie **Voreinstellungen** aus der **Xcode** Menü: 

    ![Bearbeiten von Konten in Xcode](sandboxing-images/sign11.png "Konten in Xcode bearbeiten")
2. Klicken Sie auf die **Details anzeigen...**  Schaltfläche: 

    ![Klicken auf die Schaltfläche "Details anzeigen"](sandboxing-images/sign12.png "auf die Schaltfläche \"Details anzeigen\"")
3. Klicken Sie auf die **aktualisieren** Schaltfläche (in der unteren linken Ecke).
4. Klicken Sie auf die **Fertig** Schaltfläche.

Als Nächstes müssen wir die neuen App-ID und das Bereitstellungsprofil in unsere Xamarin.Mac-Projekt auswählen. Führen Sie wir Folgendes:

1. In der **Lösung Pad**, doppelklicken Sie auf die **"Info.plist"** Datei zur Bearbeitung zu öffnen.
2. Sicherstellen, dass die **Paket-ID** mit unserer App-ID, die oben erstellten übereinstimmt (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Bearbeiten die Paket-ID](sandboxing-images/sign13.png "bearbeiten die Paket-ID")](sandboxing-images/sign13-large.png#lightbox)
3. Doppelklicken Sie anschließend auf die **Entitlements.plist** Datei, und stellen Sie sicher unsere **iCloud Schlüsselwertspeicher** und die **iCloud Container** alle unsere oben erstellten App-ID übereinstimmen (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Bearbeiten der Datei Entitlements.plist](sandboxing-images/sign17.png "Bearbeiten der Datei Entitlements.plist")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen.
4. In der **Lösung Pad**, doppelklicken Sie auf die Projektdatei, um die Optionen für die Bearbeitung zu öffnen:  

    ![Editign Optionen für die Lösung](sandboxing-images/sign14.png "Editign Optionen für die Lösung")
5. Wählen Sie **Mac signieren**, überprüfen Sie dann **Signieren Anwendungspaket** und **signieren Sie das Installationspaket**. Klicken Sie unter **Bereitstellungsprofil**, wählen Sie aus der oben erstellten: 

    ![Festlegen der Bereitstellungsprofil](sandboxing-images/sign15.png "Bereitstellungsprofil festlegen")
6. Klicken Sie auf die **Fertig** Schaltfläche.

> [!IMPORTANT]
> Sie müssen möglicherweise beenden und starten Sie Visual Studio für Mac Bezugsquelle so, dass die neuen App-ID und Provisioning-Profil, das von Xcode installiert wurde erkannt.

#### <a name="troubleshooting-provisioning-issues"></a>Problembehandlung bei der Bereitstellung

An diesem Punkt sollten Sie die Anwendung auszuführen, und stellen Sie sicher, dass alles, was signiert ist und ordnungsgemäß bereitgestellt. Wenn die Anwendung weiterhin wie zuvor ausgeführt wird, ist alles gut. Ein Fehler auftritt erhalten Sie möglicherweise ein Dialogfeld wie den folgenden Ausdruck:

[![Ein Beispiel, das Problem bereitstellungsdialogfeld](sandboxing-images/sign16.png "beispielhaft Problem bereitstellungsdialogfeld")](sandboxing-images/sign16-large.png#lightbox)

Hier sind die häufigsten Ursachen der Bereitstellung und der Signierung Probleme:

- Der App-Paket-ID stimmt nicht mit der App-ID des ausgewählten Profils überein.
- Die Developer-ID stimmt nicht mit der Developer-ID des ausgewählten Profils überein.
- Die UUID der getestete auf Mac ist nicht als Teil des ausgewählten Profils registriert.

Beheben Sie bei einem Problem führt das Problem auf der Apple-Entwicklerportal, aktualisieren Sie die Profile in Xcode, und führen Sie einen bereinigten Build in Visual Studio für Mac.

### <a name="enable-the-app-sandbox"></a>Aktivieren Sie die App-Sandkasten

Sie aktivieren den Sandkasten für die App durch ein Kontrollkästchen in Ihren Projekten Optionen auswählen. Führen Sie folgende Schritte aus:

1. In der **Lösung Pad**, doppelklicken Sie auf die **Entitlements.plist** Datei zur Bearbeitung zu öffnen.
2. Überprüfen Sie beide **aktivieren Sie Berechtigungen** und **App-Sandkasten aktivieren**: 

    [![Bearbeiten von Berechtigungen, und aktivieren Sandkasten](sandboxing-images/sign17.png "Berechtigungen bearbeiten und Verwenden einer Sandbox aktivieren")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen.

An diesem Punkt des Sandkastens App aktiviert haben, aber Sie haben nicht den erforderlichen Netzwerkzugriff für die Webansicht angegeben. Wenn Sie die Anwendung jetzt ausführen, sollten Sie ein leeres Fenster abrufen:

[![Mit der Webzugriff blockiert](sandboxing-images/sample08.png "mit der Webzugriff blockiert")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Überprüfen, dass die app Sandkastenprojekt handelt.

Abgesehen von der Ressource Blockierverhalten gibt es drei Hauptmethoden mitteilen, ob eine Anwendung Xamarin.Mac erfolgreich Sandkasten wurde:

1. Überprüfen Sie im Finder, den Inhalt von der `~/Library/Containers/` Ordner - ist die app Sandbox, wird ein Ordner mit dem Namen wie Ihre app-Paket-ID (Beispiel: `com.appracatappra.MacSandbox`): 

    [![Öffnen die app-Bündel](sandboxing-images/sample09.png "Öffnen der app-Bündel")](sandboxing-images/sample09-large.png#lightbox)
2. Das System erkennt die app als im Aktivitätsmonitor Sandkasten:
    - Starten von Aktivitätsmonitor (unter `/Applications/Utilities`). 
    - Wählen Sie **Ansicht** > **Spalten** und sicherstellen, dass die **Sandkasten** Menüelement aktiviert ist.
    - Stellen Sie sicher, dass die Sandkasten-Spalte liest `Yes` für Ihre Anwendung: 

    [![Überprüfen die app im Aktivitätsmonitor](sandboxing-images/sample10.png "überprüfen die app im Aktivitätsmonitor")](sandboxing-images/sample10-large.png#lightbox)
3. Überprüfen Sie, dass die app, die binäre Sandkastenprojekt handelt:
    - Starten Sie die Terminal-app.
    - Navigieren Sie zu den Anwendungen `bin` Verzeichnis.
    - Mit diesem Befehl ausgeben: `codesign -dvvv --entitlements :- executable_path` (, in denen `executable_path` ist der Pfad der Anwendung): 

    [![Überprüfen die app in der Befehlszeile](sandboxing-images/sample11.png "überprüfen die app in der Befehlszeile")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Debuggen einer Sandbox-app

Der Debugger eine Verbindung herstellt, Xamarin.Mac-Apps über TCP, das bedeutet, dass standardmäßig beim Sandkasten, aktivieren sie für die Verbindung der App kann, sodass, wenn Sie versuchen, die app auszuführen, ohne die richtigen Berechtigungen aktiviert, Sie einen Fehler erhalten *"Es konnte keine Verbindung mit der Debugger"*. 

[![Festlegen von Optionen](sandboxing-images/debug01.png "Festlegen von Optionen")](sandboxing-images/debug01-large.png#lightbox)

Die **zulassen ausgehende Netzwerkverbindungen (Client)** Berechtigung ist erforderlich, damit der Debugger, kann durch Aktivierung dieser Einstellung normalerweise Debuggen. Da Sie ohne Debuggen können, haben wir aktualisiert die `CompileEntitlements` Ziel für `msbuild` hinzuzufügende automatisch über diese Berechtigung die Berechtigungen für jede app, die für das Debuggen Sandbox ist nur builds. Releasebuilds sollten die Berechtigungen, die in der Berechtigungsdatei unverändert bleiben sollen angegeben verwenden.

### <a name="resolving-an-app-sandbox-violation"></a>Auflösen von eine Verletzung der App-Sandkasten

Eine Verletzung der Sandbox-App tritt auf, wenn eine Sandbox Xamarin.Mac Anwendung hat versucht, eine Ressource zuzugreifen, die verfügt nicht ausdrücklich zugelassen werden. Beispielsweise unsere Webansicht wird nicht mehr auf der Apple-Website anzeigen.

Die häufigste Ursache App Sandkasten Verstöße tritt auf, wenn die Berechtigung-Einstellungen für Mac in Visual Studio angegebene nicht die Anforderungen der Anwendung übereinstimmen. Sichern Sie erneut, zu unserem Beispiel die fehlenden Netzwerkverbindung Ansprüche, die Webansicht aus arbeiten beibehalten werden sollen.

#### <a name="discovering-app-sandbox-violations"></a>Ermittlung von Verletzungen der App-Sandkasten

Wenn Sie vermuten, eine Verletzung der App-Sandkasten in Ihrer Anwendung Xamarin.Mac auftritt dass, ist die schnellste Methode, ermitteln das Problem mithilfe der **Konsole** app.

Führen Sie folgende Schritte aus:

1. Kompilieren Sie die betreffende Anwendung und führen Sie es in Visual Studio für Mac.
2. Öffnen der **Konsole** Anwendung (über `/Applications/Utilties/`).
3. Wählen Sie **alle Nachrichten** in der Randleiste, und geben Sie `sandbox` in der Suche: 

    [![Ein Beispiel für eine Sandbox-Problem in der Konsole](sandboxing-images/resolve01.png "ein Beispiel für eine Sandbox-Problem in der Konsole")](sandboxing-images/resolve01-large.png#lightbox)

Unserem oben genannten Beispiel-app können Sie entnehmen, dass der Kernel blockiert die `network-outbound` Datenverkehr aufgrund des Sandkastens App, da wir dieses Recht nicht angefordert haben.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Beheben von App-Sandkasten Verstöße mit Berechtigungen

Nun, wir gesehen haben, wie App-Sandkasten Verstöße finden, sehen wir, wie sie behoben werden können, indem Sie Berechtigungen für die Anwendung anpassen.

Führen Sie folgende Schritte aus:

1. In der **Lösung Pad**, doppelklicken Sie auf die **Entitlements.plist** Datei zur Bearbeitung zu öffnen.
2. Klicken Sie unter der **Berechtigungen** aktivieren Sie im Abschnitt der **zulassen ausgehende Netzwerkverbindungen (Client)** Kontrollkästchen: 

    [![Bearbeiten die Berechtigungen](sandboxing-images/sign17.png "die Berechtigungen bearbeiten")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen an die Anwendung.

Wenn wir führen Sie die Schritte oben für unser Beispiel-app dann erstellen und ausführen, wird der Webinhalt wird jetzt wie erwartet angezeigt.

## <a name="the-app-sandbox-in-depth"></a>Der ausführliche App-Sandkasten

Zugriffssteuerungsmechanismen, die von der App-Sandkasten bereitgestellt sind einige und leicht zu verstehen. Allerdings ist die Möglichkeit, dass jede app die App-Sandkasten-Versorgung wird eindeutig und basierend auf den Erfordernissen der app.

Die beste Leistung zu verhindern, dass Ihre Anwendung Xamarin.Mac durch bösartigen Code ausgenutzt wird angegeben, es müssen nur eine einzelne Schwachstelle in entweder der app (oder eine der Bibliotheken oder Frameworks rechnergebunden) der app-Interaktionen mit Kontrolle über die System.

Der App-Sandkasten wurde entworfen, um zu verhindern, dass die Übernahme (oder Schäden, die es führen kann) werden, indem ermöglicht Ihnen das Festlegen der Anwendung vorgesehene Interaktionen mit dem System. Das System wird nur Zugriff auf die Ressource, die die Anwendung erfordert, um die Aufgabe zu erledigen und keine weitere gewährt.

Wenn für den Sandkasten für die App zu entwerfen, werden Sie für den ungünstigsten Fall entwerfen. Wenn die Anwendung durch bösartigen Code beeinträchtigt wird, ist es für den Zugriff auf nur die Dateien und Ressourcen in der app-Sandkasten beschränkt.

### <a name="entitlements-and-system-resource-access"></a>Berechtigungen und Zugriff auf die System-Ressourcen

Wie wir oben gesehen haben, wird eine Xamarin.Mac-Anwendung, die kein Sandkasten wurde gewährt, die Vollzugriff und den Zugriff des Benutzers, der die app ausgeführt wird. Gefährdung durch bösartigen Code kann eine app nicht geschützten als Agent für feindlichen Verhalten mit einer großen Zahl potenzielle Schäden verursacht fungieren.

Durch Aktivieren der App-Sandkasten, entfernen Sie alle bis auf einen minimalen Satz von Berechtigungen, die Sie dann erneut mit Ihrer Xamarin.Mac app-Berechtigungen müssen nur einzelne aktivieren. 

Sie Ihre Anwendungsverzeichnis App-Sandkasten Ressourcen ändern, indem bearbeiten seine **Entitlements.plist** Datei- und Überprüfung oder der Rechte erforderlich sind, aus den Dropdownfeldern Editoren auswählen:

[![Bearbeiten die Berechtigungen](sandboxing-images/sign17.png "die Berechtigungen bearbeiten")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Containerverzeichnisse und Dateisystemzugriff

Wenn Ihre Anwendung Xamarin.Mac der App-Sandkasten einführt, hat er Zugriff auf den folgenden Speicherorten:

- **Das Containerverzeichnis App** -beim ersten Ausführen, erstellt das Betriebssystem eine besondere _Containerverzeichnis_ wobei aller zugehörigen Ressourcen gelangen, nur darauf zugreifen können. Die app wird vollständige Lese-/Schreibzugriff auf dieses Verzeichnis haben.
- **App-Containerverzeichnisse** -Ihrer app Zugriff auf eine oder mehrere gewährt werden kann _Gruppe Container_ für apps in der gleichen Gruppe freigegeben wurden.
- **Dateien mit benutzerdefinierten** -erhält die Anwendung automatisch den Zugriff auf Dateien, die explizit geöffnet oder gezogen und auf die Anwendung vom Benutzer gelöscht.
- **Verknüpfte Elemente** -mit den entsprechenden Berechtigungen Ihre Anwendung Zugriff auf eine Datei mit demselben Namen, aber eine andere Erweiterung haben. Angenommen, ein Dokument, das als beide gespeichert wurde, eine `.txt` Datei und ein `.pdf`.
- **Temporären Verzeichnissen Befehlszeilentool Verzeichnisse und Ortsangaben World lesbaren** – die app kann in unterschiedlichem Umfang Zugriff auf Dateien in anderen klar definierten Speicherorten gemäß dem System.

#### <a name="the-app-container-directory"></a>Das Containerverzeichnis der app

Einer Xamarin.Mac Anwendungsverzeichnis App-Containerverzeichnis weist folgende Merkmale auf:

- Es ist in einem ausgeblendeten Speicherort im Basisverzeichnis des Benutzers (in der Regel `~Library/Containers`) und darauf zugegriffen werden kann, mit der `NSHomeDirectory` Funktion (siehe unten) in Ihrer Anwendung. Da es in das Stammverzeichnis ist, erhält jeder Benutzer ihre eigenen Container für die app.
- Die app verfügt über Lese-/Schreibzugriff auf die Container-Verzeichnis- und alle seine Unterverzeichnisse und Dateien darin uneingeschränkten.
- Die meisten der MacOS Pfad suchen-APIs sind relativ zu den app-Container. Beispielsweise der Container hat einen eigenen **Bibliothek** (Zugriff erfolgt über `NSLibraryDirectory`), **Anwendungsunterstützung** und **Voreinstellungen** Unterverzeichnisse.
- MacOS einrichtet und erzwingt die Verbindung zwischen und app und den Container über eine Codesignatur. Wird auch eine andere app versucht, mithilfe die app zu fälschen seine **Paket-ID**, es wird nicht in der Lage, Zugriff auf den Container aufgrund Codesignatur.
- Der Container ist nicht für, die generierten Benutzerdateien. Stattdessen ist es für Dateien, die Ihre Anwendung z. B. Datenbanken, Caches oder andere bestimmten Typen von Daten verwendet.
- Für _Shoebox_ Arten von apps (z. B. Apple Foto-Apps), Inhalt des Benutzers wird in den Container gesendet.

> [!IMPORTANT]
> Leider Xamarin.Mac verfügt nicht über 100 % API Coverage noch (im Gegensatz zu Xamarin.iOS), daher die `NSHomeDirectory` API wurde nicht in der aktuellen Version von Xamarin.Mac zugeordnet.

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

Beginnend mit Mac MacOS 10.7.5 (und höher), eine Anwendung kann mithilfe der `com.apple.security.application-groups` Rechtsansprüche auf freigegebene Container zuzugreifen, für alle Anwendungen in der Gruppe ist. Sie können diesen freigegebenen Container für nicht-benutzerseitigen nach außen verfügbaren Inhalt z. B. Datenbank- oder andere Arten von unterstützenden Dateien (z. B. Caches) verwenden.

Die Container-Gruppe automatisch hinzugefügt werden jede app Sandkasten Container (Wenn sie einer Gruppe gehören) und werden auf gespeichert `~/Library/Group Containers/<application-group-id>`. Die Gruppen-ID _müssen_ beginnen mit der Development-Team-ID und ein Punkt, z. B.:

```plist
<team-id>.com.company.<group-name>
```

Weitere Informationen finden Sie in der Apple- [Hinzufügen einer Anwendung zu einer Anwendungsgruppe](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) in [Berechtigung Key Reference](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Powerbox und den Dateinamen Systemzugriff außerhalb der app-container

Eine Sandbox Xamarin.Mac Anwendung erreichen Speicherorten im Dateisystem außerhalb des Containers auf folgende Weise:

- In der bestimmten Richtung des Benutzers (über öffnen und Speichern Dialogfelder oder andere Methoden, wie Sie Drag & Drop).
- Durch Verwenden von Berechtigungen für bestimmte Dateisystemspeicherorte (z. B. `/bin` oder `/usr/lib`).
* Ist bei der Speicherort im Dateisystem, in bestimmten Verzeichnissen, die World (z. B. Freigabe) lesbar sind.

_Powerbox_ ist die MacOS sicherheitstechnologie, die Interaktion mit dem Benutzer Zugriffsrechte für die Sandbox Xamarin.Mac app-Datei erweitern. Powerbox verfügt über keine API, jedoch wird transparent aktiviert, wenn die app Ruft eine `NSOpenPanel` oder `NSSavePanel`. Powerbox Zugriff erfolgt über die Berechtigungen, die Sie für Ihre Anwendung Xamarin.Mac festlegen.

Wenn eine Sandbox-app zeigt eine offene oder Dialogfeld Speichern, wird das Fenster Powerbox (und nicht AppKit) wird angezeigt und hat daher Zugriff auf eine beliebige Datei oder das Verzeichnis, das der Benutzer Zugriff hat.

Wenn ein Benutzer das Öffnen einer Datei oder eines Verzeichnisses auswählt oder Dialogfeld "Speichern" (oder zieht entweder auf das Symbol für die App), fügt Powerbox den entsprechenden Pfad für den Sandkasten für die app an.

Darüber hinaus erlaubt das System automatisch eine Sandbox-app wird Folgendes:

- Verbindung mit einer Eingabe-Methode.
- Rufen Sie vom Benutzer ausgewählten Dienste der **Services** Menü (nur für Dienste als markiert _für Sandkasten apps sicher_ vom Dienstanbieter).
- Wählen Sie geöffnete Dateien vom Benutzer über die **zuletzt verwendete öffnen** im Menü.
- Verwenden Sie kopieren und Einfügen zwischen anderen Anwendungen.
- Lesen Sie Dateien von den folgenden World lesbaren Speicherorten:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Lesen und Schreiben von Dateien in den Verzeichnissen, die von erstellte `NSTemporaryDirectory`.

Standardmäßig bleiben Dateien geöffnet, oder von einer Sandbox Xamarin.Mac app gespeichert zugegriffen werden kann, bis die app beendet wird (es sei denn, die Datei noch geöffnet war, wenn die app beendet wird). Geöffnete Dateien werden automatisch in der app Sandbox über die MacOS Resume-Funktion das nächste Mal wiederhergestellt werden, wenn, das die app gestartet wird.

Um Persistenz auf Dateien außerhalb einer Xamarin.Mac-App-Container bereitzustellen, verwenden Sie Sicherheit bezogenen Lesezeichen (siehe unten).

#### <a name="related-items"></a>Verwandte Elemente

Der Sandkasten-App ermöglicht einer app auf verwandte Elemente zuzugreifen, die den gleichen Dateinamen, jedoch andere Erweiterungen aufweisen. Das Feature besteht aus zwei Teilen: a) eine Liste aller zugehörigen Erweiterungen in der app `Info.plst` Datei, b) des Sandkastens mitteilen, welche Vorgänge die app mit diesen Dateien ausführt.

Es gibt zwei Szenarien, in denen dies sinnvoll ist:

1. Die app muss in der Lage, eine andere Version der Datei (mit einer neuen Erweiterung) gespeichert sein. Exportieren Sie beispielsweise eine `.txt` Datei wird in einem `.pdf` Datei. Um diese Situation zu behandeln, müssen Sie verwenden eine `NSFileCoordinator` auf die Datei zuzugreifen. Sie rufen die `WillMove(fromURL, toURL)` Methode zuerst, verschieben Sie die Datei in die neue Erweiterung, und rufen dann `ItemMoved(fromURL, toURL)`.
2. Die app muss eine Hauptdatei mit einer Erweiterung und mehrere unterstützende Dateien mit anderen Erweiterungen zu öffnen. Z. B. einen Film und eine Datei einen Untertitel aus. Verwenden einer `NSFilePresenter` für den Zugriff auf die sekundäre Datei. Geben Sie die Hauptdatei, die `PrimaryPresentedItemURL` -Eigenschaft und die sekundäre Datei in die `PresentedItemURL` Eigenschaft. Aufrufen, wenn die Hauptdatei geöffnet wird, die `AddFilePresenter` Methode der `NSFileCoordinator` Klasse, um die sekundäre Datei registrieren.

In beiden Szenarien, die app des **"Info.plist"** Datei muss die Dokumenttypen, die die app zu öffnen, kann zu deklarieren. Für einen beliebigen Dateityp hinzufügen der `NSIsRelatedItemType` (mit einem Wert von `YES`) dem Eintrag in der `CFBundleDocumentTypes` Array.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Öffnen und Speichern von Dialogfeld-Verhalten mit Sandbox-apps

Die folgenden sind Begrenzungen für die `NSOpenPanel` und `NSSavePanel` , wenn sie aus einer Sandbox Xamarin.Mac app aufrufen:

- Sie können nicht programmgesteuert Aufrufen der **OK** Schaltfläche.
- Sie können nicht programmgesteuert ändern, die Auswahl des Benutzers in einem `NSOpenSavePanelDelegate`.

Darüber hinaus sind die folgenden Änderungen für die Vererbung vorhanden:

- **Nicht-Sandbox-App** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Sicherheit bezogenen Lesezeichen und beständige Ressourcenzugriff

Wie bereits erwähnt, kann eine Sandbox Xamarin.Mac-Anwendung eine Datei oder Ressource außerhalb des Containers über direkte Benutzerinteraktion (wie durch PowerBox angegeben wird) zugreifen. Jedoch Zugriff auf diese Ressourcen nicht automatisch über die app gestartet wird oder das System neu gestartet wird beibehalten.

Mithilfe von _Security-Scoped Lesezeichen_, eine Sandbox Xamarin.Mac Anwendung Benutzerabsicht beibehalten und verwalten Zugriff auf die angegebenen Ressourcen nach einer app neu starten kann.

#### <a name="security-scoped-bookmark-types"></a>Sicherheit im Bereich einer Textmarke Typen

Bei der Arbeit mit Security-Scoped Lesezeichen und persistenten Zugriff auf Ressourcen stehen zwei sistine Anwendungsfälle:

- **Ein Lesezeichen App-Scoped bietet persistenten Zugriff auf eine vom Benutzer angegebene Datei oder einen Ordner.** 

    Z. B. Sandbox Xamarin.Mac zulässt, mit ein externes Dokument zur Bearbeitung öffnen (mithilfe einer `NSOpenPanel`), die app ein App-Scoped Lesezeichen erstellen, sodass es in Zukunft wieder dieselbe Datei zugreifen kann.
- **Ein Lesezeichen Document-Scoped bietet es sich um ein bestimmtes Dokument persistenten Zugriff auf eine untergeordnete Datei.** 

Eine video Bearbeitung app, die eine Projektdatei erstellt hat beispielsweise Zugriff auf die einzelnen Bilder, Videoclips und Audiodateien, die später zu einem einzigen Film zusammengefasst werden können.

Wenn der Benutzer eine Ressourcendatei in das Projekt importiert (über eine `NSOpenPanel`), die app erstellt ein Document-Scoped Lesezeichen für das Element, das in das Projekt gespeichert ist, damit die Datei immer an die app zugänglich ist.

Ein Lesezeichen Document-Scoped kann von jeder Anwendung gelöst werden, die die Lesezeichen-Daten und das Dokument selbst öffnen können. Unterstützt die Portabilität, sodass der Benutzer die Projektdateien an einen anderen Benutzer senden und having-alle Lesezeichen für diese ebenfalls funktionieren.

> [!IMPORTANT]
> Ein Lesezeichen Document-Scoped können _nur_ zeigen Sie auf eine einzelne Datei und nicht auf einen Ordner, und diese Datei kann nicht an einem Speicherort, der vom System verwendet werden (z. B. `/private` oder `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Mithilfe von Lesezeichen im Bereich der Sicherheit

Geben Sie entweder Security-Scoped Lesezeichen müssen, Sie die folgenden Schritte ausführen:

1. **Legen Sie die entsprechenden Berechtigungen in der Xamarin.Mac-app, die Security-Scoped Lesezeichen verwenden, muss** -For App-Scoped-Lesezeichen festgelegt werden, die `com.apple.security.files.bookmarks.app-scope` Berechtigung-Taste, um `true`. Legen Sie für Document-Scoped Lesezeichen, die `com.apple.security.files.bookmarks.document-scope` Berechtigung-Taste, um `true`.
2. **Erstellen Sie eine Textmarke Security-Scoped** -müssen Sie dies ausführen, für alle Dateien oder Ordner, die der Benutzer Zugriff auf bereitgestellt wurde (über `NSOpenPanel` z. B.), dass die Anwendung persistenten Zugriff benötigen. Verwenden der `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` Methode der `NSUrl` Klasse, um das Lesezeichen zu erstellen.
3. **Beheben Sie das Lesezeichen Security-Scoped** – Wenn die app auf die Ressource erneut aus (z. B. nach dem Neustart Zugriff muss) muss diese an eine URL im Bereich der Sicherheit das Lesezeichen zu beheben. Verwenden der `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` Methode der `NSUrl` Klasse, um das Lesezeichen zu beheben.
4. **Explizit benachrichtigt das System, das Sie den Zugriff auf Datei aus der URL Security-Scoped möchten** – dieser Schritt muss ausgeführt werden, sofort nach dem Abrufen der oben genannten Security-Scoped-URL oder, wenn Sie später haben Zugriff auf die Ressource wieder aufnehmen möchten aufgegeben Ihrer darauf zugreifen. Rufen Sie die `StartAccessingSecurityScopedResource ()` Methode der `NSUrl` Klasse, um den Zugriff auf eine Security-Scoped URL starten.
5. **Explizit benachrichtigt das System, das Sie haben Zugriff auf die Datei aus der URL Security-Scoped** -so bald wie möglich, sollten Sie das System informieren, wenn die app Zugriff auf die Datei nicht mehr benötigt, (z. B., wenn der Benutzer es schließt). Rufen Sie die `StopAccessingSecurityScopedResource ()` Methode der `NSUrl` Klasse, um den Zugriff auf eine URL Security-Scoped beenden.

Nachdem Sie den Zugriff auf eine Ressource aufgegeben haben, müssen Sie mit Schritt 4 erneut aus, um den Zugriff wiederherzustellen zurückgeben. Die app Xamarin.Mac neu gestartet wird, müssen Sie zu Schritt 3 zurück und das Lesezeichen wieder zu beheben.

> [!IMPORTANT]
> Fehler beim Zugriff auf Security-Scoped URL-Ressourcen freizugeben, bewirkt eine Xamarin.Mac app Kernel-Ressourcen zu gelangen. Daher kann die app nicht mehr werden Speicherorten im Dateisystem an ihren Container hinzufügen, bis er neu gestartet wird.

### <a name="the-app-sandbox-and-code-signing"></a>Die App Sandkasten und Signieren von code

Nachdem Sie den App-Sandkasten aktivieren und die speziellen Anforderungen für Ihre app Xamarin.Mac (über Berechtigungen) aktivieren, müssen Sie code für Signieren des Projekts für den Sandkasten wirksam wird. Führen Sie die Codesignatur, da die Berechtigungen für App-Sandkasten erforderliche Signatur für die app verknüpft sind. 

MacOS erzwingt eine Verknüpfung zwischen einer app-Container und seiner Codesignatur auf diese Weise keine andere Anwendung diesen Container zugreifen kann, selbst wenn sie die Paket-ID-apps-spoofing ist Dieser Mechanismus funktioniert wie folgt:

1. Wenn das System die app-Container erstellt, wird ein _Access Control List_ (ACL) für diesen Container. In der Liste der anfänglichen Zugriffssteuerungseintrag enthält der app _Anforderung festgelegten_ (DR), die beschreibt, wie künftige Versionen der Anwendung erkannt werden kann (wenn es aktualisiert wurde).
2. Jedes Mal eine Anwendung mit der gleichen Paket-ID gestartet wird, überprüft das System, dass die Signatur für die app-Code in einen der Einträge in die ACL des Containers angegebenen festgelegten Anforderungen entspricht. Wenn das System keine Übereinstimmung findet, verhindert er das Starten die app.

Code Signing funktioniert folgendermaßen:

1. Vor dem Erstellen des Projekts Xamarin.Mac, erhalten Sie ein Zertifikat für die Entwicklung, einen Verteilungspunkt-Dienstzertifikat und ein Entwicklerzertifikat-ID aus dem Apple-Entwicklerportal.
2. Wenn Mac App Store die app Xamarin.Mac verteilt, ist es mit einer Codesignatur von Apple signiert.

Beim Testen und Debuggen, müssen Sie eine Version der Anwendung Xamarin.Mac verwenden werden, dass Sie angemeldet (die dienen zum Erstellen von App-Container). Später, wenn Sie testen oder die Version aus dem Apple App Store installieren möchten, er wird mit der Apple-Signatur signiert werden, und treten Fehler auf (da es die gleiche Codesignatur wie der ursprüngliche App-Container vorhanden ist) zu starten. In diesem Fall wird ein Bericht zum Systemabsturz erstellt ähnlich der folgenden angezeigt:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Um dieses Problem zu beheben, müssen Sie zum Anpassen des ACL-Eintrags, um auf die Apple signierte Version der app zu verweisen.

Weitere Informationen zum Erstellen und zum Herunterladen der Provisioning Profile für Sandkasten erforderlich sind, finden Sie unter der [signieren und Bereitstellen der App](#Signing_and_Provisioning_the_App) obigen Abschnitt.

#### <a name="adjusting-the-acl-entry"></a>Anpassen des ACL-Eintrags

Damit wird die Apple signierte Version der app Xamarin.Mac ausführen, führen Sie folgende Schritte aus:

1. Öffnen Sie die Terminal-app (im `/Applications/Utilities`).
2. Öffnen Sie ein Finder-Fenster auf die Apple signierte Version der app Xamarin.Mac.
3. Typ `asctl container acl add -file ` in der Terminal-Fenster.
4. Ziehen Sie das Symbol für die Xamarin.Mac-app aus dem Finder-Fenster, und legen Sie sie auf der Terminal-Fenster.
5. Der vollständige Pfad zur Datei wird an den Befehl in der Terminal hinzugefügt werden.
6. Drücken Sie **EINGABETASTE** beim Ausführen des Befehls.

Die ACL des Containers enthält jetzt die designierten Code-Anforderungen für beide Versionen der app Xamarin.Mac und MacOS lässt jetzt entweder Version ausführen.

#### <a name="display-a-list-of-acl-code-requirements"></a>Zeigt eine Liste der Anforderungen der ACL-code

Sie können eine Liste der Anforderungen an die in der ACL eines Containers anzeigen, indem Sie wie folgt:

1. Öffnen Sie die Terminal-app (im `/Applications/Utilities`).
2. Geben Sie `asctl container acl list -bundle <container-name>` ein.
3. Drücken Sie **EINGABETASTE** beim Ausführen des Befehls.

Die `<container-name>` ist in der Regel die Paket-ID für die Xamarin.Mac-Anwendung.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Beim Entwerfen einer Xamarin.Mac-app für den App-Sandkasten

Es gibt ein häufigen Workflow, der beim Entwerfen einer Xamarin.Mac-app für den Sandkasten für die App ausgeführt werden sollen. Dies bedeutet, dass die Einzelheiten der Implementierung Sandkasten in einer app beabsichtigen, die für die angegebene app Funktionalität eindeutig sein.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Umfasst sechs Schritte Übernahme der App-Sandkasten

In der Regel beim Entwerfen einer Xamarin.Mac-app für den Sandkasten für die App besteht aus den folgenden Schritten:

1. Bestimmt, ob die app für Sandkasten geeignet ist.
2. Entwerfen einer Strategie für Entwicklung und Verteilung.
3. Inkompatibilitäten API zu beheben.
4. Das Projekt Xamarin.Mac die erforderliche App Sandkasten Berechtigungen zuweisen.
5. Fügen Sie die Berechtigung Trennung mit XPC hinzu.
6. Implementieren Sie eine Migrationsstrategie.

> [!IMPORTANT]
> Sie müssen nicht nur Sandbox der Hauptausführungsdatei Sie app-Bündel, aber auch jedes enthalten Helper app oder Tool in der Paketdatei. Dies ist erforderlich für eine beliebige app aus dem Mac App Store verteilt und, falls möglich, sollte für alle anderen Arten von app-Verteilung vorgenommen werden.

Eine Liste aller ausführbare Binärdateien in einen Xamarin.Mac-app-Bündel Geben Sie den folgenden Befehl in Terminaldienste:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Wobei `[Your-App-Bundle]` ist der Name und Pfad der Anwendung-Paket.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Bestimmen Sie, ob eine app Xamarin.Mac für Sandkasten geeignet ist.

Die meisten Xamarin.Mac apps sind vollständig kompatibel mit den App-Sandkasten und daher für die Sandkasten geeignet ist. Wenn die app ein Verhalten erforderlich, die den Sandkasten für die App nicht zugelassen wird ist, sollten Sie einen alternativen Ansatz.

Wenn Ihre app eines der folgenden Verhaltensweisen benötigt, ist es nicht kompatibel mit der Sandbox-App:

- **Autorisierungsdienste** -mit dem App Sandkasten kann nicht mit den Funktionen, die in beschriebenen arbeiten [Autorisierung Services C#-Referenz](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Eingabehilfen-APIs** – Sie können keine Sandkasten unterstützende z. B. Sprachausgaben oder apps, die anderen Anwendungen zu steuern.
- **Apple-Ereignisse an beliebige Apps senden** -Wenn die app erforderlich ist, Senden von Apple-Ereignissen an eine app unbekannt oder beliebige, darf nicht Sandbox sein. Eine bekannte Liste von apps aufgerufen die app kann noch Sandbox sein und müssen die Berechtigungen die aufgerufene app-Liste enthalten.
- **Info Wörterbücher in verteilten Benachrichtigungen senden, zu anderen Aufgaben** -mit dem App Sandkasten nicht einbezogen werden eine `userInfo` Wörterbuch beim Veröffentlichen einer `NSDistributedNotificationCenter` Objekt für andere Aufgaben messaging.
- **Laden von Erweiterungen Kernel** -Laden von Erweiterungen Kernel ist nicht zulässig, von der Sandbox-App.
- **Simulieren von Eingabe Benutzer öffnen und speichern Sie Dialogfelder** - programmgesteuert bearbeiten öffnen oder Speichern Dialogfelder zu simulieren, oder Ändern von Benutzereingaben von der App-Sandkasten unzulässig.
- **Zugreifen auf oder auf andere Apps Voreinstellungen** -bearbeiten die Einstellungen von anderen apps ist der App-Sandkasten verboten.
- **Konfigurieren von Netzwerkeinstellungen** -Netzwerkeinstellungen bearbeiten ist nicht zulässig, von der Sandbox-App.
- **Beenden andere Apps** -mit dem App-Sandkasten verhindert die `NSRunningApplication` an andere apps zu beenden.

### <a name="resolving-api-incompatibilities"></a>Beheben von API-Inkompatibilitäten

Beim Entwerfen einer Xamarin.Mac-app für den App-Sandkasten können Inkompatibilitäten mit der Nutzung von einigen MacOS APIs auftreten.

Hier sind einige häufig auftretende Probleme und Aufgaben aus, denen Sie ausführen können, um diese zu beheben:

- **Sie öffnen, speichern und Überwachen von Dokumenten** – Wenn Sie Dokumente, die eine beliebige Technologie außer verwenden verwalten `NSDocument`, Sie sollten zu diesem wechseln, aufgrund der integrierten Unterstützung für den Sandkasten für die App. `NSDocument` automatisch mit PowerBox funktioniert, und bietet Unterstützung für die Beibehaltung von Dokumenten in der Sandbox, wenn der Benutzer im Finder bewegt.
- **Behalten Sie den Zugriff auf Systemressourcen Datei** – Wenn die Xamarin.Mac-App hängt persistenter Zugriff auf Ressourcen außerhalb von seinem Container Sicherheit bezogenen Lesezeichen für den Zugriff verwenden.
- **Erstellen Sie ein Element für die Anmeldung für eine App** -mit dem App Sandkasten kann nicht erstellen ein Anmeldenamens mit `LSSharedFileList` noch können Sie den Status der starten-Dienste, die mit bearbeiten `LSRegisterURL`. Verwenden der `SMLoginItemSetEnabled` funktioniert wie beschrieben in Apples [Hinzufügen von Login-Elementen, die mit dem Service Management Framework](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) Dokumentation.
- **Zugreifen auf Benutzerdaten** – Wenn Sie POSIX-Funktionen wie z. B. `getpwuid` können Verzeichnisdienste Basisverzeichnis des Benutzers abrufen, erwägen Sie Kakao oder Core Foundation Symbole wie z. B. `NSHomeDirectory`.
- **Zugreifen auf die Einstellungen von anderen Apps** : Da die App-Sandkasten Pfad APIs suchen, um die app-Container leitet Voreinstellungen erfolgt innerhalb dieses Containers ändern und den Zugriff auf eine andere apps Einstellungen in nicht zulässig. 
- **Verwenden von HTML5 eingebetteten Videos in Webansichten** -Wenn die Xamarin.Mac-app WebKit verwendet, um eingebettete HTML5-Videos wiederzugeben, müssen Sie auch die app für das Framework AV Foundation verknüpfen. Der App-Sandkasten verhindert CoreMedia Play diese Videos, die andernfalls.

### <a name="applying-required-app-sandbox-entitlements"></a>Anwenden von erforderlichen Berechtigungen für App-Sandkasten

Sie müssen die Berechtigungen für jede Anwendung Xamarin.Mac bearbeiten, die Sie verwenden möchten, führen in der Sandbox-App und überprüfen Sie die **App-Sandkasten aktivieren** Kontrollkästchen.

Basierend auf den Funktionen der app, müssen Sie zur Aktivierung von anderen Berechtigungen den Zugriff auf Funktionen des Betriebssystems oder Ressourcen. App-Sandkasten funktioniert am besten, wenn Sie die Berechtigungen minimieren, die Sie anfordern, die für die Mindestanforderungen zur Ausführung Ihrer Anwendung aktiviere Berechtigungen so einfach nach dem Zufallsprinzip.

Um die Berechtigungen zu bestimmen, eine Xamarin.Mac app erfordert, führen Sie folgende Schritte aus:

1. Aktivieren Sie der App-Sandkasten, und führen Sie die app Xamarin.Mac.
2. Führen Sie die Funktionen der App.
3. Öffnen Sie die Konsolen-app (verfügbar in `/Applications/Utilities`) und suchen Sie nach `sandboxd` Verstöße in den **alle Nachrichten** Protokoll.
4. Für jede `sandboxd` Verletzung, beheben Sie das Problem mithilfe der app-Container anstelle von anderen Speicherorten im Dateisystem oder beim Anwenden von App-Sandkasten-Berechtigungen für den Zugriff auf eingeschränkte Funktionen des Betriebssystems.
5. Führen Sie erneut aus, und Testen Sie alle Xamarin.Mac app Funktionen erneut.
6. Wiederholen Sie diesen Schritt, bis alle `sandboxd` Verstöße behoben wurden.

### <a name="add-privilege-separation-using-xpc"></a>Fügen Sie Trennung von Berechtigungen, die XPC hinzu

Ein Xamarin.Mac-app für den Sandkasten für die App entwickeln, betrachten Sie die app-Verhalten im Hinblick auf Zugriffsrechte und sollten Sie erwägen mit hohem Risiko Vorgänge in ihrer eigenen XPC Dienste unterteilen.

Weitere Informationen finden Sie auf der Apple [erstellen XPC Services](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) und [-Daemons und Dienste Programmierhandbuch](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Implementieren Sie eine Migrationsstrategie

Wenn Sie eine neue, Sandbox Version einer Anwendung Xamarin.Mac, die nicht zuvor Sandbox war freigeben, müssen Sie sicherstellen, dass aktuelle Benutzern einen nahtlosen Upgradepfad zu gewährleisten. 

 Lesen Sie weitere Informationen zum Implementieren eines Containers Migration Manifests Apple [Migrieren einer App auf einem Sandkasten](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) Dokumentation.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat einer ausführlichen Übersicht über das Verwenden einer Sandbox eine Xamarin.Mac Anwendung übernommen. Zunächst haben wir eine einfach Xamarin.Mac app aus, um die Grundlagen der App Sandbox anzeigen. Als Nächstes wir wurde gezeigt, wie Sandkasten Verstöße zu beheben. Klicken Sie dann, wir haben einen detaillierten betrachten Sie die App-Sandkasten und schließlich beim Entwerfen einer Xamarin.Mac-app für den App-Sandkasten erläutert.



## <a name="related-links"></a>Verwandte Links

- [Veröffentlichen im App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Informationen zu App-Sandkasten](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Häufige Probleme mit der App-Sandkasten](https://developer.apple.com/library/content/qa/qa1773/_index.html)
