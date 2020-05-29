---
title: Geräte Stile inXamarin.Forms
description: Xamarin.Formsenthält sechs dynamische Stile, die als Geräte Stile bezeichnet werden, in der Device. Styles-Klasse. In diesem Artikel wird erläutert, wie die Geräte Stile in einer-Anwendung verwendet werden Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b835847fea39e1c2f968e7b81fb9d22f68ea461c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140100"
---
# <a name="device-styles-in-xamarinforms"></a>Geräte Stile inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin. Forms enthält sechs dynamische Stile, die als Geräte Stile bezeichnet werden, in der Device. Styles-Klasse._

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

![](device-images/device-styles.png "Device Styles on Each Platform")

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

[![](device-images/minimum-size.png "Accessible Small Device Styles on Each Platform")](device-images/minimum-size-large.png#lightbox "Accessible Small Device Styles on Each Platform")

Die folgenden Screenshots veranschaulichen die Geräte Stile auf jeder Plattform mit dem größten zugänglichen Schrift Grad:

![](device-images/maximum-size.png "Accessible Large Device Styles on Each Platform")

## <a name="related-links"></a>Verwandte Links

- [Textstile](~/xamarin-forms/user-interface/text/styles.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Device. Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)
