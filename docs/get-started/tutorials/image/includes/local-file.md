---
ms.openlocfilehash: 93ee0681adcc63fe05b4be88ff67f0aeee3e03ca
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "61384490"
---
Plattformprojekten lassen sich Bilddateien hinzufügen, auf die mit freigegebenem Code in Xamarin.Forms verwiesen werden kann. Diese Vorgehensweise ist zum Verteilen plattformspezifischer Bilder erforderlich. Beispielsweise kann es vorkommen, dass auf verschiedenen Plattformen unterschiedliche Auflösungen oder geringfügig abweichende Designs verwendet werden.

In dieser Übung passen Sie die Projektmappe **ImageTutorial** so an, dass anstelle eines Bilds, das über einen URI heruntergeladen wird, ein lokales Bild verwendet wird. Dieses ist das Xamarin-Logo, das Sie mit einem Klick auf die unten gezeigte Schaltfläche herunterladen sollten.

> [!div class="nextstepaction"]
> [XamarinLogo.png herunterladen](https://raw.githubusercontent.com/xamarin/xamarin-forms-samples/master/UserInterface/PlatformSpecifics/Droid/Resources/drawable/XamarinLogo.png)

> [!IMPORTANT]
> Wenn Sie ein Bild auf allen Plattformen nutzen möchten, *muss auf jeder Plattform der gleiche Dateiname verwendet werden*. Außerdem sollte es sich um einen gültigen Android-Ressourcennamen handeln, der nur aus Kleinbuchstaben, Zahlen, Unterstrichen und Punkten besteht.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Erweitern Sie im **Projektmappen-Explorer** im Projekt **ImageTutorial.iOS** den Knoten **Ressourcenkataloge**, und doppelklicken Sie auf **Ressourcen**, um diese zu öffnen. Klicken Sie anschließend auf der Registerkarte **Assets.xcassets** auf die Schaltfläche **Plus** und auf **Bildgruppe hinzufügen**:

    ![Screenshot: Erstellen einer neuen Bildgruppe im Ressourcenkatalog in Visual Studio](../images/vs/new-image-set.png "Neue Bildgruppe im Ressourcenkatalog")

1. Klicken Sie auf der Registerkarte **Assets.xcassets** auf die neue Bildgruppe, damit der Editor angezeigt wird:

    ![Screenshot: neue Bildgruppe im Ressourcenkatalog in Visual Studio](../images/vs/new-image-set-editor.png "Editor für Bildgruppen im Ressourcenkatalog")

1. Ziehen Sie **XamarinLogo.png** von Ihrem Dateisystem für die Kategorie **Universell** auf das Feld **1x**:

    ![Screenshot: Bildgruppe mit Bild in Visual Studio](../images/vs/image-set-with-image.png "Bildgruppe mit einem Bild")

1. Klicken Sie auf der Registerkarte **Assets.xcassets** mit der rechten Maustaste auf den Namen der neuen Bildgruppe, und benennen Sie sie in **XamarinLogo** um:

    ![Screenshot: umbenannte Bildgruppe in Visual Studio](../images/vs/rename-image-set.png "Umbenannte Bildgruppe")

    Speichern Sie, und schließen Sie die Registerkarte **Assets.xcassets**.

1. Erweitern Sie im **Projektmappen-Explorer** im Projekt **ImageTutorial.Android** den Ordner **Ressourcen**. Ziehen Sie dann **XamarinLogo.png** von Ihrem Dateisystem in den Ordner **drawable**:

    ![Screenshot: Bilddatei als Android-Ressource in Visual Studio](../images/vs/android-resource.png "Lokale Bilddatei im Ordner für Android-Ressourcen")

    > [!NOTE]
    > Visual Studio legt als Buildaktion für das Bild automatisch **AndroidResource** fest.

1. Passen Sie im Projekt **ImageTutorial** in der Datei **MainPage.xaml** die [`Image`](xref:Xamarin.Forms.Editor)-Deklaration so an, dass die lokale Datei von **XamarinLogo** angezeigt wird:

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    In diesem Codeausschnitt wird die [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft für die lokale Datei festgelegt, damit das Logo angezeigt wird. Die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)-Eigenschaft wird für iOS auf 300 und für Android auf 250 geräteunabhängige Einheiten festgelegt. Zusätzlich wird mit der [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaft angegeben, dass das Bild horizontal und zentriert ausgerichtet werden soll.

    > [!NOTE]
    > Für PNG-Bilder kann unter iOS die **PNG**-Erweiterung im Dateinamen weggelassen werden, der in der [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft festgelegt wird. Für andere Bildformate ist die Erweiterung erforderlich.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Bildansicht mit einem lokalen Bild unter iOS und Android](../images/local-file.png "Bildansicht mit einem lokalen Bild")](../images/local-file-large.png#lightbox "Bildansicht mit einem lokalen Bild")

    Weitere Informationen zu lokalen Bildern finden Sie unter [Local images (Lokale Bilder)](~/xamarin-forms/user-interface/images.md#local-images) im [Xamarin.Forms-Leitfaden](~/xamarin-forms/user-interface/images.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Doppelklicken Sie im **Lösungspad** im Projekt **ImageTutorial.iOS** auf **Assets.xcassets**, um die Datei zu öffnen. Klicken Sie anschließend in der **Assets List** (Ressourcenliste) mit der rechten Maustaste, und klicken Sie dann auf **Neue Bildzusammenstellung**:

    ![Screenshot: Erstellen einer neuen Bildgruppe im Ressourcenkatalog in Visual Studio für Mac](../images/vsmac/new-image-set.png "Neue Bildgruppe im Ressourcenkatalog")

1. Klicken Sie in der **Assets List** (Ressourcenliste) auf die neue Bildgruppe, damit der Editor angezeigt wird:

    ![Screenshot: neue Bildgruppe im Ressourcenkatalog in Visual Studio für Mac](../images/vsmac/new-image-set-editor.png "Editor für Bildgruppen im Ressourcenkatalog")

1. Ziehen Sie **XamarinLogo.png** von Ihrem Dateisystem für die Kategorie **Universell** auf das Feld **1x**:

    ![Screenshot: Bildgruppe mit Bild in Visual Studio für Mac](../images/vsmac/image-set-with-image.png "Bildgruppe mit einem Bild")

1. Doppelklicken Sie in der **Assets List** (Ressourcenliste) auf den Namen der neuen Bildgruppe, und benennen Sie sie in **XamarinLogo** um:

    ![Screenshot: umbenannte Bildgruppe in Visual Studio für Mac](../images/vsmac/rename-image-set.png "Umbenannte Bildgruppe")

1. Erweitern Sie im **Lösungspad** im Projekt **ImageTutorial.Android** den Ordner **Ressourcen**. Ziehen Sie dann **XamarinLogo.png** von Ihrem Dateisystem in den Ordner **drawable**.

1. Klicken Sie im Dialogfeld **Datei zum Ordner hinzufügen** auf **OK**.

    ![Screenshot: Bilddatei als Android-Ressource in Visual Studio für Mac](../images/vsmac/android-resource.png "Lokale Bilddatei im Ordner für Android-Ressourcen")

    > [!NOTE]
    > Visual Studio für Mac legt als Buildaktion für das Bild automatisch **AndroidResource** fest.

1. Passen Sie im Projekt **ImageTutorial** in der Datei **MainPage.xaml** die [`Image`](xref:Xamarin.Forms.Editor)-Deklaration so an, dass die lokale Datei von **XamarinLogo** angezeigt wird:

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    In diesem Codeausschnitt wird die [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft für die lokale Datei festgelegt, damit das Logo angezeigt wird. Die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)-Eigenschaft wird für iOS auf 300 und für Android auf 250 geräteunabhängige Einheiten festgelegt. Zusätzlich wird mit der [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaft angegeben, dass das Bild horizontal und zentriert ausgerichtet werden soll.

    > [!NOTE]
    > Für PNG-Bilder kann unter iOS die **PNG**-Erweiterung im Dateinamen weggelassen werden, der in der [`Source`](xref:Xamarin.Forms.Image.Source)-Eigenschaft festgelegt wird. Für andere Bildformate ist die Erweiterung erforderlich.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten.

    [![Screenshot: Bildansicht mit einem lokalen Bild unter iOS und Android](../images/local-file.png "Bildansicht mit einem lokalen Bild")](../images/local-file-large.png#lightbox "Bildansicht mit einem lokalen Bild")

    Weitere Informationen zu lokalen Bildern finden Sie unter [Local images (Lokale Bilder)](~/xamarin-forms/user-interface/images.md#local-images) im [Xamarin.Forms-Leitfaden](~/xamarin-forms/user-interface/images.md).
