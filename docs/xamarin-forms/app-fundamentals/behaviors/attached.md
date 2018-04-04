---
title: Angefügte Verhalten
description: Angefügte Verhalten sind statische Klassen mit einer oder mehreren angefügte Eigenschaften. Dieser Artikel veranschaulicht, wie erstellen und Nutzen von definierten Verhalten.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: af891ad1ff1389d5a48c6c47ba1914b8d4dfc20f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="attached-behaviors"></a>Angefügte Verhalten

_Angefügte Verhalten sind statische Klassen mit einer oder mehreren angefügte Eigenschaften. Dieser Artikel veranschaulicht, wie erstellen und Nutzen von definierten Verhalten._

## <a name="overview"></a>Übersicht

Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft. Sie sind in einer Klasse definiert, jedoch auf andere Objekte angefügt und als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten in XAML erkennbar sind.

Eine angefügte Eigenschaft definieren kann eine `propertyChanged` Delegat, der ausgeführt wird, wenn die Änderung des Werts der Eigenschaft z. B. wenn die Eigenschaft für ein Steuerelement festgelegt ist. Wenn die `propertyChanged` Delegaten ausführt, verfügt über einen Verweis auf das Steuerelement an das es angefügt wird und Parameter, enthalten die alten und neuen Werte für die Eigenschaft, übergeben. Hinzufügen neuer Funktionalitäten an das Steuerelement, das die Eigenschaft angefügt ist, durch Bearbeiten des Verweis, der wie folgt, übergeben wird, kann dieser Delegat verwendet werden:

1. Die `propertyChanged` Delegaten wandelt der kontrollverweis, die als empfangen wird eine [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)in der Steuerelementtyp ", die das Verhalten ist verbessern soll.
1. Die `propertyChanged` Delegaten ändert die Eigenschaften des Steuerelements ruft Methoden des Steuerelements oder Register-Ereignishandler für Ereignisse verfügbar gemacht werden, durch das Steuerelement, um die Kernfunktionen von Verhalten zu implementieren.

Ein Problem mit dem Verhalten angefügt ist, dass sie im definiert sind eine `static` -Klasse, mit `static` Eigenschaften und Methoden. Dies erschwert die angefügten Verhalten zu erstellen, die Zustand aufweisen. Darüber hinaus wurden Xamarin.Forms Verhalten angefügten Verhalten als den optimalen Ansatz Verhalten Konstruktion gegenüber ersetzt. Weitere Informationen zu Xamarin.Forms Verhalten, finden Sie unter [Xamarin.Forms Verhalten](~/xamarin-forms/app-fundamentals/behaviors/creating.md) und [Wiederverwendbaren Verhalten](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Erstellen ein angefügtes Verhalten

Die beispielanwendung für veranschaulicht eine `NumericValidationBehavior`, die hebt die vom Benutzer eingegebenen Wert der in ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) steuern in Rot dargestellt, ist er kein `double`. Das Verhalten wird im folgenden Codebeispiel gezeigt:

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

Die `NumericValidationBehavior` Klasse enthält eine angefügte Eigenschaft mit dem Namen `AttachBehavior` mit einem `static` Getter und Setter-Methode, die steuert, das Hinzufügen oder Entfernen des Verhaltens, das das Steuerelement an das es angefügt. Dies angefügte Eigenschaft registriert die `OnAttachBehaviorChanged` Methode, die ausgeführt wird, wenn der Wert der Eigenschaft ändert. Diese Methode registriert oder Deduplizierung registriert einen Ereignishandler für die [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) Ereignis anhand des Werts von der `AttachBehavior` -Eigenschaft. Die Kernfunktionalität des Verhaltens wird bereitgestellt, indem die `OnEntryTextChanged` -Methode, die den eingegebenen Wert analysiert die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) durch den Benutzer und Gruppen der `TextColor` -Eigenschaft auf Rot, wenn der Wert ist eine `double`.

## <a name="consuming-an-attached-behavior"></a>Ein angefügtes Verhalten nutzen

Die `NumericValidationBehavior` Klasse genutzt werden kann, durch Hinzufügen der `AttachBehavior` angefügten Eigenschaft, um eine [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) zu steuern, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Die Entsprechung [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) in c# ist im folgenden Codebeispiel gezeigt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Zur Laufzeit wird das Verhalten entsprechend die verhaltensimplementierung zur Interaktion mit dem Steuerelement reagieren. Die folgenden Screenshots veranschaulichen das angefügte Verhalten reagieren auf die Eingabe ist ungültig:

[![](attached-images/screenshots-sml.png "Beispielanwendung mit angefügten Verhalten")](attached-images/screenshots.png#lightbox "Beispielanwendung mit angefügten Verhalten")

> [!NOTE]
> Angefügte Verhalten für einen bestimmten Steuerelementtyp (oder eine übergeordnete Klasse, die für viele Steuerelemente angewendet werden kann) geschrieben werden, und sollte nur zu einem kompatiblen Steuerelement hinzugefügt werden. Unbekannte Verhalten führt versuchen, ein Verhalten auf das Steuerelement nicht kompatibel anzufügen, und richtet sich nach der verhaltensimplementierung.

### <a name="removing-an-attached-behavior-from-a-control"></a>Entfernen eines angefügten Verhaltens von einem Steuerelement

Die `NumericValidationBehavior` Klasse aus einem Steuerelement entfernt werden kann, durch Festlegen der `AttachBehavior` angefügten Eigenschaft, um `false`, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Die Entsprechung [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) in c# ist im folgenden Codebeispiel gezeigt:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Zur Laufzeit die `OnAttachBehaviorChanged` Methode ausgeführt wird, wenn der Wert des der `AttachBehavior` auf gesetzte angehängte Eigenschaft `false`. Die `OnAttachBehaviorChanged` Methode registriert den Ereignishandler für dann aufgehoben der [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) Ereignis, sicherstellen, dass das Verhalten wird nicht ausgeführt, wie der Benutzer mit dem Steuerelement interagiert.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie erstellen und Nutzen von definierten Verhalten. Angefügte Verhalten sind sofort `static` Klassen mit einer oder mehreren angefügte Eigenschaften.


## <a name="related-links"></a>Verwandte Links

- [Angefügte Verhaltensweisen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
