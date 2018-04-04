---
title: Navigation
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: aa2e2858e3bb8e435ec3f38bb3d5b249eaa6cba4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="navigation"></a>Navigation

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel von Benutzerinteraktionen mit der Benutzeroberfläche oder aus der app aufgrund interner Geschäftslogik gesteuerte Zustandsänderungen führt. Navigation kann jedoch komplexe, Implementieren in apps, die das Muster für Model-View-ViewModel (MVVM) verwenden, wenn die folgenden Herausforderungen erfüllt sein müssen:

-   So identifizieren Sie die Sicht an, zu dem navigiert werden mit einem Ansatz, der keine enge Kopplung und Abhängigkeiten zwischen den Ansichten entstehen.
-   Wie Sie den Prozess zu koordinieren, durch den die Sicht, um zu der navigiert werden instanziiert und initialisiert wird. Wenn MVVM verwenden zu können, müssen die Ansicht und ViewModel, instanziiert und miteinander verknüpft sind, über die Ansicht Bindungskontext verwendet werden. Wenn eine Anwendung einen abhängigkeitseinschleusungscontainer verwendet wird, sind möglicherweise die Instanziierung von Ansichten und Modelle anzeigen einen bestimmten Konstruktionsmechanismus erforderlich.
-   Ob Ansicht-First-Navigation oder Model First-Navigation anzeigen. Bezieht sich mit Ansicht-First-Navigation die Seite zu navigieren, den Namen des sichttyps. Während der Navigation wird die angegebene Ansicht zusammen mit der entsprechenden Ansichtsmodell und andere abhängige Dienste instanziiert. Ein alternativer Ansatz ist die Verwendung View Model First Navigation, dabei bezieht sich die Seite zu navigieren, auf den Namen des sichttyps Modell.
-   Sauberes werden getrennt wie die Navigation Verhalten der app in den Ansichten und Modelle anzeigen. Das MVVM-Muster bietet eine Trennung zwischen Benutzeroberfläches der Anwendung und der Präsentations- und der Geschäftslogik. Allerdings wird das Verhalten einer App häufig die Benutzeroberfläche und Präsentationen Teile der app erstreckt. Initiiert die Benutzer häufig Navigation von einer Sicht, und die Ansicht wird als Ergebnis der Navigation ersetzt werden. Jedoch möglicherweise Navigation oft auch initiiert oder von innerhalb des Modells anzeigen koordiniert werden.
-   Übergeben von Parametern während der Navigation zu initialisieren. Z. B. wenn der Benutzer zu einer Sicht Bestelldetails aktualisieren navigiert, müssen die Auftragsdaten an die Ansicht übergeben werden, damit die richtigen Daten angezeigt werden können.
-   Wie Co Koordinate Navigationsbereich, um sicherzustellen, dass bestimmte Geschäftsregeln nicht beachtet werden. Z. B. möglicherweise der Benutzer aufgefordert werden, vor dem Navigieren von einer Sicht, damit diese ungültigen Daten zu korrigieren können oder aufgefordert, übermitteln oder verwerfen alle datenänderungen, die innerhalb der Ansicht vorgenommen wurden.

In diesem Kapitel begegnet dieser Herausforderung durch die Bereitstellung einer `NavigationService` -Klasse, die zum Ausführen von View Model First Seitennavigation verwendet wird.

> [!NOTE]
> Die `NavigationService` verwendet werden, indem Sie die app dient nur für die hierarchische Navigation zwischen ContentPage verwendet Instanzen ausführen. Mithilfe des Diensts zum Navigieren zwischen anderen Seitentypen möglicherweise zu unerwartetem Verhalten führen.

## <a name="navigating-between-pages"></a>Navigieren zwischen Seiten

Navigationslogik kann befinden sich in einer Ansicht Code-Behind, oder in einer gebundenen Ansichtsmodell. Während platzieren Navigationslogik in einer Ansicht der einfachste Ansatz ist, ist es nicht problemlos über Komponententests getestet werden können. Platzieren die Navigationslogik in der Sicht Modellklassen also die Logik über Komponententests ausgenutzt werden kann. Darüber hinaus kann das Ansichtsmodell dann Logik zum Steuerelement Navigationsbereich, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden implementieren. Beispielsweise eine app lassen möglicherweise nicht den Benutzer Navigieren von einer Seite, ohne zuerst sicherstellen, dass die eingegebenen Daten gültig sind.

Ein `NavigationService` Klasse in der Regel aus Ansichtsmodelle Prüfbarkeit heraufstufen aufgerufen wird. Navigieren zu Ansichten von Ansichtsmodelle erfordert jedoch die Modelle anzeigen, um Verweis, und insbesondere die Ansichten, die das Modell für die aktive Ansicht nicht zugeordnet, dies jedoch nicht empfohlen wird. Aus diesem Grund die `NavigationService` präsentiert hier gibt Modelltyp Ansicht als Ziel zu navigieren.

Der eShopOnContainers mobile app verwendet die `NavigationService` Klasse, um die Ansicht Model First Navigation zu ermöglichen. Diese Klasse implementiert die `INavigationService` -Schnittstelle, die im folgenden Codebeispiel gezeigt wird:

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

Diese Schnittstelle gibt an, dass die folgenden Methoden eine Implementierungsklasse bereitgestellt werden muss:

|Methode|Zweck|
|--- |--- |
|`InitializeAsync`|Navigation zu einer der beiden Seiten führt, wenn die app gestartet wird.|
|`NavigateToAsync`|Führt die hierarchische Navigation für eine angegebene Seite aus.|
|`NavigateToAsync(parameter)`|Führt die hierarchische Navigation für eine angegebene Seite, ein Parameter übergeben wird.|
|`RemoveLastFromBackStackAsync`|Entfernt die vorherige Seite von Navigationsstapel.|
|`RemoveBackStackAsync`|Entfernt die vorherigen Seiten aus Navigationsstapel.|

Darüber hinaus die `INavigationService` Schnittstelle gibt an, dass von einer implementierende Klasse bereitstellen, muss eine `PreviousPageViewModel` Eigenschaft. Diese Eigenschaft gibt die vorherige Seite im Navigationsstapel zugeordneten Ansicht Modelltyp.

> [!NOTE]
> Ein `INavigationService` Schnittstelle würde in der Regel auch angeben, eine `GoBackAsync` -Methode, die verwendet wird, um zur vorherigen Seite im Navigationsstapel programmgesteuert zurück. Allerdings ist diese Methode in der mobilen Anwendung für eShopOnContainers nicht vorhanden, da es nicht erforderlich ist.

### <a name="creating-the-navigationservice-instance"></a>Erstellen der NavigationService-Instanz

Die `NavigationService` Klasse implementiert die `INavigationService` -Schnittstelle, die als Singleton mit der Autofac abhängigkeitseinschleusungscontainer registriert, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Die `INavigationService` Schnittstelle wird aufgelöst, der `ViewModelBase` Klassenkonstruktor, wie im folgenden Codebeispiel gezeigt:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Dies gibt einen Verweis auf die `NavigationService` -Objekt, das in der Autofac abhängigkeitseinschleusungscontainer gespeichert ist, das von erstellt wird die `InitNavigation` Methode in der `App` Klasse. Weitere Informationen finden Sie unter [navigieren bei der App wird gestartet,](#navigating_when_the_app_is_launched).

Die `ViewModelBase` -Klasse speichert die `NavigationService` -Instanz in eine `NavigationService` Eigenschaft vom Typ `INavigationService`. Aus diesem Grund alle Modellklassen, die Ableitung Anzeigen der `ViewModelBase` Klasse können mithilfe der `NavigationService` Eigenschaft, um den Zugriff auf die Methoden, die gemäß der `INavigationService` Schnittstelle. Dadurch wird der Aufwand von Räumen vermieden der `NavigationService` Objekt aus der Autofac abhängigkeitseinschleusungscontainer in jedes Modell Ansichtsklasse.

### <a name="handling-navigation-requests"></a>Verarbeitung von Anforderungen an Navigation

Xamarin.Forms stellt die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) -Klasse, die eine hierarchische Navigation Erfahrung implementiert, in dem der Benutzer können zum Navigieren durch die Seiten, die Rückwärtsrichtung, wie gewünscht ist. Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

Anstelle der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) -Klasse direkt verwendet, die eShopOnContainers app umschließt die `NavigationPage` -Klasse in der `CustomNavigationView` -Klasse, wie im folgenden Codebeispiel wird dargestellt:

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

Dieser Umbruch zur Vereinfachung der Styling dient der [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Instanz innerhalb der XAML-Datei für die Klasse.

Navigation erfolgt innerhalb von Modellklassen Ansicht durch Aufrufen eines der `NavigateToAsync` Methoden, Angeben der Ansichtstyp Modell für die Seite navigiert zu, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Das folgende Codebeispiel zeigt die `NavigateToAsync` bereitgestellten Methoden die `NavigationService` Klasse:

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

Jede Methode kann jede beliebige Modellklasse für anzeigen, die von abgeleitet ist die `ViewModelBase` Klasse, um die hierarchische Navigation durch den Aufruf Ausführen der `InternalNavigateToAsync` Methode. Darüber hinaus die zweite `NavigateToAsync` Methode ermöglicht die Navigation Leistungsdaten als Argument angegeben werden, die übergeben wird, um das Modell anzeigen, die navigiert wird, bei denen es in der Regel dient für die Initialisierung. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

Die `InternalNavigateToAsync` Methode führt die navigationsanforderung und wird im folgenden Codebeispiel gezeigt:

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

Die `InternalNavigateToAsync` -Methode führt die Navigation zu einem Ansichtsmodell zunächst die `CreatePage` Methode. Diese Methode sucht die Sicht, die für die angegebene Ansicht Modelltyp entspricht und erstellt und gibt eine Instanz dieses Typs Sicht zurück. Suchen die Sicht, die den Ansichtstyp Modell entspricht, wird einen konventionsbasierte-Ansatz, der nämlich die annimmt, dass verwendet:

-   Sichten befinden sich in derselben Assembly wie Modelltypen anzeigen.
-   Ansichten werden ein. Ansichten untergeordneter Namespace.
-   Ansichtsmodelle ein. ViewModels untergeordneter Namespace.
-   Sichtnamen entsprechen, zum Anzeigen der Modellnamen, mit "Modell" entfernt.

Bei der Instanziierung einer Ansicht hat er die entsprechenden Ansichtsmodell zugeordnet. Weitere Informationen dazu, wie in diesem Fall, finden Sie unter [erstellen automatisch mit einer Ansicht Modell Locator einem Ansichtsmodell](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Wenn die Ansicht erstellt wird eine `LoginView`, es wird eine neue Instanz der umhüllt die `CustomNavigationView` Klasse und zugewiesen wurden die [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) Eigenschaft. Andernfalls die `CustomNavigationView` Instanz abgerufen und angegeben, dass es nicht null ist, ist die [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Methode wird aufgerufen, um die Sicht erstellt wird, auf dem Navigationsstapel mithilfe von Push übertragen. Jedoch wenn abgerufenen `CustomNavigationView` Instanz ist `null`, wird die Sicht erstellt eine neue Instanz der umhüllt der `CustomNavigationView` Klasse und zugewiesen wurden die `Application.Current.MainPage` Eigenschaft. Dieser Mechanismus, der während der Navigation wird sichergestellt, werden Seiten richtig Navigationsstapel hinzugefügt, wenn sie leer ist und wenn sie Daten enthalten.

> [!TIP]
> Berücksichtigen Sie die Seiten. Zwischenspeichern Seitenergebnisse Speicherverbrauch für Sichten, die derzeit nicht angezeigt werden. Allerdings ohne Seite zwischenspeichern bedeutet es, dass XAML-Analyse und zur Erstellung der auf der Seite und dessen Ansichtsmodell werden jedes Mal ausgeführt werden, was für eine komplexe Auswirkungen auf die Leistung auswirken kann, eine neue Seite navigiert wird. Für eine gut entworfene Seite, die keine übermäßige Anzahl von Steuerelementen verwendet, sollte die Leistung ausreichend sein. Zwischenspeichern von Seiten möglicherweise jedoch hilfreich, wenn die Seite der langsamen Ladezeiten gefunden werden.

Nachdem die Sicht erstellt und navigiert, wird die `InitializeAsync` Methode von der Sicht zugeordneten Ansichtsmodell ausgeführt wird. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Navigieren bei der App wird gestartet.

Wenn die app gestartet wird, die `InitNavigation` Methode in der `App` -Klasse aufgerufen wird. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Die Methode erstellt ein neues `NavigationService` Objekt in der Autofac abhängigkeitseinschleusungscontainer und gibt einen Verweis darauf, vor dem Aufrufen der `InitializeAsync` Methode.

> [!NOTE]
> Wenn die `INavigationService` Schnittstelle wird aufgelöst, indem die `ViewModelBase` -Klasse, die Container gibt einen Verweis auf die `NavigationService` -Objekt, das erstellt wurde, wenn die InitNavigation-Methode aufgerufen wird.

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

Die `MainView` zum navigiert wird, wenn die app einen zwischengespeicherten Zugriffstokens verfügt, die für die Authentifizierung verwendet wird. Andernfalls die `LoginView` navigiert wird.

Weitere Informationen zu den Autofac abhängigkeitseinschleusungscontainer, finden Sie unter [Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Übergeben von Parametern während der Navigation

Eines der `NavigateToAsync` Methoden, die gemäß der `INavigationService` Schnittstelle ermöglicht Navigationsdaten als Argument angegeben werden, die übergeben wird, um das Modell anzeigen, die navigiert wird, bei denen es in der Regel dient für die Initialisierung.

Z. B. die `ProfileViewModel` Klasse enthält eine `OrderDetailCommand` , die ausgeführt wird, wenn der Benutzer auf eine Bestellung auswählt der `ProfileView` Seite. Dagegen führt dies die `OrderDetailAsync` Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Diese Methode ruft die Navigation zu der `OrderDetailViewModel`, und übergeben Sie ein `Order` -Instanz, die die Reihenfolge, die Auswahl des Benutzers darstellt auf die `ProfileView` Seite. Wenn die `NavigationService` Klasse erstellt die `OrderDetailView`, die `OrderDetailViewModel` Klasse instanziiert und der Ansicht zugewiesen wird [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). Navigieren Sie zu der `OrderDetailView`, die `InternalNavigateToAsync` Methode führt die `InitializeAsync` Methode der Sicht zugeordneten Ansichtsmodell.

Die `InitializeAsync` Methode definiert ist, der `ViewModelBase` Klasse als eine Methode, die überschrieben werden kann. Diese Methode gibt ein `object` Argument, das die Daten an einem Ansichtsmodell übergeben werden, während ein Navigationsvorgang darstellt. Aus diesem Grund Modell Ansichtsklassen, die Daten über ein Navigationsvorgang erhalten möchten, geben Sie eine eigene Implementierung von der `InitializeAsync` Methode, um die erforderliche Initialisierung auszuführen. Das folgende Codebeispiel zeigt die `InitializeAsync` Methode aus der `OrderDetailViewModel` Klasse:

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

Diese Methode ruft die `Order` -Instanz, die während des Vorgangs für die Navigation in der Ansichtsmodell übergeben wurde und verwendet, um die vollständige Reihenfolge abgerufen werden, von der `OrderService` Instanz.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Aufrufenden Navigation mithilfe von Verhaltensweisen

Navigation wird aus einer Sicht in der Regel durch eine Benutzerinteraktion ausgelöst. Z. B. die `LoginView` Navigation nach erfolgreicher Authentifizierung ausführt. Im folgenden Codebeispiel wird veranschaulicht, wie die Navigation durch ein Verhalten aufgerufen wird:

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

Zur Laufzeit die `EventToCommandBehavior` antwortet auf die Interaktion mit der [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Wenn die `WebView` navigiert zu einer Webseite die [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) Ereignis wird ausgelöst, die ausgeführt werden die `NavigateCommand` in der `LoginViewModel`. Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben. Diese Daten werden konvertiert, wie sie zwischen Quelle und Ziel, vom Konverter im angegebenen übergeben wird der `EventArgsConverter` -Eigenschaft, die zurückgibt, die [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) aus der [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). Aus diesem Grund, wenn die `NavigationCommand` wird ausgeführt, die Url der Webseite wird als Parameter an übergeben den registrierten `Action`.

Wiederum die `NavigationCommand` führt die `NavigateAsync` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Diese Methode ruft die Navigation zu der `MainViewModel`, und befolgen die Navigation, entfernt die `LoginView` Seite Navigationsstapel.

### <a name="confirming-or-cancelling-navigation"></a>Bestätigen oder Abbrechen der Navigation

Eine app für die Interaktion mit dem Benutzer während eines Vorgangs Navigation, damit der Benutzer bestätigen kann, oder "Abbrechen" Navigation müssen möglicherweise. Dies könnte z. B. erforderlich sein, wenn der Benutzer versucht, navigieren, bevor Sie eine Dateneingabeseite vollständig abgeschlossen. In diesem Fall eine app eine Benachrichtigung zu versehen, die ermöglicht es dem Benutzer, die Seite zu verlassen, oder den Navigationsvorgang abgebrochen, bevor sie erfolgt. Dies kann auf eine Modellklasse Ansicht mit der Antwort von einer Benachrichtigung steuern, ob Navigationsfenster aufgerufen wird erreicht werden.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms umfasst Unterstützung für die Seitennavigation, was in der Regel aus der Benutzerinteraktion mit der Benutzeroberfläche oder aus der app selbst aufgrund interner Geschäftslogik gesteuerte Zustandsänderungen führt. Navigation kann jedoch komplex, um in apps implementieren, die das MVVM-Muster verwenden.

In diesem Kapitel dargestellt eine `NavigationService` -Klasse, die verwendet wird, um die Navigation in der Model First aus Ansichtsmodelle ausführen. Platzieren die Navigationslogik in der Sicht Modellklassen bedeutet, dass, dass die Logik über automatisierte Tests ausgeführt werden kann. Darüber hinaus kann das Ansichtsmodell dann Logik zum Steuerelement Navigationsbereich, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden implementieren.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
