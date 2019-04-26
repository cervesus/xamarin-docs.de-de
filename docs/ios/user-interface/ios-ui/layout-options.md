---
title: Layoutoptionen in Xamarin.iOS
description: Dieses Dokument beschreibt verschiedene Möglichkeiten zum Anordnen von Benutzeroberflächen in Xamarin.iOS. Automatisches Anpassen der Größe und automatisches Layout werden erörtert.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b35149028763691c17fe526673d023cc9b707c28
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61246064"
---
# <a name="layout-options-in-xamarinios"></a>Layoutoptionen in Xamarin.iOS

Es gibt zwei unterschiedliche Mechanismen für die Steuerung des Layouts, wenn eine Sicht geändert oder gedreht wird:

-  **Automatisches Anpassen der Größe** : Automatisches Anpassen der Größe der Inspektor im Designer bietet eine Möglichkeit zum Festlegen der `AutoresizingMask` Eigenschaften. Hiermit wird ein Steuerelement an die Ränder des Containers verankert werden bzw. ihre Größe zu beheben. Automatisches Anpassen der Größe wird in allen Versionen von iOS. Dies wird im folgenden ausführlicher beschrieben.
-  **Automatisches Layout** – eine Funktion, die in iOS 6, die ermöglicht eine präzisere Kontrolle über die Beziehungen zwischen der UI-Steuerelemente eingeführt. Sie können die Kontrolle über die Positionen der Elemente relativ zu anderen Elementen auf der Entwurfsoberfläche angezeigt. In diesem Thema wird ausführlicher behandelt die [Automatisches Layout mit dem Xamarin.IOS-Designer](~/ios/user-interface/designer/designer-auto-layout.md) Guide.

## <a name="autosizing"></a>Automatisches Anpassen der Größe

Wenn ein Benutzer ändert die Größe eines Fensters, z. B. wenn das Gerät gedreht wird, und die Änderungen der bildschirmausrichtung, wird das System automatisch Ansichten in diesem Fenster entsprechend ihren Regeln Automatisches Anpassen der Größe ändern. Dieser Regeln können festgelegt werden, C# mithilfe der `AutoresizingMask` Eigenschaft der `UIView` oder in der **Pad "Eigenschaften"** des iOS-Designer, wie unten gezeigt:

 [![](layout-options-images/image41.png "Visual Studio für Mac-Designer")](layout-options-images/image41.png#lightbox)

Wenn ein Steuerelement ausgewählt ist, dadurch können Sie manuell angeben, den Speicherort und den Abmessungen des Steuerelements, sowie die Auswahl **Automatisches Anpassen der Größe** Verhalten. Wie im folgenden Screenshot dargestellt wird, können wir die Federn und Struts definieren Sie die ausgewählte Ansicht Beziehung, da das übergeordnete Element im Steuerelement Automatisches Anpassen der Größe verwenden:

 [![](layout-options-images/image42.png "Visual Studio für Mac-Designer")](layout-options-images/image42.png#lightbox)

Anpassen einer *Spring* führt dazu, dass die Sicht zum Ändern der Größe basierend auf der Breite oder Höhe der ihr übergeordneten Ansicht. Anpassen einer *strut* veranlasst, dass die Sicht ein konstanter Abstand zwischen sich selbst und ihrer übergeordneten Ansicht, in diesem bestimmten Edge beibehalten.

Diese Einstellungen können auch im Code festgelegt werden:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Um die Einstellungen für automatisches Anpassen der Größe zu testen, aktivieren Sie verschiedene **unterstützte Geräteausrichtungen** in den Projektoptionen:

 [![](layout-options-images/image43a.png "Einstellungen für automatisches Anpassen der Größe")](layout-options-images/image43a.png#lightbox)

In der CodeBehind können wir folgenden Code verwenden, der bewirkt, dass die beiden Textsteuerelemente horizontal ändern der Größe:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Wir können auch das Steuerelement im Designer anpassen. Der Struts auswählen, wie unten gezeigt haben bewirkt, dass das Bild, das rechts ausgerichtete bleiben, ohne vom unteren Rand der Ansicht abgeschnitten wird:

 [![](layout-options-images/autoresize.png "Standardeinstellungen")](layout-options-images/autoresize.png#lightbox)

Diese Screenshots zeigen, wie Sie die Steuerelemente zum Ändern der Größe oder positionieren selbst, wenn der Bildschirm gedreht wird:

 [![](layout-options-images/image44a.png "Standardeinstellungen")](layout-options-images/image44a.png#lightbox)

Beachten Sie, dass die Textansicht Textfeld sowohl gestreckt, um den gleichen Links zu verwenden und Rändern, aufgrund rechten der `FlexibleWidth` festlegen. Das Image hat den oberen und linken Rand flexibel, was bedeutet, dass er behält die unteren und rechten Rändern – beibehalten der Abbildung in der Ansicht an, wenn der Bildschirm gedreht wird. Komplexe Layouts erfordern in der Regel eine Kombination dieser Einstellungen auf alle sichtbaren Steuerelements auf die Benutzeroberfläche in Einklang zu bringen und zu verhindern, dass Steuerelemente überlappen, wenn die Ansicht des Bereichs (aufgrund von Rotation oder eines anderen Ereignisses ändern der Größe) ändern.





## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
