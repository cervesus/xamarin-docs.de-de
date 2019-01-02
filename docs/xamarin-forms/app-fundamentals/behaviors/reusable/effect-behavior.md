---
title: Wiederverwendbare EffectBehavior-Klasse
description: Verhalten sind ein nützlicher Ansatz, um Effekte zu einem Steuerelement hinzuzufügen. Dadurch wird standardmäßig genutzter Effektbehandlungscode von Codebehind-Dateien verwendet. In diesem Artikel wird das Erstellen und Nutzen eines Xamarin.Forms-Verhaltens dargestellt, um einem Steuerelement einen Effekt hinzuzufügen.
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 5e47799e704dfbe2c4088016d7055fc616215ea2
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056463"
---
# <a name="reusable-effectbehavior"></a>Wiederverwendbare EffectBehavior-Klasse

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)

_Verhalten sind ein nützlicher Ansatz, um Effekte zu einem Steuerelement hinzuzufügen. Dadurch wird standardmäßig genutzter Effektbehandlungscode von Codebehind-Dateien verwendet. In diesem Artikel wird das Erstellen und Nutzen eines Xamarin.Forms-Verhaltens dargestellt, um einem Steuerelement einen Effekt hinzuzufügen._

## <a name="overview"></a>Übersicht

Die `EffectBehavior`-Klasse ist ein wiederverwendbares benutzerdefiniertes Xamarin.Forms-Verhalten, das einem Steuerelement eine [`Effect`](xref:Xamarin.Forms.Effect)-Instanz anfügt, wenn das Verhalten dem Steuerelement angefügt wird, und sie entfernt die `Effect`-Klasse, wenn das Verhalten vom Steuerelement abgetrennt wird.

Die folgenden Verhaltenseigenschaften müssen für die Verwendung des Verhaltens festgelegt werden:

- **Gruppe:** der Wert des [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute)-Attributs für die Effekt-Klasse.
- **Name:** der Wert des [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute)-Attributs für die Effekt-Klasse.

Weitere Informationen zu Effekten finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

> [!NOTE]
> `EffectBehavior` ist eine benutzerdefinierte Klasse, die in [Effect Behavior sample (Effekt-Verhaltensbeispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/) gefunden werden kann. Allerdings ist sie nicht Teil von Xamarin.Forms.

## <a name="creating-the-behavior"></a>Erstellen des Verhaltens

Die `EffectBehavior`-Klasse wird von der [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)-Klasse abgeleitet, bei der `T` einer [`View`](xref:Xamarin.Forms.View)-Klasse entspricht. Dies bedeutet, dass die `EffectBehavior`-Klasse jedem beliebigen Xamarin.Forms-Steuerelement anfügt werden kann.

### <a name="implementing-bindable-properties"></a>Implementieren von bindbaren Eigenschaften

Die `EffectBehavior`-Klasse definiert zwei [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Instanzen, welche verwendet werden, um einem Steuerelement eine [`Effect`](xref:Xamarin.Forms.Effect)-Klasse anzufügen, wenn das Verhalten dem Steuerelement angefügt wird. Diese Eigenschaften werden im folgenden Codebeispiel veranschaulicht:

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

Wenn die `EffectBehavior`-Klasse genutzt wird, sollte die `Group`-Eigenschaft auf den Wert des [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute)-Attributs für den Effekt festgelegt werden. Zudem sollte die `Name`-Eigenschaft auf den Wert des [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute)-Attributs für die Effekt-Klasse festgelegt werden.

### <a name="implementing-the-overrides"></a>Implementieren der Überschreibungen

Die `EffectBehavior`-Klasse überschreibt, wie im folgenden Codebeispiel zu sehen ist, die Methoden [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) und [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) der [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)-Klasse:

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

Die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))-Methode führt das Setup aus, indem sie die `AddEffect`-Methode aufruft, wobei das angefügte Steuerelement als Parameter übergeben wird. Die [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode führt den Cleanup aus, indem sie die `RemoveEffect`-Methode aufruft, wobei das angefügte Steuerelement als Parameter übergeben wird.

### <a name="implementing-the-behavior-functionality"></a>Implementieren der Verhaltensfunktionalität

Der Zweck des Verhaltens ist es, die [`Effect`](xref:Xamarin.Forms.Effect)-Klasse, die in den `Group`- und `Name`-Eigenschaften definiert ist, einem Steuerelement anzufügen, wenn das Verhalten dem Steuerelement angefügt wird, und die `Effect`-Klasse zu entfernen, wenn das Verhalten vom Steuerelement getrennt wird. Die Kernfunktionalität des Verhaltens wird im folgenden Codebeispiel veranschaulicht:

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

Die `AddEffect`-Methode wird als Reaktion auf das Anfügen von `EffectBehavior` an ein Steuerelement ausgeführt und erhält das angefügte Steuerelement als Parameter. Die Methode fügt dann den abgerufenen Effekt der [`Effects`](xref:Xamarin.Forms.Element.Effects)-Sammlung des Steuerelements hinzu. Die `RemoveEffect`-Methode wird als Reaktion auf das Trennen von `EffectBehavior` von einem Steuerelement ausgeführt und erhält das angefügte Steuerelement als Parameter. Die Methode entfernt dann den Effekt aus der [`Effects`](xref:Xamarin.Forms.Element.Effects)-Sammlung des Steuerelements.

Die `GetEffect`-Methode verwendet die [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String))-Methode, um den [`Effect`](xref:Xamarin.Forms.Effect)-Wert abzurufen. Der Effekt wird durch eine Verkettung der `Group`- und `Name`-Eigenschaftswerte gefunden. Wenn eine Plattform den Effekt nicht bereitstellt, gibt die `Effect.Resolve`-Methode einen Wert ungleich `null` zurück.

## <a name="consuming-the-behavior"></a>Nutzen des Verhaltens

Die `EffectBehavior`-Klasse kann, wie im folgenden XAML-Codebeispiel veranschaulicht, der [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung eines Steuerelements angefügt werden:

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

Die `Group`- und `Name`-Eigenschaften des Verhaltens werden für die Effekt-Klasse in jedem plattformspezifischen Projekt auf die Werte der Attribute [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute) und [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute) festgelegt.

Zur Laufzeit, wenn das Verhalten dem [`Label`](xref:Xamarin.Forms.Label)-Steuerelement angefügt wird, wird die `Xamarin.LabelShadowEffect`-Klasse der [`Effects`](xref:Xamarin.Forms.Element.Effects)-Sammlung des Steuerelements hinzugefügt. Dadurch wird, wie in den folgenden Screenshots zu sehen ist, ein Schatten zu dem Text hinzugefügt, der vom `Label`-Steuerelement angezeigt wird:

![](effect-behavior-images/screenshots.png "Beispielanwendung mit EffectsBehavior")

Der Vorteil dieses Verhaltens zum Hinzufügen und Entfernen von Effekten bei Steuerelementen besteht darin, dass Sie Standardcode zum Verarbeiten von Effekten aus CodeBehind-Dateien entfernen können.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie einem Steuerelement mithilfe eines Verhaltens ein Effekt hinzugefügt werden kann. Die `EffectBehavior`-Klasse ist ein wiederverwendbares benutzerdefiniertes Xamarin.Forms-Verhalten, das einem Steuerelement eine [`Effect`](xref:Xamarin.Forms.Effect)-Instanz anfügt, wenn das Verhalten dem Steuerelement angefügt wird, und sie entfernt die `Effect`-Klasse, wenn das Verhalten vom Steuerelement abgetrennt wird.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Effect Behavior (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Behavior-Klasse](xref:Xamarin.Forms.Behavior)
- [Behavior<T>-Klasse<T>](xref:Xamarin.Forms.Behavior`1)
