---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF im Vergleich zu. Xamarin.Forms: Ähnlichkeiten und Unterschiede'
description: Dieses Dokument verglichen und Unterschiede aufgezeigt WPF in Xamarin.Forms. Es wird erläutert, Steuerelementvorlagen, XAML-bindungsinfrastruktur, Datenvorlagen, ItemsControl, UserControl, Navigation und URLs.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 232ddd5ea06891c70125adc9b8bf77f3e857dbd6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781968"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF im Vergleich zu. Xamarin.Forms: Ähnlichkeiten und Unterschiede

## <a name="control-templates"></a>Steuerelementvorlagen

WPF unterstützt das Konzept der *Steuerelementvorlagen* bieten die Visualisierung Anweisungen für ein Steuerelement (`Button`, `ListBox`usw..). Wie bereits erwähnt, Xamarin.Forms nutzt konkret _Rendering_ Klassen, die für diese Interaktion statt, die systemeigene Plattform (iOS, Android, usw.), um das Steuerelement visuell darzustellen.

Allerdings Xamarin.Forms _ist_ haben eine `ControlTemplate` Typ - es dient für Designumgebung `Page` Objekte. Es enthält eine Definition für eine `Page` , konsistente Inhalte bietet, aber ermöglicht es dem Benutzer der Seite zu ändern, Farben, Schriftarten usw. sogar hinzufügen Elemente aus, um sie an die Anwendung eindeutig zu machen.

Allgemeine Verwendungen für diese Dinge wie z. B. Authentifizierung Dialoge, fordert und einer standardisierten, aber Designfähige Seite aussehen und Verhalten bereitzustellen, die innerhalb der app angepasst werden kann. Als Teil dieser Unterstützung sind viele vertraut sind mit dem Namen WPF-Steuerelemente verwendet:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Es ist jedoch wichtig zu wissen, dass hierbei handelt es sich _nicht_ für denselben Zweck in Xamarin.Forms. Weitere Informationen zu dieser Funktion finden Sie die [Dokumentationsseite](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML wird als die deklarative Markupsprache für WPF und Xamarin.Forms verwendet werden. Meistens, die Syntax ist identisch – der Hauptunterschied besteht die Objekte, die den XAML-Diagrammen definiert/erstellt werden.

- Xamarin.Forms unterstützt die [XAML 2009-Spezifikation](/dotnet/framework/xaml-services/xaml-2009-language-features/); Dies erleichtert es, Daten zu definieren, wie z. B. `string`s, `int`s, usw., sowie Definieren von generischen Typen und übergeben von Argumenten an die Konstruktoren.

- Es gibt derzeit keine Möglichkeit XAML dynamisch geladen wird, wie WPF mit `XamlReader`. Sie erhalten die dieselbe grundlegende Funktionalität mit einer [NuGet-Paket](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) Obwohl.

### <a name="markup-extensions"></a>Markuperweiterungen

Xamarin.Forms unterstützt das XAML über Markuperweiterungen ähnlich wie WPF erweitern. Ausgegeben weist er die gleichen grundlegenden Bausteine:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Darüber hinaus enthält er `{x:Reference}` aus dem XAML 2009-Spezifikation und ein `{TemplateBinding}` Markuperweiterung für die spezielle Version des dient `ControlTemplate` von Xamarin.Forms unterstützt.

> [!WARNING]
> Die `ControlTemplate` Unterstützung stimmt nicht überein – Obwohl sie den gleichen Namen aufweist.

Xamarin.Forms unterstützt benutzerdefinierte Markuperweiterungen sowie -, aber die Implementierung unterscheidet sich etwas. In WPF, leiten Sie von `MarkupExtension` -eine abstrakte Basisklasse. In Xamarin.Forms, ersetzt, die mit einer Schnittstelle `IMarkupExtension` oder `IMarkupExtension<T>` flexibler ist.

WPF, wie die einzelne erforderliche Methode ist eine `ProvideValue` Methode, um den Wert aus der Markuperweiterung zurückzugeben.

## <a name="binding-infrastructure"></a>Bindungsinfrastruktur

Eines der grundlegenden Konzepte übernommen wird einer Bindung-Infrastruktur zur Verbindung mit .NET Dateneigenschaften Eigenschaften visueller Elemente. Dies ermöglicht z. B. MVVM architektonische Muster. Das Grundkonzept ist identisch: Sie haben eine bindbare Basisklasse [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), im WPF-Dies ist die [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) Klasse. Diese Basisklasse wird für alle Objekte, die als Ziele bei der Datenbindung einbezogen werden, als den Root-Vorgänger verwendet. Klicken Sie dann die abgeleiteten Klassen verfügbar machen [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) -Objekte, die als dem Sicherungsspeicher für Eigenschaftswerte fungieren (diese sind definiert als [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) Objekte in WPF).

### <a name="defining-bindable-properties"></a>Definieren von bindbare Eigenschaften

Die Definition für eine bindbare Eigenschaft in Xamarin.Forms ist identisch mit WPF:
1. Das Objekt muss abgeleitet `BindableObject`.
2. Es muss ein öffentliche statisches Feld des Typs `BindableProperty` zum Definieren der dahinter liegende Speicherschlüssel für die Eigenschaft deklariert.
3. Muss ein öffentlichen Instanz Eigenschaftenwrapper, `GetValue` und `SetValue` abrufen und ändern Sie den Eigenschaftswert.

Ein vollständiges Beispiel finden Sie unter [bindbare Eigenschaften in Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Angefügte Eigenschaften

Angefügte Eigenschaften sind eine Teilmenge der bindbare Eigenschaft, und sie funktionieren genauso in WPF ausführen. Der Hauptunterschied besteht darin, dass der Eigenschaftenwrapper Standardprojekt in diesem Fall ist, und mit einem Satz von statischen Get/Set-Methoden für die besitzende Klasse ersetzt. Finden Sie unter [angefügte Eigenschaften in Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) für Weitere Informationen.

### <a name="using-the-binding-engine"></a>Verwenden das Bindungsmodul

Der Prozess für die Verwendung des Bindungsmodul entspricht dem, wie es in WPF ist. Im Code-Behind durch Erstellen genutzt werden ein `Binding` Objekt an einem-Quellobjekt (beliebiger Typ .NET) und eine optionale Eigenschaft-Wert gebunden (wenn Standardprojekt wird das Quellobjekt als die Eigenschaft selbst – genau wie behandelt WPF). Anschließend können Sie `SetBinding` für ein beliebiges `BindableObject` , ordnen Sie die Bindung an eine `BindableProperty`.

Alternativ können Sie die Bindung Beziehung definieren, in der XAML mit dem `BindingExtension`. Er weist die gleichen grundlegenden Werte wie die Erweiterung in WPF.

Die Unterstützung der Bindung und das Modul ähneln mehr den Silverlight-Implementierung als WPF. Es gibt mehrere fehlende Funktionen, die nicht in Xamarin.Forms implementiert wurden:

- Es gibt keine Unterstützung für die folgenden Features in Bindungen:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - Weise
    - ValidatesOnExceptions
    - ValidationRules-Auflistung
    - XPath
    - Der XmlNamespaceManager
- `Binding.Mode` unterstützt keine `OneTime`, verwenden Sie stattdessen einfach `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Es gibt keine Unterstützung für `RelativeSource` Bindungen. In WPF können diese Binden an andere visuelle Elemente, die in XAML definiert. In Xamarin.Forms, kann diese Möglichkeit ebenfalls mithilfe von erreicht werden die `{x:Reference}` Markuperweiterung. Beispielsweise können angenommen, wir haben ein Steuerelement mit dem Namen "OtherControl", die Text-Eigenschaft aufweist, wir, wie folgt binden:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Dieselbe Funktion kann verwendet werden, für die `{RelativeSource Self}` Funktion. Aber es gibt keine Unterstützung zum Suchen von Vorgängern nach Typ (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Bindungskontext

In WPF können Sie definieren eine `DataContext` Eigenschaftswert, welche Reprents die Bindungsquelle Standardeinstellung. Wenn die Quelle für eine Bindung nicht definiert ist, wird dieser Eigenschaftswert verwendet. Der Wert wird in der visuellen Struktur, sodass es auf einer höheren Ebene definiert werden und dann von untergeordneten Elementen verwendet abwärts vererbt.

In Xamarin.Forms mit, dieser Funktion Firmenmaster ist, aber der Eigenschaftsname ist `BindingContext`.

#### <a name="value-converters"></a>Wertkonverter

Wertkonverter werden in Xamarin.Forms - genau wie WPF vollständig unterstützt. Die derselben Form "Schnittstelle" wird verwendet, aber Xamarin.Forms verfügt über die Schnittstelle definiert, der `Xamarin.Forms` Namespace.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM wird von WPF und Xamarin.Forms vollständig unterstützt.

WPF gehört eine integrierte `RoutedCommand` der manchmal verwendet wird. Xamarin.Forms verfügt über keine integrierte Befehle Unterstützung außerhalb der `ICommand` Schnittstellendefinition. Sie können eine Vielzahl von MVVM-Frameworks zum Hinzufügen der erforderlichen Basisklassen zum Implementieren der MVVM einschließen.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged und INotifyCollectionChanged

Beide Schnittstellen werden in Xamarin.Forms Bindungen vollständig unterstützt. Im Gegensatz zu vielen XAML-basierte Frameworks änderungsbenachrichtigungen für die Eigenschaft in Hintergrundthreads in Xamarin.Forms (wie WPF) ausgelöst werden können, und das Bindungsmodul geht ordnungsgemäß an den UI-Thread.

Beide Umgebungen darüber hinaus unterstützen `SynchronziationContext` und `async` / `await` , führen Sie den richtigen Thread zu marshallen. WPF enthält die `Dispatcher` Klasse auf alle visuellen Elemente Xamarin.Forms verfügt über eine statische Methode `Device.BeginInvokeOnMainThread` auch verwendet werden kann (obwohl `SynchronizationContext` für plattformübergreifende Codierung bevorzugt wird).

- Xamarin.Forms umfasst eine `ObservableCollection<T>` Auflistung änderungsbenachrichtigungen unterstützt.
- Sie können `BindingBase.EnableCollectionSynchronization` threadübergreifenden Aktualisierungen für eine Sammlung zu aktivieren. Die API weicht geringfügig von der WPF-Variante [Überprüfen Dokumente zur Verwendung](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/).

## <a name="data-templates"></a>Datenvorlagen

Datenvorlagen werden in Xamarin.Forms, um die Darstellung anpassen unterstützt eine `ListView` Zeile (Zelle). Im Gegensatz zu WPF die verwenden `DataTemplate`s für alle Inhalte-oriented Steuerelement Xamarin.Forms derzeit nur verwendet werden, damit `ListView`. Die Vorlagendefinition Inline definiert werden kann (zugewiesene der `ItemTemplate` Eigenschaft), oder als Ressource in eine `ResourceDictionary`.

Darüber hinaus sind sie nicht so flexibel wie ihre WPF-Entsprechung.

1. Das Stammelement der `DataTemplate` müssen _immer_ werden ein `ViewCell` Objekt.
2. Datentrigger werden in einer Vorlage von Daten vollständig unterstützt, aber muss enthalten eine `DataType` Eigenschaft gibt den Typ der Eigenschaft, die der Trigger zugeordnet ist.
3. `DataTemplateSelector` wird auch unterstützt, aber leitet sich von `DataTemplate` und wird daher nur zugewiesen direkt an die `ItemTemplate` Eigenschaft (im Vergleich zu `ItemTemplateSelector` in WPF).

## <a name="itemscontrol"></a>ItemsControl

Es ist keine integrierte Equivelent auf eine `ItemsControl` in Xamarin.Forms; ist jedoch ein [benutzerdefinierte eine für Xamarin.Forms verfügbaren hier](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Benutzersteuerelemente

In WPF `UserControl`s werden verwendet, um einen Abschnitt der Benutzeroberfläche bereitgestellt werden, die Verhalten zugeordnet ist. In Xamarin.Forms, verwenden wir die `ContentView` für denselben Zweck. Beide unterstützen Bindung und der Einschluss in XAML.

## <a name="navigation"></a>Navigation

WPF enthält ein selten verwendeter `NavigationService` konnte die verwendet werden, um eine Funktion "browserbasierte" Navigation bereitstellen. Die meisten apps nicht kümmern, dabei jedoch als auch instead verwendet verschiedene `Window` Elemente oder andere Abschnitte des Fensters zum Anzeigen von Daten.

Für Phone-Geräte, die verschiedene _Bildschirme_ sind häufig die Lösung und daher Xamarin.Forms umfasst Unterstützung für mehrere Formen der Navigation:

| Navigation-Stil | Seitentyp |
|--- |--- |
|Stapelbasierte (Push-/Pop)|NavigationPage|
|Master/Detail|MasterDetailPage|
|Tabstopps|TabbedPage|
|Streichen Sie nach links/rechts|CarouselView|

Die `NavigationPage` die am häufigsten verwendete Ansatz ist, und jede Seite hat eine `Navigation` Eigenschaft, die mithilfe von Push übertragen oder pop-Seiten, die ein- und ausschalten Navigationsstapel verwendet werden kann. Dies ist die nächstgelegenen Equivelent auf die `NavigationService` in WPF gefunden.

### <a name="url-navigation"></a>URL-navigation

WPF ist eine Technologie zur Desktop-orientierten und zum Weiterleiten von Startverhalten Befehlszeilenparameter akzeptieren. Xamarin.Forms können [deep links URL](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) springen zu einer Seite beim Start.
