---
title: Monogame-Gamepad-Referenz
description: In diesem Dokument wird Gamepad beschrieben, eine plattformübergreifende Klasse für den Zugriff auf Eingabegeräte in monogame. Es wird erläutert, wie Sie Eingaben aus dem Gamepad lesen und Beispielcode bereitstellen.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 11b1cfda435e97b09f9d1eded22f11f912d1783d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934029"
---
# <a name="monogame-gamepad-reference"></a>Monogame-Gamepad-Referenz

_Gamepad ist eine plattformübergreifende Standardklasse für den Zugriff auf Eingabegeräte in monogame._

`GamePad`kann zum Lesen von Eingaben von Eingabegeräten auf mehreren monogame-Plattformen verwendet werden. Diese Anleitung zeigt, wie Sie mit der Gamepad-Klasse arbeiten. Da sich jedes Eingabegerät im Layout und in der Anzahl der von ihm bereitgestellten Schaltflächen unterscheidet, enthält dieses Handbuch Diagramme, in denen die verschiedenen Geräte Zuordnungen angezeigt werden.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Gamepad als Ersatz für Xbox360GamePad

Die ursprüngliche XNA-API stellte die `Xbox360GamePad` Klasse zum Lesen von Eingaben von einem Spiele Controller auf der Xbox 360 oder dem PC bereit. Monogame hat dies durch eine Klasse ersetzt, `GamePad` da Xbox 360-Controller nicht auf den meisten monogame-Plattformen (z. b. IOS oder Xbox One) verwendet werden können. Trotz der Namensänderung ähnelt die Verwendung der- `GamePad` Klasse der- `Xbox360GamePad` Klasse.

## <a name="reading-input-from-gamepad"></a>Lesen von Eingaben aus Gamepad

Die- `GamePad` Klasse bietet eine standardisierte Möglichkeit zum Lesen von Eingaben auf jeder monogame-Plattform. Sie enthält Informationen über zwei Methoden:

- `GetState`– Gibt den aktuellen Zustand der Schaltflächen, die analogen Sticks und das d-Pad des Controllers zurück.
- `GetCapabilities`– Gibt Informationen zu den Funktionen von Hardware zurück, z. b. ob der Controller über bestimmte Schaltflächen verfügt oder Vibrationen unterstützt.

### <a name="example-moving-a-character"></a>Beispiel: Verschieben eines Zeichens

Der folgende Code zeigt, wie der linke Ziehpunkt zum Verschieben eines Zeichens verwendet werden kann, indem seine-Eigenschaft und die-Eigenschaft festgelegt werden `XVelocity` `YVelocity` . Bei diesem Code wird davon ausgegangen, dass `characterInstance` eine Instanz eines Objekts ist, das über die `XVelocity` Eigenschaften und verfügt `YVelocity` :

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>Beispiel: Erkennen von pushvorgängen

`GamePadState`stellt Informationen zum aktuellen Zustand des Controllers bereit, z. b. ob eine bestimmte Schaltfläche gedrückt wird. Bestimmte Aktionen (z. b. das Ausführen eines Zeichens) erfordern das überprüfen, ob die Schaltfläche gedrückt wurde (war nicht der letzte Rahmen, aber der Frame ist nicht nach unten) oder freigegeben (war der letzte Frame, aber nicht in diesem Frame).

Um diese Art von Logik auszuführen, müssen lokale Variablen erstellt werden, die den vorherigen Frame `GamePadState` und den aktuellen Frame speichern `GamePadState` . Im folgenden Beispiel wird gezeigt, wie Sie den vorherigen Frame zum Speichern und verwenden können `GamePadState` , um das Springen zu implementieren:

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed =
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>Beispiel: Überprüfen auf Schaltflächen

`GetCapabilities`kann verwendet werden, um zu überprüfen, ob ein Controller über bestimmte Hardware verfügt, z. b. eine bestimmte Schaltfläche oder einen analogen Stift Der folgende Code zeigt, wie die Schaltflächen B und Y eines Controllers in einem Spiel überprüft werden, für das das vorhanden sein beider Schaltflächen erforderlich ist:

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

IOS-apps unterstützen die Eingabe des drahtlos Spiel Controllers.

> [!IMPORTANT]
> Die nuget-Pakete für monogame 3,5 enthalten keine Unterstützung für drahtlos Spielcontroller. Die Verwendung der Gamepad-Klasse unter IOS erfordert die Verwendung von monogame 3,5 aus der Quelle oder die Verwendung der monogame-Binärdateien mit den Binärdateien 3,6.

### <a name="ios-game-controller"></a>IOS-Spiele Controller

Die `GamePad` Klasse gibt Eigenschaften zurück, die von drahtlos Controllern gelesen werden. Die Eigenschaften in `GamePad` bieten eine gute Abdeckung für die standardmäßige IOS-Controller-Hardware, wie in der folgenden Abbildung dargestellt:

![Die Eigenschaften im Gamepad bieten eine gute Abdeckung für die standardmäßige IOS-Controller-Hardware, wie in diesem Diagramm dargestellt.](input-images/image1.png)

## <a name="apple-tv"></a>Apple TV

Apple TV-Spiele können die Siri-Remote-oder drahtlos Spielcontroller für die Eingabe verwenden.

### <a name="siri-remote"></a>Siri (Remote)

*Siri Remote* ist das Native Eingabegerät für Apple TV. Obwohl die Werte von Siri Remote durch Ereignisse gelesen werden können (wie im Handbuch für die [Remote-und Bluetooth-Controller von Siri](~/ios/tvos/platform/remote-bluetooth.md)), kann die- `GamePad` Klasse Werte von der Siri-Remote Rückgabe zurückgeben.

Beachten Sie, dass `GamePad` die Eingaben nur über die Wiedergabe Schaltfläche und die Berührungs Oberfläche lesen können:

![Beachten Sie, dass Gamepad nur Eingaben aus der Wiedergabe Schaltfläche und der Touchoberfläche lesen kann.](input-images/image2.png)

Da die Berührungs Oberflächenbewegung durch die-Eigenschaft gelesen wird `DPad` , werden Verschiebungs Werte mithilfe der- `ButtonState` Klasse gemeldet. Mit anderen Worten, Werte sind nur als `ButtonState.Pressed` oder verfügbar `ButtonState.Released` , im Gegensatz zu numerischen Werten oder Gesten.

### <a name="apple-tv-game-controller"></a>Apple TV Game Controller

Spiele Controller für Apple TV Verhalten sich identisch mit Spiel Controllern für IOS-apps. Weitere Informationen finden Sie im Abschnitt [IOS Game Controller](#ios-game-controller) . 

## <a name="xbox-one"></a>Xbox One

Die Xbox One-Konsole unterstützt das Lesen von Eingaben von einem Xbox One-Spielcontroller.

### <a name="xbox-one-game-controller"></a>Xbox One-Spiel Controller

Der Xbox One-Spielcontroller ist das gängigste Eingabegerät für die Xbox One. Die- `GamePad` Klasse stellt Eingabewerte von der Game Controller-Hardware bereit.

![Die Gamepad-Klasse stellt Eingabewerte von der Game Controller-Hardware bereit.](input-images/image3.png)

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch bietet eine Übersicht über die-Klasse von monogame `GamePad` , die Implementierung von Eingabe Lese Logik und Diagramme allgemeiner `GamePad` Implementierungen.

## <a name="related-links"></a>Ähnliche Themen

- [Monogame-Gamepad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
