---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF im Vergleich zu. Xamarin.Forms: Ähnlichkeiten und Unterschiede'
description: In diesem Dokument verglichen und gegenübergestellt von WPF in Xamarin.Forms. Es wird erläutert, Steuerelementvorlagen, XAML, bindungsinfrastruktur, Datenvorlagen, ItemsControl-Element, UserControl, Navigation und URL-Navigation.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 990253cbd31ad79bc47f086dc5bd2b99233f2032
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61272661"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF im Vergleich zu. Xamarin.Forms: Ähnlichkeiten und Unterschiede

## <a name="control-templates"></a>Steuerelementvorlagen

WPF unterstützt das Konzept der *Steuerelementvorlagen* bieten die Visualisierung Anweisungen für ein Steuerelement (`Button`, `ListBox`usw..). Wie bereits erwähnt, handelt es sich bei Xamarin.Forms verwendet konkreten _Rendering_ Klassen für diese Interaktion mit die systemeigene Plattform (iOS, Android usw.), um das Steuerelement visuell darzustellen.

Allerdings Xamarin.Forms _ist_ haben eine `ControlTemplate` Typ - dient für Designs `Page` Objekte. Es bietet eine Definition für eine `Page` , konsistente Inhalte bietet, aber ermöglicht dem Benutzer der Seite ändern, Farben, Schriftarten, usw., und sogar hinzufügen, Elemente, um sie an die Anwendung eindeutig zu machen.

Allgemeine Verwendungen für diese Dinge wie z. B. Authentifizierung Dialoge, Anweisungen und einem standardisierten, aber Designfähige Seite aussehen und Verhalten bereitstellen, die innerhalb der app angepasst werden kann. Als Teil dieses Supports werden viele vertraut mit dem Namen WPF-Steuerelemente verwendet werden:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Es ist jedoch wichtig zu wissen, dass hierbei handelt es sich _nicht_ erfüllt denselben Zweck in Xamarin.Forms. Weitere Informationen zu diesem Feature finden Sie in der [Dokumentationsseite](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML wird als der deklarative Markupsprache für WPF und Xamarin.Forms verwendet. Zum größten Teil, die Syntax ist identisch – der Hauptunterschied liegt in den Objekten, die den XAML-Diagrammen definiert bzw. erstellt werden.

- Xamarin.Forms unterstützt die [XAML 2009-Spezifikation](/dotnet/framework/xaml-services/xaml-2009-language-features/); Dies erleichtert es, Daten zu definieren, wie z. B. `string`s, `int`s, usw., sowie Definieren von generischen Typen und übergeben von Argumenten an die Konstruktoren.

- Es gibt derzeit keine Möglichkeit zum dynamischen Laden von XAML wie bei WPF auch `XamlReader`. Sie erhalten die gleiche grundlegende Funktionen mit einem [NuGet-Paket](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) jedoch.

### <a name="markup-extensions"></a>Markuperweiterungen

Xamarin.Forms unterstützt, erweitern anhand von Markuperweiterungen, ähnlich wie die WPF-XAML. Standardmäßig hat die gleichen grundlegenden Komponenten:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Darüber hinaus enthält es `{x:Reference}` aus der XAML 2009-Spezifikation und eine `{TemplateBinding}` Markuperweiterung, die für die spezialisierte Version der verwendet wird `ControlTemplate` von Xamarin.Forms unterstützt.

> [!WARNING]
> Die `ControlTemplate` Unterstützung ist nicht das gleiche -, obwohl es sich um den gleichen Namen hat.

Xamarin.Forms unterstützt auch – benutzerdefinierte Markuperweiterungen, aber die Implementierung unterscheidet sich etwas. In WPF durch Ableiten von `MarkupExtension` – einer abstrakten Klasse. In Xamarin.Forms, die durch eine Schnittstelle ersetzt `IMarkupExtension` oder `IMarkupExtension<T>` die ist flexibler.

Genau wie WPF, die einzelne erforderliche Methode ist eine `ProvideValue` Methode, um den Wert aus der Markuperweiterung zurückzugeben.

## <a name="binding-infrastructure"></a>Bindungsinfrastruktur

Einer der wichtigsten Konzepte für übernommen wird zur Verbindung mit .NET Dateneigenschaften Eigenschaften visueller Elemente einer Bindung-Infrastruktur. Dies ermöglicht eine architektonische Muster wie MVVM. Das grundlegende Design ist identisch: Sie haben eine bindbare Basisklasse [BindableObject](xref:Xamarin.Forms.BindableObject), in WPF, dies ist, die [DependencyObject](xref:System.Windows.DependencyObject) Klasse. Diese Basisklasse dient als den Vorgänger Stamm für alle Objekte, die als Ziele bei der Datenbindung einbezogen werden. Die abgeleiteten Klassen Sie verfügbar machen [BindableProperty](xref:Xamarin.Forms.BindableProperty) Objekte, die als Hintergrundspeicher für Eigenschaftswerte fungieren (diese sind definiert als [DependencyProperty](xref:System.Windows.DependencyProperty) Objekte in WPF).

### <a name="defining-bindable-properties"></a>Definieren von bindbare Eigenschaften

Die Definition für eine bindbare Eigenschaft in Xamarin.Forms ist identisch mit WPF:
1. Das Objekt muss abgeleitet `BindableObject`.
2. Es muss ein öffentliche statische Feld vom Typ `BindableProperty` deklariert, um den Speicherschlüssel "Sicherung" für die Eigenschaft zu definieren.
3. Ein öffentliche Instanz-Eigenschaftenwrapper, die verwendet werden soll `GetValue` und `SetValue` abrufen und ändern Sie den Eigenschaftenwert.

Ein vollständiges Beispiel finden Sie unter [bindbare Eigenschaften in Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Angefügte Eigenschaften

Angefügte Eigenschaften sind eine Teilmenge der bindbare Eigenschaft und die gleiche Weise wie bei WPF funktionieren. Der Hauptunterschied besteht darin, dass die Eigenschaftenwrapper Ommitted in diesem Fall ist, und mit einem Satz von statischen get-/Set-Methoden für die besitzende Klasse ersetzt. Finden Sie unter [angefügte Eigenschaften in Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) für Weitere Informationen.

### <a name="using-the-binding-engine"></a>Verwenden die Bindungs-engine

Der Prozess zur Verwendung der Bindungs-Engine ist dasselbe wie in WPF. Im Code-Behind erstellen genutzt werden eine `Binding` Objekt gebunden wird, um ein Quellobjekt (beliebiger Typ von .NET) und eine optionale Eigenschaft-Wert (wenn Ommitted, wird das Quellobjekt als die Eigenschaft selbst – genau wie behandelt WPF). Anschließend können Sie `SetBinding` für jedes beliebige `BindableObject` , ordnen Sie die Bindung an eine `BindableProperty`.

Alternativ können Sie definieren die bindungsbeziehung in XAML mithilfe der `BindingExtension`. Es hat die gleichen grundlegenden Werte wie die Erweiterung in WPF.

Die bindungsunterstützung und das Modul ähneln mehr der Silverlight-Implementierung als WPF. Es gibt mehrere fehlende Features, die nicht in Xamarin.Forms implementiert wurden:

- Es gibt keine Unterstützung für Bindungen die folgenden Features:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - ValidationRules-Auflistung
    - XPath
    - XmlNamespaceManager

#### <a name="relativesource"></a>RelativeSource

Es gibt keine Unterstützung für `RelativeSource` Bindungen. In WPF können diese zum Binden an andere visuelle Elemente, die in XAML definiert. In Xamarin.Forms kann diese Möglichkeit mit erreicht werden die `{x:Reference}` Markuperweiterung. Beispielsweise können vorausgesetzt, dass es ein Steuerelement mit dem Namen "OtherControl", die eine Text-Eigenschaft gibt an, wir, wie folgt binden:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Dieselbe Funktion kann verwendet werden, für die `{RelativeSource Self}` Feature. Aber es gibt keine Unterstützung für die Suche nach übergeordneten Elementen vom Typ (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Bindungskontext

In WPF können Sie definieren eine `DataContext` Eigenschaftswert der Reprents die Standardeinstellung, die Bindungsquelle. Wenn die Quelle für eine Bindung nicht definiert ist, wird der Wert dieser Eigenschaft verwendet. Der Wert wird nach unten der visuellen Struktur, und es ermöglicht, die auf einer höheren Ebene definiert werden, und dann verwendet, von untergeordneten Elementen geerbt.

In Xamarin.Forms wird mit diesem Feature verfügbaren, aber der Eigenschaftsname ist `BindingContext`.

#### <a name="value-converters"></a>Wertkonverter

Wertkonverter sind in Xamarin.Forms: wie WPF vollständig unterstützt. Die gleiche Form der Schnittstelle wird verwendet, aber Xamarin.Forms verfügt über die Schnittstelle definiert, der `Xamarin.Forms` Namespace.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM ist vollständig sowohl WPF als auch Xamarin.Forms unterstützt.

WPF gehört eine integrierte `RoutedCommand` die manchmal verwendet wird. Xamarin.Forms verfügt über keine integrierte Unterstützung von sind mehr als die `ICommand` Schnittstellendefinition. Sie können eine Vielzahl von MVVM-Frameworks, um die erforderlichen Basisklassen zum Implementieren von MVVM hinzuzufügen einschließen.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged und INotifyCollectionChanged

Beide Schnittstellen werden in Xamarin.Forms-Bindungen vollständig unterstützt. Im Gegensatz zu vielen Frameworks, die XAML-basierte Benachrichtigungen über eigenschaftsänderungen in Hintergrundthreads in Xamarin.Forms (wie WPF) ausgelöst werden können, und die Bindungs-Engine geht ordnungsgemäß an den UI-Thread.

Darüber hinaus beide Umgebungen unterstützen `SynchronziationContext` und `async` / `await` , führen Sie den richtigen Thread zu marshallen. WPF enthält die `Dispatcher` Klasse auf alle visuellen Elemente, Xamarin.Forms verfügt über eine statische Methode `Device.BeginInvokeOnMainThread` auch verwendet werden kann (obwohl `SynchronizationContext` wird bevorzugt, für die plattformübergreifende Programmierung).

- Xamarin.Forms umfasst einen `ObservableCollection<T>` Auflistung änderungsbenachrichtigungen unterstützt.
- Sie können `BindingBase.EnableCollectionSynchronization` threadübergreifenden Aktualisierungen für eine Sammlung zu aktivieren. Die API weicht geringfügig von der WPF-Variante [überprüfen Sie die Dokumente für Nutzungsdetails](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Datenvorlagen

Datenvorlagen werden in Xamarin.Forms, um die Anpassung von unterstützt eine `ListView` Zeile (Zelle). Im Gegensatz zu WPF die verwenden `DataTemplate`s für beliebige Inhalte-orientierten Steuerelemente, Xamarin.Forms derzeit wird nur verwendet, für die `ListView`. Die Vorlagendefinition verwendet werden kann (zugewiesen der `ItemTemplate` Eigenschaft), oder als Ressource in einem `ResourceDictionary`.

Darüber hinaus sind sie nicht so flexibel wie ihre WPF-Entsprechung.

1. Das Stammelement der `DataTemplate` müssen _immer_ werden eine `ViewCell` Objekt.
2. Data-Trigger werden in einer Datenvorlage vollständig unterstützt, müssen jedoch enthalten eine `DataType` Eigenschaft gibt den Typ der Eigenschaft, die der Trigger zugeordnet ist.
3. `DataTemplateSelector` wird auch unterstützt, aber leitet sich von `DataTemplate` und daher nur erhält direkt an die `ItemTemplate` Eigenschaft (im Vergleich zu `ItemTemplateSelector` in WPF).

## <a name="itemscontrol"></a>ItemsControl

Es gibt keine integrierte Entsprechung auf ein `ItemsControl` in Xamarin.Forms; es gibt jedoch eine [benutzerdefinierte eine für Xamarin.Forms verfügbar hier](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Benutzersteuerelemente

In WPF `UserControl`s werden verwendet, um einen Abschnitt der Benutzeroberfläche bereitstellen, die Verhalten zugeordnet ist. In Xamarin.Forms verwenden wir die `ContentView` für denselben Zweck. Beide unterstützen Bindung und für die Einbindung in XAML.

## <a name="navigation"></a>Navigation

WPF enthält ein selten verwendete `NavigationService` die verwendet werden, um eine Funktion "Browser-Like" Navigation bereitzustellen. Die meisten apps nicht kümmern Sie sich jedoch und stattdessen verwendet verschiedene `Window` Elemente oder die verschiedenen Abschnitte der Fenster zum Anzeigen von Daten.

Für Phone-Geräte, die verschiedene _Bildschirme_ sind häufig die Lösung und daher Xamarin.Forms umfasst Unterstützung für verschiedene Typen der Navigation:

| Navigationsstil | Seitentyp |
|--- |--- |
|Stapelbasierte (Push/Pop)|NavigationPage|
|Master/Detail|MasterDetailPage|
|Registerkarten|TabbedPage|
|Streichen Sie nach links/rechts|CarouselView|

Die `NavigationPage` der gängigste Ansatz ist, und jede Seite hat eine `Navigation` Eigenschaft, die mithilfe von Push übertragen oder pop-Seiten, die ein- und Ausschalten der Navigationsstapel verwendet werden kann. Dies entspricht dem am nächsten an der `NavigationService` finden Sie in WPF.

### <a name="url-navigation"></a>URL-navigation

WPF ist eine desktoporientierten Technologie und Befehlszeilenparameter zur Weiterleitung einer Startverhalten akzeptieren. Xamarin.Forms können [deep linking-URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) beim Start zu einer Seite zu springen.
