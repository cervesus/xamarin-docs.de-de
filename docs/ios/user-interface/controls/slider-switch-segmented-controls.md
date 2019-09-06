---
title: Schieberegler, Switches und segmentierte Steuerelemente in xamarin. IOS
description: In diesem Dokument werden Folien, Switches und segmentierte Steuerelemente in xamarin. IOS erläutert, und es wird beschrieben, wie Sie sowohl Programm gesteuert als auch im IOS-Designer mit Ihnen arbeiten können.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: bcb860a88c67b3b2cde7336d53d717d4d9201fd4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288477"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Schieberegler, Switches und segmentierte Steuerelemente in xamarin. IOS

<a name="Sliders" />

## <a name="sliders"></a>Rutscher

Das Schieberegler-Steuerelement ermöglicht die einfache Auswahl eines numerischen Werts innerhalb eines Bereichs. Das Steuerelement ist standardmäßig auf einen Wert zwischen 0 und 1 gesetzt, aber diese Limits können angepasst werden.

 [![](slider-switch-segmented-controls-images/image25a.png "Keits")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Der folgende Screenshot zeigt die Eigenschaften, die im Designer bearbeitet werden können:

 [![](slider-switch-segmented-controls-images/image26a.png "Schieberegler-Eigenschaften")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Sie können diese Werte wie unten dargestellt im Code festlegen, einschließlich der Verknüpfung eines Handlers, um den aktuell ausgewählten Wert in `UILabel` einem-Steuerelement anzuzeigen:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Sie können die visuelle Darstellung des Schiebereglers auch anpassen, indem Sie festlegen.

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Der angepasste Schieberegler sieht wie folgt aus:

 [![](slider-switch-segmented-controls-images/image27a.png "Benutzerdefinierter Schieberegler")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Zurzeit gibt es einen [Fehler](https://stackoverflow.com/a/19496179) , `ThumbTint` der bewirkt, dass der zur Laufzeit nicht wie erwartet gerrennt wird. Sie können die folgende Codezeile **vor** dem obigen Code als Problem Umgehung hinzufügen. [[Quelle](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Sie können jedes beliebige Bild verwenden, da es überschrieben wird. Stellen Sie jedoch sicher, dass es _in_ das Verzeichnis "Resources" eingefügt wird und im Code aufgerufen wird.

<a name="Switch" />

## <a name="switch"></a>Schalter

IOS verwendet `UISwitch` als boolesche Eingabe, die auf anderen Plattformen durch eine Options Schaltfläche dargestellt werden kann. Der Benutzer kann das Steuerelement ändern, indem er den Ziehpunkt *zwischen den* ein **/aus-** Positionen verschiebt.

 [![](slider-switch-segmented-controls-images/image28a.png "Not")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Die Darstellung des Schalters kann in der **Eigenschaftenpad** des Designers angepasst werden, sodass Sie den Standardzustand, ein **-oder Ausschalten von Tönungs** -Farben und ein ein **-/ausschalten**können. Dies wird in der folgenden Abbildung veranschaulicht:

 [![](slider-switch-segmented-controls-images/image29a.png "Switch-Eigenschaften")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Die Eigenschaften des Schalters können auch im Code festgelegt werden. der folgende Code zeigt z. b. einen Schalter mit dem Standard `On`Wert:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Segmentierte Steuerelemente

Ein segmentiertes Steuerelement ist eine organisierte Methode, mit der Benutzer mit einer kleinen Anzahl von Optionen interagieren können. Die Anordnung erfolgt horizontal, und jedes Segment fungiert als separate Schaltfläche. Wenn Sie den Designer verwenden, befindet sich das segmentierte Steuerelement unter **Toolbox > Steuerelemente**und sollte wie in der folgenden Abbildung aussehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Eine einzigartige Funktion des Designers ermöglicht, dass die einzelnen Segmente auf der Entwurfs Oberfläche einzeln ausgewählt werden, wie unten dargestellt:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

Dadurch kann die Eigenschaftenpad verwendet werden, um die Eigenschaften der einzelnen Segmente genauer zu steuern. Sie können die bearbeitbaren Eigenschaften im folgenden Screenshot sehen:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmentiertes Steuerelement")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Beachten Sie, dass der segmentierte Steuerelement Stil in iOS7 veraltet ist und die Anpassungsoptionen in einer iOS7-Anwendung daher keine Auswirkung haben.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [Warnungs Controller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
