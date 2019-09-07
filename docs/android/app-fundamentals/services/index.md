---
title: Erstellen von Android-Diensten
description: In diesem Handbuch werden die xamarin. Android-Dienste erläutert, bei denen es sich um Android-Komponenten handelt, die das erledigen von Aufgaben ohne eine aktive Benutzeroberfläche ermöglichen Dienste werden häufig für Aufgaben verwendet, die im Hintergrund ausgeführt werden, wie z. b. zeitaufwändige Berechnungen, Herunterladen von Dateien, Abspielen von Musik usw. Es werden die verschiedenen Szenarios erläutert, für die Dienste geeignet sind, und es wird gezeigt, wie Sie sowohl zum Ausführen von Hintergrundaufgaben mit langer Laufzeit als auch zum Bereitstellen einer Schnittstelle für Remote Prozedur Aufrufe implementiert werden.
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: 7123d7d491bb32a8a506b308cac5ecb6458a1add
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754870"
---
# <a name="creating-android-services"></a>Erstellen von Android-Diensten

_In diesem Handbuch werden die xamarin. Android-Dienste erläutert, bei denen es sich um Android-Komponenten handelt, die das erledigen von Aufgaben ohne eine aktive Benutzeroberfläche ermöglichen Dienste werden häufig für Aufgaben verwendet, die im Hintergrund ausgeführt werden, wie z. b. zeitaufwändige Berechnungen, Herunterladen von Dateien, Abspielen von Musik usw. Es werden die verschiedenen Szenarios erläutert, für die Dienste geeignet sind, und es wird gezeigt, wie Sie sowohl zum Ausführen von Hintergrundaufgaben mit langer Laufzeit als auch zum Bereitstellen einer Schnittstelle für Remote Prozedur Aufrufe implementiert werden._

## <a name="android-services-overview"></a>Übersicht über Android-Dienste

Mobile Apps sind nicht wie Desktop-Apps. Desktops haben viele Ressourcen, z. b. Bildschirmimmobilien, Arbeitsspeicher, Speicherplatz und eine verbundene Stromversorgung, Mobile Geräte nicht. Diese Einschränkungen erzwingen, dass Mobile Apps anders Verhalten. Beispielsweise bedeutet der kleine Bildschirm auf einem mobilen Gerät in der Regel, dass jeweils nur eine APP (d. h. Aktivität) sichtbar ist. Andere Aktivitäten werden in den Hintergrund verschoben und in den Zustand "angehalten" versetzt, in dem Sie keine Arbeit ausführen können. Da eine Android-Anwendung im Hintergrund ist, bedeutet dies jedoch nicht, dass es nicht möglich ist, dass die APP weiterhin funktioniert. 

Android-Anwendungen bestehen aus mindestens einer der vier folgenden primären Komponenten: _Aktivitäten_, _Broadcast Empfänger_, _Inhaltsanbieter_und _Dienste_. Aktivitäten sind der Eckpfeiler vieler großartiger Android-Anwendungen, da Sie die Benutzeroberfläche bereitstellen, die einem Benutzer die Interaktion mit der Anwendung ermöglicht. Wenn es jedoch um gleichzeitige oder Hintergrundarbeit geht, sind Aktivitäten nicht immer die beste Wahl.

Der primäre Mechanismus für Hintergrundaufgaben in Android ist der- _Dienst_. Bei einem Android-Dienst handelt es sich um eine Komponente, die für einige Aufgaben ohne Benutzeroberfläche konzipiert ist. Ein Dienst kann eine Datei herunterladen, Musik abspielen oder einen Filter auf ein Image anwenden. Dienste können auch für die prozessübergreifende Kommunikation (prozessübergreifende Communication,_IPC_) zwischen Android-Anwendungen verwendet werden. Beispielsweise kann eine Android-App den Music Player-Dienst verwenden, der aus einer anderen APP besteht, oder eine APP kann Daten (z. b. Kontaktinformationen einer Person) über einen Dienst für andere apps verfügbar machen. 

Dienste und ihre Fähigkeit zur Durchführung von Hintergrundaufgaben sind entscheidend für eine reibungslose und fließende Benutzeroberfläche. Alle Android-Anwendungen verfügen über einen _Haupt Thread_ (auch als _Benutzeroberflächen Thread_bezeichnet), auf dem die Aktivitäten ausgeführt werden. Damit das Gerät reaktionsfähig bleibt, muss Android die Benutzeroberfläche mit der Rate von 60 Frames pro Sekunde aktualisieren können. Wenn eine Android-App zu viele Aufgaben im Haupt Thread durchführt, werden die Frames von Android gelöscht, was wiederum dazu führt, dass die Benutzeroberfläche ruckartig erscheint (manchmal auch als " _Janky_" bezeichnet). Dies bedeutet, dass alle Vorgänge, die im UI-Thread ausgeführt werden, in der Zeitspanne zwischen zwei Frames ausgeführt werden sollten, ungefähr 16 Millisekunden (1 Sekunde alle 60 Frames). 

Um dieses Problem zu beheben, kann ein Entwickler Threads in einer Aktivität verwenden, um einige Aufgaben auszuführen, die die Benutzeroberfläche blockieren würden. Dies kann jedoch zu Problemen führen. Es ist durchaus möglich, dass Android die mehrere Instanzen der Aktivität zerstört und neu erstellt. Allerdings werden die Threads von Android nicht automatisch zerstört, was zu Speicher Verlusten führen kann. Ein gutes Beispiel hierfür ist, wenn das [Gerät gedreht](~/android/app-fundamentals/handling-rotation.md) &ndash; wird, versucht Android, die Instanz der Aktivität zu zerstören und dann eine neue zu erstellen:

![Wenn das Gerät rotiert, wird Instanz 1 zerstört und Instanz 2 erstellt.](images/image-01.png)

Dies ist ein potenzieller Speicher &ndash; Platz. der Thread, der von der ersten Instanz der Aktivität erstellt wurde, wird weiterhin ausgeführt. Wenn der Thread über einen Verweis auf die erste Instanz der Aktivität verfügt, wird verhindert, dass Android das Objekt durch die Garbage Collection sammelt. Die zweite Instanz der Aktivität wird jedoch noch erstellt (was wiederum möglicherweise einen neuen Thread erstellt). Wenn Sie das Gerät mehrmals in der schnell Folge drehen, können Sie den gesamten RAM ausschöpfen und erzwingen, dass Android die gesamte Anwendung beendet, um Speicher freizugeben.

Als Faustregel gilt: Wenn die auszuführende Arbeit eine Aktivität übersteigen soll, sollte ein Dienst erstellt werden, um die Arbeit auszuführen. Wenn die Arbeit jedoch nur im Kontext einer Aktivität anwendbar ist, kann das Erstellen eines Threads zum Ausführen der Arbeit besser geeignet sein. Beispielsweise sollte das Erstellen einer Miniaturansicht für ein Foto, das soeben zu einer Fotogalerie-app hinzugefügt wurde, wahrscheinlich in einem Dienst auftreten. Ein Thread kann jedoch besser für die Wiedergabe von Musik geeignet sein, die nur dann gehört, wenn sich eine Aktivität im Vordergrund befindet.

Hintergrundaufgaben können in zwei allgemeine Klassifizierungen unterteilt werden:

1. **Aufgabe mit langer Ausführungszeit** &ndash; Diese Arbeit wird bis zum expliziten Beenden fortgeführt. Ein Beispiel für eine _Aufgabe mit langer Ausführungszeit_ ist eine APP, die Musik streamen oder die von einem Sensor gesammelten Daten überwachen muss. Diese Tasks müssen ausgeführt werden, obwohl die Anwendung über keine sichtbare Benutzeroberfläche verfügt.
2. **Periodische Aufgaben** (manchmal auch als Auftrag bezeichnet) eine periodische Aufgabe ist eine Regel, die relativ kurz ist (mehrere Sekunden) und gemäß einem Zeitplan ausgeführt wird (d. h. einmal täglich für eine Woche oder vielleicht einmal in den nächsten 60 Sekunden). &ndash; Ein Beispiel hierfür ist das Herunterladen einer Datei aus dem Internet oder das Erstellen einer Miniaturansicht für ein Bild.

Es gibt vier verschiedene Arten von Android-Diensten:

* **Gebundener Dienst** Ein _gebundener Dienst_ ist ein Dienst, dem eine andere Komponente (in der Regel eine Aktivität) gebunden ist. &ndash; Ein gebundener Dienst stellt eine Schnittstelle bereit, mit der die gebundene Komponente und der Dienst miteinander interagieren können. Wenn keine weiteren Clients an den Dienst gebunden sind, wird der Dienst von Android heruntergefahren. 

* **`IntentService`** Eine ist eine spezialisierte Unterklasse der- `Service` Klasse, die die Erstellung und Verwendung von Diensten vereinfacht. _`IntentService`_ &ndash; Ein `IntentService` ist für die Behandlung einzelner autonomer Aufrufe vorgesehen. Anders als bei einem-Dienst, der gleichzeitig mehrere- `IntentService` Aufrufe verarbeiten kann, ähnelt ein _Arbeits warte_ &ndash; Schlangen-Prozessor arbeiten in der `IntentService` Warteschlange, und ein verarbeitet jeden Auftrag einzeln in einem einzelnen Arbeits Thread. In der Regel`IntentService` ist eine nicht an eine Aktivität oder ein Fragment gebunden. 

* **Dienst wurde gestartet** . Ein _gestarteter Dienst_ ist ein Dienst, der von einer anderen Android-Komponente (z. b. einer Aktivität) gestartet wurde und fortlaufend im Hintergrund ausgeführt wird, bis der Dienst explizit angehalten wird. &ndash; Anders als bei einem gebundenen Dienst weist ein gestarteter Dienst keine Clients direkt an ihn an. Aus diesem Grund ist es wichtig, begonnene Dienste so zu entwerfen, dass Sie bei Bedarf ordnungsgemäß neu gestartet werden können.

* **Hybrid Dienst** Bei einem _Hybrid Dienst_ handelt es sich um einen Dienst, der die Merkmale eines _gestarteten Dienstanbieter_ und eines _gebundenen Dienstanbieter_aufweist. &ndash; Ein Hybrid Dienst kann von gestartet werden, wenn eine Komponente an ihn gebunden wird, oder er kann von einem Ereignis gestartet werden. Eine Client Komponente kann oder nicht an den Hybrid Dienst gebunden werden. Ein Hybrid Dienst wird weiterhin ausgeführt, bis er explizit beendet wird, oder bis keine Clients mehr an ihn gebunden sind.

Welcher Diensttyp verwendet werden muss, hängt von den Anwendungsanforderungen ab. Als Faustregel gilt: ein `IntentService` oder ein gebundener Dienst reicht für die meisten Aufgaben aus, die eine Android-Anwendung ausführen muss. Daher sollte eine der beiden Dienst Typen bevorzugt werden. Ein `IntentService` ist eine gute Wahl für "One-Shot"-Aufgaben, z. b. das Herunterladen einer Datei, während ein gebundener Dienst geeignet wäre, wenn häufig Interaktionen mit einer Aktivität/einem Fragment erforderlich sind. 

Während die meisten Dienste im Hintergrund ausgeführt werden, gibt es eine spezielle Unterkategorie, die als _Vordergrund Dienst_bezeichnet wird. Dabei handelt es sich um einen Dienst, der eine höhere Priorität erhält (im Vergleich zu einem normalen Dienst), um die Arbeit für den Benutzer auszuführen (z. b. das Abspielen von Musik). 

Es ist auch möglich, einen Dienst im eigenen Prozess auf demselben Gerät auszuführen. Dies wird manchmal auch als _Remote Dienst_ oder als _Prozess interner Dienst_bezeichnet. Dies erfordert mehr Aufwand für die Erstellung, kann jedoch nützlich sein, wenn eine Anwendung Funktionen für andere Anwendungen freigeben muss und in manchen Fällen die Benutzer Funktionalität einer Anwendung verbessert werden kann. 

### <a name="background-execution-limits-in-android-80"></a>Grenzwerte für die Hintergrund Ausführung in Android 8,0

Ab Android 8,0 (API-Ebene 26) ist eine Android-Anwendung nicht mehr in der Lage, frei im Hintergrund auszuführen. Im Vordergrund kann eine APP Dienste ohne Einschränkung starten und ausführen. Wenn sich eine Anwendung im Hintergrund bewegt, erteilt Android der APP einen gewissen Zeitraum, um Dienste zu starten und zu verwenden. Wenn diese Zeit abgelaufen ist, kann die APP keine Dienste mehr starten, und alle gestarteten Dienste werden beendet. An diesem Punkt ist es nicht möglich, dass die APP irgendeine Arbeit ausführt. Android betrachtet eine Anwendung als Vordergrund, wenn eine der folgenden Bedingungen erfüllt ist:

* Es ist eine sichtbare Aktivität (entweder gestartet oder angehalten) vorhanden.
* Die APP hat einen Vordergrund Dienst gestartet.
* Eine andere APP befindet sich im Vordergrund und verwendet Komponenten aus einer APP, die sich andernfalls im Hintergrund befinden würden. Dies ist beispielsweise der Fall, wenn die Anwendung a, die sich im Vordergrund befindet, an einen von Anwendung b bereitgestellten Dienst gebunden ist. Anwendung b würde dann auch im Vordergrund berücksichtigt werden und nicht von Android für die Verwendung im Hintergrund beendet werden.

Es gibt einige Situationen, in denen Android, auch wenn eine APP im Hintergrund ist, die APP reaktivieren und diese Einschränkungen für einige Minuten lockern können, sodass die APP einige Arbeit erledigen kann:
* Von der APP wird eine Firebase-Cloud-Nachricht mit hoher Priorität empfangen.
* Die App empfängt eine Broadcast-app. 
* Die Anwendung empfängt und führt als `PendingIntent` Antwort auf eine Benachrichtigung aus.

Vorhandene xamarin. Android-Anwendungen müssen möglicherweise ändern, wie Sie Hintergrundaufgaben durchführen, um Probleme zu vermeiden, die möglicherweise auf Android 8,0 auftreten. Im folgenden finden Sie einige praktische Alternativen zu einem Android-Dienst:

* **Planen der Ausführung von Arbeitsaufgaben im Hintergrund mithilfe des Android-Auftrags Planers oder des [Firebase-Auftrags](~/android/platform/firebase-job-dispatcher.md)**  Verteilers Diese beiden Bibliotheken bieten ein Framework für Anwendungen, mit denen Sie Hintergrundaufgaben in Aufträge aufteilen können. Dies ist eine diskrete Arbeitseinheit. &ndash; Apps können dann den Auftrag mit dem Betriebssystem zusammen mit einigen Kriterien planen, wann der Auftrag ausgeführt werden kann.
* **Starten Sie den Dienst im Vordergrund** . &ndash; ein Vordergrund Dienst ist nützlich, wenn die APP eine Aufgabe im Hintergrund ausführen muss und der Benutzer möglicherweise regelmäßig mit dieser Aufgabe interagieren muss. Der Vordergrund Dienst zeigt eine permanente Benachrichtigung an, sodass der Benutzer weiß, dass die APP eine Hintergrundaufgabe ausführen und eine Möglichkeit bietet, die Aufgabe zu überwachen oder mit Ihnen zu interagieren. Ein Beispiel hierfür wäre eine Podcasting-APP, die einen Podcast für den Benutzer wieder gibt oder vielleicht eine Podcasts-Episode herunterlädt, damit Sie später wiedergegeben werden kann. 
* **Verwenden einer Firebase-Cloud-Nachricht mit hoher Priorität (SCM)** &ndash; Wenn Android einen FCM mit hoher Priorität für eine App empfängt, kann diese APP Dienste für kurze Zeit im Hintergrund ausführen. Dies wäre eine gute Alternative zur Verwendung eines Hintergrund Dienstanbieter, der eine APP im Hintergrund abruft. 
* **Aufgaben für den Fall, dass die app in den Vordergrund rückt** , verzögern &ndash; Wenn keine der vorherigen Lösungen geeignet ist, müssen apps eine eigene Methode zum Anhalten und Fortsetzen der Arbeit entwickeln, wenn die APP im Vordergrund steht.

## <a name="related-links"></a>Verwandte Links

* [Android Oreo-Hintergrund Ausführungs Limits](https://www.youtube.com/watch?v=Pumf_4yjTMc)
