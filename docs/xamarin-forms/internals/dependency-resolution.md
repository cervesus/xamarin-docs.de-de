---
title: Abhängigkeitsauflösung in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie eine Abhängigkeits Auflösungsmethode in eingefügt Xamarin.Forms wird, damit der Abhängigkeits Injection-Container einer Anwendung die Erstellung und Lebensdauer von benutzerdefinierten Renderers, Effekten und dependencyservice-Implementierungen steuern kann.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ae30b4a4b75906613baf8a2568548c8890ccb33a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139086"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Abhängigkeitsauflösung in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)

_In diesem Artikel wird erläutert, wie eine Abhängigkeits Auflösungsmethode in eingefügt Xamarin.Forms wird, damit der Abhängigkeits Injection-Container einer Anwendung die Erstellung und Lebensdauer von benutzerdefinierten Renderers, Effekten und dependencyservice-Implementierungen steuern kann. Die Codebeispiele in diesem Artikel stammen aus dem Beispiel für die [Abhängigkeitsauflösung mithilfe von Containern](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo) ._

Im Kontext einer-Anwendung, die Xamarin.Forms das Model-View-ViewModel (MVVM)-Muster verwendet, kann ein Abhängigkeits Injektions Container zum Registrieren und Auflösen von Ansichts Modellen und zum Registrieren von Diensten und Einfügen in Ansichts Modelle verwendet werden. Während der Ansichts Modell Erstellung fügt der Container alle erforderlichen Abhängigkeiten ein. Wenn diese Abhängigkeiten nicht erstellt wurden, werden die Abhängigkeiten vom Container zuerst erstellt und aufgelöst. Weitere Informationen zur Abhängigkeitsinjektion, einschließlich Beispielen für das Einfügen von Abhängigkeiten in Ansichts Modelle, finden Sie unter [Abhängigkeitsinjektion](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Die Kontrolle über die Erstellung und die Lebensdauer von Typen in Platt Form Projekten erfolgt in der Regel durch Xamarin.Forms , wobei die-Methode verwendet wird, `Activator.CreateInstance` um Instanzen von benutzerdefinierten Renderer, Effekten und Implementierungen zu erstellen [`DependencyService`](xref:Xamarin.Forms.DependencyService) . Leider schränkt dies die Entwickler Kontrolle über die Erstellung und die Lebensdauer dieser Typen und die Möglichkeit ein, Abhängigkeiten in Sie einzufügen. Dieses Verhalten kann geändert werden, indem eine Abhängigkeits Auflösungsmethode in eingefügt wird Xamarin.Forms , mit der gesteuert wird, wie Typen erstellt werden – entweder durch den Abhängigkeits einschleusungs Container der Anwendung oder durch Xamarin.Forms . Beachten Sie jedoch, dass es nicht erforderlich ist, eine Methode zur Abhängigkeitsauflösung in einzufügen Xamarin.Forms . Xamarin.Formserstellt und verwaltet die Lebensdauer von Typen in Platt Form Projekten weiterhin, wenn eine Abhängigkeits Auflösungsmethode nicht eingefügt wird.

> [!NOTE]
> Der Schwerpunkt dieses Artikels liegt auf dem Einfügen einer Methode zur Abhängigkeitsauflösung in Xamarin.Forms , die registrierte Typen mithilfe eines Containers für die Abhängigkeitsinjektion auflöst. es ist auch möglich, eine Abhängigkeits Auflösungsmethode einzufügen, die Factorymethoden zum Auflösen registrierter Typen verwendet. Weitere Informationen finden Sie unter Beispiel für die [Abhängigkeitsauflösung mithilfe](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-factoriesdemo) von Factorymethoden.

## <a name="injecting-a-dependency-resolution-method"></a>Einfügen einer Methode zur Abhängigkeitsauflösung

Die- [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) Klasse bietet die Möglichkeit, mithilfe der-Methode eine Methode zur Abhängigkeitsauflösung in einzufügen Xamarin.Forms [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) . Wenn dann Xamarin.Forms eine Instanz eines bestimmten Typs benötigt, erhält die Abhängigkeits Auflösungsmethode die Möglichkeit, die Instanz bereitzustellen. Wenn die Abhängigkeits Auflösungsmethode `null` für einen angeforderten Typ zurückgibt, Xamarin.Forms greift auf den Versuch zurück, die Typinstanz selbst mithilfe der-Methode zu erstellen `Activator.CreateInstance` .

Im folgenden Beispiel wird gezeigt, wie die Methode zur Abhängigkeitsauflösung mit der-Methode festgelegt wird [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) :

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

In diesem Beispiel wird die Methode für die Abhängigkeitsauflösung auf einen Lambda-Ausdruck festgelegt, der den Container für die Abhängigkeitsinjektion von autofac verwendet, um alle Typen aufzulösen, die beim Container registriert wurden. Andernfalls `null` wird zurückgegeben, was dazu führt, dass Xamarin.Forms der Typ aufgelöst wird.

> [!NOTE]
> Die von einem Container für die Abhängigkeitsinjektion verwendete API ist für den Container spezifisch. Die Codebeispiele in diesem Artikel verwenden autofac als Container für die Abhängigkeitsinjektion, der den `IContainer` -Typ und den-Typ bereitstellt `ContainerBuilder` . Alternative abhängigkeiteneinschleusungs-Container könnten gleichzeitig verwendet werden, verwenden jedoch andere APIs als hier dargestellt werden.

Beachten Sie, dass es nicht erforderlich ist, beim Starten der Anwendung die Methode zur Abhängigkeitsauflösung festzulegen. Sie kann jederzeit festgelegt werden. Die einzige Einschränkung ist, dass die Xamarin.Forms Methode zur Abhängigkeitsauflösung von der Zeit wissen muss, in der die Anwendung versucht, im Container für die Abhängigkeitsinjektion gespeicherte Typen zu verarbeiten. Wenn im Abhängigkeits Injektions Containerdienste vorhanden sind, die die Anwendung während des Starts benötigt, muss die Abhängigkeits Auflösungsmethode daher früh im Lebenszyklus der Anwendung festgelegt werden. Ebenso muss, wenn der Container für die Abhängigkeitsinjektion die Erstellung und die Lebensdauer eines bestimmten verwaltet [`Effect`](xref:Xamarin.Forms.Effect) , Xamarin.Forms von der Abhängigkeits Auflösungsmethode informiert werden, bevor versucht wird, eine Ansicht zu erstellen, die diese verwendet `Effect` .

> [!WARNING]
> Das registrieren und Auflösen von Typen mit einem Container für die Abhängigkeitsinjektion wirkt sich auf die Leistung aus, da die Reflektion des Containers zum Erstellen der einzelnen Typen verwendet wird, insbesondere dann, wenn Abhängigkeiten für jede Seitennavigation in der Anwendung rekonstruiert werden. Wenn zahlreiche oder tiefe Abhängigkeiten vorhanden sind, können die Kosten für die Erstellung erheblich zunehmen.

## <a name="registering-types"></a>Registrieren von Typen

Typen müssen mit dem Container für die Abhängigkeitsinjektion registriert werden, bevor Sie Sie über die Methode zur Abhängigkeitsauflösung auflösen können. Das folgende Codebeispiel zeigt die Registrierungsmethoden, die von der Beispielanwendung in der- `App` Klasse für den autofac-Container verfügbar gemacht werden:

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

Wenn eine Anwendung eine Abhängigkeits Auflösungsmethode zum Auflösen von Typen aus einem Container verwendet, werden typregistrierungen normalerweise von Platt Form Projekten ausgeführt. Dies ermöglicht Platt Form Projekten das Registrieren von Typen für benutzerdefinierte Renderer, Effekte und [`DependencyService`](xref:Xamarin.Forms.DependencyService) Implementierungen.

Nach der Typregistrierung eines Platt Form Projekts `IContainer` muss das Objekt erstellt werden. Dies wird durch Aufrufen der- `BuildContainer` Methode erreicht. Diese Methode ruft die-Methode von autofac `Build` für die- `ContainerBuilder` Instanz auf, die einen neuen Container für die Abhängigkeitsinjektion erstellt, der die vorgenommenen Registrierungen enthält.

In den folgenden Abschnitten `Logger` wird eine Klasse, die die- `ILogger` Schnittstelle implementiert, in Klassenkonstruktoren eingefügt. Die `Logger` -Klasse implementiert einfache Protokollierungsfunktionen mithilfe der `Debug.WriteLine` -Methode und wird verwendet, um zu veranschaulichen, wie Dienste in benutzerdefinierte Renderer, Effekte und Implementierungen eingefügt werden können [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

### <a name="registering-custom-renderers"></a>Registrieren von benutzerdefinierten Renderer

Die Beispielanwendung enthält eine Seite, auf der Web-Videos abgespielt werden, deren XAML-Quelle im folgenden Beispiel gezeigt wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

Die `VideoPlayer` Ansicht wird auf jeder Plattform durch eine `VideoPlayerRenderer` Klasse implementiert, die die Funktionalität zum Abspielen des Videos bereitstellt. Weitere Informationen zu diesen benutzerdefinierten rendererklassen finden Sie unter [Implementieren eines Video Players](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

Unter IOS und der universelle Windows-Plattform (UWP) verfügen die `VideoPlayerRenderer` Klassen über den folgenden Konstruktor, der ein `ILogger` Argument erfordert:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Auf allen Plattformen wird die Typregistrierung mit dem Container für die Abhängigkeitsinjektion durch die- `RegisterTypes` Methode ausgeführt, die vor der Plattform, die die Anwendung mit der-Methode lädt, aufgerufen wird `LoadApplication(new App())` . Das folgende Beispiel zeigt die- `RegisterTypes` Methode auf der IOS-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

In diesem Beispiel wird der `Logger` konkrete Typ über eine Zuordnung für den Schnittstellentyp registriert, und der `VideoPlayerRenderer` Typ wird direkt ohne Schnittstellen Zuordnung registriert. Wenn der Benutzer zu der Seite navigiert, die die `VideoPlayer` Ansicht enthält, wird die Abhängigkeits Auflösungsmethode aufgerufen, um den `VideoPlayerRenderer` Typ aus dem Container für die Abhängigkeitsinjektion aufzulösen, der den Typ auch auflösen und `Logger` in den `VideoPlayerRenderer` Konstruktor einfügen wird.

Der `VideoPlayerRenderer` Konstruktor auf der Android-Plattform ist etwas komplizierter, da zusätzlich zum-Argument ein-Argument erforderlich ist `Context` `ILogger` :

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Das folgende Beispiel zeigt die- `RegisterTypes` Methode auf der Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In diesem Beispiel wird die- `App.RegisterTypeWithParameters` Methode `VideoPlayerRenderer` mit dem Container für die Abhängigkeitsinjektion registriert. Die Registrierungsmethode stellt sicher, dass die `MainActivity` -Instanz als `Context` -Argument eingefügt wird, und dass der `Logger` Typ als-Argument eingefügt wird `ILogger` .

### <a name="registering-effects"></a>Effekte werden registriert

Die Beispielanwendung enthält eine Seite, die einen Fingerabdruck nach Verfolgungs Effekt verwendet, um [`BoxView`](xref:Xamarin.Forms.BoxView) Instanzen um die Seite zu ziehen. Der [`Effect`](xref:Xamarin.Forms.Effect) wird `BoxView` mit dem folgenden Code hinzugefügt:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

Die `TouchEffect` -Klasse ist eine [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) , die auf jeder Plattform durch eine `TouchEffect` Klasse implementiert wird, die eine ist `PlatformEffect` . Die Platform- `TouchEffect` Klasse stellt die Funktionalität zum Ziehen der `BoxView` auf der Seite bereit. Weitere Informationen zu diesen Effekt Klassen finden Sie unter [Aufrufen von Ereignissen aus Effekten](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Auf allen Plattformen verfügt die- `TouchEffect` Klasse über den folgenden Konstruktor, für den ein Argument erforderlich ist `ILogger` :

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Auf allen Plattformen wird die Typregistrierung mit dem Container für die Abhängigkeitsinjektion durch die- `RegisterTypes` Methode ausgeführt, die vor der Plattform, die die Anwendung mit der-Methode lädt, aufgerufen wird `LoadApplication(new App())` . Das folgende Beispiel zeigt die- `RegisterTypes` Methode auf der Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

In diesem Beispiel wird der `Logger` konkrete Typ über eine Zuordnung für den Schnittstellentyp registriert, und der `TouchEffect` Typ wird direkt ohne Schnittstellen Zuordnung registriert. Wenn der Benutzer zu der Seite navigiert, die eine-Instanz enthält, an [`BoxView`](xref:Xamarin.Forms.BoxView) die die-Instanz `TouchEffect` angefügt ist, wird die Abhängigkeits Auflösungsmethode aufgerufen, um den Platt `TouchEffect` Formtyp aus dem Container für die Abhängigkeitsinjektion aufzulösen, der den Typ ebenfalls auflöst und `Logger` in den `TouchEffect` Konstruktor eingefügt.

### <a name="registering-dependencyservice-implementations"></a>Registrieren von dependencyservice-Implementierungen

Die Beispielanwendung enthält eine Seite, die [`DependencyService`](xref:Xamarin.Forms.DependencyService) auf jeder Plattform Implementierungen verwendet, damit der Benutzer ein Foto aus der Bildbibliothek des Geräts auswählen kann. Die `IPhotoPicker` -Schnittstelle definiert die Funktionalität, die von den `DependencyService` -Implementierungen implementiert wird, und wird im folgenden Beispiel gezeigt:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

In jedem Platt Form Projekt implementiert die- `PhotoPicker` Klasse die- `IPhotoPicker` Schnittstelle mithilfe von Plattform-APIs. Weitere Informationen über diese Abhängigkeits Dienste finden Sie unter [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

Unter IOS und UWP verfügen die `PhotoPicker` Klassen über den folgenden Konstruktor, für den ein `ILogger` Argument erforderlich ist:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Auf allen Plattformen wird die Typregistrierung mit dem Container für die Abhängigkeitsinjektion durch die- `RegisterTypes` Methode ausgeführt, die vor der Plattform, die die Anwendung mit der-Methode lädt, aufgerufen wird `LoadApplication(new App())` . Im folgenden Beispiel wird die- `RegisterTypes` Methode für UWP veranschaulicht:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

In diesem Beispiel wird der `Logger` konkrete Typ über eine Zuordnung für den Schnittstellentyp registriert, und der `PhotoPicker` Typ wird auch über eine Schnittstellen Zuordnung registriert.

Der `PhotoPicker` Konstruktor auf der Android-Plattform ist etwas komplizierter, da zusätzlich zum-Argument ein-Argument erforderlich ist `Context` `ILogger` :

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

Das folgende Beispiel zeigt die- `RegisterTypes` Methode auf der Android-Plattform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In diesem Beispiel wird die- `App.RegisterTypeWithParameters` Methode `PhotoPicker` mit dem Container für die Abhängigkeitsinjektion registriert. Die Registrierungsmethode stellt sicher, dass die `MainActivity` -Instanz als `Context` -Argument eingefügt wird, und dass der `Logger` Typ als-Argument eingefügt wird `ILogger` .

Wenn der Benutzer zur fotopickingseite navigiert und ein Foto auswählt, wird der `OnSelectPhotoButtonClicked` Handler ausgeführt:

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

Wenn die- [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) Methode aufgerufen wird, wird die Abhängigkeits Auflösungsmethode aufgerufen, um den `PhotoPicker` Typ aus dem Container für die Abhängigkeitsinjektion aufzulösen, der den Typ ebenfalls auflöst und `Logger` in den `PhotoPicker` Konstruktor eingefügt.

> [!NOTE]
> Die- [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) Methode muss beim Auflösen eines Typs aus dem Abhängigkeits Injektions Container der Anwendung über das verwendet werden [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

## <a name="related-links"></a>Verwandte Links

- [Abhängigkeitsauflösung mithilfe von Containern (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)
- [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Implementieren eines Videoplayers](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Aufrufen von Ereignissen aus Effekten](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Auswählen eines Fotos aus der Bildbibliothek](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
