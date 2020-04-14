---
ms.openlocfilehash: bd3f37082443e93f10f60d9466fe22aae8571b14
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "61373382"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. Bearbeiten Sie in **MainPage.xaml** die [`Editor`](xref:Xamarin.Forms.Editor)-Deklaration, um das Verhalten anzupassen:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    In diesem Codeausschnitt werden Eigenschaften festgelegt, mit denen das Verhalten des [`Editor`](xref:Xamarin.Forms.Editor)-Elements angepasst wird. Die [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize)-Eigenschaft wird auf [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) festgelegt. Damit wird angegeben, dass die Höhe des `Editor`-Elements beim Hinzufügen von Text zunimmt und beim Entfernen von Text abnimmt. Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)-Eigenschaft begrenzt die für `Editor` zulässige Eingabelänge. Zusätzlich wird die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)-Eigenschaft auf `false` festgelegt, um die Rechtschreibprüfung zu deaktivieren, während die `IsTextPredictionEnabled`-Eigenschaft auf `false` festgelegt wird, um die (automatische) Textvorhersage zu deaktivieren.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Geben Sie im [`Editor`](xref:Xamarin.Forms.Entry)-Element Text ein, und beobachten Sie, wie die Höhe des `Editor`-Elements beim Hinzufügen von Text zunimmt und beim Entfernen von Text abnimmt. Beachten Sie auch, dass maximal 200 Zeichen eingegeben werden können:

    [![Screenshot: Editor mit automatischer Größenanpassung unter iOS und Android](../images/customize-behavior.png "Editor mit automatischer Größenanpassung")](../images/customize-behavior-large.png#lightbox "Editor mit automatischer Größenanpassung")

    Weitere Informationen zum Anpassen des [`Editor`](xref:Xamarin.Forms.Editor)-Verhaltens finden Sie im [Xamarin.Forms-Leitfaden](~/xamarin-forms/user-interface/text/editor.md).

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Bearbeiten Sie in **MainPage.xaml** die [`Editor`](xref:Xamarin.Forms.Editor)-Deklaration, um das Verhalten anzupassen:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    In diesem Codeausschnitt werden Eigenschaften festgelegt, mit denen das Verhalten des [`Editor`](xref:Xamarin.Forms.Editor)-Elements angepasst wird. Die [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize)-Eigenschaft wird auf [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) festgelegt. Damit wird angegeben, dass die Höhe des `Editor`-Elements beim Hinzufügen von Text zunimmt und beim Entfernen von Text abnimmt. Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)-Eigenschaft begrenzt die für `Editor` zulässige Eingabelänge. Zusätzlich wird die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)-Eigenschaft auf `false` festgelegt, um die Rechtschreibprüfung zu deaktivieren, während die `IsTextPredictionEnabled`-Eigenschaft auf `false` festgelegt wird, um die (automatische) Textvorhersage zu deaktivieren.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten: Geben Sie im [`Editor`](xref:Xamarin.Forms.Entry)-Element Text ein, und beobachten Sie, wie die Höhe des `Editor`-Elements beim Hinzufügen von Text zunimmt und beim Entfernen von Text abnimmt. Beachten Sie auch, dass maximal 200 Zeichen eingegeben werden können:

    [![Screenshot: Editor mit automatischer Größenanpassung unter iOS und Android](../images/customize-behavior.png "Editor mit automatischer Größenanpassung")](../images/customize-behavior-large.png#lightbox "Editor mit automatischer Größenanpassung")

    Weitere Informationen zum Anpassen des [`Editor`](xref:Xamarin.Forms.Editor)-Verhaltens finden Sie im [Xamarin.Forms-Leitfaden](~/xamarin-forms/user-interface/text/editor.md).
