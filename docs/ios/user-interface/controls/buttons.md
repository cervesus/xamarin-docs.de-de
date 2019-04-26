---
title: Schaltflächen in Xamarin.iOS
description: Die UIButton-Klasse wird zum Darstellen von verschiedenen verschiedene Arten der Schaltfläche in iOS-Bildschirmen verwendet. Dieser Leitfaden beschreibt die verschiedenen Optionen für die Arbeit mit Schaltflächen in iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2018
ms.openlocfilehash: a98ddc2622682f2c105a6aff32e94bd92a5b11f2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896492"
---
# <a name="buttons-in-xamarinios"></a>Schaltflächen in Xamarin.iOS

Bei iOS der `UIButton` Klasse stellt ein Schaltflächen-Steuerelement dar.

Eine Schaltfläche mit den Eigenschaften können geändert werden, entweder programmgesteuert oder mit der **Pad "Eigenschaften"** von iOS-Designer:

![Das Pad "Eigenschaften" des iOS Designers](buttons-images/properties.png "das Pad \"Eigenschaften\" der iOS-Designer")

## <a name="creating-a-button-programmatically"></a>Programmgesteuertes Erstellen einer Schaltfläche

Ein `UIButton` kann mit nur wenigen Codezeilen erstellt werden.

- Eine Schaltfläche instanziieren und ihren Typ festlegen:

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  Die Schaltfläche "-Typ wird angegeben, indem eine `UIButtonType`:

  - `UIButtonType.System` – Ein allgemeines Schaltfläche
  - `UIButtonType.DetailDisclosure` – Gibt die Verfügbarkeit mit detaillierten Informationen, in der Regel zu einem bestimmten Element in einer Tabelle
  - `UIButtonType.InfoDark` – Gibt die Verfügbarkeit von Konfigurationsinformationen; Dunkel farbigen
  - `UIButtonType.InfoLight` – Gibt die Verfügbarkeit von Konfigurationsinformationen; helle Farben
  - `UIButtonType..AddContact` : Gibt an, dass ein Kontakt hinzugefügt werden können
  - `UIButtonType.Custom` -Anpassbare Schaltfläche

  Weitere Informationen zu den verschiedenen Arten sehen Sie sich:
  
  - Die [benutzerdefinierte Schaltflächentypen](#custom-button-types) Abschnitt dieses Dokuments
  - Die [Schaltfläche Typen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) Anleitung
  - Apple [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/).

- Definieren Sie die Größe und Position der Schaltfläche:

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- Der Text der Schaltfläche fest Verwenden der `SetTitle` -Methode, die den Text erfordert und eine `UIControlState` Wert:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  Weitere Informationen über eine Schaltfläche zu formatieren und den Text festlegen finden Sie unter:

  - Die [Formatieren einer Schaltfläche](#styling-a-button) Abschnitt dieses Dokuments
  - Die [Festlegen des Schaltflächentexts](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) Anleitung.

## <a name="handling-a-button-tap"></a>Behandeln von berühren einer Schaltfläche

Um auf Knopfdruck reagieren, geben Sie einen Handler für der Schaltfläche " `TouchUpInside` Ereignis:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` ist nicht das einzige verfügbare Schaltflächenereignis. `UIButton` ist eine untergeordnete Klasse der `UIControl`, die definiert, [viele verschiedene Ereignisse](xref:UIKit.UIControlEvent).

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>Verwendung des iOS Designers an Ereignishandler für Schaltflächen

Verwenden der **Ereignisse** Registerkarte die **Pad "Eigenschaften"** Ereignishandler an, für eine Schaltfläche auf verschiedene Ereignisse ist.

Geben Sie für das entsprechende Ereignis aus den Namen des neuen Ereignishandler oder aus der Liste auswählen. Dadurch wird einen Ereignishandler im Code für die Schaltfläche "-View-Controller erstellt.

![Registerkarte "Ereignisse" des Eigenschaftenpads](buttons-images/image1.png "Registerkarte \"Ereignisse\" des Eigenschaftenpads")

## <a name="styling-a-button"></a>Formatieren einer Schaltfläche

`UIButton` Steuerelemente können in einer Reihe verschiedener Zustände vorhanden, jede gemäß einem `UIControlState` Wert – `Normal`, `Disabled`, `Focused`, `Highlighted`usw. Jeder Zustand kann ein eindeutiges Format, die programmgesteuert oder mit der iOS-Designer angegeben angegeben werden.

> [!NOTE]
> Eine vollständige Liste aller `UIControlState` Werte sehen Sie sich die [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> Dokumentation.

Um beispielsweise die Farbe des Titels und die Schattenfarbe für festlegen `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Der folgende Code legt den Titel der Schaltfläche in einem attributierten (stilisierte) Zeichenfolge für die `UIControlState.Normal` und `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Benutzerdefinierte Schaltflächentypen

Schaltflächen mit einer `UIButtonType` von `Custom` keine Standardstile haben. Allerdings ist es möglich, die Darstellung der Schaltfläche durch Festlegen eines Bilds für die verschiedenen Zustände zu konfigurieren:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

Je nachdem, ob der Benutzer die Schaltfläche oder nicht berührt, wird es als eines der folgenden Images gerendert (`UIControlState.Normal`, `UIControlState.Highlighted` und `UIControlState.Selected` angibt, bzw.):

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted")
![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

Weitere Informationen zum Arbeiten mit benutzerdefinierten Schaltflächen finden Sie in der [verwenden Sie ein Image für eine Schaltfläche](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) Anleitung.

