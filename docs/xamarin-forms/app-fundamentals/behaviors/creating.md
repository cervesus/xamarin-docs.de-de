---
title: Xamarin.Forms-Verhalten
description: Xamarin.Forms Verhalten werden erstellt, durch Ableiten von das Verhalten oder die Verhalten<T> Klasse. In diesem Artikel wird das Erstellen und Verwenden von Xamarin.Forms Verhalten veranschaulicht.
ms.topic: article
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: c70e4c9ec49b48c3bf6ecc6a4944d992f8ae930a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms-Verhalten

_Xamarin.Forms Verhalten werden erstellt, durch Ableiten von das Verhalten oder die Verhalten<T> Klasse. In diesem Artikel wird das Erstellen und Verwenden von Xamarin.Forms Verhalten veranschaulicht._

## <a name="overview"></a>Übersicht

Der Prozess zum Erstellen einer Xamarin.Forms-Verhalten lautet wie folgt:

1. Erstellen Sie eine Klasse, die von erben die [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) oder [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) -Klasse, wobei `T` ist der Typ des Steuerelements an, das Verhalten gelten soll.
1. Überschreiben Sie die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode, um alle erforderlichen Setup auszuführen.
1. Überschreiben Sie die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methode, um alle erforderlichen cleanupmaßnahmen ausführen.
1. Implementieren Sie die Kernfunktionalität des Verhaltens.

Daraus ergibt sich die Struktur, die im folgenden Codebeispiel gezeigt:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

Die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode wird immer dann ausgelöst, unmittelbar nach dem das Verhalten eines Steuerelements zugeordnet ist. Diese Methode empfängt einen Verweis auf das Steuerelement, zu dem er verbunden ist, und kann verwendet werden, um Ereignishandler zu registrieren oder Ausführen von anderen Einrichtung, die zur Unterstützung der Verhaltensfunktionalität erforderlich ist. Sie können z. B. auf ein Ereignis für ein Steuerelement abonnieren. Die Verhaltensfunktionalität würde dann im Ereignishandler für das Ereignis implementiert werden.

Die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methode wird immer dann ausgelöst, wenn das Verhalten aus dem Steuerelement entfernt wird. Diese Methode empfängt einen Verweis auf das Steuerelement, auf dem er verbunden ist, und wird verwendet, um alle erforderlichen cleanupmaßnahmen ausführen. Sie konnten z. B. ein Ereignis für ein Steuerelement, um Speicherverluste zu verhindern kündigen.

Das Verhalten kann dann genutzt werden, durch Anfügen, damit die [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung von das entsprechende Steuerelement.

## <a name="creating-a-xamarinforms-behavior"></a>Erstellen eine Xamarin.Forms-Verhalten

Die beispielanwendung für veranschaulicht eine `NumericValidationBehavior`, die hebt die vom Benutzer eingegebenen Wert der in ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) steuern in Rot dargestellt, ist er kein `double`. Das Verhalten wird im folgenden Codebeispiel gezeigt:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Die `NumericValidationBehavior` leitet sich von der [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) -Klasse, in denen `T` ist ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode registriert einen Ereignishandler für die [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) -Ereignis mit der [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methode, die Deduplizierung beim Registrieren der `TextChanged`Ereignis, um zu verhindern, dass Arbeitsspeicher von Speicherverlusten. Die Kernfunktionalität des Verhaltens wird bereitgestellt, indem der `OnEntryTextChanged` -Methode, die den vom Benutzer in eingegebenen Wert analysiert der `Entry`, und legt sie fest der [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) -Eigenschaft auf Rot, wenn der Wert ist eine `double`.

> [!NOTE]
> **Hinweis**: Xamarin.Forms ist nicht festgelegt. die `BindingContext` eines Verhaltens, da Verhalten freigegeben und auf mehrere Steuerelemente über Stile angewendet werden können.

## <a name="consuming-a-xamarinforms-behavior"></a>Nutzen eine Xamarin.Forms-Verhalten

Jedes Xamarin.Forms-Steuerelement verfügt über eine [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung, die eine oder mehrere Verhalten können, hinzugefügt werden wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Die Entsprechung [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) in c# ist im folgenden Codebeispiel gezeigt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Zur Laufzeit wird das Verhalten entsprechend die verhaltensimplementierung zur Interaktion mit dem Steuerelement reagieren. Die folgenden Screenshots veranschaulicht das Verhalten reagieren auf die Eingabe ist ungültig:

[ ![](creating-images/screenshots-sml.png "Beispielanwendung mit Xamarin.Forms Verhalten")](creating-images/screenshots.png "Beispielanwendung mit Xamarin.Forms-Verhalten")

> [!NOTE]
> **Hinweis**: Verhalten für einen bestimmten Steuerelementtyp (oder eine übergeordnete Klasse, die für viele Steuerelemente angewendet werden kann) geschrieben sind, und sie sollte nur zu einem kompatiblen Steuerelement hinzugefügt werden. Sie versuchen, ein Verhalten auf das Steuerelement nicht kompatibel anzufügen führt eine Ausnahme ausgelöst.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Nutzen eine Xamarin.Forms-Verhalten mit einer Formatvorlage

Verhaltensweisen können auch von einer expliziten oder impliziten Stil genutzt werden. Allerdings erstellen Sie eine Formatvorlage, der festlegt der [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Eigenschaft eines Steuerelements ist nicht möglich, da die Eigenschaft schreibgeschützt ist. Die Lösung besteht darin, die Behavior-Klasse, die steuert, hinzufügen und entfernen Sie das Verhalten von einer angefügten Eigenschaft hinzugefügt. Der Prozess ist wie folgt aus:

1. Die Behavior-Klasse, die verwendet wird, steuern Sie das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement angefügt, das Verhalten wird, eine angefügte Eigenschaft hinzugefügt. Stellen Sie sicher, dass die angefügte Eigenschaft registriert eine `propertyChanged` Delegat, der ausgeführt wird, wenn der Wert der Eigenschaft ändert.
1. Erstellen einer `static` Getter und Setter für die angefügte Eigenschaft.
1. Implementieren Sie die Logik in der `propertyChanged` Delegaten hinzufügen und entfernen das Verhalten.

Das folgende Codebeispiel zeigt eine angefügte Eigenschaft, die steuert, hinzufügen und Entfernen von der `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

Die `NumericValidationBehavior` Klasse enthält eine angefügte Eigenschaft mit dem Namen `AttachBehavior` mit einem `static` Getter und Setter-Methode, die steuert, das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement an das es angefügt. Dies angefügte Eigenschaft registriert die `OnAttachBehaviorChanged` Methode, die ausgeführt wird, wenn der Wert der Eigenschaft ändert. Diese Methode fügt hinzu oder entfernt Sie das Verhalten für das Steuerelement, basierend auf dem Wert von der `AttachBehavior` -Eigenschaft.

Das folgende Codebeispiel zeigt eine *explizite* Stil für die `NumericValidationBehavior` , verwendet der `AttachBehavior` angefügten Eigenschaft, und der zugewiesen werden können [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelemente:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) angewendet werden können, um eine [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement durch Festlegen seiner [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft, um die `Style` -Zielinstanz aus, die `StaticResource` Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> **Hinweis**: während Sie hinzufügen können, um ein Verhalten, das festgelegt oder in XAML, abgefragt werden, wenn Sie Verhaltensweisen, die erstellen Bindungseigenschaften Status sie sollten nicht freigegeben werden zwischen Steuerelementen in einem `Style` in einem `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Entfernen ein Verhalten aus einem Steuerelement

Die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methode wird immer dann ausgelöst, wenn ein Verhalten von einem Steuerelement entfernt wird und verwendet wird, um alle erforderlichen cleanupmaßnahmen z. B. ein Ereignis, um zu verhindern, dass einen Speicherverlust Aufhebens ausführen. Verhalten werden jedoch nicht implizit entfernt von Steuerelementen, wenn des Steuerelements [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung geändert wird, indem eine `Remove` oder `Clear` Methode. Im folgenden Codebeispiel wird veranschaulicht, entfernen ein bestimmtes Verhalten aus einem Steuerelement `Behaviors` Auflistung:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternativ können Sie des Steuerelements des [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung kann gelöscht werden, wie im folgenden Codebeispiel gezeigt:

```csharp
entry.Behaviors.Clear();
```

Beachten Sie außerdem, dass Verhaltensweisen von Steuerelementen nicht implizit entfernt werden, wenn Seiten aus dem Navigationsstapel per pop ausgelesen werden. Stattdessen müssen sie explizit vor der Seiten außerhalb des gültigen Bereichs entfernt werden.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie erstellen und nutzen Xamarin.Forms-Verhalten. Xamarin.Forms Verhalten entstehen durch Ableiten von der [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) oder [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) Klasse.


## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-Verhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms Verhalten angewendet wird, in ein Format (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Verhalten](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Verhalten<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
