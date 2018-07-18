---
title: Erstellen einen Effekt
description: Auswirkungen die Anpassung eines Steuerelements zu vereinfachen. In diesem Artikel wird veranschaulicht, wie Sie einen Effekt zu erstellen, der die Hintergrundfarbe des Steuerelements Eintrag ändert, wenn das Steuerelement den Fokus erhält.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: b29d83999724a35293882f7b9efc0158171c4fd2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998159"
---
# <a name="creating-an-effect"></a>Erstellen einen Effekt

_Auswirkungen die Anpassung eines Steuerelements zu vereinfachen. In diesem Artikel wird veranschaulicht, wie Sie einen Effekt zu erstellen, der die Hintergrundfarbe des Steuerelements Eintrag ändert, wenn das Steuerelement den Fokus erhält._

Der Prozess zum Erstellen eines Effekts in den einzelnen plattformspezifischen Projekten lautet wie folgt aus:

1. Erstellen Sie eine Unterklasse von der `PlatformEffect` Klasse.
1. Überschreiben der `OnAttached` -Methode und Schreiben von Logik zum Anpassen des Steuerelements.
1. Überschreiben der `OnDetached` -Methode und Schreiben Logik zum Bereinigen die Anpassung von Steuerelementen, falls erforderlich.
1. Hinzufügen einer [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) -Attribut der Klasse wirksam. Dieses Attribut legt einen große Unternehmen-Namespace für Effekte, verhindern von Konflikten mit anderen Effekten mit dem gleichen Namen fest. Beachten Sie, dass dieses Attribut kann nur einmal pro Projekt angewendet werden.
1. Hinzufügen einer [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) -Attribut der Klasse wirksam. Dieses Attribut registriert den Effekt mit einer eindeutigen ID, die xamarin.Forms, zusammen mit den Gruppennamen, verwendet wird, um die Auswirkungen vor dem Übernehmen sie für ein Steuerelement zu suchen. Das-Attribut nimmt zwei Parameter: den Typnamen des Effekts, und eine eindeutige Zeichenfolge, die verwendet wird, um die Auswirkungen vor dem Übernehmen sie für ein Steuerelement zu suchen.

Die Auswirkungen kann dann verwendet werden, indem Sie ihn an das entsprechende Steuerelement anfügen.

> [!NOTE]
> Dies ist optional, ein Effekts in jedes plattformprojekt bereitstellen. Versucht, einen Effekt zu verwenden, wenn eine nicht registriert ist, gibt einen Wert ungleich Null zurück, der keine Aktionen ausführt.

Die beispielanwendung zeigt eine `FocusEffect` ändert die Hintergrundfarbe eines Steuerelements, wenn es sich um den Fokus erhält. Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](creating-images/focus-effect.png "Fokus Auswirkungen Project-Aufgaben")

Ein [ `Entry` ](xref:Xamarin.Forms.Entry) control für die `HomePage` angepasst wird, indem die `FocusEffect` Klasse in den einzelnen plattformspezifischen Projekten. Jede `FocusEffect` Klasse leitet sich von der `PlatformEffect` -Klasse für jede Plattform. Dadurch wird die `Entry` steuern, die mit einer plattformspezifischen Hintergrundfarbe, ändert das Steuerelement den Fokus erhält, wie in den folgenden Screenshots gezeigt gerendert wird:

![](creating-images/screenshots-1.png "Konzentrieren Sie sich die Auswirkungen auf jeder Plattform")
![](creating-images/screenshots-2.png "Auswirkung auf jeder Plattform konzentrieren")

## <a name="creating-the-effect-on-each-platform"></a>Erstellen die Auswirkungen auf jeder Plattform

Die folgenden Abschnitte beschreiben die plattformspezifische Implementierung der `FocusEffect` Klasse.

## <a name="ios-project"></a>iOS-Projekts

Das folgende Codebeispiel zeigt die `FocusEffect` Implementierung für das iOS-Projekt:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
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

Die `OnAttached` Methode wird die `BackgroundColor` Eigenschaft des Steuerelements, das helle Violett, mit der `UIColor.FromRGB` -Methode, und auch diese Farbe in einem Feld gespeichert. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine `BackgroundColor` Eigenschaft. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

Die `OnElementPropertyChanged` außer Kraft setzen reagiert auf Änderungen der bindbare Eigenschaften auf der Xamarin.Forms-Steuerelements. Wenn die [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) eigenschaftsänderungen, die `BackgroundColor` -Eigenschaft des Steuerelements in Weiß geändert wird, wenn das Steuerelement den Fokus besitzt, andernfalls wird es in hell Violett geändert. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine `BackgroundColor` Eigenschaft.

## <a name="android-project"></a>Android-Projekt

Das folgende Codebeispiel zeigt die `FocusEffect` Implementierung für das Android-Projekt:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

Die `OnAttached` Methodenaufrufe der `SetBackgroundColor` Methode, um die Hintergrundfarbe des Steuerelements, das light festzulegen Grün, und speichert auch diese Farbe in einem Feld. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine `SetBackgroundColor` Eigenschaft. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

Die `OnElementPropertyChanged` außer Kraft setzen reagiert auf Änderungen der bindbare Eigenschaften auf der Xamarin.Forms-Steuerelements. Wenn die [ `IsFocused` ](xref:Xamarin.Forms.VisualElement.IsFocused) -Eigenschaft, ändert die Hintergrundfarbe des Steuerelements wird auf Weiß geändert, wenn das Steuerelement den Fokus besitzt, andernfalls wird es in Hellgrün geändert. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine `BackgroundColor` Eigenschaft.

## <a name="universal-windows-platform-projects"></a>Universelle Windows Plattform-Projekten

Das folgende Codebeispiel zeigt die `FocusEffect` Implementierung für Projekte der universellen Windows-Plattform (UWP):

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
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

Die `OnAttached` Methode legt die `Background` Eigenschaft des Steuerelements auf Cyanblau und legt die `BackgroundFocusBrush` Eigenschaft weiß. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist, diese Eigenschaften fehlen. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkungen

Der Prozess für die Nutzung von eines Effekts aus einer Xamarin.Forms .NET Standard-Bibliothek oder eines Projekts für freigegebene Bibliothek lautet wie folgt aus:

1. Deklarieren Sie ein Steuerelement, das durch den Effekt angepasst wird.
1. Fügen Sie die Auswirkungen auf das Steuerelement durch Hinzufügen des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung.

> [!NOTE]
> Eine Auswirkung-Instanz kann nur an ein einzelnes Steuerelement angefügt werden. Aus diesem Grund muss ein Effekt aufgelöst werden, zweimal, um es auf zwei Steuerelementen verwenden.

## <a name="consuming-the-effect-in-xaml"></a>Nutzen die Auswirkung in XAML

Das folgende XAML-Code-Beispiel zeigt eine [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement, auf das die `FocusEffect` angefügt ist:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

Die `FocusEffect` Klasse in der .NET Standardbibliothek Effekt, Nutzung in XAML unterstützt und ist im folgenden Codebeispiel gezeigt:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

Die `FocusEffect` Klasse Unterklassen der [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) -Klasse, die einen plattformunabhängigen Effekt darstellt, die einen inneren Effekt einschließt, die in der Regel plattformspezifisch ist. Die `FocusEffect` Klasse ruft der Konstruktor der Basisklasse übergeben eines Parameters, bestehend aus einer Verkettung der Namen der Lösung (angegeben, mit der [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) -Attribut in der Klasse Auswirkungen), und die eindeutige ID, wurde angegeben, mit der [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) -Attribut in der Klasse wirksam. Aus diesem Grund, wenn die [ `Entry` ](xref:Xamarin.Forms.Entry) wird zur Laufzeit eine neue Instanz der initialisiert die `MyCompany.FocusEffect` des Steuerelements hinzugefügt [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung.

Effekte können mithilfe eines Verhaltens auch an Steuerelemente angefügt werden, oder mithilfe von angefügten Eigenschaften. Weitere Informationen zum Anfügen eines Effekts auf ein Steuerelement mithilfe eines Verhaltens finden Sie unter [Wiederverwendbaren EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Weitere Informationen zum Anfügen eines Effekts auf ein Steuerelement mithilfe von angefügten Eigenschaften finden Sie unter [übergeben von Parametern in einen Effekt](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Nutzen die Auswirkung in C&num;

Die entsprechende [ `Entry` ](xref:Xamarin.Forms.Entry) in c# wird im folgenden Codebeispiel dargestellt:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

Die `FocusEffect` angefügt ist die `Entry` Instanz, indem Sie die Auswirkungen des Steuerelements hinzufügen [ `Effects` ](xref:Xamarin.Forms.Element.Effects) -Auflistung, wie im folgenden Codebeispiel gezeigt:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

Die [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) gibt ein [ `Effect` ](xref:Xamarin.Forms.Effect) für den angegebenen Namen, die besteht aus einer Verkettung der Namen der Lösung (angegeben, mit der [ `ResolutionGroupName` ](xref:Xamarin.Forms.ResolutionGroupNameAttribute) Attribut der Auswirkungen-Klasse), und die eindeutige ID, die angegeben wurde, mit der [ `ExportEffect` ](xref:Xamarin.Forms.ExportEffectAttribute) -Attribut in der Klasse wirksam. Wenn eine Plattform für den Effekt, bieten nicht die `Effect.Resolve` Methode gibt einen nicht-`null` Wert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie Sie einen Effekt zu erstellen, die die Farbe des Hintergrunds Ändern der [ `Entry` ](xref:Xamarin.Forms.Entry) steuern, wenn das Steuerelement den Fokus erhält.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [Background-Farbe Auswirkungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Fokus Auswirkungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
