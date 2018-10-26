---
title: Einschränkungen für programmgesteuerte Layouts in Xamarin.iOS
description: Dieses Handbuch bietet arbeiten mit iOS Automatisches Layout-Einschränkungen in C# Code nicht in der iOS-Designer erstellt.
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 3d8e69af7f790415343abf464ea2bb22e879e025
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106960"
---
# <a name="programmatic-layout-constraints-in-xamarinios"></a>Einschränkungen für programmgesteuerte Layouts in Xamarin.iOS

_Dieses Handbuch bietet arbeiten mit iOS Automatisches Layout-Einschränkungen in C# Code nicht in der iOS-Designer erstellt._

Automatisches Layout (auch als "adaptive Layout" bezeichnet) ist ein reaktionsfähiges Design-Ansatz. Im Gegensatz zu transitional Layoutsystem, in dem Speicherort des Elements zu einem Zeitpunkt auf dem Bildschirm hartcodiert ist, automatisches Layout geht es um *Beziehungen* -die Positionen der Elemente relativ zu anderen Elementen auf der Entwurfsoberfläche angezeigt. Den Kern des automatischen Layouts bildet das Konzept der Einschränkungen oder Regeln, die die Platzierung eines Elements oder einer Gruppe von Elementen im Kontext von anderen Elementen auf dem Bildschirm zu definieren. Da die Elemente nicht auf eine bestimmte Position auf dem Bildschirm gebunden sind, können Einschränkungen an, ein Layout für die adaptive zu erstellen, die für verschiedene Bildschirmgrößen und geräteausrichtungen in Ordnung ist.

In der Regel bei der Arbeit mit Automatisches Layout in iOS-verwenden Sie die iOS-Designer grafisch Layout-Einschränkungen für Ihre UI-Elemente zu platzieren. Allerdings gibt es möglicherweise gelegentlich müssen Sie zum Erstellen und Anwenden von Einschränkungen in C# Code. Z. B. dynamisch mit der Erstellung UI-Elemente hinzugefügt, ein `UIView`.

Dieser Anleitung erfahren Sie, wie zum Erstellen und Verwenden von Einschränkungen mithilfe von C# Code nicht grafisch im iOS-Designer erstellt.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Programmgesteuertes Erstellen von Einschränkungen

Wie bereits erwähnt, in der Regel arbeiten mit automatischen Layouts Einschränkungen in der iOS-Designer Sie. Für die Fälle, die Sie die Einschränkungen programmgesteuert zu erstellen, haben Sie drei Optionen zur Auswahl:

* [Layout Anker](#Layout-Anchors) -diese API ermöglicht den Zugriff auf die Ankereigenschaften (z. B. `TopAnchor`, `BottomAnchor` oder `HeightAnchor`) von der UI-Elemente, die eingeschränkt wird.
* [Layout Einschränkungen](#Layout-Constraints) -können Sie Einschränkungen, die direkt mit erstellen die `NSLayoutConstraint` Klasse.
* [Visuelle Formatierung Sprache](#Visual-Format-Language) -stellt eine ASCII-Zeichnungen, wie die Methode, um die Einschränkungen zu definieren.

In den folgenden Abschnitten werden die einzelnen Optionen im Detail besprochen.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Anker für Layout

Mithilfe der `NSLayoutAnchor` -Klasse haben Sie eine fluent-Schnittstelle zum Erstellen von Einschränkungen, die auf Basis der Ankereigenschaften der UI-Elemente, die eingeschränkt wird. Z. B. einen Ansichtscontroller oberen und unteren Layoutführungslinien macht die `TopAnchor`, `BottomAnchor` und `HeightAnchor` Eigenschaften verankern, während eine Sicht Edge "," Center "," Größe "und" Baseline-Eigenschaften verfügbar macht.

> [!IMPORTANT]
> Zusätzlich zum Standardsatz der Ankereigenschaften, iOS-Sichten auch enthalten die `LayoutMarginsGuides` und `ReadableContentGuide` Eigenschaften. Diese Eigenschaften machen `UILayoutGuide` Objekte für die Arbeit mit der Ansicht Ränder und besser lesbar Inhalt Handbücher bzw.

Layout Anker bieten mehrere Methoden zum Erstellen von Einschränkungen in einem leicht lesbaren, kompakten Format an:

- **ConstraintEqualTo** -definiert eine Beziehung, in denen `first attribute = second attribute + [constant]` mit einer optional angegebenen `constant` Offsetwert.
- **ConstraintGreaterThanOrEqualTo** -definiert eine Beziehung, in denen `first attribute >= second attribute + [constant]` mit einer optional angegebenen `constant` Offsetwert.
- **ConstraintLessThanOrEqualTo** -definiert eine Beziehung, in denen `first attribute <= second attribute + [constant]` mit einer optional angegebenen `constant` Offsetwert.

Zum Beispiel:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Eine typische Layout-Einschränkung kann einfach als lineare Ausdruck ausgedrückt werden. Betrachten Sie das folgende Beispiel:

[![](programmatic-layout-constraints-images/graph01.png "Eine Layout-Einschränkung, ausgedrückt als lineare Ausdruck")](programmatic-layout-constraints-images/graph01.png#lightbox)

Die konvertiert der folgenden Codezeile C# code mit Layout-Anker:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

In denen die Teile der C# Code entsprechen, die bestimmte Teile der Formel wie folgt:

|Gleichung|Code|
|---|---|
|Element 1|PurpleView|
|Das 1-Attribut|LeadingAnchor|
|Beziehung|ConstraintEqualTo|
|Multiplikator|Der Standardwert ist 1,0 also nicht angegeben|
|Element 2|OrangeView|
|Attribut 2|TrailingAnchor|
|Konstante|10.0|

Zusätzlich zur Bereitstellung von nur die Parameter, die erforderlich sind, um eine angegebene Layout Einschränkung Gleichung zu lösen, der Layout-Anchor-Methoden erzwingen die typsicherheit der an sie übergebenen Parameter. Also horizontale Einschränkung verankert, z. B. `LeadingAnchor` oder `TrailingAnchor` kann nur verwendet werden, mit anderen horizontale Anker Typen und Multiplikatoren werden nur bereitgestellt, um größenbeschränkungen.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Layout-Einschränkungen

Sie können Einschränkungen für automatisches Layout manuell hinzufügen, indem Sie direkt zu erstellen einen `NSLayoutConstraint` in C# Code. Anders als bei Layout-Anker, müssen Sie einen Wert für alle Parameter angeben, auch wenn es keine Auswirkung auf die Einschränkung definiert haben. Daher werden letztlich eine beträchtliche schwer zu lesen, Standardcode zu erzeugen. Zum Beispiel:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

In denen die `NSLayoutAttribute` Enumeration definiert den Wert für die Ansicht der Ränder und entsprechen der `LayoutMarginsGuide` Eigenschaften, z. B. `Left`, `Right`, `Top` und `Bottom` und `NSLayoutRelation` Enumeration definiert die Beziehung, erstellt werden, zwischen den angegebenen Attributen als `Equal`, `LessThanOrEqual` oder `GreaterThanOrEqual`.

Im Gegensatz zu mit der Layout-Anchor-API, die `NSLayoutConstraint` Erstellungsmethoden kein Hervorheben von wichtigen Aspekte des einer bestimmten Einschränkung, und es gibt keine Kompilierung Zeit ausgeführt werden, auf die Einschränkung überprüft. Daher ist es einfach, die eine ungültige-Einschränkung erstellen, die zur Laufzeit eine Ausnahme ausgelöst wird.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Visuellen Formatsprache

Das Format der Sprache Visual können Sie zum Definieren von Einschränkungen mithilfe von ASCII-Zeichnungen, wie Zeichenfolgen, die bieten eine visuelle Darstellung der Einschränkung erstellt wird. Dies hat die folgenden vor- und Nachteile:

- Das Format der Sprache Visual erzwingt die Erstellung von nur gültige Einschränkungen.
 - Automatisches Layout gibt die Einschränkungen an die Konsole mit visuellen Formatsprache aus, damit die debugging-Meldungen der Code, der zum Erstellen der Einschränkung entsprechen werden.
 - Das Format der Sprache Visual können Sie mehrere Einschränkungen zur gleichen Zeit mit einem sehr kompakt Ausdruck zu erstellen.
 - Da keine Validierung Kompilieren der Visual Formatsprache Zeichenfolgen ist, können Probleme nur zur Laufzeit ermittelt werden.
 - Da das Format der Sprache Visual Visualisierung über Vollständigkeit einige Einschränkungstypen mit (z. B. Verhältnisse) können nicht erstellt werden verdeutlicht.

Wenn das Format der Sprache Visual zu verwenden, um eine Einschränkung erstellen, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine `NSDictionary` , enthält die Objekte anzeigen und Layoutführungslinien und einem Zeichenfolgenschlüssel, der verwendet wird, wenn die Formate zu definieren.
2. Erstellen Sie optional eine `NSDictionary` , definiert einen Satz von Schlüsseln und Werten (`NSNumber`) als konstanten Wert für die Einschränkung verwendet.
3. Erstellen Sie die Formatzeichenfolge, Layout, eine einzelne Spalte oder Zeile von Elementen.
4. Rufen Sie die `FromVisualFormat` Methode der `NSLayoutConstraint` Klasse, um die Einschränkungen zu generieren.
5. Rufen Sie die `ActivateConstraints` -Methode der der `NSLayoutConstraint` Klasse zum Aktivieren und die Einschränkungen gelten.

Um sowohl vorangestellte als auch eine nachfolgende Einschränkung im visuellen Formatsprache zu erstellen, können Sie beispielsweise Folgendes verwenden:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Da das Format der Sprache Visual immer 0 (null) an der übergeordneten Ansicht Ränder angefügten, wenn es sich bei den Standardabstand mit Punkt-Einschränkungen erstellt, erzeugt dieser Code identische Ergebnisse in den Beispielen oben dargestellt.

Für komplexere UI-Designs, wie z. B. mehrere untergeordnete Ansichten in einer einzelnen Zeile gibt das Format der Sprache Visual sowohl den horizontalen Abstand und die vertikale Ausrichtung an. Wie im obigen Beispiel, in dem er gibt an, die `AlignAllTop` `NSLayoutFormatOptions` richtet alle Ansichten in einer Zeile oder Spalte, die am Anfang.

Finden Sie unter Apple [Visual Format Sprache Anhang](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) finden Sie Beispiele für allgemeine Einschränkungen und die Visual Grammatik des Format-Zeichenfolge angeben.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden angezeigt, und erstellen und Verwenden von Auto-Layout-Einschränkungen in C# im Gegensatz zu diese grafisch im iOS-Designer erstellt. Zunächst, wurde es mit Layout-Anker (`NSLayoutAnchor`), automatisches Layout zu behandeln. Anschließend wurde erläutert, wie für die Arbeit mit Layout-Einschränkungen (`NSLayoutConstraint`). Abschließend können sie präsentiert, verwenden das Format der Sprache Visual für automatisches Layout.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple – Programmgesteuertes Erstellen von Einschränkungen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple - visuellen Format Sprache-Anhang](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
