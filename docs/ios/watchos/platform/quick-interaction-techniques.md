---
title: Schnelle Interaktionstechniken für WatchOS 3 in Xamarin
description: Dieser Artikel behandelt die schnelle interaktionstechniken Apple wurde hinzugefügt, in WatchOS 3 und deren Implementierung in Xamarin.iOS für Apple Watch.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 5086724b565fb95274c4988ca1b6e4bb11064575
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082267"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Schnelle Interaktionstechniken für WatchOS 3 in Xamarin

_Dieser Artikel behandelt die schnelle interaktionstechniken Apple wurde hinzugefügt, in WatchOS 3 und deren Implementierung in Xamarin.iOS für Apple Watch._

Bereitstellung schnell Benutzerinteraktionen sind Bedeutung für die Erstellung, tolle apps für Apple Watch und Schwierigkeiten. Neue watchos 3, Apple wurde Unterstützung für Gesten Erkennungen, Zugriff auf die digitale Crown und neue Benutzerbenachrichtigung und Navigation-Methoden hinzugefügt. Auf diese, zusammen mit Unterstützung für SceneKit und SpriteKit, dem können Entwickler auf einfache Weise umfassende, glanceable Schnittstellen zu erstellen, die schnelle und reaktionsfähige sind.

## <a name="what-are-quick-interactions"></a>Was sind schnelle Interaktionen

Für einen Entwickler, die zum Erstellen von Anwendungen für IOS- oder MacOS (wobei die Zeitspanne, die ein Benutzer benötigt, mit der app interagieren wird in Minuten oder Stunden gemessen) verwendet wird, kann schwierig sein, entwerfen eine erfolgreiche app für die Apple Watch und erfordert eine andere Ansatz.

In WatchOS möchte der Benutzer in der Regel ihre Finger ausgelöst, schnell interagieren mit einer app (in der Regel für kurze wenige Sekunden), und klicken Sie dann löschen Sie ihren Finger und fortfahren, was auch immer sie, dass die gerade ausgeführt wurde.

Es folgen einige Beispiele für typische schnelle Interaktionen auf der Apple Watch:

- Starten eines Zeitgebers.
- Überprüfen das Wetter an.
- Markieren ein Element aus einer Todo-Liste.

Um diese Ziele zu erreichen, muss eine app auf der Apple Watch:

- **Glanceable** – d. h., die mit einer schnellen den Benutzer Blick sollte in der Lage, die Informationen zu erhalten, die sie benötigen. 
- **Aktionen erfordernde** – was bedeutet, dass Benutzer sollten in der Lage, schnelle und fundierte Entscheidungen zu treffen.
- **Reaktionsfähige** – was bedeutet, dass den Benutzer sollte nie warten, um die Informationen zu erhalten, die sie benötigen oder um die Aktion zu erreichen, sie erhalten möchten.

### <a name="quick-interactions-length"></a>Schnelle Interaktionen Länge

Aufgrund der glanceable Apple Watch-apps, Apple empfiehlt, dass die ideale Länge für eine kurze Interaktion zwei Sekunden liegen oder weniger. Als Ergebnis dieser beiden Limit für zweite muss der Entwickler verbringen viel Zeit, die sowohl entwerfen und Implementieren einer Apple Watch-app. 

## <a name="new-watchos-3-features-and-apis"></a>Neue WatchOS 3-Funktionen und APIs

Apple hat einige neue Features und APIs WatchKit bei der Entwickler beim schnellen Interaktionen auf ihrer Apple Watch-apps hinzufügen hinzugefügt:

- WatchOS 3 ermöglicht den Zugriff auf neue Arten von Benutzereingaben wie z.B.:
    - Geste Erkennungen
    - Digitale Crown Drehung 
- WatchOS 3 bietet neue Möglichkeiten zum Anzeigen und aktualisieren die Informationen, wie z. B.:
    - Verbesserte Navigation für Tabelle
    - Neue Benutzerbenachrichtigung Framework-Unterstützung
    - Ein SpriteKit und SceneKit-integration

Durch die Implementierung dieser neuen Features, kann der Entwickler sicherstellen, dass ihre app WatchOS 3 Glanceable, Actionable und dynamisch.

### <a name="gesture-recognizer-support"></a>Unterstützung für die Erkennung von Gesten

Wenn der Entwickler Geste Erkennungen in iOS implementiert hat, sollte sie mit der Funktionsweise von Bewegung Erkennungen in WatchOS 3 vertraut. Um zu aktualisieren, sind Geste Erkennungen Objekte, die Low-Level-Touch-Ereignissen in erkennbar, vordefinierte Aktionen zu analysieren.

WatchOS 3 wird die vier folgenden Geste Freihanderkennung unterstützen:

- Diskrete Gesten-Typen:
    - Mit der Streifbewegung (`WKSwipeGestureRecognizer`).
    - Die Tippbewegung (`WKTapGestureRecognizer`).
- Fortlaufende Geste-Typen:
    - Die Pan-Geste (`WKPanGestureRecognizer`).
    - Drücken Sie Long-Wert-Bewegung (`WKLongPressGestureRecognizer`).

Um eine der neuen Geste Erkennungsprogramme zu implementieren, ziehen Sie es auf einer Entwurfsoberfläche in der iOS-Designer in Visual Studio für Mac und konfigurieren Sie seine Eigenschaften.

Reagieren Sie im Code auf die Aktion der Erkennung, behandeln die Aktion, die vom Benutzer ausgelöst wird. In diesem Fall wird dies auf die gleiche Weise durchgeführt, wie sie in iOS behandelt werden sollen.

#### <a name="discrete-gesture-states"></a>Diskreten Status der Bewegung

Für diskrete Bewegungen, die Aktion aufgerufen werden, wenn die Geste erkannt wird, sowie einen (`WKGestureRecognizerState`) als zugewiesen ist:

[![](quick-interaction-techniques-images/quick01.png "Diskreten Status der Bewegung")](quick-interaction-techniques-images/quick01.png#lightbox)

Alle diskreten Bewegungen beginnt die `Possible` Status- und Übergangsinformationen, die sich entweder die `Failed` oder `Recognized` Zustand. Wenn Sie diskrete Gesten verwenden zu können, verarbeiten nicht vom Entwickler in der Regel direkt mit dem Status. Stattdessen greifen sie auf die Aktion, die aufgerufen wird, wenn die Aktion nur erkannt wird.

#### <a name="continuous-gesture-states"></a>Fortlaufende Geste-Status

Fortlaufende Gesten unterscheiden sich leicht von diskreten Bewegungen, in dem die Aktion mehrmals aufgerufen wird, wie die Geste erkannt wird:

[![](quick-interaction-techniques-images/quick02.png "Fortlaufende Geste-Status")](quick-interaction-techniques-images/quick02.png#lightbox)

In diesem Fall fortlaufende Gesten startet mit dem `Possible` Status, aber sie Fortschritt über mehrere Updates. Hier muss der Entwickler sollten die Erkennung des Status, und aktualisieren die Benutzeroberfläche der app während der `Changed` phase, bis die Bewegung schließlich ist `Recognized` oder `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Tipps zur Verwendung von Gesten-Erkennung

Apple wird Folgendes empfohlen, bei der Verwendung von Gesten Erkennungen in WatchOS 3:

- Fügen Sie die Bewegung Freihanderkennung so gruppieren Sie Elemente anstelle von individuellen Steuerelementen hinzu. Da der Apple Watch-eine kleinere Größe des physischen Bildschirms aufweist, Gruppieren von Elementen sind meist größer und einfacher Ziele für den Benutzer erreicht wird. Darüber hinaus kann die Bewegung Freihanderkennung Konflikt mit integrierten Gesten bereits in der nativen UI-Steuerelemente.
- Legen Sie abhängigkeitsbeziehungen, die in der Watch-app-Storyboard an.
- Einige Geste haben Vorrang vor anderen Dateitypen Bewegung, z. B.:
    - Scrollen
    - Force Touch
 
### <a name="digital-crown-rotation"></a>Digitale Crown Drehung

Durch die Implementierung von digitaler Crown-Unterstützung in ihren apps für WatchOS 3, kann ein Entwickler verbesserte Navigation Geschwindigkeit und Genauigkeit Interaktionen für ihre Benutzer bereitstellen.

Da WatchOS 2, Apple Watch-app können die `WKInterfacePicker` Objekt, das die digitale Crown zugreifen, indem Sie die Angabe einer Liste von `WKPickerItems` und einen Picker-Stil (Liste, gestapeltes oder Image-Sequenz). WatchOS zulässig, klicken Sie dann den Benutzer, der digitale Crown zu verwenden, um ein Element aus der Liste auswählen.

Bei Verwendung einer `WKInterfacePicker`, WatchKit behandelt die meisten Aufgaben durch:

- Zeichnen die Liste und die Elemente der individuellen Benutzeroberfläche.
- Digitale Crown Ereignisse verarbeitet.
- Aufrufen einer Aktion an, wenn ein Element ausgewählt ist.

Neue watchos 3, verfügt der Entwickler jetzt über direkten Zugriff auf die digitale Crown drehungsereignisse, so dass sie ihre eigenen Elemente der Benutzeroberfläche zu erstellen, die auf die Rotation zu reagieren.

Digitaler Crown Zugriff wird durch die folgenden Elemente bereitgestellt:

- `WKCrownSequencer` -Bietet Zugriff auf Drehungen pro Sekunde.
- `WKCrownDelegate` -Bietet Zugriff auf rotierenden deltaereignisse.

#### <a name="rotations-per-second"></a>Drehungen pro Sekunde

Den Zugriff auf die Drehungen pro Sekunde, aus der digitalen Crown ist nützlich, beim Arbeiten mit der Physik Animationen basiert. Verwenden Sie die Drehungen pro Sekunde für den Zugriff auf die `CrownSequencer` Eigenschaft der `WKInterfaceController` der Watch-Erweiterung. Zum Beispiel:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Rotierenden Deltas

Verwenden Sie die rotierenden Deltas aus der digitalen Crown, um die Anzahl der Drehungen ermittelt. Verwenden der `CrownDidRotate` Überschreiben der Methode, die `WKCrownDelegate` die rotierenden Deltas auf. Zum Beispiel:

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

Hier verwendet die app eine Akkumulator (`AccumulatedRotations`) um die Anzahl der Drehungen zu bestimmen. Eine vollständige Drehung von der digitalen Crown entspricht eine kumulierte Delta `1.0` und eine Drehung um die Hälfte wäre `0.5`.

Apple hat überlassen dem Entwickler, um zu bestimmen, wie die Anzahl der Drehung der Vertraulichkeit der Änderungen auf das Element der Benutzeroberfläche aktualisiert entsprechen.

Das Vorzeichen (`+/-`) des rotierenden Deltas gibt die Richtung an, dass der Benutzer die digitale Crown aktiviert ist:

[![](quick-interaction-techniques-images/quick03.png "Die Vorzeichen des Deltas rotierenden gibt an, der die Richtung an, dass der Benutzer die digitale Crown aktiviert ist")](quick-interaction-techniques-images/quick03.png#lightbox)


Wenn der Benutzer nach oben Scrollen wird zurück positive Deltas und nach unten scrollen, klicken Sie dann negative Deltas zurückgegeben werden unabhängig davon, welcher Ausrichtung der Benutzer die Überwachung in trägt, WatchKit.

#### <a name="digital-crown-focus"></a>Digitale Crown Fokus

Genau wie alle anderen Elemente der Benutzeroberfläche hat die digitale Crown das Konzept der Fokus. Dieser Fokus kann von der digitalen Crown verschoben werden, für andere Elemente der Benutzeroberfläche basierend auf wie der Benutzer mit der Apple Watch interagiert. 

Beispielsweise könnte eines der folgenden Steuerelemente den Fokus von der digitalen Crown stehlen:

- Auswahl
- Slider
- Durchführen eines Bildlaufs Controller

Es ist Aufgabe des Entwicklers, um zu bestimmen, wenn ihre benutzerdefinierte Schnittstelle-Element den Fokus von der digitalen Crown werden muss. Apple empfiehlt mit, dass die neue Geste Freihanderkennung erhält den Fokus in das benutzerdefinierte UI-Element.

### <a name="vertical-paging"></a>Vertikale Paging

Das Standardverfahren, dass ein Benutzer eine Tabellenansicht in einer WatchOS-app navigiert wird, scrollen Sie zu der gewünschten Datenelement, tippen auf eine bestimmte Zeile in der Detailansicht anzuzeigen, tippen Sie auf die Schaltfläche "zurück" nach Abschluss die Details anzeigen, und wiederholen den Vorgang für alle anderen Informationen, die die y interessiert sind, aus der in der Tabelle:

[![](quick-interaction-techniques-images/quick04.png "Verschieben zwischen einer Tabelle und der Detailansicht")](quick-interaction-techniques-images/quick04.png#lightbox)

Neue watchos 3, Entwickler können vertikale Paging in ihre Steuerelemente Tabellenansicht. Dieses Feature aktiviert ist kann der Benutzer scrollen, um eine Tabellenansicht Zeile finden, und tippen Sie auf die Zeile, um die Details wie vor dem anzeigen. Allerdings können sie jetzt nach oben navigieren Sie zu wählen Sie die nächste Zeile, in der Tabelle, oder wählen Sie die vorherige Zeile (oder verwenden Sie die digitale Crown) alle ohne an die Ansicht der Tabelle zurückgeben zuerst:

[![](quick-interaction-techniques-images/quick05.png "Verschieben zwischen einer Tabelle und der Detailansicht und Wischen nach oben und unten zwischen den anderen Zeilen zu verschieben")](quick-interaction-techniques-images/quick05.png#lightbox)

Um diesen Modus aktivieren, der WatchOS-app-Storyboard für die Bearbeitung in Xcode geöffnet, wählen Sie die Ansicht für die Tabelle aus, und Überprüfen der **vertikale Detail Paging** Kontrollkästchen:

[![](quick-interaction-techniques-images/quick06.png "Aktivieren Sie das Kontrollkästchen mit vertikalen Detail Paging")](quick-interaction-techniques-images/quick06.png#lightbox)

Stellen Sie sicher, dass die Tabelle Segues verwendet wird, in der Detailansicht anzeigen und speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac, um zu synchronisieren.

Der Entwickler erreichen programmgesteuert vertikale Paging in eine bestimmte Zeile mit dem folgenden Code in einer Tabellenansicht:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Beim vertikalen Paging verwenden zu können, muss der Entwickler bewusst sein, dass WatchKit automatisch das Vorabladen von Controllern übernimmt und daher einige Lebenszyklusmethoden Controller aufgerufen werden können, bevor die Benutzeroberfläche tatsächlich angezeigt wird.

### <a name="notification-enhancements"></a>Verbesserungen der Benachrichtigung

Benachrichtigung sind die Primärform von kurze Interaktion, die für den Benutzer in der Regel auf WatchOS drückt und wurden seit dem ersten Apple Watch und WatchOS 1 verfügbar.

Eine typische Benachrichtigung Interaktion schnelle lautet wie folgt aus:

1. Der Benutzer ist mit der die Benachrichtigung Übermitteln von Haptischem Wenn eine neue Benachrichtigung empfangen wird.
2. Sie lösen die Finger, um die kurze suchen-Schnittstelle für die Benachrichtigung anzuzeigen.
3. Wenn sie den Vorgang fortsetzen zu ihren Finger ausgelöst, WatchOS erfolgt automatisch ein Übergang in die Oberfläche lange suchen-Benachrichtigung.

Es gibt mehrere Möglichkeiten, die ein Benutzer auf die Benachrichtigung reagieren kann:

- Für einen gut definierten und Benachrichtigung angezeigt wird der Benutzer keine Aktion durchführen und einfach die Benachrichtigung schließen.
- Sie können auch der Benachrichtigung zum Starten der WatchOS-app tippen.
- Für eine Benachrichtigung, die benutzerdefinierte Aktionen unterstützt, kann der Benutzer einen benutzerdefinierten Aktionen auswählen. Dies können entweder sein:
    - **Vordergrund Aktionen** -diese starten Sie die app aus, um die Aktion auszuführen.
    - **Aktionen im Hintergrund** – immer auf dem iPhone in WatchOS 2 weitergeleitet wurden, aber an die WatchApp in WatchOS 3 weitergeleitet werden können.

Neues für WatchOS 3:

* Benachrichtigung verwenden eine ähnliche API auf allen Plattformen (iOS, WatchOS, TvOS und MacOS) an.
* Lokale Benachrichtigung kann auf der Apple Watch-geplant werden.
* Hintergrund-Benachrichtigung wird an der app-Erweiterung weitergeleitet werden, wenn sie auf der Apple Watch-geplant wurden.

#### <a name="notification-scheduling-and-delivery"></a>Benachrichtigung zeitplanung und Übermittlung

Benachrichtigung des Benutzers iPhone werden auf der Apple Watch weiterleiten, den folgenden Fällen:

* Die iPhone-Bildschirm ist deaktiviert.
* Der Apple Watch ist abgenutzt werden und wurde entsperrt.

In WatchOS 3 werden lokale Benachrichtigungen kann auf der Apple Watch-geplant werden und nur auf der Apple Watch. Es obliegt dem Entwickler, planen eine entsprechende iPhone-Benachrichtigung, wenn sie von der app erforderlich ist.

Mit den gleichen Benachrichtigungsbezeichner auf der Apple Watch und iPhone-Versionen der Benachrichtigungen, verhindert es doppelte Benachrichtigungen, die auf der Apple Watch angezeigt wird. Die Apple Watch-Version, der die Benachrichtigung wird über die iPhone-Version Vorrang.

Da WatchOS 3 identisch verwendet `UINotification` -API-Framework als iOS 10, finden Sie in unserem iOS 10 [Benutzer Benachrichtigungsframeworks](~/ios/platform/user-notifications/index.md) Dokumentation.

### <a name="using-spritekit-and-scenekit"></a>Mithilfe von SpriteKit und SceneKit

Neue watchos 3, Entwickler können nun sowohl SpritKit und SceneKit-Objekte in ihr app Benutzeroberfläche Entwurf um sowohl 2D-und 3D-Grafiken zu präsentieren.

Zur Unterstützung dieser Funktion wurden zwei neue Schnittstellenklassen hinzugefügt:

- `WKInterfaceSKScene` – Für die Arbeit mit 2D-Grafiken SpriteKit.
- `WKInterfaceSCNScene` – Bei arbeitet mit 3D-Grafiken SceneKit.

Um diese Objekte zu verwenden, ziehen Sie sie in der Entwurfsoberfläche in der Watch-app-Storyboard in Interface Builder von Xcode, und Verwenden der **Attributes Inspector** konfigurieren.

Ab diesem Zeitpunkt funktioniert arbeiten mit der SpriteKit oder SceneKit-Szenen genauso wie in einer iOS-app. Die Watch-app bietet eine `WKInterfaceSKScene` durch Aufrufen einer der der `Present` Methoden. Für SceneKit, legen Sie einfach die `Scene` Eigenschaft der `WKInterfaceSCNScene` Objekt.

## <a name="actionable-complications"></a>Aktionen erfordernde Komplikationen

WatchOS 2 hat Apple Komplikationen für 3rd Party-apps eingeführt. In WatchOS 3 hat Apple die Funktionen erweitert, die ein Entwickler in eine WatchKit-Komplikation einschließen können. 

Darüber hinaus mehrere der integrierten in Watch Gesichter können jetzt Komplikationen und vorhandenen sehen Sie sich die Seite mit bereits unterstützten Komplikationen können sogar noch mehr Komplikationen nun eingefügt.

Neue ist auch die Möglichkeit für einen Benutzer zu schnell Wischen nach links oder rechts, um all die watchfaces übergehen, die sie auf ihrer Apple Watch installiert haben. Der neuen Katalog in der Apple Watch-Companion iPhone-app verwenden, kann der Benutzer hinzufügen und anpassen neue watchfaces und eine der Schwierigkeiten, die sie einschließen können.

Aufgrund dieser neuen Features empfiehlt Apple, dass jede auf der Apple Watch-app auch mindestens eine Komplikation einschließen muss und sind daher alle der nativen Apple Watch-app jetzt Komplikationen.

Schwierigkeiten bieten die folgenden Funktionen zu einer app:

- Sie sind hoch glanceable, da sie immer auf dem Zifferblatt Ihrer Apple Watch vorhanden sind.
- Komplikationen werden häufig von WatchOS aktualisiert. Jede app, die eine Komplikation auf aktuell angezeigten watchface des Benutzers enthält, wird mindestens zweimal pro Stunde aktualisiert.
- Jede app mit einer Komplikation auf des Benutzers aktuell angezeigten watchface bleibt im Arbeitsspeicher macht die app schnell starten und verbessert die Geschwindigkeit von Antworten aus der app.
- Komplikationen erleichtern es dem Benutzer, die bestimmte Funktionen in einer WatchOS-app zu starten.

## <a name="glanceable-notification"></a>Glanceable Benachrichtigung

Benachrichtigung auf der Apple Watch bieten eine hervorragende, anpassbare Möglichkeit, schnell die Benutzer über Ereignisse oder neue Informationen, wie eingehende Nachrichten oder das Erreichen eines Ziels in einer Trainings-app informiert.

Verwenden Sie eine Benachrichtigung, kann schnell wertvoller Informationen für den Benutzer angezeigt werden. In vielen Situationen kann eine gut entworfene Benachrichtigung die Notwendigkeit der Benutzer die app tatsächlich zu starten, entfernen.

Neu in WatchOS 3, unterstützen jetzt das alle Benachrichtigungen:

- SpriteKit
- SceneKit
- Inline-Video

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Verbesserte Benutzeroberfläche mit SpriteKit und SceneKit

Ein Entwickler kann in der Regel Spiele UI vorstellen, wenn SpriteKit und SceneKit erwähnt werden. Allerdings kann sowohl für SpriteKit SceneKit nützlich sein für das Erstellen von Benutzeroberflächen, die benutzerdefinierte Layouts, Inhalten enthalten, nicht spielebezogene und Animationen, die andernfalls nicht in WatchKit allein möglich sind.

Beispielsweise kann eine Benachrichtigung für Benutzer von einer app zum Teilen von Fotos SpriteKit verwenden, einschließlich des Benutzers, der das Bild sowie ein tatsächliches Bild und andere benutzerdefinierte Informationen, die die benutzerfreundlichkeit ergänzt gebucht bereitstellen eine umfassenden Benutzeroberfläche.

Darüber hinaus können sowohl für SpriteKit SceneKit mit standard WatchKit-UI-Elemente in der app-Benutzeroberfläche Entwurf kombiniert werden.

## <a name="simple-navigation"></a>Einfache Navigation

WatchOS 3 bietet mehrere Möglichkeiten, ein Entwickler die Navigation innerhalb ihrer WatchOS-apps, beispielsweise auf den neuen vereinfachen kann wie [vertikale Paging](#vertical-paging), [Erkennung Stiftbewegungserkennung](#gesture-recognizer-support) und [digitale Krone Drehung](#digital-crown-rotation) Features, die oben aufgeführten.

Die digitale Crown gilt nur für die Apple Watch und kann in viele verschiedene Möglichkeiten zur Vereinfachung der Navigation verwendet werden. Z. B. können eine Timer-Anwendung die digitale Crown um über die verfügbaren Timer Längen zu bereinigen.

Benutzerdefinierte Stiftbewegungen können die neue und einzigartigen Möglichkeiten für den Benutzer für die Interaktion mit der eine Watch-app vorhanden und können auch verwendet werden, um die app-Navigation zu vereinfachen.

Apple empfiehlt das Suchen nach Möglichkeiten, eine Kombination aus der neuen schnellen Interaktion Features hinzugefügt, die in WatchOS 3 aus, um umfassende, verwenden Sie Schnittstellen für WatchOS-app schnell und problemlos zu präsentieren.

## <a name="finishing-the-quick-interaction"></a>Die schnelle Aktivität abgeschlossen

Eine wohlgeformte kurze Interaktion Erfahrung gibt dem Benutzer die Zuversicht, löschen ihren Finger (und deaktivieren mit der app) Wenn sie die aktuelle Aktivität abgeschlossen haben.

Wo dies insbesondere wird ist ein Problem auf, wenn die Watch-app wird jegliche Art von Netzwerkverbindung oder Freigeben von Informationen für eine Begleit-iPhone-app. Dies kann häufig zu einer Anzeige wartender führen, während die Transaktion eingerichtet ist, ausgeführt wird, die während der eine kurze Interaktion nicht wünschenswert ist. Betrachten Sie das folgende Beispiel:

[![](quick-interaction-techniques-images/quick07.png "Diagramm der Watch-app eine Netzwerkverbindung ausführen und Freigeben von Informationen für eine Begleit-iPhone-app")](quick-interaction-techniques-images/quick07.png#lightbox)

1. Der Benutzer wählt ein Element auf der Apple Watch erwerben.
2. Sie tippen Sie auf die Schaltfläche "kaufen" aus.
3. Die app wird die Netzwerktransaktion gestartet und zeigt einen Indikator laden.
4. Einige Zeit später die Transaktion abgeschlossen ist und die app zeigt eine Bestätigung der Bestellung.
5. Der Benutzer löscht ihre Handgelenk und trennt den mit der app.

Ab dem Zeitpunkt, an die Benutzer die Schaltfläche "kaufen" antippt, bis die Transaktion abgeschlossen ist, müssen sie ihren Finger ausgelöst betrachten einen Indikator laden. Um diese Situation zu lösen, empfiehlt Apple, dass der Entwickler sofortiges Feedback an den Benutzer, anstatt einen Indikator laden dargestellt werden sollen. 

Verwenden die vorgeschlagenen Apple-Modell, sehen Sie sich erneut die gleichen kurze Interaktion:

[![](quick-interaction-techniques-images/quick08.png "Vorgeschlagene Modelldiagramm Äpfel")](quick-interaction-techniques-images/quick08.png#lightbox)

1. Der Benutzer wählt ein Element auf der Apple Watch erwerben.
2. Sie tippen Sie auf die Schaltfläche "kaufen" aus.
3. Die app wird die Netzwerktransaktion gestartet und zeigt eine Meldung, die besagt, dass die Bestellung erfolgreich gestartet wurde.
4. Der Benutzer löscht ihre Handgelenk und trennt den mit der app.
5. Wenn die Transaktion erfolgreich einem späteren Zeitpunkt abgeschlossen ist, zeigt die app eine lokale Benachrichtigung der Benutzer über einen erfolgreichen Kauf informiert.

Dieses Mal werden, sobald der Benutzer auf die Schaltfläche "kaufen" tippt, die sie eine Nachricht an, die besagt, dass der Kauf gestartet wurde, damit sie zuverlässig an diesem Punkt die kurze Interaktion zu beenden und löschen ihren Finger, angezeigt werden. Später werden sie über den Erfolg oder Fehlschlag der Transaktion in eine Benutzerbenachrichtigung informiert. Auf diese Weise wird der Benutzer nur während der "aktiven" Phasen des Prozesses mit der app interagiert.

Für apps, die Netzwerke arbeiten, können sie einen Hintergrund `NSURLSession` zum Verarbeiten der Netzwerkkommunikation mit einem Download-Task. Dadurch wird die app im Hintergrund, um die heruntergeladene Informationen verarbeitet reaktiviert werden. Verwenden Sie für die app, die hintergrundverarbeitung erforderlich, eine Assertion mit einer Aufgabe Hintergrund, um die erforderliche Verarbeitung zu verarbeiten.

## <a name="quick-interaction-design-tips"></a>Kurze Zusammenfassung Entwurfstipps

Da die gewünschte Länge des eine kurze Interaktion zwei Sekunden lang ist oder weniger beträgt, sollte der Entwickler auf das Design von der app-Interaktionen vom Beginn des Entwurfsprozesses konzentrieren. Suchen Sie die Bereiche, in denen diese Interaktionen (mithilfe der weiter oben vorgestellten Technik) vereinfacht werden, und verwenden die neuen Features von WatchOS 3, um die app schnell und reaktionsfähig zu machen.

Apple empfiehlt Folgendes:

- Konzentrieren Sie sich schnell Interaktionen durch Verschieben der am häufigsten verwendeten Features der app an.
- Mithilfe von Komplikationen und Benutzerbenachrichtigungen allgemeine Features und Funktionen anzeigen.
- Erstellen Sie umfassende, glanceable Benutzeroberfläche, mit SceneKit und SpriteKit.
- Wann immer möglich, vereinfachen Sie die Navigation in der app.
- Stellen Sie niemals des Benutzers warten, können sie ihren Finger löschen und so bald wie möglich mit der app zu deaktivieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die schnelle interaktionstechniken behandelt Apple wurde hinzugefügt, in WatchOS 3 und deren Implementierung in Xamarin.iOS für Apple Watch.

## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
