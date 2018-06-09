---
title: Binden von Videoquellen an den player
description: In diesem Artikel erläutert, wie zum Binden von Videoquellen an den Videoplayer, indem Sie xamarin.Forms verwenden.
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: b0efdc1a20f52231f15b7a08eb86962e2079c678
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240029"
---
# <a name="binding-video-sources-to-the-player"></a>Binden von Videoquellen an den player

Wenn die `Source` Eigenschaft von der `VideoPlayer` Ansicht um eine neue Videodatei festgelegt ist, das vorhandene Video beendet die Wiedergabe und neue Videos beginnt. Dies wird dargestellt, indem die **wählen Web Video** auf der Seite der [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) Beispiel. Die Seite enthält eine `ListView` mit dem Titel der drei Videos auf die verwiesen wird die **App.xaml** Datei:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Wenn ein Video aktiviert ist, die `ItemSelected` -Ereignishandler im Code-Behind-Datei ausgeführt wird. Der Ereignishandler entfernt alle Leerzeichen und Apostrophe aus den Titel und verwendet, die als Schlüssel definierten Ressourcen Abrufen der **App.xaml** Datei. `UriVideoSource` Objekt legen Sie dann auf die `Source` Eigenschaft von der `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

Beim ersten der Seite laden wird kein Element ausgewählt, der `ListView`, sodass Sie müssen eine für das Video zum Starten der Wiedergabe auswählen:

[![Wählen Sie die Web-Video](source-bindings-images/selectwebvideo-small.png "Web Video auswählen")](source-bindings-images/selectwebvideo-large.png#lightbox "Web Video auswählen")

Die `Source` Eigenschaft `VideoPlayer` durch eine bindbare Eigenschaft an, was bedeutet, dass sie das Ziel einer Bindung kann gesichert ist. Dies wird veranschaulicht, durch die **Binden an VideoPlayer** Seite. Das Markup in der **BindToVideoPlayer.xaml** Datei wird durch die folgende Klasse, die einen beliebigen Titel ein Video und einer entsprechenden kapselt unterstützt `VideoSource` Objekt:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

Die `ListView` in der **BindToVideoPlayer.xaml** -Datei enthält ein Array dieser `VideoInfo` Objekte, die jeweils mit einem Videotitel initialisiert und die `UriVideoSource` Objekt aus dem Ressourcenverzeichnis in  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Die `Source` Eigenschaft der `VideoPlayer` gebunden ist, um die `ListView`. Die `Path` der Bindung wird angegeben als `SelectedItem.VideoSource`, also einen zusammengesetzten Pfad bestehend aus zwei Eigenschaften: `SelectedItem` ist eine Eigenschaft des `ListView`. Das ausgewählte Element ist vom Typ `VideoInfo`, verfügt über eine `VideoSource` Eigenschaft.

Wie bei der ersten **wählen Web Video** Seite ist kein Element ursprünglich ausgewählt aus der `ListView`, daher müssen Sie eines der Videos auswählen, bevor sie die Wiedergabe beginnt.


## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
