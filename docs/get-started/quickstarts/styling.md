---
title: Formatieren einer plattformübergreifenden Xamarin.Forms-App
description: In diesem Artikel wird erläutert, wie eine plattformübergreifende xamarin. Forms-Anwendung mit XAML-Stilen formatiert wird.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: a7e1cdd59b463c38be1a49e962112cb893eed50f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488867"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>Stil einer plattformübergreifenden xamarin. Forms-Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

In dieser Schnellstartanleitung erfahren Sie Folgendes:

- Formatieren Sie eine xamarin. Forms-Anwendung mithilfe von XAML-Stilen.

In der Schnellstartanleitung erfahren Sie, wie Sie eine plattformübergreifende xamarin. Forms-Anwendung mit XAML-Stilen formatieren. Die fertige Anwendung wird unten gezeigt:

[![](styling-images/screenshots1-sml.png "Notes Page")](styling-images/screenshots1.png#lightbox "Notes Page")
[![](styling-images/screenshots2-sml.png "Note Entry Page")](styling-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>Erforderliche Komponenten

Sie sollten den [vorherigen Schnellstart](database.md) erfolgreich abgeschlossen haben, bevor Sie diesen Schnellstart durchführen. Alternativ können Sie das [vorherige Schnellstart Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) herunterladen und als Ausgangspunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Lösung Notes.

2. Doppelklicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt auf **app. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code definiert einen [`Thickness`](xref:Xamarin.Forms.Thickness) Wert, eine Reihe von [`Color`](xref:Xamarin.Forms.Color) Werten und implizite Stile für die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und [`ContentPage`](xref:Xamarin.Forms.ContentPage). Beachten Sie, dass diese Stile, die sich auf der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Anwendungsebene befinden, in der gesamten Anwendung verwendet werden können. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **app. XAML** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

3. Doppelklicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt auf **NotesPage. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code fügt der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene einen impliziten Stil für das [`ListView`](xref:Xamarin.Forms.ListView) hinzu und legt die `ListView.Margin`-Eigenschaft auf einen Wert fest, der in der `ResourceDictionary`auf Anwendungsebene definiert ist. Beachten Sie, dass der `ListView` impliziter Stil der `ResourceDictionary`auf Seitenebene hinzugefügt wurde, da er nur von der `NotesPage`genutzt wird. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an der Datei " **NotesPage. XAML** ", indem Sie **STRG + S**drücken, und schließen Sie die Datei.

4. Doppelklicken Sie in **Projektmappen-Explorer**im **Notes** -Projekt auf **noteentrypage. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                <Setter Property="CornerRadius" Value="5" />
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

    Mit diesem Code werden dem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene implizite Stile für die [`Editor`](xref:Xamarin.Forms.Editor) -und [`Button`](xref:Xamarin.Forms.Button) Ansichten hinzugefügt, und die `StackLayout.Margin`-Eigenschaft wird auf einen Wert festgelegt, der in der `ResourceDictionary`auf Anwendungsebene definiert ist. Beachten Sie, dass die `Editor`-und `Button` impliziten Stile zum `ResourceDictionary`auf Seitenebene hinzugefügt wurden, da Sie nur vom `NoteEntryPage`verwendet werden. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **noteentrypage. XAML** , indem Sie **STRG + S**drücken, und schließen Sie die Datei.

5. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die Schaltfläche **+** , um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Beobachten Sie auf jeder Seite, wie sich das Formatieren seit dem vorherigen Schnellstart geändert hat.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Notes-Projekt.

2. Doppelklicken Sie im **Lösungspad**im **Notes** -Projekt auf **app. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code definiert einen [`Thickness`](xref:Xamarin.Forms.Thickness) Wert, eine Reihe von [`Color`](xref:Xamarin.Forms.Color) Werten und implizite Stile für die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und [`ContentPage`](xref:Xamarin.Forms.ContentPage). Beachten Sie, dass diese Stile, die sich auf der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Anwendungsebene befinden, in der gesamten Anwendung verwendet werden können. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen in der Datei **app. XAML** , indem Sie **Datei > Speichern** (oder durch Drücken  **&#8984; von + S**) auswählen, und schließen Sie die Datei.

3. Doppelklicken Sie im **Lösungspad**im Projekt **Notes** auf **NotesPage. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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

    Dieser Code fügt der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene einen impliziten Stil für das [`ListView`](xref:Xamarin.Forms.ListView) hinzu und legt die `ListView.Margin`-Eigenschaft auf einen Wert fest, der in der `ResourceDictionary`auf Anwendungsebene definiert ist. Beachten Sie, dass der `ListView` impliziter Stil der `ResourceDictionary`auf Seitenebene hinzugefügt wurde, da er nur von der `NotesPage`genutzt wird. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an der Datei " **NotesPage. XAML** ", indem Sie **Datei > Speichern** (oder durch Drücken  **&#8984; von + S**) auswählen, und schließen Sie die Datei.

4. Doppelklicken Sie im **Lösungspad**im **Notes** -Projekt auf **noteentrypage. XAML** , um es zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                <Setter Property="CornerRadius" Value="5" />
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

    Mit diesem Code werden dem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)auf Seitenebene implizite Stile für die [`Editor`](xref:Xamarin.Forms.Editor) -und [`Button`](xref:Xamarin.Forms.Button) Ansichten hinzugefügt, und die `StackLayout.Margin`-Eigenschaft wird auf einen Wert festgelegt, der in der `ResourceDictionary`auf Anwendungsebene definiert ist. Beachten Sie, dass die `Editor`-und `Button` impliziten Stile zum `ResourceDictionary`auf Seitenebene hinzugefügt wurden, da Sie nur vom `NoteEntryPage`verwendet werden. Weitere Informationen zur XAML- [Formatierung finden Sie unter](deepdive.md#styling) formatieren im [xamarin. Forms-Schnellstart Deep Dive](deepdive.md).

    Speichern Sie die Änderungen an **noteentrypage. XAML** , indem Sie **Datei > Speichern** (oder durch Drücken  **&#8984; von + S**) auswählen, und schließen Sie die Datei.

5. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter Erste Schritte mit [der Schnellstartanleitung](single-page.md#building-the-quickstart).

    Klicken Sie auf der **Seite NotesPage** auf die Schaltfläche **+** , um zur **noteentrypage** zu navigieren, und geben Sie einen Hinweis ein. Beobachten Sie auf jeder Seite, wie sich das Formatieren seit dem vorherigen Schnellstart geändert hat.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

- Formatieren Sie eine xamarin. Forms-Anwendung mithilfe von XAML-Stilen.

Weitere Informationen zu den Grundlagen der Anwendungsentwicklung mit xamarin. Forms finden Sie unter Deep Dive (Schnellstart).

> [!div class="nextstepaction"]
> [Nächste](deepdive.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin. Forms-Schnellstart Deep Dive](deepdive.md)
