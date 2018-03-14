---
title: Schieberegler, Switches und segmentierte Steuerelemente
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 282a4cb59545703c5172f8747cb5b633e7b648dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>Schieberegler, Switches und segmentierte Steuerelemente

<a name="Sliders" />


## <a name="sliders"></a>Schieberegler

Das Schieberegler-Steuerelement ermöglicht einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. Das Steuerelement wird standardmäßig auf einen Wert zwischen 0 und 1, aber diese Grenzwerte angepasst werden.

 [![](slider-switch-segmented-controls-images/image25a.png "Schieberegler")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Der folgende Screenshot zeigt die Eigenschaften, die im Designer bearbeitet werden:

 [![](slider-switch-segmented-controls-images/image26a.png "Schieberegler-Eigenschaften")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Sie können diese Werte im Code festlegen, wie unten gezeigt, einschließlich verknüpft einen Handler zum Anzeigen des derzeit ausgewählten Wertes in einem `UILabel` Steuerelement:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Sie können auch die visuelle Darstellung des Schiebereglers durch Einstellung anpassen.

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Der angepasste Schieberegler sieht wie folgt:

 [![](slider-switch-segmented-controls-images/image27a.png "Benutzerdefinierte Schieberegler")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Zurzeit ist ein [Fehler](http://stackoverflow.com/a/19496179) verursacht die `ThumbTint` nicht zur Laufzeit gerendert wird, wie erwartet. Sie können die folgende Codezeile hinzufügen **vor** Code über dieses Problem zu umgehen. [[Quelle](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Können Sie Bild wird außer Kraft gesetzt werden, sondern stellen Sie sicher, dass sich befindet _in_ Ressourcenverzeichnis und in Ihrem Code aufgerufen wird.

<a name="Switch" />

## <a name="switch"></a>Schalter

iOS verwendet die `UISwitch` wie Geben Sie einen booleschen Wert ab, die durch ein Optionsfeld auf anderen Plattformen dargestellt werden kann. Der Benutzer kann das Steuerelement bearbeiten, durch Verschieben der *Thumb* zwischen der **aktivieren/deaktivieren** Positionen.

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Die Darstellung des Switches kann angepasst werden, der **Eigenschaften Pad** des Designers, um den Standardzustand zurückgesetzt steuern zu können, **aktivieren/deaktivieren Farbton** Farben und ein   **/aus-Image**. Dies wird in der folgenden Abbildung veranschaulicht:

 [![](slider-switch-segmented-controls-images/image29a.png "Switch-Eigenschaften")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Die Eigenschaften des Switches können auch im Code festgelegt werden, z. B. zeigt des folgende Codes einen Switch mit dem Standardwert des `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Segmentierte Steuerelemente

Ein segmentiertes Steuerelement ist eine organisierte ermöglichen Benutzern die Interaktion mit einer kleinen Anzahl von Optionen. Es ist horizontal angeordnet, und jedes Segment fungiert als eine separate Schaltfläche. Beim Verwenden des Designers segmentierte Steuerelements finden Sie unter **ToolBox > Steuerelemente**, und sollte wie folgt aussehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmentierte-Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Eine einzigartige Funktion des Designers kann für jedes Segment einzeln auf der Entwurfsoberfläche ausgewählt werden, wie unten gezeigt:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmentierte-Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Dadurch wird das Auffüllzeichen Eigenschaften verwendet werden, um die Eigenschaften der einzelnen Segmente genauer zu steuern. Sie können der bearbeitbaren Eigenschaften im folgenden Screenshot sehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmentierte-Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Beachten Sie, dass segmentierte Steuerelementformats in ios7 erstellt werden veraltet ist, und daher die Optionen für diese in einer Anwendung für ios7 nicht anpassen keine Auswirkungen hat.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
- [Warnung-Controller](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
