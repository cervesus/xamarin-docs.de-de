---
title: Angefügte Verhaltensweisen
description: Angefügte Verhaltensweisen sind statische Klassen mit mindestens einer angefügten Eigenschaft. In diesem Artikel wird veranschaulicht, wie Sie angefügte Verhaltensweisen erstellen und verwenden können.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 98c15f3e3503ccae164c2bb14b6de13c227893d6
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051555"
---
# <a name="attached-behaviors"></a>Angefügte Verhaltensweisen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)

_Angefügte Verhaltensweisen sind statische Klassen mit mindestens einer angefügten Eigenschaft. In diesem Artikel wird veranschaulicht, wie Sie angefügte Verhaltensweisen erstellen und verwenden können._

## <a name="overview"></a>Übersicht

Eine angefügte Eigenschaft ist eine besondere Art einer bindbaren Eigenschaft. Sie ist in einer Klasse definiert, jedoch an andere Objekte angefügt, und ist in XAML als Attribut erkennbar, das eine Klasse und einen Eigenschaftsnamen enthält, die durch einen Punkt voneinander getrennt sind.

Eine angefügte Eigenschaft kann einen `propertyChanged`-Delegaten definieren, der bei einer Änderung des Eigenschaftswerts ausgeführt wird, z. B. bei Festlegen der Eigenschaft für ein Steuerelement. Wenn der `propertyChanged`-Delegat ausgeführt wird, werden ein Verweis auf das Steuerelement, an das er angefügt ist, und Parameter übergeben, welche die alten und neuen Werte der Eigenschaft enthalten. Mithilfe dieses Delegats können neue Funktionen an das Steuerelement angefügt werden, an das die Eigenschaft angefügt ist, indem der übergebene Verweis wie folgt angepasst wird:

1. Der `propertyChanged`-Delegat wandelt den Steuerelementverweis, der als [`BindableObject`](xref:Xamarin.Forms.BindableObject) empfangen wird, in den Steuerelementtyp um, der von dem entsprechend entworfenen Verhalten verbessert wird.
1. Der `propertyChanged`-Delegat ändert Eigenschaften des Steuerelements, ruft Methoden des Steuerelements auf oder erfasst Ereignishandler für Ereignisse, die vom Steuerelement verfügbar gemacht wurden, um das Verhalten der Kernfunktionen zu implementieren.

Ein Problem bei angefügten Verhalten besteht darin, dass sie in der `static`-Klasse mit `static`-Eigenschaften und -Methoden definiert werden. Dadurch wird das Erstellen angefügter Verhalten erschwert, die einen Zustand aufweisen. Darüber hinaus ersetzen Xamarin.Forms-Verhalten angefügte Verhalten als bevorzugten Ansatz zur Erstellung von Verhalten. Weitere Informationen über Xamarin.Forms-Verhaltensweisen finden Sie unter [Xamarin.Forms Behaviors (Xamarin.Forms-Verhaltensweisen)](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Reusable Behaviors (Wiederverwendbare Verhaltensweisen)](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Erstellen eines angefügten Verhaltens

Die Beispielanwendung veranschaulicht ein `NumericValidationBehavior`-Verhalten, das den Wert rot hervorhebt, der vom Benutzer in ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement eingegeben wird, sofern es sich nicht um einen `double`-Wert handelt. Dies wird im folgenden Codebeispiel gezeigt:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

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
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Die `NumericValidationBehavior`-Klasse enthält eine angefügte Eigenschaft namens `AttachBehavior` mit einem `static`-Getter und -Setter, mit denen das Hinzufügen und Entfernen des Verhaltens aus dem Steuerelement gesteuert wird, an das das Verhalten angefügt wird. Diese angefügte Eigenschaft registriert die `OnAttachBehaviorChanged`-Methode, die ausgeführt wird, wenn der Wert der Eigenschaft geändert wird. Bei dieser Methode wird ein Ereignishandler basierend auf dem Wert der angefügten `AttachBehavior`-Eigenschaft für das Ereignis [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) registriert, oder seine Registrierung wird aufgehoben. Die Kernfunktionalität des Verhaltens wird von der `OnEntryTextChanged`-Methode bereitgestellt, die den vom Benutzer in [`Entry`](xref:Xamarin.Forms.Entry) eingegebenen Wert analysiert und die `TextColor`-Eigenschaft auf rot festlegt, wenn es sich nicht um einen `double`-Wert handelt.

## <a name="consuming-an-attached-behavior"></a>Verwenden von angefügtem Verhalten

Die `NumericValidationBehavior`-Klasse kann, wie im folgenden XAML-Codebeispiel veranschaulicht, verwendet werden, indem die angefügte `AttachBehavior`-Eigenschaft zu einem [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement hinzugefügt wird:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Das entsprechende [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Bei der Ausführung reagiert das Verhalten gemäß der Verhaltensimplementierung auf die Interaktion mit dem Steuerelement. In den folgenden Screenshots wird die Reaktion des angefügten Verhaltens auf ungültige Eingabe veranschaulicht:

[![](attached-images/screenshots-sml.png "Beispielanwendung mit angefügtem Verhalten")](attached-images/screenshots.png#lightbox "Beispielanwendung mit angefügtem Verhalten")

> [!NOTE]
> Angefügte Verhaltensweisen werden für einen spezifischen Steuerelementtyp (oder eine übergeordnete Klasse, die für mehrere Steuerelemente gelten kann) geschrieben und sollten nur zu kompatiblen Steuerelementen hinzugefügt werden. Der Versuch, ein Verhalten an ein nicht kompatibles Steuerelement anzufügen, führt zu einem unbekannten Verhalten und hängt von der Verhaltensimplementierung ab.

### <a name="removing-an-attached-behavior-from-a-control"></a>Entfernen von angefügtem Verhalten aus einem Steuerelement

Die `NumericValidationBehavior`-Klasse kann, wie im folgenden XAML-Codebeispiel veranschaulicht, entfernt werden, indem die angefügte `AttachBehavior`-Eigenschaft auf `false` festgelegt wird:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Das entsprechende [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Zur Laufzeit wird die `OnAttachBehaviorChanged`-Methode ausgeführt, wenn der Wert der angefügten `AttachBehavior`-Eigenschaft auf `false` festgelegt ist. Anschließend wird mit der `OnAttachBehaviorChanged`-Methode die Registrierung des Ereignishandlers für das Ereignis [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) aufgehoben. Dabei wird sichergestellt, dass das Verhalten nicht ausgeführt wird, während der Benutzer mit dem Steuerelement interagiert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie angefügte Verhaltensweisen erstellen und verwenden können. Angefügte Verhaltensweisen sind `static`-Klassen mit mindestens einer angefügten Eigenschaft.


## <a name="related-links"></a>Verwandte Links

- [Angefügte Verhaltensweisen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
