---
title: "Geräte-Stile"
description: "Xamarin.Forms enthält sechs dynamische Formate, bekannt als Gerät Formate, in der Device.Styles-Klasse."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7FF19ED1-0822-4238-9435-AD970317A2F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7e9d8768969affdbaf1172c59eaa5fc1e64460e8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="device-styles"></a>Geräte-Stile

_Xamarin.Forms enthält sechs dynamische Formate, bekannt als Gerät Formate, in der Device.Styles-Klasse._

Die *Gerät* Stile sind:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)

Alle sechs Formatvorlagen können nur angewendet werden, um [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen. Z. B. eine `Label` anzeigt, die den Text eines Absatzes kann seine [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/).

Das folgende Codebeispiel veranschaulicht die Verwendung der *Gerät* Formatvorlagen auf eine XAML-Seite:

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

Die Stile Gerät gebunden sind, mit der `DynamicResource` Markuperweiterung. Der dynamischen Natur der Stile in iOS angezeigt werden kann, durch Ändern der **Eingabehilfen** Einstellungen für die Textgröße. Die Darstellung der *Gerät* Formatvorlagen auf jeder Plattform unterschiedlich ist, wie in den folgenden Screenshots dargestellt:

![](device-images/device-styles.png "Gerät Formatvorlagen auf jeder Plattform")

*Gerät* Formatvorlagen können auch von abgeleitet werden, durch Festlegen der [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) Eigenschaft in den Schlüsselnamen für das Gerät-Format. Im Codebeispiel oben `myBodyStyle` erbt von [ `BodyStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/) und legt ein Zeichen mit Akzent Textfarbe fest. Weitere Informationen zu dynamischen Stil Vererbung, finden Sie unter [Dynamic Style Vererbung](~/xamarin-forms/user-interface/styles/dynamic.md#dynamic-style-inheritance).

Im folgenden Codebeispiel wird veranschaulicht, die entsprechende Seite in c#:

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

Die [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) -Eigenschaft jedes [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanz wird festgelegt, um die entsprechende Eigenschaft aus der [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Klasse.

## <a name="accessibility"></a>Zugriff

Die *Gerät* Stile respektieren Voreinstellungen für die Barrierefreiheit, damit Schriftgrade Ihren Bedürfnissen geändert werden, wie die Einstellungen für die Barrierefreiheit für jede Plattform geändert werden. Zur Unterstützung der schriftrichtung von zugegriffen werden kann, deshalb sicherstellen, dass die *Gerät* Stile dienen als Grundlage für alle Textformatvorlagen innerhalb der Anwendung.

Die folgenden Screenshots zeigen Gerät Formatvorlagen auf jeder Plattform mit den kleinsten Schriftgrad für zugegriffen werden kann:

[![](device-images/minimum-size.png "Zugegriffen werden kann, kleine Gerät Formatvorlagen auf jeder Plattform")](device-images/minimum-size-large.png#lightbox "zugegriffen werden kann, kleine Gerät Formatvorlagen auf jeder Plattform")

Die folgenden Screenshots zeigen Gerät Formatvorlagen auf jeder Plattform mit den größten Schriftgrad für zugegriffen werden kann:

![](device-images/maximum-size.png "Zugegriffen werden kann, große Gerät Formatvorlagen auf jeder Plattform")

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms enthält sechs *dynamische* Formatvorlagen, bekannt als *Gerät* Stile, in der [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Klasse. Alle sechs Formatvorlagen können nur angewendet werden, um [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen.


## <a name="related-links"></a>Verwandte Links

- [Textformate](~/xamarin-forms/user-interface/text/styles.md)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Dynamische Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device.Styles](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
