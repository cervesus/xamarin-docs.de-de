---
title: "Einschränkungen für programmgesteuerte Layout"
description: "Dieses Handbuch bietet arbeiten mit iOS Automatisches Layout Einschränkungen in C#-Code nicht in der iOS-Designer erstellt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 774d6e6ecdb081650c6f008b1ac83c397f788d5b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="programmatic-layout-constraints"></a>Einschränkungen für programmgesteuerte Layout

_Dieses Handbuch bietet arbeiten mit iOS Automatisches Layout Einschränkungen in C#-Code nicht in der iOS-Designer erstellt._

Automatisches Layout (auch als "adaptive Layout" bezeichnet) ist ein reaktionsfähiges Design-Ansatz. Im Gegensatz zu den transitional Layoutsystem, in dem jedes Element Speicherort hartcodiert bis zu einem Zeitpunkt auf dem Bildschirm ist, automatisches Layout geht *Beziehungen* -die Positionen von Elementen relativ zu anderen Elementen auf der Entwurfsoberfläche angezeigt. Das Herzstück des Automatisches Layout ist die Einschränkungen und Regeln, die die Platzierung eines Elements oder einer Gruppe von Elementen im Kontext von anderen Elementen auf dem Bildschirm zu definieren. Da die Elemente nicht an einer bestimmten Position auf dem Bildschirm gebunden sind, können Einschränkungen eine adaptive Layout zu erstellen, die für verschiedene Bildschirmgrößen und Gerät Ausrichtungen gut aussieht.

In der Regel bei der Arbeit mit Automatisches Layout in iOS verwenden Sie die iOS-Designer um grafisch Layout Einschränkungen für Ihre UI-Elemente zu platzieren. Möglicherweise gibt es jedoch vorkommen, dass Sie zum Erstellen und Anwenden von Einschränkungen in C#-Code müssen. Beispielsweise beim Verwenden dynamisch erstellter Benutzeroberflächenelemente hinzugefügt, um eine `UIView`.

Dieses Handbuch wird gezeigt, wie zum Erstellen und Arbeiten mit Einschränkungen mithilfe von C#-Code nicht in der iOS-Designer grafisch erstellt.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Programmgesteuertes Erstellen von Einschränkungen

Wie oben erwähnt, werden in der Regel Sie Auto-Layout-Einschränkungen in der iOS-Designer arbeiten. Für die Häufigkeit, mit der Sie die Einschränkungen programmgesteuert erstellt haben, haben Sie drei Optionen zur Auswahl:

* [Layout Anker](#Layout-Anchors) -dieser API ermöglicht den Zugriff auf die Premium-Eigenschaften (z. B. `TopAnchor`, `BottomAnchor` oder `HeightAnchor`) der UI-Elemente, die eingeschränkt.
* [Layout Einschränkungen](#Layout-Constraints) -können Sie Einschränkungen, die direkt mit erstellen, die `NSLayoutConstraint` Klasse.
* [Visuelle Formatierung Sprache](#Visual-Format-Language) -ermöglicht ein als ASCII-wie-Methode, die Einschränkungen zu definieren.

In den folgenden Abschnitten werden die einzelnen Optionen im Detail besprochen.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Layout Anker

Mithilfe der `NSLayoutAnchor` -Klasse haben Sie eine fluent Benutzeroberfläche zum Erstellen von Einschränkungen basierend auf den Premium-Eigenschaften der UI-Elemente, die eingeschränkt. Beispielsweise führt eine View-Controller oberen und unteren Layout macht die `TopAnchor`, `BottomAnchor` und `HeightAnchor` Eigenschaften zu verankern, während eine Sicht Edge "," Center "," Größe "und" Baseline-Eigenschaften verfügbar macht.

> [!IMPORTANT]
> Zusätzlich zu den Standardsatz von Ankereigenschaften, iOS-Sichten auch umfassen die `LayoutMarginsGuides` und `ReadableContentGuide` Eigenschaften. Diese Eigenschaften verfügbar machen `UILayoutGuide` Objekte für die Arbeit mit der Ansicht Ränder und besser lesbar Handbücher bzw. Inhalt.

Layout Anker bieten mehrere Methoden zum Erstellen von Einschränkungen in einem einfach zu lesende, kompakten Format an:

- **ConstraintEqualTo** -definiert eine Beziehung, in denen `first attribute = second attribute + [constant]` mit den optional bereitgestellten `constant` Offsetwert.
- **ConstraintGreaterThanOrEqualTo** -definiert eine Beziehung, in denen `first attribute >= second attribute + [constant]` mit den optional bereitgestellten `constant` Offsetwert.
- **ConstraintLessThanOrEqualTo** -definiert eine Beziehung, in denen `first attribute <= second attribute + [constant]` mit den optional bereitgestellten `constant` Offsetwert.

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

Die in die folgende Zeile der C#-Code mithilfe von Layout Anker umgewandelt werden würde:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

In denen entsprechen die Bestandteile der C#-Code wie folgt auf die angegebenen Teile der Formel ein:

|Gleichung|Code|
|---|---|
|Element 1|PurpleView|
|1-Attribut|LeadingAnchor|
|Beziehung|ConstraintEqualTo|
|Multiplikator|Der Standardwert ist 1,0 daher nicht angegeben|
|Element 2|OrangeView|
|2-Attribut|TrailingAnchor|
|Konstante|10.0|

Zusätzlich zur Bereitstellung nur die Parameter, die erforderlich sind, zu eine bestimmten Layout Einschränkung Gleichung zu lösen, jede der Methoden Layout Anker Erzwingen der typsicherheit, der an sie übergebenen Parameter. Daher horizontale Einschränkung verankert wie z. B. `LeadingAnchor` oder `TrailingAnchor` kann nur verwendet werden mit anderen horizontalen Anker Typen und Multiplikatoren werden nur bereitgestellt, um Einschränkungen der Datenkapazität.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Layout-Einschränkungen

Sie können Einschränkungen für automatisches Layout manuell hinzufügen, indem direkt erstellen eine `NSLayoutConstraint` in C#-Code. Im Gegensatz zur Verwendung Layout Anker, müssen Sie einen Wert für jeden Parameter angeben, auch wenn es keine Auswirkungen auf die Einschränkung definiert werden muss. Daher werden Sie annehmen, die schwer zu lesen, Standardcode erhebliche erzeugt. Zum Beispiel:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

In dem die `NSLayoutAttribute` Enumeration definiert den Wert für die Sicht Ränder und entsprechen den `LayoutMarginsGuide` Eigenschaften wie z. B. `Left`, `Right`, `Top` und `Bottom` und die `NSLayoutRelation` Enumeration definiert die Beziehung, erstellt werden, zwischen den angegebenen Attributen als `Equal`, `LessThanOrEqual` oder `GreaterThanOrEqual`.

Im Gegensatz zu mit der Layout-Anker-API, die `NSLayoutConstraint` Erstellungsmethoden die wichtigsten Aspekte einer bestimmten Einschränkung nicht hervorgehoben, und es gibt keine Kompilierung Zeit ausgeführt wird, auf die Einschränkung überprüft. Daher ist es einfach, eine ungültige Einschränkung erstellen, die zur Laufzeit eine Ausnahme ausgelöst wird.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Visuellen Formatsprache

Das visuelle Formatsprache können Sie definieren Einschränkungen mithilfe von ASCII-Grafik wie Zeichenfolgen, die bieten eine visuelle Darstellung der Einschränkung erstellt wird. Dies hat die folgenden vor- und Nachteile:

- Das visuelle Formatsprache erzwingt die Erstellung von nur gültige Einschränkungen.
 - Automatisches Layout gibt die Einschränkungen an die Konsole, indem das visuelle Formatsprache aus, damit die debugging-Meldungen den Code verwendet ähnelt, um die Einschränkung zu erstellen.
 - Das visuelle Formatsprache können Sie mehrere Einschränkungen gleichzeitig mit einem sehr compact Ausdruck zu erstellen.
 - Da es keine Kompilierung clientseitige Validierung der visuellen Formatsprache-Zeichenfolgen ist, können Probleme nur zur Laufzeit ermittelt werden.
 - Da das Format der Sprache Visual Visualisierung über Vollständigkeit hervorgehoben werden, die einige Einschränkungstypen mit (z. B. Verhältnissen) erstellt werden können.

Verwendung der visuellen Formatsprache erstellen Sie eine Einschränkung, gehen Sie folgendermaßen vor:

1. Erstellen einer `NSDictionary` , enthält das Anzeigen von Objekten und Layoutführungslinien und einen Zeichenfolgenschlüssel, der verwendet wird, wenn Sie die Formate zu definieren.
2. Erstellen Sie optional eine `NSDictionary` , definiert einen Satz von Schlüsseln und Werten (`NSNumber`) als konstanten Wert für die Einschränkung verwendet.
3. Erstellen Sie die Formatzeichenfolge zum Layout einer einzelnen Spalte oder Zeile von Elementen.
4. Rufen Sie die `FromVisualFormat` Methode der `NSLayoutConstraint` Klasse, um die Einschränkungen zu generieren.
5. Rufen Sie die `ActivateConstraints` Methode der `NSLayoutConstraint` Klasse zum Aktivieren und die Einschränkungen gelten.

Um eine führende und nachfolgende-Einschränkung in der visuellen Formatsprache zu erstellen, können Sie z. B. Folgendes verwenden:

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

Da das visuelle Formatsprache immer 0 (null) auf der übergeordneten Ansicht Ränder angefügten werden, wenn mit den Standardabstand Punkt-Einschränkungen wird erstellt, erzeugt dieser Code identische Ergebnisse in den Beispielen oben dargestellt.

Für komplexere Benutzeroberflächendesigns, z. B. mehrere untergeordnete Ansichten in einer einzelnen Zeile gibt das Format der Sprache Visual den horizontalen Abstand und die vertikale Ausrichtung. Wie im Beispiel oben, in dem sie gibt, die `AlignAllTop` `NSLayoutFormatOptions` alle Ansichten in einer Zeile oder Spalte, deren Tops richtet.

Finden Sie in der Apple- [visuellen Format Sprache Anhang](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) für einige Beispiele für allgemeine Einschränkungen und die visuellen Format Zeichenfolge Grammatik angeben.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch präsentiert erstellen und im Gegensatz zu Erstellungsaufwand in der iOS-Designer grafisch Automatisches Layout-Einschränkungen in c# verwenden. Zuerst sie Layout-Anker mit betrachtet (`NSLayoutAnchor`), automatisches Layout zu behandeln. Als Nächstes zum Arbeiten mit Layout Einschränkungen bleiben (`NSLayoutConstraint`). Schließlich es das mit den visuellen Formatsprache für automatisches Layout präsentiert.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Automatisches Layout mit dem Xamarin-Designer für iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple – Programmgesteuertes Erstellen von Einschränkungen](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple - visuellen Format Sprache Anhang](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
