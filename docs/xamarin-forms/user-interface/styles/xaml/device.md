---
title: Gerätestile in Xamarin.Forms
description: Xamarin.Forms umfasst sechs dynamische Stile, die als gerätestile, in der Klasse Device.Styles bezeichnet. In diesem Artikel wird erläutert, wie die gerätestile in einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 252f3271c7247f7070df66712035938be651e7f4
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924794"
---
# <a name="device-styles-in-xamarinforms"></a>Gerätestile in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)

_Xamarin.Forms umfasst sechs dynamische Stile, die als gerätestile, in der Klasse Device.Styles bezeichnet._

Die *Gerät* Stile sind:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

Alle sechs Formatvorlagen können nur angewendet werden, um [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Z. B. eine `Label` anzeigt, die den Text eines Absatzes festlegen kann seine [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaft [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle).

Das folgende Codebeispiel veranschaulicht die Verwendung der *Gerät* Stilen in einer XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" Icon="xaml.png">
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

Die gerätestile gebunden sind, mit der `DynamicResource` Markuperweiterung. Der dynamischen Natur der Formate finden Sie in iOS durch Ändern der **Barrierefreiheit** Einstellungen für die Textgröße. Die Darstellung der *Gerät* Stile auf jeder Plattform anders ist, wie in den folgenden Screenshots gezeigt:

![](device-images/device-styles.png "Gerätestile auf jeder Plattform")

*Gerät* Stile können auch von abgeleitet werden, durch Festlegen der [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) Eigenschaft, um den Namen des Schlüssels für das Gerät-Format. Im obigen Codebeispiel `myBodyStyle` erbt [ `BodyStyle` ](xref:Xamarin.Forms.Device.Styles.BodyStyle) und legt ein Zeichen mit Akzent Textfarbe fest. Weitere Informationen zu dynamischen stilvererbung, finden Sie unter [dynamische Stilvererbung](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance).

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende Seite in C# geschrieben wird:

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
        Icon = "csharp.png";
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

Die [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaft der einzelnen [ `Label` ](xref:Xamarin.Forms.Label) Instanz wird festgelegt, auf die entsprechende Eigenschaft aus der [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles) Klasse.

## <a name="accessibility"></a>Zugriff

Die *Gerät* Stile berücksichtigen Einstellungen für die Barrierefreiheit, damit die Einstellungen für Barrierefreiheit geändert werden, auf jeder Plattform Schriftgrade Ihren Bedürfnissen entsprechend ändern werden. Aus diesem Grund zur Unterstützung der schriftrichtung von zugegriffen werden kann, stellen sicher, dass die *Gerät* Stile werden als Grundlage für alle Formatvorlagen für Text in Ihrer Anwendung verwendet.

Die folgenden Screenshots veranschaulichen die gerätestile auf jeder Plattform mit dem kleinsten Schriftgrad für zugegriffen werden kann:

[![](device-images/minimum-size.png "Zugegriffen werden kann, kleine Gerätestile auf jeder Plattform")](device-images/minimum-size-large.png#lightbox "zugegriffen werden kann, kleine Gerätestile auf jeder Plattform")

Die folgenden Screenshots veranschaulichen die gerätestile auf jeder Plattform mit den größten Schriftgrad aus zugegriffen werden kann:

![](device-images/maximum-size.png "Zugegriffen werden kann, große Gerätestile auf jeder Plattform")

## <a name="related-links"></a>Verwandte Links

- [Textformate](~/xamarin-forms/user-interface/text/styles.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Arbeiten mit Stilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
