---
title: Einen Eintrag anpassen
description: Das Steuerelement Xamarin.Forms Eintrag ermöglicht eine einzelne Textzeile bearbeitet werden. Dieser Artikel veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das Steuerelement Eintrag und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 1e8ef47ceb381a0e4e163aaa24795d46264195da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-an-entry"></a>Einen Eintrag anpassen

_Das Steuerelement Xamarin.Forms Eintrag ermöglicht eine einzelne Textzeile bearbeitet werden. Dieser Artikel veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das Steuerelement Eintrag und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben._

Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) einer Xamarin.Forms-Anwendung in iOS-Steuerelement gerendert wird die `EntryRenderer` Klasse instanziiert, das wiederum ein systemeigenes instanziiert `UITextField` Steuerelement. Auf der Android-Plattform die `EntryRenderer` Klasse instanziiert ein `EditText` Steuerelement. Auf Windows Phone und die universelle Windows-Plattform (UWP) die `EntryRenderer` Klasse instanziiert einen `TextBox` Steuerelement. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement und den entsprechenden systemeigenen Steuerelementen, die ihn implementieren:

![](entry-images/entry-classes.png "Beziehung zwischen Eingabe und Implementieren von systemeigenen Steuerelementen")

Des Renderingprozesses kann Vorteil ausgeführt werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für das [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Entry_Control) ein benutzerdefiniertes Steuerelement mit Xamarin.Forms.
1. [Nutzen](#Consuming_the_Custom_Control) das benutzerdefinierte Steuerelement aus Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für das Steuerelement auf jeder Plattform.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement, das eine andere Hintergrundfarbe für jede Plattform verfügt.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Erstellen des benutzerdefinierten Eintrags-Steuerelements

Eine benutzerdefinierte [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement erstellt werden, indem die Erstellung von Unterklassen von der `Entry` zu steuern, wie im folgenden Codebeispiel gezeigt:

```csharp
public class MyEntry : Entry
{
}
```

Die `MyEntry` Steuerelement im Projekt portablen Klassenbibliothek (PCL) erstellt und wird einfach ein [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Steuerelement. Anpassung des Steuerelements durchgeführt werden in den benutzerdefinierten Renderer, sodass keine weitere Implementierungsdetails in erforderlich ist der `MyEntry` Steuerelement.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Nutzen das benutzerdefinierte Steuerelement

Die `MyEntry` Steuerelement kann verwiesen werden in XAML in der PCL-Projekt durch deklarieren einen Namespace für den Speicherort der und verwenden das Namespacepräfix für das Steuerelement. Im folgenden Codebeispiel wird veranschaulicht wie die `MyEntry` Steuerelement genutzt werden kann, durch die entsprechende Verwendung von XAML-Seite:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details des benutzerdefinierten Steuerelements übereinstimmen. Sobald der Namespace deklariert ist, dass das Präfix verwendet wird, um auf das benutzerdefinierte Steuerelement zu verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `MyEntry` Steuerelement genutzt werden kann, um eine C#-Seite:

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

Dieser Code instanziiert einen neuen [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) -Objekt, das angezeigt wird, wird eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) und `MyEntry` Steuerelement sowohl vertikal und horizontal auf der Seite zentriert.

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zum Anpassen der Darstellung des Steuerelements auf jeder Plattform hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `EntryRenderer` -Klasse, die das systemeigene Steuerelement rendert.
1. Überschreiben Sie die `OnElementChanged` -Methode, die systemeigene Steuerelement und Schreiben Logik zum Anpassen des Steuerelements rendert. Diese Methode wird aufgerufen, wenn das entsprechende Xamarin.Forms-Steuerelement erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern des Steuerelements Xamarin.Forms verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Ist er optional einen benutzerdefinierten Renderer in jedem plattformprojekt bereitstellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](entry-images/solution-structure.png ""Myentry" benutzerdefinierter Renderer Projekt Zuständigkeiten")

Die `MyEntry` plattformspezifischen Steuerelement gerendert wird `MyEntryRenderer` Klassen, die Ableitung der `EntryRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `MyEntry` Steuerelements mit einer plattformspezifischen Hintergrundfarbe gerendert wird, wie in den folgenden Screenshots dargestellt:

![](entry-images/screenshots.png ""Myentry" Steuerelement auf jeder Plattform")

Die `EntryRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn Sie das Xamarin.Forms-Steuerelement erstellt wird, um das entsprechende systemeigene Steuerelement rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `MyEntry` Steuerelement.

Eine überschriebene Version von den `OnElementChanged` Methode in der `MyEntryRenderer` Klasse bietet die Möglichkeit zum Durchführen der systemeigenen-Steuerelement. Durch ein typisierter Verweis auf das systemeigene Steuerelement verwendet wird, auf der Plattform zugegriffen werden kann die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` -Eigenschaft, obwohl sie nicht in der beispielanwendung verwendet wird.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: den Typnamen des Steuerelements Xamarin.Forms gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

Die folgenden Abschnitte beschreiben die Implementierung der einzelnen plattformspezifischen `MyEntryRenderer` benutzerdefinierter Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

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

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert ein iOS `UITextField` Steuerelement, mit einem Verweis auf das Steuerelement wird an des Renderers zugewiesen `Control` Eigenschaft. Klicken Sie dann Festlegen der Hintergrundfarbe hell Violett mit der `UIColor.FromRGB` Methode.

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

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

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert ein Android `EditText` Steuerelement, mit einem Verweis auf das Steuerelement wird an des Renderers zugewiesen `Control` Eigenschaft. Festlegen der Hintergrundfarbe klicken Sie dann auf hellgrün mit der `Control.SetBackgroundColor` Methode.

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Erstellen von benutzerdefinierten Renderers für Windows Phone und universelle Windows-Plattform

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für Windows Phone-und uwp:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.WinPhone81
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

Der Aufruf der Basisklasse `OnElementChanged` -Methode instanziiert einen `TextBox` Steuerelement, mit einem Verweis auf das Steuerelement wird an des Renderers zugewiesen `Control` Eigenschaft. Die Farbe des Hintergrunds wird dann auf Zyan festgelegt, durch das Erstellen einer `SolidColorBrush` Instanz.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie einen benutzerdefiniertes Steuerelement-Renderer für die Xamarin.Forms erstellen [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) -Steuerelement, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifischen Rendering außer Kraft setzen. Benutzerdefinierte Renderer bieten einen sehr effizienter Ansatz zum Anpassen der Darstellung von Xamarin.Forms-Steuerelementen. Sie können für kleine Styling Änderungen oder anspruchsvolle plattformspezifischen Layout und Verhalten Anpassung verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [CustomRendererEntry (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
