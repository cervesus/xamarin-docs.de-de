---
ms.openlocfilehash: d8f9445a0fc45c2700e8d9a901cfce9bd6307d2b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "75490642"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Bearbeiten Sie in **MainPage.xaml** die [`Image`](xref:Xamarin.Forms.Image)-Deklaration, um die Darstellung anzupassen:

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Dieser Code legt die Eigenschaft [`Aspect`](xref:Xamarin.Forms.Image.Aspect), die den Skalierungsmodus des Bilds definiert, auf [`Fill`](xref:Xamarin.Forms.Aspect.Fill) fest. Das `Fill`-Element wird in der Enumeration [`Aspect`](xref:Xamarin.Forms.Aspect) definiert und streckt das Bild so, dass es die Ansicht komplett ausfüllt. Dabei wird nicht berücksichtigt, ob das Bild verzerrt wird. Weitere Informationen zur Skalierung von Bildern finden Sie im Abschnitt [Anzeigen von Bildern](~/xamarin-forms/user-interface/images.md#display-images) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    Die Markuperweiterung `OnPlatform` ermöglicht Ihnen das plattformspezifische Anpassen der Benutzeroberfläche. In diesem Beispiel wird die Markuperweiterung dazu verwendet, die Eigenschaften [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) und [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) unter iOS auf 300 geräteunabhängige Einheiten und unter Android auf 250 geräteunabhängige Einheiten festzulegen. Weitere Informationen zur Markuperweiterung `OnPlatform` finden Sie im Abschnitt [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform) im Artikel [Verwenden von XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Zusätzlich wird mit der [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaft angegeben, dass das Bild horizontal und zentriert ausgerichtet werden soll.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten:

    [![Screenshot eines Bilds, das unter iOS und Android unterschiedlich groß angezeigt wird](../images/customize-appearance.png "Bildanpassung abhängig von der Plattform")](../images/customize-appearance-large.png#lightbox "Bildanpassung abhängig von der Plattform")

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Bearbeiten Sie in **MainPage.xaml** die [`Image`](xref:Xamarin.Forms.Image)-Deklaration, um die Darstellung anzupassen:

    ```xaml
    <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    Dieser Code legt die Eigenschaft [`Aspect`](xref:Xamarin.Forms.Image.Aspect), die den Skalierungsmodus des Bilds definiert, auf [`Fill`](xref:Xamarin.Forms.Aspect.Fill) fest. Das `Fill`-Element wird in der Enumeration [`Aspect`](xref:Xamarin.Forms.Aspect) definiert und streckt das Bild so, dass es die Ansicht komplett ausfüllt. Dabei wird nicht berücksichtigt, ob das Bild verzerrt wird. Weitere Informationen zur Skalierung von Bildern finden Sie im Abschnitt [Anzeigen von Bildern](~/xamarin-forms/user-interface/images.md#display-images) im Artikel [Bilder in Xamarin.Forms](~/xamarin-forms/user-interface/images.md).

    Die Markuperweiterung `OnPlatform` ermöglicht Ihnen das plattformspezifische Anpassen der Benutzeroberfläche. In diesem Beispiel wird die Markuperweiterung dazu verwendet, die Eigenschaften [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) und [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) unter iOS auf 300 und unter Android auf 250 festzulegen. Weitere Informationen zur Markuperweiterung `OnPlatform` finden Sie im Abschnitt [OnPlatform-Markuperweiterung](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform) im Artikel [Verwenden von XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/consuming.md).

    Zusätzlich wird mit der [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaft angegeben, dass das Bild horizontal und zentriert ausgerichtet werden soll.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot eines Bilds, das unter iOS und Android unterschiedlich groß angezeigt wird](../images/customize-appearance.png "Bildanpassung abhängig von der Plattform")](../images/customize-appearance-large.png#lightbox "Bildanpassung abhängig von der Plattform")
