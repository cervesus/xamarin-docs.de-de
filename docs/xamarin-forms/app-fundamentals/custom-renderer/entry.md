---
title: Anpassen eines Eintrags
description: Das Steuerelement zur Eingabe von Xamarin.Forms können eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das Steuerelement Eintrag ermöglicht Entwicklern das native Standardrendering mit ihren eigenen plattformspezifische Anpassungen überschrieben wird.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
ms.openlocfilehash: 7fea736b0a04a69fd64100ae1d6bcd42c244359f
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459849"
---
# <a name="customizing-an-entry"></a>Anpassen eines Eintrags

_Das Steuerelement zur Eingabe von Xamarin.Forms können eine einzelne Textzeile bearbeitet werden. In diesem Artikel wird veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das Steuerelement Eintrag ermöglicht Entwicklern das native Standardrendering mit ihren eigenen plattformspezifische Anpassungen überschrieben wird._

Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn ein [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelements von einer Xamarin.Forms-Anwendung unter iOS die `EntryRenderer` Klasse instanziiert wird, das wiederum ein systemeigenes instanziiert `UITextField` Steuerelement. Auf der Android-Plattform die `EntryRenderer` Klasse instanziiert ein `EditText` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `EntryRenderer` Klasse instanziiert ein `TextBox` Steuerelement. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement und den entsprechenden systemeigenen Steuerelementen, die sie implementieren:

![](entry-images/entry-classes.png "Beziehung zwischen Eintrag und systemeigene Steuerelemente implementieren")

Das Rendern zu kann nutzen erstellt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für das [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Entry_Control) eines benutzerdefinierten Xamarin.Forms-Steuerelements.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement mit einer anderen Hintergrundfarbe auf jeder Plattform.

> [!IMPORTANT]
> In diesem Artikel wird erläutert, wie Sie einen einfachen benutzerdefinierten Renderer erstellen. Es ist jedoch nicht erforderlich, erstellen einen benutzerdefinierten Renderer zum Implementieren einer `Entry` , die eine andere Hintergrundfarbe auf jeder Plattform verfügt. Dies kann leichter mit erreicht werden die [ `Device` ](xref:Xamarin.Forms.Device) -Klasse, oder die `OnPlatform` Markuperweiterung Clientplattform-spezifische Werte bereitstellen. Weitere Informationen finden Sie unter [Clientplattform-spezifische Werte bereitstellen](~/xamarin-forms/platform/device.md#providing-platform-specific-values) und [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Erstellen des benutzerdefinierten Eintrag-Steuerelements

Eine benutzerdefinierte [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement kann erstellt werden, indem Unterklassen der `Entry` zu steuern, wie im folgenden Codebeispiel gezeigt:

```csharp
public class MyEntry : Entry
{
}
```

Die `MyEntry` Steuerelement wird in .NET Standard Library-Projekt erstellt und ist einfach ein [ `Entry` ](xref:Xamarin.Forms.Entry) Steuerelement. Anpassung des Steuerelements durchgeführt werden in den benutzerdefinierten Renderer, sodass keine zusätzliche Implementierung erforderlich ist der `MyEntry` Steuerelement.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `MyEntry` Steuerelement kann verwiesen werden in XAML in .NET Standard Library-Projekt durch deklarieren einen Namespace für den Speicherort und verwenden das Namespacepräfix für das Steuerelement, das. Das folgende Codebeispiel zeigt die `MyEntry` Steuerelement kann von einer XAML-Seite verwendet werden:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` -Werte müssen die Details des benutzerdefinierten Steuerelements entsprechen. Sobald der Namespace deklariert ist, dass das Präfix verwendet wird, um auf das benutzerdefinierte Steuerelement zu verweisen.

Das folgende Codebeispiel zeigt die `MyEntry` Steuerelement durch einen C# -Code genutzt werden kann:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Dieser Code instanziiert ein neues [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) -Objekt, das angezeigt wird, wird eine [ `Label` ](xref:Xamarin.Forms.Label) und `MyEntry` Steuerelement sowohl vertikal und horizontal auf der Seite zentriert.

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zum Anpassen der Darstellung des Steuerelements auf jeder Plattform hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `EntryRenderer` -Klasse, die das native Steuerelement rendert.
1. Überschreiben der `OnElementChanged` -Methode, die die native Steuerelement und schreiben die Logik zum Anpassen des Steuerelements gerendert wird. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des Xamarin.Forms-Steuerelements verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Dies ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](entry-images/solution-structure.png "\"Myentry\" benutzerdefinierte Renderer Project-Aufgaben")

Die `MyEntry` -Steuerelement gerendert wird, indem Sie plattformspezifische `MyEntryRenderer` , alle abgeleiteten Klassen der `EntryRenderer` -Klasse für jede Plattform. Dies führt in jeder `MyEntry` Steuerelements mit einer plattformspezifischen Hintergrundfarbe gerendert wird, wie in den folgenden Screenshots dargestellt:

![](entry-images/screenshots.png "\"Myentry\"-Steuerelement auf jeder Plattform")

Die `EntryRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn das Xamarin.Forms-Steuerelement erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In diesem Beispiel die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `MyEntry` Steuerelement.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode in der die `MyEntryRenderer` Klasse ist der Ort, um die Anpassung von systemeigenen Steuerelementen auszuführen. Ein typisierter Verweis auf das native Steuerelement verwendet wird, auf der Plattform möglich über die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` -Eigenschaft, obwohl sie nicht in der beispielanwendung verwendet wird.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: den Typnamen des Xamarin.Forms-Steuerelements gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Die folgenden Abschnitte beschreiben die Implementierung der einzelnen plattformspezifischen `MyEntryRenderer` benutzerdefinierten Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert ein iOS `UITextField` Steuerelement, mit einem Verweis auf das Steuerelement zugewiesen wird, für des Renderers des `Control` Eigenschaft. Wird die Farbe des Hintergrunds für die helle Violett mit festgelegt die `UIColor.FromRGB` Methode.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert eine Android `EditText` Steuerelement, mit einem Verweis auf das Steuerelement zugewiesen wird, für des Renderers des `Control` Eigenschaft. Wird die Farbe des Hintergrunds für die hellgrün mit festgelegt die `Control.SetBackgroundColor` Methode.

### <a name="creating-the-custom-renderer-on-uwp"></a>Erstellen benutzerdefinierten Renderer auf UWP

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für UWP:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Der Aufruf der Basisklasse `OnElementChanged` Methode instanziiert ein `TextBox` Steuerelement mit einem Verweis auf das Steuerelement, das an des Renderers, der zugewiesen werden `Control` Eigenschaft. Die Hintergrundfarbe wird festgelegt auf Cyanblau durch das Erstellen einer `SolidColorBrush` Instanz.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie erstellen Sie einen benutzerdefiniertes Steuerelement-Renderer für die Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) -Steuerelement ermöglicht es Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifischen Rendering außer Kraft zu setzen. Benutzerdefinierte Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung von Xamarin.Forms-Steuerelementen. Sie können für kleine Formatierungsänderungen oder komplexe plattformspezifische Layout und verhaltensanpassung verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererEntry (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
