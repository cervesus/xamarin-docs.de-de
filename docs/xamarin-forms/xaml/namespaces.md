---
title: XAML-Namespaces in Xamarin.Forms
description: XAML verwendet das XML-Xmlns-Attribut für Namespacedeklarationen. In diesem Artikel stellt die Syntax des XAML-Namespace, und es wird veranschaulicht, wie einen XAML-Namespace auf einen Typ deklariert.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 85b9297a62cfb90485be2cbd927abfdcfec2f13c
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563042"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>XAML-Namespaces in Xamarin.Forms

_XAML verwendet das XML-Xmlns-Attribut für Namespacedeklarationen. In diesem Artikel stellt die Syntax des XAML-Namespace, und es wird veranschaulicht, wie einen XAML-Namespace auf einen Typ deklariert._

## <a name="overview"></a>Übersicht

Es gibt zwei XAML-Namespace-Deklarationen, die immer in das Stammelement einer XAML-Datei enthalten sind. Der erste definiert den Standardnamespace, wie in den folgenden XAML-Codebeispiel gezeigt:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Der Standardnamespace gibt an, dass Elemente, die in der XAML-Datei ohne Präfix definiert auf Xamarin.Forms-Klassen, z. B. finden Sie unter [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Die zweite Namespacedeklaration verwendet die `x` Präfix, wie in den folgenden XAML-Codebeispiel gezeigt:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML verwendet Präfixe nicht dem Standard-Namespaces, mit dem Präfix, das verwendet wird, verweisen auf Datentypen innerhalb des Namespaces zu deklarieren. Die `x` Namespace-Deklaration gibt an, dass die Elemente innerhalb der XAML mit dem Präfix definiert `x` werden verwendet, für Elemente und Attribute, die für XAML (insbesondere die 2009 XAML-Spezifikation) systemintern sind.

Der folgenden Tabelle werden die `x` Namespaceattribute von Xamarin.Forms unterstützt:

|Konstrukt|Beschreibung|
|--- |--- |
|`x:Arguments`|Gibt die Konstruktorargumente für einen nicht standardmäßigen Konstruktor oder einer Factory Methodendeklaration-Objekt.|
|`x:Class`|Gibt den Namespace und den Namen für eine Klasse, die in XAML definiert. Der Klassenname muss der Klassenname des Code-Behind-Datei übereinstimmen. Beachten Sie, dass dieses Konstrukt im Stammelement einer XAML-Datei kann nur verwendet werden.|
|`x:DataType`|Gibt den Typ des Objekts, das XAML-Element und dessen untergeordnete Elemente, die gebunden werden.|
|`x:FactoryMethod`|Gibt eine Factorymethode, die zum Initialisieren eines Objekts verwendet werden kann.|
|`x:FieldModifier`|Gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente.|
|`x:Key`|Gibt einen eindeutigen benutzerdefinierten Schlüssel für jede Ressource in einem `ResourceDictionary`. Der Wert des Schlüssels wird verwendet, um die XAML-Ressource abzurufen, und wird normalerweise verwendet, als Argument für die `StaticResource` Markuperweiterung.|
|`x:Name`|Gibt einen Common Language Runtime-Objekt an, für das XAML-Element. Festlegen von `x:Name` ähnelt der Deklaration einer Variablen im Code.|
|`x:TypeArguments`|Gibt die generischen Typargumente an den Konstruktor eines generischen Typs an.|

Weitere Informationen zu den `x:DataType` Attribut, finden Sie unter [kompiliert Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md). Weitere Informationen zu den `x:FieldModifier` Attribut, finden Sie unter [Feldmodifizierern](~/xamarin-forms/xaml/field-modifiers.md). Weitere Informationen zu den `x:Arguments`, `x:FactoryMethod`, und `x:TypeArguments` Attribute finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

In XAML erben Namespacedeklarationen vom übergeordneten Element an untergeordnete Element ein. Aus diesem Grund, erben alle Elemente innerhalb dieser Datei bei der Definition eines Namespaces in das Stammelement einer XAML-Datei die Namespacedeklaration.

## <a name="declaring-namespaces-for-types"></a>Deklarieren von Namespaces für Typen

In XAML können Typen verwiesen werden, durch die Deklaration eines XAML-Namespaces mit einem Präfix, mit der Namespacedeklaration, die den Namespacenamen des Common Language Runtime (CLR) und optional einen Assemblynamen angeben. Dies wird erreicht, indem Sie Werte für die folgenden Schlüsselwörter in der Namespacedeklaration definieren:

- **Clr-Namespace:** oder **verwenden:** : der CLR-Namespace deklariert wird, in die Assembly, die als XAML-Elemente verfügbar gemachten Typen enthält. Dieses Schlüsselwort ist erforderlich.
- **Assembly =** – die Assembly mit CLR-Namespace auf die verwiesen wird. Dieser Wert ist der Name der Assembly ohne Dateierweiterung. Der Pfad zur Assembly sollte als Referenz in der Projektdatei hergestellt werden, die die XAML-Datei enthält, die auf die Assembly verweist. Dieses Schlüsselwort ausgelassen werden, wenn die **Clr-Namespace** Wert ist innerhalb der gleichen Assembly wie der Code der Anwendung, die die Typen verweist.

Beachten Sie, dass als Trennzeichen zwischen den `clr-namespace` oder `using` -Token von seinem Wert ist eine aus ein Doppelpunkt, während getrennt die `assembly` -Token von seinem Wert wird ein Gleichheitszeichen. Das zu zwischen beiden Token zu verwendende Zeichen ist ein Semikolon.

Das folgende Codebeispiel zeigt eine XAML-Namespace-Deklaration:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternativ kann dies folgendermaßen geschrieben werden:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

Die `local` Präfix ist eine Konvention verwendet, um anzugeben, dass die Typen im Namespace für die Anwendung lokal sind. Auch wenn die Typen in einer anderen Assembly sind, sollte der Name der Assembly auch in der Deklaration des Namespaces definiert werden wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Der Namespace-Präfix wird dann angegeben, beim Deklarieren einer Instanz eines Typs aus einem importierten Namespace, als in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel die Syntax des XAML-Namespace eingeführt und wurde veranschaulicht, wie einen XAML-Namespace auf einen Typ deklariert. XAML verwendet das `xmlns` XML-Attribut für Namespace-Deklarationen und Typen kann in XAML verwiesen werden, indem eine XAML-Namespace mit einem Präfix deklariert.


## <a name="related-links"></a>Verwandte Links

- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
