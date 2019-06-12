---
title: Schieberegler, Schalter und segmentierte Steuerelemente in Xamarin.iOS
description: Dieses Dokument erläutert die Folien, Schalter und segmentierte Steuerelemente in Xamarin.iOS, die beschreibt, wie mit ihnen sowohl programmgesteuert als auch in der iOS-Designer arbeiten.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 7b66005ff64763d68dc6101985e514fa56cf896d
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827376"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Schieberegler, Schalter und segmentierte Steuerelemente in Xamarin.iOS

<a name="Sliders" />

## <a name="sliders"></a>Schieberegler

Das Schieberegler-Steuerelement ermöglicht einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. Das Steuerelement wird standardmäßig auf einen Wert zwischen 0 und 1, aber diese Grenzwerte können angepasst werden.

 [![](slider-switch-segmented-controls-images/image25a.png "Schieberegler")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Der folgende Screenshot zeigt die Eigenschaften, die im Designer bearbeitet werden:

 [![](slider-switch-segmented-controls-images/image26a.png "Schieberegler-Eigenschaften")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Sie können diese Werte im Code festlegen, wie unten gezeigt, einschließlich verknüpfen, um einen Handler zum Anzeigen des derzeit ausgewählten Werts in eine `UILabel` Steuerelement:

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

Der benutzerdefinierte Schieberegler sieht folgendermaßen aus:

 [![](slider-switch-segmented-controls-images/image27a.png "Benutzerdefinierte Schieberegler")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Es gibt derzeit ein [Fehler](https://stackoverflow.com/a/19496179) verursacht die `ThumbTint` nicht zur Laufzeit gerendert wird, wie erwartet. Sie können die folgende Codezeile hinzufügen **vor** Code über dieses Problem zu umgehen. [[Quelle](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Können Sie ein Image, wie werden überschrieben, sondern stellen Sie sicher, dass es platziert wurde _in_ das Verzeichnis "Resources" und in Ihrem Code aufgerufen wird.

<a name="Switch" />

## <a name="switch"></a>Schalter

iOS verwendet die `UISwitch` wie einen booleschen Wert eingeben, die durch ein Optionsfeld auf anderen Plattformen dargestellt werden kann. Der Benutzer kann das Steuerelement bearbeiten, durch das Verschieben der *Thumb* zwischen der **/Deaktivieren von** Positionen.

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Die Darstellung des Switches kann angepasst werden, der **Pad "Eigenschaften"** des Designers, um den Standardzustand steuern zu können, **/Deaktivieren von Farbton** Farben und **On/Off-Image**. Dies ist in der folgenden Abbildung dargestellt:

 [![](slider-switch-segmented-controls-images/image29a.png "Switch-Eigenschaften")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Die Eigenschaften des Switchs können auch im Code festgelegt werden, z. B. zeigt des folgende Codes einen Switch mit dem Standardwert von `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Segmentierte Steuerelemente

Ein segmentiertes Steuerelement ist ein organisiert, dass Benutzer mit einer kleinen Anzahl von Optionen interagieren können. Es ist horizontal angeordnet, und jedes Segment fungiert als eine gesonderte Schaltfläche klickt. Bei Verwendung des Designers, dem segmentierten Steuerelement finden Sie unter **ToolBox > Steuerelemente**, und sollte etwa wie folgt aussehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Eine einzigartige Funktion des Designers kann für jedes Segment einzeln auf der Entwurfsoberfläche ausgewählt werden, wie unten gezeigt:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Dadurch wird das Pad "Eigenschaften" verwendet werden, um die Eigenschaften der einzelnen Segmente genauer zu steuern. Sie können der bearbeitbaren Eigenschaften im folgenden Screenshot sehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Anzumerken ist, dass die segmentierte Steuerelementstil in iOS7 veraltet ist und aus diesem Grund Anpassen der Optionen in einer Anwendung für iOS7 keine Auswirkungen hat.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/monotouch/Controls/)
- [Warnungscontroller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
