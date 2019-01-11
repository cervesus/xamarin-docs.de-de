---
title: Plattformeigenschaften
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie zu nutzen, und Erstellen von Plattformeigenschaften wird.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 44d0cf3a257c00b448a6c70064af2f8e3ba63f69
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207790"
---
# <a name="platform-specifics"></a>Plattformeigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

_Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte._

Der Prozess für die Nutzung einer plattformspezifischen über XAML oder über die fluent API-Code lautet wie folgt aus:

1. Hinzufügen einer `xmlns` Deklaration oder `using` -Direktive für den [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) Namespace.
1. Hinzufügen einer `xmlns` Deklaration oder `using` -Direktive für den Namespace, der die plattformspezifischen Funktionen enthält:
    1. Unter iOS, dies ist die [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace.
    1. Unter Android, dies ist die [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace. Für Android AppCompat, ist dies die [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) Namespace.
    1. Für die universelle Windows-Plattform, dies ist die [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace.
1. Wenden Sie die plattformspezifischen von XAML oder von Code mit der `On<T>` fluent-API. Der Wert des `T` kann die [ `iOS` ](xref:Xamarin.Forms.PlatformConfiguration.iOS), [ `Android` ](xref:Xamarin.Forms.PlatformConfiguration.Android), oder [ `Windows` ](xref:Xamarin.Forms.PlatformConfiguration.Windows) Typen aus den [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) Namespace.

> [!NOTE]
> Beachten Sie, dass es wird versucht, eine plattformspezifische auf einer Plattform nutzen, in denen es nicht verfügbar ist, nicht zu einem Fehler führt. Stattdessen führt der Code ohne die plattformspezifischen angewendet wird.

Plattformeigenschaften genutzt, über die `On<T>` flusscode API-Rückgabe [ `IPlatformElementConfiguration` ](xref:Xamarin.Forms.IPlatformElementConfiguration`2) Objekte. Dadurch können mehrere Plattformeigenschaften für dasselbe Objekt mit kaskadierendem Methode aufgerufen werden.

Weitere Informationen zu den Besonderheiten der Plattform – von Xamarin.Forms bereitgestelltes, finden Sie unter [iOS Plattformeigenschaften](~/xamarin-forms/platform/ios/index.md), [Android Plattformeigenschaften](~/xamarin-forms/platform/android/index.md), und [Windows Plattformeigenschaften](~/xamarin-forms/platform/windows/index.md).

## <a name="creating-platform-specifics"></a>Erstellen von Plattformeigenschaften

Anbieter können ihre eigenen Plattformeigenschaften mit Effekte erstellen. Ein Effekt bietet es sich um die spezifische Funktionalität, die dann über eine plattformspezifische verfügbar gemacht wird. Das Ergebnis ist ein Effekt, der leichter über XAML oder über eine fluent API-Code genutzt werden kann.

Der Prozess zum Erstellen einer plattformspezifischen lautet wie folgt aus:

1. Implementieren Sie die spezifische Funktionalität als einen Effekt. Weitere Informationen finden Sie unter [erstellen einen Effekt](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Erstellen Sie eine plattformspezifische-Klasse, die den Effekt verfügbar macht. Weitere Informationen finden Sie unter [Erstellen einer plattformspezifischen Klasse](#creating).
1. Implementieren Sie in der plattformspezifischen-Klasse einer angefügten Eigenschaft, damit die plattformspezifischen über XAML verwendet werden kann. Weitere Informationen finden Sie unter [Hinzufügen einer angefügten Eigenschaft](#attached_property).
1. Implementieren Sie in der Klasse plattformspezifische Erweiterungsmethoden bereit, um die plattformspezifischen über eine fluent API-Code genutzt werden können. Weitere Informationen finden Sie unter [Erweiterungsmethoden hinzufügen](#extension_methods).
1. Ändern Sie die Implementierung der Effekt, sodass die Auswirkungen nur angewendet, wenn die plattformspezifischen auf derselben Plattform wie der Effekt aufgerufen wurde. Weitere Informationen finden Sie unter [erstellen die Auswirkungen](#creating_the_effect).

Das Ergebnis ein Effekts als eine plattformspezifische verfügbar zu machen ist, dass die Auswirkung einfacher über die XAML und über eine fluent API-Code genutzt werden kann.

> [!NOTE]
> Ist vorgesehen, dass Anbieter diese Technik zum Erstellen von eigenen Plattformeigenschaften, zur Vereinfachung der Nutzung von Benutzern verwenden. Während der Benutzer auswählen können, um ihre eigenen Plattformeigenschaften zu erstellen, darauf hinzuweisen, dass sie mehr Code als das Erstellen und nutzen einen Effekt erfordert.

Die [beispielanwendung](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/) veranschaulicht eine `Shadow` plattformspezifische, die den Text durch einen Schatten hinzufügt eine [ `Label` ](xref:Xamarin.Forms.Label) Steuerelement:

![](images/screenshots.png "Clientplattform-spezifische Shadowing")

Die [beispielanwendung](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/) implementiert die `Shadow` plattformspezifische auf jeder Plattform zum einfacheren Verständnis. Abgesehen von der jede Clientplattform-spezifische Auswirkungen Implementierung ist jedoch die Implementierung der Volumeschattenkopie-Klasse für jede Plattform weitgehend identisch. Dieses Handbuch behandelt daher die Implementierung der Volumeschattenkopie-Klasse und der zugehörigen Auswirkungen auf eine einzelne Plattform.

Weitere Informationen zu den Auswirkungen, finden Sie unter [Customizing Controls mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md).

### <a name="creating-a-platform-specific-class"></a>Erstellen einer plattformspezifischen-Klasse

Eine plattformspezifische wird als erstellt eine `public static` Klasse:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

Den folgenden Abschnitten werden die Implementierung der `Shadow` plattformspezifischen und zugehörigen wirksam.

#### <a name="adding-an-attached-property"></a>Hinzufügen einer angefügten Eigenschaft

Eine angefügte Eigenschaft muss hinzugefügt werden, um die `Shadow` plattformspezifische zum Zulassen der Nutzung durch XAML:

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

Die `IsShadowed` angefügte Eigenschaft wird verwendet, um das Hinzufügen der `MyCompany.LabelShadowEffect` Auswirkungen, und entfernt sie aus, das Steuerelement, das `Shadow` Klasse, die angefügt wird. Diese angefügte Eigenschaft registriert die `OnIsShadowedPropertyChanged` -Methode, die ausgeführt werden, wenn der Wert der Eigenschaft geändert wird. Diese Methode wiederum ruft die `AttachEffect` oder `DetachEffect` Methode zum Hinzufügen oder entfernen die Auswirkung basierend auf den Wert der `IsShadowed` angefügte Eigenschaft. Der Effekt wird hinzugefügt oder entfernt Sie aus dem Steuerelement, durch Ändern des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung.

> [!NOTE]
> Beachten Sie, dass die Auswirkungen aufgelöst wird, durch Angabe eines Werts, das besteht aus einer Verkettung der Auflösung von Namen und eindeutiger Bezeichner, der für die Implementierung der Effekt angegeben wird. Weitere Informationen finden Sie unter [erstellen einen Effekt](~/xamarin-forms/app-fundamentals/effects/creating.md).

Weitere Informationen zu angefügten Eigenschaften finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

#### <a name="adding-extension-methods"></a>Hinzufügen von Erweiterungsmethoden

Erweiterungsmethoden müssen hinzugefügt werden, um die `Shadow` plattformspezifische zum Zulassen der Nutzung über eine fluent API-Code:

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

Die `IsShadowed` und `SetIsShadowed` Erweiterungsmethoden Aufrufen der Get und set-Accessoren für die `IsShadowed` angefügten Eigenschaft, der bzw. Jede Erweiterungsmethode verarbeitet die `IPlatformElementConfiguration<iOS, FormsElement>` Typ, der angibt, dass die plattformspezifischen für aufgerufen werden kann [ `Label` ](xref:Xamarin.Forms.Label) Instanzen IOS.

#### <a name="creating-the-effect"></a>Erstellen die Auswirkungen

Die `Shadow` plattformspezifische fügt die `MyCompany.LabelShadowEffect` auf eine [ `Label` ](xref:Xamarin.Forms.Label), und entfernt sie. Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das iOS-Projekt:

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

Der `UpdateShadow` Methode legt `Control.Layer` Eigenschaften zum Erstellen des Schattens bereitgestellt, die die `IsShadowed` angefügte Eigenschaft auf festgelegt ist `true`, und vorausgesetzt, dass die `Shadow` plattformspezifische auf derselben Plattform aufgerufen wurde, die Effekt wird für implementiert. Diese Überprüfung wird ausgeführt, mit der `OnThisPlatform` Methode.

Wenn die `Shadow.IsShadowed` angefügt Änderungen von Eigenschaftswerten zur Laufzeit muss die Auswirkungen durch das Entfernen des Schattens zu reagieren. Aus diesem Grund einer überschriebenen Version der dem `OnElementPropertyChanged` Methode dient zum Reagieren auf die bindbare Eigenschaft-Änderung durch Aufrufen der `UpdateShadow` Methode.

Weitere Informationen zum Erstellen eines Effekts finden Sie unter [erstellen einen Effekt](~/xamarin-forms/app-fundamentals/effects/creating.md) und [Auswirkung-Parameter übergeben, als angefügte Eigenschaften](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### <a name="consuming-the-platform-specific"></a>Nutzen die plattformspezifischen

Die `Shadow` plattformspezifische ist in XAML verwendet, durch Festlegen der `Shadow.IsShadowed` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [ShadowPlatformSpecific (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [iOS Plattformeigenschaften](~/xamarin-forms/platform/ios/index.md)
- [Android Plattformeigenschaften](~/xamarin-forms/platform/android/index.md)
- [Windows-Plattform-Besonderheiten](~/xamarin-forms/platform/windows/index.md)
- [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
- [PlatformConfiguration API](xref:Xamarin.Forms.PlatformConfiguration)
