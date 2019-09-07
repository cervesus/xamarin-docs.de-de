---
title: Xamarin.Forms-Textformate
description: In diesem Artikel wird erläutert, wie Sie Formatieren von Text in Xamarin.Forms-Anwendungen. Stile einmal definiert und von vielen Ansichten verwendet werden können, aber ein Style kann nur mit Ansichten eines bestimmten Typs verwendet werden.
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 66d7ae722281d9034cb4cdf1501662a7de396c2d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770250"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Forms-Textformate

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Formatieren von Text in Xamarin.Forms_

Stile können verwendet werden, um die Darstellung der Bezeichnungen, Einträge und Editoren anzupassen. Stile einmal definiert und von vielen Ansichten verwendet werden können, aber ein Style kann nur mit Ansichten eines bestimmten Typs verwendet werden.
Stile können zugewiesen werden eine `Key` und selektiv mithilfe eines bestimmten Steuerelements angewendet `Style` Eigenschaft.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Integrierte Formate

Xamarin.Forms umfasst mehrere [integrierte](xref:Xamarin.Forms.Device.Styles) Formatvorlagen für gängige Szenarien:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Um eine der integrierten Formatvorlagen anzuwenden, verwenden die `DynamicResource` Markup Extension, um das Format angeben:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden die integrierte Formatvorlagen aus ausgewählt `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![Beispiel für Geräte Stile](styles-images/builtinstyles.png)

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Benutzerdefinierte Stile

Formate von Settern bestehen und Settern bestehen aus Eigenschaften und die Werte der Eigenschaften festgelegt.

In c# würde wie folgt ein benutzerdefinierter Stil für eine Bezeichnung mit rotem Text Größe 30 definiert werden:

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

Beachten Sie, dass die Ressourcen (einschließlich alle Stile) in definierten `ContentPage.Resources`, d.h. ein gleichgeordnetes Element eines je vertrauter `ContentPage.Content` Element.

![Beispiel für benutzerdefinierte Stile](styles-images/customstyle.png)

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Anwenden von Stilen

Nachdem ein Stil erstellt wurde, kann es an alle entsprechenden der Ansicht angewendet werden die `TargetType`.

In XAML, benutzerdefinierte Stile auf Ansichten durch Angabe angewendet werden ihre `Style` Eigenschaft mit einem `StaticResource` Markuperweiterung, die die gewünschte verweisstil:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

In c# Stile können entweder direkt auf eine Ansicht angewendet oder hinzugefügt und aus einer Seite abgerufen `ResourceDictionary`. So fügen Sie direkt hinzu:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Hinzufügen und Abrufen von der Seite `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Integrierte Stile werden unterschiedlich angewendet werden, da sie auf die Einstellungen für die Barrierefreiheit reagieren müssen. Anwenden von integrierten Formatvorlagen in XAML, die `DynamicResource` Markuperweiterung verwendet wird:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden die integrierte Formatvorlagen aus ausgewählt `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Zugriff

Die integrierten Stile vorhanden sein, um die Einstellungen für Barrierefreiheit respektiert vereinfachen. Wenn Sie keines der integrierten Formate zu verwenden, erhöht Schriftgrade Ihren Bedürfnissen entsprechend automatisch, wenn Benutzer ihre Einstellungen für Barrierefreiheit entsprechend festlegen.

Beachten Sie im folgende Beispiel der gleichen Seite Sichten erstellt, in denen die integrierten Formatvorlagen mit Einstellungen für die Barrierefreiheit aktiviert und deaktiviert:

Deaktiviert:

![Geräte Stile mit deaktiviertem Zugriff](styles-images/pre-access.png)

Aktiviert:

![Geräte Stile mit aktiviertem Zugriff](styles-images/post-access.png)

Um Zugriff zu gewährleisten, stellen Sie sicher, dass die integrierte Formatvorlagen, die als Grundlage für alle textbezogenen Formatvorlagen in Ihrer app verwendet werden, und Sie konsistent Formatvorlagen verwenden. Finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md) ausführliche Informationen zum Erweitern von und Arbeiten mit Stilen im Allgemeinen.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von mobilen Apps mit Xamarin.Forms, Kapitel 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [Text (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Stil](xref:Xamarin.Forms.Style)
