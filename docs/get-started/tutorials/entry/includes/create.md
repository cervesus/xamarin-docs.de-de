---
ms.openlocfilehash: 820761111c609f224a6dda14d5853777d22aa259
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "67277243"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **EntryTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **EntryTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Projektmappen-Explorer** im Projekt **EntryTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EntryTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry Placeholder="Enter text" />
        </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Entry`](xref:Xamarin.Forms.Entry)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Objekt enthält. Mit der [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder)-Eigenschaft wird der Platzhaltertext festgelegt, der bei der Anzeige des `Entry`-Elements anfänglich zu sehen ist.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot: Eintrag unter iOS und Android](../images/create-entry.png "Eintrag mit Platzhaltertext")](../images/create-entry-large.png#lightbox "Eintrag mit Platzhaltertext")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **EntryTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **EntryTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Doppelklicken Sie im **Lösungspad** im Projekt **EntryTutorial** auf die Datei **MainPage.xaml**, um sie zu öffnen. Entfernen Sie dann in **MainPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="EntryTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry Placeholder="Enter text" />
        </StackLayout>
    </ContentPage>
    ```

    In diesem Codeausschnitt wird die Benutzeroberfläche deklarativ für die Seite definiert, die ein [`Entry`](xref:Xamarin.Forms.Entry)-Element in einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Objekt enthält. Mit der [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder)-Eigenschaft wird der Platzhaltertext festgelegt, der bei der Anzeige des `Entry`-Elements anfänglich zu sehen ist.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Eintrag unter iOS und Android](../images/create-entry.png "Eintrag mit Platzhaltertext")](../images/create-entry-large.png#lightbox "Eintrag mit Platzhaltertext")
