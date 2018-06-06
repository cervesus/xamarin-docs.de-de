---
title: Layoutoptionen in Xamarin.iOS
description: Dieses Dokument beschreibt verschiedene Möglichkeiten zum Erstellen von Benutzeroberflächen in Xamarin.iOS. Es wird erläutert, automatisches Anpassen der Größe und automatisches Layout.
ms.prod: xamarin
ms.assetid: D8180FEC-F300-42C0-B029-66803E0C1A5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bad29eae308c8ca9f7228a1cbdfd69940894cf34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790115"
---
# <a name="layout-options-in-xamarinios"></a>Layoutoptionen in Xamarin.iOS

Es gibt zwei unterschiedliche Mechanismen für das Layout steuern, wenn eine Sicht geändert oder gedreht wird:

-  **Automatisches Anpassen der Größe** – Automatisches Anpassen der Größe der Inspektor im Designer bietet eine Möglichkeit zum Festlegen der `AutoresizingMask` Eigenschaften. Auf diese Weise können ein Steuerelement verankert werden, um die Ränder des Containers und/oder ihre Größe zu beheben. Automatisches Anpassen der Größe funktioniert in allen Versionen von iOS. Dies wird im folgenden ausführlicher beschrieben.
-  **Auto-Layout** – ein Feature, eingeführt in iOS 6, die eine präzisere Kontrolle über die Beziehungen zwischen den UI-Steuerelementen ermöglicht. Kontrolle über die Positionen von Elementen relativ zu anderen Elementen auf der Entwurfsoberfläche wird zugelassen. In diesem Thema wird ausführlich in die [Automatisches Layout mit dem Xamarin iOS-Designer](~/ios/user-interface/designer/designer-auto-layout.md) Handbuch.

## <a name="autosizing"></a>Automatisches Anpassen der Größe

Wenn ein Benutzer ändert die Größe eines Fensters, z. B. wenn das Gerät ein scrollvorgang ausgeführt werden und die Ausrichtung geändert wird, wird das System automatisch die Ansichten innerhalb dieses Fensters nach ihren Regeln Automatisches Anpassen der Größe angepasst. Diese Regeln können festgelegt werden in c# mit der `AutoresizingMask` Eigenschaft von der `UIView` oder in der **Eigenschaften aufgefüllt** des iOS-Designer, wie unten gezeigt:

 [![](layout-options-images/image41.png "Visual Studio für Mac-Designer")](layout-options-images/image41.png#lightbox)

Wenn ein Steuerelement ausgewählt ist, dadurch können Sie manuell den Speicherort und den Dimensionen des Steuerelements angeben sowie auswählen **Automatisches Anpassen der Größe** Verhalten. Wie im folgenden Screenshot gezeigt, wir die Springs und Struts können im Steuerelement Automatisches Anpassen der Größe Sie um die ausgewählte Ansicht Beziehung zu seinem übergeordneten Element zu definieren:

 [![](layout-options-images/image42.png "Visual Studio für Mac-Designer")](layout-options-images/image42.png#lightbox)

Anpassen einer *Spring* führt dazu, dass die Ansicht, um die Größe von auf Grundlage der Breite oder Höhe der ihr übergeordneten Ansicht. Anpassen einer *strut* veranlasst, dass die Sicht ein konstanter Abstand zwischen sich selbst und seine übergeordneten Ansicht, auf bestimmte Kante beibehalten.

Diese Einstellungen können auch im Code festgelegt werden:

```csharp
textfield1.Frame = new RectangleF(15, 277, 79, 27);
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleRightMargin | UIViewAutoresizing.FlexibleBottomMargin;
```


Aktivieren Sie zum Testen der servereinstellungen Automatisches Anpassen der Größe unterschiedliche **unterstützt Gerät Ausrichtungen** in das Projekt Optionen:

 [![](layout-options-images/image43a.png "Automatisches Anpassen der Größe-Einstellungen")](layout-options-images/image43a.png#lightbox)

Im Code-behind können wir den folgenden Code verwenden, der bewirkt, dass die zwei Textsteuerelemente horizontal Größe:

```csharp
textview1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
textfield1.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
imageview1.AutoresizingMask = UIViewAutoresizing.FlexibleTopMargin | UIViewAutoresizing.FlexibleLeftMargin;
```


Es können auch das Steuerelement im Designer anpassen. Die Struts auswählen, wie unten gezeigt führt dazu, dass das Bild, das rechts ausgerichtet bleiben, ohne dass vom unteren Rand der Ansicht abgeschnitten wird:

 [![](layout-options-images/autoresize.png "Standardeinstellungen")](layout-options-images/autoresize.png#lightbox)

Diese Screenshots zeigen, wie Sie die Steuerelemente zum Ändern der Größe oder positionieren selbst, wenn der Bildschirm gedreht wird:

 [![](layout-options-images/image44a.png "Standardeinstellungen")](layout-options-images/image44a.png#lightbox)

Beachten Sie, dass der Textansicht Textfeld sowohl gestreckt, um Links identisch zu halten und mit der rechten Ränder, aufgrund Maustaste der `FlexibleWidth` Einstellung. Das Bild hat den oberen und linken Rand flexible, d. h. er beibehält, unteren und rechten Ränder – beibehalten der Abbildung in der Ansicht, wenn der Bildschirm gedreht wird. Komplexe Layouts erfordern in der Regel eine Kombination dieser Einstellungen auf alle sichtbaren Steuerelements, die Benutzeroberfläche konsistent zu halten und zu verhindern, dass Steuerelemente überlappende Grenzen für die Sicht (aufgrund der Rotation oder einer anderen Größe) zu ändern.





## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
