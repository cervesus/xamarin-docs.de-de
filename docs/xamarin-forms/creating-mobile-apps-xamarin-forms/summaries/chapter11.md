---
title: Zusammenfassung von Kapitel 11. Die bindbare Infrastruktur
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 11. Die bindbare Infrastruktur'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: edc3dfd97457fe93a04edd82574f6ed419f5fdc1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136798"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Zusammenfassung von Kapitel 11. Die bindbare Infrastruktur

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

Jeder C#-Programmierer ist mit den *Eigenschaften* von C# vertraut. Eigenschaften enthalten einen *set*-Accessor und/oder einen *get*-Accessor. Im Kontext der Common Language Runtime werden sie häufig als *CLR-Eigenschaften* bezeichnet.

In Xamarin.Forms ist eine erweiterte Eigenschaft definiert, die als *bindbare Eigenschaft* bezeichnet wird. Sie wird von der Klasse [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) gekapselt und von der Klasse [`BindableObject`](xref:Xamarin.Forms.BindableObject) unterstützt. Diese Klassen sind verwandt, unterscheiden sich jedoch in wichtigen Punkten: Mit `BindableProperty` wird die Eigenschaft selbst definiert. `BindableObject` ist insofern mit `object` vergleichbar, als es sich um eine Stammklasse für Klassen handelt, die bindbare Eigenschaften definieren.

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms-Klassenhierarchie

Im [**ClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy)-Beispiel wird mithilfe einer Reflexion eine Xamarin.Forms-Klassenhierarchie gezeigt. Außerdem veranschaulicht dieses Beispiel, welche entscheidende Rolle `BindableObject` in dieser Hierarchie spielt. `BindableObject` wird von `Object` abgeleitet und ist die übergeordnete Klasse von [`Element`](xref:Xamarin.Forms.Element), von dem [`VisualElement`](xref:Xamarin.Forms.VisualElement) abgeleitet wird. Dies wiederum ist die übergeordnete Klasse von [`Page`](xref:Xamarin.Forms.Page) und [`View`](xref:Xamarin.Forms.View), der übergeordneten Klasse von [`Layout`](xref:Xamarin.Forms.Layout):

[![Screenshot zur gemeinsamen Klassenhierarchie](images/ch11fg01-small.png "Gemeinsame Klassenhierarchie")](images/ch11fg01-large.png#lightbox "Gemeinsame Klassenhierarchie")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject und BindableProperty

In den Klassen, die von `BindableObject` abgeleitet werden, wird bei vielen CLR-Eigenschaften davon gesprochen, dass sie von bindbaren Eigenschaften „gestützt“ werden. Bei der Eigenschaft [`Text`](xref:Xamarin.Forms.Label.Text) der Klasse `Label` handelt es sich beispielsweise um eine CLR-Eigenschaft, die Klasse `Label` definiert jedoch auch ein öffentliches statisches schreibgeschütztes Feld [`TextProperty`](xref:Xamarin.Forms.Label.TextProperty) vom Typ `BindableProperty`.

Anwendungen können die Eigenschaft `Text` von `Label` auf normale Weise festlegen oder abrufen. Alternativ kann eine Anwendung `Text` festlegen, indem sie die Methode [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) aufruft, die von `BindableObject` mit einem `Label.TextProperty`-Argument definiert wird. Gleichermaßen kann eine Anwendung den Wert der Eigenschaft `Text` abrufen, indem sie die Methode [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) aufruft (auch hier mit einem `Label.TextProperty`-Argument). Dieser Vorgang wird im Beispiel [**PropertySettings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) veranschaulicht.

Die CLR-Eigenschaft `Text` wird tatsächlich vollständig mit den Methoden `SetValue` und `GetValue` implementiert, die von `BindableObject` gemeinsam mit der statischen Eigenschaft `Label.TextProperty` definiert sind.

`BindableObject` und `BindableProperty` bieten Unterstützung für folgende Anwendungsfälle:

- Festlegen von Standardwerten für Eigenschaften
- Speichern aktueller Werte
- Bereitstellen von Mechanismen zum Überprüfen von Eigenschaftswerten
- Sicherstellen von Konsistenz für verwandte Eigenschaften innerhalb einer Klasse
- Reaktion auf Eigenschaftsänderungen
- Auslösen von Benachrichtigungen, wenn sich eine Eigenschaft ändert oder geändert hat
- Unterstützung von Datenbindung
- Unterstützung von Stilen
- Unterstützung von dynamischen Ressourcen

Wenn sich eine Eigenschaft ändert, die von einer bindbaren Eigenschaft gestützt wird, löst `BindableObject` ein [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged)-Ereignis aus, das Auskunft über die geänderte Eigenschaft gibt. Dieses Ereignis wird nicht ausgelöst, wenn die Eigenschaft auf denselben Wert festgelegt wird.

Einige Eigenschaften werden nicht durch bindbare Eigenschaften gestützt, und einige Xamarin.Forms-Klassen (z. B. `Span`) werden nicht von `BindableObject` abgeleitet. Da `BindableObject` die Methoden `SetValue` und `GetValue` definiert, können nur Klassen, die von `BindableObject` abgeleitet werden, bindbare Eigenschaften unterstützen.

Da `Span` nicht von `BindableObject` abgeleitet wird, ist keine der zugehörigen Eigenschaften &mdash; z.B. `Text` &mdash; durch eine bindbare Eigenschaft gestützt. Aus diesem Grund wird bei Festlegung von `DynamicResource` für die Eigenschaft `Text` von `Span` im Beispiel [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) des vorherigen Kapitels eine Ausnahme ausgelöst. Das Beispiel [**DynamicVsStaticCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) zeigt, wie mithilfe der Methode [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)), die von `Element` definiert wird, eine dynamische Ressource im Code angegeben wird. Das erste Argument ist ein Objekt vom Typ `BindableProperty`.

Entsprechend hat das erste Argument der von `BindableObject` definierten Methode [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) den Typ `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definieren von bindbaren Eigenschaften

Sie können eigene bindbare Eigenschaften mithilfe der statischen Methode [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) zum Erstellen eines statischen schreibgeschützten Felds vom Typ `BindableProperty` definieren.

Dieser Vorgang wird in der Klasse [`AltLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek veranschaulicht. Die Klasse wird von `Label` abgeleitet und ermöglicht die Angabe eines Schriftgrads. Dieser Schritt wird im Beispiel [**PointSizedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) gezeigt.

Es werden vier Argumente der Methode `BindableProperty.Create` benötigt:

- `propertyName`: der Textname der Eigenschaft (identisch mit dem Namen der CLR-Eigenschaft)
- `returnType`: der Typ der CLR-Eigenschaft
- `declaringType`: der deklarierende Typ der Eigenschaft
- `defaultValue`: der Standardwert der Eigenschaft

Da `defaultValue` den Typ `object` aufweist, muss der Compiler den Typ des Standardwerts bestimmen können. Wenn für `returnType` z.B. `double` festgelegt ist, sollte `defaultValue` auf einen Wert wie 0.0 anstelle von 0 festgelegt werden. Anderenfalls wird durch den Typenkonflikt während der Laufzeit eine Ausnahme ausgelöst.

Bindbare Eigenschaften umfassen zudem häufig Folgendes:

- `propertyChanged`: eine statische Methode, die aufgerufen wird, wenn sich der Eigenschaftswert ändert. Das erste Argument ist die Instanz der Klasse, deren Eigenschaft sich geändert hat.

Die übrigen Argumente für `BindableProperty.Create` sind weniger gebräuchlich:

- `defaultBindingMode`: wird in Verbindung mit der Datenbindung verwendet (siehe [**Kapitel 16: Datenbindung**](chapter16.md))
- `validateValue`: ein Rückruf zur Überprüfung auf einen gültigen Wert
- `propertyChanging`: ein Rückruf, um anzugeben, wann sich die Eigenschaft ändern wird
- `coerceValue`: ein Rückruf, um das Ändern eines festgelegten Werts in einen anderen Wert zu erzwingen
- `defaultValueCreate`: ein Rückruf, um einen Standardwert zu erstellen, der nicht von mehreren Instanzen der Klasse gemeinsam verwendet werden kann (z.B. eine Sammlung)

### <a name="the-read-only-bindable-property"></a>Die schreibgeschützte bindbare Eigenschaft

Eine bindbare Eigenschaft kann als schreibgeschützt festgelegt werden. Um eine schreibgeschützte bindbare Eigenschaft zu erstellen, muss die statische Methode [`BindableProperty.CreateReadOnly`](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) zum Definieren eines privaten statischen schreibgeschützten Felds vom Typ [`BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey) aufgerufen werden.

Definieren Sie anschließend den `set`-Accessor der CLR-Eigenschaft als `private`, um eine [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object))-Überladung mit dem Objekt `BindablePropertyKey` aufzurufen. Dadurch wird verhindert, dass die Eigenschaft außerhalb der Klasse festgelegt wird.

Dieser Vorgang wird in der Klasse [`CountedLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) veranschaulicht, die im Beispiel [**BaskervillesCount**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) verwendet wird.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 11 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Kapitel 11 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [Bindbare Eigenschaften](~/xamarin-forms/xaml/bindable-properties.md)
