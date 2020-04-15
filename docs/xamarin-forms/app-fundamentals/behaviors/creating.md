---
title: Erstellen von Xamarin.Forms-Verhalten
description: Xamarin.Forms-Verhalten werden von den Klassen „Behavior“ und „Behavior<T>“ abgeleitet. In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms-Verhalten erstellen und verwenden können.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 42ad56a7ae34bcef638ed25bea267dcabd21e20c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "77131095"
---
# <a name="create-xamarinforms-behaviors"></a>Erstellen von Xamarin.Forms-Verhalten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)

_Xamarin.Forms-Verhalten werden von der Klasse „Behavior“ oder „Behavior&lt;T&gt; abgeleitet. In diesem Artikel wird veranschaulicht, wie Sie Xamarin.Forms-Verhalten erstellen und verwenden können._

## <a name="overview"></a>Übersicht

So erstellen Sie ein Xamarin.Forms-Verhalten:

1. Erstellen Sie eine Klasse, die von der [`Behavior`](xref:Xamarin.Forms.Behavior)- oder der [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)-Klasse erbt. `T` beschreibt den Typ des Steuerelements, für das das Verhalten gelten soll.
1. Überschreiben Sie die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))-Methode, um gegebenenfalls erforderliches Setup durchzuführen.
1. Überschreiben Sie die [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode, um gegebenenfalls erforderliche Bereinigung durchzuführen.
1. Implementieren Sie die Kernfunktionalität des Verhaltens.

Dies resultiert in der Struktur, die im folgenden Codebeispiel gezeigt wird:

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

Die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))-Methode wird sofort nach dem Anfügen des Verhaltens an ein Steuerelement ausgeführt. Diese Methode empfängt einen Verweis auf das Steuerelement, dem sie angefügt ist, und kann dazu verwendet werden, die Ereignishandler zu registrieren oder andere Setupschritte durchzuführen, die zur Unterstützung der Verhaltensfunktionalität erforderlich sind. Sie können beispielsweise ein Ereignis mit einem Steuerelement verknüpfen. Die Verhaltensfunktionalität würde dann in den Ereignishandler für das Ereignis implementiert werden.

Die [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode wird ausgelöst, wenn das Verhalten aus dem Steuerelement entfernt wird. Diese Methode empfängt einen Verweis auf das Steuerelement, dem sie angefügt ist, und wird dazu verwendet, gegebenenfalls erforderliche Bereinigungen durchzuführen. Sie könnten ein Ereignis beispielsweise von einem Steuerelement lösen, um Arbeitsspeicherverluste zu verhindern.

Das Verhalten kann dann verwendet werden, indem es an die [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung des entsprechenden Steuerelements angefügt wird.

## <a name="creating-a-xamarinforms-behavior"></a>Erstellen eines Xamarin.Forms-Verhaltens

Die Beispielanwendung veranschaulicht ein `NumericValidationBehavior`-Verhalten, das den Wert rot hervorhebt, der vom Benutzer in ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement eingegeben wird, sofern es sich nicht um einen `double`-Wert handelt. Das Verhalten wird im folgenden Codebeispiel veranschaulicht:

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

Das `NumericValidationBehavior`-Verhalten wird von der [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)-Klasse abgeleitet, bei der `T` einem [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement entspricht. Die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))-Methode registriert einen Ereignishandler für das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)-Ereignis mit der [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode, die die Registrierung des `TextChanged`-Ereignisses aufhebt, um Arbeitsspeicherverluste zu verhindern. Die Kernfunktionalität des Verhaltens wird von der `OnEntryTextChanged`-Methode bereitgestellt, die den vom Benutzer in `Entry` eingegebenen Wert analysiert und die [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)-Eigenschaft auf rot festlegt, wenn es sich nicht um einen `double`-Wert handelt.

> [!NOTE]
> Xamarin.Forms legt nicht die `BindingContext`-Eigenschaft von Verhalten fest, da Verhalten mit Formatvorlagen freigegeben und auf mehrere Steuerelemente angewendet werden können.

## <a name="consuming-a-xamarinforms-behavior"></a>Nutzen eines Xamarin.Forms-Verhaltens

Jedes Xamarin.Forms-Steuerelement verfügt über eine [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung, der wie im folgenden XAML-Codebeispiel veranschaulicht mindestens ein Verhalten hinzugefügt werden kann:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Das entsprechende [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Bei der Ausführung reagiert das Verhalten gemäß der Verhaltensimplementierung auf die Interaktion mit dem Steuerelement. In den folgenden Screenshots wird die Reaktion des Verhaltens auf ungültige Eingabe veranschaulicht:

[![](creating-images/screenshots-sml.png "Sample Application with Xamarin.Forms Behavior")](creating-images/screenshots.png#lightbox "Sample Application with Xamarin.Forms Behavior")

> [!NOTE]
> Verhalten werden für einen spezifischen Steuerelementtyp (oder eine übergeordnete Klasse, die für mehrere Steuerelemente gelten kann) geschrieben und sollten nur zu kompatiblen Steuerelementen hinzugefügt werden. Wenn ein Verhalten an ein inkompatibles Steuerelement angefügt wird, wird eine Ausnahme ausgelösten.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Nutzen eines Xamarin.Forms-Verhaltens mit einer Formatvorlage

Verhalten können auch von einer expliziten oder impliziten Formatvorlage genutzt werden. Allerdings kann keine Formatvorlage erstellt werden, die die [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Eigenschaft eines Steuerelements festlegt, da die Eigenschaft schreibgeschützt ist. Die Lösung besteht darin, eine angefügte Eigenschaft zur Behavior-Klasse hinzuzufügen, die das Hinzufügen und Entfernen des Verhaltens steuert. Gehen Sie wie folgt vor:

1. Fügen Sie der Behavior-Klasse eine angefügte Eigenschaft hinzu, die steuert, ob das Verhalten an das Steuerelement angefügt oder von diesem entfernt wird. Stellen Sie sicher, dass die angefügte Eigenschaft einen `propertyChanged`-Delegaten registriert, der ausgeführt wird, wenn der Wert der Eigenschaft geändert wird.
1. Erstellen Sie einen `static`-Getter und -Setter für die angefügte Eigenschaft.
1. Implementieren Sie Logik zum Hinzufügen und Entfernen des Verhaltens im `propertyChanged`-Delegaten.

Im folgenden Codebeispiel wird eine angefügte Eigenschaft veranschaulicht, die das Hinzufügen und Entfernen von `NumericValidationBehavior` steuert:

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

Die `NumericValidationBehavior`-Klasse enthält eine angefügte Eigenschaft namens `AttachBehavior` mit einem `static`-Getter und -Setter, mit denen das Hinzufügen und Entfernen des Verhaltens aus dem Steuerelement gesteuert wird, an das das Verhalten angefügt wird. Diese angefügte Eigenschaft registriert die `OnAttachBehaviorChanged`-Methode, die ausgeführt wird, wenn der Wert der Eigenschaft geändert wird. Basierend auf dem Wert der angefügten Eigenschaft `AttachBehavior` fügt diese Methode das Verhalten an das Steuerelement an oder entfernt es.

Im folgenden Codebeispiel wird eine *explizite* Formatvorlage für `NumericValidationBehavior` veranschaulicht, die die angefügte Eigenschaft `AttachBehavior` verwendet und auf [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelemente angewendet werden kann:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Die [`Style`](xref:Xamarin.Forms.Style)-Klasse kann auf ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement angewendet werden, indem ihre [`Style`](xref:Xamarin.Forms.NavigableElement.Style)-Eigenschaft wie im folgenden Codebeispiel gezeigt mithilfe der Markuperweiterung `StaticResource` für die `Style`-Instanz festgelegt wird:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Weitere Informationen zu Formatvorlagen finden Sie unter [Styles (Formatvorlagen)](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Sie können bindbare Eigenschaften zwar zu einem Verhalten hinzufügen, das mit XAML festgelegt oder abgefragt wird, jedoch sollten sie nicht von mehreren Steuerelementen in einer `Style`-Klasse einer `ResourceDictionary`-Klasse verwendet werden, wenn Sie Verhalten erstellen, die über einen Zustand verfügen.

### <a name="removing-a-behavior-from-a-control"></a>Entfernen eines Verhaltens aus einem Steuerelement

Die [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode wird ausgelöst, wenn ein Verhalten aus einem Steuerelement entfernt wird. Sie wird dazu verwendet, gegebenenfalls erforderliche Bereinigungen wie das Lösen von einem Ereignis durchzuführen, um Arbeitsspeicherverluste zu verhindern. Allerdings werden Verhalten nicht implizit aus Steuerelementen entfernt, es sei denn, die [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung des Steuerelements wird von einer `Remove`- oder `Clear`-Methode geändert. Im folgenden Codebeispiel wird das Entfernen eines spezifischen Verhaltens aus der `Behaviors`-Sammlung eines Steuerelements veranschaulicht:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Alternativ kann die [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung des Steuerelements wie im folgenden Codebeispiel gezeigt gelöscht werden:

```csharp
entry.Behaviors.Clear();
```

Beachten Sie außerdem, dass die Verhalten nicht implizit aus Steuerelementen entfernt werden, wenn Seiten per Pop aus dem Navigationsstapel entfernt werden. Stattdessen müssen sie explizit entfernt werden, bevor die Seiten sich nicht mehr im gültigen Bereich befinden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie Xamarin.Forms-Verhalten erstellen und verwenden können. Xamarin.Forms-Verhalten werden von der Klasse [`Behavior`](xref:Xamarin.Forms.Behavior) oder [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) abgeleitet.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms Behavior (sample) (Xamarin.Forms-Verhalten (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)
- [Xamarin.Forms Behavior applied with a Style (sample) (Xamarin.Forms-Verhalten mit angewendeter Formatvorlage (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehaviorstyle)
- [Behavior class (Behavior-Klasse)](xref:Xamarin.Forms.Behavior)
- [Behavior\<T>](xref:Xamarin.Forms.Behavior`1)
