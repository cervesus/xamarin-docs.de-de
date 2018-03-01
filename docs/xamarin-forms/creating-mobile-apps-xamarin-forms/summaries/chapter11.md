---
title: "Zusammenfassung der Kapitel 11. Die bindungsfähigen-Infrastruktur"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3e3cfb55f7b96751979d14b489e892bc07817780
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Zusammenfassung der Kapitel 11. Die bindungsfähigen-Infrastruktur

Jeder C#-Programmierer ist mit c# vertraut *Eigenschaften*. Eigenschaften enthalten eine *festgelegt* Accessor und/oder eine *abrufen* Accessor. Sie werden häufig aufgerufen *CLR-Eigenschaften* für die Common Language Runtime.

Xamarin.Forms definiert eine erweiterte Eigenschaft Rollendefinition mit der Bezeichnung eine *bindbare Eigenschaft* gekapselt durch den [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Klasse und von unterstützt die [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)Klasse. Diese Klassen sind ähnliche, aber sehr unterschiedliche: die `BindableProperty` wird verwendet, um die Eigenschaft selbst definieren. `BindableObject` ähnelt `object` insofern ist eine Basisklasse für Klassen, die bindungsfähige Eigenschaften definieren.

## <a name="the-xamarinforms-class-hierarchy"></a>Die Xamarin.Forms-Klassenhierarchie

Die [ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) Beispiel verwendet Reflektion zum Anzeigen einer Klassenhierarchie von Xamarin.Forms und veranschaulichen die entscheidende Rolle `BindableObject` in dieser Hierarchie. `BindableObject` leitet sich von `Object` und ist die übergeordnete Klasse [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) aus dem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) abgeleitet wird. Dies ist die übergeordnete Klasse [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) und [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), dies ist die übergeordnete Klasse [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![Dreifacher Screenshot der Klassenhierarchie Freigabe](images/ch11fg01-small.png "Klasse Hierarchie Freigabe")](images/ch11fg01-large.png "Klasse Hierarchie freigeben")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Einen Blick in BindableObject und BindableProperty

In den Klassen, die davon Herleiten `BindableObject` viele CLR-Eigenschaften werden als "durch bindbare Eigenschaften unterstützt werden" bezeichnet. Z. B. die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Eigenschaft von der `Label` Klasse ist eine CLR-Eigenschaft, aber die `Label` -Klasse definiert außerdem ein öffentliche statisches schreibgeschützte-Feld, mit dem Namen [ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/) von Typ `BindableProperty`.

Eine Anwendung festgelegt oder abgerufen werden kann die `Text` Eigenschaft `Label` in der Regel wird dynamisch generiert oder die Anwendung festgelegt die `Text` durch Aufrufen der [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) definierte Methode `BindableObject` mit einer `Label.TextProperty` Argument. Auf ähnliche Weise kann eine Anwendung Abrufen des Werts der `Text` Eigenschaft durch Aufrufen der [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) -Methode, mit einer `Label.TextProperty` Argument. Dies wird veranschaulicht, durch die [ **PropertySettings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) Beispiel.

In der Tat die `Text` CLR-Eigenschaft ist vollständig mit implementiert die `SetValue` und `GetValue` definierten Methoden `BindableObject` in Verbindung mit der `Label.TextProperty` statische Eigenschaft.

`BindableObject` und `BindableProperty` bieten Unterstützung für:

- Erteilen die Standardwerte für Eigenschaften
- Speichern ihre aktuellen Werte
- Mechanismen für die Überprüfung der Eigenschaftswerte bereitstellen
- Wahrung der Konsistenz zwischen verwandten Eigenschaften in eine einzelne Klasse
- Reagieren auf eigenschaftenänderungen
- Benachrichtigungen auslösen, wenn eine Eigenschaft geändert wird oder wurde geändert
- Die Datenbindung unterstützen
- Unterstützung von Stilen
- Unterstützung von dynamischen Ressourcen

Wenn eine Eigenschaft, die durch eine bindbare Eigenschaft ändert, gesichert wird `BindableObject` ausgelöst wird eine [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) Ereignis Identifizieren der geänderten Eigenschaft. Dieses Ereignis wird nicht ausgelöst, wenn die Eigenschaft auf den gleichen Wert festgelegt ist.

Einige Eigenschaften werden durch bindbare Eigenschaften und einige Klassen Xamarin.Forms & #x 2014 nicht unterstützt; z. B. `Span` & #x 2014; nicht ableiten `BindableObject`. Nur eine Klasse, die abgeleitet `BindableObject` bindbare Eigenschaften können unterstützt werden, weil `BindableObject` definiert die `SetValue` und `GetValue` Methoden.

Da `Span` nicht ableiten `BindableObject`, keine seiner Eigenschaften & #x 2014; z. B. `Text` & #x 2014; die durch eine bindbare Eigenschaft gesichert werden. Deswegen eine `DynamicResource` festlegen, auf die `Text` Eigenschaft `Span` löst eine Ausnahme in der [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) in im Kapitel über das vorherige Beispiel. Die [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) Beispiel veranschaulicht, wie eine dynamische Ressourcen im Code mithilfe der [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/) definierte Methode `Element`. Das erste Argument ist ein Objekt vom Typ `BindableProperty`.

Auf ähnliche Weise die [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) definierte Methode `BindableObject` verfügt über ein erstes Argument vom Typ `BindableProperty`.

## <a name="defining-bindable-properties"></a>Definieren von bindbare Eigenschaften

Sie können eigene bindbaren Eigenschaften, die mithilfe der statischen definieren [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) -Methode oder der etwas kürzere [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) Überladung zum Erstellen eines statischen schreibgeschützten Felds des Typs `BindableProperty`.

Dies wird dargestellt, der [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek. Die Klasse leitet sich von `Label` und ermöglicht die Angabe einen Schriftgrad in Punkt. Es wird veranschaulicht, der [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) Beispiel.

Vier Argumente, die von der `BindableProperty.Create` Methode sind erforderlich:

- `propertyName`: Der Textname der Eigenschaft (identisch mit den Namen der CLR-Eigenschaft)
- `returnType`: der Typ des CLR-Eigenschaft
- `declaringType`: der Typ der Klasse deklarieren die Eigenschaft
- `defaultValue`: Standardwert der Eigenschaft

Da `defaultValue` ist vom Typ `object`, der Compiler muss in der Lage, den Standardwert zu bestimmen. Z. B. wenn die `returnType` ist `double`die `defaultValue` sollte z. B. 0,0, anstatt nur 0 festgelegt werden, oder die Typenkonflikt wird zur Laufzeit eine Ausnahme ausgelöst.

Es ist auch häufig für eine bindbare Eigenschaft enthalten:

- `propertyChanged`: eine statische Methode wird aufgerufen, wenn der Wert der Eigenschaft geändert wird. Das erste Argument ist die Instanz der Klasse, dessen Eigenschaft geändert wurde.

Die anderen Argumente `BindableProperty.Create` sind nicht so üblich:

- `defaultBindingMode`: verwendet die im Zusammenhang mit der Datenbindung (wie in beschrieben [ **Kapitel 16. Die Datenbindung**](chapter16.md))
- `validateValue`: ein Rückruf für einen gültigen Wert überprüfen
- `propertyChanging`: ein Rückruf, um anzugeben, wenn die Eigenschaft geändert wird.
- `coerceValue`: einen Rückruf an einen festgelegten Wert in einen anderen Wert umgewandelt werden.
- `defaultValueCreate`: einen Rückruf an einen Standardwert zu erstellen, die von Instanzen der Klasse (z. B. eine Sammlung) gemeinsam genutzt werden kann

### <a name="the-read-only-bindable-property"></a>Die schreibgeschützte bindbare Eigenschaft

Eine bindbare Eigenschaft kann schreibgeschützt sein. Erstellen eine bindbare Eigenschaft schreibgeschützt erfordert das Aufrufen der statischen Methode [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) oder eine kürzere [ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) definieren Sie ein private statische schreibgeschützten Feld des Typs [ `BindablePropertyKey` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

Definieren Sie anschließend auf die CLR-Eigenschaft `set` Accesor als `private` Aufrufen einer [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/) -Überladung mit dem `BindablePropertyKey` Objekt. Dadurch wird verhindert, dass die Eigenschaft, die außerhalb der Klasse festgelegt wird.

Dies wird dargestellt, der [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) Klasse zur Verwendung in der [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) Beispiel.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 11 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Kapitel 11-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
