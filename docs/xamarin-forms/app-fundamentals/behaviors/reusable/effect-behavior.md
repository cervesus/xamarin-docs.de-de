---
title: Wiederverwendbare EffectBehavior
description: "Verhalten sind eine sinnvolle Vorgehensweise zum Hinzufügen eines Effekts zu einem Steuerelement mit Codebausteinen Auswirkungen Behandlungscode aus Code-Behind-Dateien zu entfernen. Dieser Artikel veranschaulicht, wie mit einer Xamarin.Forms-Verhalten einen Effekt an ein Steuerelement hinzufügen."
ms.topic: article
ms.prod: xamarin
ms.assetid: A909B24D-960A-4023-AFF6-4B9256C55ADD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 698e2baef26a985fa40c2943cd25aefae55381f9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="reusable-effectbehavior"></a>Wiederverwendbare EffectBehavior

_Verhalten sind eine sinnvolle Vorgehensweise zum Hinzufügen eines Effekts zu einem Steuerelement mit Codebausteinen Auswirkungen Behandlungscode aus Code-Behind-Dateien zu entfernen. Dieser Artikel veranschaulicht, wie mit einer Xamarin.Forms-Verhalten einen Effekt an ein Steuerelement hinzufügen._

## <a name="overview"></a>Übersicht

Die `EffectBehavior` Klasse ist eine wiederverwendbare Xamarin.Forms benutzerdefiniertes Verhalten, die hinzugefügt ein [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) Instanz mit einem Steuerelement, wenn das Verhalten ist mit dem Steuerelement verbunden, und entfernt die `Effect` -Instanz auf, wenn das Verhalten ist vom Steuerelement getrennt.

Die folgenden Verhaltenseigenschaften müssen festgelegt werden, um das Verhalten verwendet:

- **Gruppe** – der Wert von der [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) Attribut für die Auswirkung-Klasse.
- **Namen** – der Wert von der [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) Attribut für die Klasse wirksam.

Weitere Informationen zu Auswirkungen finden Sie unter [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="creating-the-behavior"></a>Erstellen das Verhalten

Die `EffectBehavior` Klasse abgeleitet der [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) -Klasse, in denen `T` ist ein [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/). Dies bedeutet, dass die `EffectBehavior` Klasse auf jedes Steuerelement Xamarin.Forms angefügt werden kann.

### <a name="implementing-bindable-properties"></a>Implementieren von bindbare Eigenschaften

Die `EffectBehavior` Klasse definiert zwei [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) -Instanzen, die verwendet werden, um das Hinzufügen einer [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) an ein Steuerelement, wenn das Verhalten dem Steuerelement zugeordnet ist. Diese Eigenschaften werden im folgenden Codebeispiel gezeigt:

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

Wenn die `EffectBehavior` verbraucht ist, die `Group` Eigenschaft sollte festgelegt werden, auf den Wert des der [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) Attribut für den Effekt. Darüber hinaus die `Name` Eigenschaft sollte festgelegt werden, auf den Wert von der [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) Attribut für die Auswirkung-Klasse.

### <a name="implementing-the-overrides"></a>Implementieren die Außerkraftsetzungen

Die `EffectBehavior` -Klasse überschreibt die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) und [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methoden der der [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) Klasse, wie im folgenden Code gezeigt. Beispiel:

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

Die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode führt Setup durch Aufrufen der `AddEffect` -Methode, in der zugeordneten Steuerelements als Parameter übergeben. Die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) -Methode führt die Bereinigung durch Aufrufen der `RemoveEffect` -Methode, in der zugeordneten Steuerelements als Parameter übergeben.

### <a name="implementing-the-behavior-functionality"></a>Implementieren die Verhaltensfunktionalität.

Das Verhalten hinzugefügt wird die [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) definiert, der `Group` und `Name` Eigenschaften zu einem Steuerelement, wenn das Verhalten ist mit dem Steuerelement verbunden, und Entfernen der `Effect` Wenn das Verhalten ist vom Steuerelement getrennt. Im folgenden Codebeispiel wird die Kernfunktionen von Verhalten angezeigt:

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

Die `AddEffect` Methode ausgeführt wird, als Antwort auf die `EffectBehavior` dem Anfügen an ein Steuerelement, und er empfängt die angefügten als Parameter. Die Methode fügt dann die abgerufenen Auswirkungen auf des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung. Die `RemoveEffect` Methode ausgeführt wird, als Antwort auf die `EffectBehavior` wird getrennt von einem Steuerelement, und er empfängt die angefügten als Parameter. Die Methode die Auswirkungen wieder aus des Steuerelements entfernt [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung.

Die `GetEffect` -Methode verwendet die [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Methode zum Abrufen der [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/). Der Effekt ist durch eine Verkettung aus dem `Group` und `Name` Eigenschaftswerte. Wenn eine Plattform den Effekt bietet die `Effect.Resolve` Methodenrückgabewert wird nicht`null` Wert.

## <a name="consuming-the-behavior"></a>Nutzen das Verhalten

Die `EffectBehavior` Klasse angefügt werden kann, um die [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung eines Steuerelements, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

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

Die `Group` und `Name` Eigenschaften des Verhaltens werden festgelegt, auf die Werte von der [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) und [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) Attribute für die Klasse Effekt in jede Clientplattform-spezifische Projekt.

Zur Laufzeit, wenn das Verhalten an angefügt ist die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) -Steuerelement, das `Xamarin.LabelShadowEffect` werden hinzugefügt werden, um des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung. Dies führt zu einem Schatten angezeigten Elementtexts hinzugefügt wird die `Label` zu steuern, wie in den folgenden Screenshots dargestellt:

![](effect-behavior-images/screenshots.png "Beispielanwendung mit EffectsBehavior")

Der Vorteil der Verwendung dieses Verhalten hinzufügen und Entfernen der Auswirkungen von Steuerelementen ist, dass vom Code-Behind-Dateien mit Codebausteinen Auswirkungen Fehlerbehandlungscode entfernt werden kann.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mithilfe eines Verhaltens einen Effekt an ein Steuerelement hinzufügen. Die `EffectBehavior` Klasse ist eine wiederverwendbare Xamarin.Forms benutzerdefiniertes Verhalten, die hinzugefügt ein [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) Instanz mit einem Steuerelement, wenn das Verhalten ist mit dem Steuerelement verbunden, und entfernt die `Effect` -Instanz auf, wenn das Verhalten ist vom Steuerelement getrennt.


## <a name="related-links"></a>Verwandte Links

- [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Auswirkungen Verhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/effectbehavior/)
- [Verhalten](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Verhalten<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
