---
title: tvos-Benutzeroberflächen Stile in xamarin
description: In diesem Artikel werden die hellen und dunklen UI-Designs behandelt, die Apple tvos 10 hinzugefügt hat, und wie diese in einer xamarin. tvos-App implementiert werden.
ms.prod: xamarin
ms.assetid: 8BC37683-AD9E-45CD-BE40-96965618AD1D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 89756d5b897b39dd0cf45074474189a4a0a8ada8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769989"
---
# <a name="tvos-user-interface-styles-in-xamarin"></a>tvos-Benutzeroberflächen Stile in xamarin

_In diesem Artikel werden die hellen und dunklen UI-Designs behandelt, die Apple tvos 10 hinzugefügt hat, und wie diese in einer xamarin. tvos-App implementiert werden._

tvos 10 unterstützt jetzt sowohl ein dunkles als auch ein helles Design der Benutzeroberfläche, mit dem sich alle Build-in-UIKit-Steuerelemente basierend auf den Benutzereinstellungen automatisch an anpassen. Außerdem kann der Entwickler Benutzeroberflächen Elemente basierend auf dem Design, das der Benutzer ausgewählt hat, manuell anpassen und ein bestimmtes Design überschreiben.

<a name="About-the-New-User-Interface-Styles" />

## <a name="about-the-new-user-interface-styles"></a>Informationen zu den neuen Benutzeroberflächen Stilen

Wie bereits erwähnt, unterstützt tvos 10 jetzt sowohl ein dunkles als auch ein helles Design der Benutzeroberfläche, dass alle in der benutzerdefinierten UIKit-Steuerelemente basierend auf den Benutzereinstellungen automatisch an angepasst werden.

Der Benutzer kann dieses Design ändern, indem er zu **Einstellungen** > **Allgemeine** > Darstellung**wechselt und zwischen** **hell** und **dunkel**wechselt:

[![](user-interface-styles-images/theme01.png "Die app \"Einstellungen\"")](user-interface-styles-images/theme01.png#lightbox)

Wenn das **dunkle** Design ausgewählt ist, werden alle Benutzeroberflächen Elemente in einem dunklen Hintergrund zum hellen Text gewechselt:

[![](user-interface-styles-images/theme02.png "Das Design \"dunkel\"")](user-interface-styles-images/theme02.png#lightbox)

Der Benutzer kann das Design jederzeit ändern und auf der Grundlage der aktuellen Aktivität, in der sich das Apple TV-Gerät befindet, oder der Tageszeit wechseln.

Das Design der hellen Benutzeroberfläche ist das Standarddesign, und alle vorhandenen tvos-Apps verwenden immer noch das Design "Hell", unabhängig von den Einstellungen des Benutzers, es sei denn, Sie wurden für tvos 10 geändert, um das dunkle Design zu nutzen. Eine tvos 10-App bietet auch die Möglichkeit, das aktuelle Design zu überschreiben, und verwendet immer das helle oder dunkle Design für einige oder alle Benutzeroberflächen.

<a name="Adopting-the-Light-and-Dark-Themes" />

## <a name="adopting-the-light-and-dark-themes"></a>Einführung in das helle und dunkle Design

Zur Unterstützung dieses Features hat Apple der `UITraitCollection` -Klasse eine neue API hinzugefügt, und eine tvos-app muss sich für die Unterstützung der dunklen Darstellung entscheiden (über eine Einstellung in der `Info.plist` Datei).

Gehen Sie folgendermaßen vor, um die Unterstützung für ein helles und dunkles Design zu abonnieren:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Info.plist`, um sie zur Bearbeitung zu öffnen.
2. Wählen Sie die **Quell** Ansicht aus (vom unteren Rand des Editors).
3. Fügen Sie einen neuen Schlüssel hinzu, `UIUserInterfaceStyle`und nennen Sie ihn:

    [![](user-interface-styles-images/theme03.png "Der uiuserinterfakestyle-Schlüssel")](user-interface-styles-images/theme03.png#lightbox)
4. Legen Sie den Typ auf `String` fest, und geben Sie `Automatic`den folgenden Wert ein:

    [![](user-interface-styles-images/theme04.png "Automatisch eingeben")](user-interface-styles-images/theme04.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

Es gibt drei mögliche Werte für den `UIUserInterfaceStyle` Schlüssel:

- Das **Licht** erzwingt die Benutzeroberfläche der tvos-APP, immer das helle Design zu verwenden.
- **Dunkel** : erzwingt, dass die Benutzeroberfläche der tvos-App immer das Design "dunkel" verwendet.
- **Automatisches** wechseln zwischen dem hellen und dem dunklen Design basierend auf den Einstellungen des Benutzers in den Einstellungen. Dies ist die bevorzugte Einstellung.

<a name="UIKit-Theme-Support" />

### <a name="uikit-theme-support"></a>UIKit-Designunterstützung

Wenn eine tvos-App standardmäßig integrierte, integrierte `UIView` Steuerelemente verwendet, werden Sie automatisch auf das UI-Design reagieren, ohne dass ein Entwickler eingreifen muss.

Außerdem ändern `UITextView` und automatisch Ihre Farbe basierend auf dem Design der Benutzeroberfläche auswählen: `UILabel`

- Der Text wird im Design "Hell" schwarz angezeigt.
- Der Text wird im Design "dunkel" weiß angezeigt.

Wenn der Entwickler die Textfarbe je manuell ändert (entweder im Storyboard oder im Code), sind Sie für die Verarbeitung von Farbänderungen basierend auf dem Design der Benutzeroberfläche verantwortlich.

<a name="New-Blur-Effects" />

### <a name="new-blur-effects"></a>Neue Weichzeichnereffekte

Zur Unterstützung der hellen und dunklen Designs in einer tvos 10-APP hat Apple zwei neue Weichzeichnereffekte hinzugefügt. Diese neuen Effekte passen den weich zeichnenden automatisch basierend auf dem Design der Benutzeroberfläche an, das der Benutzer wie folgt ausgewählt hat:

- `UIBlurEffectStyleRegular`-Verwendet einen hellen Weichzeichner im Design "Hell" und einen dunklen Weichzeichner im Design "dunkel".
- `UIBlurEffectStyleProminent`-Verwendet einen Extra-Light-Weichzeichner im Design "Hell" und einen extra dunklen Weichzeichner im Design "dunkel".

<a name="Working-with-Trait-Collections" />

## <a name="working-with-trait-collections"></a>Arbeiten mit Merkmals Auflistungen

Die neue `UserInterfaceStyle` -Eigenschaft `UITraitCollection` der-Klasse kann verwendet werden, um das aktuell ausgewählte UI-Design zu erhalten `UIUserInterfaceStyle` , und es handelt sich um eine Enumeration mit einem der folgenden Werte:

- **Hell** : das Design der hellen Benutzeroberfläche ist ausgewählt.
- **Dunkel** : das dunkle UI-Design ist ausgewählt.
- **Nicht angegeben** : die Ansicht wurde noch nicht auf dem Bildschirm angezeigt, sodass das aktuelle UI-Design unbekannt ist.

Außerdem verfügen Merkmals Auflistungen in tvos 10 über die folgenden Features:

- Der Darstellungs Proxy kann basierend auf der `UserInterfaceStyle` eines bestimmten `UITraitCollection` angepasst werden, um Elemente wie Bilder oder Element Farben auf Grundlage des Designs zu ändern.
- Eine tvos-App kann Merkmals Sammlungs Änderungen durch über `TraitCollectionDidChange` Schreiben der- `UIView` Methode `UIViewController` einer-Klasse oder-Klasse verarbeiten.

> [!IMPORTANT]
> Die frühe Vorschauversion von xamarin. tvos für tvos 10 wird `UIUserInterfaceStyle` `UITraitCollection` noch nicht vollständig unterstützt. Die vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt.

<a name="Customizing-Appearance-Based-on-Theme" />

### <a name="customizing-appearance-based-on-theme"></a>Anpassen der Darstellung basierend auf dem Design

Für Benutzeroberflächen Elemente, die den Darstellungs Proxy unterstützen, kann ihre Darstellung basierend auf dem UI-Design Ihrer Merkmals Auflistung angepasst werden. Daher kann der Entwickler für ein bestimmtes Benutzeroberflächen Element eine Farbe für das helle Design und eine andere Farbe für das dunkle Design angeben.

```csharp
button.SetTitleColor (UIColor.Red, UIControlState.Normal);

// TODO - Pseudocode because this isn't currently supported in the preview bindings.
var light = new UITraitCollection(UIUserInterfaceStyle.Light);
var dark = new UITraitCollection(UIUserInterfaceStyle.Dark);

button.ForTraitCollection(light).SetTitleColor (UIColor.Red, UIControlState.Normal);
button.ForTraitCollection(dark).SetTitleColor (UIColor.White, UIControlState.Normal);
```

> [!IMPORTANT]
> Leider ist die xamarin. tvos-Vorschau für tvos 10 für `UIUserInterfaceStyle` `UITraitCollection`nicht vollständig unterstützt. Daher ist diese Art der Anpassung noch nicht verfügbar. Die vollständige Unterstützung wird in einer zukünftigen Version hinzugefügt.

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="responding-to-theme-changes-directly"></a>Direkte Reaktion auf Designänderungen

Wenn Entwickler die Darstellung eines UI-Elements auf der Grundlage des ausgewählten Benutzeroberflächen Designs genauer steuern müssen, können Sie die `TraitCollectionDidChange` -Methode einer `UIView` -Klasse `UIViewController` oder-Klasse überschreiben.

Beispiel:

```csharp
public override void TraitCollectionDidChange (UITraitCollection previousTraitCollection)
{
    base.TraitCollectionDidChange (previousTraitCollection);

    // Take action based on the Light or Dark theme
    ...
}
```

<a name="Responding-to-Theme-Changes-Directly" />

### <a name="overriding-a-trait-collection"></a>Überschreiben einer Merkmals Auflistung

Basierend auf dem Entwurf einer tvos-App kann es vorkommen, dass der Entwickler die Merkmals Auflistung eines bestimmten Benutzeroberflächen Elements überschreiben muss und immer ein bestimmtes UI-Design verwenden muss.

Dies kann mithilfe der `SetOverrideTraitCollection` -Methode für die `UIViewController` -Klasse erfolgen. Beispiel:

```csharp
// Create new trait and configure it
var trait = new UITraitCollection ();
...

// Apply new trait collection
SetOverrideTraitCollection (trait, this);
```

Weitere Informationen finden Sie in den Abschnitten [Merkmale](~/ios/user-interface/storyboards/unified-storyboards.md) und Überschreiben von [Merkmalen](~/ios/user-interface/storyboards/unified-storyboards.md) unserer [Einführung in die Dokumentation zu Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) .

<a name="Trait-Collections-and-Storyboards" />

### <a name="trait-collections-and-storyboards"></a>Merkmals Auflistungen und Storyboards

In tvos 10 kann das Storyboard einer APP so festgelegt werden, dass Sie auf Merkmals Auflistungen antwortet, und viele Elemente der Benutzeroberfläche können leicht und dunkel Design fähig gemacht werden. Die aktuelle xamarin. tvos-frühe Vorschau für tvos 10 unterstützt dieses Feature noch nicht im Schnittstellen-Designer, sodass das Storyboard in der Interface Builder von Xcode als Problem Umgehung bearbeitet werden muss.

Gehen Sie folgendermaßen vor, um die Unterstützung von Merkmal Sammlungen zu aktivieren

1. Klicken Sie **mit** > der rechten Maustaste auf die Storyboard-Datei im **Projektmappen-Explorer** , und wählen Sie mit**Xcode Interface Builder**öffnen:

    [![](user-interface-styles-images/theme05.png "Öffnen mit Xcode Interface Builder")](user-interface-styles-images/theme05.png#lightbox)
2. Um die Unterstützung für Merkmals Auflistungen zu aktivieren, wechseln Sie zum **Datei Inspektor** , und aktivieren Sie im Abschnitt **Interface Builder Document** die Eigenschaft **Merkmals Variationen verwenden** :

    [![](user-interface-styles-images/theme06.png "Unterstützung von Merkmal aktivieren")](user-interface-styles-images/theme06.png#lightbox)
3. Bestätigen Sie die Änderung, um Merkmals Variationen zu verwenden:

    [![](user-interface-styles-images/theme07.png "Warnung \"Merkmals Variationen verwenden\"")](user-interface-styles-images/theme07.png#lightbox)
4. Speichern Sie die Änderungen in der storyboarddatei.

Apple hat beim Bearbeiten von tvos-Storyboards in Interface Builder die folgenden Funktionen hinzugefügt:

- Der Entwickler kann verschiedene Variationen von Benutzeroberflächen Elementen angeben, die auf dem UI-Design im **Attribut Inspektor**basieren:

  - Mehrere Eigenschaften verfügen jetzt über **+** eine neben Ihnen, auf die Sie klicken können, um eine spezifische Version des Benutzeroberflächen Designs hinzuzufügen:

    [![](user-interface-styles-images/theme08.png "Benutzeroberflächen-designspezifische Version hinzufügen")](user-interface-styles-images/theme08.png#lightbox)

  - Der Entwickler kann eine neue Eigenschaft angeben oder auf die Schaltfläche **x** klicken, um Sie zu entfernen:

    [![](user-interface-styles-images/theme09.png "Geben Sie eine neue Eigenschaft an, oder klicken Sie auf die Schaltfläche \"x\"")](user-interface-styles-images/theme09.png#lightbox)
- Der Entwickler kann einen Design der Benutzeroberfläche im hellen oder dunklen Design innerhalb Interface Builder anzeigen:

  - Der untere Teil des Designoberfläche ermöglicht es dem Entwickler, das aktuelle UI-Design zu wechseln:

    [![](user-interface-styles-images/theme10.png "Der untere Teil des Designoberfläche")](user-interface-styles-images/theme10.png#lightbox)

  - Das neue Design wird in Interface Builder angezeigt, und alle spezifischen Anpassungen der Merkmals Sammlung werden angezeigt:

    [![](user-interface-styles-images/theme11.png "Das in Interface Builder angezeigte Design")](user-interface-styles-images/theme11.png#lightbox)

Außerdem verfügt der tvos-Simulator jetzt über eine Tastenkombination, die es dem Entwickler ermöglicht, beim Debuggen einer tvos-app schnell zwischen den hellen und dunklen Designs zu wechseln. Verwenden Sie die **Command-Shift-D-** Tastatursequenz, um zwischen hell und dunkel zu wechseln.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die hellen und dunklen UI-Designs behandelt, die Apple tvos 10 hinzugefügt hat, und wie diese in einer xamarin. tvos-App implementiert werden.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [Neues in tvos 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
