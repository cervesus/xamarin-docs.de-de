---
title: >
  Erstellen einer Xamarin.Forms-Shell-Anwendung
description: Das Vorgehen zum Erstellen einer Xamarin.Forms-Shell-Anwendung besteht darin, eine XAML-Datei zu erstellen, die die Shell-Klasse untergliedert, die MainPage-Eigenschaft der App-Klasse der Anwendung auf das in Unterklassen untergliederte Shell-Objekt festzulegen und dann die visuelle Hierarchie der Anwendung in der in Unterklassen untergliederten Shell-Klasse zu beschreiben.
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
ms.openlocfilehash: eec20ff6ceb4aee7e8fde59992576899690616c3
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739310"
---
# <a name="create-a-xamarinforms-shell-application"></a>Erstellen einer Xamarin.Forms-Shell-Anwendung


[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

So erstellen Sie eine Xamarin.Forms-Shell-Anwendung:

1. Erstellen Sie eine neue Xamarin.Forms-Anwendung, oder laden Sie eine vorhandene Anwendung, die Sie in eine Shell-Anwendung konvertieren möchten.
1. Fügen Sie eine XAML-Datei zum Projekt mit freigegebenem Code hinzu, das die `Shell`-Klasse untergliedert. Erstellen von [Unterklassen für die Shell-Klasse](#subclass-the-shell-class).
1. Legen Sie die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft der `App`-Klasse der Anwendung auf das `Shell`-Objekt mit Unterklassen fest. Weitere Informationen finden Sie unter [Bootstrapping für die Shell-Anwendung](#bootstrap-the-shell-application).
1. Beschreiben Sie die visuelle Hierarchie der Anwendung in der in Unterklassen untergliederten `Shell`-Klasse. Weitere Informationen finden Sie unter [Beschreiben der visuellen Hierarchie der Anwendung](#describe-the-visual-hierarchy-of-the-application).

## <a name="subclass-the-shell-class"></a>Erstellen von Unterklassen für die Shell-Klasse

Der erste Schritt bei der Erstellung einer Xamarin.Forms Shell-Anwendung besteht darin, eine XAML-Datei zum Projekt mit freigegebenem Code hinzuzufügen, das die `Shell`-Klasse untergliedert. Diese Datei kann beliebig benannt werden, aber **AppShell** wird empfohlen. Das folgende Code-Beispiel zeigt eine neu erstellte **AppShell.xaml**-Datei:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
```

Das folgende Beispiel zeigt die CodeBehind-Datei (**AppShell.xaml.cs**):

```csharp
using Xamarin.Forms;

namespace Xaminals
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
```

## <a name="bootstrap-the-shell-application"></a>Bootstrapping für die Shell-Anwendung

Nach dem Erstellen der XAML-Datei, die das `Shell`-Objekt untergliedert, sollte die [`MainPage`](xref:Xamarin.Forms.Application.MainPage)-Eigenschaft der `App`-Klasse auf das in Unterklassen untergliederte `Shell`-Objekt festgelegt werden:

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

In diesem Beispiel ist die `AppShell`-Klasse die XAML-Datei, die sich aus der `Shell`-Klasse ableitet.

> [!NOTE]
> Es wird zwar eine leere Shell-Anwendung erstellt, deren Ausführung löst aber eine `InvalidOperationException` aus.

## <a name="describe-the-visual-hierarchy-of-the-application"></a>Beschreiben der visuellen Hierarchie der Anwendung

Der letzte Schritt bei der Erstellung einer Xamarin.Forms-Shell-Anwendung ist die Beschreibung der visuellen Hierarchie der Anwendung in der in Unterklassen untergliederten `Shell`-Klasse. Eine in Unterklassen untergliederte `Shell`-Klasse besteht aus drei hierarchischen Hauptobjekten:

- `FlyoutItem` oder `TabBar`. Ein `FlyoutItem`-Objekt stellt ein oder mehrere Elemente im Flyout dar und sollte verwendet werden, wenn das Navigationsmuster für die Anwendung ein Flyout beinhaltet. Ein `TabBar`-Objekt stellt die untere Registerkartenleiste dar und sollte verwendet werden, wenn das Navigationsmuster für die Anwendung mit unteren Registerkarten beginnt. Jedes `FlyoutItem`- oder `TabBar`-Objekt ist ein untergeordnetes Objekt des `Shell`-Objekts.
- `Tab`: Stellt gruppierten Inhalt dar, der über die unteren Registerkarten navigierbar ist. Jedes `Tab` ist ein untergeordnetes Element eines `FlyoutItem`-Objekts oder eines `TabBar`-Objekts.
- `ShellContent`: Stellt die `ContentPage`-Objekte in Ihrer Anwendung dar. Jedes `ShellContent`-Objekt ist ein untergeordnetes Objekt eines `Tab`-Objekts. Wenn mehr als ein `ShellContent`-Objekt in einem `Tab`-Objekt vorhanden ist, sind die Objekte über obere Registerkarten navigierbar.

Keines dieser Objekte repräsentiert eine Benutzeroberfläche, sondern die Organisation der visuellen Struktur der Hierarchie. Die Shell wird diese Objekte übernehmen und die Benutzeroberfläche für die Navigation der Inhalte erstellen.


Die folgende XAML zeigt ein Beispiel für eine `Shell`-Klasse mit Unterklassen:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

Wird dieses Beispiel ausgeführt, zeigt der XAML-Code das `CatsPage`-Objekt an, da es das erste Inhaltselements ist, das in der in Unterklassen untergliederten `Shell`-Klasse deklariert ist:

[![Screenshot einer Shell-App unter iOS und Android](create-images/cats.png "Shell-App")](create-images/cats-large.png#lightbox "Shell-App")

Wird auf das Hamburger-Symbol gedrückt oder von links gewischt, wird das Flyout angezeigt:

[![Screenshot eines Shell-Flyouts, unter iOS und Android](create-images/flyout-reduced.png "Shell-Flyout")](create-images/flyout-reduced-large.png#lightbox "Shell-Flyout")

> [!IMPORTANT]
> In einer Shell-Anwendung wird jedes [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekt, das ein untergeordnetes Element eines `ShellContent`-Objekts ist, während des Anwendungsstarts erstellt. Hinzufügen weiterer `ShellContent`-Objekte mit diesem Ansatz führt dazu, dass zusätzliche Seiten während des Anwendungsstarts erstellt werden, was zu einer schlechten Starterfahrung führen kann. Mit der Shell können Seiten jedoch auch nach Bedarf als Reaktion auf die Navigation erstellt werden. Weitere Informationen finden Sie unter [Effizientes Laden von Seiten](tabs.md#efficient-page-loading) im Handbuch über die [Registerkarten der Xamarin.Forms-Shell](tabs.md).

## <a name="related-links"></a>Verwandte Links

- [Xaminals (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
