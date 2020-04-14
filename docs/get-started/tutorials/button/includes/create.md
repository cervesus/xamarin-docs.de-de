---
ms.openlocfilehash: 854212951844d2443c5d1b332d94b533673640c4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "67277251"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ButtonTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ButtonTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **ButtonTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Button`](xref:Xamarin.Forms.Button)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf dem `Button`-Element angezeigt wird.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Schaltfläche unter iOS und Android](../images/create-button.png "Schaltfläche mit Text")](../images/create-button-large.png#lightbox "Schaltfläche mit Text")

    Beachten Sie, dass ein [`Button`](xref:Xamarin.Forms.Button)-Element standardmäßig die gesamte verfügbare Fläche einnimmt. In diesem Fall entspricht das der Breite des übergeordneten Elements ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **ButtonTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **ButtonTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **ButtonTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Button`](xref:Xamarin.Forms.Button)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthält. Die [`Button.Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf dem `Button`-Element angezeigt wird.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Schaltfläche unter iOS und Android](../images/create-button.png "Schaltfläche mit Text")](../images/create-button-large.png#lightbox "Schaltfläche mit Text")

    Beachten Sie, dass ein [`Button`](xref:Xamarin.Forms.Button)-Element standardmäßig die gesamte verfügbare Fläche einnimmt. In diesem Fall entspricht das der Breite des übergeordneten Elements ([`StackLayout`](xref:Xamarin.Forms.StackLayout)).
