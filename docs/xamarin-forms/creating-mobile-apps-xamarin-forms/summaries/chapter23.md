---
title: Zusammenfassung der Kapitel 23. Trigger und Verhaltensweisen
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: f83a93fb5d101a7acd0a67903813dba1ba2bf8b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Zusammenfassung der Kapitel 23. Trigger und Verhaltensweisen

Trigger und Verhaltensweisen sind ähnlich, insofern, dass sie sowohl in XAML-Dateien um Element Interaktionen über die Verwendung von datenbindungen zu vereinfachen, und klicken Sie zum Erweitern der Funktionalität von XAML-Elementen verwendet werden soll. Trigger und Verhaltensweisen werden fast immer mit visual Benutzeroberflächenobjekten verwendet.

Zur Unterstützung von Triggern und Verhaltensweisen, beide `VisualElement` und `Style` unterstützen zwei Sammlungseigenschaften:

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) und [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) des Typs `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) und [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) des Typs `IList<Behavior>`

## <a name="triggers"></a>Trigger

Ein Trigger ist eine Bedingung (Änderung einer Eigenschaft oder das Auslösen eines Ereignisses), die sich in einer Antwort (eine andere Eigenschaft ändern oder Ausführen von Code) ergibt. Die `Triggers` Eigenschaft `VisualElement` und `Style` ist vom Typ `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) ist eine abstrakte Klasse, die von der vier versiegelte Klassen abgeleitet werden:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) für Antworten auf Grundlage von eigenschaftenänderungen
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) für Antworten, die basierend auf das Auslösen von Ereignissen
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) für Antworten, die basierend auf der datenbindungen
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) für Antworten, die basierend auf mehrere Trigger

Der Trigger ist immer auf das Element festgelegt, durch den Trigger, dessen Eigenschaft geändert wird.

### <a name="the-simplest-trigger"></a>Der einfachste trigger

Die [ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) -Klasse überprüft, ob eine Änderung eines Eigenschaftswerts und reagiert, indem Sie eine andere Eigenschaft desselben Elements festlegen.

`Trigger` definiert drei Eigenschaften:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) vom Typ `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) vom Typ `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) Der Typ `IList<SetterBase>`, die Content-Eigenschaft des `Trigger`

Darüber hinaus `Trigger` erfordert, dass die folgende Eigenschaft geerbt `TriggerBase` festgelegt werden:

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) um anzugeben, den Typ des Elements auf dem die `Trigger` angefügt ist

Die `Property` und `Value` bilden die Bedingung und die `Setters` Auflistung entspricht der Antwort. Wenn das angezeigte `Property` hat den Wert erkennbar `Value`, die `Setter` Objekte in der `Setters` Sammlung angewendet werden. Wenn die `Property` verfügt über einen anderen Wert der Setter werden entfernt. `Setter` definiert zwei Eigenschaften, die identisch mit der ersten beiden Eigenschaften sind `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) vom Typ `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) vom Typ `Object`

Die [ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) Beispiel wird veranschaulicht, wie eine `Trigger` angewendet ein `Entry` können erhöhen die Größe des der `Entry` über die `Scale` Eigenschaft bei der `IsFocused`Eigenschaft von der `Entry` ist `true`.

Dies ist jedoch nicht sehr häufig, die `Trigger` kann im Code festgelegt werden, als der [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) demonstriert.

Die [ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) Beispiel wird veranschaulicht, wie die `Trigger` kann festgelegt werden, einem `Style` anzuwendende mit mehreren `Entry` Elemente.

### <a name="trigger-actions-and-animations"></a>Triggeraktionen und Animationen

Es ist auch möglich, ein wenig Code basierend auf einem Trigger auszuführen. Dieser Code kann es sich um eine Animation sein, die eine Eigenschaft als Ziel verwendet. Eine gebräuchliche Möglichkeit ist die Verwendung einer [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), die zwei Eigenschaften definiert:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) Der Typ `string`, den Namen eines Ereignisses
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) Der Typ `IList<TriggerAction>`, eine Liste von Aktionen als Reaktion ausgeführt.

Um dies zu verwenden, müssen Sie eine Klasse schreiben, die abgeleitet [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), im allgemeinen `TriggerAction<VisualElement>`. Sie können Eigenschaften in dieser Klasse definieren. Dies sind einfache CLR anstelle bindbare Eigenschaften, da `TriggerAction` nicht ableiten `BindableObject`. Müssen Sie überschreiben die [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) -Methode, die aufgerufen wird, wenn die Aktion aufgerufen wird. Das Argument ist das Zielelement an.

Die [ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek ist ein Beispiel. Ruft die `ScaleTo` zu animierende Eigenschaft der `Scale` Eigenschaft eines Elements. Da eine seiner Eigenschaften des Typs ist `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) Klasse können Sie mit den Standard `Easing` statische Felder in XAML.

Die [ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) Beispiel veranschaulicht das Aufrufen der `ScaleAction` aus `EventTrigger` Objekte zur Überwachung der `Focused` und `Unfocused` Ereignisse.

Die [ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) Beispiel wird gezeigt, wie Sie einen benutzerdefinierten Beschleunigungsfunktion für definieren `ScaleAction` in einer Code-Behind-Datei.

Sie können auch mithilfe von Aktionen Aufrufen einer `Trigger` (distinguished aus `EventTrigger`). Dies erfordert, dass Sie berücksichtigen, `TriggerBase` definiert zwei Auflistungen:

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) vom Typ `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) vom Typ `IList<TriggerAction>`

Die [ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) Beispiel veranschaulicht, wie diese Auflistungen.

### <a name="more-event-triggers"></a>Weitere Ereignistriggern

Die [ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Aufrufe `ScaleTo` zweimal, um nach oben oder unten skalieren. Die [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) Beispiel verwendet diese in eine formatierte `EventTrigger` visuelles Feedback bereitstellen. wenn eine `Button` gedrückt wird. Diese doppelte Animation ist auch möglich, mithilfe von zwei Aktionen in der Auflistung des Typs [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

Die [ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Library definiert eine anpassbare Shiver-Aktion. Die [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) demonstriert, die es.

Die [ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek ist beschränkt auf `Entry` Elemente und legt die `TextColor` -Eigenschaft auf Rot, wenn die `Text` Eigenschaft ist eine `double`. Die [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) demonstriert, die es.

### <a name="data-triggers"></a>Datentrigger

Die [ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) ähnelt der `Trigger` mit dem Unterschied, dass anstelle von Überwachen eine Eigenschaft für ändert, eine Datenbindung überwacht. Dies ermöglicht eine Eigenschaft in ein Element, um eine Eigenschaft in ein anderes Element zu beeinflussen.

`DataTrigger` definiert drei Eigenschaften:

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) vom Typ `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) vom Typ `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) vom Typ `IList<SetterBase>`

Die [ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) Beispiel erfordert die [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) -Bibliothek und legt die Farben der Namen der Studenten Blau oder Folgendes ein: rosa basierend auf den `Sex` Eigenschaft:

[![Dreifacher Screenshot des Geschlecht Farben](images/ch23fg04-small.png "Geschlecht Farben")](images/ch23fg04-large.png#lightbox "Geschlecht Farben")

Die [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) Beispiel legt die `IsEnabled` Eigenschaft ein `Entry` auf `False` Wenn die `Length` Eigenschaft der `Text` Eigenschaft von der `Entry`gleich 0 ist. Beachten Sie, dass die `Text` -Eigenschaft wird initialisiert, um eine leere Zeichenfolge; der Standardwert ist `null`, und die `DataTrigger` wäre nicht voll funktionsfähig.

### <a name="combining-conditions-in-the-multitrigger"></a>Kombinieren von Bedingungen in der MultiTrigger

Die [ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) ist eine Auflistung von Bedingungen. Wenn sie alle sind `true`, und klicken Sie dann Settern für Eigenschaften angewendet werden. Die Klasse definiert zwei Eigenschaften:

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) vom Typ `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) vom Typ `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) ist eine abstrakte Klasse, und verfügt über zwei untergeordnete Klassen:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), die [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) und [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) Eigenschaften, z. B. `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), die [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) und [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) Eigenschaften, z. B. `DataTrigger`

In der [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) Beispiel wird eine `BoxView` ist nur bei vier gefärbt `Switch` Elemente sind alle aktiviert.

Die [ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) Beispiel wird veranschaulicht, wie Sie vornehmen können eine `BoxView` eine Farbe beim *alle* vier `Switch` Elemente aktiviert sind. Dies erfordert eine Anwendung von De-Morgan Recht und die gesamte Logik umkehren.

Kombinieren von AND und oder Logik ist nicht so einfach und in der Regel erfordert unsichtbar `Switch` Elemente für Zwischenergebnisse. Die [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) Beispiel wird veranschaulicht, wie eine `Button` kann aktiviert werden, wenn entweder zweier `Entry` Elemente haben Text eingegeben, aber nicht, wenn beide Text eingegeben.

## <a name="behaviors"></a>Verhalten

Sie können keine Wirkung mit einem Trigger, Sie können auch mit einem Verhalten durchführen, aber Verhalten erfordern immer eine Klasse, die abgeleitet [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) und überschreibt die folgenden zwei Methoden:

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

Das Argument ist das Element, dem das Verhalten angefügt ist. Im Allgemeinen die `OnAttachedTo` Methode fügt einige Ereignishandler und `OnDetachingFrom` getrennt werden. Da eine solche Klasse ein Zustand, der in der Regel gespeichert wird, es in der Regel kann nicht freigegeben werden einer `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) Beispiel ähnelt **TriggerEntryValidation** identisch, jedoch ein Verhalten verwendet &mdash; der [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

### <a name="behaviors-with-properties"></a>Verhalten mit Eigenschaften

`Behavior<T>` leitet sich von `Behavior`, die sich daraus ableitet `BindableObject`, sodass ein Verhalten bindbare Eigenschaften definiert werden können. Diese Eigenschaften können im datenbindungen aktiv sein.

Dies wird dargestellt, der [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) Programm, macht der verwenden die [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) -Klasse in der [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. `ValidEmailBehavior` verfügt über eine bindbare Eigenschaft schreibgeschützt, und dient als Quelle für datenbindungen.

Die [ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) Beispiel wird dieses Verhalten verwendet, zum Anzeigen von einem anderen Typ des Indikators, um zu signalisieren, dass eine e-Mail-Adresse gültig ist.

Die [ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) Beispiel ist eine Variation auf das vorherige Beispiel. ButtonGlide verwendet eine `DataTrigger` in Kombination mit diesem Verhalten.

### <a name="toggles-and-check-boxes"></a>Schaltet und Kontrollkästchen

Es ist möglich, wie z. B. das Verhalten einer Umschaltfläche in einer Klasse zu kapseln [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek, und definieren Sie alle die visuellen Elemente für die Umschaltfläche vollständig in XAML.

Die [ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) -Beispiel verwendet die `ToggleBehavior` mit einer `DataTrigger` verwenden eine `Label` mit zwei Textzeichenfolgen für die Umschaltfunktion nachverfolgten.

Die [ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) Beispiel erweitert dieses Konzept, durch den Wechsel zwischen zwei `FormattedString` Objekte.

Die [ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) -Klasse in der **Xamarin.FormsBook.Toolkit** Bibliothek leitet sich von `ContentView`, definiert ein `IsToggled` -Eigenschaft, und eine `ToggleBehavior` für die Umschaltfläche Logik. Dies erleichtert es, definieren die Umschaltfläche in XAML, wie zeigt die [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) Beispiel.

Die [ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) enthält eine [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) von abgeleitete Klasse `ToggleBase` und verwendet eine [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Klasse so erstellen Sie eine Umschaltfläche, die die Xamarin.Forms ähnelt `Switch`.

Ein [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) in der **Xamarin.FormsBook.Toolkit** bietet eine Animation verwendet, um eine animierte Hebel in stellen die [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)Beispiel.

### <a name="responding-to-taps"></a>Reagieren auf datenabzweigungen

Ein Nachteil der `EventTrigger` ist, dass Sie sie verknüpfen können keiner `TapGestureRecognizer` auf Klicks reagiert. Umgehung dieses Problems ist der Zweck des [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) in der [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

Die [ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) -Beispiel verwendet `TapBehavior` mit den früheren `ShiverAction` für getippt `BoxView` Elemente.

Die [ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) Beispiel wird gezeigt, wie durch das kapselt das Markup verringern einer `ShiverView` Klasse.

### <a name="radio-buttons"></a>Optionsfelder

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek verfügt auch über eine [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) Klasse Geschäftsgründen Optionsfelder, die gruppiert werden eine `string` Gruppenname.

Die [ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) Textzeichenfolgen für dessen Optionsfeld verwendet. Die [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) -Beispiel verwendet einen `Style` für den Unterschied zwischen den checked und unchecked-Schaltflächen aussehen. Die [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) Beispiel verwendet die geschachtelten Bilder für die Optionsfelder:

[![Dreifacher Screenshot der Sender Bilder](images/ch23fg17-small.png "Radio Schaltflächensymbole")](images/ch23fg17-large.png#lightbox "Radio Bilder")

Die [ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) Beispiel zeichnet herkömmlichen angezeigten Optionsfelder mit einem Punkt in einem Kreis.

### <a name="fades-and-orientation"></a>Ein- und Ausrichtung

Im abschließenden Beispiel [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) können Sie zwischen drei verschiedenen Farbauswahl-Ansichten mithilfe von Optionsfeldern wechseln. Drei Ansichten zur menüausblendung ein-und mit einem [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek.

Das Programm reagiert auch auf Änderungen in Hochformat-und Querformat mithilfe einer [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) in der **Xamarin.FormsBook.Toolkit** Bibliothek.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 23 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Kapitel 23-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Arbeiten mit Triggern](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms-Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
