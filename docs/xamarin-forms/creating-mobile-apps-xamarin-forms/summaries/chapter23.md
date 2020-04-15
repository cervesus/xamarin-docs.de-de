---
title: Zusammenfassung von Kapitel 23. Trigger und Verhalten
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 23. Trigger und Verhalten'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 8a1274a8447f49ce39f9c92703bbaec9e875b9e9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "70760592"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Zusammenfassung von Kapitel 23. Trigger und Verhalten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

Trigger und Verhalten sind dahingehend ähnlich, dass beide für die Verwendung in XAML-Dateien vorgesehen sind, um Elementinteraktionen über die Verwendung von Datenbindungen hinaus zu vereinfachen und um die Funktionalität von XAML-Elementen zu erweitern. Sowohl Trigger als auch Verhalten werden praktisch immer mit visuellen Benutzeroberflächenobjekten verwendet.

Um Trigger und Verhalten zu unterstützen, unterstützen sowohl `VisualElement` als auch `Style` zwei Sammlungseigenschaften:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) und [`Style.Triggers`](xref:Xamarin.Forms.Style.Triggers) vom Typ `IList<TriggerBase>`.
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) und [`Style.Behaviors`](xref:Xamarin.Forms.Style.Behaviors) vom Typ `IList<Behavior>`.

## <a name="triggers"></a>Trigger

Ein Trigger ist eine Bedingung (eine Eigenschaftsänderung oder das Auslösen eines Ereignisses), die zu einer Reaktion führt (eine andere Eigenschaftsänderung oder das Ausführen von Code). Die `Triggers`-Eigenschaft von `VisualElement` und `Style` ist vom Typ `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) ist eine abstrakte Klasse, von der vier versiegelte Klassen abgeleitet werden:

- [`Trigger`](xref:Xamarin.Forms.Trigger) für Reaktionen auf der Grundlage von Eigenschaftsänderungen.
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) für Reaktionen auf Grundlage von Ereignisauslösungen.
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) für Reaktionen auf Grundlage von Datenbindungen.
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) für Reaktionen auf der Grundlage mehrerer Trigger.

Der Trigger wird immer für das-Element festgelegt, dessen Eigenschaft durch den Trigger geändert wird.

### <a name="the-simplest-trigger"></a>Der einfachste Trigger

Die [`Trigger`](xref:Xamarin.Forms.Trigger)-Klasse überprüft, ob ein Eigenschaftswert geändert wird, und reagiert durch Festlegen einer anderen Eigenschaft desselben Elements.

`Trigger` definiert drei Eigenschaften:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) vom Typ `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) vom Typ `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) vom Typ `IList<SetterBase>`, die Inhaltseigenschaft (Content) von `Trigger`.

Außerdem erfordert `Trigger`, dass die folgende, von `TriggerBase` geerbte Eigenschaft festgelegt wird:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType), um den Typ des Elements anzugeben, an den der `Trigger` angefügt ist.

`Property` und `Value` bilden die Bedingung, und die `Setters`-Sammlung ist die Reaktion. Wenn die angezeigte `Property` den von `Value` angegebenen Wert hat, werden die `Setter`-Objekte in der `Setters`-Sammlung angewendet. Wenn `Property` einen anderen Wert aufweist, werden die Setter entfernt. `Setter` definiert zwei Eigenschaften, die mit den ersten beiden Eigenschaften von `Trigger` identisch sind:

- [`Property`](xref:Xamarin.Forms.Setter.Property) vom Typ `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) vom Typ `Object`

Im [**EntryPop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop)-Beispiel wird veranschaulicht, wie ein auf `Entry` angewendeter `Trigger` die Größe von `Entry` über die `Scale`-Eigenschaft erhöhen kann, wenn die `IsFocused`-Eigenschaft von `Entry` `true` ist.

Obwohl es nicht üblich ist, kann `Trigger` im Code festgelegt werden, wie das [**EntryPopCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode)-Beispiel veranschaulicht.

Das [**StyledTriggers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers)-Beispiel veranschaulicht, wie `Trigger` in einem `Style` festgelegt werden kann, um auf mehrere `Entry`-Elemente angewendet zu werden.

### <a name="trigger-actions-and-animations"></a>Triggeraktionen und Animationen

Es ist auch möglich, bei einem Trigger ein kleines Stück Code auszuführen. Dieser Code kann eine Animation sein, die auf eine Eigenschaft abzielt. Eine gängige Methode ist die Verwendung eines [`EventTrigger`](xref:Xamarin.Forms.EventTrigger), der zwei Eigenschaften definiert:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) vom Typ `string`, der Name eines Ereignisses.
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) vom Typ `IList<TriggerAction>`, eine Liste von als Reaktion auszuführenden Aktionen.

Um dies zu verwenden, müssen Sie eine Klasse schreiben, die von [`TriggerAction<T>`](xref:Xamarin.Forms.TriggerAction`1) abgeleitet wird, generell `TriggerAction<VisualElement>`. Sie können in dieser Klasse Eigenschaften definieren. Dabei handelt es sich eher um einfache CLR-Eigenschaften als um bindbare Eigenschaften, da `TriggerAction` nicht von `BindableObject` abgeleitet wird. Sie müssen die [`Invoke`](xref:Xamarin.Forms.TriggerAction`1.Invoke*)-Methode außer Kraft setzen, die aufgerufen wird, wenn die Aktion aufgerufen wird. Das Argument ist das Zielelement.

Die [`ScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ist ein Beispiel. Sie ruft die `ScaleTo`-Eigenschaft auf, um die `Scale`-Eigenschaft eines Elements zu animieren. Da eine ihrer Eigenschaften vom Typ `Easing` ist, können Sie mit der [`EasingConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs)-Klasse die statischen `Easing`-Standardfelder in XAML verwenden.

Das [**EntrySwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell)-Beispiel veranschaulicht, wie Sie die `ScaleAction` aus `EventTrigger`-Objekten aufrufen, die die Ereignisse `Focused` und `Unfocused` überwachen.

Das [**CustomEasingSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell)-Beispiel zeigt, wie Sie eine benutzerdefinierte Beschleunigungsfunktion für `ScaleAction` in einer CodeBehind-Datei definieren.

Sie können Aktionen auch mit einem `Trigger` aufrufen (zu unterscheiden von `EventTrigger`). Hierfür müssen Sie sich bewusst sein, dass `TriggerBase` zwei Sammlungen definiert:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) vom Typ `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) vom Typ `IList<TriggerAction>`

Im [**EnterExitSwell**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell)-Beispiel wird veranschaulicht, wie diese Sammlungen verwendet werden.

### <a name="more-event-triggers"></a>Weitere Ereignistrigger

Die [`ScaleUpAndDownAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ruft `ScaleTo` zweimal auf, um zentral hoch- und herunterzuskalieren. Das [**ButtonGrowth**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth)-Beispiel verwendet dieses in einem formatierten `EventTrigger`, um visuelles Feedback bereitzustellen, wenn auf einen `Button` gedrückt wird. Diese doppelte Animation lässt sich auch mit zwei Aktionen in der Sammlung vom Typ [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs) erzeugen.

Die [`ShiverAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs)-Klasse in der Bibliothek **Xamarin.FormsBook.Toolkit** definiert eine anpassbare Zitternaktion (Shiver). Dies wird im [**ShiverButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo)-Beispiel veranschaulicht.

Die [`NumericValidationAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs)-Klasse in der Bibliothek **Xamarin.FormsBook.Toolkit** ist auf `Entry`-Elemente beschränkt und legt die `TextColor`-Eigenschaft auf „Rot“ fest, wenn die `Text`-Eigenschaft nicht vom Typ `double` ist. Dies wird im [**TriggerEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation)-Beispiel veranschaulicht.

### <a name="data-triggers"></a>Datentrigger

[`DataTrigger`](xref:Xamarin.Forms.DataTrigger) ähnelt dem `Trigger`, mit der Ausnahme, dass keine Eigenschaft auf Wertänderungen überwacht wird, sondern eine Datenbindung. Dadurch kann eine Eigenschaft in einem Element eine Eigenschaft in einem anderen Element beeinflussen.

`DataTrigger` definiert drei Eigenschaften:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) vom Typ `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) vom Typ `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) vom Typ `IList<SetterBase>`

Das [**GenderColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors)-Beispiel erfordert die Bibliothek [**SchoolOfFineArt**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) und legt die Farben der Namen der Kursteilnehmer auf „Blau“ oder „Rosa“ fest, basierend auf ihrer `Sex`-Eigenschaft:

[![Dreifacher Screenshot von Farben für Geschlechter](images/ch23fg04-small.png "Farben für Geschlechter")](images/ch23fg04-large.png#lightbox "Farben für Geschlechter")

Im [**ButtonEnabler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler)-Beispiel wird die `IsEnabled`-Eigenschaft eines `Entry` auf `False` festgelegt, wenn die `Length`-Eigenschaft der `Text`-Eigenschaft von `Entry` gleich 0 ist. Beachten Sie, dass die `Text`-Eigenschaft mit einer leeren Zeichenfolge initialisiert wird. Der Standardwert ist `null`, und der `DataTrigger` würde nicht richtig funktionieren.

### <a name="combining-conditions-in-the-multitrigger"></a>Kombinieren von Bedingungen im „MultiTrigger“

[`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) ist eine Sammlung von Bedingungen. Wenn sie alle `true` sind, werden Setter angewendet. Die Klasse definiert zwei Eigenschaften:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) vom Typ `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) vom Typ `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) ist eine abstrakte Klasse und verfügt über zwei untergeordnete Klassen:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), die [`Property`](xref:Xamarin.Forms.PropertyCondition.Property)- und [`Value`](xref:Xamarin.Forms.PropertyCondition.Value)-Eigenschaften wie `Trigger` besitzt.
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), die [`Binding`](xref:Xamarin.Forms.BindingCondition.Binding)- und [`Value`](xref:Xamarin.Forms.BindingCondition.Value)-Eigenschaften wie `DataTrigger` besitzt.

Im [**AndConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions)-Beispiel wird eine `BoxView` nur dann eingefärbt, wenn vier `Switch`-Elemente alle aktiviert sind.

Im [**OrConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions)-Beispiel wird veranschaulicht, wie Sie eine `BoxView` einfärben können, wenn *beliebige* der vier `Switch`-Elemente aktiviert sind. Hierfür muss das De Morgansche Gesetz angewendet und die gesamte Logik umgekehrt werden.

Das Kombinieren von AND- und OR-Logik ist nicht ganz einfach und erfordert in der Regel unsichtbare `Switch`-Elemente für Zwischenergebnisse. Das [**XorConditions**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions)-Beispiel veranschaulicht, wie ein `Button` aktiviert werden kann, wenn in eins der beiden `Entry`-Elemente ein Text eingegeben wird, aber nicht, wenn in beide Text eingegeben wird.

## <a name="behaviors"></a>Verhalten

Alles, was Sie mit einem Trigger machen können, können Sie auch mit einem Verhalten erreichen, aber Verhalten erfordern immer eine Klasse, die von [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) abgeleitet wird und die folgenden zwei Methoden außer Kraft setzt:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

Das Argument ist das-Element, an das das Verhalten angefügt ist. Generell fügt die `OnAttachedTo`-Methode einige Ereignishandler an, und `OnDetachingFrom` trennt diese. Da eine solche Klasse normalerweise einen Zustand speichert, kann Sie generell nicht in einem `Style` gemeinsam genutzt werden.

Das [**BehaviorEntryValidation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation)-Beispiel ähnelt dem **TriggerEntryValidation**-Beispiel, mit der Ausnahme, dass es ein Verhalten verwendet – die [`NumericValidationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

### <a name="behaviors-with-properties"></a>Verhalten mit Eigenschaften

`Behavior<T>` wird von `Behavior` abgeleitet, das von `BindableObject` abgeleitet wird, weshalb für ein Verhalten bindbare Eigenschaften definiert werden können. Diese Eigenschaften können in Datenbindungen aktiv sein.

Dieser Vorgang wird im [**EmailValidationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo)-Programm veranschaulicht, das die [`ValidEmailBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs)-Klasse in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) verwendet. `ValidEmailBehavior` verfügt über eine schreibgeschützte, bindbare Eigenschaft und fungiert als Quelle in Datenbindungen.

Das [**EmailValidationConv**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv)-Beispiel verwendet dieses selbe Verhalten zum Anzeigen eines anderen Typs von Indikator, um zu signalisieren, dass eine E-Mail-Adresse gültig ist.

Das [**EmailValidationTrigger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger)-Beispiel ist eine Abwandlung des vorherigen Beispiels. „ButtonGlide“ verwendet einen `DataTrigger` in Kombination mit diesem Verhalten.

### <a name="toggles-and-check-boxes"></a>Umschaltflächen und Kontrollkästchen

Es ist möglich, das Verhalten einer Umschaltfläche in einer Klasse wie [`ToggleBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) zu kapseln und dann alle visuellen Elemente für die Umschaltfläche vollständig in XAML zu definieren.

Im [**ToggleLabel**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel)-Beispiel wird das `ToggleBehavior` mit einem `DataTrigger` verwendet, um ein `Label` mit zwei Textzeichenfolgen für die Umschaltfläche zu verwenden.

Das [**FormattedTextToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle)-Beispiel erweitert dieses Konzept, indem es zwischen zwei `FormattedString`-Objekten wechselt.

Die [`ToggleBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs)-Klasse in der Bibliothek **Xamarin.FormsBook.Toolkit** wird von `ContentView` abgeleitet, definiert eine `IsToggled`-Eigenschaft und enthält ein `ToggleBehavior` für die Umschaltlogik. Dies erleichtert das Definieren der Umschaltfläche in XAML, wie im [**TraditionalCheckBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox)-Beispiel veranschaulicht.

Das [**SwitchCloneDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo)-Beispiel enthält eine [`SwitchClone`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs)-Klasse, die von `ToggleBase` abgeleitet wird und eine [`TranslateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)-Klasse verwendet, um eine Umschaltfläche zu erstellen, die dem `Switch` von Xamarin.Forms ähnelt.

Eine [`RotateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) in der Bibliothek **Xamarin.FormsBook.Toolkit** stellt eine Animation bereit, die verwendet wird, um einen animierten Hebel im [**LeverToggle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)-Beispiel zu erstellen.

### <a name="responding-to-taps"></a>Reagieren auf Tippen

Ein Nachteil von `EventTrigger` besteht darin, dass Sie ihn nicht an einen `TapGestureRecognizer` anfügen können, um auf Tippen zu reagieren. Die Umgehung dieses Problems ist der Zweck von [`TapBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

Das [**BoxViewTapShiver**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver)-Beispiel verwendet `TapBehavior`, um die frühere `ShiverAction` für `BoxView`-Elemente zu verwenden, auf die getippt wurde.

Das [**ShiverViews**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews)-Beispiel zeigt, wie sich das Markup durch Kapseln einer `ShiverView`-Klasse verringern lässt.

### <a name="radio-buttons"></a>Optionsfelder

Die Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) enthält auch eine [`RadioBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs)-Klasse zum Erstellen von Optionsfeldern, die mithilfe eines `string`-Gruppennamens gruppiert werden.

Das [**RadioLabels**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels)-Programm verwendet Textzeichenfolgen für seine Optionsfelder. Das [**RadioStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle)-Beispiel verwendet einen `Style` für den Darstellungsunterschied zwischen aktivierten und deaktivierten Schaltflächen. Das [**RadioImages**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages)-Beispiel verwendet Bilder mit Rahmen für seine Optionsfelder:

[![Dreifacher Screenshot von Optionsfeldbildern](images/ch23fg17-small.png "Optionsfeldbilder")](images/ch23fg17-large.png#lightbox "Optionsfeldbilder")

Im [**TraditionalRadios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios)-Beispiel werden traditionell aussehende Optionsfelder mit einem Punkt in einem Kreis gezeichnet.

### <a name="fades-and-orientation"></a>Blenden und Ausrichtung

Das letzte Beispiel, [**MultiColorSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders), gestattet es Ihnen, mithilfe von Optionsfeldern zwischen drei verschiedenen Farbauswahlansichten zu wechseln. Die drei Ansichten werden mithilfe einer [`FadeEnableAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) in der Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ein- und ausgeblendet.

Das Programm reagiert auch auf Änderungen der Ausrichtung zwischen Hochformat und Querformat mithilfe eines [`GridOrientationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) in der Bibliothek **Xamarin.FormsBook.Toolkit**.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 23 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Kapitel 23 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Arbeiten mit Triggern](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms-Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
