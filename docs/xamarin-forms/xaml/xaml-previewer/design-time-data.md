---
title: Verwenden von Entwurfszeit Daten mit der XAML-Vorschau
description: In diesem Artikel wird erläutert, wie Entwurfszeit Daten verwendet werden, um datenintensive Layouts in der XAML-Vorschau anzuzeigen, ohne Ihre APP ausführen zu müssen.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 47171c3853fa8f5eb572971e119d51733cb53a40
ms.sourcegitcommit: 43423d4018cc0d4b0b8c98a4b3da0704495eb0cf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303244"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>Verwenden von Entwurfszeit Daten mit der XAML-Vorschau

_einige Layouts sind nur schwer zu visualisieren. Verwenden Sie diese Tipps, um die Vorschau der datenintensiven Seiten im XAML-Previewer optimal zu nutzen._

## <a name="design-time-data-basics"></a>Grundlagen zur Entwurfszeit Daten

Entwurfszeit Daten sind gefälschte Daten, die Sie festlegen, um die Visualisierung Ihrer Steuerelemente im XAML-Previewer zu vereinfachen. Fügen Sie zunächst dem Header der XAML-Seite die folgenden Codezeilen hinzu:

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

Nachdem Sie die Namespaces hinzugefügt haben, können Sie `d:` vor allen Attributen oder Steuerelementen ablegen, um Sie im XAML-Previewer anzuzeigen. Elemente mit `d:` werden zur Laufzeit nicht angezeigt.

Beispielsweise können Sie Text zu einer Bezeichnung hinzufügen, die normalerweise an Sie gebunden ist.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![Entwerfen von Zeit Daten mit Text in einem]Bezeichnungs(xaml-previewer-images/designtimedata-label-sm.png "Entwurfszeit Daten mit Text a Label")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

In diesem Beispiel zeigt der XAML-Previewer ohne `d:Text` nichts für die Bezeichnung an. Stattdessen wird "Name!" angezeigt. Dabei enthält die Bezeichnung echte Daten zur Laufzeit.

Sie können `d:` mit einem beliebigen Attribut für ein xamarin. Forms-Steuerelement verwenden, wie z. b. Farben, Schriftgrößen und Abstände. Sie können Sie sogar dem Steuerelement selbst hinzufügen:

```xaml
<d:Button Text="Design Time Button" />
```

[![Entwerfen von Zeit Daten mit einem Schaltflächen Steuer]Element(xaml-previewer-images/designtimedata-controls-sm.png "Entwurfszeit Daten mit einem Schalt") Flächen-Steuerelement](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

In diesem Beispiel wird die Schaltfläche nur zur Entwurfszeit angezeigt. Verwenden Sie diese Methode, um einen Platzhalter für ein [benutzerdefiniertes Steuerelement zu platzieren, das nicht von der XAML-Vorschau unterstützt wird](render-custom-controls.md).

## <a name="preview-images-at-design-time"></a>Anzeigen von Vorschaubildern zur Entwurfszeit

Sie können eine Entwurfszeit Quelle für Bilder festlegen, die an die Seite gebunden sind oder dynamisch geladen werden. Fügen Sie in Ihrem Android-Projekt das Image, das Sie in der XAML-Vorschau anzeigen möchten, dem Ordner **Ressourcen > drawable** hinzu. Fügen Sie im IOS-Projekt das Image dem **Ressourcen** Ordner hinzu. Sie können dieses Bild dann zur Entwurfszeit im XAML-Previewer anzeigen:

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```

[![Entwerfen von Zeit Daten mit Bildern](xaml-previewer-images/designtimedata-image-sm.png "Entwurfszeit Daten mit iamges")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Entwurfszeit Daten für ListViews

ListViews sind eine beliebte Methode zum Anzeigen von Daten in einem Mobile App. Allerdings ist es schwierig, ohne echte Daten visuell darzustellen. Wenn Sie Entwurfszeit Daten verwenden möchten, müssen Sie ein Entwurfszeit Array erstellen, das als ItemsSource verwendet werden kann. Der XAML-Previewer zeigt zur Entwurfszeit an, was in dem Array in der ListView steht.

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

[![Entwerfen von Zeit Daten mit ListView]-(xaml-previewer-images/designtimedata-itemssource-sm.png "Entwurfszeit Daten mit einer ListView")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

In diesem Beispiel wird eine ListView mit drei textcells in der XAML-Vorschau angezeigt. Sie können `x:String` in ein vorhandenes Datenmodell in Ihrem Projekt ändern.

Sie können auch ein Array von Datenobjekten erstellen. Beispielsweise können öffentliche Eigenschaften eines `Monkey`-Datenobjekts als Entwurfszeit Daten erstellt werden:

```csharp
namespace Monkeys.Models
{
    public class Monkey
    {
        public string Name { get; set; }
        public string Location { get; set; }
    }
}
```

Wenn Sie die-Klasse in XAML verwenden möchten, müssen Sie den-Namespace im Stamm Knoten importieren:

```xaml
xmlns:models="clr-namespace:Monkeys.Models"
```

```xaml
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type models:Monkey}">
                <models:Monkey Name="Baboon" Location="Africa and Asia"/>
                <models:Monkey Name="Capuchin Monkey" Location="Central and South America"/>
                <models:Monkey Name="Blue Monkey" Location="Central and East Africa"/>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="models:Monkey">
                <TextCell Text="{Binding Name}"
                          Detail="{Binding Location}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

Der Vorteil besteht darin, dass Sie eine Bindung an das tatsächliche Modell herstellen können, das Sie verwenden möchten.

## <a name="alternative-hardcode-a-static-viewmodel"></a>Alternative Hardcodieren eines statischen ViewModel

Wenn Sie keine Entwurfszeit Daten zu einzelnen Steuerelementen hinzufügen möchten, können Sie einen Mock-Datenspeicher einrichten, der an die Seite gebunden werden soll. Im Blogbeitrag von James Montemagno finden Sie Informationen zum [Hinzufügen von Entwurfszeit Daten](https://montemagno.com/xamarin-forms-design-time-data-tips-best-practices/) , um zu erfahren, wie Sie eine Bindung an ein statisches ViewModel in XAML erstellen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="requirements"></a>Anforderungen

Entwurfszeit Daten erfordern mindestens eine Version von xamarin. Forms 3,6.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense zeigt Wellenlinien unter meine Entwurfszeit Daten an

Dies ist ein bekanntes Problem und wird in einer zukünftigen Version von Visual Studio behoben. Das Projekt wird weiterhin fehlerfrei erstellt.

### <a name="the-xaml-previewer-stopped-working"></a>Der XAML-Previewer funktioniert nicht mehr.

Versuchen Sie, die XAML-Datei zu schließen und erneut zu öffnen, und bereinigen Sie das Projekt, und
