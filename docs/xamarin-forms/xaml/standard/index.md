---
title: Verwendung von XAML-Standard (Vorschau)
description: Untersuchen die Verwendung von XAML-Standard-Vorschau in Xamarin.Forms erste Schritte
ms.topic: article
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: e53df69fdfd5b5c1fc98b667d4b75d06c16c35dc
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="xaml-standard-preview"></a>Verwendung von XAML-Standard (Vorschau)

![Vorschau](~/media/shared/preview.png)

Führen Sie diese Schritte aus, um die Verwendung von XAML-Standard in Xamarin.Forms experimentieren aus:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Herunterladen der [hier NuGet-Paket in der Vorschau anzeigen](https://aka.ms/xf-xamlstandard-nuget).
2. Hinzufügen der **Xamarin.Forms.Alias** NuGet-Paket zu Projekten Xamarin.Forms PCL, .NET Standard- und Plattform.
3. Initialisieren Sie das Paket mit `Alias.Init()`
4. Hinzufügen einer `xmlns:a` Verweis `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Verwenden Sie die Typen in XAML - finden Sie unter der [steuert Verweis](controls.md) für Weitere Informationen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Herunterladen der [hier NuGet-Paket in der Vorschau anzeigen](https://aka.ms/xf-xamlstandard-nuget).
2. Hinzufügen der **Xamarin.Forms.Alias** NuGet-Paket zu Projekten Xamarin.Forms PCL, .NET Standard- und Plattform.
3. Initialisieren Sie das Paket mit `Alias.Init()`
4. Hinzufügen einer `xmlns:a` Verweis `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Verwenden Sie die Typen in XAML - finden Sie unter der [steuert Verweis](controls.md) für Weitere Informationen.

-----

Der folgende XAML-Code zeigt einige standardmäßigen XAML-Steuerelemente in einem Xamarin.Forms verwendet `ContentPage`:

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
> Erfordert die Xmlns `a:` Präfix für die Verwendung von XAML-Standardsteuerelemente ist eine Einschränkung von der aktuellen Vorschau.


## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Steuerelementreferenz](controls.md)
