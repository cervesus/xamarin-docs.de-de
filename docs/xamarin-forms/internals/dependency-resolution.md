---
title: Auflösung von Abhängigkeiten in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie beim Einfügen von einer Abhängigkeit Auflösungsmethode in Xamarin.Forms, damit eine Anwendung Dependency Injection-Container Kontrolle über die Erstellung und Lebensdauer von benutzerdefinierten Renderern, Auswirkungen und DependencyService-Implementierungen kann.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 9226e1d26dcc49b6ec82b71f7757eb0e22cd66ec
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171975"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Auflösung von Abhängigkeiten in Xamarin.Forms

_In diesem Artikel wird erläutert, wie beim Einfügen von einer Abhängigkeit Auflösungsmethode in Xamarin.Forms, damit eine Anwendung Dependency Injection-Container Kontrolle über die Erstellung und Lebensdauer von benutzerdefinierten Renderern, Auswirkungen und DependencyService-Implementierungen kann. Die Codebeispiele in diesem Artikel stammen aus der [Abhängigkeitsauflösung mithilfe von Containern](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/) Beispiel._

Im Rahmen einer Xamarin.Forms-Anwendung, die das Model-View-ViewModel (MVVM)-Muster verwendet, kann ein Container für Abhängigkeitsinjektion für das Registrieren und Auflösen von anzeigemodelle, und klicken Sie zum Registrieren von Diensten und sie dann in Ansichtsmodelle einzuschleusen verwendet werden. Während der Modellerstellung anzeigen fügt der Container alle Abhängigkeiten, die erforderlich sind. Wenn diese Abhängigkeiten nicht erstellt wurden, wird der Container erstellt, und löst zuerst die Abhängigkeiten. Weitere Informationen über Abhängigkeitsinjektion, einschließlich Beispiele für die Injektion von Abhängigkeiten in Modelle anzeigen, finden Sie unter [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Kontrolle über die Erstellung und Lebensdauer von Typen in Projekten-Plattform erfolgt normalerweise von Xamarin.Forms, mit dem verwendet die `Activator.CreateInstance` Methode zum Erstellen von Instanzen von benutzerdefinierten Renderern, Effekte und [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) Implementierungen. Leider beschränkt entwicklersteuerung für die Erstellung und Lebensdauer von diesen Typen und die Möglichkeit, Abhängigkeiten in diese einzufügen. Dieses Verhalten kann geändert werden, durch eine Abhängigkeit Auflösungsmethode in Xamarin.Forms der steuert, wie Typen erstellt werden – entweder indem Sie der Anwendung Dependency Injection-Container oder Xamarin.Forms einfügt. Beachten Sie jedoch, dass keine Notwendigkeit besteht, eine Abhängigkeit Auflösungsmethode in Xamarin.Forms einzufügen. Xamarin.Forms wird zum Erstellen und Verwalten der Lebensdauer von Typen in Projekten-Plattform, wenn eine Abhängigkeit Auflösungsmethode eingefügt wird nicht fortgesetzt.

> [!NOTE]
> Während dieser Artikel konzentriert sich auf das Einfügen von einer Abhängigkeit Auflösungsmethode in Xamarin.Forms, die registrierte Typen mit Dependency Injection-Container aufgelöst wird, ist es auch möglich, eine Abhängigkeit Auflösung-Methode einzufügen, die Factorymethoden verwendet, um aufzulösen registrierten Typen. Weitere Informationen finden Sie unter den [Abhängigkeitsauflösung mithilfe der Factorymethoden](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/FactoriesDemo/) Beispiel.

## <a name="injecting-a-dependency-resolution-method"></a>Einfügen einer Abhängigkeit Auflösung-Methode

Die [ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver) Klasse bietet die Möglichkeit, eine Abhängigkeit Auflösungsmethode in Xamarin.Forms einfügen mit der [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) Methode. Klicken Sie dann, wenn Xamarin.Forms eine Instanz eines bestimmten Typs benötigt, die Auflösungsmethode Abhängigkeit die Möglichkeit, die Instanz erhält. Wenn die Auflösungsmethode Abhängigkeit zurückgibt `null` für einen angeforderten Typ Xamarin.Forms liegt an der Versuch, den Typ zu erstellen-Instanz mithilfe der `Activator.CreateInstance` Methode.

Im folgende Beispiel veranschaulicht die legen Sie der Abhängigkeit Auflösung-Methode, mit der [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) Methode:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

In diesem Beispiel wird die Auflösungsmethode Abhängigkeit auf einen Lambda-Ausdruck festgelegt, die den Container für Abhängigkeitsinjektion Autofac verwendet wird, um alle Typen aufzulösen, die mit dem Container registriert wurden. Andernfalls `null` zurückgegeben, das führt zu Xamarin.Forms, es wird versucht, den Typ aufzulösen.

> [!NOTE]
> Von DI-Containern verwendete API ist spezifisch für den Container. Die Codebeispiele in diesem Artikel verwenden Sie Autofac als ein Container für Abhängigkeitsinjektion, bietet der `IContainer` und `ContainerBuilder` Typen. Alternative Dependency Injection-Containern konnte in gleichem Maße verwendet werden, jedoch würden unterschiedliche APIs verwenden, als hier präsentiert werden.

Beachten Sie, dass es nicht erforderlich ist, die Auflösungsmethode Abhängigkeit beim Starten der Anwendung festgelegt. Es kann zu einem beliebigen Zeitpunkt festgelegt werden. Die einzige Einschränkung besteht, dass Xamarin.Forms muss die Auflösungsmethode für die Abhängigkeit der Zeit kennen, die die Anwendung versucht, Typen, die gespeichert werden, in dem Container für Abhängigkeitsinjektion nutzen. Aus diesem Grund sind es Dienste, in dem Container für Abhängigkeitsinjektion, den während des Starts der Anwendung erforderlich sind, müssen die Auflösungsmethode Abhängigkeit zu einem frühen Zeitpunkt in den Lebenszyklus der Anwendung festgelegt werden. Auf ähnliche Weise, wenn der Dependency Injection-Container die Erstellung und Lebensdauer einer bestimmten verwaltet [ `Effect` ](xref:Xamarin.Forms.Effect), Xamarin.Forms müssen die Auflösungsmethode Abhängigkeit kennen, bevor versucht wird, erstellen Sie eine Ansicht, wird verwendet, die `Effect`.

> [!WARNING]
> Registrieren und Auflösen von Typen mit Dependency Injection-Container verfügt über die Leistung aufgrund des Containers mithilfe der Reflektion für jeden Typ erstellen, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der Anwendung wiederhergestellt wird, sind. Wenn viele oder deep-Abhängigkeiten vorhanden sind, kann die Kosten für die Erstellung erheblich erhöhen.

## <a name="registering-types"></a>Registrieren von Typen

Typen müssen mit dem Dependency Injection-Container registriert werden, bevor sie sie über die Auflösungsmethode Abhängigkeit auflösen kann. Das folgende Codebeispiel zeigt den Registrierungsmethoden, dass die beispielanwendung in verfügbar macht die `App` -Klasse, für den Autofac-Container:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

Wenn eine Anwendung eine Abhängigkeit Auflösung-Methode zum Auflösen von Typen aus einem Container verwendet wird, werden die Typ-Registrierungen in der Regel von Plattformprojekte ausgeführt. Dadurch Plattformprojekte Typen für die benutzerdefinierte Renderer, Effekte, registrieren und [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) Implementierungen.

Folgende typregistrierung aus ein Plattform-Projekt, das `IContainer` Objekt muss erstellt werden, die erfolgt durch Aufrufen der `BuildContainer` Methode. Diese Methode ruft die Autofac `Build` Methode für die `ContainerBuilder` -Instanz, die einen neue Dependency Injection-Container erstellt, die die Registrierungen enthält, die vorgenommen wurden.

In den folgenden Abschnitten, eine `Logger` Klasse, die implementiert die `ILogger` Schnittstelle in der Klasse, Konstruktoren eingefügt wird. Die `Logger` Klasse implementiert, einfache Protokollierung Funktionen über die `Debug.WriteLine` -Methode und wird verwendet, um zu veranschaulichen, wie Dienste in benutzerdefinierten Renderern Effekte, eingefügt werden können und [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) Implementierungen.

### <a name="registering-custom-renderers"></a>Registrieren benutzerdefinierte Renderer

Die beispielanwendung enthält eine Seite, die Webvideos, spielt, deren XAML-Quelle im folgenden Beispiel gezeigt wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

Die `VideoPlayer` Ansicht wird auf jeder Plattform von implementiert eine `VideoPlayerRenderer` -Klasse, mit den Funktionen für die Wiedergabe des Videos. Weitere Informationen zu diesen benutzerdefinierten Renderer-Klassen finden Sie unter [implementieren einen Videoplayer](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

Auf IOS- und die universelle Windows-Plattform (UWP) die `VideoPlayerRenderer` Klassen verfügen über den folgenden Konstruktor erfordert eine `ILogger` Argument:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Für alle Plattformen typregistrierung mit dem Dependency Injection-Container ausgeführt wird, durch die `RegisterTypes` -Methode, die aufgerufen wird, bevor Sie die Plattform laden die Anwendung mit der `LoadApplication(new App())` Methode. Das folgende Beispiel zeigt die `RegisterTypes` Methode für die iOS-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

In diesem Beispiel die `Logger` konkreter Typ über eine Zuordnung mit der Schnittstellentyp, registriert ist und die `VideoPlayerRenderer` Typ wird direkt ohne eine schnittstellenzuordnung registriert. Wenn der Benutzer navigiert, um die Seite mit der `VideoPlayer` anzeigen, die Auflösungsmethode Abhängigkeit aufgerufen, um das Beheben der `VideoPlayerRenderer` Typ aus dem Dependency Injection-Container, die ebenfalls zu beheben und eingefügt wird die `Logger` Geben Sie in die `VideoPlayerRenderer` Konstruktor.

Die `VideoPlayerRenderer` Konstruktor für die Android-Plattform ist etwas komplizierter, da es erfordert eine `Context` Argument zusätzlich zu den `ILogger` Argument:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Das folgende Beispiel zeigt die `RegisterTypes` Methode für die Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In diesem Beispiel die `App.RegisterTypeWithParameters` Methode registriert den `VideoPlayerRenderer` mit dem Dependency Injection-Container. Der Registrierungsmethode wird sichergestellt, dass die `MainActivity` Instanz wird als injiziert die `Context` Argument, und die `Logger` Typ wird als injiziert die `ILogger` Argument.

### <a name="registering-effects"></a>Registrieren von Effekten

Die beispielanwendung enthält eine Seite, die eine Fingereingabe Auswirkungen nachverfolgung verwendet, ziehen [ `BoxView` ](xref:Xamarin.Forms.BoxView) Instanzen, um die Seite. Die [ `Effect` ](xref:Xamarin.Forms.Effect) wird hinzugefügt, um die `BoxView` mithilfe des folgenden Codes:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

Die `TouchEffect` -Klasse ist eine [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) , die auf jeder Plattform von implementiert wird eine `TouchEffect` -Klasse, hat eine `PlatformEffect`. Die Plattform `TouchEffect` -Klasse enthält die Funktionen für das Ziehen der `BoxView` rund um die Seite. Weitere Informationen zu diesen Auswirkungen-Klassen finden Sie unter [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Auf allen Plattformen die `TouchEffect` -Klasse verfügt über den folgenden Konstruktor erfordert eine `ILogger` Argument:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Für alle Plattformen typregistrierung mit dem Dependency Injection-Container ausgeführt wird, durch die `RegisterTypes` -Methode, die aufgerufen wird, bevor Sie die Plattform laden die Anwendung mit der `LoadApplication(new App())` Methode. Das folgende Beispiel zeigt die `RegisterTypes` Methode für die Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

In diesem Beispiel die `Logger` konkreter Typ über eine Zuordnung mit der Schnittstellentyp, registriert ist und die `TouchEffect` Typ wird direkt ohne eine schnittstellenzuordnung registriert. Wenn der Benutzer navigiert, um die Seite mit einem [ `BoxView` ](xref:Xamarin.Forms.BoxView) Instanz, die die `TouchEffect` angefügt ist, die Auflösungsmethode Abhängigkeit aufgerufen, um die Plattform zu beheben `TouchEffect` Typ aus der Abhängigkeit Container für Abhängigkeitsinjektion, die ebenfalls zu beheben und Einfügen wird das `Logger` Geben Sie in der `TouchEffect` Konstruktor.

### <a name="registering-dependencyservice-implementations"></a>Registrieren von DependencyService-Implementierungen

Die beispielanwendung enthält eine Seite, verwendet [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) Implementierungen auf jeder Plattform, die der Benutzer ein Foto-Bildbibliothek des Geräts auswählen kann. Die `IPhotoPicker` Schnittstelle definiert die Funktionalität, die von implementiert ist die `DependencyService` -Implementierungen und wird im folgenden Beispiel angezeigt:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

In jedem plattformprojekt die `PhotoPicker` -Klasse implementiert die `IPhotoPicker` Schnittstelle, die Plattform-APIs. Weitere Informationen zu diesen abhängigkeitsdiensten ist, finden Sie unter [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Auf IOS- und UWP die `PhotoPicker` Klassen verfügen über den folgenden Konstruktor erfordert eine `ILogger` Argument:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Für alle Plattformen typregistrierung mit dem Dependency Injection-Container ausgeführt wird, durch die `RegisterTypes` -Methode, die aufgerufen wird, bevor Sie die Plattform laden die Anwendung mit der `LoadApplication(new App())` Methode. Das folgende Beispiel zeigt die `RegisterTypes` Methode für UWP:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

In diesem Beispiel die `Logger` konkreter Typ über eine Zuordnung mit der Schnittstellentyp, registriert ist und die `PhotoPicker` Typ wird auch über eine schnittstellenzuordnung registriert.

Die `PhotoPicker` Konstruktor für die Android-Plattform ist etwas komplizierter, da es erfordert eine `Context` Argument zusätzlich zu den `ILogger` Argument:

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Das folgende Beispiel zeigt die `RegisterTypes` Methode für die Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In diesem Beispiel die `App.RegisterTypeWithParameters` Methode registriert den `PhotoPicker` mit dem Dependency Injection-Container. Der Registrierungsmethode wird sichergestellt, dass die `MainActivity` Instanz wird als injiziert die `Context` Argument, und die `Logger` Typ wird als injiziert die `ILogger` Argument.

Wenn der Benutzer zu der Seite der Fotos auswählen navigiert und entscheidet sich für ein Foto, wählen die `OnSelectPhotoButtonClicked` Handler wird ausgeführt:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

Bei der [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) -Methode wird aufgerufen, die Auflösungsmethode Abhängigkeit aufgerufen, um das Beheben der `PhotoPicker` Typ aus dem Dependency Injection-Container, die ebenfalls zu beheben und eingefügt wird die `Logger` Typ in der `PhotoPicker` Konstruktor.

> [!NOTE]
> Die [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) Methode muss verwendet werden, beim Auflösen eines Typs aus der Anwendung Dependency Injection-Container über die [ `DependencyService` ](xref:Xamarin.Forms.DependencyService).

## <a name="related-links"></a>Verwandte Links

- [Abhängigkeitsauflösung mit Containern (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/DIContainerDemo/)
- [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Implementieren einen Videoplayer](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Aufrufen von Ereignissen von Auswirkungen](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Wählen ein Foto aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
