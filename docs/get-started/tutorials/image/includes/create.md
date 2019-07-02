---
ms.openlocfilehash: df2fa04deafc4438798593d3f25b85ec17363996
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277247"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ImageTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ImageTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **ImageTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die ein [`Image`](xref:Xamarin.Forms.Image) in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Image.Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft gibt an, welches Bild über einen URI angezeigt wird. Die [`Image.Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft ist vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource), wodurch Bilder aus Dateien, URIs oder Ressourcen eingebunden werden können. Weitere Informationen finden Sie im Abschnitt [Anzeigen von Bildern](~/xamarin-forms/user-interface/images.md#displaying-images) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    Die [`HeightRequest`](xref:Xamarin.Forms.VisualElement)-Eigenschaft gibt die Höhe des `Image` in geräteunabhängigen Einheiten an.

    > [!NOTE]
    > Es ist nicht notwendig, die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)-Eigenschaft in diesem Beispiel festzulegen. Denn standardmäßig behält [`Image`](xref:Xamarin.Forms.Image) das Seitenverhältnis des Bilds bei.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Bild unter iOS und Android](../images/create-image.png "Bildansicht mit Bild")](../images/create-image-large.png#lightbox "Bildansicht mit Bild")

    > [!NOTE]
    > Die [`Image`](xref:Xamarin.Forms.Image)-Ansicht speichert heruntergeladene Bilder für 24 Stunden im Cache. Weitere Informationen finden Sie unter [Downloaded image caching (Zwischenspeichern von heruntergeladenen Bildern)](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ImageTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ImageTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **ImageTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    Dieser Code definiert deklarativ die Benutzeroberfläche für die Seite, die ein [`Image`](xref:Xamarin.Forms.Image) in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Image.Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft gibt an, welches Bild über einen URI angezeigt wird. Die [`Image.Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft ist vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource), wodurch Bilder aus Dateien, URIs oder Ressourcen eingebunden werden können. Weitere Informationen finden Sie im Abschnitt [Anzeigen von Bildern](~/xamarin-forms/user-interface/images.md#displaying-images) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    Die [`HeightRequest`](xref:Xamarin.Forms.VisualElement)-Eigenschaft gibt die Höhe des `Image` in geräteunabhängigen Einheiten an.

    > [!NOTE]
    > Es ist nicht notwendig, die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)-Eigenschaft in diesem Beispiel festzulegen. Denn standardmäßig behält [`Image`](xref:Xamarin.Forms.Image) das Seitenverhältnis des Bilds bei.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten:

    [![Screenshot: Bild unter iOS und Android](../images/create-image.png "Bildansicht mit Bild")](../images/create-image-large.png#lightbox "Bildansicht mit Bild")

    > [!NOTE]
    > Die [`Image`](xref:Xamarin.Forms.Image)-Ansicht speichert heruntergeladene Bilder für 24 Stunden im Cache. Weitere Informationen finden Sie unter [Downloaded image caching (Zwischenspeichern von heruntergeladenen Bildern)](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).
