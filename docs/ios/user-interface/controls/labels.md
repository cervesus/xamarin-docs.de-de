---
title: Bezeichnungen
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 0fdeecc4465aa5709b452ef0b591ec4e5c262e3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="labels"></a>Bezeichnungen

Die `UILabel` Steuerelement dient zum Anzeigen von einzelnen und mehreren Zeilen nur-Text zu lesen. 

## <a name="implementing-a-label"></a>Implementieren eine Bezeichnung

Erstellt eine neue Bezeichnung durch Instanziieren einer [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Bezeichnungen und Storyboards

Sie können auch eine Bezeichnung an die Benutzeroberfläche hinzufügen, wenn die iOS-Designer verwenden. Suchen Sie nach **Bezeichnung** in der **Toolbox** und ziehen Sie es in Ihrer Ansicht:

![-Bezeichnung in toolbox](labels-images/image3.png)

Die folgenden Eigenschaften können auf das Auffüllzeichen Eigenschaften angepasst werden:

![Bereich für Bezeichnung-Eigenschaft](labels-images/image2.png)

- **Textkontext** – nur-Text oder attributierte. Nur-Text können Sie festlegen der [Formatierungsattribute](#Formatting_Text_and_Label) auf die gesamte Zeichenfolge. Attributierte Texte können Sie festlegen, auf unterschiedliche Zeichen oder Wörtern in der Zeichenfolge formatieren.
- **Farbe, Schriftart Ausrichtung** – Formatierung Attribute, die an die Bezeichnung angewendet werden können.
- **Zeilen** – legt die Anzahl der Zeilen, die die Bezeichnung umspannen kann. Legen Sie diesen Wert auf 0, um die Bezeichnung an, wie viele Zeilen nach Bedarf verwenden zu können.
- **Verhalten** – kann festgelegt werden, um entweder "Enabled" oder "hervorgehoben. Die Aktivierung ist standardmäßig festgelegt, deaktivierter Text erscheint in einer helleren grauer Farbe. Hervorgehoben ist standardmäßig deaktiviert, und ermöglicht die Bezeichnung an, wenn es von einem Benutzer ausgewählt wird mit einem hervorgehobenen Zustand neu gezeichnet wird.
- **Baselane und Zeilenumbruch** – 
    - Der Grundwerte bestimmt, wie der Text positioniert wird, wenn die Schriftgrade Ihren Bedürfnissen unterscheidet sich von dem angegebenen.
    - Zeilenumbrüche bestimmen, wie eine Zeichenfolge eingeschlossen oder wird abgeschnitten, wenn es mehr als eine einzelne Zeile ist.
- **Autoshrink** – bestimmt wie die Schriftart, Größe innerhalb einer Bezeichnung, minimiert werden, bei Bedarf.
- **Hervorgehoben, Schatten, Offset** – können Sie die Farbe hervorgehobener und Schattenkopien festlegen und die für Schattenoffset.

## <a name="truncating-and-wrapping"></a>Abschneiden sowie das Umbrechen

Für Informationen über die Befehlszeile in iOS unterbrochen wird, finden Sie in der [abgeschnitten und Textumbruch](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) Rezept.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Formatieren von Text und Bezeichnung

Zur Formatierung der Zeichenfolge, die Sie in einer Bezeichnung verwenden, können Sie entweder Formatierungsattribute auf die gesamte Zeichenfolge festgelegt, oder attributierte Zeichenfolgen verwenden. Die folgenden Beispiele zeigen, wie diese implementieren:

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

Weitere Informationen zur Verwendung von Stilen Text `NSAttributedString` finden Sie in der [Textstil](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) Rezept.

Bezeichnungen haben standardmäßig die `Enabled` auf True festgelegt, aber es ist möglich, festgelegt ist, dass deaktiviert, damit dem Benutzer einen Hinweis erhalten, dass ein bestimmtes Steuerelement deaktiviert ist:

```csharp
label.Enabled = false;
```

Dies legt die Bezeichnung eine helle grauer Farbe fest, wie in der folgenden Abbildung eines Beispiel des Bildschirms Einschränkungen in iOS-dargestellt:

![Deaktivierte Schaltfläche im iOS](labels-images/image1.png)

Sie können auch Textfarben hervorheben und Schattenkopien auf Ihrem Beschriftungstext für zusätzliche Effekte festlegen:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Die den Text wie folgt angezeigt:

![Hervorhebung und Schattenkopien festlegen für text](labels-images/image4.png)

Weitere Informationen zum Ändern der Schriftart von einer UILabel finden Sie in der [Ändern der Schriftart](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) Rezept.





