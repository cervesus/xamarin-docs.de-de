---
title: Darstellungs-API in xamarin. IOS
description: mit IOS können Sie visuelle Eigenschafts Einstellungen auf einer statischen Klassenebene anstelle einzelner Objekte anwenden, sodass die Änderung für alle Instanzen dieses Steuer Elements in der Anwendung gilt.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/15/2018
ms.openlocfilehash: e2dcd5ea0f099ea84d7824eda4170df8efb22cb6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937214"
---
# <a name="appearance-api-in-xamarinios"></a>Darstellungs-API in xamarin. IOS

_mit IOS können Sie visuelle Eigenschafts Einstellungen auf einer statischen Klassenebene anstelle einzelner Objekte anwenden, sodass die Änderung für alle Instanzen dieses Steuer Elements in der Anwendung gilt._

Diese Funktionalität wird in xamarin. IOS über eine statische `Appearance` Eigenschaft für alle UIKit-Steuerelemente verfügbar gemacht, die diese Funktion unterstützen. Visuelle Darstellung (Eigenschaften wie z. b. Tönungs-Farbe und Hintergrundbild) können daher problemlos angepasst werden, um der Anwendung ein konsistentes Erscheinungsbild zu geben. Die Darstellungs-API wurde in ios 5 eingeführt, und einige Teile davon wurden in ios 9 als veraltet markiert. es ist immer noch eine gute Möglichkeit, einige Formatierungs-und Design Effekte in xamarin. IOS-apps zu erzielen.

## <a name="overview"></a>Übersicht

IOS ermöglicht Ihnen die Anpassung der Darstellung vieler UIKit-Steuerelemente, damit die Standard Steuerelemente dem Branding entsprechen, das Sie auf Ihre Anwendung anwenden möchten.

Es gibt zwei verschiedene Möglichkeiten, ein benutzerdefiniertes aussehen anzuwenden:

- **Direkt auf einer Steuerelement Instanz** – Sie können die Tönungs-Farbe, das Hintergrundbild und die Titel Position (sowie einige andere Attribute) für viele Steuerelemente festlegen, einschließlich Symbolleisten, Navigationsleisten, Schaltflächen und Schieberegler.

- **Legen Sie Standardwerte für die statische Darstellung der Darstellung fest** – anpassbare Attribute für jedes Steuerelement werden über die `Appearance` statische Eigenschaft verfügbar gemacht. Alle Anpassungen, die Sie auf diese Eigenschaften anwenden, werden als Standard für alle Steuerelemente dieses Typs verwendet, der erstellt wird, nachdem die-Eigenschaft festgelegt wurde.

Die Beispielanwendung für die Darstellung veranschaulicht alle drei Methoden, wie in den folgenden Screenshots gezeigt:

[![Die Beispielanwendung "Darstellung" veranschaulicht alle drei Methoden.](introduction-to-the-appearance-api-images/appearance01-sml.png)](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Ab IOS 8 wurde der Darstellungs Proxy auf traitcollections erweitert.
 `AppearanceForTraitCollection`kann verwendet werden, um die Standarddarstellung für eine bestimmte Merkmals Auflistung festzulegen. Weitere Informationen hierzu finden Sie im Handbuch [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) ).

## <a name="setting-appearance-properties"></a>Festlegen von Darstellungs Eigenschaften

Im ersten Bildschirm wird die statische Darstellungs Klasse verwendet, um die Schaltflächen und gelben/orangefarbenen Elemente wie folgt zu formatieren:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Die grünen Element Stile werden wie folgt festgelegt, in der `ViewDidLoad` -Methode, die die Standardwerte und *Appearance* die statische Darstellungs Klasse überschreibt:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Verwenden von uiappearance in xamarin. Forms

Die Darstellungs-API kann nützlich sein, wenn Sie [die IOS-App](~/xamarin-forms/platform/ios/formatting.md#uiappearance-api) in xamarin. Forms-Lösungen formatieren. Einige Zeilen in der- `AppDelegate` Klasse können dabei helfen, ein bestimmtes Farbschema zu implementieren, ohne einen [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)erstellen zu müssen.

### <a name="custom-themes-and-uiappearance"></a>Benutzerdefinierte Designs und uiappearance

IOS ermöglicht, dass viele visuelle Attribute der Steuerelemente der Benutzeroberfläche mithilfe der *uiappearance* -APIs "thematisiert" werden, um zu erzwingen, dass alle Instanzen eines bestimmten Steuer Elements dieselbe Darstellung aufweisen. Diese wird als Darstellungs Eigenschaft für viele Klassen von Benutzeroberflächen-Steuerelementen verfügbar gemacht, nicht für einzelne Instanzen des Steuer Elements. Das Festlegen einer Anzeige Eigenschaft für die statische `Appearance` Eigenschaft wirkt sich auf alle Steuerelemente dieses Typs in der Anwendung aus.

Um das Konzept besser zu verstehen, sehen Sie sich ein Beispiel an.

Um einen bestimmten `UISegmentedControl` so zu ändern, dass er ein Magenta-tint hat, verweisen wir wie folgt auf das spezifische Steuerelement auf unserem Bildschirm `ViewDidLoad` :

```csharp
sg1.TintColor = UIColor.Magenta;
```

Alternativ können Sie den Wert im eigenschaftenpad des Designers festlegen:

[![Eigenschaftenpad tint](introduction-to-the-appearance-api-images/propertiespadtint.png)](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

Die folgende Abbildung zeigt, dass das Tönungs nur auf dem Steuerelement mit dem Namen "SG1" festgelegt wird.

[![Festlegen der einzelnen Steuerelement-Tönungs](introduction-to-the-appearance-api-images/image53.png)](introduction-to-the-appearance-api-images/image53.png#lightbox)

Um viele Steuerelemente auf diese Weise festzulegen, wäre es vollständig ineffizient, sodass wir stattdessen die statische `Appearance` Eigenschaft für die Klasse selbst festlegen können. Dies wird im folgenden Code gezeigt:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Das Bild unten zeigt nun beide segmentierten Steuerelemente, deren Darstellung auf Magenta festgelegt ist:

[![Festlegen des Erscheinungs Bilds](introduction-to-the-appearance-api-images/image54.png)](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance`Eigenschaften sollten früh im Anwendungslebenszyklus festgelegt werden, z. b. im Ereignis des appdelegaten `FinishedLaunching` oder in einem ViewController, bevor die betroffenen Steuerelemente angezeigt werden.

Ausführlichere Informationen finden Sie unter [Einführung in die](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) Darstellungs-API.

## <a name="related-links"></a>Ähnliche Themen

- [Darstellung (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/appearance)
- [Uiappearance-Protokoll Referenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Darstellung in xamarin. Forms](~/xamarin-forms/platform/ios/formatting.md#uiappearance-api)
