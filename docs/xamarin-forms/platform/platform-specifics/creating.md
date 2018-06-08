---
title: Erstellen der Plattform-Besonderheiten
description: Anbieter können ihre eigenen Plattform-Besonderheiten mit Effekte erstellen. Ein Effekt bietet die spezifische Funktionalität, die dann über eine plattformspezifische verfügbar gemacht wird. Das Ergebnis ist ein Effekt, der leichter über XAML und über die fluent-API-Code genutzt werden kann. In diesem Artikel veranschaulicht, wie einen Effekt über einen plattformspezifischen verfügbar zu machen.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: dcd22dd0d4e281245a1f6598d9a58d24a97b1f20
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848147"
---
# <a name="creating-platform-specifics"></a>Erstellen der Plattform-Besonderheiten

_Anbieter können ihre eigenen Plattform-Besonderheiten mit Effekte erstellen. Ein Effekt bietet die spezifische Funktionalität, die dann über eine plattformspezifische verfügbar gemacht wird. Das Ergebnis ist ein Effekt, der leichter über XAML und über die fluent-API-Code genutzt werden kann. In diesem Artikel veranschaulicht, wie einen Effekt über einen plattformspezifischen verfügbar zu machen._

## <a name="overview"></a>Übersicht

Der Erstellungsprozess für eine plattformspezifische lautet wie folgt:

1. Implementieren Sie die spezifische Funktionalität als Effekt. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Erstellen Sie eine plattformspezifische-Klasse, die den Effekt verfügbar machen soll. Weitere Informationen finden Sie unter [Erstellen einer Klasse plattformspezifischen](#creating).
1. Implementieren Sie in der Klasse plattformspezifischen einer angefügten Eigenschaft, damit die plattformspezifischen durch XAML genutzt werden können. Weitere Informationen finden Sie unter [Hinzufügen einer angefügten Eigenschaft](#attached_property).
1. Implementieren Sie in der Klasse plattformspezifischen Erweiterungsmethoden, damit die Clientplattform-spezifische, durch eine fluent-Code-API genutzt werden können. Weitere Informationen finden Sie unter [Hinzufügen von Erweiterungsmethoden](#extension_methods).
1. Ändern Sie die Auswirkungen-Implementierung, damit die Auswirkungen nur angewendet, wenn für dieselbe Plattform als die Auswirkung der plattformspezifischen aufgerufen wurde. Weitere Informationen finden Sie unter [den Effekt](#creating_the_effect).

Das Ergebnis ein Effekts als eine plattformspezifische verfügbar machen, ist, dass die Auswirkungen leichter über XAML und einer fluent-Code-API genutzt werden kann.

> [!NOTE]
> Ist vorgesehen, dass der Anbieter diese Technik zum Erstellen von eigenen Plattform-Besonderheiten zur Vereinfachung der Nutzung von Benutzern verwenden. Während Benutzer ihre eigenen Plattform-Besonderheiten erstellen wählen können, sollte darauf hingewiesen werden, dass sie mehr Code als erstellen und Nutzen eines Effekts erfordert.

Die beispielanwendung für veranschaulicht eine `Shadow` plattformspezifischen, die einen Schatten vom angezeigte Text hinzufügt eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelement:

![](creating-images/screenshots.png "Clientplattform-spezifische Shadowing")

Die Anwendung implementiert die `Shadow` plattformspezifischen auf jeder Plattform zur Vereinfachung der verstehen. Abgesehen von der jede Clientplattform-spezifische Auswirkungen Implementierung ist die Implementierung der Schattenkopie-Klasse jedoch weitgehend für jede Plattform. Dieses Handbuch behandelt daher die Implementierung der Schattenkopie-Klasse und der zugehörigen Auswirkungen auf eine einzelne Plattform.

Weitere Informationen zu Auswirkungen finden Sie unter [Customizing Controls mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Erstellen einer plattformspezifischen-Klasse

Ein plattformspezifisches wird als erstellt eine `public static` Klasse:

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

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Hinzufügen einer angefügten Eigenschaft

Eine angefügte Eigenschaft muss hinzugefügt werden, um die `Shadow` plattformspezifischen zum Zulassen der Nutzung durch XAML:

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

Die `IsShadowed` angefügte Eigenschaft wird verwendet, um das Hinzufügen der `MyCompany.LabelShadowEffect` Auswirkungen, und entfernen Sie sie aus dem Steuerelement, das `Shadow` Klasse angefügt ist. Dies angefügte Eigenschaft registriert die `OnIsShadowedPropertyChanged` Methode, die ausgeführt wird, wenn der Wert der Eigenschaft ändert. Wiederum diese Methode ruft die `AttachEffect` oder `DetachEffect` Methode zum Hinzufügen oder entfernen die Auswirkungen basierend auf den Wert der `IsShadowed` -Eigenschaft. Der Effekt hinzugefügt oder entfernt Sie aus dem Steuerelement durch Ändern des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung.

> [!NOTE]
> Beachten Sie, dass die Auswirkungen behoben ist, durch die Angabe eines Werts, das besteht aus einer Verkettung der Gruppenname Auflösung und eindeutiger Bezeichner, der für die Implementierung Auswirkungen angegeben wird. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).

Weitere Informationen über angefügte Eigenschaften finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Hinzufügen von Erweiterungsmethoden [c#]

Erweiterungsmethoden [c#] müssen hinzugefügt werden, um die `Shadow` plattformspezifischen zum Zulassen der Nutzung über die fluent-API-Code:

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

Die `IsShadowed` und `SetIsShadowed` Erweiterungsmethoden Aufrufen der Get und set-Accessoren für die `IsShadowed` angefügten Eigenschaft, der bzw. Jede Erweiterungsmethode arbeitet, auf die `IPlatformElementConfiguration<iOS, FormsElement>` Typ, der angibt, dass die plattformspezifischen aufgerufen werden kann, auf [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen IOS.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Erstellen die Auswirkung

Die `Shadow` plattformspezifischen fügt die `MyCompany.LabelShadowEffect` auf eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), und entfernt wird. Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das iOS-Projekt:

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

Die `UpdateShadow` -Methode legt `Control.Layer` Eigenschaften zum Erstellen des Schattens bereitgestellt, die die `IsShadowed` auf gesetzte angehängte Eigenschaft `true`, und vorausgesetzt, dass die `Shadow` plattformspezifischen für dieselbe Plattform aufgerufen wurde, die Effekt wird für implementiert. Diese Überprüfung wird ausgeführt, mit der `OnThisPlatform` Methode.

Wenn die `Shadow.IsShadowed` angefügt eigenschaftswertänderungen zur Laufzeit, muss der Auswirkungen durch das Entfernen des Schattens zu reagieren. Aus diesem Grund eine überschriebene Version der der `OnElementPropertyChanged` Methode wird verwendet, so reagieren Sie auf die Änderung bindbare Eigenschaft durch Aufrufen der `UpdateShadow` Methode.

Weitere Informationen zum Erstellen eines Effekts finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md) und [Effekt-Parameter übergeben, als angefügte Eigenschaften](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Nutzen eine plattformspezifische

Die `Shadow` plattformspezifischen belegt wurde in XAML durch Festlegen der `Shadow.IsShadowed` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Weitere Informationen zu Plattform-Besonderheiten nutzen, finden Sie unter [Plattform-Besonderheiten nutzen](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie einen Effekt über einen plattformspezifischen verfügbar zu machen. Das Ergebnis ist ein Effekt, der leichter über XAML und über die fluent-API-Code genutzt werden kann.


## <a name="related-links"></a>Verwandte Links

- [ShadowPlatformSpecific (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
