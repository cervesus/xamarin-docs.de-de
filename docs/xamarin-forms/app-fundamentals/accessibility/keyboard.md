---
title: Tastaturnavigation
description: Anstatt die Standardreihenfolge der Tabs zu verwenden, ist es manchmal notwendig, Ihre Benutzeroberfläche zu optimieren, indem Sie die Standardreihenfolge für Tabs mithilfe einer Kombination der Eigenschaften „TabIndex“ und „IsTapStop“ angeben.
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: 3e3331489c3b437366f4bdcc6990fe282d213a6a
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207881"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Tastaturnavigation in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)

Einige Benutzer haben möglicherweise Probleme mit der Ausführung von Anwendungen, bei denen die Verwendung passender Zugriffstasten nicht möglich ist. Durch das Festlegen einer Aktivierreihenfolge für Steuerelemente wird die Tastaturnavigation aktiviert und Anwendungsseiten werden entsprechend eingerichtet, Eingaben in einer bestimmten Abfolge zu empfangen.

Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der gleichen Reihenfolge, in der sie in XAML aufgelistet sind, oder in der sie programmgesteuert einer untergeordneten Sammlung hinzugefügt wurden. Diese Reihenfolge ist die Reihenfolge, in der die Steuerelemente mithilfe einer Tastatur navigiert werden, und oft ist diese Standardreihenfolge die beste Reihenfolge. Die Standardreihenfolge entspricht jedoch nicht immer der erwarteten Reihenfolge, wie im folgenden XAML-Codebeispiel zu sehen ist:

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

![](keyboard-images/default-tab-order.png "Default Row-based Tab Order (zeilenbasierte Standardaktivierreihenfolge)")

Die hier gezeigte Aktivierreihenfolge basiert auf Zeilen und entspricht der Reihenfolge, in der die Steuerelemente in XMAL aufgelistet sind. Aus diesem Grund wird durch Drücken der Tab-Taste zuerst durch [`Entry`](xref:Xamarin.Forms.Entry)-Instanzen für „Vorname“ navigiert, anschließend durch `Entry`-Instanzen für „Nachname“. Es wäre jedoch deutlich intuitiver, eine „Spalte zuerst“-Navigation zu verwenden, bei der durch Drücken der Tab-Taste durch „Vorname-Nachname“-Paare navigiert wird. Dies kann durch Angabe der Aktivierreihenfolge der Steuerelemente erreicht werden.

> [!NOTE]
> Auf der Universellen Windows-Plattform können Tastenkombinationen festgelegt werden, durch die Benutzer intuitiv und schnell navigieren können und mit denen sie mit der sichtbaren Benutzeroberfläche über eine Tastatur anstelle eines Touchpads oder einer Maus interagieren können. Weitere Informationen finden Sie unter [Festlegen von VisualElement-Zugriffstasten](~/xamarin-forms/platform/windows/visualelement-access-keys.md).

## <a name="setting-the-tab-order"></a>Festlegen der Aktivierreihenfolge

Die `VisualElement.TabIndex`-Eigenschaft wird verwendet, um die Reihenfolge anzugeben, in der [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Instanzen den Fokus erhalten, wenn der Benutzer durch Drücken der Tab-Taste durch Steuerelemente navigiert wird. Der Standardwert der Eigenschaft ist 0. Er kann auf jeden beliebigen `int`-Wert festgelegt werden.

Die folgenden Regeln gelten, wenn die Standardaktivierreihenfolge verwendet oder die `TabIndex`-Eigenschaft festgelegt wird:

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Instanzen mit einem `TabIndex` gleich 0 werden, basierend auf ihrer Deklarationsreihenfolge in XMAL oder untergeordneten Sammlungen, der Aktivierreihenfolge hinzugefügt.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Instanzen mit einem `TabIndex` größer 0 werden, basierend auf ihrem `TabIndex`-Wert, der Aktivierreihenfolge hinzugefügt.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Instanzen mit einem `TabIndex` kleiner 0 werden der Aktivierreihenfolge hinzugefügt und werden vor einem NULL-Wert angezeigt.
 - Konflikte bei einem `TabIndex` werden durch Deklaration der Reihenfolge aufgelöst.

Nach dem Definieren einer Aktivierreihenfolge, wird durch das Drücken der Tab-Taste eine Fokus-Schleife aktiviert, die Steuerelemente in aufsteigender `TabIndex`-Reihenfolge durchläuft und von vorne beginnt, sobald das letzte Steuerelement erreicht ist.

Das folgende XAML-Beispiel zeigt die `TabIndex`-Eigenschaft, die auf Eingabesteuerelemente festgelegt ist, um die „Spalte zuerst“-Navigation zu aktivieren:

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

Die Aktivierreihenfolge hier ist spaltenbasiert. Deshalb wird durch Drücken der Tab-Taste durch „Vorname-Nachname“-[`Entry`](xref:Xamarin.Forms.Entry)-Paare navigiert.

## <a name="excluding-controls-from-the-tab-order"></a>Ausschließen von Steuerelementen aus der Aktivierreihenfolge

Zusätzlich zum Festlegen der Aktivierreihenfolge von Steuerelementen kann es notwendig sein, Steuerelemente aus der Aktivierreihenfolge auszuschließen. Ein Weg, dies zu erreichen, ist das Festlegen der [`IsEnabled`](xref:Xamarin.Forms.VisualElement)-Eigenschaft der Steuerelemente auf `false`, da deaktivierte Steuerelemente aus der Aktivierreihenfolge ausgeschlossen werden.

Trotzdem kann es notwendig sein, Steuerelemente aus der Aktivierreihenfolge auszuschließen, obwohl sie nicht deaktiviert sind. Dies kann mit der `VisualElement.IsTapStop`-Eigenschaft erreicht werden, die angibt, ob ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) in der Tabstoppnavigation eingeschlossen ist. Der Standardwert ist `true`, und wenn er `false` ist, wird das Steuerelement von der Infrastruktur der Tabstoppnavigation ignoriert, unabhängig davon, ob ein `TabIndex` festgelegt ist.

## <a name="supported-controls"></a>Unterstützte Steuerelemente

Die `TabIndex`- und `IsTapStop`-Eigenschaften werden bei den folgenden Steuerelementen unterstützt, welche Tastatureingaben auf einer oder mehreren Plattformen akzeptieren:

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
> Jedes dieser Steuerelemente kann möglicherweise nicht auf jeder Plattform Fokus durch Drücken der Tab-Taste erhalten.

## <a name="related-links"></a>Verwandte Links

- [Accessibility (Barrierefreiheit (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
