---
title: Darstellungs-API in Xamarin.iOS
description: iOS können Sie visual eigenschafteneinstellungen, die auf einer statischen Klasse-Ebene und nicht für einzelne Objekte anwenden, damit die Änderung für alle Instanzen des Steuerelements in der Anwendung gilt.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0dd9832a2e4dd0803f92d6e3923fe178252211f4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103568"
---
# <a name="appearance-api-in-xamarinios"></a>Darstellungs-API in Xamarin.iOS

_iOS können Sie visual eigenschafteneinstellungen, die auf einer statischen Klasse-Ebene und nicht für einzelne Objekte anwenden, damit die Änderung für alle Instanzen des Steuerelements in der Anwendung gilt._

Diese Funktionalität ist in Xamarin.iOS verfügbar, über eine statische `Appearance` Eigenschaft bei allen UIKit-Steuerelementen, die dies unterstützen. Visuelle Darstellung (Eigenschaften wie als tönungsbild Farbe und Hintergrund) kann daher leicht angepasst werden, um Ihre Anwendung ein einheitliches Aussehen geben. Die Darstellung-API wurde in iOS 5 eingeführt und während einige Teile des Zertifikats in iOS 9 veraltet sind dies immer noch eine gute Möglichkeit, einige Stile und Designs Auswirkungen in Xamarin.iOS-apps zu erzielen.

## <a name="overview"></a>Übersicht

iOS ermöglicht, dass Sie die Darstellung von vielen UIKit-Steuerelementen zu entsprechen, um das branding, die, das Sie für Ihre Anwendung anwenden möchten, Standardsteuerelemente anpassen.

Es gibt zwei Möglichkeiten, eine benutzerdefinierte Darstellung anwenden:

- **Direkt auf eine Steuerelementinstanz** – Sie können die tönungsfarbe, Hintergrundbild Titelposition (sowie andere Attribute) für viele Steuerelemente wie Symbolleisten, Navigationsleisten, Schaltflächen und Regler festlegen.

- **Festlegen von Standardeinstellungen für die statische Eigenschaft Darstellung** – anpassbare Attribute für jedes Steuerelement werden verfügbar gemacht, über die `Appearance` statische Eigenschaft. Alle Anpassungen, die Sie auf diese Eigenschaften anwenden, werden als Standard für beliebige Steuerelemente des betreffenden Typs verwendet werden, die erstellt wird, nachdem die Eigenschaft festgelegt ist.

Die beispielanwendung für die Darstellung zeigt alle drei Methoden, wie im folgenden Screenshots gezeigt:

 [![](introduction-to-the-appearance-api-images/appearance01.png "Die Darstellung-beispielanwendung zeigt alle drei Methoden")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Ab iOS 8 wurde der Proxy für die Darstellung auf TraitCollections erweitert.
 `AppearanceForTraitCollection` kann verwendet werden, um die standarddarstellung für ein bestimmtes Merkmal Sammlung festlegen möchten. Weitere Informationen finden Sie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) Guide.


## <a name="setting-appearance-properties"></a>Festlegen von Darstellungseigenschaften

Im ersten Bildschirm wird die statische Darstellung-Klasse so formatieren Sie die Schaltflächen und gelb/Orange-Elemente wie folgt verwendet:

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

Das grüne Element werden festgelegt, wie die folgende, in der `ViewDidLoad` Methode, wobei die Standardwerte außer Kraft gesetzt und die *Darstellung* statische Klasse:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Verwenden von UIAppearance in Xamarin.Forms

Die API für die Darstellung kann nützlich sein, wenn [formatieren die iOS-app](~/xamarin-forms/platform/ios/theme.md#uiappearance) in Xamarin.Forms-Projektmappen. Ein paar Zeilen in der `AppDelegate` Klasse kann dazu beitragen, um ein bestimmtes Farbschema zu implementieren, ohne Erstellen einer [benutzerdefinierter Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Benutzerdefinierte Designs und UIAppearance

iOS kann viele visuellen Attribute des Benutzers Steuerelemente der Benutzeroberfläche "Design" Verwendung der *UIAppearance* APIs zu erzwingen, dass alle Instanzen eines bestimmten Steuerelements auf die gleiche Darstellung haben. Dies wird als ein Appearance-Eigenschaft, die für viele Schnittstelle Benutzersteuerelement-Klassen, nicht für einzelne Instanzen des Steuerelements verfügbar gemacht. Festlegen einer Anzeigeeigenschaft auf die statische `Appearance` Eigenschaft wirkt sich auf alle Steuerelemente des Typs in Ihrer Anwendung.

Um das Konzept besser zu verstehen, betrachten Sie ein Beispiel aus.

So ändern Sie einen bestimmten `UISegmentedControl` um einen Magenta Farbton, würden wir verweisen, das bestimmte Steuerelement auf unserem Bildschirm wie folgt im `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Alternativ legen Sie den Wert im Bereich "Eigenschaften" des Designers: 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Eigenschaften Pad Farbton")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

Die folgende Abbildung veranschaulicht, die diese den Farbton auf nur das Steuerelement, mit dem Namen "sg1" festlegt.

 [![](introduction-to-the-appearance-api-images/image53.png "Den Farbton einzelnes Steuerelement festlegen")](introduction-to-the-appearance-api-images/image53.png#lightbox)

So legen Sie viele Steuerelemente auf diese Weise wäre vollständig ineffizient, damit wir stattdessen die statische festlegen können `Appearance` Eigenschaft in der Klasse selbst. Dies wird im folgenden Code gezeigt:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Die folgende Abbildung veranschaulicht sowohl segmentierte Steuerelemente jetzt mit dem Erscheinungsbild auf Magenta festgelegt:

 [![](introduction-to-the-appearance-api-images/image54.png "Festlegen des Darstellung Steuerelement Farbtons")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` Eigenschaften sollten festgelegt werden früh im Lebenszyklus von Anwendungen, z. B. in der AppDelegate `FinishedLaunching` Ereignis oder in einem ViewController, damit die betroffenen Steuerelemente angezeigt werden.


Finden Sie in der [Einführung in die API für die Darstellung](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) detailliertere Informationen.


## <a name="related-links"></a>Verwandte Links

- [Darstellung (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance-Protokollreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Darstellung in Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
