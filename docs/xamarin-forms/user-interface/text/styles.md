---
title: Stile
description: Textstil in Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 764fb77bedf9e00427348e95b4ecf029ae94741f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="styles"></a>Stile

_Textstil in Xamarin.Forms_


Stile können verwendet werden, um die Darstellung der Bezeichnungen, Einträge und Editoren anzupassen. Formatvorlagen einmal definiert und durch zahlreiche Ansichten verwendet werden können, aber ein Format kann nur mit Ansichten eines bestimmten Typs verwendet werden.
Stile können weitergegeben werden, eine `Key` und selektiv mithilfe eines bestimmten Steuerelements angewendet `Style` Eigenschaft.

In diesem Artikel werden die folgenden Themen behandelt:

- **[Formatvorlagen](#Built-In_Styles)**  &ndash; Formatvorlagen Stil textbasierte Ansichten in der gesamten Ihrer app verwenden.
- **[Benutzerdefinierte Stile](#Custom_Styles)**  &ndash; benutzerdefinierte Stile definieren, wenn die integrierten Optionen ist nicht ausreichend.
- **[Anwenden von Stilen](#Applying_Styles)**  &ndash; benutzerdefinierte und integrierte Stile übernehmen, um Ihre Ansichten.
- **[Eingabehilfen](#Accessibility)**  &ndash; stellen Sie sicher, dass Text barrierefreiheiteinstellungen berücksichtigt.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Formatvorlagen

Xamarin.Forms umfassen mehrere [integrierte](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) Stile für häufige Szenarien:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Um einen der integrierten Stile anzuwenden, verwenden die `DynamicResource` Markuperweiterung das Format an:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden integrierte Formate aus ausgewählt `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Stile des Gerätebeispiel")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Benutzerdefinierte Stile

Stile der Setter bestehen und Setter von Eigenschaften bestehen und die Werte der Eigenschaften festgelegt, um.

Ein benutzerdefinierter Stil für eine Bezeichnung mit rotem Text Größe 30 würde in c# wie folgt definiert werden:

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

Beachten Sie, dass die Ressourcen (einschließlich aller Stile) definierten `ContentPage.Resources`, also ein gleichgeordnetes Element eines je vertrauter `ContentPage.Content` Element.

![](styles-images/customstyle.png "Beispiel für benutzerdefinierte Stile")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Anwenden von Stilen

Nachdem Sie ein Stil erstellt wurde, kann es auf alle Vergleichen von Sichten angewendet werden seine `TargetType`.

Benutzerdefinierte Stile gelten in XAML werden für Sichten durch Angabe ihrer `Style` Eigenschaft mit einer `StaticResource` Markuperweiterung verweisen auf die gewünschte Formatvorlage:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

In c#, Formatvorlagen können entweder direkt auf einer Sicht angewendet oder hinzugefügt und abgerufen werden von einer Seite `ResourceDictionary`. So fügen Sie direkt hinzu:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Hinzufügen und Abrufen von der Seite `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Formatvorlagen sind unterschiedlich, angewendet werden, da sie so reagieren Sie auf die Einstellungen für die Barrierefreiheit benötigen. Anwenden von Formatvorlagen in XAML wird die `DynamicResource` Markuperweiterung verwendet wird:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

In c# werden integrierte Formate aus ausgewählt `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Zugriff

Die integrierten Stile vorhanden sein, um Eingabehilfen Voreinstellungen respektieren erleichtern. Wenn Sie keines der integrierten Formate zu verwenden, werden Schriftgrade Ihren Bedürfnissen automatisch herauf, wenn Benutzer ihre Voreinstellungen Eingabehilfen entsprechend festlegen.

Betrachten Sie das folgende Beispiel der gleichen Seite Sichten erstellt, in denen die integrierten Stile mit barrierefreiheiteinstellungen aktiviert und deaktiviert:

Deaktiviert:

![](styles-images/pre-access.png "Gerät Stile mit Eingabehilfen deaktiviert")

Aktiviert:

![](styles-images/post-access.png "Gerät Stile Eingabehilfen aktiviert")

Um Zugriff zu gewährleisten, sicher, dass die integrierten Stile als Grundlage für alle Formatvorlagen bezogenen Text innerhalb Ihrer app verwendet werden und Sie Stile einheitlich verwenden. Finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md) Weitere Einzelheiten zu erweitern und Arbeiten mit Formatvorlagen im Allgemeinen.


## <a name="related-links"></a>Verwandte Links

- [Beim Erstellen mobiler Apps mit Xamarin.Forms, Kapitel 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [Text (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Style](http://developer.xamarin.comhttps://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
