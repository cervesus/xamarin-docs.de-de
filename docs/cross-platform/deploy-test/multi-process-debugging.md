---
title: Debuggen mit mehreren Prozessen
ms.prod: xamarin
ms.assetid: 852F8AB1-F9E2-4126-9C8A-12500315C599
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: c1f2781477c04e79ad190f0067d685af1c0a4119
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="multi-process-debugging"></a>Debuggen mit mehreren Prozessen

Es kommt sehr häufig vor, dass moderne, in Visual Studio für Mac entwickelte Projektmappen mehrere Projekte mit unterschiedlichen Zielplattformen umfassen. Eine Projektmappe kann beispielsweise ein Projekt für mobile Anwendungen enthalten, das sich auf die Daten stützt, die von einem Webdienstprojekt bereitgestellt werden. Beim Entwickeln dieser Projektmappe müssen möglicherweise beide Projekte gleichzeitig ausgeführt werden, damit der Entwickler Fehler beheben kann. Seit dem [Cycle 9-Release von Xamarin](https://releases.xamarin.com/stable-release-cycle-9/) kann Visual Studio für Mac mehrere Prozesse debuggen, die gleichzeitig ausgeführt werden. Dadurch können Breakpoints festgelegt, Variablen überprüft und Threads in mehreren aktiven Projekten angezeigt werden. Dies wird als _Debuggen mit mehreren Prozessen_ bezeichnet. 

Dieser Leitfaden enthält Informationen zu einigen an Visual Studio für Mac vorgenommenen Änderungen, um das Debuggen mehrerer Prozesse zu unterstützen. Er erläutert zudem die Konfiguration von Projektmappen zum Debuggen mehrerer Prozesse und das Anfügen an vorhandene Prozesse mit Visual Studio für Mac.

## <a name="requirements"></a>Anforderungen

Das Debuggen mehrerer Prozesse erfordert Visual Studio für Mac.

## <a name="ide-changes"></a>IDE-Änderungen

Um Entwickler beim Debuggen mehrerer Prozesse zu unterstützen, wurden einige Änderungen an der Benutzeroberfläche von Visual Studio für Mac vorgenommen. Die **Debugsymbolleiste** von Visual Studio für Mac wurde aktualisiert, und es gibt im Ordner **Projektmappenoptionen** einen neuen Abschnitt für die **Projektmappenkonfiguration**. Darüber hinaus werden nun im **Thread-Pad** laufende Prozesse und die Threads der einzelnen Prozesse angezeigt. Zudem zeigt Visual Studio für Mac für bestimmte Vorgänge wie die **Anwendungsausgabe** mehrere Debugpads an – eines für jeden Prozess.

### <a name="solution-configurations"></a>Projektmappenkonfigurationen

Visual Studio für Mac zeigt im Bereich **Projektmappenkonfiguration** der Debugsymbolleiste standardmäßig ein einzelnes Projekt an. Wenn eine Debugsitzung initiiert wird, ist dies das Projekt, das Visual Studio für Mac startet und an das der Debugger angefügt wird.

Zum Starten und Debuggen mehrerer Prozesse in Visual Studio für Mac muss eine _Projektmappenkonfiguration_ erstellt werden. Eine Projektmappenkonfiguration beschreibt, welche Projekte in einer Projektmappe enthalten sein sollten, wenn eine Debugsitzung durch Klicken auf die Schaltfläche **Start** oder Drücken von &#8984;&#8617; (**Cmd-Eingabetaste**) initiiert wird. Der folgende Screenshot veranschaulicht beispielhaft eine Projektmappe in Visual Studio für Mac, die mehrere Projektmappenkonfigurationen aufweist:

![](multi-process-debugging-images/mpd01-xs.png "Eine Projektmappe mit mehreren Konfigurationen")

### <a name="parts-of-the-debug-toolbar"></a>Bestandteile der Debug-Symbolleiste

Die Debug-Symbolleiste wurde geändert. Eine Projektmappenkonfiguration kann jetzt über ein Popupmenü ausgewählt werden. Der folgende Screenshot zeigt die Bestandteile der Debug-Symbolleiste:

![](multi-process-debugging-images/mpd02-xs.png "Bestandteile der Debugsymbolleiste")

1. **Projektmappenkonfiguration**: Die Projektmappenkonfiguration kann festgelegt werden, indem in der Debug-Symbolleiste auf „Projektmappenkonfiguration“ geklickt wird und die Konfiguration aus dem Popupmenü ausgewählt wird:

    ![](multi-process-debugging-images/mpd03-xs.png "Ein Beispiel für ein Popupfenster mit Projektmappenkonfigurationen")

2. **Buildziel**: Gibt das Buildziel für die Projekte an. Hier gibt es keine Veränderungen im Vergleich zu früheren Versionen von Visual Studio für Mac.
3. **Geräteziele**: Wählt die Geräte aus, auf denen die Projektmappe ausgeführt wird. Für die einzelnen Projekte kann ein separates Gerät oder ein separater Emulator identifiziert werden:

    ![](multi-process-debugging-images/mpd04-xs.png "Popupfenster, in dem die Geräte für ein Projekt angezeigt werden")

### <a name="multiple-debug-pads"></a>Mehrere Debug-Pads

Wenn die Konfiguration mehrerer Projektmappen gestartet wird, werden einige der Visual Studio für Mac-Pads mehrfach angezeigt – eines für jeden Prozess. Der folgende Screenshot zeigt beispielsweise zwei **Anwendungsausgabe**-Pads für eine Projektmappe, die zwei Projekte ausführt:

![](multi-process-debugging-images/mpd05-xs.png "Ausgabepad für eine Projektmappenkonfiguration")

### <a name="multiple-processes-and-the-active-thread"></a>Mehrere Prozesse und der _aktive Thread_

Wenn in einem Prozess ein Breakpoint ermittelt wurde, wird die Ausführung dieses Prozesses angehalten, während die anderen Prozesse weiter ausgeführt werden. Visual Studio für Mac kann in einem Einzelprozessszenario ohne großen Aufwand Informationen in einer einzelnen Gruppe von Pads anzeigen, z.B. Threads, lokale Variablen oder die Anwendungsausgabe. Wenn es jedoch mehrere Prozesse mit mehreren Breakpoints gibt, und möglicherweise auch mehrere Threads, kann es für den Entwickler zu einer Herausforderung werden, mit den Informationen aus einer Debugsitzung zurechtzukommen, die versucht, alle Informationen aus allen Threads (und Prozessen) gleichzeitig anzuzeigen.

Zur Behebung dieses Problems zeigt Visual Studio für Mac nur die Informationen aus einem Thread an. Dieser wird als _aktiver Thread_ bezeichnet. Der erste Thread, der an einem Breakpoint angehalten wird, gilt als _aktiver Thread_. Der aktive Thread ist der Thread, der den Fokus der Aufmerksamkeit des Entwicklers darstellt. Debug-Befehle, wie z.B. **Step Over** &#8679;&#8984;O (UMSCHALT-Cmd-O), werden an den aktiven Thread ausgegeben.

Im **Thread-Pad** werden Informationen für alle Prozesse und Threads angezeigt, die in der Projektmappenkonfiguration überprüft werden, und visuelle Hinweise zum aktiven Thread gegeben:

![](multi-process-debugging-images/mpd06-xs.png "Threadpad für eine Projektmappenkonfiguration")

Die Threads werden nach dem Prozess gruppiert, der sie hostet. Der Projektname und die ID des aktiven Threads werden in fett formatiertem Text angezeigt, und im Bundsteg neben dem aktiven Thread wird ein nach rechts zeigender Pfeil angezeigt. Im vorherigen Screenshot ist **Thread Nr.1** unter der **Prozess-ID 48703** (**FirstProject**) der aktive Thread.

Beim Debuggen mehrerer Prozesse kann der aktive Thread über den **Thread-Pad** gewechselt werden, um Debuginformationen zu diesem Prozess (oder Thread) anzuzeigen. Wählen Sie zum Wechseln des aktiven Threads unter **Thread-Pad** den gewünschten Thread aus, und doppelklicken Sie anschließend darauf.

#### <a name="stepping-through-code-when-multiple-projects-are-stopped"></a>Schrittweises Durchlaufen von Code, wenn mehrere Projekte angehalten werden

Wenn zwei (oder mehr) Projekte Breakpoints aufweisen, hält Visual Studio für Mac die Prozesse an. Für Code kann nur im aktiven Thread der Befehl **Step Over** ausgeführt werden. Der andere Prozess wird so lange angehalten, bis eine Bereichsänderung ermöglicht, dass der Debugger den Fokus aus dem aktiven Thread wechseln kann. Betrachten Sie beispielsweise den folgenden Screenshot, auf dem Visual Studio für Mac zwei Projekte debuggt:

![](multi-process-debugging-images/mpd09-xs.png  "Visual Studio für Mac: Debuggen zweier Projekte")

In diesem Screenshot verfügt jede Projektmappe über einen eigenen Breakpoint. Beim Starten des Debuggens wird der erste Breakpoint in **Zeile 10** von `MainClass` in **SecondProject** ermittelt. Da beide Projekte Breakpoints aufweisen, werden beide Prozesse angehalten. Sobald der Breakpoint erreicht wurde, bewirkt jedes Aufrufen von **Step Over**, dass Visual Studio für Mac Code im aktiven Thread schrittweise durchläuft.

Das schrittweise Durchlaufen von Code ist auf den aktiven Thread beschränkt. Daher durchläuft Visual Studio für Mac die einzelnen Codezeilen, während der andere Prozess weiterhin angehalten ist.

Nehmen wir den vorherigen Screenshot als Beispiel: Visual Studio für Mac lässt nach dem Beenden der `for`-Schleife zu, dass **FirstProject** so lange ausgeführt wird, bis der Breakpoint in **Zeile 11** in `MainClass` erreicht wurde. Bei jedem **Step Over**-Befehl geht der Debugger in **FirstProject** Zeile für Zeile vor, bis die internen heuristischen Algorithmen von Visual Studio für Mac den aktiven Thread zurück zu  **SecondProject** schalten.

Wenn nur für eines der Projekte ein Breakpoint festgelegt ist, würde nur dieser Prozess angehalten. Das andere Projekte würde weiter ausgeführt, bis es vom Entwickler angehalten wird oder ein Breakpoint hinzugefügt wurde.

### <a name="pausing-and-resuming-a-processes"></a>Anhalten und Fortsetzen eines Prozesses

Ein Prozess kann angehalten oder fortgesetzt werden, indem mit der rechten Maustaste auf den Prozess geklickt wird und **Anhalten** oder **Fortsetzen** aus dem Kontextmenü ausgewählt wird:

![](multi-process-debugging-images/mpd08-xs.png "Im Threadpad anhalten oder fortsetzen")

Abhängig von dem Status der Projekte, die gedebuggt werden, ändert sich die Darstellung der Debug-Symbolleiste. Wenn mehrere Projekte ausgeführt werden, werden in der Debug-Symbolleiste sowohl die Schaltfläche **Anhalten** als auch die Schaltfläche **Fortsetzen** angezeigt, wenn mindestens ein Projekt ausgeführt wird und ein Projekt angehalten wurde:

![](multi-process-debugging-images/mpd07-xs.png  "Symbolleiste debuggen")

Durch Klicken auf die Schaltfläche **Anhalten** in der **Debug-Symbolleiste** werden alle Prozesse, die gedebuggt werden, angehalten. Entsprechend werden durch Klicken auf die Schaltfläche **Fortsetzen** alle angehaltenen Prozesse fortgesetzt.

### <a name="debugging-a-second-project"></a>Debuggen eines zweiten Projekts

Sobald das erste Projekt von Visual Studio für Mac gestartet wurde, kann auch das Debugging eines zweiten Projekts vorgenommen werden. Machen Sie nach dem Starten des ersten Projekts im *Lösungspad* einen ***Rechtsklick** auf das Projekt, und wählen Sie **Start Debugging Item** (Debugging des Elements starten) aus:

![](multi-process-debugging-images/mpd13-xs.png  "Debugelement starten")

## <a name="creating-a-solution-configuration"></a>Erstellen einer Projektmappenkonfiguration

Eine _Projektmappenkonfiguration_ informiert Visual Studio für Mac darüber, welches Projekt ausgeführt werden soll, wenn mit der Schaltfläche **Start** eine Debugsitzung initiiert wird. Pro Projektmappe kann es mehrere Projektmappenkonfigurationen geben. Dadurch kann angegeben werden, welche Projekte beim Debuggen des Projekts ausgeführt werden.

So erstellen Sie eine neue Projektmappenkonfiguration in Xamaring Studio:

1. Öffnen Sie das Dialogfeld **Projektmappenoptionen** in Visual Studio für Mac, und wählen Sie **Ausführen > Konfigurationen** aus:

    ![](multi-process-debugging-images/mpd10-xs.png "Projektmappenkonfiguration im Dialogfeld „Projektmappenoptionen“")

2. Klicken Sie auf die Schaltfläche **Neu**, geben Sie den Namen der neuen Projektmappenkonfiguration ein, und klicken Sie anschließend auf **Erstellen**. Die neue Projektmappenkonfiguration erscheint im Fenster **Konfigurationen**:

    ![](multi-process-debugging-images/mpd11-xs.png  "Eine neue Projektmappenkonfiguration benennen")

3. Wählen Sie die neue Laufzeitkonfiguration aus der Konfigurationsliste aus. Im Dialogfeld **Projektmappenoptionen** werden die einzelnen Projekte in der Projektmappe angezeigt. Haken Sie jedes Projekt ab, das beim Initiieren einer Debugsitzung gestartet werden sollte:

    ![](multi-process-debugging-images/mpd12-xs.png "Projekt auswählen, um zu beginnen")

Die **MultipleProjects**-Projektmappenkonfiguration wird jetzt in der **Debug-Symbolleiste** angezeigt. Dadurch kann ein Entwickler die zwei Projekte gleichzeitig debuggen.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das Debuggen mehrerer Prozesse in Visual Studio für Mac erläutert. Einige der an der IDE vorgenommenen Änderungen zur Unterstützung des Debuggens mit mehreren Prozessen wurden behandelt und zugehörige Verhaltensweisen beschrieben.

## <a name="related-links"></a>Verwandte Links

- [Xamarin Cycle 9: Anmerkungen zu dieser Version](https://releases.xamarin.com/stable-release-cycle-9/)
