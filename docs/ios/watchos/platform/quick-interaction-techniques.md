---
title: Schnell Interaktions Techniken für watchos 3 in xamarin
description: In diesem Artikel werden die schnell Interaktions Techniken behandelt, die Apple in watchos 3 hinzugefügt hat, und wie diese in xamarin. IOS für Apple Watch implementiert werden.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e57b6df0f0137d5a8a8f2c0ba68793008986ba18
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932183"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Schnell Interaktions Techniken für watchos 3 in xamarin

_In diesem Artikel werden die schnell Interaktions Techniken behandelt, die Apple in watchos 3 hinzugefügt hat, und wie diese in xamarin. IOS für Apple Watch implementiert werden._

Das Bereitstellen von schnellen Benutzerinteraktionen ist wichtig für das Erstellen von überzeugenden Apple Watch apps und Komplikationen. Ab watchos 3 hat Apple Unterstützung für Gesten Erkennungs Tools, den Zugriff auf die Digital Crown und neue Benutzerbenachrichtigungs-und Navigationsverfahren hinzugefügt. Neben der zusätzlichen Unterstützung von scenekit und spritekit ermöglicht es dem Entwickler, problemlos umfangreiche, schnell zu erstellen, die schnell und reaktionsfähig sind.

## <a name="what-are-quick-interactions"></a>Was sind schnelle Interaktionen?

Für einen Entwickler, der zum Erstellen von Anwendungen für IOS oder macOS verwendet wird (wobei die Zeitspanne, in der ein Benutzer mit der APP interagiert, in Minuten oder Stunden gemessen wird), kann das Entwerfen einer erfolgreichen App für den Apple Watch eine Herausforderung darstellen und einen anderen Ansatz erfordern.

In watchos möchte der Benutzer in der Regel sein Handgelenk erhöhen, schnell mit einer APP interagieren (in der Regel ein paar Sekunden), das Handumdrehen ablegen und den Vorgang fortsetzen.

Im folgenden sind einige Beispiele für typische schnelle Interaktionen der Apple Watch aufgeführt:

- Starten eines Timers.
- Überprüfen des Wetters.
- Markieren eines Elements aus einer TODO-Liste.

Um diese Ziele zu erreichen, muss eine APP auf dem Apple Watch wie folgt lauten:

- **Aushebbar** : Dies bedeutet, dass der Benutzer mit einem kurzen Blick die benötigten Informationen erhalten sollte. 
- **Handlungsfähig** : Benutzer sollten in der Lage sein, schnelle, gut informierte Entscheidungen zu treffen.
- **Reaktions** fähig: Dies bedeutet, dass der Benutzer niemals darauf warten sollte, die benötigten Informationen zu erhalten oder die gewünschte Aktion zu erzielen.

### <a name="quick-interactions-length"></a>Länge der schnellen Interaktionen

Aufgrund der Blick Apple Watch-apps hat Apple vorgeschlagen, dass die ideale Länge einer schnellen Interaktion zwei Sekunden oder weniger betragen sollte. Im Ergebnis dieses zwei Sekunden Limits benötigt der Entwickler einen beträchtlichen Zeitaufwand für das Entwerfen und Implementieren einer Apple Watch-app. 

## <a name="new-watchos-3-features-and-apis"></a>Neue watchos 3-Features und APIs

Apple hat watchkit mehrere neue Features und APIs hinzugefügt, um den Entwickler beim Hinzufügen schneller Interaktionen mit Ihren Apple Watch-apps zu unterstützen:

- watchos 3 bietet Zugriff auf neue Arten von Benutzereingaben, wie z. b.:
  - Gesten Erkennungs Tools
  - Digital Crown Drehung 
- watchos 3 bietet neue Möglichkeiten zum Anzeigen und Aktualisieren von Informationen, wie z. b.:
  - Erweiterte Tabellennavigation
  - Unterstützung für neues Benutzer Benachrichtigungs Framework
  - Spritekit-und scenekit-Integration

Wenn Sie diese neuen Features implementieren, kann der Entwickler sicherstellen, dass Ihre watchos 3-APP abrechenbar und reaktionsfähig ist.

### <a name="gesture-recognizer-support"></a>Gesten Erkennungs Unterstützung

Wenn der Entwickler Gesten Erkennungs Tools in ios implementiert hat, sollten Sie sich mit der Funktionsweise von Gesten erkenzern in watchos 3 vertraut machen. Zum Aktualisieren sind Gesten Erkennungs Objekte Objekte, die Low-Level-touchereignisse in erkennbare, vordefinierte Gesten analysieren.

watchos 3 unterstützt die vier folgenden Gesten Erkennungs Tools:

- Diskrete Gesten Typen:
  - Die Schwenkbewegung ( `WKSwipeGestureRecognizer` ).
  - Die Tap-Geste ( `WKTapGestureRecognizer` ).
- Fortlaufende Gesten Typen:
  - Die schwenken-Bewegung ( `WKPanGestureRecognizer` ).
  - Die lange Press Bewegung ( `WKLongPressGestureRecognizer` ).

Wenn Sie eine der neuen Gesten Erkennungs Tools implementieren möchten, ziehen Sie Sie einfach auf eine Entwurfs Oberfläche im IOS-Designer in Visual Studio für Mac und konfigurieren Sie die zugehörigen Eigenschaften.

Reagieren Sie im Code auf die Aktion der Erkennung, um die vom Benutzer ausgelöste Bewegung zu verarbeiten. Dies erfolgt auch auf die gleiche Weise wie in ios.

#### <a name="discrete-gesture-states"></a>Diskrete Gesten Zustände

Bei diskreten Gesten wird die Aktion aufgerufen, wenn die Geste erkannt wird und ein Zustand ( `WKGestureRecognizerState` ) wie folgt zugewiesen wird:

[![Diskrete Gesten Zustände](quick-interaction-techniques-images/quick01.png)](quick-interaction-techniques-images/quick01.png#lightbox)

Alle diskreten Gesten beginnen im `Possible` -Zustand und wechseln entweder in den Zustand `Failed` oder `Recognized` . Bei der Verwendung diskreter Gesten behandelt der Entwickler im Allgemeinen nicht direkt den Zustand. Stattdessen verlassen Sie sich auf die Aktion, die aufgerufen wird, wenn die Geste nur erkannt wird.

#### <a name="continuous-gesture-states"></a>Fortlaufende Gesten Zustände

Fortlaufende Gesten unterscheiden sich geringfügig von diskreten Gesten, bei denen die Aktion mehrmals aufgerufen wird, während die Geste erkannt wird:

[![Fortlaufende Gesten Zustände](quick-interaction-techniques-images/quick02.png)](quick-interaction-techniques-images/quick02.png#lightbox)

Auch hier beginnen fortlaufende Gesten im- `Possible` Zustand, Sie werden jedoch über mehrere Updates ausgeführt. Hier muss der Entwickler den Zustand der Erkennung in Erwägung gezogen und die Benutzeroberfläche der APP während der `Changed` Phase aktualisieren, bis die Geste schließlich `Recognized` oder ist `Canceled` .

#### <a name="gesture-recognizer-usage-tips"></a>Verwendungs Tipps für Gestenerkennung

Apple empfiehlt Folgendes bei der Arbeit mit Gesten Erkennungs Programmen in watchos 3:

- Fügen Sie die Gesten erkenungen zum Gruppieren von Elementen anstelle einzelner Steuerelemente hinzu. Da die Apple Watch eine geringere physische Bildschirmgröße aufweist, sind Gruppenelemente tendenziell größere und einfachere Ziele, die der Benutzer erreichen kann. Außerdem können die Gesten Erkennungs Tools mit integrierten Gesten in Konflikt stehen, die bereits in den nativen UI-Steuerelementen vorhanden sind.
- Legen Sie Abhängigkeitsbeziehungen im Storyboard der Watch-App fest.
- Einige Gesten haben Vorrang vor anderen Gesten Typen, z. b.:
  - Scrollen
  - Force Touch

### <a name="digital-crown-rotation"></a>Digital Crown Drehung

Wenn Sie Digital Crown Unterstützung in ihren watchos 3-apps implementieren, kann ein Entwickler eine bessere Navigations Geschwindigkeit und Genauigkeits Interaktionen für Ihre Benutzer bereitstellen.

Seit watchos 2 könnte Apple Watch-APP das-Objekt verwenden, `WKInterfacePicker` um auf die Digital Crown zuzugreifen, indem eine Liste von und eine Auswahl Formatvorlage `WKPickerItems` (Listen-, gestapelte oder Bildsequenz) bereitgestellt wird. watchos gestattet dem Benutzer dann die Verwendung der Digital Crown, um ein Element aus der Liste auszuwählen.

Wenn Sie einen verwenden `WKInterfacePicker` , verarbeitet watchkit den größten Teil der Arbeit wie folgt:

- Zeichnen der Liste und der einzelnen Schnittstellen Elemente.
- Die Digital Crown Ereignisse werden verarbeitet.
- Aufrufen einer Aktion, wenn ein Element ausgewählt wird.

Neu bei watchos 3: der Entwickler verfügt jetzt über direkten Zugriff auf die Digital Crown Rotations Ereignisse, die es Ihnen ermöglichen, eigene Benutzeroberflächen Elemente zu erstellen, die auf Rotations Werte reagieren.

Digital Crown Zugriff wird von folgenden Elementen bereitgestellt:

- `WKCrownSequencer`-Ermöglicht den Zugriff auf Drehungen pro Sekunde.
- `WKCrownDelegate`-Ermöglicht den Zugriff auf Rotelle Delta Ereignisse.

#### <a name="rotations-per-second"></a>Rotationen pro Sekunde

Der Zugriff auf die Rotationen pro Sekunde aus dem Digital Crown ist bei der Arbeit mit Physik basierten Animationen nützlich. Um auf die Drehungen pro Sekunde zuzugreifen, verwenden Sie die- `CrownSequencer` Eigenschaft des der `WKInterfaceController` Watch-Erweiterung. Beispiel:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Rotale Delta

Verwenden Sie die Rotations Delta aus der Digital Crown, um die Anzahl der Rotationen zu zählen. Verwenden `CrownDidRotate` Sie die Überschreibungs Methode von `WKCrownDelegate` , um auf die Rotations Delta zuzugreifen. Beispiel:

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

Hier verwaltet die APP einen Akkumulator ( `AccumulatedRotations` ), um die Anzahl der Rotationen zu bestimmen. Eine vollständige Drehung des Digital Crown ist gleich einem akkumulierten Delta von, `1.0` und eine halbe Drehung wäre `0.5` .

Apple hat es dem Entwickler ausgelassen, um zu bestimmen, wie die rotationszähler der Vertraulichkeit von Änderungen auf dem zu aktualisierenden Benutzeroberflächen Element entsprechen.

Das Zeichen ( `+/-` ) des rotierenden Deltas gibt die Richtung an, in der der Benutzer den Digital Crown schaltet:

[![Das Vorzeichen des rotationdeltas gibt die Richtung an, in der der Benutzer den Digital Crown](quick-interaction-techniques-images/quick03.png)](quick-interaction-techniques-images/quick03.png#lightbox)

Wenn der Benutzer einen Bildlauf nach oben durchführt, gibt watchkit positive Delta-Elemente zurück. Wenn Sie den Bildlauf nach unten durchführen, werden negative Delta-Elemente zurückgegeben, unabhängig davon, in welcher Richtung der Benutzer die Überwachung durchführt.

#### <a name="digital-crown-focus"></a>Digital Crown Fokus

Ebenso wie alle anderen Schnittstellen Elemente hat der Digital Crown das Konzept des Fokus. Dieser Fokus kann vom Digital Crown auf andere Schnittstellen Elemente verlagert werden, je nachdem, wie der Benutzer mit der Überwachung interagiert. 

Beispielsweise könnte eines der folgenden Steuerelemente den Fokus des Digital Crown stehlen:

- Auswahl
- Slider
- Scrollcontroller

Es ist für den Entwickler von Bedeutung, zu bestimmen, wann sein benutzerdefiniertes Schnittstellen Element den Schwerpunkt der Digital Crown sein muss. Apple schlägt vor, mithilfe der neuen Gesten Erkennungs Punkte den Fokus auf das benutzerdefinierte UI-Element zu bringen.

### <a name="vertical-paging"></a>Vertikales Paging

Die Standardmethode, mit der ein Benutzer eine Tabellenansicht in einer watchos-App navigiert, besteht darin, einen Bildlauf zu den gewünschten Daten vorzunehmen, auf eine bestimmte Zeile zu tippen, um die ausführliche Ansicht anzuzeigen, und dann auf die Schaltfläche "zurück" zu klicken, wenn Sie die Details anzeigen und den Vorgang für alle anderen Informationen wiederholen, die für Sie von Interesse sind

[![Verschieben zwischen einer Tabelle und der Detail Ansicht](quick-interaction-techniques-images/quick04.png)](quick-interaction-techniques-images/quick04.png#lightbox)

Neu bei watchos 3, der Entwickler kann das vertikale Paging auf den Tabellen Sicht-Steuerelementen aktivieren. Wenn diese Funktion aktiviert ist, kann der Benutzer einen Bildlauf durchführen, um eine Tabellen Ansichts Zeile zu suchen und auf die Zeile zu tippen, um Details wie zuvor anzuzeigen Allerdings können Sie jetzt nach oben navigieren, um die nächste Zeile in der Tabelle oder nach unten zu wählen, um die vorherige Zeile auszuwählen (oder die Digital Crown zu verwenden), ohne dass Sie zuerst zur Tabellenansicht zurückkehren müssen:

[![Verschieben zwischen einer Tabelle und der Detail Ansicht und schwenken nach oben und unten, um zwischen den anderen Zeilen zu wechseln](quick-interaction-techniques-images/quick05.png)](quick-interaction-techniques-images/quick05.png#lightbox)

Öffnen Sie zum Aktivieren dieses Modus das Storyboard der watchos-app in Xcode für die Bearbeitung, wählen Sie die Tabellenansicht aus, und aktivieren Sie das Kontrollkästchen **vertikale Detail Auslagerung** :

[![Aktivieren des Kontrollkästchens "vertikale Detail Auslagerung"](quick-interaction-techniques-images/quick06.png)](quick-interaction-techniques-images/quick06.png#lightbox)

Stellen Sie sicher, dass in der Tabelle mithilfe von-Listen die ausführliche Ansicht angezeigt wird, und speichern Sie die Änderungen im Storyboard, und kehren Sie zur Synchronisierung zurück Visual Studio für Mac.

Der Entwickler kann das vertikale Paging Programm gesteuert mit dem folgenden Code für eine Tabellenansicht in eine bestimmte Zeile einbinden:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Wenn Sie das vertikale Paging verwenden, muss der Entwickler beachten, dass watchkit das vorab Laden von Controllern automatisch verarbeitet. Folglich können einige Controller Lebenszyklus-Methoden aufgerufen werden, bevor die Benutzeroberfläche tatsächlich sichtbar wird.

### <a name="notification-enhancements"></a>Benachrichtigungs Erweiterungen

Bei der Benachrichtigung handelt es sich um die primäre Form der schnellen Interaktion, die ein Benutzer in der Regel in watchos vorhat und seit der ersten Apple Watch und watchos 1 verfügbar ist.

Eine typische Benachrichtigungs-schnell Interaktion lautet wie folgt:

1. Der Benutzer erhält die Benachrichtigung, wenn eine neue Benachrichtigung empfangen wird.
2. Sie erhöhen das Handgelenk, um die kurz Ansicht für die Benachrichtigung anzuzeigen.
3. Wenn Sie das Handgelenk weiterhin erhalten, wechselt watchos automatisch in die Oberfläche für die Long-Look-Benachrichtigung.

Es gibt mehrere Möglichkeiten, wie ein Benutzer auf die Benachrichtigung reagieren kann:

- Für eine klar definierte und vorgestellte Benachrichtigung wird der Benutzer nichts weiter tun und die Benachrichtigung einfach verwerfen.
- Möglicherweise Tippen Sie auch auf die Benachrichtigung, um die watchos-APP zu starten.
- Für eine Benachrichtigung, die benutzerdefinierte Aktionen unterstützt, kann der Benutzer eine der benutzerdefinierten Aktionen auswählen. Folgende Aktionen sind möglich:
  - **Vordergrund Aktionen** : diese starten die APP, um die Aktion auszuführen.
  - **Hintergrund Aktionen** : wurden stets an das iPhone in watchos 2 weitergeleitet, können aber an die watchapp in watchos 3 weitergeleitet werden.

Neu für watchos 3:

- Bei der Benachrichtigung wird eine ähnliche API auf allen Plattformen verwendet (Ios, watchos, tvos und macOS).
- Lokale Benachrichtigungen können auf dem Apple Watch geplant werden.
- Die Hintergrund Benachrichtigung wird an die APP-Erweiterung weitergeleitet, wenn Sie auf dem Apple Watch geplant wurden.

#### <a name="notification-scheduling-and-delivery"></a>Benachrichtigungs Planung und-Übermittlung

Die Benachrichtigung vom iPhone des Benutzers wird an die Apple Watch weiterleiten, wenn Folgendes geschieht:

- Der Bildschirm des iPhones ist deaktiviert.
- Der Apple Watch wird bereits genutzt und wurde entsperrt.

In watchos 3 können lokale Benachrichtigungen auf dem Apple Watch geplant werden und werden nur auf der überwachen übermittelt. Der Entwickler kann eine entsprechende iPhone-Benachrichtigung planen, wenn er von der APP benötigt wird.

Durch einschließen desselben Benachrichtigungs Bezeichners sowohl in der Apple Watch-als auch in der iPhone-Version der Benachrichtigungen wird verhindert, dass doppelte Benachrichtigungen auf der Überwachung angezeigt werden. Die Apple Watch Version der Benachrichtigung hat Vorrang vor der iPhone-Version.

Da watchos 3 dasselbe API- `UINotification` Framework wie IOS 10 verwendet, finden Sie weitere Informationen in unserer Dokumentation zum IOS 10- [Benutzer Benachrichtigungs Framework](~/ios/platform/user-notifications/index.md) .

### <a name="using-spritekit-and-scenekit"></a>Verwenden von spritekit und scenekit

Neu bei watchos 3: der Entwickler kann nun sowohl spritkit-als auch scenekit-Objekte im Benutzeroberflächen Design Ihrer APP verwenden, um 2D-und 3D-Grafiken darzustellen.

Zur Unterstützung dieses Features wurden zwei neue Schnittstellen Klassen hinzugefügt:

- `WKInterfaceSKScene`: Für die Arbeit mit spritekit 2D-Grafiken.
- `WKInterfaceSCNScene`-Für die Arbeit mit scenekit 3D-Grafiken.

Um diese Objekte zu verwenden, ziehen Sie Sie einfach auf die Entwurfs Oberfläche innerhalb des Storyboards der Watch-app in der Interface Builder von Xcode, und verwenden Sie den **Attribute Inspector** , um Sie zu konfigurieren.

An diesem Punkt funktioniert das Arbeiten mit den spritekit-oder scenekit-Szenen genauso wie in einer IOS-app. Die Watch-App stellt eine `WKInterfaceSKScene` durch Aufrufen einer der- `Present` Methoden dar. Legen Sie für scenekit einfach die- `Scene` Eigenschaft des- `WKInterfaceSCNScene` Objekts fest.

## <a name="actionable-complications"></a>Ausführbare Komplikationen

In watchos 2 hat Apple Komplikationen für Drittanbieter-apps eingeführt. In watchos 3 hat Apple die Möglichkeiten erweitert, die ein Entwickler in eine watchkit-Komplikation einbeziehen kann. 

Darüber hinaus können mehr der integrierten Überwachungs Gesichter nun Komplikationen und vorhandene Watch-Gesichter enthalten, die bereits unterstützte Komplikationen enthalten können.

Außerdem ist die Möglichkeit eines Benutzers, schnell nach links oder rechts zu wechseln, um alle Überwachungs Gesichter zu übertragen, die auf dem Apple Watch installiert sind. Mithilfe des neuen Katalogs auf der begleitenden iPhone-App des Apple Watch kann der Benutzer neue Watch-Gesichter und jede der Komplikationen hinzufügen und anpassen, die Sie einschließen können.

Aufgrund dieser neuen Features schlägt Apple vor, dass jede APP auf Apple Watch auch mindestens eine Komplikation enthalten sollte. Daher haben alle apps der systemeigenen Apple Watch jetzt Komplikationen.

Komplikationen stellen die folgenden Features für eine APP bereit:

- Sie sind sehr schnell nutzbar, da Sie immer auf dem Überwachungs Gesicht vorhanden sind.
- Komplikationen werden häufig von watchos aktualisiert. Jede APP, die eine Komplikation für das aktuell angezeigte Überwachungs Gesicht des Benutzers enthält, wird mindestens zweimal pro Stunde aktualisiert.
- Jede APP, die eine Komplikation für die aktuell angezeigte Überwachung des Benutzers durchführt, wird im Arbeitsspeicher beibehalten, wodurch die app schnell gestartet wird und die Geschwindigkeit der Antworten von der APP verbessert wird.
- Komplikationen erleichtern es dem Benutzer, bestimmte Funktionen in einer watchos-APP zu starten.

## <a name="glanceable-notification"></a>Benachrichtigungs Benachrichtigung

Benachrichtigungen auf Apple Watch bieten eine gute, anpassbare Methode, um den Benutzer schnell über Ereignisse oder neue Informationen zu informieren, wie z. b. eingehende Nachrichten oder das Erreichen eines Ziels in einer Trainings-app.

Mithilfe einer Benachrichtigung können dem Benutzer wertvolle Informationen angezeigt werden. In vielen Fällen kann eine gut entworfene Benachrichtigung die Notwendigkeit entfernen, dass der Benutzer die APP tatsächlich startet.

Neu bei watchos 3: alle Benachrichtigungen unterstützen jetzt Folgendes:

- SpriteKit
- SceneKit
- Inline Video

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Erweiterte Benutzeroberfläche mit spritekit und scenekit

In der Regel kann sich ein Entwickler an der Spiel-UI vorstellen, wenn spritekit und scenekit erwähnt werden. Allerdings können spritekit und scenekit nützlich sein, um nicht-Gaming-Benutzeroberflächen zu erstellen, die angepasste Layouts, Inhalte und Animationen enthalten, die in watchkit nicht allein möglich sind.

Beispielsweise kann eine Benutzer Benachrichtigung von einer Foto Freigabe-App spritekit verwenden, um eine umfangreiche Benutzerfreundlichkeit bereitzustellen, indem der Benutzer, der das Bild gepostet hat, zusammen mit einem tatsächlichen Bild und anderen angepassten Informationen, die die Benutzerfreundlichkeit bereichern, hinzu kommt.

Darüber hinaus können spritekit und scenekit mit standardmäßigen watchkit-Benutzeroberflächen Elementen im Benutzeroberflächen Design der APP gemischt werden.

## <a name="simple-navigation"></a>Einfache Navigation

watchos 3 bietet mehrere Möglichkeiten, wie ein Entwickler die Navigation in ihren watchos-apps vereinfachen kann, wie z. b. die neue [vertikale Auslagerung](#vertical-paging), die [Unterstützung von Gesten Erkennungs](#gesture-recognizer-support) Funktionen und die oben dargestellten [Digital Crown Rotations](#digital-crown-rotation) Features.

Der Digital Crown ist für die Apple Watch eindeutig und kann auf viele verschiedene Arten verwendet werden, um die Navigation zu vereinfachen. Beispielsweise kann eine timeranwendung das Digital Crown verwenden, um die verfügbaren Zeit Geber Längen zu bereinigen.

Benutzerdefinierte Gesten können neue und einmalige Möglichkeiten für den Benutzer zur Interaktion mit einer Watch-App darstellen und können auch verwendet werden, um die APP-Navigation zu vereinfachen.

Apple empfiehlt das Suchen nach Möglichkeiten, um alle neuen schnell Interaktions Features zu kombinieren, die in watchos 3 hinzugefügt wurden, um umfassende, einfache und schnell verwendbare watchos-App-Schnittstellen zu präsentieren.

## <a name="finishing-the-quick-interaction"></a>Fertigstellung der schnellen Interaktion

Ein gut konzipiertes schnelles Interaktions Verhalten bietet dem Benutzer die Zuversicht, das Handgelenk zu löschen (und sich mit der APP zu lösen), wenn die aktuelle Interaktion abgeschlossen ist.

Dies ist insbesondere dann der Fall, wenn die Watch-App eine beliebige Art von Netzwerkverbindung oder Freigabe Informationen mit der begleitenden iPhone-App durch nimmt. Dies kann häufig zu einem wartenden Indikator führen, während die Transaktion stattfindet, was bei einer schnellen Interaktion nicht wünschenswert ist. Sehen Sie sich das folgende Beispiel an.

[![Diagramm der Watch-APP, die eine Netzwerkverbindung erstellt und Informationen mit der begleitenden iPhone-App freigibt](quick-interaction-techniques-images/quick07.png)](quick-interaction-techniques-images/quick07.png#lightbox)

1. Der Benutzer wählt ein Element aus, das Sie bei der Überwachung erwerben möchten.
2. Sie tippen auf die Schaltfläche "kaufen".
3. Die APP startet die Netzwerk Transaktion und zeigt einen Lade Indikator an.
4. Zu einem späteren Zeitpunkt wird die Transaktion abgeschlossen, und die APP zeigt einen Kaufvorgang an.
5. Der Benutzer löscht das Handgelenk und entfährt mit der app.

Ab dem Zeitpunkt, an dem der Benutzer auf die Schaltfläche "kaufen" tippt, bis die Transaktion abgeschlossen ist, wird das Handgelenk mit einem Lade Indikator angezeigt. Um diese Situation zu beheben, schlägt Apple vor, dass der Entwickler sofort Feedback für den Benutzer stellen sollte, anstatt einen Lade Indikator anzuzeigen. 

Verwenden Sie das von Apple vorgeschlagene Modell, und sehen Sie sich die gleiche schnelle Interaktion erneut an:

[![Vom Apple vorgeschlagene Modell Diagramm](quick-interaction-techniques-images/quick08.png)](quick-interaction-techniques-images/quick08.png#lightbox)

1. Der Benutzer wählt ein Element aus, das Sie bei der Überwachung erwerben möchten.
2. Sie tippen auf die Schaltfläche "kaufen".
3. Die APP startet die Netzwerk Transaktion und zeigt eine Meldung an, die besagt, dass der Kauf erfolgreich gestartet wurde.
4. Der Benutzer löscht das Handgelenk und entfährt mit der app.
5. Wenn die Transaktion einige Zeit später erfolgreich abgeschlossen wird, zeigt die APP eine lokale Benachrichtigung an, um den Benutzer über einen erfolgreichen Kauf zu informieren.

Sobald der Benutzer auf die Schaltfläche "kaufen" tippt, wird eine Meldung mit dem Hinweis angezeigt, dass der Kauf gestartet wurde, sodass er das Handgelenk und die schnelle Interaktion zu diesem Zeitpunkt zuverlässig ablegen kann. Später werden Sie über den Erfolg oder Misserfolg der Transaktion in einer Benutzer Benachrichtigung informiert. Auf diese Weise interagiert der Benutzer nur während der "aktiven" Phasen des Prozesses mit der app.

Für apps, die Netzwerke ausführen, können Sie einen Hintergrund verwenden, `NSURLSession` um die Netzwerkkommunikation mit einer Download Aufgabe zu bewältigen. Dadurch kann die APP im Hintergrund reaktiviert werden, um die heruntergeladenen Informationen zu verarbeiten. Verwenden Sie für eine APP, die eine Hintergrundverarbeitung erfordert, eine Hintergrundaufgaben Erklärung, um die erforderliche Verarbeitung zu verarbeiten.

## <a name="quick-interaction-design-tips"></a>Entwurfs Tipps für die schnelle Interaktion

Da die gewünschte Länge einer schnellen Interaktion zwei Sekunden oder weniger beträgt, sollte sich der Entwickler auf den Entwurf der Interaktionen der APP vom Anfang des Entwurfsprozesses konzentrieren. Suchen Sie nach Bereichen, in denen diese Interaktionen vereinfacht werden können (mithilfe der oben dargestellten Technik), und verwenden Sie die neuen Features von watchos 3, um die app schnell und reaktionsfähig zu machen.

Apple schlägt folgendes vor:

- Konzentrieren Sie sich auf schnelle Interaktionen, indem Sie die am häufigsten verwendeten Features der APP weiterleiten.
- Verwenden Sie Komplikationen und Benutzer Benachrichtigungen, um allgemeine Features und Funktionen zu nutzen.
- Erstellen Sie mithilfe von scenekit und spritekit eine umfassende, Benutzeroberfläche mit Blick auf die Benutzeroberfläche.
- Vereinfachen Sie die Navigation in der APP, wann immer dies möglich ist.
- Legen Sie den Benutzer niemals ab, lassen Sie das Gerät so bald wie möglich mit der APP abkoppeln.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel befasst sich mit den schnell Interaktions Techniken, die Apple in watchos 3 hinzugefügt hat, und erläutert, wie diese in xamarin. IOS für Apple Watch implementiert werden.

## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
