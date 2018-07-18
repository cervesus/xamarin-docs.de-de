---
title: Xamarin.Forms-Verhaltensweisen
description: Xamarin.Forms-Verhaltensweisen werden erstellt, durch Ableiten von das Verhalten oder die Verhalten<T> Klasse. Dieser Artikel veranschaulicht das Erstellen und Nutzen von Xamarin.Forms-Verhaltensweisen.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998894"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms-Verhaltensweisen

_Xamarin.Forms-Verhaltensweisen werden erstellt, durch Ableiten von das Verhalten oder die Verhalten<T> Klasse. Dieser Artikel veranschaulicht das Erstellen und Nutzen von Xamarin.Forms-Verhaltensweisen._

## <a name="overview"></a>Übersicht

Der Prozess zum Erstellen einer Xamarin.Forms-Verhalten lautet wie folgt aus:

1. Erstellen Sie eine Klasse, die von erbt die [ `Behavior` ](xref:Xamarin.Forms.Behavior) oder [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) -Klasse, wobei `T` ist der Typ des Steuerelements auf die das Verhalten angewendet werden soll.
1. Überschreiben der [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode zum Durchführen der Konfiguration erforderlich.
1. Überschreiben der [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methode, um alle erforderlichen Cleanups durchzuführen.
1. Implementieren Sie die Kernfunktionen des Verhaltens.

Dadurch wird in der Struktur, die im folgenden Codebeispiel gezeigt:

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

Die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode wird immer dann ausgelöst, unmittelbar nach das Verhalten auf ein Steuerelement angehängt ist. Diese Methode empfängt einen Verweis auf das Steuerelement, das es angefügt ist, und kann verwendet werden, um Ereignishandler zu registrieren, oder führen Sie andere Setup, die erforderlich sind, zur Unterstützung der Verhaltensfunktionalität. Sie können z. B. auf ein Ereignis für ein Steuerelement abonnieren. Die Funktionen Verhalten würde dann im Ereignishandler für das Ereignis implementiert werden.

Die [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methode wird immer dann ausgelöst, wenn das Verhalten aus dem Steuerelement entfernt wird. Diese Methode empfängt einen Verweis auf das Steuerelement, das es angefügt ist, und wird verwendet, um alle erforderlichen Cleanups durchzuführen. Beispielsweise könnten Sie ein Ereignisabonnement für ein Steuerelement, um Speicherverluste zu verhindern wieder abmelden.

Das Verhalten kann dann verwendet werden, durch Anfügen, damit die [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung von das entsprechende Steuerelement.

## <a name="creating-a-xamarinforms-behavior"></a>Erstellen einer Xamarin.Forms-Verhalten

Die beispielanwendung veranschaulicht eine `NumericValidationBehavior`, die hervorgehoben, dass die vom Benutzer eingegebenen Wert das in eine [ `Entry` ](xref:Xamarin.Forms.Entry) steuern in Rot angezeigt, wenn er nicht ist eine `double`. Das Verhalten wird im folgenden Codebeispiel gezeigt:

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

Die `NumericValidationBehavior` leitet sich von der [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) -Klasse, in denen `T` ist ein [ `Entry` ](xref:Xamarin.Forms.Entry). Die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode registriert einen Ereignishandler für die [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) -Ereignis, mit der [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methode Aufheben der Registrierung der `TextChanged`Ereignis, um zu verhindern, dass Arbeitsspeicher von Speicherverlusten. Die Kernfunktionen des Verhaltens erfolgt über die `OnEntryTextChanged` -Methode, die analysiert die vom Benutzer eingegebenen Wert der in der `Entry`, und legt sie fest der [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) -Eigenschaft auf Rot, wenn der Wert ist kein `double`.

> [!NOTE]
> Xamarin.Forms wird nicht festgelegt. die `BindingContext` eines Verhaltens, da das Verhalten können freigegeben und auf mehrere Steuerelemente über Stile angewendet.

## <a name="consuming-a-xamarinforms-behavior"></a>Nutzen eine Xamarin.Forms-Verhalten

Alle Xamarin.Forms-Steuerelements verfügt über eine [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung, die eine oder mehrere Verhaltensweisen können, hinzugefügt werden wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Die entsprechende [ `Entry` ](xref:Xamarin.Forms.Entry) in c# wird im folgenden Codebeispiel dargestellt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Zur Laufzeit wird das Verhalten auf Benutzerinteraktion mit dem Steuerelement, entsprechend der verhaltensimplementierung von reagieren. Die folgenden Screenshots veranschaulichen das Verhalten reagieren auf ungültige Eingabe:

[![](creating-images/screenshots-sml.png "Beispielanwendung mit Xamarin.Forms Verhalten")](creating-images/screenshots.png#lightbox "Beispielanwendung mit Xamarin.Forms-Verhalten")

> [!NOTE]
> Verhalten für einen bestimmten Steuerelementtyp (oder eine übergeordnete Klasse, die für viele Steuerelemente angewendet werden kann) geschrieben wurden, und sie sollten nur zu einem kompatiblen-Steuerelement hinzugefügt werden. Es wird versucht, fügen Sie ein Verhalten auf ein Steuerelement nicht kompatibel, führt zu einer ausgelösten Ausnahme.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Nutzen eine Xamarin.Forms-Verhalten mit einem Stil

Verhaltensweisen können auch durch eine explizite oder implizite Stil genutzt werden. Allerdings erstellen Sie einen Stil, der festlegt der [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Eigenschaft eines Steuerelements ist nicht möglich, da die Eigenschaft schreibgeschützt ist. Die Lösung besteht darin, die Behavior-Klasse, die steuert, hinzufügen und entfernen das Verhalten eine angefügte Eigenschaft hinzugefügt. Der Prozess ist wie folgt aus:

1. Hinzufügen einer angefügten Eigenschaft, die Behavior-Klasse, die verwendet wird, um das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement angefügt, das Verhalten wird, zu steuern. Stellen Sie sicher, dass die angefügte Eigenschaft registriert eine `propertyChanged` Delegat, der ausgeführt wird, wenn der Wert der Eigenschaft geändert wird.
1. Erstellen Sie eine `static` Getter und Setter für die angefügte Eigenschaft.
1. Implementieren Sie die Logik in die `propertyChanged` Delegaten hinzufügen und entfernen das Verhalten.

Das folgende Codebeispiel zeigt eine angefügte Eigenschaft, die steuert, hinzufügen und Entfernen der `NumericValidationBehavior`:

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

Die `NumericValidationBehavior` -Klasse enthält eine angefügte Eigenschaft mit dem Namen `AttachBehavior` mit einem `static` Getter und Setter, die steuert, das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement, mit der er verknüpft. Diese angefügte Eigenschaft registriert die `OnAttachBehaviorChanged` -Methode, die ausgeführt werden, wenn der Wert der Eigenschaft geändert wird. Diese Methode fügt hinzu oder entfernt das Verhalten für das Steuerelement, basierend auf den Wert des der `AttachBehavior` angefügte Eigenschaft.

Das folgende Codebeispiel zeigt eine *explizite* Stil für die `NumericValidationBehavior` , verwendet der `AttachBehavior` angefügte Eigenschaft und die auf angewendet werden können [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelemente:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

Die [ `Style` ](xref:Xamarin.Forms.Style) angewendet werden können, um eine [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement durch Festlegen der [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaft, um die `Style` -Zielinstanz aus, die `StaticResource` Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> Sie können zwar hinzufügen bindbare Eigenschaften auf ein Verhalten, das festgelegt oder in XAML, abgefragt werden, wenn Sie Verhaltensweisen erstellen, führen Sie verfügen über den Zustand sie sollte nicht freigegeben werden zwischen Steuerelementen in einem `Style` in eine `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Entfernen ein Verhalten aus einem Steuerelement

Die [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methode wird immer dann ausgelöst, wenn ein Verhalten, die von einem Steuerelement entfernt wird, und verwendet wird, um z. B. das Kündigen des Abonnements von einem Ereignis auf einen Speicherverlust zu verhindern, dass alle erforderlichen Cleanups durchzuführen. Verhalten werden jedoch nicht implizit entfernt von Steuerelementen, wenn des Steuerelements [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung geändert wird, indem eine `Remove` oder `Clear` Methode. Im folgenden Codebeispiel wird veranschaulicht, entfernen ein bestimmtes Verhalten von eines Steuerelements `Behaviors` Auflistung:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Sie können auch des Steuerelements [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung kann gelöscht werden, wie im folgenden Codebeispiel gezeigt:

```csharp
entry.Behaviors.Clear();
```

Beachten Sie außerdem, dass das Verhalten von Steuerelementen nicht implizit entfernt werden, wenn Seiten per POP vom Navigationsstapel entfernt werden. Stattdessen müssen sie explizit vor der Seiten außerhalb des gültigen Bereichs entfernt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie erstellen und Nutzen von Xamarin.Forms-Verhaltensweisen. Xamarin.Forms-Verhaltensweisen werden erstellt, durch Ableiten von der [ `Behavior` ](xref:Xamarin.Forms.Behavior) oder [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) Klasse.


## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-Verhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms-Verhalten angewendet wird, mit einer Formatvorlage (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Verhalten](xref:Xamarin.Forms.Behavior)
- [Verhalten<T>](xref:Xamarin.Forms.Behavior`1)
