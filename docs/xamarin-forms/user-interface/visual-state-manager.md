---
title: Die Xamarin.Forms visuellen Zustands-Manager
description: Verwenden der Visual State-Manager, um XAML-Elemente, die basierend auf visuelle Zustände, die im Code festgelegt zu ändern.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 228501172ede71204c64e1efe1673ce92be424ea
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656055"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Die Xamarin.Forms visuellen Zustands-Manager

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Verwenden der Visual State-Manager, um XAML-Elemente, die basierend auf visuelle Zustände, die im Code festgelegt zu ändern._

Der Visual State Manager (VSM) ist in Xamarin.Forms 3.0 neu. VSM bietet ein strukturiertes Verfahren zum visuelle Änderungen an der Benutzeroberfläche aus Code zu machen. In den meisten Fällen ist die Benutzeroberfläche der Anwendung in XAML, definiert und dieses XAML enthält Markup, beschreibt, wie der Visual State Manager wirkt sich die visuellen Elemente der Benutzeroberfläche auf.

VSM führt das Konzept der _visuelle Zustände_. Eine Xamarin.Forms-Ansicht, wie z. B. eine `Button` kann mehrere unterschiedliche visuelle Darstellungen aufweisen, abhängig von den zugrunde liegenden Zustand &mdash; gibt an, ob es deaktiviert ist, oder drücken oder hat den Eingabefokus. Hierbei handelt es sich um die Schaltfläche "-Status.

Visuelle Zustände sind in erfassten _visual Statusgruppen_. Alle visuellen Statusoptionen in eine visuelle Zustandsgruppe schließen sich gegenseitig aus. Sowohl visuelle Statusoptionen und Statusgruppen visual werden durch einfache Textzeichenfolgen identifiziert.

Der Visual State-Manager von Xamarin.Forms definiert eine visuelle Zustandsgruppe namens "CommonStates" mit drei visuelle Zustände:

- "Normal"
- "Deaktiviert"
- "Mit Fokus"

Dieser visuelle Zustandsgruppe wird für alle abgeleitete Klassen unterstützt [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), dies ist die Basisklasse für [ `View` ](xref:Xamarin.Forms.View) und [ `Page` ](xref:Xamarin.Forms.Page). 

Sie können auch eigene Gruppen visuelle Zustände definieren und visuelle Zustände, die als in diesem Artikel werden gezeigt.

> [!NOTE]
> Xamarin.Forms-Entwickler, die mit vertraut [Trigger](~/xamarin-forms/app-fundamentals/triggers.md) ist bekannt, dass der Trigger auch Visuals auf der Grundlage von Änderungen in die Eigenschaften einer Sicht oder das Auslösen von Ereignissen Benutzeroberfläche ändern können. Verwenden von Triggern für den Umgang mit verschiedenen Kombinationen dieser Änderungen kann jedoch recht verwirrend sein. In der Vergangenheit wurde der Visual State-Manager in Windows XAML-basierten Umgebungen zur Vermeidung von Verwirrung aus Kombinationen von visuellen Zuständen bandsicherungslösungen eingeführt. Bei VSM sind immer die visuellen Zustände innerhalb eine visuelle Zustandsgruppe gegenseitig. Zu jedem Zeitpunkt ist nur ein Status in jeder Gruppe für den aktuellen Status.

## <a name="the-common-states"></a>Der allgemeine Status

Der Visual State-Manager können Sie Abschnitte in der XAML-Datei enthalten, die die visuelle Darstellung einer Sicht ändern können, wenn die Ansicht normal oder deaktiviert ist oder den Eingabefokus besitzt. Diese werden als bezeichnet die _häufige Zustände_.

Nehmen wir beispielsweise an, Sie haben eine `Entry` Ansicht auf der Seite und möchten, dass die visuelle Darstellung der `Entry` auf folgende Weise ändern:

- Die `Entry` müssen einen rosa Hintergrund, wenn die `Entry` ist deaktiviert.
- Die `Entry` Gelbgrün Hintergrund sollte normalerweise haben.
- Die `Entry` doppelt normale Höhe erweitern soll, wenn es den Eingabefokus besitzt.

Sie können das Markup VSM anfügen, um eine einzelne Ansicht aus, oder Sie können es in einem Stil definieren, trifft auf mehrere Ansichten. In den nächsten beiden Abschnitten werden diese Ansätze beschrieben.

### <a name="vsm-markup-on-a-view"></a>VSM-Markup für eine Sicht

VSM-Markup zum Anfügen einer `Entry` anzuzeigen, trennen Sie zunächst die `Entry` in Start- und Endtags:

```xaml
<Entry FontSize="18">

</Entry>
```

Er erhält eine explizite Schriftgrad, da es sich bei einem der Zustände verwenden die `FontSize` Eigenschaft auf das Doppelte der Größe des Texts in der `Entry`.

Fügen Sie als Nächstes `VisualStateManager.VisualStateGroups` Tags zwischen diesen Tags:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) ist eine angefügte bindbare Eigenschaft definiert, durch die [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) Klasse. (Weitere Informationen zu angefügten bindbare Eigenschaften finden Sie im Artikel [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).) Dies ist die `VisualStateGroups` Eigenschaft angefügt ist die `Entry` Objekt.

Die `VisualStateGroups` Eigenschaft ist vom Typ [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList), dies ist eine Auflistung von [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) Objekte. In der `VisualStateManager.VisualStateGroups` Tags, fügen Sie ein Paar von `VisualStateGroup` Tags für jede Gruppe von visuellen Zuständen, die Sie einschließen möchten:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

Beachten Sie, dass die `VisualStateGroup` Tag verfügt über eine `x:Name` Attributs angegeben wird, den Namen der Gruppe. Die `VisualStateGroup` -Klasse definiert eine `Name` -Eigenschaft, die Sie stattdessen verwenden können:

```xaml
<VisualStateGroup Name="CommonStates">
```

Verwenden Sie entweder `x:Name` oder `Name` jedoch nicht beide im selben Element.

Die `VisualStateGroup` -Klasse definiert eine Eigenschaft namens [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States), dies ist eine Auflistung von [ `VisualState` ](xref:Xamarin.Forms.VisualState) Objekte. `States` ist die _Inhaltseigenschaft_ von `VisualStateGroups` einzuschließen der `VisualState` tags direkt zwischen den `VisualStateGroup` Tags. (Content-Eigenschaften werden in diesem Artikel erläuterten [Essential XAML Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

Der nächste Schritt ist ein Paar von Tags für jedes visuellen Zustand in diese Gruppe eingeschlossen werden sollen. Diese auch können Sie anhand `x:Name` oder `Name`:

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

`VisualState` definiert eine Eigenschaft namens [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters), dies ist eine Auflistung von [ `Setter` ](xref:Xamarin.Forms.Setter) Objekte. Diese entsprechen `Setter` Objekte, die auf eine [ `Style` ](xref:Xamarin.Forms.Style) Objekt.

`Setters` ist _nicht_ die Inhaltseigenschaft `VisualState`, daher wird zum Einschließen von Eigenschaftenelement-Tags für die `Setters` Eigenschaft:

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

Sie können nun einfügen, eine oder mehrere `Setter` Objekte zwischen jedem Tabellenpaar `Setters` Tags. Dies sind die `Setter` Objekte, die die visuellen Zustände, die zuvor beschriebenen definieren:

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

Jede `Setter` Tag gibt den Wert einer bestimmten Eigenschaft an, wenn dieser Zustand ist. Jede Eigenschaft, die auf eine `Setter` Objekt muss durch eine bindbare Eigenschaft unterstützt werden.

Markup etwa wie folgt, ist die Grundlage für die **VSM %.ls'-Sicht** auf der Seite die **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** -Beispielprogramm. Die Seite enthält drei `Entry` Ansichten, jedoch nur das zweite Argument ist das VSM Markup angefügt ist:

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

Beachten Sie, dass die zweite `Entry` verfügt auch über eine `DataTrigger` als Teil seiner `Trigger` Auflistung. Dies bewirkt, dass die `Entry` deaktiviert werden soll, bis etwas in die dritte eingegeben wird `Entry`. So sieht die Seite beim Start, die unter iOS, Android und die universelle Windows-Plattform (UWP):

[![VSM in Ansicht: Deaktiviertes](vsm-images/VsmOnViewDisabled.png "VSM in Ansicht-deaktiviert")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

Der aktuelle visuelle Zustand ist "Disabled" also den Hintergrund des zweiten `Entry` wird auf IOS- und Android-Bildschirme rosa. Die UWP-Implementierung von `Entry` lässt kein Festlegen von Hintergrund Farbe, wenn die `Entry` ist deaktiviert. 

Wenn Sie Text eingeben, in die dritte `Entry`, die zweite `Entry` Schalter in den Zustand "Normal", und der Hintergrund ist jetzt Gelbgrün:

[![VSM in Ansicht: Normales](vsm-images/VsmOnViewNormal.png "VSM in Ansicht (normal") )](vsm-images/VsmOnViewNormal-Large.png#lightbox)

Wenn Sie die zweite berühren `Entry`, es den Eingabefokus erhält. Er wechselt in den Zustand "Fokussiert" und zweimal Höhe erweitert:

[![VSM in Ansicht: ](vsm-images/VsmOnViewFocused.png "VSM mit Fokus im") Fokus](vsm-images/VsmOnViewFocused-Large.png#lightbox)

Beachten Sie, dass die `Entry` Gelbgrün Hintergrund wird nicht beibehalten werden, wenn es den Eingabefokus erhält. Wie der Visual State Manager zwischen visuellen Zuständen wechselt, sind die Eigenschaften, die festlegen, indem der vorherige Status nicht festgelegt. Denken Sie daran, die das visuelle Zustände schließen sich gegenseitig aus. Der Zustand "Normal" bedeutet nicht nur, dass die `Entry` aktiviert ist. Dies bedeutet, dass die `Entry` aktiviert ist und keine Eingabefokus. 

Wenn Sie möchten die `Entry` um einen Gelbgrün-Hintergrund im Zustand "Fokussiert" haben, fügen Sie ein weiteres `Setter` für diesen visuellen Status:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

In der Reihenfolge für diese `Setter` Objekte ordnungsgemäß ausgeführt werden, eine `VisualStateGroup` darf `VisualState` Objekte für alle Bundesstaaten in der Gruppe. Wenn es gibt ein visuellen Zustand, der keinen `Setter` Objekte, fügen Sie es dennoch als leeres Tag:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Visueller Zustand-Manager-Markup in einem Stil

Es ist häufig erforderlich, die das gleiche Visual State-Manager-Markup zwischen zwei oder mehreren Ansichten gemeinsam nutzen. In diesem Fall möchten Sie das Markup zu platzieren, in einem `Style` Definition.

Hier ist der implizite `Style` für die `Entry` Elemente in der **VSM auf Ansicht** Seite:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

Hinzufügen `Setter` tags für die `VisualStateManager.VisualStateGroups` bindbare Eigenschaft angefügt:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

Die Content-Eigenschaft für `Setter` ist `Value`, sodass der Wert des der `Value` Eigenschaft kann direkt in diese Tags angegeben werden. Eigenschaft des Typs ist `VisualStateGroupList`:

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

Innerhalb dieser Tags können Sie einem oder mehreren einschließen `VisualStateGroup` Objekte:

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

Hier ist die **VSM im Stil** Seite zeigt die vollständige VSM-Markup:

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

Jetzt alle die `Entry` Sichten auf dieser Seite die gleiche Weise auf ihre visuellen Zuständen zu reagieren. Beachten Sie, dass der Zustand "Fokussiert" jetzt eine zweite enthält `Setter` , die erhält jeder `Entry` eine lindgrüne Hintergrund auch, wenn es den Eingabefokus besitzt:

[![VSM im Stil](vsm-images/VsmInStyle.png "VSM im Format")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>Definieren eigener visueller Zustände

Jede abgeleitete Klasse `VisualElement` unterstützt die drei allgemeinen Status "Normal", "Mit Fokus" und "Disabled". Intern wird die [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) Klasse erkennt, wenn es aktiviert oder deaktiviert ist, oder mit Fokus oder ohne Fokus immer wird, und die statische ruft [ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) Methode:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

Dies ist der einzige Visual State-Manager-Code, die Sie in finden der `VisualElement` Klasse. Da `GoToState` für jedes Objekt, das basierend auf jede abgeleitete Klasse heißt `VisualElement`, können Sie mit der Visual State Manager `VisualElement` Objekt auf diese Änderungen reagieren.

Interessanterweise, der Namen des der visuellen Zustandsgruppe "CommonStates" wird nicht explizit verwiesen `VisualElement`. Der Gruppenname ist nicht Teil der API für den visuellen Status-Manager. In einem der zwei-Beispielprogramm, das bisher gezeigt können Sie den Namen der Gruppe aus "CommonStates" zu einem anderen ändern, und das Programm weiterhin funktionieren. Der Gruppenname ist lediglich eine allgemeine Beschreibung der Zustände in der Gruppe. Es ist implizit zu verstehen, dass die visuellen Zustände in jeder Gruppe sich gegenseitig ausschließen: Ein Status und jeweils nur ein Status sind aktuell.

Wenn Sie eigene visuelle Zustände implementieren möchten, müssen Sie zum Aufrufen `VisualStateManager.GoToState` von Code. Den meisten Fällen stellen Sie diesen Aufruf aus der CodeBehind-Datei der Page-Klasse.

Die **VSM Überprüfung** auf der Seite die **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** Beispiel wird gezeigt, wie Sie mit der Visual State-Manager im Zusammenhang mit der Validierung von Benutzereingaben. Die XAML-Datei besteht aus zwei `Label` Elemente ein `Entry`, und `Button`:

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

VSM Markup angefügt ist, mit dem zweiten `Label` (mit dem Namen `helpLabel`) und die `Button` (mit dem Namen `submitButton`). Es gibt zwei sich gegenseitig ausschließende Zuständen, mit dem Namen "Valid" und "Ungültig". Beachten Sie, dass jede der beiden Gruppen "ValidationState" enthält `VisualState` tags für "Valid" und "Ungültig", auch wenn einer dieser Datensätze in jedem Fall leer ist. 

Wenn die `Entry` enthält keine gültige Telefonnummer an, und klicken Sie dann der aktuelle Status "Ungültig" ist, und damit die zweite `Label` sichtbar ist und die `Button` deaktiviert ist:

[![VSM-Überprüfung: Ungültige]Zustands(vsm-images/VsmValidationInvalid.png "-VSM-Überprüfung-ungültig")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

Wenn eine gültige Telefonnummer eingegeben wird, klicken Sie dann der aktuelle Status ist "Gültig". Die zweite `Entry` nicht mehr angezeigt wird und die `Button` ist jetzt aktiviert:

[![VSM-Überprüfung: Gültige](vsm-images/VsmValidationValid.png "VSM-Zustands Überprüfung-gültig")](vsm-images/VsmValidationValid-Large.png#lightbox)

Die Code-Behind-Datei wird ermittelt, für die Behandlung der `TextChanged` Ereignis aus der `Entry`. Der Handler verwendet einen regulären Ausdruck, um festzustellen, ob die Eingabezeichenfolge gültig ist. Die Methode in der CodeBehind-Datei mit dem Namen `GoToState` Ruft die statische `VisualStateManager.GoToState` -Methode für beide `helpLabel` und `submitButton`:

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

Beachten Sie auch, dass die `GoToState` Methode wird vom Konstruktor initialisiert den Zustand aufgerufen. Es sollte stets aktuellen Status vorhanden sein. Aber gar nicht im Code gibt es alle Verweise auf den Namen der visuellen Zustandsgruppe, obwohl es in die XAML als "ValidationStates" Gründen der Klarheit verwiesen wird. 

Beachten Sie, dass die Code-Behind-Datei berücksichtigen muss jedes Objekt auf der Seite, die betroffen sind diese visuelle Zustände, und rufen Sie `VisualStateManager.GoToState` für jedes dieser Objekte. In diesem Beispiel ist es nur zwei Objekte (die `Label` und `Button`), es kann jedoch einige weitere.

Vielleicht Fragen Sie sich Folgendes: Warum kann die Code-Behind-Datei nicht einfach direkt auf die Objekte zugreifen, wenn die Code Behind-Datei auf jedes Objekt auf der Seite verweisen muss, die von diesen visuellen Zuständen betroffen ist. Sie könnten sicherlich. Der Vorteil der Verwendung der VSM ist jedoch, dass Sie, wie visuelle Elemente steuern können in anderen Zustand vollständig in XAML, dadurch alle von den UI-Entwurf an einem Ort bleibt reagieren. Dadurch wird die visuelle Darstellung der Einstellung durch den Zugriff auf visuelle Elemente direkt aus der CodeBehind-vermieden.

Es kann verlockend sein, erwägen eine Ableitung von einer Klasse von `Entry` und vielleicht Definieren einer Eigenschaft, die Sie an eine externe Überprüfungsfunktion festlegen können. Die abgeleitete Klasse `Entry` kann dann aufrufen, die `VisualStateManager.GoToState` Methode. Dies funktioniert gut, aber nur, wenn die `Entry` das einzige Objekt, das die verschiedenen visuellen Zustände betroffen wurden. In diesem Beispiel eine `Label` und `Button` sind ebenfalls betroffen sein. Es gibt keine Möglichkeit für VSM Markup angefügt ein `Entry` zum Steuern von anderen Objekten auf der Seite und keine Möglichkeit, für VSM Markup auf andere Objekte, um eine Änderung in den visuellen Zustand aus einem anderen Objekt zu verweisen angefügt.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Mithilfe der Visual State-Manager für das adaptive layout

Eine Xamarin.Forms-die Anwendung, die auf einem Smartphone ausgeführt wird, in der Regel in ein Portrait oder Querformat Seitenverhältnis und eine Xamarin.Forms-Anwendung, die auf dem Desktop angezeigt werden kann, kann geändert werden, um viele verschiedene Größen und Seitenverhältnisse wird davon ausgegangen. Eine überlegt entworfene Anwendung möglicherweise den Inhalt für diese verschiedenen Formfaktoren Seite bzw. im Fenster unterschiedlich angezeigt. 

Diese Technik wird manchmal als bezeichnet _adaptive Layout_. Da adaptive Layout ausschließlich eines Programms visuellen Elemente beinhaltet, ist es eine ideale Anwendung des visuellen Zustands-Manager.

Ein einfaches Beispiel ist eine Anwendung, die eine kleine Sammlung von Schaltflächen angezeigt, die der Inhalt der Anwendung auswirken. Im Hochformat möglicherweise diese Schaltflächen in einer horizontalen Zeile oben auf der Seite angezeigt:

[![Adaptives VSM-Layout: Adaptive Layout für Hochformat ((vsm-images/VsmAdaptiveLayoutPortrait.png "VSM)-") Hochformat]](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

Im Querformat das Array von Schaltflächen möglicherweise auf einer Seite verschoben, und in einer Spalte angezeigt:

[![Adaptives VSM-Layout: Adaptive]Layout von Querformat ((vsm-images/VsmAdaptiveLayoutLandscape.png "VSM): quer") Format](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

Von oben nach unten wird das Programm auf die universelle Windows-Plattform, Android und iOS ausgeführt.

Die **VSM Adaptive Layout** auf der Seite die [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) Beispiel definiert eine Gruppe namens "OrientationStates" mit zwei Zuständen, mit dem Namen "Portrait" und "Landscape". (Ein komplexer Ansatz kann auf mehrere verschiedene Breiten von Seite bzw. im Fenster basieren.) 

VSM Markup tritt an vier Stellen in der XAML-Datei. Die `StackLayout` mit dem Namen `mainStack` enthält das Menü und den Inhalt, der ist eine `Image` Element. Dies `StackLayout` sollte eine vertikale Ausrichtung im Hochformat und eine horizontale Ausrichtung im Querformatmodus ausgeführt haben:

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

Die innere `ScrollView` mit dem Namen `menuScroll` und `StackLayout` mit dem Namen `menuStack` klicken Sie im Menü von Schaltflächen zu implementieren. Die Ausrichtung dieser Layouts ist entgegengesetzten von `mainStack`. Klicken Sie im Menü sollte im Hochformat horizontale und vertikale im Querformatmodus ausgeführt werden.

Der vierte Abschnitt VSM Markup wird in ein impliziter Stil für die Schaltflächen selbst. Dieses Markup wird `VerticalOptions`, `HorizontalOptions`, und `Margin` Eigenschaften, die spezifisch für die Ausrichtungen Portait und Querformat.

Im Code-Behind-Datei wird die `BindingContext` Eigenschaft `menuStack` implementieren `Button` Befehle aus, und fügt außerdem einen Handler, der die `SizeChanged` -Ereignis der Seite:

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

Die `SizeChanged` handleraufrufen `VisualStateManager.GoToState` für die beiden `StackLayout` und `ScrollView` Elemente, und klicken Sie anschließend durchläuft die untergeordneten Elemente des `menuStack` aufzurufende `VisualStateManager.GoToState` für die `Button` Elemente.

Es mag, als ob die Code-Behind-Datei kann Änderungen der bildschirmausrichtung direkt durch das Festlegen von Eigenschaften von Elementen in der XAML-Datei verarbeiten, aber der Visual State-Manager auf jeden Fall einen strukturierteren Ansatz ist. Alles, was die visuellen Elemente in der XAML-Datei gespeichert werden, in denen sie einfacher untersucht werden, verwalten und zu ändern.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visueller Zustand-Manager mit Xamarin.University

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Video zu xamarin. Forms 3,0 Visual State Manager**

## <a name="related-links"></a>Verwandte Links

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
