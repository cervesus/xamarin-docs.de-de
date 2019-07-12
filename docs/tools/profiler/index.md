---
title: Xamarin Profiler
description: Dieses Handbuch beschreibt die wichtigsten Features von der Xamarin Profiler. Sie sehen an Profiler, profilerstellung und wann sie verwendet werden sollen und an einen Standardworkflow, für die profilerstellung von Xamarin-Anwendungen.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: lobrien
ms.author: laobri
ms.date: 06/03/2018
ms.openlocfilehash: d80363cd339d5d3177ae063df2a20d7938f59169
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832401"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_Dieses Handbuch beschreibt die wichtigsten Features von der Xamarin Profiler. Sie sehen an Profiler, profilerstellung und wann sie verwendet werden sollen und an einen Standardworkflow, für die profilerstellung von Xamarin-Anwendungen._

Eine Anwendung Erfolg hängt von der Benutzeroberfläche ab. Als Entwickler möglicherweise implementierten einige wirklich überzeugende Features in Ihrer app, aber wenn die app langsam oder Abstürze vollständig ist, der Benutzer wird wahrscheinlich es deshalb entsorgen.

In der Vergangenheit hat Mono einen leistungsfähigen Befehlszeilen Profiler vorgestellt, für das Erfassen von Informationen über Programme, die in die Mono-Laufzeit ausgeführt wird aufgerufen, die [Mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/). Der Xamarin Profiler eine grafische Benutzeroberfläche für die Mono-Log-Profiler, und unterstützt Android, iOS, TvOS und Macintosh-Anwendungen unter Mac und Android, iOS und TvOS-Anwendungen auf Windows-profilerstellung.

Der Xamarin Profiler verfügt über eine Reihe Instrumente zur profilerstellung, Zuordnungen, Zyklen und Zeit Profiler. Dieser Leitfaden wird beschrieben, was diese Instrumente messen und wie sie Ihre Anwendung analysieren und verdeutlicht die Bedeutung der Daten auf jedem Bildschirm dargestellt.

Dieses Handbuch allgemeine Profilerstellungsszenarios untersucht und führt die den Profiler als ein Tool zum Analysieren und Optimieren von IOS- und Android-Anwendungen.

## <a name="download-and-install"></a>Herunterladen und installieren

> [!NOTE]
> Sie müssen werden eine [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) Abonnenten zum Entsperren dieses Feature in entweder Visual Studio Enterprise auf Windows oder Visual Studio für Mac auf einem Mac.

Der Xamarin Profiler ist eine eigenständige Anwendung, und ist in Visual Studio für Mac und Visual Studio zum Aktivieren der profilerstellung aus, in der IDE integriert.

Laden Sie das Installationspaket für Ihre Plattform herunter:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

Nach dem Herunterladen, starten Sie den Installer, um den Xamarin Profiler dem System hinzufügen.

## <a name="profilers-and-profiling"></a>Profiler und Profilerstellung

Die profilerstellung ist ein wichtiger und häufig übersehene Schritt bei der Anwendungsentwicklung. Die profilerstellung ist eine Form der **dynamische Programmanalyse** -analysiert das Programm, während es ausgeführt werden und verwendet wird. Ein Profiler ist ein Datamining-Tool, das Informationen zu der Zeit, die Verwendung von bestimmten Methoden und die Speichermenge gesammelt. Ein Profiler können Sie einen tiefen Drilldown und analysieren Sie diese Metriken, um die Problembereiche im Code zu ermitteln.

Beim Entwerfen und Entwickeln einer Anwendung, ist es wichtig, die keine vorzeitige Optimierung; d. h. verbringt Zeit mit der Entwicklung von Code in Bereichen, die selten zugegriffen wird. Dies ist die Leistung der profilerstellung. Ein Profiler bietet einen Einblick in die am häufigsten verwendeten Teile Ihrer Codebasis und hilft suchen, Bereiche, in dem Sie alle uhrzeitverbesserungen verbringen sollten. Entwickler sollten darauf achten, um zu verstehen, wo in Ihrer Anwendung die meiste Zeit verbracht wird, und wie der Arbeitsspeicher von der Anwendung verwendet wird.

Profilerstellung bei der alle Arten der Entwicklung hilfreich ist, aber es ist besonders wichtig, in der mobilen Entwicklung. Nicht optimierter Code ist viel deutlicher auf mobilen Plattformen als auf Desktopcomputern und der Erfolg Ihrer App hängt von ansprechenden und optimierten Code, der effizient ausgeführt wird.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Der Xamarin Profiler bietet Entwicklern eine Möglichkeit zur profilieren von Anwendungen aus Visual Studio für Mac oder Visual Studio. Der Profiler sammelt und zeigt Informationen über die app, die dann vom Entwickler verwendet werden kann, um das Verhalten der Anwendung zu analysieren. Es gibt zahlreiche Möglichkeiten, die eine profilerstellung einer Anwendung mit der Xamarin Profiler, nämlich speicherprofilierung und statistisches Sampling. Diese durchgeführt werden, über die Zuordnungen und die Uhrzeit Profiler bzw. instrumentiert.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Derzeit kann der Xamarin Profiler verwendet werden, zum Testen von Xamarin.iOS, Xamarin.Android und Xamarin.Mac-Anwendungen unter Mac (über Visual Studio für Mac). Der Profiler ist ein separater Prozess aus der IDE, und daher, zusätzlich zum Starten von Visual Studio für Mac, er kann verwendet werden als eigenständige Anwendung .exe untersuchen und `.mlpd` Dateien, die von erzeugt wurden die [mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Derzeit kann der Xamarin Profiler verwendet werden, zum Testen der Xamarin.Android-apps für Windows (über Visual Studio und Visual Studio für Mac). Der Profiler ist ein separater Prozess aus der IDE, und daher, zusätzlich zum Starten von Visual Studio, er kann verwendet werden als eigenständige Anwendung .exe untersuchen und `.mlpd` Dateien, die von erzeugt wurden die [mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/) .

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Profiler-Unterstützung

Unterstützung für die Xamarin Profiler ist auf den folgenden Plattformen verfügbar:

- Visual Studio für Mac (Mac OS, mit dem Enterprise-Lizenz)
    - Android
        - Geräte und -Emulator
    - iOS
        - Geräte- und
    - TvOS (Zeit Instrumentieren wird nicht unterstützt)
        - Geräte- und
    - Mac

- Visual Studio (nur **Enterprise** Version)
    - Android
        - Geräte und -Emulator
    - iOS [experimentell]
        - Geräte- und
    - tvOS
        - Geräte- und

Beachten Sie, die Sie **nur** Profil **Debuggen** Konfigurationen.

## <a name="profiler-basics"></a>Profiler-Grundlagen

Dieser Abschnitt führt die Teile der Xamarin Profiler und tours seiner Features.

### <a name="allow-profiling-in-your-app"></a>Ermöglichen Sie die Profilerstellung in Ihrer App

Bevor Sie Ihre app erfolgreich das Profil können, müssen Sie die Profilerstellung in den Projektoptionen der app zu ermöglichen.

- iOS:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  **Erstellen > iOS-Debugging > Profilerstellung aktivieren**

  ![](images/ios-options-mac.png "iOS-Dialogfeld \"Optionen\" in Visual Studio für Mac")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **Eigenschaften > iOS-Build > Profilerstellung aktivieren**

  ![](images/ios-project-options-vs.png "iOS-Dialogfeld \"Optionen\" in Visual Studio")

-----

- Android:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  **Erstellen > Android-Debugprotokoll > Entwicklerinstrumentierung aktivieren**

  ![Android Dialogfeld "Optionen" in Visual Studio für Mac](images/android-project-options.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **Erstellen > Android-Debugprotokoll > Entwicklerinstrumentierung aktivieren**

  ![Android Dialogfeld "Optionen" in Visual Studio für Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Starten den Profiler

Der Xamarin Profiler kann gestartet werden, über die IDE, wenn Sie Ihre IOS- oder Android-Anwendung ein Profil oder als eigenständige Anwendung.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>Starten von Visual Studio für Mac

1. Zuerst stellen Sie sicher, dass die Anwendung in Visual Studio für Mac geladen, und wählen Sie die Debugkonfiguration (Standard).
2. Navigieren Sie zu **ausführen > Profilerstellung starten**in Visual Studio für Mac oder **analysieren > Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie im folgenden Diagramm veranschaulicht:

  ![](images/start-profiling-xs.png "Starten den Profiler in Visual Studio für Mac")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>Starten von Visual Studio

1.  Zuerst stellen Sie sicher, dass Ihre Anwendung in Visual Studio geladen haben, und wählen Sie die Debugkonfiguration (Standard), wie oben angegeben.
2.  Navigieren Sie zu **analysieren > Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie im folgenden Diagramm veranschaulicht:

![Starten den Profiler in Visual Studio](images/start-profiling-vs.png)

-----

Wenn die Menüelemente im nicht angezeigt werden, finden Sie in der [Handbuch zur Problembehandlung](~/tools/profiler/troubleshooting.md).

Dies startet den Profiler und startet automatisch die profilerstellung der Anwendung.

Der Profiler kann verwendet werden, um Arbeitsspeicher und Leistung zu messen. Er erreicht dies durch die Zuordnungen und die Uhrzeit Profiler Instrumente, die wir im nächsten Abschnitt detailliert vorgestellt wird.

#### <a name="saving-and-loading-profiler-sessions"></a>Speichern und Laden von Profiler-Sitzungen

Um eine Profilerstellungssitzung zu einem beliebigen Zeitpunkt zu speichern, wählen **Datei > Speichern unter...**  über die Profiler-Menüleiste. Dies speichert die Datei im _Mlpd_ Format, ein spezielles, stark komprimierten Format für Profilerstellungsdaten.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Nach der Installation von wurde der Xamarin Profiler in Ihrem Ordner "Programme" finden Sie wie im folgenden Screenshot dargestellt:

![](images/applications.png "Öffnen Sie Mac eigenständiger Profiler")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Nach der Installation wurden die Xamarin Profiler-Anwendung in Ihrem Anwendungsverzeichnis finden:

![](images/applications-vs.png "Öffnen Sie Windows eigenständiger Profiler ")

-----

Sie können laden *.mlpd* -Dateien in der Profiler durch Öffnen der eigenständigen Anwendung, auswählen **Ziel auswählen** und laden die Datei.

Weitere Informationen finden Sie unter [Generieren von Dateien von .mlpd](~/tools/profiler/troubleshooting.md#gen_mlpd).

## <a name="profiler-features"></a>Profiler-Funktionen

Der Xamarin Profiler besteht aus fünf Abschnitten, wie unten gezeigt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](images/profiler-mac-sml.png "Profiler-Abschnitte in Visual Studio für Mac")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/profiler-vs.png "Profiler-Abschnitte in Visual Studio")](images/profiler-vs.png#lightbox)

-----

- **Symbolleiste** – befindet sich am Anfang des Profilers, dies bietet Optionen zum Starten/Beenden der profilerstellung, die einen Zielprozess auswählen, das die Ausführungszeit der app anzuzeigen, und wählen die geteilten Ansichten, aus denen die Profiler-Anwendung zusammensetzt.
- **Instrumentieren Sie die Liste** – Listet alle Instrumente, die für die Profilerstellungssitzung geladen.
- **Diagramm zeichnen** – in diesen Diagrammen beziehen sich auf die die relevanten Instrumente in der Liste Instrumentieren horizontal. Ein Schieberegler (siehe unter Zeit Profiler) kann verwendet werden, um die Skalierung zu ändern.
- **Instrumentieren Sie Detailbereich** -enthält Daten, die von der ausgewählten Ansicht des aktuellen Instruments angezeigt wird. Wir betrachten diese Sichten noch ausführlicher im Abschnitt unten.
- **Prüfungsansicht** – enthält Abschnitte, die von dem segmentierten Steuerelement ausgewählt werden können. Die Abschnitte sind abhängig von der Instrumentierung ausgewählt, und enthält: Konfigurationseinstellungen, Statistik, Stapel-Trace-Informationen, und den Pfad zu Wurzeln.

### <a name="allocations"></a>Zuordnungen

Das änderungsinstrument Zuordnungen enthält ausführliche Informationen zu Objekten in der Anwendung, wie sie erstellt werden und die Garbage collection.

Am oberen Rand der Profiler wird das Zuordnungen, der die Größe des Arbeitsspeichers, die während der profilerstellung in regelmäßigen Abständen angezeigt. Derzeit ist das Diagramm Zuordnungen die Gesamtanzahl der Zuordnungen und nicht die Größe des Heaps zu diesem Zeitpunkt. Klicken Sie in gewisser Hinsicht er nie ausfällt, immer nur erhöht. Dies umfasst auch Objekte, die auf dem Stapel zugeordnet werden. Abhängig von der Common Language RuntimeVersion verwendet wird kann das Diagramm sieht anders – auch für die gleiche app aus.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](images/allocations1.png "Instrumentieren von Zuordnungen")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/allocations1-vs.png "Instrumentieren von Zuordnungen")](images/allocations1-vs.png#lightbox)

-----

Es gibt verschiedenen Datenansichten in den Zuordnungen instrumentieren, die es Entwicklern ermöglichen, wie ihre Anwendung verwenden und Freigeben von Speicher zu analysieren. Diese Ansichten werden nachfolgend beschrieben:

- **Zuordnungen** – dies Listet alle Zuordnungen und gruppiert sie nach Klassennamen. Dies bietet eine hervorragende Übersicht über Klassen und Methoden, die verwendet wird, wie häufig sie verwendet werden und die gesamte Größe der Klassen verwendet werden. Doppelklicken auf eine Klasse wird den zugeordnete Speicher angezeigt werden: 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  [![](images/allocations3.png "Registerkarte \"Zuordnungen\"")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations2-vs.png "Registerkarte \"Zuordnungen\"")](images/allocations2-vs.png#lightbox)

-----

Der prüfungsansicht für Zuordnungen bietet Optionen zum Filtern und Gruppieren von Objekten, die Bereitstellung von Statistiken für den Arbeitsspeicher belegt, und die wichtigste Zuordnungen sowie Ansichten für Stapelüberwachung und den Pfad zum Stamm.

- **Aufrufstruktur** – Dies zeigt die gesamte Aufrufstruktur aller Threads in der Anwendung und enthält Informationen über die Speichermenge, die auf jedem Knoten. Alle gleichgeordneten Knoten werden angezeigt, wenn ein Element in der Liste ausgewählt ist, grau angezeigt. Können Sie die Struktur erweitern oder doppelklicken Sie auf das Element um einen Drilldown in diesen. Wenn Sie diese Datenansicht anzeigen zu können, kann die Einstellungen für die Inspector Anzeige verwendet werden, zu verändern, wie sie angezeigt werden. Es gibt derzeit zwei Optionen:
    1.  **Umgekehrt Aufrufstruktur** – diese Version die stapelüberwachung von oben nach unten. Dies ist eine praktische Ansichtsoption wie die tiefsten-Methoden gibt an, in denen verfügt über die CPU Zeit verloren wurde.
    2.  **Separater Thread** – diese Option ordnet die Aufrufstruktur nach Thread.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  [![](images/allocations2.png "Registerkarte \"Aufruf\"")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations3-vs.png "Registerkarte \"Aufruf\"")](images/allocations3-vs.png#lightbox)

-----

- **Momentaufnahmen** – in diesem Bereich werden Informationen zu Momentaufnahmen des Arbeitsspeichers angezeigt. Um diese bei der profilerstellung einer aktiven Anwendung zu generieren, klicken Sie auf die _Kamera_ Schaltfläche auf der Symbolleiste an jedem Punkt, die Sie möchten, um festzustellen, welcher Speicher beibehalten, und veröffentlicht. Klicken Sie anschließend auf jede Momentaufnahme um untersuchen, was hinter den Kulissen passiert. Beachten Sie, dass Momentaufnahmen können nur Laufzeit-profilerstellung für eine app erstellt werden. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  [![](images/allocations4.png "Registerkarte \"Momentaufnahmen\"")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations4-vs.png "Registerkarte \"Momentaufnahmen\"")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Time-Profiler

Das änderungsinstrument Zeit Profiler misst, genau wie viel Zeit in jeder Methode einer Anwendung verwendet wird. Die Anwendung wird in regelmäßigen Abständen angehalten, und eine stapelüberwachung, die auf jeder aktive Thread ausgeführt wird. Jede Zeile in die Instrumentierung Detailbereich zeigt den Ausführungspfad, der besucht wurde.

Im Diagramm Plot zeigt, wie im nachstehenden Screenshot dargestellt, die Anzahl der Samplings, die von der app empfangen werden, während der Ausführung:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Instrumentieren der Time-Profiler](images/time1.png)](images/time1.png#lightbox) 

[![Zeit Profiler Instrumentieren – Liste der Beispiele](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Instrumentieren der Time-Profiler](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Zeit Profiler Instrumentieren – Liste der Beispiele](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Aufrufstruktur** – zeigt aufgewendete Zeit in jeder Methode:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

  [![](images/time2.png "Zeit Profiler Instrumentieren – Aufrufstruktur")](images/time2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/time2-vs.png "Zeit Profiler Instrumentieren – Aufrufstruktur")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Zyklen

Mithilfe des C# und F# verwalteter Code, es kann sein, üblich ist, und leider ziemlich einfach, Verweise auf Objekte zu erstellen, die nie gelöscht wird. Diese Instrumentieren können Sie diese Objekte zu ermitteln, und zeigen die Zyklen in Ihrer Anwendung verwiesen wird.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Instrumentieren von Zyklen](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Instrumentieren von Zyklen](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profilerstellung für Anwendungen

Derzeit können nur die Debug-Standardkonfigurationen, profiliert werden.

Wenn Sie ein Profil eine app mit einer anderen Konfiguration erstellen, werden Sie im folgenden Dialogfeld angezeigt werden:


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Dialogfeld "Fehler" die profilerstellung](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/image1vs.png "Dialogfeld \"Fehler\" die profilerstellung")](images/image1vs.png#lightbox) 

-----

Wählen Sie **Update** um den Vorgang fortzusetzen.

### <a name="sgen-garbage-collector-and-profiling"></a>SGen-Garbage Collector und Profilerstellung

Die [SGen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/) Garbage Collector wird für alle Xamarin-Plattformen verwendet.

SGen ist eine generationsbasierte GC, das Objekte einer Anwendung in drei Heaps zugeordnet werden – Nursery, Major Heap und die Large Object Space. Dies ermöglicht schnellere Ausführung der Garbagecollection. SGen ist derzeit die Standard-GC für Xamarin.Android und Xamarin.iOS Unified-Anwendungen.

Xamarin.iOS-Anwendung mithilfe der klassischen API, Boehm Garbage Collector – ein konservativer, nicht generationeller Garbage Collector verwendet. Wie es konservativ ist, ist es weniger wahrscheinlich, dass Sie verfügbaren Arbeitsspeicher freizugeben, die zu ungenauen Ergebnissen führen kann, wenn Sie den Profiler verwenden. Aus diesem Grund kann das änderungsinstrument Zuordnungen mit dem Boehm Garbage Collector verwendet werden.

Während ein Dialogfeld für die Meldung Sie aufgefordert werden, wenn Ihre app Boehm Garbage Collector verwendet, wechseln vorhandene iOS-Anwendung, die Boehm zu SGen ohne sorgfältige Forschung und gründliche Tests verwenden die Xamarin wird nicht empfohlen. Xamarin ist auch nicht empfehlen den Wechsel zum SGen, zur profilerstellung und das anschließende Wechsel zurück, wie diese Ergebnisse nicht genau Benchmarks der Auslastung des Speichers erhalten.

Weitere Informationen zur Speicherverwaltung von finden Sie in der [Arbeitsspeicher und bewährte Methoden für Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md) Guide.

## <a name="summary"></a>Zusammenfassung

In diesem Handbuch erläutert, welche die profilerstellung ist und wie es vorteilhaft sein, der Entwickler. Wir eingeführt, klicken Sie dann den Xamarin Profiler, bereitstellen, einige Verlauf und die Informationen in die Funktionsweise. Wir lassen die Funktionen von der Xamarin Profiler und schließlich untersucht die Zuordnungen und die Uhrzeit Profiler Instruments.

## <a name="related-links"></a>Verwandte Links

- [Arbeitsspeicher- und Leistungsproblemen bewährte Methoden](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Anmerkungen zu dieser Version](https://developer.xamarin.com/releases/profiler/preview/)
