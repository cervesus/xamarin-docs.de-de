---
title: Formatieren einer plattformübergreifenden Xamarin.Forms-App
description: In diesem Artikel wird erläutert, wie Sie eine plattformübergreifende Xamarin.Forms-Anwendung mit XAML-Formatvorlagen formatieren.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/07/2020
ms.openlocfilehash: fbd957c68d7a9aa2f8e44c91fab6174d8ed72014
ms.sourcegitcommit: 5bcb6158693478ca1b3f6881dc912d3e7a8d1868
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77068742"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>Formatieren einer plattformübergreifenden Xamarin.Forms-Anwendung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

In diesem Schnellstart lernen Sie Folgendes:

- Formatieren einer Xamarin.Forms-Anwendung mithilfe von XAML-Formatvorlagen

In diesem Schnellstart erfahren Sie, wie Sie eine plattformübergreifende Xamarin.Forms-Anwendung mit XAML-Formatvorlagen formatieren können. Die fertige Anwendung wird unten gezeigt:

[![](styling-images/screenshots1-sml.png "Notes Page")](styling-images/screenshots1.png#lightbox "Notes Page")
[![](styling-images/screenshots2-sml.png "Note Entry Page")](styling-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>Voraussetzungen

Sie sollten den [vorherigen Schnellstart](database.md) erfolgreich abgeschlossen haben, bevor Sie mit diesem Schnellstart beginnen. Alternativ können Sie auch das [letzte Schnellstartbeispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) herunterladen und als Startpunkt für diesen Schnellstart verwenden.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Aktualisieren der App mit Visual Studio

1. Starten Sie Visual Studio, und öffnen Sie die Projektmappe „Notes“.

2. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **App.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Dieser Code definiert einen [`Thickness`](xref:Xamarin.Forms.Thickness)-Wert, eine Reihe von [`Color`](xref:Xamarin.Forms.Color)-Werten sowie implizite Formate für die Klassen [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und [`ContentPage`](xref:Xamarin.Forms.ContentPage). Diese Formate, die sich in der Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf der Anwendungsebene befinden, können in der gesamten App verarbeitet werden. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an der Datei **App.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

3. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **NotesPage.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Mit diesem Code wird ein implizites Format für die Klasse [`ListView`](xref:Xamarin.Forms.ListView) zur Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf Seitenebene hinzugefügt und die Eigenschaft `ListView.Margin` auf einen Wert festgelegt, der in der Klasse `ResourceDictionary` auf Anwendungsebene definiert ist. Beachten Sie, dass das implizite Format der Klasse `ListView` zur Klasse `ResourceDictionary` auf Seitenebene hinzugefügt wurde, weil es nur von der Klasse `NotesPage` verarbeitet wird. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an der Datei **NotesPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie sie.

4. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **Notes** auf die Datei **NoteEntryPage.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
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

    Mit diesem Code werden implizite Formate für die Ansichten [`Editor`](xref:Xamarin.Forms.Editor) und [`Button`](xref:Xamarin.Forms.Button) zur Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf Seitenebene hinzugefügt und die Eigenschaft `StackLayout.Margin` auf einen Wert festgelegt, der in der Klasse `ResourceDictionary` auf Anwendungsebene definiert ist. Die impliziten Formate für `Editor` und `Button` wurden zu der Klasse `ResourceDictionary` auf Seitenebene hinzugefügt, weil sie nur von der Klasse `NoteEntryPage` verarbeitet werden. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an der Datei **NoteEntryPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie sie.

5. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie unter **NotesPage** auf **+** , um zur **NoteEntryPage**-Seite zu navigieren und eine Notiz einzugeben. Beobachten Sie auf jeder Seite, wie sich die Formatierung im Vergleich zum letzten Schnellstart geändert hat.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Aktualisieren der App mit Visual Studio für Mac

1. Starten Sie Visual Studio für Mac, und öffnen Sie das Projekt „Notes“.

2. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf die Datei **App.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    Dieser Code definiert einen [`Thickness`](xref:Xamarin.Forms.Thickness)-Wert, eine Reihe von [`Color`](xref:Xamarin.Forms.Color)-Werten sowie implizite Formate für die Klassen [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und [`ContentPage`](xref:Xamarin.Forms.ContentPage). Diese Formate, die sich in der Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf der Anwendungsebene befinden, können in der gesamten App verarbeitet werden. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an der Datei **App.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

3. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf die Datei **NotesPage.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    Mit diesem Code wird ein implizites Format für die Klasse [`ListView`](xref:Xamarin.Forms.ListView) zur Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf Seitenebene hinzugefügt und die Eigenschaft `ListView.Margin` auf einen Wert festgelegt, der in der Klasse `ResourceDictionary` auf Anwendungsebene definiert ist. Beachten Sie, dass das implizite Format der Klasse `ListView` zur Klasse `ResourceDictionary` auf Seitenebene hinzugefügt wurde, weil es nur von der Klasse `NotesPage` verarbeitet wird. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an der Datei **NotesPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

4. Doppelklicken Sie im **Lösungspad** im Projekt **Notes** auf die Datei **NoteEntryPage.xaml**, um sie zu öffnen. Ersetzen Sie dann den vorhandenen Code durch den folgenden Code:

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
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
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

    Mit diesem Code werden implizite Formate für die Ansichten [`Editor`](xref:Xamarin.Forms.Editor) und [`Button`](xref:Xamarin.Forms.Button) zur Klasse [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf Seitenebene hinzugefügt und die Eigenschaft `StackLayout.Margin` auf einen Wert festgelegt, der in der Klasse `ResourceDictionary` auf Anwendungsebene definiert ist. Die impliziten Formate für `Editor` und `Button` wurden zu der Klasse `ResourceDictionary` auf Seitenebene hinzugefügt, weil sie nur von der Klasse `NoteEntryPage` verarbeitet werden. Weitere Informationen zur XAML-Formatierung finden Sie unter [Format](deepdive.md#styling) in den [ausführlichen Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md).

    Speichern Sie die Änderungen an **NoteEntryPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

5. Erstellen Sie das Projekt auf jeder Plattform, und führen Sie es aus. Weitere Informationen finden Sie unter [Erstellen des Schnellstarts](single-page.md#building-the-quickstart).

    Klicken Sie unter **NotesPage** auf **+** , um zur **NoteEntryPage**-Seite zu navigieren und eine Notiz einzugeben. Beobachten Sie auf jeder Seite, wie sich die Formatierung im Vergleich zum letzten Schnellstart geändert hat.

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

- Formatieren einer Xamarin.Forms-Anwendung mithilfe von XAML-Formatvorlagen

Weitere Informationen zu den Grundlagen der Anwendungsentwicklung mit Xamarin.Forms finden Sie in den ausführlichen Erläuterungen zum Schnellstart.

> [!div class="nextstepaction"]
> [Nächste](deepdive.md)

## <a name="related-links"></a>Verwandte Links

- [Notes (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart](deepdive.md)
