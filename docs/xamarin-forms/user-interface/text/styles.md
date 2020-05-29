---
title: Xamarin.FormsTextstile
description: Dieser Artikel erläutert das Formatieren von Text in- Xamarin.Forms Anwendungen. Stile können einmalig definiert und von vielen Ansichten verwendet werden, aber ein Stil kann nur mit Ansichten eines Typs verwendet werden.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 79a86fd7a2c0f5b82ca4b3e22b3ecedf42c5a0ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136148"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.FormsTextstile

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Formatieren von Text in xamarin. Forms_

Stile können verwendet werden, um die Darstellung von Bezeichnungen, Einträgen und Editoren anzupassen. Stile können einmalig definiert und von vielen Ansichten verwendet werden, aber ein Stil kann nur mit Ansichten eines Typs verwendet werden.
Stile können zugewiesen `Key` und selektiv mithilfe der-Eigenschaft eines bestimmten Steuer Elements angewendet werden `Style` .

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Integrierte Stile

Xamarin.Formsumfasst mehrere [integrierte](xref:Xamarin.Forms.Device.Styles) Stile für gängige Szenarien:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Verwenden Sie zum Anwenden eines der integrierten Stile die `DynamicResource` Markup Erweiterung, um den Stil anzugeben:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden integrierte Stile ausgewählt aus `Device.Styles` :

```csharp
label.Style = Device.Styles.TitleStyle;
```

![Beispiel für Geräte Stile](styles-images/builtinstyles.png)

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Benutzerdefinierte Stile

Stile bestehen aus Settern und Settern und bestehen aus Eigenschaften und den Werten, auf die die Eigenschaften festgelegt werden.

In c# wird ein benutzerdefinierter Stil für eine Bezeichnung mit rotem Text der Größe 30 wie folgt definiert:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

In XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Beachten Sie, dass Ressourcen (einschließlich aller Stile) in definiert sind `ContentPage.Resources` , wobei es sich um ein gleich geordnetes Element des vertraulicheren `ContentPage.Content` Elements handelt.

![Beispiel für benutzerdefinierte Stile](styles-images/customstyle.png)

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Anwenden von Stilen

Nachdem ein Stil erstellt wurde, kann er auf jede beliebige Ansicht angewendet werden, die mit übereinstimmt `TargetType` .

In XAML werden benutzerdefinierte Stile auf Sichten angewendet, indem deren- `Style` Eigenschaft mit einer Markup Erweiterung bereitgestellt wird, `StaticResource` die auf den gewünschten Stil verweist:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

In c# können Stile entweder direkt auf eine Ansicht angewendet oder zu einer Seite hinzugefügt und daraus abgerufen werden `ResourceDictionary` . Zum direkten hinzufügen:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Zum Hinzufügen und Abrufen aus der Seite `ResourceDictionary` :

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Integrierte Stile werden unterschiedlich angewendet, da Sie auf Barrierefreiheits Einstellungen reagieren müssen. Zum Anwenden integrierter Stile in XAML `DynamicResource` wird die Markup Erweiterung verwendet:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden integrierte Stile ausgewählt aus `Device.Styles` :

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Barrierefreiheit

Die integrierten Stile sind vorhanden, damit Barrierefreiheits Einstellungen leichter zu beachten sind. Wenn Sie eines der integrierten Stile verwenden, werden die Schriftgrößen automatisch vergrößert, wenn ein Benutzer seine Barrierefreiheits Einstellungen entsprechend festlegt.

Sehen Sie sich das folgende Beispiel der gleichen Seite von Sichten an, die mit den integrierten Stilen mit aktivierten und deaktivierten Barrierefreiheits Einstellungen formatiert sind:

Deaktiviert:

![Geräte Stile mit deaktiviertem Zugriff](styles-images/pre-access.png)

Aktiviert:

![Geräte Stile mit aktiviertem Zugriff](styles-images/post-access.png)

Stellen Sie sicher, dass integrierte Stile als Grundlage für Text bezogene Stile innerhalb Ihrer APP verwendet werden, und dass Sie Stile konsistent verwenden, um die Barrierefreiheit sicherzustellen. Weitere Informationen zum Erweitern und arbeiten mit Stilen im Allgemeinen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md) .

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Mobile Apps mit Xamarin.Forms , Kapitel 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Stil](xref:Xamarin.Forms.Style)
