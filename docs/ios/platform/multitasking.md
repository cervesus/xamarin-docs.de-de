---
title: Multitasking für iPad in xamarin. IOS
description: IOS 9 unterstützt zwei apps, die gleichzeitig ausgeführt werden, unter Verwendung der FolienFolie oder der geteilten Ansicht. Es unterstützt auch Videowiedergabe Bilder.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: aeb3d01a3d0f7edbe92c9959073d859fc63486a6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031656"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Multitasking für iPad in xamarin. IOS

_IOS 9 unterstützt zwei apps, die gleichzeitig ausgeführt werden, unter Verwendung der FolienFolie oder der geteilten Ansicht. Es unterstützt auch Videowiedergabe Bilder._

![](multitasking-images/about02-sml.png "Beispiel für einen Split Screen") ![](multitasking-images/about03-sml.png "Beispiel für Bild-in-Bild")

IOS 9 fügt eine Multitasking-Unterstützung für die gleich malige Ausführung von zwei apps auf einer bestimmten iPad-Hardware hinzu. Multitasking für iPad wird durch die folgenden Features unterstützt:

- [**Folie over**](#Slide-Over) : ermöglicht es dem Benutzer, eine zweite IOS-App temporär in einem Schiebe Bereich auszuführen (entweder auf der rechten oder linken Seite des Bildschirms basierend auf der Sprache), der ungefähr 25% der derzeit ausgelaufenden Haupt-App abdeckt. Die Folie ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar.
- [**Geteilte Ansicht**](#Split-View) : auf der unterstützten iPad-Hardware (iPad Air 2, iPad Mini 4 und iPad pro) kann der Benutzer eine zweite App auswählen und parallel mit der aktuell aktiven app in einem Split Screen-Modus ausführen. Der Benutzer kann den Prozentsatz des Hauptbildschirms steuern, den jede APP einnimmt.
- [**Bild im Bild**](#Picture-in-Picture) : für apps, die Videoinhalte wiedergeben, kann das Video nun in einem Fenster wiedergegeben werden, das sich in einem Fenster befindet und in der Größe geändert werden kann Der Benutzer hat die vollständige Kontrolle über die Größe und Position dieses Fensters. Bild im Bild ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar.

Bei der [Unterstützung von Multitasking in Ihrer APP](#Supporting-Multitasking-in-your-App)sind einige Punkte zu beachten:

- [Bildschirmgröße und-Ausrichtung](#Screen-Size-Considerations)
- [Tastenkombinationen für benutzerdefinierte Hardware](#Custom-Hardware-Keyboard-Shortcuts)
- [Ressourcenverwaltung](#Resource-Management-Considerations)

Als App-Entwickler können Sie das [Multitasking](#Opting-Out-of-Multitasking)auch deaktivieren, einschließlich der [Deaktivierung der PIP-Video Wiedergabe](#Disabling-PIP-Video-Playback).

In diesem Artikel werden die Schritte beschrieben, die erforderlich sind, um sicherzustellen, dass Ihre xamarin. IOS-app in einer Multitasking-Umgebung ordnungsgemäß ausgeführt wird, oder wie Sie das Multitasking ablehnen, wenn dies für Ihre APP nicht geeignet ist.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**Multitasking für iPad-Video**

<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Multitasking-Schnellstart

Zur Unterstützung der **Folie** -oder **geteilte Ansicht** muss Ihre APP die folgenden Schritte ausführen:

- Sie sollten für IOS 9 (oder höher) erstellt werden.
- Verwenden Sie ein Storyboard für den Startbildschirm (und nicht für Image Assets).
- Verwenden Sie ein Storyboard mit den Klassen "AutoLayout" und "size" für die Benutzeroberfläche.
- Alle vier IOS-Geräte Ausrichtungen unterstützen (Hochformat, Hochformat, Querformat, Querformat Links & Querformat).

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>Informationen zum Multitasking für iPad

IOS 9 bietet neue Multitasking-Fähigkeiten auf dem iPad mit der Einführung von _Folie over_, _Split View_ (iPad Air 2, iPad Mini 4 und iPad pro) und _Bild in Bild_. Wir sehen uns diese Features in den folgenden Abschnitten genauer an.

<a name="Slide-Over" />

### <a name="slide-over"></a>Folie über

Die Funktion "Folie over" ermöglicht dem Benutzer, eine zweite App auszuwählen und Sie in einem kleinen gleitenden Bereich anzuzeigen, um eine schnelle Interaktion zu ermöglichen. Die Folie über Panel ist temporär und wird geschlossen, wenn der Benutzer wieder mit der Haupt-APP arbeitet.

[![](multitasking-images/about01.png "The Slide Over panel")](multitasking-images/about01.png#lightbox)

Beachten Sie, dass der Benutzer entscheidet, welche beiden apps parallel ausgeführt werden und dass der Entwickler keine Kontrolle über diesen Prozess hat. Daher müssen Sie einige Dinge durchführen, um sicherzustellen, dass Ihre xamarin. IOS-App ordnungsgemäß in einer Folie über dem Panel ausgeführt wird:

- **Verwenden von "AutoLayout"-und "size"-Klassen** – da ihre xamarin. IOS-APP jetzt im Bereich "Folie Auslagern" ausgeführt werden kann, können Sie sich nicht mehr auf das Gerät, seine Bildschirmgröße oder seine Ausrichtung zum Layout Ihrer Benutzeroberfläche verlassen. Um sicherzustellen, dass die Benutzeroberfläche Ihrer APP ordnungsgemäß skaliert wird, müssen Sie die Klassen "AutoLayout" und "size" verwenden. Weitere Informationen finden [Sie in unserer Einführung in die Dokumentation zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .
- **Effizientes Verwenden von Ressourcen** – da ihre App nun das System mit einer anderen laufenden app teilen kann, ist es wichtig, dass Ihre APP Systemressourcen effizient verwendet. Wenn der Arbeitsspeicher geringer wird, beendet das System automatisch die APP, die den meisten Arbeitsspeicher beansprucht. Weitere Informationen finden Sie im [Handbuch zur Energieeffizienz von Apple für IOS-apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) .

Die Folie ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar. Weitere Informationen zum Vorbereiten Ihrer APP für die Folie finden Sie in der Dokumentation zum Entwickeln von [Multitasking in Apple in der iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) -Dokumentation.

<a name="Split-View" />

### <a name="split-view"></a>Geteilte Ansicht

Auf unterstützten iPad-Hardware (iPad Air 2, iPad Mini 4 und iPad pro) kann der Benutzer eine zweite App auswählen und parallel mit der aktuell aktiven app in einem Split Screen-Modus ausführen. Der Benutzer kann den Prozentsatz des Hauptbildschirms steuern, den jede APP einnimmt, indem Sie einen Bildschirm unter Teiler ziehen.

[![](multitasking-images/about02.png "The Split View")](multitasking-images/about02.png#lightbox)

Wie bei der Folie auch, entscheidet der Benutzer, welche beiden apps parallel ausgeführt werden, und der Entwickler hat keine Kontrolle über diesen Prozess. Die geteilte Ansicht stellt daher ähnliche Anforderungen in einer xamarin. IOS-App dar:

- **Verwenden von "AutoLayout"-und "size"-Klassen** – da ihre xamarin. IOS-APP jetzt in einem Split-Screen-Modus mit der angegebenen Größe des Benutzers ausgeführt werden kann, können Sie sich nicht mehr auf das Gerät, seine Bildschirmgröße oder seine Ausrichtung zum Layout Ihrer Benutzeroberfläche verlassen. Um sicherzustellen, dass die Benutzeroberfläche Ihrer APP ordnungsgemäß skaliert wird, müssen Sie die Klassen "AutoLayout" und "size" verwenden. Weitere Informationen finden [Sie in unserer Einführung in die Dokumentation zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .
- **Effizientes Verwenden von Ressourcen** – da ihre App nun das System mit einer anderen laufenden app teilen kann, ist es wichtig, dass Ihre APP Systemressourcen effizient verwendet. Wenn der Arbeitsspeicher geringer wird, beendet das System automatisch die APP, die den meisten Arbeitsspeicher beansprucht. Weitere Informationen finden Sie im [Handbuch zur Energieeffizienz von Apple für IOS-apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) .

Weitere Informationen zum Vorbereiten Ihrer APP für die geteilte Ansicht finden Sie in der Apple-Dokumentation [unter Einführung von Multitasking-Erweiterungen in der iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) -Dokumentation.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Bild im Bild

Mit der neuen Abbildung in der Bildfunktion (auch als _PIP_bezeichnet) kann der Benutzer ein Video in einem kleinen, unverankerten Fenster ansehen, das der Benutzer überall auf dem Bildschirm über anderen ausgeführte apps positionieren kann.

[![](multitasking-images/about03.png "An example Picture in Picture floating window")](multitasking-images/about03.png#lightbox)

Wie bei der Folie und der geteilten Ansicht hat der Benutzer die vollständige Kontrolle über das Ansehen eines Videos im Bild Modus. Wenn die Hauptfunktion Ihrer APP das Ansehen von Videos ist, müssen einige Änderungen vorgenommen werden, damit Sie sich im PIP-Modus ordnungsgemäß Verhalten. Andernfalls sind keine Änderungen erforderlich, um PIP zu unterstützen.

Damit Ihre APP PIP-Videos auf der Anforderung des Benutzers anzeigt, müssen Sie entweder _avkit_ oder die _AV Foundation-APIs_verwenden. Das Media Player Framework wurde in ios 9 zurückgegeben und unterstützt PIP nicht.

Bild im Bild ist nur auf iPad pro, iPad Air, iPad Air 2, iPad Mini 2, iPad Mini 3 oder iPad Mini 4 verfügbar. Weitere Informationen finden Sie in der [Beispiel-app "pictureinpicture](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9) " und in der Abbildung von Apple [in der Abbildung Schnellstart](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) Dokumentation.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Unterstützen von Multitasking in Ihrer APP

Für jede vorhandene xamarin. IOS-APP ist die Unterstützung von Multitasking eine transparente Aufgabe, solange Ihre APP bereits den Entwurfs Handbüchern von Apple und den bewährten Methoden für IOS 8 folgt. Dies bedeutet, dass die APP Storyboards mit den Klassen "AutoLayout" und "size" für die Benutzeroberflächen Layouts verwenden sollte (Weitere Informationen finden [Sie in der Einführung zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) ).

Für diese apps sind nur wenige oder gar keine Änderungen erforderlich, um Multitasking zu unterstützen und sich in der Anwendung gut zu Verhalten. Wenn die Benutzeroberfläche Ihrer APP mit anderen Methoden erstellt wurde, z. b. bei der direkten Positionierung C# und Größenanpassung von Benutzeroberflächen Elementen im Code oder bei Verwendung bestimmter Geräte Bildschirmgrößen oder-Ausrichtungen, ist eine bedeutende Änderung erforderlich, um IOS 9-Multitasking ordnungsgemäß zu unterstützen.

Um IOS 9-Multitasking für jede neue xamarin. IOS-APP zu unterstützen, verwenden Sie für alle Benutzeroberflächen Layouts der APP erneut Storyboards mit den Klassen "AutoLayout" und "size", und implementieren Sie die Anweisungen in den folgenden Abschnitten.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Überlegungen zur Bildschirmgröße und-Ausrichtung

Vor IOS 9 konnten Sie Ihre APP für bestimmte Geräte Bildschirmgrößen und-Ausrichtungen entwerfen. Da eine App nun in einem Schiebe Bereich oder im Split-View-Modus ausgeführt werden kann, kann Sie unabhängig von der physischen Ausrichtung des Geräts oder der Bildschirmgröße entweder in einer kompakten oder regulären horizontalen Größenklasse ausgeführt werden.

[![](multitasking-images/sizeclasses01.png "Screen Size and Orientation Considerations")](multitasking-images/sizeclasses01.png#lightbox)

Auf einem iPad verfügt eine Vollbild-App über reguläre Klassen mit horizontaler und vertikaler Größe. Alle iPhones, aber iPhone 6 Plus und iPhone 6S Plus, haben in beide Richtungen in beliebiger Richtung Compact size-Klassen. Das iPhone 6 Plus-und iPhone 6S Plus im Querformat haben eine reguläre horizontale Größenklasse und eine kompakte vertikale Größenklasse (ähnlich wie ein iPad Mini).

Auf iPads, die eine Folie-und geteilte Ansicht unterstützen, können Sie die folgenden Kombinationen beenden:

| **Ausrichtung** | **Primäre App** | **Sekundäre App** |
|--- |--- |--- |
| **Hochformat** |75% des Bildschirms<br />Horizontal kompakt<br />Regulär vertikal|25% des Bildschirms<br />Horizontal kompakt<br />Regulär vertikal|
| **Landschaf** |75% des Bildschirms<br />Regulär horizontal<br />Regulär vertikal|25% des Bildschirms<br />Horizontal kompakt<br />Regulär vertikal|
| **Landschaf** |50% des Bildschirms<br />Horizontal kompakt<br />Regulär vertikal|50% des Bildschirms<br />Horizontal kompakt<br />Regulär vertikal|

Wenn in der [mulitask](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask) -Beispiel-App auf einem iPad im Querformat Vollbild ausgeführt wird, werden sowohl die Liste als auch die Detailansicht zur gleichen Zeit angezeigt:

[![](multitasking-images/sizeclasses03.png "The list and the detail view presented at the same time")](multitasking-images/sizeclasses03.png#lightbox)

Wenn dieselbe app in einer Folie über dem Panel ausgeführt wird, wird Sie als kompakte horizontale Größenklasse angelegt und zeigt nur die Liste an:

[![](multitasking-images/sizeclasses04.png "Only the list presented when the device is horizontal")](multitasking-images/sizeclasses04.png#lightbox)

Um sicherzustellen, dass Ihre APP in diesen Situationen ordnungsgemäß funktioniert, sollten Sie Merkmals Auflistungen zusammen mit Größenklassen übernehmen und den `IUIContentContainer`-und `IUITraitEnvironment`-Schnittstellen entsprechen. Weitere Informationen finden Sie in der [uitraitcollection-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) von Apple und im Leitfaden [Introduction to Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

Außerdem können Sie sich nicht mehr auf die Bildschirm Begrenzungen des Geräts verlassen, um den sichtbaren Bereich der APP zu definieren. Sie müssen stattdessen die Fenster Begrenzungen Ihrer APP verwenden. Da die Fenster Begrenzungen vollständig unter der Kontrolle des Benutzers sind, können Sie Sie nicht Programm gesteuert anpassen oder den Benutzer daran hindern, diese Begrenzungen zu ändern.

Zum Schluss muss Ihre APP eine storyboarddatei verwenden, um den Startbildschirm zu präsentieren, anstatt einen Satz von **PNG** -Bilddateien zu verwenden, und alle vier Schnittstellen Ausrichtungen (Hochformat, hoch-nach-unten-Hochformat, Querformat links und Querformat) zu unterstützen, die für Ausführung in einer Folie über Panel oder im Split-View-Modus.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Tastenkombinationen für benutzerdefinierte Hardware

In ios 9, das auf einem iPad ausgeführt wird, bietet Apple erweiterte Unterstützung für Hardware-Tastaturen. iPads haben immer grundlegende externe Tastatur Unterstützung über Bluetooth eingeschlossen, und einige Tastatur Hersteller haben Tastaturen erstellt, die hart gebundene IOS-spezifische Schlüssel enthielt.

Mit IOS 9 können apps nun eigene benutzerdefinierte Tastenkombinationen erstellen. Außerdem sind einige grundlegende Tastenkombinationen verfügbar, z. **b. Command-C** (Copy), **Command-X** (Ausschneiden), **Command-V** (einfügen) und **Command-Shift-H** (Home), ohne dass eine APP speziell darauf geschrieben wird.

Mit der **Befehlszeile** wird ein App-Umschaltung angezeigt, der es dem Benutzer ermöglicht, schnell zwischen Apps über die Tastatur zu wechseln, ähnlich wie in der Mac OS:

[![](multitasking-images/keyboard01.png "The app switcher")](multitasking-images/keyboard01.png#lightbox)

Wenn eine IOS 9-APP Tastenkombinationen enthält, kann der Benutzer die **Befehls**-, **options** -oder **Steuer** Element Taste gedrückt halten, um Sie in einem Popup Fenster anzuzeigen:

[![](multitasking-images/keyboard02.png "The keyboard shortcuts popup")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Definieren von benutzerdefinierten Tastenkombinationen

Wenn Sie den folgenden Code zu einer Ansicht oder einem Ansichts Controller in der APP hinzufügen, ist diese Ansicht oder der Controller sichtbar, wenn diese Ansicht oder der Controller sichtbar ist:

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

Zuerst überschreiben wir die `CanBecomeFirstResponder`-Eigenschaft und geben `true` zurück, damit der Ansichts-oder Ansichts Controller Tastatureingaben empfangen kann. 

Als nächstes überschreiben wir die `KeyCommands`-Eigenschaft und erstellen eine neue `UIKeyCommand` für den **Command-N-** Tastatur Strich. Wenn die Tastatureingabe aktiviert ist, wird die `NewEntry`-Methode aufgerufen (die wir für IOS 9 mit dem `Export`-Befehl verfügbar machen), um die angeforderte Aktion auszuführen.

Wenn wir diese APP auf einem iPad mit angefügter Hardware Tastatur ausführen und die Benutzereingaben **Command-N**ausführen, wird der Liste ein neuer Eintrag hinzugefügt. Wenn der Benutzer die **Befehls** Taste herunterhält, wird die Liste der Verknüpfungen angezeigt:

[![](multitasking-images/keyboard03.png "The keyboard shortcuts popup")](multitasking-images/keyboard03.png#lightbox)

Eine Beispiel Implementierung finden Sie in der Beispiel- [App für mehr Aufgaben](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask) .

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Überlegungen zur Ressourcenverwaltung

Auch für apps, die bereits IOS 8-Entwurfs Handbücher und bewährte Methoden verwenden, kann die effiziente Ressourcenverwaltung weiterhin ein Problem sein. In ios 9 verfügen apps nicht mehr über die exklusive Nutzung von Arbeitsspeicher, CPU oder anderen Systemressourcen.

Daher müssen Sie Ihre xamarin. IOS-App für die effektive Verwendung von Systemressourcen optimieren, oder es ist ein Abbruch in Situationen mit geringem Arbeitsspeicher möglich. Dies gilt auch für apps, die das Multitasking ablehnen, da eine zweite App möglicherweise weiterhin in einer Folie über dem Bereich oder in einem Bild im Bildfenster ausgeführt wird, das zusätzliche Ressourcen erfordert oder die Aktualisierungsrate unter 60 Frames pro Sekunde fällt.

Sehen Sie sich die folgenden Benutzeraktionen und ihre Auswirkungen an:

- **Eingeben von Text in einer Folie über dem Panel** , auch wenn Ihre APP keine Text Eingabe hat, kann die System Tastatur jetzt über die Benutzeroberfläche angezeigt werden. Folglich muss die APP möglicherweise auf Benachrichtigungen der Tastatur Anzeige Antworten (z. b. das Anzeigen und Ausblenden der Tastatur).
- **Ausführen einer zweiten app in einer Folie über dem Panel** : die neue APP wird jetzt im Vordergrund ausgeführt und konkurriert mit der vorhandenen APP für Systemressourcen, z. b. Arbeitsspeicher-und CPU-Zyklen.
- **Abspielen eines Videos in einem PIP-Fenster** : dieses Fenster deckt nicht nur einen Teil der Benutzeroberfläche der App ab, sondern die APP, die das Video gestartet hat, wird weiterhin im Hintergrund ausgeführt und beansprucht CPU-und Speicherressourcen.

Um sicherzustellen, dass Ihre APP Ressourcen effizient verwendet, sollten Sie die folgenden Schritte ausführen:

- **Profilerstellung für die APP mit Instrumenten** : Überprüfen Sie, ob Speicher Verluste auftreten, überprüfen Sie die CPU-Auslastung und Bereiche, in denen die APP den Haupt Thread ggf
- **Reagieren auf Zustandsübergänge-Methoden** : in Ihrer **AppDelegate.cs** -Datei Überschreibung und in der Antwort auf Zustands Änderungs Methoden, z. b. die APP, die den Hintergrund eingegeben oder zurückgibt. Geben Sie alle nicht benötigten Assets wie Bilder, Daten oder Sichten und den Ansichts Controller frei.
- Paralleles **Testen von apps mit Arbeitsspeicher intensiven apps** : führen Sie Ihre App mithilfe von "Folie out" und "Split View" auf physischer IOS-Hardware mit einer Arbeitsspeicher intensiven APP wie Maps (im Satelliten Ansichtsmodus) aus, und testen Sie, ob beide apps reaktionsfähig bleiben und nicht abstürzen.

Weitere Informationen zur Ressourcenverwaltung finden Sie im [Handbuch zur Energieeffizienz von Apple für IOS-apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) .

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Ablehnen von Multitasking

Obwohl Apple vorschlägt, dass alle IOS 9-apps Multitasking unterstützen, kann es sehr spezielle Gründe für eine APP sein, z. b. Spiele oder Kamera-apps, die erfordern, dass der vollständige Bildschirm ordnungsgemäß funktioniert.

Damit Ihre xamarin. IOS-App deaktiviert wird, können Sie die Datei " **Info. plist** " des Projekts bearbeiten, und aktivieren Sie den **voll Bild**Modus:

[![](multitasking-images/fullscreen01.png "Opting Out of Multitasking")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> Während das Deaktivieren des Multitasking verhindert, dass Ihre APP in der Folie oder in der geteilten Ansicht ausgeführt wird, verhindert es nicht, dass eine andere app in einer Folie ausgeführt wird oder ein Bild in der Bilddatei zusammen mit der App angezeigt wird.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>Deaktivieren der PIP-Video Wiedergabe

In den meisten Fällen sollte Ihre APP es dem Benutzer ermöglichen, beliebige Videoinhalte wiederzugeben, die in einem Bild im unverankerten Fenster angezeigt werden. Es kann jedoch Situationen geben, in denen dies möglicherweise nicht erwünscht ist, z. b. Videos mit Spiel Ausschnitten.

Um die PIP-Videowiedergabe zu abonnieren, führen Sie in Ihrer APP die folgenden Schritte aus:

- Wenn Sie ein `AVPlayerViewController` zum Anzeigen von Videos verwenden, legen Sie die Eigenschaft `AllowsPictureInPicturePlayback` auf `false`fest.
- Wenn Sie die `AVPlayerLayer` zum Anzeigen von Videos verwenden, instanziieren Sie keine `AVPictureInPictureController`.
- Wenn Sie ein `WKWebView` zum Anzeigen von Videos verwenden, legen Sie die Eigenschaft `AllowsPictureInPictureMediaPlayback` auf `false`fest.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Schritte erläutert, die erforderlich sind, um sicherzustellen, dass eine xamarin. IOS-app ausgeführt wird und sich in der neuen Multitasking-Funktion von IOS 9 korrekt verhält. Außerdem wurde das Opt-out-Multitasking für apps abgedeckt, bei denen es sich nicht um eine gute geeignete Funktion handelt.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Multitask (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-multitask)
- [Einführung in vereinheitlichte Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [IOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Einführung von Multitasking-Erweiterungen auf dem iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [Blog Beitrag](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
