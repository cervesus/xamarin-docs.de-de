---
title: Xamarin.Forms Multiscreen-Schnellstart
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 268622ff8bc7ff05771096549ed694c57139366d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms Multiscreen-Schnellstart

Dieser Schnellstart stellt dar, wie Sie die Phoneword-Anwendung durch Hinzufügen eines zweiten Bildschirms erweitern, um die Anrufliste für die Anwendung nachzuverfolgen. Die fertige Anwendung wird unten gezeigt:

[![](quickstart-images/intro-app-examples-sml.png "Phoneword-Anwendung")](quickstart-images/intro-app-examples.png#lightbox "Phoneword Application")

Erweitern Sie die Phoneword-Anwendung wie folgt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio. Klicken Sie auf der Startseite auf **Projekt öffnen...**, und wählen Sie im Dialogfeld **Projekt öffnen** die Projektmappendatei für das Phoneword-Projekt aus:

    ![](quickstart-images/vs/open-solution.png "Projekt öffnen")

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword**, und klicken Sie auf **Hinzufügen > Neues Element…**:

    ![](quickstart-images/vs/add-new-item.png "Neues Element hinzufügen")

3. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auf **Visual C#-Elemente > Xamarin.Forms > Inhaltsseite**, benennen Sie die neue Datei **CallHistoryPage**, und klicken Sie auf die Schaltfläche **Hinzufügen**. Dadurch wird dem Projekt eine Seite namens **CallHistoryPage** hinzugefügt:

    ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms-Projektvorlagen")

4. Entfernen Sie in **CallHistoryPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert die Benutzeroberfläche für die Seite deklarativ:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    Speichern Sie die Änderungen für **CallHistoryPage.xaml**, indem Sie **STRG+S** drücken und die Datei schließen.

5. Doppelklicken Sie im **Projektmappen-Explorer** auf **App.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/vs/open-app-class.png "„App.Xaml.cs“ öffnen")

6. Importieren Sie in **App.xaml.cs** den `System.Collections.Generic`-Namespace, fügen Sie die Deklaration der Eigenschaft `PhoneNumbers` hinzu, initialisieren Sie die Eigenschaft im `App`-Konstruktor, und initialisieren Sie die Eigenschaft [`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/), sodass sie eine [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ist. Die `PhoneNumbers`-Auflistung wird zum Speichern einer Liste jeder übersetzten Telefonnummer verwendet, die von der Anwendung aufgerufen wird:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

7. Doppelklicken Sie im **Projektmappen-Explorer** auf **MainPage.xaml**, um es zu öffnen:

    ![](quickstart-images/vs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

8. Fügen Sie in **MainPage.xaml** ein [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)-Steuerelement am Ende des [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Steuerelements hinzu. Die Schaltfläche wird verwendet, um zur Aufrufliste zu navigieren:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

9. Doppelklicken Sie im **Projektmappen-Explorer** auf **MainPage.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/vs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

10. Fügen Sie in **MainPage.xaml.cs** die Ereignishandlermethode `OnCallHistory` hinzu, und verändern Sie die Ereignishandlermethode `OnCall`, um die übersetzte Telefonnummer der `App.PhoneNumbers`-Auflistung hinzuzufügen. Aktivieren Sie anschließend `callHistoryButton`, vorausgesetzt, die Variable `dialer` ist nicht `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie **STRG+S** drücken, und schließen Sie die Datei.

11. Klicken Sie in Visual Studio auf die Menüelemente **Erstellen > Projektmappe erstellen**, oder drücken Sie **STRG+UMSCHALT+B**. Die Anwendung wird erstellt, und eine Erfolgsmeldung wird in der Statusleiste von Visual Studio angezeigt:

    ![](quickstart-images/vs/build-successful.png "Buildvorgang erfolgreich")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Anwendung erfolgreich erstellt wird.

12. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Schaltfläche, die der Play-Taste ähnelt), um die Anwendung zu starten:

    ![](quickstart-images/vs/start.png "Visual Studio-Symbolleiste")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword-Anwendung für UWP")

13. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **Phoneword.Droid**, und wählen Sie **Als Startprojekt festlegen** aus.
14. Klicken Sie in der Symbolleiste von Visual Studio auf **Starten** (die dreieckige Wiedergabetaste), um die Anwendung in einem Android-Emulator zu starten.
15. Wenn Sie ein iOS-Gerät haben und die Systemanforderungen für Mac für die Xamarin.Forms-Entwicklung erfüllt werden, verwenden Sie ein ähnliches Verfahren, um die App für das iOS-Gerät bereitzustellen. Alternativ können Sie die App dem [iOS-Remotesimulator](~/tools/ios-simulator.md) bereitstellen.

    Hinweis: Anrufe werden in keinem Simulator unterstützt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac. Klicken Sie auf der Startseite auf **Öffnen...**, und wählen Sie im Dialogfeld die Projektmappendatei für das Phoneword-Projekt aus:

    ![](quickstart-images/xs/open-solution.png "Projektmappe öffnen")

2. Wählen Sie im **Lösungspad** das Projekt **Phoneword** aus, klicken Sie mit der rechten Maustaste darauf, und klicken Sie auf **Hinzufügen > Neue Datei…**:

    ![](quickstart-images/xs/add-new-file.png "Neue Datei hinzufügen")

3. Klicken Sie im Dialogfeld **Neue Datei** auf **Formular > XAML für Formular-ContentPage**, nennen Sie die neue Datei **CallHistoryPage**, und klicken Sie auf **Neu**. Dadurch wird dem Projekt eine Seite namens **CallHistoryPage** hinzugefügt:

    ![](quickstart-images/xs/add-callhistorypage-class.png "Forms-ContentPage hinzufügen")

4. Doppelklicken Sie im **Lösungspad** auf **CallHistoryPage.xaml**, um es zu öffnen:

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "„CallHistoryPage.xaml“ öffnen")

5. Entfernen Sie in **CallHistoryPage.xaml** den gesamten Vorlagencode, und ersetzen Sie ihn durch den folgenden. Dieser Code definiert die Benutzeroberfläche für die Seite deklarativ:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    Speichern Sie die Änderungen an **CallHistoryPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

6. Doppelklicken Sie im **Lösungspad** auf **App.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/xs/open-app-class.png "„App.Xaml.cs“ öffnen")

7. Importieren Sie in **App.xaml.cs** den `System.Collections.Generic`-Namespace, fügen Sie die Deklaration der Eigenschaft `PhoneNumbers` hinzu, initialisieren Sie die Eigenschaft im `App`-Konstruktor, und initialisieren Sie die Eigenschaft [`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/), sodass sie eine [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) ist. Die `PhoneNumbers`-Auflistung wird zum Speichern einer Liste jeder übersetzten Telefonnummer verwendet, die von der Anwendung aufgerufen wird:

    ```csharp
    using System.Collections.Generic;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public static IList<string> PhoneNumbers { get; set; }

            public App()
            {
                InitializeComponent();
                PhoneNumbers = new List<string>();
                MainPage = new NavigationPage(new MainPage());
            }
            ...
        }
    }
    ```

    Speichern Sie die Änderungen an **App.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

8. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-xaml.png "„MainPage.xaml“ öffnen")

9. Fügen Sie in **MainPage.xaml** ein [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)-Steuerelement am Ende des [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)-Steuerelements hinzu. Die Schaltfläche wird verwendet, um zur Aufrufliste zu navigieren:

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    Speichern Sie die Änderungen an **MainPage.xaml**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

10. Doppelklicken Sie im **Lösungspad** auf **MainPage.xaml.cs**, um es zu öffnen:

    ![](quickstart-images/xs/open-mainpage-codebehind.png "„MainPage.xaml.cs“ öffnen")

11. Fügen Sie in **MainPage.xaml.cs** die Ereignishandlermethode `OnCallHistory` hinzu, und verändern Sie die Ereignishandlermethode `OnCall`, um die übersetzte Telefonnummer der `App.PhoneNumbers`-Auflistung hinzuzufügen. Aktivieren Sie anschließend `callHistoryButton`, vorausgesetzt, die Variable `dialer` ist nicht `null`:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            ...

            async void OnCall(object sender, EventArgs e)
            {
                ...
                if (dialer != null) {
                    App.PhoneNumbers.Add (translatedNumber);
                    callHistoryButton.IsEnabled = true;
                    dialer.Dial (translatedNumber);
                }
                ...
            }

            async void OnCallHistory(object sender, EventArgs e)
            {
                await Navigation.PushAsync (new CallHistoryPage ());
            }
        }
    }
    ```

    Speichern Sie die Änderungen an **MainPage.xaml.cs**, indem Sie auf **Datei > Speichern** klicken (oder indem Sie **&#8984;+S** drücken), und schließen Sie die Datei.

12. Wählen Sie in Visual Studio für Mac die Menüelemente **Erstellen > Alle erstellen** aus, oder drücken Sie **&#8984;+B**. Die Anwendung wird erstellt, und in der Symbolleiste von Visual Studio für Mac wird eine Erfolgsmeldung angezeigt:

    ![](quickstart-images/xs/build-successful.png "Buildvorgang erfolgreich")

    Wenn Fehler auftreten, wiederholen Sie die vorherigen Schritte, und beheben Sie sie, bis die Anwendung erfolgreich erstellt wird.

13. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung im iOS-Simulator zu starten:

    ![](quickstart-images/xs/start.png "Symbolleiste für Visual Studio für Mac")
    ![](quickstart-images/xs/phone-result-ios.png "iOS-Simulator")

    Hinweis: Anrufe werden im iOS-Simulator nicht unterstützt.

14. Wählen Sie im **Lösungspad** das Projekt **Phoneword.Droid** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus:

    ![](quickstart-images/xs/set-startup-project.png "Als Startprojekt festlegen")

15. Klicken Sie in der Symbolleiste von Visual Studio für Mac auf **Starten** (die dreieckige Schaltfläche, die einer Wiedergabetaste ähnelt), um die Anwendung in einem Android-Emulator zu starten:

    ![](quickstart-images/xs/phone-result-android.png "Android-Emulator")

    Hinweis: Anrufe werden im Android-Emulator nicht unterstützt.

-----

Herzlichen Glückwunsch zum Fertigstellen einer Multiscreen-Xamarin.Forms-Anwendung. Im [nächsten Abschnitt](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md) in diesem Handbuch werden die Schritte aus dieser exemplarischen Vorgehensweise wiederholt, um ein Verständnis für die Seitennavigation sowie die Datenbindung mithilfe von Xamarin.Forms zu erarbeiten.


## <a name="related-links"></a>Verwandte Links

- [Phoneword (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
