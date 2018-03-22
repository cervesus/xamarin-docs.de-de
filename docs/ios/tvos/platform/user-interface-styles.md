---
title: Neuer Benutzer Schnittstelle Stile
description: "Dieser Artikel behandelt das Licht und dunkel UI-Designs, Apple tvos. außerdem wurden 10 und wie diese in einer app Xamarin.tvOS implementiert hinzugefügt wurde."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e400a72f4c759662e70bfecc372134f8fda05ad6
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="new-user-interface-styles"></a>Neuer Benutzer Schnittstelle Stile

_Dieser Artikel behandelt das Licht und dunkel UI-Designs, Apple tvos. außerdem wurden 10 und wie diese in einer app Xamarin.tvOS implementiert hinzugefügt wurde._

tvos. außerdem wurden 10 nun unterstützt einen dunklen und die Benutzeroberfläche Licht Design, dass alle von der Build in UIKit Steuerelemente wird automatisch angepasst, basierend auf die Einstellungen des Benutzers. Darüber hinaus wird der Entwickler kann manuell anpassen, Benutzeroberflächenelemente auf der Grundlage des Designs, die der Benutzer ausgewählt hat und einem bestimmten Design überschreiben kann.


<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Zu den neuen Benutzer-Schnittstelle

Wie bereits erwähnt, basierend tvos. außerdem wurden 10 nun unterstützt einen dunklen und die Benutzeroberfläche Licht Design, dass alle von der Build in UIKit Steuerelemente wird automatisch angepasst, auf die Einstellungen des Benutzers ab.

Benutzer kann dieses Design wechseln, navigieren Sie zu **Einstellungen** > **allgemeine** > **Darstellung** und Wechseln zwischen **Licht**  und **dunkel**:

[![](user-interface-styles-images/theme01.png "Einstellungs-app")](user-interface-styles-images/theme01.png#lightbox)

Wenn die **dunkel** Design ausgewählt ist, alle Elemente der Benutzeroberfläche, wechselt zur hellen Text auf einem dunklen Hintergrund:

[![](user-interface-styles-images/theme02.png "Das Design "dunkel"")](user-interface-styles-images/theme02.png#lightbox)

Der Benutzer hat die Möglichkeit, wechseln Sie zu einem beliebigen Zeitpunkt das Design und empfiehlt sich daher basierend auf der aktuellen Aktivität, auf dem sich das Apple TV befindet oder die Uhrzeit.

Das Licht UI Design ist das Standarddesign und tvos. außerdem wurden vorhandene apps werden das Design "hell", unabhängig von der die Einstellungen des Benutzers, weiterhin verwendet, es sei denn, sie für tvos. außerdem wurden 10, um das Design "dunkel" nutzen geändert werden. Eine tvos. außerdem wurden 10-app hat auch die Möglichkeit, das aktuelle Design überschreiben und verwenden Sie Design "hell oder dunkel" für einige oder alle ihre Benutzeroberfläche immer.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Übernehmen die helle und dunkle Designs

Apple fügte zur Unterstützung dieser Funktion eine neue API, um die `UITraitCollection` Klasse und einer app tvos. außerdem wurden müssen teilnehmen zur Unterstützung der dunkel Darstellung (über eine Einstellung in seine `Info.plist` Datei).

Um zur Unterstützung von hellen und dunklen Design teilnehmen können, führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. Wählen Sie die **Quelle** Ansicht (von unten im Editor).
3. Fügen Sie einen neuen Schlüssel hinzu, und nennen Sie es `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "Der Schlüssel UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Lassen Sie die Typ `String` und geben Sie einen Wert von `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Geben Sie die automatische")](user-interface-styles-images/theme04.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

Es gibt drei mögliche Werte für die `UIUserInterfaceStyle` Schlüssel:

- **Light** -erzwingt tvos. außerdem wurden der Benutzeroberfläche der Anwendung immer die Design "hell" verwenden.
- **Dunkle** -erzwingt tvos. außerdem wurden der Benutzeroberfläche der Anwendung immer die Design "dunkel" verwenden.
- **Automatische** -wechselt zwischen dem hellen und dunklen Design auf der Grundlage von bevorzugten in den Einstellungen des Benutzers. Dies ist die bevorzugte Einstellung.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit Design-Unterstützung

Wenn eine tvos. außerdem wurden-app, standard, integrierte verwendet wird `UIView` -basierte Steuerelemente, sie werden automatisch zum UI-Design ohne Eingriff des Entwicklers reagieren.

Darüber hinaus `UILabel` und `UITextView` ändert sich automatisch auf ihre Farbe basiert auf dem select-UI-Design:

- Der Text wird in das Design "hell" black sein.
- Der Text wird in das Design "dunkel" weiß sein.

Wenn der Entwickler die Textfarbe manuell (entweder in die Storyboard oder den Code) geändert, werden sie für die Handhabung von farbänderungen basierend auf dem UI-Design verantwortlich.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Neue Blur Effekte

Für die Unterstützung der hellen und dunklen Design in einer tvos. außerdem wurden 10-app, hinzugefügt Apple zwei neue Weichzeichnen Effekte. Diese neue Effekte das Blur basierend auf der UI-Design, das wie folgt der Benutzer ausgewählt hat automatisch angepasst:

- `UIBlurEffectStyleRegular` -Verwendet eine helle Weichzeichnen in das Design "hell" und einem dunklen Weichzeichnen im dunklen Design.
- `UIBlurEffectStyleProminent` -Eine sehr einfache Blur in das Design "hell" und ein sehr dunkel Blur in das Design "dunkel" verwendet.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Arbeiten mit Auflistungen Merkmal ""

Die neue `UserInterfaceStyle` Eigenschaft von der `UITraitCollection` Klasse kann verwendet werden, um das aktuell ausgewählte UI Design erhalten und werden eine `UIUserInterfaceStyle` -Enumeration der einen der folgenden Werte:

- **Light** -Design "die Benutzeroberfläche des Licht" ausgewählt ist.
- **Dunkle** -die Benutzeroberfläche des dunklen Design ausgewählt ist.
- **Nicht angegebener** -Ansicht hat noch auf dem Bildschirm nicht angezeigt wurde, daher ist die aktuelle UI-Design unbekannt ist.

Darüber hinaus bieten Auflistungen Merkmal "" die folgenden Features in tvos. außerdem wurden 10:
 
- Proxy Darstellung angepasst werden basierend auf der `UserInterfaceStyle` von einer angegebenen `UITraitCollection` zu ändern, z. B. Bilder oder Element Farben auf Grundlage des Designs.
- Eine app tvos. außerdem wurden kann Merkmals sammlungsänderungen behandeln, durch Überschreiben der `TraitCollectionDidChange` Methode von einer `UIView` oder `UIViewController` Klasse.

> [!IMPORTANT]
> Unterstützt die Xamarin.tvOS frühe Vorschau für tvos. außerdem wurden 10 nicht vollständig `UIUserInterfaceStyle` für `UITraitCollection` noch. Vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Anpassen der Darstellung auf Grundlage des Designs

Für Benutzeroberflächenelemente, die die Darstellung Proxy zu unterstützen, kann ihre Darstellung basierend auf dem UI-Design ihrer Auflistung Merkmals angepasst werden. Für einen bestimmten UI-Element kann Entwickler also eine Farbe für das Design "hell" und eine andere Farbe für das Design "dunkel" angeben.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Leider die Xamarin.tvOS-Vorschau für tvos. außerdem wurden 10 nicht vollständig unterstützt `UIUserInterfaceStyle` für `UITraitCollection`, sodass diese Art von Anpassung noch nicht verfügbar ist. Vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Reagieren auf Designänderungen direkt

In der Entwickler erfordert, dass eine umfassendere Steuerung der Darstellung eines UI-Elements basierend auf dem UI-Design ausgewählt haben, überschreiben diese den `TraitCollectionDidChange` Methode von einer `UIView` oder `UIViewController` Klasse.

Zum Beispiel:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);
    
    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Eine Auflistung Merkmals überschreiben

Basierend auf den Entwurf einer App tvos. außerdem wurden, es gibt möglicherweise Zeiten, wenn der Entwickler muss, damit außer Kraft setzen der Auflistung Merkmal eines bestimmten Elements für die Benutzeroberfläche und verwenden Sie immer ein bestimmtes UI-Design.

Dies kann mithilfe der `SetOverrideTraitCollection` Methode für die `UIViewController` Klasse. Zum Beispiel:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Weitere Informationen finden Sie unter der [Traits](~/ios/user-interface/storyboards/unified-storyboards.md) und [überschreiben Traits](~/ios/user-interface/storyboards/unified-storyboards.md) Abschnitte der unsere [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Merkmals Sammlungen und Storyboards

In tvos. außerdem wurden 10 eine app Storyboard festgelegt werden kann, um auf Merkmals Sammlungen zu reagieren und viele Elemente der Benutzeroberfläche erfolgen hellen und dunklen Design beachten. Die aktuelle Xamarin.tvOS frühe Vorschau für tvos. außerdem wurden 10 unterstützt dieses Feature in der Benutzeroberflächen-Designer noch keine, sodass das Storyboard in Xcodes Benutzeroberflächen-Generator als problemumgehung bearbeitet werden muss.

Um Unterstützung von Merkmal zu aktivieren, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf das Storyboard-Datei in die **Projektmappen-Explorer** , und wählen Sie **Öffnen mit** > **Xcode Schnittstelle-Generator**: 

    [![](user-interface-styles-images/theme05.png "Mit Xcode-Schnittstelle-Generator öffnen")](user-interface-styles-images/theme05.png#lightbox) 
2. Um Merkmals Unterstützung zu aktivieren, wechseln Sie zu der **Datei Inspektor** und überprüfen Sie die **verwenden Merkmals Variationen** Eigenschaft in der **Schnittstelle-Generator-Dokument** Abschnitt: 

    [![](user-interface-styles-images/theme06.png "Unterstützung von Merkmal "" aktivieren")](user-interface-styles-images/theme06.png#lightbox)
3. Bestätigen Sie die Änderung zum Merkmal ""-Varianten verwenden: 

    [![](user-interface-styles-images/theme07.png "Die Verwendung Merkmals Variationen Warnung")](user-interface-styles-images/theme07.png#lightbox)
4. Speichern Sie die Änderungen in der Storyboard-Datei.

Apple hat die folgenden Möglichkeiten hinzugefügt, wenn tvos. außerdem wurden Storyboards in Benutzeroberflächen-Generator bearbeiten:

* Der Entwickler kann verschiedene Varianten der Elemente der Benutzeroberfläche, die basierend auf dem UI-Design im Angeben der **Attribut Inspektor**:
    
    * Mehrere Eigenschaften verfügen jetzt über eine  **+**  neben dem geklickt werden kann, um eine bestimmte Version des UI-Design hinzuzufügen: 

        [![](user-interface-styles-images/theme08.png "Fügen Sie eine bestimmte Version des UI-Design")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Der Entwickler kann eine neue Eigenschaft angeben, oder klicken Sie auf die **x** Schaltfläche, um ihn zu entfernen: 

        [![](user-interface-styles-images/theme09.png "Geben Sie eine neue Eigenschaft ein, oder klicken Sie auf die X-Schaltfläche, um ihn zu entfernen")](user-interface-styles-images/theme09.png#lightbox)
* Der Entwickler kann einen UI-Entwurf in hell oder dunkel Design in Benutzeroberflächen-Generator in der Vorschau anzeigen:
    
    * Die unteren Rand der Entwurfsoberfläche kann der Entwickler die aktuelle UI-Design wechseln: 

        [![](user-interface-styles-images/theme10.png "Die unteren Rand der Entwurfsoberfläche")](user-interface-styles-images/theme10.png#lightbox)
        
    * Benutzeroberflächen-Generator das neue Design angezeigt werden, und alle Anpassungen, die bestimmten Merkmal Auflistung werden angezeigt: 

        [![](user-interface-styles-images/theme11.png "Das Design im Benutzeroberflächen-Generator angezeigt")](user-interface-styles-images/theme11.png#lightbox)

Darüber hinaus weist die tvos. außerdem wurden Simulator jetzt eine Tastenkombination, um dem Entwickler, wechseln Sie schnell zwischen dem hellen und dunklen Design beim Debuggen einer app tvos. außerdem wurden zu ermöglichen. Verwenden der **Befehl + Umschalt + D** Tastatur Sequenz, die zwischen dem hellen und dunklen zu wechseln.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Licht behandelt und dunkel UI-Designs, Apple tvos. außerdem wurden 10 und wie diese in einer app Xamarin.tvOS implementiert hinzugefügt wurde.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Was ist neu in tvos. außerdem wurden 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
