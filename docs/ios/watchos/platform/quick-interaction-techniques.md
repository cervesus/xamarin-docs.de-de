---
title: Schnelle Interaktion Techniken für WatchOS 3 in Xamarin
description: Dieser Artikel behandelt die schnelle Interaktion Techniken Apple verfügt über zusätzliche in WatchOS 3 und deren in Xamarin.iOS für Apple Watch-Implementierung.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a62f6f153508dbd03bda569000357f3093d3e214
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791474"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Schnelle Interaktion Techniken für WatchOS 3 in Xamarin

_Dieser Artikel behandelt die schnelle Interaktion Techniken Apple verfügt über zusätzliche in WatchOS 3 und deren in Xamarin.iOS für Apple Watch-Implementierung._

Eine schnelle Benutzerinteraktionen sind für das Erstellen von überzeugenden Apple Watch-apps und Komplikationen unverzichtbar. Neue WatchOS 3, ist Apple Unterstützung für den Zugriff auf die digitalen Crown und eine neue Benachrichtigung für Benutzer und Navigation Techniken Geste Merkmale hinzugefügt. Auf diese, zusammen mit Unterstützung für die SceneKit SpriteKit, können Entwickler auf einfache Weise umfangreiche, anzeigbare Schnittstellen zu erstellen, die schnelle und Reaktionsfähigkeit sind.

## <a name="what-are-quick-interactions"></a>Was sind schnelle Interaktionen

Für Entwickler, die zum Erstellen von Anwendungen für iOS oder MacOS (wobei die Zeitdauer ein Benutzer an der app interagieren, die in Minuten oder Stunden gemessen wird) verwendet wird, kann eine Herausforderung sein, entwerfen eine erfolgreiche app für die Apple Watch und erfordert einen anderen Ansatz.

In WatchOS möchte der Benutzer in der Regel ihre Handgelenk auslösen, schnell interagieren mit einer Anwendung (normalerweise für kurzen wenigen Sekunden), und klicken Sie dann ihre Handgelenk löschen und fortsetzen nach Belieben war, dass diese Aktivität zurückzukehren.

Es folgen einige Beispiele für typische schnelle Interaktionen auf der Apple Watch:

- Startet einen Zeitgeber.
- Überprüfen das Wetter.
- Markieren ein Element aus einem TODO-Liste.

Um diese Ziele zu erreichen, muss eine Anwendung auf der Apple Watch sein:

- **Anzeigbare** – d. h., die mit einer kurzen den Benutzer Blick sollte in der Lage, die Informationen zu erhalten, die sie benötigen. 
- **Handlungsbedarf** -was bedeutet, dass Benutzer sollten in der Lage, schnell und gut fundierte Entscheidungen zu treffen.
- **Reaktionsfähig** -was bedeutet, dass den Benutzer sollte nie warten, um die Informationen zu erhalten, die sie benötigen und/oder die Aktion zu erreichen, sie erhalten möchten.

### <a name="quick-interactions-length"></a>Schnelle Interaktionen Länge

Aufgrund der anzeigbare Natur von Apple Watch-apps, Apple wird vorgeschlagen, die ideale Länge einer schnellen Aktivität zwei Sekunden liegen oder weniger. Als Ergebnis dieser zwei zweiten Grenzwert müssen der Entwickler eine erhebliche Zeit sowohl entwerfen und Implementieren einer Apple Watch-app verbringen. 

## <a name="new-watchos-3-features-and-apis"></a>Neue WatchOS 3-Funktionen und APIs

Apple hat mehrere neue Funktionen und APIs WatchKit der Entwickler bei der schnelle Interaktionen auf ihrer Apple Watch-apps hinzufügen hinzugefügt:

- WatchOS 3 bietet Zugriff auf neue Arten der Benutzereingabe angegeben, wie z. B.:
    - Geste Prüfer
    - Digitale Crown Drehung 
- WatchOS 3 bietet neue Methoden zum Anzeigen und aktualisieren die Informationen, wie z. B.:
    - Erweiterte Tabelle – navigation
    - Neue Benutzerbenachrichtigung Framework-Unterstützung
    - Integration von SpriteKit und SceneKit

Implementieren diese neuen Features, kann der Entwickler sicherstellen, dass ihre app WatchOS 3 Glanceable, Actionable und reagierend ist.

### <a name="gesture-recognizer-support"></a>Unterstützung für die Erkennung Gesten

Wenn der Entwickler in iOS-Geste Prüfer implementiert, werden sie mit der Funktionsweise von Geste Prüfer in WatchOS 3 sehr vertraut vorkommen. Um zu aktualisieren, sind Geste Prüfer Objekte, die auf niedriger Ebene Berührungsereignisse als erkennbar, vordefinierte Gesten analysiert werden soll.

WatchOS 3 unterstützt die vier folgenden Geste Merkmale:

- Diskrete Gesten Typen:
    - Streichen Sie nach Bewegung (`WKSwipeGestureRecognizer`).
    - Die Tap-Geste (`WKTapGestureRecognizer`).
- Fortlaufende Gestenhandler-Typen:
    - Die Aktion zum Schwenken (`WKPanGestureRecognizer`).
    - Long-drücken Sie die Bewegung (`WKLongPressGestureRecognizer`).

Um die neue Geste Merkmale zu implementieren, ziehen Sie es auf eine Entwurfsoberfläche in der iOS-Designer in Visual Studio für Mac und konfigurieren Sie ihre Eigenschaften.

Reagieren Sie auf die Aktion der Erkennung, behandeln die Aktion, die vom Benutzer ausgelöst wird, im Code. Erneut wird dies auf die gleiche Weise ausgeführt, als es im iOS behandelt werden würde.

#### <a name="discrete-gesture-states"></a>Diskreten Gestenhandler-Status

Bei diskreten Gesten Aktion aufgerufen werden, wenn die Aktion erkannt wird und ein Zustand (`WKGestureRecognizerState`) als zugewiesen ist:

[![](quick-interaction-techniques-images/quick01.png "Diskreten Gestenhandler-Status")](quick-interaction-techniques-images/quick01.png#lightbox)

Alle diskreten Gesten fangen die `Possible` Status- und Übergangsinformationen, die entweder die `Failed` oder `Recognized` Zustand. Wenn Sie diskrete Gesten verwenden, behandeln nicht der Entwickler direkt mit dem Status im Allgemeinen. Stattdessen greifen sie auf die Aktion aufgerufen werden, wenn die Aktion nur erkannt wird.

#### <a name="continuous-gesture-states"></a>Fortlaufende Gestenhandler-Zustände

Fortlaufende Gesten unterscheiden sich geringfügig von diskrete Gesten, wenn die Aktion mehrere Male aufgerufen wird, wie die Aktion erkannt wird:

[![](quick-interaction-techniques-images/quick02.png "Fortlaufende Gestenhandler-Zustände")](quick-interaction-techniques-images/quick02.png#lightbox)

Erneut, fortlaufende Gesten startet mit dem `Possible` Status, aber sie über mehrere Updates ausgeführt. Hier muss der Entwickler Zustands der und Aktualisieren der Benutzeroberfläche der Anwendung während der `Changed` phase, bis schließlich die Bewegung ist `Recognized` oder `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Tipps zur Verwendung von Gestenhandler-Erkennung

Apple empfehlen Folgendes aus, bei der Arbeit mit Geste in WatchOS 3:

- Fügen Sie die Bewegung Prüfer so gruppieren Sie Elemente statt einzelne Steuerelemente hinzu. Da der Apple Watch eine geringere Größe des physischen Bildschirms aufweist, gruppieren Sie Elemente tendenziell größer werden und einfacher Ziele für den Benutzer, erreicht werden soll. Darüber hinaus können die Bewegung Prüfer Konflikt mit Gesten bereits in der systemeigenen UI-Steuerelemente integriert.
- Legen Sie abhängigkeitsbeziehungen im Storyboard Watch-app.
- Einige Geste haben Vorrang gegenüber anderen Typen Geste wie z. B. ein:
    - Durchführen eines Bildlaufs
    - Touch erzwingen
 
### <a name="digital-crown-rotation"></a>Digitale Crown Drehung

Durch die Implementierung Digital Crown-Unterstützung in ihren apps WatchOS 3, kann ein Entwickler verbesserte Navigation Geschwindigkeit und Genauigkeit Interaktionen für ihre Benutzer bereitstellen.

Seit WatchOS 2, Apple Watch-app verwenden konnte die `WKInterfacePicker` Objekt, das digitale Kronenlänge zugreifen, eine Liste der `WKPickerItems` und ein Datumsauswahl-Format (Listen-, Gestapelte oder Image-Sequenz). WatchOS zulässig klicken Sie dann den Benutzer die digitale Crown zu verwenden, um ein Element aus der Liste auswählen.

Bei Verwendung einer `WKInterfacePicker`, WatchKit Großteil der Arbeit von behandelt wird:

- Zeichnen die Liste und die einzelnen Oberflächenelemente an.
- Die digitale Crown Ereignisse verarbeitet.
- Rufen eine Aktion aus, wenn ein Element ausgewählt ist.

Neue zu WatchOS 3, hat der Entwickler jetzt direkten Zugriff auf die digitalen Crown drehungsereignisse, das Ihnen ermöglicht, ihre eigenen Elemente der Benutzeroberfläche zu erstellen, die auf die Rotation reagieren.

Digitale Crown Zugriff wird durch die folgenden Elemente bereitgestellt:

- `WKCrownSequencer` – Bietet Zugriff auf Drehungen pro Sekunde.
- `WKCrownDelegate` – Bietet Zugriff auf rotierenden Delta-Ereignisse.

#### <a name="rotations-per-second"></a>Drehungen pro Sekunde

Digitale Kronenlänge zugreifen Drehungen pro Sekunde ist beim Arbeiten mit physikalische Animationen basierend hilfreich. Um Drehungen pro Sekunde zuzugreifen, verwenden die `CrownSequencer` Eigenschaft von der `WKInterfaceController` des Watch-Erweiterung. Zum Beispiel:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Rotierenden Deltas

Verwenden Sie die rotierenden Deltas aus der digitalen Crown, um die Anzahl der Drehungen zählen. Verwenden der `CrownDidRotate` Überschreiben der Methode der `WKCrownDelegate` auf rotierenden Deltas zugreifen. Zum Beispiel:

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class CrownDelegate : WKCrownDelegate
    {
        #region Computed Properties
        public double AccumulatedRotations { get; set;}
        #endregion

        #region Constructors
        public CrownDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
        {
            base.CrownDidRotate (crownSequencer, rotationalDelta);

            // Accumulate rotations
            AccumulatedRotations += rotationalDelta;
        }
        #endregion
    }
}
```

Hier die app verwaltet ein Akkumulator (`AccumulatedRotations`) um die Anzahl der Drehungen zu bestimmen. Eine vollständige Drehung von digitalen Kronenlänge entspricht eine kumulierte Delta der `1.0` und eine halbe Drehung wäre `0.5`.

Apple hat bleibt es Aufgabe des Entwicklers, um zu bestimmen, wie die Anzahl der Drehung der Vertraulichkeit der Änderungen auf das Element der Benutzeroberfläche aktualisiert entsprechen.

Das Vorzeichen (`+/-`) des rotierenden Delta gibt die Richtung an, dass der Benutzer die digitale Crown aktivieren ist:

[![](quick-interaction-techniques-images/quick03.png "Die Vorzeichen des Deltas rotierenden gibt die Richtung an, dass der Benutzer die digitale Crown aktivieren ist")](quick-interaction-techniques-images/quick03.png#lightbox)


Wenn der Benutzer sich scrolling ist zurück positive Deltas und einen Bildlauf nach unten, klicken Sie dann negative Deltas zurückgegeben werden unabhängig davon, welche Ausrichtung der Benutzer die Überwachung in tragen, WatchKit.

#### <a name="digital-crown-focus"></a>Digitale Crown Fokus

Genau wie andere Oberflächenelemente verwendet digitale Kronenlänge das Konzept der Fokus. Weg von der digitalen Crown kann diese Fokus verschoben werden, für andere Elemente der Benutzeroberfläche basierend auf wie der Benutzer mit der Apple Watch interagiert. 

Beispielsweise konnte keines der folgenden Steuerelemente den Fokus von der digitalen Crown stehlen:

- Datumsauswahl
- Schieberegler
- Durchführen eines Bildlaufs Controller

Es obliegt dem Entwickler, die bestimmen, wann ihre benutzerdefinierte Schnittstelle-Element den Fokus von der digitalen Crown werden muss. Apple wird vorgeschlagen, mit der neuen Geste Prüfer um den Fokus auf das benutzerdefinierte Element der Benutzeroberfläche zu erhalten.

### <a name="vertical-paging"></a>Vertikale Paging

Die Standardmethode, dass ein Benutzer einer Tabellenansicht in einer app WatchOS navigiert wird, einen Bildlauf zu der gewünschten Datenelement, tippen auf eine bestimmte Zeile an die Detailansicht anzuzeigen, tippen Sie auf die Schaltfläche "zurück" nach Abschluss der Details anzeigen, und wiederholen den Vorgang für alle anderen Informationen, die die y interessiert sind, aus der in der Tabelle:

[![](quick-interaction-techniques-images/quick04.png "Verschieben zwischen einer Tabelle und der Detailansicht")](quick-interaction-techniques-images/quick04.png#lightbox)

Neue zu WatchOS 3, der Entwickler kann vertikale Paging auf aktivieren die Tabellenansicht-Steuerelemente. Diese Funktion aktiviert ist kann der Benutzer scrollen, um eine Tabellenansicht Zeile suchen, und tippen Sie auf die Zeile, um seine Details als vor dem anzeigen. Allerdings können sie jetzt oben streichen wählen Sie die nächste Zeile in der Tabelle oder abwärts bis zu der vorherigen Zeile (oder die digitale Crown), alle ohne anzuzeigende Tabelle zurückgeben zuerst:

[![](quick-interaction-techniques-images/quick05.png "Verschieben zwischen einer Tabelle und der Detailansicht und Streifen nach oben oder unten verschieben zwischen den anderen Zeilen")](quick-interaction-techniques-images/quick05.png#lightbox)

Um diesen Modus aktivieren, die WatchOS app Storyboard in Xcode zum Bearbeiten geöffnet, wählen Sie die Ansicht der Tabelle und überprüfen Sie die **vertikale Detail Paging** Kontrollkästchen:

[![](quick-interaction-techniques-images/quick06.png "Aktivieren Sie das Kontrollkästchen mit vertikalen Detail Paging")](quick-interaction-techniques-images/quick06.png#lightbox)

Stellen Sie sicher, dass die Tabelle Segues verwendet, um detaillierte anzeigen und speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac synchronisiert werden.

Der Entwickler Programmbüros kann programmgesteuert vertikale Paging zu einer bestimmten Zeile, die mithilfe des folgenden Codes für eine Tabellenansicht ausführen:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Wenn vertikale Paging verwenden zu können, muss der Entwickler bewusst sein, dass WatchKit automatisch das Vorabladen von Domänencontrollern behandelt und daher einige Lifecycle Controllermethoden aufgerufen werden, können bevor die Benutzeroberfläche tatsächlich angezeigt wird.

### <a name="notification-enhancements"></a>Erweiterungen der Benachrichtigung

Benachrichtigung sind die Primärform von schnelle Interaktion, die ein Benutzer in der Regel auf WatchOS datenschreiberprozess und wurden seit dem ersten Apple Watch und WatchOS 1 verfügbar.

Eine typische Benachrichtigung Interaktion schnelle lautet wie folgt:

1. Der Benutzer idealer Benachrichtigung Haptic, wenn eine neue Benachrichtigung empfangen wird.
2. Ihre Handgelenk Beiträgen kurze suchen-Schnittstelle für die Benachrichtigung ausgelöst.
3. Wenn sie ihre Handgelenk ausgelöst beibehalten weiterhin, WatchOS erfolgt automatisch ein Übergang in die Oberfläche lange suchen-Benachrichtigung.

Es gibt mehrere Möglichkeiten, die ein Benutzer auf die Benachrichtigung reagieren kann:

- Für einen gut definierten und Benachrichtigung angezeigt wird der Benutzer nichts, und schließen einfach die Benachrichtigung.
- Sie können auch der Benachrichtigung zum Starten der app WatchOS tippen.
- Für eine Benachrichtigung, die benutzerdefinierte Aktionen unterstützt, kann der Benutzer eine der benutzerdefinierten Aktionen auswählen. Dies können entweder sein:
    - **Vordergrund Aktionen** -diese app zur Ausführung der Aktion starten.
    - **Aktionen im Hintergrund** – immer auf dem iPhone in WatchOS 2 weitergeleitet wurden, aber an die WatchApp in WatchOS 3 weitergeleitet werden können.

Für WatchOS 3:

* Benachrichtigung verwenden eine ähnliche API wie auf allen Plattformen (iOS, WatchOS, tvos. außerdem wurden und MacOS).
* Lokale Benachrichtigung, kann auf der Apple Watch geplant werden.
* Hintergrund-Benachrichtigung wird an die app-Erweiterung weitergeleitet werden, wenn sie auf der Apple Watch geplant wurden.

#### <a name="notification-scheduling-and-delivery"></a>Benachrichtigung zeitplanung und Übermittlung

Benachrichtigung des Benutzers iPhone werden auf der Apple Watch vorwärts verwendet den folgenden Fällen:

* Bildschirm für das iPhone ist deaktiviert.
* Der Apple Watch ist abgenutzt werden und wurde entsperrt.

In WatchOS 3 werden lokalen Benachrichtigungen können für die Apple Watch geplant werden und nur auf der Apple Watch. Es liegt die dem Entwickler, einen entsprechenden iPhone Benachrichtigung zu planen, wenn sie von der app erforderlich ist.

Durch Einschließen der gleichen Benachrichtigung Bezeichner auf der Apple Watch und die iPhone-Versionen der Benachrichtigungen, wird verhindert, dass es doppelte Benachrichtigungen auf der Apple Watch angezeigt wird. Die Apple Watch-Version der Benachrichtigung wird über die iPhone-Version Vorrang.

Da WatchOS 3 dieselben `UINotification` -API-Framework als iOS 10, finden Sie in unserer iOS 10 [Benutzer Benachrichtigungsframeworks](~/ios/platform/user-notifications/index.md) Dokumentation.

### <a name="using-spritekit-and-scenekit"></a>Verwenden von SpriteKit und SceneKit

Neue zu WatchOS 3, der Entwickler können nun SpritKit und SceneKit Objekte in ihrer app User Interface Design 2D-und 3D-Grafik vorhanden.

Zwei neue Schnittstellenklassen wurden hinzugefügt, um dieses Feature zu unterstützen:

- `WKInterfaceSKScene` – Für das Arbeiten mit SpriteKit 2D Grafiken.
- `WKInterfaceSCNScene` – Für das Arbeiten mit 3D-Grafiken SceneKit.

Um diese Objekte zu verwenden, ziehen Sie sie auf der Entwurfsoberfläche innerhalb der Watch-app-Storyboard in Xcodes Benutzeroberflächen-Generator, und verwenden Sie die **Attribute Inspektor** für deren Konfiguration.

Ab diesem Punkt funktioniert arbeiten mit dem SpriteKit oder SceneKit Szenen genauso wie innerhalb einer iOS-app funktioniert. Die Watch-app bietet ein `WKInterfaceSKScene` durch Aufrufen einer der `Present` Methoden. Für SceneKit, legen Sie einfach die `Scene` Eigenschaft von der `WKInterfaceSCNScene` Objekt.

## <a name="actionable-complications"></a>Handlungsbedarf Komplikationen

Apple eingeführt in WatchOS 2 Komplikationen für 3. apps von Drittanbietern. In WatchOS 3 ist Apple die Möglichkeit erweitert, die ein Entwickler in einem WatchKit Komplikation einschließen können. 

Darüber hinaus mehrere der integrierten gesichtsabbildungen können im Überwachungsfenster enthalten nun Komplikationen und vorhandenen überwachen die Seite bereits unterstützte Komplikationen kann auch Weitere Komplikationen nun enthalten.

Neue ist auch die Möglichkeit für einen Benutzer zu schnell streichen Sie nach links oder rechts, um über alle von der Überwachung gesichtsabbildungen übergehen, die auf ihrer Apple Watch auf ihnen installiert sein. Der neue Katalog in der Apple Watch Companion iPhone-app verwenden, kann der Benutzer hinzufügen und anpassen, neue Überwachung Flächen und keines der Komplikationen, die sie einschließen können.

Aufgrund dieser neuen Funktionen vorgeschlagen Apple an, dass alle Apps auf der Apple Watch auch mindestens eine Komplikation enthalten sollte und daher alle die systemeigene Apple Watch-app jetzt berücksichtigen.

Komplikationen bieten folgende Funktionen zu einer app:

- Sie sind hoch anzeigbare, da sie immer auf dem Zifferblatt der Uhr vorhanden sind.
- Komplikationen werden häufig von WatchOS aktualisiert. Jeder app, die eine Komplikation auf Zifferblatt der aktuell angezeigten Überwachen des Benutzers enthält, wird mindestens zweimal pro Stunde aktualisiert.
- Jede mit einer Komplikation auf Zifferblatt der der Benutzer aktuell angezeigte Watch-app wird im Arbeitsspeicher gespeichert, die macht der app schnell starten und die Geschwindigkeit von Antworten aus der app verbessert.
- Komplikationen erleichtern die für den Benutzer aus, um bestimmte Funktionen in einer WatchOS-app zu starten.

## <a name="glanceable-notification"></a>Anzeigbare Benachrichtigung

Benachrichtigung auf der Apple Watch bieten eine hervorragende und anpassbare Möglichkeit, schnell die Benutzer von Ereignissen oder neue Informationen, wie eingehende Nachrichten oder das Erreichen eines Ziels in einer app Trainings zu informieren.

Verwenden Sie eine Benachrichtigung, kann wertvoller Informationen schnell die dem Benutzer angezeigt werden. In vielen Situationen Dank eine gut entworfene Benachrichtigung die Notwendigkeit für den Benutzer auf die app zu starten.

Neu bei WatchOS 3, alle Benachrichtigungen jetzt Unterstützung:

- SpriteKit
- SceneKit
- Inline-Video

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Verbesserte Benutzeroberfläche mit SpriteKit und SceneKit

In der Regel kann ein Entwickler Spiel UI vorstellen beim SpriteKit und SceneKit genannt werden. SpriteKit und SceneKit kann jedoch nützlich zum Erstellen von nicht-Spiele Benutzeroberflächen, die benutzerdefinierte Layouts, Inhalt enthalten und Animationen, die nicht anderweitig in WatchKit allein möglich sind.

Angenommen, können eine Benachrichtigung für Benutzer aus einer Freigabe-app Fotos SpriteKit eine umfassenden Benutzeroberfläche bereitstellen, indem Sie z. B. des Benutzers, der das Bild sowie ein tatsächliches Bild und andere benutzerdefinierte Informationen, die die benutzerfreundlichkeit verbessern zurückgesendet.

Darüber hinaus können SpriteKit und SceneKit mit standard WatchKit-UI-Elemente in der app-Benutzeroberfläche Entwurf kombiniert werden.

## <a name="simple-navigation"></a>Einfache Navigation

WatchOS 3 bietet verschiedene Möglichkeiten, dass ein Entwickler die Navigation in ihren apps WatchOS beispielsweise auf den neuen vereinfachen [vertikale Paging](#Vertical-Paging), [Geste Erkennungsmodul Unterstützung](#Gesture-Recognizer-Support) und [digitale Crown Drehung](#Digital-Crown-Rotation) Funktionen, die oben aufgeführten.

Digitale Kronenlänge ist für die Apple Watch eindeutig und kann auf viele verschiedene Arten zum Vereinfachen der Navigation verwendet werden. Z. B. können eine Zeitgeber-Anwendung digitale Kronenlänge über die verfügbaren Timer Längen streichen.

Benutzerdefinierte Gesten können die neue und einzigartigen Möglichkeiten für den Benutzer für die Interaktion mit einem Watch-app vorhanden und können auch verwendet werden, um die app Navigation zu vereinfachen.

Apple wird empfohlen, Suche nach Möglichkeiten, um alle neuen schnelle Interaktion Funktionen hinzugefügt, die in WatchOS 3 zum Präsentieren von umfangreichen, einfach und schnell WatchOS app Schnittstellen verwenden, kombinieren.

## <a name="finishing-the-quick-interaction"></a>Die schnelle Aktivität abgeschlossen

Eine wohlgeformte schnelle Interaktion Erfahrung erhalten die Benutzer das Vertrauen ihrer Handgelenk löschen (und mit der app zu trennen) Wenn sie die aktuelle Aktivität abgeschlossen haben.

Insbesondere wird ist ein Problem beim Watch-app wird auf diese Weise jede Art von Netzwerkverbindung oder Freigeben von Informationen für eine Begleit-iPhone-app. Dies kann sich häufig auf eine wartende Indikator führen, während die Transaktion eingerichtet ist, ausgeführt wird, die während einer schnellen Interaktion nicht wünschenswert ist. Betrachten Sie das folgende Beispiel:

[![](quick-interaction-techniques-images/quick07.png "Diagramm der Watch-app auf diese Weise einer Netzwerkverbindungs und Freigeben von Informationen für eine Begleit-iPhone-app")](quick-interaction-techniques-images/quick07.png#lightbox)

1. Der Benutzer wählt ein Element auf der Apple Watch erwerben.
2. Sie tippen Sie auf die Schaltfläche "kaufen".
3. Die app startet die Netzwerktransaktion und zeigt einen Indikator laden.
4. Einige Zeit später die Transaktion abgeschlossen ist und die app zeigt eine Bestellung Unterschrift.
5. Der Benutzer löscht ihre Handgelenk und trennt, mit der app.

Ab dem Zeitpunkt der Benutzer die Schaltfläche "kaufen" tippt, bis die Transaktion abgeschlossen ist, müssen sie ihre Handgelenk ausgelöst, die ein Indikator laden ansehen. Um dieses Problem zu lösen, schlägt Apple an, dass der Entwickler instant Feedback an den Benutzer anstelle eines Indikators laden dargestellt werden sollen. 

Mithilfe von vorgeschlagenen Apple-Modell, sehen Sie sich die gleichen schnelle Interaktion erneut:

[![](quick-interaction-techniques-images/quick08.png "Vorgeschlagene Modelldiagramm Apples")](quick-interaction-techniques-images/quick08.png#lightbox)

1. Der Benutzer wählt ein Element auf der Apple Watch erwerben.
2. Sie tippen Sie auf die Schaltfläche "kaufen".
3. Die app startet die Netzwerktransaktion und zeigt eine Meldung, die besagt, dass die Bestellung erfolgreich gestartet wurde.
4. Der Benutzer löscht ihre Handgelenk und trennt, mit der app.
5. Erfolgreich einem späteren Zeitpunkt nach Abschluss die Transaktion, zeigt die app eine lokale Benachrichtigung an die Benutzer über eine erfolgreiche Bestellung zu informieren.

Dieses Mal werden, sobald der Benutzer die Schaltfläche "kaufen" tippt, die sie die folgende Meldung, dass der Kauf gestartet wurde, damit sie zuverlässig ihre Handgelenk löschen und die schnelle Interaktion zu diesem Zeitpunkt zu beenden können, angezeigt werden. Später werden sie über den Erfolg oder Misserfolg der Transaktion in einer Benutzer-Benachrichtigung informiert. Auf diese Weise wird der Benutzer nur in "aktiven" Phasen des Prozesses mit der app interagieren.

Für apps, die Netzwerken arbeiten, können sie einen Hintergrund `NSURLSession` zum Verarbeiten der Netzwerkkommunikation mit einem Download-Task. Dies ermöglicht die app, im Hintergrund, um die heruntergeladenen Informationen verarbeitet reaktiviert werden. Verwenden Sie für die app, die Verarbeitung im Hintergrund erfordern eine Assertion mit einer Aufgabe im Hintergrund behandelt die erforderliche Verarbeitung aus.

## <a name="quick-interaction-design-tips"></a>Schnelle Interaktion Entwurfstipps

Da die gewünschte Länge des eine kurze Zusammenfassung zwei Sekunden ist oder weniger beträgt, sollte der Entwickler für den Entwurf von Interaktionen an die app aus ganz am Anfang des Entwurfsprozesses konzentrieren. Auffinden von Bereichen, in denen diese Aktivitäten (mit den oben aufgeführten Verfahren) vereinfacht werden und die neuen Funktionen von WatchOS 3 verwenden, um die app schnell und reaktionsschnelle stellen.

Apple empfiehlt Folgendes:

- Konzentrieren Sie sich schnell Interaktionen durch das Kombinieren von Weiterleiten der am häufigsten verwendeten Features der app an.
- Verwenden Sie Komplikationen und Benutzerbenachrichtigungen Überwachung von gemeinsamen Features und Funktionen.
- Erstellen Sie umfassende-anzeigbare Benutzeroberfläche mit SceneKit und SpriteKit.
- Wann immer möglich, vereinfachen Sie die Navigation innerhalb der app.
- Nehmen Sie niemals des Benutzers warten, ermöglicht es ihnen, ihre Handgelenk löschen und so bald wie möglich mit der app zu trennen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die schnelle Interaktion Techniken behandelt Apple verfügt über zusätzliche in WatchOS 3 und deren in Xamarin.iOS für Apple Watch-Implementierung.

## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
