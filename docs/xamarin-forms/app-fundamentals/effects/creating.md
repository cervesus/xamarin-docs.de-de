---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0ed11ccacec73dd3c5c220c41bffd090af6c5b27
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139385"
---
# <a name="creating-an-effect"></a>Erstellen eines Effekts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)

_Durch Effekte können Steuerelemente mühelos angepasst werden. In diesem Artikel wird veranschaulicht, wie Sie einen Effekt erstellen können, der die Hintergrundfarbe des Entry-Steuerelements verändert, wenn das Steuerelement ausgewählt wird._

Der Prozess zum Erstellen eines Effekts in jedem plattformspezifischen Projekt umfasst folgende Schritte:

1. Erstellen Sie eine Unterklasse der `PlatformEffect`-Klasse.
1. Überschreiben Sie die `OnAttached`-Methode, und schreiben Sie Logik, um das Steuerelement anzupassen.
1. Überschreiben Sie die `OnDetached`-Methode, und schreiben Sie Logik, um die Anpassung des Steuerelements zu bereinigen, falls erforderlich.
1. Fügen Sie der Effect-Klasse ein [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute)-Attribut hinzu. Dieses Attribut legt einen unternehmensweiten Namespace für Effekte fest und verhindert dadurch Konflikte mit anderen Effekten mit dem gleichen Namen. Beachten Sie, dass dieses Attribut nur einmal pro Projekt angewendet werden kann.
1. Fügen Sie der Effect-Klasse ein [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute)-Attribut hinzu. Dieses Attribut registriert den Effekt mit einer eindeutigen ID, die von Xamarin.Forms verwendet wird, und den Gruppennamen, damit nach dem Effekt gesucht wird, bevor er auf ein Steuerelement angewendet wird. Das Attribut akzeptiert zwei Parameter: den Typnamen des Effekts und eine eindeutige Zeichenfolge, die für die Suche nach dem Effekt vor dessen Anwendung auf ein Steuerelement verwendet wird.

Der Effekt kann dann verwendet werden, indem er an das entsprechende Steuerelement angefügt wird.

> [!NOTE]
> Ein Effekt kann optional in jedem Plattformprojekt bereitgestellt werden. Beim Versuch, einen Effekt bei fehlender Registrierung zu verwenden, wird ein Wert ungleich NULL zurückgegeben, der keine Auswirkungen hat.

Die Beispielanwendung zeigt eine `FocusEffect`-Klasse, die die Hintergrundfarbe eines Steuerelements ändert, wenn sie ausgewählt wird. Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](creating-images/focus-effect.png "Focus Effect Project Responsibilities")

Ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement im `HomePage`-Element wird in jedem plattformspezifischen Projekt der `FocusEffect`-Klasse angepasst. Jede `FocusEffect`-Klasse wird von der `PlatformEffect`-Klasse für jede Plattform abgeleitet. Dies führt dazu, dass das `Entry`-Steuerelement mit einer plattformspezifischen Hintergrundfarbe gerendert wird, die sich ändert, wenn das Steuerelement ausgewählt wird. Dies wird in folgenden Screenshots veranschaulicht:

![](creating-images/screenshots-1.png "Focus Effect on each Platform")
![](creating-images/screenshots-2.png "Focus Effect on each Platform")

## <a name="creating-the-effect-on-each-platform"></a>Erstellen von Effect-Klassen auf verschiedenen Plattformen

In den folgenden Abschnitten wird die plattformspezifische Implementierung der `FocusEffect`-Klasse erläutert.

## <a name="ios-project"></a>iOS-Projekt

Im folgenden Codebeispiel wird die Implementierung von `FocusEffect` für das iOS-Projekt veranschaulicht:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(EffectsDemo.iOS.FocusEffect), nameof(EffectsDemo.iOS.FocusEffect))]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Die `OnAttached`-Methode legt die `BackgroundColor`-Eigenschaft des Steuerelements mithilfe der `UIColor.FromRGB`-Methode auf „hellviolett“ fest und speichert diese Farbe zusätzlich in einem Feld. Diese Funktion ist in einem `try`/`catch`-Block umschlossen, falls das an den Effekt angefügte Steuerelement über keine `BackgroundColor`-Eigenschaft verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

Die `OnElementPropertyChanged`-Überschreibung reagiert auf Änderungen bindbarer Eigenschaften des Xamarin.Forms-Steuerelements. Wenn sich die [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)-Eigenschaft ändert, wird die `BackgroundColor`-Eigenschaft des Steuerelements in „weiß“ geändert, falls das Steuerelement ausgewählt ist. Andernfalls ändert sich die Eigenschaft in „hellviolett“. Diese Funktion ist in einem `try`/`catch`-Block umschlossen, falls das an den Effekt angefügte Steuerelement über keine `BackgroundColor`-Eigenschaft verfügt.

## <a name="android-project"></a>Android-Projekt

Im folgenden Codebeispiel wird die Implementierung von `FocusEffect` für das Android-Projekt veranschaulicht:

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.Droid.FocusEffect), nameof(EffectsDemo.Droid.FocusEffect))]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color originalBackgroundColor = new Android.Graphics.Color(0, 0, 0, 0);
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached()
        {
            try
            {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor(backgroundColor);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);
            try
            {
                if (args.PropertyName == "IsFocused")
                {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor)
                    {
                        Control.SetBackgroundColor(originalBackgroundColor);
                    }
                    else
                    {
                        Control.SetBackgroundColor(backgroundColor);
                    }
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

Die `OnAttached`-Methode ruft die `SetBackgroundColor`-Methode auf, um die Hintergrundfarbe des Steuerelements auf „hellgrün“ festzulegen, und speichert diese Farbe zusätzlich in einem Feld. Diese Funktion ist in einem `try`/`catch`-Block umschlossen, falls das an den Effekt angefügte Steuerelement über keine `SetBackgroundColor`-Eigenschaft verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

Die `OnElementPropertyChanged`-Überschreibung reagiert auf Änderungen bindbarer Eigenschaften des Xamarin.Forms-Steuerelements. Wenn sich die [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)-Eigenschaft ändert, wird die Hintergrundfarbe des Steuerelements in „weiß“ geändert, falls das Steuerelement ausgewählt ist. Andernfalls ändert sich die Hintergrundfarbe in „hellgrün“. Diese Funktion ist in einem `try`/`catch`-Block umschlossen, falls das an den Effekt angefügte Steuerelement über keine `BackgroundColor`-Eigenschaft verfügt.

## <a name="universal-windows-platform-projects"></a>UWP-Projekte (Universelle Windows-Plattform)

Das folgende Codebeispiel zeigt die Implementierung von `FocusEffect` für UWP-Projekte (Universelle Windows-Plattform):

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(EffectsDemo.UWP.FocusEffect), nameof(EffectsDemo.UWP.FocusEffect))]
namespace EffectsDemo.UWP
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

Die `OnAttached`-Methode legt die `Background`-Eigenschaft des Steuerelements auf „zyanblau“ und die `BackgroundFocusBrush`-Eigenschaft auf „weiß“ fest. Diese Funktion ist in einem `try`/`catch`-Block umschlossen, falls das an den Effekt angefügte Steuerelement nicht über diese Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

## <a name="consuming-the-effect"></a>Nutzen des Effekts

Die Verarbeitung eines Effekts aus einer Xamarin.Forms-.NET Standard-Bibliothek oder einem freigegebenen Bibliotheksprojekt umfasst folgende Schritte:

1. Deklarieren Sie ein Steuerelement, das durch den Effekt angepasst wird.
1. Fügen Sie den Effekt an das Steuerelement an, indem Sie ihn zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Collection des Steuerelements hinzufügen.

> [!NOTE]
> Eine Effektinstanz kann nur an ein einziges Steuerelement angefügt werden. Aus diesem Grund muss ein Effekt zweimal aufgelöst werden, damit er für zwei Steuerelemente verwendet werden kann.

## <a name="consuming-the-effect-in-xaml"></a>Nutzen des Effekts in XAML

Das folgende XAML-Codebeispiel zeigt ein [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement, an das `FocusEffect` angefügt ist:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

Die `FocusEffect`-Klasse in der .NET Standard-Bibliothek unterstützt die Effektnutzung in XAML und wird im folgenden Beispiel veranschaulicht:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ($"MyCompany.{nameof(FocusEffect)}")
    {
    }
}
```

Die `FocusEffect`-Klasse erstellt Unterklassen der [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)-Klasse, die einen plattformunabhängigen Effekt darstellt, der einen in der Regel plattformspezifischen inneren Effekt umschließt. Die `FocusEffect`-Klasse ruft den Basisklassenkonstruktor auf, der einen Parameter bestehend aus einer Verkettung des Namens der Auflösungsgruppe (mithilfe des [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute)-Attributs der Effect-Klasse angegeben) übergibt, und die eindeutige ID, die mithilfe des [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute)-Attributs der Effect-Klasse angegeben wurde. Aus diesem Grund wird bei der Initialisierung von [`Entry`](xref:Xamarin.Forms.Entry) zur Laufzeit eine neue Instanz von `MyCompany.FocusEffect` zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Collection des Steuerelements hinzugefügt.

Effekte können auch mithilfe eines Verhaltens oder angefügter Eigenschaften an Steuerelemente angefügt werden. Weitere Informationen zum Anfügen eines Effekts an ein Steuerelement mithilfe eines Verhaltens finden Sie unter [Reusable EffectBehavior (Wiederverwendbares Effektverhalten)](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Weitere Informationen zum Anfügen eines Effekts an ein Steuerelement mithilfe angefügter Eigenschaften finden Sie unter [Passing Parameters to an Effect (Übergeben von Parametern an einen Effekt)](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Nutzen des Effekts in C&num;

Das entsprechende [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` wird an die `Entry`-Instanz angefügt, indem der Effekt wie im folgenden Codebeispiel dargestellt zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Collection des Steuerelements hinzugefügt wird:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ($"MyCompany.{nameof(FocusEffect)}"));
  ...
}
```

[`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String)) gibt für den angegebenen Namen einen [`Effect`](xref:Xamarin.Forms.Effect), eine Verkettung des Namens der Auflösungsgruppe (mithilfe des [`ResolutionGroupName`](xref:Xamarin.Forms.ResolutionGroupNameAttribute)-Attributs der Effect-Klasse angegeben), und die eindeutige ID zurück, die mithilfe des [`ExportEffect`](xref:Xamarin.Forms.ExportEffectAttribute)-Attributs der Effect-Klasse angegeben wurde. Wenn eine Plattform den Effekt nicht bereitstellt, gibt die `Effect.Resolve`-Methode einen Wert ungleich `null` zurück.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie einen Effekt erstellen können, der die Hintergrundfarbe des [`Entry`](xref:Xamarin.Forms.Entry)-Steuerelements verändert, wenn das Steuerelement ausgewählt wird.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect class (Effect-Klasse)](xref:Xamarin.Forms.Effect)
- [PlatformEffect (PlatformEffect-Klasse)](xref:Xamarin.Forms.PlatformEffect`2)
- [Background Color Effect (Hintergrundfarbeneffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-backgroundcoloreffect)
- [Focus Effect (Fokuseffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-focuseffect)
