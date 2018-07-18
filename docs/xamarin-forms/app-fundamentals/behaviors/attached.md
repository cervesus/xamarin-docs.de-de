---
title: Angefügte Verhaltensweisen
description: Angefügte Verhaltensweisen sind statische Klassen mit einer oder mehreren angefügte Eigenschaften. Dieser Artikel veranschaulicht das Erstellen und nutzen die angefügte Verhaltensweisen.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995345"
---
# <a name="attached-behaviors"></a>Angefügte Verhaltensweisen

_Angefügte Verhaltensweisen sind statische Klassen mit einer oder mehreren angefügte Eigenschaften. Dieser Artikel veranschaulicht das Erstellen und nutzen die angefügte Verhaltensweisen._

## <a name="overview"></a>Übersicht

Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft an. Sie in einer Klasse definiert, aber auf andere Objekte angefügt und eignen sich erkennbaren in XAML als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten.

Eine angefügte Eigenschaft definieren kann eine `propertyChanged` Delegat, der ausgeführt wird, wenn der Wert der Eigenschaft ändert z. B. wenn die Eigenschaft für ein Steuerelement festgelegt ist,. Wenn die `propertyChanged` Delegaten ausführt, die übergeben wurde, eines Verweis auf das Steuerelement auf dem er angefügt wird, und die Parameter, die die alten und neuen Werte für die Eigenschaft enthalten. Dieser Delegat kann verwendet werden, um neue Funktionen für das Steuerelement hinzuzufügen, die die Eigenschaft angefügt ist, durch Bearbeiten des Verweis, der wie folgt, übergeben wird:

1. Die `propertyChanged` Delegaten wandelt den Steuerelementverweis, der als empfangen wird eine [ `BindableObject` ](xref:Xamarin.Forms.BindableObject), soll, dass der Steuerelementtyp, der das Verhalten um zu verbessern.
1. Die `propertyChanged` Delegaten ändert die Eigenschaften des Steuerelements ruft Methoden des Steuerelements, oder Register-Ereignishandlern für Ereignisse verfügbar gemacht werden, durch das Steuerelement zum Implementieren der Kernfunktionen Verhalten.

Ein Problem mit dem angefügten Verhalten ist, in dem sie definiert sind eine `static` -Klasse, mit `static` Eigenschaften und Methoden. Dies erschwert das angefügte Verhaltensweisen zu erstellen, die Zustand aufweisen. Darüber hinaus haben Xamarin.Forms-Verhaltensweisen angefügte Verhaltensweisen als bevorzugten Ansatz zur Erstellung von Verhalten ersetzt. Weitere Informationen zu Xamarin.Forms-Verhaltensweisen, finden Sie unter [Xamarin.Forms-Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Wiederverwendbare Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Erstellen ein angefügtes Verhalten

Die beispielanwendung veranschaulicht eine `NumericValidationBehavior`, die hervorgehoben, dass die vom Benutzer eingegebenen Wert das in eine [ `Entry` ](xref:Xamarin.Forms.Entry) steuern in Rot angezeigt, wenn er nicht ist eine `double`. Das Verhalten wird im folgenden Codebeispiel gezeigt:

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

Die `NumericValidationBehavior` -Klasse enthält eine angefügte Eigenschaft mit dem Namen `AttachBehavior` mit einem `static` Getter und Setter, die steuert, das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement, mit der er verknüpft. Diese angefügte Eigenschaft registriert die `OnAttachBehaviorChanged` -Methode, die ausgeführt werden, wenn der Wert der Eigenschaft geändert wird. Diese Methode registriert oder Aufhebung registriert einen Ereignishandler für die [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) Ereignis anhand des Werts, der die `AttachBehavior` angefügte Eigenschaft. Die Kernfunktionen des Verhaltens erfolgt über die `OnEntryTextChanged` -Methode, die den eingegebenen Wert analysiert die [ `Entry` ](xref:Xamarin.Forms.Entry) nach Benutzer und Gruppen die `TextColor` -Eigenschaft auf Rot, wenn der Wert ist kein `double`.

## <a name="consuming-an-attached-behavior"></a>Nutzen ein angefügtes Verhalten

Die `NumericValidationBehavior` Klasse kann verwendet werden, durch das Hinzufügen der `AttachBehavior` angefügten Eigenschaft, um eine [ `Entry` ](xref:Xamarin.Forms.Entry) zu steuern, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Die entsprechende [ `Entry` ](xref:Xamarin.Forms.Entry) in c# wird im folgenden Codebeispiel dargestellt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Zur Laufzeit wird das Verhalten auf Benutzerinteraktion mit dem Steuerelement, entsprechend der verhaltensimplementierung von reagieren. Die folgenden Screenshots veranschaulichen das angefügte Verhalten reagieren auf ungültige Eingabe:

[![](attached-images/screenshots-sml.png "Beispielanwendung mit dem angefügten Verhalten")](attached-images/screenshots.png#lightbox "Beispielanwendung mit dem angefügten Verhalten")

> [!NOTE]
> Angefügte Verhaltensweisen für einen bestimmten Steuerelementtyp (oder eine übergeordnete Klasse, die für viele Steuerelemente angewendet werden kann) geschrieben wurden, und sie sollten nur zu einem kompatiblen-Steuerelement hinzugefügt werden. Unbekannter Verhalten führt und hängt die verhaltensimplementierung möchten, fügen Sie ein Verhalten auf ein Steuerelement nicht kompatibel.

### <a name="removing-an-attached-behavior-from-a-control"></a>Entfernen ein angefügtes Verhalten von einem Steuerelement

Die `NumericValidationBehavior` Klasse kann von einem Steuerelement entfernt werden, durch Festlegen der `AttachBehavior` angefügte Eigenschaft zu `false`, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Die entsprechende [ `Entry` ](xref:Xamarin.Forms.Entry) in c# wird im folgenden Codebeispiel dargestellt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Zur Laufzeit die `OnAttachBehaviorChanged` Methode ausgeführt, wenn der Wert des der `AttachBehavior` angefügte Eigenschaft festgelegt ist, um `false`. Die `OnAttachBehaviorChanged` Methode wird dann aufheben, registrieren den Ereignishandler für die [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) -Ereignis, um sicherzustellen, dass das Verhalten wird nicht ausgeführt, während der Benutzer mit dem Steuerelement interagiert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht das Erstellen und nutzen die angefügte Verhaltensweisen. Angefügte Verhaltensweisen sind `static` Klassen mit einer oder mehreren angefügte Eigenschaften.


## <a name="related-links"></a>Verwandte Links

- [Angefügte Verhaltensweisen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
