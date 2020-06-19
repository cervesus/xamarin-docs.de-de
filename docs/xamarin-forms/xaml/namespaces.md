---
title: XAML-Namespaces inXamarin.Forms
description: XAML verwendet das xmlns-XML-Attribut für Namespace Deklarationen. In diesem Artikel wird die XAML-Namespace Syntax vorgestellt, und es wird veranschaulicht, wie ein XAML-Namespace für den Zugriff auf einen Typ deklariert wird.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7f35342134767ccdadfab086bfa14f6b610b325d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84130376"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>XAML-Namespaces inXamarin.Forms

_XAML verwendet das xmlns-XML-Attribut für Namespace Deklarationen. In diesem Artikel wird die XAML-Namespace Syntax vorgestellt, und es wird veranschaulicht, wie ein XAML-Namespace für den Zugriff auf einen Typ deklariert wird._

## <a name="overview"></a>Übersicht

Es gibt zwei XAML-Namespace Deklarationen, die sich immer innerhalb des Stamm Elements einer XAML-Datei befinden. Der erste definiert den Standard Namespace, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
xmlns="http://xamarin.com/schemas/2014/forms"
```

Der Standard Namespace gibt an, dass Elemente, die in der XAML-Datei ohne Präfix definiert Xamarin.Forms sind, auf Klassen wie verweisen [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

Die zweite Namespace Deklaration verwendet das `x` Präfix, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML verwendet Präfixe, um nicht standardmäßige Namespaces zu deklarieren, wobei das Präfix beim Verweisen auf Typen innerhalb des Namespace verwendet wird. Die `x` Namespace Deklaration gibt an, dass Elemente, die innerhalb des XAML mit einem Präfix von definiert sind, `x` für Elemente und Attribute verwendet werden, die in XAML intrinsisch sind (insbesondere die 2009 XAML-Spezifikation).

In der folgenden Tabelle sind die `x` von unterstützten Namespace Attribute aufgeführt Xamarin.Forms :

|Konstrukt|Beschreibung|
|--- |--- |
|`x:Arguments`|Gibt Konstruktorargumente für einen nicht Standardkonstruktor oder für eine Factorymethoden-Objekt Deklaration an.|
|`x:Class`|Gibt den Namespace und den Klassennamen für eine Klasse an, die in XAML definiert ist. Der Klassenname muss mit dem Klassennamen der Code Behind-Datei identisch sein. Beachten Sie, dass dieses Konstrukt nur im Root-Element einer XAML-Datei angezeigt werden kann.|
|`x:DataType`|Gibt den Typ des Objekts an, an das das XAML-Element und seine untergeordneten Elemente gebunden werden.|
|`x:FactoryMethod`|Gibt eine Factorymethode an, die zum Initialisieren eines Objekts verwendet werden kann.|
|`x:FieldModifier`|Gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente an.|
|`x:Key`|Gibt einen eindeutigen benutzerdefinierten Schlüssel für jede Ressource in einem an `ResourceDictionary` . Der Wert des Schlüssels wird zum Abrufen der XAML-Ressource verwendet und in der Regel als Argument für die `StaticResource` Markup Erweiterung verwendet.|
|`x:Name`|Gibt einen Laufzeitobjekt Namen für das XAML-Element an. `x:Name`Die Einstellung ähnelt der Deklaration einer Variablen im Code.|
|`x:TypeArguments`|Gibt die generischen Typargumente für den Konstruktor eines generischen Typs an.|

Weitere Informationen zum- `x:DataType` Attribut finden Sie unter [kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md). Weitere Informationen zum- `x:FieldModifier` Attribut finden Sie unter [Feldmodifizierer](~/xamarin-forms/xaml/field-modifiers.md). Weitere Informationen über das `x:Arguments` -Attribut und das- `x:FactoryMethod` Attribut finden Sie unter Übergeben von [Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md). Weitere Informationen zum- `x:TypeArguments` Attribut finden Sie unter [Generika in XAML mit Xamarin.Forms ](generics.md).

> [!NOTE]
> Zusätzlich zu den oben aufgeführten Namespace Attributen Xamarin.Forms enthält auch Markup Erweiterungen, die über das `x` Namespace Präfix genutzt werden können. Weitere Informationen finden Sie unter [Verwenden von XAML-Markup Erweiterungen](~/xamarin-forms/xaml/markup-extensions/consuming.md).

In XAML erben Namespace Deklarationen von einem übergeordneten Element zu einem untergeordneten Element. Wenn Sie einen Namespace im Stamm Element einer XAML-Datei definieren, erben daher alle Elemente in dieser Datei die Namespace Deklaration.

## <a name="declaring-namespaces-for-types"></a>Deklarieren von Namespaces für Typen

Auf Typen kann in XAML verwiesen werden, indem ein XAML-Namespace mit einem Präfix deklariert wird, wobei die Namespace Deklaration den CLR-Namespace Namen (Common Language Runtime) und optional einen Assemblynamen angibt. Dies wird erreicht, indem Werte für die folgenden Schlüsselwörter innerhalb der Namespace Deklaration definiert werden:

- **CLR-Namespace:** oder **using:** – der in der Assembly deklarierte CLR-Namespace, der die Typen enthält, die als XAML-Elemente verfügbar gemacht werden sollen. Dieses Schlüsselwort ist erforderlich.
- **Assembly =** – die Assembly, die den CLR-Namespace enthält, auf den verwiesen wird. Dieser Wert ist der Name der Assembly ohne die Dateierweiterung. Der Pfad zur Assembly sollte als Verweis in der Projektdatei eingerichtet werden, die die XAML-Datei enthält, die auf die Assembly verweist. Dieses Schlüsselwort kann weggelassen werden, wenn der **CLR-Namespace-** Wert sich innerhalb derselben Assembly befindet wie der Anwendungscode, der auf die Typen verweist.

Beachten Sie, dass das Zeichen, das das Token `clr-namespace` oder `using` von seinem Wert trennt, ein Doppelpunkt ist, während das Zeichen, das das `assembly` Token von seinem Wert trennt, ein Gleichheitszeichen ist. Das Zeichen, das zwischen den beiden Token verwendet werden soll, ist ein Semikolon.

Das folgende Codebeispiel zeigt eine XAML-Namespace Deklaration:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternativ kann dies wie folgt geschrieben werden:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

Das `local` Präfix ist eine Konvention, mit der angegeben wird, dass die Typen im Namespace für die Anwendung lokal sind. Wenn sich die Typen in einer anderen Assembly befinden, sollte der AssemblyName auch in der Namespace Deklaration definiert werden, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Das Namespace Präfix wird dann angegeben, wenn eine Instanz eines Typs aus einem importierten Namespace deklariert wird, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Weitere Informationen zum Definieren eines benutzerdefinierten Namespace Schemas finden Sie unter [XAML Custom Namespace Schemas](custom-namespace-schemas.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die XAML-Namespace Syntax vorgestellt und gezeigt, wie ein XAML-Namespace für den Zugriff auf einen Typ deklariert wird. XAML verwendet das `xmlns` XML-Attribut für Namespace Deklarationen, und in XAML kann auf Typen verwiesen werden, indem ein XAML-Namespace mit einem Präfix deklariert wird.

## <a name="related-links"></a>Verwandte Links

- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Generika in XAML mitXamarin.Forms](generics.md)
