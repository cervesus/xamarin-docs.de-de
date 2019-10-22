---
title: Der Visual State-Manager von xamarin. Forms
description: Verwenden Sie den Visual State Manager, um Änderungen an XAML-Elementen auf der Grundlage von visuellen Zuständen vorzunehmen, die aus Code festgelegt wurden
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 228501172ede71204c64e1efe1673ce92be424ea
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656055"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Der Visual State-Manager von xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Verwenden Sie den Visual State Manager, um Änderungen an XAML-Elementen auf der Grundlage von visuellen Zuständen vorzunehmen, die aus Code festgelegt wurden_

Der Visual State Manager (VSM) ist neu in xamarin. Forms 3,0. Das VSM bietet eine strukturierte Möglichkeit, visuelle Änderungen an der Benutzeroberfläche aus dem Code vorzunehmen. In den meisten Fällen wird die Benutzeroberfläche der Anwendung in XAML definiert, und dieses XAML enthält Markup, das beschreibt, wie sich der visuelle Zustands-Manager auf die visuellen Elemente der Benutzeroberfläche auswirkt.

Das VSM führt das Konzept der _visuellen Zustände_ein. Eine xamarin. Forms-Ansicht, z. b. eine `Button`, kann je nach dem zugrunde liegenden Zustand &mdash;, ob Sie deaktiviert oder gedrückt ist oder den Eingabefokus besitzt. Dies sind die Zustände der Schaltfläche.

Visuelle Zustände werden in _visuellen Zustands Gruppen_gesammelt. Alle visuellen Zustände innerhalb einer visuellen Zustands Gruppe schließen sich gegenseitig aus. Visuelle Zustände und visuelle Zustands Gruppen werden durch einfache Text Zeichenfolgen identifiziert.

Der Visual State-Manager von xamarin. Forms definiert eine visuelle Zustands Gruppe mit dem Namen "CommonStates" mit drei visuellen Zuständen:

- "Normal"
- Deaktiviert
- Gestellt

Diese visuelle Zustands Gruppe wird für alle Klassen unterstützt, die von [`VisualElement`](xref:Xamarin.Forms.VisualElement)abgeleitet werden. Dies ist die Basisklasse für [`View`](xref:Xamarin.Forms.View) und [`Page`](xref:Xamarin.Forms.Page). 

Sie können auch eigene visuelle Zustands Gruppen und visuelle Zustände definieren, wie in diesem Artikel veranschaulicht wird.

> [!NOTE]
> Xamarin. Forms-Entwickler, die mit [Triggern](~/xamarin-forms/app-fundamentals/triggers.md) vertraut sind, wissen, dass Trigger auch Änderungen an visuellen Elementen in der Benutzeroberfläche basierend auf Änderungen in den Eigenschaften einer Ansicht oder dem Auslösen von Ereignissen vornehmen können. Die Verwendung von Triggern für verschiedene Kombinationen dieser Änderungen kann jedoch etwas verwirrend werden. In der Vergangenheit wurde der Visual State Manager in Windows XAML-basierten Umgebungen eingeführt, um die Verwirrung zu verringern, die sich aus Kombinationen von visuellen Zuständen ergibt. Beim VSM schließen sich die visuellen Zustände innerhalb einer visuellen Zustands Gruppe immer gegenseitig aus. Zu jedem Zeitpunkt ist nur ein Status in jeder Gruppe der aktuelle Zustand.

## <a name="the-common-states"></a>Die allgemeinen Zustände

Der Visual State Manager ermöglicht das Einschließen von Abschnitten in der XAML-Datei, die die visuelle Darstellung einer Ansicht ändern können, wenn die Ansicht Normal oder deaktiviert ist oder den Eingabefokus besitzt. Diese werden als _allgemeine Zustände_bezeichnet.

Angenommen, Sie haben eine `Entry` Ansicht auf der Seite, und Sie möchten, dass die visuelle Darstellung des `Entry` wie folgt geändert wird:

- Der `Entry` sollte einen rosa Hintergrund haben, wenn die `Entry` deaktiviert ist.
- Der `Entry` sollte in der Regel über einen Kalk Hintergrund verfügen.
- Der `Entry` sollte auf eine doppelte Größe erweitert werden, wenn er den Eingabefokus besitzt.

Sie können das VSM-Markup an eine einzelne Ansicht anfügen, oder Sie können es in einem Stil definieren, wenn es für mehrere Ansichten gilt. In den nächsten beiden Abschnitten werden diese Vorgehensweisen beschrieben.

### <a name="vsm-markup-on-a-view"></a>VSM-Markup für eine Ansicht

Um ein VSM-Markup an eine `Entry` Ansicht anzufügen, müssen Sie zunächst die `Entry` in Start-und Endtags aufteilen:

```xaml
<Entry FontSize="18">

</Entry>
```

Ihm wird ein expliziter Schrift Grad zugewiesen, da einer der Zustände die `FontSize`-Eigenschaft verwendet, um die Größe des Texts im `Entry` zu verdoppeln.

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

Der nächste Schritt besteht darin, ein Tagpaar für jeden visuellen Zustand in dieser Gruppe einzuschließen. Diese können auch mithilfe von `x:Name` oder `Name` identifiziert werden:

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

Beachten Sie, dass die zweite `Entry` auch eine `DataTrigger` als Teil der `Trigger` Auflistung hat. Dies bewirkt, dass der `Entry` deaktiviert wird, bis etwas in das dritte `Entry` eingegeben wird. Hier ist die Seite beim Start, die unter IOS, Android und der universelle Windows-Plattform (UWP) ausgeführt wird:

[![VSM in Ansicht: deaktiviert](vsm-images/VsmOnViewDisabled.png "VSM in Ansicht-deaktiviert")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Der aktuelle visuelle Zustand ist "deaktiviert", sodass der Hintergrund des zweiten `Entry` auf den IOS-und Android-Bildschirmen Rosa ist. Die UWP-Implementierung von `Entry` lässt das Festlegen der Hintergrundfarbe nicht zu, wenn die `Entry` deaktiviert ist. 

Wenn Sie Text in das dritte `Entry` eingeben, wechselt der zweite `Entry` in den "normalen" Zustand, und der Hintergrund ist jetzt "Kalk":

[![VSM in Ansicht: normal](vsm-images/VsmOnViewNormal.png "VSM in Ansicht (normal)")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Wenn Sie die zweite `Entry` berühren, erhält Sie den Eingabefokus. Es wechselt in den Zustand "fokussiert" und wird um die doppelte Höhe erweitert:

[![VSM in Ansicht: Fokus](vsm-images/VsmOnViewFocused.png "VSM im Fokus")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Beachten Sie, dass der `Entry` den Kalk-Hintergrund nicht beibehält, wenn er den Eingabefokus erhält. Wenn der Visual State-Manager zwischen den visuellen Zuständen wechselt, werden die Eigenschaften, die vom vorherigen Zustand festgelegt wurden, nicht festgelegt. Beachten Sie, dass sich die visuellen Zustände gegenseitig ausschließen. Der Status "Normal" bedeutet nicht nur, dass die `Entry` aktiviert ist. Dies bedeutet, dass die `Entry` aktiviert ist und keinen Eingabefokus hat. 

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

### <a name="visual-state-manager-markup-in-a-style"></a>Visual State-Manager-Markup in einem Stil

Häufig ist es erforderlich, dass Sie das gleiche Visual State Manager-Markup für zwei oder mehr Ansichten verwenden. In diesem Fall möchten Sie das Markup in eine `Style` Definition einfügen.

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

Nun reagieren alle `Entry` Ansichten auf dieser Seite auf die gleiche Weise wie Ihre visuellen Zustände. Beachten Sie auch, dass der "fokussierte" Status nun eine zweite `Setter` enthält, die jeder `Entry` einen Kalk Hintergrund übergibt, wenn er den Eingabefokus hat:

[![VSM im Stil](vsm-images/VsmInStyle.png "VSM im Stil")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definieren Ihrer eigenen visuellen Zustände

Jede Klasse, die von `VisualElement` abgeleitet wird, unterstützt die drei allgemeinen Zustände "Normal", "fokussiert" und "deaktiviert". Intern erkennt die [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) -Klasse, wann Sie aktiviert oder deaktiviert wird, oder konzentriert bzw. ohne Fokus ist, und ruft die statische [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) -Methode auf:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Dies ist der einzige Code des visuellen Zustands-Managers, den Sie in der `VisualElement`-Klasse finden. Da `GoToState` für jedes Objekt auf der Grundlage jeder Klasse aufgerufen wird, die von `VisualElement` abgeleitet ist, können Sie den Visual State Manager mit jedem `VisualElement`-Objekt verwenden, um auf diese Änderungen zu reagieren.

Interessanterweise wird in `VisualElement` nicht explizit auf den Namen der visuellen Zustands Gruppe "CommonStates" verwiesen. Der Gruppenname ist nicht Teil der API für den Visual State Manager. Innerhalb eines der beiden bisher gezeigten Beispiel Programme können Sie den Namen der Gruppe von "CommonStates" in beliebige andere ändern, und das Programm funktioniert weiterhin. Der Gruppenname ist lediglich eine allgemeine Beschreibung der Zustände in dieser Gruppe. Es ist implizit zu verstehen, dass die visuellen Zustände in jeder Gruppe sich gegenseitig ausschließen: ein einziger Status und immer nur ein Status ist aktuell.

Wenn Sie Ihre eigenen visuellen Zustände implementieren möchten, müssen Sie `VisualStateManager.GoToState` aus dem Code abrufen. In den meisten Fällen wird dieser Befehl aus der Code Behind-Datei Ihrer Page-Klasse aufgerufen.

Die Seite **VSM** -Überprüfung im **[vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispiel zeigt, wie Sie den visuellen Zustands-Manager in Verbindung mit der Eingabevalidierung verwenden. Die XAML-Datei besteht aus zwei `Label` Elementen, einem `Entry` und `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

Das VSM-Markup wird dem zweiten `Label` (mit dem Namen `helpLabel`) und dem `Button` (mit dem Namen `submitButton`) angefügt. Es gibt zwei gegenseitig ausschließende Zustände mit den Namen "valid" und "invalid". Beachten Sie, dass jede der beiden "validationstate"-Gruppen `VisualState`-Tags für "valid" und "invalid" enthält, obwohl eine der beiden "validationstate"-Tags in jedem Fall leer ist. 

Wenn das `Entry` keine gültige Telefonnummer enthält, ist der aktuelle Zustand "ungültig", sodass der zweite `Label` sichtbar und die `Button` deaktiviert ist:

[![VSM-Überprüfung: Ungültiger Status](vsm-images/VsmValidationInvalid.png "VSM-Überprüfung-ungültig")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Wenn eine gültige Telefonnummer eingegeben wird, wird der aktuelle Status "gültig". Der zweite `Entry` verschwindet, und der `Button` ist nun aktiviert:

[![VSM-Überprüfung: gültiger Status](vsm-images/VsmValidationValid.png "VSM-Validierung-gültig")](vsm-images/VsmValidationValid-Large.png#lightbox)

Die Code-Behind-Datei ist für die Behandlung des `TextChanged` Ereignisses vom `Entry` reponsible. Der Handler verwendet einen regulären Ausdruck, um zu bestimmen, ob die Eingabe Zeichenfolge gültig ist. Die-Methode in der Code Behind-Datei mit dem Namen `GoToState` Ruft die statische `VisualStateManager.GoToState`-Methode für `helpLabel` und `submitButton` auf:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

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
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

Beachten Sie auch, dass die `GoToState`-Methode vom Konstruktor aufgerufen wird, um den Status zu initialisieren. Es sollte immer ein aktueller Zustand vorhanden sein. Im Code gibt es jedoch keinen Verweis auf den Namen der visuellen Zustands Gruppe, obwohl in XAML als "ValidationStates" aus Gründen der Übersichtlichkeit darauf verwiesen wird. 

Beachten Sie, dass die Code Behind-Datei jedes Objekt auf der Seite berücksichtigen muss, das von diesen visuellen Zuständen betroffen ist, und `VisualStateManager.GoToState` für jedes dieser Objekte aufzurufen. In diesem Beispiel sind nur zwei Objekte (die `Label` und die `Button`), aber es kann auch mehrere sein.

Vielleicht Fragen Sie sich: Wenn die Code-Behind-Datei auf jedes Objekt auf der Seite verweisen muss, die von diesen visuellen Zuständen betroffen ist, warum kann die Code-Behind-Datei nicht einfach direkt auf die Objekte zugreifen? Dies könnte sicherlich der Fall sein. Der Vorteil der Verwendung des VSM besteht jedoch darin, dass Sie steuern können, wie visuelle Elemente vollständig in XAML auf einen anderen Zustand reagieren, wodurch der gesamte Benutzeroberflächen Entwurf an einem Ort beibehalten wird. Dadurch wird das Festlegen der visuellen Darstellung vermieden, indem direkt aus dem Code-Behind auf visuelle Elemente zugegriffen wird.

Es kann verlockend sein, eine Klasse von `Entry` ableiten und möglicherweise eine Eigenschaft zu definieren, die Sie auf eine externe Validierungsfunktion festlegen können. Die Klasse, die von `Entry` abgeleitet ist, kann dann die `VisualStateManager.GoToState`-Methode aufzurufen. Dieses Schema funktioniert problemlos, aber nur, wenn das `Entry` das einzige Objekt war, das von den verschiedenen visuellen Zuständen betroffen war. In diesem Beispiel sind auch ein `Label` und ein `Button` betroffen. Es gibt keine Möglichkeit für das Anfügen von VSM-Markup an eine `Entry`, um andere Objekte auf der Seite zu steuern, und für VSM-Markup, das an diese anderen Objekte angefügt wird, um auf eine Änderung des visuellen Zustands von einem anderen Objekt zu verweisen.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Verwenden des visuellen Zustands-Managers für adaptives Layout

Eine xamarin. Forms-Anwendung, die auf einem Telefon ausgeführt wird, kann in der Regel in einem hoch-oder Querformat angezeigt werden, und ein auf dem Desktop ausgeführte xamarin. Forms-Programm kann so angepasst werden, dass viele verschiedene Größen und Seitenverhältnisse angenommen werden. Eine gut entworfene Anwendung zeigt ihren Inhalt möglicherweise für diese verschiedenen Seiten-oder Fenster Formfaktoren anders an. 

Diese Technik wird manchmal als _Adaptives Layout_bezeichnet. Da das adaptive Layout lediglich die visuellen Elemente eines Programms umfasst, ist es eine ideale Anwendung des visuellen Zustands-Managers.

Ein einfaches Beispiel ist eine Anwendung, die eine kleine Auflistung von Schaltflächen anzeigt, die sich auf den Inhalt der Anwendung auswirken. Im Hochformat können diese Schaltflächen in einer horizontalen Zeile oben auf der Seite angezeigt werden:

[![Adaptive VSM-Layout: Hochformat](vsm-images/VsmAdaptiveLayoutPortrait.png "Adaptive VSM-Layout-Hochformat")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Im Querformat kann das Array von Schaltflächen auf eine Seite verschoben und in einer Spalte angezeigt werden:

[![Adaptive VSM-Layout: Querformat](vsm-images/VsmAdaptiveLayoutLandscape.png "Adaptive VSM-Layout-Querformat")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Von oben nach unten wird das Programm auf den universelle Windows-Plattform, Android und IOS ausgeführt.

Die Seite für das **VSM Adaptive Layout** im [vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) -Beispiel definiert eine Gruppe mit dem Namen "orientationstates" mit zwei visuellen Zuständen namens "Hochformat" und "Querformat". (Ein komplexerer Ansatz kann auf verschiedenen Seiten-oder fensterbreiten basieren.) 

VSM-Markup tritt an vier Stellen in der XAML-Datei auf. Die `StackLayout` mit dem Namen `mainStack` enthält sowohl das Menü als auch den Inhalt, bei dem es sich um ein `Image` Element handelt. Diese `StackLayout` sollte eine vertikale Ausrichtung im Hochformat und eine horizontale Ausrichtung im Querformat aufweisen:

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

Die inneren `ScrollView` mit dem Namen `menuScroll` und die `StackLayout` benannte `menuStack` implementieren das Menü der Schaltflächen. Die Ausrichtung dieser Layouts ist gegen `mainStack`. Das Menü sollte im Hochformat horizontal und vertikal im Querformat angezeigt werden.

Der vierte Abschnitt von VSM-Markup weist einen impliziten Stil für die Schaltflächen auf. Dieses Markup legt `VerticalOptions`-, `HorizontalOptions`-und `Margin`-Eigenschaften fest, die speziell für die Ausrichtungen "Portait" und "quer

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

Es mag so aussehen, als ob die Code-Behind-Datei Richtungsänderungen direkt behandeln kann, indem Sie die Eigenschaften der Elemente in der XAML-Datei festlegen, aber der visuelle Zustands-Manager ist definitiv ein strukturierteres Konzept. Alle visuellen Elemente werden in der XAML-Datei gespeichert, wo Sie leichter zu untersuchen, zu warten und zu ändern sind.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager mit xamarin. University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Video zu xamarin. Forms 3,0 Visual State Manager**

## <a name="related-links"></a>Verwandte Links

- [Vsmdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
