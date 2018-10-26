---
title: TvOS-Stile der Benutzeroberfläche in Xamarin
description: Dieser Artikel behandelt das Licht und dunkel UI-Designs, die Apple TvOS 10 und wie sie in einer Xamarin.tvOS-app implementiert hinzugefügt wurde.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 2536ca5d3bff3f5b7962bc4fcf58b31a130fd03c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104764"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>TvOS-Stile der Benutzeroberfläche in Xamarin

_Dieser Artikel behandelt das Licht und dunkel UI-Designs, die Apple TvOS 10 und wie sie in einer Xamarin.tvOS-app implementiert hinzugefügt wurde._

TvOS 10 nun unterstützt sowohl die Light-Benutzeroberfläche ein dunkles Design, dass alle von der integrierten UIKit Steuerelemente wird automatisch angepasst, basierend auf den Einstellungen des Benutzers. Darüber hinaus wird der Entwickler kann manuell anpassen, Benutzeroberflächenelemente auf der Grundlage des Designs, die der Benutzer ausgewählt hat, und kann außer Kraft setzen ein bestimmtes Designs.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Informationen zu den neuen Stile der Benutzeroberfläche

Wie bereits erwähnt, basierend TvOS 10 nun unterstützt sowohl die Light-Benutzeroberfläche ein dunkles Design, dass alle von der integrierten UIKit Steuerelemente wird automatisch angepasst, auf den Einstellungen des Benutzers ab.

Der Benutzer kann dieses Design wechseln, indem Sie auf **Einstellungen** > **allgemeine** > **Darstellung** und zum Wechsel zwischen **Licht**  und **dunkel**:

[![](user-interface-styles-images/theme01.png "Die Einstellungs-app")](user-interface-styles-images/theme01.png#lightbox)

Wenn die **dunkel** Design ausgewählt ist, werden alle Elemente der Benutzeroberfläche wechseln Sie zur heller Text auf dunklem Hintergrund:

[![](user-interface-styles-images/theme02.png "Das Design \"dunkel\"")](user-interface-styles-images/theme02.png#lightbox)

Der Benutzer hat die Möglichkeit, wechseln Sie zu einem beliebigen Zeitpunkt das Design und kann also basierend auf der aktuellen Aktivität, die auf dem sich das Apple TV befindet oder die Zeit des Tages erfolgen kann.

Das Design der Light-Benutzeroberfläche ist das Standarddesign und sämtlicher vorhandener TvOS-apps werden das Design "hell", unabhängig von den Einstellungen des Benutzers, weiterhin verwendet, es sei denn, sie für TvOS 10, um das Design "dunkel" nutzen geändert werden. Eine TvOS 10-app verfügt zudem über die Möglichkeit zum Überschreiben des aktuellen Designs und immer der hell oder dunkel Design für einige oder alle der Benutzeroberfläche verwenden.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Übernehmen die hellen und dunklen Designs

Um dieses Feature zu unterstützen, wurde Apple hinzugefügt, um eine neue API die `UITraitCollection` -Klasse und einer TvOS-app müssen sich um die dunkle Darstellung zu unterstützen (über eine Einstellung in der `Info.plist` Datei).

Um zur Unterstützung von im hellen und dunklen Design teilnehmen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. Wählen Sie die **Quelle** anzeigen (von unten im Editor).
3. Fügen Sie einen neuen Schlüssel hinzu, und rufen sie `UIUserInterfaceStyle`: 

    [![](user-interface-styles-images/theme03.png "Der Schlüssel UIUserInterfaceStyle")](user-interface-styles-images/theme03.png#lightbox)
4. Lassen Sie den Typ `String` , und geben Sie einen Wert von `Automatic`: 

    [![](user-interface-styles-images/theme04.png "Geben Sie die automatische")](user-interface-styles-images/theme04.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

Es gibt drei mögliche Werte für die `UIUserInterfaceStyle` Schlüssel:

- **Light** -erzwingt die TvOS-app-Benutzeroberfläche immer das Design "hell" verwenden.
- **Dunkel** -erzwingt die TvOS-app-Benutzeroberfläche immer das Design "dunkel" verwenden.
- **Automatische** -wechselt zwischen hellen und dunklen Designs auf der Grundlage von den Einstellungen des Benutzers in den Einstellungen. Dies ist die bevorzugte Einstellung.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>Unterstützung von UIKit-Designs

Wenn eine TvOS-app, integrierten verwendet wird `UIView` basierte Steuerelemente, sie werden automatisch auf das Design der Benutzeroberfläche ohne Eingriffe von Entwicklern reagieren.

Darüber hinaus `UILabel` und `UITextView` ändert sich automatisch auf ihre Farbe basierend auf der UI-Design wählen:

- Der Text wird in das Design "hell" Schwarz.
- Der Text wird im dunklen Design weiß sein.

Wenn der Entwickler die Textfarbe manuell (entweder im Storyboard oder Code) ändert, werden sie für die Farbe ändert sich basierend auf dem UI-Design Behandlung verantwortlich.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Neue Blur-Effekte

Für die Unterstützung der hellen und dunklen Design in einer TvOS 10-app, hat Apple zwei neue Blur-Effekte hinzugefügt. Diese neuen Effekte passt automatisch die Unschärfe, basierend auf der UI-Designs, die der Benutzer wie folgt ausgewählt hat:

- `UIBlurEffectStyleRegular` – Verwendet, die ein Licht in das Design "hell" und einem dunklen blur blur im dunklen Design.
- `UIBlurEffectStyleProminent` – Verwendet eine Extra-light Blur in das Design "hell" und einem sehr dunklen Blur im dunklen Design.

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Arbeiten mit Auflistungen von Merkmal

Die neue `UserInterfaceStyle` Eigenschaft der `UITraitCollection` Klasse kann verwendet werden, um das aktuell ausgewählte UI-Design zu erhalten und werden eine `UIUserInterfaceStyle` Enumeration eines der folgenden Werte:

- **Light** -Design für die Light-Benutzeroberfläche aktiviert ist.
- **Dunkel** : die Benutzeroberfläche des dunklen Designs aktiviert ist.
- **Unbekannter** : die Ansicht wurde nicht angezeigt, auf dem Bildschirm, damit die aktuelle UI-Design unbekannt ist.

Darüber hinaus weisen Merkmal Sammlungen die folgenden Funktionen in TvOS 10:
 
- Der Proxy für die Darstellung kann angepasst werden, basierend auf den `UserInterfaceStyle` von einer angegebenen `UITraitCollection` zu ändern, z. B. Bilder oder Element von Farben basierend auf dem Design.
- TvOS-app kann Merkmal sammlungsänderungen behandeln, durch Überschreiben der `TraitCollectionDidChange` Methode eine `UIView` oder `UIViewController` Klasse.

> [!IMPORTANT]
> Die frühe Xamarin.tvOS-Vorschau für TvOS 10 nicht vollständig unterstützen `UIUserInterfaceStyle` für `UITraitCollection` noch. Vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt werden.




<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Anpassen der Darstellung, die basierend auf dem Design

Für Benutzeroberflächenelemente, die den Proxy für die Darstellung zu unterstützen, kann ihre Darstellung basierend auf dem UI-Design ihrer Trait-Sammlung angepasst werden. Für ein angegebenes UI-Element, kann der Entwickler also eine Farbe für das Design "hell" und eine andere Farbe für das Design "dunkel" angeben.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Leider können die Xamarin.tvOS-Vorschau für TvOS 10 nicht vollständig unterstützen `UIUserInterfaceStyle` für `UITraitCollection`, sodass diese Art der Anpassung noch nicht verfügbar ist. Vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt werden.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Direkt reagieren, Designänderungen

In der Entwickler ist erforderlich, noch mehr Kontrolle über die Darstellung eines Benutzeroberflächenelements auf Grundlage der ausgewählten UI-Designs, überschreiben die `TraitCollectionDidChange` Methode eine `UIView` oder `UIViewController` Klasse.

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

### <a name="overriding-a-trait-collection"></a>Eine Auflistung der Eigenschaft überschreiben

Basierend auf den Entwurf einer TvOS-App, es gibt möglicherweise Zeiten, wenn der Entwickler muss die Trait-Auflistung eines angegebenen Elements der Benutzeroberfläche zu überschreiben und verwenden Sie immer ein bestimmtes Design der Benutzeroberfläche.

Dies kann erfolgen mithilfe der `SetOverrideTraitCollection` Methode für die `UIViewController` Klasse. Zum Beispiel:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Weitere Informationen finden Sie unter den ["traits"](~/ios/user-interface/storyboards/unified-storyboards.md) und [überschreiben "traits"](~/ios/user-interface/storyboards/unified-storyboards.md) Teile unserer [Einführung in Storyboards Unified](~/ios/user-interface/storyboards/unified-storyboards.md) Dokumentation.

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Merkmal Sammlungen und Storyboards

In TvOS 10 kann der app-Storyboard-Merkmal-Sammlungen reagieren festgelegt werden und viele Elemente der Benutzeroberfläche erfolgen hellen und dunklen Design kennen. Der aktuellen frühen Xamarin.tvOS-Vorschau für TvOS 10 unterstützt dieses Feature im Benutzeroberflächen-Designer noch keine, sodass das Storyboard in Interface Builder von Xcode, als problemumgehung bearbeitet werden muss.

Um Unterstützung von Merkmal zu aktivieren, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste auf die Storyboard-Datei in die **Projektmappen-Explorer** , und wählen Sie **Öffnen mit** > **Xcode Interface Builder**: 

    [![](user-interface-styles-images/theme05.png "Mit Xcode Interface Builder öffnen")](user-interface-styles-images/theme05.png#lightbox) 
2. Um Unterstützung von Merkmal zu aktivieren, wechseln Sie zu der **Dateiinspektor** und überprüfen Sie die **verwenden Merkmal Variationen** -Eigenschaft in der **Interface Builder-Dokument** Abschnitt: 

    [![](user-interface-styles-images/theme06.png "Unterstützung von Merkmal aktivieren")](user-interface-styles-images/theme06.png#lightbox)
3. Vergewissern Sie sich die Änderung zum Merkmal Variationen: 

    [![](user-interface-styles-images/theme07.png "Die Verwendung Merkmal Variationen-Warnung")](user-interface-styles-images/theme07.png#lightbox)
4. Speichern Sie die Änderungen der Storyboard-Datei.

Apple hat folgenden Möglichkeiten hinzugefügt, wenn TvOS-Storyboards in Interface Builder zu bearbeiten:

* Der Entwickler kann verschiedene Varianten der Elemente der Benutzeroberfläche, die basierend auf dem UI-Design im Festlegen der **Attributinspektor**:
    
    * Mehrere Eigenschaften verfügen nun über eine **+** nebenstehenden die geklickt werden kann, um eine bestimmte Version des UI-Design hinzuzufügen: 

        [![](user-interface-styles-images/theme08.png "Fügen Sie eine bestimmte Version des UI-Design")](user-interface-styles-images/theme08.png#lightbox) 
    
    * Der Entwickler eine neue Eigenschaft angeben kann, oder klicken Sie auf die **x** Schaltfläche, um ihn zu entfernen: 

        [![](user-interface-styles-images/theme09.png "Geben Sie eine neue Eigenschaft ein, oder klicken Sie auf die Schaltfläche \"X\", um ihn zu entfernen")](user-interface-styles-images/theme09.png#lightbox)
* Entwickler kann einen UI-Entwurf in der hell oder dunkel Design aus innerhalb von Interface Builder in Vorschau anzeigen:
    
    * Unteren Rand der Entwurfsoberfläche kann der Entwickler die aktuelle UI-Design wechseln: 

        [![](user-interface-styles-images/theme10.png "Unteren Rand der Entwurfsoberfläche")](user-interface-styles-images/theme10.png#lightbox)
        
    * Das neue Design in Interface Builder angezeigt und bestimmten Anpassungen Merkmal Auflistung angezeigt werden: 

        [![](user-interface-styles-images/theme11.png "Das Design in Interface Builder angezeigt")](user-interface-styles-images/theme11.png#lightbox)

Darüber hinaus verfügt die TvOS-Simulator nun eine Tastenkombination, die Entwickler schnell zwischen den hellen und dunklen Design, beim Debuggen einer TvOS-app wechseln können. Verwenden der **Befehl + Umschalt + D,** Tastatur Sequenz, die zwischen hellen und dunklen zu wechseln.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden das Licht behandelt und dunkel UI-Designs, die Apple TvOS 10 und wie sie in einer Xamarin.tvOS-app implementiert hinzugefügt wurde.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [Neuerungen in TvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
