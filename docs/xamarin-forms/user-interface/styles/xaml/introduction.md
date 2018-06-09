---
title: Einführung in Xamarin.Forms-Stile
description: Stile ermöglichen die Darstellung visueller Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten die Werte für die Eigenschaften, die für diesen Typ verfügbar sind.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 071cfa2ba145775c179bc85dce4fac29ba0bd8fa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245819"
---
# <a name="introduction-to-xamarinforms-styles"></a>Einführung in Xamarin.Forms-Stile

_Stile ermöglichen die Darstellung visueller Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten die Werte für die Eigenschaften, die für diesen Typ verfügbar sind._

Xamarin.Forms-Anwendungen enthalten häufig mehrere Steuerelemente, die eine identische Darstellung haben. Eine Anwendung kann z. B. mehrere müssen [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen mit dem gleichen Schriftartoptionen und Layoutoptionen, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Im folgenden Codebeispiel wird gezeigt, die entsprechende Seite, die in c# erstellt wird:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Jede [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanz verfügt über identische Eigenschaftswerte für das Steuern der Darstellung des Texts angezeigt wird, indem die `Label`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](introduction-images/no-styles.png "Bezeichnung Darstellung ohne Stile")](introduction-images/no-styles-large.png#lightbox "bezeichnen Darstellung ohne Stile")

Die Darstellung der einzelnen Steuerelemente festlegen kann sich wiederholende und fehleranfällig. Stattdessen kann ein Stil erstellt werden, die definiert die Darstellung, und klicken Sie dann auf die erforderlichen Steuerelemente angewendet.

## <a name="creating-a-style"></a>Erstellen eines Stils

Die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Klasse gruppiert eine Auflistung von Eigenschaftswerten in ein Objekt, das Sie dann auf mehreren Instanzen von visuelles Element angewendet werden kann. Dies hilft, um sich wiederholende Markup zu reduzieren und ermöglicht eine Darstellung Anwendungen leichter geändert werden.

Obwohl Stile in erster Linie für XAML-basierte Anwendungen entwickelt wurden, können sie auch in c# erstellt werden:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) in XAML erstellte Instanzen werden in der Regel definiert, einer [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) zugewiesen ist, die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung eines Steuerelements, Seite oder auf die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) Auflistung der Anwendung.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) in c# erstellte Instanzen werden in der Regel definiert, in der Page-Klasse oder in einer Klasse, die Global zugegriffen werden kann.

Auswählen des Installationsorts für das Definieren einer [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) wirkt sich darauf aus, wo sie verwendet werden kann:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen, die auf der Steuerelementebene definiert können nur auf das Steuerelement und seine untergeordneten Elemente angewendet werden.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf Seitenebene definierte Instanzen können nur auf der Seite und zu seinen untergeordneten Elementen angewendet werden.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf der Anwendungsebene definierte Instanzen können in der gesamten Anwendung angewendet werden.

Jede [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz enthält eine Auflistung einer oder mehrerer [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) Objekten, die jeweils `Setter` mit einer [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) und ein [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). Die `Property` ist der Name der bindbare Eigenschaft des Elements, das Format angewendet wird, und die `Value` ist der Wert, der für die Eigenschaft angewendet wird.

Jede [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz kann *explizite*, oder *implizite*:

- Ein *explizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz definiert, indem eine [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) und ein `x:Key` -Wert, und durch Festlegen des Zielelements [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft, um die `x:Key` Verweis. Weitere Informationen zu *explizite* Formatvorlagen, finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md).
- Ein *implizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz wird definiert, indem nur eine [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). Die `Style` Instanz dann automatisch angewendet werden, um alle Elemente dieses Typs. Hinweis Diese Unterklasse von der `TargetType` automatisch keine der `Style` angewendet. Weitere Informationen zu *implizite* Formatvorlagen, finden Sie unter [impliziten Stilen](~/xamarin-forms/user-interface/styles/implicit.md).

Beim Erstellen einer [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaft ist immer erforderlich. Das folgende Codebeispiel zeigt eine *explizite* Stil (Beachten Sie die `x:Key`) in XAML erstellt:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Anwenden einer `Style`, das Zielobjekt muss eine [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) , entspricht der [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaftswert, der der `Style`, wie im folgenden Beispiel der Verwendung von XAML-Code dargestellt:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Stile in der Ansichtshierarchie haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) festlegt [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) auf `Red` Anwendungs-Ebene überschrieben werden wird, indem eine Seite Formatvorlage, der festlegt `Label.TextColor` auf `Green`. Auf ähnliche Weise wird eine Seite Formatvorlage der Formatvorlage für ein Steuerelement überschrieben werden. Darüber hinaus, wenn `Label.TextColor` direkt auf einer Steuerelementeigenschaft, hat dies Vorrang vor alle Formatvorlagen festgelegt ist.

In die Artikeln in diesem Abschnitt wird veranschaulicht, und führen Sie zum Erstellen und Anwenden von *explizite* und *implizite* Stile, wie globale Stile erstellen, formatieren die Vererbung, reagieren auf Änderungen an Formatvorlagen zur Laufzeit und wie die integrierte Formaten in Xamarin.Forms enthalten.

> [!NOTE]
> **Was ist StyleId?**
>
> Als Xamarin.Forms 2.2 die [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) Eigenschaft zum Identifizieren der einzelnen Elemente in einer Anwendung für die Identifikation, die in der UI-Tests und Design-Datenbankmodule, z. B. Pixate verwendet wurde. Xamarin.Forms 2.2 wurde eingeführt, jedoch die [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) -Eigenschaft, die abgelöst wurde die [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) Eigenschaft. Weitere Informationen finden Sie unter [Xamarin.Forms automatisieren testen Xamarin.UITest und Test Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms-Anwendungen enthalten häufig mehrere Steuerelemente, die eine identische Darstellung haben. Die Darstellung der einzelnen Steuerelemente festlegen kann sich wiederholende und fehleranfällig. Stattdessen können Stile erstellt werden, die Darstellung anpassen Gruppierung und legt die Eigenschaften für den Steuerelementtyp "verfügbar.


## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Stil](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter-Methode](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
