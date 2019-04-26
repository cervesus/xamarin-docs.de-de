---
title: Das Model-View-ViewModel-Muster
description: In diesem Kapitel wird erläutert, wie die eShopOnContainers-mobile-app das MVVM-Muster verwendet, um die Logik für die Business und Darstellung der app aus der Benutzeroberfläche sauber zu trennen.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 87448c556c66ea086db70699848227e1f671792b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61299628"
---
# <a name="the-model-view-viewmodel-pattern"></a>Das Model-View-ViewModel-Muster

Die Xamarin.Forms-Entwicklungsumgebung umfasst in der Regel erstellen eine Benutzeroberfläche in XAML, und klicken Sie dann hinzufügen Code-Behind, die auf der Benutzeroberfläche verwendet wird. Wie apps geändert werden, und in der Größe und Umfang wachsen, können eine komplexe Wartungsprobleme auftreten. Zu diesen Problemen zählen die enge Bindung zwischen der UI-Steuerelemente und die Geschäftslogik, die wodurch die Kosten einer Änderungen der Benutzeroberfläche und die Schwierigkeit der Komponententests, die Art von Code erhöht wird.

Das Model-View-ViewModel (MVVM)-Muster hilft, sauber trennen Sie die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI). Eine saubere Trennung zwischen der Anwendungslogik und die Benutzeroberfläche verwalten kann kann, um zahlreiche Probleme bei der Entwicklung zu beheben und eine Anwendung einfacher zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und ermöglicht es Entwicklern und Benutzeroberflächen-Designer eine problemlose Zusammenarbeit ermöglichen, bei der Entwicklung ihrer jeweiligen Teile einer app.

## <a name="the-mvvm-pattern"></a>Das MVVM-Muster

Es gibt drei Hauptkomponenten in das MVVM-Muster: das Modell, Ansicht und Ansichtsmodell. Jede dient jeweils einen unterschiedlichen Zweck. Abbildung 2 – 1 zeigt die Beziehungen zwischen den drei Komponenten.

![](mvvm-images/mvvm.png "Das MVVM-Muster")

**Abbildung 2-1**: Das MVVM-Muster

Zusätzlich zu verstehen, die Verantwortlichkeiten der einzelnen Komponenten, ist es auch wichtig zu verstehen, wie sie miteinander interagieren. Auf hoher Ebene die Ansicht "kennt" das Ansichtsmodell und das Ansichtsmodell "kennt" das Modell, das Modell ist jedoch des Ansichtsmodells und das Ansichtsmodell ist keine Kenntnis von der Ansicht. Aus diesem Grund das Ansichtsmodell isoliert die Ansicht aus dem Modell, und kann das Modell unabhängig von der Ansicht zu entwickeln.

Die Vorteile der Verwendung des MVVM-Musters sind wie folgt:

-   Ist eine vorhandene modellimplementierung, die vorhandene Geschäftslogik kapselt, kann es schwierig oder riskante, um ihn zu ändern. In diesem Szenario wird das Ansichtsmodell fungiert als Adapter für die Modellklassen und können Sie vermeiden, dass wichtigeren Änderungen an der Model-Code.
-   Entwickler können die Komponententests für das Ansichtsmodell und das Modell erstellen, ohne die Ansicht. Die Komponententests für das Ansichtsmodell können genau die gleiche Funktionalität wie von der Sicht ausführen.
-   Die Benutzeroberfläche die app kann ohne Änderung des Codes, überarbeitet werden, vorausgesetzt, dass die Ansicht vollständig in XAML implementiert wird. Aus diesem Grund sollten eine neue Version der Ansicht mit dem vorhandenen Modell der Sicht arbeiten.
-   Designer und Entwickler können unabhängig voneinander und gleichzeitig ihre Komponenten während des Entwicklungsprozesses arbeiten. Designer können auf die Ansicht konzentrieren, während Entwickler auf dem Ansichtsmodell und die Modellkomponenten arbeiten können.

Der Schlüssel für die effektive Verwendung von MVVM liegt im Verständnis, app-Code in die richtigen Klassen zu berücksichtigen und zu verstehen, wie die Klassen interagieren. Den folgenden Abschnitten werden die Verantwortlichkeiten der einzelnen Klassen in das MVVM-Muster.

### <a name="view"></a>Ansicht

Die Ansicht ist dafür verantwortlich zu definieren, die Struktur, Layout und Darstellung von was der Benutzer auf dem Bildschirm angezeigt wird. In XAML, wird im Idealfall jede Ansicht mit einer begrenzten Code-Behind definiert, die keine Geschäftslogik enthalten. Jedoch kann das Code-Behind in einigen Fällen UI-Logik enthalten, das visuelle Verhalten implementiert, das zum Ausdrücken in XAML, wie z.B. Animationen schwierig ist.

In einer Xamarin.Forms-Anwendung ist eine Ansicht in der Regel eine [ `Page` ](xref:Xamarin.Forms.Page)-abgeleitet oder [ `ContentView` ](xref:Xamarin.Forms.ContentView)-abgeleitete Klasse. Sichten können jedoch auch durch eine Datenvorlage dargestellt werden, die angibt, die Elemente der Benutzeroberfläche verwendet werden, um ein Objekt visuell darzustellen, wenn er angezeigt wird. Eine Datenvorlage als Sicht keine Code-Behind, und dient zum Binden an eine bestimmte ansichtsmodelltyp.

> [!TIP]
> Vermeiden Sie aktivieren und Deaktivieren von Elementen der Benutzeroberfläche in Code-Behind. Stellen Sie sicher, dass Ansichtsmodelle zum Definieren von logischen Zustandsänderungen, die Auswirkungen auf einige Aspekte von der Ansicht angezeigt, z. B. dienen, ob ein Befehl verfügbar ist oder ein Hinweis, dass ein Vorgang aussteht. Aus diesem Grund aktivieren Sie und deaktivieren Sie die Elemente der Benutzeroberfläche durch Bindung an Eigenschaften des Modells, anstatt aktivieren und deaktivieren diese im Code-Behind.

Es gibt verschiedene Optionen zum Ausführen von Code im Ansichtsmodell in Reaktion auf Interaktionen für die Sicht, z. B. eine Schaltfläche klicken oder die Auswahl von Listenelementen. Wenn ein Steuerelement unterstützt die Befehle aus, das Steuerelement des `Command` Eigenschaft kann werden Datenbindung an ein `ICommand` Eigenschaft im Ansichtsmodell. Wenn das Steuerelement der Befehl aufgerufen wird, wird der Code in das Ansichtsmodell ausgeführt werden. Zusätzlich zu den Befehlen Verhalten können zu einem Objekt in der Ansicht angefügt werden und können für den aufzurufenden Hilfebefehl oder Ereignis ausgelöst wird lauscht. In der Antwort, das Verhalten dann aufrufen, eine `ICommand` auf das View Model oder eine Methode für das Ansichtsmodell.

### <a name="viewmodel"></a>ViewModel

Das Ansichtsmodell implementiert, Eigenschaften und Befehle, die in dem Sie die Ansicht kann die Datenbindung an und benachrichtigt den Überblick über alle Änderungen des Ansichtszustands durch Änderungsereignisse für die Benachrichtigung. Definieren die Eigenschaften und Befehle, die das Ansichtsmodell enthält die Funktionalität über die Benutzeroberfläche angeboten werden, aber die Ansicht bestimmt, wie diese Funktion, die angezeigt werden.

> [!TIP]
> Halten Sie die Benutzeroberfläche reaktionsfähig mit asynchronen Vorgängen. Mobile apps sollte im UI-Thread nicht mehr blockiert, um die Wahrnehmung des Benutzers, der Leistung zu verbessern beibehalten werden. Klicken Sie daher im Ansichtsmodell, verwenden Sie asynchrone Methoden für e/a-Vorgänge und Auslösen von Ereignissen, um Ansichten der eigenschaftenänderungen asynchron zu benachrichtigen.

Das Ansichtsmodell ist auch verantwortlich für die Koordination der Ansicht Interaktionen mit jeder Modellklassen, die erforderlich sind. In der Regel besteht eine 1: n Beziehung zwischen dem Ansichtsmodell und ViewModel-Klassen. Das Ansichtsmodell kann auch ViewModel-Klassen direkt auf die Sicht verfügbar zu machen, damit die Steuerelemente in der Ansicht die Daten direkt an diese gebunden werden können. In diesem Fall müssen die ViewModel-Klassen entworfen werden, die Datenbindung unterstützen, und Ändern von Benachrichtigungsereignissen.

Jedes Ansichtsmodell enthält Daten über ein Modell in ein Formular, das die Ansicht nutzen kann. Um dies zu erreichen, führt das Ansichtsmodell manchmal der Datenkonvertierung. Platzieren diese Datenkonvertierung in das Ansichtsmodell ist eine gute Idee, da sie Eigenschaften enthält, denen die Sicht gebunden werden kann. Das Ansichtsmodell kann z. B. die Werte von zwei Eigenschaften, die für die Anzeige von der Sicht erleichtern kombinieren.

> [!TIP]
> Zentralisieren Sie die Konvertierung von Daten in einer Konvertierungsebene. Es ist auch möglich, Konverter als eine separate Datenschicht für die Konvertierung zu verwenden, die zwischen dem Ansichtsmodell und der Ansicht befindet. Dies kann z. B. erforderlich, sein, wenn Daten erfordert spezielle Formatierung, die das Ansichtsmodell nicht zur Verfügung stellt.

In der Reihenfolge für das Ansichtsmodell an bidirektionale Datenbindung mit der Ansicht teilzunehmen, müssen seine Eigenschaften Auslösen der `PropertyChanged` Ereignis. Ansichtsmodelle diese Anforderung erfüllen, durch die Implementierung der `INotifyPropertyChanged` Schnittstelle und Auslösen der `PropertyChanged` Ereignis aus, wenn eine Eigenschaft geändert wird.

Für Sammlungen, die geeignete Ansicht `ObservableCollection<T>` wird bereitgestellt. Diese Auflistung implementiert die geänderte Auflistung Benachrichtigung sind daher weniger den Entwickler zu implementieren die `INotifyCollectionChanged` Schnittstelle für Sammlungen.

### <a name="model"></a>Modell

ViewModel-Klassen sind nicht-Visual-Klassen, die die Daten der app zu kapseln. Aus diesem Grund kann das Modell der Darstellung der app-Domänenmodell, in der Regel enthält ein Datenmodell, zusammen mit Business und Validierungslogik betrachtet werden. Beispiele für Modellobjekte sind datentransferobjekte (DTOs), Plain Old CLR Objects (POCOs) und erstellte Entität und Proxyobjekten.

ViewModel-Klassen werden in der Regel verwendet, in Verbindung mit Diensten oder -Repositorys, die Zugriff auf Daten und Zwischenspeichern zu kapseln.

## <a name="connecting-view-models-to-views"></a>Herstellen einer Verbindung Ansichten mit Anzeigemodelle

Ansichtsmodelle können über die Datenbindungsfunktionen von Xamarin.Forms mit Ansichten verbunden sein. Es gibt verschiedene Methoden, die zum Erstellen von Ansichten und Ansichtsmodelle und ordnen Sie sie zur Laufzeit verwendet werden können. Diese Ansätze fallen in zwei Kategorien, bekannt als Zusammenstellung der ersten und der ersten Ansicht Komposition von Modellen. Auswählen zwischen Zusammenstellung der ersten und der Ansicht, dass die erste Komposition von Modellen ein Problem des Geschmacks und die Komplexität ist. Alle Methoden verwenden jedoch das gleiche Ziel, die für die Ansicht, damit ein Ansichtsmodell, das die Eigenschaft "BindingContext" zugewiesen ist.

Ansicht erste Komposition der app im Prinzip Ansichten besteht, die mit den Ansichtsmodellen zu verbinden, von denen sie abhängen. Der Hauptvorteil dieses Ansatzes ist, dass es einfach, zum Erstellen von lose gekoppelten Einheit testfähiger apps vereinfacht, da die Ansichtsmodelle keine Abhängigkeit mehr von den Ansichten selbst haben. Es ist auch einfach zu verstehen die Struktur der app durch die visuelle Struktur folgt, anstatt zum Nachverfolgen der codeausführung, um zu verstehen, wie Klassen erstellt und verknüpft sind. Darüber hinaus entspricht das Xamarin.Forms Navigationssystem, das Erstellen von Seiten verantwortlich ist, bei der Navigation werden dadurch eine Zusammenstellung der ersten Modell komplex und mit der Plattform falsch ausgerichtete erste Erstellung der Ansicht.

Ansicht erste Komposition von Modellen in der app im Prinzip Modelle anzeigen, mit einem Dienst wird für die Suche nach der Ansicht für ein Ansichtsmodell verantwortlich besteht. Erste Komposition von Modellen Ansicht natürlichere für einige Entwickler, da es sich bei die Erstellung der Sicht, abstrahiert werden kann, sodass sie sich auf die logische nicht-UI-Struktur der app konzentrieren. Darüber hinaus können sie Modelle anzeigen, von der anderen Modelle anzeigen erstellt werden. Allerdings dieser Ansatz ist häufig komplexe, und es kann schwierig zu verstehen, wie Sie die verschiedenen Teile der app erstellt und verknüpft werden.

> [!TIP]
> Halten Sie ViewModels und Ansichten unabhängig. Die Bindung von Ansichten auf eine Eigenschaft in einer Datenquelle sollte die principal Abhängigkeit von der Ansicht auf die entsprechende Ansichtsmodell. Insbesondere keine Verweistypen anzeigen, wie z. B. [ `Button` ](xref:Xamarin.Forms.Button) und [ `ListView` ](xref:Xamarin.Forms.ListView), aus der anzeigemodelle. Befolgen Sie die hier beschriebenen Prinzipien, können anzeigemodelle isoliert, wodurch die Wahrscheinlichkeit, dass von Softwarefehlern vom Bereich einschränken getestet werden.

Den folgenden Abschnitten werden die Hauptmethoden der Ansichten mit anzeigemodelle.

### <a name="creating-a-view-model-declaratively"></a>Erstellen ein Ansichtsmodell deklarativ

Der einfachste Ansatz ist für die Ansicht, um die entsprechende Ansichtsmodell in XAML deklarativ zu instanziieren. Wenn die Sicht erstellt wird, wird auch das entsprechende ViewModel-Objekt erstellt werden. Dieser Ansatz wird im folgenden Codebeispiel veranschaulicht:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Wenn die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) erstellt wird, wird eine Instanz von der `LoginViewModel` wird automatisch erstellt und als der Ansicht [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext).

Diese deklarativen Erstellung und Zuweisung des Ansichtsmodells durch die Sicht hat den Vorteil, dass es einfach ist, aber den Nachteil hat, dass es sich um einen (parameterlosen) Standardkonstruktor im Ansichtsmodell erfordert.

### <a name="creating-a-view-model-programmatically"></a>Programmgesteuertes Erstellen von Anzeigemodellen

Eine Sicht kann Code verfügen, in der Code-Behind-Datei, die sich im Ansichtsmodell zuzuweisende ergibt die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft. Dies erfolgt häufig im Konstruktor der Ansicht, wie im folgenden Codebeispiel gezeigt:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Die programmgesteuerte Erstellung und die Zuweisung des Ansichtsmodells in der Ansicht CodeBehind hat den Vorteil, dass es einfach ist. Der wichtigste Nachteil dieses Ansatzes ist jedoch, dass die Sicht, um das Ansichtsmodell alle erforderlichen Abhängigkeiten bereitzustellen muss. Mithilfe von DI-Containern kann dabei helfen, lose Kopplung zwischen der Ansicht und Ansichtsmodell beizubehalten. Weitere Informationen finden Sie unter [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Erstellen einer Ansicht definiert als eine Datenvorlage

Eine Sicht kann definiert wird, als eine Datenvorlage und einen ansichtsmodelltyp zugeordnet werden. Datenvorlagen können als Ressourcen definiert werden, oder sie verwendet werden können innerhalb des Steuerelements, die das Ansichtsmodell angezeigt werden. Der Inhalt des Steuerelements ist die Modellinstanz anzeigen und die Datenvorlage wird verwendet, um sie darzustellen. Dieses Verfahren ist ein Beispiel für eine Situation, in der das View Model zuerst gefolgt von der Erstellung der Sicht instanziiert wird.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Automatisch erstellen ein Ansichtsmodell mit einem View Model-Locator

Ein View-Model-Locator ist eine benutzerdefinierte Klasse, die die Instanziierung der anzeigemodelle und ihre Zuordnung zu Ansichten verwaltet. In der mobilen Anwendung "eshoponcontainers" die `ViewModelLocator` -Klasse verfügt über eine angefügte Eigenschaft, `AutoWireViewModel`, wird verwendet, um Sichten Ansichtsmodelle zugeordnet werden. In der Ansicht XAML ist angefügte Eigenschaft festgelegt, auf "true", um anzugeben, dass das Ansichtsmodell automatisch mit der Ansicht verbunden werden sollen, wie im folgenden Codebeispiel gezeigt:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

Die `AutoWireViewModel` -Eigenschaft ist eine bindbare Eigenschaft, die auf "false" initialisiert, und wenn der Wert ändert, die `OnAutoWireViewModelChanged` Ereignishandler wird aufgerufen. Diese Methode löst das Ansichtsmodell für die Ansicht. Im folgenden Codebeispiel wird veranschaulicht, wie dies erreicht wird:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

Die `OnAutoWireViewModelChanged` -Methode versucht, die das Ansichtsmodell mit einem konventionsbasierten Ansatz zu beheben. Diese Konvention wird vorausgesetzt, dass:

-   Anzeigemodelle, sind in der gleichen Assembly wie Ansichtstypen.
-   Sichten befinden sich in einem. Untergeordneter Ansichten-Namespace.
-   Ansichtsmodelle befinden sich in einem. ViewModels untergeordneter Namespace.
-   Sichtnamen-Modell mit Ansichtsnamen entsprechen und enden mit "ViewModel".

Schließlich die `OnAutoWireViewModelChanged` Methode legt die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) des Typs aufgelöst ansichtsmodelltyp anzeigen. Weitere Informationen zum Beheben von ansichtsmodelltyp finden Sie unter [Auflösung](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Dieser Ansatz hat den Vorteil, dass eine app verfügt über eine einzelne Klasse, die für die Instanziierung der anzeigemodelle und die Verbindung mit Ansichten zuständig ist.

> [!TIP]
> Verwenden Sie einen Ansicht-Modell-Locator, zur Vereinfachung der Ersetzung. Ein Ansicht-Modell-Locator kann auch als ein Punkt, der Ersatz für alternative Implementierungen von Abhängigkeiten, wie z. B. für Unit Tests oder Entwurf Zeitdaten verwendet werden.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Aktualisieren von Sichten als Reaktion auf Änderungen in der zugrunde liegenden anzeigen, Modell oder das Modell

Alle Ansichtsmodell und ViewModel-Klassen, die an eine Ansicht verfügbar sind, sollten implementieren die `INotifyPropertyChanged` Schnittstelle. Diese Schnittstelle in einem Ansichtsmodell oder Modellklasse implementiert, ermöglicht der Klasse, die änderungsbenachrichtigungen von datengebundenen Steuerelementen in der Ansicht bereitstellen, wenn der zugrunde liegende Eigenschaftswert geändert wird.

Apps sollten für die korrekte Verwendung von Benachrichtigung der Eigenschaftenänderung, so entworfen werden, indem Sie die folgenden Anforderungen erfüllen:

-   Immer das Auslösen einer `PropertyChanged` Ereignis, wenn eine öffentliche Eigenschaft-Wert ändert. Nehmen Sie nicht an, das Auslösen der `PropertyChanged` Ereignis kann aufgrund von Kenntnisse wie XAML-Bindung tritt auf, ignoriert werden.
-   Immer das Auslösen einer `PropertyChanged` -Ereignis für eine berechnete Eigenschaften, deren Werte durch andere Eigenschaften in der Ansicht verwendet werden-Modell oder das Modell.
-   Auslösen von immer die `PropertyChanged` Ereignis am Ende der Methode, die eine Eigenschaft geändert wird, oder wenn das Objekt in einen sicheren Zustand bekannt ist. Auslösen des Ereignisses unterbricht den Vorgang, durch die Ereignishandler synchron aufrufen. Gerade ein Vorgang in diesem Fall kann es das Objekt, das Rückruffunktionen verfügbar, wenn es in einem unsicheren, teilweise aktualisierten Zustand befindet. Darüber hinaus ist es möglich, dass überlappende Änderungen, die von ausgelöst werden `PropertyChanged` Ereignisse. Kaskadierende Änderungen erfordern in der Regel Updates abgeschlossen sein, bevor die kaskadierende Änderung sicher ausgeführt werden.
-   Auslösen von nie eine `PropertyChanged` Ereignis, wenn die Eigenschaft nicht geändert wird. Dies bedeutet, dass Sie die alten und neuen Werte, vor dem Auslösen vergleichen müssen der `PropertyChanged` Ereignis.
-   Auslösen von nie die `PropertyChanged` Ereignis während des Ansichtsmodells-Konstruktor, wenn Sie eine Eigenschaft initialisieren. Datengebundene Steuerelemente in der Ansicht werden nicht für die Änderung an diesem Punkt Benachrichtigungen abonniert haben.
-   Auslösen von nie mehr als eine `PropertyChanged` -Ereignis mit dem gleichen Argument der Eigenschaft Name innerhalb einer einzelnen synchronen Aufruf eine öffentliche Methode einer Klasse. Angenommen, ein `NumberOfItems` -Eigenschaft, deren Sicherungsspeicher der `_numberOfItems` Feld, wenn eine Methode inkrementiert `_numberOfItems` 50 Mal während der Ausführung einer Schleife, es sollte nur auslösen Benachrichtigung der Eigenschaftenänderung auf die `NumberOfItems` einmal-Eigenschaft Nachdem die gesamte Arbeit abgeschlossen ist. Für asynchrone Methoden, auslösen, die `PropertyChanged` -Ereignis für einen angegebenen Eigenschaftsnamen in jedem Segment Verwenden einer Kette asynchrone Fortsetzung synchron.

Die eShopOnContainers-mobile app verwendet die `ExtendedBindableObject` Klasse änderungsbenachrichtigungen, die im folgenden Codebeispiel dargestellt ist:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Die Xamarin.Form [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) -Klasse implementiert die `INotifyPropertyChanged` Schnittstelle, und bietet eine [ `OnPropertyChanged` ](xref:Xamarin.Forms.BindableObject.OnPropertyChanged(System.String)) Methode. Die `ExtendedBindableObject` -Klasse bietet die `RaisePropertyChanged` Methode aufzurufende Eigenschaft änderungsbenachrichtigung und auf diese Weise wird die Funktionalität der `BindableObject` Klasse.

Jede ViewModel-Klasse in der mobilen app für "eshoponcontainers" leitet sich von der `ViewModelBase` -Klasse, die wiederum von abgeleitet ist die `ExtendedBindableObject` Klasse. Aus diesem Grund jede ViewModel-Klasse verwendet die `RaisePropertyChanged` -Methode in der die `ExtendedBindableObject` -Klasse, Benachrichtigung der Eigenschaftenänderung bereitzustellen. Im folgenden Codebeispiel wird veranschaulicht, wie die mobile app "eshoponcontainers" Benachrichtigung der Eigenschaftenänderung mithilfe eines Lambda-Ausdrucks aufgerufen:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Beachten Sie, dass es sich bei Verwendung eines Lambda-Ausdrucks auf diese Weise umfasst leichten Leistungseinbußen, da der Lambda-Ausdruck muss für jeden Aufruf ausgewertet werden. Obwohl die Leistungskosten klein ist und eine app normalerweise nicht beeinträchtigt wird, können die Kosten entstehen, wenn vorhanden sind, dass viele Benachrichtigungen ändern. Der Vorteil dieses Ansatzes ist jedoch, dass es während der Kompilierung typsicherheit und refactoring-Unterstützung beim Umbenennen von Eigenschaften bereitstellt.

## <a name="ui-interaction-using-commands-and-behaviors"></a>UI-Interaktion mithilfe von Befehlen und Verhaltensweisen

In mobilen apps werden Aktionen in der Regel als Reaktion auf eine Benutzeraktion wie z. B. Klicken auf eine Schaltfläche, aufgerufen werden, die durch das Erstellen eines ereignishandlers in der CodeBehind-Datei implementiert werden können. Allerdings in das MVVM-Muster, die Verantwortung für die Implementierung der Aktions und dem Ansichtsmodell liegt, und Einfügen von Code im Code-Behind sollte vermieden werden.

Befehle bieten eine bequeme Möglichkeit, die Aktionen darstellen, die an Steuerelemente der Benutzeroberfläche gebunden werden kann. Diese kapseln des Codes, der die Aktion implementiert, und dabei helfen, die sie von dessen visuelle Darstellung in der Ansicht entkoppelt bleiben. Xamarin.Forms enthält Steuerelemente, die deklarativ mit einem Befehl verbunden werden können, und diese Steuerelemente werden der Befehl aufgerufen, wenn der Benutzer mit dem Steuerelement interagiert.

Verhaltensweisen können auch Steuerelemente deklarativ mit einem Befehl verbunden sein. Verhalten können jedoch verwendet werden, um eine Aktion aufzurufen, die eine Reihe von Ereignissen, die ausgelöst wird, wird von einem Steuerelement zugeordnet ist. Aus diesem Grund Adresse Verhalten vieler dieselben Szenarios wie die Befehls-Steuerelementen, und gleichzeitig ein größeres Maß an Flexibilität und Kontrolle. Darüber hinaus können Verhalten auch Befehlsobjekte oder Methoden mit Steuerelementen zuordnen, die nicht speziell entworfen wurden, für die Interaktion mit Befehlen verwendet werden.

### <a name="implementing-commands"></a>Implementieren von Befehlen

Ansichtsmodelle machen in der Regel Befehlseigenschaften, für die Bindung aus der Ansicht, die Objektinstanzen, implementieren die `ICommand` Schnittstelle. Geben Sie eine Reihe von Xamarin.Forms-Steuerelementen ein `Command` -Eigenschaft, die Daten werden kann, die an gebunden ein `ICommand` Objekt des Modells anzeigen. Die `ICommand` Schnittstelle definiert eine `Execute` -Methode, die den Vorgang selbst kapselt, ein `CanExecute` Methode, die angibt, ob der Befehl aufgerufen werden kann, und ein `CanExecuteChanged` Ereignis tritt auf, wenn Änderungen, die sich auf, gibt an, ob auftreten der Befehl ausgeführt wird. Die [ `Command` ](xref:Xamarin.Forms.Command) und [ `Command<T>` ](xref:Xamarin.Forms.Command) von Xamarin.Forms bereitgestellten Klassen implementieren die `ICommand` -Schnittstelle, in denen `T` ist der Typ der Argumente für `Execute`und `CanExecute`.

In ein Ansichtsmodell, muss ein Objekt des Typs [ `Command` ](xref:Xamarin.Forms.Command) oder [ `Command<T>` ](xref:Xamarin.Forms.Command) für jede öffentliche Eigenschaft im Ansichtsmodell vom Typ `ICommand`. Die `Command` oder `Command<T>` -Konstruktor erfordert eine `Action` Rückrufobjekt, das aufgerufen wurde die `ICommand.Execute` -Methode wird aufgerufen. Die `CanExecute` -Methode ist eine optionale Konstruktorparameter, und eine `Func` zurückgibt, die eine `bool`.

Der folgende code zeigt, wie eine [ `Command` ](xref:Xamarin.Forms.Command) -Instanz, die einen Registrierungsbefehl darstellt, wird erstellt, indem gibt dabei einen Delegaten, der `Register` Methode anzeigen:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Der Befehl wird verfügbar gemacht, an die Ansicht über eine Eigenschaft, die ein Verweis ein `ICommand`. Wenn die `Execute` Methode wird aufgerufen, auf die [ `Command` ](xref:Xamarin.Forms.Command) -Objekts können sie einfach den Aufruf der Methode im Ansichtsmodell über der Delegat, der im angegebenen leitet die `Command` Konstruktor.

Eine asynchrone Methode kann von einem Befehl aufgerufen werden, mithilfe der `async` und `await` Schlüsselwörter, die beim Angeben des Befehls `Execute` delegieren. Dies bedeutet, dass der Rückruf ist ein `Task` und gewartet werden soll. Z. B. der folgende code zeigt, wie eine [ `Command` ](xref:Xamarin.Forms.Command) -Instanz, die einen Befehl Anmeldung darstellt, wird erstellt, indem Sie einen Delegaten angeben der `SignInAsync` Methode anzeigen:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parameter können übergeben werden, um die `Execute` und `CanExecute` Aktionen mithilfe der [ `Command<T>` ](xref:Xamarin.Forms.Command) Klasse, um den Befehl zu instanziieren. Beispielsweise der folgende code zeigt, wie eine `Command<T>` Instanz wird verwendet, um anzugeben, dass die `NavigateAsync` Methode müssen ein Argument des Typs `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

Sowohl die [ `Command` ](xref:Xamarin.Forms.Command) und [ `Command<T>` ](xref:Xamarin.Forms.Command) Klassen, die der Delegat, der die `CanExecute` -Methode in jeder Konstruktor ist optional. Wenn Sie ein Delegaten nicht angegeben ist, die `Command` zurück `true` für `CanExecute`. Das Ansichtsmodell kann jedoch eine Änderung des Befehls angeben `CanExecute` Status durch Aufrufen der `ChangeCanExecute` Methode für die `Command` Objekt. Dies bewirkt, dass die `CanExecuteChanged` Ereignis ausgelöst wurde. Alle Steuerelemente in der Benutzeroberfläche, die an den Befehl gebunden sind werden deren Aktivierungsstatus entsprechend die Verfügbarkeit des Befehls von datengebundenen dann aktualisiert werden.

#### <a name="invoking-commands-from-a-view"></a>Aufrufen von Befehlen aus einer Sicht

Im folgenden Codebeispiel wird veranschaulicht, wie eine [ `Grid` ](xref:Xamarin.Forms.Grid) in die `LoginView` bindet an die `RegisterCommand` in die `LoginViewModel` Klasse, indem eine [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Instanz:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Ein Befehlsparameter kann auch optional definiert werden mithilfe der [ `CommandParameter` ](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter) Eigenschaft. Der Typ des erwarteten Arguments wird angegeben, der `Execute` und `CanExecute` Methoden als Ziel. Die [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) wird automatisch der Zielbefehl aufgerufen, wenn der Benutzer das angefügte Steuerelement interagiert. Der Befehlsparameter, falls angegeben, wird als Argument übergeben werden, um der Befehls `Execute` delegieren.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Implementieren von Verhaltensweisen

Verhaltensweisen können Funktionen für die UI-Steuerelemente hinzugefügt werden, ohne dass sie um eine Unterklasse. Stattdessen wird die Funktion in einer Verhaltensklasse implementiert und an das Steuerelement angefügt, als wäre sie ein Teil des Steuerelements selbst. Verhalten ermöglichen es, Code zu implementieren, die Sie normalerweise schreiben müsste, als Code-Behind, da Sie direkt mit der API eines Steuerelements in einer Weise interagiert werden soll, es kann präzise angefügt an das Steuerelement, und für die Wiederverwendung über mehr als eine Sicht oder app gepackt. Im Kontext von MVVM sind die Verhaltensweisen eine nützliche Methode für das Verbinden von Steuerelementen mit Befehlen.

Ein Verhalten, das ein Steuerelement über angefügte Eigenschaften zugeordnet ist, wird als bezeichnet ein *Verhalten angefügt*. Das Verhalten können Sie dann die verfügbar gemachte API des Elements, dem sie Funktionen hinzufügen möchten, die steuern, oder andere Steuerelemente, in der visuellen Struktur der Sicht zugeordnet ist. Die eShopOnContainers-mobile-app enthält die `LineColorBehavior` -Klasse, die ein Verhalten angefügt ist. Weitere Informationen zu diesem Verhalten finden Sie unter [Anzeigen von Validierungsfehlern](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Eine Xamarin.Forms-Verhaltensweise ist eine abgeleitete Klasse die [ `Behavior` ](xref:Xamarin.Forms.Behavior) oder [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) -Klasse, in denen `T `ist der Typ des Steuerelements auf die das Verhalten angewendet werden soll. Diese Klassen bieten `OnAttachedTo` und `OnDetachingFrom` -Methoden, die Logik bereitstellen, die ausgeführt werden, wenn das Verhalten ist angefügt und von Steuerelementen getrennt überschrieben werden sollen.

In der mobilen Anwendung "eshoponcontainers" die `BindableBehavior<T>` Klasse leitet sich von der [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) Klasse. Der Zweck der `BindableBehavior<T>` Klasse ist eine Basisklasse für Xamarin.Forms-Verhaltensweisen, die erfordern die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) des Verhaltens, das auf das angefügte Steuerelement festgelegt werden.

Die `BindableBehavior<T>` -Klasse stellt eine überschreibbare `OnAttachedTo` Methode, die festlegt der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) das Verhalten und eine überschreibbare `OnDetachingFrom` -Methode, die bereinigt die `BindingContext`. Darüber hinaus speichert die Klasse einen Verweis auf das angefügte Steuerelement in der `AssociatedObject`-Eigenschaft.

Die eShopOnContainers-mobile-app enthält eine `EventToCommandBehavior` -Klasse, die als Reaktion auf das Auftreten eines Ereignisses ein Befehl ausgeführt wird. Diese Klasse wird von der `BindableBehavior<T>` -Klasse so, dass das Verhalten kann zu binden und Ausführen einer `ICommand` gemäß einer `Command` Eigenschaft, wenn das Verhalten genutzt wird. Das folgende Codebeispiel zeigt die `EventToCommandBehavior`-Klasse:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

Die `OnAttachedTo` und `OnDetachingFrom` Methoden zum Registrieren und Aufheben der Registrierung eines ereignishandlers für das Ereignis definiert, der `EventName` Eigenschaft. Klicken Sie dann, wenn das Ereignis ausgelöst wird, die `OnFired` Methode wird aufgerufen, die den Befehl ausführt.

Der Vorteil der Verwendung der `EventToCommandBehavior` zum Ausführen eines Befehls, wenn ein Ereignis ausgelöst wird, ist, dass Befehle mit Steuerelementen verknüpft werden können, die entworfen wurden, für die Interaktion mit den Befehlen. Darüber hinaus verschiebt diese Ereignisbehandlungscode in Modelle anzeigen, in der sie Komponententests sein.

#### <a name="invoking-behaviors-from-a-view"></a>Aufrufen von Verhaltensweisen aus einer Sicht

Die `EventToCommandBehavior` ist besonders nützlich für das Anfügen eines Befehls an ein Steuerelement, das Befehle nicht unterstützt. Z. B. die `ProfileView` verwendet die `EventToCommandBehavior` zum Ausführen der `OrderDetailCommand` bei der [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignis ausgelöst wird, auf die [ `ListView` ](xref:Xamarin.Forms.ListView) , des Benutzers Bestellungen aufgelistet sind, siehe Im folgenden Code:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

Zur Laufzeit die `EventToCommandBehavior` reagiert auf die Interaktion mit der [ `ListView` ](xref:Xamarin.Forms.ListView). Wenn ein Element ausgewählt ist, der `ListView`, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) Ereignis wird ausgelöst, die ausgeführt werden die `OrderDetailCommand` in die `ProfileViewModel`. Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben. Diese Daten konvertiert werden, wie sie zwischen Quelle und Ziel, vom Konverter im angegebenen übergeben wird die `EventArgsConverter` -Eigenschaft, die zurückgibt der [ `Item` ](xref:Xamarin.Forms.ItemTappedEventArgs.Item) von der `ListView` aus der [ `ItemTappedEventArgs` ](xref:Xamarin.Forms.ItemTappedEventArgs). Aus diesem Grund, wenn die `OrderDetailCommand` ausgeführt wird, den ausgewählten `Order` der registrierten Aktion als Parameter übergeben wird.

Weitere Informationen zu Verhalten finden Sie unter [Verhaltensweisen](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Zusammenfassung

Das Model-View-ViewModel (MVVM)-Muster hilft, sauber trennen Sie die Logik für die Business und Präsentation von einer Anwendung über die Benutzeroberfläche (UI). Eine saubere Trennung zwischen der Anwendungslogik und die Benutzeroberfläche verwalten kann kann, um zahlreiche Probleme bei der Entwicklung zu beheben und eine Anwendung einfacher zu testen, verwalten und entwickeln. Sie können Code wiederverwenden Verkaufschancen erheblich verbessern und ermöglicht es Entwicklern und Benutzeroberflächen-Designer eine problemlose Zusammenarbeit ermöglichen, bei der Entwicklung ihrer jeweiligen Teile einer app.

Mithilfe des MVVM-Muster, die Benutzeroberfläche der app und die zugrunde liegende Logik der Präsentations- und ist in drei separate Klassen aufgeteilt: die Ansicht, die die Benutzeroberfläche und die Benutzeroberfläche kapselt Logik zu verwenden. Das Ansichtsmodell, das Darstellungslogik und Status kapselt; und das Modell, das Geschäftslogik und Daten der app kapselt.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
