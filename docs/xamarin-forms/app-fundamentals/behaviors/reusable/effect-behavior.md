---
title: Wiederverwendbare EffectBehavior
description: Verhalten sind eine nützliche Methode für einen Effekt auf ein Steuerelement, entfernen die Verarbeitung von Code aus dem Code-Behind-Dateien mit Codebausteinen-Effekt hinzufügen. Dieser Artikel veranschaulicht das Erstellen und nutzen ein Xamarin.Forms-Verhalten, um einen Effekt auf ein Steuerelement hinzufügen.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2696f0103ce1aa969039c982fb9b82f89b37811e
ms.sourcegitcommit: 06a52ac36031d0d303ac7fc8163a59c178799c80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50911592"
---
# <a name="reusable-effectbehavior"></a>Wiederverwendbare EffectBehavior

_Verhalten sind eine nützliche Methode für einen Effekt auf ein Steuerelement, entfernen die Verarbeitung von Code aus dem Code-Behind-Dateien mit Codebausteinen-Effekt hinzufügen. Dieser Artikel veranschaulicht das Erstellen und nutzen ein Xamarin.Forms-Verhalten, um einen Effekt auf ein Steuerelement hinzufügen._

## <a name="overview"></a>Übersicht

Die `EffectBehavior` Klasse ist ein wiederverwendbarer benutzerdefinierten Xamarin.Forms-Verhalten, das hinzugefügt ein [ `Effect` ](xref:Xamarin.Forms.Effect) Instanz eines Steuerelements, wenn das Verhalten, auf das Steuerelement angefügt ist, und entfernt die `Effect` Instanz, wenn Sie das Verhalten ist getrennt vom Steuerelement.

Das Verhalten verwendet, müssen die folgenden Verhaltenseigenschaften festgelegt werden:

- **Gruppe** – der Wert des der [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) Attribut für die Klasse wirksam.
- **Namen** – der Wert des der [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) Attribut für die Klasse wirksam.

Weitere Informationen zu den Auswirkungen, finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

> [!NOTE]
> Die `EffectBehavior` ist eine benutzerdefinierte Klasse, die gefunden werden kann die [Auswirkung Verhalten Beispiel](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/), und sind nicht Teil von Xamarin.Forms.

## <a name="creating-the-behavior"></a>Erstellen das Verhalten

Die `EffectBehavior` Klasse leitet sich von der [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) -Klasse, in denen `T` ist eine [ `View` ](xref:Xamarin.Forms.View). Dies bedeutet, dass die `EffectBehavior` Klasse, die alle Xamarin.Forms-Steuerelements angefügt werden kann.

### <a name="implementing-bindable-properties"></a>Implementieren die bindbare Eigenschaften

Die `EffectBehavior` -Klasse definiert zwei [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Instanzen, die verwendet werden, um das Hinzufügen einer [ `Effect` ](xref:Xamarin.Forms.Effect) an ein Steuerelement, wenn das Verhalten für das Steuerelement angehängt ist. Diese Eigenschaften werden im folgenden Codebeispiel gezeigt:

```csharp
public class EffectBehavior : Behavior<View>
{
  public static readonly BindableProperty GroupProperty =
    BindableProperty.Create ("Group", typeof(string), typeof(EffectBehavior), null);
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(EffectBehavior), null);

  public string Group {
    get { return (string)GetValue (GroupProperty); }
    set { SetValue (GroupProperty, value); }
  }

  public string Name {
    get { return(string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }
  ...
}
```

Wenn der `EffectBehavior` verbraucht ist, der `Group` Eigenschaft muss festgelegt werden, auf den Wert des der [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) Attribut für den Effekt. Darüber hinaus die `Name` Eigenschaft muss festgelegt werden, auf den Wert des der [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) Attribut für die Klasse wirksam.

### <a name="implementing-the-overrides"></a>Implementieren die Außerkraftsetzungen

Die `EffectBehavior` -Klasse überschreibt die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) und [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methoden der [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) Klasse, wie im folgenden Code gezeigt. Beispiel:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  protected override void OnAttachedTo (BindableObject bindable)
  {
    base.OnAttachedTo (bindable);
    AddEffect (bindable as View);
  }

  protected override void OnDetachingFrom (BindableObject bindable)
  {
    RemoveEffect (bindable as View);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode führt Setup durch Aufrufen der `AddEffect` -Methode, das angefügte Steuerelement als Parameter übergeben. Die [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) -Methode führt die Bereinigung durch Aufrufen der `RemoveEffect` -Methode, das angefügte Steuerelement als Parameter übergeben.

### <a name="implementing-the-behavior-functionality"></a>Implementieren der Verhaltensfunktionalität.

Die das Verhalten dient zum Hinzufügen der [ `Effect` ](xref:Xamarin.Forms.Effect) in definiert die `Group` und `Name` Eigenschaften eines Steuerelements, wenn das Verhalten angefügt ist, auf das Steuerelement, und entfernen Sie die `Effect` Wenn das Verhalten ist getrennt vom Steuerelement. Die Kernfunktionen von Verhalten wird im folgenden Codebeispiel dargestellt:

```csharp
public class EffectBehavior : Behavior<View>
{
  ...
  void AddEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Add (GetEffect ());
    }
  }

  void RemoveEffect (View view)
  {
    var effect = GetEffect ();
    if (effect != null) {
      view.Effects.Remove (GetEffect ());
    }
  }

  Effect GetEffect ()
  {
    if (!string.IsNullOrWhiteSpace (Group) && !string.IsNullOrWhiteSpace (Name)) {
      return Effect.Resolve (string.Format ("{0}.{1}", Group, Name));
    }
    return null;
  }
}
```

Die `AddEffect` Methode wird ausgeführt, als Reaktion auf die `EffectBehavior` angefügt wird, ein Steuerelement, und das angefügte Steuerelement als Parameter erhält. Die Methode fügt dann die abgerufenen Auswirkung des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung. Die `RemoveEffect` Methode wird ausgeführt, als Reaktion auf die `EffectBehavior` getrennt von einem Steuerelement und das angefügte Steuerelement als Parameter erhält. Die Methode entfernt dann die Auswirkungen aus des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung.

Die `GetEffect` -Methode verwendet die [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) Methode zum Abrufen der [ `Effect` ](xref:Xamarin.Forms.Effect). Der Effekt ist durch eine Verkettung von befinden die `Group` und `Name` Eigenschaftswerte. Wenn eine Plattform für den Effekt, bieten nicht die `Effect.Resolve` Methode gibt einen nicht-`null` Wert.

## <a name="consuming-the-behavior"></a>Nutzen das Verhalten

Die `EffectBehavior` Klasse angefügt werden kann, um die [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung eines Steuerelements, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Behaviors>
    <local:EffectBehavior Group="Xamarin" Name="LabelShadowEffect" />
  </Label.Behaviors>
</Label>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};
label.Behaviors.Add (new EffectBehavior {
  Group = "Xamarin",
  Name = "LabelShadowEffect"
});
```

Die `Group` und `Name` festgelegt auf die Werte der Eigenschaften des Verhaltens der [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) und [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) Attribute für die Klasse Auswirkungen in einzelnen plattformspezifische Projekt.

Zur Laufzeit, wenn das Verhalten angefügt ist, auf die [ `Label` ](xref:Xamarin.Forms.Label) -Steuerelement, das `Xamarin.LabelShadowEffect` des Steuerelements hinzugefügt [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung. Dadurch wird ein Schatten hinzugefügt wird, auf den Text angezeigt, indem die `Label` zu steuern, wie in den folgenden Screenshots gezeigt:

![](effect-behavior-images/screenshots.png "Beispielanwendung mit EffectsBehavior")

Der Vorteil der Verwendung dieses Verhalten hinzufügen und entfernen Effekte von Steuerelementen ist, dass zur Behandlung von Auswirkungen partitionierungsaufgaben aus Code-Behind-Dateien entfernt werden kann.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mithilfe eines Verhaltens einen Effekt auf ein Steuerelement hinzufügen. Die `EffectBehavior` Klasse ist ein wiederverwendbarer benutzerdefinierten Xamarin.Forms-Verhalten, das hinzugefügt ein [ `Effect` ](xref:Xamarin.Forms.Effect) Instanz eines Steuerelements, wenn das Verhalten, auf das Steuerelement angefügt ist, und entfernt die `Effect` Instanz, wenn Sie das Verhalten ist getrennt vom Steuerelement.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Verhalten der Auswirkungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Verhalten](xref:Xamarin.Forms.Behavior)
- [Verhalten<T>](xref:Xamarin.Forms.Behavior`1)
