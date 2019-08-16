---
title: Navigation in der Unternehmens Anwendung
description: In diesem Kapitel wird erläutert, wie die eshoponcontainers-Mobile App das Anzeigen von Model First-Navigation aus Ansichts Modellen ausführt.
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3ca06ae6fb26fce87f14b9cdb34a700ef49655e1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528387"
---
# <a name="enterprise-app-navigation"></a>Navigation in der Unternehmens Anwendung

Xamarin. Forms bietet Unterstützung für die Seitennavigation, die in der Regel durch die Interaktion des Benutzers mit der Benutzeroberfläche oder von der APP selbst infolge interner, Logik gesteuerter Zustandsänderungen resultiert. Allerdings kann die Navigation in apps, die das Model-View-ViewModel (MVVM)-Muster verwenden, sehr komplex sein, da die folgenden Anforderungen erfüllt sein müssen:

- Identifizieren der Sicht, zu der navigiert werden soll, mithilfe eines Ansatzes, der keine enge Kopplung und Abhängigkeiten zwischen Sichten einführt.
- Der Prozess, mit dem die Sicht, zu der navigiert werden soll, koordiniert und initialisiert wird. Bei der Verwendung von MVVM müssen das Ansichts-und Ansichts Modell instanziiert und einander über den Bindungs Kontext der Ansicht zugeordnet werden. Wenn eine APP einen Container für die Abhängigkeitsinjektion verwendet, erfordert die Instanziierung von Sichten und Ansichts Modellen möglicherweise einen bestimmten Konstruktions Mechanismus.
- Gibt an, ob die Ansicht erste Navigation durchgeführt oder die Modell erste Navigation angezeigt werden soll. Beim Anzeigen der ersten Navigation verweist die Seite, zu der navigiert werden soll, auf den Namen des Ansichts Typs. Während der Navigation wird die angegebene Sicht zusammen mit dem entsprechenden Ansichts Modell und anderen abhängigen Diensten instanziiert. Ein alternativer Ansatz ist die Verwendung der View Model-First-Navigation, bei der die Seite, zu der navigiert werden soll, auf den Namen des Ansichts Modell Typs verweist.
- Es wird erläutert, wie das Navigationsverhalten der app in den Ansichten und Ansichts Modellen ordnungsgemäß getrennt wird. Das MVVM-Muster bietet eine Trennung zwischen der Benutzeroberfläche der APP und ihrer Darstellung und Geschäftslogik. Das Navigationsverhalten einer APP umfasst jedoch häufig die Benutzeroberflächen-und Präsentations Teile der app. Der Benutzer initiiert häufig die Navigation aus einer Ansicht, und die Ansicht wird als Ergebnis der Navigation ersetzt. Allerdings muss die Navigation häufig auch im Ansichts Modell initiiert oder koordiniert werden.
- Übergeben von Parametern während der Navigation für Initialisierungs Zwecke. Wenn der Benutzer z. b. zu einer Ansicht navigiert, um Bestelldetails zu aktualisieren, müssen die Bestelldaten an die Ansicht übermittelt werden, damit die richtigen Daten angezeigt werden können.
- Erfahren Sie, wie Sie die Navigation koordinieren, um sicherzustellen, dass bestimmte Geschäftsregeln befolgt werden. Beispielsweise werden Benutzer möglicherweise aufgefordert, vor einer Ansicht zu navigieren, damit Sie alle ungültigen Daten korrigieren können, oder Sie werden aufgefordert, Datenänderungen, die in der Ansicht vorgenommen wurden, zu übermitteln oder zu verwerfen.

In diesem Kapitel werden diese Herausforderungen behandelt `NavigationService` , indem eine Klasse vorgestellt wird, die für die Ausführung der ersten Seitennavigation in View Model verwendet wird.

> [!NOTE]
> Der `NavigationService` von der APP verwendete dient nur der hierarchischen Navigation zwischen ContentPage-Instanzen. Die Verwendung des-Dienstanbieter zum Navigieren zwischen anderen Seiten Typen kann zu unerwartetem Verhalten führen.

## <a name="navigating-between-pages"></a>Navigieren zwischen Seiten

Navigations Logik kann sich im Code Behind einer Ansicht oder in einem Daten gebundenen Ansichts Modell befinden. Das Platzieren der Navigations Logik in einer Ansicht ist möglicherweise der einfachste Ansatz, aber es ist nicht einfach, über Komponententests zu testen. Das Platzieren der Navigations Logik in Ansichts Modellklassen bedeutet, dass die Logik mithilfe von Komponententests ausgeführt werden kann. Außerdem kann das Ansichts Modell eine Logik zum Steuern der Navigation implementieren, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden. Beispielsweise kann eine APP nicht zulassen, dass der Benutzer von einer Seite weg navigiert, ohne zuvor sicherzustellen, dass die eingegebenen Daten gültig sind.

Eine `NavigationService` Klasse wird in der Regel von Ansichts Modellen aufgerufen, um die Test barkeit zu fördern. Das Navigieren zu Sichten von Ansichts Modellen erfordert jedoch, dass die Ansichts Modelle auf Sichten verweisen, und insbesondere auf Sichten, denen das aktive Ansichts Modell nicht zugeordnet ist. Dies wird nicht empfohlen. Daher gibt das `NavigationService` hier aufgeführte den Ansichts Modelltyp als Ziel für die Navigation an.

Der eshoponcontainers-Mobile App verwendet `NavigationService` die-Klasse, um die erste Navigation im Ansichts Modell bereitzustellen. Diese Klasse implementiert die `INavigationService` -Schnittstelle, die im folgenden Codebeispiel gezeigt wird:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Diese Schnittstelle gibt an, dass eine implementierende Klasse die folgenden Methoden bereitstellen muss:

|Methode|Zweck|
|--- |--- |
|`InitializeAsync`|Führt die Navigation zu einer von zwei Seiten durch, wenn die APP gestartet wird.|
|`NavigateToAsync`|Führt eine hierarchische Navigation zu einer angegebenen Seite aus.|
|`NavigateToAsync(parameter)`|Führt eine hierarchische Navigation zu einer angegebenen Seite durch und übergibt einen Parameter.|
|`RemoveLastFromBackStackAsync`|Entfernt die vorherige Seite aus dem Navigations Stapel.|
|`RemoveBackStackAsync`|Entfernt alle vorherigen Seiten aus dem Navigations Stapel.|

Außerdem gibt die `INavigationService` -Schnittstelle an, dass eine implementierende Klasse `PreviousPageViewModel` eine Eigenschaft bereitstellen muss. Diese Eigenschaft gibt den Ansichts Modelltyp zurück, der der vorherigen Seite im Navigations Stapel zugeordnet ist.

> [!NOTE]
> Eine `INavigationService` Schnittstelle würde normalerweise auch `GoBackAsync` eine-Methode angeben, die für die programmgesteuerte Rückkehr zur vorherigen Seite im Navigations Stapel verwendet wird. Diese Methode fehlt jedoch in den eshoponcontainers-Mobile App, da Sie nicht erforderlich ist.

### <a name="creating-the-navigationservice-instance"></a>Erstellen der NavigationService-Instanz

Die `NavigationService` -Klasse, die die `INavigationService` -Schnittstelle implementiert, wird als Singleton mit dem Container für die Abhängigkeitsinjektion von autofac registriert, wie im folgenden Codebeispiel gezeigt:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

Die `INavigationService` -Schnittstelle wird `ViewModelBase` im-Klassenkonstruktor aufgelöst, wie im folgenden Codebeispiel gezeigt:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Dadurch wird ein Verweis auf das `NavigationService` -Objekt zurückgegeben, das im Container für die Abhängigkeitsinjektion von autofac gespeichert `InitNavigation` ist, der `App` von der-Methode in der-Klasse erstellt wird. Weitere Informationen finden Sie unter [Navigieren beim Starten der APP](#navigating_when_the_app_is_launched).

Die `ViewModelBase` -Klasse speichert `NavigationService` die-Instanz `NavigationService` in einer Eigenschaft vom `INavigationService`Typ. Daher können alle Ansichts Modellklassen, die von der `ViewModelBase` -Klasse abgeleitet werden, `NavigationService` die-Eigenschaft verwenden, um auf die `INavigationService` von der-Schnittstelle angegebenen Methoden zuzugreifen. Dadurch wird der Aufwand für das Einfügen `NavigationService` des Objekts aus dem Container der autofac-Abhängigkeitsinjektion in jede Ansichts Modell Klasse vermieden.

### <a name="handling-navigation-requests"></a>Behandeln von Navigationsanforderungen

Xamarin. Forms stellt die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) -Klasse bereit, die eine hierarchische Navigations Darstellung implementiert, bei der der Benutzer nach Belieben durch Seiten navigieren kann. Weitere Informationen zur hierarchischen Navigation finden Sie unter [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Hierarchische Navigation).

Anstatt die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) -Klasse direkt zu verwenden, umschließt die eshoponcontainers `NavigationPage` -APP die `CustomNavigationView` -Klasse in der-Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Der Zweck dieser Wrapping ist die einfache Formatierung der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) -Instanz in der XAML-Datei für die-Klasse.

Die Navigation erfolgt innerhalb von Ansichts Modellklassen, indem eine `NavigateToAsync` der-Methoden aufgerufen wird. dabei wird der Ansichts Modelltyp für die Seite angegeben, zu der navigiert wird, wie im folgenden Codebeispiel gezeigt:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

Das folgende Codebeispiel zeigt die `NavigateToAsync` Methoden, die von `NavigationService` der-Klasse bereitgestellt werden:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Jede Methode ermöglicht allen Ansichts Modellklassen, die von `ViewModelBase` der-Klasse abgeleitet werden, die hierarchische Navigation durch Aufrufen der `InternalNavigateToAsync` -Methode. Außerdem ermöglicht die zweite `NavigateToAsync` Methode die Angabe von Navigationsdaten als Argument, das an das Ansichts Modell, zu dem navigiert wird, an das Ansichts Modell übermittelt wird, in dem es normalerweise für die Initialisierung verwendet wird. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

Die `InternalNavigateToAsync` -Methode führt die Navigations Anforderung aus, und wird im folgenden Codebeispiel gezeigt:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

Die `InternalNavigateToAsync` Methode führt die Navigation zu einem Ansichts Modell durch, `CreatePage` indem zuerst die-Methode aufgerufen wird. Diese Methode dient zum Lokalisieren der Sicht, die dem angegebenen Ansichts Modelltyp entspricht, und erstellt eine Instanz dieses Ansichts Typs und gibt diese zurück. Beim Suchen der Sicht, die dem Ansichts Modelltyp entspricht, wird ein auf Konventionen basierender Ansatz verwendet, der Folgendes voraussetzt:

- Sichten befinden sich in derselben Assembly wie Ansichts Modelltypen.
- Sichten befinden sich in einer. Views untergeordneter Namespace.
- Ansichts Modelle befinden sich in einer. Untergeordneter ViewModels-Namespace.
- Ansichts Namen entsprechen Ansichts Modellnamen, wobei "Model" entfernt wurde.

Wenn eine Sicht instanziiert wird, wird Sie mit dem entsprechenden Ansichts Modell verknüpft. Weitere Informationen zur Vorgehensweise finden Sie unter [Automatisches Erstellen eines Ansichts Modells mit einem Ansichts Modell-Locator](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Wenn die Sicht, die erstellt wird `LoginView`, eine ist, wird Sie in eine neue Instanz `CustomNavigationView` der-Klasse umschließt [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) und der-Eigenschaft zugewiesen. Andernfalls wird die `CustomNavigationView` -Instanz abgerufen, und es wird angegeben, dass Sie nicht [`PushAsync`](xref:Xamarin.Forms.NavigationPage) NULL ist. die-Methode wird aufgerufen, um die Sicht, die erstellt wird, auf den Navigations Stapel zu verschieben. Wenn die abgerufene `CustomNavigationView` Instanz jedoch ist `null`, wird die Sicht, die erstellt wird, in `CustomNavigationView` eine neue Instanz der-Klasse gepackt und `Application.Current.MainPage` der-Eigenschaft zugewiesen. Dieser Mechanismus stellt sicher, dass Seiten während der Navigation ordnungsgemäß dem Navigations Stapel hinzugefügt werden, wenn er leer ist, und wenn er Daten enthält.

> [!TIP]
> Zwischenspeichern von Seiten. Das Zwischenspeichern von Seiten führt zur Arbeitsspeicher Auslastung für Sichten, die derzeit nicht angezeigt werden. Ohne Seiten Zwischenspeicherung bedeutet dies jedoch, dass die XAML-Verarbeitung und-Erstellung der Seite und das Ansichts Modell jedes Mal erfolgen, wenn eine neue Seite zu navigieren, was sich auf die Leistung einer komplexen Seite auswirkt. Bei einer gut entworfenen Seite, die keine übermäßige Anzahl von Steuerelementen verwendet, sollte die Leistung ausreichen. Das Zwischenspeichern von Seiten kann jedoch hilfreich sein, wenn langsame Seiten Ladezeiten auftreten.

Nachdem die Sicht erstellt und dorthin navigiert wurde, wird `InitializeAsync` die-Methode des zugeordneten Ansichts Modells der Sicht ausgeführt. Weitere Informationen finden Sie unter [übergeben von Parametern während der Navigation](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Navigieren beim Starten der APP

Wenn die APP gestartet wird, wird `InitNavigation` die-Methode `App` in der-Klasse aufgerufen. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Die-Methode erstellt ein `NavigationService` neues-Objekt im Container für die Abhängigkeitsinjektion von autofac und gibt einen Verweis darauf zurück `InitializeAsync` , bevor die zugehörige-Methode aufgerufen wird.

> [!NOTE]
> Wenn die `INavigationService` Schnittstelle von der `ViewModelBase` -Klasse aufgelöst wird, gibt der Container einen Verweis `NavigationService` auf das Objekt zurück, das beim Aufrufen der initnavigation-Methode erstellt wurde.

Im folgenden Codebeispiel wird die `NavigationService` `InitializeAsync` -Methode veranschaulicht:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

Wird `MainView` auf den navigiert, wenn die APP über ein zwischengespeichertes Zugriffs Token verfügt, das für die Authentifizierung verwendet wird. Andernfalls wird der `LoginView` navigiert zu.

Weitere Informationen zum Container "autofac-Abhängigkeitsinjektion" finden [Sie unter Einführung in die Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Übergeben von Parametern während der Navigation

Eine der `NavigateToAsync` -Methoden, die durch die `INavigationService` -Schnittstelle angegeben wird, ermöglicht das Angeben von Navigationsdaten als Argument, das an das Ansichts Modell, zu dem navigiert wird, an das Ansichts Modell geleitet wird

Beispielsweise enthält die `ProfileViewModel` -Klasse einen `OrderDetailCommand` , der ausgeführt wird, wenn der Benutzer eine Bestellung auf `ProfileView` der Seite auswählt. Dies führt wiederum die `OrderDetailAsync` -Methode aus, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Diese Methode ruft die `OrderDetailViewModel`Navigation zum auf und übergibt eine `Order` Instanz, die die vom Benutzer auf der `ProfileView` Seite ausgewählte Reihenfolge darstellt. Wenn die `NavigationService` -Klasse den `OrderDetailView`erstellt, `OrderDetailViewModel` wird die-Klasse instanziiert [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)und der der Ansicht zugewiesen. Nach dem Navigieren zum `OrderDetailView`führt die `InternalNavigateToAsync` -Methode die `InitializeAsync` -Methode des zugeordneten Ansichts Modells der Sicht aus.

Die `InitializeAsync` -Methode wird in der `ViewModelBase` -Klasse als eine Methode definiert, die überschrieben werden kann. Diese Methode gibt ein `object` Argument an, das die Daten darstellt, die während eines Navigations Vorgangs an ein Ansichts Modell übermittelt werden sollen. Daher bieten Ansichts Modellklassen, die Daten von einem Navigations Vorgang empfangen möchten, eine eigene Implementierung der `InitializeAsync` -Methode, um die erforderliche Initialisierung auszuführen. Das folgende Codebeispiel zeigt die `InitializeAsync` -Methode aus `OrderDetailViewModel` der-Klasse:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Diese Methode ruft die `Order` -Instanz ab, die während des Navigations Vorgangs an das Ansichts Modell geleitet wurde, und verwendet Sie, um die vollständigen Bestelldetails aus der `OrderService` -Instanz abzurufen.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Aufrufen der Navigation mithilfe von Verhalten

Die Navigation wird normalerweise durch eine Benutzerinteraktion von einer Ansicht ausgelöst. Beispielsweise führt die `LoginView` Navigation nach erfolgreicher Authentifizierung durch. Im folgenden Codebeispiel wird gezeigt, wie die Navigation durch ein Verhalten aufgerufen wird:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Zur Laufzeit antwortet der `EventToCommandBehavior` auf die [`WebView`](xref:Xamarin.Forms.WebView)Interaktion mit. Wenn die `WebView` zu einer Webseite navigiert, wird das [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) -Ereignis ausgelöst `NavigateCommand` , das in der ausgeführt `LoginViewModel`wird. Standardmäßig werden die Ereignis Argumente für das-Ereignis an den Befehl übermittelt. Diese Daten werden konvertiert, wenn Sie zwischen Quelle und Ziel durch den Konverter, der in der `EventArgsConverter` -Eigenschaft angegeben ist, [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) übermittelt [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs)werden, der die aus der zurückgibt. Wenn der `NavigationCommand` ausgeführt wird, wird daher die URL der Webseite als Parameter an die registrierte `Action`übergeben.

`NavigationCommand` Wiederum`NavigateAsync` führt die-Methode aus, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Diese Methode ruft die Navigation zum `MainViewModel`auf, und die folgende Navigation entfernt `LoginView` die Seite aus dem Navigations Stapel.

### <a name="confirming-or-cancelling-navigation"></a>Bestätigung oder Abbruch der Navigation

Eine APP muss möglicherweise während eines Navigations Vorgangs mit dem Benutzer interagieren, damit der Benutzer die Navigation bestätigen oder abbrechen kann. Dies kann z. b. erforderlich sein, wenn der Benutzer versucht, zu navigieren, bevor eine Dateneingabe Seite vollständig abgeschlossen ist. In dieser Situation sollte eine APP eine Benachrichtigung bereitstellen, die es dem Benutzer ermöglicht, von der Seite zu navigieren oder den Navigations Vorgang vor dem Auftreten abzubrechen. Dies kann in einer Ansichts Modell Klasse erreicht werden, indem die Antwort aus einer Benachrichtigung verwendet wird, um zu steuern, ob die Navigation aufgerufen wird.

## <a name="summary"></a>Zusammenfassung

Xamarin. Forms bietet Unterstützung für die Seitennavigation, die in der Regel durch die Interaktion des Benutzers mit der Benutzeroberfläche oder von der APP selbst infolge interner, Logik gesteuerte Zustandsänderungen resultiert. Allerdings kann die Navigation in apps, die das MVVM-Muster verwenden, komplex sein.

In diesem Kapitel wurde `NavigationService` eine-Klasse vorgestellt, die verwendet wird, um die Ansicht Model-First-Navigation von Ansichts Modellen auszuführen. Das Platzieren der Navigations Logik in Ansichts Modellklassen bedeutet, dass die Logik durch automatisierte Tests ausgeführt werden kann. Außerdem kann das Ansichts Modell eine Logik zum Steuern der Navigation implementieren, um sicherzustellen, dass bestimmte Geschäftsregeln erzwungen werden.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
