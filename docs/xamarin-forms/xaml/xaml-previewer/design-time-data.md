---
title: Verwenden von Entwurfszeitdaten zu benötigen mit dem XAML-Vorschau
description: In diesem Artikel wird erläutert, wie Entwurfszeitdaten zu verwenden, um Daten mit hohem Layouts in der XAML-Vorschau anzuzeigen, ohne die app ausgeführt wird.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 60074c3c1b69a57d313ad0243246ba6db93dde3d
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557432"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>Verwenden von Entwurfszeitdaten zu benötigen mit dem XAML-Vorschau

_Einige Layouts sind schwierig, ohne Daten zu visualisieren. Verwenden Sie diese Tipps, um die optimale Nutzung der Vorschau Ihrer Daten mit hohem Seiten in der XAML-Vorschau machen._

## <a name="design-time-data-basics"></a>Entwerfen Sie Time Data-Grundlagen

Entwurfszeitdaten ist gefälschten Daten, die Sie festlegen, um die Steuerelemente zur Visualisierung in der XAML-Vorschau zu erleichtern. Fügen Sie zunächst mit dem Header der XAML-Seite die folgenden Codezeilen hinzu:

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

Sie können nach dem Hinzufügen von Namespaces, Ablegen `d:` vor jedes Attribut und das Steuerelement, um sie in der XAML-Vorschau anzuzeigen. Elemente mit `d:` werden nicht zur Laufzeit angezeigt.

Beispielsweise können Sie Text in eine Bezeichnung hinzufügen, die an sie gebundene Daten in der Regel verfügt.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![Entwerfen von Uhrzeitdaten mit Text in eine Bezeichnung](xaml-previewer-images/designtimedata-label-sm.png "Design time Daten mit eine Bezeichnung")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

In diesem Beispiel ohne `d:Text`, die XAML-Vorschau wird mit "nothing" für die Bezeichnung angezeigt. Stattdessen zeigt es "Name". in dem die Bezeichnung um echte Daten zur Laufzeit hat.

Sie können `d:` mit einem beliebigen Attribut für eine Xamarin.Forms-Steuerelements, wie Farben und Schriftgrade Ihren Bedürfnissen entsprechend Abstand. Sie können auch auf das Steuerelement selbst hinzufügen:

```xaml
<d:Button Text="Design Time Button" />
```

[![Entwerfen von Uhrzeitdaten mit einem Schaltflächen-Steuerelement](xaml-previewer-images/designtimedata-controls-sm.png "Entwerfen von Uhrzeitdaten mit einem Schaltflächen-Steuerelement")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

In diesem Beispiel wird die Schaltfläche nur zur Entwurfszeit angezeigt. Mit dieser Methode können Sie einen Platzhalter in für put ein [benutzerdefiniertes Steuerelement, die nicht von der XAML-Vorschau unterstützt](render-custom-controls.md).

## <a name="preview-images-at-design-time"></a>Vorschaubilder zur Entwurfszeit

Sie können eine Entwurfszeit-Quelle für Images festlegen, die auf der Seite gebunden oder dynamisch geladen werden. Fügen Sie im Android-Projekt, das Bild, das Sie in der XAML-Vorschau zum anzeigen möchten die **Ressourcen > Drawable** Ordner. Fügen Sie in Ihrem iOS-Projekt, das Bild, das die **Ressourcen** Ordner. Sie können dieses Image dann in der XAML-Vorschau zur Entwurfszeit anzeigen:

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```
[![Entwerfen von Uhrzeitdaten mit Bildern](xaml-previewer-images/designtimedata-image-sm.png "Zeitdaten mit Iamges entwerfen")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Die Entwurfszeitdaten für ListViews

Listenansichten sind ein beliebtes Verfahren zum Anzeigen von Daten in einer mobilen app. Sie sind jedoch schwierig, ohne sich um echte Daten zu visualisieren. Um Entwurfszeitdaten mit ihnen zu verwenden, müssen Sie ein Design Time Array die Verwendung als ItemsSource zu erstellen. Die XAML-Vorschau zeigt an, was in diesem Array in Ihre ListView zur Entwurfszeit ist.

```xaml
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Item One</x:String>
                <x:String>Item Two</x:String>
                <x:String>Item Three</x:String>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding ItemName}"
                          d:Text="{Binding .}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

[![Entwerfen von Uhrzeitdaten mit einer ListView](xaml-previewer-images/designtimedata-itemssource-sm.png "entwerfen Zeitdaten mit einer ListView")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

In diesem Beispiel wird eine von drei TextCells ListView, in der XAML-Vorschau angezeigt. Sie können ändern, `x:String` zu einem vorhandenen Datenmodell in Ihrem Projekt.

Finden Sie unter [montemagnoss Hanselman.Forms app](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47) für ein etwas komplexeres Beispiel.

## <a name="alternative-hardcode-a-static-viewmodel"></a>Alternative: Hartcodierung einer statischen "ViewModel"

Wenn Sie nicht Entwurfszeitdaten zu einzelnen Steuerelementen hinzufügen möchten, können Sie ein mock Datenspeicher zum Binden an Ihrer Seite einrichten. Finden Sie in der montemagnos [Blogbeitrag zum Hinzufügen von Entwurfszeitdaten](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) zu erfahren, wie Sie an einem statischen ViewModel in XAML zu binden.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="requirements"></a>Anforderungen

Entwurfszeitdaten ist mindestens die Version von Xamarin.Forms-3.6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense zeigt Wellenlinien unter meinem Entwurfszeitdaten zu benötigen

Dies ist ein bekanntes Problem und wird in einer zukünftigen Version von Visual Studio behoben werden. Das Projekt wird weiterhin ohne Fehler erstellt.

### <a name="the-xaml-previewer-stopped-working"></a>Die XAML-Vorschau funktioniert nicht

Versuchen Sie es schließen und erneuten Öffnen der XAML-Datei, und bereinigen und das Projekt erneut erstellen.
