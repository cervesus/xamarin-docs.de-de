---
title: Formatieren einer plattformübergreifenden Xamarin.Forms-App
description: In diesem Artikel wird erläutert, wie zum Formatieren einer plattformübergreifenden Xamarin.Forms-Anwendung mit XAML-Stile wird.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 53d2a0aae3398ea31676fbc3dab95d500dfe2104
ms.sourcegitcommit: e45f0cd6d7d4a77dba5ecaad4d7894025005a2dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67309506"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>Formatieren einer plattformübergreifenden Xamarin.Forms-Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)

In dieser schnellstartanleitung erfahren Sie, wie Sie:

- Formatieren einer Xamarin.Forms-Anwendung, die XAML-Stile verwenden.

Der Schnellstart erläutert, wie Sie eine plattformübergreifende Xamarin.Forms-Anwendung mit XAML-Formatvorlagen zu formatieren. Die fertige Anwendung wird unten gezeigt:

[![](styling-images/screenshots1-sml.png "Anmerkungen zu dieser Seite")](styling-images/screenshots1.png#lightbox "– Anmerkungen zu dieser Seite")
[![](styling-images/screenshots2-sml.png "Beachten Sie die Seite für")](styling-images/screenshots2.png#lightbox "Hinweis Seite für")

### <a name="prerequisites"></a>Vorraussetzungen

Sie sollte erfolgreich abgeschlossen. die [vorherigen schnellstartanleitung](database.md) bevor Sie versuchen, die in dieser schnellstartanleitung. Alternativ Herunterladen der [vorherigen schnellstartbeispiel](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/) und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Anmerkungen zu dieser Lösung.

2. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **"App.xaml"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Dieser Code definiert eine [ `Thickness` ](xref:Xamarin.Forms.Thickness) Wert, der eine Reihe von [ `Color` ](xref:Xamarin.Forms.Color) Werte und implizite Stile für die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) und [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Beachten Sie, dass diese Stile, die in der Anwendung sind [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), in der gesamten Anwendung genutzt werden können. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **"App.xaml"** durch Drücken von **STRG + S**, und schließen Sie die Datei.

3. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NotesPage.xaml** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Dieser Code Fügt einen impliziten Stil für die [ `ListView` ](xref:Xamarin.Forms.ListView) auf der Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und legt die `ListView.Margin` Eigenschaft mit einem Wert in der Anwendungsebene definiert `ResourceDictionary` . Beachten Sie, dass die `ListView` impliziter Stil wurde hinzugefügt, um auf Seitenebene `ResourceDictionary`, da sie nur von genutzt wird die `NotesPage`. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml** durch Drücken von **STRG + S**, und schließen Sie die Datei.

5. In **Projektmappen-Explorer**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NoteEntryPage.xaml** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Dieser Code fügt implizite Stile für die [ `Editor` ](xref:Xamarin.Forms.Editor) und [ `Button` ](xref:Xamarin.Forms.Button) Ansichten auf der Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und legt die `StackLayout.Margin` Eigenschaft mit einem Wert in der Anwendungsebene definiert `ResourceDictionary`. Beachten Sie, dass die `Editor` und `Button` implizite Stilen wurden hinzugefügt, um auf Seitenebene `ResourceDictionary`, da sie nur, indem verwendet werden die `NoteEntryPage`. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NoteEntryPage.xaml** durch Drücken von **STRG + S**, und schließen Sie die Datei.

6. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Beachten Sie auf jeder Seite wie die Formatierung aus vorherigen Schnellstart geändert hat.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Notes-Projekt.

2. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **"App.xaml"** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Dieser Code definiert eine [ `Thickness` ](xref:Xamarin.Forms.Thickness) Wert, der eine Reihe von [ `Color` ](xref:Xamarin.Forms.Color) Werte und implizite Stile für die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) und [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Beachten Sie, dass diese Stile, die in der Anwendung sind [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), in der gesamten Anwendung genutzt werden können. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **"App.xaml"** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

3. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NotesPage.xaml** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Dieser Code Fügt einen impliziten Stil für die [ `ListView` ](xref:Xamarin.Forms.ListView) auf der Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und legt die `ListView.Margin` Eigenschaft mit einem Wert in der Anwendungsebene definiert `ResourceDictionary` . Beachten Sie, dass die `ListView` impliziter Stil wurde hinzugefügt, um auf Seitenebene `ResourceDictionary`, da sie nur von genutzt wird die `NotesPage`. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NotesPage.xaml** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

5. In der **Lösungspad**in die **Anmerkungen zu dieser** Projekt, doppelklicken Sie auf **NoteEntryPage.xaml** um ihn zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    Dieser Code fügt implizite Stile für die [ `Editor` ](xref:Xamarin.Forms.Editor) und [ `Button` ](xref:Xamarin.Forms.Button) Ansichten auf der Seitenebene [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und legt die `StackLayout.Margin` Eigenschaft mit einem Wert in der Anwendungsebene definiert `ResourceDictionary`. Beachten Sie, dass die `Editor` und `Button` implizite Stilen wurden hinzugefügt, um auf Seitenebene `ResourceDictionary`, da sie nur, indem verwendet werden die `NoteEntryPage`. Weitere Informationen zu XAML-Formatierung, finden Sie unter [formatieren](deepdive.md#styling) in die [tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NoteEntryPage.xaml** dazu **Datei > Speichern** (oder durch Drücken der  **&#8984; + S**), und schließen Sie die Datei.

6. Erstellen Sie und führen Sie das Projekt auf jeder Plattform. Weitere Informationen finden Sie unter [des Schnellstarts erstellen](single-page.md#building-the-quickstart).

    Auf der **NotesPage** drücken Sie die **+** Schaltfläche, um das Navigieren zu der **NoteEntryPage** und geben Sie einen Hinweis. Beachten Sie auf jeder Seite wie die Formatierung aus vorherigen Schnellstart geändert hat.

::: zone-end


## <a name="next-steps"></a>Nächste Schritte

In dieser schnellstartanleitung haben Sie gelernt, wie Sie:

- Formatieren einer Xamarin.Forms-Anwendung, die XAML-Stile verwenden.

Weitere Informationen zu den Grundlagen der Anwendungsentwicklung mit Xamarin.Forms weiterhin die tieferer Einblick in Schnellstart.

> [!div class="nextstepaction"]
> [Nächste](deepdive.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)
- [Tieferer Einblick in Xamarin.Forms-Schnellstart](deepdive.md)
