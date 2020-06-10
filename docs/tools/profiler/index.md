---
title: Xamarin Profiler
description: In diesem Leitfaden werden die wichtigsten Features der Xamarin Profiler erläutert. Dabei werden Profiler, Profilerstellung und Zeitpunkt der Verwendung sowie ein Standard Workflow für die Profilerstellung von xamarin-Anwendungen untersucht.
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: davidortinau
ms.author: daortin
ms.date: 06/03/2018
ms.openlocfilehash: b8b3ca4892e849f9bf08ca2910798c4b2d0f9f6f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573473"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_In diesem Leitfaden werden die wichtigsten Features der Xamarin Profiler erläutert. Dabei werden Profiler, Profilerstellung und Zeitpunkt der Verwendung sowie ein Standard Workflow für die Profilerstellung von xamarin-Anwendungen untersucht._

Der Erfolg einer Anwendung hängt von der Benutzer Leistung ab. Als Entwickler haben Sie möglicherweise einige wirklich tolle Features in Ihrer APP implementiert, aber wenn die APP träge oder vollständig ausfällt, wird der Benutzer Sie wahrscheinlich entfernen.

In der Vergangenheit hat Mono einen leistungsfähigen befehlszeilenprofiler zum Sammeln von Informationen zu Programmen, die in der Mono-Laufzeit ausgeführt werden, als [Mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/)vorgestellt. Der Xamarin Profiler eine grafische Schnittstelle für den Mono Log Profiler und unterstützt die Profilerstellung für Android-, Ios-, tvos-und Mac-Anwendungen auf Mac-und Android-, IOS-und tvos-Anwendungen unter Windows.

Der Xamarin Profiler verfügt über eine Reihe von Tools für die Profilerstellung – Zuordnungen, Zyklen und Zeit Profiler. In dieser Anleitung wird erläutert, was diese Instrumente messen und wie Sie Ihre Anwendung analysieren. Außerdem wird erläutert, wie die Daten auf den einzelnen Bildschirmen dargestellt werden.

In diesem Handbuch werden gängige Profil Erstellungs Szenarien behandelt und der Profiler als Tool für die Analyse und Optimierung von IOS-und Android-Anwendungen vorgestellt.

## <a name="download-and-install"></a>Herunterladen und Installieren

> [!NOTE]
> Sie müssen ein [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) Abonnent sein, um dieses Feature entweder in Visual Studio Enterprise unter Windows oder Visual Studio für Mac auf einem Mac entsperren zu können.

Bei der Xamarin Profiler handelt es sich um eine eigenständige Anwendung, die in Visual Studio für Mac und Visual Studio integriert ist, um die Profilerstellung innerhalb der IDE zu ermöglichen.

Laden Sie das Installationspaket für Ihre Plattform herunter:

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac-1.6.13-11.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/XamarinProfiler.Windows.Installer.1.6.10-15.msi)

Starten Sie nach dem herunterladen das Installationsprogramm, um dem System die Xamarin Profiler hinzuzufügen.

## <a name="profilers-and-profiling"></a>Profiler und Profilerstellung

Die Profilerstellung ist ein wichtiger und oft übersehener Schritt bei der Anwendungsentwicklung. Die Profilerstellung ist eine Form der **dynamischen Programmanalyse** . Sie analysiert das Programm, während es ausgeführt wird und verwendet wird. Ein Profiler ist ein Data Mining Tool, das Informationen über die Zeit Komplexität, die Verwendung bestimmter Methoden und den zugeordneten Arbeitsspeicher sammelt. Mit einem Profiler können Sie einen Drilldown durchführen und diese Metriken analysieren, um Problembereiche im Code zu ermitteln.

Beim Entwerfen und entwickeln einer Anwendung ist es wichtig, nicht vorzeitig zu optimieren. Das heißt, Sie müssen den Code in Bereichen entwickeln, auf die nur selten zugegriffen wird. Dies ist die Leistungsfähigkeit der Profilerstellung. Ein Profiler bietet Einblicke in die am häufigsten verwendeten Teile Ihrer Codebasis und hilft dabei, Bereiche zu finden, in denen Sie Zeit für Verbesserungen aufwenden sollten. Entwickler sollten darauf achten, dass Sie wissen, wo die meiste Zeit in Ihrer Anwendung aufgewendet wird und wie der Arbeitsspeicher von Ihrer Anwendung verwendet wird.

Die Profilerstellung ist für alle Arten der Entwicklung hilfreich, aber Sie ist für die Entwicklung mobiler Anwendungen besonders wichtig. Nicht optimierter Code ist auf mobilen Plattformen viel deutlicher als auf Desktop Computern, und der Erfolg Ihrer APP hängt von einem schönen und optimierten Code ab, der effizient ausgeführt wird.

## <a name="xamarin-profiler"></a>Xamarin Profiler

Der Xamarin Profiler stellt Entwicklern eine Möglichkeit zum Erstellen von Profilen für Anwendungen in Visual Studio für Mac oder Visual Studio zur Verfügung. Der Profiler sammelt und zeigt Informationen über die APP an, die dann vom Entwickler verwendet werden können, um das Verhalten der Anwendung zu analysieren. Es gibt verschiedene Möglichkeiten zum Erstellen eines Profils für eine Anwendung mit dem Xamarin Profiler, nämlich Speicher Profilerstellung und statistische Stichprobe. Diese werden über die Zuordnungen und Zeit Profil Erstellungs Instrumente durchgeführt.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Derzeit können die Xamarin Profiler verwendet werden, um xamarin. IOS-, xamarin. Android-und xamarin. Mac-Anwendungen auf dem Mac (über Visual Studio für Mac) zu testen. Der Profiler ist ein separater Prozess von der IDE und kann daher nicht nur über Visual Studio für Mac gestartet werden, sondern kann als eigenständige Anwendung verwendet werden, um exe-Dateien und Dateien zu überprüfen, `.mlpd` die aus dem [Mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/)erstellt wurden.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Derzeit kann der Xamarin Profiler verwendet werden, um xamarin. Android-Apps unter Windows (über Visual Studio und Visual Studio für Mac) zu testen. Der Profiler ist ein separater Prozess von der IDE. Daher kann er zusätzlich zum Starten von Visual Studio als eigenständige Anwendung verwendet werden, um exe-Dateien und Dateien zu überprüfen, `.mlpd` die aus dem [Mono Log Profiler](https://www.mono-project.com/docs/debug+profile/profile/profiler/)erstellt wurden.

-----

<a name="Profiler_Support"></a>

## <a name="profiler-support"></a>Profiler-Unterstützung

Die Unterstützung für die Xamarin Profiler ist auf den folgenden Plattformen verfügbar:

- Visual Studio für Mac (macOS, mit Enterprise-Lizenz)
  - Android
    - Gerät und Emulator
  - iOS
    - Gerät und Simulator
  - tvos (Zeit Instrument wird nicht unterstützt)
    - Gerät und Simulator
  - Mac

- Visual Studio (nur **Enterprise** -Version)
  - Android
    - Gerät und Emulator
  - IOS [experimentell]
    - Gerät und Simulator
  - tvOS
    - Gerät und Simulator

Beachten Sie, dass Sie **nur** Profile für **Debugkonfigurationen** erstellen können.

## <a name="profiler-basics"></a>Grundlagen zu Profiler

In diesem Abschnitt werden die Teile des Xamarin Profiler vorgestellt und die zugehörigen Features erläutert.

### <a name="allow-profiling-in-your-app"></a>Profilerstellung in ihrer App zulassen

Bevor Sie ein Profil für Ihre APP erstellen können, müssen Sie die Profilerstellung in den Projektoptionen der App zulassen.

- iOS:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  **Erstellen > IOS-Debug> aktivieren der Profilerstellung**

  ![Dialog Feld für IOS-Optionen in Visual Studio für Mac](images/ios-options-mac.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  **Eigenschaften > IOS-Build > Profilerstellung aktivieren**

  ![Dialog Feld "IOS-Optionen" in Visual Studio](images/ios-project-options-vs.png)

-----

- Android:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  **Erstellen > Android Debug-> Entwickler Instrumentation aktivieren**

  ![Dialog Feld für Android-Optionen in Visual Studio für Mac](images/android-project-options.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  **Erstellen > Android Debug-> Entwickler Instrumentation aktivieren**

  ![Dialog Feld für Android-Optionen in Visual Studio für Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Starten des Profilers

Der Xamarin Profiler kann von Ihrer IDE aus gestartet werden, wenn Sie die Profilerstellung für Ihre IOS-oder Android-Anwendung oder als eigenständige Anwendung durchlaufen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>Starten von Visual Studio für Mac

1. Stellen Sie zunächst sicher, dass die Anwendung in Visual Studio für Mac geladen ist, und wählen Sie die (standardmäßige) Debugkonfiguration aus.
2. Navigieren Sie zu **ausführen, > starten Sie die Profilerstellung**in Visual Studio für Mac, oder analysieren Sie **> Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie in der folgenden Abbildung gezeigt:

  ![Der Profiler wird von Visual Studio für Mac gestartet.](images/start-profiling-xs.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>Starten von Visual Studio aus

1. Stellen Sie zunächst sicher, dass die Anwendung in Visual Studio geladen ist, und wählen Sie die (standardmäßige) Debugkonfiguration aus, wie oben angegeben.
2. Navigieren Sie zum **Analysieren > Xamarin Profiler** in Visual Studio, um den Profiler zu öffnen, wie in der folgenden Abbildung veranschaulicht:

![Starten des Profilers aus Visual Studio](images/start-profiling-vs.png)

-----

Wenn die Menü Elemente nicht angezeigt werden, lesen Sie den [Leitfaden zur Problem](~/tools/profiler/troubleshooting.md)Behandlung.

Dadurch wird der Profiler gestartet, und die Profilerstellung für die Anwendung wird automatisch gestartet.

Der Profiler kann verwendet werden, um den Arbeitsspeicher und die Leistung zu messen. Dies erfolgt über die Instrumente für Zuordnungen und Zeitprofile, die im nächsten Abschnitt ausführlich erläutert werden.

#### <a name="saving-and-loading-profiler-sessions"></a>Speichern und Laden von Profiler-Sitzungen

Um eine Profil Erstellungs Sitzung zu einem beliebigen Zeitpunkt zu speichern, wählen Sie in der Profiler-Menüleiste **Datei > speichern unter...** aus. Dadurch wird die Datei im _MLPD_ -Format gespeichert, ein spezielles, stark komprimiertes Format für die Profilerstellung von Daten.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Nachdem es installiert wurde, finden Sie die Xamarin Profiler wie im folgenden Screenshot gezeigt im Ordner "Anwendungen":

![Eigenständigen Profiler über Mac öffnen](images/applications.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Nachdem die Anwendung installiert wurde, befindet sich die Xamarin Profiler Anwendung im Anwendungsverzeichnis:

![Öffnen Sie den eigenständigen Profiler unter Windows.](images/applications-vs.png)

-----

Sie können *. MLPD* -Dateien in den Profiler Laden, indem Sie die eigenständige Anwendung öffnen, **Wählen Sie Ziel auswählen** und die Datei laden.

Weitere Informationen finden Sie unter " [Erstellen von MLPD-Dateien](~/tools/profiler/troubleshooting.md#gen_mlpd)".

## <a name="profiler-features"></a>Profiler-Features

Der Xamarin Profiler besteht aus fünf Abschnitten, wie unten dargestellt:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Profiler-Abschnitte in Visual Studio für Mac](images/profiler-mac-sml.png)](images/profiler-mac.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Profiler-Abschnitte in Visual Studio](images/profiler-vs.png)](images/profiler-vs.png#lightbox)

-----

- **Symbolleiste** – am Anfang des Profilers finden Sie Optionen zum Starten/Starten der Profilerstellung, Auswählen eines Ziel Prozesses, Anzeigen der Laufzeit der APP und Auswählen der geteilten Sichten, die die Profiler-Anwendung bilden.
- **Instrumentations Liste** – Hiermit werden alle für die Profil Erstellungs Sitzung geladenen Instrumente aufgelistet.
- **Plotdiagramm** – diese Diagramme beziehen sich horizontal auf die relevanten Instrumente in der Instrumentations Liste. Mithilfe eines Schiebereglers (unten Zeit Profiler) kann die Skala geändert werden.
- **Instrumentierungsdetailbereich** -enthält Daten, die von der ausgewählten Ansicht des aktuellen Instrumentations Instruments angezeigt werden. Diese Sichten werden im folgenden Abschnitt ausführlicher erläutert.
- **Inspektoransicht** – enthält Abschnitte, die vom segmentierten Steuerelement ausgewählt werden können. Die Abschnitte sind vom ausgewählten Instrument abhängig und beinhaltet: Konfigurationseinstellungen, Statistiken, Stapel Verfolgungs Informationen und Pfad zu Stamm Elementen.

### <a name="allocations"></a>Bewilli

Das Zuordnungs Instrument bietet ausführliche Informationen zu Objekten in der Anwendung, während diese erstellt und in der Garbage Collection gesammelt werden.

Am Anfang des Profilers befindet sich das Zuordnungs Diagramm, in dem die Menge an Arbeitsspeicher angezeigt wird, die in regelmäßigen Abständen während der Profilerstellung zugeordnet wird. Derzeit ist das Zuordnungs Diagramm die Gesamtzahl der Zuordnungen und nicht die Größe des Heaps zu diesem Zeitpunkt. In gewisser Hinsicht geht es nie Weg, sondern es wird immer nur noch zunehmen. Dies schließt im Stapel zugeordnete Objekte ein. Abhängig von der verwendeten Laufzeitversion kann das Diagramm anders aussehen – auch für die gleiche APP.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Zuordnungs Instrument](images/allocations1.png)](images/allocations1.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Zuordnungs Instrument](images/allocations1-vs.png)](images/allocations1-vs.png#lightbox)

-----

Es gibt unterschiedliche Datenansichten im Zuordnungs Instrument, mit denen Entwickler die Verwendung und Freigabe von Arbeitsspeicher durch Ihre Anwendung analysieren können. Diese Sichten werden im folgenden beschrieben:

- **Zuordnungen – hiermit** wird eine Liste aller Zuordnungen angezeigt und nach Klassennamen gruppiert. Dies bietet eine gute Übersicht über die verwendeten Klassen und Methoden, die Häufigkeit der Verwendung und die kollektive Größe der verwendeten Klassen. Beim Doppelklicken auf eine Klasse wird der zugeordnete Arbeitsspeicher angezeigt: 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  [![Registerkarte "Zuweisungen](images/allocations3.png)](images/allocations3.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  [![Registerkarte "Zuweisungen](images/allocations2-vs.png)](images/allocations2-vs.png#lightbox)

-----

Die inspektoransicht für Zuordnungen bietet Optionen zum Filtern und Gruppieren von Objekten, zur Bereitstellung von Statistiken für zugeordnete Arbeitsspeicher und zu den obersten Zuordnungen sowie Ansichten für die Stapel Überwachung und den Pfad zum Stamm.

- **CallTree** – zeigt die gesamte Aufrufstruktur aller Threads in der Anwendung an und enthält Informationen über den Arbeitsspeicher, der auf den einzelnen Knoten zugeordnet wird. Wenn ein Element in der Liste ausgewählt wird, werden alle neben geordneten Knoten grau angezeigt. Sie können die Struktur erweitern oder auf das Element doppelklicken, um einen Drilldown auszuführen. Wenn Sie diese Datenansicht anzeigen, kann die Anzeigeeinstellungen-inspektoransicht verwendet werden, um die dargestellte Darstellung zu ändern. Derzeit gibt es zwei Optionen:
    1. **Umgekehrte Aufrufstruktur** – berücksichtigt die Stapel Überwachung von oben nach unten. Dies ist eine bequeme Ansichts Option, da Sie die tiefststen Methoden angibt, bei denen die CPU ihre Zeit aufgewendet hat.
    2. **Separat durch Thread** – diese Option organisiert die Aufrufstruktur nach Thread.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  [![Registerkarte "Callcenter"](images/allocations2.png)](images/allocations2.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  [![Registerkarte "Callcenter"](images/allocations3-vs.png)](images/allocations3-vs.png#lightbox)

-----

- **Momentaufnahmen** – in diesem Bereich werden Informationen zu Speicher Momentaufnahmen angezeigt. Wenn Sie diese während der Profilerstellung für eine Live Anwendung generieren möchten, klicken Sie auf der Symbolleiste auf die Schaltfläche _Kamera_ an jedem Punkt, den Sie sehen möchten, welcher Arbeitsspeicher beibehalten und freigegeben wird. Sie können dann auf jede Momentaufnahme klicken, um herauszufinden, was im Hintergrund geschieht. Beachten Sie, dass Momentaufnahmen nur erstellt werden können, wenn Live-Profilerstellung für eine APP 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  [![Momentaufnahmen](images/allocations4.png)](images/allocations4.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  [![Momentaufnahmen](images/allocations4-vs.png)](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>Zeit Profiler

Mit dem instrumentierungsinstrumentierungsinstrument wird genau gemessen, wie viel Zeit in den einzelnen Methoden einer Anwendung aufgewendet wird. Die Anwendung wird in regelmäßigen Abständen angehalten, und es wird eine Stapel Überwachung in jedem aktiven Thread ausgeführt. Jede Zeile im Bereich "Instrumentations Details" zeigt den Ausführungs Pfad an, der befolgt wurde.

Das Zeichnungs Diagramm zeigt, wie im folgenden Screenshot gezeigt, die Anzahl der von der APP empfangenen Beispiele an, während Sie ausgeführt wird:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Zeit-profilerinstrument](images/time1.png)](images/time1.png#lightbox) 

[![Zeit Profil Erstellungs Instrument – Beispielliste](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Zeit-profilerinstrument](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![Zeit Profil Erstellungs Instrument – Beispielliste](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **Aufruf** Struktur – zeigt die für jede Methode aufgewendeten Zeit an:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

  [![Zeit Profil Erstellungs Instrument – Aufruf Struktur](images/time2.png)](images/time2.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

  [![Zeit Profil Erstellungs Instrument – Aufruf Struktur](images/time2-vs.png)](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>Cycles

Durch die Verwendung von verwaltetem Code in c# und F # kann es recht häufig vorkommen, und leider ist es recht einfach, Verweise auf Objekte zu erstellen, die nie verworfen werden. Mit diesem Instrument können Sie diese Objekte lokalisieren und die Zyklen anzeigen, auf die in der Anwendung verwiesen wird.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Cycles-Instrument](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Cycles-Instrument](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>Profil Erstellungs Anwendungen

Derzeit können nur die standarddebugkonfigurationen erstellt werden.

Wenn Sie ein Profil für eine APP mit einer anderen Konfiguration erstellen, wird das folgende Meldungs Dialogfeld angezeigt:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Profil Erstellungs Fehler](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Profil Erstellungs Fehler](images/image1vs.png)](images/image1vs.png#lightbox) 

-----

Wählen Sie **Aktualisieren** , um fortzufahren.

### <a name="sgen-garbage-collector-and-profiling"></a>Sgen-Garbage Collector und Profilerstellung

Der [Sgen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/) -Garbage Collector wird für alle xamarin-Plattformen verwendet.

Sgen ist ein generationsübergreifender GC, mit dem Objekte einer Anwendung in drei Heaps zugeordnet werden – Gärtnerei, Haupt Heap und Large Object Raum. Dies ermöglicht eine schnellere Ausführung von Garbage Collection. Sgen ist derzeit die Standard-GC für xamarin. Android und vereinheitlichte xamarin. IOS-Anwendungen.

Die xamarin. IOS-Anwendung, die den Classic API verwendet, verwendet den Boehm-GC – eine konservative, nicht generelle Garbage Collector. Da es konservativ ist, ist es weniger wahrscheinlich, dass Sie verfügbaren Arbeitsspeicher freigeben, was bei der Verwendung des Profilers zu ungenauen Ergebnissen führen kann. Aus diesem Grund kann das Zuordnungs Instrument nicht mit dem Boehm-Garbage Collector verwendet werden.

Obwohl Sie mit einem Meldungs Dialogfeld aufgefordert werden, wenn Ihre APP den Boehm-GC verwendet, empfiehlt xamarin nicht, eine vorhandene IOS-Anwendung, die Boehm verwendet, ohne sorgfältige Untersuchung und gründliche Tests in Sgen zu wechseln. Xamarin empfiehlt außerdem nicht, zur Profilerstellung zu Sgen zu wechseln und dann zurückzukehren, da diese Ergebnisse keine genauen Benchmarks der Speicherauslastung bereitstellen.

Weitere Informationen zur Speicherverwaltung finden Sie im Handbuch mit [bewährten Methoden für Arbeitsspeicher und Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md) .

## <a name="summary"></a>Zusammenfassung

In dieser Anleitung haben wir uns mit der Profilerstellung und der Vorteile des Entwicklers beschäftigt. Anschließend haben wir die Xamarin Profiler eingeführt und einige Verlaufs Informationen und Informationen zur Funktionsweise bereitgestellt. Schließlich haben wir die Features der Xamarin Profiler besucht und die Zuordnungen und die Zeit der Profiler-Instrumente untersucht.

## <a name="related-links"></a>Verwandte Links

- [Bewährte Methoden für Arbeitsspeicher und Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Versionshinweise](/xamarin/tools/profiler/release-notes/)
