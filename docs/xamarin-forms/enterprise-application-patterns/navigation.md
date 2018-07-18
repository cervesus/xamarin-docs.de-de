---
title: Enterprise-App-Navigation
description: In diesem Kapitel wird erläutert, wie die mobile app "eshoponcontainers" View Model First Navigation aus Ansichtsmodelle ausführt.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d306b0c1c0d08129671e27b96911ec771acb658e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994769"
---
# <a name="enterprise-app-navigation"></a>Enterprise-App-Navigation

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel über die Interaktion des Benutzers mit der Benutzeroberfläche oder über der app selbst aufgrund von internen Zustand der gesteuerte Änderungen führt. Navigation kann jedoch komplex, um in apps, die die Model-View-ViewModel (MVVM)-Muster verwenden, implementieren, wie die folgenden Probleme, die erfüllt sein müssen:

-   So die Ansicht, zu dem navigiert werden zu identifizieren, verwenden einen Ansatz, der nicht über eine enge Kopplung und Abhängigkeiten zwischen den Ansichten einführt.
-   Wie den Prozess zu koordinieren, mit dem die Ansicht, um Sie zu der navigiert werden instanziiert und initialisiert wird. Wenn MVVM verwenden zu können, müssen die Ansicht und ViewModel, instanziiert und miteinander über den Bindungskontext der Ansicht zugeordnet werden. Wenn eine app einen Dependency Injection-Container verwendet wird, möglicherweise die Instanziierung von Ansichten und Ansichtsmodelle einen bestimmten Konstruktionsmechanismus.
-   Ob führen die Navigation von View-First oder Model First-Navigation anzeigen. Mit View-First-Navigation bezieht sich auf den Namen des sichttyps "die Seite zu navigieren". Während der Navigation wird die angegebene Ansicht zusammen mit der entsprechenden Ansichtsmodell und andere abhängige Dienste instanziiert. Ein alternativer Ansatz ist die Verwendung View Model First Navigation, verweist, in dem die Seite zu navigieren, auf den Namen der ansichtsmodelltyp.
-   Um sauber trennen Sie wie die Standardnavigations-Verhalten der app in den Ansichten und ViewModels. Das MVVM-Muster bietet eine Trennung zwischen Benutzeroberfläches der Anwendung und die Präsentation und Geschäftslogik. Das Verhalten einer App wird jedoch häufig nicht die Benutzeroberfläche und Präsentationen Teile der app umfassen. Der Benutzer wird häufig Navigation aus einer Sicht initiieren, und infolge der Navigation mithilfe die Sicht ersetzt werden. Allerdings kann Navigation oft auch initiiert oder von in das Ansichtsmodell koordiniert werden müssen.
-   Übergeben von Parametern während der Navigation für Zwecke der Initialisierung. Z. B. wenn der Benutzer zu einer Ansicht zum Aktualisieren der Auftragsdetails navigiert, müssen die Auftragsdaten an die Ansicht übergeben werden, damit die richtigen Daten angezeigt werden können.
-   Informationen zum Abstimmen Navigation, um sicherzustellen, dass ein bestimmter Geschäftsregeln antwortkette befolgt werden. Z. B. können der Benutzer aufgefordert werden, vor der Navigation von einer Sicht, damit sie die ungültigen Daten korrigieren können oder aufgefordert, zu übermitteln oder verwerfen alle datenänderungen, die in der Ansicht vorgenommen wurden.

In diesem Kapitel begegnet dieser Herausforderung durch die Bereitstellung einer `NavigationService` -Klasse, die zum Ausführen von View Model First Seitennavigation verwendet wird.

> [!NOTE]
> Die `NavigationService` ein, die die app nur hierarchische Navigation zwischen Instanzen von ContentPage ausführen soll. Mit dem Dienst zum Navigieren zwischen anderen Seitentypen möglicherweise zu unerwartetem Verhalten führen.

## <a name="navigating-between-pages"></a>Navigieren zwischen Seiten

Navigationslogik kann befinden sich in einer Ansicht des Code-Behind- oder in einer gebundenen Ansichtsmodell. Während der Platzierung von Navigationslogik in einer Ansicht der einfachste Ansatz sein könnte, ist es nicht einfach durch Komponententests getestet werden. Modellklassen Navigationslogik in der Ansicht platzieren bedeutet, dass die Logik über Komponententests ausgeführt werden kann. Darüber hinaus kann das Ansichtsmodell implementieren Logik zum Steuerelement-Navigation, um sicherzustellen, dass ein bestimmter Geschäftsregeln erzwungen werden. Beispielsweise eine app ermöglicht möglicherweise die nicht den Benutzer Navigieren von einer Seite, ohne zuerst sicherstellen, dass die eingegebenen Daten ungültig sind.

Ein `NavigationService` Klasse in der Regel aus Modelle anzeigen, zur Förderung der testbarkeit aufgerufen wird. Navigieren zu Ansichten von Ansichtsmodelle erfordert jedoch die Ansichtsmodelle, Verweis, und insbesondere Ansichten, die das Modell für die aktive Ansicht nicht zugeordnet, dies jedoch nicht empfohlen wird. Aus diesem Grund die `NavigationService` präsentiert wird hier der anzeigen-Modelltyp als Ziel zu navigieren.

Die eShopOnContainers-mobile app verwendet die `NavigationService` Klasse, um die View Model First-Navigation zu ermöglichen. Diese Klasse implementiert die `INavigationService` -Schnittstelle, die im folgenden Codebeispiel gezeigt wird:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Diese Schnittstelle gibt an, dass eine implementierende Klasse die folgenden Methoden angeben muss:

|Methode|Zweck|
|--- |--- |
|`InitializeAsync`|Navigation zu einer der beiden Seiten führt, wenn die app gestartet wird.|
|`NavigateToAsync`|Führt die hierarchische Navigation für eine angegebene Seite.|
|`NavigateToAsync(parameter)`|Führt die hierarchische Navigation für eine angegebene Seite, ein Parameter übergeben wird.|
|`RemoveLastFromBackStackAsync`|Wird die vorherige Seite von dem Navigationsstapel entfernt.|
|`RemoveBackStackAsync`|Entfernt alle in die vorherigen Seiten aus dem Navigationsstapel.|

Darüber hinaus die `INavigationService` Schnittstelle gibt an, dass von einer implementierende Klasse bereitstellen, muss ein `PreviousPageViewModel` Eigenschaft. Diese Eigenschaft gibt die vorherige Seite im Navigationsstapel zugeordneten ansichtsmodelltyp zurück.

> [!NOTE]
> Ein `INavigationService` Schnittstelle in der Regel würde auch angeben, ein `GoBackAsync` -Methode, die verwendet wird, um programmgesteuert zur vorherigen Seite im Navigationsstapel zurückzugeben. Allerdings ist diese Methode fehlen in der mobilen app für "eshoponcontainers", da dies nicht erforderlich ist.

### <a name="creating-the-navigationservice-instance"></a>Erstellen der NavigationService-Instanz

Die `NavigationService` Klasse implementiert die `INavigationService` Schnittstelle, die als Singleton mit den Autofac Dependency Injection-Container registriert ist, wie im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Die `INavigationService` Schnittstelle wird aufgelöst, der `ViewModelBase` Klassenkonstruktor, wie im folgenden Codebeispiel gezeigt:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Dies gibt einen Verweis auf die `NavigationService` -Objekt, das in den Autofac Dependency Injection-Container gespeichert ist, die von erstellt wird die `InitNavigation` -Methode in der die `App` Klasse. Weitere Informationen finden Sie unter [Navigation bei der App wird gestartet.](#navigating_when_the_app_is_launched).

Die `ViewModelBase` -Klasse speichert die `NavigationService` -Instanz in eine `NavigationService` -Eigenschaft vom Typ `INavigationService`. Aus diesem Grund alle Modellklassen, Anzeigen der `ViewModelBase` Klasse, können die `NavigationService` Eigenschaft, um den Zugriff auf die Methoden, die gemäß der `INavigationService` Schnittstelle. Dies vermeidet den Mehraufwand der injizierung der `NavigationService` Objekt aus dem Autofac Dependency Injection-Container in jede ViewModel-Klasse.

### <a name="handling-navigation-requests"></a>Verarbeiten von Navigationsanforderungen

Xamarin.Forms stellt die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Klasse, die eine hierarchische Navigation bereit implementiert, in dem der Benutzer können Navigation durch Seiten, und in der Rückwärtsrichtung wie gewünscht ist. Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

Anstatt verwenden die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) -Klasse direkt verwenden, die "eshoponcontainers" app bindet die `NavigationPage` -Klasse in der `CustomNavigationView` Klasse, wie im folgenden Codebeispiel wird gezeigt:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Der Zweck dieser Wrapper ist für einfache Formatierung den [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) Instanz in der XAML-Datei für die Klasse.

Navigation erfolgt in der ViewModel-Klassen durch den Aufruf eines der `NavigateToAsync` Methoden, die den Typ des Ansicht-Modell für die Seite, wie im folgenden Codebeispiel wird gezeigt, zu dem navigiert wird:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Das folgende Codebeispiel zeigt die `NavigateToAsync` von bereitgestellten Methoden die `NavigationService` Klasse:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Jede Methode ermöglicht das View Model-Klasse, die von abgeleitet der `ViewModelBase` Klasse, um hierarchische Navigation durch den Aufruf auszuführen. die `InternalNavigateToAsync` Methode. Darüber hinaus die zweite `NavigateToAsync` Methode ermöglicht die Navigation als Argument angegeben werden, die an das Ansichtsmodell, zu dem navigiert, wo sie in der Regel verwendet wird für die Initialisierung übergeben wird. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

Die `InternalNavigateToAsync` Methode führt die navigationsanforderung und wird im folgenden Codebeispiel dargestellt:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

Die `InternalNavigateToAsync` Methode führt die Navigation zu einem View Model durch den ersten Aufruf der `CreatePage` Methode. Diese Methode sucht die Ansicht, die die angegebene ansichtsmodelltyp entspricht und erstellt und gibt eine Instanz dieses Typs anzeigen. Suchen die Ansicht, die den Ansichtstyp für das Modell entspricht, verwendet einen konventionsbasierten Ansatz, der annimmt, dass:

-   Ansichten sind in der gleichen Assembly wie die Typen des Modells anzeigen.
-   Sichten befinden sich in einem. Untergeordneter Ansichten-Namespace.
-   Ansichtsmodelle befinden sich in einem. ViewModels untergeordneter Namespace.
-   Sichtnamen entsprechen, zum Anzeigen der Modellnamen, mit "Model" entfernt.

Wenn eine Ansicht instanziiert wird, wird es der entsprechende Ansichtsmodell zugeordnet. Weitere Informationen dazu, wie in diesem Fall, finden Sie unter [automatisch ein Ansichtsmodell mit einem View Model-Locator erstellen](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Wenn die Ansicht erstellt wird eine `LoginView`, es wird eine neue Instanz der umhüllt die `CustomNavigationView` Klasse zugewiesen, und wählen Sie die [ `Application.Current.MainPage` ](xref:Xamarin.Forms.Application.MainPage) Eigenschaft. Andernfalls die `CustomNavigationView` Instanz abgerufen, und vorausgesetzt, dass es nicht null ist, ist die [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage) Methode wird aufgerufen, um die Ansicht erstellt wird, auf den Navigationsstapel übertragen. Jedoch wenn abgerufenen `CustomNavigationView` Instanz `null`, wird die Sicht erstellt eine neue Instanz der umhüllt die `CustomNavigationView` Klasse zugewiesen, und wählen Sie die `Application.Current.MainPage` Eigenschaft. Dieser Mechanismus, der während der Navigation wird sichergestellt, werden Seiten richtig auf den Navigationsstapel hinzugefügt, wenn sie leer ist, und wenn es Daten enthält.

> [!TIP]
> Erwägen Sie die Zwischenspeicherung von Seiten. Zwischenspeichern von Seiten führt die arbeitsspeichernutzung für Ansichten, die derzeit nicht angezeigt werden. Allerdings ohne Zwischenspeicherung Seite bedeutet es, dass XAML zu analysieren und Erstellung von auf der Seite und die Ansichtsmodell erfolgt jedes Mal, wenn eine neue Seite, zu dem navigiert werden die Auswirkungen auf die Leistung für eine komplexe Seite verfügen können. Für eine gut entworfene Seite, die keine übermäßige Anzahl von Steuerelementen verwendet, sollte die Leistung ausreichend sein. Zwischenspeichern von Seiten kann jedoch hilfreich, wenn es sich bei langsamen Ladezeiten aufgetreten sind.

Nachdem die Ansicht erstellt und zum navigiert, die `InitializeAsync` -Methode der zugeordneten Ansichtsmodell der Ansicht ausgeführt wird. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Navigieren bei der die App wird gestartet.

Wenn die app gestartet wird, die `InitNavigation` -Methode in der die `App` -Klasse aufgerufen wird. Das folgende Codebeispiel zeigt diese Methode:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Die Methode erstellt ein neues `NavigationService` Objekt in den Autofac Dependency Injection-Container, und gibt einen Verweis auf die sie vor dem Aufrufen der `InitializeAsync` Methode.

> [!NOTE]
> Wenn die `INavigationService` Schnittstelle wird aufgelöst, indem die `ViewModelBase` -Klasse, die der Container gibt einen Verweis auf die `NavigationService` -Objekt, das erstellt wurde, als die InitNavigation-Methode aufgerufen wird.

Das folgende Codebeispiel zeigt die `NavigationService` `InitializeAsync` Methode:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

Die `MainView` zum navigiert wird, wenn die app verfügt über einen zwischengespeicherten Zugriffstokens, die für die Authentifizierung verwendet wird. Andernfalls die `LoginView` navigiert wird.

Weitere Informationen zu den Autofac-abhängigkeitsinjektionscontainer, finden Sie unter [Einführung in Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Übergeben von Parametern während der Navigation

Eines der `NavigateToAsync` Methoden, die gemäß der `INavigationService` Schnittstelle ermöglicht Navigationsdaten als Argument angegeben werden, die an das Ansichtsmodell, zu dem navigiert, wo sie in der Regel verwendet wird für die Initialisierung übergeben wird.

Z. B. die `ProfileViewModel` -Klasse enthält eine `OrderDetailCommand` , die ausgeführt wird, wenn der Benutzer eine Bestellung auf auswählt der `ProfileView` Seite. Im Gegenzug führt dies die `OrderDetailAsync` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Diese Methode ruft die Navigation zu den `OrderDetailViewModel`, und übergeben Sie ein `Order` -Instanz, die Reihenfolge, die der Benutzer darstellt auf ausgewählt, der `ProfileView` Seite. Wenn die `NavigationService` Klasse erstellt, die `OrderDetailView`, wird die `OrderDetailViewModel` Klasse instanziiert wird, und der Ansicht zugeordnet [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext). Nach der Navigation zu den `OrderDetailView`, wird die `InternalNavigateToAsync` Methode führt die `InitializeAsync` Methode der Sicht zugeordneten Ansichtsmodell.

Die `InitializeAsync` Methode definiert ist, der `ViewModelBase` Klasse als eine Methode, die überschrieben werden kann. Diese Methode gibt ein `object` Argument, das Daten an ein Ansichtsmodell übergeben werden, während ein Navigationsvorgang darstellt. Aus diesem Grund ViewModel-Klassen, die Daten über ein Navigationsvorgang erhalten möchten, geben Sie ihre eigene Implementierung von der `InitializeAsync` Methode, um die erforderliche Initialisierung auszuführen. Das folgende Codebeispiel zeigt die `InitializeAsync` Methode aus der `OrderDetailViewModel` Klasse:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Diese Methode ruft die `Order` -Instanz, die während des Vorgangs für die Navigation in das Ansichtsmodell übergeben wurde und wird verwendet, um die vollständige Reihenfolge Abrufen von details der `OrderService` Instanz.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Aufrufende Navigation mithilfe von Verhaltensweisen

Navigation wird in der Regel aus einer Sicht durch eine Benutzerinteraktion ausgelöst. Z. B. die `LoginView` Navigation, die nach der erfolgreichen Authentifizierung ausführt. Im folgenden Codebeispiel wird veranschaulicht, wie die Navigation durch Verhalten aufgerufen wird:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Zur Laufzeit die `EventToCommandBehavior` reagiert auf die Interaktion mit der [ `WebView` ](xref:Xamarin.Forms.WebView). Wenn der `WebView` navigiert zu einer Webseite, die [ `Navigating` ](xref:Xamarin.Forms.WebView.Navigating) Ereignis wird ausgelöst, die ausgeführt wird der `NavigateCommand` in der `LoginViewModel`. Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben. Diese Daten konvertiert werden, wie sie zwischen Quelle und Ziel, vom Konverter im angegebenen übergeben wird die `EventArgsConverter` -Eigenschaft, die gibt die [ `Url` ](xref:Xamarin.Forms.WebNavigationEventArgs.Url) aus der [ `WebNavigatingEventArgs` ](xref:Xamarin.Forms.WebNavigatingEventArgs). Aus diesem Grund, wenn die `NavigationCommand` wird ausgeführt, die Url der Webseite wird übergeben als Parameter an die registrierte `Action`.

Im Gegenzug die `NavigationCommand` führt die `NavigateAsync` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Diese Methode ruft die Navigation zu den `MainViewModel`, und befolgen die Navigation, entfernt der `LoginView` Seite, von dem Navigationsstapel.

### <a name="confirming-or-cancelling-navigation"></a>Bestätigen oder Abbrechen der Navigation

Eine app müssen, damit der Benutzer bestätigen oder Abbrechen der Navigation mit dem Benutzer während eines Vorgangs Navigation interagieren. Dies kann beispielsweise erforderlich, sein, wenn der Benutzer versucht, Sie navigieren, bevor Sie eine Dateneingabeseite vollständig abgeschlossen werden müssen. In diesem Fall sollte eine app eine Benachrichtigung bereitstellen, mit dem Benutzer, die von der Seite Weg navigieren oder den Navigationsvorgang abgebrochen, bevor er stattfindet. Dies kann erfolgen in einer View Model-Klasse mit, dass die Antwort auf eine Benachrichtigung um zu steuern, ob die Navigation aufgerufen wird.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel über die Interaktion des Benutzers über die Benutzeroberfläche oder über die app selbst aufgrund von internen Zustand der gesteuerte Änderungen führt. Navigation kann jedoch komplex, um in apps zu implementieren, die das MVVM-Muster verwenden.

In diesem Kapitel präsentiert eine `NavigationService` -Klasse, die zum Ausführen der Navigation in der Model First von ViewModels verwendet wird. Modellklassen Navigationslogik in der Ansicht platzieren bedeutet, dass die Logik durch automatisierte Tests ausgeführt werden kann. Darüber hinaus kann das Ansichtsmodell implementieren Logik zum Steuerelement-Navigation, um sicherzustellen, dass ein bestimmter Geschäftsregeln erzwungen werden.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
