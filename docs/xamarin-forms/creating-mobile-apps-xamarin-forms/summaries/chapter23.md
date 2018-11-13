---
title: Zusammenfassung der Kapitel 23. Trigger und Verhaltensweisen
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 23. Trigger und Verhaltensweisen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 4bfa2bed7061e031c55ccbdb7f576aa02c17581a
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563991"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Zusammenfassung der Kapitel 23. Trigger und Verhaltensweisen

Trigger und Verhaltensweisen ähneln, insofern, dass sie sowohl in XAML-Dateien zur Vereinfachung der Element-Interaktionen über die Verwendung von datenbindungen und zum Erweitern der Funktionalität von XAML-Elementen verwendet werden sollen. Sowohl Trigger und Verhaltensweisen werden fast immer mit visual Benutzeroberflächenobjekten verwendet.

Trigger und Verhaltensweisen, unterstützen beide `VisualElement` und `Style` unterstützen zwei Sammlungseigenschaften:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) und [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers) des Typs `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) und [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors) des Typs `IList<Behavior>`

## <a name="triggers"></a>Trigger

Ein Trigger ist eine Bedingung aus (Ändern einer Eigenschaft oder das Auslösen eines Ereignisses), die zu einer Antwort (eine andere Änderung der Eigenschaft oder Ausführung des Codes) führt. Die `Triggers` Eigenschaft `VisualElement` und `Style` ist vom Typ `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) ist eine abstrakte Klasse, die von der vier versiegelte Klassen abgeleitet werden:

- [`Trigger`](xref:Xamarin.Forms.Trigger) für die Antworten anhand von eigenschaftenänderungen
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) für Antworten, die basierend auf das Auslösen von Ereignissen
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) für die Antworten anhand von datenbindungen
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) für Antworten, die basierend auf mehreren Triggern

Der Trigger ist immer auf das Element festgelegt, durch den Trigger, dessen Eigenschaft geändert wird.

### <a name="the-simplest-trigger"></a>Der einfachste trigger

Die [ `Trigger` ](xref:Xamarin.Forms.Trigger) Klasse überprüft, ob eine Änderung eines Eigenschaftswerts und reagiert durch eine andere Eigenschaft desselben Elements festlegen.

`Trigger` definiert drei Eigenschaften:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) Der Typ `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) Der Typ `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) Der Typ `IList<SetterBase>`, die Content-Eigenschaft `Trigger`

Darüber hinaus `Trigger` erfordert, dass die folgende Eigenschaft von geerbt `TriggerBase` festgelegt werden:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) an den Typ des Elements auf dem die `Trigger` angefügt ist

Die `Property` und `Value` bilden die Bedingung an, und die `Setters` Auflistung ist die Antwort. Wenn das angezeigte `Property` hat den Wert von `Value`, die `Setter` Objekte in der `Setters` Sammlung angewendet werden. Wenn die `Property` wurde von einem anderen Wert für die Setter werden entfernt. `Setter` definiert zwei Eigenschaften, die als die ersten beiden Eigenschaften sind `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) Der Typ `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) Der Typ `Object`

Die [ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) Beispiel wird veranschaulicht, wie eine `Trigger` angewendet ein `Entry` können erhöhen die Größe des der `Entry` über die `Scale` Eigenschaft bei der `IsFocused`Eigenschaft der `Entry` ist `true`.

Obwohl es nicht sehr häufig, die `Trigger` kann im Code festgelegt werden, als der [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) veranschaulicht.

Die [ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) Beispiel wird veranschaulicht, wie die `Trigger` kann festgelegt werden, einem `Style` Zuweisen von mehreren `Entry` Elemente.

### <a name="trigger-actions-and-animations"></a>Auslösen von Aktionen und Animationen

Es ist auch möglich, ein wenig Code basierend auf einem Trigger ausgeführt. Dieser Code kann es sich um eine Animation sein, die eine Eigenschaft ausgerichtet ist. Eine gängige Möglichkeit ist die Verwendung einer [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger), das die zwei Eigenschaften definiert:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) Der Typ `string`, den Namen eines Ereignisses
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) Der Typ `IList<TriggerAction>`, eine Liste der Aktionen, die als Reaktion ausgeführt.

Um dies zu verwenden, müssen Sie eine Klasse schreiben, die von abgeleitet [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1), in der Regel `TriggerAction<VisualElement>`. Sie können Eigenschaften in dieser Klasse definieren. Dies sind einfache CLR-Eigenschaften anstelle von bindbare Eigenschaften, da `TriggerAction` nicht abgeleitet `BindableObject`. Müssen Sie überschreiben die [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*) -Methode, die aufgerufen wird, wenn die Aktion aufgerufen wird. Das Argument ist der Target-Element.

Die [ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek ist ein Beispiel. Ruft die `ScaleTo` zu animierende Eigenschaft der `Scale` Eigenschaft eines Elements. Da eine der Eigenschaften des Typs ist `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) Klasse können Sie den Standard `Easing` statischen Feldern in XAML.

Die [ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) Beispiel veranschaulicht das Aufrufen der `ScaleAction` aus `EventTrigger` Objekte zur Überwachung der `Focused` und `Unfocused` Ereignisse.

Die [ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) Beispiel wird gezeigt, wie Sie definieren einen benutzerdefinierten Beschleunigungsfunktion für `ScaleAction` in einer CodeBehind-Datei.

Sie können auch Aktionen, die mit Aufrufen einer `Trigger` (distinguished auf `EventTrigger`). Dies erfordert, dass Ihnen bewusst ist, die `TriggerBase` definiert zwei Auflistungen:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) Der Typ `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) Der Typ `IList<TriggerAction>`

Die [ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) Beispiel veranschaulicht, wie diese Auflistungen verwenden.

### <a name="more-event-triggers"></a>Weitere-Ereignis ausgelöst

Die [ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliotheksaufrufe `ScaleTo` zweimal, um die zentral hoch-und Herunterskalieren. Die [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) Beispiel verwendet diese in eine formatierte `EventTrigger` von visuellem Feedback bei der ein `Button` gedrückt wird. Diese doppelte Animation ist auch möglich ist, verwenden zwei Aktionen in der Auflistung des Typs [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

Die [ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Library definiert eine anpassbare Shiver-Aktion. Die [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) veranschaulicht sie.

Die [ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek ist beschränkt auf `Entry` Elemente und legt die `TextColor` -Eigenschaft auf Rot, wenn die `Text` Eigenschaft ist keiner `double`. Die [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) veranschaulicht sie.

### <a name="data-triggers"></a>Datentrigger

Die [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) ist vergleichbar mit der `Trigger` mit dem Unterschied, dass anstelle der Überwachung einer Eigenschaft für Änderungen an, eine Datenbindung überwacht. Dadurch wird eine Eigenschaft in ein Element auf eine Eigenschaft in ein anderes Element auswirken.

`DataTrigger` definiert drei Eigenschaften:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) Der Typ `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) Der Typ `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) Der Typ `IList<SetterBase>`

Die [ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) Beispiel erfordert die [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) -Bibliothek und legt die Farben der Namen der Studenten in Blau oder Folgendes ein: rosa basierend auf den `Sex` Eigenschaft:

[![Dreifacher Screenshot Geschlecht Farben](images/ch23fg04-small.png "Geschlecht Farben")](images/ch23fg04-large.png#lightbox "Geschlecht Farben")

Die [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) Beispiel legt die `IsEnabled` Eigenschaft eine `Entry` zu `False` Wenn die `Length` Eigenschaft der `Text` Eigenschaft der `Entry`gleich 0 ist. Beachten Sie, dass die `Text` Eigenschaft ist auf eine leere Zeichenfolge initialisiert; standardmäßig ist dieser Wert `null`, und die `DataTrigger` wäre nicht ordnungsgemäß funktionieren.

### <a name="combining-conditions-in-the-multitrigger"></a>Kombinieren von Bedingungen in der MultiTrigger

Die [ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger) ist eine Auflistung von Bedingungen. Wenn sie alle sind `true`, und klicken Sie dann die Setter angewendet werden. Die Klasse definiert zwei Eigenschaften:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) Der Typ `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) Der Typ `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) ist eine abstrakte Klasse, und verfügt über zwei untergeordnete Klassen:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), die [ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property) und [ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value) Eigenschaften wie `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), die [ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding) und [ `Value` ](xref:Xamarin.Forms.BindingCondition.Value) Eigenschaften wie `DataTrigger`

In der [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) Beispiel einer `BoxView` ist nur dann gefärbt, wenn vier `Switch` Elemente sind alle aktiviert.

Die [ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) Beispiel wird verdeutlicht, wie Sie eine `BoxView` eine Farbe wenn *alle* von vier `Switch` Elemente aktiviert sind. Dies erfordert eine Anwendung De Morgans gesetzlichen Bestimmungen und die gesamte Logik umkehren.

Kombinieren von AND und oder Logik ist nicht ganz einfach und erfordert normalerweise, dass unsichtbar `Switch` Elemente für die Zwischenergebnisse. Die [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) Beispiel wird veranschaulicht, wie eine `Button` kann aktiviert werden, wenn eine von zwei `Entry` Elemente haben eingegebenen Text, jedoch nicht, wenn beide haben Text eingegeben.

## <a name="behaviors"></a>Verhalten

Sie können keine mit einem Trigger, Sie können auch mit einem Verhalten durchführen, aber Verhalten erfordern immer eine abgeleitete Klasse [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) und überschreibt die folgenden zwei Methoden:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

Das Argument ist das Element, dem das Verhalten zugeordnet ist. Im Allgemeinen die `OnAttachedTo` -Methode fügt einige Ereignishandler und `OnDetachingFrom` getrennt werden. Da eine solche Klasse in der Regel einen Zustand gespeichert, es in der Regel kann nicht freigegeben werden einem `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) Beispiel ähnelt **TriggerEntryValidation** , jedoch ein Verhalten verwendet &mdash; der [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

### <a name="behaviors-with-properties"></a>Verhalten mit Eigenschaften

`Behavior<T>` leitet sich von `Behavior`, die sich daraus ableitet `BindableObject`, sodass die bindbare Eigenschaften für ein Verhalten definiert werden können. Diese Eigenschaften können im datenbindungen aktiv sein.

Dies wird veranschaulicht, der [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) Programm, mit der, Verwendung von der [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) -Klasse in der [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. `ValidEmailBehavior` eine bindbare Eigenschaft schreibgeschützt ist, und dient als Quelle von datenbindungen.

Die [ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) Beispiel verwendet dieses Verhalten zum Anzeigen von einem anderen Typ des Indikators, um zu signalisieren, dass eine e-Mail-Adresse gültig ist.

Die [ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) Beispiel ist eine Abwandlung des vorherigen Beispiels. ButtonGlide verwendet eine `DataTrigger` in Kombination mit der dieses Verhalten.

### <a name="toggles-and-check-boxes"></a>Kontrollkästchen und schaltet

Es ist möglich, die das Verhalten einer Umschaltfläche in einer Klasse zu kapseln, wie z. B. [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek, und definieren Sie dann alle die visuellen Objekte für die Umschaltfunktion vollständig in XAML.

Die [ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) -Beispiel verwendet die `ToggleBehavior` mit einer `DataTrigger` verwenden eine `Label` mit zwei Textzeichenfolgen für die Umschaltfläche.

Die [ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) Beispiel erweitert dieses Konzept durch den Wechsel zwischen zwei `FormattedString` Objekte.

Die [ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek leitet sich von `ContentView`, definiert ein `IsToggled` -Eigenschaft, und umfasst eine `ToggleBehavior` für die Umschaltfunktion die Logik. Dies erleichtert es, definieren die Umschaltfläche in XAML, wie die [ **TraditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) Beispiel.

Die [ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) umfasst eine [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) abgeleitete Klasse `ToggleBase` und verwendet eine [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Klasse, um eine Umschaltfläche zu erstellen, die das Xamarin.Forms ähnelt `Switch`.

Ein [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) in die **Xamarin.FormsBook.Toolkit** bietet eine Animation verwendet, um eine animierte Hebel in die [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)Beispiel.

### <a name="responding-to-taps"></a>Auf tippen zu reagieren

Ein Nachteil bei der `EventTrigger` besteht darin, dass Sie sie anfügen können keine `TapGestureRecognizer` auf tippen zu reagieren. Das Problem umgehen, ist der Zweck der [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) in die [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

Die [ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) wird `TapBehavior` mit dem früheren `ShiverAction` für getippt `BoxView` Elemente.

Die [ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) Beispiel wird gezeigt, wie Sie im Markup durch das Kapseln von Verkürzen eine `ShiverView` Klasse.

### <a name="radio-buttons"></a>Optionsfelder

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek verfügt auch über eine [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) -Klasse für die Optionsfelder, die nach gruppiert sind, sodass eine `string` Gruppenname.

Die [ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) Textzeichenfolgen für dessen Optionsfeld verwendet. Die [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) -Beispiel verwendet einen `Style` für den Unterschied in der Darstellung zwischen checked und unchecked-Schaltflächen. Die [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) Beispiel verwendet eine geschachtelte Images für die Optionsfelder:

[![Dreifacher Screenshot Radio Images](images/ch23fg17-small.png "Radio Schaltflächenbilder")](images/ch23fg17-large.png#lightbox "Radio Schaltflächenbilder")

Die [ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) Beispiel zeichnet herkömmliche erscheinen Optionsfelder mit einem Punkt in einem Kreis.

### <a name="fades-and-orientation"></a>Wird ausgeblendet und-Ausrichtung

Das letzte Beispiel [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) können Sie zwischen drei verschiedenen Farbauswahl-Ansichten mithilfe von Optionsfeldern wechseln. Die drei Ansichten ein-und Abmelden verwenden eine [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) in die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

Die Anwendung reagiert auch auf Änderungen in Ausrichtung zwischen Hoch- und Querformat mit einer [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) in die **Xamarin.FormsBook.Toolkit** Bibliothek.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 23 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Kapitel 23-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Arbeiten mit Triggern](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms-Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
