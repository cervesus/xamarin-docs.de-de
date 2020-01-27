---
ms.openlocfilehash: 841dac9486097e27923ccfe582803b4ec50371cf
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "60896677"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ändern Sie in der Datei **MainPage.xaml** die [`Label`](xref:Xamarin.Forms.Label)-Deklaration, um Text mit mehreren Formaten in einem einzelnen `Label`-Element darzustellen.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    Dieser Code zeigt Text in einem einzigen [`Label`](xref:Xamarin.Forms.Label)-Element an, das mehrere Formate verwendet. Der Text im ersten [`Span`](xref:Xamarin.Forms.Span)-Element wird mit dem im `Label`-Element festgelegten Format angezeigt, während der Text in den zweiten und dritten `Span`-Instanzen mit dem im `Label`-Element festgelegten Format und dem in jedem `Span`-Element zusätzlich festgelegten Format angezeigt wird.

    > [!NOTE]
    > Die [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)-Eigenschaft vom Typ [`FormattedString`](xref:Xamarin.Forms.FormattedString), die mindestens eine [`Span`](xref:Xamarin.Forms.Span)-Instanz umfasst.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Beachten Sie, wie sich die Darstellung von [`Label`](xref:Xamarin.Forms.Label) geändert hat:

    [![Screenshot: Bezeichnung mit formatiertem Text unter iOS und Android](../images/label-formatted-text.png "Bezeichnung mit formatiertem Text")](../images/label-formatted-text-large.png#lightbox "Bezeichnung mit formatiertem Text")

    Weitere Informationen zur Einstellung der [`Span`](xref:Xamarin.Forms.Span)-Darstellung finden Sie im Leitfaden [Xamarin.Forms-Bezeichnung](~/xamarin-forms/user-interface/text/label.md) im Abschnitt [Formatierter Text](~/xamarin-forms/user-interface/text/label.md#formatted-text).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Ändern Sie in der Datei **MainPage.xaml** die [`Label`](xref:Xamarin.Forms.Label)-Deklaration, um Text mit mehreren Formaten in einem einzelnen `Label`-Element darzustellen.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    Dieser Code zeigt Text in einem einzigen [`Label`](xref:Xamarin.Forms.Label)-Element an, das mehrere Formate verwendet. Der Text im ersten [`Span`](xref:Xamarin.Forms.Span)-Element wird mit dem im `Label`-Element festgelegten Format angezeigt, während der Text in den zweiten und dritten `Span`-Instanzen mit dem im `Label`-Element festgelegten Format und dem in jedem `Span`-Element zusätzlich festgelegten Format angezeigt wird.

    > [!NOTE]
    > Die [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)-Eigenschaft vom Typ [`FormattedString`](xref:Xamarin.Forms.FormattedString), die mindestens eine [`Span`](xref:Xamarin.Forms.Span)-Instanz umfasst.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Beachten Sie, wie sich die Darstellung von [`Label`](xref:Xamarin.Forms.Label) geändert hat:

    [![Screenshot: Bezeichnung mit formatiertem Text unter iOS und Android](../images/label-formatted-text.png "Bezeichnung mit formatiertem Text")](../images/label-formatted-text-large.png#lightbox "Bezeichnung mit formatiertem Text")

    Weitere Informationen zur Einstellung der [`Span`](xref:Xamarin.Forms.Span)-Darstellung finden Sie im Leitfaden [Xamarin.Forms-Bezeichnung](~/xamarin-forms/user-interface/text/label.md) im Abschnitt [Formatierter Text](~/xamarin-forms/user-interface/text/label.md#formatted-text).
