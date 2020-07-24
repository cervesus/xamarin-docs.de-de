---
title: Geräte Stile inXamarin.Forms
description: Xamarin.Formsenthält sechs dynamische Stile, die als Geräte Stile bezeichnet werden, in der Device. Styles-Klasse. In diesem Artikel wird erläutert, wie die Geräte Stile in einer-Anwendung verwendet werden Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04669479bb321da4fee6c45fd0f2c00deb5bbf1a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929726"
---
# <a name="device-styles-in-xamarinforms"></a>Geräte Stile inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin.Formsenthält sechs dynamische Stile, die als Geräte Stile bezeichnet werden, in der Device. Styles-Klasse._

Die *Geräte* Stile lauten:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Alle sechs Stile können nur auf Instanzen angewendet werden [`Label`](xref:Xamarin.Forms.Label) . Beispielsweise kann ein `Label` -Objekt, das den Text eines Absatzes anzeigt, seine- [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaft auf festlegen [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) .

Im folgenden Codebeispiel wird die Verwendung der *Geräte* Stile in einer XAML-Seite veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die Geräte Stile werden mithilfe der `DynamicResource` Markup Erweiterung an gebunden. Die dynamische Natur der Stile kann in ios angezeigt werden, indem die **Barrierefreiheits** Einstellungen für die Textgröße geändert werden. Die Darstellung der *Geräte* Stile ist auf den einzelnen Plattformen anders, wie in den folgenden Screenshots zu sehen:

![Geräte Stile auf jeder Plattform](device-images/device-styles.png)

*Geräte* Stile können auch von abgeleitet werden, indem die- [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) Eigenschaft auf den Schlüsselnamen für den Geräte Stil festgelegt wird. Im obigen Codebeispiel `myBodyStyle` erbt von [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) und legt eine Textfarbe mit Akzent fest. Weitere Informationen zur Vererbung dynamischer Stile finden Sie unter [dynamische Stil Vererbung](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Im folgenden Codebeispiel wird die entsprechende Seite in c# veranschaulicht:

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

Die- [`Style`](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaft jeder [`Label`](xref:Xamarin.Forms.Label) Instanz wird auf die entsprechende-Eigenschaft der- [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) Klasse festgelegt.

## <a name="accessibility"></a>Barrierefreiheit

Die *Geräte* Stile berücksichtigen Barrierefreiheits Einstellungen, sodass sich die Schriftart Größen ändern, wenn die Barrierefreiheits Einstellungen auf den einzelnen Plattformen geändert werden. Daher müssen Sie zur Unterstützung von Barrierefreiheits textsicher stellen, dass die *Geräte* Stile als Grundlage für beliebige Text Stile in der Anwendung verwendet werden.

Die folgenden Screenshots veranschaulichen die Geräte Stile auf jeder Plattform mit der kleinsten zugänglichen Schriftart Größe:

[![Barrierefreie kleine Geräte Stile auf jeder Plattform](device-images/minimum-size.png)](device-images/minimum-size-large.png#lightbox "Barrierefreie kleine Geräte Stile auf jeder Plattform")

Die folgenden Screenshots veranschaulichen die Geräte Stile auf jeder Plattform mit dem größten zugänglichen Schrift Grad:

![Barrierefreie große Geräte Stile auf jeder Plattform](device-images/maximum-size.png)

## <a name="related-links"></a>Verwandte Links

- [Textstile](~/xamarin-forms/user-interface/text/styles.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Device. Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [style](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)
