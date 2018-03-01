---
title: MonoGame GamePad Verweis
description: "GamePad ist ein standard, plattformübergreifende Klasse für den Zugriff auf Geräte, die in MonoGame Eingabe."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 14a38c07b2ae5552cd9fb67d0cec581eafbf61cb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="monogame-gamepad-reference"></a>MonoGame GamePad Verweis

_GamePad ist ein standard, plattformübergreifende Klasse für den Zugriff auf Geräte, die in MonoGame Eingabe._

`GamePad` kann zum Lesen von Eingabegeräten auf mehreren MonoGame-Plattformen verwendet werden. Diese Anleitung zeigt, wie zur Bearbeitung der GamePad-Klasse. Dieses Handbuch enthält Diagramme, die die verschiedenen Gerät Zuordnungen angezeigt, da jedes Eingabegerät, das Layout und die Anzahl der bereitgestellten im Detail variiert.


# <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>GamePad als Ersatz für Xbox360GamePad

Der ursprüngliche bereitgestellten XNA-API die `Xbox360GamePad` -Klasse zum Lesen von Eingaben aus einer Gamecontroller auf der Xbox 360 oder PC. MonoGame wurde ersetzt, das mit einem `GamePad` -Klasse seit Xbox 360-Controller auf den meisten MonoGame-Plattformen (z. B. iOS oder Xbox One) verwendet werden können. Trotz der Namensänderung die Verwendung der `GamePad` -Klasse ähnelt der `Xbox360GamePad` Klasse.


# <a name="reading-input-from-gamepad"></a>Lesen von Eingaben über GamePad

Die `GameController` -Klasse bietet eine standardisierte Möglichkeit der Lesen von Eingaben auf jeder Plattform MonoGame. Es enthält Informationen über zwei Methoden:

 - `GetState` – Gibt den aktuellen Status von Schaltflächen, analogen Sticks und Steuerkreuz des Controllers zurück.
 - `GetCapabilities` – Gibt Informationen zu den Funktionen der Hardware, wie z. B. Schaltflächen oder unterstützt das Vibrationen, ob der Controller sicher ist.


## <a name="example-moving-a-character"></a>Beispiel: Verschieben eines Zeichens

Der folgende Code zeigt, wie der linken Ziehpunkt Stick verwendet werden kann, so verschieben Sie ein Zeichen durch Festlegen seiner `XVelocity` und `YVelocity` Eigenschaften. Dieser Code setzt voraus, dass `characterInstance` ist eine Instanz eines Objekts mit `XVelocity` und `YVelocity` Eigenschaften:


```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```


## <a name="example-detecting-pushes"></a>Beispiel: Erkennen von Pushbenachrichtigungen

`GamePadState` enthält Informationen zu den aktuellen Status des Controllers an, z. B., ob eine bestimmte gedrückt wird. Bestimmte Aktionen, z. B. ein Zeichen zu wechseln, wodurch erfordern überprüfen, wenn der Knopf (konnte nicht nach unten letzten Frame, aber dieses Rahmens ist) oder freigegeben (nach unten letzten Frame, aber nicht nach unten dieses Rahmens wurde). 

Diese Art von Logik, lokale Variablen ausführen, die den vorherigen Frames speichern `GamePadState` und den aktuellen Frame `GamePadState` muss erstellt werden. Im folgende Beispiel wird gezeigt, wie das Speichern und verwenden den vorherigen Frames `GamePadState` springen zu implementieren:


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


## <a name="example-checking-for-buttons"></a>Beispiel: Überprüfung für Schaltflächen

`GetCapabilities` kann verwendet werden, zu prüfen, ob ein Controller bestimmter Hardware, z. B. eine bestimmte Schaltfläche oder analogen Stick enthält. Der folgende Code zeigt, wie für die Schaltflächen "B" und "Y" auf einem Domänencontroller in einem Spiel überprüft erfordert das Vorhandensein der beiden Schaltflächen:


```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```


# <a name="ios"></a>iOS

iOS-apps unterstützen drahtlosen Gamecontroller Eingabe.

> [!IMPORTANT]
> Die NuGet-Pakete für MonoGame 3.5 enthalten keine Unterstützung für drahtlose Gamecontroller. Verwenden der GamePad-Klasse auf iOS erfordert MonoGame 3.5 von der Quelle erstellen oder verwenden die MonoGame 3.6 NuGet-Binärdateien. 



## <a name="ios-game-controller"></a>iOS Gamecontroller

Die `GamePad` Klasse gibt die Eigenschaften von wireless-Controllern. Die Eigenschaften in der `GamePad` gute codeabdeckung für das standardmäßige iOS Controller stellen Hardware zur Verfügung, wie im folgenden Diagramm dargestellt:

![](input-images/image1.png "Die Eigenschaften in der GamePad bieten gute codeabdeckung für das standardmäßige iOS Controller Hardware, wie in diesem Diagramm angezeigt")


# <a name="apple-tv"></a>Apple TV

Apple TV-Spiele können die Siri Remote oder drahtlosen Gamecontroller für die Eingabe.


## <a name="siri-remote"></a>Siri Remote

*Siri Remote* ist das native Eingabegerät für App TV. Obwohl Werte aus den Siri Remote über Ereignisse gelesen werden können (entsprechend der [Siri Remote und Bluetooth-Controller-Handbuch](~/ios/tvos/platform/remote-bluetooth.md)), wird die `GamePad` Klasse kann Werte aus der entfernten Siri zurückgegeben.

Beachten Sie, dass `GamePad` können nur lesen Eingaben über die entsprechende Schaltfläche und touch-Oberfläche: 

![](input-images/image2.png "Beachten Sie, dass GamePad kann nur Eingaben über die entsprechende Schaltfläche Lesen und touch-Oberfläche")

Seit einem-Oberfläche Bewegung lesen ist die `DPad` Eigenschaft Bewegung gemeldeten Werte werden mithilfe der `ButtonState` Klasse. Das heißt, sind nur als verfügbar `ButtonState.Pressed` oder `ButtonState.Released`, im Gegensatz zu numerischen Werten oder Gesten.


## <a name="apple-tv-game-controller"></a>Apple TV Gamecontroller

Gamecontroller für App TV Verhalten sich ebenso wie Gamecontroller für iOS-apps. Weitere Informationen finden Sie unter der [iOS Gamecontroller Abschnitt](#iOS_Game_Controller). 


# <a name="xbox-one"></a>Xbox One

Die Xbox One-Konsole unterstützt Lesen von Eingaben aus einer Xbox One Gamecontroller.


## <a name="xbox-one-game-controller"></a>Xbox One Gamecontroller

Die Xbox One Gamecontroller ist die am häufigsten verwendeten Eingabegerät, das für die Xbox One. Die `GamePad` -Klasse bietet Eingabewerte von der Hardware Gamecontroller.

![](input-images/image3.png "Die Klasse GamePad bietet Eingabewerte von der Hardware Gamecontroller")


# <a name="summary"></a>Zusammenfassung

Dieses Handbuch liefert einen Überblick über die MonoGame `GamePad` Klasse zum Implementieren von Eingabe-lesen-Logik und die Diagramme der gemeinsamen `GamePad` Implementierungen.

## <a name="related-links"></a>Verwandte Links

- [MonoGame GamePad](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
