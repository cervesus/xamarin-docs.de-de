---
title: Plattformeigenschaften
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Platt Form Besonderheiten verwendet und erstellt werden.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8478db85bd9904ee6c5cfeab9b2af390e7d3096d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139502"
---
# <a name="platform-specifics"></a>Plattformeigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden._

Der Prozess für die Verwendung einer plattformspezifischen über XAML oder über die fließende Code-API sieht wie folgt aus:

1. Fügen Sie eine `xmlns` Deklaration oder eine `using` Direktive für den [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) Namespace hinzu.
1. Fügen Sie eine `xmlns` Deklaration oder eine `using` Direktive für den Namespace hinzu, der die plattformspezifischen Funktionen enthält:
    1. Unter IOS ist dies der [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace.
    1. Unter Android ist dies der- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace. Für Android AppCompat ist dies der [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) Namespace.
    1. Auf dem universelle Windows-Plattform ist dies der [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace.
1. Wenden Sie das plattformspezifische aus XAML oder Code mit der `On<T>` fließenden API an. Der Wert von `T` kann die [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS) Typen, [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android) oder [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) aus dem- [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) Namespace sein.

> [!NOTE]
> Beachten Sie, dass der Versuch, Eine plattformspezifische Plattform auf einer Plattform zu verwenden, bei der es nicht verfügbar ist, zu einem Fehler führt. Stattdessen wird der Code ausgeführt, ohne dass die plattformspezifische Anwendung angewendet wird.

Platt Form Besonderheiten, die durch die fließende Code-API verwendet werden, `On<T>` geben [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) Objekte zurück. Dadurch können mehrere Platt Form Besonderheiten für das gleiche Objekt mit Methoden kaskadierenden aufgerufen werden.

Weitere Informationen zu den von bereitgestellten Platt Form Besonderheiten Xamarin.Forms finden Sie unter [IOS-Platt Form](~/xamarin-forms/platform/ios/index.md)Besonderheiten, [Android-Platt Form Besonderheiten](~/xamarin-forms/platform/android/index.md)und [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md).

## <a name="creating-platform-specifics"></a>Erstellen von plattformspezifischen Besonderheiten

Anbieter können Ihre eigenen Platt Form Besonderheiten mit Effekten erstellen. Durch einen Effekt werden die spezifischen Funktionen bereitgestellt, die dann über einen plattformspezifischen bereitgestellt werden. Das Ergebnis ist ein Effekt, der leichter durch XAML und durch eine fließende Code-API verwendet werden kann.

Der Prozess zum Erstellen einer plattformspezifischen Vorgehens Folge sieht wie folgt aus:

1. Implementieren Sie die jeweilige Funktionalität als Auswirkung. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Erstellen Sie eine plattformspezifische Klasse, die den Effekt verfügbar macht. Weitere Informationen finden Sie unter [Erstellen einer plattformspezifischen Klasse](#creating-a-platform-specific-class).
1. Implementieren Sie in der plattformspezifischen Klasse eine angefügte-Eigenschaft, damit der plattformspezifische durch XAML verwendet werden kann. Weitere Informationen finden Sie unter [Hinzufügen einer angefügten Eigenschaft](#adding-an-attached-property).
1. Implementieren Sie in der plattformspezifischen Klasse Erweiterungs Methoden, damit das plattformspezifische über eine fließende Code-API verwendet werden kann. Weitere Informationen finden Sie unter [Hinzufügen von Erweiterungs Methoden](#adding-extension-methods).
1. Ändern Sie die Effekt Implementierung so, dass der Effekt nur angewendet wird, wenn der plattformspezifische auf derselben Plattform wie der Effekt aufgerufen wurde. Weitere Informationen finden Sie unter [Erstellen des Effekts](#creating-the-effect).

Das Ergebnis der Bereitstellung eines Effekts als plattformspezifisch ist, dass der Effekt leichter durch XAML und durch eine fließende Code-API genutzt werden kann.

> [!NOTE]
> Es ist geplant, dass die Lieferanten dieses Verfahren verwenden, um Ihre eigenen Platt Form Besonderheiten zu erstellen, um die Benutzerfreundlichkeit zu verbessern. Benutzer können Ihre eigenen Platt Form Besonderheiten erstellen, aber beachten Sie, dass Sie mehr Code als das Erstellen und Nutzen eines Effekts erfordert.

Die [Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) veranschaulicht einen `Shadow` plattformspezifischen, der dem Text, der von einem-Steuerelement angezeigt wird, einen Schatten hinzufügt [`Label`](xref:Xamarin.Forms.Label) :

![](images/screenshots.png "Shadow Platform-Specific")

Die [Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) implementiert die `Shadow` plattformspezifischen plattformspezifischen auf den einzelnen Plattformen, um das Verständnis zu vereinfachen. Abgesehen von den einzelnen plattformspezifischen Effekt Implementierungen ist die Implementierung der Schatten Klasse für jede Plattform jedoch größtenteils identisch. Daher behandelt dieses Handbuch die Implementierung der Schatten Klasse und die damit verbundenen Auswirkungen auf eine einzelne Plattform.

Weitere Informationen zu Effekten finden Sie unter [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md).

### <a name="creating-a-platform-specific-class"></a>Erstellen einer plattformspezifischen Klasse

Eine plattformspezifische wird als Klasse erstellt `public static` :

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

In den folgenden Abschnitten wird die Implementierung der `Shadow` plattformspezifischen und zugehörigen Auswirkungen erörtert.

#### <a name="adding-an-attached-property"></a>Hinzufügen einer angefügten Eigenschaft

Eine angefügte Eigenschaft muss der `Shadow` plattformspezifischen-Eigenschaft hinzugefügt werden, um die Nutzung durch XAML zuzulassen:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

Die `IsShadowed` angefügte-Eigenschaft wird verwendet, um den Effekt dem Steuerelement hinzuzufügen, dem `MyCompany.LabelShadowEffect` die- `Shadow` Klasse angefügt ist, und aus diesem entfernt. Diese angefügte Eigenschaft registriert die `OnIsShadowedPropertyChanged`-Methode, die ausgeführt wird, wenn der Wert der Eigenschaft geändert wird. Diese Methode ruft wiederum die- `AttachEffect` oder- `DetachEffect` Methode auf, um die Auswirkung basierend auf dem Wert der angefügten-Eigenschaft hinzuzufügen oder zu entfernen `IsShadowed` . Der Effekt wird dem Steuerelement hinzugefügt bzw. daraus entfernt, indem die-Auflistung des Steuer Elements geändert wird [`Effects`](xref:Xamarin.Forms.Element.Effects) .

> [!NOTE]
> Beachten Sie, dass der Effekt aufgelöst wird, indem Sie einen Wert angeben, der eine Verkettung des Auflösungs Gruppennamens und des eindeutigen Bezeichners ist, der für die Effekt Implementierung angegeben wird. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).

Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

#### <a name="adding-extension-methods"></a>Hinzufügen von Erweiterungs Methoden

Der plattformspezifischen Erweiterungs Methoden müssen hinzugefügt werden `Shadow` , um die Nutzung durch eine fließende Code-API zu ermöglichen:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

Die `IsShadowed` `SetIsShadowed` Erweiterungs Methoden und rufen den Get-und Set-Accessor für die `IsShadowed` angefügte Eigenschaft auf. Jede Erweiterungsmethode wird auf den- `IPlatformElementConfiguration<iOS, FormsElement>` Typ angewendet, der angibt, dass der plattformspezifische für [`Label`](xref:Xamarin.Forms.Label) Instanzen von IOS aufgerufen werden kann.

#### <a name="creating-the-effect"></a>Erstellen des Effekts

Das `Shadow` plattformspezifische fügt dem `MyCompany.LabelShadowEffect` eine hinzu [`Label`](xref:Xamarin.Forms.Label) und entfernt Sie. Im folgenden Codebeispiel wird die Implementierung von `LabelShadowEffect` für das iOS-Projekt veranschaulicht:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Die- `UpdateShadow` Methode legt die `Control.Layer` Eigenschaften zum Erstellen des Schattens fest, vorausgesetzt, die `IsShadowed` angefügte-Eigenschaft ist auf festgelegt `true` , und es wird vorausgesetzt, dass das `Shadow` plattformspezifische auf derselben Plattform aufgerufen wurde, für die der Effekt implementiert wurde. Diese Überprüfung wird mit der-Methode durchgeführt `OnThisPlatform` .

Wenn `Shadow.IsShadowed` sich der Wert der angefügten Eigenschaft zur Laufzeit ändert, muss der Effekt durch Entfernen des Schattens beantwortet werden. Daher wird eine überschriebene Version der- `OnElementPropertyChanged` Methode verwendet, um auf die bindbare Eigenschafts Änderung zu reagieren, indem die-Methode aufgerufen wird `UpdateShadow` .

Weitere Informationen zum Erstellen eines Effekts finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md) und [übergeben von Effekt Parametern als angefügte Eigenschaften](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### <a name="consuming-the-platform-specific"></a>Verwenden der plattformspezifischen

Die `Shadow` plattformspezifische wird in XAML verwendet, indem die `Shadow.IsShadowed` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Shadowplatformspecific (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [IOS-Platt Form Besonderheiten](~/xamarin-forms/platform/ios/index.md)
- [Android-plattformspezifische Besonderheiten](~/xamarin-forms/platform/android/index.md)
- [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md)
- [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
- [Platformconfiguration-API](xref:Xamarin.Forms.PlatformConfiguration)
