---
title: Sandboxing einer xamarin. Mac-app
description: In diesem Artikel wird das Sandkasten einer xamarin. Mac-Anwendung für die Veröffentlichung im App Store behandelt. Sie deckt alle Elemente ab, die in Sandboxing fließen, z. b. Container Verzeichnisse, Berechtigungen, Benutzer seitig festgelegte Berechtigungen, Berechtigungs Trennung und Kernel Erzwingung.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 02059c43d26c2e685abd685231fe5faf3d7a6bfe
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030113"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Sandboxing einer xamarin. Mac-app

in diesem Artikel wird beschrieben, wie Sie eine xamarin. Mac-Anwendung im App Store veröffentlichen.  _Sie deckt alle Elemente ab, die in Sandboxing fließen, z. b. Container Verzeichnisse, Berechtigungen, Benutzer seitig festgelegte Berechtigungen, Berechtigungs Trennung und Kernel Erzwingung._

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie die Möglichkeit, eine Anwendung in einem Sandkasten zu verwenden, wie bei der Arbeit mit Ziel-C oder Swift.

[![Ein Beispiel für die laufende App](sandboxing-images/intro01.png "Ein Beispiel für die laufende App")](sandboxing-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Sandbox in einer xamarin. Mac-Anwendung und alle Elemente erläutert, die in Sandkasten einfließen: Container Verzeichnisse, Berechtigungen, Benutzer seitig ermittelte Berechtigungen, Berechtigungs Trennung und Kernel Erzwingung. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die`Register`und`Export`Attribute erläutert, die zum Verknüpfen der C# Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

## <a name="about-the-app-sandbox"></a>Informationen zum App-Sandkasten

Der APP-Sandkasten bietet eine starke Verteidigung gegen Schäden, die durch die Durchführung einer bösartigen Anwendung auf einem Mac verursacht werden können, indem der Zugriff auf Systemressourcen durch eine Anwendung eingeschränkt wird.

Eine nicht-Sandkasten Anwendung verfügt über die vollständigen Rechte des Benutzers, der die app ausgeführt wird, und kann auf alles zugreifen, was der Benutzer möglich ist. Wenn die APP Sicherheitslücken (oder ein beliebiges Framework verwendet) enthält, kann ein Hacker diese Schwächen potenziell ausnutzen und die APP verwenden, um die Kontrolle über den Mac zu übernehmen, auf dem er ausgeführt wird.

Durch die Beschränkung des Zugriffs auf Ressourcen pro Anwendung stellt eine Sandbox-App eine Reihe von Schutz vor Diebstahl, Beschädigung oder böswilliger Absicht für den Teil einer Anwendung dar, die auf dem Computer des Benutzers ausgeführt wird.

Der APP-Sandkasten ist eine in macOS integrierte Zugriffs Steuerungs Technologie (auf Kernel Ebene erzwungen), die eine doppelte Strategie bietet:

1. Mithilfe der APP-Sandbox kann der Entwickler beschreiben, _wie_ eine Anwendung mit dem Betriebssystem interagieren soll. auf diese Weise erhalten Sie nur die Zugriffsrechte, die erforderlich sind, um die Aufgabe zu erledigen.
2. Der APP-Sandkasten ermöglicht dem Benutzer, über die Dialogfelder öffnen und speichern, Drag & Drop-Vorgänge und andere gängige Benutzerinteraktionen nahtlos weiteren Zugriff auf das System zu gewähren.

### <a name="preparing-to-implement-the-app-sandbox"></a>Vorbereiten der Implementierung der APP-Sandbox

Die Elemente der APP-Sandbox, die im Artikel ausführlich erläutert werden, lauten wie folgt:

- Container Verzeichnisse
- Berechtigten
- Vom Benutzer festgelegte Berechtigungen
- Berechtigungs Trennung
- Kernel Erzwingung

Nachdem Sie diese Details verstanden haben, können Sie einen Plan für die Übernahme der APP-Sandbox in ihrer xamarin. Mac-Anwendung erstellen.

Zuerst müssen Sie bestimmen, ob Ihre Anwendung ein guter Kandidat für das Sandkasten ist (die meisten Anwendungen sind). Als nächstes müssen Sie alle API-Inkompatibilitäten auflösen und ermitteln, welche Elemente der APP-Sandbox benötigt werden. Betrachten Sie schließlich die Verwendung der Berechtigungs Trennung, um die Verteidigungs Stufe Ihrer Anwendung zu maximieren.

Bei der Übernahme der APP-Sandbox sind einige Dateisystem Speicherorte, die von Ihrer Anwendung verwendet werden, unterschiedlich. Die Anwendung verfügt insbesondere über ein Container Verzeichnis, das für Anwendungs Unterstützungs Dateien, Datenbanken, Caches und andere Dateien verwendet wird, die keine Benutzer Dokumente sind. Sowohl macOS als auch Xcode bieten Unterstützung für die Migration dieser Dateitypen von Ihren Legacy-Speicherorten zum Container.

## <a name="sandboxing-quick-start"></a>Sandkasten-Schnellstart

In diesem Abschnitt erstellen wir eine einfache xamarin. Mac-app, die eine Webansicht verwendet (die eine Netzwerkverbindung erfordert, die unter Sandkasten eingeschränkt ist, sofern nicht ausdrücklich angefordert) als Beispiel für den Einstieg in den App-Sandkasten.

Wir überprüfen, ob die Anwendung tatsächlich in einem Sandkasten ist, und erfahren, wie Sie häufige Fehler bei der APP-Sandbox beheben und beheben.

### <a name="creating-the-xamarinmac-project"></a>Erstellen des xamarin. Mac-Projekts

Gehen Sie folgendermaßen vor, um das Beispiel Projekt zu erstellen:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf die neue Projekt Mappe **.** klicken.
2. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Mac** - > **App** > **Cocoa-App**aus:

    [![Erstellen einer neuen Cocoa-App](sandboxing-images/sample01.png "Erstellen einer neuen Cocoa-App")](sandboxing-images/sample01-large.png#lightbox)
3. Klicken Sie auf die Schaltfläche **weiter** , geben Sie `MacSandbox` als Projektnamen ein, und klicken Sie auf die Schaltfläche **Erstellen**

    [![Eingeben des App-namens](sandboxing-images/sample02.png "Eingeben des App-namens")](sandboxing-images/sample02-large.png#lightbox)
4. Doppelklicken Sie im **Lösungspad**auf die Datei **Main. Storyboard** , um Sie zur Bearbeitung in Xcode zu öffnen:

    [![Bearbeiten des Haupt Storyboards](sandboxing-images/sample03.png "Bearbeiten des Haupt Storyboards")](sandboxing-images/sample03-large.png#lightbox)
5. Ziehen Sie eine **Webansicht** auf das Fenster, geben Sie die Größe des Inhalts Bereichs an, und legen Sie fest, dass Sie mit dem Fenster vergrößert und verkleinert werden soll:

    [![Hinzufügen einer Webansicht](sandboxing-images/sample04.png "Hinzufügen einer Webansicht")](sandboxing-images/sample04-large.png#lightbox)
6. Erstellen Sie ein Outlet für die Webansicht namens `webView`:

    [![Erstellen eines neuen Outlets](sandboxing-images/sample05.png "Erstellen eines neuen Outlets")](sandboxing-images/sample05-large.png#lightbox)
7. Kehren Sie zu Visual Studio für Mac zurück, und doppelklicken Sie in der **Lösungspad** auf die Datei **ViewController.cs** , um Sie für die Bearbeitung zu öffnen.
8. Fügen Sie die folgende using-Anweisung hinzu: `using WebKit;`
9. Führen Sie die `ViewDidLoad`-Methode wie folgt aus:

    ```csharp
    public override void AwakeFromNib ()
    {
        base.AwakeFromNib ();
        webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
    }
    ```

10. Speichern Sie die Änderungen.

Führen Sie die Anwendung aus, und stellen Sie sicher, dass die Apple-Website wie folgt im-Fenster angezeigt wird:

[![Zeigt eine Beispiel-App-Run an](sandboxing-images/sample06.png "Zeigt eine Beispiel-App-Run an")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>Signieren und Bereitstellen der APP

Bevor wir die APP-Sandbox aktivieren können, müssen wir zuerst die xamarin. Mac-Anwendung bereitstellen und signieren.

Gehen Sie folgendermaßen vor:

1. Melden Sie sich beim Apple Developer Portal an:

    [![Anmelden beim Apple-Entwickler Portal](sandboxing-images/sign01.png "Anmelden beim Apple-Entwickler Portal")](sandboxing-images/sign01-large.png#lightbox)
2. Wählen Sie **Zertifikate, Bezeichner & profile**aus:

    [![Auswählen von Zertifikaten, Bezeichner & Profilen](sandboxing-images/sign02.png "Auswählen von Zertifikaten, Bezeichner & Profilen")](sandboxing-images/sign02-large.png#lightbox)
3. **Wählen Sie**unter **Mac-apps**die Option Bezeichner aus:

    [![Auswählen von bezeichlern](sandboxing-images/sign03.png "Auswählen von bezeichlern")](sandboxing-images/sign03-large.png#lightbox)
4. Erstellen Sie eine neue ID für die Anwendung:

    [![Erstellen einer neuen APP-ID](sandboxing-images/sign04.png "Erstellen einer neuen APP-ID")](sandboxing-images/sign04-large.png#lightbox)
5. Wählen Sie unter **Bereitstellungs profile**die Option **Entwicklung**:

    [![Auswählen der Entwicklung](sandboxing-images/sign05.png "Auswählen der Entwicklung")](sandboxing-images/sign05-large.png#lightbox)
6. Erstellen Sie ein neues Profil, und wählen Sie **Mac-app-Entwicklung**:

    [![Erstellen eines neuen Profils](sandboxing-images/sign06.png "Erstellen eines neuen Profils")](sandboxing-images/sign06-large.png#lightbox)
7. Wählen Sie die oben erstellte APP-ID aus:

    [![Auswählen der APP-ID](sandboxing-images/sign07.png "Auswählen der APP-ID")](sandboxing-images/sign07-large.png#lightbox)
8. Wählen Sie die Entwickler für dieses Profil aus:

    [![Hinzufügen von Entwicklern](sandboxing-images/sign08.png "Hinzufügen von Entwicklern")](sandboxing-images/sign08-large.png#lightbox)
9. Wählen Sie die Computer für dieses Profil aus:

    [![Auswählen der zulässigen Computer](sandboxing-images/sign09.png "Auswählen der zulässigen Computer")](sandboxing-images/sign09-large.png#lightbox)
10. Benennen Sie das Profil:

    [![Benennen eines Namens für das Profil](sandboxing-images/sign10.png "Benennen eines Namens für das Profil")](sandboxing-images/sign10-large.png#lightbox)
11. Klicken Sie auf die Schaltfläche **done** .

> [!IMPORTANT]
> In einigen Fällen müssen Sie das neue Bereitstellungs Profil direkt aus dem Apple-Entwickler Portal herunterladen und darauf doppelklicken, um es zu installieren. Möglicherweise müssen Sie Visual Studio für Mac auch abbrechen und neu starten, bevor Sie auf das neue Profil zugreifen können.

Als nächstes müssen wir die neue APP-ID und das Profil auf dem Entwicklungs Computer laden. Führen Sie die folgenden Schritte aus:

1. Starten Sie Xcode, und wählen Sie im **Xcode** -Menü die Option **Einstellungen** aus:

    ![Bearbeiten von Konten in Xcode](sandboxing-images/sign11.png "Bearbeiten von Konten in Xcode")
2. Klicken Sie auf die Schaltfläche **Details anzeigen...** :

    ![Klicken auf die Schaltfläche Details anzeigen](sandboxing-images/sign12.png "Klicken auf die Schaltfläche Details anzeigen")
3. Klicken Sie auf die Schaltfläche **Aktualisieren** (in der unteren linken Ecke).
4. Klicken Sie auf die Schaltfläche **done** .

Als nächstes müssen wir die neue APP-ID und das Bereitstellungs Profil in unserem xamarin. Mac-Projekt auswählen. Führen Sie die folgenden Schritte aus:

1. Doppelklicken Sie im **Lösungspad**auf die Datei **Info. plist** , um Sie zur Bearbeitung zu öffnen.
2. Stellen Sie sicher, dass der **Bündel Bezeichner** mit der oben erstellten APP-ID übereinstimmt (Beispiel: `com.appracatappra.MacSandbox`):

    [![Bearbeiten der Bündel-ID](sandboxing-images/sign13.png "Bearbeiten der Bündel-ID")](sandboxing-images/sign13-large.png#lightbox)
3. Doppelklicken Sie dann auf die Datei "Berechtigungsdatei" **. plist** , und stellen Sie sicher, dass der **icloud-Schlüssel-Wert-Speicher** und die **icloud-Container** mit der oben erstellten APP-ID identisch sind (Beispiel: `com.appracatappra.MacSandbox`)

    [![Bearbeiten der Datei "Berechtigungen. plist"](sandboxing-images/sign17.png "Bearbeiten der Datei "Berechtigungen. plist"")](sandboxing-images/sign17-large.png#lightbox)
4. Speichern Sie die Änderungen.
5. Doppelklicken Sie im **Lösungspad**auf die Projektdatei, um die Optionen für die Bearbeitung zu öffnen:

    ![Die Optionen der Projekt Mappe werden bearbeitet.](sandboxing-images/sign14.png "Die Optionen der Projekt Mappe werden bearbeitet.")
6. Wählen Sie **Mac-Signatur**aus, und aktivieren Sie dann **das Anwendungs Bündel Signieren** und **das Installationspaket Signieren**. Wählen Sie unter **Bereitstellungs Profil**das oben erstellte aus:

    ![Festlegen des Bereitstellungs Profils](sandboxing-images/sign15.png "Festlegen des Bereitstellungs Profils")
7. Klicken Sie auf die Schaltfläche **done** .

> [!IMPORTANT]
> Möglicherweise müssen Sie Visual Studio für Mac beenden und neu starten, damit die neue APP-ID und das Bereitstellungs Profil erkannt werden, das von Xcode installiert wurde.

#### <a name="troubleshooting-provisioning-issues"></a>Problembehandlung bei Bereitstellungs Problemen

An diesem Punkt sollten Sie versuchen, die Anwendung auszuführen und sicherzustellen, dass alles ordnungsgemäß signiert und bereitgestellt wurde. Wenn die APP weiterhin wie zuvor ausgeführt wird, ist alles gut. Falls ein Fehler auftritt, wird möglicherweise ein Dialogfeld wie das folgende angezeigt:

[![Ein Beispiel Dialogfeld für Bereitstellungs Probleme](sandboxing-images/sign16.png "Ein Beispiel Dialogfeld für Bereitstellungs Probleme")](sandboxing-images/sign16-large.png#lightbox)

Im folgenden finden Sie die häufigsten Ursachen für Bereitstellungs-und Signatur Probleme:

- Die APP-Bündel-ID stimmt nicht mit der APP-ID des ausgewählten Profils identisch.
- Die Entwickler-ID stimmt nicht mit der Entwickler-ID des ausgewählten Profils.
- Die UUID des zu testenden Mac wird nicht als Teil des ausgewählten Profils registriert.

Wenn ein Problem vorliegt, beheben Sie das Problem im Apple Developer Portal, aktualisieren Sie die Profile in Xcode, und führen Sie einen sauberen Build in Visual Studio für Mac aus.

### <a name="enable-the-app-sandbox"></a>Aktivieren der APP-Sandbox

Sie aktivieren den App-Sandkasten durch Aktivieren eines Kontrollkästchens in den Projektoptionen. Führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Lösungspad**auf die Datei " **Berechtigungen. plist** ", um Sie für die Bearbeitung zu öffnen.
2. Aktivieren Sie beide **Berechtigungen aktivieren** und **App-Sandboxing aktivieren**:

    [![Bearbeiten von Berechtigungen und Aktivieren von Sandkasten](sandboxing-images/sign17.png "Bearbeiten von Berechtigungen und Aktivieren von Sandkasten")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen.

An diesem Punkt haben Sie die APP-Sandbox aktiviert, aber Sie haben nicht den erforderlichen Netzwerk Zugriff für die Webansicht bereitgestellt. Wenn Sie die Anwendung jetzt ausführen, sollte ein leeres Fenster angezeigt werden:

[![Die Blockierung des Webzugriffs wird angezeigt.](sandboxing-images/sample08.png "Die Blockierung des Webzugriffs wird angezeigt.")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Überprüfen, ob die APP Sandkasten ist

Abgesehen vom Verhalten beim Blockieren von Ressourcen gibt es drei Hauptmethoden, um zu ermitteln, ob eine xamarin. Mac-Anwendung erfolgreich in einen Sandkasten konvertiert wurde:

1. Überprüfen Sie im Finder den Inhalt des Ordners "`~/Library/Containers/`": Wenn die APP Sandkasten ist, wird ein Ordner mit dem Namen wie die Bündel-ID Ihrer App angezeigt (Beispiel: `com.appracatappra.MacSandbox`):

    [![Öffnen des Pakets der APP](sandboxing-images/sample09.png "Öffnen des Pakets der APP")](sandboxing-images/sample09-large.png#lightbox)
2. Im System sehen Sie, dass die APP im Aktivitäts Monitor als Sandkasten angezeigt wird:
    - Starten Sie den Aktivitäts Monitor (unter `/Applications/Utilities`).
    - Wählen Sie > **Spalten** **anzeigen** aus, und stellen Sie sicher, dass das Menü Element **Sandbox** aktiviert ist.
    - Stellen Sie sicher, dass die Sandbox-Spalte `Yes` für Ihre Anwendung liest:

    [![Überprüfen der APP im Aktivitäts Monitor](sandboxing-images/sample10.png "Überprüfen der APP im Aktivitäts Monitor")](sandboxing-images/sample10-large.png#lightbox)
3. Überprüfen Sie, ob die Binärdatei der APP Sandkasten ist:
    - Starten Sie die Terminal-app.
    - Navigieren Sie zum `bin` Verzeichnis Anwendungen.
    - Führen Sie den folgenden Befehl aus: `codesign -dvvv --entitlements :- executable_path` (wobei `executable_path` der Pfad zu Ihrer Anwendung ist):

    [![Überprüfen der app in der Befehlszeile](sandboxing-images/sample11.png "Überprüfen der app in der Befehlszeile")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Debuggen einer Sandbox-App

Der Debugger stellt über TCP eine Verbindung mit xamarin. Mac-apps her. Dies bedeutet, dass beim Aktivieren von Sandboxing standardmäßig keine Verbindung mit der APP hergestellt werden kann. Wenn Sie also versuchen, die APP auszuführen, ohne dass die entsprechenden Berechtigungen aktiviert sind, erhalten Sie die Fehlermeldung *"Es konnte keine Verbindung mit dem Debugger hergestellt werden."* .

[![Festlegen der erforderlichen Optionen](sandboxing-images/debug01.png "Festlegen der erforderlichen Optionen")](sandboxing-images/debug01-large.png#lightbox)

Die Berechtigung **ausgehende Netzwerkverbindungen zulassen (Client)** ist die Berechtigung, die für den Debugger erforderlich ist. durch Aktivieren dieser Option wird das Debugging normal zugelassen. Da Sie nicht ohne diese Debuggen können, haben wir das `CompileEntitlements` Ziel für `msbuild` aktualisiert, um diese Berechtigung automatisch den Berechtigungen für jede APP hinzuzufügen, die nur für Debugbuilds in einem Sandkasten enthalten ist. Releasebuilds sollten die Berechtigungen unverändert verwenden, die in der Berechtigungsdatei angegeben sind.

### <a name="resolving-an-app-sandbox-violation"></a>Auflösen einer APP Sandbox-Verletzung

Eine APP-Sandbox-Verletzung tritt auf, wenn eine Sandbox-xamarin. Mac-Anwendung versucht, auf eine Ressource zuzugreifen, die nicht explizit zulässig ist. Beispielsweise kann unsere Webansicht die Apple-Website nicht mehr anzeigen.

Die häufigste Quelle für App-Sandkasten Verletzungen tritt auf, wenn die in Visual Studio für Mac angegebenen Berechtigungseinstellungen nicht den Anforderungen der Anwendung entsprechen. Wieder zurück zum Beispiel gibt es die fehlenden Netzwerk Verbindungs Berechtigungen, mit denen die Webansicht weiterhin funktioniert.

#### <a name="discovering-app-sandbox-violations"></a>Erkennen von App-Sandkasten Verstößen

Wenn Sie vermuten, dass in ihrer xamarin. Mac-Anwendung eine Verletzung der APP-Sandbox auftritt, können Sie das Problem am schnellsten mithilfe der **Konsolen** -App ermitteln.

Führen Sie folgende Schritte aus:

1. Kompilieren Sie die betreffende APP, und führen Sie Sie aus Visual Studio für Mac aus.
2. Öffnen Sie die **Konsolen** Anwendung (von `/Applications/Utilties/`).
3. Wählen Sie in der Rand Leiste **alle Meldungen** aus, und geben Sie `sandbox` in die Suche ein:

    [![Ein Beispiel für ein Sandkasten Problem in der-Konsole](sandboxing-images/resolve01.png "Ein Beispiel für ein Sandkasten Problem in der-Konsole")](sandboxing-images/resolve01-large.png#lightbox)

In der obigen Beispiel-App können Sie sehen, dass die Kernal den `network-outbound`-Datenverkehr aufgrund der APP-Sandbox blockiert, da wir das Recht nicht angefordert haben.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Beheben von App-Sandkasten Verstößen mit Berechtigungen

Nachdem Sie nun gesehen haben, wie Sie nach App-Sandkasten Verletzungen suchen, sehen wir uns an, wie Sie durch die Anpassung der Berechtigungen unserer Anwendung gelöst werden können.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Lösungspad**auf die Datei " **Berechtigungen. plist** ", um Sie für die Bearbeitung zu öffnen.
2. Aktivieren Sie im Abschnitt **Berechtigungen** das Kontrollkästchen **ausgehende Netzwerkverbindungen zulassen (Client)** :

    [![Bearbeiten der Berechtigungen](sandboxing-images/sign17.png "Bearbeiten der Berechtigungen")](sandboxing-images/sign17-large.png#lightbox)
3. Speichern Sie die Änderungen an der Anwendung.

Wenn wir die oben genannten Schritte für unsere Beispiel-App durchführen und Sie dann erstellen und ausführen, wird der Webinhalt wie erwartet angezeigt.

## <a name="the-app-sandbox-in-depth"></a>Ausführliche Informationen zum App-Sandkasten

Die Zugriffs Steuerungsmechanismen, die von der APP-Sandbox bereitgestellt werden, sind nur wenige und leicht verständlich. Die Art und Weise, in der der APP-Sandkasten von jeder App übernommen wird, ist jedoch eindeutig und basiert auf den Anforderungen der app.

Wenn Sie Ihre xamarin. Mac-Anwendung am besten durch schädlichen Code ausnutzen möchten, benötigen Sie nur eine einzige Schwachstelle in der APP (oder einer der Bibliotheken oder Frameworks, die Sie nutzt), um die Kontrolle über die Interaktionen der APP mit dem Anlage.

Der APP-Sandkasten wurde entwickelt, um die Übernahme zu verhindern (oder den Schaden zu begrenzen, der verursacht werden kann), indem Sie die beabsichtigten Interaktionen der Anwendung mit dem System angeben können. Das System gewährt nur Zugriff auf die Ressource, die Ihre Anwendung benötigt, um Ihre Arbeit zu erledigen, und nichts anderes.

Wenn Sie für die APP-Sandbox entwerfen, entwerfen Sie das Szenario für einen ungünstigsten Fall. Wenn die Anwendung von schädlichem Code kompromittiert wird, ist Sie nur auf die Dateien und Ressourcen in der Sandbox der APP beschränkt.

### <a name="entitlements-and-system-resource-access"></a>Berechtigungen und Zugriff auf Systemressourcen

Wie bereits erwähnt, erhält eine xamarin. Mac-Anwendung, die nicht in einem Sandkasten erfolgt ist, die vollständigen Rechte und den Zugriff des Benutzers, der die app ausgeführt hat. Wenn Sie von schädlichem Code kompromittiert werden, kann eine nicht geschützte App als Agent für ein schädliches Verhalten fungieren, bei dem es sich um ein umfangreiches Potenzial für Schäden handelt.

Wenn Sie die APP-Sandbox aktivieren, entfernen Sie alle bis auf einen minimalen Berechtigungs Satz, den Sie dann nur Bedarfs abhängig mithilfe der Berechtigungen ihrer xamarin. Mac-app wieder aktivieren.

Sie ändern die APP-Sandkasten Ressourcen Ihrer Anwendung, indem Sie die Datei "Berechtigungsdatei" " **Berechtigungen. plist** " Bearbeiten und die erforderlichen Rechte in den Editor-Dropdown Feldern aktivieren:

[![Bearbeiten der Berechtigungen](sandboxing-images/sign17.png "Bearbeiten der Berechtigungen")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Container Verzeichnisse und Dateisystem Zugriff

Wenn die xamarin. Mac-Anwendung den App-Sandkasten annimmt, hat Sie Zugriff auf die folgenden Speicherorte:

- **Das App-Container-Verzeichnis** : bei der ersten Testlauf erstellt das Betriebssystem ein spezielles _Container Verzeichnis_ , in das alle Ressourcen zugreifen, auf das nur zugegriffen werden kann. Die APP erhält vollständigen Lese-/Schreibzugriff auf dieses Verzeichnis.
- **App-Gruppen Container Verzeichnisse** : Ihrer APP kann der Zugriff auf einen oder mehrere _Gruppen Container_ gewährt werden, die von apps in derselben Gruppe gemeinsam genutzt werden.
- **Benutzerdefinierte Dateien** : die Anwendung erhält automatisch Zugriff auf Dateien, die vom Benutzer explizit geöffnet oder gezogen und auf der Anwendung abgelegt werden.
- **Verwandte Elemente** : mit den entsprechenden Berechtigungen kann Ihre Anwendung Zugriff auf eine Datei mit demselben Namen, aber einer anderen Erweiterung haben. Beispielsweise ein Dokument, das sowohl als `.txt` Datei als auch als `.pdf`gespeichert wurde.
- **Temporäre Verzeichnisse, Befehlszeilen Tool Verzeichnisse und bestimmte weltweit lesbare Speicherorte** : Ihre APP hat unterschiedliche Zugriffsstufen auf Dateien an anderen klar definierten Speicherorten, wie im System angegeben.

#### <a name="the-app-container-directory"></a>Das App-Container Verzeichnis

Das App-Container Verzeichnis einer xamarin. Mac-Anwendung weist die folgenden Eigenschaften auf:

- Sie befindet sich an einem ausgeblendeten Speicherort im Stammverzeichnis des Benutzers (in der Regel `~Library/Containers`), und der Zugriff auf die `NSHomeDirectory`-Funktion (siehe unten) in der Anwendung ist möglich. Da Sie sich im Stammverzeichnis befindet, erhält jeder Benutzer einen eigenen Container für die app.
- Die APP hat uneingeschränkten Lese-/Schreibzugriff auf das Container Verzeichnis und alle darin enthaltenen Unterverzeichnisse und Dateien.
- Die meisten APIs für die Pfad Suche von macOS sind relativ zum Container der app. Der Container verfügt z. b. über eine eigene **Bibliothek** (Zugriff über `NSLibraryDirectory`), Unterverzeichnisse für **Anwendungsunterstützung** und **Einstellungen** .
- macOS erstellt und erzwingt die Verbindung zwischen der APP und dem zugehörigen Container per Code Signatur. Auch wenn eine andere APP versucht, die App mithilfe Ihres **Bündel Bezeichners**zu spodieren, kann Sie aufgrund der Code Signatur nicht auf den Container zugreifen.
- Der Container ist nicht für vom benutzergenerierte Dateien. Stattdessen handelt es sich um Dateien, die von der Anwendung verwendet werden, z. b. Datenbanken, Caches oder andere spezifische Datentypen.
- Für _Shoebox_ -Typen von apps (wie die Foto-APP von Apple) wird der Inhalt des Benutzers in den Container überlaufen.

> [!IMPORTANT]
> Leider verfügt xamarin. Mac noch nicht über eine 100%-API-Abdeckung (im Gegensatz zu xamarin. IOS), daher wurde die `NSHomeDirectory`-API in der aktuellen Version von xamarin. Mac nicht zugeordnet.

Als vorübergehende Problem Umgehung können Sie den folgenden Code verwenden:

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Das Container Verzeichnis der APP-Gruppe

Ab Mac macOS 10.7.5 (und höher) kann eine Anwendung mit der `com.apple.security.application-groups` Berechtigung auf einen gemeinsam genutzten Container zugreifen, der allen Anwendungen in der Gruppe gemeinsam ist. Sie können diesen freigegebenen Container für Nichtbenutzer seitige Inhalte wie z. b. Datenbanken oder andere Arten von Unterstützungs Dateien (z. b. Caches) verwenden.

Die Gruppen Container werden automatisch dem Sandbox Container jeder app hinzugefügt (wenn Sie Teil einer Gruppe sind) und werden unter `~/Library/Group Containers/<application-group-id>`gespeichert. Die Gruppen-ID _muss_ mit der Entwicklungs Team-ID und einem bestimmten Zeitraum beginnen, z. b.:

```plist
<team-id>.com.company.<group-name>
```

Weitere Informationen finden Sie unter [Hinzufügen einer Anwendung zu einer Anwendungs Gruppe](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) in der [Referenz zu Berechtigungs Schlüsseln](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)in Apple.

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>PowerBox-und Dateisystem Zugriff außerhalb des App-Containers

Eine Sandkasten-xamarin. Mac-Anwendung kann auf folgende Weise auf Dateisystem Pfade außerhalb ihres Containers zugreifen:

- In der spezifischen Richtung des Benutzers (über Dialogfelder öffnen und speichern oder andere Methoden wie Drag & Drop).
- Mithilfe von Berechtigungen für bestimmte Dateisystem Speicherorte (z. b. `/bin` oder `/usr/lib`).
- Wenn sich der Speicherort des Dateisystems in bestimmten Verzeichnissen befindet, die für die Welt lesbar sind (z. b. Freigabe).

_PowerBox_ ist die macOS-Sicherheitstechnologie, die mit dem Benutzer interagiert, um die Dateizugriffsrechte für die Sandbox-xamarin. Mac-app zu erweitern. PowerBox verfügt über keine API, wird jedoch transparent aktiviert, wenn die APP eine `NSOpenPanel` oder `NSSavePanel`aufruft. Der Zugriff auf den PowerBox-Server wird über die Berechtigungen aktiviert, die Sie für Ihre xamarin. Mac-Anwendung festlegen.

Wenn eine Sandbox-App ein Dialogfeld zum Öffnen oder speichern anzeigt, wird das Fenster von PowerBox (und nicht von AppKit) angezeigt und hat daher Zugriff auf alle Dateien oder Verzeichnisse, auf die der Benutzer Zugriff hat.

Wenn ein Benutzer eine Datei oder ein Verzeichnis aus dem Dialogfeld "Öffnen" oder "Speichern" auswählt (oder entweder auf das Symbol der APP zieht), fügt PowerBox den zugehörigen Pfad zum Sandkasten der APP hinzu.

Außerdem lässt das System die folgenden Aktionen automatisch für eine Sandbox-APP zu:

- Verbindung mit einer System Eingabemethode.
- Ruft die Dienste auf, die vom Benutzer im Menü **Dienste** ausgewählt wurden (nur für Dienste, die vom Dienstanbieter als _sicher für Sandbox-apps_ gekennzeichnet sind).
- Dateien öffnen wählen Sie im Menü " **zuletzt geöffnet** " den Benutzer aus.
- Verwenden Sie kopieren & Einfügen zwischen anderen Anwendungen.
- Lesen Sie die Dateien aus den folgenden weltweit lesbaren Speicherorten:
  - `/bin`
  - `/sbin`
  - `/usr/bin`
  - `/usr/lib`
  - `/usr/sbin`
  - `/usr/share`
  - `/System`
- Lesen und Schreiben von Dateien in den Verzeichnissen, die von `NSTemporaryDirectory`erstellt wurden.

Der Standardwert ist, dass Dateien, die durch eine Sandkasten-xamarin. Mac-app geöffnet oder gespeichert werden, so lange zugänglich sind, bis die APP beendet wird (es sei denn, die Datei war nach wie vor geöffnet). Geöffnete Dateien werden beim nächsten Start der APP automatisch in der Sandbox der APP über das macOS-Feature wieder hergestellt.

Verwenden Sie zum Bereitstellen von Persistenz für Dateien, die sich außerhalb des Containers einer xamarin. Mac-app befinden, die sicherheitsbezogenen Lesezeichen (siehe unten).

#### <a name="related-items"></a>Verknüpfte Elemente

Die APP-Sandbox ermöglicht einer APP den Zugriff auf verwandte Elemente, die den gleichen Dateinamen, aber unterschiedliche Erweiterungen aufweisen. Die Funktion besteht aus zwei Teilen: a) einer Liste verwandter Erweiterungen im `Info.plst` Datei der APP, b), um der Sandbox mitzuteilen, was die APP mit diesen Dateien tun wird.

Es gibt zwei Szenarien, in denen dies sinnvoll ist:

1. Die APP muss in der Lage sein, eine andere Version der Datei (mit einer neuen Erweiterung) zu speichern. Beispielsweise das Exportieren einer `.txt` Datei in eine `.pdf` Datei. Um diese Situation zu beheben, müssen Sie für den Zugriff auf die Datei eine `NSFileCoordinator` verwenden. Zuerst wird die `WillMove(fromURL, toURL)`-Methode aufgerufen, die Datei wird in die neue Erweiterung verschoben und dann `ItemMoved(fromURL, toURL)`aufgerufen.
2. Die APP muss eine Hauptdatei mit einer Erweiterung und mehreren unterstützenden Dateien mit unterschiedlichen Erweiterungen öffnen. Beispielsweise ein Film und eine Untertiteldatei. Verwenden Sie eine `NSFilePresenter`, um Zugriff auf die sekundäre Datei zu erhalten. Stellen Sie die Hauptdatei für die `PrimaryPresentedItemURL`-Eigenschaft und die sekundäre Datei für die `PresentedItemURL`-Eigenschaft bereit. Wenn die Hauptdatei geöffnet wird, müssen Sie die `AddFilePresenter`-Methode der `NSFileCoordinator`-Klasse zum Registrieren der sekundären Datei abrufen.

In beiden Szenarien muss die Datei " **Info. plist** " der APP die Dokumenttypen deklarieren, die von der APP geöffnet werden können. Fügen Sie für jeden Dateityp den `NSIsRelatedItemType` (mit dem Wert `YES`) zu seinem Eintrag im `CFBundleDocumentTypes` Array hinzu.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Öffnen und Speichern des Dialog Felds Verhalten mit Sandbox-apps

Die folgenden Grenzwerte gelten für die `NSOpenPanel` und `NSSavePanel`, wenn Sie von einer Sandkasten-xamarin. Mac-app aufgerufen werden:

- Sie können die Schaltfläche **OK** nicht Programm gesteuert aufrufen.
- Sie können die Auswahl eines Benutzers in einer `NSOpenSavePanelDelegate`nicht Programm gesteuert ändern.

Außerdem sind die folgenden Vererbungs Änderungen vorhanden:

- **Nicht-Sandkasten-App** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Sicherheitsbezogene Lesezeichen und persistente Ressourcen Zugriff

Wie bereits erwähnt, kann eine Sandbox-xamarin. Mac-Anwendung mithilfe direkter Benutzerinteraktion (wie von PowerBox bereitgestellt) auf eine Datei oder Ressource außerhalb ihres Containers zugreifen. Der Zugriff auf diese Ressourcen wird jedoch nicht automatisch über App-Starts oder Systemneustarts hinweg beibehalten.

Durch die Verwendung von _Lesezeichen im Sicherheits_Bereich kann eine Sandbox-xamarin. Mac-Anwendung die Benutzer Absicht beibehalten und den Zugriff auf bestimmte Ressourcen nach einem App-Neustart erhalten.

#### <a name="security-scoped-bookmark-types"></a>Lesezeichen Typen mit Sicherheitsbereich

Beim Arbeiten mit sicherheitsbezogenen Lesezeichen und permanentem Ressourcen Zugriff gibt es zwei Anwendungsfälle:

- **Ein Lesezeichen in der App ermöglicht einen permanenten Zugriff auf eine benutzerdefinierte Datei oder einen benutzerdefinierten Ordner.**

    Wenn die Sandbox-Anwendung z. b. die Verwendung von zum Öffnen eines externen Dokuments zur Bearbeitung ermöglicht (mit einem `NSOpenPanel`), kann die APP ein Lesezeichen für eine APP erstellen, damit Sie in Zukunft erneut auf dieselbe Datei zugreifen kann.
- **Ein Lesezeichen mit Dokumentbereich bietet einen permanenten Zugriff auf eine unter Datei in einem bestimmten Dokument.**

Beispielsweise eine Video Bearbeitungs-APP, die eine Projektdatei erstellt, die Zugriff auf die einzelnen Bilder, Videoclips und Audiodateien hat, die später in einem einzelnen Film kombiniert werden.

Wenn der Benutzer eine Ressourcen Datei in das Projekt importiert (über eine `NSOpenPanel`), erstellt die APP ein Dokument mit Dokumentbereich für das Element, das im Projekt gespeichert ist, damit die APP immer auf die Datei zugreifen kann.

Ein Lesezeichen im Dokumentbereich kann von jeder Anwendung aufgelöst werden, die die Lesezeichen Daten und das Dokument selbst öffnen kann. Dies unterstützt die Portabilität und ermöglicht dem Benutzer, die Projektdateien an einen anderen Benutzer zu senden, sodass alle Lesezeichen ebenfalls für Sie funktionieren.

> [!IMPORTANT]
> Ein Lesezeichen mit Dokumentbereich kann _nur_ auf eine einzelne Datei und nicht auf einen Ordner verweisen, und diese Datei darf sich nicht an einem vom System verwendeten Speicherort befinden (z. b. `/private` oder `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Verwenden von Lesezeichen mit Sicherheitsbereich

Wenn Sie beide Arten von Lesezeichen verwenden, müssen Sie die folgenden Schritte ausführen:

1. **Legen Sie die entsprechenden Berechtigungen in der xamarin. Mac-app fest, die sicherheitsbezogene Lesezeichen verwenden** müssen. Legen Sie für Lesezeichen mit Anwendungsbereich den `com.apple.security.files.bookmarks.app-scope` Berechtigungsschlüssel auf `true`fest. Legen Sie für Lesezeichen im Dokumentbereich den `com.apple.security.files.bookmarks.document-scope` Berechtigungsschlüssel auf `true`fest.
2. **Erstellen Sie ein Lesezeichen mit Sicherheits** Bereich. Dies geschieht für alle Dateien oder Ordner, auf die der Benutzer Zugriff gewährt hat (z. b. über `NSOpenPanel`), für die die APP permanenten Zugriff benötigt. Verwenden Sie die `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)`-Methode der `NSUrl`-Klasse, um das Lesezeichen zu erstellen.
3. **Auflösen des sicherheitsbezogenen Lesezeichens** : Wenn die APP erneut auf die Ressource zugreifen muss (z. b. nach einem Neustart), muss das Lesezeichen in eine sicherheitsbezogene URL aufgelöst werden. Verwenden Sie die `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)`-Methode der `NSUrl`-Klasse, um das Lesezeichen aufzulösen.
4. **Explizites Benachrichtigen des Systems, dass Sie auf die Datei aus der sicherheitsbezogenen URL zugreifen möchten** : dieser Schritt muss unmittelbar nach dem Abrufen der sicherheitsbezogenen URL oberhalb von oder erfolgen, wenn Sie später wieder Zugriff auf die Ressource erhalten möchten, nachdem Sie haben Sie Ihren Zugriff darauf aufgegeben. Aufrufen der `StartAccessingSecurityScopedResource ()`-Methode der `NSUrl`-Klasse, um mit dem Zugriff auf eine sicherheitsbezogene URL zu beginnen.
5. **Benachrichtigen Sie das System explizit, dass Sie mit der sicherheitsbezogenen URL auf die Datei zugegriffen** haben: so bald wie möglich sollten Sie das System informieren, wenn die APP keinen Zugriff mehr auf die Datei benötigt (z. b. wenn der Benutzer Sie schließt). Aufrufen der `StopAccessingSecurityScopedResource ()`-Methode der `NSUrl`-Klasse, um den Zugriff auf eine sicherheitsbezogene URL zu verhindern.

Nachdem Sie den Zugriff auf eine Ressource aufgegeben haben, müssen Sie erneut zu Schritt 4 zurückkehren, um den Zugriff wiederherzustellen. Wenn die xamarin. Mac-app neu gestartet wird, müssen Sie zu Schritt 3 zurückkehren und das Lesezeichen erneut auflösen.

> [!IMPORTANT]
> Wenn Sie den Zugriff auf URL-Ressourcen im Bereich der Sicherheit nicht freigeben, wird eine xamarin. Mac-app dazu führen, dass Kernel Ressourcen beschädigt werden. Folglich kann die APP dem Container keine Dateisystem Speicherorte mehr hinzufügen, bis er neu gestartet wird.

### <a name="the-app-sandbox-and-code-signing"></a>App-Sandbox und Code Signatur

Nachdem Sie die APP-Sandbox aktiviert und die speziellen Anforderungen für Ihre xamarin. Mac-app (über Berechtigungen) aktiviert haben, müssen Sie das Projekt codieren, damit das Sandbox wirksam wird. Sie müssen die Code Signierung durchführen, da die für das App-Sandkasten erforderlichen Berechtigungen mit der Signatur der APP verknüpft sind.

macOS erzwingt eine Verknüpfung zwischen dem Container einer APP und der zugehörigen Code Signatur. auf diese Weise kann keine andere Anwendung auf diesen Container zugreifen, auch wenn Sie die APP-Bündel-ID Spoofing. Dieser Mechanismus funktioniert wie folgt:

1. Wenn das System den Container der App erstellt, wird eine _Access Control Liste_ (ACL) für diesen Container festgelegt. Der erste Zugriffs Steuerungs Eintrag in der Liste enthält die fest _gelegte Anforderung_ der APP (Dr), die beschreibt, wie zukünftige Versionen der APP erkannt werden können (bei der Aktualisierung).
2. Jedes Mal, wenn eine APP mit der gleichen Paket-ID gestartet wird, überprüft das System, ob die Code Signatur der APP den in einem der Einträge in der ACL des Containers angegebenen Anforderungen entspricht. Wenn das System keine Entsprechung findet, verhindert es, dass die APP gestartet wird.

Die Code Signatur funktioniert wie folgt:

1. Bevor Sie das xamarin. Mac-Projekt erstellen, erhalten Sie ein Entwicklungs Zertifikat, ein Verteilungs Zertifikat und ein Entwickler-ID-Zertifikat aus dem Apple Developer Portal.
2. Wenn die xamarin. Mac-app im Mac App Store verteilt wird, wird Sie mit einer Apple-Code Signatur signiert.

Beim Testen und Debuggen verwenden Sie eine Version der xamarin. Mac-Anwendung, die Sie signiert haben (die zum Erstellen des App-Containers verwendet wird). Wenn Sie später die Version aus dem Apple App Store testen oder installieren möchten, wird Sie mit der Apple-Signatur signiert und kann nicht gestartet werden (da Sie nicht die gleiche Code Signatur wie der ursprüngliche App-Container hat). In dieser Situation erhalten Sie einen Absturz Bericht ähnlich dem folgenden:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Um dieses Problem zu beheben, müssen Sie den ACL-Eintrag so anpassen, dass er auf die von Apple signierte Version der APP verweist.

Weitere Informationen zum Erstellen und Herunterladen der für das Sandkasten erforderlichen Bereitstellungs Profile finden Sie im obigen Abschnitt [Signieren und Bereitstellen der APP](#Signing_and_Provisioning_the_App) .

#### <a name="adjusting-the-acl-entry"></a>Anpassen des ACL-Eintrags

Gehen Sie folgendermaßen vor, damit die von Apple signierte Version der xamarin. Mac-app ausgeführt werden kann:

1. Öffnen Sie die Terminal-app (in `/Applications/Utilities`).
2. Öffnen Sie ein Finder-Fenster mit der von Apple signierten Version der xamarin. Mac-app.
3. Geben Sie im Terminal Fenster `asctl container acl add -file` ein.
4. Ziehen Sie das Symbol der xamarin. Mac-app aus dem Fenster Finder, und legen Sie es im Terminal Fenster ab.
5. Der vollständige Pfad zur Datei wird dem Befehl im Terminal hinzugefügt.
6. Drücken **Sie die Eingabe** Taste, um den Befehl auszuführen.

Die ACL des Containers enthält jetzt die festgelegten Code Anforderungen für beide Versionen der xamarin. Mac-app, und macOS ermöglicht jetzt die Ausführung beider Versionen.

#### <a name="display-a-list-of-acl-code-requirements"></a>Anzeigen einer Liste der ACL-Code Anforderungen

Sie können eine Liste der Code Anforderungen in der ACL eines Containers anzeigen, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie die Terminal-app (in `/Applications/Utilities`).
2. Geben Sie `asctl container acl list -bundle <container-name>` ein.
3. Drücken **Sie die Eingabe** Taste, um den Befehl auszuführen.

Der-`<container-name>` ist in der Regel der Bündel Bezeichner für die xamarin. Mac-Anwendung.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Entwerfen einer xamarin. Mac-app für die APP-Sandbox

Beim Entwerfen einer xamarin. Mac-app für die APP-Sandbox sollte ein allgemeiner Workflow befolgt werden. Dies bedeutet, dass die Besonderheiten der Implementierung von Sandbox in einer APP für die Funktionalität der jeweiligen App eindeutig sind.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Sechs Schritte für die Übernahme der APP-Sandbox

Das Entwerfen einer xamarin. Mac-app für die APP-Sandbox besteht in der Regel aus den folgenden Schritten:

1. Bestimmen Sie, ob die APP für Sandboxing geeignet ist.
2. Entwerfen Sie eine Entwicklungs-und Verteilungs Strategie.
3. Beheben Sie alle API-Inkompatibilitäten.
4. Wenden Sie die erforderlichen App-Sandkasten Berechtigungen auf das xamarin. Mac-Projekt an.
5. Fügen Sie die Berechtigungs Trennung mithilfe von XPC hinzu.
6. Implementieren Sie eine Migrationsstrategie.

> [!IMPORTANT]
> Sie dürfen nicht nur die ausführbare Hauptdatei in der APP Bundle, sondern auch jede enthaltene Hilfsanwendung oder jedes Tool in diesem Bündel in der Sandbox. Dies ist für alle apps erforderlich, die aus dem Mac App Store verteilt werden, und sollte, sofern möglich, für jede andere Form der APP-Verteilung erfolgen.

Geben Sie im Terminal den folgenden Befehl ein, um eine Liste aller ausführbaren Binärdateien im Paket einer xamarin. Mac-app zu erhalten:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Dabei ist `[Your-App-Bundle]` der Name und der Pfad zum Paket der Anwendung.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Bestimmen, ob eine xamarin. Mac-app für Sandkasten geeignet ist

Die meisten xamarin. Mac-apps sind vollständig kompatibel mit dem App-Sandkasten und somit für Sandboxing geeignet. Wenn die APP ein Verhalten erfordert, dass die APP-Sandbox nicht zulässt, sollten Sie einen alternativen Ansatz in Erwägung gezogen.

Wenn Ihre APP eines der folgenden Verhaltensweisen erfordert, ist Sie nicht mit der APP-Sandbox kompatibel:

- **Autorisierungs Dienste** : mit dem App-Sandkasten können Sie nicht mit den Funktionen arbeiten, die unter [Autorisierungs Dienste C-Referenz](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)beschrieben sind.
- **Barrierefreiheits-APIs** : Sie können keine unterstützenden apps, wie z. b. Sprachausgabe oder apps, die andere Anwendungen steuern.
- **Apple-Ereignisse an beliebige apps senden** : Wenn die App Apple-Ereignisse an eine unbekannte, beliebige App senden muss, kann Sie nicht in einem Sandkasten angezeigt werden. Für eine bekannte Liste von apps, die als App bezeichnet werden, kann die APP weiterhin in einem Sandkasten enthalten sein, und die Berechtigungen müssen die aufgerufene App-Liste enthalten.
- **Senden von Benutzer Informations Wörterbüchern in verteilten Benachrichtigungen an andere Aufgaben** : mit dem App-Sandkasten ist es nicht möglich, ein `userInfo` Wörterbuch zu senden, wenn ein `NSDistributedNotificationCenter`-Objekt für das Senden von anderen Aufgaben
- **Laden von Kernel Erweiterungen** : das Laden von Kernel Erweiterungen ist durch den App-Sandkasten unzulässig.
- **Simulieren von Benutzereingaben in Dialogfeldern zum Öffnen und speichern** : Programm gesteuertes Bearbeiten von Dialogfeldern zum Öffnen oder speichern zum simulieren oder Ändern von Benutzereingaben ist für die APP-Sandbox unzulässig.
- Der **Zugriff auf oder das Festlegen von Einstellungen für andere apps** : das Bearbeiten der Einstellungen anderer apps ist durch den App-Sandkasten unzulässig.
- **Konfigurieren von Netzwerkeinstellungen** : das Manipulieren von Netzwerkeinstellungen ist für die APP-Sandbox unzulässig.
- **Beenden anderer apps** : die APP-Sandbox untersagt die Verwendung von `NSRunningApplication` zum Beenden anderer apps.

### <a name="resolving-api-incompatibilities"></a>Auflösen von API-Inkompatibilitäten

Beim Entwerfen einer xamarin. Mac-app für die APP-Sandbox treten möglicherweise Inkompatibilitäten bei der Verwendung von einigen macOS-APIs auf.

Im folgenden finden Sie einige häufige Probleme, die Sie beheben können, um Sie zu beheben:

- **Öffnen, speichern und Nachverfolgen von Dokumenten** : Wenn Sie Dokumente mit einer anderen Technologie als `NSDocument`verwalten, sollten Sie aufgrund der integrierten Unterstützung für den App-Sandkasten zu dieser wechseln. `NSDocument` wird automatisch mit PowerBox verwendet und bietet Unterstützung für die Aufbewahrung von Dokumenten in ihrer Sandbox, wenn der Benutzer Sie im Finder verschiebt.
- **Beibehalten des Zugriffs auf Datei System Ressourcen** : Wenn die xamarin. Mac-APP vom permanenten Zugriff auf Ressourcen außerhalb ihres Containers abhängt, verwenden Sie sicherheitsbezogene Lesezeichen, um den Zugriff zu erhalten.
- **Erstellen eines Anmelde Elements für eine APP** : mit dem App-Sandkasten können Sie kein Anmelde Element mit `LSSharedFileList` erstellen, und Sie können den Status der Start Dienste nicht mithilfe von `LSRegisterURL`bearbeiten. Verwenden Sie die `SMLoginItemSetEnabled`-Funktion, wie in Äpfel [Hinzufügen von Anmelde Elementen mithilfe der Service Management Framework](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) -Dokumentation beschrieben.
- **Zugreifen auf Benutzerdaten** : Wenn Sie POSIX-Funktionen verwenden, z. b. `getpwuid`, um das Basisverzeichnis des Benutzers von den Verzeichnisdiensten abzurufen, empfiehlt es sich, Cocoa-oder Core Foundation-Symbole wie `NSHomeDirectory`zu verwenden.
- **Zugreifen auf die Einstellungen anderer apps** : da die APP-Sandbox Pfad Suche-APIs an den Container der APP weiterleitet, erfolgt das Ändern von Einstellungen innerhalb dieses Containers, und der Zugriff auf andere App-Einstellungen ist nicht zulässig.
- **Verwenden von HTML5 Embedded-Videos in Webansichten** : Wenn die xamarin. Mac-app WebKit zur Wiedergabe von eingebetteten HTML5-Videos verwendet, müssen Sie die APP auch mit dem AV Foundation-Framework verknüpfen. Mit dem App-Sandkasten wird verhindert, dass CoreMedia diese Videos wieder gibt.

### <a name="applying-required-app-sandbox-entitlements"></a>Erforderliche App-Sandkasten Berechtigungen werden angewendet

Sie müssen die Berechtigungen für jede xamarin. Mac-Anwendung, die Sie in der APP-Sandbox ausführen möchten, bearbeiten und das Kontrollkästchen **App-Sandboxing aktivieren aktivieren** .

Basierend auf der Funktionalität der APP müssen Sie möglicherweise andere Berechtigungen aktivieren, um auf Betriebssystem Features oder-Ressourcen zuzugreifen. App-Sandbox funktioniert am besten, wenn Sie die Berechtigungen minimieren, die Sie für das erforderliche Minimum benötigen, um Ihre APP auszuführen, sodass Sie die Berechtigungen nur nach dem Zufallsprinzip aktivieren.

Gehen Sie folgendermaßen vor, um zu bestimmen, welche Berechtigungen für eine xamarin. Mac-app erforderlich sind:

1. Aktivieren Sie die APP-Sandbox, und führen Sie die xamarin. Mac-app aus.
2. Führen Sie die Funktionen der APP aus.
3. Öffnen Sie die Konsolen-app (in `/Applications/Utilities`verfügbar), und suchen Sie im Protokoll **alle Meldungen** nach `sandboxd` Verletzungen.
4. Beheben Sie das Problem für jede `sandboxd` Verletzung entweder mithilfe des App-Containers anstelle anderer Dateisystem Speicherorte, oder wenden Sie App-Sandbox-Berechtigungen an, um den Zugriff auf eingeschränkte Betriebssystemfunktionen zu ermöglichen.
5. Führen Sie erneut aus, und testen Sie alle xamarin. Mac-App-Features erneut.
6. Wiederholen Sie diesen Vorgang, bis alle `sandboxd` Verstöße aufgelöst wurden.

### <a name="add-privilege-separation-using-xpc"></a>Hinzufügen der Berechtigungs Trennung mithilfe von XPC

Wenn Sie eine xamarin. Mac-app für die APP-Sandbox entwickeln, sehen Sie sich das Verhalten der APP im Hinblick auf Berechtigungen und Zugriff an, und erwägen Sie dann, Hochrisiko Vorgänge in Ihre eigenen XPC-Dienste zu trennen.

Weitere Informationen finden Sie im Programmier Handbuch [Erstellen von XPC-Diensten](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) und [Daemons und Diensten](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)von Apple.

### <a name="implement-a-migration-strategy"></a>Implementieren einer Migrationsstrategie

Wenn Sie eine neue Sandkasten Version einer xamarin. Mac-Anwendung freigeben, die zuvor nicht in einem Sandkasten enthalten war, müssen Sie sicherstellen, dass die aktuellen Benutzer über einen Smooth-Upgradepfad verfügen.

 Ausführliche Informationen zum Implementieren eines Container Migrations Manifests finden Sie unter Apple [Migrieren einer APP in eine Sandbox](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) -Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Sand Boxing einer xamarin. Mac-Anwendung ausführlich erläutert. Zuerst haben wir eine einfache xamarin. Mac-app erstellt, um die Grundlagen der APP-Sandbox anzuzeigen. Als nächstes haben wir gezeigt, wie Sandbox Verletzungen aufgelöst werden. Anschließend haben wir uns eingehend mit dem App-Sandkasten beschäftigt. Schließlich haben wir uns mit dem Entwerfen einer xamarin. Mac-app für die APP Sandbox beschäftigt.

## <a name="related-links"></a>Verwandte Links

- [Veröffentlichen im App Store](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Informationen zu app-Sandbox](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Häufige Probleme bei der APP-Sandbox](https://developer.apple.com/library/content/qa/qa1773/_index.html)
