---
title: Bezeichnungen in Xamarin.iOS
description: In diesem Dokument wird erläutert, wie Bezeichnungen in Xamarin.iOS verwendet wird. Es wird beschrieben, wie Bezeichnungen programmgesteuert oder mit der iOS-Designer erstellen.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: cca74ac74e5077822193f6dd97a69f8d9b823561
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227828"
---
# <a name="labels-in-xamarinios"></a>Bezeichnungen in Xamarin.iOS

Die `UILabel` Steuerelement dient zum Anzeigen von einzelnen und mehrzeilige, nur-Text zu lesen. 

## <a name="implementing-a-label"></a>Implementieren eine Bezeichnung

Eine neue Bezeichnung wird erstellt, durch die Instanziierung einer [ `UILabel` ](xref:UIKit.UILabel):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Bezeichnungen und Storyboards

Sie können auch eine Bezeichnung an Ihrer Benutzeroberfläche hinzufügen, bei Verwendung des iOS-Designers. Suchen Sie nach **Bezeichnung** in die **Toolbox** und ziehen Sie es in der Ansicht:

![-Bezeichnung in toolbox](labels-images/image3.png)

Die folgenden Eigenschaften können auf das Pad "Eigenschaften" angepasst werden:

![Bereich für Bezeichnung-Eigenschaft](labels-images/image2.png)

- **Textkontext** : einfache oder attributierte. Nur-Text können Sie die [Formatierungsattribute](#Formatting_Text_and_Label) auf die gesamte Zeichenfolge. Attributierte Texte können Sie festlegen, andere Zeichen oder Wörtern in der Zeichenfolge formatieren.
- **Farbe, Schriftart Ausrichtung** – Formatierung Attribute, die auf die Bezeichnung angewendet werden können.
- **Zeilen** – legt die Anzahl der Zeilen, die die Bezeichnung erstrecken können. Legen Sie diesen Wert auf 0, um die Größe der Bezeichnung auf beliebig viele Positionen nach Bedarf verwenden.
- **Verhalten** – kann entweder "aktiviert" oder "hervorgehobener festgelegt werden. Aktiviert ist standardmäßig festgelegt, deaktivierten Text erscheint in einer helleren graue Farbe. Hervorgehoben ist standardmäßig deaktiviert, und die Bezeichnung aus, wenn sie von einem Benutzer ausgewählt wird mit dem markierten Status neu gezeichnet werden können.
- **Baselane und Zeilenumbruch** : 
    - Der Grundwerte bestimmt, wie der Text positioniert, wenn die Schriftgrade unterscheidet sich von dem angegebenen.
    - Zeilenumbrüche bestimmen, wie eine Zeichenfolge eingeschlossen oder wird abgeschnitten, wenn er länger als eine einzelne Zeile ist.
- **Automatische Verkleinerung** – bestimmt wie die Schriftart, Größe in eine Bezeichnung, minimiert wird, falls erforderlich.
- **Hervorgehoben, Schatten, Offset** – können Sie die Farbe der markiert und Schattenkopien festgelegt, und der Schatten versetzt.

## <a name="truncating-and-wrapping"></a>Abschneiden sowie das Umbrechen

Informationen über die Befehlszeile in iOS unterbrochen wird, finden Sie in der [abgeschnitten und Umbrechen von Text](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) Anleitung.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Formatieren von Text und Beschriftung

Klicken Sie zum Formatieren der Zeichenfolge, die Sie in einer Bezeichnung verwenden können Sie entweder die Formatierungsattribute auf die gesamte Zeichenfolge, oder Sie können attributierte Zeichenfolgen verwenden. Die folgenden Beispiele zeigen, wie dies implementiert wird:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Weitere Informationen zur Verwendung von formatieren Text `NSAttributedString` finden Sie in der [Text mit dem Format](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) Anleitung.

Standardmäßig Bezeichnungen auf der `Enabled` auf True festgelegt, aber es ist möglich, festgelegt ist, dass deaktiviert, um dem Benutzer geben einen Hinweis, dass ein bestimmtes Steuerelement deaktiviert ist:

```csharp
label.Enabled = false;
```

Hiermit wird die Bezeichnung auf eine einfache graue Farbe an, wie im folgenden Beispielbild des Bildschirms Einschränkungen in iOS dargestellt:

![Deaktivierte Schaltfläche im iOS](labels-images/image1.png)

Sie können auch die Textfarben hervorheben und Schatten zu Ihrem Beschriftungstext für weitere Effekte festlegen:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Die den Text wie folgt angezeigt:

![Hervorhebung und Schattenkopien, die für Text festlegen](labels-images/image4.png)

Weitere Informationen zum Ändern der Schriftart von einer UILabel finden Sie in der [die Schriftart ändern](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) Anleitung.





