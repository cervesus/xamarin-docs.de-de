---
title: MonoGame GamePad-Referenz
description: Dieses Dokument beschreibt GamePad, einer plattformübergreifenden-Klasse für den Zugriff auf Geräte, die in MonoGame Eingabe. Es wird erläutert, wie aus den Gamepad Eingabe zu lesen und stellt Beispielcode bereit.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c1c03e0ec17ade57536b4ed121469e3ae2274e75
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668971"
---
# <a name="monogame-gamepad-reference"></a>MonoGame GamePad-Referenz

_GamePad ist ein standard, Cross-Platform-Klasse für den Zugriff auf Eingabegeräte in MonoGame._

`GamePad` kann verwendet werden, von Eingabegeräte auf mehreren Plattformen von MonoGame lesen. Dieses Handbuch veranschaulicht, wie Arbeiten Sie mit der GamePad-Klasse. Da jede Eingabegerät unterscheidet sich in Layout und die Anzahl der bereitgestellten, enthält dieses Handbuch Diagramme, die die verschiedenen Geräte Zuordnungen zu zeigen.

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>GamePad als Ersatz für Xbox360GamePad

Die ursprüngliche bereitgestellte XNA-API die `Xbox360GamePad` Klasse zum Lesen von Eingaben über ein game Controller für die Xbox 360 oder PC. MonoGame wurde ersetzt, dies mit einem `GamePad` Klasse, da die Xbox 360-Controller für die meisten MonoGame-Plattformen (z. B. iOS oder deiner Xbox One) verwendet werden können. Trotz der Namensänderung die Verwendung von der `GamePad` Klasse ist vergleichbar mit der `Xbox360GamePad` Klasse.

## <a name="reading-input-from-gamepad"></a>Lesen von Eingaben über GamePad

Die `GamePad` Klasse bietet eine standardisierte Möglichkeit Lesen von Eingaben auf jeder Plattform MonoGame. Es enthält Informationen über zwei Methoden:

- `GetState` – Gibt den aktuellen Status von Schaltflächen, analoge Sticks und Steuerkreuz des Controllers zurück.
- `GetCapabilities` – Informationen zu den Funktionen der Hardware, zurückgibt, z. B., ob der Controller bestimmte hat Schaltflächen oder Vibration unterstützt.

### <a name="example-moving-a-character"></a>Beispiel: Ein Zeichen verschieben

Der folgende Code zeigt, wie der linke Ziehpunkt Stick verwendet werden kann, verschieben Sie ein Zeichen durch Festlegen seiner `XVelocity` und `YVelocity` Eigenschaften. Dieser Code setzt voraus, dass `characterInstance` ist eine Instanz eines Objekts mit `XVelocity` und `YVelocity` Eigenschaften:

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>Beispiel: Erkennen von Pushvorgängen

`GamePadState` enthält Informationen zu den aktuellen Zustand des Controllers, z. B., ob eine bestimmte Schaltfläche geklickt wird. Bestimmte Aktionen ausführen, z. B. indem ein Zeichen zu wechseln, erfordert überprüfen, wenn die Schaltfläche mithilfe von Push übertragen wurde (war nicht nach unten letzten Frame, aber Sie diesen Frame ist) oder freigegeben (war Sie letzten Frame, aber nicht nach diesem Frame unten).

Um diese Art von Logik, lokale Variablen auszuführen, die des vorherigen Frames zu speichern `GamePadState` und des aktuellen Frames `GamePadState` muss erstellt werden. Das folgende Beispiel zeigt, wie das Speichern und verwenden die vorherigen Frames `GamePadState` springen zu implementieren:

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

### <a name="example-checking-for-buttons"></a>Beispiel: Überprüfen für Schaltflächen

`GetCapabilities` kann verwendet werden, zu überprüfen, ob ein Controller über eine bestimmte Hardware, z. B. eine bestimmte Schaltfläche oder einen analogen Stick verfügt. Der folgende Code zeigt, wie für die Schaltflächen "B" und "Y" auf einem Domänencontroller in einem Spiel überprüft das Vorhandensein der beiden Schaltflächen müssen:

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

iOS-apps unterstützt die drahtlose Gamecontroller Eingabe.

> [!IMPORTANT]
> Die NuGet-Pakete für MonoGame 3.5 enthalten keine Unterstützung für drahtlose Gamecontroller. Mithilfe der GamePad-Klasse unter iOS ist erforderlich, MonoGame 3.5 von der Quelle erstellen, oder verwenden die MonoGame-3.6-NuGet-Binärdateien.

### <a name="ios-game-controller"></a>iOS-Spiel-Controller

Die `GamePad` Klasse zurückgibt, Eigenschaften, die von drahtlosen Controllern zu lesen. Die Eigenschaften in der `GamePad` bieten eine gute codeabdeckung für den standard iOS Controller Hardware, wie im folgenden Diagramm dargestellt:

![](input-images/image1.png "Die Eigenschaften in den GamePad bieten eine gute codeabdeckung für den standard-iOS Controller Hardware, wie in diesem Diagramm dargestellt.")

## <a name="apple-tv"></a>Apple TV

Apple TV-Spiele können die Siri Remote oder drahtlosen Gamecontroller für die Eingabe.

### <a name="siri-remote"></a>Siri-Remote

*Siri Remote* ist das systemeigene Eingabegerät für Apple TV. Obwohl Werte aus der Siri Remote über Ereignisse gelesen werden können (siehe die [Siri Remote- und Bluetooth-Controller –](~/ios/tvos/platform/remote-bluetooth.md)), wird die `GamePad` Klasse kann Werte von Siri Remote zurückgeben.

Beachten Sie, dass `GamePad` kann nur von der Schaltfläche "Abspielen" Lesen und touch-Oberfläche:

![](input-images/image2.png "Beachten Sie, dass GamePad kann nur von der Schaltfläche \"Abspielen\" zu lesen und touch-Oberfläche")

Seit der Touch-Oberfläche verschieben wird gelesen, über die `DPad` Eigenschaft Bewegung gemeldeten Werte werden mithilfe der `ButtonState` Klasse. Das heißt, Werte stehen nur als `ButtonState.Pressed` oder `ButtonState.Released`, im Gegensatz zu numerischen Werten oder Gesten.

### <a name="apple-tv-game-controller"></a>Apple TV-Gamecontroller

Gamecontroller für Apple TV Verhalten sich identisch, Gamecontroller für iOS-apps. Weitere Informationen finden Sie unter den [iOS Game Controller-Abschnitt](#iOS-game-controller). 

## <a name="xbox-one"></a>Xbox One

Die Xbox One-Konsole unterstützt, Lesen von Eingaben über eine Xbox One Gamecontroller.

### <a name="xbox-one-game-controller"></a>Spiele Xbox One-Controller

Der Xbox One game Controller ist das am häufigsten verwendeten Eingabegerät für Xbox One. Die `GamePad` Klasse bietet der Eingabewerten von der Hardware auf Gamecontroller.

![](input-images/image3.png "Die GamePad-Klasse bietet der Eingabewerten von der Hardware auf Gamecontroller")

## <a name="summary"></a>Zusammenfassung

Diese Anleitung enthält einer Übersicht über die MonoGame `GamePad` Klasse zum Implementieren von Eingabe-lesen-Logik und Diagramme der allgemeinen `GamePad` Implementierungen.

## <a name="related-links"></a>Verwandte Links

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
