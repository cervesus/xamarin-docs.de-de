---
title: Zusammenfassung der Kapitel 11. Die bindbare Infrastruktur
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 11. Die bindbare Infrastruktur'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: f9e3326c0f55469cfa84a019a674679d82dfc007
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054235"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Zusammenfassung der Kapitel 11. Die bindbare Infrastruktur

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

Jeder C#-Programmierer ist mit c# vertraut *Eigenschaften*. Eigenschaften enthalten eine *festgelegt* Accessor und/oder einen *erhalten* Accessor. Sie werden häufig aufgerufen *CLR-Eigenschaften* für die Common Language Runtime.

Xamarin.Forms definiert eine erweiterte Eigenschaftsdefinition, die mit dem Namen eine *bindbare Eigenschaft* gekapselt durch den [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Klasse und von unterstützt die [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)Klasse. Diese Klassen sind ähnliche, aber recht unterschiedliche: die `BindableProperty` wird verwendet, um die Eigenschaft selbst definieren `BindableObject` ähnelt `object` , es ist eine Basisklasse für Klassen, die bindbare Eigenschaften definieren.

## <a name="the-xamarinforms-class-hierarchy"></a>Die Xamarin.Forms-Klassenhierarchie

Die [ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) Beispiel verwendet Reflektion, um die Anzeige einer Klassenhierarchie von Xamarin.Forms und veranschaulichen der entscheidenden Rolle, die von den `BindableObject` in dieser Hierarchie. `BindableObject` leitet sich von `Object` und ist die übergeordnete Klasse von [ `Element` ](xref:Xamarin.Forms.Element) aus dem [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) abgeleitet wird. Dies ist die übergeordnete Klasse von [ `Page` ](xref:Xamarin.Forms.Page) und [ `View` ](xref:Xamarin.Forms.View), dies ist die übergeordnete Klasse von [ `Layout` ](xref:Xamarin.Forms.Layout):

[![Dreifacher Screenshot der Klassenhierarchie, die gemeinsame Nutzung](images/ch11fg01-small.png "Klasse Hierarchie Freigabe")](images/ch11fg01-large.png#lightbox "Klasse Hierarchie freigeben")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Einen Einblick in BindableObject und BindableProperty

In den Klassen, die von abgeleitet `BindableObject` viele CLR-Eigenschaften werden als "von bindbare Eigenschaften unterstützt werden" bezeichnet. Z. B. die [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft der `Label` Klasse ist eine CLR-Eigenschaft, aber die `Label` -Klasse definiert außerdem ein öffentliche statische schreibgeschützte-Feld, mit dem Namen [ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty) von Typ `BindableProperty`.

Eine Anwendung festlegen oder abrufen kann die `Text` Eigenschaft `Label` in der Regel oder die Anwendung kann festlegen, die `Text` durch Aufrufen der [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) definierte Methode `BindableObject` mit eine `Label.TextProperty` Argument. Auf ähnliche Weise kann eine Anwendung Abrufen des Werts der `Text` Eigenschaft durch Aufrufen der [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) -Methode, mit einer `Label.TextProperty` Argument. Dies wird veranschaulicht, durch die [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) Beispiel.

In der Tat die `Text` CLR-Eigenschaft wird mithilfe von vollständig implementiert die `SetValue` und `GetValue` definierten Methoden `BindableObject` in Verbindung mit der `Label.TextProperty` statische Eigenschaft.

`BindableObject` und `BindableProperty` bieten Unterstützung für:

- Bietet Standardwerte für Eigenschaften
- Speichern ihre aktuellen Werte
- Bereitstellen von Mechanismen zur Validierung von Eigenschaftswerten
- Aufrechterhaltung der Konsistenz zwischen verwandten Eigenschaften, die in einer einzelnen Klasse
- Reagieren auf Änderungen der Eigenschaften
- Auslösen von Benachrichtigungen, wenn eine Eigenschaft geändert wird, oder geändert wurde
- Die Datenbindung unterstützen
- Unterstützung von Stilen
- Unterstützung von dynamischen Ressourcen

Wenn eine Eigenschaft, die durch eine bindbare Eigenschaft ändert, unterstützt wird `BindableObject` löst eine [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis Identifizieren der geänderten Eigenschaft. Dieses Ereignis wird nicht ausgelöst, wenn die Eigenschaft auf den gleichen Wert festgelegt ist.

Einige Eigenschaften werden nicht durch die bindbare Eigenschaften und einige Klassen Xamarin.Forms unterstützt &mdash; wie z. B. `Span` &mdash; leiten Sie nicht von `BindableObject`. Nur eine abgeleitete Klasse `BindableObject` können bindbare Eigenschaften unterstützt, da `BindableObject` definiert die `SetValue` und `GetValue` Methoden.

Da `Span` nicht von abgeleitet `BindableObject`, keine seiner Eigenschaften &mdash; wie z. B. `Text` &mdash; verfügen über eine bindbare Eigenschaft. Deshalb ist eine `DynamicResource` festlegen für die `Text` Eigenschaft `Span` löst eine Ausnahme in der [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) Beispiel im vorherigen Kapitel. Die [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) Beispiel veranschaulicht die legen Sie eine dynamische Ressourcen im Code mithilfe der [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) definierte Methode `Element`. Das erste Argument ist ein Objekt des Typs `BindableProperty`.

Auf ähnliche Weise die [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) definierte Methode `BindableObject` verfügt über ein erstes Argument vom Typ `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definieren von bindbare Eigenschaften

Sie können Ihre eigenen bindbare Eigenschaften, die mit der statischen definieren [ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) Methode zum Erstellen eines statischen schreibgeschützten Felds vom Typ `BindableProperty`.

Dies wird veranschaulicht, der [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die Klasse leitet sich von `Label` und ermöglicht die Angabe einen Schriftgrad in Punkt. Es wird veranschaulicht, der [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) Beispiel.

Vier Argumente von der `BindableProperty.Create` Methode erforderlich sind:

- `propertyName`: Der Textname der Eigenschaft (die identisch mit den Namen der CLR-Eigenschaft)
- `returnType`: der Typ der CLR-Eigenschaft
- `declaringType`: der Typ der Klasse deklarieren die Eigenschaft
- `defaultValue`: der Wert der Eigenschaft standardmäßig

Da `defaultValue` ist vom Typ `object`, der Compiler muss in der Lage, den Standardwert zu bestimmen. Z. B. wenn die `returnType` ist `double`, `defaultValue` sollte z. B. 0,0, anstatt nur 0 festgelegt werden, oder die Typenkonflikt wird zur Laufzeit eine Ausnahme ausgelöst.

Es ist auch sehr häufig für eine bindbare Eigenschaft enthalten:

- `propertyChanged`: eine statische Methode wird aufgerufen, wenn der Wert der Eigenschaft geändert wird. Das erste Argument ist die Instanz der Klasse, dessen Eigenschaft geändert wurde.

Die anderen Argumente für `BindableProperty.Create` sind nicht so häufig:

- `defaultBindingMode`: im Zusammenhang mit der Datenbindung verwendet (siehe [ **Kapitel 16. Die Datenbindung**](chapter16.md))
- `validateValue`: ein Rückruf zu prüfen, ein gültiger Wert
- `propertyChanging`: ein Rückruf, um anzugeben, wenn die Eigenschaft geändert wird.
- `coerceValue`: einen Rückruf, der einen festgelegten Wert in einen anderen Wert umgewandelt werden soll.
- `defaultValueCreate`: ein Rückruf, der einen Standardwert zu erstellen, die zwischen Instanzen der Klasse (z. B. eine Sammlung) gemeinsam genutzt werden kann

### <a name="the-read-only-bindable-property"></a>Die schreibgeschützte bindbare Eigenschaft

Eine bindbare Eigenschaft kann schreibgeschützt sein. Erstellen eine nur-Lese bindbare Eigenschaft erfordert das Aufrufen der statischen Methode [ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) ein privates statisches schreibgeschützten Feld des Typs definieren [ `BindablePropertyKey` ](xref:Xamarin.Forms.BindablePropertyKey).

Definieren Sie anschließend auf die CLR-Eigenschaft `set` Accesor als `private` zum Aufrufen einer [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) -Überladung mit dem `BindablePropertyKey` Objekt. Dadurch wird verhindert, dass die Eigenschaft, die außerhalb der Klasse festgelegt wird.

Dies wird veranschaulicht, der [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) Klasse zur Verwendung in der [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) Beispiel.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 11 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Kapitel 11-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md)
