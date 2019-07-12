---
title: Multitasking für iPad im Xamarin.iOS
description: iOS 9 unterstützt zwei apps, die auf der gleichen Zeit, Folie über oder geteilte Ansicht ausgeführt wird. Darüber hinaus wird die Wiedergabe von Bild-im-Bild-Video unterstützt.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 888e00fbdbf30b5b2842bc30822a55f57372eb34
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831809"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Multitasking für iPad im Xamarin.iOS

_iOS 9 unterstützt zwei apps, die auf der gleichen Zeit, Folie über oder geteilte Ansicht ausgeführt wird. Darüber hinaus wird die Wiedergabe von Bild-im-Bild-Video unterstützt._

![](multitasking-images/about02-sml.png "Bildschirm Beispiel teilen") ![](multitasking-images/about03-sml.png "Bild-Beispiel")

iOS 9 fügt Multitasking-Unterstützung für die Ausführung von zwei apps gleichzeitig auf bestimmte iPad-Hardware. Multitasking für iPad wird über die folgenden Funktionen unterstützt:

- [**Folie über** ](#Slide-Over) -ermöglicht dem Benutzer, die vorübergehend eine zweite iOS-app in eine Folie, Bereich (entweder auf den rechten oder linken Seite des Bildschirms basierend auf Sprache Richtung) ausgeführt, die CA. 25 % der Haupt-app ausgeführten abdeckt. Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.
- [**Geteilte Ansicht** ](#Split-View) – der Benutzer kann auf unterstützten iPad-Hardware (iPad Air 2, iPad Mini-4 und iPad nur Pro), wählen Sie eine zweite app und Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirms-Modus ausführen. Der Benutzer kann es sich um den Prozentsatz des Hauptbildschirms steuern, die jeder app belegt.
- [**Bild in Abbildung** ](#Picture-in-Picture) : für apps, die Wiedergabe von Videoinhalten, das video kann jetzt in einem verschiebbaren und in der Größe veränderbaren Fenster wiedergegeben werden, die über die anderen apps, die derzeit auf dem iOS-Gerät ausgeführten gleitet. Der Benutzer hat vollständige Kontrolle über die Größe und Position des Fensters. Bild in Abbildung steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.

Es gibt eine Reihe von Faktoren zu berücksichtigenden Aspekte beim [Multitasking in Ihrer app Unterstützung](#Supporting-Multitasking-in-your-App), einschließlich:

- [Bildschirmgröße und-Ausrichtung](#Screen-Size-Considerations)
- [Benutzerdefinierte Tastenkombinationen in Visual Studio](#Custom-Hardware-Keyboard-Shortcuts)
- [Ressourcenverwaltung](#Resource-Management-Considerations)

Als app-Entwickler können Sie auch [Multitasking abwählen](#Opting-Out-of-Multitasking), einschließlich [deaktivieren PIP-Videowiedergabe](#Disabling-PIP-Video-Playback).

Dieser Artikel behandelt die erforderlichen Schritte zum sicherstellen, dass Ihre Xamarin.iOS-app wird in einem Multitaskingumgebung ordnungsgemäß ausgeführt oder wie Multitasking kündigen, ist dies nicht gut für Ihre app anpassen.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**Multitasking für iPad-video**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Multitasking-Schnellstart

Zur Unterstützung **Folie über** oder **geteilte Ansicht** Ihre app muss gehen Sie folgendermaßen vor:

- Für iOS 9 (oder höher) erstellt werden.
- Verwenden Sie ein Storyboard für seine Startbildschirm (und nicht Bildanlagen Sie).
- Verwenden Sie ein Storyboard mit Autolayout und Größenklassen, für die Benutzeroberfläche.
- Alle 4 iOS geräteausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts) zu unterstützen.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>Klicken Sie zum Multitasking für iPad

iOS 9 bietet neue Funktionen von Multitasking für iPad mit der Einführung von _Folie über_, _geteilte Ansicht_ (iPad Air 2, iPad Mini-4 und nur iPad Pro) und _Picture in Picture_. Wir werfe genauer an diese Funktionen in den folgenden Abschnitten.

<a name="Slide-Over" />

### <a name="slide-over"></a>Folie über

Das Feature Folie über ermöglicht den Benutzer eine zweite app auswählen und in einem kleinen gleitende Panel auf kurze Interaktion anzuzeigen. Der Bereich Folie über ist vorübergehend und wird geschlossen, wenn der Benutzer zurück zum Arbeiten mit der Haupt-app erneut wechselt.

[![](multitasking-images/about01.png "Der Bereich Folie über")](multitasking-images/about01.png#lightbox)

Das wichtigste daran ist, dass der Benutzer entscheidet, welche zwei apps ausgeführt werden Seite-an-Seite und der Entwickler keine Kontrolle über diesen Prozess verfügt. Daher sind einige Punkte, die Sie benötigen, um sicherzustellen, dass es sich bei Ihrer Xamarin.iOS-app in einem Panel Folie über ordnungsgemäß ausgeführt wird:

- **Verwenden Sie Automatisches Layout und die Größenklassen** – da es sich bei Ihrer Xamarin.iOS-app jetzt in der Folie hochskalierung Seitenleiste ausgeführt werden kann, Sie können nicht mehr verlassen auf dem Gerät, die Bildschirmgröße oder die Ausrichtung auf Layout die Benutzeroberfläche. Um sicherzustellen, dass Ihre app die Schnittstelle ordnungsgemäß skaliert wird, müssen Sie Automatisches Layout und die Größenklassen verwendet. Weitere Informationen finden Sie unserem [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
- **Ressourcen effizient verwenden** , da Ihre app jetzt das System mit einer anderen ausgeführten app Teilen kann, ist es wichtig, dass Ihre app Ressourcen effizient verwendet. Wenn Speicher mit geringer Dichte ist, endet das System automatisch die app, die am meisten Speicher belegen wird. Finden Sie unter Apple [Energie Effizienz-Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) Weitere Details.

Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4. Weitere Informationen zum Vorbereiten Ihrer app auf der Folie über finden Sie unter Apple [Einführung Multitasking Erweiterungen auf einem iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) Dokumentation.

<a name="Split-View" />

### <a name="split-view"></a>Geteilte Ansicht

Der Benutzer kann auf unterstützten iPad Hardware (iPad Air 2, iPad Mini-4 und iPad nur Pro) Wählen Sie eine zweite app und Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirms-Modus ausführen. Benutzer kann steuern, den Prozentsatz des Hauptbildschirms, die belegt, jede app durch Ziehen einer auf dem Bildschirm Unterteiler.

[![](multitasking-images/about02.png "Der geteilten Ansicht")](multitasking-images/about02.png#lightbox)

Wie schieben Sie auf der Benutzer entscheidet, welche zwei apps Seite-an-Seite ausgeführt werden soll, und in diesem Fall hat des Entwicklers keine Kontrolle über diesen Prozess. Daher stellt geteilte Ansicht ähnliche Anforderungen an die auf einer Xamarin.iOS-app:

- **Verwenden Sie Automatisches Layout und die Größenklassen** – da es sich bei Ihrer Xamarin.iOS-app jetzt in einem Split-Bildschirm-Modus auf die angegebene Größe des Benutzers ausgeführt werden kann, Sie können nicht mehr verlassen auf dem Gerät, die Bildschirmgröße oder die Ausrichtung auf Layout die Benutzeroberfläche. Um sicherzustellen, dass Ihre app die Schnittstelle ordnungsgemäß skaliert wird, müssen Sie Automatisches Layout und die Größenklassen verwendet. Weitere Informationen finden Sie unserem [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
- **Ressourcen effizient verwenden** , da Ihre app jetzt das System mit einer anderen ausgeführten app Teilen kann, ist es wichtig, dass Ihre app Ressourcen effizient verwendet. Wenn Speicher mit geringer Dichte ist, endet das System automatisch die app, die am meisten Speicher belegen wird. Finden Sie unter Apple [Energie Effizienz-Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) Weitere Details.

Weitere Informationen zum Vorbereiten Ihrer app für geteilte Ansicht finden Sie unter Apple [Einführung Multitasking Erweiterungen auf einem iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) Dokumentation.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Picture in Picture

Das neue Bild im Bild-Funktion (auch bekannt als _PIP_) ermöglicht dem Benutzer, ein Video in einem kleinen, unverankertes Fenster, die der Benutzer an einer beliebigen Stelle auf dem Bildschirm oben andere das Ausführen von apps positioniert werden kann.

[![](multitasking-images/about03.png "Ein Beispiel für Bild in Abbildung unverankerten Fenster")](multitasking-images/about03.png#lightbox)

Wie hat der Benutzer mit Folie über "und" geteilte Ansicht, vollständige Kontrolle über ein Video in der Abbildung im Bild-Modus. Wenn Ihrer app "main"-Funktion ist Video ansehen, benötigen sie einige Änderungen im PIP-Modus ordnungsgemäß verhält. Andernfalls werden keine Änderungen erforderlich, um PIP zu unterstützen.

Für Ihre app auf den PIP-Video zur Anforderung des Benutzers anzuzeigen, benötigen Sie entweder verwenden _AVKit_ oder _AV-Foundation-APIs_. Das Media Player-Framework wurde in iOS 9 abgeschrieben wurden und PIP nicht unterstützt.

Bild in Abbildung steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4. Weitere Informationen finden Sie unsere [PictureInPicture-Beispiel-App](https://developer.xamarin.com/samples/ios/iOS9/) und Apple [Bild im Bild Schnellstart](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) Dokumentation.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Unterstützung von Multitasking in Ihrer App

Für alle vorhandenen Xamarin.iOS-app ist die Multitasking unterstützen eine transparente Aufgabe, so lange Ihre app bereits Apple designleitfäden und bewährte Methoden für iOS 8 folgt. Dies bedeutet, dass die app Storyboards mit Autolayout und Größenklassen für seine Benutzeroberfläche Layouts verwendet werden sollte (finden Sie unter unserem [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Informationen).

Für diese apps können sind wenige oder gar keine Änderungen erforderlich, um die Multitasking unterstützen und auch darin verhält. Wenn Ihre app die Benutzeroberfläche ist, wurde erstellt mit anderen Methoden wie z. B. direkt Positionierung und größenanpassung von Elementen der Benutzeroberfläche in C# code oder basiert es auf Bildschirmgrößen bestimmten Gerät oder die Ausrichtungen, benötigen sie erhebliche Änderungen an iOS 9-Multitasking ordnungsgemäß zu unterstützen.

Um iOS 9-Multitasking auf alle neuen Xamarin.iOS-app zu unterstützen, verwenden Sie Storyboards mit Autolayout und Größenklassen für alle von der app-Benutzeroberfläche Layouts erneut, und implementieren Sie die Anweisungen in den folgenden Abschnitten an.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Bildschirmgröße und Ausrichtung Überlegungen

Vor iOS 9 können Sie die app anhand von bestimmten Gerät Bildschirmgrößen und-Ausrichtungen entwerfen. Da eine app nun in einem Panel Folie heraus oder im Modus für geteilte Ansicht ausgeführt werden kann, kann entweder in einer Klasse compact oder regulären horizontale Größe auf dem iPad, unabhängig von der das Gerät die physische Ausrichtung oder Bildschirm Größe ausgeführt gefunden werden.

[![](multitasking-images/sizeclasses01.png "Bildschirmgröße und Ausrichtung Überlegungen")](multitasking-images/sizeclasses01.png#lightbox)

Auf einem iPad hat eine Vollbild-Anwendung reguläre Klassen der horizontale und vertikale Größe. Alle iPhones, aber das iPhone 6 Plus und iPhone 6 s haben Sie darüber hinaus können Compact Größenklassen in beide Richtungen Ausrichtung. Das iPhone 6 Plus und iPhone 6 s Plus im Querformatmodus ausgeführt haben, einer normalen Klasse der horizontalen Größe und eine kompakte vertikale Größe-Klasse (ähnlich wie einem iPad Mini).

Auf iPads, die Folie über "und" geteilte Ansicht unterstützen, können Sie die folgenden Kombinationen Schluss:

| **Ausrichtung** | **Primären App** | **Sekundäre App** |
|--- |--- |--- |
| **Portrait** |75 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|25 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|
| **Querformat** |75 % des Bildschirms<br />Reguläre Horizontal<br />Reguläre vertikal|25 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|
| **Querformat** |50 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|50 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|

Im Beispiel [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) -app, wenn es auf einem iPad im Querformat "Vollbild" ausgeführt wird, wird er der Liste und aus der Detailansicht zur gleichen Zeit darstellen:

[![](multitasking-images/sizeclasses03.png "Die Liste und der Detailansicht, die gleichzeitig angezeigt")](multitasking-images/sizeclasses03.png#lightbox)

Wenn in einem Panel schieben Sie über die gleiche app ausgeführt wird, ist als Compact horizontale Größenklasse angeordnet, und zeigt nur die Liste:

[![](multitasking-images/sizeclasses04.png "Nur die Liste angezeigt, wenn das Gerät horizontalen Position befindet.")](multitasking-images/sizeclasses04.png#lightbox)

Um sicherzustellen, dass Ihre app richtig funktioniert in diesen Situationen, sollte Sie Trait-Sammlungen sowie Größenklassen übernehmen und entsprechen den `IUIContentContainer` und `IUITraitEnvironment` Schnittstellen. Finden Sie unter Apple [UITraitCollection Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) und unseren [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Anleitung finden Sie weitere Informationen.

Darüber hinaus wird Sie nicht mehr auf den Geräten Bildschirm Grenzen zum Definieren der app angezeigten Bereich verlassen können, müssen Sie Ihrer app Fenstergrenzen stattdessen zu verwenden. Da Fenstergrenzen vollständig unter der Kontrolle des Benutzers enthalten sind, nicht programmgesteuert passen Sie sie oder verhindern, dass den Benutzer diese Grenzen zu ändern.

Schließlich muss Ihre app eine Storyboard-Datei verwenden, zur Darstellung des Bildschirms starten, statt eine Reihe von **PNG** Bilddateien und unterstützen alle vier schnittstellenausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts) für die Ausführung in einem Panel Folie über oder im Modus für geteilte Ansicht berücksichtigt werden.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Benutzerdefinierte Tastenkombinationen in Visual Studio

In iOS 9 ausgeführt wird, auf einem iPad, Apple hat der erweiterte Support für Hardware-Tastaturen. iPads enthalten immer grundlegende externe Bildschirmtastatur-Unterstützung über Bluetooth und einigen Tastatur Herstellern erstellte Tastaturen, die verdrahteten iOS-spezifische Schlüssel enthalten.

Nun können apps in iOS 9, eigene benutzerdefinierte Tastenkombinationen erstellen. Darüber hinaus sind einige grundlegende Tastenkombinationen verfügbar, z. B. **Befehl-C-** (Kopieren), **Befehl-X** (Ausschneiden), **Befehl-v** (Einfügen) und **Befehl-Umschalt-H**  (home), ohne eine app wird insbesondere geschriebene Reaktion darauf.

**Registerkarte "von"** wird ein app-Switcher, die dem Benutzer ermöglicht, schnell wechseln zwischen apps über die Tastatur, ähnlich wie Mac OS angezeigt:

[![](multitasking-images/keyboard01.png "Der app-switcher")](multitasking-images/keyboard01.png#lightbox)

Wenn eine app für iOS 9 Tastenkombinationen in Visual Studio enthält, die Benutzer halten und auf die **Befehl**, **Option** oder **Steuerelement** Schlüssel, um sie in einem Popupfenster anzuzeigen:

[![](multitasking-images/keyboard02.png "Die Pop-up mit Tastenkombinationen")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Definieren von benutzerdefinierten Tastenkombinationen

Wenn wir fügen den folgenden Code auf eine Sicht oder View Controller in unserer app ab, wenn diese Ansicht bzw. Domänencontroller angezeigt wird, wird eine benutzerdefinierte Tastenkombination verfügbar sein:

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

Zunächst überschreiben die `CanBecomeFirstResponder` Eigenschaft und die Rückgabewerte `true` damit die Ansicht oder View Controller Tastatureingaben empfangen kann. 

Als Nächstes überschreiben wir die `KeyCommands` Eigenschaft und erstellen Sie ein neues `UIKeyCommand` für die **Befehl-N** Tastatureingabe. Wenn die Tastatureingabe aktiviert ist, rufen wir die `NewEntry` Methode (, die wir mit iOS 9 verfügbar machen die `Export` Befehl), die angeforderte Aktion auszuführen.

Wenn das Ausführen dieser app auf einem iPad mit einem Hardwaretastatur und der Benutzer eingibt **Befehl-n**, ein neuer Eintrag wird zur Liste hinzugefügt werden. Wenn der Benutzer auf gedrückt hält die **Befehl** Schlüssel, die Liste der Verknüpfungen werden angezeigt:

[![](multitasking-images/keyboard03.png "Die Pop-up mit Tastenkombinationen")](multitasking-images/keyboard03.png#lightbox)

Informieren Sie sich das Beispiel [Multitasking app](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) eine beispielimplementierung.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Überlegungen zur Verwaltung von Ressourcen

Auch für apps, die bereits iOS 8 durch seine designleitfäden und bewährte Methoden verwenden, möglicherweise effizienter ressourcenverwaltung weiterhin ein Problem. In iOS 9 müssen apps nicht mehr die exklusive Verwendung des Arbeitsspeichers, CPU oder andere Systemressourcen.

Daher müssen Sie Ihre Xamarin.iOS-app, um Systemressourcen effektiv verwenden optimieren, oder sie Gesichter Beendigung in Situationen mit wenig Arbeitsspeicher. Dies gilt gleichermaßen für apps, die Multitasking deaktivieren, da eine Sekunde möglicherweise app weiterhin in eine Folie über Bereich oder ein Bild im Bild-Fenster, erfordern zusätzliche Ressourcen oder die Häufigkeit der Aktualisierung verursachen, die Verfügbarkeit von weniger als 60 Bilder pro Sekunde ausgeführt werden.

Betrachten Sie die folgenden Benutzeraktionen und deren Auswirkungen:

- **Eingeben von Text in einem Panel Folie über** -selbst wenn Ihre app keine Texteingabe enthält, die Systemtastatur kann jetzt angezeigt werden über die Benutzeroberfläche. Daher müssen die app auf der Tastatur anzeigebenachrichtigungen (z. B. das Anzeigen und Ausblenden der Tastaturfokus) zu reagieren.
- **Ausführung einer zweiten App in einem Panel Folie über** – die neue app wird jetzt im Vordergrund ausgeführt und mit der vorhandenen app für Systemressourcen wie Arbeitsspeicher und CPU-Zyklen.
- **Wiedergeben eines Videos in einem Fenster PIP** – nicht nur dieses Fenster zu Teil Ihrer app-Schnittstelle überdecken kann, aber die app, die das Video gestartet weiterhin im Hintergrund ausgeführt und Nutzen von CPU-und Arbeitsspeicherressourcen.

Um sicherzustellen, dass Ihre app Ressourcen effizient verwendet, sollten Sie folgendermaßen vor:

- **Profilerstellung für die App mit Instruments** -überprüfen Sie, ob Speicherverluste, offenkundig CPU-Auslastung und Bereiche, in dem die app möglicherweise werden der Hauptthread blockiert.
- **Auf Zustand Übergänge Methoden zu reagieren** – klicken Sie im Ihre **Datei "appdelegate.cs"** Datei außer Kraft setzen und die Antwort auf Status ändern Sie Methoden wie z. B. die app eingeben oder im Hintergrund zurückgeben. Veröffentlichen Sie alle nicht erforderlichen Ressourcen wie Bilder, Daten oder Ansichten und ansichtscontroller.
- **Testen Sie die Seite-an-Seite mit grafikintensiven Apps Arbeitsspeicher** : Ausführen der app, die einer rechenintensiven app Speicher wie z. B. Zuordnungen (im Modus der Satelliten-Ansicht) mithilfe von Folie, und geteilte Ansicht auf physischen iOS-Hardware, und Testen Sie, dass beide apps reaktionsfähig bleiben, und führen Sie nicht abstürzt.

Finden Sie unter Apple [Energie Effizienz-Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) für Weitere Informationen zu Resource Manager.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Deaktivierung der Multitasking

Beim Apple empfiehlt, dass alle apps für iOS 9 Multitasking unterstützen, unter Umständen sehr spezifische Gründe für eine app nicht zu, wie z. B. Spiele oder Kamera-apps, die den gesamten Bildschirm ordnungsgemäße erfordern.

Bearbeiten Sie für Ihre Xamarin.iOS-app zu deaktivieren, die ausgeführt wird, entweder eine Folie, im Bedienfeld "oder im Modus für geteilte Ansicht des Projekts **" Info.plist "** Datei, und überprüfen Sie **Vollbild erfordert**:

[![](multitasking-images/fullscreen01.png "Deaktivierung der Multitasking")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> Während der Deaktivierung der Multitasking Ihrer app wird verhindert, dass in schieben, oder die geteilte Ansicht ausgeführt werden, verhindert es einer anderen Anwendung nicht schieben, oder ein Bild in Abbildung video ausgeführt wird, zusammen mit Ihrer app angezeigt.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>Deaktivieren PIP-Videowiedergabe

In den meisten Fällen sollte Ihre app können Benutzer, alle video Inhalte wiederzugeben, die in einem Bild in Abbildung unverankerten Fenster angezeigt. Möglicherweise gibt es jedoch Situationen, in denen dies nicht, wie z. B. Spiele Ausschneiden Szene Videos erwünscht sein kann.

Um die Videowiedergabe PIP kündigen möchten, gehen Sie in Ihrer app:

- Bei Verwendung einer `AVPlayerViewController` um Video anzuzeigen, legen die `AllowsPictureInPicturePlayback` Eigenschaft `false`.
- Bei Verwendung der `AVPlayerLayer` zum Anzeigen von Video nicht instanziieren einer `AVPictureInPictureController`.
- Bei Verwendung einer `WKWebView` um Video anzuzeigen, legen die `AllowsPictureInPictureMediaPlayback` Eigenschaft `false`.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die erforderlichen Schritte zum sicherstellen, dass eine Xamarin.iOS-app ausgeführt wird und in iOS 9 neue Multitasking-Fähigkeit für iPads, verhalten sich korrekt behandelt. Darüber hinaus behandelt diese Entscheidung-Out-of Multitasking für apps, wenn es nicht gut geeignet ist.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Multitasking (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Einführung in die einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Einführung von Verbesserungen für Multitasking auf dem iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [Blogbeitrag](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
