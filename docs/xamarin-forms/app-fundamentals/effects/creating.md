---
title: Erstellen eines Effekts
description: Effekte vereinfachen die Anpassung eines Steuerelements an. In diesem Artikel wird veranschaulicht, wie einen Effekt erstellt, der die Hintergrundfarbe des Steuerelements Eintrag ändert, wenn das Steuerelement den Fokus erhält.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 12b8906dd4562e58dede181e773e4046b8434214
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="creating-an-effect"></a>Erstellen eines Effekts

_Effekte vereinfachen die Anpassung eines Steuerelements an. In diesem Artikel wird veranschaulicht, wie einen Effekt erstellt, der die Hintergrundfarbe des Steuerelements Eintrag ändert, wenn das Steuerelement den Fokus erhält._

Der Prozess zum Erstellen eines Effekts in jedem Projekt plattformspezifischen lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `PlatformEffect` Klasse.
1. Überschreiben Sie die `OnAttached` -Methode und Schreiben Logik zum Anpassen des Steuerelements.
1. Überschreiben Sie die `OnDetached` -Methode und Schreiben Logik für die Anpassung Steuerelement bereinigen, falls erforderlich.
1. Hinzufügen einer [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) -Attribut auf die Auswirkungen-Klasse. Dieses Attribut legt einen Unternehmen wide-Namespace für die Effekte, die verhindert, dass Konflikte mit anderen Effekten mit dem gleichen Namen. Beachten Sie, dass dieses Attribut kann nur einmal pro Projekt angewendet werden.
1. Hinzufügen einer [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) -Attribut auf die Auswirkungen-Klasse. Dieses Attribut registriert die Auswirkung einer eindeutigen ID, die von Xamarin.Forms, zusammen mit den Gruppennamen verwendet wird, um die Auswirkungen vor dem Zuweisen eines Steuerelements zu suchen. Das Attribut nimmt zwei Parameter: den Typnamen des Effekts und eine eindeutige Zeichenfolge, die verwendet wird, um die Auswirkungen vor dem Zuweisen eines Steuerelements zu suchen.

Die Auswirkungen kann dann verwendet werden, indem Sie sie auf das entsprechende Steuerelement anfügen.

> [!NOTE]
> Ist er optional ein Effekts in jede plattformprojekt bereitstellen. Bei dem Versuch, eine Auswirkung zu verwenden, wenn ein solcher registriert ist nicht gibt einen Wert ungleich Null zurück, der keine Aktionen ausführt.

Die beispielanwendung für veranschaulicht eine `FocusEffect` die Farbe des Hintergrunds eines Steuerelements ändert, wenn es den Fokus erhält. Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](creating-images/focus-effect.png "Fokus Auswirkungen Projekt Zuständigkeiten")

Ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) control für die `HomePage` angepasst wird, indem die `FocusEffect` Klasse in jedem Projekt plattformspezifischen. Jede `FocusEffect` Klasse leitet sich von der `PlatformEffect` Klasse für jede Plattform. Dadurch wird die `Entry` steuern, die mit einer plattformspezifischen Hintergrundfarbe, die geändert, wenn das Steuerelement den Fokus erhält, wie in den folgenden Screenshots dargestellt gerendert wird:

![](creating-images/screenshots-1.png "Auswirkungen auf jeder Plattform konzentrieren")
![](creating-images/screenshots-2.png "Effekt auf jeder Plattform konzentrieren")

## <a name="creating-the-effect-on-each-platform"></a>Erstellen die Auswirkungen auf jeder Plattform

Die folgenden Abschnitte beschreiben die Clientplattform-spezifische Implementierung der `FocusEffect` Klasse.

## <a name="ios-project"></a>iOS-Projekt

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

Die `OnAttached` Methode legt die `BackgroundColor` Eigenschaft des Steuerelements hell Violett mit der `UIColor.FromRGB` -Methode, und außerdem in einem Feld speichert diese Farbe. Diese Funktion umschlossen ist ein `try` / `catch` für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine Sperren ein `BackgroundColor` Eigenschaft. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

Die `OnElementPropertyChanged` Außerkraftsetzung antwortet mit bindbare eigenschaftenänderungen auf das Xamarin.Forms-Steuerelement. Wenn die [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) eigenschaftenänderungen, die `BackgroundColor` -Eigenschaft des Steuerelements in Weiß geändert wird, wenn das Steuerelement den Fokus besitzt, andernfalls wird er in Violett hell geändert. Diese Funktion umschlossen ist ein `try` / `catch` für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine Sperren ein `BackgroundColor` Eigenschaft.

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

Die `OnAttached` Methodenaufrufe der `SetBackgroundColor` Methode zum Festlegen der Hintergrundfarbe des Steuerelements hell Grün und speichert auch diese Farbe in einem Feld. Diese Funktion umschlossen ist ein `try` / `catch` für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine Sperren ein `SetBackgroundColor` Eigenschaft. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

Die `OnElementPropertyChanged` Außerkraftsetzung antwortet mit bindbare eigenschaftenänderungen auf das Xamarin.Forms-Steuerelement. Wenn die [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) eigenschaftenänderungen, die Hintergrundfarbe des Steuerelements Weiß geändert wird, wenn das Steuerelement den Fokus besitzt, andernfalls wird es in Grün geändert. Diese Funktion umschlossen ist ein `try` / `catch` für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine Sperren ein `BackgroundColor` Eigenschaft.

## <a name="universal-windows-platform-projects"></a>Universelle Windows-Plattformprojekten

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

Die `OnAttached` Methode legt die `Background` Eigenschaft des Steuerelements Zyan und legt die `BackgroundFocusBrush` Eigenschaft weiß. Diese Funktion umschlossen ist ein `try` / `catch` für den Fall, dass das Steuerelement, das der Effekt an diese Eigenschaften nicht blockieren. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkung

Der Prozess für die Nutzung eines Effekts aus einer Xamarin.Forms .NET Standardbibliothek oder gemeinsam genutzte Bibliothek Projekt lautet wie folgt:

1. Deklarieren Sie ein Steuerelement, das den Effekt angepasst werden.
1. Fügen Sie die Auswirkungen auf das Steuerelement, indem Sie des Steuerelements hinzugefügt [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung.

> [!NOTE]
> Eine Auswirkung-Instanz kann nur ein einzelnes Steuerelement zugeordnet werden. Aus diesem Grund muss ein Effekt zweimal, um es auf zwei Steuerelementen verwenden aufgelöst werden.

## <a name="consuming-the-effect-in-xaml"></a>Nutzen den Effekt in XAML

Das folgende Beispiel zeigt für die Verwendung von XAML-Code ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement, das `FocusEffect` angefügt ist:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

Die `FocusEffect` Klasse in der Standardbibliothek .NET unterstützt Sie Verbrauch Effekt in XAML und wird im folgenden Codebeispiel gezeigt:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

Die `FocusEffect` -Klasse Unterklassen der [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse, die einen plattformunabhängigen Effekt darstellt, die einen inneren Effekt umschließt, die in der Regel plattformspezifischen ist. Der `FocusEffect` -Klasse ruft die Basisklasse-Konstruktors und übergibt einen Parameter, bestehend aus einer Verkettung der Gruppenname Auflösung (angegeben, mit der [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) -Attribut für die Auswirkung-Klasse), und die eindeutige ID, wurde angegeben, mit der [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) -Attribut für die Klasse wirksam. Aus diesem Grund, wenn der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wird zur Laufzeit eine neue Instanz der initialisiert die `MyCompany.FocusEffect` an des Steuerelements hinzugefügt wird [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung.

Effekte können auch an Steuerelemente mithilfe eines Verhaltens angefügt werden, oder mithilfe von angefügten Eigenschaften. Weitere Informationen zum Anfügen eines Effekts an ein Steuerelement mithilfe eines Verhaltens finden Sie unter [Wiederverwendbaren EffectBehavior](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Weitere Informationen zum Anfügen eines Effekts an ein Steuerelement mithilfe von angefügten Eigenschaften finden Sie unter [Passing Parameters to eines Effekts](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Nutzen den Effekt in C&num;

Die Entsprechung [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) in c# ist im folgenden Codebeispiel gezeigt:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

Die `FocusEffect` angefügt ist die `Entry` Instanz durch Hinzufügen des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) -Auflistung, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

Die [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) gibt eine [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) für den angegebenen Namen, dem besteht aus einer Verkettung der Gruppenname Auflösung (angegeben, mit der [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) -Attribut für die Klasse wirksam), und die eindeutige ID, die angegeben wurde, mit der [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) -Attribut für die Klasse wirksam. Wenn eine Plattform den Effekt bietet die `Effect.Resolve` Methodenrückgabewert wird nicht`null` Wert.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie einen Effekt erstellt wird, die Farbe des Hintergrunds des geändert, die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) steuern, wenn das Steuerelement den Fokus erhält.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Background-Farbe wirksam (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Fokus wirksam (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
