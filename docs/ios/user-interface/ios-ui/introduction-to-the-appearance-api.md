---
title: API-Darstellung
description: iOS können Sie visuelle eigenschafteneinstellungen auf der Ebene einer statischen Klasse statt auf einzelne Objekte anwenden, damit die Änderung auf alle Instanzen des Steuerelements in der Anwendung gilt.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 7c7e4909cc12f49411c527af12fc0e4855979804
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="appearance-api"></a>API-Darstellung

_iOS können Sie visuelle eigenschafteneinstellungen auf der Ebene einer statischen Klasse statt auf einzelne Objekte anwenden, damit die Änderung auf alle Instanzen des Steuerelements in der Anwendung gilt._

Diese Funktionalität wird in Xamarin.iOS bereitgestellt, über einen statischen `Appearance` Eigenschaft für alle UIKit-Steuerelemente, die sie unterstützen. Visuelle Darstellung (Eigenschaften wie als Bild für Farbton Farbe und Hintergrund) kann daher problemlos angepasst werden, auf um Ihre Anwendung ein einheitliches Erscheinungsbild zu verleihen. Die Darstellung-API wurde in iOS 5 eingeführt, und zwar einige Teile davon in iOS 9 veraltet sind ist immer noch eine gute Möglichkeit, einige Erstellen von Formaten und Designumgebung Effekte in Xamarin.iOS apps zu erzielen.

## <a name="overview"></a>Übersicht

iOS können Sie anpassen, die Darstellung von vielen UIKit Steuerelemente stellen die Standardsteuerelemente entsprechen, um das branding, die, das Sie für Ihre Anwendung anwenden möchten.

Es gibt zwei Möglichkeiten, um eine benutzerdefinierte Darstellung anzuwenden:

- **Direkt auf eine Instanz eines Steuerelements** – Sie können den Farbton, Hintergrundbild Titelposition (sowie einige andere Attribute) für viele Steuerelemente, einschließlich Symbolleisten, Navigationsleisten, Schaltflächen und Schieberegler festlegen.

- **Festlegen der Standardeinstellungen für die Darstellung statische Eigenschaft** – anpassbare Attribute für jedes Steuerelement über verfügbar gemachte sind die `Appearance` statische Eigenschaft. Alle Anpassungen, die Sie für diese Eigenschaften gelten werden als Standard für jedes Steuerelement dieses Typs verwendet werden, die erstellt wird, nachdem die Eigenschaft festgelegt ist.

Die beispielanwendung Darstellung zeigt alle drei Methoden, wie in den folgenden Screenshots dargestellt:

 [![](introduction-to-the-appearance-api-images/appearance01.png "Die Darstellung-beispielanwendung für veranschaulicht alle drei Methoden")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Der Proxy für die Darstellung wurde ab iOS 8 um TraitCollections erweitert.
 `AppearanceForTraitCollection` kann verwendet werden, um die standarddarstellung für ein bestimmtes Merkmal Auflistung festgelegt. Erfahren Sie mehr Informationen finden Sie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) Handbuch.


## <a name="setting-appearance-properties"></a>Festlegen von Darstellungseigenschaften

Im ersten Bildschirm ist die statische Darstellung-Klasse zum Formatieren von Schaltflächen und gelb/Orange Elemente wie folgt verwendet:

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

Die grünen Element werden festgelegt, in der `ViewDidLoad` Methode die Standardwerte überschreibt und die *Darstellung* statische Klasse:

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

Die API Darstellung kann nützlich sein, wenn [formatieren die iOS-app](~/xamarin-forms/platform/ios/theme.md#uiappearance) in Xamarin.Forms-Lösungen. Einige Zeilen in der `AppDelegate` Klasse kann dazu beitragen, um ein bestimmtes Farbschema zu implementieren, ohne Erstellen einer [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Benutzerdefinierte Designs und UIAppearance

iOS viele visuellen Attribute des Benutzers kann Steuerelemente der Benutzeroberfläche mithilfe von "Designs" werden die *UIAppearance* APIs den Zugriff auf alle Instanzen eines bestimmten Steuerelements auf die gleiche Darstellung haben zu erzwingen. Dies wird als Darstellungseigenschaft auf viele Benutzer Schnittstelle-Steuerelementklassen und nicht auf einzelner Instanzen des Steuerelements verfügbar gemacht. Festlegen einer Display-Eigenschaft auf die statische `Appearance` Eigenschaft wirkt sich auf alle Steuerelemente des Typs in der Anwendung.

Betrachten Sie ein Beispiel, um besser zu verstehen das Konzept.

So ändern Sie einen bestimmten `UISegmentedControl` um Magenta Farbton haben, würden wir verweisen auf bestimmte Steuerelement auf unserem Bildschirm wie folgt in `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Legen Sie alternativ den Wert in das Auffüllzeichen Eigenschaften des Designers: 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Eigenschaften mit Leerstellen auffüllen Farbton")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

Die folgende Abbildung veranschaulicht, die dies den Farbton für nur auf das Steuerelement, mit dem Namen "sg1" festlegt.

 [![](introduction-to-the-appearance-api-images/image53.png "Den Farbton einzelnes Steuerelement festlegen")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Viele Steuerelemente, die auf diese Weise festgelegt wäre vollständig ineffizient, daher können wir stattdessen die statische festlegen `Appearance` Eigenschaft für die Klasse selbst. Dies wird im Code unten gezeigt:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Die folgende Abbildung veranschaulicht beide segmentierte Steuerelemente jetzt mit der Darstellung auf Magenta festgelegt:

 [![](introduction-to-the-appearance-api-images/image54.png "Festlegen des Darstellung Steuerelement Farbtons")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` Eigenschaften sollte festgelegt werden einem frühen Zeitpunkt im Lebenszyklus Anwendung, z. B. in der AppDelegate `FinishedLaunching` -Ereignis oder in einem ViewController, damit die betroffenen Steuerelemente angezeigt werden.


Finden Sie in der [Einführung in die Darstellung API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) detailliertere Informationen.


## <a name="related-links"></a>Verwandte Links

- [Darstellung (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [UIAppearance-Protokollreferenz](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Darstellung in Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
