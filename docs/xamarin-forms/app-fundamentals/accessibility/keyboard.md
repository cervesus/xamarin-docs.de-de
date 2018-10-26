---
title: Tastaturnavigation
description: Statt die Standardabfolge der Registerkarte ", ist es manchmal notwendig, um die Benutzeroberfläche durch Angeben der Reihenfolge der Registerkarten mit einer Kombination der Eigenschaften TabIndex und IsTapStop zu optimieren.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: f703dff56d2947c35a9bc76e0eb909bfe9023bac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131554"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Tastaturnavigation in Xamarin.Forms

Einige Benutzer haben Probleme bei der Verwendung von Anwendungen, die entsprechende Tastenkombination Zugriff nicht bereitstellen. Geben Sie eine Aktivierreihenfolge von Steuerelementen Tastaturnavigation und bereitet die Anwendungsseiten zum Empfangen von Eingaben in einer bestimmten Reihenfolge vor.

Standardmäßig wird die Aktivierreihenfolge von Steuerelementen der gleichen Reihenfolge, in der sie aufgelistet in XAML, oder eine untergeordnete Auflistung programmgesteuert hinzugefügt. Diese Reihenfolge ist die Reihenfolge, in der die Steuerelemente über mit einer Tastatur navigiert werden soll, und diese Standardreihenfolge ist häufig die optimale Reihenfolge. Allerdings ist die Standardreihenfolge nicht immer identisch mit der erwarteten Reihenfolge wie in den folgenden XAML-Codebeispiel gezeigt:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

Der folgende Screenshot zeigt die Standardaktivierreihenfolge für dieses Codebeispiel:

![](keyboard-images/default-tab-order.png "Standardreihenfolge der Zeile auf der Registerkarte")

Hier die Aktivierreihenfolge basiert auf der Zeile, und ist die Reihenfolge, in der die Steuerelemente in der XAML aufgeführt sind. Aus diesem Grund Vorname navigiert durch Drücken der Tab-Taste [ `Entry` ](xref:Xamarin.Forms.Entry) -Instanzen, gefolgt von Nachname `Entry` Instanzen. Jedoch wäre eine intuitivere Benutzeroberfläche mit einer Registerkarte "Spalte zuerst"-Navigation, sodass Vorname Nachname Paare durch Drücken der Tab-Taste navigiert werden. Dies kann durch Angabe der Aktivierreihenfolge der Eingabesteuerelemente erreicht werden.

> [!NOTE]
> Klicken Sie auf der universellen Windows-Plattform können Tastenkombinationen in Visual Studio definiert werden, die eine intuitive Methode für Benutzer schnell navigieren und interagieren mit der Benutzeroberfläche der Anwendung sichtbar über eine Tastatur anstelle von per Touch oder eine Maus bereitstellen. Weitere Informationen finden Sie unter [Einstellung VisualElement Zugriffsschlüssel](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys).

## <a name="setting-the-tab-order"></a>Festlegen der Aktivierreihenfolge

Die `VisualElement.TabIndex` Eigenschaft wird zum Angeben der Reihenfolge, in der [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Instanzen den Fokus erhalten, wenn der Benutzer durch Steuerelemente navigiert wird, durch Drücken der Tab-Taste. Der Standardwert der Eigenschaft ist 0, und es kann festgelegt werden, um eine `int` Wert.

Die folgenden Regeln gelten, wenn die Standardaktivierreihenfolge verwenden oder die Einstellung der `TabIndex` Eigenschaft:

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Instanzen mit einem `TabIndex` gleich 0 werden mit der Tab-Reihenfolge, basierend auf deren Reihenfolge der Deklaration in XAML oder untergeordnete Sammlungen hinzugefügt.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Instanzen mit einem `TabIndex` größer als 0, bis der Tab-Reihenfolge hinzugefügt werden basierend auf ihren `TabIndex` Wert.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Instanzen mit einem `TabIndex` kleiner als 0 werden die Tab-Reihenfolge hinzugefügt, und vor einer NULL-Wert angezeigt werden.
 - Konflikt auf ein `TabIndex` werden aufgelöst, indem die Reihenfolge der Deklaration.

Nach dem Definieren einer Aktivierreihenfolge, drücken die Tab-Taste den Fokus durch Steuerelemente in aufsteigender Zyklus wird `TabIndex` Reihenfolge, um auf den Anfang umschließen, sobald das letzte Steuerelement erreicht ist.

Das folgende XAML-Beispiel zeigt die `TabIndex` -Eigenschaft auf Benutzereingabe-Steuerelemente festgelegt werden, um die Registerkarte "Spalte zuerst"-Navigation ermöglichen:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

Der folgende Screenshot zeigt die Aktivierreihenfolge für dieses Codebeispiel:

![](keyboard-images/correct-tab-order.png "Spaltenbasierte Aktivierreihenfolge")

Die Aktivierreihenfolge hier ist die Spalte-basiert. Aus diesem Grund Nachname-Vorname navigiert durch Drücken der Tab-Taste [ `Entry` ](xref:Xamarin.Forms.Entry) Paare.

## <a name="excluding-controls-from-the-tab-order"></a>Ausschließen von Steuerelementen aus der Aktivierreihenfolge

Zusätzlich zum Festlegen der Aktivierreihenfolge von Steuerelementen, kann es zum Ausschließen der Aktivierreihenfolge der Steuerelemente notwendig sein. Eine Möglichkeit zu erreichen ist durch Festlegen der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement) Eigenschaft von Steuerelementen `false`, da deaktivierten Steuerelemente aus der Aktivierreihenfolge ausgeschlossen werden.

Allerdings ist es möglicherweise notwendig, um Steuerelemente aus der Aktivierreihenfolge ausschließen, selbst wenn sie deaktiviert sind nicht. Dies kann erreicht werden, mit der `VisualElement.IsTapStop` Eigenschaft, die angibt, ob eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) in die tabstoppnavigation eingeschlossen ist. Der Standardwert ist `true`, und wenn sein Wert ist `false` des Steuerelements wird durch die tabstoppnavigation Infrastruktur, unabhängig davon ignoriert, wenn eine `TabIndex` festgelegt ist.

## <a name="supported-controls"></a>Unterstützte Steuerelemente

Die `TabIndex` und `IsTapStop` Eigenschaften werden unterstützt, auf die folgenden Steuerelemente, die Tastatureingaben auf eine oder mehrere Plattformen zu akzeptieren:

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> Jedes dieser Steuerelemente ist nicht die Registerkarte mit Fokus auf jeder Plattform.

## <a name="related-links"></a>Verwandte Links

- [Barrierefreiheit (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
