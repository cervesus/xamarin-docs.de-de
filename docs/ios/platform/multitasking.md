---
title: "Multitasking für iPad"
description: "iOS 9 unterstützt zwei Web-apps auf der gleichen Zeit, Folie über oder geteilten Ansicht ausgeführt wird. Darüber hinaus wird die Wiedergabe im Bild Video unterstützt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 34b51f784b549caa0dda2eeda066bb39dfc13020
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="multitasking-for-ipad"></a>Multitasking für iPad

_iOS 9 unterstützt zwei Web-apps auf der gleichen Zeit, Folie über oder geteilten Ansicht ausgeführt wird. Darüber hinaus wird die Wiedergabe im Bild Video unterstützt._

![](multitasking-images/about02-sml.png "Bildschirm Beispiel teilen") ![ ] (multitasking-images/about03-sml.png "Bild-Beispiel")

iOS 9 fügt Multitasking-Unterstützung für die Ausführung von zwei Web-apps zur gleichen Zeit auf bestimmte iPad-Hardware. Multitasking für iPad wird über die folgenden Funktionen unterstützt:

- [**Folie über** ](#Slide-Over) -ermöglicht es dem Benutzer eine zweite iOS-app vorübergehend in eine Folie out Bereich (entweder auf der linken oder rechten Seite des Bildschirms basierend auf Sprache Richtung) ausgeführt wird, die etwa 25 % der Haupt-app derzeit ausgeführt abdeckt. Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4.
- [**Geteilte Ansicht** ](#Split-View) – der Benutzer kann auf unterstützten iPad Hardware (iPad iPad Mini-4 und iPad Pro nur per Funk 2) Wählen Sie eine zweite-app und führen Sie es Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirmmodus. Der Benutzer kann den Anteil der Hauptbildschirm steuern, die jede app belegt.
- [**Bild im Bild** ](#Picture-in-Picture) – für apps, die Wiedergabe Videoinhalten, das video kann nun in einem Fenster verschiebbares und in der Größe veränderbaren abgespielt werden, die über die andere apps, die derzeit auf dem iOS-Gerät ausgeführt: verschiebt. Der Benutzer hat die vollständige Kontrolle über die Größe und Position des Fensters. Bild im Bild sind nur ein iPhone Pro iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3 oder iPad Mini-4 verfügbar.

Es gibt eine Anzahl von zu berücksichtigenden Aspekte [Multitasking in Ihrer app unterstützen](#Supporting-Multitasking-in-your-App), einschließlich:

- [Bildschirmgröße und Ausrichtung](#Screen-Size-Considerations)
- [Benutzerdefinierte Tastenkombinationen](#Custom-Hardware-Keyboard-Shortcuts)
- [Ressourcenverwaltung](#Resource-Management-Considerations)

App-Entwickler können Sie auch [opt-Out-of Multitasking](#Opting-Out-of-Multitasking), einschließlich [deaktivieren PIP Videowiedergabe](#Disabling-PIP-Video-Playback).

In diesem Artikel werden die erforderlichen Schritte zum sicherstellen, dass Ihre app Xamarin.iOS wird in einem Multitaskingumgebung ordnungsgemäß ausgeführt oder wie Multitasking abzuwählen, wenn es sich nicht um eine gute ist zu, für Ihre app groß behandelt.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**Multitasking für iPad, indem [Xamarin University](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Multitasking-Schnellstart

Zur Unterstützung **Folie über** oder **geteilten Ansicht** Ihrer app müssen folgende:

 - Für iOS 9 (oder höher) erstellt werden.
 - Verwenden Sie ein Storyboard für seine Bildschirm starten (und nicht image Bestand).
 - Verwenden Sie ein Storyboard mit Autolayout und Größenklassen für die Benutzeroberfläche.
 - Alle 4 iOS-Gerät Ausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts) zu unterstützen.

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>Zum Multitasking für iPad

iOS 9 bietet die neuen Funktionen von Multitasking auf dem iPad durch die Einführung des _Folie über_, _geteilten Ansicht_ (iPad Mini-4-iPad und iPhone Pro nur per Funk 2) und _Bild im Bild_. Wir führen näher auf diese Funktionen in den folgenden Abschnitten.

<a name="Slide-Over" />

### <a name="slide-over"></a>Folie über

Das Feature Folie über ermöglicht dem Benutzer eine zweite app auswählen und in einem kleinen gleitende Bereich auf schnelle Interaktion anzeigt. Das Panel Folie über ist temporär und wird geschlossen, wenn der Benutzer wechselt zurück zum Arbeiten mit der Haupt-app erneut aus.

[![](multitasking-images/about01.png "Das Panel Folie über")](multitasking-images/about01.png#lightbox)

Die wichtigste daran ist, dass der Benutzer entscheidet, welche zwei Web-apps-Seite-an-Seite und hat der Entwickler keine Kontrolle über diesen Prozess ausgeführt werden. Daher sind einige Punkte, die Sie benötigen, um sicherzustellen, dass Ihre app Xamarin.iOS in einem Bereich Folie über ordnungsgemäß ausgeführt wird:

- **Verwenden Sie Autolayout und Größenklassen** – da Xamarin.iOS app jetzt im Bereich Folie horizontaler Seiten ausgeführt werden kann, Sie können nicht mehr verwenden, auf dem Gerät, dessen Bildschirmgröße oder die Ausrichtung zu Layout die Benutzeroberfläche. Um sicherzustellen, dass die app der Schnittstelle ordnungsgemäß skaliert, müssen Sie Autolayout und Größenklassen zu verwenden. Weitere Informationen finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
- **Verwenden Sie Ressourcen effizient** – da Ihre app jetzt das System mit einem anderen ausgeführten app freigeben kann, ist wichtig, dass Ihre app Systemressourcen effizient verwendet. Wenn Speicher mit geringer Dichte wird, wird das System automatisch die app beendet, die den meisten Speicherplatz in Anspruch nimmt. Finden Sie in der Apple- [Energie Effizienz Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) Weitere Details.

Schieben Sie über steht nur auf einem iPad Pro, iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3, oder iPad Mini-4. Weitere Informationen zum Vorbereiten Ihrer app auf Folie über finden Sie unter der Apple- [einführen Multitasking Verbesserungen auf iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) Dokumentation.

<a name="Split-View" />

### <a name="split-view"></a>Geteilte Ansicht

Der Benutzer kann auf unterstützten iPad Hardware (iPad iPad Mini-4 und iPad Pro nur per Funk 2) Wählen Sie eine zweite app und führen Sie es Seite-an-Seite mit der aktuell ausgeführten app in einem geteilten Bildschirmmodus. Der Benutzer kann den Anteil der Hauptbildschirm, die jede app durch Ziehen belegt steuern eine auf dem Bildschirm Unterteiler.

[![](multitasking-images/about02.png "Die geteilte Ansicht")](multitasking-images/about02.png#lightbox)

Der Benutzer entscheidet, welche zwei Web-apps-Seite-an-Seite ausgeführt werden und auch der Entwickler hat keinen Einfluss auf diesen Prozess, wie Folie über. Daher stellt geteilten Ansicht ähnlich wie Anforderungen an die in einem Xamarin.iOS-app:

- **Verwenden Sie Autolayout und Größenklassen** – da Xamarin.iOS app nun in einem geteilten Bildschirmmodus zur angegebenen Größe des Benutzers ausgeführt werden kann, Sie können nicht mehr verwenden, auf dem Gerät, dessen Bildschirmgröße oder die Ausrichtung zu Layout die Benutzeroberfläche. Um sicherzustellen, dass die app der Schnittstelle ordnungsgemäß skaliert, müssen Sie Autolayout und Größenklassen zu verwenden. Weitere Informationen finden Sie in unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.
- **Verwenden Sie Ressourcen effizient** – da Ihre app jetzt das System mit einem anderen ausgeführten app freigeben kann, ist wichtig, dass Ihre app Systemressourcen effizient verwendet. Wenn Speicher mit geringer Dichte wird, wird das System automatisch die app beendet, die den meisten Speicherplatz in Anspruch nimmt. Finden Sie in der Apple- [Energie Effizienz Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) Weitere Details.

Weitere Informationen zum Vorbereiten Ihrer app für geteilte Ansicht finden Sie unter der Apple- [einführen Multitasking Verbesserungen auf iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) Dokumentation.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Bild im Bild

Das neue Bild im Bild-Funktion (auch bekannt als _PIP_) ermöglicht es dem Benutzer ein Video in einem kleinen schwebenden Fenster überwachen, die der Benutzer an einer beliebigen Stelle auf dem Bildschirm über andere ausgeführte apps positionieren kann.

[![](multitasking-images/about03.png "Ein Beispiel für Bild in unverankerte Fenster "Bild"")](multitasking-images/about03.png#lightbox)

Als hat der Benutzer mit Folie über und geteilten Ansicht volle Kontrolle über ein Video in der Abbildung im Bild Modus beobachten. Ist Ihre app Hauptfunktion Video ansehen, benötigen sie einige Änderungen im PIP-Modus ordnungsgemäß verhält. Andernfalls werden keine Änderungen erforderlich, zur Unterstützung von PIP.

Damit Ihre app PIP Video über die Anforderung des Benutzers anzuzeigen, müssen Sie entweder verwenden _AVKit_ oder _AV Foundation APIs_. Das Media Player Framework wurde in iOS 9 Abschreibung wurde und PIP nicht unterstützt.

Bild im Bild sind nur ein iPhone Pro iPad Air, iPad Air-2, iPad Mini-2, iPad Mini-3 oder iPad Mini-4 verfügbar. Weitere Informationen finden Sie unter unsere [PictureInPicture Beispiel-App](https://developer.xamarin.com/samples/ios/iOS9/) und Apple [Bild in Schnellstart Bild](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) Dokumentation.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Unterstützen von Multitasking in Ihrer App

Für alle bisherigen Xamarin.iOS app ist die Unterstützung von Multitasking eine transparente Aufgabe, wie Ihre app bereits Apple Entwurf Anleitungen und bewährte Methoden für iOS 8 folgt. Dies bedeutet, dass die app Storyboards mit Autolayout und Größenklassen für die Benutzeroberfläche Layouts verwendet werden sollte (finden Sie unter unsere [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) für Weitere Informationen).

Für diese apps sind nur wenig oder keine Änderungen erforderlich, damit Multitasking unterstützt und in ihr Verhalten. Wenn Ihre app-Benutzeroberfläche erstellt wurde, mit anderen Methoden wie z. B. direkt Position und Größe der Benutzeroberflächenelemente im C#-Code oder für bestimmte Geräte Bildschirmgrößen oder Ausrichtungen basiert, benötigen ihn signifikante Änderung iOS 9-Multitasking korrekt unterstützt werden.

Unterstützung von iOS 9-Multitasking auf alle neuen Xamarin.iOS app erneut verwenden Sie Storyboards mit Autolayout und Größenklassen für alle von der app-Benutzeroberfläche Layouts und implementieren Sie die Anweisungen in den folgenden Abschnitten.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Bildschirmgröße und Ausrichtung Überlegungen

Bevor Sie iOS 9 konnte Sie Ihrer app anhand der bestimmten Gerät Bildschirmgrößen und Ausrichtungen entwerfen. Da eine app nun in einem Bereich Folie Out oder in Teilen Ansichtsmodus ausgeführt werden kann, kann sich entweder in einer Klasse compact oder reguläre horizontale Größe auf iPad, unabhängig von der physischen Ausrichtung oder Bildschirm Größe des Geräts ausgeführt gefunden werden.

[![](multitasking-images/sizeclasses01.png "Bildschirmgröße und Ausrichtung Überlegungen")](multitasking-images/sizeclasses01.png#lightbox)

Auf einem iPad hat eine Vollbild-Anwendung reguläre horizontale und vertikale Größe-Klassen. Alle iPhones, aber das iPhone 6 Plus und iPhone 6 s Plus, haben Sie kompakt Klassen in beide Richtungen Ausrichtung. Das iPhone 6 Plus und iPhone 6 s Plus im Querformat haben eine reguläre horizontale Größe-Klasse und einer Compact vertikale Größe-Klasse (ähnlich wie bei einem iPad Mini).

Auf iPads, die Folie über und geteilte Ansicht unterstützen, können Sie die folgenden Kombinationen Schluss:

| **Ausrichtung** | **Primäre App** | **Sekundäre App** |
|--- |--- |--- |
| **Portrait** |75 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|25 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|
| **Querformat** |75 % des Bildschirms<br />Reguläre Horizontal<br />Reguläre vertikal|25 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|
| **Querformat** |50 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|50 % des Bildschirms<br />Compact Horizontal<br />Reguläre vertikal|

Im Beispiel [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) app, wenn er Vollbild auf einem iPad im Querformat-Modus ausgeführt wird, wird es der Liste und die Detailansicht gleichzeitig vorhanden:

[![](multitasking-images/sizeclasses03.png "Die Liste und der Detailansicht angezeigt, die zur gleichen Zeit")](multitasking-images/sizeclasses03.png#lightbox)

Wenn in einem Bereich schieben Sie über die gleiche app ausgeführt wird, wird als Compact horizontale Größe Klasse angeordnet und zeigt nur die Liste:

[![](multitasking-images/sizeclasses04.png "Nur der Liste angezeigt, wenn das Gerät horizontale ist")](multitasking-images/sizeclasses04.png#lightbox)

Um sicherzustellen, dass Ihre app in diesen Situationen ordnungsgemäß verhält, sollten Sie übernehmen Merkmals Sammlungen zusammen mit der Größe von Klassen und entsprechen dem `IUIContentContainer` und `IUITraitEnvironment` Schnittstellen. Finden Sie in der Apple- [UITraitCollection-Klassenreferenz](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) und unseren [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Weitere Informationen.

Darüber hinaus können Sie nicht mehr beruhen auf den Grenzen des Geräte-Bildschirm sichtbaren Bereich der app zu definieren, Sie müssen stattdessen Fenstergrenzen Ihrer app verwenden. Da Fenstergrenzen vollständig unter der Kontrolle des Benutzers sind, nicht programmgesteuert anzupassen oder verhindern, dass der Benutzer diese Grenzen zu ändern.

Schließlich muss Ihrer app eine Storyboarddatei verwenden, um seine Bildschirm starten, im Gegensatz zur Verwendung der eines Satzes von präsentieren **PNG** Bilddateien und unterstützt alle vier Schnittstelle Ausrichtungen (Hochformat, nach unten zeigende Hochformat, Querformat links und Querformat rechts) für die Ausführung in einem Bereich Folie über oder im Modus für geteilte Ansicht berücksichtigt werden.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Benutzerdefinierte Tastenkombinationen

In iOS 9 ausgeführt wird, auf einem iPad, Apple verfügt über erweiterte Unterstützung für Hardware Tastaturen. iPads einbezogen haben immer grundlegende externe Tastaturfunktionen über Bluetooth und einige Tastatur Hersteller erstellt Tastaturen, die drahtgebundenen iOS-spezifische Schlüssel enthalten.

Jetzt können apps mit iOS 9, eigene benutzerdefinierte Tastenkombinationen erstellen. Darüber hinaus sind einige grundlegende Tastenkombinationen verfügbar wie **Befehl-c** (Kopieren), **Befehl X** (cut) **Befehl V** (Einfügen) und **Befehl-Umschalt-H**  (home), ohne eine app wird auf sie ausdrücklich schriftliche reagieren.

**– Registerkarte "Befehl"** wird ein app-Switcher, die dem Benutzer ermöglicht, wechseln Sie schnell zwischen apps über die Tastatur, ähnlich wie die Mac OS bringen:

[![](multitasking-images/keyboard01.png "Der Switcher app")](multitasking-images/keyboard01.png#lightbox)

Wenn eine app für iOS 9 Tastenkombinationen enthält, die Benutzer kann halten Sie auf der **Befehl**, **Option** oder **Steuerelement** Schlüssel, um sie in einem Popupfenster anzuzeigen:

[![](multitasking-images/keyboard02.png "Die Tastatur Tastenkombinationen-popup")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Definieren von Tastenkombinationen

Wenn wir fügen den folgenden Code auf eine Sicht oder View Controller in dieser app ab, wenn diese Ansicht bzw. Domänencontroller angezeigt wird, wird eine benutzerdefinierte Tastenkombination verfügbar sein:

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

Zuerst, überschreiben die `CanBecomeFirstResponder` Eigenschaft und die Rückgabewerte `true` damit die Ansicht bzw. View Controller Tastatureingaben empfangen kann. 

Als Nächstes überschreiben die `KeyCommands` Eigenschaft und erstellen Sie ein neues `UIKeyCommand` für die **Befehl N** Tastatureingabe. Wenn die Tastatureingabe aktiviert ist, nennen wir die `NewEntry` Methode (, die wir mit iOS 9 verfügbar machen die `Export` Befehl), die angeforderte Aktion auszuführen.

Wenn beim Ausführen dieser app auf einem iPad mit einer Hardwaretastatur angefügt und vom Benutzer eingegebenen **Befehl N**, ein neuer Eintrag wird zur Liste hinzugefügt werden. Wenn der Benutzer auf gedrückt hält die **Befehl** Schlüssel, die Liste der Verknüpfungen wird angezeigt:

[![](multitasking-images/keyboard03.png "Die Tastatur Tastenkombinationen-popup")](multitasking-images/keyboard03.png#lightbox)

Sie finden Sie im Beispiel [Multitasking app](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) eine Beispiel-Implementierung.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Überlegungen zur Verwaltung von Ressourcen

Auch für apps, die bereits iOS 8 des Entwurfs von Führungslinien und bewährte Methoden verwenden, möglicherweise effizienter ressourcenverwaltung weiterhin ein Problem. In iOS 9 müssen die apps nicht mehr ausschließliche Verwendung von Arbeitsspeicher, CPU oder andere Systemressourcen.

Daher müssen Sie Ihre app Xamarin.iOS, um die effektive Verwendung von Systemressourcen optimieren oder gewinnen Beendigung unter unzureichendem Arbeitsspeicher. Dies gilt ebenso für apps, die von Multitasking teilnehmen, seit ein zweites möglicherweise app weiterhin in einer Folie über Bereich oder ein Bild im Fenster des Bild, erfordern zusätzliche Ressourcen oder die Aktualisierungsrate verursacht unter 60 Frames pro Sekunde fällt ausgeführt werden.

Betrachten Sie die folgenden Benutzeraktionen und deren Bedeutung:

- **Eingeben von Text in einem Bereich Folie über** – selbst wenn Ihre app keine Texteingabe enthält, die Tastatur System kann jetzt angezeigt werden über die Benutzeroberfläche. Daher müssen die app So reagieren Sie auf der Tastatur anzeigebenachrichtigungen (z. B. ein- und Ausblenden von der Tastatur).
- **Ausführen einer zweiten App in einem Bereich Folie über** -die neue app wird jetzt im Vordergrund ausgeführt und mit der vorhandenen app für Systemressourcen wie Arbeitsspeicher und CPU-Zyklen konkurrieren.
- **Wiedergabe eines Videos in einem Fenster PIP** – nicht nur in diesem Fenster Teil der app-Schnittstelle deckt, aber die app, die das Video gestartet ist weiterhin im Hintergrund ausgeführt und CPU-und Arbeitsspeicherressourcen consuming.

Um sicherzustellen, dass Ihre app Ressourcen effizient verwendet, sollten Sie Folgendes tun:

- **Profilerstellung für die App mit den Instrumenten** -prüfen auf Speicherverluste, bei einer offenkundig CPU-Auslastung und Bereiche, in denen die app den Haupt-Thread blockieren.
- **Reagieren auf Status Übergänge Methoden** – In Ihre **AppDelegate.cs** Datei Override "und" Antwort auf Status ändern Sie Methoden wie z. B. die app eingeben oder aus Hintergrund zurückgegeben. Lassen Sie alle nicht benötigten Ressourcen wie z. B. Bilder, Daten oder Sichten und View-Controller.
- **Testen Sie die Seite-an-Seite mit Speicher anspruchsvolle Apps** - Ausführen der app eine Arbeitsspeicher-rechenintensive app wie z. B. Zuordnungen (in Satelliten-Ansichtsmodus) Folie Out und geteilter Ansicht, die auf physischen e Hardware mit und Testen Sie, dass beide apps weiterhin auf Benutzerinteraktionen reagieren und nicht stürzt ab.

Finden Sie in der Apple- [Energie Effizienz Handbuch für iOS-Apps](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) für Weitere Informationen zur ressourcenverwaltung von.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Die Entscheidung Out-of Multitasking

Während Apple bereits vermuten lässt, dass alle apps für iOS 9 Multitasking unterstützen, kann es sehr besondere Gründe für eine app nicht zu z. B. Spiele oder apps, die Kamera, die den gesamten Bildschirm ordnungsgemäß funktioniert erfordern.

Damit Ihre app Xamarin.iOS opt-Out-of entweder mit einer Folie Out Bereich als oder geteilten Ansicht Modus ausgeführt wird, bearbeiten Sie des Projekts **"Info.plist"** Datei, und überprüfen Sie **Vollbild erfordert**:

[![](multitasking-images/fullscreen01.png "Die Entscheidung Out-of Multitasking")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> **Hinweis:** während Opting-Out-of Multitasking verhindert, dass der Ihrer app in Folie Out "oder" geteilte Ansicht ausgeführt wird, ist es **nicht** verhindern, dass eine andere app ausgeführt schieben, oder ein Bild im Bild Video anzeigt, die zusammen mit Ihrer App.




<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>Deaktivieren PIP-Videowiedergabe

In den meisten Fällen sollte die app können Benutzer alle Videoinhalten wiedergegeben, die in einem Bild im unverankerte Fenster "Bild" angezeigt. Möglicherweise gibt es jedoch Situationen, in denen dies nicht, wie z. B. Spiel ausgeschnittene Szene Videos erwünscht sein kann.

Um der Videowiedergabe PIP teilnehmen, gehen Sie in Ihrer app:

 - Bei Verwendung einer `AVPlayerViewController` Video anzeigen möchten, legen Sie die `AllowsPictureInPicturePlayback` Eigenschaft, um `false`.
 - Bei Verwendung der `AVPlayerLayer` um Video anzuzeigen, nicht instanziieren einer `AVPictureInPictureController`.
 - Bei Verwendung einer `WKWebView` Video anzeigen möchten, legen Sie die `AllowsPictureInPictureMediaPlayback` Eigenschaft, um `false`.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die erforderlichen Schritte zum sicherstellen, dass eine Xamarin.iOS app ausgeführt wird und ordnungsgemäß in iOS 9 neue Multitasking-Fähigkeit für iPads behandelt. Darüber hinaus behandelt diese Entscheidung Out-of Multitasking für apps, wenn er nicht gut geeignet ist.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Multitasking (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Einführung in die einheitliche Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Einsetzen von Erweiterungen für Multitasking auf iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [Blogbeitrag](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
