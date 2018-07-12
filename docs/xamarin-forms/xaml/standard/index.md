---
title: XAML-Standard (Vorschau)
description: In diesem Artikel wird erläutert, wie Sie die ersten Schritte bei der Untersuchung der Standard-Vorschauversion von XAML in Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38838009"
---
# <a name="xaml-standard-preview"></a>XAML-Standard (Vorschau)

![Vorschau](~/media/shared/preview.png)

Mit dem standardmäßigen XAML in Xamarin.Forms experimentieren möchten, gehen Sie wie folgt vor:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Herunterladen der [Vorschau NuGet-Pakets hier](https://aka.ms/xf-xamlstandard-nuget).
2. Hinzufügen der **Xamarin.Forms.Alias** NuGet-Paket zu Ihren Projekten Xamarin.Forms .NET Standard und Plattform.
3. Initialisieren Sie das Paket mit `Alias.Init()`
4. Hinzufügen einer `xmlns:a` Verweis `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Verwenden von Typen in XAML - finden Sie unter den [Steuerelemente Referenz](controls.md) für Weitere Informationen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Herunterladen der [Vorschau NuGet-Pakets hier](https://aka.ms/xf-xamlstandard-nuget).
2. Hinzufügen der **Xamarin.Forms.Alias** NuGet-Paket zu Ihren Projekten Xamarin.Forms .NET Standard und Plattform.
3. Initialisieren Sie das Paket mit `Alias.Init()`
4. Hinzufügen einer `xmlns:a` Verweis `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Verwenden von Typen in XAML - finden Sie unter den [Steuerelemente Referenz](controls.md) für Weitere Informationen.

-----

Die folgenden XAML veranschaulicht einige XAML-Standard-Steuerelemente in einer Xamarin.Forms verwendet `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Erfordert die Xmlns `a:` Präfix für den XAML-Standardsteuerelementen ist eine Einschränkung des die aktuelle Vorschau.


## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Steuerelementreferenz](controls.md)
