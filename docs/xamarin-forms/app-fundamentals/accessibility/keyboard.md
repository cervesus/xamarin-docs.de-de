---
title: 'title: "Barrierefreiheit der Tastatur" description: "Anstatt die Standardreihenfolge der Registerkarten zu verwenden, ist es manchmal notwendig, die Barrierefreiheit Ihrer Benutzeroberfläche zu optimieren, indem Sie die Reihenfolge der Registerkarten mit einer Kombination aus den Eigenschaften „TabIndex“ und „IsTabStop“ angeben."'
description: 'ms.prod: xamarin ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 05/09/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1590d0a4f9716541f18bc4f50a2c480c5e4478a
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84129778"
---
# <a name="keyboard-accessibility-in-xamarinforms"></a>Barrierefreiheit der Tastatur in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

Benutzer, die Sprachausgaben verwenden oder Mobilitätseinschränkungen haben, können bei der Nutzung von Anwendungen Probleme haben, bei denen die Verwendung passender Zugriffstasten nicht möglich ist. Für Xamarin.Forms-Anwendungen kann eine erwartete Aktivierreihenfolge angegeben werden, um die Benutzerfreundlichkeit und Barrierefreiheit zu optimieren. Durch das Festlegen einer Aktivierreihenfolge für Steuerelemente wird die Tastaturnavigation aktiviert, und Anwendungsseiten werden entsprechend eingerichtet, Eingaben in einer bestimmten Abfolge zu empfangen. Zudem kann die Sprachausgabe dem Benutzer fokussierbare Elemente vorlesen.

Standardmäßig entspricht die Aktivierreihenfolge von Steuerelementen der gleichen Reihenfolge, in der sie in XAML aufgelistet sind, oder in der sie programmgesteuert einer untergeordneten Sammlung hinzugefügt wurden. In dieser Reihenfolge wird mit der Tastatur durch Steuerelemente navigiert, und diese werden von der Sprachausgabe gelesen. Diese Standardreihenfolge ist meist die beste Lösung. Die Standardreihenfolge entspricht jedoch nicht immer der erwarteten Reihenfolge, wie im folgenden XAML-Codebeispiel zu sehen ist:

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

![](keyboard-images/default-tab-order.png "Default Row-based Tab Order")

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

![](keyboard-images/correct-tab-order.png "Column-based Tab Order")

Die Aktivierreihenfolge hier ist spaltenbasiert. Deshalb wird durch Drücken der Tab-Taste durch „Vorname-Nachname“-[`Entry`](xref:Xamarin.Forms.Entry)-Paare navigiert.

> [!IMPORTANT]
> Sprachausgaben unter iOS und Android berücksichtigen das `TabIndex`-Attribut einer [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Klasse, wenn die Elemente auf dem Bildschirm gelesen werden, auf die zugegriffen werden kann.

## <a name="excluding-controls-from-the-tab-order"></a>Ausschließen von Steuerelementen aus der Aktivierreihenfolge

Zusätzlich zum Festlegen der Aktivierreihenfolge von Steuerelementen kann es notwendig sein, Steuerelemente aus der Aktivierreihenfolge auszuschließen. Ein Weg, dies zu erreichen, ist das Festlegen der [`IsEnabled`](xref:Xamarin.Forms.VisualElement)-Eigenschaft der Steuerelemente auf `false`, da deaktivierte Steuerelemente aus der Aktivierreihenfolge ausgeschlossen werden.

Trotzdem kann es notwendig sein, Steuerelemente aus der Aktivierreihenfolge auszuschließen, obwohl sie nicht deaktiviert sind. Dies kann mit der `VisualElement.IsTabStop`-Eigenschaft erreicht werden, die angibt, ob ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) in der Tabstoppnavigation eingeschlossen ist. Der Standardwert ist `true`, und wenn er `false` ist, wird das Steuerelement von der Infrastruktur der Tabstoppnavigation ignoriert, unabhängig davon, ob ein `TabIndex` festgelegt ist.

## <a name="supported-controls"></a>Unterstützte Steuerelemente

Die `TabIndex`- und `IsTabStop`-Eigenschaften werden bei den folgenden Steuerelementen unterstützt, welche Tastatureingaben auf einer oder mehreren Plattformen akzeptieren:

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

- [Accessibility (Barrierefreiheit (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)
