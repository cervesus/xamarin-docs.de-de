---
title: Xamarin. Forms Visual State Manager
description: Verwenden der Visual State-Manager, um XAML-Elemente, die basierend auf visuelle Zustände, die im Code festgelegt zu ändern.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: 086adee4dc6b921abe92f6486186023a3125695c
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636050"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin. Forms Visual State Manager

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Verwenden Sie den Visual State Manager, um Änderungen an XAML-Elementen auf der Grundlage von visuellen Zuständen vorzunehmen, die aus Code festgelegt wurden_

Der Visual State Manager (VSM) stellt eine strukturierte Möglichkeit dar, visuelle Änderungen an der Benutzeroberfläche aus dem Code vorzunehmen. In den meisten Fällen ist die Benutzeroberfläche der Anwendung in XAML, definiert und dieses XAML enthält Markup, beschreibt, wie der Visual State Manager wirkt sich die visuellen Elemente der Benutzeroberfläche auf.

Das VSM führt das Konzept der _visuellen Zustände_ein. Eine xamarin. Forms-Ansicht, z. b. eine `Button`, kann je nach dem zugrunde liegenden Zustand &mdash;, ob Sie deaktiviert oder gedrückt ist oder den Eingabefokus besitzt. Hierbei handelt es sich um die Schaltfläche "-Status.

Visuelle Zustände werden in _visuellen Zustands Gruppen_gesammelt. Alle visuellen Statusoptionen in eine visuelle Zustandsgruppe schließen sich gegenseitig aus. Sowohl visuelle Statusoptionen und Statusgruppen visual werden durch einfache Textzeichenfolgen identifiziert.

Der Visual State-Manager von xamarin. Forms definiert eine visuelle Zustands Gruppe mit dem Namen "CommonStates" mit den folgenden visuellen Zuständen:

- "Normal"
- "Deaktiviert"
- "Mit Fokus"
- Gewählte

Diese visuelle Zustands Gruppe wird für alle Klassen unterstützt, die von [`VisualElement`](xref:Xamarin.Forms.VisualElement)abgeleitet werden. Dies ist die Basisklasse für [`View`](xref:Xamarin.Forms.View) und [`Page`](xref:Xamarin.Forms.Page).

Sie können auch eigene Gruppen visuelle Zustände definieren und visuelle Zustände, die als in diesem Artikel werden gezeigt.

> [!NOTE]
> Xamarin. Forms-Entwickler, die mit [Triggern](~/xamarin-forms/app-fundamentals/triggers.md) vertraut sind, wissen, dass Trigger auch Änderungen an visuellen Elementen in der Benutzeroberfläche basierend auf Änderungen in den Eigenschaften einer Ansicht oder dem Auslösen von Ereignissen vornehmen können. Verwenden von Triggern für den Umgang mit verschiedenen Kombinationen dieser Änderungen kann jedoch recht verwirrend sein. In der Vergangenheit wurde der Visual State-Manager in Windows XAML-basierten Umgebungen zur Vermeidung von Verwirrung aus Kombinationen von visuellen Zuständen bandsicherungslösungen eingeführt. Bei VSM sind immer die visuellen Zustände innerhalb eine visuelle Zustandsgruppe gegenseitig. Zu jedem Zeitpunkt ist nur ein Status in jeder Gruppe für den aktuellen Status.

## <a name="common-states"></a>Allgemeine Zustände

Der Visual State Manager ermöglicht das Einschließen von Markup in die XAML-Datei, die die visuelle Darstellung einer Ansicht ändern kann, wenn die Ansicht Normal oder deaktiviert ist oder den Eingabefokus besitzt. Diese werden als _allgemeine Zustände_bezeichnet.

Angenommen, Sie haben eine `Entry` Ansicht auf der Seite, und Sie möchten, dass die visuelle Darstellung des `Entry` wie folgt geändert wird:

- Der `Entry` sollte einen rosa Hintergrund haben, wenn die `Entry` deaktiviert ist.
- Der `Entry` sollte in der Regel über einen Kalk Hintergrund verfügen.
- Der `Entry` sollte auf eine doppelte Größe erweitert werden, wenn er den Eingabefokus besitzt.

Sie können das Markup VSM anfügen, um eine einzelne Ansicht aus, oder Sie können es in einem Stil definieren, trifft auf mehrere Ansichten. In den nächsten beiden Abschnitten werden diese Ansätze beschrieben.

### <a name="vsm-markup-on-a-view"></a>VSM-Markup für eine Sicht

Um ein VSM-Markup an eine `Entry` Ansicht anzufügen, müssen Sie zunächst die `Entry` in Start-und Endtags aufteilen:

```xaml
<Entry FontSize="18">

</Entry>
```

Ihm wird ein expliziter Schrift Grad zugewiesen, da einer der Zustände die `FontSize`-Eigenschaft verwendet, um die Größe des Texts im `Entry`zu verdoppeln.

Fügen Sie als nächstes `VisualStateManager.VisualStateGroups` Tags zwischen diesen Tags ein:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) ist eine angefügte, bindbare Eigenschaft, die von der [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) -Klasse definiert wird. (Weitere Informationen zu Eigenschaften angehängter Bindungen finden Sie im Artikel [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).) Auf diese Weise wird die `VisualStateGroups`-Eigenschaft an das `Entry` Objekt angefügt.

Die `VisualStateGroups`-Eigenschaft ist vom Typ [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList), bei dem es sich um eine Auflistung von [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) Objekten handelt. Fügen Sie innerhalb der `VisualStateManager.VisualStateGroups` Tags ein paar `VisualStateGroup` Tags für jede Gruppe von visuellen Zuständen ein, die Sie einschließen möchten:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Beachten Sie, dass das `VisualStateGroup`-Tag über ein `x:Name` Attribut verfügt, das den Namen der Gruppe angibt. Die `VisualStateGroup`-Klasse definiert eine `Name` Eigenschaft, die Sie stattdessen verwenden können:

```xaml
<VisualStateGroup Name="CommonStates">
```

Sie können entweder `x:Name` oder `Name` verwenden, aber nicht beide Elemente im selben Element.

Die `VisualStateGroup`-Klasse definiert eine Eigenschaft mit dem Namen [`States`](xref:Xamarin.Forms.VisualStateGroup.States), bei der es sich um eine Auflistung von [`VisualState`](xref:Xamarin.Forms.VisualState) Objekten handelt. `States` ist die _Content-Eigenschaft_ `VisualStateGroups`, sodass Sie die `VisualState` Tags direkt zwischen den `VisualStateGroup`-Tags einschließen können. (Inhalts Eigenschaften werden im Artikel [grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)erläutert.)

Der nächste Schritt ist ein Paar von Tags für jedes visuellen Zustand in diese Gruppe eingeschlossen werden sollen. Diese können auch mithilfe von `x:Name` oder `Name`identifiziert werden:

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

`VisualState` definiert eine Eigenschaft mit dem Namen [`Setters`](xref:Xamarin.Forms.VisualState.Setters), bei der es sich um eine Auflistung von [`Setter`](xref:Xamarin.Forms.Setter) Objekten handelt. Dies sind die gleichen `Setter` Objekte, die Sie in einem [`Style`](xref:Xamarin.Forms.Style) -Objekt verwenden.

`Setters` ist _nicht_ die Content-Eigenschaft von `VisualState`, daher ist es erforderlich, Eigenschaften Element Tags für die `Setters`-Eigenschaft hinzufügen:

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

Sie können jetzt ein oder mehrere `Setter` Objekte zwischen jedem Paar `Setters` Tags einfügen. Dies sind die `Setter`-Objekte, die die zuvor beschriebenen visuellen Zustände definieren:

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

Jedes `Setter` Tag gibt den Wert einer bestimmten Eigenschaft an, wenn dieser Zustand aktuell ist. Jede Eigenschaft, auf die von einem `Setter` Objekt verwiesen wird, muss durch eine bindbare Eigenschaft unterstützt werden.

Ein ähnliches Markup ist die Grundlage für die **VSM auf der Ansichts** Seite im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispielprogramm. Die Seite enthält drei `Entry` Ansichten, aber nur für die zweite ist das VSM-Markup angefügt:

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

Beachten Sie, dass die zweite `Entry` auch eine `DataTrigger` als Teil der `Trigger` Auflistung hat. Dies bewirkt, dass der `Entry` deaktiviert wird, bis etwas in das dritte `Entry`eingegeben wird. So sieht die Seite beim Start, die unter iOS, Android und die universelle Windows-Plattform (UWP):

[![VSM in Ansicht: deaktiviert](vsm-images/VsmOnViewDisabled.png "VSM in Ansicht-deaktiviert")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Der aktuelle visuelle Zustand ist "deaktiviert", sodass der Hintergrund des zweiten `Entry` auf den IOS-und Android-Bildschirmen Rosa ist. Die UWP-Implementierung von `Entry` lässt das Festlegen der Hintergrundfarbe nicht zu, wenn die `Entry` deaktiviert ist.

Wenn Sie Text in das dritte `Entry`eingeben, wechselt der zweite `Entry` in den "normalen" Zustand, und der Hintergrund ist jetzt "Kalk":

[![VSM in Ansicht: normal](vsm-images/VsmOnViewNormal.png "VSM in Ansicht (normal)")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Wenn Sie die zweite `Entry`berühren, erhält Sie den Eingabefokus. Er wechselt in den Zustand "Fokussiert" und zweimal Höhe erweitert:

[![VSM in Ansicht: Fokus](vsm-images/VsmOnViewFocused.png "VSM im Fokus")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Beachten Sie, dass der `Entry` den Kalk-Hintergrund nicht beibehält, wenn er den Eingabefokus erhält. Wie der Visual State Manager zwischen visuellen Zuständen wechselt, sind die Eigenschaften, die festlegen, indem der vorherige Status nicht festgelegt. Denken Sie daran, die das visuelle Zustände schließen sich gegenseitig aus. Der Status "Normal" bedeutet nicht nur, dass die `Entry` aktiviert ist. Dies bedeutet, dass die `Entry` aktiviert ist und keinen Eingabefokus hat.

Wenn Sie möchten, dass die `Entry` über einen Kalk Hintergrund im Zustand "fokussiert" verfügen, fügen Sie diesem visuellen Zustand eine weitere `Setter` hinzu:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

Damit diese `Setter` Objekte ordnungsgemäß funktionieren, muss ein `VisualStateGroup` `VisualState` Objekte für alle Zustände in dieser Gruppe enthalten. Wenn es einen visuellen Zustand gibt, der keine `Setter` Objekte enthält, schließen Sie ihn trotzdem als leeres Tag ein:

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>Visueller Zustand-Manager-Markup in einem Stil

Es ist häufig erforderlich, die das gleiche Visual State-Manager-Markup zwischen zwei oder mehreren Ansichten gemeinsam nutzen. In diesem Fall möchten Sie das Markup in eine `Style` Definition einfügen.

Im folgenden finden Sie die vorhandenen impliziten `Style` für die `Entry` Elemente auf der Seite **VSM auf der Ansichts** Seite:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

Fügen Sie `Setter` Tags für die `VisualStateManager.VisualStateGroups` angefügte bindbare Eigenschaft hinzu:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

Die Content-Eigenschaft für `Setter` ist `Value`, sodass der Wert der Eigenschaft `Value` direkt innerhalb dieser Tags angegeben werden kann. Diese Eigenschaft ist vom Typ `VisualStateGroupList`:

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

Innerhalb dieser Tags können Sie einen der weiteren `VisualStateGroup` Objekte einschließen:

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

Der Rest des Markups VSM ist der gleiche wie zuvor.

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

Nun reagieren alle `Entry` Ansichten auf dieser Seite auf die gleiche Weise wie Ihre visuellen Zustände. Beachten Sie auch, dass der "fokussierte" Status nun eine zweite `Setter` enthält, die jeder `Entry` einen Kalk Hintergrund übergibt, wenn er den Eingabefokus hat:

[![VSM im Stil](vsm-images/VsmInStyle.png "VSM im Stil")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Visuelle Zustände in xamarin. Forms

In der folgenden Tabelle sind die visuellen Zustände aufgeführt, die in xamarin. Forms definiert sind:

| Klasse | Zustände | Weitere Informationen |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [Visuelle Schaltflächen für Flächen](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [Visuelle Zustände von carouselview](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `CollectionView` | `Selected` | [Farbe für ausgewähltes Element ändern](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color) |
| `ImageButton` | `Pressed` | [Visuelle Status von ImageButton](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [Allgemeine Zustände](#common-states) |

Auf jeden dieser Zustände kann über die visuelle Zustands Gruppe mit dem Namen `CommonStates`zugegriffen werden.

## <a name="set-state-on-multiple-elements"></a>Festlegen des Zustands für mehrere Elemente

In den vorherigen Beispielen wurden visuelle Zustände an einzelne Elemente angefügt und verarbeitet. Es ist jedoch auch möglich, visuelle Zustände zu erstellen, die an ein einzelnes Element angefügt sind, aber Eigenschaften für andere Elemente innerhalb desselben Bereichs festlegen. Dadurch wird vermieden, dass visuelle Zustände für jedes Element wiederholt werden müssen, auf dem die Zustände arbeiten.

Der [`Setter`](xref:Xamarin.Forms.Setter) -Typ verfügt über eine `TargetName`-Eigenschaft vom Typ `string`, die das Ziel Element darstellt, das die `Setter` für einen visuellen Zustand bearbeiten wird. Wenn die `TargetName`-Eigenschaft definiert ist, legt der `Setter` die in `TargetName` definierte `Property` des-Elements auf `Value`fest:

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

In diesem Beispiel wird für eine `Label` mit dem Namen `label` die Eigenschaft `TextColor` auf `Red`festgelegt. Wenn Sie die `TargetName`-Eigenschaft festlegen, müssen Sie in `Property`den vollständigen Pfad zur-Eigenschaft angeben. Wenn Sie die `TextColor`-Eigenschaft für eine `Label`festlegen möchten, wird `Property` als `Label.TextColor`angegeben.

> [!NOTE]
> Jede Eigenschaft, auf die von einem `Setter` Objekt verwiesen wird, muss durch eine bindbare Eigenschaft unterstützt werden.

Die Seite **VSM mit Setter TargetName** im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispiel zeigt, wie der Status für mehrere Elemente aus einer einzelnen visuellen Zustands Gruppe festgelegt wird. Die XAML-Datei besteht aus einer `StackLayout`, die ein `Label` Element, eine `Entry`und eine `Button`enthält:

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

VSM-Markup ist an den `StackLayout`angefügt. Es gibt zwei gegenseitig ausschließende Zustände mit dem Namen "Normal" und "gedrückt", wobei jeder Zustand `VisualState` Tags enthält.

Der Status "Normal" ist aktiv, wenn das `Button` nicht gedrückt wird und eine Antwort auf die Frage eingegeben werden kann:

[![VSM Setter TargetName: normaler Status](vsm-images/VsmSetterTargetNameNormal.png "VSM Setter TargetName-normal")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

Der Zustand "gedrückt" wird aktiv, wenn die `Button` gedrückt wird:

[![VSM Setter TargetName: gedrückter Zustand](vsm-images/VsmSetterTargetNamePressed.png "VSM-Setter TargetName-gedrückt")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

Die "gedrückte" `VisualState` gibt an, dass die `Scale` Eigenschaft beim Drücken der `Button` vom Standardwert von 1 in 0,8 geändert wird. Außerdem wird für den `Entry` mit der Bezeichnung `entry` die Eigenschaft `Text` auf Paris festgelegt. Das Ergebnis ist, dass beim Drücken der `Button` eine etwas geringere Größe erreicht wird und die `Entry` Paris anzeigt. Wenn die `Button` freigegeben wird, wird Sie auf den Standardwert 1 umgestellt, und die `Entry` zeigt jeden zuvor eingegebenen Text an.

> [!IMPORTANT]
> Eigenschafts Pfade werden derzeit in `Setter` Elementen, die die `TargetName`-Eigenschaft angeben, nicht unterstützt.

## <a name="define-your-own-visual-states"></a>Definieren Ihrer eigenen visuellen Zustände

Jede Klasse, die von `VisualElement` abgeleitet wird, unterstützt die drei allgemeinen Zustände "Normal", "fokussiert" und "deaktiviert". Intern erkennt die [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) -Klasse, wann Sie aktiviert oder deaktiviert wird, oder konzentriert bzw. ohne Fokus ist, und ruft die statische [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) -Methode auf:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Dies ist der einzige Code des visuellen Zustands-Managers, den Sie in der `VisualElement`-Klasse finden. Da `GoToState` für jedes Objekt auf der Grundlage jeder Klasse aufgerufen wird, die von `VisualElement`abgeleitet ist, können Sie den Visual State Manager mit jedem `VisualElement`-Objekt verwenden, um auf diese Änderungen zu reagieren.

Interessanterweise wird in `VisualElement`nicht explizit auf den Namen der visuellen Zustands Gruppe "CommonStates" verwiesen. Der Gruppenname ist nicht Teil der API für den visuellen Status-Manager. In einem der zwei-Beispielprogramm, das bisher gezeigt können Sie den Namen der Gruppe aus "CommonStates" zu einem anderen ändern, und das Programm weiterhin funktionieren. Der Gruppenname ist lediglich eine allgemeine Beschreibung der Zustände in der Gruppe. Es ist implizit klar, dass die visuellen Zustände in einer beliebigen Gruppe sich gegenseitig ausschließende sind: ein Zustand, und nur ein Status ist dem neuesten Stand zu einem beliebigen Zeitpunkt.

Wenn Sie Ihre eigenen visuellen Zustände implementieren möchten, müssen Sie `VisualStateManager.GoToState` aus dem Code abrufen. Den meisten Fällen stellen Sie diesen Aufruf aus der CodeBehind-Datei der Page-Klasse.

Die Seite **VSM** -Überprüfung im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispiel zeigt, wie Sie den visuellen Zustands-Manager in Verbindung mit der Eingabevalidierung verwenden. Die XAML-Datei besteht aus einer `StackLayout`, die zwei `Label` Elemente enthält, eine `Entry`und eine `Button`:

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

VSM-Markup wird an den `StackLayout` (mit dem Namen `stackLayout`) angefügt. Es gibt zwei gegenseitig ausschließende Zustände mit dem Namen "valid" und "invalid", wobei jeder Status `VisualState` Tags enthält.

Wenn das `Entry` keine gültige Telefonnummer enthält, ist der aktuelle Zustand "ungültig", sodass die `Entry` einen rosa Hintergrund hat, der zweite `Label` sichtbar ist und die `Button` deaktiviert ist:

[![VSM-Überprüfung: Ungültiger Status](vsm-images/VsmValidationInvalid.png "VSM-Überprüfung-ungültig")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Wenn eine gültige Telefonnummer eingegeben wird, klicken Sie dann der aktuelle Status ist "Gültig". Der `Entry` erhält einen Kalk Hintergrund, der zweite `Label` verschwindet, und die `Button` ist nun aktiviert:

[![VSM-Überprüfung: gültiger Status](vsm-images/VsmValidationValid.png "VSM-Validierung-gültig")](vsm-images/VsmValidationValid-Large.png#lightbox)

Die Code-Behind-Datei ist für die Behandlung des `TextChanged` Ereignisses vom `Entry`verantwortlich. Der Handler verwendet einen regulären Ausdruck, um festzustellen, ob die Eingabezeichenfolge gültig ist. Die-Methode in der Code Behind-Datei mit dem Namen `GoToState` Ruft die statische `VisualStateManager.GoToState`-Methode für `stackLayout`auf:

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

Beachten Sie auch, dass die `GoToState`-Methode vom Konstruktor aufgerufen wird, um den Status zu initialisieren. Es sollte stets aktuellen Status vorhanden sein. Aber gar nicht im Code gibt es alle Verweise auf den Namen der visuellen Zustandsgruppe, obwohl es in die XAML als "ValidationStates" Gründen der Klarheit verwiesen wird.

Beachten Sie, dass die Code Behind-Datei nur das-Objekt auf der Seite berücksichtigen muss, das die visuellen Zustände definiert, und `VisualStateManager.GoToState` für dieses Objekt aufzurufen. Dies liegt daran, dass beide visuellen Zustände mehrere Objekte auf der Seite als Ziel haben.

Vielleicht Fragen Sie sich: Wenn die Code-Behind-Datei auf das Objekt auf der Seite verweisen muss, die die visuellen Zustände definiert, warum kann die Code-Behind-Datei nicht einfach direkt auf dieses und andere Objekte zugreifen? Sie könnten sicherlich. Der Vorteil der Verwendung der VSM ist jedoch, dass Sie, wie visuelle Elemente steuern können in anderen Zustand vollständig in XAML, dadurch alle von den UI-Entwurf an einem Ort bleibt reagieren. Dadurch wird die visuelle Darstellung der Einstellung durch den Zugriff auf visuelle Elemente direkt aus der CodeBehind-vermieden.

## <a name="visual-state-triggers"></a>Trigger für visuellen Zustand

Visuelle Zustände unterstützen Zustands Trigger, bei denen es sich um eine spezialisierte Gruppe von Triggern handelt, die die Bedingungen definieren, unter denen eine [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

Zustands Trigger werden der [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) -Auflistung einer [`VisualState`](xref:Xamarin.Forms.VisualState)hinzugefügt. Diese Auflistung kann einen Single State-Trigger oder mehrere State-Trigger enthalten. Eine [`VisualState`](xref:Xamarin.Forms.VisualState) wird angewendet, wenn alle Zustands Trigger in der Auflistung aktiv sind.

Wenn Sie Status Trigger zum Steuern von visuellen Zuständen verwenden, verwendet xamarin. Forms die folgenden Rang folgen Regeln, um zu bestimmen, welcher Trigger (und die entsprechenden [`VisualState`](xref:Xamarin.Forms.VisualState)) aktiv sein werden:

1. Alle Auslösers, die von [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)abgeleitet werden.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) aktiviert, weil die [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) Bedingung erfüllt ist.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) aktiviert, weil die [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) Bedingung erfüllt ist.

Wenn mehrere Trigger gleichzeitig aktiv sind (z. b. zwei benutzerdefinierte Trigger), hat der erste im Markup deklarierte Trigger Vorrang.

Weitere Informationen zu Status Triggern finden Sie unter State Triggers ( [Zustands Trigger](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)).

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>Verwenden des visuellen Zustands-Managers für adaptives Layout

Eine Xamarin.Forms-die Anwendung, die auf einem Smartphone ausgeführt wird, in der Regel in ein Portrait oder Querformat Seitenverhältnis und eine Xamarin.Forms-Anwendung, die auf dem Desktop angezeigt werden kann, kann geändert werden, um viele verschiedene Größen und Seitenverhältnisse wird davon ausgegangen. Eine überlegt entworfene Anwendung möglicherweise den Inhalt für diese verschiedenen Formfaktoren Seite bzw. im Fenster unterschiedlich angezeigt.

Diese Technik wird manchmal als _Adaptives Layout_bezeichnet. Da adaptive Layout ausschließlich eines Programms visuellen Elemente beinhaltet, ist es eine ideale Anwendung des visuellen Zustands-Manager.

Ein einfaches Beispiel ist eine Anwendung, die eine kleine Sammlung von Schaltflächen angezeigt, die der Inhalt der Anwendung auswirken. Im Hochformat möglicherweise diese Schaltflächen in einer horizontalen Zeile oben auf der Seite angezeigt:

[![Adaptive VSM-Layout: Hochformat](vsm-images/VsmAdaptiveLayoutPortrait.png "Adaptive VSM-Layout-Hochformat")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Im Querformat das Array von Schaltflächen möglicherweise auf einer Seite verschoben, und in einer Spalte angezeigt:

[![Adaptive VSM-Layout: Querformat](vsm-images/VsmAdaptiveLayoutLandscape.png "Adaptive VSM-Layout-Querformat")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Von oben nach unten wird das Programm auf die universelle Windows-Plattform, Android und iOS ausgeführt.

Die Seite für das **VSM Adaptive Layout** im [vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) -Beispiel definiert eine Gruppe mit dem Namen "orientationstates" mit zwei visuellen Zuständen namens "Hochformat" und "Querformat". (Ein komplexer Ansatz kann auf mehrere verschiedene Breiten von Seite bzw. im Fenster basieren.)

VSM Markup tritt an vier Stellen in der XAML-Datei. Die `StackLayout` mit dem Namen `mainStack` enthält sowohl das Menü als auch den Inhalt, bei dem es sich um ein `Image` Element handelt. Diese `StackLayout` sollte eine vertikale Ausrichtung im Hochformat und eine horizontale Ausrichtung im Querformat aufweisen:

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

Die inneren `ScrollView` mit dem Namen `menuScroll` und die `StackLayout` benannte `menuStack` implementieren das Menü der Schaltflächen. Die Ausrichtung dieser Layouts ist gegen `mainStack`. Klicken Sie im Menü sollte im Hochformat horizontale und vertikale im Querformatmodus ausgeführt werden.

Der vierte Abschnitt VSM Markup wird in ein impliziter Stil für die Schaltflächen selbst. Dieses Markup legt `VerticalOptions`-, `HorizontalOptions`-und `Margin`-Eigenschaften fest, die für das hoch-und Querformat spezifisch sind.

Mit der Code-Behind-Datei wird die `BindingContext`-Eigenschaft `menuStack` festgelegt, um `Button`-Befehle zu implementieren. Außerdem wird ein Handler an das `SizeChanged`-Ereignis der Seite angefügt:

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

Der `SizeChanged` Handler ruft `VisualStateManager.GoToState` für die beiden Elemente `StackLayout` und `ScrollView` auf und durchläuft dann die untergeordneten Elemente von `menuStack`, um `VisualStateManager.GoToState` für die `Button` Elemente aufzurufen.

Es mag, als ob die Code-Behind-Datei kann Änderungen der bildschirmausrichtung direkt durch das Festlegen von Eigenschaften von Elementen in der XAML-Datei verarbeiten, aber der Visual State-Manager auf jeden Fall einen strukturierteren Ansatz ist. Alles, was die visuellen Elemente in der XAML-Datei gespeichert werden, in denen sie einfacher untersucht werden, verwalten und zu ändern.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visueller Zustand-Manager mit Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Video zu xamarin. Forms 3,0 Visual State Manager**

## <a name="related-links"></a>Verwandte Links

- [Vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [Zustands Trigger](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
