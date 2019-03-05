# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Bearbeiten Sie in **MainPage.xaml** die [`Entry`](xref:Xamarin.Forms.Entry)-Deklaration, um das Verhalten anzupassen:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    In diesem Codeausschnitt werden Eigenschaften festgelegt, mit denen das Verhalten von [`Entry`](xref:Xamarin.Forms.Entry) angepasst wird. Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)-Eigenschaft begrenzt die Eingabelänge für `Entry`, und die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)-Eigenschaft ist auf `false` festgelegt, um die Rechtschreibprüfung zu deaktivieren. Ebenso wird die [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)-Eigenschaft auf `false` festgelegt, um die (automatische) Textvorhersage zu deaktivieren. Darüber hinaus stellt die [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)-Eigenschaft sicher, dass eingegebene Zeichen mithilfe eines Kennwortzeichens (schwarzer Kreis) maskiert werden.

    > [!NOTE]
    > Bei einigen Texteingabeszenarios wie der Eingabe eines Kennworts sind die Rechtschreibprüfung und die Textvorhersage störend und sollten daher deaktiviert werden.

1. Klicken Sie in der Symbolleiste von Visual Studio auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Remotesimulator oder Android-Emulator zu starten: Geben Sie Text in [`Entry`](xref:Xamarin.Forms.Entry) ein. Wie Sie beobachten können, wird jedes Zeichen durch ein Zeichen zur Kennwortmaskierung ersetzt. Es können maximal 15 Zeichen eingegeben werden:

    [![Screenshot: Eingabe mit durch Kennwortzeichen maskiertem Text unter iOS und Android](../images/customize-behavior.png "Eingabe mit maskierten Kennwortzeichen")](../images/customize-behavior-large.png#lightbox "Eingabe mit maskierten Kennwortzeichen")

    Weitere Informationen zum Anpassen des [`Entry`](xref:Xamarin.Forms.Entry)-Verhaltens finden Sie im Artikel [Xamarin.Forms-Eintrag](~/xamarin-forms/user-interface/text/entry.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Bearbeiten Sie in **MainPage.xaml** die [`Entry`](xref:Xamarin.Forms.Entry)-Deklaration, um das Verhalten anzupassen:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    In diesem Codeausschnitt werden Eigenschaften festgelegt, mit denen das Verhalten von [`Entry`](xref:Xamarin.Forms.Entry) angepasst wird. Die [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)-Eigenschaft begrenzt die Eingabelänge für `Entry`, und die [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)-Eigenschaft ist auf `false` festgelegt, um die Rechtschreibprüfung zu deaktivieren. Ebenso wird die [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)-Eigenschaft auf `false` festgelegt, um die (automatische) Textvorhersage zu deaktivieren. Darüber hinaus stellt die [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)-Eigenschaft sicher, dass eingegebene Zeichen mithilfe eines Kennwortzeichens (schwarzer Kreis) maskiert werden.

    > [!NOTE]
    > Bei einigen Texteingabeszenarios wie der Eingabe eines Kennworts sind die Rechtschreibprüfung und die Textvorhersage störend und sollten daher deaktiviert werden.

1. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf die Schaltfläche zum **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im ausgewählten iOS-Simulator oder Android-Emulator zu starten. Geben Sie Text in [`Entry`](xref:Xamarin.Forms.Entry) ein. Wie Sie beobachten können, wird jedes Zeichen durch ein Zeichen zur Kennwortmaskierung ersetzt. Es können maximal 15 Zeichen eingegeben werden:

    [![Screenshot: Eingabe mit durch Kennwortzeichen maskiertem Text unter iOS und Android](../images/customize-behavior.png "Eingabe mit maskierten Kennwortzeichen")](../images/customize-behavior-large.png#lightbox "Eingabe mit maskierten Kennwortzeichen")

    Weitere Informationen zum Anpassen des [`Entry`](xref:Xamarin.Forms.Entry)-Verhaltens finden Sie im Artikel [Xamarin.Forms-Eintrag](~/xamarin-forms/user-interface/text/entry.md).
