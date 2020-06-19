---
title: 'Xamarin.Forms: Visual State-Manager'
description: Verwenden Sie den Visual State Manager, um Änderungen an XAML-Elementen auf der Grundlage von visuellen Zuständen vorzunehmen, die aus Code festgelegt wurden
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3dda730446ec2b4268f42ee5af853400b33565d9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946227"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin.Forms: Visual State-Manager

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Verwenden Sie den Visual State Manager, um Änderungen an XAML-Elementen auf der Grundlage von visuellen Zuständen vorzunehmen, die aus Code festgelegt wurden_

Der Visual State Manager (VSM) stellt eine strukturierte Möglichkeit dar, visuelle Änderungen an der Benutzeroberfläche aus dem Code vorzunehmen. In den meisten Fällen wird die Benutzeroberfläche der Anwendung in XAML definiert, und dieses XAML enthält Markup, das beschreibt, wie sich der visuelle Zustands-Manager auf die visuellen Elemente der Benutzeroberfläche auswirkt.

Das VSM führt das Konzept der _visuellen Zustände_ein. Eine Ansicht, wie z. b., Xamarin.Forms `Button` kann je nach dem zugrunde liegenden Zustand &mdash; , ob Sie deaktiviert oder gedrückt ist oder den Eingabefokus besitzt, verschiedene visuelle Darstellungen aufweisen. Dies sind die Zustände der Schaltfläche.

Visuelle Zustände werden in _visuellen Zustands Gruppen_gesammelt. Alle visuellen Zustände innerhalb einer visuellen Zustands Gruppe schließen sich gegenseitig aus. Visuelle Zustände und visuelle Zustands Gruppen werden durch einfache Text Zeichenfolgen identifiziert.

Der Xamarin.Forms Visual State Manager definiert eine visuelle Zustands Gruppe mit dem Namen "CommonStates" mit den folgenden visuellen Zuständen:

- "Normal"
- Deaktiviert
- Gestellt
- Gewählte

Diese visuelle Zustands Gruppe wird für alle Klassen unterstützt, die von abgeleitet [`VisualElement`](xref:Xamarin.Forms.VisualElement) werden. Dies ist die Basisklasse für [`View`](xref:Xamarin.Forms.View) und [`Page`](xref:Xamarin.Forms.Page) .

Sie können auch eigene visuelle Zustands Gruppen und visuelle Zustände definieren, wie in diesem Artikel veranschaulicht wird.

> [!NOTE]
> Xamarin.FormsEntwickler, die mit [Triggern](~/xamarin-forms/app-fundamentals/triggers.md) vertraut sind, wissen, dass Trigger auch Änderungen an visuellen Elementen in der Benutzeroberfläche basierend auf Änderungen in den Eigenschaften einer Ansicht oder dem Auslösen von Ereignissen vornehmen können. Die Verwendung von Triggern für verschiedene Kombinationen dieser Änderungen kann jedoch etwas verwirrend werden. In der Vergangenheit wurde der Visual State Manager in Windows XAML-basierten Umgebungen eingeführt, um die Verwirrung zu verringern, die sich aus Kombinationen von visuellen Zuständen ergibt. Beim VSM schließen sich die visuellen Zustände innerhalb einer visuellen Zustands Gruppe immer gegenseitig aus. Zu jedem Zeitpunkt ist nur ein Status in jeder Gruppe der aktuelle Zustand.

## <a name="common-states"></a>Allgemeine Zustände

Der Visual State Manager ermöglicht das Einschließen von Markup in die XAML-Datei, die die visuelle Darstellung einer Ansicht ändern kann, wenn die Ansicht Normal oder deaktiviert ist oder den Eingabefokus besitzt. Diese werden als _allgemeine Zustände_bezeichnet.

Angenommen, Sie haben eine `Entry` Ansicht auf der Seite, und Sie möchten, dass die visuelle Darstellung von auf `Entry` folgende Weise geändert wird:

- Der `Entry` sollte einen rosa Hintergrund haben, wenn `Entry` deaktiviert ist.
- Der `Entry` sollte in der Regel über einen Kalk Hintergrund verfügen.
- Der Wert `Entry` sollte auf eine doppelte Größe erweitert werden, wenn er den Eingabefokus besitzt.

Sie können das VSM-Markup an eine einzelne Ansicht anfügen, oder Sie können es in einem Stil definieren, wenn es für mehrere Ansichten gilt. In den nächsten beiden Abschnitten werden diese Vorgehensweisen beschrieben.

### <a name="vsm-markup-on-a-view"></a>VSM-Markup für eine Ansicht

Um das VSM-Markup an eine Ansicht anzufügen, müssen Sie `Entry` zunächst die `Entry` in Start-und Endtags aufteilen:

```xaml
<Entry FontSize="18">

</Entry>
```

Ihm wird eine explizite Schriftgröße zugewiesen, da einer der Zustände die- `FontSize` Eigenschaft verwendet, um die Größe des Texts in der zu verdoppeln `Entry` .

Fügen Sie als nächstes `VisualStateManager.VisualStateGroups` Tags zwischen diesen Tags ein:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)ist eine angefügte bindbare Eigenschaft, die von der-Klasse definiert wird [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) . (Weitere Informationen zu Eigenschaften angehängter Bindungen finden Sie im Artikel [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).) Auf diese Weise wird die- `VisualStateGroups` Eigenschaft an das-Objekt angefügt `Entry` .

Die- `VisualStateGroups` Eigenschaft ist vom Typ [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) , bei dem es sich um eine Auflistung von-Objekten handelt [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) . `VisualStateManager.VisualStateGroups`Fügen Sie innerhalb der Tags ein Tagpaar `VisualStateGroup` für jede Gruppe von visuellen Zuständen ein, die Sie einschließen möchten:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Beachten Sie, dass das- `VisualStateGroup` Tag über ein Attribut verfügt, `x:Name` das den Namen der Gruppe angibt. Die- `VisualStateGroup` Klasse definiert eine `Name` Eigenschaft, die Sie stattdessen verwenden können:

```xaml
<VisualStateGroup Name="CommonStates">
```

Sie können entweder `x:Name` oder, `Name` aber nicht beides im selben Element verwenden.

Die- `VisualStateGroup` Klasse definiert eine Eigenschaft [`States`](xref:Xamarin.Forms.VisualStateGroup.States) mit dem Namen, die eine Auflistung von- [`VisualState`](xref:Xamarin.Forms.VisualState) Objekten ist. `States`ist die _Content-Eigenschaft_ von, `VisualStateGroups` sodass Sie die `VisualState` Tags direkt zwischen den `VisualStateGroup` Tags einschließen können. (Inhalts Eigenschaften werden im Artikel [grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)erläutert.)

Der nächste Schritt besteht darin, ein Tagpaar für jeden visuellen Zustand in dieser Gruppe einzuschließen. Diese können auch mithilfe von oder identifiziert werden `x:Name` `Name` :

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState`definiert eine Eigenschaft [`Setters`](xref:Xamarin.Forms.VisualState.Setters) mit dem Namen, die eine Auflistung von- [`Setter`](xref:Xamarin.Forms.Setter) Objekten ist. Dabei handelt es sich um dieselben `Setter` Objekte, die Sie in einem- [`Style`](xref:Xamarin.Forms.Style) Objekt verwenden.

`Setters`ist _nicht_ die Content-Eigenschaft von. `VisualState` daher ist es erforderlich, Eigenschaften Element Tags für die-Eigenschaft einzuschließen `Setters` :

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Sie können jetzt ein oder mehrere `Setter` Objekte zwischen jedem Tagpaar einfügen `Setters` . Dies sind die `Setter` Objekte, die die zuvor beschriebenen visuellen Zustände definieren:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Jedes `Setter` Tag gibt den Wert einer bestimmten Eigenschaft an, wenn dieser Zustand aktuell ist. Jede Eigenschaft, auf die von einem- `Setter` Objekt verwiesen wird, muss durch eine bindbare Eigenschaft unterstützt werden.

Ein ähnliches Markup ist die Grundlage für die **VSM auf der Ansichts** Seite im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispielprogramm. Die Seite enthält drei `Entry` Ansichten, aber nur für den zweiten ist das VSM-Markup angefügt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Beachten Sie, dass der zweite `Entry` auch über einen `DataTrigger` als Teil der-Auflistung verfügt `Trigger` . Dadurch wird das `Entry` deaktiviert, bis etwas in das dritte eingegeben wird `Entry` . Hier ist die Seite beim Start, die unter IOS, Android und der universelle Windows-Plattform (UWP) ausgeführt wird:

[![VSM in Ansicht: deaktiviert](vsm-images/VsmOnViewDisabled.png "VSM in Ansicht-deaktiviert")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Der aktuelle visuelle Zustand ist "deaktiviert", sodass der Hintergrund der zweiten `Entry` auf den IOS-und Android-Bildschirmen Rosa ist. Die UWP-Implementierung von `Entry` lässt das Festlegen der Hintergrundfarbe nicht zu, wenn `Entry` deaktiviert ist.

Wenn Sie Text in das dritte eingeben `Entry` , wechselt der zweite `Entry` in den "normalen" Zustand, und der Hintergrund ist jetzt "Kalk":

[![VSM in Ansicht: normal](vsm-images/VsmOnViewNormal.png "VSM in Ansicht (normal)")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Wenn Sie die zweite berühren `Entry` , erhält Sie den Eingabefokus. Es wechselt in den Zustand "fokussiert" und wird um die doppelte Höhe erweitert:

[![VSM in Ansicht: Fokus](vsm-images/VsmOnViewFocused.png "VSM im Fokus")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Beachten Sie, dass der den `Entry` Kalk Hintergrund nicht beibehält, wenn er den Eingabefokus erhält. Wenn der Visual State-Manager zwischen den visuellen Zuständen wechselt, werden die Eigenschaften, die vom vorherigen Zustand festgelegt wurden, nicht festgelegt. Beachten Sie, dass sich die visuellen Zustände gegenseitig ausschließen. Der Status "Normal" bedeutet nicht nur, dass die `Entry` aktiviert ist. Dies bedeutet, dass der `Entry` aktiviert ist und keinen Eingabefokus hat.

Wenn das-Element `Entry` einen Kalk-Hintergrund im Zustand "fokussiert" aufweisen soll, fügen Sie einen weiteren `Setter` zu diesem visuellen Zustand hinzu:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Damit diese `Setter` Objekte ordnungsgemäß funktionieren, muss ein- `VisualStateGroup` `VisualState` Objekt für alle Zustände in dieser Gruppe enthalten sein. Wenn es einen visuellen Zustand gibt, der keine Objekte enthält `Setter` , schließen Sie ihn trotzdem als leeres Tag ein:

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>Visual State-Manager-Markup in einem Stil

Häufig ist es erforderlich, dass Sie das gleiche Visual State Manager-Markup für zwei oder mehr Ansichten verwenden. In diesem Fall möchten Sie das Markup in eine `Style` Definition einfügen.

Im folgenden finden Sie die vorhandenen impliziten `Style` für die `Entry` Elemente auf der Seite **VSM auf der Ansichts** Seite:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`Setter`Tags für die `VisualStateManager.VisualStateGroups` angefügte bindbare Eigenschaft hinzufügen:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

Die Content-Eigenschaft für `Setter` ist `Value` , sodass der Wert der `Value` Eigenschaft direkt innerhalb dieser Tags angegeben werden kann. Diese Eigenschaft hat den folgenden Typ `VisualStateGroupList` :

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style>
```

Innerhalb dieser Tags können Sie eines der folgenden `VisualStateGroup` Objekte einschließen:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Der Rest des VSM-Markups ist das gleiche wie zuvor.

Dies ist das **VSM auf** der Seite "Style", das das komplette VSM-Markup anzeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

Nun reagieren alle `Entry` Ansichten auf dieser Seite auf die gleiche Weise wie Ihre visuellen Zustände. Beachten Sie auch, dass der "fokussierte" Zustand jetzt eine Sekunde enthält `Setter` , die jeweils `Entry` einen Kalk Hintergrund enthält, wenn Sie den Eingabefokus besitzt:

[![VSM im Stil](vsm-images/VsmInStyle.png "VSM im Stil")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Visuelle Zustände inXamarin.Forms

In der folgenden Tabelle sind die visuellen Zustände aufgeführt, die in definiert sind Xamarin.Forms :

| Klasse | Zustände | Weitere Informationen |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [Visuelle Schaltflächen für Flächen](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [Kontrollkästchen visuelle Zustände](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [Visuelle Zustände von carouselview](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [Visuelle Status von ImageButton](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [Visuelle Zustände von RadioButton](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `Switch` | `On`, `Off` | [Visuelle Zustände wechseln](~/xamarin-forms/user-interface/switch.md#switch-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [Allgemeine Zustände](#common-states) |

Auf jeden dieser Zustände kann über die visuelle Zustands Gruppe mit dem Namen zugegriffen werden `CommonStates` .

Außerdem implementiert den- `CollectionView` `Selected` Zustand. Weitere Informationen finden Sie unter [Ändern der Farbe des ausgewählten Elements](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color).

## <a name="set-state-on-multiple-elements"></a>Festlegen des Zustands für mehrere Elemente

In den vorherigen Beispielen wurden visuelle Zustände an einzelne Elemente angefügt und verarbeitet. Es ist jedoch auch möglich, visuelle Zustände zu erstellen, die an ein einzelnes Element angefügt sind, aber Eigenschaften für andere Elemente innerhalb desselben Bereichs festlegen. Dadurch wird vermieden, dass visuelle Zustände für jedes Element wiederholt werden müssen, auf dem die Zustände arbeiten.

Der- [`Setter`](xref:Xamarin.Forms.Setter) Typ verfügt über eine- `TargetName` Eigenschaft vom Typ `string` , die das Ziel Element darstellt, das `Setter` für einen visuellen Zustand bearbeitet wird. Wenn die- `TargetName` Eigenschaft definiert ist, `Setter` legt den `Property` des in definierten Elements `TargetName` auf fest `Value` :

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

In diesem Beispiel `Label` `label` wird die-Eigenschaft eines mit dem Namen `TextColor` auf festgelegt `Red` . Wenn Sie die- `TargetName` Eigenschaft festlegen, müssen Sie den vollständigen Pfad zur-Eigenschaft in angeben `Property` . Daher wird zum Festlegen der- `TextColor` Eigenschaft für ein-Objekt `Label` `Property` als angegeben `Label.TextColor` .

> [!NOTE]
> Jede Eigenschaft, auf die von einem- `Setter` Objekt verwiesen wird, muss durch eine bindbare Eigenschaft unterstützt werden.

Die Seite **VSM mit Setter TargetName** im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispiel zeigt, wie der Status für mehrere Elemente aus einer einzelnen visuellen Zustands Gruppe festgelegt wird. Die XAML-Datei besteht aus einer, die `StackLayout` ein `Label` -Element, ein `Entry` -Element und ein-Element enthält `Button` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Das VSM-Markup ist an das angefügt `StackLayout` . Es gibt zwei gegenseitig ausschließende Zustände mit dem Namen "Normal" und "gedrückt", wobei jeder Zustand `VisualState` Tags enthält.

Der Status "Normal" ist aktiv, wenn das `Button` nicht gedrückt wird und eine Antwort auf die Frage eingegeben werden kann:

[![VSM Setter TargetName: normaler Status](vsm-images/VsmSetterTargetNameNormal.png "VSM Setter TargetName-normal")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

Der Zustand "gedrückt" wird aktiv, wenn `Button` gedrückt wird:

[![VSM Setter TargetName: gedrückter Zustand](vsm-images/VsmSetterTargetNamePressed.png "VSM-Setter TargetName-gedrückt")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

Das "gedrückt" `VisualState` gibt an, dass die- `Button` Eigenschaft beim Drücken `Scale` von auf den Standardwert von 1 bis 0,8 geändert wird. Außerdem wird die- `Entry` Eigenschaft des namens `entry` `Text` auf Paris festgelegt. Das Ergebnis ist daher, dass bei `Button` gedrückter-Taste ein etwas kleiner wird, und das `Entry` zeigt Paris an. Wenn der `Button` freigegeben wird, wird er auf den Standardwert 1 umgestellt, und der `Entry` zeigt alle zuvor eingegebenen Text an.

> [!IMPORTANT]
> Eigenschafts Pfade werden derzeit nicht in-Elementen unterstützt `Setter` , die die- `TargetName` Eigenschaft angeben.

## <a name="define-your-own-visual-states"></a>Definieren Ihrer eigenen visuellen Zustände

Jede Klasse, die von abgeleitet wird, `VisualElement` unterstützt die allgemeinen Zustände "Normal", "fokussiert" und "deaktiviert". Außerdem `CollectionView` unterstützt die-Klasse den Status "ausgewählt". Intern erkennt die [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) -Klasse, wann Sie aktiviert oder deaktiviert wird, oder konzentriert bzw. ohne Fokus ist, und ruft die statische [ `VisualStateManager.GoToState` ] (Xref:) auf Xamarin.Forms . VisualStateManager. godestate ( Xamarin.Forms . Visualelement, System. String))-Methode:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Dies ist der einzige Code des visuellen Zustands-Managers, den Sie in der- `VisualElement` Klasse finden. Da `GoToState` für jedes-Objekt basierend auf jeder Klasse aufgerufen wird, die von abgeleitet wird `VisualElement` , können Sie den visuellen Zustands-Manager mit jedem-Objekt verwenden, `VisualElement` um auf diese Änderungen zu reagieren.

Interessanterweise wird in auf den Namen der visuellen Zustands Gruppe "CommonStates" nicht explizit verwiesen `VisualElement` . Der Gruppenname ist nicht Teil der API für den Visual State Manager. Innerhalb eines der beiden bisher gezeigten Beispiel Programme können Sie den Namen der Gruppe von "CommonStates" in beliebige andere ändern, und das Programm funktioniert weiterhin. Der Gruppenname ist lediglich eine allgemeine Beschreibung der Zustände in dieser Gruppe. Es ist implizit zu verstehen, dass die visuellen Zustände in jeder Gruppe sich gegenseitig ausschließen: ein einziger Status und immer nur ein Status ist aktuell.

Wenn Sie Ihre eigenen visuellen Zustände implementieren möchten, müssen Sie den `VisualStateManager.GoToState` Code aus dem Code abrufen. In den meisten Fällen wird dieser Befehl aus der Code Behind-Datei Ihrer Page-Klasse aufgerufen.

Die Seite **VSM** -Überprüfung im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispiel zeigt, wie Sie den visuellen Zustands-Manager in Verbindung mit der Eingabevalidierung verwenden. Die XAML-Datei besteht aus einer `StackLayout` , die zwei `Label` Elemente enthält `Entry` :, und `Button` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

VSM-Markup ist an den `StackLayout` (mit dem Namen) angefügt `stackLayout` . Es gibt zwei gegenseitig ausschließende Zustände mit dem Namen "valid" und "invalid", wobei jeder Zustand `VisualState` Tags enthält.

Wenn das `Entry` keine gültige Telefonnummer enthält, ist der aktuelle Zustand "ungültig", und das `Entry` hat einen rosa Hintergrund, das zweite `Label` ist sichtbar, und das `Button` ist deaktiviert:

[![VSM-Überprüfung: Ungültiger Status](vsm-images/VsmValidationInvalid.png "VSM-Überprüfung-ungültig")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Wenn eine gültige Telefonnummer eingegeben wird, wird der aktuelle Status "gültig". Der `Entry` erhält einen Kalk Hintergrund, der zweite `Label` verschwindet, und der `Button` ist nun aktiviert:

[![VSM-Überprüfung: gültiger Status](vsm-images/VsmValidationValid.png "VSM-Validierung-gültig")](vsm-images/VsmValidationValid-Large.png#lightbox)

Die Code-Behind-Datei ist für die Behandlung des- `TextChanged` Ereignisses aus der verantwortlich `Entry` . Der Handler verwendet einen regulären Ausdruck, um zu bestimmen, ob die Eingabe Zeichenfolge gültig ist. Die-Methode in der Code Behind-Datei mit dem Namen `GoToState` Ruft die statische- `VisualStateManager.GoToState` Methode für auf `stackLayout` :

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

Beachten Sie auch, dass die- `GoToState` Methode vom-Konstruktor aufgerufen wird, um den Status zu initialisieren. Es sollte immer ein aktueller Zustand vorhanden sein. Im Code gibt es jedoch keinen Verweis auf den Namen der visuellen Zustands Gruppe, obwohl in XAML als "ValidationStates" aus Gründen der Übersichtlichkeit darauf verwiesen wird.

Beachten Sie, dass die Code Behind-Datei nur das Objekt auf der Seite berücksichtigen muss, das die visuellen Zustände definiert, und `VisualStateManager.GoToState` für dieses Objekt aufzurufen. Dies liegt daran, dass beide visuellen Zustände mehrere Objekte auf der Seite als Ziel haben.

Vielleicht Fragen Sie sich: Wenn die Code-Behind-Datei auf das Objekt auf der Seite verweisen muss, die die visuellen Zustände definiert, warum kann die Code-Behind-Datei nicht einfach direkt auf dieses und andere Objekte zugreifen? Dies könnte sicherlich der Fall sein. Der Vorteil der Verwendung des VSM besteht jedoch darin, dass Sie steuern können, wie visuelle Elemente vollständig in XAML auf einen anderen Zustand reagieren, wodurch der gesamte Benutzeroberflächen Entwurf an einem Ort beibehalten wird. Dadurch wird das Festlegen der visuellen Darstellung vermieden, indem direkt aus dem Code-Behind auf visuelle Elemente zugegriffen wird.

## <a name="visual-state-triggers"></a>Trigger für visuellen Zustand

Visuelle Zustände unterstützen Zustands Trigger, bei denen es sich um eine spezialisierte Gruppe von Triggern handelt, die die Bedingungen definieren, unter denen eine [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

Zustandstrigger werden der Sammlung [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) eines [`VisualState`](xref:Xamarin.Forms.VisualState) hinzugefügt. Diese Sammlung kann Trigger mit einem oder mehreren Zustandstriggern enthalten. Ein [`VisualState`](xref:Xamarin.Forms.VisualState) wird angewendet, wenn alle Zustandstrigger in der Sammlung aktiv sind.

Bei Verwendung von Zustandstriggern zur Steuerung visueller Zustände befolgt Xamarin.Forms die folgenden Prioritätsregeln, um zu bestimmen, welcher Trigger (und welches entsprechende [`VisualState`](xref:Xamarin.Forms.VisualState)-Element) aktiv ist:

1. Alle von [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleiteten Trigger.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger), der aktiviert wird, da die Bedingung [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) erfüllt ist.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger), der aktiviert wird, da die Bedingung [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) erfüllt ist.

Wenn mehrere Trigger gleichzeitig aktiv sind (z. B. zwei benutzerdefinierte Trigger), hat der erste im Markup deklarierte Trigger Vorrang.

Weitere Informationen zu Zustandstriggern finden Sie unter [Zustandstrigger](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>Verwenden des visuellen Zustands-Managers für adaptives Layout

Eine Xamarin.Forms auf einem Telefon ausgeführte Anwendung kann normalerweise in einem hoch-oder Querformat angezeigt werden, und ein Xamarin.Forms Programm, das auf dem Desktop ausgeführt wird, kann geändert werden, um viele verschiedene Größen und Seitenverhältnisse zu übernehmen. Eine gut entworfene Anwendung zeigt ihren Inhalt möglicherweise für diese verschiedenen Seiten-oder Fenster Formfaktoren anders an.

Diese Technik wird manchmal als _Adaptives Layout_bezeichnet. Da das adaptive Layout lediglich die visuellen Elemente eines Programms umfasst, ist es eine ideale Anwendung des visuellen Zustands-Managers.

Ein einfaches Beispiel ist eine Anwendung, die eine kleine Auflistung von Schaltflächen anzeigt, die sich auf den Inhalt der Anwendung auswirken. Im Hochformat können diese Schaltflächen in einer horizontalen Zeile oben auf der Seite angezeigt werden:

[![Adaptive VSM-Layout: Hochformat](vsm-images/VsmAdaptiveLayoutPortrait.png "Adaptive VSM-Layout-Hochformat")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Im Querformat kann das Array von Schaltflächen auf eine Seite verschoben und in einer Spalte angezeigt werden:

[![Adaptive VSM-Layout: Querformat](vsm-images/VsmAdaptiveLayoutLandscape.png "Adaptive VSM-Layout-Querformat")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Von oben nach unten wird das Programm auf den universelle Windows-Plattform, Android und IOS ausgeführt.

Die Seite für das **VSM Adaptive Layout** im [vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) -Beispiel definiert eine Gruppe mit dem Namen "orientationstates" mit zwei visuellen Zuständen namens "Hochformat" und "Querformat". (Ein komplexerer Ansatz kann auf verschiedenen Seiten-oder fensterbreiten basieren.)

VSM-Markup tritt an vier Stellen in der XAML-Datei auf. Der `StackLayout` benannte `mainStack` enthält sowohl das Menü als auch den Inhalt, bei dem es sich um ein `Image` Element handelt. Dies `StackLayout` sollte eine vertikale Ausrichtung im Hochformat und eine horizontale Ausrichtung im Querformat aufweisen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">
                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">
                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">
                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>
                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

Der innere `ScrollView` `menuScroll` und der `StackLayout` benannte `menuStack` implementieren das Menü der Schaltflächen. Die Ausrichtung dieser Layouts ist gegen `mainStack` . Das Menü sollte im Hochformat horizontal und vertikal im Querformat angezeigt werden.

Der vierte Abschnitt von VSM-Markup weist einen impliziten Stil für die Schaltflächen auf. Dieses Markup legt `VerticalOptions` `HorizontalOptions` `Margin` die Eigenschaften, und für das hoch-und Querformat fest.

Mit der Code-Behind-Datei `BindingContext` wird die-Eigenschaft von auf die Implementierung der Befehlszeile festgelegt. `menuStack` `Button` Außerdem wird ein Handler an das- `SizeChanged` Ereignis der Seite angefügt:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

Der `SizeChanged` Handler ruft `VisualStateManager.GoToState` für die beiden `StackLayout` Elemente und auf `ScrollView` und durchläuft dann die untergeordneten Elemente von, `menuStack` um `VisualStateManager.GoToState` für die Elemente aufzurufen `Button` .

Es mag so aussehen, als ob die Code-Behind-Datei Richtungsänderungen direkt behandeln kann, indem Sie die Eigenschaften der Elemente in der XAML-Datei festlegen, aber der visuelle Zustands-Manager ist definitiv ein strukturierteres Konzept. Alle visuellen Elemente werden in der XAML-Datei gespeichert, wo Sie leichter zu untersuchen, zu warten und zu ändern sind.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager mit xamarin. University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms3,0 Visual State Manager-Video**

## <a name="related-links"></a>Verwandte Links

- [Vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [Zustandstrigger](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
