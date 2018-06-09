---
title: Das Model-View-ViewModel-Muster
description: In diesem Kapitel wird erläutert, wie die mobilen Anwendung für eShopOnContainers MVVM-Muster verwendet, um die Logik für die Business und Darstellung der app aus der Benutzeroberfläche ordnungsgemäß zu trennen.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fe2cace6a0fc3a1d901f55556eed09380f8f2006
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245431"
---
# <a name="the-model-view-viewmodel-pattern"></a>Das Model-View-ViewModel-Muster

Die entwicklererfahrung Xamarin.Forms beinhaltet in der Regel erstellen eine Benutzeroberfläche in XAML, und fügen Code-Behind, die für die Benutzeroberfläche ausgeführt wird. Wie apps geändert werden und in Größe und Bereich vergrößert, können komplexe Wartungsprobleme auftreten. Zu diesen Problemen zählen die enge Kopplung zwischen den Benutzeroberflächen-Steuerelemente und die Geschäftslogik, die wodurch die Kosten für UI Änderungen und UnitTests solcher Code ist schwierig gestalten erhöht wird.

Das Model-View-ViewModel (MVVM)-Muster hilft, um die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI) ordnungsgemäß zu trennen. Eine saubere Trennung zwischen der Anwendungslogik und der Benutzeroberfläche verwalten können Sie um zahlreiche Entwicklungsprobleme zu beheben, und erleichtert werden kann eine Anwendung zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und erlaubt Entwicklern und UI-Designer mehr einfacher zusammenarbeiten, bei der Entwicklung ihrer jeweiligen Teile einer app.

## <a name="the-mvvm-pattern"></a>Das Muster MVVM

Es gibt drei Hauptkomponenten im Muster MVVM: das Modell, Ansicht und das Modell anzeigen. Jeweils einen unterschiedlichen Zweck. Abbildung 2 – 1 zeigt die Beziehungen zwischen den drei Komponenten.

![](mvvm-images/mvvm.png "Das Muster MVVM")

**Abbildung 2: 1**: die MVVM-Muster

Zusätzlich zu den Aufgaben der einzelnen Komponenten, ist es auch wichtig zu verstehen, wie sie miteinander interagieren. Auf hoher Ebene die Ansicht "kennt" das Ansichtsmodell und das Ansichtsmodell "kennt" das Modell jedoch das Modell ist keine Kenntnis von den Ansichtsmodell und das Ansichtsmodell ist nicht der Sicht bekannt. Daher das Ansichtsmodell isoliert die Sicht aus dem Modell und das Modell, unabhängig von der Ansicht entwickeln kann.

Die Vorteile der Verwendung des Musters MVVM sind wie folgt:

-   Ist eine vorhandenes Modell-Implementierung, die vorhandene Geschäftslogik kapselt, kann es schwierig oder riskant, um ihn zu ändern. In diesem Szenario das Ansichtsmodell fungiert als Adapter für die Modellklassen und ermöglicht es Ihnen, wichtige Änderungen an der Model-Code vermeiden.
-   Entwickler können die Komponententests für die Ansicht und das Modell erstellen, ohne Sie in der Ansicht. Komponententests für das ViewModel Formularvorlagen genau die gleiche Funktionalität wie von der Sicht verwendet.
-   Die Benutzeroberfläche die app kann ohne Änderung des Codes überarbeitet werden, vorausgesetzt, dass die Sicht vollständig in XAML implementiert wird. Daher sollte eine neue Version der Ansicht mit der vorhandenen Ansichtsmodell funktionieren.
-   Designer und Entwickler können unabhängig voneinander und gleichzeitig ihre Komponenten während des Entwicklungsprozesses arbeiten. Entwickler können für die Sicht konzentrieren, während der Entwickler die Ansichtsmodell und die Modellkomponenten arbeiten können.

Der Schlüssel zum effektiven Verwendung von MVVM liegt im Verständnis der Vorgehensweise zum Verlagern von app-Code in die richtige Klassen und zu verstehen, wie die Klassen interagieren. In den folgenden Abschnitten werden die Aufgaben der einzelnen Klassen im MVVM Muster erläutert.

### <a name="view"></a>Ansicht

Die Ansicht ist dafür verantwortlich zu definieren, die Struktur, die Layout und die Darstellung der sieht des Benutzers auf dem Bildschirm. Im Idealfall jede Ansicht in XAML wird mit eingeschränkten Code-Behind definiert, die nicht von Geschäftslogik enthält. Jedoch kann der Code-Behind in einigen Fällen Benutzeroberflächen-Logik enthalten, die visuelle Verhalten, das schwer implementiert zu in XAML, wie z. B. Animationen express ist.

In einer Xamarin.Forms-Anwendung, eine Sicht ist in der Regel eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-abgeleitet oder [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-Klasse. Sichten können jedoch auch durch eine Datenvorlage dargestellt werden, die angibt, die Elemente der Benutzeroberfläche verwendet werden, um ein Objekt visuell darzustellen, wenn er angezeigt wird. Eine Datenvorlage als Sicht keine Code-Behind und dient zum Binden an eine bestimmte Ansicht Modelltyp.

> [!TIP]
> Vermeiden Sie aktivieren und Deaktivieren von UI-Elemente in den Code-Behind. Sicherstellen Sie, dass die Modelle anzeigen sind verantwortlich für die Definition von logischen Zustandsänderungen, die Auswirkungen auf einige Aspekte der Ansicht anzeigen, z. B., ob ein Befehl verfügbar ist oder ein Hinweis, dass ein Vorgang aussteht. Aus diesem Grund aktivieren Sie und deaktivieren Sie die Elemente der Benutzeroberfläche von Bindung zum Anzeigen von Modelleigenschaften, statt aktivieren und deaktivieren sie im Code-Behind.

Es gibt mehrere Optionen zum Ausführen des Codes auf das Ansichtsmodell in Reaktion auf Interaktionen für die Sicht, z. B. eine Schaltfläche klicken oder die Elementauswahl. Wenn ein Steuerelement Befehle, das Steuerelement des unterstützt `Command` Eigenschaft kann Daten gebunden werden ein `ICommand` -Eigenschaft für das Modell anzeigen. Wenn das Steuerelement-Befehl aufgerufen wird, wird der Code in das Ansichtsmodell ausgeführt werden. Zusätzlich zu den Befehlen Verhalten angefügt werden können, um ein Objekt in der Ansicht und überwachen können entweder einen Befehl aufgerufen werden oder ein Ereignis ausgelöst werden soll. Reaktion, klicken Sie dann das Verhalten aufrufen kann ein `ICommand` auf das Modell anzeigen oder eine Methode für das Modell anzeigen.

### <a name="viewmodel"></a>ViewModel

Das Ansichtsmodell implementiert, Eigenschaften und Befehle in dem die Sicht kann Daten gebunden werden sollen, und benachrichtigt den Überblick über alle Zustandsänderungen über Änderungsbenachrichtigungsereignisse. Die Eigenschaften und Befehle, die das Ansichtsmodell bereitstellt, definieren die Funktionalität über die Benutzeroberfläche angeboten werden, aber die Ansicht bestimmt, wie diese Funktionalität, die angezeigt werden.

> [!TIP]
> Die Reaktionsfähigkeit der Benutzeroberflächenautomatisierungs mit asynchronen Vorgängen. Mobile apps sollten im UI-Thread "Blockierung aufgehoben", um die Benutzer wahrgenommene Leistung zu verbessern. Deshalb im Modell anzeigen, verwenden Sie asynchrone Methoden für e/a-Vorgänge und Auslösen von Ereignissen, um Ansichten der eigenschaftenänderungen asynchron zu benachrichtigen.

Das Ansichtsmodell ist auch verantwortlich für die Koordinierung der Ansicht Interaktionen mit alle Modellklassen, die erforderlich sind. In der Regel besteht eine 1: n-Beziehung zwischen der Ansichtsmodell und den Modellklassen. Das Ansichtsmodell empfiehlt sich direkt auf die Sicht Modellklassen verfügbar zu machen, damit die Steuerelemente in der Ansicht der Daten direkt an diese gebunden werden können. In diesem Fall müssen die Modellklassen entworfen werden, die Datenbindung zu unterstützen und Ändern von Benachrichtigungsereignissen.

Jedes Ansichtsmodell enthält Daten aus einem Modell in ein Formular, das die Sicht problemlos nutzen kann. Um dies zu erreichen, führt das Ansichtsmodell manchmal Datenkonvertierung. Platzieren diese Datenkonvertierung in das Ansichtsmodell ist eine gute Idee, da es sich um Eigenschaften bereitstellt, denen die Sicht gebunden werden kann. Das Ansichtsmodell kann z. B. die Werte von zwei Eigenschaften für die Anzeige von der Sicht erleichtern kombinieren.

> [!TIP]
> Datenkonvertierungen in einer Konvertierungsebene zu zentralisieren. Es ist auch möglich, Konverter als eine separate Konvertierung Datenschicht zu verwenden, die zwischen dem Modell anzeigen und die Sicht befindet. Dies kann z. B. erforderlich sein, wenn Daten erfordert spezielle Formatierung aus, die nicht das Ansichtsmodell bereitstellt.

In der Reihenfolge für das ViewModel zur Teilnahme an bidirektionale Datenbindung mit der Ansicht, deren Eigenschaften auslösen müssen die `PropertyChanged` Ereignis. Ansichtsmodelle erfüllen diese Anforderung durch Implementieren der `INotifyPropertyChanged` Schnittstelle, und durch das Auslösen der `PropertyChanged` Ereignis aus, wenn eine Eigenschaft geändert wird.

Bei Auflistungen, die Sicht benutzerfreundliche `ObservableCollection<T>` bereitgestellt wird. Diese Auflistung implementiert die Sammlung wurde geändert. Benachrichtigung, den Entwickler zu implementieren sind die `INotifyCollectionChanged` Schnittstelle Auflistungen.

### <a name="model"></a>Modell

Modellklassen sind nicht sichtbare Gesamtwerte-Klassen, die die app-Daten zu kapseln. Aus diesem Grund kann das Modell der Darstellung der app-Domänenmodell, in der Regel enthält ein Datenmodell zusammen mit Business und Validierung Logik betrachtet werden. Beispiele für Modellobjekte sind datenübertragungsobjekte (DTOs), Plain Old CLR Objects (POCOs), und generierte Entität und Proxy-Objekte.

Modellklassen dienen in der Regel in Verbindung mit Diensten oder Repositorys, die Zugriff auf Daten und das Zwischenspeichern zu kapseln.

## <a name="connecting-view-models-to-views"></a>Herstellen einer Verbindung Ansichtsmodelle mit Ansichten

Modelle anzeigen können über die Datenbindungsfunktionen des Xamarin.Forms mit Ansichten verbunden sein. Es gibt viele Ansätze, die zum Erstellen von Sichten und Anzeigen von Modellen und ordnen Sie sie zur Laufzeit verwendet werden können. Diese Ansätze werden in zwei Kategorien eingeteilt, bekannt als Zusammenstellung der ersten und der ersten Zusammenstellung der Modell. Auswählen zwischen Zusammenstellung der ersten und zeigen Sie die erste Komposition Modell ein Problem des Einstellung und die Komplexität ist. Alle Ansätze verwenden jedoch das gleiche Ziel, das für die Sicht auf einem Ansichtsmodell seine BindingContext-Eigenschaft zugewiesen haben.

Ansicht erste Zusammensetzung der app konzeptionell Ansichten besteht, die Verbindung mit der Modelle anzeigen, denen sie abhängig sind. Der Hauptvorteil dieses Ansatzes ist, dass einfach erstellen lose miteinander verknüpfte Einheit testfähig apps erschwert die Modelle anzeigen Schreibberechtigung keine Abhängigkeit mehr von den Ansichten selbst. Es ist auch einfach, zu verstehen die Struktur der app, befolgen die visuelle Struktur, statt zum Verfolgen der codeausführung, um zu verstehen, wie Klassen erstellt und verknüpft werden. Darüber hinaus wird der Xamarin.Forms Navigationssystem, die verantwortlich für das Erstellen von Seiten navigiert wird, wodurch eine Zusammenstellung der ersten Modell komplex und mit der Plattform falsch ausgerichtete Ansicht erste Konstruktion ausgerichtet.

Ansicht Modell erste Zusammensetzung der app konzeptionell Modelle anzeigen, mit einem Dienst, die verantwortlich für die Suche nach der Ansicht für ein Modell der Sicht besteht. Modell erste Zusammenstellung idealer erwarten, dass einige Entwickler, die seit die Erstellung der Sicht, sodass sie legen den Schwerpunkt auf der logischen nicht-UI-Struktur der app abstrahiert werden kann. Darüber hinaus können sie Modelle anzeigen von anderen Modelle anzeigen erstellt werden. Allerdings diese Vorgehensweise ist oft komplex und schwierig zu verstehen, wie die verschiedenen Teile der app erstellt und verknüpft werden eingesetzt.

> [!TIP]
> Halten Sie Modelle anzeigen und Ansichten unabhängig. Die Bindung an Ansichten an eine Eigenschaft in einer Datenquelle muss die Sicht principal Abhängigkeit von der entsprechenden Ansichtsmodell. Insbesondere nicht Referenztypen anzeigen, wie z. B. [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) und [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), aus der Modelle anzeigen. Anhand der Prinzipien können Modelle anzeigen isoliert, wodurch die Wahrscheinlichkeit von Softwarefehlern vom Bereich einschränken getestet werden.

In den folgenden Abschnitten werden die Hauptverfahren für das Verbinden von Modelle anzeigen von Ansichten erläutert.

### <a name="creating-a-view-model-declaratively"></a>Erstellen ein Ansichtsmodell deklarativ

Die einfachste Vorgehensweise ist für die Ansicht deklarativ seiner entsprechenden Ansichtsmodell in XAML instanziiert. Wenn die Sicht erstellt wird, wird auch das entsprechende Modell Ansichtsobjekt erstellt werden. Dieser Ansatz wird in das folgende Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Wenn die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) erstellt werden, wird eine Instanz von der `LoginViewModel` wird automatisch erstellt und festgelegt, wie der Sicht [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Diese deklarativen Erstellung und Zuweisung von View-Modell, indem Sie die Sicht hat den Vorteil, einfach ist, aber hat dies den Nachteil, dass sie einen (parameterlosen) Standardkonstruktor in das Ansichtsmodell erfordert.

### <a name="creating-a-view-model-programmatically"></a>Erstellen ein Ansichtsmodell programmgesteuert

Eine Sicht kann Code verfügen, in der Code-Behind-Datei, die in das Ansichtsmodell zugewiesen wird, führt seine [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Eigenschaft. Dies geschieht häufig in der Ansicht-Konstruktor wie im folgenden Codebeispiel gezeigt:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Die programmgesteuerte Erstellung und Zuweisung von das Ansichtsmodell innerhalb der Ansicht CodeBehind hat den Vorteil, dass es einfacher ist. Der wichtigste Nachteil dieses Ansatzes ist jedoch, dass die Sicht alle erforderlichen Abhängigkeiten der Sicht Modell bereitstellen muss. Verwenden einen abhängigkeitseinschleusungscontainer können losen Kopplung zwischen der Ansicht und Ansichtsmodell verwalten. Weitere Informationen finden Sie unter [Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Erstellen Sie eine Sicht definiert, wie eine Datenvorlage

Eine Sicht kann als eine Datenvorlage definiert und einen Modelltyp Sicht zugeordnet werden. Datenvorlagen können als Ressourcen definiert werden können, oder es Inline definiert, in das Steuerelement, das das Ansichtsmodell angezeigt wird. Der Inhalt des Steuerelements ist die Modellinstanz anzeigen und die Datenvorlage wird verwendet, um es darzustellen. Dieses Verfahren ist ein Beispiel für eine Situation, in der das Ansichtsmodell zuerst gefolgt von der Erstellung der Sicht instanziiert wird.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Erstellen ein Ansichtsmodell automatisch mit Locator Modell anzeigen

Ein Ansicht Modell Locator ist eine benutzerdefinierte Klasse, die die Instanziierung des Modelle anzeigen und ihre Zuordnung mit Ansichten verwaltet. In der mobilen app eShopOnContainers der `ViewModelLocator` -Klasse verfügt über eine angefügte Eigenschaft `AutoWireViewModel`, dient außerdem zur Sichten Ansichtsmodelle zugeordnet werden. In der Ansicht XAML ist angefügte Eigenschaft festgelegt, auf "true", um anzugeben, dass das Ansichtsmodell in die Ansicht automatisch verbunden werden sollen, wie im folgenden Codebeispiel wird gezeigt:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

Die `AutoWireViewModel` Eigenschaft ist eine bindbare Eigenschaft, die auf "false" initialisiert wird, und wenn sich der Wert ändert, die `OnAutoWireViewModelChanged` Ereignishandler wird aufgerufen. Diese Methode löst das Ansichtsmodell für die Sicht. Im folgenden Codebeispiel wird veranschaulicht, wie dies erreicht wird:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

Die `OnAutoWireViewModelChanged` Methode versucht, die Verwendung eines Ansatzes konventionsbasierte Ansichtsmodell zu beheben. Diese Konvention wird Folgendes vorausgesetzt:

-   Ansichtsmodelle befinden sich in derselben Assembly wie Ansichtstypen.
-   Ansichten werden ein. Ansichten untergeordneter Namespace.
-   Ansichtsmodelle ein. ViewModels untergeordneter Namespace.
-   Ansichtsmodell Namen entsprechen den Sichtnamen und "ViewModel" enden.

Schließlich die `OnAutoWireViewModelChanged` Methode legt die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des sichttyps für den Modelltyp aufgelöst anzeigen. Weitere Informationen zum Beheben von der Sicht Modelltyp finden Sie unter [Auflösung](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Dieser Ansatz hat den Vorteil, dass eine app verfügt über eine einzelne Klasse, die für die Instanziierung des Modelle anzeigen und die Verbindung mit Ansichten zuständig ist.

> [!TIP]
> Verwenden Sie zur Vereinfachung der Ersetzung Locator Modell anzeigen. Ansicht Modell Locator kann auch als Ausgangspunkt Ersatz für alternative Implementierungen von Abhängigkeiten, z. B. für Einheit testen bzw. einen anderen Entwurf Zeitdaten verwendet werden.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Aktualisierbare Sichten als Reaktion auf Änderungen in der zugrunde liegenden anzeigen, Modell oder ein

Alle Ansichtsmodell und Modellklassen, die für eine Sicht zugänglich sind sollten implementieren die `INotifyPropertyChanged` Schnittstelle. Implementieren diese Schnittstelle in einem Modell anzeigen oder eine Modellklasse kann die Klasse für änderungsbenachrichtigungen auf datengebundene Steuerelemente in der Ansicht bereitstellen, wenn der zugrunde liegenden Eigenschaftswert ändert.

Apps sollten für die korrekte Verwendung von änderungsbenachrichtigung, so entworfen werden, indem Sie die folgenden Anforderungen erfüllen:

-   Immer durch das Auslösen einer `PropertyChanged` -Ereignis, wenn eine öffentliche Eigenschaft-Wert geändert wird. Sollte nicht davon ausgegangen, durch das Auslösen der `PropertyChanged` Ereignis kann aufgrund von Kenntnisse wie XAML-Bindung erfolgt ignoriert werden.
-   Immer durch das Auslösen einer `PropertyChanged` Ereignis für eine beliebige berechnete Eigenschaften, deren Werte werden durch andere Eigenschaften in der Ansicht verwendet, Modell oder ein.
-   Immer durch das Auslösen der `PropertyChanged` Ereignis am Ende der Methode, besteht eine Eigenschaft ändern, oder wenn das Objekt in einem abgesicherten Zustand befinden bezeichnet wird. Durch das Auslösen des Ereignisses unterbricht den Vorgang, indem Sie Ereignishandler synchron aufrufen. In diesem Fall in der Mitte eines Vorgangs kann es das Objekt, das Rückruffunktionen verfügbar, wird in einem unsicheren, teilweise aktualisierten Zustand. Darüber hinaus ist es möglich, dass kaskadierende Änderungen, die von ausgelöst werden `PropertyChanged` Ereignisse. Kaskadierende Änderungen erfordern in der Regel Updates abgeschlossen sein, bevor die kaskadierende Änderung auszuführende sicher ist.
-   Nie durch das Auslösen einer `PropertyChanged` -Ereignis, wenn die Eigenschaft nicht geändert wird. Dies bedeutet, dass Sie die alten und neuen Werte vor dem Auslösen vergleichen, müssen die `PropertyChanged` Ereignis.
-   Nie durch das Auslösen der `PropertyChanged` Ereignis bei einem Ansichtsmodell-Konstruktor, wenn Sie eine Eigenschaft initialisieren. Datengebundene Steuerelemente in der Sicht werden nicht zum Empfangen von änderungsbenachrichtigungen an diesem Punkt abonniert haben.
-   Auslösen von nie mehr als eine `PropertyChanged` Ereignis mit dem gleichen Argument der Eigenschaft Name innerhalb einer einzelnen synchronen Aufruf eine öffentliche Methode einer Klasse. Angenommen, ein `NumberOfItems` -Eigenschaft, deren Sicherungsspeicher der `_numberOfItems` Feld, wenn eine Methode inkrementiert `_numberOfItems` 50 Mal während der Ausführung einer Schleife, sie sollten nur auslösen, änderungsbenachrichtigung für die Eigenschaft auf die `NumberOfItems` einmal, Eigenschaft Nachdem die gesamte Arbeit abgeschlossen ist. Für asynchrone Methoden Auslösen der `PropertyChanged` -Ereignis für einen angegebenen Eigenschaftsnamen in jedem synchronen Segment einer Kette von asynchronen Fortsetzung.

Der eShopOnContainers mobile app verwendet die `ExtendedBindableObject` Klasse bereitstellen änderungsbenachrichtigungen, die im folgenden Codebeispiel dargestellt ist:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Der Xamarin.Form [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) -Klasse implementiert die `INotifyPropertyChanged` Schnittstelle, und bietet eine [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) Methode. Die `ExtendedBindableObject` -Klasse stellt die `RaisePropertyChanged` Eigenschaft Instanzenmethode änderungsbenachrichtigung und in diesem Fall verwendet die Funktionalität von bereitgestellten der `BindableObject` Klasse.

Jede Ansicht Modellklasse in der mobilen Anwendung für eShopOnContainers leitet sich von der `ViewModelBase` -Klasse, die wiederum von abgeleitet ist die `ExtendedBindableObject` Klasse. Jede Ansicht Modellklasse aus diesem Grund verwendet der `RaisePropertyChanged` Methode in der `ExtendedBindableObject` Klasse, um die Eigenschaft änderungsbenachrichtigung bereitstellt. Im folgenden Codebeispiel wird veranschaulicht, wie die mobilen Anwendung für eShopOnContainers änderungsbenachrichtigung für die Eigenschaft mit einem Lambdaausdruck aufruft:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Beachten Sie, dass einen geringfügiger Leistungsabfall, Kosten, da der Lambda-Ausdruck wurde ausgewertet wird für jeden Aufruf mit einem Lambda-Ausdruck auf diese Weise umfasst. Obwohl die Leistungskosten klein ist und eine app würden normalerweise nicht beeinträchtigt, können Kosten entstehen, wenn vorliegen, dass viele Benachrichtigungen zu ändern. Der Vorteil dieses Ansatzes ist jedoch, dass es Kompilierzeit typsicherheit und umgestaltungsunterstützung beim Umbenennen von Eigenschaften zur Verfügung.

## <a name="ui-interaction-using-commands-and-behaviors"></a>Interaktion mit der Benutzeroberfläche mithilfe von Befehlen und Verhalten

Bei mobilen apps werden Aktionen in der Regel als Antwort auf eine Benutzeraktion, z. B. auf eine Schaltfläche aufgerufen werden, die durch einen Ereignishandler erstellen, in der Code-Behind-Datei implementiert werden können. Allerdings im Muster MVVM die Verantwortung für die Implementierung der Aktions liegt, mit dem Modell anzeigen, und platzieren Code im Code-Behind sollte vermieden werden.

Befehle bieten eine einfache Möglichkeit, die Aktionen darstellen, die an Steuerelemente in der Benutzeroberfläche gebunden werden kann. Sie kapselt den Code, der die Aktion implementiert und mit Ihnen können sie aus der visuellen Darstellung in der Ansicht entkoppelt. Xamarin.Forms enthält Steuerelemente, die deklarativ mit einem Befehl verbunden werden können, und diese Steuerelemente werden der Befehl aufgerufen, wenn der Benutzer mit dem Steuerelement interagiert.

Verhalten ermöglichen außerdem Steuerelemente deklarativ mit einem Befehl verbunden sein. Verhaltensweisen können jedoch verwendet werden, eine Aktion aufrufen, die eine Reihe von Ereignissen, die ausgelöst wird, von einem Steuerelement zugeordnet ist. Verhalten sind daher viele der gleichen Szenarios wie Steuerelemente Befehl aktiviert und bietet ein höheres Maß an Flexibilität und Kontrolle beziehen. Darüber hinaus können Verhaltensweisen auch Befehlsobjekte oder Methoden mit Steuerelementen zuordnen, die für die Interaktion mit Befehle nicht speziell entwickelt wurden, verwendet werden.

### <a name="implementing-commands"></a>Implementieren von Befehlen

Modelle anzeigen in der Regel verfügbar zu machen Befehlseigenschaften für die Bindung aus der Sicht, die Objektinstanzen, implementieren die `ICommand` Schnittstelle. Geben Sie eine Reihe von Kontrollmechanismen Xamarin.Forms eine `Command` -Eigenschaft, die Daten ausgetauscht werden kann gebunden werden, um ein `ICommand` Objekt des Modells anzeigen. Die `ICommand` Schnittstelle definiert einen `Execute` -Methode, die den Vorgang selbst kapselt, ein `CanExecute` Methode, die angibt, ob der Befehl aufgerufen werden kann, und ein `CanExecuteChanged` das Ereignis tritt bei Änderungen, die Einfluss auf, ob auftreten der Befehl ausgeführt werden soll. Die [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) und [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Klassen, die bereitgestellt werden, indem Sie Xamarin.Forms, implementieren die `ICommand` -Schnittstelle, in denen `T` ist der Typ der Argumente für `Execute`und `CanExecute`.

Innerhalb eines Modells anzeigen, muss ein Objekt des Typs [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) oder [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) für jede öffentliche Eigenschaft im Modell Sicht vom Typ `ICommand`. Die `Command` oder `Command<T>` -Konstruktor erfordert ein `Action` Callback-Objekt, das aufgerufen wurde die `ICommand.Execute` Methode aufgerufen wird. Die `CanExecute` -Methode ist eine optionale Konstruktorparameter und ist ein `Func` zurückgibt eine `bool`.

Der folgende code zeigt, wie eine [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) -Instanz, die einen Befehl Register darstellt, erstellt wird, indem ein Delegat, der die `Register` Modell anzuzeigen:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Der Befehl wird verfügbar gemacht, auf die Sicht über eine Eigenschaft, die einen Verweis auf ein `ICommand`. Wenn die `Execute` Methode aufgerufen wird die [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) -Objekts können sie einfach den Aufruf der Methode in das Modell anzeigen, über der Delegat, der im angegebenen leitet die `Command` Konstruktor.

Eine asynchrone Methode kann durch einen Befehl aufgerufen werden, mithilfe der `async` und `await` Schlüsselwörter, wenn der Befehl unter Angabe `Execute` delegieren. Dies bedeutet, dass der Rückruf eine `Task` und abgewartet werden soll. Z. B. der folgende code zeigt, wie eine [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) -Instanz, die einen Befehl zum Anmelden darstellt, erstellt wird, indem ein Delegat, der die `SignInAsync` Modell anzuzeigen:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parameter können übergeben werden, um die `Execute` und `CanExecute` Aktionen mithilfe der [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Klasse, um den Befehl zu instanziieren. Z. B. der folgende code zeigt, wie eine `Command<T>` Instanz wird verwendet, um anzugeben, dass die `NavigateAsync` Methode wird ein Argument des Typs benötigt `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

Sowohl die [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) und [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Klassen, die der Delegat, der die `CanExecute` Methode in jedem Konstruktor ist optional. Wenn ein Delegat nicht angegeben ist, die `Command` zurück `true` für `CanExecute`. Das Ansichtsmodell kann jedoch eine Änderung des Befehls angeben `CanExecute` Status durch Aufrufen der `ChangeCanExecute` Methode für die `Command` Objekt. Dies bewirkt, dass die `CanExecuteChanged` Ereignis ausgelöst wurde. Alle Steuerelemente in der Benutzeroberfläche, die an den Befehl gebunden sind werden dann deren Aktivierungsstatus die Verfügbarkeit des Befehls von datengebundenen entsprechend aktualisiert.

#### <a name="invoking-commands-from-a-view"></a>Aufrufen von Befehlen aus einer Sicht

Das folgende Codebeispiel zeigt, wie eine [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) in der `LoginView` bindet an die `RegisterCommand` in der `LoginViewModel` Klasse, indem eine [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Instanz:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Ein Befehlsparameter kann auch optional definiert werden mithilfe der [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) Eigenschaft. Der Typ des erwarteten Arguments wird angegeben, der `Execute` und `CanExecute` Methoden als Ziel. Die [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) den Zielbefehl automatisch aufgerufen, wenn der Benutzer mit dem angefügten Steuerelement interagiert. Der Befehlsparameter, falls vorhanden, wird als Argument übergeben werden, an des Befehls `Execute` delegieren.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementieren von Verhalten

Verhalten ermöglichen, Benutzeroberflächen-Steuerelemente hinzugefügt werden, ohne dass Unterklasse diese Funktionalität. Stattdessen wird die Funktionalität in einer Verhaltensklasse implementiert und an das Steuerelement verbunden, als wäre er Teil des Steuerelements selbst. Verhalten ermöglichen es Ihnen, Code zu implementieren, die normalerweise als CodeBehind, schreiben, da sie direkt mit der API des Steuerelements, so interagiert, werden deutlicher an das Steuerelement angefügt, und können für die Wiederverwendung über mehr als eine Sicht oder eine app gepackt. Im Kontext des MVVM sind Verhaltensweisen hilfreich bei der für die Verbindung von Steuerelementen mit Befehlen.

Ein Verhalten, das an ein Steuerelement über angefügte Eigenschaften zugeordnet ist, wird als bezeichnet ein *angefügten Verhalten*. Das Verhalten können Sie die API verfügbar gemacht, der das Element an das es angefügt ist, um Funktionen, die steuern, oder andere Steuerelemente, in der visuellen Struktur der Sicht hinzuzufügen. Die mobile app eShopOnContainers enthalten die `LineColorBehavior` Klasse, die ein Verhalten angefügt ist. Weitere Informationen zu diesem Verhalten finden Sie unter [Anzeigen von Validierungsfehlern](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Eine Xamarin.Forms-Verhaltensweise ist eine Klasse, die von abgeleitet ist die [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) oder [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) -Klasse, in denen `T `ist der Typ des Steuerelements an, das Verhalten gelten soll. Diese Klassen bieten `OnAttachedTo` und `OnDetachingFrom` -Methoden, die Logik bereitstellen, die ausgeführt wird, wenn das Verhalten ist angefügt und von Steuerelementen getrennt überschrieben werden sollte.

In der mobilen app eShopOnContainers der `BindableBehavior<T>` Klasse leitet sich von der [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) Klasse. Der Zweck der `BindableBehavior<T>` Klasse ist Xamarin.Forms Verhaltensweisen eine Basisklasse bereit, die erfordern die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des Verhaltens, das für das angefügte Steuerelement festgelegt werden.

Die `BindableBehavior<T>` -Klasse stellt eine überschreibbare `OnAttachedTo` Methode, die festlegt der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) das Verhalten und eine überschreibbare `OnDetachingFrom` -Methode, die bereinigt die `BindingContext`. Darüber hinaus die Klasse speichert einen Verweis auf das angefügte Steuerelement in der `AssociatedObject` Eigenschaft.

Die mobile app eShopOnContainers enthält eine `EventToCommandBehavior` -Klasse, die als Antwort auf ein Ereignis auftritt, einen Befehl ausführt. Diese Klasse leitet sich von der `BindableBehavior<T>` Klasse, damit das Verhalten kann zum Binden und Ausführen einer `ICommand` Angaben von eine `Command` Eigenschaft, wenn das Verhalten genutzt wird. Das folgende Codebeispiel zeigt die `EventToCommandBehavior`-Klasse:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

Die `OnAttachedTo` und `OnDetachingFrom` Methoden zum Registrieren und Aufheben der Registrierung eines ereignishandlers für das Ereignis definiert, der `EventName` Eigenschaft. Klicken Sie dann, wenn das Ereignis ausgelöst wird, die `OnFired` Methode wird aufgerufen, der den Befehl ausgeführt.

Der Vorteil der Verwendung der `EventToCommandBehavior` zum Ausführen eines Befehls, wenn ein Ereignis ausgelöst wird, ist, dass Befehle Steuerelemente zugeordnet werden können, die für die Interaktion mit Befehlen entworfen wurden. Darüber hinaus verschiebt diesen Ereignisbehandlungscode auf Modelle anzeigen, bei denen es Komponententest nicht.

#### <a name="invoking-behaviors-from-a-view"></a>Aufrufen von Verhaltensweisen aus einer Sicht

Die `EventToCommandBehavior` ist besonders nützlich zum Anfügen eines Befehls an ein Steuerelement, die Befehle nicht unterstützt. Z. B. die `ProfileView` verwendet die `EventToCommandBehavior` zum Ausführen der `OrderDetailCommand` bei der [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) Ereignis ausgelöst wird, auf die [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , des Benutzers Bestellungen aufgelistet sind, wie dargestellt Im folgenden Code:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

Zur Laufzeit die `EventToCommandBehavior` antwortet auf die Interaktion mit der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Wenn ein Element ausgewählt ist, der `ListView`, die [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) Ereignis wird ausgelöst, die ausgeführt werden die `OrderDetailCommand` in der `ProfileViewModel`. Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben. Diese Daten werden konvertiert, wie sie zwischen Quelle und Ziel, vom Konverter im angegebenen übergeben wird der `EventArgsConverter` Eigenschaft, die zurückgibt der [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) von der `ListView` aus der [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). Aus diesem Grund, dass bei der `OrderDetailCommand` ausgeführt wird, den ausgewählten `Order` an der registrierten Aktion als Parameter übergeben wird.

Weitere Informationen zu Verhalten, finden Sie unter [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Zusammenfassung

Das Model-View-ViewModel (MVVM)-Muster hilft, um die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI) ordnungsgemäß zu trennen. Eine saubere Trennung zwischen der Anwendungslogik und der Benutzeroberfläche verwalten können Sie um zahlreiche Entwicklungsprobleme zu beheben, und erleichtert werden kann eine Anwendung zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und erlaubt Entwicklern und UI-Designer mehr einfacher zusammenarbeiten, bei der Entwicklung ihrer jeweiligen Teile einer app.

Mithilfe der MVVM-Muster, die Benutzeroberfläche der app und die zugrunde liegende Logik der Präsentations- und ist in drei separate Klassen unterteilt: der Sicht, die Benutzeroberfläche und eine Benutzeroberfläche kapselt Logik; das Modell anzeigen, das Präsentations- und der Status kapselt; und das Modell, das Geschäftslogik der Anwendung und die Daten kapselt.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
