---
title: Xamarin Profiler
description: Dieses Handbuch untersucht die Hauptfunktionen von der Xamarin Profiler. Es werden auf Profiler, profilerstellung und wann sie verwendet werden soll und auf einen standard-Workflow für das Erstellen von Anwendungsprofilen Xamarin aussehen.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: topgenorth
ms.author: toopge
ms.date: 06/03/2018
ms.openlocfilehash: 42a8a2e3751d111f6ba8ccbea32e0446460f9a29
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793874"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Dieses Handbuch untersucht die Hauptfunktionen von der Xamarin Profiler. Es werden auf Profiler, profilerstellung und wann sie verwendet werden soll und auf einen standard-Workflow für das Erstellen von Anwendungsprofilen Xamarin aussehen._

Eine Anwendung Erfolg hängt von der Benutzeroberfläche zur ab. Als Entwickler möglicherweise implementiert haben einige wirklich awesome Funktionen in Ihrer app, aber wenn die Anwendung langsam oder vollständige Abstürze (crashes) befindet, der Benutzer wird wahrscheinlich stattdessen davon.

In der Vergangenheit hat Mono einen leistungsfähigen Befehlszeilen Profiler Funktionsumfang, bezeichnet Sammeln von Informationen über Programme, die in den Mono-Runtime ausgeführt werden. die [Mono / Protokoll Profiler](http://www.mono-project.com/docs/debug+profile/profile/profiler/). Der Xamarin Profiler eine grafische Benutzeroberfläche für den Mono / Log-Profiler und profilerstellung für Android, iOS, tvos. außerdem wurden, und Macintosh-Anwendungen für Mac und Android, iOS und tvos. außerdem wurden Anwendungen unter Windows unterstützt.

Der Xamarin Profiler bietet eine Reihe von Instrumente zur Verfügung, für die profilerstellung – Zuordnungen, Zyklen und Zeit Profiler. Dieses Handbuch wird erklärt, was diese Instrumente messen und wie sie Ihre Anwendung analysieren und verdeutlicht die Bedeutung der Daten auf den einzelnen Bildschirmen angezeigt.

Dieses Handbuch allgemeine Profilerstellungsdaten Szenarien untersucht und bringt den Profiler als Tool zum Analysieren und Optimieren von IOS- und Android-Anwendungen.

## <a name="download-and-install"></a>Herunterladen und installieren

> [!NOTE]
> Sie müssen werden eine [Visual Studio Enterprise](https://www.visualstudio.com/vs/compare/) Abonnenten für diese Funktion entweder Visual Studio Enterprise unter Windows oder Visual Studio für Mac auf einem Macintosh-Computer zu entsperren

Der Xamarin Profiler ist eine eigenständige Anwendung, und ist in Visual Studio für Mac und Visual Studio zum Aktivieren der profilerstellung mithilfe der IDE integriert.

Laden Sie das Installationspaket für Ihre Plattform herunter:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Nachdem das Download abgeschlossen ist, starten Sie das Installationsprogramm der Xamarin Profiler mit Ihrem System hinzufügen.

## <a name="profilers-and-profiling"></a>Profiler und Profilerstellung

Die profilerstellung ist ein wichtiges und häufig übersehene Schritt bei der Anwendungsentwicklung. Die profilerstellung ist eine Form der **dynamische Analyse** -er das Programm analysiert, während er ausgeführt wird und verwendet wird. Ein Profiler ist ein Datamining-Tool, das Informationen über die Zeitkomplexität, die Verwendung von bestimmten Methoden und den zugeordneten Speicher erfasst. Ein Profiler können Sie einen tiefen Drilldown und analysieren diese Metriken um Problembereiche im Code zu ermitteln.

Beim Entwerfen und Entwickeln einer Anwendung, ist es wichtig, die keine vorzeitige Optimierung; Zeit, die Entwicklung des Codes in Bereichen, die selten zugegriffen wird, also verbringen. Dies ist die Leistungsfähigkeit der profilerstellung. Ein Profiler bietet einen Einblick in die am häufigsten verwendet häufig Teile des Codes, die Basis und hilft suchen, Bereiche, in dem Sie machen uhrzeitverbesserungen ausgeben soll. Entwickler sollten darauf achten, um zu verstehen, in dem meiste Zeit aufgewendet wird, in der Anwendung, und wie Speicher von der Anwendung verwendet werden.

Profilerstellung ist nützlich in allen Typen von Entwicklung, sondern ist es besonders in mobile Entwicklung entscheidend. Nicht optimierter Code ist wesentlich deutlicher auf mobilen Plattformen als auf Desktopcomputern und der Erfolg Ihrer App hängt ansprechender und optimierten Code, der effizient ausgeführt wird.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Der Xamarin Profiler bietet Entwicklern eine Möglichkeit zum Profil von Anwendungen in Visual Studio für Mac oder Visual Studio. Der Profiler sammelt und zeigt Informationen zur app, die dann vom Entwickler verwendet werden kann, um eine Anwendung zu analysieren. Es gibt diverse Möglichkeiten, die eine profilerstellung einer Anwendung mit der Xamarin Profiler, nämlich profilerstellung für .NET-Arbeitsspeicher und statistische Sampling. Diese werden über die Zuordnungen und die Uhrzeit Profiler durchgeführt bzw. instrumentiert.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Derzeit kann der Xamarin Profiler testen Xamarin.Mac, Xamarin.iOS und Xamarin.Android Anwendungen auf einem Mac (über Visual Studio für Mac) verwendet werden. Der Profiler ist ein separater Prozess von der IDE aus, und daher zusätzlich zum Starten von Visual Studio für Mac, es kann verwendet werden als eigenständige Anwendung .exe untersuchen und `.mlpd` Dateien, die von erzeugt wurden die [Mono / Protokoll Profiler](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Derzeit kann der Xamarin Profiler testen Xamarin.Android-apps unter Windows (über Visual Studio und Visual Studio für Mac) verwendet werden. Der Profiler ist ein separater Prozess von der IDE aus, und daher zusätzlich zu den von Visual Studio wird gestartet, es kann verwendet werden als eigenständige Anwendung .exe untersuchen und `.mlpd` Dateien, die von erzeugt wurden die [Mono / Protokoll Profiler](http://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Profiler-Unterstützung

Unterstützung der Xamarin Profiler steht auf den folgenden Plattformen zur Verfügung:

 - Visual Studio für Mac (MacOS mit Enterprise-Lizenz)
    - Android
        - Geräte- und Emulator
    - iOS
        - Geräte- und Simulator
    - tvos. außerdem wurden (Zeit Instrument wird nicht unterstützt)
        - Geräte- und Simulator
    - Mac

 - Visual Studio (nur **Enterprise** Version)
    - Android
        - Geräte- und Emulator
    - iOS [experimentell]
        - Geräte- und Simulator
    - tvOS
        - Geräte- und Simulator

Beachten Sie, die Sie können **nur** Profil **Debuggen** Konfigurationen.

## <a name="profiler-basics"></a>Profiler-Grundlagen

In diesem Abschnitt werden die Bestandteile der Xamarin Profiler eingeführt und tours seine Funktionen.

### <a name="allow-profiling-in-your-app"></a>Zulassen Sie die Profilerstellung in Ihrer App

Bevor Sie erfolgreich Profil Ihrer app erstellen können, müssen Sie die Profilerstellung in der app-Projektoptionen zuzulassen.

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  **Erstellen Sie > iOS Debuggen > Profilerstellung aktivieren**

  ![](images/ios-options-mac.png "iOS-Dialogfeld \"Optionen\" in Visual Studio für Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Eigenschaften > iOS-Build > Profilerstellung aktivieren**

  ![](images/ios-project-options-vs.png "iOS-Dialogfeld \"Optionen\" in Visual Studio")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  **Erstellen Sie > Android Debug > Aktivieren Sie die Developer-Instrumentation**

  ![Android Dialogfeld "Optionen" in Visual Studio für Mac](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **Erstellen Sie > Android Debug > Aktivieren Sie die Developer-Instrumentation**

  ![Android Dialogfeld "Optionen" in Visual Studio für Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Starten den Profiler

Der Xamarin Profiler kann gestartet werden, von der IDE angezeigt, wenn Sie IOS- oder Android-Anwendung ein Profil erstellen oder als eigenständige Anwendung.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Starten von Visual Studio für Mac

1. Stellen Sie zunächst sicher, Ihre Anwendung in Visual Studio für Mac geladen haben, und wählen Sie die Debugkonfiguration (Standard).
2. Navigieren Sie zu **ausführen > Profilerstellung starten**in Visual Studio für Mac oder **analysieren > Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie im folgenden Diagramm veranschaulicht:

  ![](images/start-profiling-xs.png "Starten den Profiler von Visual Studio für Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Starten von Visual Studio

1.  Stellen Sie zunächst sicher, Ihre Anwendung in Visual Studio geladen haben, und wählen Sie die Debugkonfiguration (Standard), wie oben angegeben.
2.  Navigieren Sie zu **analysieren > Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie im folgenden Diagramm veranschaulicht:

![Starten den Profiler von Visual Studio](images/start-profiling-vs.png)

-----

Wenn die Menüelemente nicht angezeigt werden, finden Sie in der [Handbuch zur Problembehandlung](~/tools/profiler/troubleshooting.md).

Dies startet den Profiler und automatisch mit der profilerstellung der Anwendung beginnt.

Der Profiler kann verwendet werden, um Arbeitsspeicher und die Leistung zu messen. Sie erreicht dies durch die Zuordnungen und die Uhrzeit Profiler Instrumente, die im nächsten Abschnitt ausführlich untersucht wird.

#### <a name="saving-and-loading-profiler-sessions"></a>Speichern und Laden von Profiler-Sitzungen

Um eine Profilerstellungssitzung zu einem beliebigen Zeitpunkt zu speichern, wählen Sie **Datei > Speichern unter...**  über die Profiler-Menüleiste. Dies spart die Datei im _Mlpd_ Format, eine spezielle, stark komprimierten Format für Profilerstellungsdaten.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Nach der Installation von wurde kann der Xamarin Profiler im Ordner "Anwendungen" ermittelt werden, wie im folgenden Screenshot gezeigt:

![](images/applications.png "Eigenständige Profiler über Mac öffnen")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nach der Installation von wurde kann die Xamarin Profiler-Anwendung im Verzeichnis Ihrer Anwendung ermittelt werden:

![](images/applications-vs.png "Öffnen Sie Windows eigenständige Profiler ")

-----

Sie laden können *.mlpd* Dateien in den Profiler, indem Sie die eigenständige Anwendung öffnen auswählen **Ziel auswählen** und Laden der Datei.

Weitere Informationen finden Sie unter [generieren .mlpd Dateien ](~/tools/profiler/troubleshooting.md#gen_mlpd).

## <a name="profiler-features"></a>Profiler-Funktionen

Der Xamarin Profiler besteht aus fünf Abschnitte, wie unten gezeigt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Profiler-Abschnitte in Visual Studio für Mac")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Profiler-Abschnitte in Visual Studio")](images/profiler-vs.png#lightbox)

-----

- **Symbolleiste** – befindet sich auf den Anfang des Profilers und bietet Optionen zum Starten/Beenden, die ein Profil erstellen, wählen Sie einen Zielprozess, zeigen Sie die Ausführungszeit der app an, und wählen die Split-Ansichten, die der Profiler-Anwendung ergeben.
- **Instrumentieren Liste** – Listet alle Geräte, die für die Profilerstellungssitzung geladen.
- **Diagramm zeichnen** – in diesen Diagrammen beziehen sich horizontal auf die relevanten Geräte in der Liste instrumentieren. Ein Schieberegler (darunter Zeit Profiler gezeigt) kann verwendet werden, so ändern Sie die Skala.
- **Instrumentieren der Detailbereich** -enthält Daten, die von der ausgewählten Sicht von der aktuellen Instrument angezeigt wird. Wir werden diese Sichten ausführlicher im Abschnitt weiter unten näher betrachten.
- **Ansicht der Inspektor** – dies gegliedert, die vom segmentierte Steuerelement ausgewählt werden können. In den Abschnitten hängen von der ausgewählten instrumentieren und enthält: Konfigurationseinstellungen, Statistiken, Stapel-Trace-Informationen und den Pfad Stämme.

### <a name="allocations"></a>Zuordnungen

Die Zuordnungen Instrument enthält ausführliche Informationen zu Objekten in der Anwendung an, wie sie erstellt wird und die Sammlung veralteter Objekte aufgenommen.

Am Anfang des Profilers wird das Zuordnungen, die die Größe des Arbeitsspeichers in regelmäßigen Abständen während der profilerstellung anzeigt. Derzeit ist die Zuordnungen Diagramm die Gesamtanzahl der Zuordnungen und nicht die Größe des Heaps zu diesem Zeitpunkt. In gewisser Weise wird nie ausfallen, nur einmal erhöht. Dies umfasst die Objekte, die auf dem Stapel zugeordnet. Abhängig von der Common Language Runtime-Version verwendet wird kann das Diagramm anders – auch für dieselbe app suchen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](images/allocations1.png "Instrumentieren der Zuordnungen")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "Instrumentieren der Zuordnungen")](images/allocations1-vs.png#lightbox)

-----

Es gibt verschiedenen Datenansichten in die Zuordnungen instrumentieren, die es Entwicklern ermöglichen, wie ihre Anwendung verwenden und Freigeben von Arbeitsspeicher zu analysieren. Diese Ansichten werden im folgenden beschrieben:

 -   **Zuordnungen** – Dies zeigt eine Liste aller speicherbelegungen und gruppiert sie nach Klassennamen. Dies bietet eine gute Übersicht über Klassen und Methoden, die verwendet wird, wie oft sie verwendet werden und die kollektiven Größe der Klassen verwendet werden. Doppelklick auf eine Klasse zeigt den belegten Speicher: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  [![](images/allocations3.png "Registerkarte \"Zuordnungen\"")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "Registerkarte \"Zuordnungen\"")](images/allocations2-vs.png#lightbox)

-----

Die Inspektor-Ansicht für Zuordnungen bietet Optionen zum Filtern und Gruppieren von Objekten, das Bereitstellen einer Statistik zur Speicher belegt, und die oberen Zuordnungen sowie Ansichten für Stapelüberwachung und den Pfad zum Stamm.

 -   **Auslastungsaufrufstruktur** – Dies zeigt die gesamte Aufrufstruktur aller Threads in der Anwendung und schließt Informationen zu den Arbeitsspeicher auf jedem Knoten reserviert. Wenn ein Element in der Liste ausgewählt ist, werden alle gleichgeordneten Knoten angezeigt grau angezeigt. Sie können erweitern die Struktur oder doppelklicken Sie auf das Element, um einen Drilldown hinein. Zum Anzeigen von dieser Datenansicht kann die Anzeige der Einstellungen Inspektor verwendet werden, so ändern Sie die Möglichkeit, die sie angezeigt wird. Es gibt derzeit zwei Optionen:
    1.  **Invertiert Aufrufstrukturansicht** – dies berücksichtigt die stapelüberwachung von oben nach unten. Dies ist eine bequeme Ansichtsoption wie er die tiefsten Methoden angibt, in denen verfügt über die CPU Zeit verloren wurde.
    2.  **Separaten Thread** – diese Option werden die Aufrufstruktur nach Thread organisiert.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  [![](images/allocations2.png "Registerkarte \"Aufruf\"")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "Registerkarte \"Aufruf\"")](images/allocations3-vs.png#lightbox)

-----

 -   **Momentaufnahmen** – dieser Bereich zeigt Informationen zu Momentaufnahmen des Arbeitsspeichers. Um diese während der profilerstellung einer live-Anwendung zu generieren, klicken Sie auf die _Kamera_ auf der Symbolleiste an jedem Punkt, die Sie möchten, um festzustellen, welche Speicher beibehalten, und freigegeben. Sie können jede Momentaufnahme zum Durchsuchen, was hinter den Kulissen geschieht klicken. Beachten Sie, dass die Momentaufnahmen können nur bei live profilerstellung für eine app erstellt werden. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  [![](images/allocations4.png "Registerkarte \"Momentaufnahmen\"")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "Registerkarte \"Momentaufnahmen\"")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Time-Profiler

Die Zeit Profiler Instrument misst, genau wie viel Zeit in jeder Methode einer Anwendung verwendet wird. Die Anwendung wird in regelmäßigen Abständen angehalten, und eine stapelüberwachung auf jeder aktive Thread ausgeführt wird. Jede Zeile im Bereich Details Instrument zeigt den Ausführungspfad, der ausgeführt wurden.

Im Diagramm zeichnen angezeigt, wie im folgenden Screenshot gezeigt, die Anzahl der Samplings, die von der Anwendung empfangen werden, während der Ausführung:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Instrumentieren der Time-Profiler](images/time1.png)](images/time1.png#lightbox) 

[![Zeit Profiler Instrument – Liste der Beispiele](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Instrumentieren der Time-Profiler](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Zeit Profiler Instrument – Liste der Beispiele](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Auslastungsaufrufstruktur** – zeigt aufgewendete Zeit in jeder Methode:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

  [![](images/time2.png "Zeit Profiler Instrument – Aufrufstruktur")](images/time2.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "Zeit Profiler Instrument – Aufrufstruktur")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Zyklen

Durch die Verwendung von c# und F#-verwalteten Code kann es sein, recht häufig vorkommt, und leider recht einfach, Verweise auf Objekte zu erstellen, die nie gelöscht wird. Diese Instrumentieren können Sie diese Objekte zu ermitteln und Anzeigen der Zyklen in Ihrer Anwendung verwiesen wird.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Zyklen Instrumentieren](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Zyklen Instrumentieren](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Erstellen von Anwendungsprofilen

Derzeit können nur die Debug-Standardkonfigurationen profiliert werden.

Wenn Sie ein Profil eine app mit einer anderen Konfiguration erstellen, werden Sie im folgenden Dialogfeld angezeigt:


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Profilerstellung Fehlerdialogfeld](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "Profilerstellung Fehlerdialogfeld")](images/image1vs.png#lightbox) 

-----

Wählen Sie **Update** um den Vorgang fortzusetzen.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen-Garbage Collector und Profilerstellung

Die [SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/) Garbage Collection für alle Xamarin-Plattformen verwendet wird.

SGen ist ein generationsbasierten globaler Katalog, die Objekte einer Anwendung in drei Heaps reserviert – Baumschule, Major-Heap und den großen Objekts Speicherplatz. Dies ermöglicht die schnellere Ausführung der Garbagecollection. SGen ist derzeit für Xamarin.Android, das Standard-GC und Unified Xamarin.iOS Anwendungen.

Xamarin.iOS-Anwendung mithilfe der klassische API verwendet die Boehm GC – eine konservative, nicht Generation Garbage collection. Wie es konservative handelt, ist es weniger wahrscheinlich, dass Sie die zu ungenauen Ergebnissen führen kann, wenn der Profiler mit verfügbaren Arbeitsspeicher freizugeben. Aus diesem Grund kann das Instrument Zuordnungen mit dem Boehm Garbage Collector verwendet werden.

Während ein Dialogfeld Meldung Sie aufgefordert werden, wenn Ihre app das Boehm GC verwendet, empfiehlt Xamarin nicht, Wechsel der vorhandenen iOS-Anwendung, die auf SGen Boehm ohne eine sorgfältige Forschung und gründlichen Tests verwenden. Xamarin ist auch nicht empfohlen, wechseln zur SGen zur profilerstellung und wechseln dann zurück, wie diese Ergebnisse nicht genau Benchmarks speicherauslastung bereitstellt.

Weitere Informationen zur arbeitsspeicherverwaltung von, finden Sie in der [Arbeitsspeicher und bewährte Methoden für Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md) Handbuch.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch erläutert wir, welche die profilerstellung ist und wie es vorteilhaft sein, der Entwickler ist. Klicken Sie dann der Xamarin Profiler, bietet einige Verlauf und der Informationen in der Funktionsweise eingeführt. Schließlich wir die Funktionen von der Xamarin Profiler toured und untersucht die Zuordnungen und die Uhrzeit Profiler Instrumente.

## <a name="related-links"></a>Verwandte Links

- [Arbeitsspeicher- und Leistungsproblemen bewährte Methoden](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/profiler/preview/)
