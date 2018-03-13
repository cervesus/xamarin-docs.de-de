---
title: Erstellen von Android-Diensten
description: "Dieses Handbuch erläutert Xamarin.Android-Dienste, die Android-Komponenten sind, mit die Arbeit ohne eine aktive Benutzeroberfläche ausgeführt werden können. Dienste für Aufgaben, die im Hintergrund, wie viel Zeit Berechnungen, Herunterladen von Dateien, Wiedergeben von Musik, ausgeführt werden sehr häufig verwendet werden und so weiter. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind und zeigt, wie sie sowohl zum Ausführen von Hintergrundaufgaben langer sowie für die Bereitstellung einer Schnittstelle für Remoteprozeduraufrufe implementieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 5dc1fb0fb02014e123b3a161394155bde725f288
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="creating-android-services"></a>Erstellen von Android-Diensten

_Dieses Handbuch erläutert Xamarin.Android-Dienste, die Android-Komponenten sind, mit die Arbeit ohne eine aktive Benutzeroberfläche ausgeführt werden können. Dienste für Aufgaben, die im Hintergrund, wie viel Zeit Berechnungen, Herunterladen von Dateien, Wiedergeben von Musik, ausgeführt werden sehr häufig verwendet werden und so weiter. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind und zeigt, wie sie sowohl zum Ausführen von Hintergrundaufgaben langer sowie für die Bereitstellung einer Schnittstelle für Remoteprozeduraufrufe implementieren._

## <a name="android-services-overview"></a>Android: Übersicht

Mobile apps sind nicht wie desktop-apps. Desktops verfügen über viel Mengen von Ressourcen, z. B. Bildschirmfläche, Arbeitsspeicher, Speicherplatz und eine verbundene Stromversorgung, mobile Geräte nicht. Diese Einschränkungen erzwingen mobilapps anders verhält. Beispielsweise bedeutet die kleine Bildschirm auf einem mobilen Gerät in der Regel, dass jeweils nur eine app (d. h. Aktivität) angezeigt wird. Andere Aktivitäten im Hintergrund verschoben und in einem angehaltenen Zustand, in dem sie alle Aufgaben erledigen kann, abgelegt. Jedoch nur verwendet werden, weil eine Android-Anwendung wird bedeutet der Hintergrund nicht, dass es für die app weiterhin nicht möglich ist. 

Android-Anwendungen bestehen aus mindestens einer der folgenden vier Hauptkomponenten: _Aktivitäten_, _Broadcast Empfänger_, _Inhaltsanbieter_, und _Services_. Aktivitäten sind der Eckpfeiler einer viele hervorragende Android-Anwendungen, da sie die Benutzeroberfläche, die einem Benutzer ermöglicht bereitstellen, mit der Anwendung interagieren. Bei der gleichzeitigen ausführt oder Verarbeitung im Hintergrund, sind jedoch Aktivitäten nicht immer die beste Wahl.
 
Der primäre Mechanismus für die Verarbeitung im Hintergrund in Android ist die _Service_. Ein Android-Dienst ist eine Komponente, die Aufgaben ohne Benutzeroberfläche konzipiert ist. Ein Dienst möglicherweise eine Datei herunterladen, Musik wiedergeben oder Anwenden eines Filters auf ein Bild. Dienste können auch verwendet werden, für die prozessübergreifende Kommunikation (_IPC_) zwischen der Android-Anwendungen. Z. B. kann ein Android-app verwendet die Musik-Player-Dienst, der aus einer anderen app ist, oder eine app möglicherweise Daten (z. B. Kontaktinformationen für eine Person) an andere apps über einen Dienst verfügbar machen. 

Dienste und ihre Fähigkeit zum Ausführen der Verarbeitung im Hintergrund, sind entscheidend für eine reibungslose und flüssigen Benutzeroberfläche bereitstellen. Alle Android-Anwendungen haben ein _Hauptthread_ (auch bekannt als ein _UI-Thread_) auf dem die Aktivitäten ausgeführt werden. Um das Gerät reaktionsfähig bleibt, muss Android können die Benutzeroberfläche mit einer Rate von 60 Frames pro Sekunde aktualisiert. Wenn eine Android-app führt zu viel Arbeit für den Hauptthread, Android löscht Frames, womit wiederum die Benutzeroberfläche holprig angezeigt werden (manchmal auch als bezeichnet _Janky_). Dies bedeutet, dass jede Arbeit, die im UI-Thread ausgeführt, in dem Zeitraum zwischen zwei Frames, etwa 16 Millisekunden ausführen sollten (1 Sekunde alle 60 Frames). 

Um dieses Problem zu beheben, kann ein Entwickler verwenden Threads in einer Aktivität für einige Aufgaben, die die Benutzeroberfläche verhindern würden. Dies kann jedoch Probleme verursachen. Es ist gut möglich, dass Android wird gelöscht und neu mehrere Instanzen der Aktivität erstellen. Allerdings wird Android nicht automatisch die Threads zerstört zu Speicherverlusten führen kann. Ein gutes Beispiel hierfür ist, wenn die [Gerät rotiert](~/android/app-fundamentals/handling-rotation.md) &ndash; Android versucht, Zerstören der Instanz der Aktivität, und erstellen Sie eine neue neu:

![Wenn das Gerät dreht, Instanz1 vernichtet und die Instanz 2 wird erstellt.](images/image-01.png)

Hierbei handelt es sich um einen potenziellen Speicherverlust &ndash; der von der ersten Instanz der Aktivität erstellte Thread wird weiterhin ausgeführt werden. Wenn der Thread einen Verweis auf die erste Instanz der Aktivität enthält, wird dies erfassen das Objekt Garbage Android verhindert. Allerdings ist die zweite Instanz der Aktivität weiterhin erstellt (die wiederum einen neuen Thread erstellen kann). Drehen das Gerät mehrere Male in rascher möglicherweise alle RAM ausgeschöpft und dem erzwungenen Android zum Beenden der gesamten Anwendung Arbeitsspeicher freigeben.

Als Faustregel gilt Wenn der auszuführenden Arbeiten eine Aktivität Überleben sollten sollte dann ein Dienst erstellt werden zum Ausführen dieser Aktionen. Jedoch wenn Arbeit nur im Kontext einer Aktivität anwendbar ist, kann dann erstellen einen Thread zum Ausführen der Aktionen besser geeignet sein. Erstellen einer Miniaturansicht für ein Foto, das nur eine Foto-app-Katalog hinzugefügte sollte z. B. in einem Dienst wahrscheinlich auftreten. Ein Thread möglicherweise jedoch besser geeignet ist, Musik wiedergeben, die nur während eine Aktivität im Vordergrund ist gehört werden sollte.

Verarbeitung im Hintergrund kann in zwei allgemeine Klassifizierungen unterteilt werden:

1. **Lange Ausführung der Aufgabe** &ndash; Dies ist die Arbeit, die durchgeführt werden, bis er explizit beendet wird. Ein Beispiel für eine _Aufgabe mit langer_ ist eine app, die Musik streamt oder aus einem Sensor gesammelte Daten überwachen müssen. Diese Aufgaben müssen ausführen, auch wenn die Anwendung keine sichtbare Benutzeroberfläche verfügt.
2. **Periodische Aufgaben** &ndash; (auch bezeichnet als eine _Auftrag_) eine regelmäßige Task ist eine relativ kurzer Dauer (mehrere Sekunden) ist, und nach einem Zeitplan ausgeführt wird (d. h. einmal pro Tag für eine Woche oder möglicherweise nur einmal in der nächste 60 Sekunden). Ein Beispiel hierfür ist eine Datei aus dem Internet heruntergeladen oder generieren eine Miniaturansicht für ein Bild.

Es gibt vier verschiedene Typen von Android-Dienste:

* **Dienst gebunden** &ndash; ein _Dienst gebunden_ ist ein Dienst, der eine andere Komponente (normalerweise eine Aktivität) gebunden ist. Eine gebundene Service bietet eine Schnittstelle, die die gebundenen Komponente und der Dienst miteinander interagieren kann. Sobald Sie nicht mehr Clients an den Dienst gebunden sind, wird Android den Dienst heruntergefahren.

* **`IntentService`** &ndash; Ein  _`IntentService`_  ist eine spezielle Unterklasse von der `Service` Klasse, die diensterstellung und Verwendung zu vereinfachen. Ein `IntentService` Verarbeitung einzelner autonome Aufrufe vorgesehen ist. Im Gegensatz zu einem Dienst gleichzeitig mehrere Aufrufe bewältigt werden kann, ein `IntentService` ist eher wie ein _Warteschlange Prozessor arbeiten_ &ndash; Arbeit wird in die Warteschlange gestellt und ein `IntentService` jeden Auftrag eine zu einem Zeitpunkt auf einem einzelnen Worker-Thread verarbeitet. In der Regel eine`IntentService` nicht an eine Aktivität oder ein Fragment gebunden ist. 

* **Gestarteter Dienst** &ndash; ein _gestarteter Dienst_ ist ein Dienst, der durch eine andere Android-Komponente (z. B. eine Aktivität) gestartet wurde und fortlaufendes wird im Hintergrund ausgeführt werden, bis ein Element explizit teilt die Dienst beenden. Im Gegensatz zu einem gebundenen Dienst muss ein gestarteten Dienst keine Clients, die direkt an diesen gebunden. Aus diesem Grund ist es wichtig, gestartete Diensten so entwerfen, dass sie ordnungsgemäß nach Bedarf neu gestartet werden können.

* **Hybriddienst** &ndash; ein _hybriddienst_ ist ein Dienst mit den Eigenschaften von einer _gestarteter Dienst_ und ein _Dienst gebunden_. Ein hybriddienst kann von gestartet werden, wenn eine Komponente an es gebunden oder er durch ein Ereignis gestartet werden kann. Eine Clientkomponente kann oder nicht mit dem hybriddienst gebunden werden kann. Ein hybriddienst weiterhin ausgeführt, bis er explizit angewiesen wird, beenden, oder es sind keine mehr Clients an diesen gebunden.

Welche Art des Diensts zum sehr anwendungsanforderungen abhängig ist. Als Faustregel gilt ein `IntentService` oder ein Dienst gebundener sind ausreichend für die meisten Aufgaben, die eine Android-Anwendung ausführen müssen, damit die Einstellung auf einen dieser zwei Typen von Diensten gewährt werden soll. Ein `IntentService` ist eine gute Wahl für "One-Shot" Vorgänge, z. B. das Herunterladen von Dateien, während ein gebundener Dienst wäre geeignet, wenn häufige Interaktionen mit einer Aktivität/Fragment erforderlich ist. 

Während die meisten Dienste im Hintergrund ausgeführt werden, besteht eine spezielle Unterkategorie bekannt als ein _Vordergrund Dienst_. Dies ist ein Dienst, der eine höhere Priorität (im Vergleich zu einem normalen Dienst) angegeben wird, um einige Aufgaben für den Benutzer (z. B. Wiedergabe Musik). 

Es ist auch möglich, einen Dienst in seinem eigenen Prozess ausgeführt werden, auf dem gleichen Gerät, dies wird manchmal als bezeichnet eine _Remotedienst_ oder als ein _Out-of-Process-Dienst_. Dies erfordert mehr Aufwand zu erstellen, jedoch können für eine Anwendung bei Funktionen mit anderen Anwendungen gemeinsam muss hilfreich sein, und können in einigen Fällen verbessern die benutzerfreundlichkeit einer Anwendung. 

Jeder dieser Dienste verfügt über eigene Eigenschaften und Verhalten hin und wird in ihre eigenen Handbüchern ausführlich erläutert werden.
