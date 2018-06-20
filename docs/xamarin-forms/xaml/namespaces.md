---
title: Verwendung von XAML-Namespaces in Xamarin.Forms
description: XAML verwendet das XML-Xmlns-Attribut für Namespacedeklarationen. Dieser Artikel führt die Verwendung von XAML-Namespace-Syntax und zeigt, wie einen XAML-Namespace auf einen Typ zu deklarieren.
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: 25299bc3b56c2fbb748db202e43e75be183cce66
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209296"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Verwendung von XAML-Namespaces in Xamarin.Forms

_XAML verwendet das XML-Xmlns-Attribut für Namespacedeklarationen. Dieser Artikel führt die Verwendung von XAML-Namespace-Syntax und zeigt, wie einen XAML-Namespace auf einen Typ zu deklarieren._

## <a name="overview"></a>Übersicht

Es gibt zwei XAML-Namespacedeklarationen, die immer in das Stammelement einer XAML-Datei enthalten sind. Die erste definiert im Standard-Namespace, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

Der Standardnamespace gibt an, dass Elemente, die in der XAML-Datei ohne Präfix definiert z. B. auf Xamarin.Forms-Klassen verwiesen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Die zweite Namespacedeklaration verwendet die `x` Präfix, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML verwendet Präfixe Nichtstandard-Namespaces, mit dem Präfix verwendet werden, wenn Sie Typen innerhalb des Namespaces verweisen zu deklarieren. Die `x` Namespacedeklaration gibt an, dass die Elemente innerhalb der XAML-Code mit dem Präfix definiert `x` für Elemente und Attribute, die für XAML (insbesondere der XAML-Spezifikation 2009) verwendet werden.

Die folgende Tabelle enthält die `x` Namespaceattributen von Xamarin.Forms unterstützt:

|Konstrukt|Beschreibung|
|--- |--- |
|`x:Arguments`|Gibt die Konstruktorargumente für einen nicht trivialen Konstruktor oder eine Objektdeklaration der Factory-Methode.|
|`x:Class`|Gibt den Namen Namespace- und Klassennamen für eine Klasse, die in XAML definiert. Der Klassenname muss der Klassenname des Code-Behind-Datei übereinstimmen. Beachten Sie, dass dieses Konstrukt nur im Stammelement einer XAML-Datei angezeigt werden kann.|
|`x:FactoryMethod`|Gibt eine Factorymethode, die verwendet werden kann, um ein Objekt zu initialisieren.|
|`x:FieldModifier`|Gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente.|
|`x:Key`|Gibt einen eindeutigen benutzerdefinierten Schlüssel für jede Ressource in einem `ResourceDictionary`. Der Schlüssel-Wert wird verwendet, um die Verwendung von XAML-Ressource abgerufen werden soll, und wird normalerweise verwendet, als Argument für die `StaticResource` Markuperweiterung.|
|`x:Name`|Gibt den Namen des Runtime-Objekt für die Verwendung von XAML-Element. Festlegen von `x:Name` ähnelt dem Deklarieren einer Variablen im Code.|
|`x:TypeArguments`|Gibt die generischen Typargumente an den Konstruktor eines generischen Typs an.|

Weitere Informationen zu den `x:FieldModifier` -Attribut angegeben wird, finden Sie unter [Feldmodifizierern](~/xamarin-forms/xaml/field-modifiers.md). Weitere Informationen zu den `x:Arguments`, `x:FactoryMethod`, und `x:TypeArguments` Attribute finden Sie unter [übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md).

In XAML erben Namespacedeklarationen vom übergeordneten Element zum untergeordneten Element. Aus diesem Grund erben, wenn Sie einen Namespace im Stammelement einer XAML-Datei definieren, alle Elemente innerhalb dieser Datei die Namespacedeklaration.

## <a name="declaring-namespaces-for-types"></a>Deklarieren von Namespaces für Typen

Typen können in XAML durch das Deklarieren von eines XAML-Namespaces mit einem Präfix, mit der Namespacedeklaration angeben, die Common Language Runtime (CLR) Namespacenamen und optional die Namen einer Assembly verwiesen werden. Dies wird erreicht, indem Sie Werte für die folgenden Schlüsselwörter in der Namespacedeklaration definieren:

- **Clr-Namespace:** oder **verwenden:** – CLR-Namespace deklariert, innerhalb der Assembly mit den Typen als XAML-Elemente verfügbar machen. Dieses Schlüsselwort ist erforderlich.
- **Assembly =** – die Assembly, auf die verwiesen wird CLR-Namespace enthält. Dieser Wert ist der Name der Assembly ohne Dateierweiterung. Der Pfad zur Assembly sollte als Verweis in der Projektdatei, die die Verwendung von XAML-Datei enthält, die auf die Assembly verwiesen wird, hergestellt werden. Dieses Schlüsselwort kann ausgelassen werden, wenn die **Clr-Namespace** Wert ist innerhalb derselben Assembly wie den Code der Anwendung, die die Typen verweist.

Beachten Sie, dass Trennzeichen getrennt der `clr-namespace` oder `using` -token von der Wert ist eine aus ein Doppelpunkt, während das Trennen von Zeichen der `assembly` -token von seinen Wert wird ein Gleichheitszeichen. Das Zeichen, die zwischen zwei Token verwenden, ist ein Semikolon.

Das folgende Codebeispiel zeigt eine XAML-Namespace-Deklaration:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternativ kann dies als geschrieben werden:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

Die `local` Präfix ist eine Konvention verwendet wird, um anzugeben, dass die Typen innerhalb des Namespaces für die Anwendung lokal sind. Alternativ können Sie sollte die Typen in einer anderen Assembly sind, der Name der Assembly zudem in der Namespacedeklaration definiert werden wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

Das Namespacepräfix wird dann angegeben werden, wenn eine Instanz eines Typs aus einem importierten Namespace deklarieren, als im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel die Syntax der XAML-Namespace eingeführt und veranschaulicht, wie einen XAML-Namespace auf einen Typ zu deklarieren. XAML verwendet die `xmlns` XML-Attribut für den Namespacedeklarationen und Typen in XAML verwiesen werden kann, indem ein XAML-Namespace mit einem Präfix deklariert.


## <a name="related-links"></a>Verwandte Links

- [Übergeben von Argumenten in XAML](~/xamarin-forms/xaml/passing-arguments.md)
