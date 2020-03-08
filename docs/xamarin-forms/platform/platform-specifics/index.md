---
title: Plattformeigenschaften
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Platt Form Besonderheiten verwendet und erstellt werden.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: f6190b9c0d29d57d6d509bdff25e2ce3572e3a3c
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78910550"
---
# <a name="platform-specifics"></a>Plattformeigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

_Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden._

Der Prozess für die Nutzung einer plattformspezifischen über XAML oder über die fluent API-Code lautet wie folgt aus:

1. Fügen Sie eine `xmlns` Deklaration oder eine `using`-Direktive für den [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) Namespace hinzu.
1. Fügen Sie eine `xmlns` Deklaration oder `using`-Direktive für den Namespace hinzu, der die plattformspezifischen Funktionen enthält:
    1. Unter IOS ist dies der [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace.
    1. Unter Android ist dies der [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace. Für Android AppCompat ist dies der [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) -Namespace.
    1. Auf dem universelle Windows-Plattform ist dies der [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace.
1. Wenden Sie das plattformspezifische aus XAML oder Code mit der `On<T>` fließend-API an. Der Wert von `T` kann die Typen " [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS)", " [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android)" oder " [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) " aus dem [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) -Namespace sein.

> [!NOTE]
> Beachten Sie, dass es wird versucht, eine plattformspezifische auf einer Plattform nutzen, in denen es nicht verfügbar ist, nicht zu einem Fehler führt. Stattdessen führt der Code ohne die plattformspezifischen angewendet wird.

Platt Form Besonderheiten, die durch die `On<T>` Hashcode-API genutzt werden, geben [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) Objekte zurück. Dadurch können mehrere Plattformeigenschaften für dasselbe Objekt mit kaskadierendem Methode aufgerufen werden.

Weitere Informationen zu den Platt Form Besonderheiten, die von xamarin. Forms bereitgestellt werden, finden Sie unter [IOS-Platt Form Besonderheiten](~/xamarin-forms/platform/ios/index.md), [Android-Platt Form Besonderheiten](~/xamarin-forms/platform/android/index.md)und [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md).

## <a name="creating-platform-specifics"></a>Erstellen von plattformspezifischen Besonderheiten

Anbieter können Ihre eigenen Platt Form Besonderheiten mit Effekten erstellen. Ein Effekt bietet es sich um die spezifische Funktionalität, die dann über eine plattformspezifische verfügbar gemacht wird. Das Ergebnis ist ein Effekt, der leichter über XAML oder über eine fluent API-Code genutzt werden kann.

Der Prozess zum Erstellen einer plattformspezifischen lautet wie folgt aus:

1. Implementieren Sie die spezifische Funktionalität als einen Effekt. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Erstellen Sie eine plattformspezifische-Klasse, die den Effekt verfügbar macht. Weitere Informationen finden Sie unter [Erstellen einer plattformspezifischen Klasse](#creating-a-platform-specific-class).
1. Implementieren Sie in der plattformspezifischen-Klasse einer angefügten Eigenschaft, damit die plattformspezifischen über XAML verwendet werden kann. Weitere Informationen finden Sie unter [Hinzufügen einer angefügten Eigenschaft](#adding-an-attached-property).
1. Implementieren Sie in der Klasse plattformspezifische Erweiterungsmethoden bereit, um die plattformspezifischen über eine fluent API-Code genutzt werden können. Weitere Informationen finden Sie unter [Hinzufügen von Erweiterungs Methoden](#adding-extension-methods).
1. Ändern Sie die Implementierung der Effekt, sodass die Auswirkungen nur angewendet, wenn die plattformspezifischen auf derselben Plattform wie der Effekt aufgerufen wurde. Weitere Informationen finden Sie unter [Erstellen des Effekts](#creating-the-effect).

Das Ergebnis ein Effekts als eine plattformspezifische verfügbar zu machen ist, dass die Auswirkung einfacher über die XAML und über eine fluent API-Code genutzt werden kann.

> [!NOTE]
> Ist vorgesehen, dass Anbieter diese Technik zum Erstellen von eigenen Plattformeigenschaften, zur Vereinfachung der Nutzung von Benutzern verwenden. Während der Benutzer auswählen können, um ihre eigenen Plattformeigenschaften zu erstellen, darauf hinzuweisen, dass sie mehr Code als das Erstellen und nutzen einen Effekt erfordert.

Die [Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) veranschaulicht einen `Shadow` plattformspezifischen, der einen Schatten zu dem Text hinzufügt, der von einem [`Label`](xref:Xamarin.Forms.Label) Steuerelement angezeigt wird:

![](images/screenshots.png "Shadow Platform-Specific")

Die [Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific) implementiert die `Shadow` plattformspezifisch auf den einzelnen Plattformen, um das Verständnis zu vereinfachen. Abgesehen von der jede Clientplattform-spezifische Auswirkungen Implementierung ist jedoch die Implementierung der Volumeschattenkopie-Klasse für jede Plattform weitgehend identisch. Dieses Handbuch behandelt daher die Implementierung der Volumeschattenkopie-Klasse und der zugehörigen Auswirkungen auf eine einzelne Plattform.

Weitere Informationen zu Effekten finden Sie unter [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md).

### <a name="creating-a-platform-specific-class"></a>Erstellen einer plattformspezifischen Klasse

Eine plattformspezifische wird als `public static` Klasse erstellt:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

In den folgenden Abschnitten wird die Implementierung des `Shadow` plattformspezifischen und zugehörigen Effekts erläutert.

#### <a name="adding-an-attached-property"></a>Hinzufügen einer angefügten Eigenschaft

Eine angefügte Eigenschaft muss der `Shadow` plattformspezifisch hinzugefügt werden, um die Nutzung durch XAML zuzulassen:

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

Die `IsShadowed` angefügte-Eigenschaft wird verwendet, um den `MyCompany.LabelShadowEffect` Effekt dem Steuerelement hinzuzufügen, dem die `Shadow`-Klasse angefügt ist, und daraus zu entfernen. Diese angefügte Eigenschaft registriert die `OnIsShadowedPropertyChanged`-Methode, die ausgeführt wird, wenn der Wert der Eigenschaft geändert wird. Diese Methode ruft wiederum die `AttachEffect` oder `DetachEffect` Methode auf, um die Auswirkung basierend auf dem Wert der angefügten Eigenschaft `IsShadowed` hinzuzufügen oder zu entfernen. Der Effekt wird dem Steuerelement hinzugefügt bzw. daraus entfernt, indem die [`Effects`](xref:Xamarin.Forms.Element.Effects) Auflistung des Steuer Elements geändert wird.

> [!NOTE]
> Beachten Sie, dass die Auswirkungen aufgelöst wird, durch Angabe eines Werts, das besteht aus einer Verkettung der Auflösung von Namen und eindeutiger Bezeichner, der für die Implementierung der Effekt angegeben wird. Weitere Informationen finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md).

Weitere Informationen zu angefügten Eigenschaften finden Sie unter [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

#### <a name="adding-extension-methods"></a>Hinzufügen von Erweiterungsmethoden

Erweiterungs Methoden müssen der `Shadow` plattformspezifisch hinzugefügt werden, um die Nutzung durch eine fließende Code-API zu ermöglichen:

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

Die Erweiterungs Methoden `IsShadowed` und `SetIsShadowed` rufen die Get-und Set-Accessoren für die `IsShadowed` angefügte Eigenschaft bzw. Jede Erweiterungsmethode wird auf den `IPlatformElementConfiguration<iOS, FormsElement>` Typ angewendet, der angibt, dass der plattformspezifische auf [`Label`](xref:Xamarin.Forms.Label) -Instanzen von IOS aufgerufen werden kann.

#### <a name="creating-the-effect"></a>Erstellen des Effekts

Der `Shadow` plattformspezifisch fügt die `MyCompany.LabelShadowEffect` einer [`Label`](xref:Xamarin.Forms.Label)hinzu und entfernt Sie. Im folgenden Codebeispiel wird die Implementierung von `LabelShadowEffect` für das iOS-Projekt veranschaulicht:

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

Mit der `UpdateShadow`-Methode werden `Control.Layer` Eigenschaften zum Erstellen des Schattens festgelegt, vorausgesetzt, die `IsShadowed` angefügte-Eigenschaft wird auf `true`festgelegt, und es wird vorausgesetzt, dass die `Shadow` plattformspezifisch auf derselben Plattform aufgerufen wurde, für die der Effekt implementiert ist. Diese Überprüfung wird mit der `OnThisPlatform`-Methode durchgeführt.

Wenn sich der `Shadow.IsShadowed` angefügten Eigenschafts Wert zur Laufzeit ändert, muss der Effekt durch Entfernen des Schattens beantwortet werden. Daher wird eine überschriebene Version der `OnElementPropertyChanged`-Methode verwendet, um auf die Änderung der bindbaren Eigenschaft zu reagieren, indem Sie die `UpdateShadow`-Methode aufrufen.

Weitere Informationen zum Erstellen eines Effekts finden Sie unter [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md) und [übergeben von Effekt Parametern als angefügte Eigenschaften](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

### <a name="consuming-the-platform-specific"></a>Verwenden der plattformspezifischen

Der `Shadow` plattformspezifisch wird in XAML verwendet, indem die `Shadow.IsShadowed` angefügte Eigenschaft auf einen `boolean` Wert festgelegt wird:

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

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Shadowplatformspecific (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shadowplatformspecific)
- [IOS-Platt Form Besonderheiten](~/xamarin-forms/platform/ios/index.md)
- [Android-plattformspezifische Besonderheiten](~/xamarin-forms/platform/android/index.md)
- [Windows-Platt Form Besonderheiten](~/xamarin-forms/platform/windows/index.md)
- [Anpassen von Steuerelementen mit Effekten](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md)
- [Platformconfiguration-API](xref:Xamarin.Forms.PlatformConfiguration)
