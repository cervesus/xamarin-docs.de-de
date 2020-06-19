---
title: Binden von Videoquellen an den Player
description: In diesem Artikel wird beschrieben, wie Videoquellen mithilfe von Xamarin.Forms an den Videoplayer gebunden werden.
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c02764d6160bb8ce2481a3cf67e357933d25934f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133730"
---
# <a name="binding-video-sources-to-the-player"></a>Binden von Videoquellen an den Player

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Das vorhandene Video wird angehalten und das neue Video startet, wenn die `Source`-Eigenschaft der `VideoPlayer`-Ansicht auf eine neue Videodatei festgelegt wird. Dies wird anhand der Seite **Auswählen des Webvideos** des Beispiels [**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) veranschaulicht. Die Seite enthält auch eine `ListView`-Klasse mit den Titelnamen der drei Videos, auf die in der Datei **App.xaml** verwiesen wird:

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

Wenn ein Video ausgewählt wurde, wird der Ereignishandler `ItemSelected` in der CodeBehind-Datei ausgeführt. Der Handler entfernt alle Leerzeichen und Apostrophen aus dem Titel und nutzt diesen als Schlüssel, um eine der in der Datei **App.xaml** definierten Ressourcen abzurufen. Das `UriVideoSource`-Objekt wird anschließend auf die `Source`-Eigenschaft des `VideoPlayer` festgelegt.

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

Beim erstmaligen Laden der Seite wird in `ListView` kein Element ausgewählt, sodass Sie eines auswählen müssen, damit das Video abgespielt wird:

[![Auswählen des Webvideos](source-bindings-images/selectwebvideo-small.png "Auswählen des Webvideos")](source-bindings-images/selectwebvideo-large.png#lightbox "Auswählen des Webvideos")

Die `Source`-Eigenschaft von `VideoPlayer` wird durch eine bindbare Eigenschaft gesichert, d.h. sie kann das Ziel einer Datenbindung sein. Dies wird auf der Seite **Bindung an den VideoPlayer** veranschaulicht. Das Markup in der Datei **BindToVideoPlayer.xaml** wird von der folgenden Klasse unterstützt, die einen Titel eines Videos und das dazugehörige `VideoSource`-Objekt kapselt:

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

Die `ListView`-Klasse in der Datei **BindToVideoPlayer.xaml** enthält ein Array der `VideoInfo`-Objekte, von denen jedes mit einem Videotitel und dem `UriVideoSource`-Objekt aus dem Ressourcenverzeichnis in der Datei **App.xaml** initialisiert wird:

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

Die `Source`-Eigenschaft von `VideoPlayer` wird an `ListView` gebunden. Die `Path`-Klasse der Bindung wird als `SelectedItem.VideoSource` bestimmt, ein zusammengesetzter Pfad aus zwei Eigenschaften: `SelectedItem` ist eine Eigenschaft von `ListView`. Das ausgewählte Element ist vom Typ `VideoInfo`, der eine `VideoSource`-Eigenschaft aufweist.

Wie bei der ersten Seite **Auswählen des Webvideos** ist anfänglich kein Element aus `ListView` ausgewählt, sodass Sie eines der Videos auswählen müssen, damit es abgespielt wird.

## <a name="related-links"></a>Verwandte Links

- [Video Player Demos (Videoplayerdemos (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
