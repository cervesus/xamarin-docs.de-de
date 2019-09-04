---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF im Vergleich zu Xamarin. Forms: Ähnlichkeiten & Unterschiede'
description: Dieses Dokument vergleicht und vergleicht WPF mit xamarin. Forms. Es werden Steuerungs Vorlagen, XAML, Bindungs Infrastruktur, Datenvorlagen, ItemsControl, UserControl, Navigation und URL-Navigation erläutert.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 149636719c7f8046b8a32d8d2f4157b663388cb1
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227611"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF im Vergleich zu Xamarin. Forms: Ähnlichkeiten & Unterschiede

## <a name="control-templates"></a>Steuerelementvorlagen

WPF unterstützt das Konzept von *Steuerelement Vorlagen* , die die Visualisierungs Anweisungen für`Button`ein `ListBox`Steuerelement (, usw.) bereitstellen. Wie bereits erwähnt, verwendet xamarin. Forms konkrete _Renderingklassen_ , die mit der nativen Plattform (Ios, Android usw.) interagieren, um das Steuerelement visuell darzustellen.

Xamarin. Forms _weist_ jedoch einen `ControlTemplate` -Typ auf, der für die `Page` Verwendung von-Objekten verwendet wird. Sie stellt eine Definition für einen `Page` bereit, der konsistenten Inhalt bereitstellt, der Benutzer der Seite jedoch das Ändern von Farben, Schriftarten usw. und sogar das Hinzufügen von Elementen ermöglicht, um ihn für die Anwendung eindeutig zu gestalten.

Gängige Verwendungen hierfür sind z. b. Authentifizierungs Dialogfelder, Eingabe Aufforderungen und, um ein standardisiertes, aber beschreibbares Seiten Erscheinungsbild bereitzustellen, das in der App angepasst werden kann. Im Rahmen dieser Unterstützung werden viele vertraute WPF-benannte Steuerelemente verwendet:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Es ist jedoch wichtig zu wissen, dass diese _nicht_ denselben Zweck in xamarin. Forms erfüllen. Weitere Informationen zu diesem Feature finden Sie auf der [Dokumentationsseite](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML wird als deklarative Markup Sprache für WPF und xamarin. Forms verwendet. Größtenteils ist die Syntax identisch. der primäre Unterschied sind die Objekte, die von den XAML-Diagrammen definiert/erstellt werden.

- Xamarin. Forms unterstützt die [XAML 2009-Spezifikation](/dotnet/framework/xaml-services/xaml-2009-language-features/). Dies vereinfacht die Definition von Daten wie z `string`. b. s, `int`s usw. sowie das Definieren von generischen Typen und das Übergeben von Argumenten an Konstruktoren.

- Es gibt derzeit keine Möglichkeit zum dynamischen Laden von XAML wie WPF mit `XamlReader`. Sie können jedoch die gleichen grundlegenden Funktionen mit einem [nuget-Paket](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) erhalten.

### <a name="markup-extensions"></a>Markuperweiterungen

Xamarin. Forms unterstützt das Erweitern von XAML durch Markup Erweiterungen, ähnlich wie WPF. Standardmäßig gibt es die gleichen grundlegenden Bausteine:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Darüber hinaus enthält `{x:Reference}` Sie aus der XAML 2009-Spezifikation und eine `{TemplateBinding}` Markup Erweiterung, die für die spezialisierte Version von `ControlTemplate` verwendet wird, die von xamarin. Forms unterstützt wird.

> [!WARNING]
> Die `ControlTemplate` Unterstützung ist nicht identisch, auch wenn Sie denselben Namen hat.

Xamarin. Forms unterstützt auch benutzerdefinierte Markup Erweiterungen, aber die Implementierung unterscheidet sich geringfügig. In WPF müssen Sie von `MarkupExtension` einer abstrakten Basisklasse ableiten. In xamarin. Forms wird dies durch eine Schnittstelle `IMarkupExtension` ersetzt, oder `IMarkupExtension<T>` die ist flexibler.

Ebenso wie WPF ist die einzige erforderliche Methode eine `ProvideValue` Methode, um den Wert aus der Markup Erweiterung zurückzugeben.

## <a name="binding-infrastructure"></a>Bindungs Infrastruktur

Eines der wichtigsten Konzepte, die übertragen werden, ist eine Daten Bindungs Infrastruktur, mit der Sie visuelle Eigenschaften mit .NET-Daten Eigenschaften verbinden können. Dies ermöglicht Architekturmuster wie MVVM. Der grundlegende Entwurf ist identisch: Sie verfügen über eine bindbare [bindableobject](xref:Xamarin.Forms.BindableObject)-Basisklasse, in WPF ist dies die [DependencyObject](xref:System.Windows.DependencyObject) -Klasse. Diese Basisklasse wird als Stamm Vorgänger für alle Objekte verwendet, die als Ziele bei der Datenbindung verwendet werden. Die abgeleiteten Klassen machen dann [bindableproperty](xref:Xamarin.Forms.BindableProperty) -Objekte verfügbar, die als Sicherungs Speicher für Eigenschaftswerte fungieren (diese werden als [DependencyProperty](xref:System.Windows.DependencyProperty) -Objekte in WPF definiert).

### <a name="defining-bindable-properties"></a>Definieren von bindbare Eigenschaften

Die Definition für eine bindbare Eigenschaft in xamarin. Forms ist identisch mit WPF:
1. Das-Objekt muss von `BindableObject`abgeleitet werden.
2. Es muss ein öffentliches statisches Feld vom Typ `BindableProperty` deklariert sein, um den Sicherungs Speicher Schlüssel für die Eigenschaft zu definieren.
3. Es sollte ein Eigenschafts Wrapper für öffentliche Instanzen vorhanden `GetValue` sein `SetValue` , der und verwendet, um den Eigenschafts Wert abzurufen und zu ändern.

Ein vollständiges Beispiel finden Sie unter [bindbare Eigenschaften in xamarin. Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Angefügte Eigenschaften

Angefügte Eigenschaften sind eine Teilmenge der bindbaren Eigenschaft und funktionieren genauso wie in WPF. Der Hauptunterschied besteht darin, dass in diesem Fall der Eigenschafts Wrapper konvertiert und durch einen Satz statischer Get/Set-Methoden für die besitzende Klasse ersetzt wird. Weitere Informationen finden Sie [unter angefügte Eigenschaften in xamarin. Forms](~/xamarin-forms/xaml/attached-properties.md) .

### <a name="using-the-binding-engine"></a>Verwenden der Bindungs-Engine

Der Prozess für die Verwendung der Bindungs-Engine ist der gleiche wie in WPF. Sie kann im Code-Behind verwendet werden, indem ein `Binding` -Objekt erstellt wird, das an ein Quell Objekt (beliebiger .NET-Typ) und einen optionalen Eigenschafts Wert gebunden ist (bei der Übermittlung wird das Quell Objekt wie WPF als die Eigenschaft selbst behandelt). Anschließend können Sie für `SetBinding` alle `BindableObject` verwenden, um die Bindung einem `BindableProperty`zuzuordnen.

Alternativ können Sie die Bindungs Beziehung in XAML mithilfe `BindingExtension`von definieren. Sie verfügt über die gleichen grundlegenden Werte wie die Erweiterung in WPF.

Die Bindungs Unterstützung und-Engine ähneln eher der Silverlight-Implementierung als WPF. Es gibt mehrere fehlende Features, die in xamarin. Forms nicht implementiert wurden:

- Die folgenden Funktionen in Bindungen werden nicht unterstützt:
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
  - ValidationRules-Sammlung
  - XPath
  - XmlNamespaceManager

#### <a name="relativesource"></a>RelativeSource

`RelativeSource` Bindungen werden nicht unterstützt. In WPF können diese an andere visuelle Elemente gebunden werden, die in XAML definiert sind. In xamarin. Forms kann dieselbe Funktion mithilfe der `{x:Reference}` Markup Erweiterung erreicht werden. Angenommen, wir haben ein Steuerelement mit dem Namen "othercontrol", das über eine Text-Eigenschaft verfügt, können wir wie folgt binden:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Die gleiche Funktion kann für das `{RelativeSource Self}` Feature verwendet werden. Es gibt jedoch keine Unterstützung für die Suche nach Vorgängern`{RelativeSource FindAncestor}`nach Typ ().

#### <a name="binding-context"></a>Bindungs Kontext

In WPF können Sie einen `DataContext` Eigenschafts Wert definieren, der die Standard Bindungs Quelle zurückgibt. Wenn die Quelle für eine Bindung nicht definiert ist, wird dieser Eigenschafts Wert verwendet. Der Wert wird in der visuellen Struktur geerbt, sodass er auf einer höheren Ebene definiert und dann von untergeordneten Elementen verwendet werden kann.

In xamarin. Forms ist dieselbe Funktion verfügbar, aber der Eigenschaftsname ist `BindingContext`.

#### <a name="value-converters"></a>Wertkonverter

Wert Konverter werden in xamarin. Forms wie WPF vollständig unterstützt. Dieselbe Schnittstellen Form wird verwendet, aber xamarin. Forms verfügt über die-Schnittstelle, `Xamarin.Forms` die im-Namespace definiert ist.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM wird sowohl von WPF als auch von xamarin. Forms vollständig unterstützt.

WPF enthält ein integriertes `RoutedCommand` , das manchmal verwendet wird. Xamarin. Forms verfügt über keine integrierte Befehls Unterstützung, die über `ICommand` die Schnittstellen Definition hinausgeht. Sie können eine Vielzahl von MVVM-Frameworks einschließen, um die erforderlichen Basisklassen zum Implementieren von MVVM hinzuzufügen.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged und INotifyCollectionChanged

Beide Schnittstellen werden in xamarin. Forms-Bindungen vollständig unterstützt. Im Gegensatz zu vielen XAML-basierten Frameworks können Benachrichtigungen über Eigenschafts Änderungen für Hintergrundthreads in xamarin. Forms (genauso wie WPF) ausgelöst werden, und die Bindungs-Engine wechselt ordnungsgemäß in den UI-Thread.

Außerdem unterstützen `SynchronziationContext` beide Umgebungen / und `async` `await` , um geeignete Thread Marshalling durchzuführen. WPF enthält die `Dispatcher` -Klasse für alle visuellen Elemente, xamarin. Forms verfügt über eine `Device.BeginInvokeOnMainThread` statische Methode, die auch verwendet werden `SynchronizationContext` kann (obwohl für plattformübergreifendes Programmieren bevorzugt).

- Xamarin. Forms enthält eine `ObservableCollection<T>` , die Sammlungs Änderungs Benachrichtigungen unterstützt.
- Sie können verwenden `BindingBase.EnableCollectionSynchronization` , um Thread übergreifende Updates für eine Sammlung zu aktivieren. Die API unterscheidet sich geringfügig von der WPF-Variation. [Weitere Informationen zur Verwendung finden Sie in der](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)Dokumentation.

## <a name="data-templates"></a>Datenvorlagen

Datenvorlagen werden in xamarin. Forms zum Anpassen des Renderings einer `ListView` Zeile (Zelle) unterstützt. Im Gegensatz zu WPF, `DataTemplate`das s für ein beliebiges Inhalts orientiertes Steuerelement verwenden kann, verwendet xamarin. `ListView`Forms diese zurzeit nur für. Die Vorlagen Definition kann Inline (der `ItemTemplate` -Eigenschaft zugewiesen) oder als Ressource in einer `ResourceDictionary`definiert werden.

Außerdem sind Sie nicht so flexibel wie Ihre WPF-Entsprechung.

1. Das root-Element von `DataTemplate` muss _immer_ ein `ViewCell` -Objekt sein.
2. Daten Trigger werden in einer Daten Vorlage vollständig unterstützt, müssen jedoch `DataType` eine Eigenschaft enthalten, die den Typ der Eigenschaft angibt, mit der der Trigger verknüpft ist.
3. `DataTemplateSelector`wird ebenfalls unterstützt, wird jedoch `DataTemplate` von abgeleitet und wird daher direkt der `ItemTemplate` -Eigenschaft (vs. `ItemTemplateSelector` in WPF) zugewiesen.

## <a name="itemscontrol"></a>ItemsControl

In xamarin. Forms ist kein integriertes `ItemsControl` Äquivalent zu einem vorhanden. es ist jedoch ein [benutzerdefiniertes für xamarin. Forms verfügbar](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Benutzer Steuerelemente

In WPF `UserControl`werden s verwendet, um einen Abschnitt der Benutzeroberfläche bereitzustellen, der das zugehörige Verhalten aufweist. In xamarin. Forms wird das `ContentView` für denselben Zweck verwendet. Beide unterstützen die Bindung und Einbindung in XAML.

## <a name="navigation"></a>Navigation

WPF enthält ein selten verwendetes `NavigationService` , das verwendet werden kann, um ein Browser ähnliches Navigations Feature bereitzustellen. Die meisten apps haben dies jedoch nicht gestört, und stattdessen `Window` wurden unterschiedliche Elemente oder verschiedene Abschnitte des Fensters zum Anzeigen von Daten verwendet.

Auf Telefongeräten sind häufig verschiedene _Bildschirme_ die Projekt Mappe, sodass xamarin. Forms Unterstützung für verschiedene Formen der Navigation bietet:

| Navigationsstil | Seitentyp |
|--- |--- |
|Stapel basiert (Push/Pop)|NavigationPage|
|Master/Detail|MasterDetailPage|
|Registerkarten|TabbedPage|
|Nach links/rechts schwenken|Carouselview|

Der `NavigationPage` ist der gängigste Ansatz, und jede Seite verfügt über `Navigation` eine-Eigenschaft, die verwendet werden kann, um Seiten in den und aus dem Navigations Stapel zu verschieben. Dies ist das nächstgelegene Äquivalent zu `NavigationService` dem in WPF gefundenen.

### <a name="url-navigation"></a>URL-Navigation

WPF ist eine Desktop orientierte Technologie, die Befehlszeilenparameter zum direkten Startverhalten annehmen kann. Xamarin. Forms kann [Deep URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) -Verknüpfungen verwenden, um zu einer Seite beim Start zu springen.
