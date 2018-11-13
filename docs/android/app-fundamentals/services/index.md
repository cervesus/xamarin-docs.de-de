---
title: Erstellen von Android-Dienste
description: Dieses Handbuch behandelt die Xamarin.Android-Dienste, die Android-Komponenten sind, mit denen arbeiten, ohne eine aktive Benutzeroberfläche ausgeführt werden. Dienste für Aufgaben, die im Hintergrund, z. B. Herunterladen von Dateien, Wiedergeben von Musik, Zeit in Anspruch nehmen Berechnungen durchgeführt werden sehr häufig verwendet werden und so weiter. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind, und zeigt, wie sie sowohl zum Ausführen von Hintergrundaufgaben für lang ausgeführte als auch nur für die Bereitstellung einer Schnittstelle für Remoteprozeduraufrufe implementieren.
ms.prod: xamarin
ms.assetid: BA371A59-6F7A-F62A-02FC-28253504ACC9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: dfc0e1cb7239381ef2f495b0f9774d390b0dc82e
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527195"
---
# <a name="creating-android-services"></a>Erstellen von Android-Dienste

_Dieses Handbuch behandelt die Xamarin.Android-Dienste, die Android-Komponenten sind, mit denen arbeiten, ohne eine aktive Benutzeroberfläche ausgeführt werden. Dienste für Aufgaben, die im Hintergrund, z. B. Herunterladen von Dateien, Wiedergeben von Musik, Zeit in Anspruch nehmen Berechnungen durchgeführt werden sehr häufig verwendet werden und so weiter. Es wird erläutert, die verschiedenen Szenarien, denen für Dienste geeignet sind, und zeigt, wie sie sowohl zum Ausführen von Hintergrundaufgaben für lang ausgeführte als auch nur für die Bereitstellung einer Schnittstelle für Remoteprozeduraufrufe implementieren._

## <a name="android-services-overview"></a>Übersicht über die Android-Dienste

Mobile apps sind nicht wie desktop-apps. Desktops besitzen umfangreiche Mengen von Ressourcen wie z. B. Bildschirmfläche, Arbeitsspeicher, Speicherplatz und eine verbundene Stromversorgung, mobile Geräte nicht. Diese Einschränkungen erzwingen, dass mobile apps unterschiedlich Verhalten. Beispielsweise bedeutet, dass der kleine Bildschirm auf einem mobilen Gerät in der Regel, dass diese nur eine app (d. h.-Aktivität) zu einem Zeitpunkt angezeigt wird. Andere Aktivitäten sind in den Hintergrund verschoben und in einem angehaltenen Zustand, in dem Ausführen jeglicher arbeiten kann nicht, übertragen. Allerdings nur, weil in eine Android-Anwendung ist bedeutet der Hintergrund nicht, dass es für die app weiterhin nicht möglich ist. 

Android-Anwendungen bestehen aus mindestens einer der folgenden vier Hauptkomponenten: _Aktivitäten_, _Broadcast Receiver_, _Inhaltsanbieter_, und _Services_. Aktivitäten bilden den Eckpfeiler der viele hervorragende Android-Anwendungen, da sie auf die Benutzeroberfläche, die einem Benutzer ermöglicht bereitstellen, mit der Anwendung interagieren. Beim gleichzeitigen ausführen oder von Hintergrundaufgaben betrifft, sind jedoch Aktivitäten nicht immer die beste Wahl.
 
Der primäre Mechanismus für die Verarbeitung im Hintergrund in Android ist die _Service_. Ein Android-Dienst ist eine Komponente, die entwickelt wurde, ohne Benutzeroberfläche ausgeführt werden. Ein Dienst möglicherweise Herunterladen einer Datei, Musik spielen oder Anwenden eines Filters zu einem Bild. Dienste können auch verwendet werden, für die prozessübergreifende Kommunikation (_IPC_) zwischen der Android-Anwendungen. Beispielsweise könnte eine Android-app verwenden die Musik-Player-Dienst, der von einer anderen app ist, oder kann in eine app-Daten (z. B. Kontaktinformationen für eine Person) an andere apps über einen Dienst verfügbar machen. 

Dienste und ihre Fähigkeit zum Ausführen von Hintergrundaufgaben, sind entscheidend, um eine reibungslose und fließende Benutzeroberfläche bereitstellen. Alle Android-Anwendungen haben ein _Hauptthread_ (auch bekannt als eine _UI-Thread_) auf dem die Aktivitäten ausgeführt werden. Um das Gerät reaktionsfähig zu halten, muss Android die Benutzeroberfläche mit einer Rate von 60 bps und aktualisieren können. Wenn eine Android-app führt zu viel Arbeit für den Hauptthread, Android löscht Frames, wodurch wiederum die Benutzeroberfläche abgehackt angezeigt werden (manchmal auch als bezeichnet _Janky_). Dies bedeutet, dass in der Zeitspanne zwischen zwei Frames, etwa 16 Millisekunden für alle Aufgaben im UI-Thread ausführen sollten (1 Sekunde alle 60 Bilder). 

Um dieses Problem zu beheben, kann ein Entwickler verwenden Threads in einer Aktivität können einige Vorgänge, die die Benutzeroberfläche verhindern würden. Allerdings kann dies Probleme verursachen. Es ist gut möglich, dass Android wird zerstört und neu mehrere Instanzen der Aktivität erstellt. Allerdings wird Android nicht automatisch die Threads, zerstört der zu Speicherverlusten führen kann. Ein gutes Beispiel hierfür ist, wenn die [Gerät gedreht wird](~/android/app-fundamentals/handling-rotation.md) &ndash; Android versucht, zerstören die Instanz der Aktivität und erstellen Sie eine neue neu:

![Wenn das Gerät dreht, zerstört wird Instanz 1 und Instanz 2 wird erstellt.](images/image-01.png)

Hierbei handelt es sich um einen potenziellen Speicherverlust &ndash; der von der ersten Instanz der Aktivität erstellte Thread wird weiterhin ausgeführt werden. Wenn der Thread einen Verweis auf die erste Instanz der Aktivität enthält, wird dies Garbage, die das Objekt sammeln Android verhindert. Allerdings wird die zweite Instanz der Aktivität weiterhin erstellt (die wiederum einen neuen Thread erstellen kann). Drehen das Gerät mehrere Male in rascher Folge möglicherweise hat das Ziel aller ist den RAM verbrauchen und erzwungen, Android, beenden Sie die gesamte Anwendung aus, um den Arbeitsspeicher freizugeben.

Als Faustregel gilt Wenn die Arbeit zu eine Aktivität, Überleben sollten sollte klicken Sie dann ein Dienst erstellt werden um diese Aufgabe auszuführen. Allerdings ist die Arbeit nur im Kontext einer Aktivität, möglicherweise erstellen Sie dann einen Thread, um die Arbeit zu erledigen besser geeignet sein. Erstellen einer Miniaturansicht für ein Foto, das nur eine Foto-app-Katalog hinzugefügte sollte z. B. in einem Dienst wahrscheinlich auftreten. Ein Thread möglicherweise jedoch einige Musik wiedergeben, die nur wiedergegeben werden sollte, während eine Aktivität im Vordergrund ist besser geeignet.

Hintergrundaufgaben kann in zwei groben Klassifizierungen unterteilt werden:

1. **Lange ausgeführten Aufgabe** &ndash; Dies ist die Arbeit, die ausgeführt werden, bis er explizit beendet wird. Ein Beispiel für eine _Aufgabe mit langer_ ist eine app, die Musik streamt oder von einem Sensor gesammelte Daten überwachen müssen. Diese Tasks müssen ausgeführt werden, auch wenn die Anwendung keine sichtbare Benutzeroberfläche verfügt.
2. **Regelmäßige Tasks** &ndash; (auch bezeichnet als eine _Auftrag_) eine periodische Aufgabe wird ein relativ kurz (mehrere Sekunden) hat und nach einem Zeitplan ausgeführt wird (z. B. einmal pro Tag für eine Woche oder auch nur einmal der 60 Sekunden) weiter. Ein Beispiel hierfür ist eine Datei aus dem Internet herunterladen oder Generieren einer Miniaturansicht für ein Bild.

Es gibt vier verschiedene Typen von Android-Dienste:

* **Gebundene Dienst** &ndash; ein _Service gebunden_ ist ein Dienst, der eine andere Komponente (in der Regel eine Aktivität) gebunden wurde. Ein gebundener Dienst bietet eine Schnittstelle, die die gebundenen Komponente und der Dienst miteinander interagieren kann. Wenn keine weitere Clients, die an den Dienst gebunden vorhanden sind, wird Android den Dienst heruntergefahren. 

* **`IntentService`** &ndash; Ein _`IntentService`_ ist eine spezielle Unterklasse des der `Service` -Klasse, die Erstellung und Verwendung vereinfacht. Ein `IntentService` einzelne autonome Aufrufe verarbeiten soll. Im Gegensatz zu einem Dienst, der gleichzeitig mehrere Aufrufe verarbeiten kann, eine `IntentService` eher wie eine _arbeiten Warteschlange Prozessor_ &ndash; Arbeit wird in die Warteschlange eingereiht und ein `IntentService` jeden Auftrag eine zu einem Zeitpunkt auf einem einzigen Arbeitsthread verarbeitet. In der Regel eine`IntentService` nicht an eine Aktivität oder ein Fragment gebunden ist. 

* **Gestarteter Dienst** &ndash; ein _gestarteter Dienst_ ist ein Dienst, der von einigen anderen Android-Komponente (z. B. eine Aktivität) gestartet wurde und wird fortlaufend im Hintergrund ausgeführt, bis etwas explizit weist die Dienst beenden. Im Gegensatz zu einem gebundenen Dienst muss ein gestarteter Dienst keine Clients, die direkt an sie gebunden. Aus diesem Grund ist es wichtig, gestartete Dienste so entwerfen, dass sie ordnungsgemäß nach Bedarf neu gestartet werden können.

* **Hybriddienst** &ndash; ein _hybriddienst_ ist ein Dienst mit den Eigenschaften von einer _gestarteter Dienst_ und _Dienst gebunden_. Ein hybriddienst kann von gestartet werden, wenn eine Komponente binden, oder er durch ein Ereignis gestartet werden kann. Eine Clientkomponente kann oder nicht mit dem hybriddienst gebunden werden kann. Weiterhin wird ein hybriddienst ausgeführt, bis er explizit angewiesen wird, beenden oder es sind keine weitere Clients, die an es gebunden.

Der Typ des Diensts zum Maße abhängig von den anwendungsanforderungen ist. Als Faustregel gilt ein `IntentService` oder ein Dienst gebundener sind ausreichend für die meisten Aufgaben, die eine Android-Anwendung ausführen müssen, damit die Einstellung auf einen dieser beiden Arten von Diensten zugewiesen werden soll. Ein `IntentService` ist eine gute Wahl für "One-Shot" Aufgaben wie das Herunterladen einer Datei, während ein gebundener Dienst wäre geeignet, wenn häufige Interaktionen mit einer Aktivität/-Fragment erforderlich ist. 

Während die meisten Dienste im Hintergrund ausgeführt werden, besteht eine besondere Unterkategorie, bekannt als eine _Vordergrund Service_. Dies ist ein Dienst, der eine höhere Priorität, die (im Vergleich zu einem normalen Dienst) zugewiesen ist, können einige Vorgänge für den Benutzer (z. B. das Abspielen von Musik). 

Es ist auch möglich, einen Dienst in seinem eigenen Prozess auf demselben Gerät ausführen, dies wird manchmal als bezeichnet ein _Remotedienst_ oder als ein _Out-of-Process-Dienst_. Dies erfordert mehr Aufwand zu erstellen, aber für eine Anwendung bei Funktionen mit anderen Anwendungen gemeinsam muss nützlich sein kann, und können in einigen Fällen verbessern die benutzerfreundlichkeit einer Anwendung. 

### <a name="background-execution-limits-in-android-80"></a>Hintergrund Ausführung Grenzwerte für Android 8.0

Ab Android 8.0 (API-Ebene 26), einer Android-Anwendung nicht mehr haben die Möglichkeit, die kostenlos im Hintergrund ausgeführt werden. Wenn im Vordergrund kann eine app starten und Dienste ohne Einschränkungen ausgeführt. Wenn eine Anwendung in den Hintergrund verschoben wird, wird Android app gewährt eine gewisse Zeit zum Starten und Verwenden von Webdiensten. Sobald diese Zeitspanne verstrichen ist, die app kann nicht mehr alle Dienste gestartet und beendet alle Dienste, die gestartet wurden. An diesem Punkt ist es nicht möglich, für die app aus, um Aufgaben zu erledigen. Android berücksichtigt es sich um eine Anwendung im Vordergrund werden, wenn eine der folgenden Bedingungen erfüllt sind:

* Ist eine sichtbare Aktivität (gestartet oder angehalten) vorhanden.
* Die app wurde einen Foreground-Dienst gestartet.
* Eine andere app im Vordergrund ist und Komponenten aus einer app, die andernfalls im Hintergrund verwendet. Ein Beispiel hierfür ist, wenn Anwendung ein, die im Vordergrund ist,, um ein Dienst gebunden ist von Anwendung b Anwendung B dann wäre auch im Vordergrund betrachtet und von Android nicht beendet werden, werden im Hintergrund.

Es gibt einige Situationen, auch wenn eine app im Hintergrund ist, Android wird reaktiviert, die app und entspannen Sie sich diese Einschränkungen für ein paar Minuten, ermöglicht es der app einige Aufgaben ausführen:
* Eine hohe Priorität Firebase Cloud-Nachrichten, die von der app empfangen wird.
* Die app empfängt eine Übertragung an. 
* Die Anwendung empfängt eine führt eine `PendingIntent` als Reaktion auf eine Benachrichtigung.

Vorhandene Xamarin.Android-Anwendungen möglicherweise ändern, ihre Leistung Hintergrundaufgaben, um Probleme zu vermeiden, die unter Android 8.0 auftreten können. Hier sind einige praktischen Alternativen zu einer Android-Dienst:

* **Planen der Verarbeitung zur Ausführung im Hintergrund in der Android-Auftragsplaner oder [Firebase Auftrag Dispatcher](~/android/platform/firebase-job-dispatcher.md)**  &ndash; diese beiden Bibliotheken bieten ein Framework für Anwendungen, um Hintergrundaufgaben in Trennen_Aufträge_, eine einzelne Arbeitseinheit. Apps können klicken Sie dann den Auftrag mit dem Betriebssystem sowie zu den Kriterien zu planen, wenn der Auftrag ausgeführt werden kann.
* **Starten Sie den Dienst im Vordergrund** &ndash; ein Foreground-Dienst eignet sich für die bei die app im Hintergrund einige Aufgaben durchführen muss, und der Benutzer kann diese Aufgabe in regelmäßigen Abständen zu interagieren muss. Der Vordergrund-Dienst wird eine dauerhafte Benachrichtigung angezeigt, damit, dass der Benutzer bewusst, dass die app eine Hintergrundaufgabe ausgeführt wird, und bietet auch eine Möglichkeit zum Überwachen oder interagieren mit der Aufgabe. Ein Beispiel hierfür wäre eine Podcasts-app, die einen Podcast an, die dem Benutzer wiedergeben oder vielleicht ein Podcast Episode herunterladen, damit es später noch Mal gefallen werden kann. 
* **Verwenden Sie eine hohe Priorität Firebase Cloud-Nachricht (FCM)** &ndash; Wenn Android eine hohe Priorität FCM für eine app empfängt, wird diese app Services im Hintergrund ausgeführt werden, für einen kurzen Zeitraum zugelassen. Dies wäre eine gute Alternative, dass einen Hintergrunddienst, der eine app im Hintergrund abruft. 
* **Verzögern der Arbeit für wann die app im Vordergrund hat** &ndash; Wenn keiner der vorherigen Lösungen zu bewältigen sind, Entwickeln von apps müssen eigene Weise zum Anhalten und Fortsetzen der Arbeit bei die app in den Vordergrund gestellt.

## <a name="related-links"></a>Verwandte Links

* [Ausführung im Hintergrund für Android Oreo beschränkt.](https://www.youtube.com/watch?v=Pumf_4yjTMc)
